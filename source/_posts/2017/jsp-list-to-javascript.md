---
title: JSP list to javascript
date: 2017-02-15
tags: [jsp, list, javascript]
categories:
- Programming
- JSP
---

# JSP list to javascript

JSP 로 부터 넘어온 리스트 객체를 자바스크립트 리스트 객체로 변환합니다.

```javascript
var obj_list = [
	<c:forEach items="${jsp_list}" var="item" varStatus="loop">
    	{ 'seq' : '${item.seq}'
    		, 'type' : '${item.type}'
    		, 'title' : '${item.title}'
    		, 'content' : '${item.content}'
    		, 'lon_x' : '${item.lon_x}'
    		, 'lat_y' : '${item.lat_y}'
    	} ${not loop.last ? ',' : ''}
  	</c:forEach>
];
```

리스트를 이용하여 처리합니다.

```javascript
// 이벤트 목록 마커 생성
for (var i = 0, len = obj_list.length; i < len; i++) {
  	var item = obj_list[i];
  	// codes...
}
```
