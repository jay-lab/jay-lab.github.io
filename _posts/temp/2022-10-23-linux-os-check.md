---
layout: single
title: "[LINUX] OS Version Check"
categories: LINUX
# tag: python
tag: [LINUX, OS]
toc: true
toc_label: "Contents" # toc 제목
toc_icon: "cog" # toc 아이콘(톱니바퀴)
author_profile: false
classes: wide
sidebar:
    nav: "docs"



---



**리눅스 OS 확인하는 방법 ** 
<br> → 간단 정리
{: .notice--info}



# # 일반적인 커널에 대한 정보

```python
[root@localhost ~]# uname -a
Linux localhost.localdomain 2.6.32-71.el6.x86_64 #1 SMP Fri May 20 03:51:51 BST 2011 x86_64 x86_64 x86_64 GNU/Linux
```

# # OS버전에 대한 정보 1

```python
[root@localhost ~]# cat /etc/issue
CentOS Linux release 6.0 (Final)
Kernel \\r on an \\m
```

# # OS버전에 대한 정보 2

```python
[root@localhost ~]# cat /etc/redhat-release
CentOS Linux release 6.0 (Final)
```

# # OS버전에 대한 정보 3

```python
[root@localhost ~]# cat /etc/*release*
CentOS Linux release 6.0 (Final)
CentOS Linux release 6.0 (Final)
CentOS Linux release 6.0 (Final)
cpe:/o:centos:linux:6:GA
```

# # OS bit 확인하기

```python
[root@localhost ~]# getconf LONG_BIT
64
```



※ 그 외 lsb_release -a 도 있음