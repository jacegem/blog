---
title: '[Leaflet] 플러그인 PolylineDecorator 사용하기'
date: 2018-01-23 15:52:12
tags: [leaflet, javascript, PolylineDecorator]
categories:
- Programming
- Javascript
---

# [Leaflet] 플러그인 PolylineDecorator 사용하기

![](https://goo.gl/twfamv)

Leaflet 플러그인 중에서, Markers & renders에 있는 `Leaflet.PolylineDecorator`를 사용하여 폴리라인을 꾸며보겠습니다.

`https://github.com/bbecquet/Leaflet.PolylineDecorator` 깃헙 페이지에서 사용법을 확인할 수 있습니다. 

간단하게 사용하기 위해 CDN(https://cdnjs.com/libraries/leaflet-polylinedecorator)에 있는 leaflet-polylinedecorator.js를 사용하겠습니다. 

## Leaflet CSS, JS 추가

```html
<html>

<head>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.2.0/dist/leaflet.css"/>
  <script src="https://unpkg.com/leaflet@1.2.0/dist/leaflet.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet-polylinedecorator/1.1.0/leaflet.polylineDecorator.js"></script>
</head>

<body>
</body>

</html>
```

leaflet 사용을 위해서 css, js를 포함하는 기본 코드를 작성합니다. `leaflet-polylinedecorator.html`(또는 원하는 파일명)로 파일을 저장합니다.

## 지도 영역 추가

```html
<body>
  <div id="mapid" style="height:100%;"></div>
</body>
```

## 지도 생성 스크립트

```html
<body>
  <div id="mapid" style="height:100%;"></div>
  <script>
    var lat = 37.481;
    var lng = 126.893;
    var zoom = 20;
    var mymap = L.map('mapid', {
        center: [lat, lng],
        zoom: zoom
    });
    L.tileLayer('http://xdworld.vworld.kr:8080/2d/Base/201710/{z}/{x}/{y}.png').addTo(mymap);
  </script>
</body>
```

vworld 에서 제공하는 타일맵 서비스를 이용합니다. (http://xdworld.vworld.kr:8080/2d/Base/201710/{z}/{x}/{y}.png)

지도를 생성하고, center, zoom을 설정하여 최초 보이는 영역을 설정하였습니다. 

![leaflet](https://goo.gl/oQqtp4)

지금까지 작성한 파일을 저장하고, 크롬 브라우저로 실행하면 브라우저에 지도가 나오는 모습을 확인할 수 있습니다.

## 위치 데이터 생성

```html
<script>
  var x = 37.481;
  var y = 126.893;
  var z = 20;
  var mymap = L.map('mapid', {
      center: [x, y],
      zoom: z
  });
  L.tileLayer('http://xdworld.vworld.kr:8080/2d/Base/201710/{z}/{x}/{y}.png').addTo(mymap);

  var points = [];  
  var scale = 1000;
  for (var i = 0; i < 10; i++) {
      points.push([x + i / scale, y + i / scale]);
  }

  for (var i = 0; i < points.length; i++) {
      var point = points[i];
      L.marker(point).addTo(mymap);
  }
</script>
```

지도상에 생성한 위치에 마커가 표시됩니다. 

![](https://goo.gl/UidDnb)

## polylineDecorator 생성

```html
<script>
  // 이전 코드
  
  var points = [];  
  var scale = 1000;
  for (var i = 0; i < 10; i++) {
      points.push([x + i / scale, y + i / scale]);
  }

  for (var i = 0; i < points.length; i++) {
      var point = points[i];
      L.marker(point).addTo(mymap);
  }
  
  var highlightPolyline = L.polylineDecorator(pointList, {
      patterns: [ pattern, pattern ]
  }).addTo(mymap);
  
</script>
```

`pattern` 들을 입력하여 polylineDecorator를 생성합니다. 

```html
<script>
  // 이전 코드 
  
  var highlightPolyline = L.polylineDecorator(pointList, {
      patterns: [{
          offset: 0,
          repeat: 20,
          symbol: L.Symbol.dash({
              pixelSize: 10,
              pathOptions: {
                  color: '#f00',
                  weight: 2
              }
          })
      }]
  }).addTo(mymap);
  
</script>
```

dash 심볼을 추가하여 생성하였습니다. 

![](https://goo.gl/H37a98)

```html
<script>
  // 이전 코드 
  
  var highlightPolyline = L.polylineDecorator(pointList, {
      patterns: [{
          offset: 0,
          repeat: 20,
          symbol: L.Symbol.dash({
              pixelSize: 10,
              pathOptions: {
                  color: '#f00',
                  weight: 2
              }
          })
      }, {
          offset: 0,
          repeat: 50,
          symbol: L.Symbol.arrowHead({
              pixelSize: 15,
              polygon: false,
              pathOptions: {
                  stroke: true
              }
          })
      }]
  }).addTo(mymap);
  
</script>
```

patterns 속성에는 여러개의 pattern을 넣을 수 있습니다. 화살표를 하나 더 추가하여 생성하였습니다. 

![](https://goo.gl/8AypU8)

## fitBounds

```html
<script>
  // 이전 코드 
  
  var highlightPolyline = L.polylineDecorator(pointList, {
      patterns: [{
          offset: 0,
          repeat: 20,
          symbol: L.Symbol.dash({
              pixelSize: 10,
              pathOptions: {
                  color: '#f00',
                  weight: 2
              }
          })
      }, {
          offset: 0,
          repeat: 50,
          symbol: L.Symbol.arrowHead({
              pixelSize: 15,
              polygon: false,
              pathOptions: {
                  stroke: true
              }
          })
      }]
  }).addTo(mymap);
  
  mymap.fitBounds(highlightPolyline.getBounds());  
</script>
```

fitBounds 함수를 호출하여 지도상에서 생성한 polylineDecorator가 모두 보이게 됩니다. 

![](https://goo.gl/BZp9iq)

## 애니메이션

```html
<script>
  // 이전 코드 
  
  var highlightPolyline = L.polylineDecorator(pointList, {
      patterns: [{
          offset: 0,
          repeat: 20,
          symbol: L.Symbol.dash({
              pixelSize: 10,
              pathOptions: {
                  color: '#f00',
                  weight: 2
              }
          })
      }, {
          offset: 0,
          repeat: 50,
          symbol: L.Symbol.arrowHead({
              pixelSize: 15,
              polygon: false,
              pathOptions: {
                  stroke: true
              }
          })
      }]
  }).addTo(mymap);
  
  mymap.fitBounds(highlightPolyline.getBounds());  
  
  var arrowRepeat = 50;
  var arrowOffset = 0;

  var animationTimer = window.setInterval(function () {
      highlightPolyline.setPatterns([{
          offset: 0,
          repeat: 20,
          symbol: L.Symbol.dash({
              pixelSize: 10,
              pathOptions: {
                  color: '#f00',
                  weight: 2
              }
          })
      }, {
          offset: arrowOffset,
          repeat: arrowRepeat,
          symbol: L.Symbol.arrowHead({
              pixelSize: 15,
              polygon: false,
              pathOptions: {
                  stroke: true
              }
          })
      }])
      arrowOffset += 5;
      if (arrowOffset > arrowRepeat)
          arrowOffset = 0;
  }, 100);
</script>
```

![](https://goo.gl/fyhbCv)

