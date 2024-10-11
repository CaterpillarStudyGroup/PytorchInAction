# 图像特征

## 信息提取

### 显式提取

1. 提取关键点

通常用于与人相关的应用，例如要提取人的动作（趋势）

分割、crop

### 隐式提取

通过Encoder得到特征在latent space的embedding。Encoder和Decoder通常成对使用，通过Decoder把latent embedding还原成原始信息。  

VAE Embedding, ViT Embedding, 自定义Embedding    

1. Clip Embedding    
CLIP 是一个预训练模型。它将图像和文本在 latent 空间对齐。使得图像的 Embedding 能够提取图像中的 high level 的语义信息。    
常用于作为控制信号，从 high level 引导图像/视频的生成。   
示例代码：[](./image_clip_embedding.md)    
开源范例：[](https://caterpillarstudygroup.github.io/ReadPapers/46.html)     

2. SD AutoEncoder    
SD AutoEncoder 是预训练模型 SD 中的一部分，本质上是一个 VAE. SD Encoder 保留了图像的结构化特点，因此 SD Encoder 得到的是 2D Embedding.    
示例代码：[TODO]    
开源范例：[link](https://caterpillarstudygroup.github.io/ReadPapers/46.html)  

3. VQGAN
VQGAN是ModelScopeT2V中用于图像编码的预训练模型。
示例代码：[TODO]
开源范例：ModelScopeT2V

## 数据增强

## 数据清洗