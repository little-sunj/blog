---
title: "SQLD 05 GROUP BY"
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
- GROUP BY 절에서는  ALIAS명을 사용할 수 없다
- 집계 함수는 WHERE절에는 올 수 없다. (WHERE절이 먼저 수행)
- WHERE절은 전체 데이터를 GROUP으로 나누기 전에 행들을 미리 제거시킨다.
- HAVING절은 GROUP BY절의 기준 항목이나 소그룹의 집계 함수를 이용한 조건을 표시할 수 있다.
-  GOUP BY절에 의한 소그룹별로 만들어진 집계 데이터 중, HAVING절에서 제한 조건을 두어 조건을 만족하는 내용만 출력한다.
- HAVING절은 일반적으로 GROUP BY절 뒤에 위치한다.