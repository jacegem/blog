---
title: '[OpenCV-Python Tutorials] 11. 이미지 임계값'
date: 2018-01-11 19:15:03
tags:
---

# [OpenCV-Python Tutorials] 11. 이미지 임계값

## 목표

- 이 튜토리얼에서는 Simple thresholding, Adaptive thresholding, Otsu의 thresholding 등을 배웁니다.
- `cv2.threshold`, `cv2.adaptiveThreshold` 등의 함수를 배웁니다.

## 단순 임계값

여기서 문제는 간단합니다. 픽셀 값이 임계 값보다 크면 하나의 값 (흰색 일 수 있음)이 지정되고, 그렇지 않으면 다른 값 (검정 일 수 있음)이 지정됩니다. 사용 된 함수는 `cv2.threshold`입니다. 

첫 번째 인수는 원본 이미지이며 `그레이 스케일 이미지여야만` 합니다. 두 번째 인수는 픽셀 값을 분류하는 데 사용되는 임계 값입니다. 세 번째 인수는 픽셀 값이 임계 값보다 크거나 (때로는 작지 만) 주어질 값을 나타내는 maxVal입니다. OpenCV는 다양한 스타일의 임계 값을 제공하며 함수의 네 번째 매개 변수에 의해 결정됩니다. 다른 유형은 다음과 같습니다.

- cv2.THRESH_BINARY
- cv2.THRESH_BINARY_INV
- cv2.THRESH_TRUNC
- cv2.THRESH_TOZERO
- cv2.THRESH_TOZERO_INV

문서는 각 유형의 의미를 명확히 설명합니다. 문서를 확인하십시오.

2 개의 출력이 얻어진다. 첫 번째는 나중에 설명 할 `retval`입니다. 두 번째 출력은 `임계값 이미지`입니다.

코드:

```python
import cv2
from matplotlib import pyplot as plt

img = cv2.imread('gradient.jpg', 0)
ret, thresh1 = cv2.threshold(img, 127, 255, cv2.THRESH_BINARY)
ret, thresh2 = cv2.threshold(img, 127, 255, cv2.THRESH_BINARY_INV)
ret, thresh3 = cv2.threshold(img, 127, 255, cv2.THRESH_TRUNC)
ret, thresh4 = cv2.threshold(img, 127, 255, cv2.THRESH_TOZERO)
ret, thresh5 = cv2.threshold(img, 127, 255, cv2.THRESH_TOZERO_INV)

titles = ['Original Image', 'BINARY', 'BINARY_INV', 'TRUNC', 'TOZERO', 'TOZERO_INV']
images = [img, thresh1, thresh2, thresh3, thresh4, thresh5]

for i in range(6):
    plt.subplot(2, 3, i + 1), plt.imshow(images[i], 'gray')
    plt.title(titles[i])
    plt.xticks([]), plt.yticks([])

plt.show()
```

> 여러 이미지를 플롯하기 위해 `plt.subplot()` 함수를 사용했습니다. 자세한 내용은 Matplotlib 문서를 확인하십시오.

결과는 다음과 같습니다.

![Original Image](https://goo.gl/YGKS6e)

![](https://goo.gl/oxm8bT)


```python
def threshold(src, thresh, maxval, type, dst=None): # real signature unknown; restored from __doc__
    """ threshold(src, thresh, maxval, type[, dst]) -> retval, dst """
    pass
```


## 적응형 임계값

이전 섹션에서는 임계 값으로 전역 값을 사용했습니다. 그러나 이미지가 다른 영역에서 다른 조명 조건을 갖는 모든 조건에서 좋지 않을 수 있습니다. 이 경우 적응형 임계값을 찾습니다. 이 알고리즘은 이미지의 작은 영역에 대한 임계값을 계산합니다. 따라서 동일한 이미지의 서로 다른 영역에 대해 서로 다른 임계값을 얻을 수 있으며 다양한 조명을 사용하여 이미지를 더 나은 결과를 얻을 수 있습니다.

3 개의 `'특수'입력` 매개 변수와 하나의 출력 인수만 있습니다.

`적응 방법` - 임계 값 계산 방법을 결정합니다.
- cv2.ADAPTIVE_THRESH_MEAN_C : 임계값은 인접 지역의 평균입니다.
- cv2.ADAPTIVE_THRESH_GAUSSIAN_C : 임계값은 가중치 가우시안 윈도우인 이웃값의 가중치 합입니다.

`Block Size` - 인접 지역의 크기를 결정합니다.

`C` - 계산 된 평균 또는 가중 평균에서 빼는 상수입니다.

아래 코드는 다양한 조명을 사용하는 이미지의 전역 임계값과 적응 임계값을 비교합니다.

```python
import cv2
from matplotlib import pyplot as plt

img = cv2.imread('sunrise.jpg', 0)
img = cv2.medianBlur(img, 5)

ret, th1 = cv2.threshold(img, 127, 255, cv2.THRESH_BINARY)
th2 = cv2.adaptiveThreshold(img, 255, cv2.ADAPTIVE_THRESH_MEAN_C, cv2.THRESH_BINARY, 11, 2)
th3 = cv2.adaptiveThreshold(img, 255, cv2.ADAPTIVE_THRESH_GAUSSIAN_C, cv2.THRESH_BINARY, 11, 2)

titles = ['Original Image', 'Global Thresholding (v = 127)',
          'Adaptive Mean Thresholding', 'Adaptive Gaussian Thresholding']
images = [img, th1, th2, th3]

for i in range(4):
    plt.subplot(2, 2, i + 1), plt.imshow(images[i], 'gray')
    plt.title(titles[i])
    plt.xticks([]), plt.yticks([])

plt.show()

```

![Original Image](https://goo.gl/GwkEtu)

![adaptiveThreshold](https://goo.gl/LrYtzY)

```python
def adaptiveThreshold(src, maxValue, adaptiveMethod, thresholdType, blockSize, C, dst=None): # real signature unknown; restored from __doc__
    """ adaptiveThreshold(src, maxValue, adaptiveMethod, thresholdType, blockSize, C[, dst]) -> dst """
    pass
```


## Otsu 이선화

첫 번째 섹션에서는 `retVal`이라는 두 번째 매개 변수가 있다고 말씀드렸습니다. 오츠(Otsu)의 바이너리화(Binarization)를 할 때 사용됩니다. 무엇일까요?

전역 thresholding에서는 임계값에 임의의 값을 사용했습니다. 그렇다면 우리가 선택한 가치가 얼마나 좋은지 알 수 있습니까? 답은 시행 착오 방법입니다. 그러나 `bimodal 이미지`를 고려하십시오 (간단히 말하면, bimodal 이미지는 히스토그램에 두 개의 피크가 있는 이미지입니다). 그 이미지의 경우, 우리는 임계값으로 그 피크의 중간에서 값을 취할 수 있습니다. 맞습니까? Otsu 이진화가 그것이다. 따라서 간단한 말로, bimodal 이미지의 이미지 히스토그램으로부터 임계값을 자동으로 계산합니다. (bimodal이 아닌 이미지의 경우 이진화가 정확하지 않습니다.)

이를 위해 우리의 `cv2.threshold()` 함수가 사용되었지만 여분의 플래그인 `cv2.THRESH_OTSU`를 전달합니다. `임계값의 경우 단순히 0을 전달`하십시오. 그런 다음 알고리즘은 최적임계 값을 찾아서 두 번째 출력인 `retVal`로 반환합니다. Otsu thresholding이 사용되지 않으면 retVal은 사용 된 임계값과 같습니다.

아래 예제를 확인하십시오. 입력 이미지는 잡음이 많은 이미지입니다. 첫 번째 경우에는 전역 임계값을 127로 적용했습니다. 두 번째 경우에는 Otsu의 임계값을 직접 적용했습니다. 세 번째 경우에는 노이즈를 제거하기 위해 5x5 가우시안 커널로 이미지를 필터링한 다음 `Otsu thresholding`을 적용했습니다. 노이즈 필터링이 어떻게 결과를 향상시키는 확인하십시오.


```python
import cv2
from matplotlib import pyplot as plt

img = cv2.imread('model.jpg', 0)

# global thresholding
ret1, th1 = cv2.threshold(img, 127, 255, cv2.THRESH_BINARY)

# Otsu's thresholding
ret2, th2 = cv2.threshold(img, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)

# Otsu's thresholding after Gaussian filtering
blur = cv2.GaussianBlur(img, (5, 5), 0)
ret3, th3 = cv2.threshold(blur, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)

# plot all the images and their histograms
images = [img, 0, th1,
          img, 0, th2,
          blur, 0, th3]
titles = ['Original Noisy Image', 'Histogram', 'Global Thresholding (v=127)',
          'Original Noisy Image', 'Histogram', "Otsu's Thresholding",
          'Gaussian filtered Image', 'Histogram', "Otsu's Thresholding"]

for i in range(3):
    plt.subplot(3, 3, i * 3 + 1), plt.imshow(images[i * 3], 'gray')
    plt.title(titles[i * 3]), plt.xticks([]), plt.yticks([])
    plt.subplot(3, 3, i * 3 + 2), plt.hist(images[i * 3].ravel(), 256)
    plt.title(titles[i * 3 + 1]), plt.xticks([]), plt.yticks([])
    plt.subplot(3, 3, i * 3 + 3), plt.imshow(images[i * 3 + 2], 'gray')
    plt.title(titles[i * 3 + 2]), plt.xticks([]), plt.yticks([])
plt.show()
```

결과:

![](https://goo.gl/tkpc4n)

![원본 이미지](https://goo.gl/NRX1TV)


## Otsu 이진화 동작 방법

이 섹션에서는 실제로 작동하는 방법을 보여주기 위해 Python이 Otsu의 이진화를 구현하는 방법을 보여줍니다. 관심이 없으면 이것을 건너 뛸 수 있습니다.

Otsu의 알고리즘은 bimodal 이미지로 작업하기 때문에 관계에 의해 주어진 `가중 클래스 내 분산`을 최소화하는 임계 값 (t)을 찾으려고합니다.

$$
\sigma_w^2(t) = q_1(t)\sigma_1^2(t)+q_2(t)\sigma_2^2(t)
$$

where

$$
\begin{array}{l}
q_1(t) = \sum_{i=1}^{t} P(i) \quad \& \quad q_1(t) = \sum_{i=t+1}^{I} P(i) 

\mu_1(t) = \sum_{i=1}^{t} \frac{iP(i)}{q_1(t)} \quad \& \quad \mu_2(t) = \sum_{i=t+1}^{I} \frac{iP(i)}{q_2(t)} 

\sigma_1^2(t) = \sum_{i=1}^{t} [i-\mu_1(t)]^2 \frac{P(i)}{q_1(t)} \quad \& \quad \sigma_2^2(t) = \sum_{i=t+1}^{I} [i-\mu_1(t)]^2 \frac{P(i)}{q_2(t)}
\end{array}
$$

실제로 두 개의 피크 사이에있는 t의 값을 찾음으로써 두 클래스에 대한 분산이 최소가 되도록 합니다. 다음과 같이 간단하게 파이썬으로 구현할 수 있습니다 :

```python
import cv2
import numpy as np

img = cv2.imread('model.jpg', 0)
blur = cv2.GaussianBlur(img, (5, 5), 0)

# find normalized_histogram, and its cumulative distribution function
hist = cv2.calcHist([blur], [0], None, [256], [0, 256])
hist_norm = hist.ravel() / hist.max()
Q = hist_norm.cumsum()

bins = np.arange(256)

fn_min = np.inf
thresh = -1

for i in range(1, 256):
    p1, p2 = np.hsplit(hist_norm, [i])  # probabilities
    q1, q2 = Q[i], Q[255] - Q[i]  # cum sum of classes
    b1, b2 = np.hsplit(bins, [i])  # weights

    # finding means and variances
    m1, m2 = np.sum(p1 * b1) / q1, np.sum(p2 * b2) / q2
    v1, v2 = np.sum(((b1 - m1) ** 2) * p1) / q1, np.sum(((b2 - m2) ** 2) * p2) / q2

    # calculates the minimization function
    fn = v1 * q1 + v2 * q2
    if fn < fn_min:
        fn_min = fn
        thresh = i

# find otsu's threshold value with OpenCV function
ret, otsu = cv2.threshold(blur, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)

print(thresh, ret)
```

(일부 기능은 여기에서 새로 추가되었지만 다음 장에서 설명 할 것입니다.)

## 추가 리소스

디지털 이미지 프로세싱, Rafael C. Gonzalez

## 연습 문제

Otsu의 이진화에 사용할 수있는 최적화가 있습니다. 검색하고 구현할 수 있습니다.




## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_thresholding/py_thresholding.html
- https://pixabay.com



<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>



