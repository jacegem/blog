---
title: "Cesium Viewer 화면 설정 확인 "
date: 2018.05.09
tags: [cesium, viewer, react, javascript]
categories:
  - Programming
  - Javascript
---

# Cesium Viewer 화면 설정 확인

[Viewer - Cesium Documentation](https://cesiumjs.org/Cesium/Build/Documentation/Viewer.html) 에서 자세한 내용을 확인 할 수 있습니다.

![기본화면](http://bit.ly/2rvYd9J)

## animation: false

```javascript
import React, { Component } from "react"
import { Viewer } from "cesium"

class CesiumPage3 extends Component {
  componentDidMount() {
    this.viewer = new Viewer(this.cesiumContainer, {
      animation: false,
    })
  }

  render() {
    return (
      <div
        className="cesiumWidget"
        ref={element => (this.cesiumContainer = element)}
      />
    )
  }
}

export default CesiumPage3
```

![animation: false](http://bit.ly/2rzjTSy)

`좌측 하단`의 애니메이션 위젯이 사라졌습니다.

## timeline: false

```javascript
this.viewer = new Viewer(this.cesiumContainer, {
  animation: false,
  timeline: false,
})
```

![timeline: false](http://bit.ly/2rtYwlr)

하단의 타임라인 위젯이 사라졌습니다.

## navigationHelpButton: false

```javascript
this.viewer = new Viewer(this.cesiumContainer, {
  animation: false,
  timeline: false,
  navigationHelpButton: false,
})
```

![navigationHelpButton: false](http://bit.ly/2rvelsd)

우측 상단의 네비게이션 도움말 버튼이 사라졌습니다.

## fullscreenButton: false

```javascript
this.viewer = new Viewer(this.cesiumContainer, {
  animation: false,
  timeline: false,
  navigationHelpButton: false,
  fullscreenButton: false,
})
```

![fullscreenButton: false](http://bit.ly/2ruGDTs)

우측 하단의 전체화면 버튼이 사라졌습니다.

## AccessToken 설정

https://cesium.com/ion/tokens 에서 가입하면 발급되는 `Access Token`을 사용합니다. Default 토큰를 사용하였습니다. 오른쪽의 복사 버튼을 클릭하면 토큰이 복사됩니다.

![](http://bit.ly/2rvgPGL)

```javascript
import React, { Component } from "react"
import { Viewer, Ion } from "cesium"

class CesiumPage3 extends Component {
  componentDidMount() {
    Ion.defaultAccessToken =
      "eyJclkjciOiJIUzI1NiIsInR5acvf6IkpXVCJ9.eyJqdGkiOiIFHGzNjFkZC0zMmRmLTRkN2EtOWJiasdcWQ0MzYyOGUzMDkiLCJpZCI6Nzc4LCJpYXQiOjE1MjU4MjkzMTF9_YOUR_ACCESS_TOKEN"

    this.viewer = new Viewer(this.cesiumContainer, {
      animation: false,
      timeline: false,
      navigationHelpButton: false,
      fullscreenButton: false,
    })
  }

  render() {
    return (
      <div
        className="cesiumWidget"
        ref={element => (this.cesiumContainer = element)}
      />
    )
  }
}

export default CesiumPage3
```

![AccessToken](http://bit.ly/2rAt05t)

하단의 AccessToken관련 메시지가 사라졌습니다.
