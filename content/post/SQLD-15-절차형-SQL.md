---
title: "SQLD 15 절차형 SQL"
date: 2020-07-23T19:23:15+09:00
categories:
- SQLD
- 2과목 SQL 기본 및 활용
tags:
- SQLD
keywords:
- SQLD
---

<!--more-->
# 절차형 SQL

&nbsp;

## 1.절차형 SQL 개요

일반적인 개발 언어처럼 SQL에도 절차 지향적인 프로그램이 가능하도록 PL(Procedural Language)/SQL, T-SQL 등의 절차형 SQL을 제공하고 있다. SQL문의 연속적인 실행이나 조건에 따른 분기처리를 이용하여 특정 기능을 수행하는 저장 모듈을 생성할 수 있다.

-----

&nbsp;

## 1.PL/SQL 개요

오라클의 PL/SQL은 절차적 프로그래밍을 가능하게하는 트랜잭션 언어로, Block구조로 되어있고 Block내에는 DML문장과 Query문장, 그리고 절차형 언어(IF, LOOP)등을 사용할 수 있다.   

&nbsp;

- **저장모듈** : PL/SQL문장을 DB서버에 저장하여 사용자과 애플리케이션 사이에서 공유할 수 있도록 만든 일종의 SQL컴포넌트 프로그램. 독립적으로 실행되거나 다른 프로그램으로부터 실행될 수 있는 완전한 실행 프로그램이다. 오라클의 저장모듈에는 Procedure, User Defined Function, Trigger가 있다.

&nbsp;

-----

- **PL/SQL 특징**
> 1. Block구조로 되어있어 기능별로 모듈화 가능
> 2. 변수, 상수 등을 선언해 SQL문장 간 값을 교환
> 3. IF, LOOP등의 절차형 언어를 사용해 절차적인 프로그램이 가능
> 4. DBMS정의 에러나 사용자 정의 에러를 정의해 사용 가능
> 5. 오라클에 내장되어있어서 오라클과 PL/SQL을 지원하는 어떤 서버로도 프로그램을 옮길 수 있다.
> 6. 응용 프로그램의 성능을 향상시킨다.
> 7. 여러 SQL문장을 Block으로 묶고 한 번에 서버로 보내기 때문에 통신량을 줄일 수 있다.

&nbsp;

- PL/SQL Architecture
![SQL_230](https://user-images.githubusercontent.com/28701069/88285072-734cfd00-cd29-11ea-8a94-c92231cd8f89.jpg)

block프로그램을 입력받으면 SQL문장과 프로그램문장을 구분하여 처리한다.

&nbsp;

-----

- **PL/SQL 구조**

![SQL_231](https://user-images.githubusercontent.com/28701069/88285092-7ba53800-cd29-11ea-931e-5c0192d2dda8.jpg)

-----

- **PL/SQL 기본 문법**

```sql
CREATE [OR REPLACE] Procedure [Procedure_name] 
( argument1 [mode] data_type1, 
argument2 [mode] date_type2, 
... ... ) 
IS [AS] 
... ... 
BEGIN 
... ... 
EXCEPTION 
... ... 
END; 
/

-- CREATE : 프로시저 생성. DB내 저장되며 필요시 호출하여 실행 할 수 있다.
--[OR REPLACE] : DB내 같은 이름의 프로시저가 있을 경우, 
--          기존의 프로시저를 무시하고 새로운 내용으로 덮어쓰겠다는 의미.
--argument : 프로시저 호출시 들어올 값, 
--          혹은 프로시저에서 처리한 결과값을 운영 체제로 리턴시킬 매개변수를 지정할 때 사용
--[mode] : 매개변수 지정. 3가지 유형이 있다. 
--          1. IN : 운영 체제에서 프로시저로 전될될 변수
--          2. OUT : 프로시저에서 처리된 결과. 운영체제로 전달
--          3. INOUT : IN과 OUT 두 가지의 기능을 동시에 수행
-- / : 마지막 슬래쉬는 DB에 프로시저를 컴파일 하라는 명령어이다.

--생성된 프로시저 삭제 
DROP Procedure [Procedure_name];
```

-----

&nbsp;

## 3.T-SQL 개요

&nbsp;

- **T-SQL 특징**

SQL server를 제어하기 위한 언어. MS사에서 ANSI/ISO표준에 약간의 기능을 더 추가하여 보완적으로 만든 것이다.    

기능은 아래와 같다.

- 변수 선언 기능 : @@ 이라는 전역변수(시스템 함수)와 @라는 지역변수가 있다.
- 전역변수 : 이미 SQL서버에 내장된 값
- 지역변수 : 사용자가 자신의 연결 시간 동안만 사용하기 위해 만들어지는 변수
- 데이터 유형 (int, float, varchar등)
- 연산자 : 산술(+,-,*,/), 비교(=,<,>,<>), 논리 (and, or, not) 사용 가능
- 흐름 제어 가능 : IF-ELSE, WHILE, CASE-THEN
- 주석 기능 

&nbsp;

-----

- **T-SQL 구조**

![SQL_232](https://user-images.githubusercontent.com/28701069/88286850-5ebe3400-cd2c-11ea-8717-9b35feb7d00a.jpg)

T-SQL에서는 BEGIN, END문을 반드시 사용해야하는 것은 아니지만 블록 단위로 처리하고자 할 때는 반드시 작성해야 한다.   

&nbsp;

-----

- **T-SQL 기본 문법**

```sql
CREATE Procedure [schema_name.]Procedure_name 
@parameter1 data_type1 [mode], 
@parameter2 date_type2 [mode], 
... ... 
WITH AS 
... ... 
BEGIN 
... ... 
ERROR 처리 
... ... 
END;

--[mode] 부분에 지정할 수 있는 매개변수는 4가지가 있다.
-- 1. VARYING : 결과 집합이 출력 매개 변수로 사용되도록 지정한 CURSOR매기변수에만 적용된다.
-- 2. DEFAULT : 프로시저 호출시 지정된 매개변수가 지정되지 않을 경우 기본값으로 처리한다.
-- 3. OUT / OUTPUT : 프로시저에서 처리된 결과 값을 EXECUTE문 호출 시 반환한다.
-- 4. READONLY : 프로시저 본문 내 매개 변수를 업데이트하거나 수정할 수 없다. 
--          (ex. 매개변수 유형이 사용자 정의 테이블 형식인 경우)

--WITH 부분에 지정할 수 있는 옵션은 3가지가 있다.
-- 1. RECOMPILE : 프로시저가 런타임에 컴파일된다. DB엔진에서 현재 프로시저의 계획을 캐시하지 않는다. 
--          저장 프로시저 안에 있는 개별 쿼리에 대한 계획을 삭제할 때 RECOMPILE 힌트를 사용한다.
-- 2. ENCRYPTION : CREATE PROCEDURE문의 원본 텍스트가 알아보기 어려운 형식으로 변환된다.
--          변조된 출력은 카탈로그 뷰 어디에도 직접 표시되지 않는다.
--          원본을 볼 수 있는 방법이 없기 때문에 반드시 원본 백업을 해두어야한다.
-- 3. EXECUTE AS : 해당 저장 프로시저를 실행할 보안 컨텍스트를 지정한다.

--프로시저 삭제 명령어
DROP Procedure [schema_name.]Procedure_name;
```

프로시저 변경이 필요할 경우 오라클은 [CREATE OR REPLACE]와 같이 하나의 구문으로 처리하지만 SQL Server는 CREATE구문을 ALTER구문으로 변경하여야 한다.

&nbsp;

-----

&nbsp;

