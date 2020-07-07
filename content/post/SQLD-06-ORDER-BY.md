---
title: "SQLD 06 ORDER BY"
date: 2020-07-07T22:00:52+09:00
categories:
- SQLD
- 2과목 SQL 기본 및 활용
tags:
- SQLD
keywords:
- SQLD
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# SQLD 06. ORDER BY

### ORDER BY 정렬

특정 칼럼을 기준으로 정렬하여 출력하는데 사용.   
별도로 지정하지 않으면 기본적으로 오름차순이 적용된다.   
SQL문장의 맨 마지막에 위치한다.
```sql
SELECT 칼럼명
FROM 테이블명
WHERE 조건
GROUP BY 칼럼이나 표현식
HAVING 그룹조건
ORDER BY 칼럼이나 표현식

ASC : 오름차순
DESC : 내림차순
```

오라클에서는 NULL값을 가장 작은 값으로 간주하여 오름차순 정렬시 맨 먼저 오게된다.   

-----

### SELECT 문장 실행 순서

GROUP BY절과 ORDER BY가 같이 사용될 때 SELECT문장은 6개의 절로 구성된다.   
수행 단계는 아래와 같다.

```sql
5  SELECT 칼럼명 
1  FROM 테이블명  
2  WHERE 조건
3  GROUP BY 칼럼이나 표현식
4  HAVING 그룹조건
6  ORDER BY 칼럼이나 표현식

--1. 발췌 대상 테이블을 참조한다. (FROM)
--2. 발췌 대상 데이터가 아닌 것은 제거한다. (WHERE)
--3. 행들을 소그룹화 한다. (GROUP BY)
--4. 그룹핑된 값의 조건에 맞는 것만을 출력한다. (HAVING)
--5. 데이터 값을 출력/계산한다. (SELECT)
--6. 데이터를 정렬한다. (ORDER BY)
```

- 이는 옵티마이저가 문장의 SYNTAX, SEMANTIC에러를 점검하는 순서이기도 하다.    
*예) FROM 절에 정의되지 않은 테이블 칼럼을 WHERE절에서 사용하면 에러 발생*
   
- **ORDER BY 절에는 SELECT목록에 나타나지 않은 문자형 항목이 포함될 수 있다**.    
관계형 DB가 데이터를 메모리에 올릴 때 행 단위로 모든 칼럼을 가져오게 되므로, SELECT절에서 일부 칼럼만 선택하더라도 ORDER BY절에서 메모리에 올라와 있는 다른 칼럼의 데이터를 사용할 수 있기 때문이다.   
(단, SELECT DISTINCT를 지정하거나 SQL문장에 GROUP BY절이 있거나 또는 SELECT 문에 UNION연산자가 있으면 열 정의가 SELECT 목록에 표시되어야 한다.)   

- **서브쿼리의 SELECT절에서 선택되지 않은 칼럼들은 서브쿼리 범위를 벗어나면 더 이상 사용할 수 없다. (인라인 뷰도 동일)**   

- **GROUP BY 절에서 그룹핑 기준을 정의하게 되면, 그룹핑 기준에 사용된 칼럼과 집계 함수에 사용 될 수 있는 숫자형 데이터 칼럼들의 집합을 새로 만든다.**   
개별 데이터는 저장하지 않는다. (사용시 에러 발생)   

>SELECT절에서는 그룹핑 기준과 숫자 형식 칼럼의 집계 함수를 사용할 수 있지만. 그룹핑 기준외의 문자 형식 칼럼은 정할 수 없다.

```sql
--오류예시

SELECT JOB, SAL
FROM EMP
GROUP BY JOB
HAVING COUNT(*) > 0
ORDER BY SAL;

--ERROR : SELECT JOB, SAL -- GROUP BY 표현식이 아니다.


SELECT JOB
FROM EMP
GROUP BY JOB
HAVING COUNT(*) > 0
ORDER BY SAL;

--ERROR : ORDER BY SAL -- GROUP BY 표현식이 아니다.
```

```sql
--정상예시
SELECT JOB
FROM EMP
GROUP BY JOB
HAVING COUNT(*) > 0
ORDER BY MAX(EMPNO), MAX(MGR), SUM(SAL), COUNT(DEPTNO), MAX(HIREDATE);

--JOB
-------
--MANAGER
--PRESIDENT
--SALESMAN
--ANALYST
--CLERK
```

-----

### TOP N 쿼리

- ROWNUM
오라클에서는 데이터의 일부가 먼저 추출된 후 데이터에 대한 정렬 작업이 일어난다.   
그렇기 때문에, 순위가 높은 N개의 ROW를 추출하기 위해 ORDER BY절과 WHERE절의 ROWNUM 조건을 같이 사용하는 걸로는 원하는 결과를 얻을 수 없으므로 주의해야한다.

```sql
--잘못 작성된 쿼리 예시
SELECT ENAME, SAL
FROM EMP
WHERE ROWNUM <4
ORDER BY SAL DESC;

--이 실행결과는 상위 3명을 출력한 것이 아니라, 무작위로 추출된 3명에 한해서 급여를 내림차순으로 정렬한 결과이다.

--ORDER BY절이 사용되는 경우 오라클은 ROWNUM조건을 ORDER BY보다 먼저 처리되는 WHERE절에서 처리한다.
```

```sql
-- 올바른 예시
-- 위 예시에서 원하는 데이터를 얻기 위해서는 인라인 뷰에서 먼저 데이터 정렬을 수행한 후 메인쿼리에서 ROWNUM조건을 사용해야 한다.

SELECT ENAME, SAL
FROM (
	SELECT ENAME, SAL
	FROM EMP
	ORDER BY SAL DESC
	)
WHERE ROWNUM <4;

/*
추가로, 원하는 추출 결과와 동일한 순서로 정렬된 인덱스가 존재한다면 그 인덱스를 사용하여 동일한 결과를 얻을 수도 있다.
*/
```

----

- TOP N
SQL Server는 TOP조건을 사용하게 되면 별도 처리 없이 관련 ORDER BY절의 데이터 정렬 후 원하는 일부 데이터만 쉽게 출력 할 수 있다.

```sql
TOP (Expression) [PERCENT] [WITH TIES]
```
> - **WITH TIES 옵션은 동일 수치의 데이터를 추가로 더 추출한다.**   
> ORDER BY절의 조건 기준으로   
>TOP N의 마지막 행으로 표시되는 추가 행의 데이터가 같을 경우   
>N+ 동일 정렬 순서 데이터를 추가 반환하도록 지정하는 옵션이다.  




```sql
--예시
SELECT TOP(2) WITH TIES ENAME, SAL
FROM EMP
ORDER BY SAL DESC;

/*
사원테이블에서 급여가 높은 2명을 내림차순으로 출력하는데
같은 급여를 받는 사원이 있으면 같이 출력한다.
*/
```