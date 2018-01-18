---
title: 이클립스 코드정렬에서 span 태그 정렬
date: 2017-01-21 19:13:45
tags: [eclipse, formatting, span, tag]
categories:
- Application
- IDE
---

# 이클립스 코드정렬에서 span 태그 정렬

이클립스에서 `Ctrl + Shift + f` 키를 눌러서 코드 정렬을 실행합니다.

```html
<span class="area-select"> <label for="count_1" class="sub-title">이름</label> <input type="text" title="이름 입력" id="name" name="name" class="sub-box">
</span> <span class="area-select"> <label for="count_2" class="sub-title">생년월일</label> <input type="text" title="생년월일 입력" placeholder="선택하세요" name="birth" id="birth" class="txt-calendar sub-box datepicker2">
	<button type="button" class="btn-calendar">달력</button>
</span> <span class="area-select"> <span class="sub-title">성별</span>
```

위와 같이 `span` 태그가 구분되지 않고 한줄에 나오는 경우 옵션을 수정합니다.

## 옵션 수정

메뉴 Window -> Preferences -> Web -> HTML Files -> Editor 를 선택합니다.

![](https://goo.gl/vm7V5Y)

우측 Line width 의 값을 `999`로 수정합니다. (혹은 알맞은 수로 변경합니다. 한줄에 나오는 최대 문자열 수 입니다.)

inline Elements 에서 `span`을 선택한 후 `Remove` 버튼을 클릭하여 해당 태그를 제거합니다.

이후 `Ctrl + Shift + f` 키를 눌러서 코드 정렬을 실행합니다.

```html
<span class="area-select">
	<label for="count_1" class="sub-title">이름</label> <input type="text" title="이름 입력" id="name" name="name" class="sub-box">
</span>
<span class="area-select">
	<label for="count_2" class="sub-title">생년월일</label> <input type="text" title="생년월일 입력" placeholder="선택하세요" name="birth" id="birth" class="txt-calendar sub-box datepicker2">
	<button type="button" class="btn-calendar">달력</button>
</span>
```

span 태그가 새 줄로 정렬되는 것을 확인 할 수 있습니다.



## 출처

- https://stackoverflow.com/questions/10298024/eclipse-html-editor-each-input-tag-on-the-new-line
