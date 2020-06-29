---
title: "SQLD DML / TCL / WHERE절"
date: 2020-06-22T21:59:51+09:00
categories:
- SQLD
- 2과목 SQL 기본 및 활용
tags:
- SQLD
keywords:
- SQLD

---

<!--more-->

# SQLD 03. DML / TCL / WHERE절


## DML

>INSERT
>UPDATE
>DELETE
>SELECT

**Wild Card**
애스터리스크(*) : 테이블의 모든 칼럼 정보 보고싶은경우 사용.   

**합성연산자 (CONCATENATION)**   
- 문자와 문자를 연결하는 경우 2개의 수직 바에 의해 이루어진다(oracle)   
- 문자와 문자를 연결하는 경우 + 표시에 의해 이루어진다(SQL server)
- 두 벤더 모두 concat함수 사용 가능
- 칼럼과 문자 또는 다른 칼럼과 연결시킨다
- 문자표현식의 결과에 의해 새로운 칼럼을 생성한다.


----------------

## TCL

COMMIT / ROLLBACK

**특성**
- 원자성
- 일관성
- 고립성
- 지속성

> 데이터의 무결성 보장
> 영구적 변경 하기전 데이터의 변경사항 확인
> 논리적으로 연관된 작업을 그룹핑하여 처리 가능

**AUTO COMMIT** : DBMS가 트랜잭션 컨트롤. (SQL server 기본방식)   
**암시적 트랜잭션** : 트랜잭션 시작은 DBMS가 처리. 끝은 사용자가 명시적으로 commit/rollback처리   
**명시적 트랜잭션** : 트랜잭션의 시작과 끝을 모두 사용자가 명시    

DDL문장을 실행하면 전후시점에 자동으로 commit된다.   
DML문장 이후에 커밋없이 DDL문장이 실행되면 DDL수행전에 자동으로 커밋된다.   
DB정상접속 종료시 자동으로 커밋된다.   
DB이상종료시 자동으로 롤백된다.   

----------------

## WHERE 절


### 연산자의 종류   
-비교연산자 : = / > / >= / < / >=
-SQL연산자 : BETWEEN a AND b / IN(list) / LIKE '비교문자열' / IS NULL
-논리연산자 : AND / OR / NOT
-부정비교연산자 : != / ^= / <>  (3가지 모두 '같지 않다'는 뜻)
-부정SQL연산자 : NOT BETWEEN a AND b / NOT IN (list) / IS NOT NULL  

>**연산자 우선순위**   
> 1.괄호
> 2.NOT 연산자
> 3.비교연산자, SQL비교연산자
> 4.AND
> 5.OR

<br/>
<br/>
<br/>

### 비교연산자

-CHAR변수나 VARCHAR2와 같은 문자형 타입은 인용부호(작은 따옴표, 큰 따옴표)로 묶어서 비교처리.   

![SQLD01](https://user-images.githubusercontent.com/28701069/85293419-44e4c380-b4d8-11ea-8fe1-2ccc18fe63e5.jpg)


- 문자유형과 달리 숫자 유형 칼럼의 경우 숫자로 변환이 가능한 문자열과 비교되면 상대 탕비을 숫자 타입으로 바꾸어 비교한다.
```SQL
A   WHERE HEIGHT >= 170 
B   WHERE HEIGHT >='170'
```
위 A조건을 B라고 표현하더라도 HEIGHT칼럼이 숫자 유형인 경우 내부적으로 '170' -> 170으로 바꾸어 처리한다.

<br/>
<br/>
<br/>

### SQL연산자

SQL문장에서 사용하도록 기본적으로 예약되어 있는 연산자.

> BETWEEN a AND b
> IN (list)
> LIKE '비교문자열'
> IS NULL

<br/>

- 다중리스트   : 문장을 짧게 만들어 주면서도 성능 측면에서도 장점을 가지므로 적극적인 사용이 권고된다.   
```SQL
SELECT ENAME, JOB, DEPTNO
FROM EMP
WHERE (JOB, DEPTNO) IN (('MANAGER', 20), ('CLERK', 30));
```
<br/>
<br/>

- LIKE : 와일드카드를 사용할 수 있다. 한 개 혹은 0 개 이상의 문자를 대신해서 사용하기 위한 특수문자를 의미한다. (조합하여 사용 가능)   
> % : 0개 이상의 어떤 문자를 의미
> _ : 1개인 단일 문자를 의미

<br/>
<br/>

- IS NULL 연산자
null은 값이 존재하지 않는 것으로, 확정되지 않은 값을 표현. 공백과 달리 비교자체가 불가능한 값이다. 

> NULL값과의 수치연산은 NULL을 리턴한다
> NULL 값과의 비교연산은 거짓(false)를 리턴한다.
> 어떤 값과 비교할 수도 없으며, 특정 값 보다 크다, 적다라고 표현할 수 없다.

```SQL
WHERE SOMETHING = NULL; (X)
WHERE SOMETHING IS NULL; (O)
```

<br/>
<br/>
<br/>

### 논리연산자
비교연산자나 SQL 비교 연산자들로 이루어진 여러 개의 조건들을 논리적으로 연결시키기 위해서 사용되는 연산자이다.
> AND
> OR
> NOT

*논리연산자들이 여러 개가 같이 사용되었을 때 처리 우선순위는, (), NOT, AND, OR 순서이다.

<br/>
<br/>
<br/>


### 부정 연산자
비교연산자, SQL비교연산자에 대한 부정 표현을 부정 논리 연산자, 부정 SQL연산자로 구분할 수 있다.

>- 부정 논리연산자
>!= : 같지않다
>^=  : 같지않다
><> : 같지않다(ANIS/ISO 표준. 모든 운영체제에서 사용 가능)
>NOT 칼럼명 = : ~와 같지 않다.
>NOT 칼럼명 > : ~보다 크지 않다.

>- 부정 SQL연산자
>NOT BETWEEN a AND b : a와 b 값 사이에 있지 않다. (a,b값을 포함하지 않음)
>NOT IN (list) list 값과 일치하지 않는다.
>IS NOT NULL : NULL값을 갖지 않는다.

<br/>
<br/>
<br/>

### ROWNUM, TOP 사용
- ROWNUM : 오라클에서 칼럼과 비슷한 성격의 Pseudo column으로써 SQL처리결과 집합의 각 행에 대해 임시로 부여되는 일련번호다. 원하는 만큼의 행만 가져오고 싶을때 WHERE절에서 행의 개수를 제한하는 목적으로 사용된다. 
```SQL
SELECT TEST_NAME FROM PLAYER WHERE ROWNUM = 1;
```

테이블 내의 고유한 키나 인덱스 값을 만들 수 있다.
```SQL
UPDATE TABLE_TEST
SET COLUMN1 = ROWNUM;
```

<br/>

- TOP절 : SQL server는 TOP절을 사용하여 결과 집합으로 출력되는 행의 수를 제한할 수 있다.
```SQL
TOP (Expression) [PERCENT] [WITH TIES]

 *Expression: 반환할 행의 수를 지정하는 숫자
 *PERCENT : 쿼리 결과 집합에서 처음 Expression%의 행만 반환됨을 나타낸다.
 *WITH TIES : ORDER BY 절이 지정된 경우에만 사용할 수 있으며, 
            TOP N(PERCENT)의 마지막 행과 같은 값이 있는 경우 
            추가 행이 출력되도록 지정할수있다.

한 건의 행만 가져오고 싶은 경우
SELECT TOP(1) PLAYER_NAME FROM PLAYER;

두 건 이상의 행을 가져오고 싶은 경우
SELECT TOP(N) PLAYER_NAME FROM PLAYER;

```  

ORDER BY 절이 사용되지 않으면 오라클의 ROWNUM과 SQL Server의 TOP절은 같은 기능을 하지만, ORDER BY 절이 사용되면 기능의 차이가 발생한다.
***