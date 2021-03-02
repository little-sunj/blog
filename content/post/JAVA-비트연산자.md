---
title: "JAVA 비트연산자"
date: 2021-03-02T20:25:58+09:00
categories:
- language
- java
tags:
- JAVA
- 비트연산자
keywords:
- JAVA
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# 비트연산자

&nbsp;

- 10진수를 2진수로 변환

```java

//1. 숫자를 2진수 String으로 반환 
String x = Integer.toBinaryString(33);
System.out.println(" x : " + x); // x : 100001

/*
8진수는 toOctalString
16진수는 toHexString
*/

//2. 2진수 string을 10진수 숫자로 반환
int y = Integer.valueOf(x, 2);
System.out.println(" y : " + y); // y : 33

/*
8진수는 Integer.valueOf(x, 8);
16진수는 Integer.valueOf(x, 16);
*/


```


&nbsp;

- 비트 연산식

| 연산식 |  |
|--|--|
| `~x` | 정수 x의 비트값을 0을 1로 1을 0으로 반전시켜 반환  |
| `x&y` | 정수 x와 y의 비트값을 비교해서 같은 자리의 비트가 모두 1일때 1을 반환  |
| `x^y` | 정수 x와 y의 비트값을 비교해서 같은 자리의 비트가 다를때 1을 반환 |
| `x<<y` | 정수 x의 각 비트를(이진값) y만큼 왼쪽으로 이동 (빈자리 0으로 채움) |
| `x>>y` | 정수 x의 각 비트를(이진값) y만큼 오른쪽으로 이동 |
| `x>>>y` | 정수 x의 각 비트를(이진값) y만큼 오른쪽으로 이동 (빈자리 0으로 채움) |

&nbsp;

- 예제코드 (작성중)

```java

// ~x



```

&nbsp;


-----

- reference

https://coding-factory.tistory.com/521
