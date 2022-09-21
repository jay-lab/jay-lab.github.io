---
layout: single
title: "스프링과 장고에 대한 생각 정리"
categories: [PYTHON]
tag: [SPRING, DJANGO]
toc: true
toc_label: "Contents" # toc 제목
toc_icon: "cog" # toc 아이콘(톱니바퀴)
author_profile: false
sidebar:
    nav: "docs"
---



# Spring과 Django를 놓고 

나의 프로그래밍 시작은 자바와 스프링프레임워크였다. 개발자 시작부터 약 2년 가량 금융권 SI업무를 하는 동안, 스프링 외에는 다른 프레임워크를 해본 적이 없었다. 지금의 회사에서 파이썬과 장고를 접하게 되었고 업무량에 의해 아직까지도 깊게 이해하지는 못했지만 나름 '맛보기' 정도는 했다 생각한다.

지금 이 글을 적게된 이유도 바로 이것 때문이다. 스프링도, 장고도 단지 '맛보기' 만 했기에 스스로 떳떳하지 못하고 신규 기능을 추가하거나 새로운 프로젝트를 해야할 때 자신이 없었다. 그래서 슬슬 업무가 익숙해지는 이 시기에 내 앞날을 위해 하나만 정해서 '깊이'를 만들어 보고싶다는 생각을 했고, 스프링과 장고 둘 중 어느 것을 우선적으로 해야할지 비교해보는 시간을 가져보았다.

물론 나 스스로 '둘 다' 잘할 수 있으면 좋겠다. 그러나 현실적인 상황을 고려할 때 우선순위를 정하고 '선택과 집중'이 필요했다.

그래서 어느 것에 집중해보기로 했는지 결론부터 말하자면 '장고'부터 파보기로 결정했다.  
이유는 다음과 같다.



## 현재 맡고있는 업무

내가 언제 우연찮게 이직을 하게 될지 모르겠지만, 우리나라 대부분의 개발직군은 스프링을 사용한다는 점에서 더욱 더 스프링프레임워크를 준비해 놓아야하는게 아닐까 하는 불안감은 지워지지 않는다.

그러나 현재 위치에서 배울 수 있는 것을 외면한채 미래만 보고 준비하는게 과연 현명한 것일까 하는 의문이 위의 고민과 대치되었다.

## 장고의 미래

장고는 인터프리터 언어다. 이렇다보니 컴파일 언어에 비해 느릴수 밖에 없다는 점은 항상 빠지지않는 단점으로 꼽혔다. 내가 생각해도 '빠르다'라는 것은 굉장한 장점이고, 장고가 타 프레임워크에 비해 느리다는 점은 안타까웠다. 이러한 점에서 내 생각이긴 하지만, 이를 극복할 수 있는 몇가지 방안을 다음과 같이 꼽아보았다.

1. **지속적으로 업데이트 될 파이썬**
   현재 기준, 베타 버전인 파이썬 3.11은 이전보다 평균 25% 더 빠른 속도(최대 60%)를 가진다고 한다. 물론 그래봤자 인터프리터 방식이 컴파일 방식보다 빠를 순 없지만, 지금도 지속되고있는 하드웨어의 발전에 힘입어 그 속도의 간격은 더욱이 체감할 수 없는 수준으로 좁혀지지 않을까싶다.

2. **PyPy + JIT 컴파일(Just In Time)**

   JIT 컴파일 방식은 인터프리트 방식과 정적컴파일 방식을 적절하게 혼합한 방식이다. 실행 시점에 기계어코드를 생성(전통적인 인터프리트 방식)하면서 그 코드를 캐싱하여 같은 함수가 여러 번 불릴 때, 해당 캐시를 활용함으로써 정적컴파일 방식의 이점을 얻을 수 있다.
   기존의 파이썬 인터프리터는 정확하게는 C언어로 구현되어 CPython이라고 하는데, PyPy는 JIT컴파일 방식을 제공하는, 파이썬(RPython)으로 만들어진 파이썬 인터프리터다. 실행 내용마다 차이가 있겠지만 한 블로거의 실험내용에서 C언어의 경우 0.4초, python의 경우 15초 걸리던 것이 pypy로 0.8초까지 단축된 결과를 보았다. 아직까지는 특정 프로그램에서 이러한 효과가 반감되거나 아예없는 한계가 있다는 단점이 있지만, 이 또한 지금도 계속해서 업데이트 되는 중이라는 점은 파이썬 + Django 조합을 낙관적으로 바라볼만 한 요소로 다가왔다.  
   
   > [출처 1]<https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=lsm_origin&logNo=120210926900>
   >
   > [출처 2]<https://speed.pypy.org/>
   >
   > [출처 3]<https://lincolnloop.com/blog/faster-django-sites-pypy/>
   >
   > [출처 4]<https://seolin.tistory.com/119>
   
   

## 나의 이상

사실 여기까지만 보면 Spring Boot도 개발속도, 실행 속도 딱히 빠짐이 없는데 그럼에도 불구하고 장고의 낙관적인 면만 나열하였다. "여기저기 겨우겨우 장점 영끌해서 겨우 스프링 부트랑 같거나 그 이하"인 것 같지만, 오히려 반대로 개발속도와 실행속도가 큰차이가 나지 않는다면, AWS와의 조합을 고려하면 파이썬을 빼놓을 수 없고 파이썬의 막강한 확장성을 감안한다면 장고를 깊게 알아가보는것도 좋은 선택이라고 생각한다. 많은 회사에서 스프링을 사용하고있는 만큼 추후 스프링도 본격적으로 해볼 생각이지만 일단은 장고로 가벼운 웹과 api를 구현할 수 있는 수준까지는 끌어올려 보려한다.

> "파이썬은 AWS, Google Cloud, Azure등에서 전폭적으로 초기 단계부터 지원한 프레임워크"
>출처 : <https://elky.tistory.com/652>

   