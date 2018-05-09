---
title: "[책] Learning React"
date: 2018.05.17
tags: [react, learning]
categories:
- Progrmming
- Javascript
---

# [책] Learning React


[![](http://image.yes24.com/goods/58543289/66x96)알렉스 뱅크스,이브 포셀로 공저/오현석 역](http://blog.yes24.com/lib/adon/View.aspx?blogid=9654534&goodsno=58543289&idx=25562&ADON_TYPE=B&regs=b)

리액트를 사용하는 전체 사이트 
<https://github.com/facebook/react/wiki/Sites-Using-React>

## 예약어와 className

자바스크립트에는 개발자가 임의로 변수나 객체 이름 등에 사용할 수 없는 예약어가 있다. ES2016 기준으로 그 예약어 목록은 다음과 같다.

- break 
- case
- class
- catch
- const
- continue
- debugger
- default
- delete
- do
- else
- export
- extends
- finally
- for 
- function
- if
- import
- in
- instanceof
- new
- return
- super
- switch
- this
- throw
- try
- typeof
- var
- void
- while
- with
- yield

리액트가 지원하는 전체 태그와 속성: https://facebook.github.io/react/docs/tags-and-attributes.html 

## 대소문자 구별

대소문자의 구별은 중요한 사항이다.
HTML 엘리먼트를 나타낼 때는 태그를 소문자로 써야 한다.

컴포넌트를 나타낼 때는 그 이름에 대문자가 사용돼야 한다. 
대소문자 구별을 정확히 해야 한다.

## 이벤트

https://www.kirupa.com/html5/javascript_events.htm

합성 이벤트(synthetic evnet)라고 하는 리액트에 특정적인 이벤트 유형인 SyntheticEvent를 다룬다.
항상 브라우저의 네이티브 이벤트를 wrapping하는 SyntheticEvent 타입을 인자로 받는다.

SyntheticEvent 문서 : https://facebook.github.io/react/docs/events.html

리액트에서 이벤트 핸들러 안의 this는 DOM 세게에서와는 다르다. 리액트가 아닌 세계에서 이벤트 핸들러 안의 this는 리스닝하는 대상 엘리먼트를 참조한다.

# 컴포넌트 생명주기

https://www.kirupa.com/react/lifecycle_example.htm

## 초기 렌더링 단계

1. getDefaultProps
2. getInitialState
3. componentWillMount
4. render
5. componentDidMount

## 업데이트 단계

1. shouldComponentUpdate
2. componentWillUpdate
3. render
4. componentDidUpdate

## 속성 변경 시

1. componentWillReceiveProps
2. shouldComponentUpdate
3. componentWillUpdate
4. render
5. componentDidUpdate

## 언마운트 단계

1. componentWillUnmount



## 리액트 라우터

매칭되는 URL일 경우 각자 지정된 컴포넌트를 화면에 보여준다.
원한다면 Route를 중첩해 사용할 수 있다.

```javascript
ReactDOM.render(
  <Router>
    <Route path="/" component={App}>
      <IndexRoute component={Home} />
      <Route path="stuff" component={Stuff}>
        <Route path="blah" component={Blah}/>
      </Route>
    </Route>
  <Router>,
  destination    
);
```

- https://github.com/ReactTraining/react-router/blob/v3/docs/guides/RouteMatching.md
- [React Router: Declarative Routing for React.js](https://reacttraining.com/react-router/web/guides/philosophy)

## 액티브 링크

Link 인스턴스에 activeClassName 이라는 속성을 설정하고 링크가 액티브하게 되면 적용할 CSS 클래스의 이름을 지정한다.

```javascript
var App = React.createClass({
  render: function(){
    return (
      <div>
        <h1>Simple SPA</h1>
        <ul className="header" >
          <li><Link to="/" activeClassName="active">Home</Link></li>
          ...
        </ul
      </div>
    )
  }
})
```

[React! React! React!](https://www.kirupa.com/react/examples/todo.htm)

https://www.kirupa.com/react/setting_up_react_environment.htm

- 바벨: https://babeljs.io/
- npm 문서: https://docs.npmjs.com/
- 웹팩: https://webpack.github.io/
- 바우어(Bower): https://bower.io
