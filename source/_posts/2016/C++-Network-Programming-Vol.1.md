title: '책.C++ Network Programming Vol.1'
date: 2015-02-05 10:11:31
tags:
  - cpp
  - c++
  - network
  - programming	
---
[TOC]



# 책. C++ Network Programming Vol.1


> 운영체제 API 를 직접적으로 사용하여 프로그래밍 하는 것은 다음과 같은 문제점을 야기할 수 있습니다.
1. 오류를 만들기 쉽습니다.
2. 부적절한 디자인 기술 적용을 유발할 수 있습니다.


[예제로 보는 Socket API의 문제점 ](http://www.joinc.co.kr/modules/moniwiki/wiki.php/Site/C++/ACE/Documents/tutorialv1#s-9.4)


##ACE 소켓 Wrapper Facade 클래스

### ACE Socket Wrapper Facades 구조 
ACE Socket wrapper facade class는 다음과 같은 기능들을 제공한다.
> ACE\_SOCK\_* : 인터넷 영역 소켓 API함수들을 캡슐화 시킨 클래스  
ACE\_L\_SOCK\_* : Unix 영역(Local) 소켓 API함수들을 캡슐화 시킨 클래스 


### ACE Socket Wrapper Facades의 규칙 
Socket API에서 능동적으로 연결하기 위한 connect()함수와 능동적으로 연결하기 위해서 accept()함수를, 마지막으로 통신을 위해서 read(), write()함수를 사용하는 것과 마찬가지로, active connection role 과 passive connection role, communication role을 가지고 있다.  
1. `능동적 접속 규칙`active connection role(ACE\_SOCK\_Connector)은 원격지로 연결을 초기화하는 규칙을 가진다.
2. `수동적 접속 규칙`passive connection role(ACE\_SOCK\_Acceptor)은 원격지의 연결을 받아들이는 규칙을 가진다.
3. `통신 규칙`communication role(ACE\_SOCK\_Stream)은 연결이 된후 어플리케이션관 데이터를 교환하기 위한 규칙을 가진다.


	ACE 프로그래밍 가이드라인에 대한 문서는 $ACE_ROOT/docs/ACE-guidelines.html 을 참조.


`특성값(Trait)`은 템플릿 클래스의 행동을 설정하기 위하여 한 무리의 특징들을 정의하고 결합하기 위해 사용될 수 있는 C++ 일반적(generic) 프로그래밍 용어입니다.

                +-----------------+      +----------------+  
                | ACE_SOCK_Stream |      | ACE_INET_Addr  |
                +-----------------+      +----------------+
                        ^                        ^
                        |                        |
                +-----------------\      +----------------\
                | PEER_STREAM     |      | PEER_ADDR      | 
                +-----------------+      +----------------+
                        |                        |
    +---------------------------------------------------------------------+
    | ACE_SOCK_Connector                                                  |
    +---------------------------------------------------------------------+
    +---------------------------------------------------------------------+
    | + connect (stream : ACE_SOCK_Stream&, remote_addr : ACE_Addr &,     |
    |            timeout : ACE_Time_Value *= 0) : int                     |
    | + complete (stream : ACE_SOCK_Stream&, remote_addr : ACE_Addr *=0,  |
    |            timeout : ACE_Time_Value *= 0) : int                     |
    +---------------------------------------------------------------------+

- PEER\_ADDR : 이 특성값은 ACE 소켓 wrapper facade 클래스와 관련된 ACE\_INET\_Addr 주소 지정 클래스를 정의
- PEER\_STREAM : ACE\_SOCK\_Acceptor 와 ACE\_SOCK\_Connector 팩토리 클래스와 관련된 ACE\_SOCK\_Stream 데이터 전송 클래스를 정의


> 선형화(Linearization) :  배열, 연결 리스트, 그래프와 같은 고급 타입의 데이터와 raw 상태의 메모리 버퍼간의 변환을 다룹니다.  


> 마샬링/역마샬링(Marshaling/demarshaling) : 상이한 컴파일러간 정렬 규칙 및 다른 바이트 순서 규칙을 가진 하드웨어 명령들의 환경 안에서 정확하게 동작하도록 합니다.




- - -
#### 용어 

| ==용어==  | ==뜻== 
|--------|--------
|Accidental Complexity | 우발적 복합성 
| inherent complexity | 고유한 복잡성
| ACE | ADAPTIVE Communication Environment 
| SAP | service access point 
| TLI | Transport Layer Interface

- - -
#### 참고
<http://www.joinc.co.kr/modules/moniwiki/wiki.php/Site/C++/ACE/Documents/tutorialv1>  



