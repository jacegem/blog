title: '[excel] 문자열 나누기 - 데이터 타입과 길이 분할'
tags: [ excel, substring, type, length, 엑셀, 문자열, 분할 ]
date: 2016-01-12 16:13:06

---

[excel] 문자열 나누기 - 데이터 타입과 길이 분할


VARCHAR2(80)과 같은 데이터 값을 분할하여 VARCHAR2, 80으로 만들기 위한 함수를 생성합니다.

처음 조건으로 '(' 를 포함하고 있는가를 확인합니다. FIND 함수를 사용합니다. NUMBER와 같이 길이 정보가 없는 경우가 있기에 이를 구별할 필요가 있습니다. 

	=FIND("(",E2)

해당 문자를 포함하고 있으면 인덱스를 반환하지만 없으면 `#VALUE!` 에러를 반환하게 됩니다. 에러를 처리하기 위해 ISNUMBER 함수를 사용하여 감싸줍니다. 
	
	=ISNUMBER(FIND("(",E2))
	
'(' 를 포함하고 있는 문자열인 경우에는 true, 아니면 false 를 반환합니다. 이 함수를 IF 함수로 감싸서, '(' 문자를 포함하고 있는 경우와 아닌 경우를 나눠서 처리합니다. 

	=IF(ISNUMBER(FIND("(",E2)), "포함하는 경우", "없는 경우")

각각 "포함하는 경우", "없는 경우"를 구분하여 함수를 생성합니다.
포함하는 경우에는 '(' 문자 이전까지의 문자를 반환하도록 합니다. VARCHAR2(80) 의 경우 VARCHAR2 가 반환되도록 하는 것이 목적입니다. 

	=LEFT(E2, FIND("(",E2) -1)

포함하지 않는 경우에는 그 자체로 반환하면 됩니다. 

	=E2
	
두 가지 경우에 대한 내용을 모두 IF 함수 안에 포함합니다. 

	=IF(ISNUMBER(FIND("(",E2)), LEFT(E2, FIND("(",E2) -1), E2 )

이 함수를 사용하여 타입 정보를 추출합니다. 

뒤에 있는 길이 정보도 동일한 방식으로 추출합니다. 길이 정보는 뒤에 있는 ')' 문자 이전까지의 정보를 추출하기 때문에 MID 함수를 사용하여 추출합니다. 길이 정보가 없는 경우에는 빈문자열을 반환하도록 처리합니다.

	=IF(ISNUMBER(FIND("(",E2)), MID(E2, FIND("(",E2)+1, LEN(E2)-FIND("(",E2)-1 ), "" )




## 참고 
* http://www.tuning-java.com/267
* http://seongsland.tistory.com/136
* http://mainia.tistory.com/498
