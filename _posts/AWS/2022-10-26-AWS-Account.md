---
layout: single
title: "[AWS] AWS Account"
categories: [AWS]
tag: [BASIC]
toc: true
toc_label: "Contents" # toc 제목
toc_icon: "cog" # toc 아이콘(톱니바퀴)
author_profile: false
sidebar:
    nav: "docs"

---



**AWS 계정 만들기 및 첫 설정** 
{: .notice--info}



- 처음 생성할 때 본인 명의의 신용카드 필요
- AWS 계정을 처음 생성하면 루트 유저와 기본 리소스(기본 VPC)등이 생성됨
- AWS 계정 아이디가 부여됨(숫자)
  - 추후 AWS 계정에 별명 지정 가능(문자)



**루트유저**

- 생성한 계정의 모든 권한을 자동으로 가지고 있음
- 생성시 만든 이메일 주소로 로그인
- 탈취당했을 때 복구가 매우 힘듬. 사용을 자제하고 MFA 설정 필요
- 루트유저는 고나리용으로만 이용: 계정 설정 변경, 빌링 등
- AWS API 호출 불가(AccessKey/Secret AccessKey 부여 불가)

**IAM User**

- IAM을 통해 생성한 유저
- 만들 때 주어진 아이디로 로그인(※이메일이 아니라 아이디)
- 기본 권한 없음 : 권한 부여 필요
  - 예 : 관리자용 개발자용 디자이너용 회계용 등등 각각의 IAM User권한 분류 필요

- 꼭 사람이 아닌 어플리케이션 등의 가상의 주체를 대표할 수도 있음
- AWS API 호출 가능
  - AccessKey : 아이디 개념
  - Secret Access Key : 패스워드 개념
- AWS의 관리를 제외한 모든 작업은 관리용 IAM User를 만들어 사용
- 권한 부여시 루트 유저와 같이 모든 권한을 가질 수 있지만 빌링 관련 권한은 루트유저가 부여해주어야 함

