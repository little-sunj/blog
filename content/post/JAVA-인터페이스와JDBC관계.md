---
title: "JAVA 인터페이스와JDBC관계"
date: 2020-09-24T19:13:09+09:00
categories:
- JAVA
- JAVA기본
tags:
- JAVA
- 인터페이스와 JDBC
- 인강노트
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# 인터페이스와 JDBC의 관계 

&nbsp;


### 인터페이스(규약)와 JDBC의 관계

Java에서 JDBC프로그래밍을 하기 위해서는 벤더에서 제공하는 클래스를 이용해야한다.   
> 벤더에서 제공하는 클래스가 통일되어 있지 않다면, 자바 개발자들은 모든 데이터베이스의 동작을 알고 있어야 JDBC 프로그래밍이 가능하다는 의미이다.  &#10140; 사실상 불가능하다.

> 벤더별로 데이터베이스 접속방법, CRUD 동작방법 등이 다르다.

```java

/*JDBC 프로그래밍*/
//DB벤더 (공급자)

//oracle
class JavaOracle {
    //접속동작
    oracleConnect() {}
}

//my-SQL
class JavaMySQL {
    //접속동작
    mysqlConnect() {}
}

//oracle
class JavaMsSQL {
    //접속동작
    mssqlConnect() {}
}
```


[![jdbc](https://user-images.githubusercontent.com/28701069/94134165-9d8f7800-fe9c-11ea-8103-2d5b3133321b.PNG)](https://user-images.githubusercontent.com/28701069/94134165-9d8f7800-fe9c-11ea-8103-2d5b3133321b.PNG)

&nbsp;

-----

&nbsp;


### 인터페이스와 인터페이스의 상속관계

```java

public interface A {
    public void m();
}

public interface B extends A {
    public void z();
}

//반드시 override를 해야한다. (다형성이 보장된다.)

public class X implements B {
    public void m() {}
    public void z() {}
}


//객체생성법
B r = new X();
A r = new X();

```

- 다중상속관계에 있는 클래스 구조

```java

public class Dog extends Animal implements Pet, Robots {}


/*

abstract       interface        interface
------------  --------------  ---------------
|  Animal  |  |  <<Pet>>   |  | <<Robots>>  |
-----^------  ------^-------  ------^--------
     |--------------|---------------|
                    |
                ---------
                |  Dog  |
                ---------
*/


```