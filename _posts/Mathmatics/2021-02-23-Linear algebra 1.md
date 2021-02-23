---
title: "[Math] Linear algebra 1"
date: 2021-02-23 15:13:00
author: Leechanhyuk
categories: Mathmatics
tags: Mathmatics Linear_argebra
cover: "/assets/instacode.png"
toc: true
---

**Linear algebra 1강**

**본 포스팅은 김영길 교수님의 유튜브 강의를 보고 개인적인 공부 목적으로 적은 것 임을 밝힙니다. 문제가 될 시 삭제하겠습니다.**

===

## 목차

1. **Vector space**
2. Linear transformation and matrix
3. System of linear equations
4. Determinant
5. Diagonalization
6. Inner product space

===

**Algebra**

> 의미 : 대수학 / 목적 : 방정식을 어떻게 풀건가

방정식을 풀려면 어떻게 해야하나?

기본적으로는 덧셈 및 곱셈에 대한 항등원이나 역원이 존재해야 소거법으로 풀이가 가능하다.

또한 교환 법칙 및 결합 법칙이 성립해야하며

0이 존재해야 한다.

**Vector**

> 크기와 방향이 있는 것. [1 2 3 4] 따위로 표기.

Vector space의 정의를 위해선 우선 Scalar들의 공간인 Field의 정의가 선제되어야 한다.

**Field**

덧셈과 곱셈, 두 개의 연산을 가지고 있음.

스칼라 끼리의 두 연산에 대해서 닫혀 있어야 Field (연산을 진행하더라도 Field내에 속해야 한다는 의미)

따라서 교환 법칙 (Commutative), 결합 법칙 (Associative)을 만족해야하며

항등원(Identity element) - (Additive identity, Multiplicative identity)를 보유해야하며

0 외에 다른 수에 대해서는 역원 (곱해서 1이 되는 수) 을 가져야 한다.

상단의 성질을 모두 만족시키는 집합을 **Field**라고 한다.

{a + b(root)2 | a, b (- Q} 일 때 이 집합 역시 하나의 Field가 된다.

반대로, 자연수나 정수같은 경우, 곱셈에 대한 역원이 존재할 수 없기 때문에, 이는 Field가 아니다.

**Vector space**

벡터 Space는 Field를 포함한다.

또한 연산은 총 두개, 벡터끼리의 덧셈, 벡터와 스칼라의 곱셈이 존재한다.

Zero_Vector가 있어야한다.

Ex ) Vector F가 있을 때, Vector space는 F^n으로 나타낸다. 이는 F를 여러번 곱한게 아니라, n은 차원을 나타내는 수이고, 2차원이면 F^2, 3차원이면 F^3이 되겠지.

이 때 F^n은 교환 법칙 및 결합 법칙이 모두 성립한다.

  



























