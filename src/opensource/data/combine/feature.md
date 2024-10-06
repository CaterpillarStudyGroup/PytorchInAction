# concat

1. 两个Clip Embedding做concat 

> stablediffusion

```python
class FrozenCLIPT5Encoder(AbstractEncoder):
    def __init__(self, clip_version="openai/clip-vit-large-patch14", t5_version="google/t5-v1_1-xl", device="cuda",
                 clip_max_length=77, t5_max_length=77):
        super().__init__()
        self.clip_encoder = FrozenCLIPEmbedder(clip_version, device, max_length=clip_max_length)
        self.t5_encoder = FrozenT5Embedder(t5_version, device, max_length=t5_max_length)
        print(f"{self.clip_encoder.__class__.__name__} has {count_params(self.clip_encoder) * 1.e-6:.2f} M parameters, "
              f"{self.t5_encoder.__class__.__name__} comes with {count_params(self.t5_encoder) * 1.e-6:.2f} M params.")

    def encode(self, text):
        return self(text)

    def forward(self, text):
        clip_z = self.clip_encoder.encode(text)
        t5_z = self.t5_encoder.encode(text)
        return [clip_z, t5_z]
```

2. 调制

调制是指通过调整特征中每个元素的均值和方差，使某些元素与其它元素区别开来。但又不影响特征的原始结构。

调制应用于两个具有相似结构的feature，其中一个feature通过网络生成具有相同结构的2通道信息，分别是均值和方差，把均值和方差bitwise地作用到另一个feature上。

**适用场景**：两个具有相似结构的feature。    
**示例代码**：不开源    
**开源范例**: [47](https://caterpillarstudygroup.github.io/ReadPapers/47.html)   

3. 相加