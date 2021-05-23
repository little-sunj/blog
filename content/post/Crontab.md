---
title: "Crontab"
date: 2021-05-23T21:23:13+09:00
categories:
- backend
- Crontab
tags:
- backend
- Crontab
keywords:
- Crontab
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# Crontab

&nbsp;

### 기본

- 한 줄에 하나의 명령만 쓴다. 

- 크론탭 설정
```shell
crontab -e 
```

- 크론탭 내용보기
```shell
crontab -l
#cat명령어를 사용한 것처럼 표준 출력으로 내용 확인가능 
```

&nbsp;

-----

### 주기설정

```shell
*       *       *       *       *
분      시간    일      월      요일
(0-59)  (0-23)  (1-31)  (1-12)  (0-7)  #0과 7은 일요일. 1이 월요일이고 6이 토요일
```

- 예시
```shell
# 매분 test.sh 실행
* * * * * /home/script/test.sh

# 매주 금요일 오전 5시 45분에 test.sh 를 실행
45 5 * * 5 /home/script/test.sh

# 매일 매시간 0분, 20분, 40분에 test.sh 를 실행
0,20,40 * * * * /home/script/test.sh

# 매일 1시 0분부터 30분까지 매분 tesh.sh 를 실행
0-30 1 * * * /home/script/test.sh

# 매 10분마다 test.sh 를 실행
*/10 * * * * /home/script/test.sh

# 5일에서 6일까지 2시,3시,4시에 매 10분마다 test.sh 를 실행
*/10 2,3,4 5-6 * * /home/script/test.sh

```

&nbsp;

-----

### 로깅


- 리눅스 특수문자
```shell
# '>' 표준 출력 (new)
$ ls > test.txt # 표준 출력을 파일에 기록한다. 다만 test.txt 파일은 업데이트가 아닌 새로 작성을 하게 되므로 주의.

# '>>' 표준 출력 (append)
$ ls >> test.txt # 표준 출력을 파일 끝에 덧붙인다.

# '<' 표준 입력
$ cat < test.txt # 파일에서 표준 입력으로 읽어들인다.

# '*' 모든 문자와 일치하는 와일드 카드 문자
$ ls tes* # test.txt, tes/123.txt 등 일치하는 모든 파일/디렉토리(내부)가 출력된다.
# '?' 하나의 문자와 일치하는 와일드 카드 문자
$ ls test.tx? # test.txt, test.txx 등 하나 일치한 파일을 출력한다.

# 파이프 문자 예제
# ps 명령어를 통한 표준 출력을 프로세스 정보 중 "tomcat"이 들어간 프로세스를 찾기 위해 표준 입력으로 삽입한다.
$ ps -ef | grep tomcat

; # 명령의 끝을 나타낸다.
|| # 이전의 명령이 실패하면 실행하는 조건문 문자
&& # 이전의 명령이 성공하면 실행하는 조건문 문자
& # 명령을 백그라운드에서 실행한다.
$ # 변수에 접근할 수 있는 문자

0 # stdin (표준 입력)
1 # stdout (표준 출력)
2 # stderr (에러 출력)

# 표준 에러 리다이렉션 ( stderr 만 출력 )
$ ./test.sh >> ./test.log 2>&1 # test.sh을 실행하면서 나온 표준 에러를 test.log 파일에 덧붙여 쓴다.

# 표준 출력이 필요 없다.
$ ./test.sh >> /dev/null 2>&1
```

- 로깅
```shell
* * * * * /home/script/test.sh > /home/script/test.sh.log 2>&1
# 매 분마다 test.sh.log 파일이 갱신되어 작업 내용 확인 가능
# 2>&1 제거시 쉘스크립트에서 표준 출력 내용만 나온다.
```


&nbsp;

-----

### 크론탭 백업

- 크론탭 내용을 txt 파일로 만들어 저장
```shell
crontab -l > /home/bak/crontab_bak.txt

#자동화 (매일 오후 11시 50분에 크론탭을 백업)
50 23 * * * crontab -l > /home/bak/crontab_bak.txt
```


&nbsp;

-----


&nbsp;

#### reference
- [리눅스 크론탭(Linux Crontab) 사용법](https://jdm.kr/blog/2)
  
&nbsp;


-----