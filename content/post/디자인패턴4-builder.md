---
title: "디자인패턴4 Builder"
date: 2020-07-07T18:52:54+09:00
categories:
- language
- java
- design pattern
tags:
- JAVA
- 디자인패턴
- Builder
keywords:
- 인강노트

---

<!--more-->
# Builder

**객체를 생성할 때 흔하게 사용하는 패턴**    

```java
Member member = Member.builder()
	.name("SUN")
	.age("32")
	.build();

//이런 패턴을 Dot(.) Chain문법이라고 부른다.
```

> **GoF-Design-Pattern (1994년)**
>  - 객체의 생성 알고리즘과 조립 방법을 분리하는 것이 목적
>     
> **이펙티브 자바 (2001년)**
> - 코드 읽기/유지보스가 편해지므로 빌더 패턴을 쓰라고 한다.   
> - 객체 일관성, 변경 불가능성 등의 특징  
> 

  
 
 규칙) 생성자 인자가 많을 때는 빌더 패턴 적용을 고려.
&#10140; 객체 생성을 깔끔/유연하게 하기 위한 기법

-----

- 생성자 인자가 많을 때 불편한 예시

```java
package step1;

import java.time.LocalDate;

public class Person {
	
	private String firstName;
	private String lastName;
	private LocalDate birthDate;
	private String addressOne;
	private String addressTwo;
	private String sex;
	private boolean driverLicence;
	private boolean married;
	
	//아래처럼 생성자를 만들경우, 일부값만 초기화 하고싶어지는 케이스가 많아질수록 불편해진다. 
	/*
	 * public Person(String firstName, String lastName, LocalDate birthDate, String
	 * addressOne, String addressTwo, String sex, boolean driverLicence, boolean
	 * married) { super(); this.firstName = firstName; this.lastName = lastName;
	 * this.birthDate = birthDate; this.addressOne = addressOne; this.addressTwo =
	 * addressTwo; this.sex = sex; this.driverLicence = driverLicence; this.married
	 * = married; }
	 * 
	 * public Person(String firstName, String lastName) { super(); this.firstName =
	 * firstName; this.lastName = lastName; }
	 */

	public String getFirstName() {
		return firstName;
	}
	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}
	public String getLastName() {
		return lastName;
	}
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
	public LocalDate getBirthDate() {
		return birthDate;
	}
	public void setBirthDate(LocalDate birthDate) {
		this.birthDate = birthDate;
	}
	public String getAddressOne() {
		return addressOne;
	}
	public void setAddressOne(String addressOne) {
		this.addressOne = addressOne;
	}
	public String getAddressTwo() {
		return addressTwo;
	}
	public void setAddressTwo(String addressTwo) {
		this.addressTwo = addressTwo;
	}
	public String getSex() {
		return sex;
	}
	public void setSex(String sex) {
		this.sex = sex;
	}
	public boolean isDriverLicence() {
		return driverLicence;
	}
	public void setDriverLicence(boolean driverLicence) {
		this.driverLicence = driverLicence;
	}
	public boolean isMarried() {
		return married;
	}
	public void setMarried(boolean married) {
		this.married = married;
	}
}
```

```java
package step1;

public class TestPattern {

	public static void main(String[] args) {
		Person p1 = createPersonForTesting();
		System.out.println(p1.getFirstName());

	}
	
	public static Person createPersonForTesting() {
		
		Person person = new Person();
		person.setFirstName("FirstName");
		person.setLastName("LastName");
		person.setAddressOne("Address1");
		person.setAddressTwo("Address2");
		person.setSex("Man");
		person.setDriverLicence(false);
		person.setMarried(true);
		//멤버변수가 많다면 변수에 값 세팅이 어려워진다.
		
		return person;
	}

}
```


-------------

- 구현 예제
```java
package step2;

import java.time.LocalDate;
import java.time.Month;

public class TestPattern {

	public static void main(String[] args) {
		
		Person p1 = Person.builder()
				.FirstName("SUN")
				.LastName("Jung")
				.AddressOne("Seoul")
				.AddressTwo("Korea")
				.BirthDate(LocalDate.of(1989, Month.APRIL, 6))
				.Sex("female")
				.DriverLicence(true)
				.Married(false)
				.build();
		
		System.out.println(p1.getBirthDate());
	}
}
```
```java
package step2;

import java.time.LocalDate;

public class Person {
	
	private String firstName;
	private String lastName;
	private LocalDate birthDate;
	private String addressOne;
	private String addressTwo;
	private String sex;
	private boolean driverLicence;
	private boolean married;
	

	public String getFirstName() {
		return firstName;
	}
	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}
	public String getLastName() {
		return lastName;
	}
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
	public LocalDate getBirthDate() {
		return birthDate;
	}
	public void setBirthDate(LocalDate birthDate) {
		this.birthDate = birthDate;
	}
	public String getAddressOne() {
		return addressOne;
	}
	public void setAddressOne(String addressOne) {
		this.addressOne = addressOne;
	}
	public String getAddressTwo() {
		return addressTwo;
	}
	public void setAddressTwo(String addressTwo) {
		this.addressTwo = addressTwo;
	}
	public String getSex() {
		return sex;
	}
	public void setSex(String sex) {
		this.sex = sex;
	}
	public boolean isDriverLicence() {
		return driverLicence;
	}
	public void setDriverLicence(boolean driverLicence) {
		this.driverLicence = driverLicence;
	}
	public boolean isMarried() {
		return married;
	}
	public void setMarried(boolean married) {
		this.married = married;
	}
	
	public static PersonBuilder builder() {
		return new PersonBuilder();
	}

}
```
```java
package step2;

import java.time.LocalDate;

public class PersonBuilder {
	
	private String firstName;
	private String lastName;
	private LocalDate birthDate;
	private String addressOne;
	private String addressTwo;
	private String sex;
	private boolean driverLicence;
	private boolean married;
	
	public PersonBuilder FirstName(String firstName) {
		this.firstName = firstName;
		return this;
	}
	public PersonBuilder LastName(String lastName) {
		this.lastName = lastName;
		return this;
	}
	public PersonBuilder BirthDate(LocalDate birthDate) {
		this.birthDate = birthDate;
		return this;
	}
	public PersonBuilder AddressOne(String addressOne) {
		this.addressOne = addressOne;
		return this;
	}
	public PersonBuilder AddressTwo(String addressTwo) {
		this.addressTwo = addressTwo;
		return this;
	}
	public PersonBuilder Sex(String sex) {
		this.sex = sex;
		return this;
	}
	public PersonBuilder DriverLicence(boolean driverLicence) {
		this.driverLicence = driverLicence;
		return this;
	}
	public PersonBuilder Married(boolean married) {
		this.married = married;
		return this;
	}
	
	public Person build() {
		
		Person person = new Person();
		person.setFirstName(firstName);
		person.setLastName(lastName);
		person.setBirthDate(birthDate);
		person.setAddressOne(addressOne);
		person.setAddressTwo(addressTwo);
		person.setSex(sex);
		person.setDriverLicence(driverLicence);
		person.setMarried(married);
		return person;
	}
}

//필요한 만큼만 세팅하면 된다. 다양한 생성자를 만들 필요가 없어지게 된다.
```


&nbsp;

###### reference
[인프런 강의 : 디자인패턴withJAVA](https://www.inflearn.com/course/Design-pattern-java/dashboard)


-----