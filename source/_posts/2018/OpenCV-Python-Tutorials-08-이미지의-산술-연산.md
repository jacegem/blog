---
title: '[OpenCV-Python Tutorials] 08 이미지의 산술 연산'
date: 2018-01-05 19:12:55
tags: [opencv, python, tutorial]
category:
- Programming
- Python
---

# [OpenCV-Python Tutorials] 08 이미지의 산술 연산

## 목표

- 더하기, 빼기, 비트 연산 등과 같은 이미지에 대한 여러 가지 산술 연산에 대해 배웁니다.
- cv2.add(), cv2.addWeighted() 등의 함수를 배우게 됩니다.

## 이미지 추가

OpenCV 함수 `cv2.add()` 또는 단순히 numpy 연산인 `res = img1 + img2`로 두 개의 이미지를 추가 할 수 있습니다. 두 이미지는 모두 같은 깊이와 유형이어야 하며 두 번째 이미지는 스칼라 값일 수 있습니다.

> OpenCV 추가와 Numpy 추가에는 차이가 있습니다. OpenCV 추가는 포화된 작업이며 Numpy 추가는 모듈러스 연산입니다.

예를 들어, 다음 샘플을 확인하십시오.

```python
>>> x = np.uint8([250])
>>> y = np.uint8([10])

>>> print cv2.add(x,y) # 250+10 = 260 => 255
[[255]]

>>> print x+y          # 250+10 = 260 % 256 = 4
[4]
```

두 개의 이미지를 추가하면 더 잘 보입니다. OpenCV 기능이 더 나은 결과를 제공합니다. 항상 OpenCV 기능을 더 잘 사용하십시오.

### 코드

```python
import cv2
import numpy as np

x = np.uint8([250])
y = np.uint8([10])

print(cv2.add(x, y))  # 250+10 = 260 => 255
print(x + y)  # 250+10 = 260 % 256 = 4
```

## 이미지 블렌딩

이것은 이미지 추가이지만 이미지에 다른 가중치가 주어지기 때문에 블렌딩이나 투명성을 부여합니다. 이미지는 아래의 방정식에 따라 추가됩니다.

$$
g(x) = (1-\alpha)f_0(x) + \alpha f_1(x)
$$

$$\alpha$$ 값을 0에서 1로 변경하면서 이미지간에 멋진 전환을 수행 할 수 있습니다.

섞기 위해 두 개의 이미지를 사용했습니다. 첫 번째 이미지에는 0.7의 가중치가 부여되고 두 번째 이미지에는 0.3이 주어집니다. `cv2.addWeighted()`는 이미지에 다음 방정식을 적용합니다.

$$
dst = \alpha \cdot img1 + \beta \cdot img2 + \gamma
$$

여기서 $$\gamma$$는 0으로 간주됩니다.

```python
import cv2

img1 = cv2.imread('resize_model1.jpg')
img2 = cv2.imread('resize_model2.jpg')

dst = cv2.addWeighted(img1, 0.7, img2, 0.3, 0)

cv2.imshow('dst', dst)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

아래의 결과를 확인하십시오:

![](https://goo.gl/DWRX2h)

> 두 이미지의 사이즈가 동일해야 합니다.


## 비트 연산

여기에는 비트 AND, OR, NOT 및 XOR 연산이 포함됩니다. 그들은 직사각형이 아닌 ROI 등을 정의하고 작업하면서 이미지의 어떤 부분을 추출하는 동안 매우 유용 할 것입니다 (아래 장에서 보게 될 것입니다). 아래에서는 이미지의 특정 영역을 변경하는 방법에 대한 예제를 보도록 하겠습니다.

OpenCV 로고를 이미지 위에 올려 놓고 싶습니다. 두 개의 이미지를 추가하면 색이 바뀝니다. 그것을 섞으면 투명한 효과를 얻습니다. 그러나 나는 그것을 불투명하게 하고 싶다. 직사각형 영역이라면 마지막 장에서했던 것처럼 ROI를 사용할 수 있습니다. 그러나 OpenCV 로고는 직사각형이 아닙니다. 그래서 아래와 같이 bitwise 연산으로 할 수 있습니다:

```python
import cv2

img1 = cv2.imread('resize_model1.jpg')
img2 = cv2.imread('opencv-logo-white.png')

# I want to put logo on top-left corner, So I create a ROI
rows, cols, channels = img2.shape
print(rows, cols, channels)         # 222 180 3

roi = img1[0:rows, 0:cols]

# Now create a mask of logo and create its inverse mask also
img2gray = cv2.cvtColor(img2, cv2.COLOR_BGR2GRAY)
ret, mask = cv2.threshold(img2gray, 10, 255, cv2.THRESH_BINARY)
mask_inv = cv2.bitwise_not(mask)

# Now black-out the area of logo in ROI
img1_bg = cv2.bitwise_and(roi, roi, mask=mask_inv)

# Take only region of logo from logo image.
img2_fg = cv2.bitwise_and(img2, img2, mask=mask)

# Put logo in ROI and modify the main image
dst = cv2.add(img1_bg, img2_fg)
img1[0:rows, 0:cols] = dst

cv2.imshow('bg', img1)
cv2.imshow('res', img1)
cv2.imshow('res', img1)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

아래 결과를 보십시오. 왼쪽 이미지는 우리가 만든 마스크를 보여줍니다. 오른쪽 이미지는 최종 결과를 보여줍니다. 이해를 돕기 위해 위의 코드에서 모든 중간 이미지, 특히 `img1_bg` 및 `img2_fg`를 표시하십시오.

![img1_bg](https://goo.gl/YnKTiM)
![img2_fg](https://goo.gl/UiWZCy)
![비트연산](https://goo.gl/qbchHu)


## 연습 문제

`cv2.addWeighted` 함수를 사용하여 폴더에 있는 이미지간에 부드러운 전환이 슬라이드 쇼 만들기


## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_core/py_image_arithmetics/py_image_arithmetics.html

<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>


