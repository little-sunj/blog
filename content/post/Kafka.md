---
title: "Kafka"
date: 2021-05-20T19:04:32+09:00
categories:
- backend
- Kafka
tags:
- backend
- Kafka
keywords:
- Kafka
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# Kafka

&nbsp;

### Apache Kafka란

- 실시간으로 기록 스트림을 publish, subscribe, 저장 및 처리할 수 있는 분산 데이터 스트리밍 플랫폼
- 엔터프라이즈 메시징 시스템의 대안. (LinkedIn이 내부 메시지 처리 시스템으로 개발. 오픈소스)
- 클러스터로서 작동한다

> **클러스터** : 각기 다른 서버들을 하나로 묶어서 하나의 시스템같이 동작하게 함으로써, 클라이언트들에게 고가용성의 서비스를 제공하는 것. 묶여있는 특정 시스템중 하나에 장애가 발생하면, 정보 제공 포인트는 다른 정상서버로 이동한다. 사용자로 하여금 서버 기반 정보를 지속적으로, 끊기지 않게 제공받을 수 있게 한다.

&nbsp;

-----

### **Pub-Sub 모델**
- 발행/구독 모델
  - Pub/Sub모델이 아닌 경우에 비해 각자의 역할이 분리되고 구조가 훨씬 단순해진다.
- publisher는 메시지를 topic을 통해 카테고리화 한다.
- 분류된 메시지를 받기 원하는 receiver는 그 topic을 구독(subscribe)함으로써 메시지를 읽어 올 수 있다.
- publisher와 subscriber모두 topic만 바라보면 되고 서로에 대해 몰라도 된다.
  - 서비스 서버들은 Kafka로 메시지를 보내는 역할만 하면 되고, 모니터링이나 분석 시스템들도 서비스 서버들의 상태와 상관없이 Kafka에 저장되어 있는 메시지만 읽어오면 된다.
- 하나의 토픽에 multi 프로듀서 / 컨슈머 접근이 가능한 구조다.
- 1개의 프로듀서는 여러개의 토픽에 메시지 전송이 가능하다.

&nbsp;

| 일반 메시징 시스템 | Kafka |
|--|--|
| 컨슈머가 메시지를 읽어가면 큐에서 바로 메시지를 삭제한다. | 컨슈머가 읽어가도 보관주기 동안 디스크에 남아있다.  |

> 트래픽이 일시적으로 폭증해 컨슈머의 처리가 늦어지더라도 Kafka의 디스크에 메시지가 저장되어 있기 때문에 컨슈머는 메시지 손실 없이 작업할 수 있다.
> 컨슈머를 잠시 중단하고 버그 Fix 후 Kafka 디스크에 있는 메시지를 다시 가져가 작업하는 것도 가능하다.

&nbsp;

-----

### Topic & partition

- 메시지는 topic으로 분류되고, topic은 여러개의 partition으로 나눠진다.
- partition내 한 칸은 로그라고 부른다.
- 데이터는 로그 1개에 순차적으로 append된다.
- 메시지의 위치를 나타내는게 offset인데, 배열의 index를 생각하면 쉬울듯

![0_dEeuSOb7Z8K7---q](https://user-images.githubusercontent.com/28701069/118963504-76dcff00-b9a1-11eb-9eb7-43a2067602b5.png)

- 몇 천건의 메시지가 동시에 Kafka에 쓰여진다고 가정했을 때 순차적으로 append되면 처리가 버거을 수 있기 때문에 여러개의 파티션을 두어 분산저장을 하게 된다. (병렬처리)
- **그러나 한 번 늘린 파티션은 절대로 줄일 수 없기 때문에 운영중에 파티션을 늘리는 것은 신중해야한다.**
- 파티션을 늘릴시 메시지는 round-robin방식으로 쓰여진다. 메시지 순서가 중요한 모델(ex.증권시스템)이라면 상당히 위험할 것이다.
- Consumer Group은 하나의 topic에 대한 책임을 가지고 있다. 그룹내 특정 컨슈머가 다운되면, 특정 파티션에 대한 소비를 할 수 없는 상황이 오게 될 것이다. 이런경우 rebalance를 통해 그룹 내 다른 컨슈머가 해당 파티션의 소비를 이어서 하게 된다. offset의 정보를 그룹내에서 서로 공유하기 때문에 다운되기 직전 offset위치를 알고 그 이후부터 소비하게 된다.


&nbsp;

-----

### Broker & Zookeepr

- broker = 카프카 서버
- 동일한 노드내에서 여러개의 broker서버를 띄울 수도 있다
- zookeeper는 이러한 분산 메시지 큐의 정보를 관리해준다
- zookeeper는 반드시 실행되어야 한다

&nbsp;

-----

### Replication

- broker여러대를 사용하는 경우 하나의 서버만 leader가 되고 나머지는 follower가 된다.
- producer가 메시지를 쓰고, consumer가 메시지를 읽는 건 오로지 leader가 담당한다.
- 나머지 follower들은 leader와 싱크를 맞춘다.
- leader가 죽는 경우 나머지 follower중 하나가 leader가 된다.

&nbsp;

-----


&nbsp;

#### reference
- [RedHat Apache Kafka(아파치 카프카)란? 소개](https://www.redhat.com/ko/topics/integration/what-is-apache-kafka)
- [Kafka 이해하기](https://medium.com/@umanking/%EC%B9%B4%ED%94%84%EC%B9%B4%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C-%EC%9D%B4%EC%95%BC%EA%B8%B0-%ED%95%98%EA%B8%B0%EC%A0%84%EC%97%90-%EB%A8%BC%EC%A0%80-data%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C-%EC%9D%B4%EC%95%BC%EA%B8%B0%ED%95%B4%EB%B3%B4%EC%9E%90-d2e3ca2f3c2)
- [Kafka의 특징](https://goodgid.github.io/Kafka-Features/)
- [클러스터란 , 서버클러스터 정의하기](https://allpartner.tistory.com/11)

&nbsp;


-----