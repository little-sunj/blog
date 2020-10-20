---
title: "다양한AOP구현방법"
date: 2020-10-17T16:40:59+09:00
categories:
- JAVA
- SPRING
tags:
- JAVA
- SPRING
- 인강노트
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# 다양한 AOP 구현방법

&nbsp;


1. **컴파일**
   - A.java ---(AOP)---> A.class 
   컴파일할때 중간에 추가된다 : AspectJ컴파일러

2. **바이트코드 조작**
   - A.java ---> A.class ---(AOP)---> 메모리
    클래스 로더가 클래스를 읽어서 메모리에 올릴때 조작

3. **프록시 패턴**
   - (스프링 AOP가 사용하는 방법)


&nbsp;

- 프록시 예시
```java


public interface Payment {
    void pay(int amount);
}


public class Store {
    Payment payment;

    public Store(Payment payment) {
        this.payment = payment;
    }

    public void buysomething(int amount) {
        payment.pay(amount);
    }
}

public class Cash implements Payment {
    @Override
    public void pay(int amount) {
        System.out.println(amount + " 현금 결제");
    }
}


//프록시
public class CashPerf implements Payment {

Payment cash = new Cash();

    @Override
    public void pay(int amount) {
        // if (amout < 100) {
        //     System.out.println(amount + " 신용카드");
        // } else {
        //     cash.pay(amout);
        // }
        //CashPerf가 알아서 판단 cash를 쓸지, creditcard를 쓸지)

        StopWatch stopWatch = new StopWatch();
        stopWatch.start();

        cash.pay(amount);

        stopWatch.stop();
        System.out.println(stopWatch.prettyPrint());
    }
}


public class StoreTest {

    @Test
    public void testPay() {
        Payment cashPerf = new CashPerf(); //프록시코드사용
        Store store = new Store(cashPerf);
        store.buysomething(amount:100);
    }
}

//새로운 코드를 추가하였지만 기존 코드를 건드리지 않았다는 점이 중요하다.
//스프링에서는 @Transactional같은 어노테이션이 붙은 경우 프록시가 생성된다.

```


&nbsp;

-----