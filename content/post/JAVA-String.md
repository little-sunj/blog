---
title: "JAVA String"
date: 2020-09-27T19:32:50+09:00
categories:
- language
- java
tags:
- JAVA
- String
- 인강노트
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# String 

&nbsp;


###  String 클래스

1. 자바에서 문자열은 쌍 따옴표 " " 로 감싸면 된다.
2. 자바에서 문자열을 저장하는 기본 자료형(DataType)은 없다.
3. 문자열은 여러가지 조작을 할 수 있기 때문에 별도의 클래스로 자료형 `(java.lang.String)`을 만들어 두었다.   
4. 그래서 자바에서 문자열은 책, 영화, 회원 처럼 **객체**로 취급된다.

&nbsp;

- 문자열 생성방법
```java
//1. new 로 생성
String str1 = new String("APPLE");
String str2 = new String("APPLE");
```
![0927_1](https://user-images.githubusercontent.com/28701069/94363303-4c7dbf00-00fc-11eb-896b-9f4b9abaff62.PNG)
서로 다른 번지수를 가지고 있기 때문에 비교하려면 가리키고 있는 값을 비교해야한다.

&nbsp;

```java
//2. 문자열 상수로 생성
String str3 = "APPLE";
String str4 = "APPLE";
```
![0927_2](https://user-images.githubusercontent.com/28701069/94363304-4d165580-00fc-11eb-82c1-6b731792599f.PNG)

객체기 때문에 문자열 상수(**객체**)가 생성된다.   
Literal Pool 메모리 영역에 생성되기 때문에 재활용이 가능하다.

&nbsp;

-----


-----

&nbsp;

###### reference
[인프런 강의 : JAVA TPC](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%9E%85%EB%AC%B8-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D/dashboard)


-----