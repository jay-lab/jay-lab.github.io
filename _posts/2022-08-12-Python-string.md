---
layout: single
title: "[파이썬] 문자열 붙이기"
categories: PYTHON
# tag: python
tag: [POYTHON]
toc: true
toc_label: "My Table of Contents" # toc 제목
toc_icon: "cog" # toc 아이콘(톱니바퀴)
author_profile: false
#classes: wide
sidebar:
    nav: "docs"
---

PYTHON에서 문자열을 붙이는 몇가지 방법들

# 가장 기본적인 방법

```python
first_name = 'John'
last_name = 'Doe'
age = 22

print('He is ' + first_name + ' ' + last_name + ', and he is ' + str(age) + ' years old.')
```