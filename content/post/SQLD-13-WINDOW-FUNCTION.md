---
title: "SQLD 13 WINDOW FUNCTION"
date: 2020-07-21T21:10:25+09:00
categories:
- database
- sql
tags:
- SQLD
keywords:
- SQLD
---

<!--more-->
# 윈도우 함수 (WINDOW FUNCTION)

&nbsp;

## 1.WINDOW FUNCTION 개요

행과 행간의 관계를 쉽게 정의하기 위해 만든 함수이다.   
분석함수나 순위함수로도 알려져 있으며 데이터웨어하우스에서 발전한 기능이다. 다른 함수와는 달리 중첩(NEST)해서 사용하지는 못하지만, 서브쿼리에서는 사용할 수 있다.

&nbsp;

- **종류**   
크게 다섯개의 그룹으로 분류할 수 있는데, 벤더별로 지원하는 함수에는 차이가 있다.   
1. 그룹 내 순위 관련 함수.   
    RANK | DENSE_RANK | ROW_NUMBER
2. 그룹 내 집계관련 함수.   
    SUM, MAX, MIN, AVG, COUNT
3. 그룹 내 행 순서 관련 함수.   
    FIRST_VALUE, LAST_VALUE, LAG, LEAD
4. 그룹 내 비율 관련 함수.   
    CUME_DIST, PERCENT_RANK, NTILE, RATIO_TO_REPORT
5. 선형 분석을 포함한 통계 분석 관련 함수
    (통계특화 기능이기 때문에 가이드에서는 설명을 생략한다.)

&nbsp;

- **SYNTAX**   
WINDOW함수에는 OVER문구가 키워드로 필수로 포함된다.
```sql
SELECT WINDOW_FUNCTION (ARGUMENTS) OVER
([PARTITION BY 칼럼] [ORDER BY 절] [WINDOWING 절])
FROM 테이블명;

--WINDOW_FUNCTION :
--      기존에 사용하던 함수도 있고, 새롭게 WINDOW함수용으로 추가된 함수도 있다.
--ARGUMENTS(인수) : 
--      함수에 따라 0~N개의 인수가 지정될 수 있다.
--PARTITION BY :
--      전체 집합을 기준에 의해 소그룹으로 나눌 수 있다.
--ORDER BY :
--      어떤 항목에 대해 순위를 지정할 지 기술한다.
--WINDOWING :
--      함수의 대상이 되는 행 기준의 범위를 강력하게 지정할 수 있다.
--      ROWS는 물리적인 결과 행의 수를,
--      RANGE는 논리적인 값에 의한 범위를 나타내는데 
--      둘 중 하나를 선택해서 사용할 수 있다.
```

```sql
BETWEEN 사용 타입 
ROWS | RANGE BETWEEN 
UNBOUNDED PRECEDING | CURRENT ROW | VALUE_EXPR PRECEDING/FOLLOWING 
AND 
UNBOUNDED FOLLOWING | CURRENT ROW | VALUE_EXPR PRECEDING/FOLLOWING 

BETWEEN 미사용 타입 
ROWS | RANGE 
UNBOUNDED PRECEDING | CURRENT ROW | VALUE_EXPR PRECEDING
```


-----

&nbsp;

## 2.그룹 내 순위 함수

&nbsp;

- **RANK함수**   
ORDER BY를 포함한 쿼리문에서 특정 항목(칼럼)에 대한 순위를 구하는 함수이다. 이때 특정범위내에서 순위를 구할 수도 있고 전체 데이터에 대한 순위를 구할 수도 있다. 동일한 값에 대해서는 동일한 순위를 부여하게 된다.

```sql
SELECT  JOB, ENAME, SAL, 
        RANK( ) OVER (ORDER BY SAL DESC) ALL_RANK, 
        RANK( ) OVER (PARTITION BY JOB ORDER BY SAL DESC) JOB_RANK --같은 JOB내에서 순위부여
FROM EMP;

--하나의 SQL문장에 ORDER BY SAL DESC조건과 PARTITION BY JOB 조건이 충돌이 났기 때문에 
--JOB별로는 정렬이 되지 않고 ORDER BY SAL DESC조건으로 정렬이 된다.
```

&nbsp;

- **DENSE_RANK함수**   
RANK함수와 흡사하나, 동일한 순위를 하나의 건수로 취급하는 것이 다르다.
```sql
SELECT  JOB, ENAME, SAL, 
        RANK( )         OVER (ORDER BY SAL DESC) RANK, 
        DENSE_RANK( )   OVER (ORDER BY SAL DESC) DENSE_RANK
FROM EMP;

--동일순위 데이터가 있을 경우, RANK는 그 다음순번이 +1번째, DENSE_RANK는 +중복된수 번째로 밀려난다.
--ex) rank 2가 2개 있을 경우 RANK는 다음순번이 3, DENSE_RANK는 다음순번이 4가 된다.
```

&nbsp;

- **ROW_NUMBER함수**   
RANK / DENSE_RANK함수와 달리 동일한 값도 고유한 순위를 부여한다.
```sql
SELECT  JOB, ENAME, SAL, 
        RANK( )         OVER (ORDER BY SAL DESC) RANK, 
        ROW_NUMBER( )   OVER (ORDER BY SAL DESC) ROW_NUMBER
FROM EMP;

--ROW_NUMBER는 유니크한 순위를 정한다. 같은 데이터내에서는 어떤 순서가 정해질지 알 수 없다. 
--(오라클의 경우 rowid가 적은 행이 먼저 나온다.)
--이 부분은 데이터베이스별로 다른 결과가 나올 수 있으므로, 순서까지 관리하고 싶다면 
--ROW_NUMBER( ) OVER (ORDER BY SAL DESC, ENAME) 와 같이 ORDER BY절을 이용해 추가로 정렬기준을 정의해야한다.
```

-----

&nbsp;

## 3.일반 집계 함수

&nbsp;

- **SUM함수**  
```sql
SELECT  MGR, 
        ENAME, 
        SAL, 
        SUM(SAL) OVER (PARTITION BY MGR RANGE UNBOUNDED PRECEDING) MGR_SUM 
FROM EMP; 
--PARTITION BY MGR 구문을 통해 매니저별로 데이터를 파티션화 한다.
--RANGE UNBOUNDED PRECEDING : 현재 행을 기준으로 파티션 내의 첫 번째 행까지의 범위를 지정한다.
--(같은값은 같은 ORDER로 취급하여 누적합을 중복하지 않는다.)
```

&nbsp;

- **MAX함수**  
```sql
SELECT  MGR, 
        ENAME, 
        SAL 
FROM    (SELECT MGR, 
                ENAME, 
                SAL, 
                MAX(SAL) OVER (PARTITION BY MGR) as IV_MAX_SAL 
        FROM EMP) 
WHERE SAL = IV_MAX_SAL ;

--INLINE VIEW를 이용해 파티션별 최대값을 가진 행만 추출할 수도 있다.
```

&nbsp;

- **MIN함수**  
```sql
SELECT  MGR, 
        ENAME, 
        HIREDATE, 
        SAL, 
        MIN(SAL) OVER(PARTITION BY MGR ORDER BY HIREDATE) as MGR_MIN 
FROM EMP;
```

&nbsp;

- **AVG함수**  
```sql
SELECT  MGR, 
        ENAME, 
        HIREDATE, 
        SAL, 
        ROUND (AVG(SAL) OVER (PARTITION BY MGR ORDER BY HIREDATE 
                ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING)) 
as MGR_AVG
FROM EMP; 

--ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING : 
--  현재 행을 기준으로 파티션 내에서 앞의 한 건, 현재 행, 뒤의 한 건을 범위로 지정한다. 
--  (ROWS는 현재 행의 앞뒤 건수를 말하는 것임)
```

&nbsp;

- **COUNT함수**  
```sql
SELECT ENAME, 
SAL, 
COUNT(*) OVER (ORDER BY SAL RANGE BETWEEN 50 PRECEDING AND 150 FOLLOWING) as SIM_CNT 
FROM EMP; 

/*
RANGE BETWEEN 50 PRECEDING AND 150 FOLLOWING : 
현재 행의 급여값을 기준으로 급여가 -50에서 +150의 범위 내에 포함된 모든 행이 대상이 된다. 
(RANGE는 현재 행의 데이터 값을 기준으로 앞뒤 데이터 값의 범위를 표시하는 것임)
*/
```



-----

&nbsp;

## 4.그룹 내 행 순서 함수

&nbsp;

- **FIRST_VALUE함수**  
파티션별 윈도우에서 가장 먼저 나온 값을 구한다. (SQL server에서는 지원하지 않는 기능).   
MIN함수를 활용하여 같은 결과를 얻을 수도 있다.

```sql
--부서별 연봉이 높은 순으로 정렬하고, 파티션내 가장 먼저나온 값을 출력
SELECT  DEPTNO, 
        ENAME, 
        SAL, 
        FIRST_VALUE(ENAME) OVER (PARTITION BY DEPTNO 
                                ORDER BY SAL DESC, 
                                ENAME ASC 
                                ROWS UNBOUNDED PRECEDING) 
        as DEPT_RICH 
FROM EMP; 

--RANGE UNBOUNDED PRECEDING : 현재 행을 기준으로 파티션 내의 첫 번째 행까지의 범위를 지정한다.
```

FIRST_VALUE는 공동 등수를 인정하지 않고, 처음 나온 행만을 처리하기 때문에 공동 등수가 있고, 의도적으로 세부 항목을 정렬하고 싶다면 별도의 정렬조건을 가진 INLINE VIEW를 사용하거나, OVER()내의 ORDER BY절에 칼럼을 추가해야 한다.

&nbsp;

- **LAST_VALUE함수**  
파티션별 윈도우에서 가장 나중에 나온 값을 구한다. (SQL server에서는 지원하지 않는 기능).   
MAX함수를 활용하여 같은 결과를 얻을 수도 있다. LAST_VALUE도 공동 등수를 인정하지 않는다.

```sql
 SELECT DEPTNO, 
        ENAME, 
        SAL, 
        LAST_VALUE(ENAME) OVER (PARTITION BY DEPTNO 
                                ORDER BY SAL DESC 
                                ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) 
        as DEPT_POOR 
 FROM EMP; 
 
 --ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING: 
 --     현재 행을 포함해서 파티션 내의 마지막 행까지의 범위를 지정한다.
```

&nbsp;

- **LAG함수**  
파티션별 윈도우에서 이전 몇 번째 행의 값을 가져올 수 있다. (SQL server에서는 지원하지 않는 기능).   
3개의 ARGUMENTS까지 사용할 수 있는데, 두 번째 인자는 몇 번째 앞의 행을 가져올지 결정하는 것이고 (DEFAULT 1), 세 번째 인자는 NVL이나 ISNULL기능과 같은 역할이다.

```sql
SELECT  ENAME, 
        HIREDATE, 
        SAL, 
        LAG(SAL, 2, 0) OVER (ORDER BY HIREDATE) as PREV_SAL 
FROM EMP 
WHERE JOB = 'SALESMAN';

--LAG(SAL, 2, 0)의 기능은 두 행 앞의 SALARY를 가져오고, 
--가져올 값이 없는 경우는 0으로 처리한다.
```

&nbsp;

- **LEAD함수**  
파티션별 윈도우에서 이후 몇 번째 행의 값을 가져올 수 있다. (SQL server에서는 지원하지 않는 기능).   

```sql
SELECT  ENAME, 
        HIREDATE, 
        LEAD(HIREDATE, 1) OVER (ORDER BY HIREDATE) as "NEXTHIRED" 
FROM EMP;
```

-----

&nbsp;

## 5.그룹 내 비율 함수

&nbsp;

- **RATIO_TO_REPORT함수**   
파티션 내 전체 SUM(칼럼)값에 대한 행별 칼럼값의 백분율을 소수점으로 구할 수 있다.   
결과값은 > 0 & <= 1 의 범위를 가진다. 그리고 개별 RATIO의 합은 1이 된다. (SQL server에서는 지원하지 않는 기능).   

```sql
SELECT  ENAME, 
        SAL, 
        ROUND(RATIO_TO_REPORT(SAL) OVER (), 2) as R_R 
FROM EMP 
WHERE JOB = 'SALESMAN';
```

&nbsp;

- **PERCENT_RANK함수**   
파티션별 윈도우에서 제일 먼저 나오는 것을 0으로, 제일 늦게 나오는 것을 1로 하여, 값이 아닌 행의 순서별 백분율을 구한다. 결과값은 >= 0 & <= 1 의 범위를 가진다. (SQL server에서는 지원하지 않는 기능)   

```sql
SELECT  DEPTNO, 
        ENAME, 
        SAL, 
        PERCENT_RANK() OVER (PARTITION BY DEPTNO ORDER BY SAL DESC) as P_R 
FROM EMP;
```

&nbsp;

- **CUME_DIST함수**   
파티션별 윈도우의 전체건수에서 현재 행보다 작거나 같은 건수에 대한 누적백분율을 구한다. 결과값은 > 0 & <= 1 의 범위를 가진다. (SQL server에서는 지원하지 않는 기능)   

```sql
SELECT  DEPTNO, 
        ENAME, 
        SAL, 
        CUME_DIST() OVER (PARTITION BY DEPTNO ORDER BY SAL DESC) as CUME_DIST 
FROM EMP;
```

&nbsp;

- **NTILE함수**   
파티션별 전체 건수를 ARGUMENT값으로 N등분한 결과를 구할 수 있다.

```sql
--전체 사원을 급여가 높은 순서로 정렬하고, 급여를 기준으로 4개의 그룹으로 분류한다.
SELECT  ENAME, 
        SAL, 
        NTILE(4) OVER (ORDER BY SAL DESC) as QUAR_TILE 
FROM EMP;

/*
전체 데이터가 14건이라고 할 때, NTILE(4)의 의미는 14명을 4개 조로 나눈다는 것이다.
몫이 3, 나머지가 2명일때 나머지 2명은 앞의 조부터 할당한다.
즉, 4명+4명+3명+3명으로 파티션이 나뉘게 된다.
*/
```