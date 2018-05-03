---
title: '[Leaflet] 시작하기 Quick Start Guide'
date: 2018.01.22 15:51:20
tags: [leaflet, javascript]
categories:
- Programming
- Javascript
---

# [Leaflet] 시작하기 Quick Start Guide

http://leafletjs.com/examples/quick-start/ 에 있는 가이드 문서를 보면서 실습을 진행하겠습니다.

## Leaflet CSS, JS 추가

```html
<html>

<head>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.2.0/dist/leaflet.css"/>
  <script src="https://unpkg.com/leaflet@1.2.0/dist/leaflet.js"></script>
</head>

<body>
</body>

</html>
```

leaflet 사용을 위해서 css, js를 포함하는 기본 코드를 작성합니다. `leaflet.html`(또는 원하는 파일명)로 파일을 저장합니다.

## 지도 영역 추가

```html
<body>
  <div id="mapid" style="height:100%;"></div>
</body>
```

body 태그안에 맵을 표출할 영역을 div 태그로 생성합니다. 브라우저 전체에 표출되도록 높이를 100%로 하는 스타일을 추가하였습니다. 

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

## 마커 추가

지정한 lat, lng 좌표에 마커를 추가합니다. 

```html
<script>
    var lat = 37.481;
    var lng = 126.893;
    var zoom = 20;
    var mymap = L.map('mapid', {
        center: [lat, lng],
        zoom: zoom
    });
    L.tileLayer('http://xdworld.vworld.kr:8080/2d/Base/201710/{z}/{x}/{y}.png').addTo(mymap);

    var marker = L.marker([lat, lng]).addTo(mymap);
</script>
```

`var marker = L.marker([lat, lng]).addTo(mymap);` 코드를 추가한 후 브라우저로 파일을 열면 마커가 추가된 모습을 확인할 수 있습니다. 

![마커 추가](https://goo.gl/z5kW28)

## 원 추가

지정한 lat, lng 좌표에 원를 추가합니다. 색상, 투명도, 반경등을 설정할 수 있습니다.

```html
<script>
    var lat = 37.481;
    var lng = 126.893;
    var zoom = 20;
    var mymap = L.map('mapid', {
        center: [lat, lng],
        zoom: zoom
    });
    L.tileLayer('http://xdworld.vworld.kr:8080/2d/Base/201710/{z}/{x}/{y}.png').addTo(mymap);

    var marker = L.marker([lat, lng]).addTo(mymap);

    var distance = 0.001;

    var circle = L.circle([lat - distance, lng - distance], {
        color: 'red',
        fillColor: '#f03',
        fillOpacity: 0.5,
        radius: 50
    }).addTo(mymap);
</script>
```

![원 추가](https://goo.gl/Lz2Xbd)


## 폴리곤 추가

좌표 정보들을 입력하여 폴리곤을 생성합니다. 

```html
<script>
    var lat = 37.481;
    var lng = 126.893;
    var zoom = 20;
    var mymap = L.map('mapid', {
        center: [lat, lng],
        zoom: zoom
    });
    L.tileLayer('http://xdworld.vworld.kr:8080/2d/Base/201710/{z}/{x}/{y}.png').addTo(mymap);

    var marker = L.marker([lat, lng]).addTo(mymap);

    var distance = 0.001;

    var circle = L.circle([lat - distance, lng - distance], {
        color: 'red',
        fillColor: '#f03',
        fillOpacity: 0.5,
        radius: 50
    }).addTo(mymap);
    
    var polygon = L.polygon([
        [lat + distance, lng + distance],
        [lat + distance, lng - distance],
        [lat - distance, lng - distance],
        [lat - distance, lng + distance]
    ]).addTo(mymap);
</script>
```

![폴리곤 추가](https://goo.gl/PXHPib)

## 팝업 추가

위에서 생성한 마커, 원, 폴리콘에 팝업을 추가합니다. 

```html
<script>
    var lat = 37.481;
    var lng = 126.893;
    var zoom = 20;
    var mymap = L.map('mapid', {
        center: [lat, lng],
        zoom: zoom
    });
    L.tileLayer('http://xdworld.vworld.kr:8080/2d/Base/201710/{z}/{x}/{y}.png').addTo(mymap);

    var marker = L.marker([lat, lng]).addTo(mymap);

    var distance = 0.001;

    var circle = L.circle([lat - distance, lng - distance], {
        color: 'red',
        fillColor: '#f03',
        fillOpacity: 0.5,
        radius: 50
    }).addTo(mymap);
    
    var polygon = L.polygon([
        [lat + distance, lng + distance],
        [lat + distance, lng - distance],
        [lat - distance, lng - distance],
        [lat - distance, lng + distance]
    ]).addTo(mymap);

    marker.bindPopup("<b>Hello world!</b><br>I am a popup.").openPopup();
    circle.bindPopup("I am a circle.");
    polygon.bindPopup("I am a polygon.");
</script>
```

![팝업 추가](https://goo.gl/K8HGTv)

마커, 원, 폴리곤을 클릭하면 각각 지정한 팝업에 표출됩니다.

```html
<script>
    // 이전 코드
    
    var popup = L.popup()
        .setLatLng([lat + distance, lng + distance])
        .setContent("I am a standalone popup.")
        .openOn(mymap);
</script>
```

객체에 연결하지 않고 팝업만 지도위에 추가할 수 도 있습니다.

![팝업 추가](https://goo.gl/2UomrQ)

## 이벤트 추가

이벤트를 등록합니다.

```html
<script>
    // 이전 코드

    function onMapClick(e) {
        alert("You clicked the map at " + e.latlng);
    }

    mymap.on('click', onMapClick);
</script>
```

클릭이벤트를 등록하고, 이벤트가 발생하면, 좌표정보를 보여줍니다.

![이벤트 추가](https://goo.gl/2bXAhe)

```html
<script>
    // 이전 코드

    // function onMapClick(e) {
    //     alert("You clicked the map at " + e.latlng);
    // }

    // mymap.on('click', onMapClick);

    var popup = L.popup();

    function onMapClick(e) {
        popup
            .setLatLng(e.latlng)
            .setContent("You clicked the map at " + e.latlng.toString())
            .openOn(mymap);
    }

    mymap.on('click', onMapClick);
</script>
```

alert창 대신에, 팝업에 좌표정보가 표출되도록 변경하였습니다.

![이벤트 추가](https://goo.gl/9jzVLL)


