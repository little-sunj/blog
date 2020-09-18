---
title: "JAVA Method Overloading"
date: 2020-09-18T21:15:32+09:00
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
# 메서드 오버로딩 (Method Overloading)

&nbsp;


### Method Overloading
같은 이름의 메소드를 여러 개 가지면서 매개변수의 유형과 개수가 다르도록 하는 기술.   
> 메서드의 signature가 다르면 된다 (signature : 매개변수의 타입, 개수)

&nbsp;

- **정적 바인딩(컴파일 시점에서 호출될 메서드가 이미 결정되어 있는 바인딩) &#10140; 속도와는 관계 없다.**

&nbsp;

```java

public class OverLoad() {

    public void hap(int a, int b) {
        System.out.println(a+b);
    }
    public void hap(float a, int b) {
        System.out.println(a+b);
    }
    public void hap(float a, float b) {
        System.out.println(a+b);
    }
}

//중복해서 선언하더라도
//컴파일러가 처음부터 메소드명을 다르게 해서 생성해둔다.
//그래서 동일한 메소드명으로 찾거나 하는 작업 없이 호출시점에 이미 결정되어 있다.
public class Test {
    public static void main(String[] args) {
        OverLoad ov = new OverLoad();
        //1 
        ov.hap(34.6f, 46);
        //컴파일러가 만들어내는 메서드 이름
        //hap_int_int(int a, int b)

        //2
        ov.hap(34, 62);
        //컴파일러가 만들어내는 메서드 이름
        //hap_float_int(float a, int b)

        //3
        ov.hap(74.6f, 26.6f);
        //컴파일러가 만들어내는 메서드 이름
        //hap_float_float(float a, float b)
    }
}


```

