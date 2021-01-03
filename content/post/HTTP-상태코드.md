---
title: "HTTP 상태코드"
date: 2021-01-03T20:17:45+09:00
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
# HTTP 상태코드
클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능

&nbsp;

- **1xx (Informational)**
  - 요청이 수신되어 처리중 (거의 사용되지 않음)
- **2xx (Successful)**
  - 요청 정상 처리
  - 200 : OK / 201 : Created / 202 : Accepted / 204 : No Content (서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음 ex.웹 문서 편집기에서 save 버튼)
- **3xx (Redirection)**
  - 요청을 완료하려면 추가 행동이 필요
  - **웹 브라우저는 3xx응답의 결과에 location헤더가 있으면, location위치로 자동 이동한다.**
  - 종류
    - 영구 리다이렉션 : 특정 리소스의 URI가 영구적으로 이동 (301, 308)
    - 일시 리다이렉션 : 일시적인 변경 (302, 307, 303) ex.주문 완료 후 주문 내역 화면으로 이동, PRG (POST/Redirect/Get) : post로 주문 후에 웹 브라우저를 새로고침하는 등
    - 특수 리다이렉션 : 결과 대신 캐시를 사용 (304). 클라이언트에게 리소스가 수정되지 않았음을 알려준다. (캐시로 리다이렉트.) 304응답은 로컬 캐시를 사용해야하므로 메시지 바디를 포함하면 안된다.
- **4xx (Client Error)**
  - 클라이언트 오류, 잘못된 문법 등으로 서버가 요청을 수행할 수 없음
- **5xx (Server Error)**
  - 서버 오류. 서버가 정상 요청을 처리하지 못함

클라이언트가 인식할 수 없는 상태코드를 서버가 반환하는 경우, 클라이언트는 상위 상태코드로 해석해서 처리한다. 향후 새로운 상태코드가 추가되어도 클라이언트를 변경하지 않아도 된다. `ex. 299 -> 2xx (Success)`


&nbsp;

-----

&nbsp;

#### reference
- [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)

&nbsp;

-----
