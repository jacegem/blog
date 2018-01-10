title: '[toad] Script 정보에서 커멘트 정보 얻는 정규표현식'
tags: [toad, script, regex, comment]
date: 2016-01-24 20:31:04

---

## Comment from TOAD script
	...
	COMMENT ON TABLE SCHEMA.TABLE IS '테이블명';
	
	COMMENT ON COLUMN SCHEMA.TABLE.COL_YYMM IS '컬럼년월';
	
	COMMENT ON COLUMN SCHEMA.TABLE.COL_NO IS '컬럼번호';
	
	COMMENT ON COLUMN SCHEMA.TABLE.COL_CODE IS '컬럼코드';
	...

toad 에서 테이블 script 정보를 확인하면 위와 같은 형식으로 되어 있는 스크립트를 확인할 수 있습니다. ERD 확인이 어려운 경우에 스크립트에 있는 Comment정보로 한글명을 확인합니다. 

## Regular expression

엑셀에 넣기 위해 각 항목을 탭으로 구분되도록 변경합니다. 

```re
COMMENT ON (TABLE|COLUMN) \w+?\.(\w+)\.?(\w+)? IS '(\w+)';\s*
```

모두 시작은 `COMMENT ON`으로 시작하므로 문자열 그대로 사용합니다. `COMMENT ON`

그 다음 TABLE 또는 COLUMN 문자열이 올수 있으므로 그룹으로 지정합니다. `(TABLE|COLUMN)`

그 다음 첫번째 점(.) 이전에는 스키마정보가 있습니다만 필요 없으므로 이부분은 그룹핑에서 제외합니다. `\w+?\.`

그 다음 테이블명이 옵니다. 그룹핑을 합니다. 그 뒤에 테이블인 경우에는 아무것도 없지만 컬럼명인 경우에는 점(.)이 따라옵니다. `(\w+)\.?`

그 다음 컬럼명이 옵니다. 그룹핑을 하지만 테이블인 경우에는 해당 내용이 없으므로 `?`를 사용합니다. `(\w+)?`

그 다음 IS 문자열이 옵니다. 그대로 적습니다. `IS`

그 다음 한글로 된 커멘트가 옵니다. 커멘트는 작은따옴표로 감싸여 있습니다. `'(\w+)'`

그 다음 세미콜론(;)이 오고 줄바꿈기호가 있습니다. `;\s*`

그룹핑한 정보들만으로 치환하여 원하는 값을 추출합니다. 

```re
$1\t$2\t$3\t$4\n
```

그룹핑된 정보들을 탭 구분으로 변경합니다. 맨 마지막에는 줄바꿈을 포함합니다.

`$1`에는 TABLE 또는 COLUMN 문자열이 매칭되어 있습니다.
`$2`에는 테이블명이 매칭되어 있습니다.
`$3`에는 컬럼명이 매칭되어 있습니다. 테이블 커멘트에서는 아무것도 매칭되어 있지 않습니다.
`$4`에는 한글설명이 매칭되어 있습니다.


## Notepad++ 를 이용한 치환

![notepad++](https://goo.gl/hL4Lr7)

[notepad++]에서 치환한 모습입니다.


```re
찾을내용: COMMENT ON (TABLE|COLUMN) \w+?\.(\w+)\.?(\w+)? IS '(\w+)';\s*
바꿀내용: $1\t$2\t$3\t$4\n
```
