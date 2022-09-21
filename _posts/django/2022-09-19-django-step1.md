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



# - Django -

## Django 기본

virew.py파일 예제

```python
from rest_framework.views import APIView

class Main(APIView):
  def get(self, request):
    feed_list = Feed.objects.all()
    return render(request, 'instagram/main.html', context=dict(feeds=feed_list))
```

- rest_freamwork.views로 부터 import한 APIView를 Main 클래스의 파라미터로 사용

- 템플릿 HTML에 값을 전달하고자 할 때 `context`

- HTML에서는 feeds라는 dict 사용 가능

```html
/* 예시 */
{% for feed in feeds %}
	<p>{{feed.content}}</p>
	<p>{{feed.user_id}}</p>
	<p>{{feed.like_count}}</p>
	<img src="{{feed.img_path}}"/></img>
{% endfor %}
```



