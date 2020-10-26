---
title: "JAVA OOP"
date: 2020-10-26T22:51:09+09:00
categories:
- JAVA
- 객체지향 프로그래밍
tags:
- JAVA
- 객체지향 프로그래밍
- 스터디노트
keywords:
- 스터디노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# 객체지향 프로그래밍 (Object-Oriented Programming)




&nbsp;


## 특징

1. **추상화**
    - 구체적인 사물들의 공통적인 특징을 파악해서 하나의 개념(집합)으로 다룬다.
2. **캡슐화**
    - 정보은닉(information hiding) : 필요가 없는 정보는 외부에서 접근하지 못하도록 제한한다.
3. **일반화 관계**
    - 여러 개체들이 가진 공통된 특성을 부각시켜 하나의 개념이나 법칙으로 성립
4. **다형성**
    - 서로 다른 클래스의 객체가 같은 메시지를 받았을 때 각자의 방식으로 동작하는 능력


&nbsp;

## 5대 원칙 (SOLID)
- **S** : 단일 책임 원칙 (SPR : single responsibility principle)
    - 객체는 단 하나의 책임만 가져야 한다.
- **O** : 개방-폐쇄 원칙 (OCP : open closed principle)
    - 기존의 코드를 변경하지 않으면서 기능을 추가할 수 있도록 설계가 되어야 한다.
- **L** : 리스코프 치환 원칙 (LSP : liskov substitution principle)
    - 일반화 관계에 대한 이야기며, 자식 클래스는 최소한 자신의 부모 클래스에서 가능한 행위는 수행할 수 있어야 한다.
- **I** : 의존 역전 원칙 (DIP : dependency inversion principle)
    - 의존 관계를 맺을 때 변화하기 쉬운 것 또는 자주 변화하는 것보다는 변화하기 어려운 것, 거의 변화가 없는 것에 의존하라는 것이다.
- **D** : 인터페이스 분리 원칙 (ISP : interface segretation principle)
    - 인터페이스를 클라이언트에 특화되도록 분리시키라는 설계 원칙이다.


&nbsp;


-----
