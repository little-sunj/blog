---
title: "SPRING IoC, DI, Container"
date: 2021-01-09T18:58:02+09:00
categories:
- framework
- spring
tags:
- spring
- IoC
- DI
- Container
- 인강노트
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# IoC, DI, 스프링 컨테이너

&nbsp;

### 제어의 역전 (Inversion of Control)
기존 프로그램은 클라이언트 구현 객체가 스스로 필요한 서버 구현 객체를 생성하고, 연결하고, 실행했다. 즉, 구현 객체가 프로그램의 제어 흐름을 스스로 컨트롤했다. 하지만 프로그램의 제어 흐름을 담당하는 AppConfig가 등장한 이후, 구현 객체는 자신의 로직을 실행하는 역할만 담당한다.   
이렇듯 **프로그램의 제어 흐름을 외부에서 관리하는 것을 제어의 역전(IoC)** 라고 한다.

&nbsp;

-----


### 의존관계 주입 (Dependency Injection)
`serviceImpl`은 인터페이스에 의존하고, 실제 어떤 구현 객체가 사용될지는 **모른다** . 의존관계는 **정적인 클래스 의존 관계와, 실행 시점에 결정되는 동적인 객체(인스턴스) 의존관계** 두가지를 분리해서 생각해야 한다. **실행시점** 에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결 되는 것을 **의존관계 주입** 이라고 한다. 

&nbsp;

##### 정적인 클래스 의존관계
클래스가 사용하는 import 코드만 보고 의존관계를 쉽게 판단할 수 있다. (ex. import한 인터페이스) 

##### 동적인 객체 인스턴스 의존관계
애플리케이션 **실행 시점(런타임)**에 실제 생성된 객체 인스턴스의 참조가 연결된 의존 관계이다. (의존관계 주입) 객체 인스턴스를 생성하고, 그 참조값을 전달해서 연결된다.

&nbsp;

> 의존관계 주입을 사용하면 **정적인 클래스 의존관계**를 변경하지 않고,    
> **동적인 객체 인스턴스의 의존관계**를 쉽게 변경할 수 있다.


&nbsp;

-----

### Ioc 컨테이너, DI 컨테이너
- AppConfig처럼 객체를 생성하고 관리하면서 의존관계를 연결해 주는 것을 **IoC컨테이너** 또는 **DI 컨테이너** 라고 한다.
- 의존관계 주입에 초점을 맞추어 최근에는 주로 DI컨테이너라 한다.

&nbsp;


### 스프링 컨테이너
`ApplicationContext`를 스프링 컨테이너라 한다. `AppConfig`를 사용해서 직접 객체를 생성하고 DI를 하지 않고 스프링 컨테이너를 통해서 사용한다. `@Configuration` 어노테이션이 붙은 `AppConfig`내 `@Bean`이 적힌 메서드를 모두 호출해서 반환된 객체를 스프링 컨테이너에 등록하고, 이 객체들을 **스프링 빈** 이라고 한다. 스프링 빈은 `applicationContext.getBean(NAME, TYPE)`메서드를 사용해서 찾을 수 있다. 

> 정확히는 `BeanFactory`와 `ApplicationContext`로 구분된다. `BeanFactory`를 직접 사용하는 경우는 거의 없으므로 일반적으론 `ApplicationContext`를 스프링 컨테이너라 한다.

&nbsp;

#### 생성과정

##### 1. 컨테이너 생성

```java
ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
//ApplicationContext : 스프링 컨테이너 인터페이스
//AnnotationConfigApplicationContext : 어노테이션 기반. 인터페이스 구현체
//AppConfig.class에 있는 환경설정 정보를 가지고 스프링 컨테이너에서 넣고 관리한다.

//스프링 컨테이너는 XML 기반으로 만들 수 있고, annotation 기반의 자바 설정 클래스로 만들 수 있다.
```

##### 2. 빈 등록
- 빈 이름은 메서드 이름을 사용한다.
- 직접 부여할 수도 있다. `@Bean(name="memberService")`
- **빈 이름은 항상 다른 이름을 부여** 해야 한다. 같은 이름을 사용하면 다른 빈이 무시되거나, 오류가 발생할 수 있다.


##### 2. 스프링 빈 의존관계 설정
- 컨테이너는 설정 정보를 참고해서 의존관계를 주입한다.
- 스프링에서는 빈을 먼저 생성하고, 의존관계를 주입하는 것으로 단계가 나누어져 있다. 그러나 자바코드에 따라 스프링 빈을 등록하면 생성자를 호출하면서 의존관계 주입까지 한번에 처리하도록 할 수 있다.

&nbsp;



- `AnnotationConfigApplicationContext.getBeanDifinitionNames()` : 스프링에 등록된 모든 빈 이름을 조회
- `AnnotationConfigApplicationContext.getBean()` : 빈 이름으로 빈 객체(인스턴스) 조회
- `BeanDefinition.ROLE_APPLICATION` : 일반적으로 사용자가 정의한 빈
- `BeanDefinition.ROLE_INFRASTRUCTURE` : 스프링 내부 빈

-----


&nbsp;

#### reference
- [스프링 핵심원리](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard)

&nbsp;

-----