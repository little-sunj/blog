---
title: "SQLD 05 집계 함수"
date: 2020-07-05T20:08:42+09:00
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

# SQLD 05. 집계

### 집계 함수(Aggregate Function)

여러 행들의 그룹이 모여서 그룹당 단 하나의 결과를 돌려주는 다중행 함수

- GROUP BY절은 행들을 소그룹화 한다.
- SELECT, HAVING, ORDER BY 절에 사용할 수 있다.

> - ALL : Default옵션이므로 생략 가능
> - DISTINCT : 같은 값을 하나의 데이터로 간주할 때 사용

![SQL_193](https://user-images.githubusercontent.com/28701069/86531807-63e34c80-beff-11ea-836f-ab2444926812.jpg)

집계함수는 그룹에 대한 정보를 제공하므로 주로 숫자유형에 사용되지만, MAX, MIN, COUNT함수는 문자, 날짜 유형에도 적용이 가능한 함수이다.

-----
### GROUP BY

FROM절과 WHERE절 뒤에 오며, 데이터들을 작은 그룹으로 분류하여 소그룹에 대한 항목별로 통계 정보를 얻을 때 추가로 사용된다.

```SQL
SELECT [DISTINCT] 칼럼명 [ALIAS명]
FROM 테이블명
[WHERE 조건식]
[GROUP BY 칼럼이나 표현식]
[HAVING 그룹조건식]
```

- GROUP BY 절을 통해 소그룹별 기준을 정한 후, SELECT절에 집계 함수를 사용한다.
- 집계 함수의 통계 정보는 NULL값을 가진 행을 **제외**하고 수행한다.
- GROUP BY 절에서는  ALIAS명을 사용할 수 없다(SELECT절과 ORDER BY절에서만 재활용)
- 집계 함수는 WHERE절에는 올 수 없다. (WHERE절이 먼저 수행)
- WHERE절은 전체 데이터를 GROUP으로 나누기 전에 행들을 미리 제거시킨다.
- HAVING절은 GROUP BY절의 기준 항목이나 소그룹의 집계 함수를 이용한 조건을 표시할 수 있다.
-  GOUP BY절에 의한 소그룹별로 만들어진 집계 데이터 중, HAVING절에서 제한 조건을 두어 조건을 만족하는 내용만 출력한다.
- HAVING절은 일반적으로 GROUP BY절 뒤에 위치한다.


-----
### HAVING

WHERE절에서는 집계함수를 사용할 수 없다. FROM절에 정의된 집합(주로 테이블)의 개별 행에 WHERE절의 조건절이 먼저 적용되고, WHERE절의 조건에 맞는 행이 GROUP BY절의 대상이 된다.   
그런 다음 결과 집합의 행에 HAVING조건절이 적용된다.   
HAVING은 WHERE절과 비슷하지만 **그룹을 나타내는 결과 집합의 행에 조건이 적용된다는 점**에서 차이가 있다.   

```sql
SELECT POSITION P, ROUND(AVG(HEIGHT),2) AVG_H
FROM PLAYER
GROUP BY POSITION
HAVING AVG(HEIGHT) >= 180;

--GROUP BY 와 HAVING절의 순서를 지키는 것을 권고.
--논리적으로 그룹핑되어 통계 정보가 만들어지고, 이후 적용된 결과 값에 대한 조건적용이므로
```

- GROUP BY연산 전 WHERE절에서 조건을 적용하여 필요 데이터만 추출하여 연산하는 방법
- GROUP BY연산 후 HAVING절에서 필요한 데이터만 필터링 하는 방법
> 위 2가지는 같은 실행 결과를 얻을 수 있으나, 가능하면 WHERE절에서 조건절을 적용하여 GROUP BY의 계산 대상을 줄이는 것이 효율적인 자원 사용에 유리하다.
   
>**주의할 점**
>- WHERE절의 조건 변경은 대상 데이터의 개수가 변경 = 결과 데이터 값이 변경   
>- HAVING절의 조건 변경은 결과 데이터 변경은 X, 출력 레코드 개수만 변경.    


<br/>

-----
### CASE 표현 활용

집계함수 (CASE( )) ~ GROUP BY 기능은, 모델링의 제1정규화로 인해 반복되는 칼럼의 경우 구분 칼럼을 두고 여러 개의 레코드로 만들어진 집합을, 정해진 칼럼 수만큼 확장해서 집계 보고서를 만드는 유용한 기법이다.

-----
### 집계 함수와 NULL처리

- 다중 행 함수는 입력 값으로 전체 건수가 NULL값인 경우만 함수 결과가 NULL이 나온다.   
- 전체 건수 중에서 일부만 NULL인 경우, NULL인 행을 다중 행 함수의 대상에서 제외한다.   
*Ex) 100명 중 10명의 성적이 NULL일때 평균을 구하면(AVG) 90명의 성적에 대해 평균값을 구하게 된다.*   

CASE표현시 ELSE절 생락 -> DEFAULT값이 NULL이다.   
같은 결과를 얻을 수 있따면 가능한 ELSE절의 상수값을 지정하지 않거나, ELSE절을 작성하지 않는 것이 자원 절약에 유리하다.   
```sql
SUM(NVL(SAL, 0)) --이렇게 작성하면 데이터 건수만큼 연산이 일어나게 된다.
-- 개별 데이터의 SAL값이 NULL인 경우 자동으로 SUM 연산에서 빠지는데, 불필요하게 0로 변환시키는 셈이다.

--NULL이 아닌 0으로 표시하고 싶은 경우,
NVL(SUM(SAL), 0) --처럼 전체 SUM의 결과가 NULL인 경우에만 한 번 NVL함수를 사용하면 된다.


--예시1 (데이터가 없을시 0으로 표시)
SELECT TEAM_ID,
	NVL(SUM(CASE POSITION WHEN 'FW' THEN 1 END), 0) FW
FROM PLAYER
GROUP BY TEAM_ID;
--예시2 (데이터가 없을시 0으로 표시)
SELECT TEAM_ID,
	NVL(SUM(CASE WHEN POSITION ='FW' THEN 1 END), 0) FW
FROM PLAYER
GROUP BY TEAM_ID;
```


