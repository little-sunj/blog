---
title: "디자인패턴10 Strategy"
date: 2020-07-12T23:21:23+09:00
categories:
- JAVA
- 디자인패턴
tags:
- JAVA
- 디자인패턴
- strategy
keywords:
- 인강노트
---

<!--more-->

# Strategy

**하나의 추상적인 접근점(인터페이스)를 만들어 여러 알고리즘이 서로 교환 가능하도록 하는 패턴**   
**동일 목적 알고리즘 선택 적용 문제**

- 인터페이스   
    - 기능에 대한 선언(구현과의 분리)   
    - 여러가지 기능을 사용하기 위한 단일 통로 (하나의 인터페이스에 여러가지 기능이 만들어진다.)   

- 사용 예   
    - 워드문서에서 프린터, 폰트사용   
    - 정수 배열에 대해 사용하는 정렬 알고리즘   
    - 게임 캐릭터의 무기 (교체 후) 사용  



### 기능 위임 이해를 위한 예시

```java
package case1.step2;

public interface BInterface {
	//1.기능선언
	void funcA();
}
```
```java
package case1.step2;

public class BImplement implements BInterface{

	//2.기능구현
	@Override
	public void funcA() {
		System.out.println("AAA");
	}

}
```
```java
package case1.step2;

public class AObj {
	
	BInterface bInterface;
	
	public AObj() {
		bInterface = new BImplement();
	}
	public void SomeFunc() {
		//수행
		//기능 구현을 위임. Delegate
		bInterface.funcA();
		bInterface.funcA();
	}

}
```
```java
package case1.step2;

public class TestPattern {

	public static void main(String[] args) {

		AObj o = new AObj();
		o.SomeFunc();
	}

}
```


------------------

### 알고리즘의 캡슐화, 약한 결합으로 strategy 패턴 구현 예제

```java
package case2.step2;

public abstract class Database {
	public String name;
	public double rows;
	
	//데이터베이스마다 접속 라이브러리가 다르다.
	public abstract void connectDatabase();
	
	//표준 SQL문을 사용한다. (업무처리 방식이 같다.)
	public void insertData() {
		System.out.println(name+" 에 데이터를 추가했습니다.");
	}
	public void selectData() {
		System.out.println(name+" 에서 데이터를 "+ rows+"개 조회했습니다.");
	}

}
```
```java
package case2.step2;

public class MySQL extends Database{
	public MySQL() {
		name = "MySQL";
		rows = 20;
	}
	@Override
	public void connectDatabase() {
		System.out.println(name + "**** 접속했습니다.");
		
	}
}
```
```java
package case2.step2;

public class Informix extends Database{
	public Informix() {
		name = "Informix";
		rows = 40;
	}
	@Override
	public void connectDatabase() {
		System.out.println(name + "**** 접속했습니다.");
	}
}
```
```java
package case2.step2;

enum DBTYPE {MySQL, Informix}

public class DatabaseUse {
	
	private Database db;
	
	//기능 선택
	public void connect(DBTYPE dbType) {
		switch(dbType) {
		case MySQL :
			db = new MySQL();
			break;
		case Informix :
			db = new Informix();
			break;
		}
		if (db == null) {
			System.out.println("데이터베이스를 먼저 선택하세요");
		} else {
			db.connectDatabase();
		}
	}
	
	public void select() {
		db.selectData();
	}
}
```
```java
package case2.step2;

public class TestPattern {

	public static void main(String[] args) {
		
		DatabaseUse myDb = new DatabaseUse();
		
		//데이터베이스세팅
		myDb.connect(DBTYPE.MySQL);
		myDb.select();
		
		//데이터베이스세팅
		myDb.connect(DBTYPE.Informix);
		myDb.select();

	}

}

//실행결과
/*
MySQL**** 접속했습니다.
MySQL 에서 데이터를 20.0개 조회했습니다.
Informix**** 접속했습니다.
Informix 에서 데이터를 40.0개 조회했습니다.
*/
```

### 단점
위 예제에서 Oracle클래스를 세팅하고 사용하고자 할 경우 Oracle클래스 추가 & DatabaseUse에도 관련 코드를 추가해야 한다.   
단점 보완시 아래처럼 코드를 수정한다.

> 추가된 클래스
```java
package case2.step3;

public class Oracle extends Database{
	
	public Oracle() {
		name = "Oracle";
		rows = 50;
	}
	@Override
	public void connectDatabase() {
		System.out.println(name + "**** 접속했습니다.");
		
	}

}
```
> DatabaseUse 수정
```java
package case2.step3;

public class DatabaseUse {
	
	//접근점
	private Database db;
	
	//데이터베이스 교환 가능하도록
	public void setDatabase(Database db) {
		this.db = db;
	}
	
	//기능 선택
	public void connect() {
		if (db == null) {
			System.out.println("데이터베이스를 먼저 선택하세요.");
		} else {
			//Function Delegate : 구체적인 데이터베이스의 종류는 몰라도 기능 사용 가능
			db.connectDatabase();
		}
	}
	
	public void select() {
		db.selectData();
	}
}
```
```java
package case2.step3;

public class TestPattern {

	public static void main(String[] args) {
		
		//데이터베이스를 전략적으로 선택하여 사용한다.
		DatabaseUse myDb = new DatabaseUse();
		myDb.connect();
		
		//A(DatabaseUse)에게 같은 일을 시켰지만 동작은 B(MySQL)가 한다.
		myDb.setDatabase(new MySQL());
		myDb.connect();
		myDb.select();
		//A(DatabaseUse)에게 같은 일을 시켰지만 동작은 B(Informix)가 한다.
		myDb.setDatabase(new Informix());
		myDb.connect();
		myDb.select();
		//추가된 클래스
		//기존 코드의 수정 없이 오라클 접속 기능을 추가할 수 있다.
		myDb.setDatabase(new Oracle());
		myDb.connect();
		myDb.select();

	}

}


//실행결과
/*
데이터베이스를 먼저 선택하세요.
MySQL**** 접속했습니다.
MySQL 에서 데이터를 20.0개 조회했습니다.
Informix**** 접속했습니다.
Informix 에서 데이터를 40.0개 조회했습니다.
Oracle**** 접속했습니다.
Oracle 에서 데이터를 50.0개 조회했습니다.
*/
```
