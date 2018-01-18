---
title: HTTP Content-Type 정리
date: 2017-02-09 19:13:45
tags: [http, content, type]
categories:
- Programming
- Protocol
---

# HTTP Content-Type 정리

출처 : http://hbesthee.tistory.com/45

# 언어에 따른 선언 방식

|언어 | 방식 |
|--|--|
|ASP|	<% Response.ContentType = "text/html" %>|
|JSP|	<%@ page contentType="text/html" %>|
|PHP|	<?PHP header("Content-Type:text/html"); ?>|
|Perl|	print "Content-type: text/html\n\n";|

#Content-Type 의 종류.

1) Multipart Related MIME 타입
  - Content-Type: Multipart/related <-- 기본형태
  - Content-Type: Application/X-FixedRecord
2) XML Media의 타입
 - Content-Type: text/xml
 - Content-Type: Application/xml
 - Content-Type: Application/xml-external-parsed-entity
 - Content-Type: Application/xml-dtd
 - Content-Type: Application/mathtml+xml
 - Content-Type: Application/xslt+xml
3) Application의 타입
 - Content-Type: Application/EDI-X12 <--  Defined in RFC 1767
 - Content-Type: Application/EDIFACT <--  Defined in RFC 1767
 - Content-Type: Application/javascript <-- Defined in RFC 4329
 - Content-Type: Application/octet-stream  : <-- 디폴트 미디어 타입은 운영체제 종종 실행파일, 다운로드를 의미
 - Content-Type: Application/ogg <-- Defined in RFC 3534
 - Content-Type: Application/x-shockwave-flash <-- Adobe Flash files
 - Content-Type: Application/json <-- JavaScript Object Notation JSON; Defined in RFC 4627
 - Content-Type: Application/x-www-form-urlencode <-- HTML Form 형태
* x-www-form-urlencode와 multipart/form-data은 둘다 폼 형태이지만 x-www-form-urlencode은 대용량 바이너리 테이터를 전송하기에 비능률적이기 때문에 대부분 첨부파일은 multipart/form-data를 사용하게 된다.

4) 오디오 타입
- Content-Type: audio/mpeg <-- MP3 or other MPEG audio
- Content-Type: audio/x-ms-wma <-- Windows Media Audio;
- Content-Type: audio/vnd.rn-realaudio <--  RealAudio;  등등

5) Multipart 타입
- Content-Type: multipart/mixed: MIME E-mail;
- Content-Type: multipart/alternative: MIME E-mail;
- Content-Type: multipart/related: MIME E-mail <-- Defined in RFC 2387 and used by MHTML(HTML mail)
- Content-Type: multipart/formed-data  <-- 파일 첨부
6) TEXT 타입
- Content-Type: text/css
- Content-Type: text/html
- Content-Type: text/javascript
- Content-Type: text/plain
- Content-Type: text/xml
좀더 다양한 포맷에 대해서는 아래의 URL을 참고하기 바랍니다.
http://www.iana.org/assignments/media-types/
