---
title: '[OpenCV-Python Tutorials] 12. 이미지의 기하학적 변환'
date: 2018-01-12 19:15:26
tags:
---

# [OpenCV-Python Tutorials] 12. 이미지의 기하학적 변환

## 목표

- translation, 회전, affine 변환 등과 같은 이미지에 다른 기하학적 변환을 적용하는 방법을 배웁니다.
- 다음 함수를 볼 수 있습니다 : `cv2.getPerspectiveTransform`

## 변환

OpenCV는 모든 종류의 변형을 가질 수 있는 두 가지 변환 함수, `cv2.warpAffine` 및 `cv2.warpPerspective`를 제공합니다. `cv2.warpPerspective`는 3x3 변환 행렬을 입력으로 받는 반면 `cv2.warpAffine`은 2x3 변환 행렬을 사용합니다.

## 스케일링

크기 조절은 단지 이미지의 크기를 조절하는 것입니다. OpenCV는 이 목적을 위해 함수 `cv2.resize()`를 제공합니다. 이미지의 크기는 수동으로 지정하거나 배율 인수를 지정할 수 있습니다. 다른 보간 방법이 사용됩니다. 바람직한 보간 방법은 축소를 위한 `cv2.INTER_AREA`와 확대를 위한 `cv2.INTER_CUBIC`(느림) & `cv2.INTER_LINEAR`입니다. 기본값으로, 모든 사이즈 변경을 위해 사용되는 보간 법은 `cv2.INTER_LINEAR`입니다. 다음 방법 중 하나를 사용하여 입력 이미지의 크기를 조정할 수 있습니다.

```python
import cv2
import numpy as np

img = cv2.imread('messi5.jpg')
res = cv2.resize(img,None,fx=2, fy=2, interpolation = cv2.INTER_CUBIC)

#OR

height, width = img.shape[:2]
res = cv2.resize(img,(2*width, 2*height), interpolation = cv2.INTER_CUBIC)
```


cv2.resize() 함수의 파라미터

- src: 스케일링할 이미지
- dsize:  변경할 사이즈. 가로, 세로 형태의 tuple
- fx – 가로 사이즈의 배수. 2배로 크게하려면 2. 반으로 줄이려면 0.5
- fy – 세로 사이즈의 배수
- interpolation – 보간법



### 코드

```python
import cv2
from matplotlib import pyplot as plt

img = cv2.imread('beach.jpg')

res1 = cv2.resize(img, None, fx=2, fy=2, interpolation=cv2.INTER_CUBIC)

height, width = img.shape[:2]
res2 = cv2.resize(img, (int(0.5 * width), int(0.5 * height)), interpolation=cv2.INTER_CUBIC)

plt.subplot(1, 2, 1), plt.imshow(cv2.cvtColor(res1, cv2.COLOR_BGR2RGB)), plt.title('res1')
plt.subplot(1, 2, 2), plt.imshow(cv2.cvtColor(res2, cv2.COLOR_BGR2RGB)), plt.title('res2')
plt.show()
```

![](https://goo.gl/cD9SFp)


> matplotlib를 사용하여 표출할 경우에는 `cv2.cvtColor(IMAGE, cv2.COLOR_BGR2RGB)`를 호출하여 BGR 을 RGB로 변경해주어야 합니다. 


## 변환

변환은 물체의 위치를 이동시키는 것입니다. $$(x, y)$$ 방향의 변화를 알고 있다면 $$(t_x, t_y)$$, 다음과 같이 변환 행렬 $$\textbf{M}$$을 만들 수 있습니다 :

$$
M = \begin{bmatrix} 1 & 0 & t_x \\\\ 0 & 1 & t_y  \end{bmatrix}
$$

`np.float32` 유형의 Numpy 배열로 만들고 `cv2.warpAffine()` 함수로 전달할 수 있습니다. 아래 (100,50) 이동하는 아래 예를 참조하세요.

```python
import cv2
import numpy as np

img = cv2.imread('model.jpg', 1)
rows, cols = img.shape[:2]

M = np.float32([[1, 0, 100], [0, 1, 50]])
dst = cv2.warpAffine(img, M, (cols, rows))

cv2.imshow('Original', img)
cv2.imshow('warpAffine', dst)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

> `cv2.warpAffine()` 함수의 세 번째 인수는 출력 이미지의 크기이며, 너비, 높이 형태여야 합니다. `width = 열 수` 및  `height = 행 수`를 기억하세요.

아래 결과를 확인하세요:

![Original](https://goo.gl/16V83L)

![warpAffine](https://goo.gl/ArBbgX)



## 회전

각도 $$\theta$$에 대한 이미지의 회전은 형태의 변환 행렬에 의해 이루어진다.

$$
M = \begin{bmatrix} cos\theta & -sin\theta \\\\ sin\theta & cos\theta   \end{bmatrix}
$$

그러나 OpenCV는 조정 가능한 회전 중심으로 크기가 조정 된 회전을 제공하므로 원하는 위치에서 회전 할 수 있습니다. 수정된 변환 행렬은 아래 수식에 의해 주어진다.

$$
\begin{bmatrix} \alpha &  \beta & (1- \alpha )  \cdot center.x -  \beta \cdot center.y \\\\ - \beta &  \alpha &  \beta \cdot center.x + (1- \alpha )  \cdot center.y \end{bmatrix}
$$

where:

$$
\begin{array}{l} \alpha =  scale \cdot \cos \theta , \\\\ \beta =  scale \cdot \sin \theta \end{array}
$$

이 변환 행렬을 찾기 위해 OpenCV는 `cv2.getRotationMatrix2D` 함수를 제공합니다. 크기 조정없이 중앙을 기준으로 이미지를 90도 회전시키는 아래 예제를 확인하십시오.

```python
import cv2

img = cv2.imread('rotate.jpg', 1)
rows, cols = img.shape[:2]

M = cv2.getRotationMatrix2D((cols / 2, rows / 2), 90, 1)
dst = cv2.warpAffine(img, M, (cols, rows))

cv2.imshow('Original', img)
cv2.imshow('warpAffine', dst)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

결과보기 :

![Original](https://goo.gl/VKisuY)

![warpAffine](https://goo.gl/6UgyPq)


## Affine 변환

Affine 변환에서 원본 이미지의 모든 평행선은 출력 이미지에서 여전히 평행합니다. 변환 행렬을 찾으려면 입력 이미지와 출력 이미지의 해당 위치에서 세 점이 필요합니다. 그런 다음 `cv2.getAffineTransform`은 `cv2.warpAffine`에 전달 될 2x3 행렬을 작성합니다.

아래 예제를 확인하고 선택한 점(녹색으로 표시)을 확인하십시오.

```python
import cv2
import numpy as np
from matplotlib import pyplot as plt

img = cv2.imread('affine.jpg')
rows, cols, ch = img.shape[:3]

pts1 = np.float32([[50, 50], [200, 50], [50, 200]])
pts2 = np.float32([[10, 100], [200, 50], [100, 250]])

M = cv2.getAffineTransform(pts1, pts2)

dst = cv2.warpAffine(img, M, (cols, rows))

plt.subplot(121), plt.imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB)), plt.title('Input')
plt.subplot(122), plt.imshow(cv2.cvtColor(dst, cv2.COLOR_BGR2RGB)), plt.title('Output')
plt.show()

```

결과보기 :

![](https://goo.gl/LymgSu)




## 원근법 변환

원근감 변환의 경우 3x3 변환 행렬이 필요합니다. 직선은 변환 후에도 직선으로 유지됩니다. 이 변환 행렬을 찾으려면 입력 이미지와 출력 이미지의 해당 점에 4 포인트가 필요합니다. 이 4 점 중 3 점은 동일 선상에 있지 않아야 합니다. 그러면 변환 행렬은 함수 `cv2.getPerspectiveTransform`에서 찾을 수 있습니다. 그런 다음이 3x3 변환 행렬로 `cv2.warpPerspective`를 적용하십시오.

아래 코드를 참조하십시오.

```python
import cv2
import numpy as np
from matplotlib import pyplot as plt

img = cv2.imread('sudoku.jpg')
rows, cols, ch = img.shape[:3]

# pts1 = np.float32([[56, 65], [368, 52], [28, 387], [389, 390]])
pts1 = np.float32([[101, 131], [500, 114], [66, 504], [544, 489]])
pts2 = np.float32([[0, 0], [300, 0], [0, 300], [300, 300]])

M = cv2.getPerspectiveTransform(pts1, pts2)

dst = cv2.warpPerspective(img, M, (300, 300))

plt.subplot(121), plt.imshow(img), plt.title('Input')
plt.subplot(122), plt.imshow(dst), plt.title('Output')
plt.show()
```
결과:


![](https://goo.gl/JYXXN2)

> np.float32([[101, 131], [500, 114], [66, 504], [544, 489]]) 에서 배열은 좌상, 우상, 좌하, 우하 의 순으로 포인트를 나열한 것입니다.

![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/perspective.jpg)

## 추가 리소스

- `컴퓨터 비전 : 알고리즘 및 응용`, Richard Szeliski (“Computer Vision: Algorithms and Applications”, Richard Szeliski)






## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_thresholding/py_thresholding.html
- http://opencv-python.readthedocs.io/en/latest/#
- https://blog.naver.com/PostView.nhn?blogId=samsjang&logNo=220504966397&parentCategoryNo=&categoryNo=66&viewDate=&isShowPopularPosts=false&from=postList



<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>




