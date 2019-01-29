---
title: "localStorage 에 checkbox 상태 저장"
date: 2018.09.04
tags: [localStorage, checkbox, input]
categories:
  - Programming
  - Javascript
---

# localStorage 에 checkbox 상태 저장

## change 이벤트 받기

HTML 에서 checkbox input 삽입

```javascript
<input type="checkbox" id="checkboxShowGPSInfo" />
```

자바스크립트에서 change 이벤트 정의

```javascript
$(function() {
  $("#checkboxShowGPSInfo").change(function() {
    if ($("#checkboxShowGPSInfo").is(":checked")) {
      alert("체크박스 체크했음!")
    } else {
      alert("체크박스 체크 해제!")
    }
  })
})
```

## localStorage 에 상태저장

```javascript
$(function() {
  var checkboxShowGPSInfo = $("#checkboxShowGPSInfo")

  checkboxShowGPSInfo.change(function() {
    showGPSInfo = !!checkboxShowGPSInfo.is(":checked")
    localStorage["showGPSInfo"] = showGPSInfo
  })
})
```

change 이벤트시에 `showGPSInfo` 이름으로 상태값을 저장합니다.

## localStorage에서 값 불러오기

```javascript
$(function() {
  showGPSInfo = localStorage["showGPSInfo"] || false
  showGPSInfo = showGPSInfo === "true"

  var checkboxShowGPSInfo = $("#checkboxShowGPSInfo")
  checkboxShowGPSInfo.prop("checked", showGPSInfo)

  checkboxShowGPSInfo.change(function() {
    showGPSInfo = !!checkboxShowGPSInfo.is(":checked")
    localStorage["showGPSInfo"] = showGPSInfo
  })
})
```

localStorage 에서는 모든값을 `문자열로 저장`하기 때문에 이를 확인하여 `boolean`으로 변경합니다.

```javascript
showGPSInfo = showGPSInfo === "true"
```

showGPSInfo 의 값이 "true" 이면 `true` 를 할당합니다. 아니면 `false` 입니다.
