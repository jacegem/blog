---
title: '[OpenCV-Python Tutorials] 13. 이미지 Smoothing'
date: 2018-01-13 19:15:42
tags:
---

# [OpenCV-Python Tutorials] 13. 이미지 Smoothing

## 목표

배울 내용

- 다양한 저역 통과 필터로 이미지를 흐리게 처리.
- 이미지에 맞춤 필터 적용 (2D convolution)

## 2D Convolution (이미지 필터링)

1차원 신호의 경우 이미지는 다양한 저역 통과 필터 (LPF), 고역 통과 필터 (HPF) 등으로 필터링 할 수도 있습니다. `LPF`는 노이즈를 제거하거나 이미지를 흐리게 합니다. `HPF` 필터는 이미지의 가장자리를 찾는 데 도움이 됩니다.

OpenCV는 이미지로 커널을 컨볼루션하는 함수, `cv2.filter2D()`를 제공합니다. 예를 들어 이미지에 대한 평균화 필터를 시도해 보겠습니다. 5x5 평균 필터 커널은 다음과 같이 정의 할 수 있습니다.

$$
K =  \frac{1}{25} \begin{bmatrix} 1 & 1 & 1 & 1 & 1  \\\\ 1 & 1 & 1 & 1 & 1 \\\\ 1 & 1 & 1 & 1 & 1 \\\\ 1 & 1 & 1 & 1 & 1 \\\\ 1 & 1 & 1 & 1 & 1 \end{bmatrix}
$$

위의 커널로 필터링하면 다음과 같은 결과가 발생합니다. 각 픽셀은 5x5 윈도우 픽셀의 가운데에 위치하며 이 창에 속하는 모든 픽셀을 더한 다음 결과를 25로 나눕니다. 해당 창 내부의 픽셀 값 중 이 작업은 출력 필터링 된 이미지를 생성하기 위해 이미지의 모든 픽셀에 대해 수행됩니다. 이 코드를 실행하고 결과를 확인하십시오.

```python
import cv2
import numpy as np
from matplotlib import pyplot as plt

img = cv2.imread('model.jpg')
img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

size = 50
kernel = np.ones((size, size), np.float32) / (size * size)
dst = cv2.filter2D(img, -1, kernel)

cv2.imwrite('model_output.png', cv2.cvtColor(dst, cv2.COLOR_RGB2BGR))

plt.subplot(121), plt.imshow(img), plt.title('Original')
plt.xticks([]), plt.yticks([])
plt.subplot(122), plt.imshow(dst), plt.title('Averaging')
plt.xticks([]), plt.yticks([])
plt.show()

```
결과:

![원본](https://goo.gl/5Z27T3)

![변환](https://goo.gl/yEXJfg)

![비교](https://goo.gl/nHDULS)

> 변환 결과를 차이나게 만들기 위해서, 50 X 50 으로 변경하였습니다. 



## 이미지 블러링 (이미지 스무딩)

이미지 블러링은 이미지를 저역 통과 필터 커널과 컨볼루션함으로써 달성됩니다. 소음을 제거 할 때 유용합니다. 이 필터를 적용하면 이미지에서 고주파 콘텐츠 (예 : 노이즈, 가장자리)가 실제로 흐려져 가장자리가 흐려집니다. (모서리를 흐리게 하지 않는 흐림 기법이 있습니다.) OpenCV는 주로 네 가지 유형의 흐림 기술을 제공합니다.

### 1. 평균화

이는 정규화된 상자 필터로 이미지를 컨벌루션하여 수행됩니다. 커널 영역 아래의 모든 픽셀의 평균을 취하고 중심 요소를 평균으로 바꿉니다. 이것은 `cv2.blur()` 또는 `cv2.boxFilter()` 함수에 의해 수행됩니다. 커널에 대한 자세한 내용은 문서를 확인하십시오. 커널의 너비와 높이를 지정해야합니다. 3x3 정규화 된 상자 필터는 다음과 같습니다.

$$
K =  \frac{1}{9} \begin{bmatrix} 1 & 1 & 1  \\\\ 1 & 1 & 1 \\\\ 1 & 1 & 1 \end{bmatrix}
$$

> 정규화된 상자 필터를 사용하지 않으려면 `cv2.boxFilter()`를 사용하고 `normalize = False` 인수를 함수에 전달합니다.

아래의 샘플 데모를 5x5 크기의 커널로 확인하십시오.

```python
import cv2
from matplotlib import pyplot as plt

img = cv2.imread('image.jpg')
img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

blur = cv2.blur(img, (5, 5))

plt.subplot(121), plt.imshow(img), plt.title('Original')
plt.xticks([]), plt.yticks([])
plt.subplot(122), plt.imshow(blur), plt.title('Blurred')
plt.xticks([]), plt.yticks([])
plt.show()
```

결과:

![](https://goo.gl/3gYe62)


### 2. 가우시안 필터링

이러한 접근법에서, 동일한 필터 계수로 구성된 박스 필터 대신에, 가우시안 커널이 사용된다. 함수 `cv2.GaussianBlur()`로 끝납니다. 커널의 너비와 높이를 지정해야 합니다. 커널의 너비와 높이는 양수이면서 홀수여야 합니다. X와 Y 방향의 표준 편차 `sigmaX`와 `sigmaY`를 각각 지정해야 합니다. sigmaX 만 지정된 경우 sigmaY는 sigmaX와 동일하게 취급됩니다. 둘 다 0으로 주어지면 커널 크기로부터 계산됩니다. 가우시안 필터링은 이미지에서 가우스 노이즈를 제거하는 데 매우 효과적입니다.

원하는 경우, 함수 `cv2.getGaussianKernel()`을 사용하여 가우스 커널을 생성 할 수 있습니다.

위의 코드는 가우시안 블러링으로 수정될 수 있습니다.

```python
blur = cv2.GaussianBlur(img,(5,5),0)
```

```python
import cv2
from matplotlib import pyplot as plt

img = cv2.imread('happy.png')
img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

blur = cv2.GaussianBlur(img,(5,5),0)

plt.subplot(121), plt.imshow(img), plt.title('Original')
plt.xticks([]), plt.yticks([])
plt.subplot(122), plt.imshow(blur), plt.title('Blurred')
plt.xticks([]), plt.yticks([])
plt.show()
```

결과:

![](https://goo.gl/k4tNJ8)



## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_filtering/py_filtering.html



<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>



