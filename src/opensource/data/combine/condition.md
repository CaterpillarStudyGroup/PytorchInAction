控制信号的注入，是feature结合的一种具体的应用场景，因为比较常见，单独提出来作为一节。

# ControlNet

# condition编码后UNet feature做cross attention

这种方法通常用于非结构化的Embedding的注入，例如Clip Embedding。

1. **注入方式**：图像/文本条件经过Clip Embedding之后与UNet Spatial Attention Layer的第一层进行cross attention。  
M
**示例代码**：[TODO]
**开源范例**：ModelScopeT2V  
odelScopeT2V的spatial attention有两层。第一层是feature与embedding的cross attention，第二层是self-attention。 

2. Stable Diffusion
3. 图像经过Image Encoder之后与UNet的feature进行cross attention。  

# condition与noise在channel维度的concat

这种方法通常用于与图像具有相似结构的Embedding的注入，例如VQGAN Embedding。
示例代码：[TODO]  
开源范例：
1. ModelScopeT2V
ModelScopeT2V的spatial attention有两层。第一层是feature与embedding的cross attention，第二层是self-attention。  
2. Stable Diffusion