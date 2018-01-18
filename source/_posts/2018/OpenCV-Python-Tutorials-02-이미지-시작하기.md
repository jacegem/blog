---
title: '[OpenCV-Python Tutorials 02] 이미지 시작하기'
date: 2018-01-02 19:10:35
tags: [opencv, python, tutorial]
category:
- Programming
- Python
---

# [OpenCV-Python Tutorials 02] 이미지 시작하기

모든 파일은 [Github](https://github.com/jacegem/OpenCV-Python-Tutorials)에서 확인 할 수 있습니다.

## 목표

- 여기에서 이미지를 읽는 방법, 이미지를 표시하는 방법 및 이미지를 다시 저장하는 방법에 대해 배웁니다.
- 다음 함수를 배웁니다. cv2.imread(), cv2.imshow(), cv2.imwrite()
- 선택적으로 Matplotlib을 사용하여 이미지를 표시하는 방법을 배우게됩니다.

## OpenCV 사용

### 이미지 읽기

이미지를 읽으려면 `cv2.imread()` 함수를 사용하십시오. 이미지는 작업 디렉토리에 있거나 이미지의 전체 경로가 주어져야 합니다.

두 번째 인수는 이미지를 읽어야하는 방법을 지정하는 플래그입니다.

- cv2.IMREAD_COLOR : 컬러 이미지를 로드합니다. 이미지의 투명성은 무시됩니다. 기본 플래그입니다.
- cv2.IMREAD_GRAYSCALE : 이미지를 회색조 모드로 로드합니다.
- cv2.IMREAD_UNCHANGED : 알파 채널을 포함하여 이미지를 로드합니다.

> 이 세 개의 플래그 대신 정수 1, 0 또는 -1을 각각 전달할 수 있습니다.

아래 코드를 참조하십시오.

```python
import numpy as np
import cv2

# Load an color image in grayscale
file = '01_model.jpg'
img = cv2.imread(file, 0)
```

> 이미지 경로가 잘못 되어도 오류는 발생하지 않지만 img(`print img`)는 아무것도(`None`) 표시되지 않습니다.



## 이미지 표시

창에 이미지를 표시하려면 `cv2.imshow()` 함수를 사용하십시오. 창은 이미지 크기에 자동으로 맞춰집니다.

첫 번째 파라미터는 창 이름을 나타내는 문자열입니다. 두 번째 파라미터는 출력할 이미지입니다. 원하는 만큼 창을 만들 수 있지만 창이름은 다르게 해야 합니다.

```python
import numpy as np
import cv2

# Load an color image in grayscale
file = '01_model.jpg'
img = cv2.imread(file, 0)

cv2.imshow('image', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

윈도우의 스크린 샷은 다음과 같습니다.

![](https://goo.gl/ZDWpCQ)

`cv2.imread(file, 0)` 호출시에 두번째 파라미터로 `0`을 전달하여 그레이스케일 이미지로 출력됩니다. 

```python
import numpy as np
import cv2

# 컬러 이미지를 로드 합니다. 
file = '01_model.jpg'
img = cv2.imread(file, cv2.IMREAD_COLOR)

cv2.imshow('image', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

두번째 파라미터에 `cv2.IMREAD_COLOR`를 전달하여 컬러 이미지로 출력되도록 변경합니다. 

![](https://goo.gl/UhQyQE)

`cv2.waitKey()`는 키보드 바인딩 함수입니다. 인수는 밀리 초 단위의 시간입니다. 이 함수는 키보드 이벤트에 대해 지정된 밀리 초를 기다립니다. 그 시간에 아무 키나 누르면 프로그램이 계속됩니다. 0을 전달하면 키 스트로크가 무기한 대기됩니다. 키 `a`가 눌려 졌을 때 등과 같은 특정 키 스트로크를 감지하도록 설정할 수도 있습니다.

`cv2.destroyAllWindows()`는 우리가 만든 모든 창을 단순히 파괴합니다. 특정 윈도우를 파기하려면 정확한 윈도우 이름을 인수로 전달하는 `cv2.destroyWindow()` 함수를 사용하십시오.

이미 창을 만들고 나중에 이미지를 로드 하는 특별한 경우가 있습니다. 이 경우 창 크기를 조정할 수 있는지 여부를 지정할 수 있습니다. 이것은 `cv2.namedWindow()` 함수로 처리됩니다.

플래그 기본값은 `cv2.WINDOW_AUTOSIZE`입니다. 그러나 플래그를 `cv2.WINDOW_NORMAL`로 지정하면 윈도우의 크기를 조정할 수 있습니다. 이미지의 크기가 너무 크고 트랙 바를 윈도우에 추가 할 때 유용합니다.

아래 코드를 참조하십시오.

```python
cv2.namedWindow('image', cv2.WINDOW_NORMAL)
cv2.imshow('image',img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

## 이미지 저장

이미지를 저장하려면 `cv2.imwrite()` 함수를 사용하십시오.

첫 번째 인수는 파일 이름이고, 두 번째 인수는 저장할 이미지입니다.

```python
import numpy as np
import cv2

# 컬러 이미지를 로드 합니다.
file = '01_model.jpg'
img = cv2.imread(file, cv2.IMREAD_COLOR)

cv2.imwrite('04_image_write_output.png', img)
```

그러면 작업 디렉토리에 이미지가 PNG 형식으로 저장됩니다.

![](https://goo.gl/5qjVya)

작업폴더에 파일이 생성되는 것을 확인할 수 있습니다.

소스파일은 모두 [Github](https://github.com/jacegem/OpenCV-Python-Tutorials)에서 확인할 수 있습니다. 

## Sum it up

아래 프로그램은 그레이 스케일로 이미지를 로드하고, 표시하고, 's'를 누르면 이미지를 저장하고 종료합니다. 아니면 `ESC` 키를 누르면 저장하지 않고 종료합니다.

```python
import numpy as np
import cv2

# Load an color image in grayscale
file = '01_model.jpg'
img = cv2.imread(file, 0)

cv2.imshow('image', img)
k = cv2.waitKey(0)
if k == 27:         # wait for ESC key to exit
    cv2.destroyAllWindows()
elif k == ord('s'): # wait for 's' key to save and exit
    cv2.imwrite('05_sum_it_up_output.png', img)
    cv2.destroyAllWindows()
```

## Matplotlib 사용하기

`Matplotlib`은 다양한 플로팅 방법을 제공하는 Python용 플로팅 라이브러리입니다. 당신은 다음 내용에서 Matplotlib에 대해 볼 수 있습니다. 여기서는 Matplotlib을 사용하여 이미지를 표시하는 방법을 배우게 됩니다. Matplotlib을 사용하여 이미지를 확대/축소하고 저장할 수 있습니다.

> matplotlib는 파이썬에서 자료를 차트(chart)나 플롯(plot)으로 시각화(visulaization)하는 패키지이다. 

```python
import numpy as np
import cv2
from matplotlib import pyplot as plt

file = '01_model.jpg'
img = cv2.imread(file, 0)
plt.imshow(img, cmap = 'gray', interpolation = 'bicubic')
plt.xticks([]), plt.yticks([])  # to hide tick values on X and Y axis
plt.show()
```

화면의 스크린 샷은 다음과 같습니다.

![](https://goo.gl/SSjxK4)

> Matplotlib에서는 다양한 플롯팅 옵션을 사용할 수 있습니다. 자세한 내용은 Matplotlib 문서를 참조하십시오.

> OpenCV로로드 된 컬러 이미지는 `BGR` 모드입니다. 그러나 Matplotlib은 `RGB` 모드로 표시됩니다. 따라서 OpenCV로 이미지를 읽으면 컬러 이미지가 Matplotlib에서 올바르게 표시되지 않습니다. 자세한 내용은 `연습 문제`를 참조하십시오.

## Additional Resources

- [Matplotlib Plotting Styles and Features](http://matplotlib.org/api/pyplot_api.html)

## 연습 문제
OpenCV에서 컬러 이미지를로드하여 Matplotlib에 표시하려고 할 때 약간의 문제가 있습니다. [이 토론](https://stackoverflow.com/questions/15072736/extracting-a-region-from-an-image-using-slicing-in-python-opencv/15074748#15074748)을 읽고 이해하십시오.



## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_gui/py_image_display/py_image_display.html#display-image
- https://blog.naver.com/PostView.nhn?blogId=samsjang&logNo=220499281999&parentCategoryNo=&categoryNo=66&viewDate=&isShowPopularPosts=false&from=postList


<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>


