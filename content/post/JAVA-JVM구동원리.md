---
title: "JAVA JVM구동원리"
date: 2020-09-07T17:36:15+09:00
categories:
- JAVA
- JVM
tags:
- JAVA
- JVM
keywords:
- 인강노트

---

<!--more-->
# JVM




&nbsp;


- **JAVA - 운영체제 독립적.**

```
bin 폴더:   .class 실행을 위한 파일
            (바로 실행되는 파일은 아니다. 이를 운영체제에 맞게 실행해주는 것이 JVM) 
            byte code
src 폴더:   .java 소스파일
```
&nbsp;

&#10102; `.java`    
&#10103; `javac.exe` / **1차 컴파일** &#10140; byte code (OS독립적인 형태)   
&#10104; `.class`  `java.exe` : jvm통해 실행   
&#10105; 실행엔진 JVM에서 byte code를 각 OS에 맞게 **2차 컴파일** &#10140; exe code   

&nbsp;


-----

