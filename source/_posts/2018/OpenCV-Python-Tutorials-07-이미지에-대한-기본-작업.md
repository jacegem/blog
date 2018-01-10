---
title: '[OpenCV-Python Tutorials 07] 이미지에 대한 기본 작업'
date: 2018-01-05 19:12:36
tags:
---

# [OpenCV-Python Tutorials 07] 이미지에 대한 기본 작업

모든 파일은 [Github](https://github.com/jacegem/OpenCV-Python-Tutorials)에서 확인 할 수 있습니다.

## 목표

배울 내용:

- 픽셀 값에 액세스 및 수정
- 이미지 속성에 액세스
- 이미지 영역 설정 (ROI: Region of Image)
- 이미지 분할 및 병합

이 섹션의 거의 모든 작업은 주로 OpenCV보다는 Numpy와 관련이 있습니다. OpenCV로 더 최적화 된 코드를 작성하기 위해서는 Numpy에 대한 좋은 지식이 필요합니다.

(예제는 단일 라인 코드이기 때문에 Python 터미널에 표시되는 내용입니다.)

## 픽셀 값 액세스 및 수정

먼저 컬러 이미지를 로드 해 봅시다.

```python
>>> import cv2
>>> import numpy as np

>>> img = cv2.imread('messi5.jpg')
```

행 및 열 좌표로 픽셀 값에 액세스 할 수 있습니다. `BGR` 이미지의 경우 Blue, Green, Red 값의 배열을 반환합니다. 회색 음영 이미지의 경우 해당 강도 만 반환됩니다.

```python
>>> px = img[100,100]
>>> print px
[178 197 205]

# accessing only blue pixel
>>> blue = img[100,100,0]
>>> print blue
178
```

`img[100,100,0]` 에서 배열 3번째 값이 `0`이므로 blue 입니다.

BGR 이므로 아래와 같습니다.

- 0: Blue
- 1: Green
- R: Red


같은 방법으로 픽셀값을 수정할 수 있습니다.

```python
>>> img[100,100] = [255,255,255]
>>> print img[100,100]
[255 255 255]
```

전체 코드는 다음과 같습니다.

```python
import cv2
import numpy as np

image_file = 'ball_image.jpg'
img = cv2.imread(image_file)

px = img[100, 100]
print(px)

blue = img[100,100,0]
print(blue)

img[100,100] = [255,255,255]
print(img[100,100])
```


> Numpy는 빠른 배열 계산을 위해 최적화된 라이브러리입니다. 따라서 각 픽셀 값에 액세스하고 수정하는 것은 매우 느릴 것이며 권장하지 않습니다.

> 위에서 언급 한 방법은 일반적으로 배열 영역을 선택하는 데 사용됩니다. 예를 들어 처음 5 행과 마지막 3 열을 말합니다. 개별 픽셀에 액세스하는 경우 Numpy 배열 메서드인 `array.item()` 및 `array.itemset()`을 사용하는 것이 더 좋습니다. 하지만 항상 `스칼라`를 반환합니다. 따라서 모든 B, G, R 값에 액세스하려면 `array.item()`을 개별적으로 호출해야합니다.
> 

더 나은 픽셀 액세스 및 편집 방법 :

```python
# accessing RED value
>>> img.item(10,10,2)
59

# modifying RED value
>>> img.itemset((10,10,2),100)
>>> img.item(10,10,2)
100
```

## 이미지 속성에 액세스하기

이미지 속성에는 행 수, 열 및 채널, 이미지 데이터 유형, 픽셀 수 등이 포함됩니다.

이미지의 모양은 `img.shap`e에 의해 액세스됩니다. 행, 열 및 채널 수의 튜플을 반환합니다 (이미지가 색상인 경우).

```python
>>> print img.shape
(342, 548, 3)
```

> 이미지가 회색조인 경우 반환되는 튜플에는 행과 열만 포함됩니다. 따라서 로드 된 이미지가 회색조 또는 컬러 이미지인지 확인하는 좋은 방법입니다.

`img.size`는 총 픽셀 수에 액세스합니다.

```python
>>> print img.size
562248
```

이미지 데이터 유형은 `img.dtype`를 통해서 얻을 수 있습니다.

```python
>>> print img.dtype
uint8
```

> OpenCV-Python 코드에서 많은 수의 오류가 잘못된 `데이터 유형`으로 인해 발생하기 때문에 `img.dtype`은 디버깅하는 동안 매우 중요합니다.

터미널이 아닌 소스파일로 작성한 전체 코드는 다음과 같습니다.

```python
import cv2
import numpy as np

image_file = 'ball_image.jpg'
img = cv2.imread(image_file)

print("shape:" + str(img.shape))
print("size:" + str(img.size))
print("dtype:" + str(img.dtype))
```

결과

```python
shape:(205, 246, 3)
size:151290
dtype:uint8
```

## 이미지 ROI

때로는 이미지의 특정 영역을 가지고 작업을 해야 합니다. 이미지에서 눈을 검색하려면 먼저 얼굴을 찾을 때까지 이미지에서 얼굴 검색을 수행 한 다음 얼굴 영역 내에서 눈을 검색하십시오. 이 방법은 정확도와 성능 향상시킵니다 (눈이 항상 얼굴에 있기 때문에 우리는 작은 영역만 검색을 수행하면 됩니다)

ROI는 Numpy 색인을 사용하여 다시 얻습니다. 여기서 공을 선택하여 이미지의 다른 영역으로 복사합니다.

```python
>>> ball = img[280:340, 330:390]
>>> img[273:333, 100:160] = ball
```

전체 코드는 다음과 같습니다.

```python
import cv2

image_file = 'ball_image.jpg'
img = cv2.imread(image_file)

ball = img[0:113, 349:459]
img[100:213, 449:559] = ball

cv2.imshow('image', img)
cv2.waitKey(0)
cv2.destroyAllWindows()

```

결과

![](https://goo.gl/zRqz81)


## 이미지 채널 분할 및 병합

필요한 경우 이미지의 B, G, R 채널을 개별 평면으로 분할 할 수 있습니다. 그런 다음 개별 채널을 다시 병합하여 BGR 이미지를 다시 형성 할 수 있습니다. 이것은 다음과 같이 수행 할 수 있습니다.

```python
>>> b,g,r = cv2.split(img)
>>> img = cv2.merge((b,g,r))
```

또는 

```python
>>> b = img[:,:,0]
```

빨간색 픽셀을 모두 0으로 만들고 싶다면, 이렇게 분할하고 0으로 놓을 필요가 없습니다. 더 빠르다고 말하는 Numpy 색인 생성을 간단하게 사용할 수 있습니다.

```python
img[:,:,2] = 0
```

`cv2.split()`은 값 비싼 연산이므로 (시간의 관점에서) 필요한 경우에만 사용하십시오. Numpy 색인 생성은 훨씬 효율적이기 때문에 가능하면 사용해야 합니다.

## 이미지에 테두리 만들기 (안쪽 여백 만들기)

포토 프레임과 같은 이미지 주위에 테두리를 만들려면 `cv2.copyMakeBorder()` 함수를 사용할 수 있습니다. 그러나 이 함수는 컨볼루션 연산, 제로 패딩 (zero padding) 등의 애플리케이션이 더 많습니다. 이 함수는 다음과 같은 인수를 취합니다.

- src - 입력 이미지
- 위쪽, 아래쪽, 왼쪽, 오른쪽 - 경계의 폭 (해당 방향의 픽셀 수)
- borderType - 추가되는 경계의 종류를 정의하는 플래그. 다음 유형이 될 수 있습니다.
    - cv2.BORDER_CONSTANT - 일정한 색상의 테두리를 추가합니다. 값은 다음 인수로 제공되어야합니다.
    - cv2.BORDER_REFLECT - 테두리는 다음과 같이 테두리 요소를 반영합니다. fedcba|abcdefgh|hgfedcb
    - cv2.BORDER_REFLECT_101 또는 cv2.BORDER_DEFAULT - 위와 동일하지만 다음과 같이 약간 변경되었습니다. gfedcb|abcdefgh|gfedcba
    - cv2.BORDER_REPLICATE - 마지막 요소는 다음과 같이 전체적으로 복제됩니다. aaaaaa|abcdefgh|hhhhhhh
    - cv2.BORDER_WRAP - 설명 할 수 없습니다. 다음과 같이 보일 것이다 : cdefgh|abcdefgh|abcdefg


- value - 경계 형이 `cv2.BORDER_CONSTANT`의 경우는 경계의 색


> `abcdefgh` 는 원본을 의미합니다.
> fedcba`|`abcdefgh`|`hgfedcb 는 세로줄 `|`을 기준으로 양쪽으로 반영된 상태를 표시합니다. 원본이 abcdefgh 이므로, 왼쪽에 fedcba 의 순서로 반영된 이미지가 나오고 오른쪽에는 hgfedcb 으로 반영된 이미지가 나오는 것을 표시하고 있습니다.



다음은 더 나은 이해를 위해 이러한 모든 경계 유형을 보여주는 샘플 코드입니다.

```python
import cv2
import numpy as np
from matplotlib import pyplot as plt

BLUE = [255, 0, 0]

image_file = 'opencv-logo-white.png'
img1 = cv2.imread(image_file)

pixel = 100

replicate = cv2.copyMakeBorder(img1, pixel, pixel, pixel, pixel, cv2.BORDER_REPLICATE)
reflect = cv2.copyMakeBorder(img1, pixel, pixel, pixel, pixel, cv2.BORDER_REFLECT)
reflect101 = cv2.copyMakeBorder(img1, pixel, pixel, pixel, pixel, cv2.BORDER_REFLECT_101)
wrap = cv2.copyMakeBorder(img1, pixel, pixel, pixel, pixel, cv2.BORDER_WRAP)
constant = cv2.copyMakeBorder(img1, pixel, pixel, pixel, pixel, cv2.BORDER_CONSTANT, value=BLUE)

plt.subplot(231), plt.imshow(img1, 'gray'), plt.title('ORIGINAL')
plt.subplot(232), plt.imshow(replicate, 'gray'), plt.title('REPLICATE')
plt.subplot(233), plt.imshow(reflect, 'gray'), plt.title('REFLECT')
plt.subplot(234), plt.imshow(reflect101, 'gray'), plt.title('REFLECT_101')
plt.subplot(235), plt.imshow(wrap, 'gray'), plt.title('WRAP')
plt.subplot(236), plt.imshow(constant, 'gray'), plt.title('CONSTANT')

plt.show()
```

아래 결과를보십시오. (이미지는 matplotlib과 함께 표시되므로 RED 및 BLUE 평면이 상호 교환됩니다.)

![](https://goo.gl/9wK5jC)




## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_core/py_basic_ops/py_basic_ops.html#basic-ops


<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>


