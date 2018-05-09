---
title: "[책] React.JS 프로 리액트 (2/2)"
date: 2018.05.08
tags: [react, front-end]
categories:
- Progrmming
- Javascript
---

# [책] React.JS 프로 리액트 (2/2)



React.js를 이용한 모던 프런트엔드 구축

[![](http://image.yes24.com/goods/27871271/147x215)</br>프로 리액트 React.js를 이용한 모던 프런트엔드 구축</br>카시우 지 소자 안토니우 저/최민석 역](http://blog.yes24.com/lib/adon/View.aspx?blogid=9654534&goodsno=27871271&idx=25563&ADON_TYPE=B&regs=b)

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
## HMR

웹팩이 널리 알려지는 데 기여한 특성 중 하나로 HMR `Hot Module Replacement`이 있다. 

1. 웹팩 구성에 `HotModuleReplacementPlugin`을 추가한다.
2. 웹팩 개발 서버 구성에 `hot` 매개변수를 추가하다.

중요 사항을 정리하면 다음과 같다.

- 웹팩과 바벨은 서로 다른 별개의 도구다.
- 두 도구는 잘 어울린다.
- 바벨은 플러그인을 통해 확장할 수 있다.
- HMR은 컴포넌트의 코드를 수정할 때 브라우저에서 실시간으로 컴포넌트를 업데이트할 수 있는 웹팩 플러그인이며, 제대로 작동하려면 모듈에 특수한 코드를 추가해야 한다.
- 바벨의  react-transform-hmr 플러그인은 자동으로 모든 리액트 컴포넌트에 필요한 HMR코드를 삽입한다.

npm을 이용해 필요한 바벨 플러그인을 설치한다.

```shell
$ npm install --save-dev babel-plugin-react-transform react-transfomr-hmr
```