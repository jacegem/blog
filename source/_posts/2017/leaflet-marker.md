---
title: '[leaflet] divIcon 마커 찍기'
date: 2017-02-20
tags: [leaflet, javascript, div, icon]
categories:
- Programming
- Javascript
---

# [leaflet] divIcon 마커 찍기

## 데이터 구조

```javascript
items = [
    {   
        buld_nm : 'A 아파트',
        st_x : '126.123',
        st_y : '37.123'
    }, {
        buld_nm : 'B 백화점',
        st_x : '123.123',
        st_y : '34.123'
    }
]
```

## 함수 호출

```javascript
// 마커 찍기
mymap.addSearchMarker(this.items);
```

## 마커 레이어 추가

```javascript
var cities = new L.LayerGroup();

L.marker([39.61, -105.02]).bindPopup('This is Littleton, CO.').addTo(cities),
L.marker([39.74, -104.99]).bindPopup('This is Denver, CO.').addTo(cities),
L.marker([39.73, -104.8]).bindPopup('This is Aurora, CO.').addTo(cities),
L.marker([39.77, -105.23]).bindPopup('This is Golden, CO.').addTo(cities);
```

## 레이어 관리 오브젝트

```javascript
var layerMap = {};
```

```javascript
var key = "searchMarker";
var layer = layerMap[key];
if (layer) map.removeLayer(layer);

layer = new L.LayerGroup().addTo(map);
layerMap[key] = layer;
```

## 데이터 삽입

for 문 사용
```javascript
for (var i =0; i < items.length; i++){
	item = items[i];
    L.marker([item.st_y, item.st_x]).addTo(layer);
}
```

마커가 안보이는 경우 x, y 의 순서를 바꿔서 입력해 봅니다.

## divIcon 사용

```javascript
var searchIcon = L.divIcon({
    //iconSize: new L.Point(50, 50),
	iconSize: null,
    html: '<span class="map-point">A</span>'
});

for (var i =0; i < items.length; i++){
	var item = items[i];
	L.marker([item.st_y, item.st_x], {icon: searchIcon}).addTo(layer).bindPopup(item.buld_nm);
}
```

## divIcon 반환 함수 생성

i 가 0 이면 `A`, 1이면 `B`를 반환하도록 함수 생성

```javascript
function getSearchIcon(i){
	// 0 : A
	var chr = String.fromCharCode(65 + i)

	var searchIcon = L.divIcon({  
		iconSize: null,
	    html: '<span class="map-point">' + chr + '</span>',
	    iconAnchor : [ 13, 32 ],
		popupAnchor : [ 0, -10 ]
	});

	return {
		icon: searchIcon
	}
}
```

호출 함수

```javascript
for (var i =0; i < items.length; i++){
	var item = items[i];
	L.marker([item.st_y, item.st_x], getSearchIcon(i)).bindTooltip(item.buld_nm, tooltipOption).addTo(layer);
}
```








## 출처
- http://leafletjs.com/reference-1.0.3.html#marker
- http://bl.ocks.org/ismyrnow/6123517
- https://gis.stackexchange.com/questions/114956/leaflet-how-do-you-use-removelayer
