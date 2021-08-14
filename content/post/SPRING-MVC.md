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