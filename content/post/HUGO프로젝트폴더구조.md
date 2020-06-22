---
title: "HUGO프로젝트폴더구조"
date: 2020-06-07T17:54:58+09:00
categories:
- blog
- hugo
tags:
- hugo
- github page
keywords:
- hugo
---

# HUGO 프로젝트 폴더구조   



###### 2020.06.07   

### 마크다운 언어 연습 겸 커스터마이징 팁 기록   

기본적으로 프로젝트 루트 폴더내에서 우선적으로 파일들을 찾게되고, 없을시 themes내 파일들을 적용하게 된다.    

> 즉, 원본 테마 파일을 수정하지 않고, 루트로 복사하여 커스터마이징하는것이 바람직하다. (lookup order)   

layout/partials 폴더 내 파일들을 layout/index.html나 layout/_default경로내 파일들에 짜깁기 하게되기때문에 커스터마이징시 **partial폴더가 가장 중요**하다.   

세부 커스터마이징은 각 테마별로 제공하는 README를 참조하여 작업한다.   


> Written with [StackEdit](https://stackedit.io/).