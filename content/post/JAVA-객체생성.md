---
title: "JAVA 객체생성"
date: 2020-09-15T18:55:32+09:00
categories:
- JAVA
- JVM
tags:
- JAVA
- JVM
- 인강노트
keywords:
- 인강노트
---

<!--more-->
# 객체생성

&nbsp;

### 기본자료형(PDT) VS 사용자정의자료형(UDDT)

- '기본자료형' : 컴파일러에서 기본적으로 제공해주는 자료형   
- '사용자정의자료형' : 사용자가 직접 만들어서 사용하는 자료형   
    - class로 새로운 자료형을 만든다.

&nbsp;

-----

### 객체생성과정
```java
BookDTO book = new BookDTO();//new 객체 생성 명령어.

public BookDTO() {super();}
//생성 메서드 default constructor(기본생성자) -> 메모리에 만드는 역할
//기본생성자 : 객체를 메모리에 생성하도록 내부적으로 설계가 되어있다. (기계어코드)

//stack Area
// book (heap Area BookDTO타입의 번지를 가리킨다.)
// this (자기 자신을 가르키는 객체. heap Area BookDTO타입의 번지를 가리킨다.)

//heap Area 메모리에 생성 -> 객체생성과정
// bookDTO [ title / price / company/ page ]

```
&nbsp;

1. **상태정보(변수)** : attribute, property, member   
    제목, 출판사, ~~ISBN~~, 저자, 가격, 페이지수, ~~두께~~...   
        &nbsp;&nbsp;    &#129047;   
    `Modeling` : 필요한 속성만 뽑아내는 과정   
        &nbsp;&nbsp;    &#129047;   
    제목, 출판사, 저자, 가격, 페이지수   

&nbsp;

2. **행위정보** : 동작(method), 기능(function)   

&nbsp;

```java

public class BookDTO {
    public String title;
    public int price;
    public String company;
    public int page;
}

BookDTO book = new BookDTO(); // --> 객체생성

//생성 후 객체 접근
book.title
book.price
book.company
book.page
/*
.(dot)연산자 : 접근/참조연산자

객체의 상태정보를 직접 접근하면 잘못된 데이터가 저장될 수 있다.
설계를 잘 하는 것이 중요하다.
외부에서 해당 객체에 마음대로 접근하지 못하도록 하기 위해선 정보은닉이 필요하다. 
public -> private으로 선언
*/


```

&nbsp;

-----

### 생성자 메서드 (Constructor)

1. 객체를 **생성**할 때 사용되는 메서드
2. 객체 생성 후 객체의 **초기화**를 하는 역할 수행
3. 특징
    - 클래스이름과 동일한 메서드
    - 메서드의 return type이 없다 (void가 아님)
    - public접근 권한을 가진다. (private생성자도 있다.)
    - 생성자가 없을 때는 기본 생성자가 만들어진다.

&nbsp;

#### 생성자 중복정의
> 생성자 메서드를 활용하여 객체를 적절하게 초기화하라. 중복정의(Overloading)

기본생성자는 초기화 작업이 없다.

```java

public class BookVO() {
    private String title;
    private int price;
    private String company;
    private int page;

    public BookVO() {super();} //default생성자만 가지고는 초기화를 하지 못한다.
}
BookVO b = new BookVO();


public class BookVO() {
    private String title;
    private int price;
    private String company;
    private int page;

   //초기화
   //Overloading constructor :초기화를 위해서 (중복 정의된 생성자)
    public BookVO(String title, int price, String company, int page) {
        this.title=title;
        this.price=price;
        this.company=company;
        this.page=page;
    }
}
BookVO b = new BOOKVO("자바", 20000, "출판사A", 790); //초기값
```

&nbsp;

#### private 생성자 메서드(Private Constructor)


> 객체생성에 관여하는 생성자 메서드가 private접근제어를 가지면 **객체를 생성할 수 없다**는 뜻이 된다.   
> 그러므로 **객체를 생성하지 않고도 사용가능**해야 된다. (클래스의 모든 멤버가 **static 멤버**가 되어야 한다.)


```java

public class Book {
    private Book() {
        //클래스 내 모든 멤버가 static멤버이면 
        //인위적으로 private생성자로 만들어 객체생성을 막을 수도 있다.
        //어떤 객체에 생성자가 private이면 **모든**멤버는 static이 붙어야한다.
    }
    
    //인스턴스 메서드 (static키워드가 붙지 않은 메서드)
    //new를 통해 객체생성이 불가능하기 때문에 이 메소드는 호출 불가. (static을 붙여주어야 정상)
    //생성자가 private이기 때문에 객체생성이 되지 않으므로 메모리에 올라가있지 않다.
    public void title() {  
    }
    //클래스 메서드
    //**클래스를 사용하는 시점에서 static멤버는 자동으로 메모리 로딩**
    // 메모리에 로딩된 이후에 호출이 되게된다.
    public static void java() {
    }
}
```

- non-static멤버인 경우 (인스턴스 메서드 : 객체생성 후에 접근 가능한 메서드) 
    - 객체생성 후 접근 가능

```java
BookVO b = new BookVO();
b.title();
```

- static 멤버인 경우 (클래스 메서드)
    - 객체생성 없이 접근가능(클래스 이름으로 접근)    
```java
Book.java();
```

> 자주 사용하는 객체나 동작은 static을 사용한다.
ex) 자바 API의 SYSTEM.  MATH. 등등..

&nbsp;

-----

### class, object, instance 상호관계

> class, object, instance = 객체

&nbsp;

- class ( 객체설계 모델링 / 설계도 / 자료구조 )
```java
pubic class BookDTO {
    private String title;
    private int price;
    private String company;
    private int page;
}
```

&#129047;

- object ( 객체 변수 )
```java
BookDTO b1;
BookDTO b2;
BookDTO b3;

//추상적. 아직 객체 생성이 되지 않아 객체를 가리키는 번지가 없다면 b1, b2, b3는 구별할 수 없다.
```

&#129047;

- instance ( 인스턴스 변수 )
```java
b = new BookDTO(); //memory Heap Area
//객체가 생성이 되고 메모리에 담김. 실체가 생긴다.
//b가 구체적인 번지를 담는다. b1, b2, b3가 구분이 가능해진다.
```

> object &#10140; instance : 인스턴스화   
> 인스턴스가 만들어져야 데이터를 넣고 뺄수 있다.   
> `BookDTO b = new BookDTO();`는 정확히는 '인스턴스 생성하는 과정' 인 것이다.   

&nbsp;

-----