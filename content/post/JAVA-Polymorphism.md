---
title: "JAVA Polymorphism"
date: 2020-09-22T19:56:45+09:00
categories:
- language
- java
tags:
- JAVA
- 다형성
- 인강노트
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# 다형성 

&nbsp;


### message polymorphism
상성관계에 있는 클래스에서 상위클래스가 동일한 메시지로 하위 클래스들을 서로 다르게 동작시키는 객체지향 원리(개념)

```java
Animal r = new Dog();
r.eat();
r = new Cat();
r.eat();

//부모가 가지지 않은 method를 사용할시
Animal r = new Cat();
r.night(); //--> (X) night()는 재정의가 되지 않아서 접근불가. 사용할 수 없다

Cat c = (Cat)r; //downcasting
c.night(); //이렇게 다운캐스팅하여 하위클래스의 메소드를 사용 가능하다.

```

- 다형성   
상위클래스인 `Animal`이 하위클래스인 `Dog`와 `Cat`에게 동일하게 먹어라(eat)라고 메시지를 보냈을 때 하위 클래스인 Dog와 Cat의 `eat()`메서드가 **서로 다르게 동작되는 객체지향 원리**

&nbsp;

-----

&nbsp;


### 다형성 이론의 전제조건 (부모 클래스를 잘 활용하라)

- 상속관계가 되어야 한다.   
- 객체생성을 upcasting으로 할 것(상위클래스가 하위클래스에게 메세지를 보내야 하므로)   
    - upcasting이 되면 downcasting을 할 수 없다   
- 하위 클래스가 반드시 재정의(Override)해야 한다. (다형성이 보장되기 위해선)   
- 동적 바인딩을 통해 실현된다.
    - 동적 바인딩 : 실행시점에서 사용될 메서드가 결정되는 바인딩. 프로그램의 속도를 떨어뜨리는 원인이 된다.

&nbsp;

-----

&nbsp;


### 다형성활용 방법
부모 클래스를 잘 활용하라

&nbsp;
 
1. 다형성인수(데이터이동)
```java
Dog d = new Dong();
display(d);
Cat c = new Cat();
display(c);

//다형성 인수 사용 (Animal의 모든 하위클래스 받을 수 있게된다.)
public static void display(Animal r) { 
    r.eat();
}
```

2. 다형성배열 (서로 다른 객체를 담을 수 있다.)
```java
Animal[] r = new Animal[2]; //서로 다른 하위클래스를 담을 수 있다.
r[0] = new Dog();
r[1] = new Cat();

r[0].eat();
r[1].eat();
```


-----

&nbsp;

###### reference
[인프런 강의 : JAVA TPC](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%9E%85%EB%AC%B8-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D/dashboard)


-----