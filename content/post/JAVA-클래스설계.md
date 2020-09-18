---
title: "JAVA 클래스설계"
date: 2020-09-17T20:39:27+09:00
categories:
- JAVA
- JAVA
tags:
- JAVA
- 클래스설계
- 인강노트
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# 클래스 설계 (Model : DTO, VO)

&nbsp;


### 정보은닉(private)
다른 객체(class)로부터 접근을 막는 것(private).   
setter, getter method를 통해 private 멤버변수에 접근한다.

&nbsp;

### 잘 설계 된 DTO, VO

```java
public class MemberVO {
    //1
    private String name;
    private int age;

    //2
    public MemberVO() {}
    public MemberVO(String name, int age) {
        this.name = name;
        this.age = age;
    }

    //3
    public void setName(String name) {
        this.name = name;
    }
    public String getName() {
        return name;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String getAge() {
        return age;
    }

    //4
    @Override
    public String toString() {
        return "MemberVO [name = " + name + ", age=" + age+ "]";
    }
}

MemberVO m = new MemberVO();
//m.name = "김멤버"; 방식으로 접근할 수 없다
//setter getter메소드도 메모리에 올라간다.

m.setName("김멤버");
m.getName();

```

&nbsp;

&#10102; private으로 객체의 상태를 보호한다. (정보은닉)   
&#10103; 디폴트 생성자를 명시적으로 만든다. 오버로딩 생성자를 만들어 적절하게 초기화 한다.    
&nbsp; &nbsp;  - 객체를 생성하는 작업은 생성자 내부에서 JNM이 자동으로 처리한다.   
&#10104; private으로 만들어진 멤버변수를 접근하기 위해서 setter, getter method를 만든다.   
&nbsp; &nbsp;  - **DI (Dependency Injection :종속객체 주입)** setter, getter의 역할   
&#10105; 객체가 가지고 있는 값 전체를 출력하기 위한 toString() method를 재정의 한다.

&nbsp;

-----