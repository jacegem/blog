---
title: '[OpenCV-Python Tutorials 03] 비디오 시작하기'
date: 2018-01-03 19:11:01
tags: [opencv, python, tutorial]
category:
- Programming
- Python
---

# [OpenCV-Python Tutorials 03] 비디오 시작하기

모든 파일은 [Github](https://github.com/jacegem/OpenCV-Python-Tutorials)에서 확인 할 수 있습니다.

## 목표

- 비디오 읽기, 비디오 디스플레이 및 비디오 저장 방법을 배웁니다.
- 카메라에서 캡처하여 표시하는 방법을 배웁니다.
- 다음 함수를 배웁니다. cv2.VideoCapture(), cv2.VideoWriter()


## 카메라에서 비디오 캡처

가끔 우리는 카메라로 라이브 스트림을 캡처 해야 합니다. OpenCV는 매우 간단한 인터페이스를 제공합니다. 카메라에서 비디오를 캡처하고 (저는 노트북의 내장 웹캠을 사용하고 있습니다.) 그레이 스케일 비디오로 변환하여 표시하는 간단한 작업부터 시작합니다.

비디오를 캡처하려면 `VideoCapture` 객체를 만들어야 합니다. 인수는 장치 색인이나 비디오 파일 이름이 될 수 있습니다. 장치 색인은 카메라를 지정하는 번호입니다. 일반적으로 하나의 카메라가 연결됩니다 (필자의 경우). 단순히 0 (또는 -1)을 전달합니다. 1을 전달하여 두 번째 카메라를 선택할 수 있습니다. 그 후에 프레임 단위로 캡처 할 수 있습니다. 그러나 마지막에 `release` 하는 것을 잊지 마십시오.

```python
import numpy as np
import cv2

cap = cv2.VideoCapture(0)

while(True):
    # Capture frame-by-frame
    ret, frame = cap.read()

    # Our operations on the frame come here
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # Display the resulting frame
    cv2.imshow('frame',gray)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# When everything done, release the capture
cap.release()
cv2.destroyAllWindows()
```

내장 웹캠이 없는 경우에는 에러가 발생하므로, `파일에서 비디오 재생하기` 내용을 확인하여 저장 파일을 읽도록 변경합니다.






`cap.read()`는 bool (True/False)을 반환합니다. 프레임을 올바르게 읽으면 True입니다. 따라서 이 반환 값을 확인하여 동영상의 끝을 확인할 수 있습니다.

경우에 따라 `cap`이 캡처를 초기화하지 않았을 수 있습니다. 이 경우이 코드는 오류를 표시합니다. `cap.isOpened()` 메소드를 통해서 초기화되었는지 여부를 확인할 수 있습니다. `True`이면 OK입니다. 그렇지 않으면 `cap.open()`을 사용하여 열 수 있습니다.

또한 `cap.get(propId)` 메소드를 사용하여 이 비디오의 일부 기능에 액세스 할 수 있습니다. 여기서 `propId`는 0에서 18사이의 숫자입니다. 각 숫자는 비디오의 속성을 나타내며 (해당 비디오에 적용 가능할 경우) 자세한 내용은 여기서 볼 수 있습니다 : [Property Identifier.](http://docs.opencv.org/2.4/modules/highgui/doc/reading_and_writing_images_and_video.html#videocapture-get). 이 값 중 일부는 `cap.set(propId, value)`을 사용하여 수정할 수 있습니다. Value는 원하는 새 값입니다.

| 속성 | 설명 |
|-------|------|
| CV_CAP_PROP_POS_MSEC | 밀리 세컨드 단위의 비디오 파일의 현재의 위치 또는 비디오 캡춰의 타임 스탬프.
| CV_CAP_PROP_POS_FRAMES | 다음에 디코드 또는 캡춰되는, 프레임의 0베이스의 인덱스 |
| CV_CAP_PROP_POS_AVI_RATIO |  비디오 파일의 상대 위치 : 0 - 필름의 시작 부분, 필름의 끝 부분.
| CV_CAP_PROP_FRAME_WIDTH |  비디오 스트림의 프레임의 폭. |
| CV_CAP_PROP_FRAME_HEIGHT |  비디오 스트림의 프레임의 높이. |
| CV_CAP_PROP_FPS |  프레임 속도.|
| CV_CAP_PROP_FOURCC | 코덱의 4-character 코드입니다. | 
| CV_CAP_PROP_FRAME_COUNT | 비디오 파일의 프레임 수. |
| CV_CAP_PROP_FORMAT |  retrieve()에 의해 반환 된 Mat 객체의 형식.|
| CV_CAP_PROP_MODE | 현재의 캡춰 모드를 나타내는 백엔드 고유의 값. |
| CV_CAP_PROP_BRIGHTNESS |  이미지의 밝기입니다 (카메라에만 해당). | 
| CV_CAP_PROP_CONTRAST | 이미지의 명암 (카메라에만 해당).|
| CV_CAP_PROP_SATURATION |  이미지 포화 (카메라 만).|
| CV_CAP_PROP_HUE | 이미지의 색조입니다 (카메라에만 해당).|
| CV_CAP_PROP_GAIN |  이미지의 게인 (카메라에만 해당).|
| CV_CAP_PROP_EXPOSURE |  노출 (카메라에만 해당).|
| CV_CAP_PROP_CONVERT_RGB | 이미지를 RGB로 변환할지 어떨지를 나타내는 Boolean 형의 플래그. | 
| CV_CAP_PROP_WHITE_BALANCE_U |  화이트 밸런스 설정의 U 값 (참고 : 현재 DC1394 v 2.x 백엔드에서만 지원됨) | 
| CV_CAP_PROP_WHITE_BALANCE_V | 화이트 밸런스 설정의 V 값 (참고 : 현재 DC1394 v 2.x 백엔드에서만 지원됨) | 
| CV_CAP_PROP_RECTIFICATION |  스테레오 카메라의 정류 플래그 (참고 : 현재 DC1394 v 2.x 백엔드에서만 지원됨)|
| CV_CAP_PROP_ISO_SPEED | 카메라의 ISO 속도 (참고 : 현재 DC1394 v 2.x 백엔드에서만 지원됨)|
| CV_CAP_PROP_BUFFERSIZE | 내부 버퍼 메모리에 저장된 프레임 수 (참고 : 현재 DC1394 v 2.x 백엔드에서만 지원됨) |

예를 들어, 프레임 너비와 높이를 `cap.get(3)`과 `cap.get(4)`로 확인할 수 있습니다. 기본적으로 640x480을 제공하는 것을 320x240으로 수정하고 싶다면 `ret = cap.set(3,320)` 와 `ret = cap.set(4,240)` 만 사용하십시오.


```python
ret = cap.set(CV_CAP_PROP_FRAME_WIDTH,320)
ret = cap.set(CV_CAP_PROP_FRAME_HEIGHT,240)
```

> 오류가 발생하면 다른 카메라 응용 프로그램 (예 : Linux의 Cheese)을 사용하여 카메라가 제대로 작동하는지 확인하십시오.

## 파일에서 비디오 재생하기

카메라에서 캡쳐하는 것과 같습니다. 비디오 파일 이름으로 카메라 인덱스를 변경하면 됩니다. 또한 프레임을 표시하는 동안 `cv2.waitKey()`을 이용하여 적절한 시간을 사용하십시오. 너무 적으면 비디오가 매우 빠르며 너무 높으면 비디오가 느려집니다 (즉, 비디오를 느린 동작으로 표시 할 수 있습니다). 정상적인 경우 25 밀리 초가 정상입니다.

```python
import numpy as np
import cv2

file = '01_video.mp4'
cap = cv2.VideoCapture(file)

while(True):
    # Capture frame-by-frame
    ret, frame = cap.read()

    # Our operations on the frame come here
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # Display the resulting frame
    cv2.imshow('frame',gray)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# When everything done, release the capture
cap.release()
cv2.destroyAllWindows()
```

![](https://goo.gl/z1FW2d)

동영상을 끝내기 위해서는 `q` 키를 입력해야 합니다. 

> ffmpeg 또는 gstreamer의 올바른 버전이 설치되어 있는지 확인하십시오. 때로는 ffmpeg / gstreamer가 잘못 설치되어 비디오 캡쳐 작업에 문제가 생깁니다. 

## 비디오 저장하기

따라서 우리는 비디오를 캡처하여 프레임 단위로 처리하고 해당 비디오를 저장하려고 합니다. 이미지의 경우 매우 간단합니다. `cv2.imwrite()`만 사용하면됩니다. 비디오에서는  좀 더 많은 작업이 필요합니다.

이번에는 `VideoWriter` 객체를 만듭니다. 출력 파일 이름을 지정해야 합니다 (예 : output.avi). 그런 다음 `FourCC` 코드를 지정해야 합니다 (다음 단락에 세부사항이 설명되어 있습니다). 그런 다음 초당 프레임 수(fps)와 프레임 크기를 전달해야 합니다. 그리고 마지막 하나는 `isColor` 플래그입니다. `True`이면 인코더는 컬러 프레임을 요구하고 그렇지 않으면 그레이 스케일 프레임과 함께 작동합니다.

`FourCC`는 비디오 코덱을 지정하는 데 사용되는 4 바이트 코드입니다. 사용 가능한 코드 목록은 [fourcc.org](http://www.fourcc.org/codecs.php)에서 찾을 수 있습니다. 플랫폼에 따라 다릅니다. 다음 코덱은 잘 작동합니다.

> FourCC(Four Character Code)는 말 그대로 "4글자 코드"라는 뜻이며, 4 바이트로 된 문자열은 데이터 형식을 구분하는 고유 글자가 된다.

- Fedora : DIVX, XVID, MJPG, X264, WMV1, WMV2. (XVID가 더 바람직합니다.  MJPG는 고화질 비디오를 만듭니다 .X264는 매우 작은 크기의 비디오를 제공합니다)
- Windows의 경우 : DIVX (테스트 및 추가 예정)

FourCC 코드는 MJPG의 경우 `cv2.VideoWriter_fourcc('M', 'J', 'P', 'G')` 또는 `cv2.VideoWriter_fourcc(*'MJPG')`로 전달됩니다.

카메라의 코드 캡처 아래에서 모든 프레임을 수직 방향으로 뒤집어 저장합니다.

```python
import numpy as np
import cv2

cap = cv2.VideoCapture(0)

# Define the codec and create VideoWriter object
fourcc = cv2.VideoWriter_fourcc(*'XVID')
out = cv2.VideoWriter('output.avi',fourcc, 20.0, (640,480))

while(cap.isOpened()):
    ret, frame = cap.read()
    if ret==True:
        frame = cv2.flip(frame,0)

        # write the flipped frame
        out.write(frame)

        cv2.imshow('frame',frame)
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break
    else:
        break

# Release everything if job is finished
cap.release()
out.release()
cv2.destroyAllWindows()
```

![](https://goo.gl/18FYFD)

위아래가 뒤집어진 영상이 저장된 것을 확인할 수 있습니다. 

![](https://goo.gl/CW1Lwf)


## 출처

- https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_gui/py_image_display/py_image_display.html#display-image
- https://videos.pexels.com/


<script src="https://gist.github.com/jacegem/60ce233cf6adaa7a385233e1f164ed13.js"></script>























