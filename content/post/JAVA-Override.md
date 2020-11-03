---
title: "JAVA Override"
date: 2020-09-20T12:06:47+09:00
categories:
- JAVA
- JAVA기본
tags:
- JAVA
- 재정의
- 인강노트
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# 재정의 (Override) 

&nbsp;


### Override
상속관계에서 상속받은 하위 클래스가 상위 클래스의 동작을 수정하는 것.   



```java

public class Animal {
    public void eat() { System.out.println("?"); } //포괄적, 추상적
}

//Override

public class Dog extends Animal {
    public void eat() { System.out.println("개처럼 먹다."); }
}
public class Cat extends Animal {
    public void eat() { System.out.println("고양이처럼 먹다."); }
}
```

&nbsp;

-----

&nbsp;


#### Override 객체 생성방법 2가지 
- Type 1
```java
Dog d = new Dog();
d.eat();
```
![1](https://user-images.githubusercontent.com/28701069/93693685-06de5680-fb3e-11ea-85ff-c061005a1e1b.PNG)

`d.eat();`에서 `eat`는 자식의(`Dog`)의 `eat()`를 가리키게 된다.   
메모리에 부모와 자식 메서드가 **공존**하지만 자식 메서드가 실행된다.   
자식클래스의 내용을 알고있는 경우 사용.

&nbsp;

&nbsp;

- Type 2
```java
//Upcasting(자동형변환) 방법. 부모자식간 자동으로 형이 바뀐다. (반대는 downcasting)
Animal d = new Dog();
d.eat();
```

![image](https://user-images.githubusercontent.com/28701069/93693829-f0d19580-fb3f-11ea-83ea-a4d37f944d31.png)

`d.eat();`에서 부모의(`Animal`)로 접근한다.(컴파일 시점에선 부모의 eat).   
실행시 `eat()`가 재정의 되어있는지 체크하고 자식의 `eat()`를 찾아 실행하게 된다. (동적바인딩)   
실행시 접근이 되기 때문에 d.eat()가 부모의 eat일지, 자식의 eat일지 실행 전에는 알 수 없다.   
본래로라면 메모리상 접근이 되지 않겠지만 Override했기 때문에 접근이 가능해진다.   
자식클래스의 내용을 모르고 있는 경우에도 사용이 가능하다는 장점이 있다. (ex. class(실행)파일은 있는데 java소스 내용을 모른다거나..)

&nbsp;

- **동적 바인딩** : 호출될 메서드가 실행시점에 결정되는 바인딩   
프로그램의 속도가 떨어지는 원인이 되지만 이점이 더 많기 때문에 사용된다.   

> 하위 클래스의 동작 내용을 모르고도 부모 클래스 Override를 통해 하위 클래스에 접근 할 수 있다.

&nbsp;

-----

&nbsp;


#### Object Casting (객체 형 변환)

상속관계에 있는 클래스들 간의 형(DataType)을 바꾸는 것

```java
//Upcasting (자동형변환)
Animal a = new Dog();
        a = new Cat();


//Downcasting (강제형변환)
Dog d = (Dog)a;
Cat c = (Cat)a;
```



-----

&nbsp;

###### reference
[인프런 강의 : JAVA TPC](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%9E%85%EB%AC%B8-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D/dashboard)


-----