---
title: '[OpenCV-Python Tutorials] 16. Canny Edge Detection'
date: 2018-01-16 19:16:50
tags: [opencv, python, tutorial]
category:
- Programming
- Python
---

# [OpenCV-Python Tutorials] 16. Canny Edge Detection

## 목표

- Canny edge detection의 개념
- OpenCV 함수 : `cv2.Canny()`

![](https://goo.gl/kAxhZQ)


## 이론

Canny Edge Detection은 널리 사용 되는 가장자리 감지 알고리즘입니다. 이것은 1986 년 John F. Canny가 개발했습니다. 이것은 다중 단계 알고리즘이며 각 단계를 거치게 됩니다.

### Noise 제거

가장자리 감지는 이미지의 노이즈에 영향 받기 쉽기 때문에 5x5 가우스 필터를 사용하여 이미지의 노이즈를 제거해야 합니다. 이전 장에서 이미 살펴 보았습니다.

### 이미지의 강도 기울기 찾기 (Finding Intensity Gradient of the Image)

Smoothened 이미지는 Sobel 커널로 수평 및 수직 방향으로 필터링되어 수평 방향 $$(G_x)$$ 및 수직 방향 $$(G_y)$$에서 1차 미분을 얻습니다. 이 두 이미지에서 다음과 같이 에지 기울기와 각 픽셀의 방향을 찾을 수 있습니다.

$$
\begin{array}{r}
Edge\_Gradient \; (G) = \sqrt{G_x^2 + G_y^2}
\\
Angle \; (\theta) = \tan^{-1} \bigg(\frac{G_y}{G_x}\bigg)
\end{array}
$$

기울기 방향은 항상 가장자리에 수직입니다. 수직, 수평 및 대각선 방향을 나타내는 4 개의 각도 중 하나로 반올림됩니다.

### Non-maximum 억제

그레디언트의 크기와 방향을 얻은 후 가장자리를 구성하지 않을 수 있는 원치 않는 픽셀을 제거하기 위해 이미지의 전체 스캔이 수행됩니다. 이를 위해, 모든 픽셀에서 픽셀은 그라디언트 방향에서 인접 픽셀의 로컬 최대 값인지 확인합니다. 아래 이미지를 확인하십시오.

![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/nms.jpg)

점 A는 가장자리에 있습니다 (수직 방향). 경사 방향은 가장자리에 수직입니다. 점 B와 C는 기울기 방향입니다. 따라서 점 A와 점 B를 사용하여 점 A와 점 B를 확인하여 극대점을 찾습니다. 그렇다면 다음 단계로 간주되며, 그렇지 않으면 억제됩니다 (0으로 설정).

간단히 말해서, 결과는 "얇은 가장자리"가 있는 이진 이미지입니다.

### Hysteresis Thresholding

이 단계에서는 모든 가장자리가 실제로 가장자리인지 아닌지를 결정 합니다. 이를 위해 두 개의 임계값 인 `minVal`과 `maxVal`이 필요합니다. 강도 기울기가 maxVal보다 큰 모서리는 모서리이어야 하며 minVal보다 작은 모서리는 반드시 모서리가 아니므로 버려야 합니다. 이 두 임계 값 사이에있는 사람들은 연결 상태에 따라 가장자리 또는 비 가장자리로 분류됩니다. 픽셀이 `"확실한"`픽셀에 연결되면 가장자리의 일부로 간주됩니다. 그렇지 않으면 삭제됩니다. 아래 이미지를 참조하십시오.

![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/hysteresis.jpg)

가장자리 A는 maxVal보다 높으므로 "확실한 가장자리"로 간주됩니다. 에지 C는 maxVal보다 낮지만 에지 A에 연결되므로 유효한 에지로 간주되어 전체 곡선을 얻습니다. 그러나 가장자리 B는 minVal보다 크고 가장자리 C와 동일한 영역에 있지만 어떤 "확실한 가장자리"에도 연결되어 있지 않으므로 삭제됩니다. 따라서 올바른 결과를 얻으려면 minVal 및 maxVal을 선택해야한다는 것이 매우 중요합니다.

이 단계는 또한 가장자리가 긴 선이라고 가정 할 때 작은 픽셀 노이즈를 제거합니다.

그래서 우리가 마침내 얻을 수있는 것은 이미지의 강한 가장자리입니다.

## OpenCV에서 Canny Edge Detection

OpenCV는 위의 모든 것을 단일 함수, `cv2.Canny()`에 넣습니다. 우리는 그것을 어떻게 사용하는지 볼 것입니다. 첫 번째 인자는 우리의 입력 이미지입니다. 두 번째 및 세 번째 인수는 각각 minVal 및 maxVal입니다. 세 번째 인수는 조리개 값입니다. 그것은 이미지 구배 찾기에 사용되는 Sobel 커널의 크기입니다. 기본적으로 3입니다. 마지막 인수는 그라디언트 크기를 찾는 방정식을 지정하는 L2gradient입니다. `True`이면 위에서 언급 한 방정식을 사용합니다. 

기본값은 `False`이며, $$Edge\_Gradient \; (G) = |G_x| + |G_y|$$. 를 사용합니다. 

```python
import cv2
import numpy as np
from matplotlib import pyplot as plt

img = cv2.imread('messi5.jpg',0)
edges = cv2.Canny(img,100,200)

plt.subplot(121),plt.imshow(img,cmap = 'gray')
plt.title('Original Image'), plt.xticks([]), plt.yticks([])
plt.subplot(122),plt.imshow(edges,cmap = 'gray')
plt.title('Edge Image'), plt.xticks([]), plt.yticks([])

plt.show()
```

결과:

![원본](https://goo.gl/BJmd8x)

![](https://goo.gl/UpBmjH)


## 추가 리소스

- 위키피디아의 [Canny Edge Detector](https://en.wikipedia.org/wiki/Canny_edge_detector)
- Canny Edge Detection Tutorial (Bill Green, 2002)

## 연습 문제

- 두 개의 트랙 바를 사용하여 임계 값을 변경할 수있는 Canny 가장자리 감지를 찾으려면 작은 응용 프로그램을 작성하십시오. 이렇게 하면 임계 값의 영향을 이해할 수 있습니다.


```python
import cv2

img = cv2.imread('canny.jpg')
img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
cv2.namedWindow('image')


def nothing(x):
    pass


# create trackbars for color change
cv2.createTrackbar('threshold1', 'image', 0, 255, nothing)
cv2.createTrackbar('threshold2', 'image', 0, 255, nothing)

while True:
    threshold1 = cv2.getTrackbarPos('threshold1', 'image')
    threshold2 = cv2.getTrackbarPos('threshold2', 'image')

    edges = cv2.Canny(img, threshold1, threshold2)

    cv2.imshow('image', edges)
    k = cv2.waitKey(1) & 0xFF
    if k == 27:
        break

cv2.destroyAllWindows()
```


![](https://goo.gl/kAxhZQ)


> threshold1, threshold2 의 값을 변경시키면서 Canny Edge Detection의 변화를 확인 할 수 있습니다. 




## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_canny/py_canny.html
- https://blog.naver.com/PostView.nhn?blogId=samsjang&logNo=220507996391&parentCategoryNo=&categoryNo=66&viewDate=&isShowPopularPosts=false&from=postList





<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>
















