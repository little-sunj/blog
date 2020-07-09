---
title: "디자인패턴6 Adapter"
date: 2020-07-09T21:25:41+09:00
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

# Adapter

**한 클래스의 인터페이스를 클라이언트에서 사용하고자 하는 다른 인터페이스로 변환한다.**   
**어댑터를 이용하면 인터페이스 호환성 문제 때문에 같이 쓸 수 없는 클래스들을 연결해서 쓸 수 있다.**

>'이미 제공되어 있는 것'과 '필요한 것'사이의 '차이'를 없애주는 디자인패턴이다.   

- Wrapper패턴으로 불리기도 한다.   
- 두 종류가 있다.     
	- 클래스에 의한 Adapter 패턴 (상속을 사용)   
	- 인스턴스에 의한 Adapter 패턴 (위임을 사용)   

**Adapter패턴은 기존의 클래스를 개조해서 필요한 클래스를 만든다.**   
- 이 패턴으로 필요한 메소드를 발빠르게 만들 수 있다.   
- 만약 버그가 발생해도 기존의 클래스에는 버그가 없으므로 Adapter역할의 클래스를 중점적으로 확인하면 되기 때문에, 프로그램 검사도 용이하다.   


이미 만들어진 클래스를 새 인터페이스에 맞게 개조시킬 때는 당연히 Adapter패턴을 사용해야 한다.
그렇지 않고 기존 클래스의 소스를 수정하려고 하면 테스트가 이미 끝난 기존 클래스를 수정한 후에 다시 테스트해야한다.   

Adapter패턴은 기존의 클래스를 수정하지 않고 목적한 인터페이스에 맞추려는 것이다. 또한 Adapter 패턴에서는 기존 클래스의 소스 프로그램이 반드시 필요한 것은 아니다. 기존 클래스의 사양(Interface)만 알면 새로운 클래스를 만들 수 있다.  

>**참고 - 관련패턴**
>- Bridge 패턴
>기능의 계층과 구현의 계층을 연결시키는 패턴이다.
>(Adapter패턴은 인터페이스가 서로 다른 클래스를 연결)
>- Decorator 패턴
>인터페이스를 수정하지 않고 기능을 추가하는 패턴이다.
>(Adapter패턴은 인터페이스의 차이를 조정하기 위한 패턴)


- 예제1 (인스턴스에 의한 Adapter 패턴)
```java
package case1;

public interface Aplayer {
	
	void play(String fileName);
	void stop();

}
```
```java
package case1;

public class AplayerImpl implements Aplayer {

	@Override
	public void play(String fileName) {
		System.out.println("(A) "+fileName);
	}

	@Override
	public void stop() {
		
	}

}
```
```java
package case1;

public interface Bplayer {
	
	void playFile(String fileName);
	void stopFile();

}
```
```java
package case1;

public class BplayerImpl implements Bplayer { 
	
	
	@Override
	public void playFile(String fileName) {
		System.out.println("(B) "+fileName);		
	}

	@Override
	public void stopFile() {
		
	}


}
```
```java
package case1;

//B --> A
public class BtoAAdapter implements Aplayer{
	
	private Bplayer media;
	
	public BtoAAdapter(Bplayer media) {
		this.media = media;
	}

	@Override
	public void play(String fileName) {
		//A의 메소드로 B의 메서드 호출
		System.out.println("Using Adapter... :");
		media.playFile(fileName);
		
	}

	@Override
	public void stop() {
		System.out.println("Using Adapter... :");
		media.stopFile();
		
	}
	
	/*
	 * 기존 코드에서 수정해야 할 노력과 분량을 어댑터 클래스 생성에 사용.
	 * 기존에 잘 사용되던 클래스이므로
	 * 버그가 발생하면 어댑터 클래스를 집중적으로 체크
	 * */

}
```
```java
package case1;

public class TestPattern {

	public static void main(String[] args) {

		Aplayer p1 = new AplayerImpl();
		p1.play("aaa.mp3");
		
		//계약기간 만료로 AplayerImpl를 사용할 수 없다.
		
		//Bplayer : 새로 도입된 방식 (잘 동작할 것이다.)
		Bplayer p2 = new BplayerImpl();
		p2.playFile("bbb.mp3");
		
		/*
		 * Aplayer obj = (어댑터) = new BplayerImpl();
		 * 기존의 잘 동작하던 코드와 새로 도입된 코드를 변경없이 사용하고 싶은 것이다.
		 * 어댑터 적용 후 에러가 난다면 어댑터 부분만 확인하면 된다.
		 * */
		p1 = new BtoAAdapter(new BplayerImpl());
		p1.play("ccc.mp3");
		
		/*
		 * 현재 모든 코드가 Aplayer인터페이스에 맞춰서 코딩되어 있다.
		 * 그러므로 여기서 Aplayer에 대입되는 객체만 수정해주면
		 * Aplayer인터페이스가 사용되는 부분에서는 수정할 필요가 없다.
		 * */
		
	}

}


//실행결과
/*
(A) aaa.mp3
(B) bbb.mp3
Using Adapter... :
(B) ccc.mp3
*/
```

- 예제2 (클래스에 의한 Adapter 패턴)

```java
package case2;

public interface Aplayer {
	
	void play(String fileName);
	void stop();

}
```
```java
package case2;

public class AplayerImpl implements Aplayer {

	@Override
	public void play(String fileName) {
		System.out.println("(A) "+fileName);
	}

	@Override
	public void stop() {
		
	}

}
```
```java
package case2;

//인터페이스가 아닌 추상클래스
public abstract class Bplayer {
	public abstract void playFile(String fileName);
	public abstract void stopFile();
}
```
```java
package case2;

public class BplayerImpl extends Bplayer { 
	
	
	@Override
	public void playFile(String fileName) {
		System.out.println("(B) "+fileName);		
	}

	@Override
	public void stopFile() {
		
	}


}

```
```java
package case2;

/*
 * class adapter implementation
 * extedns와 implements를 동시에 사용해 다중 상속을 흉내내었다.
 * */
//B --> A
public class BtoAAdapter extends BplayerImpl implements Aplayer{
	
	//A의 메서드 구현
	@Override
	public void play(String fileName) {
		//A의 메소드로 B의 메서드 호출
		System.out.println("Using Adapter... :");
		playFile(fileName);
		
	}

	@Override
	public void stop() {
		
	}
	

}
```
```java
package case2;

public class TestPattern {

	public static void main(String[] args) {

		//기존에 사용하던 방식
		Aplayer p1 = new AplayerImpl();
		p1.play("aaa.mp3");
		
		//계약기간 만료로 AplayerImpl를 사용할 수 없다.
		Bplayer p2 = new BplayerImpl();
		p2.playFile("bbb.mp3");
		
		Aplayer p3 = new BtoAAdapter();
		p3.play("ccc.mp3");
		
	}

}

//실행결과
/*
(A) aaa.mp3
(B) bbb.mp3
Using Adapter... :
(B) ccc.mp3
*/
```