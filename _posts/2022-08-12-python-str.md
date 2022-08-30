---
layout: single
title: "[PYTHON] 문자열 붙이기"
categories: PYTHON
# tag: python
tag: [PYTHON]
toc: true
toc_label: "Contents" # toc 제목
toc_icon: "cog" # toc 아이콘(톱니바퀴)
author_profile: false
classes: wide
sidebar:
    nav: "docs"
---

<br>
PYTHON에서 문자열을 붙이는 4가지 방법  
<br>

# 1. 가장 기본적인 방법
```python
first_name = 'John'
last_name = 'Doe'
age = 22

print('He is ' + first_name + ' ' + last_name + ', and he is ' + str(age) + ' years old.')
```
---
<br>

# 2. % 연산자
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

---
<br>

# 3. str.format
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

---
<br>

# 4. f문
```python
first_name = 'John'
last_name = 'Doe'
age = 22

# 방법1
print(f'He is {first_name} {last_name} and he is {age} years old.')

# 방법2 - 여러줄이 띄어져있는 경우에도 사용 가능
print(f'''He is {first_name} {last_name}
and he is {age} years old.''')
```
파이썬3.6 부터 지원하는 방식
