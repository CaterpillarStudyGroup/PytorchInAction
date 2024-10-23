# 训练流程

```python
# prepare optimizer and criterion
optimizer = torch.optim.Adam(model.parameters(), lr=self.config.learning_rate)
scheduler = torch.optim.lr_scheduler.StepLR(optimizer, step_size=20, gamma=0.1)
criterion = torch.nn.CrossEntropyLoss()

# prepare dataloader
train_dl = dataloader.train_dataloader()
valid_dl = dataloader.valid_dataloader()

# prepare model
model = model.to(self.device)

self.logger.info('start training...')
self.logger.info(f'device: {self.device}')
for epoch in range(self.config.epochs):
    # train epoch
    train_epoch(epoch, model, train_dl, optimizer, scheduler, criterion)

    # valid epoch
    valid_epoch(epoch, model, valid_dl, criterion)

    # save model
    torch.save(model.state_dict(), self.model_dir / f"model_{epoch+1}.pkl")
self.logger.info('training done!')
```

# 训练一个epoch

```
def train_epoch(epoch, model, train_dl, optimizer, scheduler, criterion):
    model.train()
    train_loss = []
    tbar = tqdm(train_dl, total=len(train_dl), desc='Training')
    for idx, batch in enumerate(tbar):
        feature, label = batch
        feature = feature.to(device)
        label = label.to(device)

        optimizer.zero_grad()
        pred = model(feature)
        loss = criterion(pred, label)
        loss.backward()
        optimizer.step()

        train_loss.append(loss.item())
        # update bar
        tbar.set_description(f'Epoch [{epoch + 1}/{self.config.epochs}]')
        tbar.set_postfix(loss=f'{np.mean(train_loss):.4f}',
                            lr=scheduler.get_last_lr()[0])
    scheduler.step()
```

# 组件

Dataloader
Optimizer
- Adam
Loss
- CrossEntropyLoss, MSELoss
Scheduler
- LambdaLR, StepLR


# Reference

https://emoryhuang.cn/blog/4276851715.html