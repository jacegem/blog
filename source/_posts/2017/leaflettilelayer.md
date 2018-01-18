---
title: Leaflet.TileLayer
date: 2017-02-21
tags: [leaflet, javascript, tilelayer, gis]
categories:
- Programming
- Javascript
---

# Leaflet.TileLayer

출처 : http://leafletjs.com/reference.html#tilelayer

# TileLayer

맵상에서 타일 레이러를 불러오고 표시하는데 사용합니다. ILayer 인터페이스를 구현합니다.

## 사용 예

```javascript
L.tileLayer('http://{s}.tile.osm.org/{z}/{x}/{y}.png?{foo}', {foo: 'bar'}).addTo(map);
```

## 생성

| 생성 | 설명 |
| -- | -- |
| L.tileLayer( <String> urlTemplate, <TileLayer options> options? ) | 주어진 URL 템플릿로 타일 레이어 오브젝트와 옵션 오브젝트를 초기화합니다.  |
