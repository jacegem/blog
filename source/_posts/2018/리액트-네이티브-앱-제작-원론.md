---
title: "[책] 리액트 네이티브 앱 제작 원론"
date: 2018.05.21
tags: [react-native, react, native, app]
categories:
- Programming
- Javascript
---

# [책] 리액트 네이티브 앱 제작 원론

[![](http://image.yes24.com/goods/58409654/147x215)<br>리액트 네이티브 앱 제작 원론<br>에릭 마시엘로,제이콥 프리드만 공저/이태상 역](http://blog.yes24.com/lib/adon/View.aspx?blogid=9654534&goodsno=58409654&idx=25576&ADON_TYPE=B&regs=b)

## 컴포넌트 업데이트

```javascript
shouldComponentUpdate(nextProps, nextState){
  if (this.props.uid !== nextProps.uid){
    return true;
  }
  return false;
}
```

## 리액트 네이티브

> 바벨이 낯설다면 https://babeljs.io/repl 에서 바벨 리플(Babel REPL)이라는 온라인 트랜스파일러를 사용해 볼 수 있다.

## 엑스코드 설치

![XCODE](https://content.screencast.com/users/beneapp/folders/Snagit/media/ec19a4b8-1dea-4cb1-83c9-7877b3ebb5b8/2018-04-25_07-12-32.png)

## 홈브루 설치

https://brew.sh

터미널에 붙여 넣어 실행한다.

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## 노드, npm, 왓치맨, 플로 설치

```sh
brew install node
brew install watchman
brew install flow
```

## 리액트 네이티브 CLI 설치

```sh
npm install -g react-native-cli
```

## Error: ACCESS:permission denied...

1.  npm config get prefix 를 실행한다.
2.  /usr/local 이 보인다면 sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}를 실행한다.
3.  패스워드를 물어보면 패스워드를 입력하고 리턴을 누른다. 이로써 권한 설정이 완료된다.
4.  만약 npm config get prefix 를 실행했을 때 다른 결과가 보였다면, npm 권한 문제를 해결하기 위한 자세한 사항을 설명하는 https://docs.npmjs.com/getting-started/fixing-npm-permissions 페이지를 확인해 본다.

## 루트 컴포넌트 등록

```javascript
AppRegistry.registerComponent("HelloWrold", () => HelloWorld);
```

## 플렉스박스

justifyContent 속성을 사용하면 메인 축을 따라 아이템을 배치할 수 있다. 메인 축이 아닌 축을 크로스 축이라고 하며, alignItems 속성을 통해 크로스 축을 따라 아이템을 배치할 수 있다.

- 플렉스박스 개구리(http://flexboxfroggy.com)

# 리액트 네이티브 컴포넌트

# 플럭스와 리덕스

## 플럭스 구현하기

높은 수준에서 보면 플럭스는 하나의 순환적인 패턴이라고 생각할 수 있다.
대개 그 순환은 뷰레이어에서 시작한다.
뷰레이어는 리액트 컴포넌트다.
사용자가 `뷰`와 상호작용을 하면 플럭스는 `액션 생성자`를 사용해 `액션`을 만든다.
이 액션은 `디스패처`에 전달되는데, 디스패처는 한 번에 하나의 액션만을 처리하는 싱글턴 컴포넌트다.
디스패처는 액션을 다시 해당 애플리케이션 `스토어`에 전달(디스패치)한다.
각 스토어는 액션을 처리하고 그에 따라 자신의 내부 상태를 변경한다.
그 다음엔 `스토어`는 변경사항을 등록된 뷰들에게 전파하고, 이는 다시 렌더링을 일으키며, 이로써 하나의 순환이 완성된다.

## 리덕스 설치

```shell
$ npm install redux --save
```

## 리액트-리덕스 설치

```shell
$ npm install react-redux --save
```

# 애니메이션과 제스처

- Animated
- LayoutAnimation
- PanResponder

```shell
$ react-native install react-native-vector-icons
```

- https://www.decoide.org

react-native-web

```shell
$ npm install --save react react-native-web
```

- https://github.com/Microsoft/react-native-windows

```shell
$ npm install --save-dev rnpm-plugin-windows
$ react-native windows
$ react-native run-windows
```

- https://github.com/ptmt/react-native-macos

```shell
$ npm install react-native-macos-cli -g
```

# 참조 링크

- https://github.com/jondot/awesome-react-native
- http://reactnative.cc
- https://devchat.tv/react-native-radio
- http://www.reactnative.com
- https://github.com/react-native-training/react-natvie-training
- https://expo.io
- https://js.coach/?search=react-native
- https://github.com/donglowder/react-native-appletv
