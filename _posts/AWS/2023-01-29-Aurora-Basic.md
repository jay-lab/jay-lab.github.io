---
layout: single
title: "[AWS] Aurara DB Basic"
categories: [AWS]
tag: [RDS. TIL]
#toc: true
#toc_label: "Contents" # toc 제목
#toc_icon: "cog" # toc 아이콘(톱니바퀴)
author_profile: false
published: false
sidebar:
    nav: "docs"
---



**RDS Basic**
{: .notice--info}



- 오픈소스 데이터의 간편성, 비용효율성을 결합
- MySQL, PosstgreSQL 호환 관계형 데이터베이스
- 최대 MySQL의 5배 빠름
- 최대 PosstgreSQL의 3배 빠름
- 1/10비용으로 상용 DB의 보안, 가용성 및 안정성 제공
- 하드웨어 프로비저닝, DB설정, 패치 및 백업과 같은 시간 소모적인 관리 작업을 자동화하는 AWS RDS 에서 모든것을 관리할 수 있음



![AWS 강좌 Amazon Aurora아마존 오로라(0)](../../images/2023-01-29-Aurora-Basic/AWS%20%E1%84%80%E1%85%A1%E1%86%BC%E1%84%8C%E1%85%AA%20Amazon%20Aurora%E1%84%8B%E1%85%A1%E1%84%86%E1%85%A1%E1%84%8C%E1%85%A9%E1%86%AB%20%E1%84%8B%E1%85%A9%E1%84%85%E1%85%A9%E1%84%85%E1%85%A1(0).jpg)

일반적으로 RDS는 하나의 인스턴스와 하나의 EBS 조합으로 구성되어있음

하나의 AZ에 인스턴스, EBS가 쌍으로 존재

이러한 점에서 나오는 단점을 해결하기 위해 오로라는 아키텍쳐단에서 많은 변화가 생김



- Single-Master모드 : writer 인스턴스 노드가 하나
  일반적으로 이러한 싱글마스터 모드가 사용됨, 멀티마스터모드의 경우 장점이 있지만 사용할 수 없는 기능들도 몇가지 있음

![AWS 강좌 Amazon Aurora아마존 오로라(1)](../../images/2023-01-29-Aurora-Basic/AWS%20%E1%84%80%E1%85%A1%E1%86%BC%E1%84%8C%E1%85%AA%20Amazon%20Aurora%E1%84%8B%E1%85%A1%E1%84%86%E1%85%A1%E1%84%8C%E1%85%A9%E1%86%AB%20%E1%84%8B%E1%85%A9%E1%84%85%E1%85%A9%E1%84%85%E1%85%A1(1).jpg)

- Multi-Master모드 : writer 인스턴스 노드가 총 4개까지 가능
  읽기노드 여러개를 통해 부하분산 가능

![AWS 강좌 Amazon Aurora아마존 오로라(2)](../../images/2023-01-29-Aurora-Basic/AWS%20%E1%84%80%E1%85%A1%E1%86%BC%E1%84%8C%E1%85%AA%20Amazon%20Aurora%E1%84%8B%E1%85%A1%E1%84%86%E1%85%A1%E1%84%8C%E1%85%A9%E1%86%AB%20%E1%84%8B%E1%85%A9%E1%84%85%E1%85%A9%E1%84%85%E1%85%A1(2).jpg)



- 특징

![AWS 강좌 Amazon Aurora아마존 오로라(3)](../../images/2023-01-29-Aurora-Basic/AWS%20%E1%84%80%E1%85%A1%E1%86%BC%E1%84%8C%E1%85%AA%20Amazon%20Aurora%E1%84%8B%E1%85%A1%E1%84%86%E1%85%A1%E1%84%8C%E1%85%A9%E1%86%AB%20%E1%84%8B%E1%85%A9%E1%84%85%E1%85%A9%E1%84%85%E1%85%A1(3).jpg)

![AWS 강좌 Amazon Aurora아마존 오로라(4)](../../images/2023-01-29-Aurora-Basic/AWS%20%E1%84%80%E1%85%A1%E1%86%BC%E1%84%8C%E1%85%AA%20Amazon%20Aurora%E1%84%8B%E1%85%A1%E1%84%86%E1%85%A1%E1%84%8C%E1%85%A9%E1%86%AB%20%E1%84%8B%E1%85%A9%E1%84%85%E1%85%A9%E1%84%85%E1%85%A1(4).jpg)

![AWS 강좌 Amazon Aurora아마존 오로라(5)](../../images/2023-01-29-Aurora-Basic/AWS%20%E1%84%80%E1%85%A1%E1%86%BC%E1%84%8C%E1%85%AA%20Amazon%20Aurora%E1%84%8B%E1%85%A1%E1%84%86%E1%85%A1%E1%84%8C%E1%85%A9%E1%86%AB%20%E1%84%8B%E1%85%A9%E1%84%85%E1%85%A9%E1%84%85%E1%85%A1(5).jpg)

"1초 전까지의 데이터는 보장", "1분 안에 복구 가능"

![AWS 강좌 Amazon Aurora아마존 오로라(6)](../../images/2023-01-29-Aurora-Basic/AWS%20%E1%84%80%E1%85%A1%E1%86%BC%E1%84%8C%E1%85%AA%20Amazon%20Aurora%E1%84%8B%E1%85%A1%E1%84%86%E1%85%A1%E1%84%8C%E1%85%A9%E1%86%AB%20%E1%84%8B%E1%85%A9%E1%84%85%E1%85%A9%E1%84%85%E1%85%A1(6).jpg)

![AWS 강좌 Amazon Aurora아마존 오로라(7)](../../images/2023-01-29-Aurora-Basic/AWS%20%E1%84%80%E1%85%A1%E1%86%BC%E1%84%8C%E1%85%AA%20Amazon%20Aurora%E1%84%8B%E1%85%A1%E1%84%86%E1%85%A1%E1%84%8C%E1%85%A9%E1%86%AB%20%E1%84%8B%E1%85%A9%E1%84%85%E1%85%A9%E1%84%85%E1%85%A1(7).jpg)

![AWS 강좌 Amazon Aurora아마존 오로라(8)](../../images/2023-01-29-Aurora-Basic/AWS%20%E1%84%80%E1%85%A1%E1%86%BC%E1%84%8C%E1%85%AA%20Amazon%20Aurora%E1%84%8B%E1%85%A1%E1%84%86%E1%85%A1%E1%84%8C%E1%85%A9%E1%86%AB%20%E1%84%8B%E1%85%A9%E1%84%85%E1%85%A9%E1%84%85%E1%85%A1(8).jpg)



### ❤️Backtrack

스냅샷, 자동백업을통해 복수했을때에는 새로운 RDS 인스턴스를 만들었어야 했다.

반면, 오로라의 Backtrack은 기존의 DB를 특정 시점으로 되돌리는 것으로써 where절 없는 Delete문으로인해 실수로 데이터 전체가 날아가버렸을 경우, 새로운 DB를 생성하는 것보다 훨씬 빠르게 복구를 할 수 있다.

앞 뒤로 시점을 이동할 수 있기 때문에 원하는 지점을 빠르게 찾을 수 있다.
"이 시점에 이 스냅샷이 적절할까?" 라는 고민을 획기적으로 줄여줌

![AWS 강좌 Amazon Aurora아마존 오로라(9)](../../images/2023-01-29-Aurora-Basic/AWS%20%E1%84%80%E1%85%A1%E1%86%BC%E1%84%8C%E1%85%AA%20Amazon%20Aurora%E1%84%8B%E1%85%A1%E1%84%86%E1%85%A1%E1%84%8C%E1%85%A9%E1%86%AB%20%E1%84%8B%E1%85%A9%E1%84%85%E1%85%A9%E1%84%85%E1%85%A1(9).jpg)

![AWS 강좌 Amazon Aurora아마존 오로라(10)](../../images/2023-01-29-Aurora-Basic/AWS%20%E1%84%80%E1%85%A1%E1%86%BC%E1%84%8C%E1%85%AA%20Amazon%20Aurora%E1%84%8B%E1%85%A1%E1%84%86%E1%85%A1%E1%84%8C%E1%85%A9%E1%86%AB%20%E1%84%8B%E1%85%A9%E1%84%85%E1%85%A9%E1%84%85%E1%85%A1(10).jpg)





- Multi-Master

![AWS 강좌 Amazon Aurora아마존 오로라(11)](../../images/2023-01-29-Aurora-Basic/AWS%20%E1%84%80%E1%85%A1%E1%86%BC%E1%84%8C%E1%85%AA%20Amazon%20Aurora%E1%84%8B%E1%85%A1%E1%84%86%E1%85%A1%E1%84%8C%E1%85%A9%E1%86%AB%20%E1%84%8B%E1%85%A9%E1%84%85%E1%85%A9%E1%84%85%E1%85%A1(11).jpg)