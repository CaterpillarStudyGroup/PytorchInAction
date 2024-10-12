# 视频特征

## 信息提取

### 显式信息提取

光流、Motion、

### 隐式信息提取

1. Image Encoder/Video Decoder  
Encoder复用SD和Image Encoder，Decoder在SD的Image Decoder的结构上增加了时序层，使得解码出的视频有更好的连续性
**示例代码**：不开源  
**开源范例**：[48](https://caterpillarstudygroup.github.io/ReadPapers/48.html)

## 数据清洗

1. 通过光流法检测和去除静止视频，否则生成出的视频运动幅度较小。    
**示例代码**：[TODO]  
**开源范例**：[TODO]