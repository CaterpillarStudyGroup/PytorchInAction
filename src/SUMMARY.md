# Summary

[Introduction](README.md)

# pytorch的基本用法
- [声明](./basic/create.md)
- [初始化](./basic/init.md)
- [层](./basic/layers.md)
- [数据](./basic/dataloader.md)
- [训练](./basic/train.md)


# 开源代码实战
- [数据]()
  - [特征数据的加工]()
    - [图像信息](./opensource/data/feature/image.md)
    - [图像信息 - Clip Embedding](./opensource/data/feature/image_clip_embedding.md)
    - [视频信息](./opensource/data/feature/video.md)
    - [文本信息](./opensource/data/feature/text.md)
    - [类别信息](./opensource/data/feature/class.md)
    - [2D Pose信息](./opensource/data/feature/2DPose.md)
    - [拖拽交互信息](./opensource/data/feature/drag.md)
  - [标签数据的监督](./opensource/data/label.md)
  - [数据的结合]()
    - [不同特征的结合](./opensource/data/combine/feature.md)
    - [不同特征的融合](./opensource/data/combine/multimodal.md)
    - [控制信号的注入](./opensource/data/combine/condition.md)
    - [不同数据集的结合](./opensource/data/combine/dataset.md)
    - [不同目标的结合](./opensource/data/combine/target.md)
  - [数据同步](./opensource/data/sync.md)
  - [预处理和后处理](./opensource/data/prepost.md)
- [模型]()
  - [参数初始化](./opensource/model/init.md)
  - [时序层](./opensource/model/temporal.md)
- [框架]()
  - [训练方法](./opensource/framework/training.md)
  - [长序列推断策略](./opensource/framework/long_term.md)
# pytorch深入理解
- [单机多卡](./opensource/DP.md)
- [多机多卡](./opensource/DDP.md)
# 其它常用库

