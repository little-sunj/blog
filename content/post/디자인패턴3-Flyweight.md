---
title: "디자인패턴3 Flyweight"
date: 2020-07-06T19:41:28+09:00
categories:
- language
- java
- design pattern
tags:
- JAVA
- 디자인패턴
- flyweight
keywords:
- 인강노트

---

<!--more-->
# Flyweight

**비용이 큰 자원을 공통으로 사용할 수 있도록 만드는 패턴**    

자원에 대한 비용은 크게 두 가지로 나눠볼 수 있다.
- **중복 생성될 가능성이 높은 경우**   
동일한 자원이 자주 사용될 가능성이 매우 높다는 것을 의미. 공통 자원 형태로 관리해 주는 편이 좋다.
- **자원 생성 비용은 큰데 사용 빈도가 낮은 경우**   
 미리 생성해 두는것은 낭비. 요청이 있을때 생성해서 제공하는 것이 좋다.
 
 플라이웨이트 패턴은 자원 생성과 제공을 책임진다.    
 생성을 담당하는 factory역할과 관리 역할을 분리하는 것이 좋을 수 있으나, 일반적으로는 두 역할의 크기가 그리 크지 않아서 하나의 클래스 담당하도록 구현한다.
```java
public class Tree {
	//공통 중복 영역
	Object mesh;
	Texture bark;
	Texture leaves;
	....
	//개별영역
	Position position;
	Double height;
	Double thickness;
}
//나무를 여러개 만드는경우 mesh / bark / leaves 객체가 각각 생성된다.
```
```java
//이를 객체 공유방식으로 다시 구성하게 되면 각각 객체를 만들지 않고 미리 만들어진 TreeModel을 사용하게 된다.
//공유할 객체를 감쌀 나무모델 클래스를 정의
public class TreeModel {
	Mesh mesh;
	Texture bark;
	Object leaves;
	public TreeModel() {
		mesh = new Mesh();
		bark= new Texture("bark");
		leaves= new Texture("leaves");
	}
}
//실제로 매번 생성할 나무 클래스를 정의
public class Tree {
	TreeModel treeModel; //공유객체
	Position position;
	Double height;
	Double thickness;
}
```

```java
//static변수로 선언된 객체를 하나 만들어 모든 Tree객체의 내부에서 사용되는 TreeModel에서 공유한다.
//팩토리 메소드를 정의한다.
public class TreeFactory {
	private static final TreeModel sharedTreeModel = new TreeModel();
	//static 메소드는 1개만 만들어지기 때문에 나머지 정보가 모두 다르더라도 sharedTreeModel은 1개로 고정
	static public Tree create(Position p, double h, double th) {
		Tree tree = new Tree();
		tree.setPosition(p);
		tree.setHeight(h);
		tree.setThickness(th);
		tree.setTreeModel(sharedTreeModel);
		
	}
}
```

------

### 플라이웨이트 패턴 장단점

**장점**
- 많은 객체를 만들 때 성능을 향상시킬 수 있다.
- 많은 객체를 만들 때 메모리를 줄일 수 있다.
- state pattern과 쉽게 결합될 수 있다.

**단점**
- 특정 인스턴스의 공유 컴포넌트를 다르게 행동하게 하는 것이 불가능하다. (개별설정이 불가능하다.)

<br/>
-----


*적용된 예시*
> JDK
> java.lang.String   
> java.lang.Integer#valueOf(int)   
> java.lang.Boolean#valueOf(boolean)   
> java.lang.Byte#valueOf(byte)   
> java.lang.Character#valueOf(char)   
>워드 프로세스에서 문자들의 그래픽적 표현에 대한 자료구조

-----

### 자바 코드에서 볼 수 있는 플라이웨이트 패턴
- 예제
```java
package case1.step1;

public class TestPattern {

	public static void main(String[] args) {
		String str1 = new String("홍길동");
		String str2 = new String("홍길동");
		String str3 = "홍길동";
		String str4 = "홍길동";

		System.out.println("Flyweight Pattern");
	}
}

```
결과 
> str1과 str2는 다른 id값을 가지고 있다 = new로 생성된 서로다른 객체 
> str4는 str3를 참조하고있음. ->중복방지를 위해 flyweight패턴이 적용되어있는 것이다.
![image](https://user-images.githubusercontent.com/28701069/86578512-e24ef580-bfb6-11ea-81a5-1cf9a39484fa.png)

```java
package case1.step2;

public class TestPattern {

	public static void main(String[] args) {
		MyData md1 = new MyData();
		md1.xpos = 10;
		md1.ypos = 11;
		md1.name = "홍길동";
		
		MyData md2 = new MyData();
		md2 = md1; //md1과 md2의 id값이 같아짐 (같은값 참조)
		
		MyData md3 = new MyData();
		md3.xpos = 20;
		md3.ypos = 21;
		md3.name = "손오공";
		
		md2.name = "전우치"; //md2가 변하면서 공유하고잇는 md1도 변화하게 된다.
		md2.xpos = 5;
	}
}
class MyData {
	int xpos;
	int ypos;
	String name;	
}
```


-----

### 플라이웨이트 패턴 구현

- Sample
```java
package case2;

public class Subject {
	private String name;
	
	public Subject(String name) {
		this.name = name;
	}

}
```
```java
package case2;

import java.util.HashMap;
import java.util.Map;

public class FlyweightFactory {
	private static Map<String, Subject> map = new HashMap<String, Subject>();
	
	//java.lang.String에 적용된 방식과 동일한 방식이다.
	//key값이 맵에 없으면 추가하고 있으면 재사용한다.
	public Subject getSubject(String key) {
		Subject subject = map.get(key);
		if (subject == null) {
			subject = new Subject(key);
			map.put(key, subject);
			
			System.out.println("새로 생성  :" +key);
		}else {
			System.out.println("재사용 :" +key);
		}
		return subject;
	}
}
```
```java
package case2;

public class TestPattern {

	public static void main(String[] args) {
		
		FlyweightFactory fw = new FlyweightFactory();
		fw.getSubject("a");
		fw.getSubject("a");
		fw.getSubject("b");
		fw.getSubject("b");

	}

}

//실행결과
/*
새로 생성  :a
재사용 :a
새로 생성  :b
재사용 :b
*/
```


&nbsp;

###### reference
[인프런 강의 : 디자인패턴withJAVA](https://www.inflearn.com/course/Design-pattern-java/dashboard)


-----