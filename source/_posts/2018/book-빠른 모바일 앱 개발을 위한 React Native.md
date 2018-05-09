---
title: "[책] 빠른 모바일 앱 개발을 위한 React Native"
date: 2018.05.16
tags: [react, native]
categories:
- Progrmming
- Javascript
---

# [책] 빠른 모바일 앱 개발을 위한 React Native

자바스크립트로 만드는 네이티브 모바일 앱 개발 가이드

[![빠른 모바일 앱 개발을 위한 React Native(리액트 네이티브)](http://image.yes24.com/goods/30498529/147x215) 바니 아이젠먼 저/이종은 역](http://blog.yes24.com/lib/adon/View.aspx?blogid=9654534&goodsno=30498529&idx=25551&ADON_TYPE=B®s=b)



# 플랫폼 API

XHR은 XMLHttpRequest의 줄임말이다.

XHR을 이용하여 사진을 POST로 요청하는 기본 구조

```javascript
var xhr = new XMLHttpRequest();
xhr.open('POST', 'http://posttestserver.com/post.php');
var formdata = new FormData();
formdata.append('image', {...this.state.photo, name: 'image.jpg'});
xhr.send(formdata);
```

> Promise는 비동기 방식을 처리할때 사용되는 객체이다. https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise


### SmarterWeather 프로젝트 폴더 및 파일 구조
- SmarterWeather
  - Forecast
    - index.js
  - LocationButton
    - index.js
    - style.js
  - PhotoBackdrop
    - camera_roll_example.js
    - index.js
    - local_image.js
    - style.js
  - android
    - app
    - build
    - build.gradle
    - gradle
    - gradle.properties
    - gradlew
    - gradlew.bat
    - settings.gradle
  - index.android.js
  - index.ios.js
  - ios
    - SmarterWeather
    - SmarterWeather.xcodeproj
    - SmarterWeatherTests
    - main.jsbundle
  - node_modules
    - react-native
  - package.json
  - styles
    - typography.js
  - weather_project.js

# 모듈

> lodash와 underscore는 유용한 함수들을 모아둔 자바스크립트 유틸리티로 거의 동일한 기능과 API 구조이다. 두 라이브러리는 합쳐질 예정이다.

---

- rnpm (React Native Package Manager)

---

- RCTBridgeModule 해더 불어오기
- RCTBridgeModule 인터페이스 정의하기
- RCT_EXPORT_MODULE 매크로 실행하기
- RCT_EXPORT_METHOD 매크로를 이용하여 하나 이상의 함수를 익스포트(export)하기

---

- brew update
- brew upgrade
- brew upgrade node
- rm -rf node_modules
- npm install (yarn)

# 안드로이드 애플리케이션 배포하기

로만 프로젝트 : https://romannurik.github.io/AndroidAssetStudio/icons-launcher.html

## 배포용 APK 만들기

1. 서명 키 생성
2. 그래들(gradle) 변수 설정
3. 애플리케이션 그래들 설정에 서명 설정 추가
4. 배포용 APK 생성
5. 배포용 APK를 디바이스에 설치

```shell
$ keytool -genkey -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
```

~/.gradle/gradle.properties 에 변수 추가
```javascript
MYAPP_RELEASE_STORE_FILE=my-release-key.keystore
MYAPP_RELEAES_KEY_ALIAS=my-key-alias
MYAPP_RELEASE_STORE_PASSWORD=******
MYAPP_RELEASE_KEY_PASSWORD=******
```

```shell
$ cd android && ./gradlew assembleRelease
```

프로젝트의 android/ 디렉토리에서 다음 명령어를 실행하여 서명된 APK를 설치할 수 있다.
```shell
$ ./gradlew installRelease
```


# ES6 문법

## 비구조화
ES5
```javascript
var myObj = {a:1, b:2}
var a = myObj.a;
var b = myObj.b;
```

ES6 비구조화
```javascript
var {a, b} = myObj
```

## 모듈 불러오기
ES5
```javascript
var OtherComponent = require('./other_component');
var MyComponent = React.createClass({
  ...
});
module.exports = MyComponent;
```

ES6 모듈 불러오기
```javascript
import OtherComponent from './other_component';
var MyComponent = React.createClass({
  ...
});
export default MyComponent;
```

## 함수 축약 표현식
ES5
```javascript
render: function(){
  return <Text>hi</Text>
}
```

ES6 함수 축약 표현식

```javascript
render() {
  return <Text>hi</Text>
}
```

## 화살표 함수
ES5
```javascript
var callbackFunc = function(val){
  console.log('Do something');
}.bind(this);
```

ES6 화살표 함수
```javascript
var callbackFunc = (val) => {
  console.log('Do something');
}
```

## 문자열 조립
ES5
```javascript
var API_KEY = 'abcd';
var url = 'http://someapi.com/request?key=' + API_KEY;
```

ES6 문자열 조립
```javascript
var API_KEY = 'abcd';
var url = `http://someapi.com/request?key={API_KEY}`;
```

## 클래스
ES5
```javascript
var HelloMessage = React.createClass({
  render: function(){
    ...
  }
});
```

ES6 클래스
```javascript
class HelloMessage extends Component {
  render() {
    ...
  }
}
```


## 출처

- https://github.com/yomybaby/learning-react-native/blob/master/Depends/iOS/PianoStairs.mp4?raw=true


- https://github.com/brentvatne/react-native-video
- https://facebook.github.io/react-native/docs/native-modules-android.html
- https://github.com/brentvatne/react-native-linear-gradient



