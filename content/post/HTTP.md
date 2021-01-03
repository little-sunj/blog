---
title: "HTTP"
date: 2020-10-28T20:32:38+09:00
categories:
- network
tags:
- network
- HTTP
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

&nbsp;

-----

##  HTTP 기반 API

- XMLHttpRequest API
    - 가장 일반적. user agent와 서버간의 데이터를 교환하는데 사용된다.
- Fetch API
- 서버-전송 이벤트
  - 서버가 전송 메커니즘으로 HTTP를 사용하여 클라이언트로 이벤트를 보낼 수 있도록 하는 단방향 서비스.

-----

##  HTTP 메서드 종류

#### 주요 메서드
- GET : 리소스 조회
  - 서버에 전달하고 싶은 데이터는 QUERY(쿼리 파라미터, 쿼리 스트링)을 통해서 전달
  - 메시지 바디를 사용해서 데이터를 전달할 수 있지만, 지원하지 않는 곳이 많아서 권장하지 않는다.
- POST : 요청 데이터 처리, 주로 등록에 사용
  - **메시지 바디를 통해 서버로 요청 데이터 전달**
  - 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행한다.
  - 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용. 요청에 포함 된 표현을 처리
    - ex1) HTML 양식에 입력 된 필드와 같은 데이터 블록을 데이터 처리 프로세스에 제공 (회원가입, 주문)
    - ex2) 게시판, 뉴스 그룹, 메일링 리스트, 블로그 또는 유사한 기사 그룹에 메시지 게시 (게시판 글쓰기, 댓글 달기)
    - ex3) 서버가 아직 식별하지 않은 새 리소스 생성 (신규 주문 생성)
    - ex4) 기존 자원에 데이터 추가 (한 문서 끝에 내용 추가)
  - **리소스 URI에 POST요청이 들어오면 요청 데이터를 어떻게 처리할지 리소스마다 따로 정해야한다. 즉, 정해진 것이 없다.** 
  - 다른 메서드로 처리하기 애매한 경우 사용하기도 한다.
  - POST의 결과로 새로운 리소스가 생성되지 않을 수도 있다. (컨트롤URI를 사용하기도 한다. ex - POST /orders/{orderId}/start-delivery)
- PUT : 리소스가 있으면 **완전히** 대체(덮어버린다), 해당 리소스가 없으면 생성
  - **클라이언트가 리소스를 식별** : 클라이언트가 리소스 위치를 **알고 URI 지정**. 이것이 POST와의 큰 차이점이다.
- PATCH : 리소스 부분 변경
  - 간혹 PATCH를 지원하지 못하는 서버도 있다. 그런 경우 POST를 사용하면 된다.
- DELETE : 리소스 삭제

#### 기타 메서드
- HEAD : GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환
- OPTIONS : 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS에서 사용)
- CONNECT : 대상 자원으로 식별되는 서버에 대한 터널을 설정
- TRACE : 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행

&nbsp;

-----

## HTTP 메서드 속성

#### 안전 (safe)
- 호출해도 리소스를 변경하지 않는다. (ex) GET) POST등은 리소스를 변경할 수 있으므로 해당되지 않는다.

#### 멱등 (Idempotent)
- f(f(x)) = f(x)
- 한 번 호출하든 두 번 호출하든 100번 호출하든 결과가 똑같다.
- 멱등 메서드 
  - GET : 한 번 조회하든, 두 번 조회하든 같은 결과 조회
  - PUT : 결과를 대체. 따라서 같은 요청을 여러번 해도 결과는 같다.
  - DELETE : 결과를 삭제한다. 같은 요청을 여러번 해도 삭제된 결과는 똑같다.
  - **POST : 멱등이 아니다.** 여러번 호출시 같은 결제가 중복 발생할 수 있다.
- 활용 
  - 자동 복구 매커니즘
  - 서버가 TIMEOUT 등으로 정상 응답을 못 주었을 때, 클라이언트가 같은 요청을 다시 해도 되는가의 판단근거.
  - **멱등은 외부 요인으로 중간에 리소스가 변경되는 것 까지는 고려하지 않는다.** : 중간에 바뀌면 멱등하지 않다고 판단.

#### 캐시가능 (cacheable)
- 응답 결과 리소스를 캐시해서 사용해도 되는가.
- GET / HEAD / POST / PATCH 캐시가능
- 실제로는 GET / HEAD 정도만 캐시로 사용
  - POST / PATCH는 본문 내용까지 캐시 키로 고려해야 하는데, 구현이 쉽지 않다.

&nbsp;

-----

## HTTP 메서드 활용

### 클라이언트에서 서버로 데이터 전송

- 데이터 전달 방식은 크게 2가지
  - 쿼리 파라미터를 통한 데이터 전송
    - GET
    - 주로 정렬 필터 (검색어)
  - 메시지 바디를 통한 데이터 전송
    - POST / PUT / PATCH 
    - 회원가입, 상품주문, 리소스 등록, 리소스 변경
  
- CASE 4가지
  - 정적 데이터 조회
    - 이미지, 정적 텍스트 문서
    - 조회는 GET 사용
    - 정적 데이터는 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능
  - 동적 데이터 조회
    - 주로 검색, 게시판 목록에서 정렬 필터 (검색어)
    - 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용
    - 조회는 GET 사용 (쿼리 파라미터 사용해서 데이터 전달)
  - HTML FORM을 통한 데이터 전송
    - 회원 가입, 상품 주문, 데이터 변경
    - **GET / POST만 지원한다.**
    - submit 버튼 클릭시 웹 브라우저가 form 데이터를 읽어서 http 메시지를 생성
    - key=value 형식으로 생성된다.
    - 전송 데이터를 url encoding 처리
    - 파일 전송시에는 `enctype="multipart/form-data"`를 추가한다. 바이너리 데이터를 전송할때 주로 사용된다.
    ```
    //이렇게 추가하면 http메시지가 아래처럼 생성된다.

    POST /home HTTP/1.1
    Host: localhost:8080
    content-type: multipart/form-data; boundary=----XXX //웹 브라우저가 알아서 랜덤으로 생성
    content-Length : 10000

    ------XXX
    Content-Disposition: form-data; name="username"

    kim
    ------XXX
    Content-Disposition: form-data; name="age"

    20
    ------XXX
    Content-Disposition: form-data; name="file1"; filename="image.png"
    Content-Type:image/png

    123123Dsdfkjsel234...
    ------XXX

    ```
  - HTML API를 통한 데이터 전송
    - 회원 가입, 상품 주문, 데이터 변경
    - 서버 to 서버(백엔드 시스템 통신), 앱 클라이언트, 웹 클라이언트 (ajax)
    - POST / PUT / PATCH  메시지 바디를 통해 데이터 전송
    - GET 조회, 쿼리 파라미터로 데이터 전달
    - Content-Type: application/json을 주로 사용 (사실상 표준)
      - TEXT / XML / JSON 등등..




&nbsp;

-----

&nbsp;

#### reference
- [MDN HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP)
- [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)

&nbsp;

-----