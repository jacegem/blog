title: '[jstl] callback 함수명 전달하기'
tags: [jstl, callback, function]
date: 2016-01-24 20:26:07

---

`JSTL`을 사용하여 child window에서 부모창 함수를 호출합니다. 팝업창 생성시에 부모창의 함수명을 서버로 전달하고, 해당 함수명을 받아서 호출할 수 있도록 합니다.

	JSTL : JavaServer Pages Standard Tag Library

파라미터로 전달한 함수명을 이용하여 함수를 호출합니다.

```js
<%@ page import="org.apache.commons.lang3.StringUtils"%>
function callback() {
	parent.<%=!StringUtils.defaultString(request.getParameter("callback")).equals("") ? request.getParameter("callback") : "defaultCallback"%>();
}
```

`!(not)` 을 제거하고 삼항연산자의 순서를 변경합니다. 

```js
parent.<%=StringUtils.defaultString(request.getParameter("callback")).equals("") ? "defaultCallback" : request.getParameter("callback") %>();
```

다른 방법으로는 서버로 전달하지 않고, 부모창에서 바로 자식창에 호출받을 callback 함수를 지정할 수도 있습니다. 또는 `eval()` 함수를 이용하여 서버로 전달된 함수명을 통해서 호출할 수도 있습니다.