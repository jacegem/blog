---
title: JSP include 사용
date: 2017-02-14
tags: [jsp, include]
categories:
- Programming
- JSP
---

# JSP include 사용

## include 호출 페이지

```html
<jsp:include page="../include/menu.jsp">
	<jsp:param name="type" value="tag" />
	<jsp:param name="title" value="<h1 class='area-logo'> ****MY_LOGO**** </h1>" />
</jsp:include>
```

title 로 전달하는 종류가 2가지인 경우 `type` 으로 그 대상을 구분합니다.

- `tag`: 전달된 내용을 그대로 출력
- `text`: 텍스트를 일정 태그안에 넣어서 출력

## inlucde 페이지

```html
<c:choose>
	<c:when test="${param.type == 'tag'}">${param.title}</c:when>
	<c:otherwise><h1 class="area-tit">${param.title}</h1></c:otherwise>
</c:choose>
```

choose JSTL을 사용하여 분기 처리합니다.

type 으로 전달된 값을 확인하여 'tag' 텍스트인지 확인합니다. `${param.type == 'tag'}` 맞으면 전달된 title을 그대로 출력합니다.
아닐 경우에는 `<h1 class="area-tit">` 태그로 텍스트를 출력합니다.
