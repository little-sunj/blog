---
title: "Network HTTP"
date: 2020-10-28T20:32:38+09:00
categories:
- network
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
HTML문서와 같은 리소스들을 가져올 수 있도록 해주는 프로토콜(데이터의 교환 방식을 정의하는 규칙의 집합). 웹에서 이루어지는 모든 데이터 교환의 기초이다. 

&nbsp;

- 사람이 읽을 수 있도록 고안되었다. 
- 확장 가능하다.
  - HTTP header는 HTTP를 확장하고 실험하게 쉽게 만들어준다. 
- 클라이언트 서버 프로토콜 
    - 수신자(보통 웹브라우저)측에 의해 요청이 초기화 되는 프로토콜을 의미한다. 
    - 클라이언트가 요청을 생성하기 위한 연결을 연다음 응답을 받을때까지 대기한다.   
- 무상태 프로토콜 
  - 서버가 두 요청간에 어떤한 데이터(상태)도 유지하지 않음을 의미. 비연결성/비상태성) 
  - 연결 상태를 유지하지 않고, 연결 해제 후에도 상태정보를 저장하지 않기 때문에 서버의 자원을 크게 절약할 수 있다. 
  - 하지만 이로인해 사용자를 식별할 수 없어서 같은 사용자가 요청을 여러번 하더라도 매번 새로운 사용자로 인식하는 단점이 있다. (이런 비연결/비상태 특성을 쿠키와 세션으로 보완하여 로그인 상태를 유지하게 된다.)
- 상태는 없지만(무상태) 세션은 있다.
   

&nbsp;

-----

##  HTTP 기반 시스템 구성

```

client <──────> proxy  <──────> proxy <──────> server
_______________________________________________________

client : user agent. '항상' 요청을 보내는 개체로 주로 브라우저다.

server : 통신 채널의 반대편에서 client에 의한 요청에 대한 응답을 제공한다.

proxy : 웹브라우저와 서버 사이 애플리케이션 계층에서 동작하는 컴퓨터/머신들. 
        다양한 기능을 수행할 수 있다.
        - 캐싱 : 공개 또는 비공개 가능 
        - 필터링 : 바이러스 백신 스캔 또는 유해 컨텐츠 차단
        - 로드 밸런싱 : 여러 서버들이 서로 다른 요청을 처리하도록 허용
        - 인증 : 다양한 리소스에 대한 접근 제어
        - 로깅 : 이력 정보 저장

```

&nbsp;

-----

##  HTTP 흐름

클라이언트가 서버와 통신하고자 할 때, 최종 서버가 됐든 중간 프록시가 됬든, 아래 과정을 수행한다.

&nbsp;

1. TCP연결을 연다.
    - 클라이언트는 새 연결을 열거나, 기존 연결을 재사용하는 등, 서버에 대한 여러 TCP연결을 열 수 있다.
2. HTTP 메시지를 전송한다.
    - HTTP 메시지는 인간이 읽을 수 있다. 
3. 서버에 의해 전송된 응답을 읽어들인다.
4. 연결을 닫거나 다른 요청들을 위해 재사용한다.

HTTP파이프라인이 활성화되면, 응답을 완전히 수신할 때까지 기다리지 않고 여러 요청을 보낼 수 있다. 

&nbsp;

-----

##  HTTP 메시지

&nbsp;

#### - 요청(Request)

![HTTP_Request](https://user-images.githubusercontent.com/28701069/98465174-c3819980-220a-11eb-93fe-b74b4a9631fd.png)

- 구성요소 
  - method
    - 보통 클라이언트가 수행하고자 하는 동작을 정의한 GET / POST같은 동사나 OPTIONS나 HEAD 같은 명사다. 
  - path 
    - 가져오려는 리소스의 경로 
    - 프로토콜(https://), 도메인, TCP포트인 요소들을 제거한 리소스의 URL
  - version of protocol 
    - 프로토콜의 버전
  - Headers
    - 서버에 대한 추가 정보를 전달하는 선택적 헤더들
  - POST와 같은 몇 가지 메서드를 위한, 전송된 리소스를 포함하는 본문. (응답의 본문과 유사)

&nbsp;
&nbsp;

#### - 응답(Response)

![HTTP_Response](https://user-images.githubusercontent.com/28701069/98465322-bfa24700-220b-11eb-9893-820aad1b2b54.png)

- 구성요소 
  - version of the protocol
    - 프로토콜의 버전
  - status code
    - 요청의 성공 여부와, 그 이유를 나타내는 상태코드
  - status message
    - 상태코드의 짧은 설명을 나타내는 상태 메시지 (아무 영향력이 없다)
  - Headers
    - 요청 헤더와 비슷하다.
  - 선택 사항으로, 가져온 리소스가 포함되는 본문


-----

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


&nbsp;

-----

##  HTTP 기반 API

- XMLHttpRequest API
    - 가장 일반적. user agent와 서버간의 데이터를 교환하는데 사용된다.
- Fetch API
- 서버-전송 이벤트
  - 서버가 전송 메커니즘으로 HTTP를 사용하여 클라이언트로 이벤트를 보낼 수 있도록 하는 단방향 서비스.

-----

&nbsp;

#### 공부자료
- [MDN HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP)

###### 마지막 업데이트일자 - 2020.11.08


&nbsp;

-----