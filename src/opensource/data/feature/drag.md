# 拖拽交互信息

## 提取信息

### 显式编码

K * N * S * S * C 矩阵形式的[拖拽编码](https://caterpillarstudygroup.github.io/ReadPapers/47.html#%E6%8B%96%E5%8A%A8%E7%BC%96%E7%A0%81)    

**编码方式**：  
这是一个hand-crafted的编码方式，编码结果为：K * N * s * s * C  
K：控制点的个数  
N：视频帧数  
s：分辨率  
c：6，分别是start location, current localtion, end_location  
格子没有控制点经过填-1  

**适用场景**：这种数据格式与原始视频具有相同的结构，便于特征的调制。    
**示例代码**：不开源    
**开源范例**: [47](https://caterpillarstudygroup.github.io/ReadPapers/47.html)   

### 相对于上一帧的稀疏光流表示

w * h * 2的 矩阵，描述相对于上一帧的位移。由于是拖拽是稀疏光流，大部分格子上是没有拖拽信息的。把这些空白填上learnable embedding，表明用户没有在这些地方指定输入。  

**适用场景**：这种数据格式与原始视频具有相同的结构，便于特征的调制。    
**示例代码**：不开源    
**开源范例**: [51](https://caterpillarstudygroup.github.io/ReadPapers/51.html)  

### 生成dense光流信息

**适用场景**：稀疏光流信息可能存在歧义，当需要准确地控制或者要降低学习难度时，可以先生成dense光流。  
**开源范例**:   
[51](https://caterpillarstudygroup.github.io/ReadPapers/51.html)  
[Motion-I2V](https://caterpillarstudygroup.github.io/ReadPapers/44.html)  


### 隐式编码    

## 数据清洗


