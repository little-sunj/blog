---
title: "DOCKER 명령어"
date: 2020-12-01T17:20:25+09:00
categories:
- devOps
- docker
tags:
- devOps
- docker
- 명령어
keywords:
- docker
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# DOCKER 명령어

&nbsp;

### 1. 이미지를 내려받아 컨테이너로 만들고 실행

```docker
docker run -it node
```

컴퓨터 내에서 node이미지를 찾아본 뒤, 없는 경우 docker hub에서 해당 이름으로 등록된 이미지를 내려받아 컨테이너로 만들고 실행한다.

이미지는 한번 내려받으면 여러개의 컨테이너를 생성할 수 있다.
(class로 인스턴스를 만드는 느낌..?)

여기서 `-it` 명령어는 컨테이너를 연 다음 그 환경 안에서 CLI를 사용하겠다는 의미.
(node라면 자바스크립트 콘솔이 사용 가능해진다)

&nbsp;

### 2. 다운받은 이미지 목록 확인

```docker
docker images
```

&nbsp;

### 3. 컨테이너 정보 확인

```docker
docker ps
```

현재 실행중인 컨테이너 정보를 확인할 수 있다. 컨테이너 고유 ID,  컨테이너명, 생성되어서 얼마간 일을 하고있는지 등을 보여준다.

```docker
docker ps -a
```

모든 컨테이너 정보를 확인할 수 있다.

&nbsp;

### 4. (컨테이너명)내에서 bash shell을 실행 (리눅스 가상환경)

```docker
docker exec -it (컨테이너명) bash
```

&nbsp;

### 5. 도커 버전 확인

```docker
docker -v
```

&nbsp;

### 6. 도커 이미지 다운 (태그는 옵션)

```terminal
docker pull {이미지명}:{태그}
// 예: docker pull python:3
```

&nbsp;

### 7. 이미지로 컨테이너 생성

```terminal
docker create {옵션} {이미지명}:{태그}
// 예: docker create -it python
```

&nbsp;

### 8. 만들어진 컨테이너 시작 (이미지에 CMD로 지정해놓은 작업 시키기)

```terminal
docker start {컨테이너 id 또는 이름}
```

&nbsp;

### 9. 컨테이너로 들어가기 (컨테이너 내 CLI 이용하기)

```terminal
docker attach {컨테이너 id 또는 이름}
```

&nbsp;

### 10. 동작중인 컨테이너 재시작

```terminal
docker restart {컨테이너 id 또는 이름}
```

&nbsp;

### 11. 도커 컨테이너의 내부 쉘에서 빠져나오기 (컨테이너를 종료)

```terminal
exit
또는
Ctrl + D
```

&nbsp;

### 12. 도커 컨테이너의 내부 쉘에서 빠져나오기 (컨테이너를 종료하지 않음)

```terminal
Ctrl + P, Q
```

&nbsp;

### 13. 컨테이너 삭제

```terminal
docker rm {컨테이너 id 또는 이름}
​
cf) 모든 컨테이너 삭제
docker rm `docker ps -a -q`
```

&nbsp;

### 14. 이미지 삭제

```terminal
docker rmi {옵션} {이미지 id}
​
컨테이너가 있을 시 강제삭제: -f 옵션 사용
```

&nbsp;

### 15. 모든 컨테이너와 이미지 등 도커 요소 중지 및 삭제

```terminal
# 모든 컨테이너 중지
docker stop $(docker ps -aq)
​
# 사용되지 않는 모든 도커 요소(컨테이너, 이미지, 네트워크, 볼륨 등) 삭제
docker system prune -a

# 아래를 복붙하여 함께 실행하면 편리합니다.
docker stop $(docker ps -aq)
docker system prune -a
(확인 질문에 y로 답하고 마무리.)
```

&nbsp;

### 16. 도커파일로 이미지 생성

```terminal
# Dockerfile 파일이 있는 디렉토리 기준.  마지막의 . 이 상대주소
docker build -t {이미지명} .
```

&nbsp;

### 17. 도커 컴포즈 실행

```terminal
# docker-compose 파일이 있는 디렉토리 기준
docker-compose up

백그라운드에서 데몬으로 돌도록 하려면 -d 옵션을 붙인다.
```

&nbsp;

-----

#### 명령어 옵션

| 옵션 | 설명  |
|--|--|
| -d | 데몬으로 실행(백그라운드에서 알아서 돌라고 하기) |
| -it | 컨테이너로 들어갔을 때 bash로 CLI입출력을 사용할 수 있도록 해준다 |
| --name {이름} | 컨테이너 이름 지정 |
| -p {호스트의 포트 번호}:{컨테이너의 포터 번호} | 호스트와 컨테이너의 포트 연결 |
| --rm | 컨테이너 종료시 (내부에서 돌아가는 작업이 끝나면) 컨테이너 제거 |
| -v {호스트의 디렉토리}:{컨테이너의 디렉토리} | 호스트와 컨테이너의 디렉토리 연결|

&nbsp;

-----

#### reference

[얄코-가장 쉽게 배우는 도커](https://www.youtube.com/watch?v=hWPv9LMlme8)

&nbsp;

-----
