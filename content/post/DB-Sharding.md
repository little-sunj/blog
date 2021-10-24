---
title: "DB Sharding"
date: 2021-10-20T16:22:24+09:00
categories:
- DB
- postgresql
tags:
- DB
- sharding
keywords:
- tech
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# DB Sharding

1. **sharding 이란?**
   - 관계형 데이터베이스에서 대량의 데이터를 처리하기 위해 데이터를 파티셔닝 하는 기술.
   - DBMS레벨에서 데이터를 나누는 것이 아니고 데이터베이스 자체를 분할하는 방식이다.
   - 따라서 어플리케이션 레벨에서 구현해야 한다.
   - ex)고객정보를 미국고객 샤드A, 아시아 고객 샤드B, 유럽고객 샤드C와 같은 방식으로 분할해서 저장

2. **sharding 전략**
    - **Vertical Partitioning**
      - 테이블별로 서버 분할
      - ex) 사용자 프로필정보용 서버, 사용자 친구리스트용 서버, 사용자가 만든 컨텐츠용 서버 등으로 분할
      - 장점 : 구현이 간단하고 전체 시스템에 큰 변화가 필요 없다.
      - 단점 : 각 서버의 데이터가 점점 거대해지면 추가 샤딩이 필요해진다.
    - **Range Based Partitioning**
      - 하나의 feature(또는 table)가 점점 거대해지는 경우 서버를 분리하는 방식.
      - ex)사용자가 많은 경우 지역정보를 이용해 분리하거나 년도별로 분리하거나 우편번호로 분리하는 등
      - 주의점 : 데이터 분할 방법이 예측 가능해야 한다.
    - **Key or Hash Based Partitioning**
      - 웹2.0사이트들의 기본 파티셔닝 방식. 엔티티를 해쉬함수에 넣어서 나오는 값을 이용해 서버를 정하는 방식
      - 주의점 : 해쉬결과 데이터가 균등하게 분포되도록 해쉬함수를 정하는것이 중요하다.
      - 서버 수를 늘리기 위해 해쉬함수를 변경하는 작업이 무지무지 비싸다.
    - **Directory Based Partitioning**
      - 파티셔닝 메커니즘을 제공하는 추상화된 서비스를 만드는 것이다. (DB엑세스 코드와는 떨어져 있는) 샤드키를 look-up할 수 있으면 되므로 구현은 DB와 캐시를 적절히 조합해서 만들 수 있다.
  
3. **문제점 및 고려사항**
    - **데이터 재분배**
      - sharding된 DB의 물리적인 용량한계나 성능한계에 다르면 shard의 수를 늘리는 scale-up작업이 필요하다. 서비스 정지 없이 scale-up할 수 있도록 설계방향을 잡아야 한다.
    - **데이터 조인하기**
      - sharding-db반의 조인이 불가능한 점을 염두해야 한다. shard의 목적이 대용량 데이터 처리이므로 대용량처리시 수행성능을 위해 데이터 중복은 trade-off관계임을 알아 둘 것.
    - **데이터 파티션하는 방법**
      - 해쉬함수를 잘 설계해야한다.
    - **트랜잭션 문제**
      - global transaction을 사용하면 shard DB간의 트랜잭션도 가능하다. 성능저하의 문제가 있다.
    - **Global unique key**
      - DBMS에서 제공하는 auto-increament를 사용하면 key가 중복될 수 있기 때문에, application레벨에서 key생성을 담당해야 한다.
    - **데이터는 작게**
      - table단위를 가능한 작게 만들어야 한다.


----------


# Herding elephants: Lessons learned from sharding Postgres at Notion 요약


### Deciding when to shard
지난 한 세기동안 수많은 블로그 포스트들이 조급한 sharding의 위험성에 대해 상세히 설명해왔다.
- 유지보수 부담 증가, application level 코드에서 새로 발견되는 제약들, 독립적인 아키텍처 구성 등.
우리에게 scale sharding은 언제 도입할지의 문제일 뿐, 불가피한 것이었다.

급변 포인트는 postgres의 `VACUUM`프로세스가 지속적으로 멈추기 시작하면서 왔다. 데이터베이스가 dead tuple들로부터 디스크space를 되찾는것을 막기 때문이었다. 디스크 용량이 커지면서, postgres의 safety 매커니즘 (기존 데이터를 지키기 위해 모든 write프로세스를 멈추는 transaction ID(TXID) wraparound) 가능성에 걱정이 커졌다. 
제품에 실질적인 위협이 되는 TXID wraparound 때문에 infrastructure팀은 두배로 일하게 되었다.

### Designing a sharding scheme

##### Application-level sharding

##### Decision 1: Shard all data transitively related to block

##### Decision 2: Partition block data by workspace ID

##### Decision 3: Capacity planning

### Migrating to shards

##### Double-writing with an audit log

##### Backfilling old data

##### Verifying data integrity

### Difficult lessons learned





---------



https://genesis8.tistory.com/211
https://www.notion.so/blog/sharding-postgres-at-notion