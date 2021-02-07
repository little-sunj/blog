---
title: "SPRING 엔티티설계주의사항"
date: 2021-02-07T16:02:37+09:00
categories:
- framework
- spring
tags:
- spring
- 엔티티설계
- 인강노트
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# ENTITY 설계시 주의사항


&nbsp;

### 1. 엔티티에는 가급적 SETTER를 사용하지 않는다.
- Setter가 모두 열려있다면 변경 포인트가 너무 많아 유지보수가 힘들다.

&nbsp;

### 2. 모든 연관관계는 지연로딩(`LAZY`)으로 설정한다.
- 즉시로딩(`EAGER`)는 예측이 어렵고 어떤 SQL이 실행될지 추적하기 힘들다. (EAGER시 연관된 데이터를 다 끌고옴) 특히 JPQL을 실행할 때 N+1 문제가 자주 발생한다. 절대 쓰지 말것.
- 연관된 엔티티를 함께 DB에서 조회해야 하면, fetch join또는 엔티티 그래프 기능을 사용한다.
- `@XToOne(OneToOne, ManyToOne)`관계는 기본이 즉시로딩이므로 직접 지연로딩으로 설정해야 한다.

&nbsp;

### 3. 컬렌션은 필드에서 초기화 한다.
- `null` 문제에 안전하다.
- 하이버네이터는 엔티티를 (persist)영속화 할 때, 컬렉션을 감싸서 하이버네이트가 제공하는 내장 컬렉션으로 변경한다. 메서드에서 컬렉션을 잘못 생성하면 하이버네이트 내부 매커니즘에 문제가 발생할 수 있다. 따라서 필드레벨에서 생성하는 것이 가장 안전하고, 코드도 간결하다.
- 컬렉션을 가급적 변경하지 말 것. (하이버네이트가 추척할 수 있는 관리클래스로 변경된걸 굳이 또 조작하지 말 것)

```java
Member member = new Member();
System.out.println(member.getOrdders().getClass());
em.persist(team);
System.out.println(member.getOrdders().getClass());

//출력결과
//class java.util.ArrayList
//class org.hibernate.collection.internal.PersistentBag
```

&nbsp;

### 4. 테이블, 컬럼명 생성 전략.
- 스프링 부트에서 하이버네이트 기본 매핑 전략을 변경해서 실제 테이블 필드명은 다르다.
    1. **하이버네이트 기존 구현**
       1. 인티티의 필드명을 그대로 테이블 명으로 사용
          - `SpringPhysivalNamingStrategy` 
    2. **스프링 부트 신규 설정 (엔티티(필드)) -> 테이블(컬럼))** 
       1. 카멜 케이스 -> 언더스코어(`memberPoint` -> `member_point`)
       2. `.` (점) -> `_ `(언더스코어)
       3. 대문자 -> 소문자


&nbsp;

### 5.엔티티나 임베디드 타입은 자바 기본 생성자를 `protected`로 설정한다.
- JPA 스펙상 엔티티나 임베디드 타입은 자바 기본 생성자를 `public`또는 `protected`로 설정해야 한다. `protected`가 그나마 더 안전하다. JPA가 이런 제약을 두는 이유는 JPA 구현 라이브러리가 객체를 생성할 때 리플렉션 같은 기술을 사용할 수 있도록 지원해야 하기 때문이다.
- 생성자에서 값을 모두 초기화해서 변경 불가능한 클래스를 만든다.
- 값 타입으로 변경을 허용하지 않는 것이 좋은 설계다. Setter를 아예 제공하지 않는다.


&nbsp;

-----
