---
title: "JAVA디자인패턴1"
date: 2020-07-02T22:26:23+09:00
categories:
- JAVA
- 디자인패턴
tags:
- JAVA
- 디자인패턴
keywords:
- 인강노트
---

<!--more-->

# 디자인패턴

> ### 알고리즘 
>- 문제해결을 위한 처리 절차   
>- 상황에 맞는 최적의 갖는 처리 절차 사용    

ex) 여러 개의 데이터가 있을 때 순서에 맞게 정렬하는 방법 (단순 정렬, 버블 정렬, 삽입 정렬, 쉘 정렬, 퀵 정렬)

알고리즘 != 디자인패턴



### 효율적인 프로그래밍을 하고 있다고 판단하는 기준
- 중복된 코드가 없다   
- 변경된 요구사항을 순조롭게 받아들일 수 있어야 한다.


*디자인패턴을 적용하여 프로그램을 만드는 중요한 이유는 다양한 추가 요구사항에 대해 좀 더 쉽게 대응하기 위해서이다. 즉, 코드 수정이 쉽도록 하기 위함이다.*


-----

### 디자인패턴의 종류   

- 오리지널 파운데이션 패턴(GoF 패턴)   
- 썬의 J2EE 패턴   
- JSP 패턴   
- 아키텍쳐 패턴   
- 게임 디자인 패턴   
- ...

----

### GoF 패턴

 1. 싱글턴 패턴   
 2. 플라이웨이트 패턴   
 3. 빌더 패턴   
 4. 옵저버 패턴   
 5. 어댑터 패턴   
 6. 파사드 패턴   
 7. 데코레이션 패턴   
 8. 브릿지 패턴   
 9. 스트래티지 패턴   


-----
<br/>
<br/>

## 디자인 패턴과 클래스의 다형성

### 클래스의 특성을 이용한 디자인패턴의 시작

- 밸류타입 사이에서의 형변환  
```java
//자동(묵시적) 형변환
int intA = 10;
double douA = intA;

//강제(명시적) 형변환
double douB = 12.34;
int intB=(int)douB;
```

<br/>


- 래퍼런스타입 사이에서의 형변환
클래스 사이에서도 형변환이 일어나는데, 클래스 사이의 형변환은 상위클래스와 하위클래스 사이에서 일어나게 된다.   
&#10140; *하위클래스의 객체는 상위클래스의 객체에 대입할 수 있다.*

디자인패턴의 출발점이 된다.


-----
<br/>
<br/>

## DI (Dependency Injection) 의존주입   
#### 클래스의 약한 결합, 강한 결합의 이해


객체를 사용하는 2가지 방법
- 직접 생성 : 생성부터 메모리 관리를 위한 소멸까지 해당 객체의 라이프사이클을 개발자가 관리 = 강한결합
 ```mermaid
graph TB
subgraph A객체
newB
newC 
end
```
- 의존 주입 : 다른 누군가가 생성한 객체를 사용만 하면 되므로 개발자가 관리할 것이 적어진다는 장점이 있음 = 약한결합
 ```mermaid
graph TB
newB-->setter
newC-->construct
subgraph A객체
setter
construct
end
```


- **약한 결합을 사용하는 프로그래밍은 다른 클래스의 변화에 보다 더 안전하고 유연하게 대처가능**
```java
import java.util.Date;

public class UnderstandDI {

	public static void main(String[] args) {
		
		//기존에 API나 프레임워크에서 제공하는 기능들도 강한결합, 약한결합을 만들 수 있다.
		//날짜를 구하기 위해서는 Date클래스에 의존해야한다.
		//강한 결합
		Date date = new Date();
		System.out.println(date);
	}

	//약한결합
	public static void getDate(Date d) {
		Date date = d;
		System.out.println(date);
	}
	
	public static void memberUse1() {
		//강한결합 : 직접 생성
		Member m1 = new Member();
	}

	public static void memberUse2(Member m) {
		//약한 결합 : 생성된 것을 주입 받음 - 의존 주입 (Dependency Injection)
		Member m2 = m;
	}
}

//Member를 사용한다 --> Member의 기능에 의존한다 라는 의미
class Member {
	String name;
	String nickname;
	public Member() {}
	/*
	 * 위 public Member를 private으로 변경시,강한 결합에서 에러 발생
	 * */
}
```