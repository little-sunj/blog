---
title: "SPRING MVC패턴"
date: 2021-08-13T20:42:27+09:00
categories:
- framework
- spring
tags:
- spring
- mvc
- 인강노트
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# MVC 패턴

&nbsp;

### MVC 컨트롤러의 단점
- 포워드 중복
  - View로 이동하는 코드가 항상 중복 호출. 메서드로 공통화 하더라도 메서드를 항상 직접 호출.
- viewPath에 중복
  - prefix `/WEB-INF/views/`와 suffix `.jsp` 중복
  - jsp가 아닌 thymeleaf같은 다른 뷰로 변경한다면 전체코드를 다 변경해야한다.
- 사용하지 않는 코드
  - `HttpServletRequest request, HttpServletResponse response` 
- `HttpServletRequest request, HttpServletResponse response` 를 사용하는 코드는 테스트케이스 작성이 어렵다.
- 공통처리가 어렵다.
  - 기능이 복잡할수록 컨트롤러에서 공통으로 처리해야 하는 부분이 점점 더 많이 증가.
  - **프론트 컨트롤러(Front Controller)** 패턴을 도입하면 해결할 수 있다.

&nbsp;

-----


### 프론트 컨트롤러 패턴

- 프론트 컨트롤러가 서블릿 하나로 클라이언트의 요청을 받는다.
- 프론트 컨트롤러가 요청에 맞는 컨트롤러를 찾아서 호출
- 하나의 입구
- 공통처리 가능
- 프론트 컨트롤러를 제외한 나머지 컨트롤러는 서블릿을 사용하지 않아도 된다.
- **스프링 웹 MVC의 DispatcherServlet**이 **FrontController**패턴으로 구현되어 있다.

&nbsp;

-----



#### reference
- [스프링 MVC](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/lecture/71184?tab=note)

&nbsp;

-----