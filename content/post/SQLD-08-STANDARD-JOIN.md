---
title: "SQLD 08 STANDARD JOIN"
date: 2020-07-13T20:45:08+09:00
categories:
- database
- sql
tags:
- SQLD
keywords:
- SQLD
---

<!--more-->

# SQLD 08. 표준 조인 (STANDARD JOIN)

&nbsp;
## 개요

DBMS벤더별로 문법이나 사용되는 용어의 차이가 있으나, ANSI/ISO SQL표준을 통해 많은 기능이 상호 벤치마킹하고 발전하면서 DBMS간의 평준화를 이루어 가고 있다.   
대표적인 표준 기능은 아래 내용을 포함한다.   
&nbsp;
- STANDARD JOIN 기능 추가 (CROSS, OUTER JOIN등 새로운 FROM절 JOIN기능들)   
- SCALAR SUBQUERY, TOP-N QUERY등의 새로운 서브쿼리 기능들   
- ROLLUP, CUBE, GROUPING SETS등의 새로운 리포팅 기능   
- WINDOW FUNCTION같은 새로운 개념의 분석 기능들   
&nbsp;

-----

### 일반 집합 연산자

![external-content duckduckgo](https://user-images.githubusercontent.com/28701069/87416385-98df5580-c609-11ea-9a3f-7f8b1d317d80.png)

E.C.CODD 박사의 논문에 언급된 8가지 관계형 대수는 다시 각각 4개의 일반 집합 연산자와 순수 관계 연산자로 나눌 수 있으며, 관계형 데이터베이스 엔진 및 SQL의 기반 이론이 되었다.   
일반 집합 연산자를 현재의 SQL과 비교하면,   
&nbsp;

1. **UNION 연산은 UNION 기능으로.**   
    UNION 연산은 수학적 합집합을 제공하기 위해, 공통 교집합의 중복을 없애기 위한 사전 작업으로 시스템에 부하를 주는 정렬 작업이 발생한다. 이후 UNION ALL기능의 추가되었는데, 특별한 요구 사항이 없다면 공통집합을 중복해서 그대로 보여주기 때문에 정렬 작업이 일어나지 않는 장점을 가진다. 만일 UNION과 UNION ALL의 출력결과가 같다면, 응답속도 향상이나 자원 효율화 측면에서 UNION ALL을 사용하길 권고한다.
2. **INTERSECTION 연산은 INTERSECT 기능으로.**   
    수학의 교집합으로써, 두 집합의 공통집합을 추출한다.
3. **DIFFERENCE 연산은 EXCEPT(Oracle은 MINUS) 기능으로.**   
    수학의 차집합으로써 첫 번째 집합에서 두 번째 집합과의 공통집합을 제외한 부분이다. 대다수 벤더는 EXCEPT를, 오라클은 MINUS용어를 사용한다. 
4. **PRODUCT 연산은 CROSS JOIN 기능으로.**   
    곱집합으로, JOIN조건이 없는 경우 생길 수 있는 모든 데이터의 조합을 말한다. 양쪽 집합의 M*N건의 데이터 조합이 발생하며, CARTESIAN(수학자 이름) PRODUCT라고도 표현한다.


-----

### 순수 관계 연산자

![이미지 3](https://user-images.githubusercontent.com/28701069/87417924-d47b1f00-c60b-11ea-9851-37fc05a0c820.png)

관계형 데이터베이스를 구현하기 위해 새롭게 만들어진 연산자이다. 순수 관계 연산자를 현재의 SQL문장과 비교하면 다음과 같다.   
&nbsp;

5. **SELECT 연산은 WHERE 절로 구현**   
    SELECT연산은 SQL문장에서는 WHERE절의 조건절 기능으로 구현이 되었다.   
    (주의 : select연산 != select절)
6. **PROJECT 연산은 SELECT 절로 구현**   
    SELECT절의 칼럼 선택 기능으로 구현이 되었다.
7. **(NATURAL) JOIN 연산은 다양한 JOIN기능으로 구현**   
    JOIN연산은 WHERE절의 INNER JOIN조건과 함께 FROM절의 NATURAL JOIN, INNER JOIN, OUTER JOIN, USING조건절, ON조건절 등으로 가장 다양하게 발전하였다.
8. **DIVIDE 연산은 현재 사용되지 않는다**   
    나눗셈과 비슷한 개념으로 왼쪽의 집합을 'XZ'로 나누었을 때, 즉 'XZ'를 모두 가지고 있는 'A'가 답이 되는 기능으로 현재는 사용되지 않는다. 
&nbsp;

RDBMS는 요구사항 분석, 개념적 데이터 모델링, 논리적 데이터 모델링, 물리적 데이터 모델링 단계를 거치게 되는데, 이 단계에서 엔티티 확정 및 정규화 과정, 그리고 M:M(다대다) 관계를 분해하는 절차를 거치게 된다.   
(**정규화** : 데이터 정합성과 데이터 저장 공간의 절약을 위해 엔티티를 최대한 분리하는 작업)   

&nbsp;

-----

## FROM절 JOIN 형태

- INNER JON
- NATURAL JOIN
- USING 조건절
- ON 조건절
- CROSS JOIN
- OUTER JOIN

&nbsp;

-----

## INNER JOIN

내부 JOIN 이라고도 한다. JOIN조건에서 동일한 값이 있는 행만 반환한다.   
INNER JOIN표시는 그 동안 WHERE절에서 사용하던 JOIN조건을 FROM절에서 정의하겠다는 표시이므로 **USING조건절이나 ON조건절을 필수로 사용**해야 한다.

&nbsp;

-----

## NATURAL JOIN

두 테이블 간의 동일한 이름을 갖는 모든 칼럼들에 대해 EQUI(=) JOIN을 수행한다.   
NATURAL JOIN이 명시되면, 추가로 USING조건절, ON조건절, WHERE 절에서 JOIN조건을 정의할 수 없다.   
SQL server에서는 지원하지 않는 기능이다.

```sql
SELECT DEPTNO, EMPNO, ENAME, DNAME
FROM EMP NATURAL JOIN DEPT;

--별도의 JOIN칼럼을 지정하지 않았지만, 
--두 개의 테이블에서 DEPTNO라는 공통된 칼럼을 자동으로 인식하여 JOIN을 처리한 것이다. 
--JOIN에 사용된 칼럼들은 같은 유형이어야하며, 
--ALIAS나 테이블명과 같은 접두사를 붙일 수 없다. 
--(EMP.DEPTNO 나 A.DEPTNO 사용 불가)
```

- '*' 와일드카드처럼 별도의 칼럼 순서를 지정하지 않으면 NATURAL JOIN의 기준이 되는 칼럼들이 다른 칼럼보다 먼저 출력된다.   
- 동일한 칼럼명이라도 다른 용도의 데이터를 저장하는경우도 있으니 주의해서 사용해야한다.   
- NATURAL JOIN은 JON에 사용된 같은 이름의 칼럼을 **하나로 처리**하지만, INNER JOIN은 **2개의 칼럼**으로 표시한다.

&nbsp;

-----

## USING 조건절

NATURAL JOIN에서는 모든 일치되는 칼럼들에 대해 JOIN이 이루어지지만, FROM절의 USING조건절은 같은 이름을 가진 칼럼들 중에서 원하는 칼럼에 대해서만 선택적으로 EQUI JOIN을 할 수가 있다.   
SQL server에서는 지원하지 않는 기능이다.

```sql
SELET *
FROM DEPT JOIN DEPT_TEMP
USING (DEPTNO);

-- USING절의 열 부분은 식별자를 가질 수 없다. 
-- DEPT.DEPTNO, DEPT.DNAME (X)
-- DEPTNO, DEPT.DNAME (O)
```

- '*' 와일드카드처럼 별도의 칼럼 순서를 지정하지 않으면 USING조건절의 기준이 되는 칼럼들이 다른 칼럼보다 먼저 출력된다.   
- USING 조건절도 JOIN칼럼에 대해서는 ALIAS나 테이블명과 같은 접두사를 붙일 수 없다. 

&nbsp;

-----

## ON 조건절

JOIN서술부(ON조건절)와 비 JOIN서술부(WHERE 조건절)을 분리하여 이해가 쉬우며, 칼럼명이 다르더라도 JOIN조건을 사용할 수 있는 장점이 있다.

```sql
SELECT E.EMPNO, E.ENAME, D.DEPTNO, D.DEPTNM
FROM EMP E JOIN DEPT D
ON (E.DEPTNO = D.DEPTNO);
```

이름이 다른 칼럼명을 JOIN조건으로 사용하거나, JOIN칼럼을 명시하기 위해 사용한다.   
ON 조건절을 사용한 JOIN의 경우는 ALIAS나 테이블 명과 같은 접두사를 자용하여 SELECT에 사용되는 칼럼을 논리적으로 명확하게 지정해주어야 한다.   
WHERE절의 JOIN조건과 같은 기능을 하면서도, 명시적으로 JOIN조건을 구분할 수 있어 많이 사용된다.   
FROM절에 테이블이 많이 사용될 경우 다소 복잡해보여 가독성이 떨어지는 단점이 있다.  

&nbsp;

- ON조건절과 WHERE검색 조건은 충돌 없이 사용할 수 있다.
```sql
SELECT E.EMPNO, E.ENAME, D.DEPTNO, D.DEPTNM
FROM EMP E JOIN DEPT D
ON (E.DEPTNO = D.DEPTNO)
WHERE E.DEPTNO = '30';
```

- ON조건절 + 데이터 검증 조건 추가 사용 가능.
```sql
SELECT E.EMPNO, E.ENAME, D.DEPTNO, D.DEPTNM
FROM EMP E JOIN DEPT D
ON (E.DEPTNO = D.DEPTNO AND E.MGR = 9999);
```

&nbsp;

-----

## CROSS JOIN

E.F.CODD박사가 언급한 일반 집합 연산자의 PRODUCT개념으로 테이블 간 JON조건이 없는 경우 생길 수 있는 모든 데이터의 조합을 말한다. 정상적인 데이터 모델이라면 필요한 경우가 많지 않지만, 간혹 튜닝이나 리포트를 작성하기 위해 고의적으로 사용하는 경우가 있을 수 있다.

```sql
SELECT ENAME, DNAME
FROM EMP CROSS JOIN DEPT
ORDER BY ENAME;

--EMP 건수 * DEPT 건수 만큼 결과 출력
```

- WHERE절에 JOIN조건을 추가할 수 있다. 그러나 이 경우는 INNER JOIN과 같은 결과를 얻기 때문에 CROSS JOIN을 사용하는 의미가 없으지므로 권고하지 않는다.   
- 데이터웨어하우스의 개별 DIMENSION을 FACT칼럼과 JOIN하기 전에 모든 DIMENSION의 CROSS PRODUCT를 먼저 구할 때 유용하게 사용할 수 있다.

&nbsp;

-----

## OUTER JOIN

JOIN조건에서 동일한 값이 없는 행도 반환할 때 사용할 수 있다. STANDARD OUTER JOIN을 사용하므로써 대부분의 RDBMS간의 호환성을 확보할 수 있으므로 명시적인 OUTER JOIN을 사용할 것이 권장된다.   
OUTER JOIN역시 JOIN조건을 FROM절에서 정의하겠다는 표시이므로 **USING조건절이나 ON조건절을 필수적으로 사용**해야 한다.   

LEFT/RIGHT OUTER JOIN의 경우 기준의 되는 테이블이 조인 수행시 무조건 드라이빙 테이블이 된다. 옵티마이저는 이 원칙에 위배되는 다른 실행계획을 고려하지 않는다.

&nbsp;

- LEFT OUTER JOIN   
수행시 먼저 표기된 좌측 테이블에 해당하는 데이터를 먼저 읽은 후, 나중 표기된 우측 테이블에서 JOIN대상 데이터를 읽어 온다. LEFT JOIN으로 OUTER키워드를 생략해서 사용할 수 있다.
```sql
SELECT E.ENAME, D.DNAME, E.ID
FROM EMP E LEFT JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

&nbsp;

- RIGHT OUTER JOIN   
LEFT JOIN과 반대로 우측 테이블이 기준이 되어 결과를 생성한다. RIGHT JOIN으로 OUTER키워드를 생략할 수 있다.
```sql
SELECT E.ENAME, D.DNAME, E.ID
FROM EMP E RIGHT OUTER JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

&nbsp;

- FULL OUTER JOIN   
조인 수행시 좌/우 테이블의 모든 데이터를 읽어 JOIN하여 결과를 생성한다. (합집합)   
단, UNION ALL이 아닌 UNION기능과 같으므로 중복되는 데이터는 삭제한다. FULL JOIN으로 OUTER키워드를 생략할 수 있다. 
```sql
SELECT E.ENAME, D.DNAME, E.ID
FROM EMP E FULL OUTER JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

&nbsp;

-----

## INNER vs OUTER vs CROSS JOIN 비교

![external-content duckduckgo](https://user-images.githubusercontent.com/28701069/87423814-e235a200-c615-11ea-8cc7-6822d10d816a.jpg)

- **INNER JOIN**   
 양쪽 테이블 모두 존재하는 B-B, C-C인 2건 출력

- **LEFT OUTER JOIN**   
 TAB1기준으로 키 값 조합이 B-B, C-C, D-NULL, E-NULL인 4건 출력

- **RIGHT OUTER JOIN**   
 TAB2기준으로 키 값 조합이 NULL-A, B-B, C-C인 3건 출력

- **FULL OUTER JOIN**   
  양 테이블 기준으로 NULL-A, B-B, C-C, D-NULL, E-NULL 인 5건 출력

- **CROSS JOIN**   
 JOIN가능한 모든 경우의 수를 표시하지만 단, OUTER JOIN은 제외한다.   
 양쪽 테이블 TAB1, TAB2의 데이터를 곱한 3*4=12건 출력
 키 값 조합은 B-A, B-B, B-C. C-A, C-B, C-C, D-A, D-B, D-C, E-A, E-B, E-C인 12건이다.