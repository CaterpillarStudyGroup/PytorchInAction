# DistributedDataParallel

支持多机多卡，效率高，但是要折腾一下代码，DDP是一个支持多机多卡、分布式训练的深度学习工程方法。

在DDP模式下，会有N个进程被启动，每个进程在一张卡上加载一个模型，这些模型的参数在数值上是相同的。
在模型训练时，各个进程通过一种叫Ring-Reduce的方法与其他进程通讯，交换各自的梯度，从而获得所有进程的梯度；
各个进程用平均后的梯度更新自己的参数，因为各个进程的初始参数、更新梯度是一致的，所以更新后的参数也是完全相同的。

每个进程一张卡，这是DDP的最佳使用方法。
每个进程多张卡，并行模式。一个模型的不同部分分布在不同的卡上面。例如，网络的前半部分在0号卡上，后半部分在1号卡上。这种场景，一般是因为我们的模型非常大，大到一张卡都塞不下batch size = 1的一个模型。

- group，即进程组。默认情况下，只有一个组。这个可以先不管，一直用默认的就行。
- world size，表示全局的并行数，如果只开了一个进程那就是1。
- rank，当前进程的序号，用于进程间通讯。对于16的world size来说，就是0,1,2,…,15。注意：rank=0的进程就是master进程。
- local_rank，又一个序号。这是每台机子上的进程的序号。机器一上有0,1,2,3,4,5,6,7，机器二上也有0,1,2,3,4,5,6,7。**一般情况下，你需要用这个local_rank来手动设置当前模型是跑在当前机器的哪块GPU上面的。**

```python
# 获取world size，在不同进程里都是一样的
torch.distributed.get_world_size()
# 获取rank，每个进程都有自己的序号，各不相同
torch.distributed.get_rank()
# 获取local_rank。一般情况下，你需要用这个local_rank来手动设置当前模型是跑在当前机器的哪块GPU上面的。
torch.distributed.local_rank()
```

Demo:

```python
## main.py文件
import os
import torch
import torch.nn as nn
from torch import optim
from torch.nn.parallel import DistributedDataParallel as DDP
import torch.distributed as dist
from torch.utils.data import DataLoader, data
import warnings 
warnings.filterwarnings('ignore')
local_rank = int(os.environ["LOCAL_RANK"])

torch.cuda.set_device(local_rank)
dist.init_process_group(backend='nccl')
device = torch.device("cuda", local_rank)

# print(f"world size: {torch.distributed.get_world_size()}")
print(f"rank: {torch.distributed.get_rank()}")

class CPPDataset(data.Dataset):
    def __init__(self, seqs, labels):
        self.seqs = seqs
        self.labels = labels
    def __len__(self):
        return len(self.seqs)
    def __getitem__(self, idx):
        return self.seqs[idx], self.labels[idx]


# 构造模型
model = nn.Linear(10, 1).to(device)
model = DDP(model, device_ids=[local_rank], output_device=local_rank)

X_train = torch.Tensor(64,10).to(device)
y_train = torch.zeros(64).to(device)
train_dataset = CPPDataset(X_train, y_train)

# 新增1：使用DistributedSampler，DDP帮我们把细节都封装起来了。用，就完事儿！
#       sampler的原理，后面也会介绍。
train_sampler = torch.utils.data.distributed.DistributedSampler(train_dataset)
# 需要注意的是，这里的batch_size指的是每个进程下的batch_size。也就是说，总batch_size是这里的batch_size再乘以并行数(world_size)。
trainloader = torch.utils.data.DataLoader(train_dataset, batch_size=4, sampler=train_sampler)

loss_fn = nn.MSELoss()

for epoch in range(4):
    # 新增2：设置sampler的epoch，DistributedSampler需要这个来维持各个进程之间的相同随机数种子
    trainloader.sampler.set_epoch(epoch)
    # 后面这部分，则与原来完全一致了。
    for x, yy in trainloader:
        prediction = model(x)
        
        loss = loss_fn(prediction.squeeze(), yy)
        loss.backward()
        optimizer = optim.SGD(model.parameters(), lr=0.001)
        optimizer.step()
        
    # 保存模型参数只需要在rank=0上保存
    if dist.get_rank() == 0:
            checkpoint = {
                "net": model.module.state_dict()
            }
            torch.save(checkpoint, 'model.pt'))
```

最终通过运行torchrun --nproc_per_node 4 main.py 即可实现单机多卡训练。
当你希望在一个机器上同时跑两个torchrun程序，请使用torchrun --nproc_per_node 4 -master_port=22224 main.py，master_port的数字不能与正在运行的程序一致，会产生冲突。

注意点：

1. 保存模型 checkpoint、记录 tensorboard、输出准确率、甚至是 tqdm 都只让主进程（rank0）执行，其他进程不执行。
2. 在用 DDP 封装模型后，模型的本体（我们定义的那个类）是 model.module；所以保存模型时，最好保存 model.module.state_dict()，否则存下来的参数的 key 前面会多一个 module.，不便再次 load 模型。
3. 我们经常会在训练的每个 epoch 后进行 evaluate / inference，为避免有些进程测试完之后开始了下一轮训练，但其他进程还在测试，最好在每个 epoch 开始训练前（或者每个 epoch 完成训练后）用 dist.barrier() 同步一下。对于数据的读取是采用主进程预读取并缓存，然后其它进程从缓存中读取，不同进程之间的数据同步具体通过torch.distributed.barrier()实现。
4. 单卡 evaluate / inference 用的模型最好是本地模型 model.module 而非 DDP 包装的模型，否则非主进程会在第 3 条的 dist.barrier() 处卡死，推测原因是 DDP 包装的模型会在一些地方同步 bucket。
5. 如果模型在forward的时候出现没有使用的output，比如(logits, _)= model(x_pad, mask)，则有可能会报错，报错内容如下。需要在封装DDP的地方增加一个参数 find_unused_parameters=True)

# 参考链接
                        
原文链接：https://blog.csdn.net/qq_41020633/article/details/128902848