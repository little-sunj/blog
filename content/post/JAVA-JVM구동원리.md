---
title: "JAVA JVM구동원리"
date: 2020-09-07T17:36:15+09:00
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


#### 프로그래밍의 3대 요소 : 변수(Variable), 자료형(Datatype), 할당(Assign)   

- **변수** : 기억공간. 데이터를 저장할 메모리 공간의 이름(symbol)   
            변수 생성을 위해선 아래 두가지 조건이 필요하다.   
                1. 크기   
                2. 어떤 종류의 데이터를 저장할 것인가  

- **자료형** : 변수의 **크기**와 변수에 저장될 데이터의 **종류**(dataType)를 결정하는 것.   

- **할당** : 변수에 값을 저장(대입, 할당)하는 것.   


&nbsp;

#### 기본자료형(PDT) 
컴파일러에서 기본적으로 제공해주는 자료형   
*정해져 있다.   
`int a =10;` &#10140; 메모리 : `a [10]`

&nbsp;

#### 사용자정의자료형(UDDT) - 객체 자료형(Object dataType)
필요에 의해서 새롭게 만들어 사용하는 자료형.    
만드는 도구, 설계하는 도구, 모델링하는 도구가 필요하다. = **class**   
ex) memberVO, bookDTO, String   
객체(변수) `Book b;` &#10140; `b [번지]` 메모리 : `[번지수에 담긴 VO]`    
해당 번지로 저장된 값 메모리에서 찾아온다.   
cf) VO - Value Object / DTO - data transfer object   

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

------

#### 변수와 배열

- **Array(배열)** : 객체. 동일타입 데이터를 여러개 저장하기 위한 연속적인 메모리 구조   
1. 많은 수의 변수를 만들기가 용이하다.
2. 기억공간 접근이 쉽다. (반복문)
3. 데이터 이동이 쉽다. (데이터를 하나의 형태로 담아서 이동)
4. [단점] 서로 다른 데이터타입 (이질적구조, 객체)를 저장할 수 없다.

**2차원 배열** : 1차원 배열이 여러개로 된 형태
cf) a라는 아파트에 3개(a[0], a[1], a[2])동이 있고 각 동은 3층이다.

-----

#### 메서드와 변수의 공통점


- 변수와 메서드는 둘 다 1개의 값을 가진다.
- 변수 앞에 데이터 타입을 선언하듯, 메서드명 앞에 데이터 타입 선언

-----


#### 메서드의 매개변수 전달기법

parameter를 어떻게 전달하느냐에 따라 값전달/번지전달 기법이 나뉜다.


- Call by Value (값 전달 기법) : 기억공간 개별
```
  int a = 10;
  int b = 20;
  int v = sum(a, b);  // 메서드 호출부
   //호출부에 들어가는 인수값들을 '실인수'라고 하며
 public int sum (int a, int b) { // 받는 메소드의 파라미터를 '매개변수' 또는 '가인수' 라고한다.
 }
```
 - 메서드가 호출되려면 '실인수'와 '가인수'의 개수와 데이터타입이 같아야한다.



- Call by Reference (번지 전달 기법) : 기억공간 공유
```
 int[] arr = {10, 20, 30}; // 중괄호 부분 '{ }'를 초기화 리스트 라고한다. 생성과 동시에 초기화.
```

 **arr은 객체 = 번지가 저장되어있다. 함수가 호출되면 번지가 전달된다.**   
 즉, 메소드에서 받을떄 같은 번지를 참조하게 되어 같은 기억공간을 공유하게 된다.   


&nbsp;

 -----

&nbsp;

## JVM Memoery Model


&nbsp;

#### JVM이 class를 실행하는 절차

1. 해당 클래스를 현재 디렉토리에서 찾는다.
    - 성공 : 해당 클래스를 찾음
    - 실패 : 해당 클래스를 찾지 못함. 환경변수 classpath변수에 디렉토리를 잡아줄 수 있다. 
2. 찾으면 클래스 내부에 있는 static키워드가 있는 메서드를 메모리로 로딩한다.
    - 프로그램이 시작하기 전에 로딩된다.
    - method Area의 static zone에 로딩한다. main() method
3. static zone에서 main()메소드를 실행한다. (호출, 시작)
    - main() method가 호출되면 호출정보가 stack Area에 들어간다. (push)
    - 프로그램이 시작되는 부분이다. 
    - PC[프로그램카운터]의 위치가 현재 동작되고 있는 메서드다.
4. stack Area가 비어 있으면 프로그램이 종료된 것이다.

&nbsp;

- 예시
```java

public class TPC08 {

    public static void main(String[] args) {
        int a = 10;
        int b = 20;
        TPC08 tpc = new TPC08();
        int v = tpc.add(a, b); //add() call
        System.out.println(v);
    }

    public int add(int a, int b) {
        int sum = a + b;
        return sum;
    }

}

```
위 코드는 아래와 같이 설명된다.

[![jvm](https://user-images.githubusercontent.com/28701069/93017232-086dc300-f602-11ea-86ab-95b75e6a316e.PNG)](https://user-images.githubusercontent.com/28701069/93017232-086dc300-f602-11ea-86ab-95b75e6a316e.PNG)

Stack Area : LIFO (Last In, First Out)