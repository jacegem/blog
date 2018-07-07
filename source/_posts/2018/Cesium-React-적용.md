---
title: "Cesium-React 적용"
date: 2018.05.14
tags: [Cesium, React, javascript, react, gis, 3D]
categories:
- Programming
- Javascript
---

# Cesium-React 적용

출처: [cesium-react - npm](https://www.npmjs.com/package/cesium-react)

## create-react-app

react 프로젝트 생성

```shell
create-react-app cesium-react-test
cd cesium-react-test
yarn eject
```

## yarn install

모듈 설치

* cesium
* cesium-react
* html-webpack-include-assets-plugin
* copy-webpack-plugin

```shell
yarn add cesium cesium-react html-webpack-include-assets-plugin copy-webpack-plugin
```

## webpack.config 수정

### config/webpack.config.dev.js

```javascript
// config/webpack.config.dev.js

const HtmlIncludeAssetsPlugin = require("html-webpack-include-assets-plugin");
const CopyWebpackPlugin = require("copy-webpack-plugin");

module.exports = {
  externals: {
    cesium: "Cesium"
  },  
  plugins: {
    new CopyWebpackPlugin([
      {
        from: "node_modules/cesium/Build/CesiumUnminified",
        to: "cesium"
      }
    ]),
    new HtmlIncludeAssetsPlugin({
      append: false,
      assets: [
        "cesium/Widgets/widgets.css",
        "cesium/Cesium.js"
      ]
    }),
    new webpack.DefinePlugin({
      "process.env": {
        CESIUM_BASE_URL: JSON.stringify("/cesium")
      }
    }),
    // ...
  }
  // ...
}
```

### config/webpack.config.prod.js

```javascript
// config/webpack.config.prod.js

const HtmlIncludeAssetsPlugin = require("html-webpack-include-assets-plugin");
const CopyWebpackPlugin = require("copy-webpack-plugin");

module.exports = {
  externals: {
    cesium: "Cesium"
  },  
  plugins: {
    new CopyWebpackPlugin([
      {
        from: "node_modules/cesium/Build/Cesium",
        to: "cesium"
      }
    ]),
    new HtmlIncludeAssetsPlugin({
      append: false,
      assets: [
        "cesium/Widgets/widgets.css",
        "cesium/Cesium.js"
      ]
    }),
    new webpack.DefinePlugin({
      "process.env": {
        CESIUM_BASE_URL: JSON.stringify("/cesium")
      }
    }),
    // ...
  }
  // ...
}
```

## 소스 수정

### App.js 수정

```javascript
import React, { Component } from "react";
import logo from "./logo.svg";
import "./App.css";
import CesiumPage from "./CesiumPage";

class App extends Component {
  render() {
    return (
      <div className="App">
        <CesiumPage />
      </div>
    );
  }
}

export default App;
```

### CesiumPage.js 생성

```javascript
import React, { Component } from "react";
import { Cartesian3 } from "cesium";
import { Viewer, Entity } from "cesium-react";

class CesiumPage extends Component {
  render() {
    return (
      <div>
        cesium page
        <Viewer full>
          <Entity name="Seoul" position={Cartesian3.fromDegrees(126.97224, 37.40076, 100)} point={{ pixelSize: 10 }}>
            test
          </Entity>
        </Viewer>
      </div>
    );
  }
}

export default CesiumPage;
```

![Cesium-React](http://bit.ly/2rsqm1f)
