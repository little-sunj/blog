---
title: "디자인패턴7 Facade"
date: 2020-07-11T21:11:55+09:00
categories:
- JAVA
- 디자인패턴
tags:
- JAVA
- 디자인패턴
- Facade
keywords:
- 인강노트
---

<!--more-->

# Facade

**하위 시스템을 보다 쉽게 사용할 수 있게 해주는 고급 인터페이스를 정의한다.**
   

파사드 패턴은 어댑터 패턴과 거의 같은 방식으로 작동하지만 **서로 다른 목적**을 가지고 있다.     

| Adapter | Facade |
|--|--|
| 원래 코드를 다른 코드와 작동할 수 있게 하는 래퍼를 제공 | 원래 코드를 더 쉽게 처리 할 수 있는 래퍼를 제공 |


파사드 디자인 패턴은 오브젝트나 클래스 인터페이스가 작동하기에 너무 어려울 경우, 쉽게 사용할 수 있는 프론트엔드 인터페이스를 제공한다.   
- 캡슐화되지 않은 코드를 처리할 때 사용   
- 원하는 코드르 다시 작성할 수 없을때 일반적으로 사용    
	- (단점)파사드를 사용하면 문제를 해결할 수 있지만 기본 코드가 변경되면 파사드 패턴도 변경해야 한다.   

### 예제
```java
package case1;

public class ComplexProduct {
	char nameChars[] = new char[10];

	public ComplexProduct() {
	}
	
	public void setFirstNameCharacter(char c) {
		nameChars[0] = c;
	}
	public void setSecondNameCharacter(char c) {
		nameChars[1] = c;
	}
	public void setThirdNameCharacter(char c) {
		nameChars[2] = c;
	}
	public void setFourthNameCharacter(char c) {
		nameChars[3] = c;
	}
	public void setFifthNameCharacter(char c) {
		nameChars[4] = c;
	}
	public void setSixthNameCharacter(char c) {
		nameChars[5] = c;
	}
	
	public String getName() {
		return new String(nameChars);
	}

}
```
```java
package case1;

public class SimpleProductFacade {
	ComplexProduct difficultProduct;
	
	public SimpleProductFacade() {
		difficultProduct = new ComplexProduct();
	}
	
	public void setName(String n) {
		char chars[] = n.toCharArray();
		
		if (chars.length > 0) {
			difficultProduct.setFirstNameCharacter(chars[0]);
		}
		if (chars.length > 1) {
			difficultProduct.setSecondNameCharacter(chars[1]);
		}
		if (chars.length > 2) {
			difficultProduct.setThirdNameCharacter(chars[2]);
		}
		if (chars.length > 3) {
			difficultProduct.setFourthNameCharacter(chars[3]);
		}
		if (chars.length > 4) {
			difficultProduct.setFifthNameCharacter(chars[4]);
		}
		if (chars.length > 5) {
			difficultProduct.setSixthNameCharacter(chars[5]);
		}
	}
	
	public String getName() {
		return difficultProduct.getName();
	}

}
```
```java
package case1;

public class TestPattern {

	public static void main(String[] args) {
		SimpleProductFacade simpleProductFacade = new SimpleProductFacade();
		simpleProductFacade.setName("printer");
		System.out.println("the product is a...." + simpleProductFacade.getName());
	}

}

//실행결과
//the product is a....printe
```


### 예시2


파사드 패턴은 여러가지 복잡한 것들을 하나로 간주해서 편하게 다루는 방법이다. 그렇기에 파사드 패턴을 사용하면 최소 단위로 클래스를 설계할 수 있게 된다.   
(지나치게 잘게 쪼개는 것도 그다지 바람직한 것은 아님.)   
ex) Home클래스 내 Computer, Light, Radio 클래스가 종속되어있을경우 Office클래스가 나왔을 때, Computer클래스의 기능을 다시 구현해야 하는 사태가 발생한다. 최소 단위로 쪼개어 구현했다면 그러지 않아도 될 것이다.

```java
package case2;

public class Computer {
	private boolean turnedOn = false;
	
	public void turnOn() {
		turnedOn = true;
		System.out.println("Computer ---ON");
	}
	
	public void turnOff() {
		turnedOn = false;
		System.out.println("Computer ---OFF");
	}
	public boolean isTurnedOn() {
		return turnedOn;
	}

}
```
```java
package case2;

public class Light {
	
	private boolean turnedOn = false;
	
	public void turnOn() {
		turnedOn = true;
		System.out.println("Light ---ON");
	}
	
	public void turnOff() {
		turnedOn = false;
		System.out.println("Light ---OFF");
	}
	public boolean isTurnedOn() {
		return turnedOn;
	}

}
```
```java
package case2;

public class Radio {
	
	private boolean turnedOn = false;
	
	public void turnOn() {
		turnedOn = true;
		System.out.println("Radio ---ON");
	}
	
	public void turnOff() {
		turnedOn = false;
		System.out.println("Radio ---OFF");
	}
	public boolean isTurnedOn() {
		return turnedOn;
	}

}
```
```java
package case2;

public class HomeFacade {
	
	private Computer computer;
	private Light light;
	private Radio radio;
	
	public HomeFacade(Computer computer, Light light, Radio radio) {
		this.computer = computer;
		this.light = light;
		this.radio = radio;
	}
	
	public void homeIn() {
		System.out.println("집에오면 computer / radio / light ON");
		if(!computer.isTurnedOn()) {
			computer.turnOn();
		}
		if(!light.isTurnedOn()) {
			light.turnOn();
		}
		if(!radio.isTurnedOn()) {
			radio.turnOn();
		}
	}
	
	public void homeOut() {
		System.out.println("외출할떈 computer / radio / light OFF");
		if(computer.isTurnedOn()) {
			computer.turnOff();
		}
		if(light.isTurnedOn()) {
			light.turnOff();
		}
		if(radio.isTurnedOn()) {
			radio.turnOff();
		}
	}
	

}
```
```java
package case2;

public class TestPattern {

	public static void main(String[] args) {
		
		Computer c = new Computer();
		Light l = new Light();
		Radio r = new Radio();
		
		//이전 사용 방식
		c.turnOff();
		l.turnOff();
		r.turnOff();
		
		//파사드 패턴 적용 후 방식
		HomeFacade home = new HomeFacade(c, l, r);
		home.homeIn();
		
	}

}
```