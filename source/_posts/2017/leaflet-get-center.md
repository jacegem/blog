---
title: Leaflet 화면 중앙 위치 값 얻기
date: 2017-02-19
tags: [leaflet, javascript]
categories:
- Programming
- Javascript
---

# Leaflet 화면 중앙 위치 값 얻기

```javascript
map.getCenter()
```

![](https://goo.gl/zwToLZ)

```javascript
center = map.getCenter();
center.x = center.lng;
center.y = center.lat;
```
