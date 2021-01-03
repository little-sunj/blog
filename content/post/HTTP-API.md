---
title: "HTTP API"
date: 2021-01-03T19:34:38+09:00
categories:
- network
tags:
- network
- HTTP
- API
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# HTTP API

&nbsp;

### 설계 예시

- **POST 기반 등록**
  ```
  //members 컬렉션에서 API 설계 예시
  회원 목록 /members        //GET
  회원 등록 /members        //POST
  회원 조회 /members/{id}   //GET
  회원 수정 /members/{id}   //PATCH, PUT, POST
  회원 삭제 /members/{id}   //DELETE
  ```
  - 클라이언트는 등록될 리소스의 URI를 모른다.
  - **서버가 새로 등록될 리소스 URI를 생성해준다.**(Location: /members/100)
  - **컬렉션(Collection)**
    - 서버가 관리하는 리소스 디렉토리
    - 서버가 리소스의 URI를 생성하고 관리 
    - 여기서 members를 컬렉션이라고 한다.
  - 가장 많이 사용되는 유형이다.

&nbsp;

- **PUT 기반 등록**
  ```
  //files 스토어에서 API 설계 예시
  파일 목록 /files              //GET
  파일 조회 /files/{filename}   //GET
  파일 등록 /files/{filename}   //PUT
  파일 삭제 /files/{filename}   //DELETE
  파일 대량 등록 /files         //POST
  ```
  - **클라이언트가 리소스 URI를 알고 있어야 한다.** ex.PUT `/files/image.jpg`
  - 클라이언트가 직접 리소스의 URI를 지정한다.
  - **스토어 (Store)**
    - 정적 컨텐츠 관리, 원격 파일 관리
    - 클라이언트가 관리하는 리소스 저장소
    - 클라이언트가 리소스의 URI를 알고 관리한다.
    - 여기서 files를 스토어 라고한다.

&nbsp;

- **HTML FORM 사용**
  - 웹 페이지 회원 관리
  - 순수하게는 GET / POST 만 지원하므로 제약이 있다.
  - AJAX같은 기술을 사용해서 해결 가능
  ```
  //members 컬렉션에서 API 설계 예시
  회원 목록         /members                            //GET
  회원 등록 폼      /members/new                        //GET
  회원 등록         /members/new, /members              //POST
  회원 조회         /members/{id}                       //GET
  회원 수정         /members/{id}                       //GET
  회원 수정 폼      /members/{id}/edit, /members/{id}   //POST
  회원 삭제         /members/{id}/delete                //POST
  ```
  - 컨트롤 URI 를 사용해야한다.
    - 제약을 해결하기 위해 동사로 된 리소스 경로 사용 (ex. `/new`, `/edit`, `/delete`)
    - HTTP 메서드로 해결하기 애매한 경우 사용(HTTP API 포함) 

&nbsp;

-----

### URI 설계 개념

##### 문서 (document)
- 단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 row)
- `/members/1`, `/files/image.png`

##### 컬렉션 (Collection)
- 서버가 관리하는 리소스 디렉토리
- `/members`

##### 스토어
- 클라이언트가 관리하는 자원 저장소
- `/files`

##### 컨트롤러(Controller) / 컨트롤 URI
- 문서 / 컬렉션 / 스토어로 해결하기 어려운 추가 프로세스 실행
- 동사를 직접 사용
- `/members/{id}/delete`

> 실제 업무에서는 어쩔 수 없이 컨트롤 URI를 사용해야 하는 경우가 많다.    
> 하지만 무분별한 사용은 지양한다.

&nbsp;

-----

&nbsp;

#### reference
- [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)
- [REST API naming guide](https://restfulapi.net/resource-naming/)

&nbsp;

-----
