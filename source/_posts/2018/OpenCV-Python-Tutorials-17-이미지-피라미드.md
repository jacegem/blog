---
title: '[OpenCV-Python Tutorials] 17. 이미지 피라미드'
date: 2018-01-17 19:17:09
tags:
---

# [OpenCV-Python Tutorials] 17. 이미지 피라미드

## 목표

- 이미지 피라미드에 대해 알아보겠습니다.
- Image 피라미드를 사용하여 새로운 이미지를 만들어볼게요
- 함수: `cv2.pyrUp()`, `cv2.pyrDown()`

## 이론

일반적으로 우리는 일정한 크기의 이미지로 작업했습니다. 그러나 어떤 경우에는 같은 이미지의 다른 해상도의 이미지로 작업해야 합니다. 예를 들어, 얼굴과 같은 이미지에서 무언가를 검색하는 동안 이미지에 어떤 크기의 물체가 나타날지 확신 할 수 없습니다. 이 경우, 우리는 해상도가 다른 일련의 이미지를 만들고 모든 이미지에서 객체를 검색해야합니다. 서로 다른 해상도의 이미지 세트를 `Image Pyramids`라고 부릅니다. 왜냐하면 피라미드처럼 맨 아래에 가장 큰 이미지가 가장 큰 이미지가 있는 스택에 보관되어 있기 때문입니다.

이미지 피라미드에는 두 가지 종류가 있습니다. 

1. 가우시안 피라미드
2. 라플라시안 피라미드

Gaussian 피라미드의 높은 레벨(저해상도)은 낮은 레벨(고해상도) 이미지에서 연속적인 행과 열을 제거하여 생성합니다.  이렇게 하면, $$ M \times N $$ 이미지는 $$M/2 \times N/2$$ 이미지가 됩니다. 따라서 면적은 원래 면적의 1/4로 줄어 듭니다. 그것은 `옥타브`라고 합니다. 피라미드에서 위쪽으로 갈수록 같은 패턴이 계속됩니다 (즉, 해상도가 감소합니다). 마찬가지로 확장하면서 면적은 각 단계에서 4 배가 됩니다. `cv2.pyrDown()` 및 `cv2.pyrUp()` 함수를 사용하여 가우시안 피라미드를 찾을 수 있습니다.

```python
import cv2

img = cv2.imread('model.jpg')
titles = ['orginal', 'level1', 'level2', 'level3']

images = []
images.append(img)

for i in range(3):
    img = cv2.pyrDown(img)
    images.append(img)

for i in range(4):
    cv2.imshow(titles[i], images[i])

cv2.waitKey(0)
cv2.destroyAllWindows()
```

아래는 이미지 피라미드의 4 단계입니다.

![original](https://goo.gl/Kdokqq)

![level1](https://goo.gl/A8LmZt)

![level2](https://goo.gl/aNJ4yb)

![level3](https://goo.gl/SBJnHn)


이제 `cv2.pyrUp()` 함수를 사용하여 이미지 피라미드로 이동할 수 있습니다.

```python
higher_reso2 = cv2.pyrUp(lower_reso)
```

```python
import cv2

img = cv2.imread('small.jpg')
titles = ['orginal', 'level1', 'level2', 'level3']

images = []
images.append(img)

for i in range(3):
    img = cv2.pyrUp(img)
    images.append(img)

for i in range(4):
    cv2.imshow(titles[i], images[i])

cv2.waitKey(0)
cv2.destroyAllWindows()
```

![original](https://goo.gl/FZLunw)

![level1](https://goo.gl/WSCGr2)

![level2](https://goo.gl/1HZJSb)


Laplacian 피라미드는 Gaussian Pyramids에서 형성됩니다. 거기에 대한 독점적인 기능은 없습니다. Laplacian 피라미드 이미지는 가장자리 이미지와 같습니다. 요소의 대부분은 0입니다. 이미지 압축에 사용됩니다. 라플라시안 피라미드의 레벨은 가우시안 피라미드의 해당 레벨과 가우시안 피라미드의 상위 레벨 확장 버전의 차이에 의해 형성됩니다. Laplacian 레벨의 세 가지 레벨은 아래와 같이 보일 것입니다 (내용을 향상시키기 위해 대비가 조정 됨).


```python
import cv2

img = cv2.imread('large.jpg')
titles = ['orginal', 'level1', 'level2', 'level3']

images = []
images.append(img)

for i in range(3):
    img = cv2.pyrDown(img)
    images.append(img)

for i in range(3):
    resize = cv2.resize(images[i]
                        , dsize=(images[i+1].shape[1], images[i+1].shape[0])
                        , interpolation=cv2.INTER_CUBIC)
    images[i] = cv2.subtract(resize, images[i+1])
    cv2.imshow(titles[i], images[i])

cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](https://goo.gl/AKdhSm)

![](https://goo.gl/qSVRya)

![](https://goo.gl/oDqMXS)



## 피라미드를 사용한 이미지 블렌딩

피라미드의 한 응용 프로그램은 이미지 블렌딩입니다. 예를 들어, 이미지 바느질에서는 두 이미지를 함께 쌓아야 하지만 이미지간에 불연속성으로 인해 잘 보이지 않을 수 있습니다. 이 경우 피라미드로 이미지 블렌딩을 하면 이미지에 많은 데이터를 남기지 않고도 원활한 혼합이 가능합니다. 하나의 고전적인 예는 Orange와 Apple의 두 과일을 혼합 한 것입니다. 지금 하고 있는 것을 이해하기 위해 지금 결과를 확인해보세요

![](https://opencv-python-tutroals.readthedocs.io/en/latest/_images/orapple.jpg)

추가 리소스의 첫 번째 참조를 확인하십시오. 이미지 블렌딩, Laplacian Pyramids 등의 전체 다이어그램 세부 정보가 있습니다. 간단히 말해서 다음과 같이 수행됩니다.

- 사과와 오렌지의 두 이미지 로드
- 사과와 오렌지에 대한 가우시안 피라미드를 찾으십시오 (이 예제에서는 레벨 수가 6입니다).
- 가우시안 피라미드에서 라플라시안 피라미드를 찾으십시오.
- 이제 Laplacian Pyramids의 각 레벨에서 사과의 왼쪽 절반과 오렌지의 오른쪽 절반에 합칩니다.
- 마지막으로이 공동 이미지 피라미드에서 원래 이미지를 재구성하십시오.

전체 코드는 다음과 같습니다. (간단히하기 위해 각 단계가 별도로 수행되므로 더 많은 메모리가 필요할 수 있습니다. 원하는 경우 최적화 할 수 있습니다).

```python
import cv2
import numpy as np

A = cv2.imread('resize_a.jpg')
B = cv2.imread('resize_b.jpg')

B = cv2.resize(B, dsize=(A.shape[1], A.shape[0]), interpolation=cv2.INTER_CUBIC)

# generate Gaussian pyramid for A
G = A.copy()
gpA = [G]
for i in range(6):
    G = cv2.pyrDown(G)
    gpA.append(G)

# generate Gaussian pyramid for B
G = B.copy()
gpB = [G]
for i in range(6):
    G = cv2.pyrDown(G)
    gpB.append(G)

# generate Laplacian Pyramid for A
lpA = [gpA[5]]
for i in range(5, 0, -1):
    GE = cv2.pyrUp(gpA[i])
    L = cv2.subtract(gpA[i - 1], GE)
    lpA.append(L)

# generate Laplacian Pyramid for B
lpB = [gpB[5]]
for i in range(5, 0, -1):
    GE = cv2.pyrUp(gpB[i])
    L = cv2.subtract(gpB[i - 1], GE)
    lpB.append(L)

# Now add left and right halves of images in each level
LS = []
for la, lb in zip(lpA, lpB):
    rows, cols, dpt = la.shape
    ls = np.hstack((la[:, 0:int(cols / 2)], lb[:, int(cols / 2):]))
    LS.append(ls)

# now reconstruct
ls_ = LS[0]
for i in range(1, 6):
    ls_ = cv2.pyrUp(ls_)
    ls_ = cv2.add(ls_, LS[i])

# image with direct connecting each half
real = np.hstack((A[:, :int(cols / 2)], B[:, int(cols / 2):]))

cv2.imwrite('Pyramid_blending2.jpg', ls_)
cv2.imwrite('Direct_blending.jpg', real)
```


> 두 이미지의 크기가 동일해야 합니다. 
> `ls = np.hstack((la[:, 0:int(cols / 2)], lb[:, int(cols / 2):]))` 에서 정수값이 입력되야 하므로, `int()` 를 사용하였습니다.

![Direct_blending](https://goo.gl/hBNGXr)

![Pyramid_blending2](https://lh3.googleusercontent.com/-clhOritegYQ/WY0AelkVJ2I/AAAAAAAAEuk/G7fKKbKrW9kisAnAeWTzeCv1VLepvGd6ACHMYCw/s0/Pyramid_blending2.jpg)



## Additional Resources

- [Image Blending](http://pages.cs.wisc.edu/~csverma/CS766_09/ImageMosaic/imagemosaic.html)




## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_pyramids/py_pyramids.html
- https://blog.naver.com/PostView.nhn?blogId=samsjang&logNo=220508552078&parentCategoryNo=&categoryNo=66&viewDate=&isShowPopularPosts=false&from=postList





<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>

















