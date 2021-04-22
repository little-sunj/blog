---
title: "GIT"
date: 2021-04-16T16:12:54+09:00
categories:
- Version Control System
- GIT
tags:
- GIT
- 인강노트
keywords:
- 인강노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# what is GIT

### intro

- Version Control System
	- Track History
	- Work Together
	- types
		- Centralized Type
			- 모든 팀 멤버가 중앙 서버에 연결
			- 가장 최신 소스를 내려받고 올린다
			- ex. SVN, Team Foundation Server
			- 서버에 문제 발생시 모두의 업무가 영향을 받는다는 단점이 있다
		- Distributed Type
			- 중앙 서버에 문제가 발생해도 타인과 다이렉트로 연결할 수 있다
			- ex. GIT, Mercurial

-----

- GIT
	- 가장 많이 사용되는 형상관리 시스템
	- 무료, 오픈소스
	- 빠르고 Scalable
	- Cheap Branching / Merging
	- SW개발자라면 필수로 공부할 것

- Using GIT
	- the command line
		- GUI툴은 한계가 있다. 언제나 사용할 수 있는것이 아닌 경우도 있고, 지원되지 않는 기능도 있을 수 있다.
	- code editors & IDEs
	- Graphical user interfaces
		- GitKraken이나 SourceTree가 가장 인기

-----

### Configuring Git

- settings
	- name
	- email
	- default editor
	- line ending handling  `core.autocrlf`
		- `\r` : **C**arriage **R**eturn , `\n` : **L**ine **F**eed
		- window인 경우
			- 줄의 끝이 `\r` 와 `\n`로 마크된다. 원격 리포지토리에 올리고 내려받을 때 carriage return을 제거, 추가해야한다.
			- `core.autocrlf`를 `true`로 세팅한다.
		- MacOS인 경우
			- 줄의 끝이 `\n`로 마크된다.
			- `core.autocrlf`를 `input`으로 세팅한다.
	- levels
		- System : all users
		- Global : all repositories of the current user
		- Local : the current repository



-----

&nbsp;

```
git init //Initialize empty git repository
rm -rf .git //remove git
```

- git commit에 포함되는 정보
	- ID
	- message
	- Date/time
	- author
	- complete snapshot


-----

&nbsp;

- commit은 너무 크지도 작지도 않은 정도가 좋다. check point마다 진행한다.
- Bug Fix 와 typographic error는 따로 커밋한다.

-----

&nbsp;

- reference
 [공부자료](https://codewithmosh.com/)

