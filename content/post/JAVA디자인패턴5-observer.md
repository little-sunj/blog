---
title: "JAVA디자인패턴5 Observer"
date: 2020-07-09T00:38:20+09:00
categories:
- JAVA
- 디자인패턴
tags:
- JAVA
- 디자인패턴
- Observer
keywords:
- 인강노트
---

<!--more-->


# Observer

**한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들에게 연락이 가고 자동으로 내용이 갱신된다.**    
**일대다(One-to-Many) 의존성을 정의한다.**

![external-content duckduckgo](https://user-images.githubusercontent.com/28701069/86918741-0f8bd700-c162-11ea-993e-a3faab6200a6.png)
[출처 : 위키피디아]

하나의 주제 객체와 주제 객체에 의존하고있는 옵저버 인터페이스. 옵저버 인터페이스를 구현한 여러개의 옵저버 객체로 구성된다.

![observerPattern](https://user-images.githubusercontent.com/28701069/86919866-d2c0df80-c163-11ea-9e53-84ed8792924d.PNG)
![observer2](https://user-images.githubusercontent.com/28701069/86920224-6b575f80-c164-11ea-8fdb-e8c52de454c1.PNG)

------

### **자바 내장 옵저버**    
> **내장 Observable는 JAVA9 이후로 Deprecated되었다.**

java.util.Observer 인터페이스와 java.util.Observable클래스를 사용할 수 있다.   
push/pull방식 모두 사용 가능.   

java.util.Observer 인터페이스를 구현하고 java.util.Observable객체의 addObserver()메소드를 호출하면 옵저버 목록에 추가되고 deleteObserver()를 호출하면 옵저버 목록에서 제거된다.   
   
- 연락돌리는 방법    
java.util.Observable를 상속받는 주제 클래스에서 setChanged()메소드를 호출해서 객체의 상태가 바뀌었다는 것을 알린 후 notifyObserver(Object arg) *(push방식)* 또는 notifyObservers()메소드를 호출하면 된다. 
- 옵저버 객체가 연락을 받는 방법   
update(Observable o, Object arg)메소드를 구현하기만 하면된다.    
Observable o = 연락을 보내는 주제 객체가 인자로 전달
Object arg = notifyObservers(Object arg) 메소드에서 인자로 전달된 데이터 객체가 넘어온다.

```java
package case1;

import java.util.Observable;

//감시의 대상
public class PlayController extends Observable{
	
	private boolean bPlay; //실행 여부
	
	public PlayController() {
		
	}
	
	//데이터를 전달 받아 플래그 값을 변경하고,
	//새로운 데이터가 들어왔음을 알린다.
	public void setFlag(boolean bPlay) {
		this.bPlay = bPlay;
		setChanged();
		notifyObservers();
		//Observerable 안에 모두 구현되어있기 때문에 메서도 호출만 해주면 된다.
	}
	
	//실행여부 플래그 값 반환
	public boolean getFlag() {
		return bPlay;
	}
}
```

```java
package case1;

import java.util.Observable;
import java.util.Observer; //The type Observer is deprecated since version 9


public class MyClassA implements Observer{
	
	Observable observable; //등록될 Observable
	private boolean bPlay; //실행여부
	
	public MyClassA(Observable o) {
		this.observable = o;
		Observable.addObserver(this);	//옵저버에 현재 클래스 등록
	}
	
	@Override
	public void update(Observable o, Object arg) {
		if (o instanceof PlayController) {
			PlayController myControl = (PlayController)o;
			this.bPlay = myControl.getFlag();
			myActControl();
		}
		
	}
	
	public void myActControl() {
		if(bPlay) {
			System.out.println("MyClassA : 동작을 시작합니다");
		}else {
			System.out.println("MyClassA : 동작을 정지합니다");
		}
	}

}
```
```java
package case1;

import java.util.Observable;
import java.util.Observer; //The type Observer is deprecated since version 9


public class MyClassB implements Observer{
	
	Observable observable; //등록될 Observable
	private boolean bPlay; //실행여부
	
	public MyClassB(Observable o) {
		this.observable = o;
		Observable.addObserver(this);	//옵저버에 현재 클래스 등록
	}
	
	@Override
	public void update(Observable o, Object arg) {
		if (o instanceof PlayController) {
			PlayController myControl = (PlayController)o;
			this.bPlay = myControl.getFlag();
			myActControl();
		}
		
	}
	
	public void myActControl() {
		if(bPlay) {
			System.out.println("MyClassB : 동작을 시작합니다");
		}else {
			System.out.println("MyClassB : 동작을 정지합니다");
			observable.deleteObserver(this); //옵저버를 삭제 (이후 옵저버 연락을 받지 못하게된다.)
		}
	}

}
```
```java
package case1;

public class TestPattern {

	public static void main(String[] args) {
		
		PlayController controller = new PlayController();

		MyClassA a = new MyClassA(controller);
		MyClassB b = new MyClassB(controller);
		
		System.out.println("-----모든 클래스 일시 정지---");
		controller.setFlag(false);
		
		System.out.println();
		System.out.println("-----모든 클래스 다시 시작---");
		controller.setFlag(true);
		
		System.out.println();
	}

}
```

---
### 자바 내장 옵저버 패턴의 단점과 한계

1. Observable은 클래스다.
	- Observable이 클래스기 떄문에 서브클래스를 만들어야 한다. 이미 다른 수퍼클래스를 확장하고 있는 클래스에 Observable의 기능을 추가할 수 없기 때문이다. &#10140; 재사용성에 제약이 생긴다.
2. Observable 클래스의 핵심 메소드를 외부에서 호출할 수 없다.
	-  Observable 의 setChanged()메소드가 protected로 선언되어 있다. Observable 의 서브클래스에서만 setChanged()를 호출할 수 있다. 직접 어떤 클래스를 만들고, Observable 의 서브클래스를 인스턴스 변수로 사용하는 방법도 쓸 수 없다. &#10140; 디자인은 상속보다는 구성을 사용한다는 디자인 원칙에도 위배된다.


> **내장 Observable는 JAVA9 이후로 Deprecated되었다.**


----
