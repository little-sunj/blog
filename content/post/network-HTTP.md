---
title: "Network HTTP"
date: 2020-10-28T20:32:38+09:00
categories:
- network
- HTTP
tags:
- network
- HTTP
- HTTP Header
keywords:
- 스터디노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# HTTP

회사에서 프로젝트를 진행하던 중, ajax가 특정 조건에서 계속해서 406에러를 떨어뜨려 한참을 헤멨다. 일정에 쫒겨ㅠㅠㅠㅠ 어쩔수 없이 다른 방식으로 구현했지만, 명확한 원인을 파악하지 못해 '네트워크 공부좀 할껄..' 하는 후회가 들었다. 한(?)을 풀겸 원인파악 + 나중에라도 해당 화면을 개선하기 위해 따로 공부하기로 하였다. 개념이 명확하지 않은 용어들도 있으나, 차근차근 채워나가기로 한다.



&nbsp;

##  Overview
클라이언트가 요청을 생성하기 위한 연결을 연다음 응답을 받을때까지 대기하는 클라이언트-서버 모델을 따른다.   
무상태 프로토콜이다. (서버가 두 요청간에 어떤한 데이터(상태)도 유지하지 않음을 의미)

&nbsp;

##  HTTP Header

클라이언트와 서버가 요청 또는 응답으로 부가적인 정보를 전송할 수 있도록 해준다.

&nbsp;

#### 컨텍스트에 따른 분류
- General header
    - 요청과 응답 모두에 적용. body에서 최종적으로 전송되는 데이터와는 관련이 없다.
    - ex) Date, Cache-Control, Connection
- Request header
    - fetch될 리소스나 클라이언트 자체에 대한 자세한 정보를 담는다.
    - message content와는 관련이 없다.
    - ex) Accept , Accept-* , If-*  : 조건부 요청 수행 허용
    - ex) Cookie, User-Agent, Referer : 서버가 응답을 맞출 수 있도록 컨텍스트를 명확히한다.
    - request header내 있는 모든 header가 request header인 것은 아니다.
      - ex)POST request에서 content-length는 사실 entity header다.
    - Request header 예시 
    ```HTTP
    GET /home.html HTTP/1.1
    Host: developer.mozilla.org
    User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
    Accept-Language: en-US,en;q=0.5
    Accept-Encoding: gzip, deflate, br
    Referer: https://developer.mozilla.org/testpage.html
    Connection: keep-alive
    Upgrade-Insecure-Requests: 1
    If-Modified-Since: Mon, 18 Jul 2016 02:36:04 GMT
    If-None-Match: "c561c68d0ba92bbeb8b0fff2a9199f722e3a621a"
    Cache-Control: max-age=0
    ```
- Response header
    - 위치 또는 서버 자체에 대한 정보 (이름, 버전)와 같이 응답에 대한 부가적인 정보를 갖는다.
- Entity header
    - 컨텐츠 길이나 MIME타입과 같이 엔티티 바디에 대한 자세한 정보를 포함한다.

&nbsp;

#### 프록시 처리방법에 따른 분류
- End-to-End header
    - 반드시 메시지의 최종 수신자에게 전송되어야 한다. (요청:서버 / 응답:클라이언트) 중간 프록시는 반드시 종단 간 헤더를 수정되지 않은 상태로 재전송해야하며 캐시는 이를 반드시 저장해야한다.
- Hop-by-hop header
    - 단일 전송-레벨 연결에서만 의미가 있다. 프록시에 의해 재전송되거나 캐시되어서는 안된다. 
    - 홉간 헤더는 Connection 일반 헤더를 사용해 설정될 수도 있다.
- ex) connection, Keep-Alive, proxy-authenticate, proxy-authorization, TE, Trailer, Transfer-Encoding, Upgrade. 

-----

&nbsp;

#### 공부자료
- [MDN HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP)

###### 마지막 업데이트일자 - 2020.10.28


&nbsp;

-----