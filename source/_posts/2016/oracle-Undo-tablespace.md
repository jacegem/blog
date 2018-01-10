title: '[oracle] Undo tablespace'
tags: [ oracle, undo, tablespace ]
date: 2016-01-07 06:13:36

---
[오라클] Undo tablespace

Undo tablespace는 사용자가 rollback 을 하는 경우에 사용하기 위한 데이터를 저장하는 곳입니다.

### Undo 파라미터 확인

	show parameter undo;
![실행결과](https://goo.gl/6w0Yf7)

### 새로운 undo tablespace 생성
	CREATE UNDO TABLESPACE UNDOTBS2 
	DATAFILE '/home/oradata/undotbs2.dbf' SIZE 10M
    AUTOEXTEND ON MAXSIZE 100m;

자동 확장 설정 : AUTOEXTEND
확장 단위 설정 : ON NEXT 5M 
무한 확장 설정 : MAXSIZE UNLIMITED
	
    AUTOEXTEND ON NEXT 5M MAXSIZE UNLIMITED

### 생성한 undo tablespace 확인
	SELECT tablespace_name, contents, extent_management
  	FROM dba_tablespaces
 	WHERE contents = 'UNDO';

### undo tablespace에 설정된 rollback segment 확인
	SELECT segment_name, tablespace_name, status
      FROM dba_rollback_segs
      ORDER BY 2;

### undo tablespace 이름 변경
	ALTER SYSTEM SET UNDO_TABLESPACE = UNDOTBS2;

### 기존 undo tablespace 삭제
	DROP TABLESPACE UNDOTBS1;



## 참고
* http://thankyeon.tistory.com/31
* https://community.oracle.com/thread/454837?start=0&tstart=0