---
title: react.js
date: 2017-03-05
tags: [react.js, react, javascript]
categories:
- Programming
- Javascript
---

# react.js


문서강좌: https://velopert.com/reactjs-tutorials

동영상: https://www.inflearn.com/course/react-%EA%B0%95%EC%A2%8C-velopert/?subscribe

ReactJS & [[Express]]

React Fundamentals
- [[Express]]
- [[ES6]]
- MongoDB mongoose

Virtual DOM

DOM(Document Object Model)

Redux, Webpack

## React.js 소개

atom 에디터, slack

agility.js, angular.js, aria temmplates, backbone.js, batman.js, bolt, canjs, chaplin + brunch, closure, cujo.js, dart, derby, dermis, dijon, dojo, due, ember.js, epitome, ext.js, funnyface.js, gwt, kendo ui, knockback.js, knockoutjs, maria, marionette, meteor, montage, olives, plastrongjs, puremvc, rappid.js, sammy.js, serenade.js, socketstream, soma.js, spine, stapes, yui

또 다른 프레임워크/ 프레임워크가 아닌 `라이브러리`

frame; 틀

> "A Javascript Library for Building User Interface"

Reactive Native

Angular2 Native

Virtual DOM (가상 돔)

## React.js 장점과 단점

React 의 장점? Virtual Dom 사용. 배우기 간단하다. 단 하나를 사용한다. `Component`

뛰어난 Garbage Collection, 메모리 관리, 성능

서버 & 클라이언트 렌더링

초기 구동 딜레이 & SEO(검색엔진최적화)

매우 간편한 UI 수정 및 재사용(컴포넌트화)

페이스북이 밀어준다.

php CodeIgniter 의 몰락, 인기

laravel

 강좌로 돌아가기 velopert(김 민준)의 프로필 사진강사
VELOPERT(김 민준)  9 분
React.js 장점과 단점

Links:
- http://slides.com/minjunkim-1/deck (강의에서 사용되는 슬라이드)
- https://velopert.com/reactjs-tutorials (React.js 텍스트 강좌 목록)
- http://incleaf.github.io/react-settin… (IE8 이하 브라우저 지원)

다른 프레임워크나 라이브러리와 `혼용`가능

React 의 단점

View Only. IE8 이하 `지원 X`

![](https://goo.gl/7BTN5e)

# 섹션 2. REACT.JS 시작하기

## CODEPEN 설정. ES6 클래스

 강좌로 돌아가기 velopert(김 민준)의 프로필 사진강사
VELOPERT(김 민준)  4 분
Codepen 설정 , ES6 클래스
2편 강좌부터는 React.js를 직접 사용해볼텐데요, 기초개념을 배우는 과정에서 환경설정을 편하게 하기 위해 Codepen.io 라는 웹 서비스를 사용합니다. 우리가 첫 컴포넌트를 만들어볼텐데, 만들기전에 ES6의 새로운 문법인 class 에 대해서도 간단하게 짚고 넘어갑시다


Links:
- http://slides.com/minjunkim-1/deck (강의에서 사용되는 슬라이드)
- https://codepen.io/pen/ (코드펜)
- http://kangax.github.io/compat-table/… (런타임별 ES6 호환률)
- https://developer.mozilla.org/ko/docs… (JavaScript 클래스)
- https://velopert.com/reactjs-tutorials (React.js 텍스트 강좌 목록)

Codepen.io

환경 설정.

Babel ES6 사용. ES5 로 변환. 여러 브라우저 호환

- react.min.js
- react.dom.min.js 15버전 이상부터 분리됨

ES6에 새로 도입된 문법 `class`


## JSX 의 특징

```javascript
class Codelab extends React.Component{
  render(){
    return(
       <div>CodeLab</div>
    )
  }
}

class App extends React.Compoent{
  render(){
    return (
      <Codelab/>
    )
  }
}

ReactDOM.render(<App/>.document.getElementById("root"));

```

```html
<div id="root"></div>
```


자바스크립트에서 HTML 형식 문법을 사용할 수 있다.
babel 에서 jsx 로더를 사용하여 변환해 준다.


let 은 블록 범위

2. Javascript Expression
3. Inline Style
4. Comments 멀티라인 주석을 사용한다.

## props

Links:
- http://slides.com/minjunkim-1/deck#/11 (강의에서 사용되는 슬라이드)
- https://facebook.github.io/react/docs… (React PropTypes)
- https://velopert.com/reactjs-tutorials (React.js 텍스트 강좌 목록)

---

- 컴포넌트 내부의 Immutable Data
- JSX 내부에 {this.props.propsName}
- 컴포넌트를 사용 할 때, <> 괄호 안에 propsName="value"
- this.props.children은 기본적으로 갖고 있는 props로서, <Cpnt> 여기에 있는 값이 들어간다.</Cpnt>

---


```javascript
<div>
  <h1>Hello {this.props.name}</h1>
  <div>{this.props.children}</div>
</div>
```

### 기본 값 설정

Componet.defaultProps={...}

```javascript
class App extends React.Compoent{
  render(){
    return (
      <div>{this.props.value}</div>
    )
  }
}

App.defaultProps = {
  value: 0
}
```

### Type 검증

Componet.propType = {...}

```javascript
class App extends React.Componet{
  render(){
    return (
      <div>
        {this.props.value}
        {this.props.secondValue}
        {this.props.thirdValue}
      </div>
    )
  }
}


App.propTypes = {
  value: React.PropTypes.string,
  secondValue: React.PropType.number,
  thirdValue: React.PropTypes.andy.isRequired
}

```


### 적용하기

`min` 버전은 에러가 뜨지 않는다.

## state

Links:
- http://slides.com/minjunkim-1/deck#/10 (강의에서 사용되는 슬라이드)
- http://bit.ly/ReactCodePen (코드펜 새 React 프로젝트)
- https://velopert.com/867 (JSX 텍스트 강좌)

---

  - 유동적인 데이터
  - JSX 내부에 {this.state.stateName}
  - 최기값 설정이 필수, 생성자(constructor)에서 this.state={} 으로 설정
  - 값을 수정 할 때에는 this.setState({...}), 렌더링 된 다음엔 this.state = 절대 사용하지 말것

기본값을 설정하지 않으면, 에러가 발생함.

```javascript
class Counter extends React.Component{
  constructor(props){
    super(props);
    this.state = {
      value:0
    }
  }

  render(){
    return(
      <div>
        <h2>{this.state.value}</h2>
        <button>Press Me</button>
      </div>
    )
  }
}
```

## 컴포넌트 매핑(Component Mapping)

비슷한 코드를 반복해서 렌더링

JavaScript - map

```javascript
arr.map(callback, [thisArg])
```

- callback 새로운 배열의 요소를 생성하는 함수로서, 다음 세가지 인수를 가집니다.
  - currentValue 현재 처리되고 있는 요소
  - index 현재 처리되고 있는 요소의 index 값
  - array 메소드가 불려진 배열
- thisArg (선택항목) callback 함수 내부에서 사용 할 this 값을 설정

```javascript
/* ES6 Syntax */
let numbers = [1,2,3,4,5];

let result = numbers.map((num)=>{
  return num*num;
})
```

`arrow function`(...) => {...}

### 컴포넌트 매핑

Links:

- http://slides.com/minjunkim-1/deck/#/12 (강의에서 사용되는 슬라이드)
- http://bit.ly/ReactCodePen (코드펜 새 React 프로젝트)
- http://codepen.io/velopert/pen/JKxKay (컴포넌트 매핑 예제 코드)
- https://developer.mozilla.org/ko/docs… (자바스크립트 배열의 Map 메소드)
- https://developer.mozilla.org/en-US/d… (자바스크립트 Arrow function)
- http://es6console.com/ (ES6 – ES5 변환)


```javascript
class ContactInfo extends React.Component{
  render(){
    return (
      <div>{this.props.contact.name} {this.props.contact.phone}</div>
    )
  }
}

class Contact extends React.Component{
  constructor(props){
    super(props);
    this.state = {
      contactData : {
        {name:'Abet', phone:'010-0000-2222'},
        {name:'Bbet', phone:'010-0000-2222'},
        {name:'Cbet', phone:'010-0000-2222'}
      }
    }
  }

  render(){
    const mapToComponet = (data) => {
      return data.map((contact, i) => {
        return (
          <ContactInfo contact ={contact} key={i} />;
        )
      })
    };
    return (
      <div>
        <div>Abet 010-0000-2222</div>
        <div>Bbet 010-0000-2222</div>
        <div>Cbet 010-0000-2222</div>
      </div>
    )
  }
}
```

`const` 변할일이 없는 값을 지정

```javascript
return(
  <div>
    {mapToComponent(this.state.contactData)}
  </div>
)
```


# 섹션 3. 개발환경 설정, 프로젝트 진행

## 작업환경 설정하기

https://www.nitrous.io/
c9.io
groom.io
codeanywhere.com

## React Project 만들기

Links:

- http://slides.com/minjunkim-1/deck#/13/1
- React Project 만들기 텍스트 강좌: https://velopert.com/814
- React 텍스트 강좌 목록: https://velopert.com/reactjs-tutorials

```
c:\>git --version
git version 2.10.1.windows.1

c:\>node -v
v6.9.1
```

### Global Dependency 설치

```
sudo npm install -g webpack webpack-dev-server
```

1. `webpack`: 브라우저 위에서 import(require)를 할 수 있게 해주고 자바스크립트 파일들을 하나로 합쳐줍니다.
2. `webpack-dev-server`: 별도의 서버를 구축하지 않고도 static 파일을 다루는 웹서버를 열 수 있으며 hot-loader 를 통하여 코드가 수정 될 때마다 자동으로 리로드 되게 할 수 있습니다.

### 프로젝트 생성

```
mkdir react-fundamentals
cd react-fundamentasl
npm init
```

생활코딩 nodejs
