---
title: "JAVA 동일한구조 vs 이질적인구조"
date: 2020-09-19T21:55:45+09:00
categories:
- language
- java
tags:
- JAVA
- 배열,클래스
- 인강노트
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---


<!--more-->
# 동일한 구조 VS 이질적인 구조

&nbsp;


### 배열 VS 클래스의 관계


- 배열   
동일한 데이터로 이루어진 객체(Object)   
동일한 데이터 구조   

```java
int[] arr = new int[5];
//구조의 이름 : int[]
```

- 클래스   
여러 데이터로 이루어진 객체   
이질적인 데이터 구조   

```java
MovieVO vo = new MovieVO();
//구조의 이름 : MovieVO
```

- Object배열   
객체 배열

```java
MovieVO[] vo = new MovieVO[5];
//구조의 이름 : MovieVO[]
```

&nbsp;

-----

### Class
- DataType측면 : 새로운 자료형을 만드는(설계하는)도구 = **모델링** 도구   
- OOP(객체지향)측면 : 객체의 상태정보와 행위정보를 추출하여 **캡슐화**하는 도구   

&nbsp;

### Model
class를 Model이라도고 부른다. (역할이 정해지므로)

&nbsp;

### Model의 종류 
(3가지는 거의 대부분 만들게 되어있다)   
1. DTO (Data Transter Object) : 데이터 구조, 데이터를 담는 역할, 이동하기 위해서 데이터를 담는다.   
    - VO (Value Object) : 객체를 담아서 하나의 값(덩어리)으로 취급한다는 의미
2. DAO (Data Access Object) : 데이터를 처리하는 역할(비즈니스 로직). DB와 CRUD하는 역할
3. Utility (Helper Object) : 도움을 주는 기능을 제공하는 역할(날짜, 시간, 통화, 인코딩 등)


-----

&nbsp;

###### reference
[인프런 강의 : JAVA TPC](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%EC%9E%85%EB%AC%B8-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D/dashboard)


-----