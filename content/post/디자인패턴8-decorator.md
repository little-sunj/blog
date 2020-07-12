---
title: "디자인패턴8 Decorator"
date: 2020-07-12T22:13:53+09:00
categories:
- JAVA
- 디자인패턴
tags:
- JAVA
- 디자인패턴
- Decorator
keywords:
- 인강노트
---

<!--more-->
# Decorator

**객체의 결합을 통해 기능을 동적으로 유연하게 확장 할 수 있게 해주는 패턴**

- 기본 기능에 추가할 수 있는 기능 종류가 많은 경우, 각 추가 기능을 Decorator클래스로 정의한 후 필요한 Decorator객체를 조합함으로써 추가 기능의 조합을 설계하는 방식.   
ex) 기본도로 표시 기능에 차선표시, 교통량표시, 교차로 표시, 단속 카메라 표시의 4가지 추가 기능이 있을때 추가기능의 모든 조합은 15가지가 된다. 데코레이터 패턴을 이용하여 추가 기능의 조합을 동적으로 생성할 수 있다.   

> - 상속을 통해 확장할 수도 있지만, 상속은 디자인 유연성 면에서는 별로 좋지 않다.
>  - 기존 코드를 수정하지 않고도 행동을 확장하는 방법이 필요하다.
>  - 상속대신 데코레이터 패턴을 통해 행동을 확장 할 수 있다.    

----------------

### 적용 예시

- 자바 입출력의 필터 스트림
기반스트림과 보조스트림을 데코레이터 패턴을 배우고 다시 정리해보면 많은 클래스들이 좀 더 쉽게 다가온다.

```java
//java file IO
BufferedReader br = null;
try {
	br = new BufferedReader(new FileReader(new File("test.txt")));
} catch (Exception e) {
	e.printStackTrace();
}
```

----------------

### 단점
1. 데코레이터 패턴을 사용하면 자잘한 객체들이 매우 많이 추가될 수 있고, 코드가 필요 이상으로 복잡해질 수도 있다.
2. 구성요소(component)를 초기화하는데 필요한 코드가 훨씬 복잡해진다. 구성요소 인스턴스만 만든다고해서 일이 끝나는게 아니라 많은 데코레이터로 wrapping해야 하는 경우가 있다. 그래서 데코레이터 패턴은 팩토리 패턴이나 빌더 패턴과 함께 사용된다.
3. 구상 구성요소(Concrete component)의 형식을 알아내서 그 결과를 바탕으로 어떤 작업을 처리하는 코드(특정 형식에 의존하는 클라이언트 코드)에는 적용할 수 없다. 기존의 구성요소 HouseBlend였을떄 기존의 구성요소를 데코레이터로 감싸게 되면, 그 구성요소가 HouseBlend인지 아닌지를 알 수가 없게 된다.


----------------

### 예시

>base 패키지

```java
package case1.base;

public abstract class IceCream {
	
	protected String description = "";
	
	public String getDescription() {
		return this.description;
	}
	
	public abstract int price();

}
```
```java
package case1.base;

public class IceCreamCake extends IceCream{

	public IceCreamCake() {
		this.description = "아이스크림(케이크)";
	}
	
	@Override
	public int price() {
		return 1500;
	}
}
```
```java
package case1.base;

public class IceCreamCone extends IceCream{
	
	public IceCreamCone() {
		this.description = "아이스크림(콘)";
	}
	
	@Override
	public int price() {
		return 2000;
	}

}
```
```java
package case1.base;

public class IcePop extends IceCream{
	
	public IcePop() {
		this.description = "아이스크림(바:막대)";
	}
	
	@Override
	public int price() {
		return 1000;
	}

}
```

>decorator 패키지
```java
package case1.decorator;

import case1.base.IceCream;

//첨가물을 나타내는 Abstract 클래스 (Decorator 클래스)
//데코레이터 클래스의 형식은 그 클래스가 감싸고 있는 클래스의 형식을 반영한다.
//그러므로, IceCream객체가 들어갈 자리에 들어갈 수 있어야 하므로 IceCream클래스를 상속한다.

public abstract class Decorator extends IceCream {
	
	//설명을 추가해야 하므로 서브클래스에서 꼭 구현하도록 강제한다.
	public abstract String getDescription();
	//비용은 이미 추상메서드이므로 서브클래스에서 꼭 구현해야 한다.

}
```
```java
package case1.decorator;

import case1.base.IceCream;

public class Melon extends Decorator {
	
		IceCream iceCream;
		
		public Melon(IceCream iceCream) {
			this.iceCream = iceCream;
		}

		@Override
		public String getDescription() {
			return iceCream.getDescription() + " + 멜론";
		}
		
		@Override
		public int price() {
			return 300 + iceCream.price();
		}

}
```
```java
package case1.decorator;

import case1.base.IceCream;

public class StrawBerry extends Decorator {
	//strawberry는 decorator기 때문에 Decorator를 상속
	
	//wrapping하고자 하는 음료를 저장하기 위한 인스턴스 변수
	IceCream iceCream;
	
	public StrawBerry(IceCream iceCream) {
		this.iceCream = iceCream;
	}

	@Override
	public String getDescription() {
		return iceCream.getDescription() + " + 딸기";
	}
	
	@Override
	public int price() {
		return 500 + iceCream.price();
	}
}
```
> use 패키지
```java
package case1.use;

import case1.base.IceCream;
import case1.base.IceCreamCake;
import case1.base.IceCreamCone;
import case1.base.IcePop;
import case1.decorator.Melon;
import case1.decorator.StrawBerry;

public class Testpattern {

	public static void main(String[] args) {
		
		IceCream iceCream1 = new IceCreamCone();
		System.out.println(iceCream1.getDescription() + " 가격 :" + iceCream1.price());
		
		//아래와 같은 식으로 기본에 첨가물들을 Wrapping해서 가격과 설명을 추가할 수 있다.
		IceCream iceCream2 = new IceCreamCake();
		iceCream2 = new StrawBerry(iceCream2);
		System.out.println(iceCream2.getDescription() + " 가격 :" + iceCream2.price());
		
		IceCream iceCream3 = new Melon(new StrawBerry(new IcePop()));
		System.out.println(iceCream3.getDescription() + " 가격 :" + iceCream3.price());

	}

}
  //실행결과
  /*
    아이스크림(콘) 가격 :2000
    아이스크림(케이크) + 딸기 가격 :2000    
    아이스크림(바:막대) + 딸기 + 멜론 가격 :1800
  */
```

----------------

### 예제2

> base 패키지
```java
package case2.base;

//기본이 되는 클래스는 일반 클래스나 추상클래스일 수 있다.
public abstract class Student {
	
	protected String description = "no particular";
	
	public String getDescription() {
		return description;
	}

}
```
```java
package case2.base;

public class AmericanStudent extends Student {
	public AmericanStudent() {
		this.description = "American Student";
	}
}
```
```java
package case2.base;

public class KoreanStudent extends Student {
	public KoreanStudent() {
		this.description = "Korean Student";
	}
}
```

> decorator 패키지
```java
package case2.decorator;

import case2.base.Student;

public abstract class StudentDecorator extends Student{
	//설명을 추가해야 하므로 서브클래스에서 꼭 구현하도록 강제한다.
	public abstract String getDescription();
}
```
```java
package case2.decorator;

import case2.base.Student;

public class Art extends StudentDecorator{
	
	private Student student;
	
	public Art(Student student) {
		this.student = student;
	}

	@Override
	public String getDescription() {
		return student.getDescription() + " + Like Art";
	}
	
	public void draw() {
		System.out.println("draw");
	}

}
```
```java
package case2.decorator;

import case2.base.Student;

public class Science extends StudentDecorator{
	
private Student student;
	
	public Science(Student student) {
		this.student = student;
	}

	@Override
	public String getDescription() {
		return student.getDescription() + " + Like Science";
	}
	
	public void calculateStuff() {
		System.out.println("scientific calculation");
	}
	

}
```

> use패키지

```java

package case2.use;

import case2.base.AmericanStudent;
import case2.base.Student;
import case2.decorator.Art;
import case2.decorator.Science;

public class TestPattern {

	public static void main(String[] args) {
		Student s1 = new AmericanStudent();
		System.out.println(s1.getDescription());
		
		Science s2 = new Science(s1);
		System.out.println(s2.getDescription());
		
		Art s3 = new Art(s2);
		System.out.println(s3.getDescription());

	}

}
//실행결과
/*
American Student
American Student + Like Science
American Student + Like Science + Like Art
*/
```