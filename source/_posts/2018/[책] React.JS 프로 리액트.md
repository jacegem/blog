---
title: "[책] React.JS 프로 리액트"
date: 2018.05.08
tags: [react, front-end]
categories:
- Progrmming
- Javascript
---

# [책] React.JS 프로 리액트

React.js를 이용한 모던 프런트엔드 구축

[![](http://image.yes24.com/goods/27871271/147x215)</br>프로 리액트 React.js를 이용한 모던 프런트엔드 구축</br>카시우 지 소자 안토니우 저/최민석 역](http://blog.yes24.com/lib/adon/View.aspx?blogid=9654534&goodsno=27871271&idx=25563&ADON_TYPE=B&regs=b)

# 라우팅

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

# 동형 리액트 애플리케이션

동형`isomorphic` 자바스크립트 애플리케이션은 코드가 전체 또는 부분적으로 클라이언트와 서버 간에 공유되는 애플리케이션이다.

# 리액트 컴포넌트의 테스트

## Jest

제스트는 리액트가 권장하는 테스트 프레임워크다. 널리 이용하는 재스민`Jasmine` 프레임워크를 기반으로 하며 여러 유용한 기능이 추가됐다.

- 가상 DOM 구현으로 테스트를 실행한다.(따라서 테스트를 명령줄에서 실행 할 수 있다.)
- JSX를 기본적으로 지원한다.

# 웹팩

## 소스맵 생성

원래 작성된 파일을 가리키는 소스맵을 생성하는 devtool 옵션

| devtool 옵션                 | 설명                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| source-map                   | 모든 기능이 포함된 완전한 소스맵을 별도의 파일로 생성<br />이 옵션은 최고 품질의 소스맵을 생성하지만 빌드 프로세스가 느려진다. |
| cheap-module-source-map      | 별도의 파일에 칼럼 매핑을 제외한 소스 맵을 생성<br />칼럼 매핑을 생략하면 빌드 속도는 향상되지만 디버깅 할 때는 약간의 불편함이 있다.<br />브라우저 개발자 툴은 원래 소스 파일의 행만 가리킬 수 있으며, 특정 칼럼(또는 문자)을 가리킬 수 없다. |
| eval-source-map              | `eval`를 사용해 동일한 파일 안에 전체 소스맵과 소스코드 모듈을 중첩해 번들로 만든다.<br />이 옵션을 사용하면 빌드 시간에 대한 부담 없이 모든 기능이 포함된 소스맵을 생성할 수 있지만 자바스크립트를 실행할 때 성능과 보안이 저하되는 단점이 있다.<br />즉, 개발 중에는 유용하지만 실무 버전을 빌드할 때는 사용하지 말아야 한다. |
| cheap-module-eval-source-map | 빌드 중에 소스 맵을 생성하는 가장 빠른 방법이다. 생성되는 소스맵에는 번들 자바스크립트 파일이 칼럼 매핑을 제외하고 동일하게 인라인으로 포함된다.<br />이전 옵션과 마찬가지로 자바스크립트 실행 시간에 부정적인 영향을 미치므로 실무용 번들을 생성할 때는 적합하지 않다. |

## 웹팩 개발 서버

devserver 설정

| Devserver 설정     | 설명                                                         |
| ------------------ | ------------------------------------------------------------ |
| contentBase        | 기본적으로 웹팹 개발 서버는 프로젝트 루트에 있는 파일을 서비스 한다.<br />다른 폴더의 파일을 서비스하려면 이 설정으로 특정 컨텐츠 기반을 구성해야 한다. |
| port               | 사용할 포트를 지정하며, 생략할 경우 기본값은 `8080`이다.     |
| inline             | `true`로 설정하면 작은 클라이언트 엔트리를 번들에 삽입해 페이지가 변경되면 새로 고친다. |
| colors             | 서버가 터미널에 출력하는 내용에 색상을 지정한다.             |
| historyApiFallback | HTML5  히스토리 API를 이용하는 단일 페이지 애플리케이션을 개발할 때 유용한 옵션으로서 `true`로 설정하면 기존 에셋과 매핑되지 않는 웹팩 개발 서버에 대한 모든 요청이 곧바로 `/`로 라우팅 된다.{} |

```javascript
{
    "scripts":{
        "start": "node_modules/.bin/webpack-dev-server --progress"
    }
}
```
>  `--progress` 매개변수는 명령줄에서만 사용할 수 있으며, 빌드 단계 중에 터미널에 진행 표시줄을 표시한다.

## 로더

웹팩의 가장 흥미로운 기능 중 하나로 로더 `loader`가 있다.
로더를 이용하면 외부 스크립트와 도구를 토앻 소스 파일을 전처리하고 다양한 변경과 변환을 적용할 수 있다.

로더는 별도로 설치해야 하며ㅕ, webpack.config.js의 `modules`키에서 구성해야 한다. 

- test: 이 로더로 처리하기 위해 일치해야 하는 파일 확장자를 비교하는 정규 표현식(필수)
- loader: 로더의 이름(필수)
- include/exclude: 로더가 명시적으로 추가하거나 무시할 폴더와 파일을 수동으로 지정하는 옵션 설정
- Query: 로더로 추가 옵션을 전달하는 데 이용되는 쿼리 설정

```shell
$ npm install --save json-loader
```

웹팩 구성

```javascript
module.exports = {
    devtool: 'eval-source-map',
    entry: __dirname + '/app/main.js',
    output: {
        ...
    }, // Omitted for brevity,
    module: {
        loaders: [
            {
                test: /\.json$/,
                loader: 'json'                
            }
        ]    
    },
    devServer: {
        contentBase: './public',
        colors: true,
        historyApiFallback: true,
        inline: true        
    }    
}
```
## 바벨

- 아직 일부 브라우저에서 지원되지 않는 자바스크립트의 다음 버전을 이용할 수 있게 해준다.
- 리액트의 JSX와 같은 자바스크립트 구문 확장을 이용할 수 있게 해준다.

```shell
$ npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-reset-react
```



```javascript
module.exports = {
    devtool: 'eval-source-map',
    entry: __dirname + '/app/main.js',
    output: {
        ...
    }, // Omitted for brevity,
    module: {
        loaders: [
            {
                test: /\.json$/,
                loader: 'json'                
            },
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel',
                query: {
                    presets: ['es2015', 'react']
                }
            }
        ]    
    },
    devServer: {
        contentBase: './public',
        colors: true,
        historyApiFallback: true,
        inline: true        
    }    
}
```
### 바벨 파일 구성

```javascript
module.exports = {
    devtool: 'eval-source-map',
    entry: __dirname + '/app/main.js',
    output: {
        ...
    }, // Omitted for brevity,
    module: {
        loaders: [
            {
                test: /\.json$/,
                loader: 'json'                
            },
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel',
            }
        ]    
    },
    devServer: {
        contentBase: './public',
        colors: true,
        historyApiFallback: true,
        inline: true        
    }    
}
```
`.babelrc`라는 파일을 만든다.

```javascript
{
    "presets": ["react", "es2015"]
}
```
