---
title: "[책] React.JS 프로 리액트 (1/2)"
date: 2018.05.08
tags: [react, front-end]
categories:
- Progrmming
- Javascript
---

# [책] React.JS 프로 리액트 (1/2)

React.js를 이용한 모던 프런트엔드 구축

[![](http://image.yes24.com/goods/27871271/147x215)</br>프로 리액트 React.js를 이용한 모던 프런트엔드 구축</br>카시우 지 소자 안토니우 저/최민석 역](http://blog.yes24.com/lib/adon/View.aspx?blogid=9654534&goodsno=27871271&idx=25563&ADON_TYPE=B&regs=b)

# 시작하기

## 리액트의 정의

리액트는 자바스크립트와 (필요에 따라) XML을 이용해 조합형 사용자 인터페이스를 구축하는 엔진이다.

## 리액트의 장점

- 편리한 반응형 렌더링
- 순수 자랍스크립트를 이용한 컴포넌트 기반 개발
- 문서 모델의 유연한 추상화

## 컴포넌트 계층 정의

1. 컴포넌트는 단일 관심사를 가져야 하며 작아야 한다. 즉, 컴포넌트는 한 가지 일만 해야 한다. 컴포넌트가 더 성장하는 경우 작은 하위 컴포넌트로 분할해야 한다.
2. 프로젝트의 와이어프레임과 레이아웃을 분석하면 컴포넌트 계층에 대한 많은 힌트를 얻을 수 있다.
3. 데이터 모델이 주목한다. 인터페이스와 데이터 모델은 동일한 정보 아키텍처를 따르는 예가 많기 때문에 UI를 컴포넌트로 분리하는 작업도 아주 쉽게 해결되는 경우가 많다. 즉, 데이터 모델의 한 족가을 나타내는 컴포넌트로 분리할 수 있다.


# 정교한 상호작용

## 리액트의 애니메이션

리액트는 애니메이션을 처리하는 기본 방법으로 애드온 모듈의 일부인 고수준 ReactCSSTransitionGroup을 제공한다.

## CSS 트랜지션과 애니메이션의 기초

CSS를 이용한 애니메이션에는 CSS 트랜지션과 CSS 키프레임 애니메이션의 두 가지 범주가 있다.

- `CSS 트랜지션`은 시작 상태와 종료 상태의 두 가지 고유한 상태 간에 값을 보간하는 애니메이션 기법이다.
- `CSS 키프레임` 애니메이션은 시작과 종료 외에도 키프레임을 이용해 중간 단계를 제어하는 방법으로 더 복잡한 애니메이션을 만들 수 있게 해준다.

## 키프레임 애니메이션

```css
@keyframes pulsing-heart {
    0% { transform: none; }
    50% { transform: scale(1.4); }
    100% { transform: none; }
}
```
## ReactCSSTransitionGroup 요소 추가

```html
return(
	<div>
    	<ReactCSSTransitionGroup transitionName="example"
    							 transitionEnterTimeout={300}
       							 transitionLeaveTimeout={300}
    		{shoppingItems}
    	</ReactCSSTransitionGroup>
    </div>
)
```


```css
.example-enter {
    opacity: 0;
    transform: translateX(-250px);
}
.example-enter.example-enter-active {
    opacity: 1;
    transform: translateX(0);
    transition: 0.3s;
}
.example-leave {
    opacity: 1;
    transform: translateX(0);
}
.example-leave.example-leave-active {
    opacity: 0;
    transform: translateX(250px);
    transition: 0.3s;
}
```

## 드래그 앤드 드롭

리액트 DnD는 외부 라이브러리이므로 npm을 이용해 설치하고 의존성으로 선언해야 한다.

```shell
$ npm install --save react-dnd@2.x.x react-dnd-html5-backend@1.x.x
```

# 라우팅

## 리액트 라우터

- Router, Route: 라우트를 선언적으로 애플리케이션의 화면 계층과 매핑하는 데 이용한다.
- Link: 올바른 href로 완전 접근이 가능한 앵커 태그를 만드는 데 이용한다. 프로젝트를 탐색하는 유일한 방법은 아니지만 일반적으로 최종 사용자가 상호작용하는 주 형식이다.

## 프로그래밍 방식으로 라우트 변경

history 객체의 탐색 메서드

| 메서드       | 설명                                                         |
| ------------ | ------------------------------------------------------------ |
| pushState    | 새로운 URL로 이동하는 기본 히스토리 탐색 메서드<br />옵션 매캐변수 객체를 지정<br />history.pushState(null, '/user/123')<br />history.pushState({showGrades: true}, '/user/123') |
| replaceState | pushState와 동일한 구문을 사용하며 현재 URL을 새로운 URL로 대체<br />히스토리의 길이에 영향 주지 않고 URL을 대체한다는 점에서 리다이렉션과 비슷 |
| goBack       | 탐색 히스토리에서 한 항목 뒤로 이동                          |
| goForward    | 탐색 히스토리 한 항목 앞으로 이동                            |
| Go           | n 또는 -n만큼 앞으로 또는 뒤로 이동                          |
| createHref   | 라우터의 구성을 이용해 URL을 생성                            |

## 히스토리

리액트 라우터는 히스토리 라이브러리에 기반을 둔다.
기본적으로 리액트 라우터는 URL의 해시(#) 부분을 기준으로 라우트를 생성한다.(예:example.com/#/path)

# 플럭스를 이용한 리액트 애플리케이션 설계

플럭스는 기본적으로 세 부분으로 구성된다.

- 액션 action
- 스토어 store
- 디스패처 dispatcher 

## 은행계좌 애플리케이션

https://speakerdeck.com/jmorrell/jsconf-uy-flux-those-who-forget-the-past-dot-dot-dot-1

### 애플리케이션의 상수

constant.js

```javascript
export default {
    CREATED_ACCOUNT: 'created account',
    WITHDREW_FROM_ACCOUNT: 'withdrew from account',
    DEPOSITED_INTO_ACCOUNT: 'deposited in account'
};
```
### 디스패처

```javascript
import {Dispatcher} from 'flux';
export default new Dispatcher();
```


```javascript
import {Dispatcher} from 'flux';

class AppDispatcher extends Dispatcher{
    dispatch(action={}){
        console.log('Dispatched', action);
        super.dispatcher(action)
    }
}
export default new AppDispatcher();
```
# 성능 튜닝

리액트 컴포넌트에서 setState를 호출할 떄마다 해당 항목을 즉시 업데이트하는 대신 `더티`로 표시한다. 즉, 컴포넌트의 상태를 변경해도 즉시 효과가 나타나지는 않으며, 리액트는 이벤트 루프를 이용해 변경 사항을 일괄 처리를 통해 렌더링한다.

기본적으로 `shouldComponentUpdate`는 기본적으로 true를 반환하지만 false를 반환하도록 메서드를 다시 구현하면 리액트가 디시 렌더링 할 때 이 컴포넌트와 자식을 제외한다.

## 리액트 퍼프

리액트 퍼프`React Perf`는 앱의 성능에 대한 전반적인 개요 정보를 확인하고 `shouldComponentUpdate` 생명주기 후크를 구현할 성능 최적화 기회를 찾는 데 도움을 주는 프로파일링 도구다.

리액트 퍼프 메서드

| 유효성 검사기               | 설명                                                         |
| :-------------------------- | ------------------------------------------------------------ |
| Perf.start() 및 Perf.stop() | 측정을 시작 및 중지한다. 두 메서드 중간에 수행된 리액트 작업은 분석을 위해 기록된다. |
| Perf.printInclusive()       | 소비된 전체 시간을 출력한다.                                 |
| Perf.printExclusive()       | 컴포넌트를 마운팅하는 데 소비된 시간 출력<br />속성을 처리하거나 componentWillMount 및 componentDidMount 호출 시간을 제외하고 소비된 나머지 시간을 출력한다. |
| Perf.printWasted()          | 허비된 시간을 출력한다.<br />허비된 시간은 DOM에 변경된 사항이 없기 떄문에 동일하게 유지되므로 사실상 아무것도 렌더링하지 않는 컴포넌트에 소비된 시간이다. |

> 리액트 퍼프 애드온으로 유용한 정보를 얻을 수 있지만 이것만으로 애플리케이션의 모든 최적화 기회를 찾을 수 있는 것은 아니다. 직접 테스트와 디버그를 하고 `브라우저의 개발자 도구를 리액트 퍼프와 함께 사용`해야 한다.

## 리액트 퍼프 설치와 이용

```shell
$ npm install --save react-addones-perf
```



```javascript
import React, { component } from 'react';
import { render } from 'react-dom';
import Perf from 'react-addons-perf';
import Clock from './Clock';

class App extends Component {...}

Perf.start();
redner(<App />, document.getElementById('root'));
Perf.stop();
Perf.printInclusive();
Perf.printWasted();
```


## shouldComponentUpdate

리액트의 `shouldComponentUpdate` 수명주기 메서드는 렌더링 프로세스가 시작하기 직전에 트리거되며, 해당하는 트리를 렌더링에서 제외할 수 있는 기회를 제공한다. 

```javascript
shouldComponentUpdate(nextPros, nextState){
    // 숫자 값이 변경되지 않았으면 다시 렌더링하지 않는다
    return nextProps.value !== this.props.value;
}
```


## shallowCompoare 애드온

리액트는 shouldComponentUpdate에서 사용할 수 있는 `shallowCompare` 라는 애드온을 제공한다. 이 애드온은 객체의 속성과 상태를 간소하게 비교하고 변경 사항이 있는지 여부를 반환한다.

shallowCompare 애드온이 만능은 아니지만 다음 조건에 해당하는 앱에서는 성능 향상 효과를 기대할 수 있다.

- 간소하게 비교하려는 컴포넌트가 `순수` 컴포넌트다. 즉, 속성과 상태가 동일할 경우 동일한 결과를 렌더링한다.
- 상태를 조작하는 데 변경 불가능한 값이나 리액트의 불변성 도우미를 이용한다.

```shell
$ npm install --save react-addons-shallow-compare
```

