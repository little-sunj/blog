---
title: "WEB SESSION COOKIE CACHE"
date: 2020-11-09T19:05:36+09:00
categories:
- WEB
tags:
- WEB
- 세션
- 쿠키
- 캐시
keywords:
- WEB
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# 세션 / 쿠키 / 캐시

&nbsp;


### SESSION
- 웹서버에 저장.
- 서버에 직접 접근하지 않는 한 데이터 탈취가 어렵다.

&nbsp;

-----

###  COOKIE
- 사용자의 브라우저에 저장
- 보안성이 없다.
- (key, value)로 구성된다.
- 이름, 값, 만료 날짜/시간(=쿠키 저장기간), 경로 정보 등이 들어있다.
- 서버에서 HTTP response header에 set-cookie 속성을 이용해 클라이언트에 쿠키를 제공한다.
- 클라이언트의 상태 정보를 로컬에 저장했다가 request시 참조한다.


-----

&nbsp;


###  CACHE
- CSS / JS 파일등을 브라우저에 저장 
- 자원을 아끼는 용도


&nbsp;

-----

&nbsp;

###### reference
[https://jeong-pro.tistory.com/80](https://jeong-pro.tistory.com/80)


-----