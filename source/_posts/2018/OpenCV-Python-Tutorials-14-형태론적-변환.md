---
title: '[OpenCV-Python Tutorials] 14. 형태론적 변환'
date: 2018-01-14 19:15:58
tags:
---

# [OpenCV-Python Tutorials] 14. 형태론적 변환

## 목표

이 장에서는
- Erosion, Dilation, Opening, Closing과 같은 다른 형태학적인 작동을 배울 것입니다.
- `cv2.erode()`, `cv2.dilate()`, `cv2.morphologyEx()` 등과 같은 다른 함수를 보게 될 것입니다.

## 이론

형태학적 변형은 이미지 모양을 기반으로 한 몇 가지 간단한 작업입니다. 일반적으로 이진 이미지에서 수행됩니다. 그것은 두 개의 입력이 필요합니다. 하나는 우리의 원래 이미지이고, 두 번째는 `구조 요소` 또는 `커널`이라고 불리며 동작의 성격을 결정합니다. 두 가지 기본 형태학 연산자는 Erosion과 Dilation입니다. 그런 다음 Opening, Closing, Gradient 등과 같은 변형 된 형태도 작용합니다. 우리는 다음 이미지를 통해 하나씩 살펴볼 것입니다.

![](https://goo.gl/zaxQX1)

![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/j.png)


## 1. Erosion

Erosion의 기본 아이디어는 토양 침식과 같습니다. 전경 물체의 경계를 부식시킵니다 (항상 전경을 흰색으로 유지하려고 노력합니다). 그러면 어떻게 됩니까? 커널은 2D 컨볼루션 에서 처럼 이미지를 따라 슬라이드합니다. 원래 이미지의 픽셀 (1 또는 0)은 커널 아래의 모든 픽셀이 1 인 경우에만 1로 간주되고, 그렇지 않으면 침식 (0으로 설정)됩니다.

그렇다면 경계 근처의 모든 픽셀은 커널의 크기에 따라 버려집니다. 따라서 전경 물체의 두께나 크기가 줄어들거나 단순히 이미지의 흰색 영역이 줄어 듭니다. 작은 흰색 노이즈를 없애고 (색상 공간 장에서 보았듯이) 연결된 두 객체를 분리하는 등의 작업에 유용합니다.

여기 예를 들어 5x5 커널에 1x5 커널을 사용할 것입니다. 그것이 어떻게 작동하는지 살펴보겠습니다.

```python
import cv2
import numpy as np

img = cv2.imread('snow.jpg', 0)
kernel = np.ones((5, 5), np.uint8)
erosion = cv2.erode(img, kernel, iterations=1)

cv2.imshow('erosion', erosion)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

결과:


![Erosion](https://goo.gl/kZ1xzE)
![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/erosion.png)


## 2. Dilation

그것은 Erosion의 정반대입니다. 여기서, 커널 아래의 적어도 하나의 픽셀이 '1'이면 픽셀 요소는 '1'이다. 따라서 이미지의 흰색 영역이 증가하거나 전경 오브젝트의 크기가 커집니다. 일반적으로 노이즈 제거와 같은 경우에는 erosion 후에 Dilation를 사용합니다. 왜냐하면 erosion은 백색 잡음을 제거하지만 또한 우리의 대상 객체 또한 축소시킵니다. 그런 다음에 dilate 합니다. 노이즈는 사라지므로 되살아 나지는 않습니다. 또한 객체의 깨진 부분을 결합하는 데 유용합니다.

```python
dilation = cv2.dilate(img,kernel,iterations = 1)
```

```python
import cv2
import numpy as np

img = cv2.imread('snow.jpg', 0)
kernel = np.ones((5, 5), np.uint8)
dilation = cv2.dilate(img,kernel,iterations = 1)

cv2.imshow('dilation', dilation)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

결과

![](https://goo.gl/voU4kB)
![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/dilation.png)


## 3. Opening

오프닝은 erosion과 dilation의 또 다른 이름입니다. 위에서 설명했듯이 노이즈를 제거하는데 유용합니다. 여기서는 `cv2.morphologyEx()` 함수를 사용합니다.

```python
opening = cv2.morphologyEx(img, cv2.MORPH_OPEN, kernel)
```

```python
import cv2
import numpy as np

img = cv2.imread('snow.jpg', 0)
kernel = np.ones((5, 5), np.uint8)
opening = cv2.morphologyEx(img, cv2.MORPH_OPEN, kernel)

cv2.imshow('opening', opening)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

결과:

![](https://goo.gl/2HzkaC)
![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/opening.png)



## 4. Closing

Closing은  Opening과 반대입니다. 전경 오브젝트 내부의 작은 구멍이나 오브젝트의 작은 검은 점을 닫을 때 유용합니다.

```python
closing = cv2.morphologyEx(img, cv2.MORPH_CLOSE, kernel)
```

```python
import cv2
import numpy as np

img = cv2.imread('snow.jpg', 0)
kernel = np.ones((5, 5), np.uint8)
closing = cv2.morphologyEx(img, cv2.MORPH_CLOSE, kernel)

cv2.imshow('closing', closing)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

결과:

![](https://goo.gl/XfxiMr)
![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/closing.png)


## 5. Morphological Gradient (형태학 기울기)

그것은 이미지의 dilation과 erosion의 차이입니다.

결과는 객체의 윤곽선처럼 보입니다.

```python
gradient = cv2.morphologyEx(img, cv2.MORPH_GRADIENT, kernel)
```

```python
import cv2
import numpy as np

img = cv2.imread('snow.jpg', 0)
kernel = np.ones((5, 5), np.uint8)
gradient = cv2.morphologyEx(img, cv2.MORPH_GRADIENT, kernel)

cv2.imshow('gradient', gradient)
cv2.waitKey(0)
cv2.destroyAllWindows()
```


결과:

![](https://goo.gl/n1XTqV)
![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/gradient.png)


## 6. Top Hat

그것은 입력 이미지와 Opening 이미지의 차이입니다. 아래 예제는 9x9 커널에 대해 수행됩니다.

```python
tophat = cv2.morphologyEx(img, cv2.MORPH_TOPHAT, kernel)
```

```python
import cv2
import numpy as np

img = cv2.imread('snow.jpg', 0)
kernel = np.ones((5, 5), np.uint8)
tophat = cv2.morphologyEx(img, cv2.MORPH_TOPHAT, kernel)

cv2.imshow('tophat', tophat)
cv2.waitKey(0)
cv2.destroyAllWindows()
```


결과:

![](https://goo.gl/9rVVJh)

![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/tophat.png)

## 7. Black Hat


입력 이미지와 closing 이미지의 차이입니다. 

```python
blackhat = cv2.morphologyEx(img, cv2.MORPH_BLACKHAT, kernel)
```

```python
import cv2
import numpy as np

img = cv2.imread('snow.jpg', 0)
kernel = np.ones((5, 5), np.uint8)
blackhat = cv2.morphologyEx(img, cv2.MORPH_BLACKHAT, kernel)

cv2.imshow('blackhat', blackhat)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

결과:

![](https://goo.gl/ptdx2j)

![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/blackhat.png)

## 구조 요소

Numpy의 도움을 받아 앞의 예제에서 수동으로 구조 요소를 만들었습니다. 사각형 모양입니다. 그러나 어떤 경우에는 타원형 / 원형 커널이 필요할 수 있습니다. 그래서 이 목적을 위해 OpenCV는 `cv2.getStructuringElement()` 함수를 가지고 있습니다. 커널의 모양과 크기 만 전달하면 원하는 커널을 얻을 수 있습니다.

```python
# Rectangular Kernel
>>> cv2.getStructuringElement(cv2.MORPH_RECT,(5,5))
array([[1, 1, 1, 1, 1],
       [1, 1, 1, 1, 1],
       [1, 1, 1, 1, 1],
       [1, 1, 1, 1, 1],
       [1, 1, 1, 1, 1]], dtype=uint8)

# Elliptical Kernel
>>> cv2.getStructuringElement(cv2.MORPH_ELLIPSE,(5,5))
array([[0, 0, 1, 0, 0],
       [1, 1, 1, 1, 1],
       [1, 1, 1, 1, 1],
       [1, 1, 1, 1, 1],
       [0, 0, 1, 0, 0]], dtype=uint8)

# Cross-shaped Kernel
>>> cv2.getStructuringElement(cv2.MORPH_CROSS,(5,5))
array([[0, 0, 1, 0, 0],
       [0, 0, 1, 0, 0],
       [1, 1, 1, 1, 1],
       [0, 0, 1, 0, 0],
       [0, 0, 1, 0, 0]], dtype=uint8)
```

## 추가 리소스

- HIPR2에서의 [형태론적 조작](http://homepages.inf.ed.ac.uk/rbf/HIPR2/morops.htm)




## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_morphological_ops/py_morphological_ops.html


<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>










