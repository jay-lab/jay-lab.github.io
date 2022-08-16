---
layout: single
title: "[PYTHON] ARGPARSE"
categories: PYTHON
# tag: python
tag: [PYTHON]
toc: true
toc_label: "My Table of Contents" # toc 제목
toc_icon: "cog" # toc 아이콘(톱니바퀴)
author_profile: false
classes: wide
sidebar:
    nav: "docs"

---



유용한 argparse에 대하여 간단한 예제

```python
import argparse

# 인자값을 받을 수 있는 인스턴스 생성
parser = argparse.ArgumentParser(description='사용법 테스트입니다.')

# 입력받을 인자값 등록
parser.add_argument("-testArg", dest="testDest", help="date(YYYYMMDD) for daily crawl job")
# 입력받은 인자값을 args에 저장 (type: namespace)
args = parser.parse_args()

if args.testDest:
    print("결과값: ", args.testDest)
```

`» python argparseTest.py -testArg=123
결과값:  123`

