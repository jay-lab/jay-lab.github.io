---
layout: single
title: "[PYTHON] injection 대응"
categories: PYTHON
# tag: python
tag: [PYTHON, INJECTION]
toc: true
author_profile: false
sidebar:
    nav: "docs"
---

***Injetion : 악의적인 사용자가 보안상의 취약점을 이용하여, 임의의 SQL 문을 주입하고 실행되게 하여 데이터베이스가 비정상적인 동작을 하도록 조작하는 행위***

# Injection

실무에서 접하게된 상황은 고객사 측 보안팀에서 모의해킹 테스트 과정 중 Injection 공격 시도 후 이에 대한 조치를 요구했을 때였다.  
해당 보안팀에서는 홑따옴표(') 혹은 주석(--)과 같은 문자를 활용한 쿼리 일부를  DB와 통신하는 API의 인자값으로 넘겨 모의해킹을 했던 것이 injection을 알게된 계기였다.

# Injection 공격 예시

```javascript
/* 어떠한 POST방식 API에 
아래와 같은 인자값을 넘기는 것이 정상적인 방법일 때*/
{ testKey : "testValue" }

/* 악의적인 사용자는 다음과 같이 인자값을 설정하여 쿼리를 조작할 수 있다. */
{ testKey : "testValue' and '해킹'='해킹" }

```

위 인자값이 SQL Query에 적용되면

정상적인 인자값을 넘겨받은 쿼리는 다음과 같이 설정되지만,
```
select * from testTable where key = 'testValue'
```

Injection 공격을 시도한 쿼리는 아래처럼 되겠다.
```
select * from testTable where key = 'testValue' and '해킹'='해킹'
```

일단 위 둘은 같은 결과를 반환하겠지만, 모의해킹 중 저러한 파라미터를 넘겼음에도 같은 결과가 반환되었다는 것 자체만으로 Injection 공격에 뚫렸다는 의미.

진짜 해커라면 이를 악용하여 DB를 어떻게든 할 수 있게된다.(테이블 삭제, 고객정보 탈취, 데이터 변경  등)

<br>

# python에 대한 injection 방어 코드

## python에서 실무에서 사용된 DB 호출 코드 방식 (Injection 대응 전)
```python
import psycopg2
from psycopg2.extras import RealDictCursor

# DB 연결 준비
conn = psycopg2.connect("dbname={} user={} host={} password={}".format(db_name, db_user, db_host, db_pass))

# DB 호출함수 선언
def fetch(conn, query):
    result = []
    cursor = conn.cursor(cursor_factory=RealDictCursor)
    cursor.execute(query) # SQL 실행

    # DB조회 결과값 담기
    raw = cursor.fetchall()
    for line in raw:
        result.append(line)

    return result

# SQL 쿼리 선언
query = "select pjt_no, item_type from testTable "
query += "where pjt_no = '" + pjt_no + "' "
query += "and item_type = '" + type_cd2 + "'"

# DB 호출함수 실행
result = fetch(conn=conn, query=query)
```

## Injection 대응 후
```python
import psycopg2
from psycopg2.extras import RealDictCursor

# DB 연결 준비
conn = psycopg2.connect("dbname={} user={} host={} password={}".format(db_name, db_user, db_host, db_pass))

# DB 호출함수 선언
def fetch(conn, query, sql_arg):
    result = []
    cursor = conn.cursor(cursor_factory=RealDictCursor)
    cursor.execute(query, sql_arg) # SQL 실행

    # DB조회 결과값 담기
    raw = cursor.fetchall()
    for line in raw:
        result.append(line)

    return result

# SQL 쿼리 선언
query = "select pjt_no, item_type from testTable "
query += "where pjt_no = %(pjt_no)s"
query += "and item_type = %(item_type)s"
sql_arg = {
  'pjt_no' : pjt_no,
  'item_type' : item_type,
}

# DB 호출함수 실행
result = fetch(conn=conn, query=query, sql_arg=sql_arg)
```

## Injection 대응방법 2 - 딕셔너리 타입 말고 tuple 타입으로 넘기는 것도 가능
```python
import psycopg2
from psycopg2.extras import RealDictCursor

# DB 연결 준비
conn = psycopg2.connect("dbname={} user={} host={} password={}".format(db_name, db_user, db_host, db_pass))

# DB 호출함수 선언
def fetch(conn, query, sql_arg):
    result = []
    cursor = conn.cursor(cursor_factory=RealDictCursor)
    cursor.execute(query, sql_arg) # SQL 실행

    # DB조회 결과값 담기
    raw = cursor.fetchall()
    for line in raw:
        result.append(line)

    return result

# SQL 쿼리 선언
query = "select pjt_no, item_type from testTable "
query += "where pjt_no = %s"
query += "and item_type = %s"
sql_arg = (pjt_no, item_type)

# DB 호출함수 실행
result = fetch(conn=conn, query=query, sql_arg=sql_arg)
```
---
<br>

# ISSUE
## 이슈 1) %()s... 그럼 정수형은?

파이썬에서 `%s`는 문자형을 받을때 사용한다. 정수는 `%d`로 받는다.  
그럼 DB에서 컬럼이 int 타입일 때, Injection 대응코드도 `%(변수명)d` 혹은 `%d`로 되어야하는거 아닌가?

관련 문서를 찾지못하여 직접 확인한 결과 다음과 같이 알 수 있었다.

결과 : 문자형으로 넘겨주면 된다. 

1. DB에서 컬럼이 int 타입이라도 인자값은 문자형으로 만들어서 넘긴다.
   즉, Argument 부터 정수라면 문자열로 형변환해서 넘긴다.
2. 그것을 `%()s`로 받아주면 된다.

```python
test = 1 # 인자값

query = 'select * from testTable where key = %(str_test)s'
query_paramDict = { 'str_test' : str(test) }

cursor.execute(query, query_paramDict)
```

## 이슈 2) 테이블에 null값이 되도록 해야할 경우
update나 insert시 전달받은 인자값이 공백('')일 때,
테이블에는 부득이하게 null을 넣고자할 경우.  

```python
# 'test'라는 변수에 전달받은 인자값이 공백일 경우

query = 'select * from testTable where key = %(str_test)s'
query_paramDict = { 'str_test' : str(test) }

if test is '': query = query.replace('%(str_test)s', 'null')

cursor.execute(query, query_paramDict)
```
