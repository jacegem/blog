---
title: 'v-model 한글 처리'
date: 2017-04-04 19:13:45
tags: [javascript, vue, model]
categories:
- Programming
- Javascript
---

## v-model 한글 처리

v-model 사용시 한글을 정상적으로 처리되지 않는다.

- [v-model의 한글 사용 문제을 v-on을 사용한 해결법](https://jsfiddle.net/kciter/tLz9gt4o/) - @kciter
- [v-model 사용시 한글 사용 문제](https://jsfiddle.net/kciter/b5qhxbfh/) - @kciter

`v-on`을 사용하여 해결하여야 함.

HTML 에 `v-on:input="typing"`을 추가함

```html
<input type="text" class="form-control" name="text" v-on:input="typing" v-model="text" placeholder="검색어를 입력하세요">
```

typing 메소드를 추가함.

```javascript
methods: {
    typing: function(e){
        this.text = e.target.value
    },
}
```


## ajax.post 사용시 파라미터 전달

this.$http.post() 사용시에 파라미터가 전달되지 않는다.

아래 내용을 추가한다.

```javascript
new Vue({
    http: {
        emulateJSON: true,
        emulateHTTP: true
    }
})
```




## 출처
- https://vuejs-kr.github.io/snippets/
- https://jsfiddle.net/kciter/tLz9gt4o/
- https://jsfiddle.net/kciter/tLz9gt4o/
- https://github.com/pagekit/vue-resource/blob/develop/docs/http.md
- https://laracasts.com/discuss/channels/vue/vue-resource-post-request-no-data
