---
title: PostGIS 공간쿼리
date: 2017-03-02
tags: [query, sql, postgis]
categories:
- Programming
- SQL
---

# PostGIS 공간쿼리

목표 : 전달된 좌표를 기반으로 가장 가까운 순으로 출력한다.

## Geometry 로 변경

좌표를 Geometry 로 변경

```sql
select ST_SetSRID(ST_Point(126.94130, 37.37736), 4326)
```

문자열을 변수로 받는 경우는 double precision 으로 변경

```sql
ST_SetSRID(ST_Point(CAST(#{lng} as double precision), CAST(#{lat} as double precision)), 4326)
```

## Geometry 에서 SRID 확인

```sql
st_srid(geom)
```

## SRID 변경

- https://postgis.net/docs/ST_Transform.html

```
geometry ST_Transform(geometry g1, integer srid);
geometry ST_Transform(geometry geom, text to_proj);
geometry ST_Transform(geometry geom, text from_proj, text to_proj);
geometry ST_Transform(geometry geom, text from_proj, integer to_srid);
```

## Transform and Update table

update  구문을 사용하니, 아래 처럼 에러메시지가 나옵니다.

```
ERROR: Geometry SRID (4326) does not match column SRID (97308)
```

`Alter table`로 컬럼 타입을 변경합니다.

```sql
ALTER TABLE my_table
    ALTER COLUMN geom TYPE geometry(MultiPolygon,4326) USING ST_Transform(geom,4326);
```

## 거리 구하기

```sql
ST_Distance(geom, ST_SetSRID(ST_Point(126.94130, 37.37736), 4326))
```

## 멀티폴리곤 중앙점 구하기

```sql
select ST_Centroid(geom) from my_table

-- 결과
"0101000020E6100000A2ED516D84BA5F4041F9855740B54240"
"0101000020E6100000E6824D190DB95F407DB540B5CAB34240"
"0101000020E61000007CB14A8739B95F40D1C60B02A0B34240"
```

```sql
select st_astext(ST_Centroid(geom)) from my_table

-- 결과
"POINT(126.914332704552 37.4160260585945)"
"POINT(126.891424489684 37.4046236577951)"
"POINT(126.894136259978 37.4033205564025)"
```

```sql
select st_x(ST_Centroid(geom)), st_y(ST_Centroid(geom)) from my_table

-- 결과
126.914332704552;37.4160260585945
126.891424489684;37.4046236577951
126.894136259978;37.4033205564025
```
