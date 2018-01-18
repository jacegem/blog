---
title: HTML `<input>` checked 속성
date: 2017-02-08 19:13:45
tags: [html, input, checked]
categories:
- Programming
- HTML
---

# HTML `<input>` checked 속성

http://www.w3schools.com/tags/att_input_checked.asp

## Example

선택된 체크박스가 포함된 폼의 HTML 코드

```html
<form action="demo_form.asp">
  <input type="checkbox" name="vehicle" value="Bike"> I have a bike<br>
  <input type="checkbox" name="vehicle" value="Car" checked> I have a car<br>
  <input type="submit" value="Submit">
</form>
```

## 정의 및 사용법

`checked` 속성은 부울 속성입니다.

checked 가 존재하면 그것은 페이지가 로드될 때 `<input>`가 미리 선택된 상태를 나타냅니다. (checked)

checked 속성은 `<input type="checkbox">` 와 `<input type="radio">` 로 사용할 수 있습니다.

또한 checked 속성은 자바스크립트를 통해서 페이지가 로드된 후에도 설정할 수 있습니다.

## 브라우저 지원

표의 수치는 완전히 특성을 지원하는 브라우저 버전을 나타냅니다.

|Attribute|  ![](http://www.w3schools.com/images/compatible_chrome.gif) |  ![](http://www.w3schools.com/images/compatible_edge.gif) |   ![](http://www.w3schools.com/images/compatible_firefox.gif)    | ![](http://www.w3schools.com/images/compatible_safari.gif)   |  ![](http://www.w3schools.com/images/compatible_opera.gif)  |
|--|--|--|--|--|--|
|checked|	1.0|	2.0|	1.0|	1.0|	1.0|


## HTML 4.01 과 HTML5 에서의 차이점

없음


## HTML 과 XHTML 에서의 차이점

XHTML에서는 속성 최소화가 금지되어 있습니다.

checked 속성은 `<input checked="checked" />` 와 같이 지정해야 합니다.


## 문법

`<input checked>`
