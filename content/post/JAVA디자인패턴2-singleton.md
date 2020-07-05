---
title: "JAVA디자인패턴2 Singleton"
date: 2020-07-05T16:47:29+09:00
categories:
- JAVA
- 디자인패턴
tags:
- JAVA
- 디자인패턴
- singleton
keywords:
- 인강노트
---

<!--more-->

# Singleton

**최대 N개로 객체 생성을 제한하는 패턴**    


- 개발자는 객체의 최대 개수를 제한할 필요가 생긴다.
> 객체가 너무 많아지면 컴퓨터 자원을 과도하게 사용하게 되고, 이는 프로그램 전체의 속도를 느리게 할 수 있다.

생성되는 객체 최대 개수를 제한하는 데 있어 객체의 생성을 요청하는 쪽에서는 일일이 신경쓰지 않아도 되도록 만들어주는 것이 중요하다.

- 예제
```java
package case1.step1;

public class Datebase {
	
	private static Datebase singleton;
	private String name;

	private Datebase(String name) {
		try {
			Thread.sleep(100);
			this.name = name;
		} catch (Exception e) {
			
		}
	}

	public static Datebase getInstance(String name) {
		if (singleton == null) {
			singleton = new Datebase(name);
		}
		return singleton;
	}
	public String getName() {
		return name;
	}
	
}
```

```java
package case1.step1;

public class TestPattern1 {

	public static void main(String[] args) {
		
		/*
		 * Datebase d1 = new Datebase("1"); 
		 * Datebase d2 = new Datebase("2"); 
		 * Datebase d3 = new Datebase("3"); 
		 * Datebase d4 = new Datebase("4"); 
		 * Datebase d5 = new Datebase("5"); 
		 * Datebase d6 = new Datebase("6");
		 * 
		 * 위 처럼 생성시 Database X 6만큼의 리소스 차지
		 * 이를 방지하기 위해 Database.java내 생성자를 private으로 선언
		 */
		
		Datebase database;
		database = Datebase.getInstance("첫 번째 Database");
		System.out.println("This is the "+database.getName());
		
		database = Datebase.getInstance("두 번째 Database");
		System.out.println("This is the "+database.getName());
		
		System.out.println("database use");
		
		/* 결과값 -->
		 * This is the 첫 번째 Database
		 * This is the 첫 번째 Database
		 * 
		 * 두번째 getInstance시 기존에 생성된 값이 있으므로..
		 * */

	}

}
```


### 생성자 문제점과 해결 / 쓰레드 사용시 문제점 파악하기

빠르게 동작하는 경우, (거의 동시시작) singleton==null로 인식되어 객체가 여러개 생성 될 수 있다.

```java
package case1.step1;

public class TestPattern2 {
	
	static int nNum = 0;

	public static void main(String[] args) {

		Runnable task = () -> { //thread 내용
			try {
				nNum++;
				Datebase database = Datebase.getInstance(nNum + "번째 Database");
				System.out.println("This is the "+database.getName());
			} catch (Exception e) {
				
			}
		};
		
		for (int i=0; i<10; i++) { //thread 10개 시작
			Thread t = new Thread(task); 
			t.start();
		}
		/* 실행결과 
		 * 
		 * 	This is the 3번째 Database
			This is the 8번째 Database
			This is the 2번째 Database
			This is the 6번째 Database
			This is the 10번째 Database
			This is the 6번째 Database
			This is the 4번째 Database
			This is the 7번째 Database
			This is the 5번째 Database
			This is the 9번째 Database
		   >>>10개가 생성됨
		  
		    순서는 메모리 여유대로 진행되기 때문에 꼭 1~10 차례로 되지는않는다.
		  (거의 동시에 시작됨 -> 그래서 각각의 singleton이 null이라고 인식)
		  */
	}
}
```

### 쓰레드 사용시 문제점 해결 및 좀 더 효율적인 코드 만들기

```java
	//synchronized 예약어를 추가하므로써, 동시에 여러 쓰레드가 접근하더라도
	//줄을 서서 앞 쓰레드가 끝나기를 기다리게 된다. (차례도록 진행하도록)
	public synchronized static Datebase getInstance(String name) {
		if (singleton == null) {
			singleton = new Datebase(name);
		}
		return singleton;
	}

/* 수정 후 실행 결과
This is the 2번째 Database
This is the 2번째 Database
This is the 2번째 Database
This is the 2번째 Database
This is the 2번째 Database
This is the 2번째 Database
This is the 2번째 Database
This is the 2번째 Database
This is the 2번째 Database
This is the 2번째 Database
>>> 1개가 생성됨
*/

// but 비용이 많이 들어간다. --> 쓰레드가 많을 경우 길어짐
```

```java
package case1.step3;

public class Datebase {
	
	private static Datebase singleton = new Datebase("products");
	//static이기 때문에 프로그램 실행시 객체가 만들어진다. '이미' 생성된 상태가 된다.
	//아래 getInstance에서 null여부를 확인할 필요가 없어진다. (이미 있으므로)
	private String name;

	private Datebase(String name) {
		try {
			Thread.sleep(100);
			this.name = name;
		} catch (Exception e) {
			
		}
	}

	public static Datebase getInstance(String name) {
		return singleton;
	}
	public String getName() {
		return name;
	}
	
}

/*
수정 후 실행결과 
This is the products
This is the products
This is the products
This is the products
This is the products
This is the products
This is the products
This is the products
This is the products
This is the products
>>> 1개가 생성됨
*/
```

요즘은 프레임워크가 잘 되어있어서 직접 구현할 일이 많지 않다. (DB연결이나 Log writer등에서 사용) 



- SAMPLE (LogWriter)

```java
package case2;

import java.io.BufferedWriter;
import java.io.FileWriter;

public class LogWriter {

	private static LogWriter singleton = new LogWriter();
	private static BufferedWriter bw;
	
	private LogWriter() {
		try {
			bw = new BufferedWriter(new FileWriter("log.txt"));
		}catch(Exception e) {
			
		}
	}
	
	public static LogWriter getInstance() {
		return singleton;
	}
	
	public synchronized void log(String str) {
		
		try {
			//현재 날짜와 시각 추가
			//bw.wrtie(LocaleDateTime.now() + " : " + str + "\n");
			bw.write(str+"\n");
			bw.flush();
		}catch(Exception e) {
			
		}
	}
	
	@Override
	protected void finalize() {
		try {
			super.finalize();  
			//note : The method finalize() from the type Object is deprecated since version 9
			bw.close();
		}catch(Throwable ex) {
			
		}
	}
}
```

```java
package case2;

public class TestPattern1 {

	public static void main(String[] args) {
		
		LogWriter logger;
		
		logger = LogWriter.getInstance();
		logger.log("토마토");
		
		logger = LogWriter.getInstance();
		logger.log("당근");

	}

}
```

```java
package case2;

public class TestPattern2 {

	public static void main(String[] args) {
		for (int i=0; i<50; i++) {
			//쓰레드마다 구분되는 로그 작성용 파라미터
			Thread t = new ThreadSub(i);
			t.start();
		}
	}
}

//외부 클래스
class ThreadSub extends Thread {
	int num;
	
	public ThreadSub(int num) {
		this.num = num;
	}

	@Override
	public void run() {
		LogWriter logger = LogWriter.getInstance();
		if (num < 10) {
			logger.log("*** 0" + num + " ***");
		} else {
			logger.log("*** " + num + " ***");
		}
	}
}
```