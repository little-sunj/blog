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

## 2.PL/SQL 개요

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

## 4.Procedure의 생성과 활용


- Procedure기능 예시

![SQL_233](https://user-images.githubusercontent.com/28701069/88450198-23d11300-ce88-11ea-9e73-c70d2804315a.jpg)

```sql
--oracle
CREATE OR REPLACE Procedure p_DEPT_insert       -------------① 
    ( v_DEPTNO in number, 
    v_dname in varchar2, 
    v_loc in varchar2, 
    v_result out varchar2)
IS 
cnt number := 0; 
BEGIN 
    SELECT COUNT(*) INTO CNT                    -------------② 
    FROM DEPT 
    WHERE DEPTNO = v_DEPTNO 
        AND ROWNUM = 1; 
    if cnt > 0 then                             -------------③ 
        v_result := '이미 등록된 부서번호이다'; 
    else 
        INSERT INTO DEPT (DEPTNO, DNAME, LOC)   -------------④ 
        VALUES (v_DEPTNO, v_dname, v_loc); 
        COMMIT;                                 -------------⑤ 
        v_result := '입력 완료!!'; 
    end if; 
EXCEPTION                                       -------------⑥ 
    WHEN OTHERS THEN 
    ROLLBACK; 
    v_result := 'ERROR 발생'; 
END; /


--SQL Server 
CREATE Procedure dbo.p_DEPT_insert -------------① 
@v_DEPTNO int, 
@v_dname varchar(30), 
@v_loc varchar(30), 
@v_result varchar(100) OUTPUT 
AS 
DECLARE @cnt int 
SET @cnt = 0 
BEGIN 
    SELECT @cnt=COUNT(*) -------------② 
    FROM DEPT WHERE DEPTNO = @v_DEPTNO 
    IF @cnt > 0 -------------③ 
    BEGIN  
        SET @v_result = '이미 등록된 부서번호이다' 
        RETURN 
    END 
    ELSE 
    BEGIN 
        BEGIN TRAN 
        INSERT INTO DEPT (DEPTNO, DNAME, LOC) -------------④ 
        VALUES (@v_DEPTNO, @v_dname, @v_loc) 
        IF @@ERROR<>0 
            BEGIN ROLLBACK -------------⑥ 
            SET @v_result = 'ERROR 발생' 
            RETURN 
        END 
        ELSE 
        BEGIN 
            COMMIT -------------⑤ 
            SET @v_result = '입력 완료!!' 
            RETURN 
        END 
    END 
END


/*
① - 테이블에 들어갈 칼럼 값을 입력받는다.
② - 입력 받은 코드가 존재하는지 확인한다.
③ - 코드가 존재하면 '이미 등록된 부서번호이다' 라는 메시지를 출력값에 넣는다.
④ - 코드가 존재하지 않으면 입력받은 필드 값으로 새로운 레코드를 입력한다.
⑤ - 새 값이 정상적으로 입력되었을 경우 COMMIT명력어를 통해 트랜잭션을 종료한다.
⑥ - 에러 발생시 모든 트랜잭션을 취소하고 에러 메시지를 출력값에 넣는다.
*/
```

- 프로시저 작성시 주의해야 할 문법적 요소   

1. PL/SQL 및 T-SQL에서는 다양한 변수가 있다. SCALAR변수(예제의 cnt라는 값)는 사용자의 임시 데이터를 하나만 저장할 수 있는 변수이며 거의 모든 형태의 데이터 유형을 지정할 수 있다.
2. PL/SQL에서 사용하는 SELECT문장은 결과값이 반드시 있어야 하며, 무조건 하나여야 한다. 조회결과가 없거나 하나 이상인 경우는 에러가 발생한다. (T-SQL에서는 값이 없어도 에러가 발생하지 않는다.)
3. T-SQL을 비롯하여 일반적으로 대입 연산자는 "="를 사용하지만 PL/SQL에서는 ":="를 사용한다.
4. 에러처리를 담당하는 EXCEPTION에서는 WHEN ~ THEN절을 사용하여 에러의 종류별로 적절히 처리한다. OTHERS를 이용하여 모든 에러를 처리할 수 있지만 정확하게 에러를 처리하는 것이 좋다.


-----

&nbsp;

## 5.User Defined Function의 생성과 활용

Procedure처럼 절차형 SQL을 로직과 함께 DB내에 저장해 놓은 명령문의 집합을 의미한다. SUM, SUBSTR, NVL등의 함수는 벤더에서 미리 만들어둔 내장 함수이고, 사용자가 별도로 함수를 만들 수도 있다.   

Function이 Procedure와 다른 점은 RETURN을 사용해서 하나의 값을 **반드시** 되돌려 줘야 한다는 것이다.

```sql
--oracle
CREATE OR REPLACE Function UTIL_ABS 
(v_input in number) ---------------- ① 
    return NUMBER 
IS 
    v_return number := 0; ---------------- ② 
BEGIN 
    if v_input < 0 then ---------------- ③ 
        v_return := v_input * -1; 
    else 
        v_return := v_input; 
    end if; 
    RETURN v_return; ---------------- ④ 
END; 
/


--SQL Server 
CREATE Function dbo.UTIL_ABS 
(@v_input int) ---------------- ① 
RETURNS int 
AS 
BEGIN 
    DECLARE @v_return int ---------------- ② 
    SET @v_return=0 
    IF @v_input < 0 ---------------- ③ 
        SET @v_return = @v_input * -1 
    ELSE 
        SET @v_return = @v_input 
    RETURN @v_return; ---------------- ④ 
END

/*
① - 값을 입력받는다.
② - 리턴 값을 받아 줄 변수인 v_return 을 선언한다.
③ - 입력 값이 음수이면 -1을 곱하여 v_return 변수에 대입한다.
④ - v_return변수를 리턴한다.
*/
```


-----

&nbsp;

## 6.Trigger의 생성과 활용

Trigger란 특정한 테이블에 INSERT, UPDATE, DELETE와 같은 DML문이 수행되었을 때, DB에서 자동으로 동작하도록 작성된 프로그램이다. 사용자가 직접 호출하여 사용하는것이 아니고 DB에서 자동적으로 수행하게 된다.   
테이블과 뷰, DB작업을 대상으로 정의할 수 있으며, 전체 트랜잭션 작업에 대해 발생되는 Trigger와 각 행에 대해서 발생되는 Trigger가 있다.   

```sql
--ex) 주문한 건이 입력될 때마다, 일자별 상품별로 판매수량과 판매금액을 집계하여 집계자료를 보관
--Oracle 
CREATE OR REPLACE Trigger SUMMARY_SALES ---------------- ① Trigger선언
AFTER INSERT  --레코드가 입력이 된 후 Trigger발생
ON ORDER_LIST   --ORDER_LIST 테이블에 Trigger설정
FOR EACH ROW    --각 row마다 Trigger적용
DECLARE ---------------- ②  : 값을 저장할 변수를 선언하고, 신규입력 데이터를 저장한다.
    o_date ORDER_LIST.order_date%TYPE; 
    o_prod ORDER_LIST.product%TYPE; 
BEGIN 
    o_date := :NEW.order_date;  -- NEW : 신규로 입력된 레코드의 정보를 가지고있는 구조체 (OLD는 수정, 삭제전 레코드를 가지고 있는 구조체)
    o_prod := :NEW.product; 
    UPDATE SALES_PER_DATE ---------------- ③ 먼저 입력된 주문 내역의 주문 일자와 상품을 기준으로 테이블 업데이트
        SET qty = qty + :NEW.qty, 
        amount = amount + :NEW.amount 
    WHERE sale_date = o_date 
    AND product = o_prod; 
    if SQL%NOTFOUND then ---------------- ④ 처리결과가 SQL%NOTFOUND시 SALES_PER_DATE에 집계데이터 입력
        INSERT INTO SALES_PER_DATE 
        VALUES(o_date, o_prod, :NEW.qty, :NEW.amount); 
    end if; 
END; 
/

```

![SQL_236](https://user-images.githubusercontent.com/28701069/88539062-7c044280-d04b-11ea-94e7-ec102ec4a1d8.jpg)

```sql
--SQL Server 
CREATE Trigger dbo.SUMMARY_SALES ---------------- ① 선언
ON ORDER_LIST 
AFTER INSERT 
AS 
DECLARE 
    @o_date DATETIME,@o_prod INT,@qty int, @amount int 
BEGIN 
    SELECT @o_date=order_date, @o_prod=product, @qty=qty, @amount=amount 
    FROM inserted ---------------- ② inserted : 신규 입력 레코드 담는 구조체 / deleted : 수정,삭제전 레코드값 담는 구조체
    UPDATE SALES_PER_DATE ---------------- ③  먼저입력된 값 기준으로 SALES_PER_DATE 업데이트
        SET qty = qty + @qty, 
            amount = amount + @amount 
    WHERE sale_date = @o_date 
    AND product = @o_prod; 
    IF @@ROWCOUNT=0 ---------------- ④ 처리결과가 0일시 SALES_PER_DATE에 집계데이터 입력
        INSERT INTO SALES_PER_DATE 
        VALUES(@o_date, @o_prod, @qty, @amount) 
END;
```

![SQL_237](https://user-images.githubusercontent.com/28701069/88539139-a6560000-d04b-11ea-910a-bfed3db91657.jpg)

> Trigger는 DB에 의해 자동 호출되지만 결국 INSERT / UPDATE / DELETE문과 하나의 트랜잭션 안에서 일어나는 일련의 작업들이라 할 수 있다.   
> DB보안의 적용, 유효하지 않은 트랜잭션의 예방, 업무 규칙 자동 적용 제공 등에 활용될 수 있다.


-----

&nbsp;

## 7.프로시저와 트리거의 차이점

![SQL_238](https://user-images.githubusercontent.com/28701069/88540803-98ee4500-d04e-11ea-82e5-4bc206b9c2b5.jpg)
