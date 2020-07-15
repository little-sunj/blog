---
title: "SQLD 04 FUNCTION"
date: 2020-06-29T20:45:41+09:00
categories:
- SQLD
- 2과목 SQL 기본 및 활용
tags:
- SQLD
keywords:
- SQLD

---

<!--more-->

# SQLD 04. FUNCTION

### 1.내장 함수 개요(BUILT-IN FUNCTION)

> 단일행 함수 : 함수의 입력 값이 단일행 값이 입력됨
> 다중행 함수 : 여러 행의 값이 입력됨 (집계 함수 / 그룹 함수 / 윈도우 함수)

<br/>
<br/>

- 단일행 함수

함수는 입력되는 값이 아무리 많아도 출력은 하나만 된다는 M:1 관계라는 특징을 가지고있다. 단일행 함수의 경우 단일행 내에 있는 하나의 값 또는 여러 값이 입력 인수로 표현될 수 있다. 
```SQL
함수명 (칼럼이나 표현식 [, Arg1, Arg2, ...])
```
<br/>

![img1 daumcdn](https://user-images.githubusercontent.com/28701069/86002113-dc06c980-ba4a-11ea-88be-27bfab4db38a.jpg)

<br/>

- 단일행 함수 주요 특징
>  - SELECT, WHERE, ORDER BY 절에 사용 가능하다.
>  - 각 행에 대해 개별적으로 작용하여 데이터 값들을 조작하고, 각각의 행에 대한 조작 결과를 리턴한다.
>  - 여러 인자를 입력해도 단 하나의 결과만 리턴한다.
>   - 함수의 인자로 상수, 변수, 표현식이 사용 가능하고, 하나의 인수를 가지는 경우도 있지만 여러개의 인수를 가질 수도 있다.
>   - 특별한 경우가 아니면 함수의 인자로 함수를 사용하는 함수의 중첩이 가능하다.


--------

### 2.문자형 함수

문자 데이터를 매개 변수로 받아들여서 문자나 숫자 값의 결과를 돌려준다.

![img2](https://user-images.githubusercontent.com/28701069/86367958-98fd5e00-bcb7-11ea-817f-03f80e090ad1.png)

```sql
SELECT LENGTH('SQL Expert')
FROM   DUAL;

--result
10
```
오라클은 **SELECT절과 FROM절 2 개의 절을 SELECT문장의 필수 절로 지정**하였으므로 사용자 테이블이 필요 없는 SQL문장의 경우에도 필수적으로 DUAL이라는 테이블을 FROM절에 지정한다.

> ### DUAL 테이블
> - 사용자 SYS가 소유하며 모든 사용자가 액세스 가능
>  - SELECT ~ FROM ~ 의 형식을 갖추기 위한 일종의 DUMMY테이블
>  - DUMMY라는 문자열 유형의 칼럼에 'X'라는 값이 들어 있는 행을 1건 포함.
>  ```sql
>  SELECT * FROM DUAL;
>  
>  --result
>  DUMMY
>  ------
>  X
>  ```

함수는 여러 개 중첩하여 사용이 가능하다.   
함수 내부에 다른 함수를 사용하며 안쪽에 위치해 있는 함수부터 실행된다.   

--------

### 3.숫자형 함수

숫자 데이터를 입력받아 처리하고 숫자를 리턴하는 함수이다.

![4](https://user-images.githubusercontent.com/28701069/86370554-d9121000-bcba-11ea-8c8d-b2d4053e9af5.jpg)

--------

### 4.날짜형 함수

DATE타입의 값을 연산하는 함수.

![SQL_186](https://user-images.githubusercontent.com/28701069/86371280-aa486980-bcbb-11ea-80b3-953e9c88ccc3.jpg)

>Date변수가 DB에 어떻게 저장되는지 살펴보면, 내부적으로   
>세기(Century), 년(year), 월(month), 일(day), 시(hours), 분(minutes), 초(seconds)와 같은 숫자형식으로 변환하여 저장한다.   
>
>*여러 형식으로 출력되고, 날짜 계산에도 사용되기 때문에 편리성을 위해 숫자형으로 저장하는 것.*

 ![SQL_187](https://user-images.githubusercontent.com/28701069/86372082-b254d900-bcbc-11ea-8e7c-675c6901caee.jpg)


--------

### 5.변환형 함수

특정 데이터 타입을 다양한 형식으로 출력하고 싶을 경우에 사용.

- 명시적(Explicit) 데이터 유형 변환 : 데이터 변환형 함수로 데이터 유형을 변환하도록 명시해 주는 경우
- 암시적(Implicit) 데이터 유형 변환 : 데이터베이스가 자동으로 데이터 유형을 변환하여 계산하는 경우

암시적 데이터 유형 변환의 경우 성능 저하 발생 가능.  DB가 자동으로 계산하지 않는 경우가 있어 에러 발생가능   
&#10140; 명시적 방법을 사용하는 것이 바람직하다.

![SQL_189](https://user-images.githubusercontent.com/28701069/86372692-7ff7ab80-bcbd-11ea-9468-c702161b8ca0.jpg)


--------

### 6.CASE 표현

IF-THEN-ELSE 논리와 유사한 방식으로 표현식을 작성해서 SQL의 비교 연산 기능을 보완하는 역할을 한다. 함수와 같은 성격.    
CASE표현을 하기 위해서는 조건절을 표현하는 두 가지 방법이 있고, 오라클의 경우 DECODE 함수를 사용할 수 있다.
   
- 함수의 성질을 가지고 있으므로, 다른 함수처럼 중첩해서 사용할 수 있다.
   


- 단일행 CASE표현의 종류
![img1111](https://user-images.githubusercontent.com/28701069/86373677-9ce0ae80-bcbe-11ea-96da-d9c95272290a.png)



--------

### 7.NULL 관련 함수

>**NVL/ISNULL 함수**
> - 아직 정의되지 않은 값이다. 공백/0 이 아니다.
> - 결과값을 NULL이 아닌 다른 값을 얻고자 할 때 NVL/ISNULL함수를 사용한다. (대상이 숫자인경우 주로 0, 문자인 경우 x와 같이 해당 시스템에서 의미 없는 문자로 바꾸어주는 경우가 많다.)
> - 널 값을 포함하는 연산의 경우 결과값도 NULL이다. 


![SQL_192](https://user-images.githubusercontent.com/28701069/86530922-f54ec080-bef7-11ea-8dd8-2637d822528f.jpg)


```sql
oracle
NVL(NULL판단대상, 'NULL일때 대체값')
sql server
ISNULL(NULL판단대상, 'NULL일때 대체값')
```

>**NULL과 공집합**
>- 조건에 맞는 데이터가 한 건도 없는 경우를 '공집합'이라 하고 NULL 데이터와는 또 다르게 이해해야 한다.
> - NVL/NULL 함수는 NULL값을 다른 값으로 바꾸는 함수이지 공집합을 대상으로 하지 않는다.
> - 인수의 값이 공집합인 경우는 NVL/ISNULL함수를 사용해도 역시 공집합이 출력된다.
> - 적절한 집계 함수를 찾아서 NVL함수대신 적용할 수 있다.
> ```sql
> SELECT MAX(MGR) MGR FROM EMP WHERE ENAME='JSC';
> --다른 함수와 달리 집계 함수와 Scalar subquery의 경우는 인수의 결과 값이 공집합인 경우에도 NULL을 출력한다.   
> 
> SELECT NVL(MAX(MGR), 9999) MGR FROM EMP WHERE ENAME='JSC';
> ```
> 

>**NULLIF**
>특정값을 NULL로 대체해야하는 경우에 사용

>**COALESCE**
>인수의 숫자가 한정되어 있지 않으며, 임의의 개수 EXPR에서 NULL이 아닌 최초의 EXPR을 나타낸다. 만일 모든 EXPR이 NULL이면 NULL을 리턴한다.
>```sql
>COALESCE (EXPR1, EXPR2, ...)
> 
> 아래 두 쿼리는 결과가 같다.
> SELECT ENAME, COMM, SAL, COALESCE(COMM, SAL) COAL
> FROM EMP;
>
>SELECT ENAME, COMM, SAL,
>				CASE WHEN COMM IS NOT NULL
>				THEN COMM
>				ELSE (CASE WHEN SAL IS NOT NULL
>							THEN SAL
>							ELSE NULL
>						END)
>				END COAL
>FROM EMP;
>```