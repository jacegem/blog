---
title: '[Leaflet] 플러그인 heatmap.js 사용하기'
date: 2018-01-24 15:52:29
tags: [leaflet, javascript, heatmap]
categories:
- Programming
- Javascript
---

# [Leaflet] 플러그인 heatmap.js 사용하기

![heatmap.js](https://goo.gl/1Y46i3)

`heatmap.js`를 사용하여 heatmap을 표현해 보겠습니다. 

![heatmap.js](https://goo.gl/ySxq5x)

https://www.patrick-wied.at/static/heatmapjs/example-heatmap-leaflet.html 에서 예제를 확인할 수 있습니다. 

## Leaflet CSS, JS 추가

```html
<html>

<head>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.2.0/dist/leaflet.css"/>
  <script src="https://unpkg.com/leaflet@1.2.0/dist/leaflet.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/heatmap.js/2.0.2/heatmap.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/leaflet-heatmap@1.0.0/leaflet-heatmap.min.js"></script>
</head>

<body>
</body>

</html>
```

leaflet 사용을 위해서 css, js를 포함하는 기본 코드를 작성합니다. `leaflet-heatmap.html`(또는 원하는 파일명)로 파일을 저장합니다.

leaflet에서 heatmap.js를 사용하여 표현하기 위해서는 `heatmap.js` 와 `leaflet-heatmap.js`를 추가합니다.

## 지도 영역 추가

```html
<body>
  <div id="mapid" style="height:100%;"></div>
  <script>
  // sciprt 작성
  </script>
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
    L.tileLayer('http://tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(mymap);
  </script>
</body>
```

오픈스트리트맵('http://tile.openstreetmap.org/{z}/{x}/{y}.png')을 사용하여 지도를 생성합니다. 

## 좌표 정보 생성

```html
<script>
  var lat = 37.481;
  var lng = 126.893;
  var zoom = 20;

  var mymap = L.map('mapid', {
      center: [lat, lng],
      zoom: zoom,
  });

  L.tileLayer('http://tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(mymap);

  var pointList = [];
  var pointCnt = 10;
  var scale = 1000;

  for (var i = 0; i < pointCnt; i++) {
      pointList.push({
          lat: lat + i / scale,
          lng: lng + i / scale,
          count: i
      }, );
  }

  for (var i = 0; i < pointCnt; i++) {
      var point = pointList[i];
      L.marker({
          lat: point.lat,
          lng: point.lng
      }).addTo(mymap);
  }
</script>
```

heatmap 표현을 위해서는 데이터가 필요하기 때문에 좌표정보와 데이터를 포함하는 오브젝트를 생성하여 처리합니다.

```javascript
{
    lat: lat,
    lng: lng,
    count: i
}
```

![OpenStreetMap](https://goo.gl/hP7M1p)

## heatmap 데이터 설정

```html
<script>
  // 이전 코드
  
  var pointList = [];
  var pointCnt = 10;
  var scale = 1000;

  for (var i = 0; i < pointCnt; i++) {
      pointList.push({
          lat: lat + i / scale,
          lng: lng + i / scale,
          count: i
      }, );
  }

  for (var i = 0; i < pointCnt; i++) {
      var point = pointList[i];
      L.marker({
          lat: point.lat,
          lng: point.lng
      }).addTo(mymap);
  }
  
  var cfg = {
      //"radius": 2,
      "radius": 200,
      "maxOpacity": .5,
      //"scaleRadius": true,
      "scaleRadius": false,
      "useLocalExtrema": true,
      latField: 'lat',
      lngField: 'lng',
      valueField: 'count'
  };

  var heatmapLayer = new HeatmapOverlay(cfg).addTo(mymap);

  var testData = {
      max: 10,
      data: pointList
  }

  heatmapLayer.setData(testData);  
  
</script>
```

해당 줌 영역에서 heatmap이 보이도록 설정값을 수정하였습니다. 

- "radius": 200,
- "scaleRadius": false,

![heatmapLayer](https://goo.gl/b3mvVa)

```html
<script>
  // 이전 코드
  
  var heatmapLayer = new HeatmapOverlay(cfg).addTo(mymap);

  var max = 10;
  var animationTimer = window.setInterval(function(){
      for(var i =0; i < pointCnt; i++){
          var point = pointList[i];
          point.count = ( point.count + 1 ) % max
      }
      var testData = {
          max: max,
          data: pointList
      }
      heatmapLayer.setData(testData);
  }, 100)  
</script>
```

setInterval을 사용하여 애니메이션 효과를 추가할 수 있습니다. 

![animation](https://goo.gl/bSUpAF)

