<link rel="stylesheet" type="text/css" href="theme/auto-number-title.css" />

# 图像特征

## 信息提取

### 显式提取

分割、crop

### 隐式提取

VAE Embedding, ViT Embedding, 自定义Embedding   

1. Clip Emgedding    
Clip 是一个预训练模型。它将图像和文本在 latent 空间对齐。使得图像的 Emgedding 能够提取图像中的常用于作为控制信号，从 high level 引导图像/视频的生成。   
示例代码：[TODO]    
开源范例：46     

2. SD AutoEncoder    
SD AutoEncoder 是预训练模型 SD 中的一部分，本质上是一个 VAE. SD Encoder 保留了图像的结构化特点，因此 SD Encoder 得到的是 2D Embedding.    
示例代码：[TODO]    
开源范例：46  

## 数据增强

## 数据清洗

# 不同特征的混合