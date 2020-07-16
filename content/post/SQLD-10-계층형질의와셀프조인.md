---
title: "SQLD 10 계층형 질의와 셀프 조인"
date: 2020-07-16T18:12:56+09:00
categories:
- SQLD
- 2과목 SQL 기본 및 활용
tags:
- SQLD
keywords:
- SQLD
---

<!--more-->

# SQLD 10. 계층형 질의와 셀프 조인 

&nbsp;

## 계층형 질의 (Hierarchical Query)

테이블에 계층형 데이터가 존재하는 경우 계층형 질의(Hierarchical Query)를 사용한다.
- **계층형 데이터** : 동일 테이블에 계층적으로 상위와 하위 데이터가 포함된 데이터.   
    ex) 사원 테이블내 사원들 사이 상위 사원(관리자)와 하위 사원 관계가 존재하는 등..   

엔터티를 순환관계 데이터 모델로 설계할 경우 계층형 데이터가 발생한다.   
순환관계 데이터 모델의 예시 : 조직, 사원, 메뉴 등..

![SQL_206](https://user-images.githubusercontent.com/28701069/87662925-f7d5d380-c79d-11ea-8712-647bf9e13db4.jpg)

&nbsp;

-----

&nbsp;
## Hierarchical Query - Oracle 
```sql
SELECT ...
FROM --테이블
WHERE condition AND condition...
START WITH condition 
CONNECT BY [NOCYCLE] condition AND condition...
[ORDER SIBLINGS BY column, column, ...]


-- START WITH : 계층 구조 전개의 시작 위치를 지정하는 구문이다. 즉, 루트 데이터를 지정한다. (액세스)
-- CONNECT BY : 다음에 전개될 자식 데이터를 지정하는 구문이다. 
--              자식 데이터는 CONNECT BY절에 주어진 조건을 만족해야한다. (조인)
-- PRIOR      : CONNECT BY 절에 사용되며, 현재 읽은 칼럼을 지정한다. 
--              'PRIOR 자식=부모' 형태를 사용하면 부모 -> 자식 방향으로 전개하는 순방향 전개를 한다. 
--              'PRIOR 부모=자식' 형태를 사용하면 반대로 자식 -> 부모 방향으로 역방향 전개를 한다.
-- NOCYCLE    : 데이터를 전개하면서 이미 나타났던 동일한 데이터가 전개 중에 다시 나타난다면 
--              이것을 가리켜 사이클(Cycle)이 형성되었다고 말한다. 
--              사이클이 발생한 데이터는 런타임 오류가 발생한다.
--              그러나 NOCYCLE을 추가하면 사이클이 발생한 이후의 데이터는 전개하지 않는다.
-- ORDER SIBLINGS BY : 형제 노드(동일level) 사이에서 정렬을 수행한다.
-- WHERE      : 모든 전개를 수행한 후에 지정된 조건을 만족하는 데이터만 추출한다. (필터링)
```

오라클은 계층형 질의를 사용할 때 다음과 같은 가상 칼럼(Pseudo Column)을 제공한다.   
![SQL_208](https://user-images.githubusercontent.com/28701069/87664140-b9d9af00-c79f-11ea-95f6-ec52bf52c6af.jpg)

&nbsp;

- 순방향전개   
```sql
--순방향 sample
SELECT  LEVEL, LPAD(' ',4*(LEVEL-1)) || EMPNO 사원,
        MGR 관리자, CONNECT_BY_ISLEAF ISLEAF
FROM    EMP
START   WITH MGR IS NULL
CONNECT BY PRIOR EMPNO = MGR;
```
![SQL_209](https://user-images.githubusercontent.com/28701069/87664465-408e8c00-c7a0-11ea-9016-5e9774b329ff.jpg)

&nbsp;

- 역방향전개      
```sql
--역방향 sample
SELECT  LEVEL, LPAD(' ',4*(LEVEL-1)) || EMPNO 사원,
        MGR 관리자, CONNECT_BY_ISLEAF ISLEAF
FROM    EMP
START   WITH MGR = '7876'
CONNECT BY PRIOR MGR = EMPNO;
```
![SQL_210](https://user-images.githubusercontent.com/28701069/87664593-7c295600-c7a0-11ea-98e0-1f7ad961b235.jpg)

역방향 전개이기 때문에 하위 데이터에서 상위 데이터로 전개된다. 내용을 제외하고 표시 형태는 순방향 전개와 동일하다. D는 루트 데이터기 때문에 레벨이 1이다. D의 상위 데이터인 C는 레벨이 2이다. 그리고 C의 상위데이터는 A는 레벨이 3이다. 리프 데이터는 A다.   

루트 및 레벨은 전개되는 방향에 따라 반대가 됨을 알 수 있다. 

&nbsp;

오라클은 사용자 편의를 위해 아래 함수들을 제공한다.

![SQL_211](https://user-images.githubusercontent.com/28701069/87664612-83e8fa80-c7a0-11ea-949a-596b83018785.jpg)

```sql
--sample
SELECT  CONNECT_BY_ROOT(EMPNO) 루트사원, 
        SYS_CONNECT_BY_PATH(EMPNO, '/') 경로,
        EMPNO 사원,
        MGR 관리자
FROM    EMP
START WITH MGR IS NULL
CONNECT BY PRIOR EMPNO = MGR;
```

&nbsp;

-----

&nbsp;

## 셀프 조인 (SELF JOIN)


동일 테이블 사이의 조인을 말한다. FROM절에 동일 테이블이 두 번 이상 나타난다. 동일 테이블 사이의 조인을 수행하면 테이블과 칼럼 이름이 모두 동일하기 때문에 식별을 위해 반드시 ALIAS를 사용해야 한다.   

기본적인 사용법은 다음과 같다.

```sql
SELECT  ALIAS1.COLUMN1,
        ALIAS2.COLUMN2,...
FROM    TABLE1 ALIAS1,
        TABLE2 ALIAS2
WHERE   ALIAS1.COLUMN2 = ALIAS2.COLUMN1;

SELECT  WORKER.EMPNO empno,
        WORKER.ENAME ename,
        MANAGER.ENAME manager_name
FROM    EMP WORKER,
        EMP MANAGER
WHERE   WORKER.MGR = MANAGER.EMPNO;

/*
사원이라는 테이블 속에는 사원과 관리자가 모두 하나의 사원이라는 개념으로 입력되어있다.
이를 이용해 셀프 조인으로 "자신과 상위, 차상위 관리자를 같은 줄에 표시하라" 와 같은 문제를 해결할 수 있다. 
(FROM절에 사원 테이블을 두 번 사용)

자신과 자신의 직속 관리자는 동일한 행에서 구할 수 있으나, 차상위 관리자는 바로 구할 수 없다. 자신의 직속 관리자를 기준으로 사원 테이블과 한번 더 셀프조인을 수행해야 한다.
OUTER조인을 사용 해야 한다. 
(INNER JOIN 사용시 자신의 관리자가 존재하지 않는 경우 관리자 테이블에서 조인대상이 없기때문에 결과 누락 발생가능)
*/

SELECT  E1.EMPNO    empno,
        E1.MGR      manager,
        E2.MGR      upr_manager
FROM    EMP E1,
LEFT OUTER JOIN EMP E2
ON (E1.MGR = E2.EMPNO)
ORDER BY E2.MGR DESC, E1.MGR, E1.EMPNO;
```

![SQL_213](https://user-images.githubusercontent.com/28701069/87666305-581b4400-c7a3-11ea-941d-2bad3ed0156b.jpg)

셀프조인은 동일한 테이블이지만, 개념적으로는 두 개의 서로 다른 테이블(사원, 관리자)를 사용하는 것과 동일하다. 다른 테이블인 것처럼 처리하기 위해 테이블 별칭이 사용된다.  