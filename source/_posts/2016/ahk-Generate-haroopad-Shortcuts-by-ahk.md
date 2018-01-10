title: '[ahk] Generate haroopad Shortcuts by AHK'
date: 2016-01-05 10:11:31
tags:
	- ahk
	- autohotkey
	- haroopad
	- shortcut
---
오토핫키로 하루패드 단축키 생성하기

#### 뷰
단축키 | 설명 | AHK 변경키 | 코드
-------|------|---------|------
SHIFT-CTRL-1|(에디터:뷰어)모드|F1|F1::Send +^1
SHIFT-CTRL-2|(뷰어:에디터)모드|F2|F2::Send +^2
SHIFT-CTRL-3|에디터 모드|F3|F3::Send +^3
SHIFT-CTRL-4|뷰어 모드|F4|F4::Send +^4
CTRL-F11|전체화면 모드 토글|F11|F11::Send ^{F11}
CTRL-ALT-P|프리젠테이션 모드|F12|F12::Send ^!p

사용할 코드는 아래와 같습니다. 

```autohotkey
F1::Send +^1
F2::Send +^2
F3::Send +^3
F4::Send +^4
F11::Send ^{F11}
F12::Send ^!p
```

AHK를 실행하고, 트레이 아이콘에서, `Window Spy`를 실행합니다. 

![window spy](https://goo.gl/0nuSQ2)

`Active Window Info` 정보를 확인합니다.

![Active Window Info](https://goo.gl/1W1ZT2)

`ahk_exe haroopad.exe` 를 확용하여 해당 윈도우를 확인합니다.
`haroopad.ahk` 파일명으로 파일을 생성합니다.


## 전체 코드
```autohotkey
#ifWinActive ahk_exe haroopad.exe

F1::Send +^1
F2::Send +^2
F3::Send +^3
F4::Send +^4
F11::Send ^{F11}
F12::Send ^!p

#ifWinActive

```

하루패드에서만 단축키를 적용하기 위해서, `#ifWindActive`를 사용하였습니다.


### 유의 사항
대문자 P와 소문자 p는 다르므로, 정확히 작성해야 합니다.
