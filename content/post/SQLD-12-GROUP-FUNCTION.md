---
title: "SQLD 12 GROUP FUNCTION"
date: 2020-07-20T20:53:59+09:00
categories:
- SQLD
- 2과목 SQL 기본 및 활용
tags:
- SQLD
keywords:
- SQLD
---

<!--more-->

# 그룹 함수 (GROUP FUNCTION)

&nbsp;


## 1.데이터 분석 개요

데이터 분석을 위해 정의된 표준 함수 3가지는 아래와 같다.

&nbsp;

1. **AGGREGATE FUNCTION**   
    GROUP AGGREGATE FUNCTION이라고도 한다. GROUP FUNCTION의 한 부분으로 분류하며 COUNT, SUM, AVG, MAX, MIN외 각종 집계 함수들이 포함되어있다.
2. **GROUP FUNCTION**   
    GROUPING함수, CASE함수 외, 
    - ROLL UP 함수 : 소그룹간의 소계를 계산. GROUP BY의 확장된 형태로 사용하기 쉬우며, 병렬로 수행이 가능하다. 계층적 분류를 포함하고 있는 데이터의 집계에 적합하도록 되어 있다.   
    - CUBE함수 : GROUP BY 항목들간 다차원적인 소계를 계산. 결합 가능한 모든 값에 대하여 다차원적인 집계를 생성하게 되므로 ROLLUP에 비해 다양한 데이터를 얻는 장점이 있는 반면, 시스템 부하를 많이 주는 단점이 있다.   
    - GROUPING SETS함수 : 원하는 부분의 소계만 손쉽게 추출할 수 있는 장점이 있다.
    ROLLUP, CUBE, GROUPING SETS 결과에 대한 정렬이 필요한 경우 ORDER BY절에 정렬 칼럼을 명시해야 한다.
3. **WINDOW FUNCTION**
    분석함수나 순위함수로도 알려져 있다. 데이터 웨어하우스에서 발전한 기능이다.

&nbsp;

-----

## 2.ROLL UP

ROLLUP에 지정된 grouping columns의 list는 subtotal을 생성하기 위해 사용되어지며, grouping columns수를 N이라고 했을 떄 N+1 level의 subtotal이 생성된다. **ROLLUP의 인수는 계층구조이므로 인수 순서가 바뀌면 수행 결과도 바뀌므로 주의해야한다.**

```sql
--ROLLUP함수 사용
SELECT DNAME, JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal" 
FROM EMP, DEPT 
WHERE DEPT.DEPTNO = EMP.DEPTNO 
GROUP BY ROLLUP (DNAME, JOB);
ORDER BY DNAME, JOB;

--실행결과에서는 GROUPING COLUMNS(DNAME, JOB)에 대해 다음과 같은 추가 LEVEL의 집계가 생성된다.
--L1 : GROUP BY수행시 생성되는 표준 집계
--L2 : DNAME별 모든 JOB의 SUBTOTAL
--L3 : GRAND TOTAL (마지막행 1건)
```

ROLLUP의 경우 계층 간 집계에 대해서는 LEVEL별 순서(L1->L2->L3)를 정렬하지만, 계층 내 GROUP BY수행시 생성되는 표준 집계에는 별도의 정렬을 지원하지 않는다. (필요시 별도의 ORDER BY를 사용해야한다.)


```sql
--ROLLUP함수 + ORDER BY 사용
SELECT DNAME, JOB, COUNT(*) "Total Empl", SUM(SAL) "Total Sal" 
FROM EMP, DEPT 
WHERE DEPT.DEPTNO = EMP.DEPTNO 
GROUP BY ROLLUP (DNAME, JOB) 
ORDER BY DNAME, JOB ;


--GROUPING 함수 사용
SELECT  DNAME, GROUPING(DNAME), 
        JOB, GROUPING(JOB), 
        COUNT(*) "Total Empl", 
        SUM(SAL) "Total Sal" 
FROM EMP, DEPT 
WHERE DEPT.DEPTNO = EMP.DEPTNO 
GROUP BY ROLLUP (DNAME, JOB);

--**GROUPING함수** ROLLUP, CUBE, GROUPING SETS등 새로운 그룹함수를 지원.
--ROLLUP이나 CUBE에 의한 소계가 계산된 결과에는 GROUPING(EXPR)=1이 표시되고,
--그 외 결과에는 GROUPING(EXPR)=0이 표시된다.
--집계레코드를 구분할 수 있다.

--GROUPING함수와 CASE/DECODE를 이용해, 소계를 나타내는 필드에 원하는 
--문자열을 지정할 수 있다.  (WHEN/THEN)
SELECT  DECODE(GROUPING(DNAME), 1, 'All Departments', DNAME) AS DNAME, 
        DECODE(GROUPING(JOB), 1, 'All Jobs', JOB) AS JOB, 
        COUNT(*) "Total Empl", 
        SUM(SAL) "Total Sal" 
FROM EMP, DEPT 
WHERE DEPT.DEPTNO = EMP.DEPTNO 
GROUP BY ROLLUP (DNAME, JOB);
```

&nbsp;

-----

## 3.CUBE

결합가능한 모든 값에 대하여 다차원 집계를 생성한다. CUBE를 사용할 경우엔 내부적으로는 grouping columns의 순서를 바꾸어서 또 한번의 쿼리를 추가 수행해야 한다. 뿐만 아니라 grand total은 양쪽의 쿼리에서 모두 생성이 되므로 한 번의 query에서는 제거되어야만 하기 때문에 ROLLUP에 비해 시스템의 연산 대상이 많다.   

grouping columns이 가질 수 있는 모든 경우에 대하여 subtotal을 생성해야 하는 경우 CUBE를 사용하는것이 바람직하나, 시스템에 많은 부담을 주므로 주의해야한다. 표시된 인수들에 대한 계층별 집계를 구할 수 있으며, 이때 표시된 인수들 간에는 계층구조인 ROLLUP과는 달리 평등한 관계이므로 인수의 순서가 바뀌는 경우 데이터 결과는 같다. (순서는 달라질 수 있음).   
결과정렬이 필요한 경우 ORDER BY절을 이용해야 한다.

```sql
SELECT CASE     GROUPING(DNAME) 
                WHEN 1 THEN 'All Departments' 
                ELSE DNAME 
        END AS DNAME, 
        CASE    GROUPING(JOB) 
                WHEN 1 
                THEN 'All Jobs' 
                ELSE JOB
         END AS JOB, 
        COUNT(*) "Total Empl", 
        SUM(SAL) "Total Sal" 
FROM EMP, DEPT 
WHERE DEPT.DEPTNO = EMP.DEPTNO 
GROUP BY CUBE (DNAME, JOB) ;
```

&nbsp;

-----

## 4.GROUPING SETS

GROUPING SETS에 표시된 인수들에 대한 개별 집계를 구할 수 있으며, 이때 표시된 인수들은 계층구조인 ROLLUP과는 달리 평등한 관계이므로 인수의 순서가 바뀌어도 결과는 같다. GROUPING STS함수도 결과에 대한 정렬이 필요한 경우 ORDER BY를 사용한다.

```sql
SELECT  DECODE(GROUPING(DNAME), 1, 'All Departments', DNAME) AS DNAME, 
        DECODE(GROUPING(JOB), 1, 'All Jobs', JOB) AS JOB, 
        COUNT(*) "Total Empl", 
        SUM(SAL) "Total Sal" 
FROM EMP, DEPT 
WHERE DEPT.DEPTNO = EMP.DEPTNO 
GROUP BY GROUPING SETS (JOB, DNAME);

--여러개 인수를 이용한 GROUPING SETS이용
SELECT DNAME, JOB, MGR, SUM(SAL) "Total Sal" 
FROM EMP, DEPT 
WHERE DEPT.DEPTNO = EMP.DEPTNO 
GROUP BY GROUPING SETS ((DNAME, JOB, MGR), (DNAME, JOB), (JOB, MGR)); 

--GROUPING SETS 함수 사용시 괄호로 묶은 집합별로(괄호 내는 계층구조가 아닌 
--하나의 데이터로 간주함) 집계를 구할 수 있다.
```