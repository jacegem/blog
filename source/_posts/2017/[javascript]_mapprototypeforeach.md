---
title: '[javascript] Map.prototype.forEach()'
date: 2017-01-01 19:13:45
tags: javascript
categories:
- Programming
- Javascript
---

# [javascript] Map.prototype.forEach()

forEach() 함수는 입력한 순서에 따라 맵 오브젝트안에 있는 각각 키/값 쌍을 제공하는 기능을 실행합니다.

## 문법
```javascript
myMap.forEach(callback[, thisArg])
```

### 파라미터

callback
: 각 요소들을 실행할 함수

thisArg
: 콜백함수를 실행할 때 사용할 값

### Return value

undefined.

## 설명

forEach 함수는 맵에 실제로 존재하는 각 키 마다 제공된 콜백에 의해 실행된다. 지워진 키에 대해서는 호출되지 않는다. 그러나 값은 존재하고 undefined 인 경우에도 실해된다.

콜백은 세가지 파라미터와 함께 호출된다.

- 요소 값
- 요소 키
- 전달된 맵 객체

If a thisArg parameter is provided to forEach, it will be passed to callback when invoked, for use as its this value.  Otherwise, the value undefined will be passed for use as its this value.  The this value ultimately observable by callback is determined according to the usual rules for determining the this seen by a function.

Each value is visited once, except in the case when it was deleted and re-added before forEach has finished. callback is not invoked for values deleted before being visited. New values added before forEach has finished will be visited.

forEach executes the callback function once for each element in the Map object; it does not return a value.

## Examples
### Printing the contents of a Map object

The following code logs a line for each element in an Map object:

```javascript
function logMapElements(value, key, map) {
    console.log("m[" + key + "] = " + value);
}
new Map([["foo", 3], ["bar", {}], ["baz", undefined]]).forEach(logMapElements);
// logs:
// "m[foo] = 3"
// "m[bar] = [object Object]"
// "m[baz] = undefined"
```

## Specifications

Specification	Status	Comment
ECMAScript 2015 (6th Edition, ECMA-262)
The definition of 'Map.prototype.forEach' in that specification.	Standard	Initial definition.
ECMAScript 2017 Draft (ECMA-262)
The definition of 'Map.prototype.forEach' in that specification.	Draft	 

## Browser compatibility
Desktop Mobile
Feature	Chrome	Firefox (Gecko)	Internet Explorer	Opera	Safari
Basic support	38	25.0 (25.0)	11	25	7.1

## See also
Array.prototype.forEach()
Set.prototype.forEach()
