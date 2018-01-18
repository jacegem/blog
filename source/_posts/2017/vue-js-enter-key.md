---
title: 'Vue.js 엔터키 입력 처리'
date: 2017-04-08 19:13:45
tags: [javascript, vue, enter, key, input]
categories:
- Programming
- Javascript
---


# Vue.js 엔터키 입력 처리

keyup 이벤트를 사용한다.

```html
<input v-on:keyup.enter="submit">
```

짧게 표현도 가능하다.

```html
<!-- also works for shorthand -->
<input @keyup.enter="submit">
```




## 출처
- https://vuejs.org/v2/guide/events.html
