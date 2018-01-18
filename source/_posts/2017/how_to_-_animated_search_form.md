---
title: How TO - Animated Search Form
date: 2017-02-07 19:13:45
tags: [css, animation, search, form]
categories:
- Programming
- CSS
---


# How TO - Animated Search Form

출처: http://www.w3schools.com/howto/howto_css_animated_search.asp

CSS와 애니메이션 검색 폼을 작성하는 방법에 대해 알아봅니다.

## 애니메이션 검색 폼 생성

## 1단계 HTML 추가

### Example

```html
<input type="text" name="search" placeholder="Search..">
```

## 2단계 CSS 추가

### Example

```css
.input[type=text] {
    width: 130px;
    -webkit-transition: width 0.4s ease-in-out;
    transition: width 0.4s ease-in-out;
}

/* When the input field gets focus, change its width to 100% */
input[type=text]:focus {
    width: 100%;
}
```

HTML 폼 스타일하는 방법에 대한 자세한 내용 [CSS 폼 튜토리얼](http://www.w3schools.com/css/css_form.asp)에서 확인할 수 있습니다.
