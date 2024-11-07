0. 随机种子

```python
generator = torch.Generator(device=torch.device("cuda:0"))
generator.manual_seed(torch.initial_seed())
```

1. 初始化为全0

```python
self.lora_A = nn.Parameter(self.weight.new_zeros((r, num_embeddings)))
nn.init.zeros_(self.lora_A)
```

2. 随机高斯初始化

```python
self.lora_B = nn.Parameter(self.weight.new_zeros((embedding_dim, r)))
nn.init.normal_(self.lora_B)
```