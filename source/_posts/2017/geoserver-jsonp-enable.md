---
title: '[GeoServer] JSONP 활성화'
date: 2017-02-04 19:13:45
tags: [geoserver, jsonp]
categories:
- Application
- GIS
---

### [GeoServer] JSONP 활성화

`web.xml` 파일을 수정합니다.

 톰캣에 설치한 경우는 아래 경로에 있습니다.

 /tomcat/webapps/geoserver/WEB-INF/web.xml

```xml
<context-param>
    <param-name>ENABLE_JSONP</param-name>
    <param-value>true</param-value>
</context-param>
```

위의 내용을 입력한 후 서버를 재시작하여 jsonp 를 활성화 합니다.

#### 출처

- https://gis.stackexchange.com/questions/57494/geoserver-2-3-how-to-enable-jsonp
