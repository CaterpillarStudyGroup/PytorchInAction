控制信号的注入，是feature结合的一种具体的应用场景，因为比较常见，单独提出来作为一节。

# ControlNet

# condition编码后UNet feature做cross attention

这种方法通常用于非结构化的Embedding的注入，例如Clip Embedding。

1. **注入方式**：图像/文本条件经过Clip Embedding之后与UNet Spatial Attention Layer的第一层进行cross attention。  
M
**示例代码**：[TODO]
**开源范例**：ModelScopeT2V  
odelScopeT2V的spatial attention有两层。第一层是feature与embedding的cross attention，第二层是self-attention。 

2. ControlNet

3. **注入方式**：图像经过Image Encoder之后与UNet的feature进行cross attention。  
**示例代码**：不开源    
**开源范例**: [47](https://caterpillarstudygroup.github.io/ReadPapers/47.html)   
47中实际上是把视频首帧作为reference Image，修改UNet Spatial Layer的self attention层，改成与首帧的cross attention。所以在47的UNet Spatial Layer设计中，第一层是与(Reference Image Clip Embedding, drag token)的cross attention，第二帧是与首帧UNet Feature的cross atttention。 

# condition与noise在channel维度的concat

这种方法通常用于与图像具有相似结构的Embedding的注入，例如VQGAN Embedding。
示例代码：[TODO]  
开源范例：  
1. Stable Diffusion（图像生成模型）

> &#x2705; 如果是图像生成模型，condition可以是reference image的2D embedding。

2. ModelScopeT2V（视频生成模型）
ModelScopeT2V的spatial attention有两层。第一层是feature与embedding的cross attention，第二层是self-attention。  

> &#x2705; 如果是视频生成模型，个人认为，reference image不适合用这种方式注入。因为reference image是静态的，不能与要生成的视频在时序上对齐。  

# condition用同样的方式embedding之后直接替换noise的某些帧

这种方式可以实现强制要求生成的某些帧为指定内容。  

**示例代码**：不开源    
**开源范例**: [Motion-I2V](https://caterpillarstudygroup.github.io/ReadPapers/44.html)  