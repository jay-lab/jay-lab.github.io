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



파이썬 스크립트 실행시 사용자 임의의 Argument값을 넘겨서 활용할 수 있는 기본적인 방법을 알아본다.



# argparse



**유용한 argparse에 대하여 간단한 예제**

```python
import argparse

# 인자값을 받을 수 있는 인스턴스 생성
parser = argparse.ArgumentParser(description='사용법 테스트입니다.')

# 입력받을 인자값 등록
parser.add_argument("-testArg", dest="testDest", help="python argparse.py -testArg={입력값} 형태로 입력하세요~")

# 입력받은 인자값을 args에 저장 (type: namespace)
args = parser.parse_args()

if args.testDest:
    print("결과값: ", args.testDest)
```

**결과값 :**

```shell
» python argparseTest.py --help
usage: argparseTest.py [-h] [-testArg TESTDEST]

사용법 테스트입니다.

optional arguments:
  -h, --help         show this help message and exit
  -testArg TESTDEST  python argparse.py -testArg={입력값} 형태로 입력하세요~
```

```shell
» python argparseTest.py -testArg=123
결과값:  123`
```







