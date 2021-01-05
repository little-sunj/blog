---
title: "SPRING 객체지향"
date: 2021-01-05T15:43:54+09:00
categories:
- framework
- spring
tags:
- spring
- 객체지향
- 인강노트
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# SPRING 객체지향

- 스프링에서 다형성 + OCP, DIP를 가능하게 하는 기술
  - DI (Dependency Injection) : 의존관계, 의존성 주입
  - DI 컨테이너 제공
- 클라이언트 코드의 변경 없이 기능 확장


설계에 **역할**과 **구현**을 분리할 것. 유연하게 변경할 수 있도록 하는 것이 좋은 객체지향 설계이다.
이상적으로는 모든 설계에 인터페이스를 부여하는 것이 좋다.

하지만 인터페이스는 추상화라는 비용이 발생하기 때문에 실무에서 기능을 확장할 가능성이 없다면, 구체적인 클래스를 직접 사용하고, 향후 꼭 필요할 때 리팩토링해서 인터페이스를 도입할 수도 있다.


&nbsp;

-----