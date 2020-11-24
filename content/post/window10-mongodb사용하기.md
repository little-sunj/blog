---
title: "Window10 Mongodb사용하기"
date: 2020-11-17T21:07:37+09:00
categories:
- database
- mongoDB
tags:
- window10 mongoDB
keywords:
- window10 mongoDB
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# WINDOW 10에서 MONGODB 사용하기

&nbsp;


인강이 맥 위주로 진행되어서 헤메다 찾은 튜토리얼이다.

#### reference
[https://www.youtube.com/watch?v=FwMwO8pXfq0](https://www.youtube.com/watch?v=FwMwO8pXfq0)

-----

### 요약

1. 설치 후 `c:/data/db` 폴더 생성
2. (실행파일이 있는 경로로 이동) `mongod` 실행 후 새 창을 열어 `mongo` 실행
3. mongod를 실행한 command prompt를 켜둘 것
4. `use 'db명'` 을 해도 저장된 데이터가 없으면 `show dbs`를 해도 나타나지 않는다.
5. `db.books.insert({"key":"value"})` 처럼 json형태로 저장한다.



![image](https://user-images.githubusercontent.com/28701069/99389821-f1599300-291a-11eb-9592-2cc1699b6fc0.png)

&nbsp;

- 실행파일 경로로 이동하지 않고 어디서든 명령어를 사용하고 싶다면 환경변수에 등록해주고 PC를 재부팅하면 된다.

-----

