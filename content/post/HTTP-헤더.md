---
title: "HTTP 헤더"
date: 2021-01-04T19:28:49+09:00
categories:
- network
tags:
- network
- HTTP
- 상태코드
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# HTTP Header

### 개요

- HTTP전송에 필요한 모든 부가 정보를 담는다.
- 표준 헤더 필드가 굉장히 많다.
- 필요시 임의로 추가할 수 있다.
- header-hield = field-name ":" OWS field-value OWS (OWS : 띄어쓰기 허용)
- field-name은 대소문자 구분이 없다.

&nbsp;

-----


### 분류

- General 헤더 : 메시지 전체에 적용되는 정보 (ex. `connection: close`)
- Request 헤더 : 요청 정보 (ex. `user-agent: Mozilla/5.0`)
- Response 헤더 : 응답 정보 (ex. `server : apache`)
- Entity 헤더 : 엔티티 바디 정보 (ex. `Content-type : text/html`, `content-length: 1000`)

**2014년부터 엔티티(entity) > 표현(representation)으로 용어가 변경되었다.**
```
representation = representation metadata +  data
(표현 = 표현 메타데이터 + 표현 데이터)
```
- 메시지 본문을 통해 representation 데이터 전달.
- 메시지 본문 = 페이로드(payload)
- **표현**은 요청이나 응답에서 전달할 실제 데이터
- **표현 헤더는 표현 데이터**를 해석할 수 있는 정보 제공
  - 데이터 유형(html, json), 데이터 길이, 압축 정보 등

&nbsp;

-----

### 표현

- Content-type : 표현 데이터의 형식
  - 미디어 타입 / 문자 인코딩
  - ex. `text/html; charset=utf-8`, `application/json`, `image/png`
- content-encoding : 표현 데이터의 압축 방식
  - 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
  - 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
  - ex. `gzip`,`deflate`, `identity(압축 안한다는 뜻)`
- content-language : 표현 데이터의 자연 언어
  - ex. `ko`, `en`, `en-US`
- content-length : 표현 데이터의 길이
  - 바이트 단위
  - transfer-encoding(전송코딩)을 사용하면 content-length를 사용하면 안된다. (전송코딩내 정보가 담겨있다.)

> 표현 헤더는 전송 / 응답 둘다 사용

&nbsp;

-----

### 협상 (contents negotiation)
클라이언트가 선호하는 표현 요청

- accept : 클라이언트가 선호하는 미디어 타입 전달
- accept-charset : 클라이언트가 선호하는 문자 인코딩
- accept-encoding : 클라이언트가 선호하는 압축 인코딩
- accept-language : 클라이언트가 선호하는 자연 언어

> 협상 헤더는 요청시에만 사용한다.

&nbsp;

#### 협상과 우선순위 (quality alues)
- 0~1 사이의 숫자 중 **값이 클수록 높은 우선순위를 가진다.**
  - 생략하면 1
  - accept-language : `ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7`
    - 1순위 `ko-KR;q=1` (q생략됨)
    - 2순위 `ko;q=0.9`
    - 3순위 `en-US;q=0.8`
    - 4순위 `en;q=0.7`
- **구체적인 것이 우선한다**
  - accept : `text/*`, `text/plain`, `text/plain;format=flowed`, `*/*`
    - 1순위 `text/plain;format=flowed`
    - 2순위 `text/plain`
    - 3순위 `text/*`
    - 4순위 `*/*`
- **구체적인 것을 기준으로 미디어 타입을 맞춘다.**
  - accept : `text/*;q=0.3`, `text/html;q=0.7`, `text/html;level=1`, `text/html;level=2;q=0.3`, `*/*;q=0.5`
      - | Media Type | quality |
        |--|--|
        | text/html;level=1 | 1 |
        | text/html; | 0.7 |
        | text/plain; | 0.3 |
        | image/jpg; | 0.5 |
        | text/html;level=2 | 0.4 |
        | text/html;level=3 | 0.7 |
&nbsp;

-----

### 전송방식

- 단순 전송
  - content-length
- 압축 전송
  - content-encoding
- 분할 전송
  - transfer-encoding
  - 모든 데이터가 올 때까지 기다리지 않고 오는대로 바로바로 표시할 수 있다.
  - content-length를 추가하면 안된다. (분할해서 오기 때문에 content길이를 알 수 없다.)
- 범위 전송
  - range, content-range
  - 받다가 끊어졌거나 하는 경우, 받은 부분을 제하고 나머지를 보내오라 요청 하는 등

&nbsp;

-----

### 일반 정보 헤더
일반 정보성 헤더들

- **From** : 유저 에이전트의 이메일 정보
  - 일반적으로 잘 사용되지 않음
  - 검색 엔진 같은 곳에서, 주로 사용
  - 요청에서 사용
- **Referer** : 이전 웹 페이지 주소
  - 자주 사용된다.
  - 현재 요청된 페이지의 이전 웹 페이지 주소
  - A &#10140; B 로 이동하는 경우 B를 요청할 때 Referer: A를 포함해서 요청
  - Referer를 사용해서 유입 경로 분석 가능
  - 요청에서 사용
  - Referer는 단어 Referrer의 오타이다.
    - 이미 상용화되어서 고치지 못하게 되었다..
- **User-Agent** : 유저 에이전트 애플리케이션 정보
  - 클라이언트의 애플리케이션 정보 (웹 브라우저 정보 등)
  - 통계 정보
  - 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
  - 요청에서 사용
- **Server** : 요청을 처리하는 ORIGIN 서버의 소프트웨어 정보
  - 실제 요청에 응하는 응답을 해주는 서버를 ORIGIN 서버라고 한다.
  - 응답에서 사용한다.
- **Date** : 메시지가 발생한 날짜와 시간
  - 응답에서만 사용한다.

&nbsp;

-----

### 특별 정보 헤더

- **Host** : 요청한 호스트 정보 (도메인)
  - 요청에서 사용
  - **필수값**이다
  - 하나의 서버가 여러 도메인을 처리해야 할 때
  - 하나의 IP주소에 여러 도메인이 적용되어 있을 때 받을 도메인 지정
- **Location** : 페이지 리다이렉션
  - 웹 브라우저는 3xx 응답의 결과에 location헤더가 있으면, location위치로 자동 이동 (리다이렉트)
  - 201에서 location값은 요청에 의해 생성된 리소스 URI
- **Allow** : 허용 가능한 HTTP 메서드
  - 405 (Method not allowed) 에서 응답에 포함해야한다.
  - Allow : GET, HEAD, PUT
  - 자주 사용되지는 않는다.
- **Retry-After** : 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
  - 503 (service Unavailable) : 서비스가 언제까지 불능인지 알려줄 수 있다.

&nbsp;

-----

### 인증 헤더

- **Authorization** : 클라이언트 인증 정보를 서버에 전달
- **WWW-Authenticate** : 리소스 접근시 필요한 인증 방법 정의
  - 401 Unauthorized 응답과 함께 사용

&nbsp;

-----

### 쿠키

- **Set-Cookie** : 서버에서 클라이언트로 쿠키 전달(응답)
- **Cookie** : 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP요청시 서버로 전달한다.
  - 모든 요청에 쿠키 정보를 자동으로 포함한다.
    - 네트워크 트래픽 추가 유발
    - set-cookie에서 sessionId, 도메인, 만료시간 등을 추가해 제한한다.
    - 최소한의 정보만 사용한다.
    - 서버에 전송하지 않고 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지를 참고
    - 보안에 민감한 데이터는 저장하면 안된다.

- 사용처
  - 사용자 로그인 세션 관리
  - 광고 정보 트래킹

&nbsp;

#### 쿠키 생명 주기 
- `set-cookie : expires=Sat...`
  - 만료일이 되면 쿠키 삭제
- `set-cookie : max-age=3600`
  - 0이나 음수를 지정하면 쿠키 삭제
- 세션 쿠키 : 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
- 영속 쿠키 : 만료 날짜를 지정하면 해당 날짜까지 유지

&nbsp;

#### 쿠키 도메인 (domain)
- 명시 : 명시한 문서 기준 도메인 + 서브 도메인 포함
  - `domain=example.com` 으로 지정할 시
    - `dev.example.com`에서도 접근 가능
- 생략 : 현재 문서 기준 도메인만 적용
  - `domain=example.com` 으로 지정할 시
    - `dev.example.com`에서 접근 불가. `example.com`에서만 접근 가능

&nbsp;

#### 쿠키 경로 (path)
- 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
- 일반적으로 `path=/` 루트로 지정

&nbsp;

#### 쿠키 보안 (secure, httpOnly, SameSite)
- **Secure**
  - 쿠키는 http, https를 구분하지 않고 전송
  - secure를 적용하면 https인 경우에만 전송
- **HttpOnly**
  - XSS 공격 방지
  - 자바스크립트에서 접근 불가(document.cookie)
  - HTTP전송에만 사용
- **SameSite**
  - XSRF 공격 방지
  - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송

-----
-----

## 캐시와 조건부 요청

- 캐시 사용시 캐시 가능 시간동안 네트워크를 사용하지 않아도 된다.
- 네트워크 사용량을 줄일 수 있다.
- 브라우저 로딩 속도가 매우 빠르다
- 빠른 사용자 경험
- 캐시 유효 시간 초과시 서버를 통해 데이터를 다시 조회하고 캐시를 갱신한다.
  - case 1. 서버에서 기존 데이터를 변경함
  - case 2. 서버에서 기존 데이터를 변경하지 않음
    - 데이터를 전송하는 대신에 저장해두었던 캐시를 재사용 할 수 있다.
    - 단 클라이언트의 데이터와 서버의 데이터가 동일함을 검증해야한다.

&nbsp;

#### 검증 헤더 (validator)
- 캐시 데이터와 서버 데이터가 같은지 검증하는 데이터
- last-Modified, ETag

&nbsp;

#### 조건부 요청 헤더
- 검증 헤더로 조건에 따른 분기
- If-Modified Since : Last-Modified 사용
  - **검증 헤더 추가**
      1. `Last-Modified` : 마지막 수정된 시간 추가해서 응답
      2. 캐시 만료 후 요청시 `if-modified-since`에 캐시가 가지고 있는 마지막 수정시간 추가 = **조건부 요청**
      3. 서버에서 데이터 최종 수정일을 비교해서 검증
      4. 수정이 안된 경우 서버는 `304 Not Modified`라고 응답한다. 이때 HTTP Body는 전송하지 않는다.
      5. 클라이언트는 응답 결과를 확인 후, 캐시의 메타 정보를 갱신하여 재사용한다.
      6. 결과적으로 네트워크 다운로드가 발생하지만 용량이 적은 헤더만 받아 굉장히 효율적이다.
  - 단점 
    - 1초 미만 단위로 캐시 조정이 불가능
    - 날짜 기반의 로직 사용
    - 서버에서 별도의 캐시 로직을 관리하고 싶은 경우
- If-None-Match : ETag (Entity Tag) 사용
  - 캐시용 데이터에 임의의 고유한 버전 이름을 달아둔다 (ex. `Etag:v1.0`, `ETag:asgd234..(해시값)`)
  - 데이터가 변경되면 이 이름을 바꾸어서 변경한다 (Hash를 다시 생성) (ex. `ETag:aaa` &#10140; `Etag:bbb`)
  - ETag만 보내서 같으면 유지하고 다르면 다시 받는다.
  - **캐시 제어 로직을 서버에서 완전히 관리**
  - 클라이언트는 캐시 메커니즘을 모른다.
- 조건이 만족하면 200 OK
- 조건이 만족하지 않으면 304 Not Modified

&nbsp;

-----

### 캐시 제어 헤더
- **Cache-Control** : 캐시 제어
  - 캐시 지시어 (directives)
  - `Cache-Control: max-age` : 캐시 유효 시간, 초 단위
  - `Cache-Control: no-cache` : 데이터는 캐시해도 되지만, 항상 원(origin)서버에 검증하고 사용
  - `Cache-Control: no-store` : 데이터에 민감한 정보가 있으므로 저장하면 안됨 (메모리에 사용하고 최대한 빨리 삭제)
- **Pragma** : 캐시 제어 (하위호환)
- **Expires** : 캐시 유효 기간 (하위호환)
  - `cache-control: max-age` 사용을 권장한다. 함께 사용하면 `expires`는 무시된다.

&nbsp;

-----

### 프록시 캐시

```
┌────────┐  0.1sec   ┌──╌───╌──╌──╌──╌──┐  0.5sec   ┌──╌────────────────┐
│ client │───────────│   proxy server   │───────────│   origin server   │
└────────┘           └────╌──╌──╌──╌────┘           └───────────────────┘
한국 브라우저           한국 프록시 캐시 서버           미국 원 서버
                        (CDN 등..)
private 캐시            public 캐시                   
```

- **Cache-Control: public** : 응답이 public 캐시에 저장되어도 됨
- **Cache-Control: private** : 응답이 해당 사용자만을 위한 것. private 캐시에 저장해야 함(기본값)
- **Cache-Control: s-maxage** : 프록시 캐시에만 적용되는 max-age
- **Age: 60** : (HTTP 헤더) 오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간(초)

&nbsp;

-----

### 캐시 무효화

- **Cache-Control**
  - : no-cache - 데이터는 캐시해도 되지만 항상 원 서버에 검증
  - : no-store - 데이터에 민감한 정보가 있으므로 저장하면 안됨
  - : must-realidate - 캐시 만료 후 최초 조회시에 원 서버에 검증. 원 서버 접근 실패시 반드시 오류가 발생해야 한다. (504 Gateaway timeout). 캐시 유효 시간이라면 캐시를 사용한다.
- **Pragma: no-cache** : HTTP 1.0  하위호환
- 확실하게 캐시 무효화를 하려면 다 적용할 것


&nbsp;

-----

&nbsp;

#### reference
- [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)

&nbsp;

-----