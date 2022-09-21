---
layout: single
title: "Django ORM $ QuerySet"
categories: [DJANGO]
tag: [ORM, QuerySet]
toc: true
toc_label: "Contents" # toc 제목
toc_icon: "cog" # toc 아이콘(톱니바퀴)
author_profile: false
sidebar:
    nav: "docs"
---



# 터미널에서 DB 조회하기 (ORM)

## **ORM**

- 객채(Object)의 관계(Relational)를 연결(Mapper)해주는 것.  
- 객체 지향적인 방법을 사용하여 데이터베이스의 데이터를 쉽게 조작.  
- 즉 Django의 ORM은 파이썬과 데이터베이스의 SQL사이의 통역사 역할을 하며,  
  SQL(Structed Query Language)을 사용하지 않고도 파이썬만으로 데이터베이스 쿼리 요청 가능

### ORM의 장점

- Django 에 구현된 각 RDBMS 별 Wrapper를 통해 RDBMS 종류에 상관없이 데이터 조작 가능
- 구조화된 언어인 SQL을 대체하려는 목적만큼, 직관적인 객체지향 프로그래밍
- 기존의 DB 기반 구성을 객체 기반 구성으로 확장하여, 컴포넌트를 조합하는 방식으로 개발 가능

### ORM의 단점

- SQL 구문의 생성을 추상화하여 구현 하였으므로,  
  복잡한 쿼리의 경우 비효율적으로 SQL 구문이 생성될 수 있다.

<br>

## QuerySet

: 특정 모델을 통해 DB를 조회하고 그 DB조회 결과값을 QuerySet(쿼리셋)이라고 부르고 있다.



## ORM 사용 예

### 예시 1.

```python
class MainCategory(models.Model):
    name = models.CharField(max_length=200)

    class Meta:
        db_table = 'main_categories'
```

<br>shell에서 `MainCategory.objects.create(name='스킨 케어')` 하면 데이터 생성 완료

```python
MainCategory.objects.all()
# <QuerySet [<MainCategory: MainCategory object (1)>]>
```

이 처럼 모델과 ORM을 통해 쿼리셋을 반환 받은 것을 확인 가능

*위의 ORM이 어떤 SQL 쿼리를 대체한 것인지 확인하는 방법

```python
uery = MainCategory.objects.all().query
print(query)
# `SELECT main_categories`.`id`, `main_categories`.`created_at`, `main_categories`.`updated_at`, `main_categories`.`name` FROM `main_categories`
```



### 예시 2.

터미널에서 장고와 인터렉티브하게 커뮤니케이션 할 수 있는 console을 호출

```shell
python manage.py shell
```

```python
# 객체 조회
>>> from blog.models import Post

>>> Post.objects.all()
<QuerySet [<Post: my post title>, <Post: another post title>]>


# 객체 생성
>>> from django.contrib.auth.models import User

>>> User.objects.all()
<QuerySet [<User: admin>]>

>>> me = User.objects.get(username='admin')

>>> Post.objects.create(author=me, title='Sample title', text='Test')
(
	파이썬 콘솔에서 추가한 게시글은 아래와 같이 조회되긴 하지만, commit과 같은 개념의
	publish()를 적용해주어야 한다. 
	>>> post = Post.objects.get(title="Sample title")
	>>> post.publish()
)

>>> Post.objects.all()
<QuerySet [<Post: my post title>, <Post: another post title>, <Post: Sample title>]>


# 필터링하기
>>> Post.objects.filter(author=me)
[<Post: Sample title>, <Post: Post number 2>, <Post: My 3rd post!>, <Post: 4th title of post>]

>>> Post.objects.filter(title__contains='title')
[<Post: Sample title>, <Post: 4th title of post>]


# 정렬하기(오름차순)
>>> Post.objects.order_by('created_date')
[<Post: Sample title>, <Post: Post number 2>, <Post: My 3rd post!>, <Post: 4th title of post>]
# 정렬하기(내림차순)
>>> Post.objects.order_by('-created_date')
[<Post: 4th title of post>,  <Post: My 3rd post!>, <Post: Post number 2>, <Post: Sample title>]


# 쿼리셋 연결(chaining)
>>> Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')

>>> exit()
```

