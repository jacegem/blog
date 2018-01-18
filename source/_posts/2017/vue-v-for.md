---
title: 'Vue v-for 사용'
date: 2017-04-12 19:13:45
tags: [javascript, vue, for]
categories:
- Programming
- Javascript
---

# Vue v-for 사용

## 데이터 요청

```javascript
this.$http.post('/web/stat/mapSearch.json', {'text': this.text, 'x': center.x, 'y':center.y})
.then(function(response){
	debugger;
	list = response.data.list;
	page = response.data.page;
}, function(response){
	//debugger;
});
```

파라미터가 전달되지 않을 시에, 아래 내용을 추가한다.

```javascript
http: {
    emulateJSON: true,
    emulateHTTP: true
},
```

결과 데이터 확인

![](https://goo.gl/8DGxhh)


페이지 정보를 가지고 최소, 최대 페이지를 구한다.

![](https://goo.gl/crrwz0)

내부적으로 계산하는 코드 추가

```javascript
data: {
	type : '',
	text : '',
	items: [],
	page : {},
	displayCount : 5,
	pageStart : 1,
	pageEnd : 10,
},    
```
data 를 정의 해 놓고, `this`로 사용한다.

```javascript
this.items = response.data.list;
this.page = response.data.page;    	        	
this.pageStart = this.page.pageNo - parseInt((this.displayCount - 1) / 2, 10)
if (this.pageStart < 1) this.pageStart = 1;
this.pageEnd = this.pageStart + this.displayCount - 1;
if (this.pageEnd > this.page.pageCount) this.pageEnd = this.page.pageCount;   	
```

## range 사용

range 메소드 생성

```javascript
range: function (start, end){
    list = []
    for (i = start; i <= end ; i ++) list.push(i);
    return list;
}
```

```html
<div class="btn-group btn-group-xs" role="group" aria-label="page number" v-for="n in range(5, 20)">
	<button type="button" class="btn">{{ n }}</button>										
</div>
```

![](https://goo.gl/69zumk)

표출되는 것을 확인한 이후, click 함수 이벤트를 추가한다.

```html
<button type="button" class="btn" v-on:click="search(n)">{{ n }}</button>
```

search 함수 변경

```javascript
search: function(page){
	center = wmap.getCenter();    	        
       this.$http.post('/web/stat/mapSearch.json', {'text': this.text, 'x': center.x, 'y':center.y, 'row_count': this.rowCount, 'page': page})
       .then(function(response){
       	debugger;
       	this.items = response.data.list;
       	this.page = response.data.page;    	        	
       	this.pageStart = this.page.pageNo - parseInt((this.displayCount - 1) / 2, 10)
       	if (this.pageStart < 1) this.pageStart = 1;
       	this.pageEnd = this.pageStart + this.displayCount - 1;
       	if (this.pageEnd > this.page.pageCount) this.pageEnd = this.page.pageCount;   	        	
       }, function(response){
       	//debugger;
       });
},
```


## active 지정하기

![](https://goo.gl/imfQmd)





## 출처
- https://github.com/vuejs/vue/issues/211
- https://github.com/vuejs/vue/issues/3641
