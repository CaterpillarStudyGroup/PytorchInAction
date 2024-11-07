```python
# 使用register_modules，将参数设为内部属性。
self.register_modules(
    vae=vae,
    text_encoder=text_encoder,
    tokenizer=tokenizer,
    unet=unet,
    scheduler=scheduler,
    safety_checker=safety_checker,
    feature_extractor=feature_extractor,
    image_encoder=image_encoder,
)
```

# Reference
                        
原文链接：https://blog.csdn.net/weixin_48598862/article/details/135920566