---
title: "JAVA Wrapper"
date: 2020-09-27T20:13:33+09:00
categories:
- JAVA
- JAVA기본
tags:
- JAVA
- wrapper
- 인강노트
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# Wrapper 

&nbsp;


###  Wrapper 클래스
기본자료형을 객체 자료형으로 사용할 수 있도록 만들어 놓은 자료형 (**포장 클래스**)   
기본 자료형대신 Object클래스의 자식 클래스(객체)로 활용 가능

&nbsp;

|  기본자료형 |  객체자료형 | 사용 예  |
|---|---|---|
|  int | Integer   | 1, new Integer(1)   |
| float  |  Float | 23.4f, new Float(23.4f)  |
| char  | Character  | 'A', new Character('A')  |
|  boolean | Boolean   | true, new Boolean(true)   |


&nbsp;

&#10140; 변수에 1을 저장하는 방법 2가지
```java
int a = 1;
Integer b = new Integer(1);
int v = b.intValue();
```
int형은 객체/클래스가 아니기 때문에 Object의 하위 클래스가 아니다. Integer를 사용하여 Object클래스의 자식 클래스로 활용이 가능하다.

&nbsp;

&#10140; 기본자료형을 Object[]배열에 저장할 경우
```java
Object[] obj = new Object[3];
obj[0] = 1;
obj[1] = 2;
obj[2] = 3;

Object[] obj = new Object[3];
obj[0] = new Integer(1);
obj[1] = new Integer(2);
obj[2] = new Integer(3);
```

&nbsp;

**컴파일러가 자동으로 Boxing/Unboxing 해준다.**
```java
Integer a = 1; //Boxing
int b = new Integer(10); //unBoxing
//같은 타입이 아니지만 혼용해서 사용이 가능하다.
```

&nbsp;


-----

&nbsp;

###### reference
[인프런 강의 : JAVA TPC](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%9E%85%EB%AC%B8-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D/dashboard)


-----