---
title: OpenCV Crop Frame and Save
date: 2017-02-26
tags: [OpenCV, python]
categories:
- Programming
- Python
---

# OpenCV Crop Frame and Save

```python
count = 0
cv2.imwrite("frame%d.jpg" % count, frame)
count = count+1
```

```python
import cv2
img = cv2.imread("lenna.png")
crop_img = img[200:400, 100:300] # Crop from x, y, w, h -> 100, 200, 300, 400
# NOTE: its img[y: y + h, x: x + w] and *not* img[x: x + w, y: y + h]
cv2.imshow("cropped", crop_img)
cv2.waitKey(0)
```



## 출처
- http://stackoverflow.com/questions/27378662/save-video-as-frames-opencvpy
- http://stackoverflow.com/questions/15589517/how-to-crop-an-image-in-opencv-using-python
