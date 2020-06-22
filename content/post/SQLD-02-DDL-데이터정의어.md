---
title: "SQLD 02 DDL 데이터정의어"
date: 2020-06-18T22:10:17+09:00
categories:
- SQLD
- 2과목 SQL 기본 및 활용
tags:
- SQLD
keywords:
- SQLD
---

<!--more-->


# SQLD 02. DDL 데이터 정의어 (Data definition language)


### 1.데이터 유형

데이터베이스의 테이블에 특정자료를 입력할 때, 자료를 받아들일 공간을 자료의 유형별로 나누는 기준.

- **CHARACTER** : 고정길이 문자열 정보   
- **VARCHAR** : 가변길이 문자열 정보   
- **NUMERIC** : 정수, 실수 등 숫자 정보   
- **DATE** : 날짜와 시각 정보   



### 2.CREATE TABLE

```SQL
CREATE TABLE 테이블이름 (
	칼럼명1 DataType [default형식],
	칼럼명2 DataType [default형식]
);
```


테이블 생성시에는 해당 테이블에 입력될 데이터를 정의하고, 정의한 데이터를 어떠한 데이터 유형으로 선언할 것인지를 결정해야 한다.

- 기본키 : 고유식별   
- 기본키 & 외래키 : 테이블간 관계 설정

>**Constraint**
>사용자가 원하는 조건의 데이터만 유지하기 위한, 즉 데이터의 무결성을 유지하기 위한 방법으로 테이블의 특정 칼럼에 설정하는 제약    
>종류로는 PRIMARY KEY / UNIQUE KEY / NOT NULL / CHECK / FOREIGN KEY 가 있다.   
> 1. 칼럼레벨 정의방식   
> 2. 테이블레벨 정의방식   



### 3.ALTER TABLE
```SQL
ALTER TABLE 테이블이름 
ADD 추가할 칼럼명   데이터 유형;   
```
칼럼 추가/삭제, 제약조건 추가/삭제   

- **ADD COLUMN** : 컬럼추가 (맨 마지막 컬럼이 된다. 위치 지정 불가)
- **DROP COLUMN** : 컬럼삭제 (데이터 여부 관계없이 모두 삭제 가능) 한 번에 한 개만 가능.
- **MODIFY COLUMN** : 데이터 유형, default값, not null제약조건 변경 포함, 컬럼정의 변경
- **RENAME COLUMN** : 컬럼명 변경 (일부 DBMS만 지원하는 기능)
- **DROP CONSTRAINT** : 제약조건 삭제
- **ADD CONSTRAINT** : 제약조건 추가



### 4.RENAME TABLE
```SQL
RENAME 변경전 테이블명 TO 변경후 테이블명;
```

테이블의 이름 변경



### 5.DROP TABLE
```SQL
DROP TABLE 테이블이름 [CASCADE CONSTRAINT]; 
```
테이블 삭제 (완전 구조 삭제)


### 6.TRUNCATE TABLE
```SQL
TRUNCATE TABLE 테이블명;
```
테이블 내 모든 행 제거. 저장공간 재사용이 가능하도록 해제(즉, 데이터만 전체 삭제)   
처리방식과 Auto commit때문에 DML이 아닌 DDL로 분류. Delete와는 처리방식이 다르며 복구가 불가능하다.

