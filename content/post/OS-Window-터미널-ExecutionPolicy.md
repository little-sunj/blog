---
title: "OS Window 터미널 ExecutionPolicy"
date: 2020-12-01T16:56:53+09:00
categories:
- OS
- window
tags:
- OS
- window
- Terminal
- 실행정책
keywords:
- Window
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# WINDOW TERMINAL ExecutionPolicy 설정

&nbsp;

설치된 http server 버전을 확인하고자 `http-server --version`를 윈도우 터미널에 입력했더니 아래와 같은 메시지가 출력되었다.

```
http-server : 이 시스템에서 스크립트를 실행할 수 없으므로 C:\Users\SunJung\AppData\Roaming\npm\http-server.ps1 파일을
로드할 수 없습니다. 자세한 내용은 about_Execution_Policies(https://go.microsoft.com/fwlink/?LinkID=135170)를 참조하십시오.
위치 줄:1 문자:1
+ http-server --version
+ ~~~~~~~~~~~
    + CategoryInfo          : 보안 오류: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```

(위 메시지에서 제공하는 링크를 타면 상세 내용을 확인할 수 있다.)<br/>
파워셀이 안전상의 이유로 스크립트를 허용하지 않는단 이야기.
아래와 같이 권한을 설정하여 스크립트를 실행할 수 있다.

```terminal
Set-ExecutionPolicy -Scope CurrentUser
```

그러면 터미널이 매개변수에 대한 값을 제공하라고 하는데
`Unrestricted`, `RemoteSigned`, `AllSigned`, `Restricted`, `Default`, `Bypass`, `Undefined` 중 하나의 값을 입력해야 한다.

권한 제한을 풀고싶은 케이스니까

```terminal
ExecutionPolicy: Unrestricted
```

라고 입력하면 스크립트를 실행할 수 있게된다.

&nbsp;

-----
