# 初始化

## 参数的初始化

1. 随机高斯初始化

**开源范例**： [LORA](https://caterpillarstudygroup.github.io/ReadPapers/46.html)

## 插件的初始化

插件是指用于调整预训练大模型的附件，依附于预训练大模型而存在。在训练开始时，挂上插件的效果应该于没有挂插件的效果相同(效果为0)，但又要能训得起来（导数不为0），因此需要特殊的设计。  

1. Zero Conv

**开源范例**： [ControlNet]()

2. 对 A 使用随机高斯初始化，对 B 使用零，因此 ΔW = BA 在训练开始时为零。  

**开源范例**： [LORA](https://caterpillarstudygroup.github.io/ReadPapers/46.html)