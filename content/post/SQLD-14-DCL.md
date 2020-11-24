---
title: "SQLD 14 DCL"
date: 2020-07-22T20:01:00+09:00
categories:
- database
- sql
tags:
- SQLD
keywords:
- SQLD
---

<!--more-->
# DCL(DATA CONTROL LANGUAGE)

&nbsp;

## 1.DCL(DATA CONTROL LANGUAGE) 개요

유저를 생성하고 권한을 제어할 수 있는 명령어.

-----

&nbsp;

## 2.유저와 권한

대부분의 DB는 데이터 보호와 보안을 위해서 유저와 권한을 관리하고 있다. 

![SQL_223](https://user-images.githubusercontent.com/28701069/88169144-893dbd80-cc56-11ea-80de-b1dff36bde25.jpg)


오라클과 SQL server의 사용자에 대한 아키텍처는 다른 면이 많다.   
**오라클**은 유저를 통해 DB에 접속을 하는 형태이다. 아이디와 비밀번호 방식으로 인스턴스에 접속을 하고 그에 해당하는 스키마에 오브젝트 생성등의 권한을 부여받게 된다.   

**Sql server**는 인스턴스에 접속하기 위해 로그인이라는 것을 생성하게 되며, 인스턴스 내에 존재하는 다수의 데이터베이스에 연결하여 작업하기 위해 유저를 생성한 후 로그인과 유저를 매핑해 주어야한다. 특정 유저는 특정 DB내의 특정 스키마에 대해 권한을 부여받을 수 있다. SQL server로그인은 2가지 방식으로 가능하다.   

1. Windows인증방식 : Windows에 로그인한 정보를 가지고 접속하는 방식이다. Microsoft Windows사용자 계정을 통해 연결되면 SQL Server는 운영 체제의 Windows보안 주체 토큰을 사용해 이름과 암호가 유효한지 확인한다. (즉, windows에서 사용자 ID를 확인) SQL server는 암호를 요청하지 않으며 ID의 유효성 검사를 수행하지 않는다. Windows인증은 기본 인증 모드이며 SQL Server인증보다 훨씬 더 안전하다. Kerberos보안 프로토콜을 사용하고, 암호 정책을 적용하여 강력한 암호에 대해 적합한 복잡성 수준을 유지하도록 하며, 계정 잠금 및 암호 만료를 지원한다. SQL server가 Windows에서 제공하는 자격 증명을 신뢰하므로 Windows인증을 사용한 연결을 트러스트된 연결이라고도 한다.
2. 혼합 모드(Windows인증 or SQL 인증)방식 : 기본적으로 Windows인증으로도 SQL server에 접속 가능하며, 오라클의 인증과 같은 방식으로 사용자 아이디와 비밀번호로 접속하는 방식이다.   

![SQL_224](https://user-images.githubusercontent.com/28701069/88169935-bfc80800-cc57-11ea-8e2a-4b5d40cf268c.jpg)

-----

&nbsp;

- **유저 생성과 시스템 권한 부여**

모든 DDL문장 (CREATE / ALTER / DROP / RENAME등)은 그에 해당하는 권한이 있어야 실행할 수 있다. 이러한 권한을 시스템 권한 이라고 한다. 일반적으로 일일히 유저에게 부여하기보단 ROLE을 이용해 간편하게 부여하게 된다.   

새로운 유저를 생성하려면 유저 생성 권한이 있어야한다.
```sql
GRANT CREATE USER TO SCOTT; 
--권한 부여

CONN SCOTT/TIGER
--DB연결

CREATE USER SUN IDENTIFIED BY PASSWORD11;
--사용자 생성
```

유저가 생성되었지만 아무 권한도 부여받지 못했다면 로그인시 CREATE SESSION권한이 없다는 오류가 발생한다. 
```sql
--CREATE SESSION권한 부여
CONN SCOTT/TIGER 
--연결. 
GRANT CREATE SESSION TO SUN; 
--권한부여. 
CONN SUN/PASSWORD11 
--연결.
```

테이블생성 권한 부여
```sql

CONN SYSTEM/MANAGER 
--연결되었다. 
GRANT CREATE TABLE TO SUN; 
--권한이 부여되었다. 
CONN SUN/PASSWORD11 
--연결되었다. 
CREATE TABLE MENU ( 
    MENU_SEQ NUMBER NOT NULL, 
    TITLE VARCHAR2(10)
     ); 
--테이블이 생성되었다.
```

-----

&nbsp;

- **OBJECT에 대한 권한 부여**

특정유저가 소유한 객체 권한. 오브젝트 권한은 테이블, 뷰등에 대한 SELECT / INSERT / DELETE / UDPATE 작업 명령어를 의미한다.

![SQL_225](https://user-images.githubusercontent.com/28701069/88171313-159daf80-cc5a-11ea-832f-a95e92d2dbd4.jpg)

모든 유저는 자신이 생성한 테이블 외에 다른 유저의 테이블에 접근하려면 해당 테이블에 대한 오브젝트 권한을 소유자로부터 부여받아야 한다. 

```sql
--SELECT 권한 부여
GRANT SELECT ON MENU TO SCOTT;

--오브젝트 권한은 SELECT / INSERT / DELETE / UDPATE 등의 권한을 따로따로 관리한다.
```

-----

&nbsp;

## 3.ROLE을 이용한 권한 부여

유저를 생성하면 앞서보았듯 많은 권한을 부여해야 한다.   
관리자는 유저가 생성될 때마다 이 작업을 수행해야 하므로 번거로울 것이다. 이 문제를 줄이기 위한 것이 ROLE이다. 관리자 ROLE을 생성해 각종 권한을 부여한 후 ROLE을 다른 ROLE이나 유저에게 부여할 수 있다.   
ROLE에는 시스템 권한과 오브젝트 권한을 모두 부여할 수 있으며, 유저에게 직접 부여될 수도 있고, 다른 ROLE에 포함하여 유저에게 부여될 수도 있다.

![SQL_226](https://user-images.githubusercontent.com/28701069/88171758-db80dd80-cc5a-11ea-8d0c-9b8ed1d32245.jpg)

```sql
REVOKE CREATE SESSION, CREATE TABLE FROM SUN;
--권한 취소는 REVOKE를 사용한다.

CONN SYSTEM/MANAGER 
--연결되었다. 
CREATE ROLE LOGIN_TABLE; 
--롤이 생성되었다. 
GRANT CREATE SESSION, CREATE TABLE TO LOGIN_TABLE; 
--권한이 부여되었다. 
GRANT LOGIN_TABLE TO JISUNG; 
--권한이 부여되었다. 
CONN JISUNG/KOREA7 
--연결되었다. 
CREATE TABLE MENU2( MENU_SEQ NUMBER NOT NULL, TITLE VARCHAR2(10)); 
--테이블이 생성되었다.

```

자주 사용하는 ROLE로는 CONNECT와 RESOURCE가 있다.

![SQL_227](https://user-images.githubusercontent.com/28701069/88172318-bf317080-cc5b-11ea-8bcd-1b905882d4a9.jpg)

CONNECT는 CREATE SESSION과 같은 로그인 권한이 포함되어 있고, RESOURCE는 CREATE TABLE과 같은 오브젝트의 생성 권한이 포함되어있다. 일반적으로 이 2개를 사용하여 기본 권한을 부여한다.   

유저를 삭제하는 명령어는 DROP USER이고, CASCADE옵션을 주면 해당 유저가 생성한 오브젝트를 먼저 삭제한 후 유저를 삭제한다.


SQL Server에서는 위와 같이 ROLE을 생성하여 사용하기보단 기본적으로 제공되는 ROLE에 멤버로 참여하는 방식이다. 
![SQL_228](https://user-images.githubusercontent.com/28701069/88172576-2cdd9c80-cc5c-11ea-915e-d41ccc4bebb8.jpg)

![SQL_229](https://user-images.githubusercontent.com/28701069/88172572-2bac6f80-cc5c-11ea-9d8c-236a6f7a4cd7.jpg)