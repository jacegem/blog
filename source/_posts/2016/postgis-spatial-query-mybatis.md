---
title: 마이바티스 쿼리 생성 - PostGIS 공간 쿼리
date: 2017-03-01
tags: [Mybatis, query, sql, postgis]
categories:
- Programming
- SQL
---

# 마이바티스 쿼리 생성 - PostGIS 공간 쿼리

거리순으로 대상을 조회합니다.

## 기본 조회 쿼리

```sql
SELECT * FROM my_table where my_column like '%#{text}%';
SELECT ST_SetSRID(ST_Point(CAST(#{lng} as double precision), CAST(#{lat} as double precision)), 4326);
```

## with 절을 사용

```sql
WITH center AS (
        SELECT ST_SetSRID(ST_Point(126.94130, 37.42187), 4326) as point
     )
SELECT point from center
```

## 거리를 비교

```sql
WITH center AS (
        SELECT ST_SetSRID(ST_Point(126.94130, 37.42187), 4326) as geom
     ), dist AS (
    SELECT ST_Distance(ST_Centroid(geom), (SELECT geom FROM center)) as dist, * FROM my_table where my_column like '%대림%'
     )
SELECT * FROM dist
```

## 정렬을 추가

```sql
WITH center AS (
        SELECT ST_SetSRID(ST_Point(126.94130, 37.42187), 4326) as geom
     ), dist AS (
    SELECT ST_Distance(ST_Centroid(geom), (SELECT geom FROM center)) as dist, * FROM my_table where my_column like '%대림%'
     ), dist_order AS (
    SELECT * FROM dist order by dist.dist asc
     )
SELECT * FROM dist_order
```

## 페이징 추가

```sql
WITH center AS (
        SELECT ST_SetSRID(ST_Point(126.94130, 37.42187), 4326) as geom
     ), dist AS (
    SELECT ST_Distance(ST_Centroid(geom), (SELECT geom FROM center)) as dist, * FROM my_table where my_column like '%대림%'
     ), dist_order AS (
    SELECT * FROM dist order by dist.dist asc limit  10 offset (3 - 1) * 10
     )
SELECT * FROM dist_order
```

## 마이바티스로 변경

변수 목록
* #{text}
* #{x}
* #{y}
* #{page}
* #{row_count}

```sql
<select id="listCnt" resultType="int">
    <bind name="pattern" value="'%' + text + '%'" />
    WITH center AS (
        SELECT ST_SetSRID(ST_Point(CAST(#{x} as double precision), CAST(#{y} as double precision)), 4326) as geom
    ), list AS (
        SELECT ST_Distance(ST_Centroid(geom), (SELECT geom FROM center)) as dist, * FROM my_table where my_column like #{pattern}
    ), list_order AS (
        SELECT * FROM dist order by list.dist asc limit #{row_count} offset (#{page} - 1) * #{row_count}
    )
    SELECT * FROM list_order
</select>
```


## 총 수를 구하는 쿼리

```xml
<select id="listCnt" resultType="int">
    <bind name="pattern" value="'%' + text + '%'" />
    SELECT count(*) as count FROM my_table where my_column like #{pattern}
</select>
```


## 에러

The column index is out of range: 1, number of columns: 0.

`like` 검색시에 `bind`를 사용합니다.

```xml
<select id="select" parameterType="java.util.Map" resultType="ViaDTO">

    <bind name="pattern" value="'%' + P_NOMBRE + '%'" />

    SELECT A.ID_VIAJE ID, A.NOMBRE, A.DESCRIPCION, A.FINICIO, A.FFIN, A.LOGO, A.URL,
            A.ID_CLIENTE IDCLIENTE, B.NOMBRE CLIENTE
        FROM VIAJE A
        INNER JOIN CLIENTE B ON (A.ID_CLIENTE = B.ID_CLIENTE)
        WHERE A.ESTATUS = 1

    <if test="P_NOMBRE != null">
        AND A.NOMBRE LIKE #{pattern}
    </if>
</select>
```
