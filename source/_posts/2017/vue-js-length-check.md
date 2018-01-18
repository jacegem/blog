---
title: '[Vue.js] input 값 길이 체크'
date: 2017-04-09 19:13:45
tags: [javascript, vue, length, check, input]
categories:
- Programming
- Javascript
---


# [Vue.js] input 값 길이 체크

input 입력 데이터의 길이를 확인해서, 길이 초과시에 메시지를 출력한다.

## v-on:input 추가

```html
<input type="text" id="title" v-on:input="title_typing" name="title" value="" title="제목" class="form-control" />
```

## vue 생성

`methods` 를 추가합니다.

```javascript
var vue_event_regist = new Vue({
	el: '#title',
    methods: {
        title_typing : function(e){    	
            this.max_length(e, 5, '제목', '#title');
        },
        max_length : function(e, len, title, id){
            var val =  e.target.value;    			
            if (val.length > len){    				
                var msg = title + '의 최대 입력 길이는 ' + len + '입니다.';
                fnShowMsg(msg);        			
                $(id).val(val.substring(0, len));
            }
        }
    }
});
```

`watch`를 사용하였으나, 한글입력시에 제대로 처리되지 않아, `methods`로 바꾸고, `jquery` 를 같이 사용하였습니다.



## 출처
- https://vuejs.org/v2/guide/computed.html#Watchers
- https://vuejs.org/v2/guide/computed.html#Computed-vs-Watched-Property
