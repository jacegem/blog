title: '[jquery] jquery를 사용한 input, select 읽기전용으로 변경'
tags: [jquery, input, select, selector]
date: 2016-01-24 20:39:06

---
## jquery를 사용한 input 읽기전용 속성 변경

input 태그에 `readonly` 속성을 추가하여 읽기 전용으로 변경합니다.

```js
$(function(){
	$('input').prop('readonly', true);	// 모든 input 태그를 readonly로 변경함.
});
```

페이지 `onload` 이벤트시에 모든 input태그를 읽기전용으로 변경합니다.


## jquery를 사용한 option 비활성화

select에는 readonly가 없으므로 option에 diabled를 설정해야 합니다. 각 option에 disabled를 설정하여 변경이 안되도록 할 수 있습니다.

```js
$(function(){
	$('option').attr('disabled', true);	// option 태그를 모두 disabled 로 변경함.
});
```
페이지 onload 이벤트시에 모든 option태그를 비활성화 시킵니다.



## 참고
* http://stackoverflow.com/questions/4610652/jquery-select-option-disabled-if-selected-in-other-select

