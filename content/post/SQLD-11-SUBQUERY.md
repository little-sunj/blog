---
title: "SQLD 11 SUBQUERY"
date: 2020-07-19T13:02:40+09:00
categories:
- SQLD
- 2과목 SQL 기본 및 활용
tags:
- SQLD
keywords:
- SQLD

---

<!--more-->

# 서브쿼리 (SUBQUERY)

&nbsp;

하나의 SQL문안에 포함되어있는 또 다른 SQL문. 알려지지 않는 기준을 이용한 검색을 위해 사용한다.

&nbsp;

- **JOIN** : 참여하는 모든 테이블이 대등한 관계. 참여 테이블들의 칼럼 자유롭게 사용 가능. 집합간의 곱(product)관계로, (1:M)시 (1*M)레벨이 집합이 생성된다.   
- **SUBQUERY** :  서브쿼리에서는 메인쿼리 컬럼 사용가능. 메인쿼리에선 서브쿼리의 칼럼 사용 불가. 서브쿼리 레벨과는 상관없이 항상 메인쿼리 레벨로 결과 집합이 생성된다.

질의 결과에 서브쿼리 칼럼을 표시해야 한다면, 조인 방식으로 변환하거나 함수, 스칼라 서브쿼리(Scalar Subquery)등을 사용해야 한다.

&nbsp;

 - 주의사항
 1. 서브쿼리를 괄호로 감싸서 사용한다.
 2. 단일행(single row)또는 복수 행(multiple row) 비교 연산자와 함께 사용 가능하다. 단일 행 비교연산자는 서브쿼리의 결과가 반드시 1건이하여야 하고 복수 행 비교 연산자는 서브쿼리의 결과 건수와 상관없다.
 3. 서브쿼리에서는 order by를 사용하지 못한다. order by절은 select절에서 오직 한 개만 올 수 있기 때문에 메인쿼리 마지막에 위치해야한다.

&nbsp;

 - 사용가능 위치
 1. SELECT 절
 2. FROM 절
 3. WHERE 절
 4. HAVING 절
 5. ORDER BY 절
 6. INSERT문의 VALUES절
 7. UPDATE문의 SET절

&nbsp;

서브쿼리는 동작방식이나, 반환되는 데이터의 형태에 따라 분류할 수 있다.
![SQL_215](https://user-images.githubusercontent.com/28701069/87874364-3e2c6c00-ca04-11ea-8452-6931c380b4b7.jpg)
![SQL_216](https://user-images.githubusercontent.com/28701069/87874363-3cfb3f00-ca04-11ea-8702-7b8ba647f52f.jpg)

서브쿼리는 메인쿼리 안에 포함된 종속적인 관계이기 때문에 논리적인 실행순서는 항상 메인쿼리에서 읽혀진 데이터에 대해 서브쿼리에서 해당 조건이 만족하는지를 확인하는 방식으로 수행되어야한다. 그러나 실제 서브쿼리 실행순서는 상황에 따라 달라질 수 있다.

&nbsp;

-----

## 1.단일 행 서브쿼리

단일 행 비교연산자와 함께 사용할 때는 서브쿼리 결과 건수가 반드시 1건 이하여야 한다.    
테이블 전체에 하나의 그룹함수를 적용할 때는 그 결과값이 1건이 생성되기 때문에 단일 행 서브쿼리로서 사용이 가능한다. 

```sql
SELECT  PLAYER_NAME plName, 
        PLAYER_POSITION plPosition, 
        BACK_NO bkNo
FROM    PLAYER
WHERE   HEIGHT <= ( SELECT AVG(HEIGHT)
                    FROM PLAYER)
ORDER BY PLAYER_NAME;
```

&nbsp;

-----

## 2.다중 행 서브쿼리

서브쿼리 결과가 2건 이상 반환될 수 있다면 반드시 다중 행 비교 연산자 (IN, ALL, ANY, SOME)과 함께 사용해야 한다. 

![SQL_219](https://user-images.githubusercontent.com/28701069/87874993-18559600-ca09-11ea-9fc0-95a6f8e54533.jpg)

```sql
SELECT  REGION_NAME 연고지명, 
        TEAM_NAME 팀명, 
        E_TEAM_NAME 영문팀명 
FROM    TEAM 
WHERE   TEAM_ID IN (SELECT TEAM_ID FROM PLAYER WHERE PLAYER_NAME = '정현수') 
ORDER BY TEAM_NAME;
```

&nbsp;

-----

## 3.다중 칼럼 서브쿼리

서브쿼리 결과로 여러 개의 칼럼이 반환되어 메인쿼리의 조건과 동시에 비교된다.

```sql
 SELECT TEAM_ID 팀코드, 
        PLAYER_NAME 선수명, 
        POSITION 포지션, 
        BACK_NO 백넘버, 
        HEIGHT 키 
 FROM   PLAYER 
 WHERE  (TEAM_ID, HEIGHT) IN (SELECT TEAM_ID, MIN(HEIGHT) FROM PLAYER GROUP BY TEAM_ID) 
 ORDER BY TEAM_ID, PLAYER_NAME;

--sql server에서는 지원하지 않는 기능이다. 
```

&nbsp;

-----

## 4.연관 서브쿼리

서브쿼리 내에 메인쿼리 칼럼이 사용된 서브쿼리이다.

```sql
SELECT  T.TEAM_NAME 팀명, 
        M.PLAYER_NAME 선수명, 
        M.POSITION 포지션, 
        M.BACK_NO 백넘버, 
        M.HEIGHT 키 
FROM    PLAYER M, TEAM T 
WHERE M.TEAM_ID = T.TEAM_ID AND M.HEIGHT < ( SELECT AVG(S.HEIGHT) 
                                            FROM PLAYER S 
                                            WHERE S.TEAM_ID = M.TEAM_ID AND S.HEIGHT IS NOT NULL 
                                            GROUP BY S.TEAM_ID ) 
ORDER BY 선수명;

-- 팀 소속의 평균키를 구하고, 그 평균키와 선수의 키를 비교하여 적을경우 선수정보를 출력한다.
-- 평균키 보다 선수의 키가 크거나 같으면 조건에 맞지 않기 때문에 해당 데이터는 출력되지 않는다.
-- 이 작업을 메인쿼리에 존재하는 모든 행에 대해 반복 수행한다.
```

EXISTS 서브쿼리는 항상 연관 서브쿼리로 사용된다. **아무리 조건을 만족하는 건이 여러 건이더라도 조건을 만족하는 1건만 찾으면 추가적인 검색을 진행하지 않는다.**

```sql
 SELECT STADIUM_ID ID, 
        STADIUM_NAME 경기장명 
 FROM   STADIUM A 
 WHERE EXISTS ( SELECT 1 
                FROM SCHEDULE X 
                WHERE X.STADIUM_ID = A.STADIUM_ID 
                AND X.SCHE_DATE BETWEEN '20120501' AND '20120502')
```
&nbsp;

-----

## 5.그밖에 위치에서 사용하는 서브쿼리

&nbsp;

### a.SELECT 절 서브쿼리

스칼라 서브쿼리(Scalar subquery). 한 행, 한 칼럼만을 반환하는 서브쿼리다.칼럼을 쓸 수 있는 대부분의 곳에서 사용할 수 있다. (결과값이 2건 이상 반환되면 오류를 반환한다.)

```sql
SELECT  PLAYER_NAME 선수명, 
        HEIGHT 키, 
        (SELECT AVG(HEIGHT) FROM PLAYER X WHERE X.TEAM_ID = P.TEAM_ID) 팀평균키 
FROM PLAYER P
```

&nbsp;

### b.FROM 절 서브쿼리

인라인 뷰(inline view)라고 한다. 테이블 명이 올 수 있는 곳에서 사용할 수 있다. 서브쿼리의 결과가 마치 실행 시에 동적으로 생성된 테이블인 것처럼 사용 될 수 있다. 인라인 뷰는 SQL문이 실행될 때만 임시적으로 생성되는 동적인 뷰이기 때문에 DB에 해당 정보가 저장되지 않는다. 그래서 일반적인 뷰를 정적 뷰(static view)라고 하고 인라인 뷰를 동적 뷰(dynamic view)라고도 한다.   

서브쿼리 칼럼은 메인쿼리에서 사용 될 수 없다. 그러나 인라인 뷰는 **동적으로 생성된 테이블**이다. JOIN방식을 사용하는 것과 같기 때문에 인라인 뷰의 칼럼은 SQL문에서 자유롭게 참조할 수 있다.   

```sql
SELECT  T.TEAM_NAME 팀명, 
        P.PLAYER_NAME 선수명, 
        P.BACK_NO 백넘버 
FROM    (SELECT TEAM_ID, PLAYER_NAME, BACK_NO FROM PLAYER WHERE POSITION = 'MF') P, 
        TEAM T 
WHERE P.TEAM_ID = T.TEAM_ID 
ORDER BY 선수명;
```

인라인 뷰에서는 ORDER BY절도 사용 가능하다. 인라인 뷰에서 먼저 정렬을 수행하고, 결과 중에서 데이터를 추출하는 것을 **TOP-N쿼리**라고 한다. TOP-N쿼리를 수행하기 위해서는 정렬작업과 정렬 결과 중에서 일부 데이터만을 추출할 수 있는 방법이 필요하다. 오라클에서는 ROWNUM이라는 연산자를 통해 결과로 추출하고자 하는 데이터 건수를 제약할 수 있다.

```sql
SELECT  PLAYER_NAME 선수명, 
        POSITION 포지션, 
        BACK_NO 백넘버, 
        HEIGHT 키 
FROM (  SELECT PLAYER_NAME, POSITION, BACK_NO, HEIGHT 
        FROM PLAYER 
        WHERE HEIGHT IS NOT NULL 
        ORDER BY HEIGHT DESC) 
WHERE ROWNUM <= 5;
```

&nbsp;

### c.HAVING 절 서브쿼리

HAVING절은 그룹함수와 함께 사용될 때 그룹핑된 결과에 대해 부가적인 조건을 주기 위해서 사용한다.

```sql
SELECT  P.TEAM_ID 팀코드, 
        T.TEAM_NAME 팀명, 
        AVG(P.HEIGHT) 평균키 
FROM PLAYER P, TEAM T
WHERE P.TEAM_ID = T.TEAM_ID 
GROUP BY P.TEAM_ID, T.TEAM_NAME 
HAVING AVG(P.HEIGHT) < (SELECT AVG(HEIGHT) 
                        FROM PLAYER 
                        WHERE TEAM_ID ='K02')
```

&nbsp;

### d.UPDATE문 SET절 서브쿼리

서브쿼리를 사용한 변경 작업시 서브쿼리 결과가 NULL을 반환할 경우 해당 컬럼의 결과가 NULL이 될 수 있기 때문에 주의해야한다.


```sql
UPDATE TEAM A 
SET A.STADIUM_NAME = (SELECT X.STADIUM_NAME FROM STADIUM X WHERE X.STADIUM_ID = A.STADIUM_ID);
```

&nbsp;

### d.INSERT문 VALUES절 서브쿼리

```sql
INSERT INTO PLAYER(PLAYER_ID, PLAYER_NAME, TEAM_ID) 
VALUES((SELECT TO_CHAR(MAX(TO_NUMBER(PLAYER_ID))+1) FROM PLAYER), '홍길동', 'K06');
```

-----

## 6.뷰 (VIEW)

테이블은 실제로 데이터를 가지고 있는 반면, 뷰(View)는 실제 데이터를 가지고 있지 않다. 단지 뷰정의(view definition)만을 가지고 있다. 질의에서 뷰가 사용되면 뷰 정의를 참조해서 DBMS내부적으로 질의를 재작성하여 수행한다. 실제 데이터를 가지고 있지 않지만 테이블이 수행하는 역할을 하기 때문에 가상 테이블(virtual table)이라고도 한다.

![SQL_221](https://user-images.githubusercontent.com/28701069/87875744-54d7c080-ca0e-11ea-89b8-929a1b29aad5.jpg)

```sql
CREATE VIEW V_PLAYER_TEAM AS
    SELECT  P.PLAYER_NAME, 
            P.POSITION, 
            P.BACK_NO, 
            P.TEAM_ID, 
            T.TEAM_NAME 
FROM    PLAYER P, 
        TEAM T 
WHERE P.TEAM_ID = T.TEAM_ID;

-- 테이블 뿐 아니라 이미 존재하는 뷰를 참조해서도 생성할 수 있다.
-- 뷰를 포함하는 뷰를 잘못 생성하는 경우 성능상의 문제를 유발할 수 있으므로, 
-- 뷰와 SQL의 수행원리를 잘 이해하고 사용하여야 한다.

CREATE VIEW V_PLAYER_TEAM_FILTER AS 
        SELECT  PLAYER_NAME, 
                POSITION, 
                BACK_NO, 
                TEAM_NAME 
FROM V_PLAYER_TEAM 
WHERE POSITION IN ('GK', 'MF');

SELECT PLAYER_NAME, 
POSITION, 
BACK_NO, 
TEAM_ID, 
TEAM_NAME 
FROM V_PLAYER_TEAM WHERE PLAYER_NAME LIKE '김%'

-- 뷰를 제거하기 위해서는 DROP VIEW문을 사용한다.
DROP VIEW V_PLAYER_TEAM; 
DROP VIEW V_PLAYER_TEAM_FILTER;
```
