---
title: "SQLD 09 SET OPERATOR"
date: 2020-07-15T20:30:24+09:00
categories:
- SQLD
- 2과목 SQL 기본 및 활용
tags:
- SQLD
keywords:
- SQLD
---

<!--more-->

# SQLD 09. 집합 연산자 (SET OPERATOR)

2개 이상의 테이블에서 조인을 사용하지 않고 연관된 데이터를 조회할 수 있는 또 하나의 방법이다.   
집합 연산자는 여러개의 질의의 결과를 연결하여 하나로 결합하는 방식을 사용한다. 즉, 2개 이상의 질의 결과를 하나의 결과로 만들어 준다.   
일반적으로 서로 다른 테이블에서 유사한 형태의 결과를 반환하는 것을 하나의 결과로 합치고자 할 때와 동일 테이블에서 서로 다른 질의를 수행하여 결과를 합치고자 할 때 사용할 수 있다. 이 외에도 튜닝관점에서 실행계획을 분리하고자 하는 목적으로도 사용할 수 있다.   

&nbsp;

**사용 제약조건**
1. SELECT절의 칼럼 수가 동일할 것
2. SELECT절의 동일 위치에 존재하는 칼럼의 데이터 타입이 상호 호환 가능할 것   

제약조건만 만족한다면 어떤 형태의 SELECT문이라도 이용할 수 있다. 집합연산자는 여러개의 SELECT문을 연결하는 것에 지나지 않는다.   


&nbsp;

![SQL_204](https://user-images.githubusercontent.com/28701069/87540740-250d7d80-c6db-11ea-87d2-02a81adb2673.jpg)

![SQL_205](https://user-images.githubusercontent.com/28701069/87540895-58e8a300-c6db-11ea-8899-2eb2aa51ced1.jpg)

&nbsp;

- UNION ALL을 제외한 다른 집합 연산자에서는 SQL문의 **결과 집합에서 먼저 중복된 건을 배제하는 작업을 수행한 후에 집합 연산을 적용**한다. (논리적 관점의 처리)   
- EXCEPT 연산에서는 순서가 중요하다. (앞의 집합의 결과에서 뒤의 집합의 결과를 빼는 것)    
- INTERSECT 연산자는 EXISTS 또는 IN 서브쿼리를 이용한 SQL문으로도 변경 가능하다.
- MINUS 연산자는 NOT EXISTS 또는 NOT IN 서브쿼리를 이용한 SQL문으로도 변경 가능하다.   

&nbsp;

```sql
--SQL문의 형태는 아래와 같다.

SELECT '칼럼명2', '칼럼명2', ...
FROM '테이블명1'
[WHERE '조건식']
[GROUP BY '칼럼이나 표현식']
[HAVING '그룹조건식']

'집합연산자'

SELECT '칼럼명2', '칼럼명2', ...
FROM '테이블명2'
[WHERE '조건식']
[GROUP BY '칼럼이나 표현식']
[HAVING '그룹조건식']

ORDER BY 1,2; --최종 결과에 대한 정렬처리이므로 맨 마지막에 한번만 기술

--결과를 표시할 때 HEADING부분은 첫 번째 SQL문에서 사용된 HEADING이 적용된다.
```

