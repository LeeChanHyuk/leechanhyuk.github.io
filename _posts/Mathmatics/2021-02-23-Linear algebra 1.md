---
title: "[Math] Linear algebra 1"
date: 2021-02-23 15:13:00
author: Leechanhyuk
categories: Mathmatics
tags: Mathmatics Linear_argebra
use_math: true
cover: "/assets/instacode.png"
toc: true
---

**Linear algebra 1강**

**본 포스팅은 김영길 교수님의 유튜브 강의를 보고 개인적인 공부 목적으로 적은 것 임을 밝힙니다. 문제가 될 시 삭제하겠습니다.**

* * *

## 목차

1. **Vector space**
2. Linear transformation and matrix
3. System of linear equations
4. Determinant
5. Diagonalization
6. Inner product space

* * *

## Algebra

> 의미 : 대수학 / 목적 : 방정식을 어떻게 풀건가

방정식을 풀려면 어떻게 해야하나?

기본적으로는 덧셈 및 곱셈에 대한 항등원이나 역원이 존재해야 소거법으로 풀이가 가능하다.

또한 교환 법칙 및 결합 법칙이 성립해야하며

0이 존재해야 한다.

## Vector

> 크기와 방향이 있는 것. [1 2 3 4] 따위로 표기.

Vector space의 정의를 위해선 우선 Scalar들의 공간인 Field의 정의가 선제되어야 한다.

## Field

덧셈과 곱셈, 두 개의 연산을 가지고 있음.

스칼라 끼리의 두 연산에 대해서 닫혀 있어야 Field (연산을 진행하더라도 Field내에 속해야 한다는 의미)

따라서 교환 법칙 (Commutative), 결합 법칙 (Associative)을 만족해야하며

항등원(Identity element) - (Additive identity, Multiplicative identity)를 보유해야하며

0 외에 다른 수에 대해서는 역원 (곱해서 1이 되는 수) 을 가져야 한다.

상단의 성질을 모두 만족시키는 집합을 **Field** 라고 한다.

$$a + b\sqrt{2} | a, b \in Q$$ 일 때 이 집합 역시 하나의 Field가 된다.

반대로, 자연수나 정수같은 경우, 곱셈에 대한 역원이 존재할 수 없기 때문에, 이는 Field가 아니다.

## Vector space

벡터 Space는 Field를 포함한다.

또한 연산은 총 두개, 벡터끼리의 덧셈, 벡터와 스칼라의 곱셈이 존재한다. (덧셈이 존재하고 Function이 존재하는데 어떻게 뺄셈이 없지?)

Zero_Vector가 있어야한다.

Ex ) Vector F가 있을 때, Vector space는 F^n으로 나타낸다. 이는 F를 여러번 곱한게 아니라, n은 차원을 나타내는 수이고, 2차원이면 F^2, 3차원이면 F^3이 되겠지.

이 때 F^n은 교환 법칙 및 결합 법칙이 모두 성립한다.

Vector space는 항상 Zero-Vector를 포함해야 한다.

따라서 이는 Vector space에 속한 하나의 Vector는 덧셈 및 곱셈의 역원이 있어야 한다는 말과도 같다.

(역원이 있어야 Zero vector를 이끌어낼 수 있으므로)

강의에서는 Vector space를 유한 차수의 Polynomial 집합으로 규정하고 있다.

이는 Vector 자체는 방향과 크기를 가지는 것 이지만, 이 Vector가 어떠한 공간을 나타내기 위해서는

다항식의 결합을 통해서 수학적 식으로 표현된다는 것으로 이해했다.

* * *

Example 6 ) $$S = {(a1,a2) | a1,a2 \in R }$$ 일 때

(a1, a2) + (b1, b2) = (a1+b1, a2-b2)로 정의하고

c(a1, a2) = (ca1, ca2)로 정의했을 때 이 공간은 Vector space가 맞는가?

여기서는 Vector에 대한 교환 법칙이 성립되지 않기 때문에

이 공간은 Vector space가 아니다.(5-3 != 3-5)

* * *

Example 7 ) $$S = {(a1, a2 | a1, a2 \in R)}$$ 일 때

(a1, a2) + (b1, b2) = (a1 + b1, 0) 이고

c(a1, a2) = (ca1, 0) 일 때 이는 Vector space가 맞는가?

여기서는 a2 + b2가 0이 되므로, a2=1 b2=0일 때 1+0=0이 되므로 0이 덧셈에 대한 항등원이 되지 않으므로 Vector space가 아니고

ca2 = 0이므로, c=1, a2=2일 때, 1*2=0일 때도 역시 1이 곱셈에 대한 항등원이 되지 못하므로 Vector space가 아니다.

* * *

위의 두 예제를 통해서는 같은 Vector이 더라도, Function에 따라서 Vector space를 구성하지 못할 수 도 있다는 것을 나타낸다.

위 예제 및 설명을 통해서 이미 알 수 있었겠지만, Vector space에서는 덧셈 및 곱셈에 대한 항등원 및 역원이 존재해야하고

교환법칙 ,결합법칙 및 분배법칙이 작용해야 하며, 추가적인 성질은

1. Zero-Vector는 Vector-space내에서 Unique해야하고

2. 특정 수의 역원 역시 Unique 해야한다.

* * *

## Subspace

Subspace라는 건 Subset + Vector space이다.

즉, Vector space에 속하는 집합인데, 그 집합 내부적으로도 Vector space의 조건들을 만족시키는 집합을 의미한다. (즉, 닫혀있으면 된다)

하지만, 교환 법칙 및 결합 법칙 등은 당연히 존재한다. 왜냐하면 이미 Vector space내에 있었으니까

예를 들자면, Vector space가 Zero-vector 하나로만 이루어져 있을 때

여기서는 Vector space = subspace가 된다.

여기에서 반례는 오직 덧셈이나 곱셈을 했을 때, 규정된 공간보다 더 밖에 있는 것이 될 경우밖에 존재하지 않는다.

여기서 드는 의문점은, 해당 Vector space가 Vector의 Polynomial으로 구성되어 있다고 할 때

그러면 덧셈을 무한히 하는 Polynomial이라고 가정을 하면, 언젠가는 당연히 해당 Vector space를 초과하지 않나?

정리하자면, 애초부터 Vector space가 R^3과 같이 특정 범위가 아니라 차원 단위일 때는 가능하지만

그게 아니라면, 아.. 애초부터 차원단위가 될 수 밖에 없겠구나 Zero-vector로 구성된 Vector space가 아니면.



