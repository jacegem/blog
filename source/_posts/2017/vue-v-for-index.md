---
title: '[Vue.js] v-for 에서 index 사용하기'
date: 2017-04-11 19:13:45
tags: [javascript, vue, for, index]
categories:
- Programming
- Javascript
---

# [Vue.js] v-for 에서 index 사용하기

## v-for 기본 사용

```javascript
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
```

## index 사용

```javascript
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ index }} - {{ item.message }}
  </li>
</ul>
```

## method 추가

```javascript
methods: {
    chr: function(index){
        return String.fromCharCode(65 + index);
    },
    // 생략...
}
```


## HTML 수정

```html
<li class="list-group-item" v-for="item in items" v-on:click="move(item.bd_mgt_sn, item.st_x, item.st_y)"><span class="map-point">A</span>{{ item.buld_nm }}</li>
```
```html
<li class="list-group-item" v-for="(item, index) in items" v-on:click="move(item.bd_mgt_sn, item.st_x, item.st_y)"><span class="map-point">{{ chr(index) }}</span>{{ item.buld_nm }}</li>
```


v-for 인덱스에 따라 다른 결과가 나오도록 함.

인덱스
```
0 => `A`
1 => `B`
2 => `C`
...
```


![](https://goo.gl/gH19IL)






## 출처
- https://vuejs.org/v2/guide/list.html
