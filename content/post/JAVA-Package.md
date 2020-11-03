---
title: "JAVA Package"
date: 2020-09-27T19:05:49+09:00
categories:
- JAVA
- JAVA기본
tags:
- JAVA
- 패키지
- 인강노트
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# Package 

&nbsp;


###  패키지 (package)
기능이 비슷한 클래스를 모아서 쉽게 관리하기위해 /package 외부에서 접근하는 것을 막을 때 사용

- package 외부(outside) 에서 접근하는 방법
```java

//import.kr.tpc.MyClass;
//import.kr.tpc.*;

public class TPC30 {
    public static void main(String[] args) {
        // MyClass my = new MyClass();
        kr.tpc.MyClass my = new kr.tpc.MyClass(); //public권한인 경우에 접근가능

        int v = my.sum(10, 100);
    }
}

package kr.tpc; //패키지 선언문
class MyClass { 
//접근권한 생략시 "public"인것이 아님 default접근 권한을 가진다. 즉 패키지 밖에서는 접근할 수 없다.
//만약 패키지 선언문이 없다면 생략값이 "public"이 된다.
    public int sum(int a, int b) {
        return a+b;
    }
}


/*

- class full name을 알아야 한다. (kr.tpc.MyClass)
- 접근권한을 알아야한다. (public)
- import구문을 이해해야 한다.

*/

```

- 클래스의 이름은 2가지이다.   
&#10140; 기본이름 : MyClass   
&#10140; 패키지를 포함한 이름(class full name) : kr.tpc.MyClass   


- default 접근권한   
package 내부에 있는 클래스에게는 public 접근 권한   
package 외부에 있는 클래스에게는 private 접근 권한   

> package 내부에 있는 클래스의 접근권한이 생략되면 기본적으로 default 접근권한을 가진다. (public 접근권한이 아님)

&nbsp;

-----


&nbsp;


###  자바 API (Java API : 2단계 package로 구성됨)

자바에서 제공해주는 API는 JDK설치시 보통 jrt-fs.jar파일내 있다.   
(클래스 여러개를 jar로 묶어서 배포.)   

```java
/*
[jrt-fs.jar]
    |
    |_ [java] package
            |
            |_ [lang]  String..etc
            |_ [util]  ArrayList..etc
            |_ [io]
            |_ [sql]
            |_ [net]
            ...
*/





//import.java.lang.String;
//import.java.lang.*; -> default패키지기 때문에 생략해도 항상 포함
public class TPC31 {

    public static void main(String[] args) {
       //String str = new String("TEST");
       java.lang.String str = new java.lang.String("TEST");
    }
}

package java.lang;

public class String {
    //문자열 조작 함수들로 구성
    public String toLowerCase() {} 
    public int length() {}
    public char charAt(int pos) {}
}
```

&nbsp;

-----


-----

&nbsp;

###### reference
[인프런 강의 : JAVA TPC](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%9E%85%EB%AC%B8-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D/dashboard)


-----