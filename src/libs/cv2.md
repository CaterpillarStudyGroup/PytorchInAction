1. 读入图像

```python
src_img = cv2.imread(src_img_p)
```

2. 读入视频

```python
dri_img_lst = []

cap = cv2.VideoCapture(dri_video_p)
while True:
    ret, f = cap.read()
    if not ret: break
    dri_img_lst.append(f)
```