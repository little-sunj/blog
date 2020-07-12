---
title: "디자인패턴9 Bridge"
date: 2020-07-12T23:00:29+09:00
categories:
- JAVA
- 디자인패턴
tags:
- JAVA
- 디자인패턴
- Bridge
keywords:
- 인강노트
---

<!--more-->

# Bridge



**기능의 계층과 구현의 계층을 연결시키는 패턴**



브릿지 패턴을 이해하기 위해서는 다음을 먼저 이해해야 한다.
- 기능 클래스 계층
- 구현 클래스 계층

이는 다음 두가지 경우로 나눠서 생각해 볼 수 있다.
- **Case1** : 새로운 기능을 추가하고 싶은 경우
- **Case2** : 새로운 구현을 추가하고 싶은 경우


하위의 구상 클래스를 만드려고 할 때, 의도를 확인해봐야한다.
> *기능을 추가하려고 하는가? 구현을 추가하려고 하는가?* 


'기능의 클래스 계층'과 '구현의 클래스 계층'을 두 개의 독립된 클래스 계층으로 분리한다. 단순히 분리만 하면 흩어져 버리기 때문에 두개의 클래스 계층 사이에 다리를 놓는 일이 필요하다.  



adaptor패턴과 매우 흡사하며, 다음과 같은 효과를 가진다.
- 장점 : 인터페이스와 실제 구현부를 서로 다른 방식으로 변경해야 할 경우 유용하다.
- 단점 : 디자인이 복잡해진다.


---------

### 예시

```java
package step1;

public interface IRobot {
	void powerOn();
	void powerOff();
}
```
```java
package step1;

public class RobotModel1 implements IRobot{
	@Override
	public void powerOn() {
		System.out.println("type1 : power ON");
	}
	@Override
	public void powerOff() {
		System.out.println("type1 : power OFF");		
	}

}
```
```java
package step1;

public class RobotModel2 implements IRobot{
	@Override
	public void powerOn() {
		System.out.println("type2 : power ON");
	}
	@Override
	public void powerOff() {
		System.out.println("type2 : power OFF");		
	}
}
```
```java
package step1;

//구현의 추가는 계속할 수 있다.
//RobotModel1, RobotModel2, ...
public class TestPattern {
	
	public static void main(String[] args) {
		IRobot robot1 = new RobotModel1();
		robot1.powerOn();
		robot1.powerOff();

		System.out.println("-----------");
		
		IRobot robot2 = new RobotModel2();
		robot2.powerOn();
		robot2.powerOff();
	}
}

//실행결과
/*
type1 : power ON
type1 : power OFF
-----------
type2 : power ON
type2 : power OFF

*/
```

### 단점   
인터페이스에 기능이 새로 추가될 경우, 관련 클래스들을 모두 수정해 주어야할 필요가 있다.   
그리고 robotModel1에서만 필요하고, robotModel2에서는 필요없더라도, 인터페이스가 추가되었기 때문에 불필요한 기능추가가 있을 수 있다.


### 단점보완 예제
```java
package step2;

public interface IRobot {
	void powerOn();
	void powerOff();
}
```
```java
package step2;

// IRobot, RobotModel1, RobotModel2의 변경 없이 기능을 추가하고자 한다.
public class IAction {
	private IRobot robot;
	
	public IAction(IRobot robot) {
		this.robot = robot;
	}
	
	//IRobot의 기능을 전부 똑같이 구현한다.
	void powerOn() {
		robot.powerOn();
	}
	void powerOff() {
		robot.powerOff();
	}

}
```
```java
package step2;

public class Cook extends IAction {

	public Cook(IRobot robot) {
		super(robot);
	}

	//행동(요리)추가
	public void doCook() {
		System.out.println("요리를 합니다.");
	}
}
```
```java
package step2;

public class RobotModel1 implements IRobot{

	@Override
	public void powerOn() {
		System.out.println("type1 : power ON");
	}

	@Override
	public void powerOff() {
		System.out.println("type1 : power OFF");		
	}

}
```
```java
package step2;

public class RobotModel2 implements IRobot{
	@Override
	public void powerOn() {
		System.out.println("type2 : power ON");
	}

	@Override
	public void powerOff() {
		System.out.println("type2 : power OFF");		
	}
}
```
```java
package step2;

//기능의 추가 : 행동 추가
//추가된 기능에 대한 구현의 추가 : 요리, 청소 등 추가..
public class TestPattern {
	
	public static void main(String[] args) {
		IRobot robot = new RobotModel1();
		Cook work = new Cook(robot);
		work.powerOn();
		work.doCook();
		work.powerOff();

		System.out.println("-----------");
		
		robot = new RobotModel2();
		work = new Cook(robot);
		work.powerOn();
		work.doCook();
		work.powerOff();
	}
}
//실행결과
/*
type1 : power ON
요리를 합니다.
type1 : power OFF
-----------
type2 : power ON
요리를 합니다.
type2 : power OFF

*/
```