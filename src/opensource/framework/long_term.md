# 长序列推断策略

1. MultiDiffusion
训练时无特征处理。  
推断时，把推断序列切成有重叠的clip。在一个step中推断所有的clip的噪声，把overlap的噪声average，得到的噪声进入下一个step。  
**示例代码**：[TODO]    
**开源范例**:   
[37](https://caterpillarstudygroup.github.io/ReadPapers/37.html)  
MultiDiffusion