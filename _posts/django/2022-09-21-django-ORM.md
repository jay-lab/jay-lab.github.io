---
layout: single
title: "Django 기초"
categories: [DJANGO]
tag: [DJANGO]
toc: true
toc_label: "Contents" # toc 제목
toc_icon: "cog" # toc 아이콘(톱니바퀴)
author_profile: false
sidebar:
    nav: "docs"
---



# 터미널에서 DB 조회하기 (ORM)

> **ORM :**  
> 객채(Object)의 관계(Relational)를 연결(Mapper)해주는 것.  
>
> 객체 지향적인 방법을 사용하여 데이터베이스의 데이터를 쉽게 조작.  
>
> 즉 Django의 ORM은 파이썬과 데이터베이스의 SQL사이의 통역사 역할을 하며,
>
> SQL(Structed Query Language)을 사용하지 않고도 파이썬만으로 데이터베이스 쿼리 요청 가능

> **QuerySet :**  
> 특정 모델을 통해 DB를 조회하고 그 DB조회 결과값을 QuerySet(쿼리셋)이라고 부르고 있다.

## **ORM :**  

- 객채(Object)의 관계(Relational)를 연결(Mapper)해주는 것.  
- 객체 지향적인 방법을 사용하여 데이터베이스의 데이터를 쉽게 조작.  
- 즉 Django의 ORM은 파이썬과 데이터베이스의 SQL사이의 통역사 역할을 하며,  
  SQL(Structed Query Language)을 사용하지 않고도 파이썬만으로 데이터베이스 쿼리 요청 가능

### ORM 장점

- Django 에 구현된 각 RDBMS 별 Wrapper를 통해 RDBMS 종류에 상관없이 데이터 조작 가능
- 구조화된 언어인 SQL을 대체하려는 목적만큼, 직관적인 객체지향 프로그래밍
- 기존의 DB 기반 구성을 객체 기반 구성으로 확장하여, 컴포넌트를 조합하는 방식으로 개발 가능

### ORM 단점

- SQL 구문의 생성을 추상화하여 구현 하였으므로,  
  복잡한 쿼리의 경우 비효율적으로 SQL 구문이 생성될 수 있다.



