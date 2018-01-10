---
title: '[OpenCV-Python Tutorials] 15. 이미지 Gradients'
date: 2018-01-15 19:16:32
tags:
---
# [OpenCV-Python Tutorials] 15. 이미지 Gradients

## 목표

- Image gradients, edges 찾기
- cv2.Sobel(), cv2.Scharr(), cv2.Laplacian()

## 이론

OpenCV는 Sobel, Scharr 및 Laplacian의 세 가지 유형의 그래디언트 필터 또는 하이 패스 필터를 제공합니다. 이것들을 각각 알아보겠습니다.

### 1. Sobel과 Scharr Derivatives

Sobel 연산자는 Gaussian smoothing 과 미분 연산으로 노이즈에 더 강합니다. `yorder` 및 `xorder` 인수에 의해 각각 파생 될 방향을 수직 또는 수평으로 지정할 수 있습니다. `ksize` 인수로 커널의 크기를 지정할 수도 있습니다. ksize = -1 인 경우 3x3 Scharr 필터가 사용되어 3x3 Sobel 필터보다 우수한 결과를 제공합니다. 사용된 커널에 대한 문서를 참조하십시오.

### 2. 라플라시안 파생

릴랙스에 의해 주어진 이미지의 라플라시안을 계산합니다. $$\Delta src = \frac{\partial ^2{src}}{\partial x^2} + \frac{\partial ^2{src}}{\partial y^2}$$ 여기서 각 파생물은 Sobel 파생을 사용하여 발견됩니다. `ksize = 1`이면 다음 커널이 필터링에 사용됩니다.

$$
kernel = \begin{bmatrix} 0 & 1 & 0 \\\\ 1 & -4 & 1 \\\\ 0 & 1 & 0  \end{bmatrix}
$$

## 코드

아래의 코드는 모든 다이어그램의 연산자를 보여줍니다. 모든 커널은 5x5 크기입니다. 출력 이미지의 깊이는 -1을 전달하여 np.uint8 유형으로 결과를 얻습니다.

```python
import cv2
from matplotlib import pyplot as plt

img = cv2.imread('kakuro.png')

laplacian = cv2.Laplacian(img, cv2.CV_64F)
sobelx = cv2.Sobel(img, cv2.CV_64F, 1, 0, ksize=5)
sobely = cv2.Sobel(img, cv2.CV_64F, 0, 1, ksize=5)

plt.subplot(2, 2, 1), plt.imshow(img, cmap='gray')
plt.title('Original'), plt.xticks([]), plt.yticks([])
plt.subplot(2, 2, 2), plt.imshow(laplacian, cmap='gray')
plt.title('Laplacian'), plt.xticks([]), plt.yticks([])
plt.subplot(2, 2, 3), plt.imshow(sobelx, cmap='gray')
plt.title('Sobel X'), plt.xticks([]), plt.yticks([])
plt.subplot(2, 2, 4), plt.imshow(sobely, cmap='gray')
plt.title('Sobel Y'), plt.xticks([]), plt.yticks([])

plt.show()
```

결과:

![](https://goo.gl/FZan6C)


## 하나의 중요한 문제!

마지막 예에서 출력 데이터 유형은 cv2.CV_8U 또는 np.uint8입니다. 하지만 약간의 문제가 있습니다. 화이트 투 블랙 (Black to White) 전환은 네거티브 (Negative) 슬로프 (Negative Slope)로 간주되는 반면, 블랙 투 화이트 전환은 포지티브 슬로프 (Positive Slope) (양의 값을 가짐)로 취해진다. 따라서 데이터를 np.uint8로 변환하면 모든 음의 기울기가 0이됩니다. 간단한 말로, 당신은 그 가장자리를 놓치게됩니다.

두 모서리를 모두 감지하려면 출력 데이터 유형을 cv2.CV_16S, cv2.CV_64F 등의 상위 형식으로 유지하고 절대 값을 취한 다음 다시 cv2.CV_8U로 변환하십시오. 아래 코드는 수평 소벨 (Sobel) 필터와 결과의 차이에 대한이 절차를 보여줍니다.

```python
import cv2
import numpy as np
from matplotlib import pyplot as plt

img = cv2.imread('box.png', 0)

# Output dtype = cv2.CV_8U
sobelx8u = cv2.Sobel(img, cv2.CV_8U, 1, 0, ksize=5)

# Output dtype = cv2.CV_64F. Then take its absolute and convert to cv2.CV_8U
sobelx64f = cv2.Sobel(img, cv2.CV_64F, 1, 0, ksize=5)
abs_sobel64f = np.absolute(sobelx64f)
sobel_8u = np.uint8(abs_sobel64f)

plt.subplot(1, 3, 1), plt.imshow(img, cmap='gray')
plt.title('Original'), plt.xticks([]), plt.yticks([])
plt.subplot(1, 3, 2), plt.imshow(sobelx8u, cmap='gray')
plt.title('Sobel CV_8U'), plt.xticks([]), plt.yticks([])
plt.subplot(1, 3, 3), plt.imshow(sobel_8u, cmap='gray')
plt.title('Sobel abs(CV_64F)'), plt.xticks([]), plt.yticks([])

plt.show()
```

아래의 결과를 확인하십시오 :

![](https://goo.gl/Ck2oxd)










## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_gradients/py_gradients.html



<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>



















