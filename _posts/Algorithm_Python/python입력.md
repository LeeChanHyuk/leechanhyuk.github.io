---
title: "[Algorithm] 파이썬 입력"
date: 2021-11-16 22:10:00
author: Leechanhyuk
categories: Algorithm
tags: Algorithm baekjoon
cover: "/assets/instacode.png"
toc: true
---

# 시작하기전에

  - 머리속 알고리즘에 관한 지식 정리 목적으로 간단하게 적어보는 글입니다. 자세한 정보는 다른 포스트를 참조하시는게 좋을 수 있습니다.

# 입력 받는 방법

  ```python
  import sys
  
  # input은 입력을 받을 수 있으나 느림
  input1 = input()

  # sys.stdin.readline()이 input보다 훨씬 빠름. 사용 필수
  input2 = sys.stdin.readline()

  # input2에는 개행 문자 '\n'가 포함되어 있어서 삭제 필요
  input2 = input2.strip()

  # input2는 숫자 입력이라, 입력을 숫자 자료형 (int)로 변형시켜주고 싶음.
  input2_map = map(int, input2)

  # input2에 있는 숫자 객체들을 차례로 출력하고 싶음
  for i in range(len(list(input2))):
      print(input2.__next__())

  # list 형태로 간편하게 관리하고 싶다면
  input2_list = list(input2_map)

  ```