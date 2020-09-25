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


&nbsp;

-----

&nbsp;


### Object class, toString()


#### Object Class
- 모든 클래스의 root클래스
    - 최상위 클래스(상속관계에서)
    - Object클래스를 잘 활용하면 프로그램을 유연하게 만들 수 있다.


- 클래스 설계시 생략된 코드(3곳*)

```java
import java.lang.*;  //1

public class A extends Object {//2
    public A() {super();} //3

    public void display() {}

    @Override
    public String toString() {} /*object 내 toString() override*/
}
```

- Object class를 이용한 객체 생성
```java

Object o = new A();
A a = (A)o;
a.display(); // ((A)o).display();
```

- toString()
1. 재정의를 안 했을 경우 (번지출력)
2. 재정의를 했을 경우 (재정의된 메서드 실행)

&nbsp;

-----

&nbsp;


### Object class활용 (다형성 인수, 다형성 배열)

- 다형성 인수
```java
public class ObjectTest {

    public static void main (String args) {
        display(new A());
        display(new B());
    }
    public void display(Object o) { //다형성 인수
        if (o instanceof A) {
            ((A)o).go();
        } else {
            ((B)o).go();
        }

    }

}
```


- 다형성 배열   
서로 다른 객체를 (A, B) 배열에 저장할시 부모객체로 Object활용
```java
public class ObjectTest {
    public static void main(String args) {
        OBject[] o = new Object[2];
        o[0] = new A();
        o[1] = new B();

        for (int i = 0; i < o.length; i++) {
            if (o[i] instanceof A) {
                ((A)o[i]).go();
            } else {
                ((B)o[i]).go();
            }
        }
    }
}
```


&nbsp;

-----