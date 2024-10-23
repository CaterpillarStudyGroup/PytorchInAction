# 准备数据流程

```python
def create_dataloader():
    # create dataset
    dataset = MyDataset(...)

    # split dataset
    dataset_train, dataset_test = split_dataset(dataset, rate=0.8)
    dataset_valid, dataset_test = split_dataset(dataset_test, rate=0.5)

    train_dataloader = DataLoader(
            dataset=dataset_train,
            batch_size=...,
            pin_memory=True,
            shuffle=True,
        )

    valid_dataloader = DataLoader(
            dataset=dataset_valid,
            batch_size=...,
            shuffle=False,
        )

    test_dataloader = DataLoader(
            dataset=dataset_test,
            batch_size=...,
            shuffle=False,
        )
```

# 读入数据

```python
class MyDataset(Dataset):
    def __init__(self, config, data):
        super().__init__()
        self.config = config
        self.data = data

    def __len__(self):
        return len(self.data.data)

    def __getitem__(self, idx):
        feature = torch.Tensor(self.data.data)[idx]
        label = torch.LongTensor(self.data.target)[idx]
        return feature, label
```

# reference

链接: https://emoryhuang.cn/blog/4276851715.html#load-create-dataset
