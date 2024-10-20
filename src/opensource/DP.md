# DataParallel

只支持单机多卡，代码很方便，只需要添加一行，但是效率比较低，不推荐使用

## 局限性

在DP模式中，总共只有一个进程（受到GIL很强限制）。master节点相当于参数服务器，其会向其他卡广播其参数；在梯度反向传播后，各卡将梯度集中到master节点，master节点对搜集来的参数进行平均后更新参数，再将参数统一发送到其他卡上。这种参数更新方式，会导致master节点的计算任务、通讯量很重，从而导致网络阻塞，降低训练速度。

## 优点

代码实现简单。

```python
import os

# new
local_rank = int(os.environ["LOCAL_RANK"])
import torch
import torch.nn as nn
from torch import optim
import torch.distributed as dist

# 将模型加载到对应的gpu上
torch.cuda.set_device(local_rank)
dist.init_process_group(backend='nccl')
device = torch.device("cuda", local_rank)

# 构造模型
model = nn.Linear(10, 10).to(device)
model = nn.parallel.DistributedDataParallel(model, device_ids=[local_rank], output_device=local_rank)

# 前向传播
outputs = model(torch.randn(20, 10).to(device))
labels = torch.randn(20, 10).to(device)
loss_fn = nn.MSELoss()
loss_fn(outputs, labels).backward()
# 后向传播
optimizer = optim.SGD(model.parameters(), lr=0.001)
optimizer.step()
```

运行：

```
torchrun main.py
```