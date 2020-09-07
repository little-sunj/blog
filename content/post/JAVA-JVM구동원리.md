---
title: "JAVA JVM구동원리"
date: 2020-09-07T17:36:15+09:00
categories:
- JAVA
- JVM
tags:
- JAVA
- JVM
keywords:
- 인강노트

---

<!--more-->
# JVM




&nbsp;


- **JAVA - 운영체제 독립적.**

```
bin 폴더:   .class 실행을 위한 파일
            (바로 실행되는 파일은 아니다. 이를 운영체제에 맞게 실행해주는 것이 JVM) 
            byte code
src 폴더:   .java 소스파일
```
&nbsp;

&#10102; `.java`    
&#10103; `javac.exe` / **1차 컴파일** &#10140; byte code (OS독립적인 형태)   
&#10104; `.class`  `java.exe` : jvm통해 실행   
&#10105; 실행엔진 JVM에서 byte code를 각 OS에 맞게 **2차 컴파일** &#10140; exe code   

&nbsp;


-----


#### 프로그래밍의 3대 요소 : 변수, 자료형(datatype), 할당(=)   

- **변수** : 기억공간. 데이터를 저장할 메모리 공간의 이름(symbol)   
            변수 생성을 위해선 아래 두가지 조건이 필요하다.   
                1. 크기   
                2. 어떤 종류의 데이터를 저장할 것인가  

- **자료형** : 변수의 **크기**와 변수에 저장될 데이터의 **종류**(dataType)를 결정하는 것.   

- **할당** : 변수에 값을 저장(대입, 할당)하는 것.   


&nbsp;

#### 기본자료형(PDT) 
컴파일러에서 기본적으로 제공해주는 자료형

&nbsp;

#### 사용자정의자료형(UDDT) - 객체 자료형(Object dataType)
필요에 의해서 새롭게 만들어 사용하는 자료형.    
만드는 도구, 설계하는 도구, 모델링하는 도구가 필요하다. = **class**   
ex) memberVO, bookDTO, String

&nbsp;

#### 변수선언 
메모리에 기억공간을 만드는 것   
변수가 선언되면 ST(변수테이블)에 등록된다.

> ST테이블을 거쳐서 메모리에 접근.   
> ST테이블에 변수가 없을 시 'can not find symbol' ERROR

&nbsp;

#### Symbol Table(변수목록표)
변수가 기억공간을 할당받으면 변수의 번지가 등록되는 테이블   


| 변수이름(key) | 값(value) |
|--|--|
| a | 100 |
| b | 200 |

&nbsp;

#### Memory
| Address |  | 값 |
|--|--|--|
| 100번지 | a | 10 |
| 200번지 | b | 80 |



&nbsp;