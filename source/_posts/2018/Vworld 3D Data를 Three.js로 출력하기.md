---
title: "Vworld 3D Data를 Three.js로 출력하기"
date: 2018.10.17
tags: [vworld, 3d, data, threejs, javascript]
categories:
- Programming
- Javascript
---

# Vworld 3D Data를 Three.js로 출력하기

# Vworld WebGL기반 3차원 지도

[Vworld WebGL기반 3차원 지도](http://map.vworld.kr/map/wcmaps.do#) 에 접속하시면 아래와 같은 화면을 볼 수 있습니다.

![](https://lh3.googleusercontent.com/-XmBI4rDR5YI/W63qpRTd0QI/AAAAAAAAG40/ZgA5mYbYLiA899XwwFiG-nc0zD7xC5ofACHMYCw/s0/chrome_2018-09-28_45.jpg)

여기서 WebGL 이란

- 웹 기반의 그래픽 라이브러리 
- 자바스크립트 프로그래밍 언어를 통해서 사용
- 호환성이 있는 웹 브라우저에서 인터랙티브한 3D 그래픽을 사용할 수 있도록 제공

(출처: 위키피디아)




# [3D 데이터API 레퍼런스  소개](http://dev.vworld.kr/dev/v4dv_3ddataintro_s001.do)

데이터 API는 브이월드에서 제공하는 다양한 국가공간정보를 외부(기관,기업,개인등)에서 활용할 수 있도록 `XML또는 JSON형식으로 데이터를 제공하는 API서비스` 입니다. 



## 어떻게 사용해야 하나요?

![](https://lh3.googleusercontent.com/-ybCi5A2RMhw/W63nc3InV7I/AAAAAAAAG4o/sZHXwxCjyLgjIoP-VeAOjRpd1YvgLYYowCHMYCw/s0/StrokesPlus_2018-09-28_44.png)



## 어떤 데이터가 어떻게 제공 되나요?

![](https://lh3.googleusercontent.com/-cySQn8IweXA/W7FnAJst5II/AAAAAAAAG5Q/SlTmi_r7atomOOw5-e4W6pJNSXOz7snEQCHMYCw/s0/StrokesPlus_2018-10-01_46.png)



# [3D 데이터API 레퍼런스](http://dev.vworld.kr/dev/v4dv_3ddataintro_s001.do)

![](https://lh3.googleusercontent.com/-Njh-dbKYA3U/W7FnRDwOaTI/AAAAAAAAG5Y/3NPb5FinLfguauWLHtPbO7q84HteKZ4XQCHMYCw/s0/StrokesPlus_2018-10-01_47.png)



## 키 발급

`개발자센터 > 인증키 >  인증키 발급`에서 키를 발급 받을 수 있습니다. (`로그인` 되어 있어야 이용 가능합니다. )

![](https://lh3.googleusercontent.com/-f-jbniH3hRQ/W7FnsoV-viI/AAAAAAAAG5g/CDAfCeGR6d8O1I9X6V7tPDYd47Y20sJ2ACHMYCw/s0/StrokesPlus_2018-10-01_48.png)

```javascript
// 발급 받은 키
3XE3C10E-82B7-3E7F-AC31-37175F32CDE4


```

> 개인 키를 발급 받아서 사용하시기 바랍니다.



## [레이어 목록 요청 API](http://dev.vworld.kr/dev/v4dv_3ddataguide_s002.do)

![](https://lh3.googleusercontent.com/-MDUSKVnYJDA/W7Fn8yNZfQI/AAAAAAAAG5o/Np0ZpPRIpH41oMUZlU_qJ-KTBpxvtm5oACHMYCw/s0/StrokesPlus_2018-10-01_49.png)

### 출력 결과

![](https://lh3.googleusercontent.com/-wooW0QPFAzI/W7Fw--F4UKI/AAAAAAAAG6Q/SnjUMNc0fh8RRz88_gR5z0WnzE1T_UEnwCHMYCw/s0/StrokesPlus_2018-10-01_53.png)

### 각 요소별 정의

![](https://lh3.googleusercontent.com/-E05iuxMOY2A/W7FxIiL7_SI/AAAAAAAAG6U/5TRqms4yK2IzKcI7VwjJbZOvIlfsJOEnQCHMYCw/s0/StrokesPlus_2018-10-01_54.png)


### 데이터 요청

발급받은 키를 사용하여 요청합니다.

| URL     | http://xdworld.vworld.kr:8080/XDServer/3DData |
| ------- | --------------------------------------------- |
| Version | 2.0.0.0                                       |
| Request | `GetCapabilites`                              |
| Key     | 3XE3C10E-82B7-3E7F-AC31-37175F32CDE4          |

> // 요청 URL
>
> http://xdworld.vworld.kr:8080/XDServer/3DData?Version=2.0.0.0&Request=GetCapabilities&Key=3XE3C10E-82B7-3E7F-AC31-37175F32CDE4


### 요청 결과

```xml
<GetCapabilities>
<SCRIPT id="allow-copy_script">
(function agent() { let isUnlockingCached = false const isUnlocking = () => isUnlockingCached document.addEventListener('allow_copy', event => { const { unlock } = event.detail isUnlockingCached = unlock }) const copyEvents = [ 'copy', 'cut', 'contextmenu', 'selectstart', 'mousedown', 'mouseup', 'mousemove', 'keydown', 'keypress', 'keyup', ] const rejectOtherHandlers = e => { if (isUnlocking()) { e.stopPropagation() if (e.stopImmediatePropagation) e.stopImmediatePropagation() } } copyEvents.forEach(evt => { document.documentElement.addEventListener(evt, rejectOtherHandlers, { capture: true, }) }) })()
</SCRIPT>
<Version>2.0.0.0</Version>
<Layers Count="77">
<Layer Name="tile" Type="terrainImg" MinLevel="0" MaxLevel="15"/>
<Layer Name="dem" Type="terrainDEM" MinLevel="0" MaxLevel="15"/>
<Layer Name="2015" Type="time" MinLevel="0" MaxLevel="15"/>
<Layer Name="2016" Type="time" MinLevel="0" MaxLevel="15"/>
<Layer Name="2017" Type="time" MinLevel="0" MaxLevel="15"/>
<Layer Name="2018" Type="time" MinLevel="0" MaxLevel="16"/>
<Layer Name="tile_mo" Type="terrainImg_MO" MinLevel="0" MaxLevel="18"/>
<Layer Name="tile_mo_2015" Type="terrainImg_2015" MinLevel="0" MaxLevel="18"/>
<Layer Name="tile_mo_HD" Type="terrainImg_MO_HD" MinLevel="0" MaxLevel="18"/>
<Layer Name="tile_mo2d" Type="terrainImg_MO2D" MinLevel="0" MaxLevel="15"/>
<Layer Name="tile_mo2d_HD" Type="terrainImg_MO2DHD" MinLevel="0" MaxLevel="15"/>
<Layer Name="dem_mo" Type="terrainDEM_MO" MinLevel="0" MaxLevel="15"/>
<Layer Name="indoor_build" Type="Indoor" MinLevel="0" MaxLevel="15"/>
<Layer Name="indoor_build_HD" Type="Indoor" MinLevel="0" MaxLevel="15"/>
<Layer Name="facility_build_boan" Type="boan1" MinLevel="0" MaxLevel="15"/>
<Layer Name="facility_bridge_boan" Type="boan1" MinLevel="0" MaxLevel="15"/>
<Layer Name="facility_build_lod1" Type="boan1" MinLevel="0" MaxLevel="15"/>
<Layer Name="hybrid_2017" Type="boan2" MinLevel="5" MaxLevel="15"/>
<Layer Name="facility_build" Type="Real3D" MinLevel="0" MaxLevel="15"/>
<Layer Name="facility_build_world" Type="Real3D" MinLevel="0" MaxLevel="15"/>
<Layer Name="facility_build_world_mo" Type="Real3D" MinLevel="0" MaxLevel="15"/>
<Layer Name="facility_bridge" Type="Real3D" MinLevel="0" MaxLevel="14"/>
<Layer Name="facility_dokdo" Type="Real3D" MinLevel="0" MaxLevel="13"/>
<Layer Name="facility_build_at" Type="Real3D" MinLevel="0" MaxLevel="15"/>
<Layer Name="facility_build_mo" Type="Real3D" MinLevel="0" MaxLevel="15"/>
<Layer Name="facility_dokdo_mo" Type="Real3D" MinLevel="0" MaxLevel="13"/>
<Layer Name="facility_bridge_mo" Type="Real3D" MinLevel="0" MaxLevel="14"/>
<Layer Name="facility_bridge_ag" Type="Real3D" MinLevel="0" MaxLevel="14"/>
<Layer Name="facility_build_ag" Type="Real3D" MinLevel="0" MaxLevel="15"/>
<Layer Name="facility_bridge_ag_mo" Type="Real3D" MinLevel="0" MaxLevel="14"/>
<Layer Name="facility_build_ag_mo" Type="Real3D" MinLevel="0" MaxLevel="15"/>
<Layer Name="facility_indoor" Type="Real3D" MinLevel="0" MaxLevel="15"/>
<Layer Name="facility_olympic2018" Type="Real3D" MinLevel="0" MaxLevel="15"/>
<Layer Name="tile_bridge_mo" Type="terrainImg_wgt" MinLevel="10" MaxLevel="14"/>
<Layer Name="tile_build_mo" Type="terrainImg_wgt" MinLevel="10" MaxLevel="15"/>
<Layer Name="poi_base" Type="Point3D" MinLevel="0" MaxLevel="16"/>
<Layer Name="poi_base_u8" Type="Point3D" MinLevel="0" MaxLevel="16"/>
<Layer Name="poi_base_north" Type="Point3D" MinLevel="0" MaxLevel="14"/>
<Layer Name="poi_base_world" Type="Point3D" MinLevel="0" MaxLevel="14"/>
<Layer Name="poi_bound" Type="Point3D" MinLevel="0" MaxLevel="11"/>
<Layer Name="poi_bound_north" Type="Point3D" MinLevel="0" MaxLevel="8"/>
<Layer Name="poi_bound_world" Type="Point3D" MinLevel="0" MaxLevel="7"/>
<Layer Name="poi_road" Type="Point3D" MinLevel="0" MaxLevel="14"/>
<Layer Name="poi_road_north" Type="Point3D" MinLevel="0" MaxLevel="14"/>
<Layer Name="poi_em_base" Type="Point3D" MinLevel="0" MaxLevel="16"/>
<Layer Name="poi_em_bound" Type="Point3D" MinLevel="0" MaxLevel="12"/>
<Layer Name="poi_em_dept" Type="Point3D" MinLevel="0" MaxLevel="7"/>
<Layer Name="poi_em_dokdo" Type="Point3D" MinLevel="0" MaxLevel="15"/>
<Layer Name="poi_em_embassy" Type="Point3D" MinLevel="0" MaxLevel="6"/>
<Layer Name="poi_em_issue" Type="Point3D" MinLevel="0" MaxLevel="4"/>
<Layer Name="poi_em_korea" Type="Point3D" MinLevel="0" MaxLevel="4"/>
<Layer Name="poi_em_park" Type="Point3D" MinLevel="0" MaxLevel="4"/>
<Layer Name="poi_em_whc" Type="Point3D" MinLevel="0" MaxLevel="4"/>
<Layer Name="poi_base_north_u8" Type="Point3D" MinLevel="0" MaxLevel="14"/>
<Layer Name="poi_base_world_u8" Type="Point3D" MinLevel="0" MaxLevel="14"/>
<Layer Name="poi_bound_u8" Type="Point3D" MinLevel="0" MaxLevel="11"/>
<Layer Name="poi_bound_north_u8" Type="Point3D" MinLevel="0" MaxLevel="8"/>
<Layer Name="poi_bound_world_u8" Type="Point3D" MinLevel="0" MaxLevel="7"/>
<Layer Name="poi_road_u8" Type="Point3D" MinLevel="0" MaxLevel="14"/>
<Layer Name="poi_road_north_u8" Type="Point3D" MinLevel="0" MaxLevel="14"/>
<Layer Name="poi_indoor" Type="Point3D" MinLevel="0" MaxLevel="14"/>
<Layer Name="hybrid_road" Type="HybridImg" MinLevel="5" MaxLevel="16"/>
<Layer Name="hybrid_bound" Type="HybridImg" MinLevel="0" MaxLevel="14"/>
<Layer Name="hybrid_add_sansatai" Type="HybridImg" MinLevel="5" MaxLevel="14"/>
<Layer Name="hybrid_road_mo" Type="HybridImg" MinLevel="5" MaxLevel="16"/>
<Layer Name="hybrid_silgam" Type="HybridImg" MinLevel="5" MaxLevel="15"/>
<Layer Name="hybrid_2016" Type="HybridImg" MinLevel="5" MaxLevel="15"/>
<Layer Name="bill_Tree" Type="Billboard3D" MinLevel="0" MaxLevel="16"/>
<Layer Name="timeseries" Type="TimeseriesImg" MinLevel="0" MaxLevel="16"/>
<Layer Name="T197801" Type="TimeseriesImg" MinLevel="0" MaxLevel="16"/>
<Layer Name="T198901" Type="TimeseriesImg" MinLevel="0" MaxLevel="16"/>
<Layer Name="T195001" Type="TimeseriesImg" MinLevel="0" MaxLevel="16"/>
<Layer Name="T195101" Type="TimeseriesImg" MinLevel="0" MaxLevel="16"/>
<Layer Name="T195212" Type="TimeseriesImg" MinLevel="0" MaxLevel="16"/>
<Layer Name="T199601" Type="TimeseriesImg" MinLevel="0" MaxLevel="16"/>
<Layer Name="T200601" Type="TimeseriesImg" MinLevel="0" MaxLevel="16"/>
<Layer Name="T200712" Type="TimeseriesImg" MinLevel="0" MaxLevel="14"/>
</Layers>
</GetCapabilities>
```



이 중에서 `facility_build` 를 사용하겠습니다.


```xml
<Layer Name="facility_build" Type="Real3D" MinLevel="0" MaxLevel="15"/>
```


## [데이터 검색 요청 API(GetLayerExists)](http://dev.vworld.kr/dev/v4dv_3ddataguide_s002.do)

![](https://lh3.googleusercontent.com/-gcqUwonAZhw/W7FyP7QImLI/AAAAAAAAG6k/GslG2NVFVS8_LrIIptjl3-BqLJbwr7b5ACHMYCw/s0/StrokesPlus_2018-10-01_56.png)

### 요청 변수

![](https://lh3.googleusercontent.com/-kDt3PHuS8oA/W7FyU4Z48nI/AAAAAAAAG6o/U1tLjwJ2I801Teyp184OzoIJY-6Xo6GxgCHMYCw/s0/StrokesPlus_2018-10-01_57.png)

### 출력결과

![](https://lh3.googleusercontent.com/-O6jJdTpdiy4/W7Fydj0bhfI/AAAAAAAAG6w/rYAqSScpUG0jycdYLSPsOFZeLlsOVeUbwCHMYCw/s0/StrokesPlus_2018-10-01_58.png)

### 각 요소별 정의

![](https://lh3.googleusercontent.com/-jaxdC62fuj8/W7FyhNdlgcI/AAAAAAAAG64/8X6E6lmvz7EPVcGqa902E8xGg0_sCX1uQCHMYCw/s0/StrokesPlus_2018-10-01_59.png)



### 데이터 요청

발급받은 키를 사용하여 요청합니다.

| URL     | http://xdworld.vworld.kr:8080/XDServer/3DData |
| ------- | --------------------------------------------- |
| Version | 2.0.0.0                                       |
| Request | `GetLayerExists`                              |
| Key     | 3XE3C10E-82B7-3E7F-AC31-37175F32CDE4          |

추가 되는 내용들

| Layer     | `facility_build`                            |
| --------- | ------------------------------------------- |
| Level     | `15`                                        |
| BBOX      | `126.976987,37.570187,126.979433,37.574251` |
| CheckFlag | `True`                                      |

> http://xdworld.vworld.kr:8080/XDServer/3DData?Version=2.0.0.0&Request=GetLayerExists&Key=3XE3C10E-82B7-3E7F-AC31-37175F32CDE4&Layer=facility_build&Level=15&BBOX=126.976987,37.570187,126.979433,37.574251&CheckFlag=True



### 요청 결과

```xml
<SearchResult>
<Version>2.0.0.0</Version>
<Layer Name="facility_build">
<Nodes Level="15" NodeCount="18">
<Node IDX="279417" IDY="116117" Flag="false"/>
<Node IDX="279418" IDY="116117" Flag="false"/>
<Node IDX="279419" IDY="116117" Flag="false"/>
<Node IDX="279420" IDY="116117" Flag="false"/>
<Node IDX="279417" IDY="116118" Flag="false"/>
<Node IDX="279418" IDY="116118" Flag="false"/>
<Node IDX="279419" IDY="116118" Flag="false"/>
<Node IDX="279420" IDY="116118" Flag="false"/>
<Node IDX="279417" IDY="116119" Flag="false"/>
<Node IDX="279418" IDY="116119" Flag="false"/>
<Node IDX="279419" IDY="116119" Flag="false"/>
<Node IDX="279420" IDY="116119" Flag="false"/>
<Node IDX="279417" IDY="116120" Flag="false"/>
<Node IDX="279418" IDY="116120" Flag="false"/>
<Node IDX="279419" IDY="116120" Flag="false"/>
<Node IDX="279420" IDY="116120" Flag="false"/>
<Node IDX="279417" IDY="116121" Flag="false"/>
<Node IDX="279418" IDY="116121" Flag="false"/>
<Node IDX="279419" IDY="116121" Flag="false"/>
<Node IDX="279420" IDY="116121" Flag="false"/>
</Nodes>
</Layer>
</SearchResult>
```

이 중에서 가장 마지막에 있는 것을 이용해보도록 하겠습니다. 

```xml
<Node IDX="279420" IDY="116121" Flag="false"/>
```





## [레이어 노드 및 객체 요청 API(GetLayer)](http://dev.vworld.kr/dev/v4dv_3ddataguide_s002.do)

### 요청 URL

![](https://lh3.googleusercontent.com/-ckP_PsyMmas/W7GIhETirxI/AAAAAAAAG7E/fsiFpDNgBhEF81UidsCQuSnHLURAl4yigCHMYCw/s0/StrokesPlus_2018-10-01_60.png)

### 요청 변수

![](https://lh3.googleusercontent.com/-BqddgDvn7VE/W7GI4lqk4UI/AAAAAAAAG7M/jjJKBh2LBFUh3kjwZesC0R8GNDdeHmX2QCHMYCw/s0/StrokesPlus_2018-10-01_61.png)



### 데이터 요청

| URL     | http://xdworld.vworld.kr:8080/XDServer/3DData |
| ------- | --------------------------------------------- |
| Version | 2.0.0.0                                       |
| Request | `GetLayer`                                    |
| Key     | 3XE3C10E-82B7-3E7F-AC31-37175F32CDE4          |

추가 되는 내용들

| Layer | `facility_build` |
| ----- | ---------------- |
| Level | 15               |
| IDX   | `279420`         |
| IDY   | `116121`         |

>  http://xdworld.vworld.kr:8080/XDServer/3DData?Version=2.0.0.0&Request=GetLayer&Key=3XE3C10E-82B7-3E7F-AC31-37175F32CDE4&Layer=facility_build&Level=15&IDX=279420&IDY=116121

위의 요청을 보내면 [3DData](https://drive.google.com/a/knou.ac.kr/file/d/1HA8XiPB_Gtli7u_PV30zN4JOfFMGhdcS/view?usp=drivesdk) 바이너리 파일이 리턴됩니다.



### 요청 결과

[VWorld 3D Map Binary Data File Format](http://www.vworld.kr/notice/VWorld_Data_Format_D.pdf)을 확인하여 리턴된 바이너리 파일을 분석합니다. 

![](https://lh3.googleusercontent.com/-4HYABenr8jw/W7GgCOmEThI/AAAAAAAAG7c/Trq0Tbv6NZo_wd0j0DxVWxhFC4LUnfPdACHMYCw/s0/2018-10-01_62.png)

#### Header 정보

| Name         | Size(byte) | Type | 설명         |
| ------------ | ---------- | ---- | ---------- |
| Level        | 4          | u32  | 레벨 (0 베이스) |
| IDX          | 4          | u32  | 경도 ID      |
| IDY          | 4          | u32  | 위도 ID      |
| Object Count | 4          | u32  | 객체의 총수     |



#### Model Object 정보

| Name           | Size(byte)     | Type      | 설명                                                    |
| -------------- | -------------- | --------- | ----------------------------------------------------- |
| Version        | 4              | u32       | 객체의 등록 버전 (XDO 파일의 버전정보)                              |
| Type           | 1              | u8        | 객체의 타입 (객체의 타입정보 - 8로 고정: 예약됨)                        |
| KeyLen         | 1              | u8        | 객체의 키값 크기                                             |
| Key            | KeyLen         | char*     | 객체의 키값 (객체의 고유 ID 값)                                  |
| CenterPos      | 16             | vector2dd | 객체의 중심좌표 (경위도 radian) (Model이 위치 할 공간좌표 경위도값)         |
| altitude       | 4              | f32       | 객체의 고도값 (Model이 위치 할 공간의 해발고도 높이)                     |
| box            | 48             | aabbox3dd | 객체의 바운더리 (구면 Vector) (Model 의 3차원 최대, 최소 공간 영역 좌표 정보) |
| ImgLevel       | 1              | u8        | 이미지 레벨 (사용되는 LOD 레벨의 수)                               |
| dataFileLen    | 1              | u8        | 객체의 파일이름 길이                                           |
| dataFile       | dataFileLen    | char*     | 객체의 파일이름 (확장자 포함) (Tile 내에 실제 Model의 XDO 파일명)         |
| imgFileNameLen | 1              | u8        | 이미지 파일이름 길이                                           |
| imgFileName    | imgFileNameLen | char*     | 이미지 파일이름 (XDO 파일이 사용하는 Texture 이미지 명)                 |



#### Parsing

```javascript
var idx = 279420
var idy = 116121
var layerUrl = `http://xdworld.vworld.kr:8080/XDServer/3DData?Version=2.0.0.0&Request=GetLayer&Key=3XE3C10E-82B7-3E7F-AC31-37175F32CDE4&Layer=facility_build&Level=15&IDX=${idx}&IDY=${idy}`;

var promise = $.ajax({
    url: layerUrl,
    dataType: "binary",
});

promise.done(function (data) {
    var p = new Parser(data);
    
    // header
    _data.level = p.getUint4();
    _data.idx = p.getUint4();
    _data.idy = p.getUint4();
    _data.objectCount = p.getUint4();

    // model object
    _data.version = p.getVersion();
    _data.type = p.getUint1();
    var lenStr = p.getLenStr();
    _data.keyLen = lenStr.len;
    _data.key = lenStr.str;

    _data.centerPosX = p.getFloat8();
    _data.centerPosY = p.getFloat8();
    _data.altitude = p.getFloat4();
    _data.box = p.getBox3dd();

    _data.imgLevel = p.getUint1();
    var lenStr = p.getLenStr();
    _data.dataFileLen = lenStr.len;
    _data.dataFileName = lenStr.str;

    var lenStr = p.getLenStr();
    _data.imgFileLen = lenStr.len;
    _data.imgFileName = lenStr.str;

    resolve();
});
```

이렇게 하면 아래와 같은 정보를 얻을 수 있습니다.

![크롬 개발자 도구 캡쳐 화면](https://lh3.googleusercontent.com/-z5g_pDXKnVc/W7GoTCCCBBI/AAAAAAAAG7o/evljzZGpHWsfGntyNi8kBq9nHJpvK-LowCHMYCw/s0/StrokesPlus_2018-10-01_63.png)

여기서 얻은 파일명을 사용하여 3D data 를 조회합니다. 

- dataFileName : `a13_52.xdo`
- imgFileName: `a13_52.jpg`



### Parser Class

```javascript
$(document).ready(function () {
    $.ajaxSetup({
        beforeSend: function (jqXHR, settings) {
            if (settings.dataType === 'binary') {
                settings.xhr().responseType = 'blob';
            }
        }
    });

    $.ajaxTransport("+binary", function (options, originalOptions, jqXHR) {
        if (window.FormData && ((options.dataType && (options.dataType == 'binary'))
                || (options.data && ((window.ArrayBuffer && options.data instanceof ArrayBuffer)
                    || (window.Blob && options.data instanceof Blob))))) {
            return {
                send: function (headers, callback) {
                    var xhr = new XMLHttpRequest(),
                        url = options.url,
                        type = options.type,
                        async = options.async || true,                        
                        dataType = options.responseType || "arraybuffer",
                        data = options.data || null;

                    xhr.addEventListener('load', function () {
                        var data = {};
                        data[options.dataType] = xhr.response;
                        callback(xhr.status, xhr.statusText, data, xhr.getAllResponseHeaders());
                    });

                    xhr.open(type, url, async);

                    for (var i in headers) {
                        xhr.setRequestHeader(i, headers[i]);
                    }

                    xhr.responseType = dataType;
                    xhr.send(data);
                },
                abort: function () {
                }
            };
        }
    });
});

class Parser {
    constructor(data) {
        this.dv = new DataView(data);
        this.endian = true;
        this.offset = 0;
    }

    getUint4() {
        const val = this.dv.getUint32(this.offset, this.endian);
        this.offset += 4;
        return val;
    }

    getUint1() {
        const val = this.dv.getUint8(this.offset, this.endian);
        this.offset += 1;
        return val;
    }

    getUint2() {
        const val = this.dv.getUint16(this.offset, this.endian);
        this.offset += 2;
        return val;
    }

    getLenStr() {
        const len = this.getUint1();

        var str = "";
        var val = "";
        for (var i = 0; i < len; i++) {
            val = this.getUint1();
            str += String.fromCharCode(val);
        }
        return {
            len: len,
            str: str,
        };
    }

    getFloat8() {
        const val = this.dv.getFloat64(this.offset, this.endian);
        this.offset += 8;
        return val;
    }

    getFloat4() {
        const val = this.dv.getFloat32(this.offset, this.endian);
        this.offset += 4;
        return val;
    }

    getVersion() {
        const val = `${this.getUint1()}.${this.getUint1()}.${this.getUint1()}.${this.getUint1()}`;
        return val;
    }

    getBox() {
        var minX = this.getFloat8();
        var maxX = this.getFloat8();
        var minY = this.getFloat8();
        var maxY = this.getFloat8();
        var minZ = this.getFloat8();
        var maxZ = this.getFloat8();
        return {
            minX: minX,
            maxX: maxX,
            minY: minY,
            maxY: maxY,
            minZ: minZ,
            maxZ: maxZ
        }
    }

    getVector2df() {
        var x = this.getFloat4();
        var y = this.getFloat4();
        return {
            x: x,
            y: y
        }
    }

    getVector3df() {
        var x = this.getFloat4();
        var y = this.getFloat4();
        var z = this.getFloat4();
        return {
            x: x,
            y: y,
            z: z
        }
    }

    getVector3dd() {
        var x = this.getFloat8();
        var y = this.getFloat8();
        var z = this.getFloat8();
        return {
            x: x,
            y: y,
            z: z
        }
    }

    //http://irrlicht.sourceforge.net/docu/structirr_1_1video_1_1_s3_d_vertex.html
    getCountVert() {
        const count = this.getUint4();
        var vert = [];
        for (var i = 0; i < count; i++) {
            const pos = this.getVector3df();
            const normal = this.getVector3df();
            const uv = this.getVector2df();

            vert.push({
                pos: pos,
                normal: normal,
                uv: uv
            })
        }
        return {
            count: count,
            vert: vert
        };
    }

    getCountIndex() {
        const count = this.getUint4();
        var index = [];
        for (var i = 0; i < count; i++) {
            const val = this.getUint2();
            index.push(val)
        }
        return {
            count: count,
            index: index
        };
    }


    getBox3dd() {
        const min = this.getVector3dd();
        const max = this.getVector3dd();
        return {
            min: min,
            max: max
        }
    }
}
```







### 데이터 파일 조회

위에서 얻은 데이터 파일명을 사용하여 조회합니다.

| URL      | http://xdworld.vworld.kr:8080/XDServer/3DData |
| -------- | --------------------------------------------- |
| Version  | 2.0.0.0                                       |
| Request  | GetLayer                                      |
| Key      | 3XE3C10E-82B7-3E7F-AC31-37175F32CDE4          |
| Layer    | facility\_build                               |
| Level    | 15                                            |
| IDX      | 279420                                        |
| IDY      | 116121                                        |
| DataFile | a13_52.xdo                                    |

---


요청 URL

http://xdworld.vworld.kr:8080/XDServer/3DData?Version=2.0.0.0&Request=GetLayer&Key=3XE3C10E-82B7-3E7F-AC31-37175F32CDE4&Layer=facility_build&Level=15&IDX=279420&IDY=116121&DataFile=a13_112.xdo

[VWorld 3D Map Binary Data File Format](http://www.vworld.kr/notice/VWorld_Data_Format_D.pdf)을 확인하여 리턴된 바이너리 파일을 분석합니다. 

**Real3D Model Data** **(\*.xdo)** **Format 구성**

![](https://lh3.googleusercontent.com/-29DO330ZM0k/W7GsGucWJ6I/AAAAAAAAG70/apI8NfA5RB0OC3LZmIne07IB4gstz1mMgCHMYCw/s0/2018-10-01_64.png)

####  Object Attribute

| Name       | Size(byte) | Type      | 설명                                                             |
| ---------- | ---------- | --------- | -------------------------------------------------------------- |
| Type       | 1          | u8        | 객체의 타입 (객체의 형식 Real3D Model 은 8 고정으로 사용)                       |
| Object ID  | 4          | u32       | 객체의 고유번호 (객체의 고유 ID 식별번호, 레이어 기준으로 객체 등록 번호)                   |
| KeyLen     | 1          | u8        | 객체의 키값 길이                                                      |
| Key        | KeyLen     | char*     | 객체의 키값 (객체의 고유 식별 문자열)                                         |
| Object Box | 48         | aabbox3dd | 객체의 범위 (구면 Vector) (객체의 최대 최소 영역 범위, World Wind 구면 Vector 좌표계) |
| Altitude   | 4          | f32       | 객체의 높이 (객체의 바닥면 기준 해발고도 높이)                                    |

![](https://lh3.googleusercontent.com/-tEvzVfDH_LM/W7GtMh5QgPI/AAAAAAAAG78/eHF255-ttJg7T1K1v9n_hSSIEH5AYpLmACHMYCw/s0/StrokesPlus_2018-10-01_65.png)

#### 3D Mesh Data 

| Name          | Size (byte)   | Type       | 설명                                      |
| ------------- | ------------- | ---------- | --------------------------------------- |
| Vertex Count  | 4             | u32        | 버텍스 총수                                  |
| Vertex        | Vertex Count  | S3DVertex* | 버텍스 값 (형상을 구성하는 정규화된 공간 좌표점)            |
| Indexed Count | 4             | u32        | 인덱스 총수                                  |
| Indexed       | Indexed Count | u16*       | 인덱스 값 (Vertex 정점을 삼각형 그릴 시에 그리는 순서 리스트) |
| Color         | 4             | u32        | 객체색상 (Mesh의 재질의 색상(A8R8G8B8))           |
| ImageLevel    | 1             | byte       | 이미지 LOD 총 레벨 (LOD 텍스처의 레벨 단계)           |
| ImageNameLen  | 1             | byte       | 텍스쳐 이미지명 길이                             |
| imageName     | ImageNameLen  | char*      | 텍스처 이미지명(확장자 제외, jpg) (LOD 텍스처 파일명)     |

![](https://lh3.googleusercontent.com/-iVzjXGFo8F0/W7GuHNcKf7I/AAAAAAAAG8I/3lnHauHvuRYkZoCd-g4oKUXqnsDO0H1ZwCHMYCw/s0/StrokesPlus_2018-10-01_67.png)

index를 확인합니다. 

![](https://lh3.googleusercontent.com/-Ke4ikMlsRCc/W7GuRCxMYLI/AAAAAAAAG8Q/OoYKsesnvJUwS-TFBj52GE7hJ9xmz0LWQCHMYCw/s0/StrokesPlus_2018-10-01_68.png)



vertex 를 확인합니다.

![](https://lh3.googleusercontent.com/--2mb0fayDvY/W7GudoSoh0I/AAAAAAAAG8Y/bHAE1Ztq-p4a48JbzuSV_tIHcuPK7Bh9gCHMYCw/s0/StrokesPlus_2018-10-01_70.png)

### [V-World 2013 Data Format Schema](http://www.vworld.kr/notice/VWorld_Data_Format.pdf)

**S3DVertex**

| Name   | Size(byte) | Type      | 설명     |
| ------ | ---------- | --------- | ------ |
| Pos    | 12         | vector3df | 버텍스 좌표 |
| Normal | 12         | vector3df | 버텍스 노말 |
| UV     | 8          | vector2df | 텍스처 UV |

**aabbox3d**

| Name | Size(byte) | Type      | 설명     |
| ---- | ---------- | --------- | ------ |
| Min  | 24         | vector3dd | 최소 좌표점 |
| Max  | 24         | vector3dd | 최대 좌표점 |

**vector2df**

| Name | Size(byte) | Type  | 설명   |
| ---- | ---------- | ----- | ---- |
| x    | 4          | float | x축 값 |
| y    | 4          | float | y축 값 |

**vector2dd**

| Name | Size(byte) | Type   | 설명   |
| ---- | ---------- | ------ | ---- |
| x    | 8          | double | x축 값 |
| y    | 8          | double | y축 값 |

**vector3df**

| Name | Size(byte) | Type  | 설명   |
| ---- | ---------- | ----- | ---- |
| x    | 4          | float | x축 값 |
| y    | 4          | float | y축 값 |
| z    | 4          | float | z축 값 |

**vector3dd**

| Name | Size(byte) | Type   | 설명   |
| ---- | ---------- | ------ | ---- |
| x    | 8          | double | x축 값 |
| y    | 8          | double | y축 값 |
| z    | 8          | double | z축 값 |



### 이미지 파일 조회

위에서 얻은 이미지 파일명을 사용하여 조회합니다.

| URL      | http://xdworld.vworld.kr:8080/XDServer/3DData |
| -------- | --------------------------------------------- |
| Version  | 2.0.0.0                                       |
| Request  | GetLayer                                      |
| Key      | 3XE3C10E-82B7-3E7F-AC31-37175F32CDE4          |
| Layer    | facility\_build                               |
| Level    | 15                                            |
| IDX      | 279420                                        |
| IDY      | 116121                                        |
| DataFile | `a13\_52.jpg`                                 |

> http://xdworld.vworld.kr:8080/XDServer/3DData?Version=2.0.0.0&Request=GetLayer&Key=3XE3C10E-82B7-3E7F-AC31-37175F32CDE4&Layer=facility_build&Level=15&IDX=279420&IDY=116121&DataFile=a13_52.jpg

다운로드 받은 파일의 확장자를 `jpg`로 변경하여 이미지를 확인할 수 있습니다. 

![](https://lh3.googleusercontent.com/-0cecQkRHoG8/W7Gv2tv0vHI/AAAAAAAAG8o/OTySg4q9OHo4pVrbcPNTU6A79yaA3sBVgCHMYCw/s0/3DData%2B%25283%2529.jpg)

여기까지 해서 오브젝트 생성을 위한 데이터를 모두 얻었습니다.



## 오브젝트 생성 (Three.js 사용)

기본 구조는 이렇습니다. 

1. Geometry  생성
2. 버텍스 추가
3. 버텍스 인덱스 추가
4. UV 추가
5. 이미지 파일 추가
6. 메테리얼 생성
7. 메시 생성

### 샘플 코드

```javascript
var vert = data.vert;
var index = data.index;

// 1. Geometry  생성
var geometry = new THREE.Geometry();

for (var i = 0; i < vert.length; i += 3) {
    var v1 = vert[i];
    var v2 = vert[i + 1];
    var v3 = vert[i + 2];
	
    // 2. 버텍스 추가
    geometry.vertices.push(new THREE.Vector3(v1.pos.x, v1.pos.y, v1.pos.z));
    geometry.vertices.push(new THREE.Vector3(v2.pos.x, v2.pos.y, v2.pos.z));
    geometry.vertices.push(new THREE.Vector3(v3.pos.x, v3.pos.y, v3.pos.z));

    // 3. 버텍스 인덱스 추가
    geometry.faces.push(new THREE.Face3(index[i], index[i + 1], index[i + 2]));
	
    // 4. UV 추가
    geometry.faceVertexUvs[0].push([
        new THREE.Vector2(v1.uv.x, 1 - v1.uv.y),
        new THREE.Vector2(v3.uv.x, 1 - v3.uv.y),
        new THREE.Vector2(v2.uv.x, 1 - v2.uv.y)
    ]);
}

geometry.uvsNeedUpdate = true;

// 5. 이미지 파일 추가
const textureLoader = new THREE.TextureLoader();
textureLoader.crossOrigin = "Anonymous";
const myTexture = textureLoader.load(jpgUrl);

// 6. 메테리얼 생성
var material = new THREE.MeshBasicMaterial({
    side: THREE.DoubleSide,
    map: myTexture
});

// 7. 메시 생성
var build = new THREE.Mesh(geometry, material);
```

> 코드상에 잘못된 부분이 있으면 말씀해 주세요.

여기까지가 메시를 생성하는 부분이고, 이후에는 `Three.js Scene`에 추가합니다.

1. Scene 생성
2. 카메라 생성
3. 렌더러 생성
4. 컨테이너에 추가
5. 메쉬 추가

```html
<html>
  <div id="ThreeContainer"></div>
  <script>
      function initThree() {
        var fov = 45;
        var width = window.innerWidth;
        var height = window.innerHeight;
        var aspect = width / height;
        var near = 1;
        var far = 10 * 1000 * 1000;

        // 1. Scene 생성
        _three.scene = new THREE.Scene();
        // 2. 카메라 생성
        _three.camera = new THREE.PerspectiveCamera(fov, aspect, near, far);
        // 3. 렌더러 생성
        _three.renderer = new THREE.WebGLRenderer({alpha: true});
        // 4. 컨테이너에 추가
        _ThreeContainer.appendChild(_three.renderer.domElement);
      }
      
      function init3DObject(){
          // ... 위의 코드 ...  
          
          var buildGroup = new THREE.Group();
          buildGroup.add(build);
          // 5. 메쉬 추가
          _three.scene.add(buildGroup); // don’t forget to add it to the Three.js scene manually
      }
      
      initThree(); // Initialize Three.js renderer
      init3DObject(); // Initialize Three.js object mesh with Cesium Cartesian coordinate system
  </script>
</html>

```



이렇게 하면 Vworld 로 부터 받은 3D data 를 Three.js 로 그릴 수 있습니다. 

![](https://lh3.googleusercontent.com/-gHP0eQsRf3I/W7GzdYX4_PI/AAAAAAAAG80/hrM8sb9VcO8mgEzAww15Nuc-o2oG08tUwCHMYCw/s0/StrokesPlus_2018-10-01_71.png)




