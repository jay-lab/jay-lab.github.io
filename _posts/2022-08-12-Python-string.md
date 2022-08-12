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

# % 연산자
```python
first_name = 'John'
last_name = 'Doe'
age = 22

print(('He is %s %s and is %i years old.') % (first_name, last_name, age))
```
- %s: str 타입받기
- %i: int 타입받기
- %f: float 타입받기

위 처럼, 여러개의 요소(문자열) 전달이 필요할때 tuple타입으로 순서대로 선언.
전달할 요소가 하나라면 튜플로 하지않아도 된다.

# str.format
```python
first_name = 'John'
last_name = 'Doe'
age = 22

print('He is {first_name} {last_name} and he is {age} years old'
      .format(first_name=first_name,
              last_name=last_name,
              age=age))
```
파이썬3 부터 지원하는 방식.
% 연산자 방식과 달리 타입을 일일이 명시하지 않아도 된다는 장점.

# f문
```python
first_name = 'John'
last_name = 'Doe'
age = 22

print(f'He is {first_name} {last_name} and he is {age} years old.')
```
파이썬3.6 부터 지원하는 방식
여러줄이 띄어져있는 경우에도 사용 가능
```python
first_name = 'John'
last_name = 'Doe'
age = 22

print(f'''He is {first_name} {last_name}
and he is {age} years old.''')
```