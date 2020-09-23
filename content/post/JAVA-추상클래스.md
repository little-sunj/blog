---
title: "JAVA 추상클래스"
date: 2020-09-23T14:10:17+09:00
categories:
- JAVA
- JAVA기본
tags:
- JAVA
- 추상클래스
- 인강노트
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# 추상클래스 

&nbsp;


### 추상클래스 활용 (일부 다형성 보장)

```java
//추상 클래스(불완전한 객체)
public abstract class Animal {

    //추상 메서드 - 불완전한 메서드 : 구현부가 없다. (객체생성불가)
    //**재정의를 해야한다. -> 자식 메서드에서 반드시 구현해주어야한다.
    public abstract void eat(); 

    public void move(){ //구현메서드
        System.out.println("무리를 지어서 이동한다.");
    }
}


Animal r = new Dog();
r.eat(); //반드시 동작

Animal r = new Cat();
r.eat(); //반드시 동작

```

&nbsp;

-----

&nbsp;

### 인터페이스 활용 (100% 다형성 보장)
100% 추상메서드만 가능. 구현된 메서드를 가질 수 없다.

```java
//인터페이스(불완전한 객체)
public interface Animal {

    //구현된 메서드를 가질 수 없다.
    public abstract void eat(); 
    //하위 클래스의 동작방식을 몰라도 인터페이스로 100%동작 시킬 수 있다.
    
}

public class Cat implements Animal {
    //추상메서드 반드시 구현
    @Override
    public void eat() {

    }
}

public class Dog implements Animal {
    //추상메서드 반드시 구현
    @Override
    public void eat() {
        
    }
}

```


&nbsp;

-----

&nbsp;

### 추상클래스와 인터페이스 (다형성을 보장하기 위함)

- 부모클래스를 추상클래스나 인터페이스로 만들어야 다형성을 보장(재정의를 반드시 해야 됨)할 수 있다.   

- 공통점 : 다형성을 보장하기 위해 등장. 객체를 생성 할 수 없다. 하위클래스에 의해 구현되어야 한다. (Override) 부모(상위클래스)의 역할로 사용한다. (upcasting으로 객체를 생성). 추상메서드를 가진다.   

- abstract : 서로 기능이 비슷한 자식 클래스 사용시   
    서로 기능이 비슷한 클래스의 공통부분을 묶을 때 사용. 구현 메서드와 추상 메서드를 함께 가질 수 있다. 50% 디자인(설계), 50% 구현. extends keyword 사용.   
- interface : 서로 기능이 다른 자식 클래스 사용시   
    서로 기능이 다른 클래스의 공통부분을 묶을 때 사용. 100%추상 메서드로 이루어진다. 100% 디자인(설계), 규약. implements keyword 사용. 다중상속 형태를 지원한다.    final static멤버변수를 가질 수 없다.   

