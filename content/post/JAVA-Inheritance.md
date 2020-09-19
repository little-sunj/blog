---
title: "JAVA Inheritance"
date: 2020-09-19T22:41:28+09:00
categories:
- JAVA
- JAVA기본
tags:
- JAVA
- 상속
- 인강노트
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# 상속 (Inheritance) (수평적 구조 VS 수직적 구조)

&nbsp;


### 상속 &#10140; 클래스의 설계 (행위적 측면)


- 수평적 설계   
    - 코드의 중복이 발생   
    - 새로운 요구사항에 대한 코드의 수정이 불가피   
    - 관리가 어렵다 (부모 자식 관계)   

- 수직적 설계 (계층화, 상속구조)   
    - 수평적 설계의 단점 극복   
    - 확장이 쉽다   
    - 코드가 복잡해진다. (이점이 많다)   

&nbsp;

-----

&nbsp;

### 상속 개념
자식이 생성되기 전에 부모를 먼저 생성   
부모부터 자식까지 차례로 객체가 생성된다. 


```java

//상속 Memory
//상속관계는 메모리적으로 이해할 것
/*

------------
|  Object   |  <<extends  상속 체이닝
------------      |
|  Animal   |  ___|  
------------      |
| Dog | Cat |  ___|
-------------
*/

//상속에서 부모와 자식에 연결되는 방법 -> 생성자
//super() : 자신의 생성자에서 부모의 생성자를 호출

public class Animal extends Object {
    public String name;
    public String part;
    
    public Animal() {
        super();       //부모의 new Object(); 호출
    } 
    public void eat() {} //공통
}

public class Dog extends Animal {
    public Dog() {
        super();    //부모의 new Animal(); 호출
    }
    public void eat() {} //공통
}

public class Cat extends Animal {
    public Cat() {
        super();    //부모의 new Animal(); 호출
    }
    public void eat() {} //공통
}


```