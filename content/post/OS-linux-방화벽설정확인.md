---
title: "OS Linux 방화벽설정확인"
date: 2020-12-14T23:02:14+09:00
categories:
- OS
- linux
tags:
- OS
- linux
- firewall
keywords:
- linux
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# 방화벽 설정 확인

&nbsp;

- 방화벽 유무 확인
```
rpm -qa | grep firewalld
```

&nbsp;

- 방화벽 시작
```
systemctl start firewalld
systemctl enable firewalld
```

&nbsp;

- 포트 허용 & 재시작
```
firewall-cmd --permanent --zone=public --add-port=8080/tcp
firewall-cmd --reload
```

&nbsp;

- 설정된 방화벽 확인
```
firewall-cmd --permanent --list-all
```

&nbsp;

- LISTEN상태 포트 확인
```
netstat -nap | grep LISTEN

//8080을 확인하고 싶은 경우
netstat -an | grep 8080

```

&nbsp;

-----