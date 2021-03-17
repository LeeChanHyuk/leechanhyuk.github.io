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

또한 연산은 총 두개, 벡터끼리의 덧셈, 벡터와 스칼라의 곱셈이 존재한다.

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

또한 닫혀있어야 한다는 의미는 subspace 내부의 벡터들의 선형 결합으로 도출된 결과 값 역시 해당 공간에 포함되어야 한다는걸 의미한다.

따라서 해당 subspace에 속한 벡터의 span이 해당 subspace를 이룬다고 생각하면 될 듯 하다.

이렇게 생각을 했을 때는, 왜 subspace에 대한 설명이 선형결합 및 span보다 앞쪽에 있는지 알 순 없지만 아무튼..

결론적으로 R^3 공간의 Subspace를 나타내보면

- R^3 Space

- plane on the origin (원점을 지나는 면) - 닫혀 있기 때문에 반드시 원점을 지나겠지?

- line on the origin (원점을 지나는 선) - 선도 마찬가지로 닫혀있는 집합이기 때문에 당연히 원점을 지나겠지?

- Zero-Vector

가 된다.

또한 , Vector space가 Zero-vector 하나로만 이루어져 있을 때

Subspace는 

- Zero-vector

가 되므로 이 경우의 Vector space == Subspace가 된다.


* * *

Example 5)

M_m*n(R) (여기서 R은 행렬의 Entry가 전부 실수에서 왔다는 의미)

여기서 Entry들이 모두 양수이면 이건 Subspace가 되는가?

이 공간이 Subspace가 되려면 원소들이 모두 양수라도, 곱하는 수가 음수이면

연산의 결과가 음수가 나오기 때문에 닫혀있지 않아서 Subspace가 될 수 없다.

* * *

Thm 1.4

두 개의 Subspace의 교집합은 Subspace인가?

Subspace 내에 원소들은 덧셈 및 스칼라 곱에 닫혀있어서 이 집합 역시 Subspace이다. (조사 필요)

## Linear combination

Def : v = a1u1 + a2u2 + ... + anun 일 대 v는 u1, u2, ..., un의 Linear combination이다.

어떤 방정식을 여러 방정식의 Linear combination으로 나타내고, Linear combination의 계수를 찾기위해 행렬식을 푼다고 할 때

[1 3]         [2]
[-2 -5] [a] = [-2]
[-5 -4] [b]   [12]
[-3 -9]       [-6] 

는 어떻게 풀 수 있을까?

맨 먼저 생각이든건 Inverse matrix였는데, 여기서는 정방행렬이 아니므로 그는 사용할 수 없다.

또 determinant 역시 정방행렬이 아니어서 불가.

정답은 Gaussian elemination이다. (가우스 소거법)

푸는 방법은 제일 앞의 행렬을 Pivot으로 잡고, 각 행의 원소들을 서로 더해주거나 빼주어서

많은 원소들이 0이 되게 만들면, a 및 b를 구할 수 있다.

이 때 제일 아래부터 원소를 0으로 채워주는데 성공해서 계단형태가 되면 이것을 Echelon form이라고 부른다.

## Span

Span은 어떤 Vector space의 vector들의 모든 선형 결합 (Linear combination) 을 포함하는 집합이다.

예를 들어, Span{(1,0,0), (0,1,0)}은 모든 x, y집합을 표현할 수 있으므로 x-y plain 전체를 의미한다.

* * *

Thm 1.5

Span은 항상 subspace이다.

여기서 다시 복습하자면, Subspace는 항상 닫혀있어야한다.

어떤 집합이 있다고 가정했을 때, 

x = a1u1 + ... + anun
Y = b1u1 + ... + bnun
일 때, 이 두 식을 더하거나, 스칼라 곱을 하더라도

Span은 모든 선형 결합을 포함하는 집합이므로, 무한히 더하거나 뺄 수 있으므로 더하거나 곱하는 연산에 있어서 닫혀있다.

따라서 Span을 통해 형성된 공간은 그 벡터를 포함하는 공간의 Subspace가 된다.

## Linear dependence & independence

Vector u1, u2, ..., un이 있을 때

a1u1 + a2u2 + ... + anun = 0 일 때

적어도 하나 이상의 an이 0이 아니면

u1, u2 ... un은 Linearly dependent하다고 말한다.

즉, un까지의 벡터로 선형 결합 혹은 곱으로 다른 un까지의 벡터를 만들 수 있다면 이는 Linearly dependent 하다는 것이다.

만약 어떤 벡터 집합에서 Zero-vector가 포함되어 있다면

그 집합은 항상 Linearly-dependent하다. 왜냐하면 다른 벡터로 Zero-vector를 만들어낼 수 있으니까

따라서 어떠한 Subspace는 항상 Zero-vector를 포함하므로 항상 Linearly-dependent하다고 말할 수 있다.

Linear independence는 a1u1 + ... + anun = 0일 때, 이 식을 만족하는 a1, a2, ... , an이 모두 0일 때 u1, ... , un은 Linearly independent하다고 말 할수 있다.

직관적으로도 알 수 있겠지만, 집합이 커지면 커질수록 집합의 원소가 서로 Dependent할 확률은 높아진다.

* * *

Property

- 공집합은 Linearly-independent

- 원소가 하나인 집합이 있을 때 그게 Zero-vector가 아니면 그 집합은 Linearly-independent하다.

* * *

## Linear dependence 까지의 정리

Vector space는 해당 공간 내에서 닫혀있어야 한다.

Span은 span 하고자 하는 원소(들)의 선형 결합으로 이루어진 공간을 뜻한다.

이렇게 생성된 공간은 반드시 그 역원 및 항등원을 포함하므로 원점을 지날 수 밖에 없다.

예를 들어, 한 벡터가 있다고 치면 그 벡터의 span은 그 벡터의 진행 방향 및 그 역방향을 포함하는 선이 되겠지

또, Independent한 두 벡터가 있다고 치면, 그 두 벡터의 span은 두 벡터를 감싸는 평면이 되겠지 (물론 원점도 반드시 포함)

또 어떠한 원소를 포함하는 Vector space가 있을 대, 그 원소(들)의 span은 원래 Vector space의 subspace가 되겠지.

마지막으로, Independent한 세 개의 백터가 있다고 치면, 그 때 부터는 3차원 공간에 있는 모든 좌표를 나타낼 수 있는 것이다.

## Basis

Basis는 V의 Linearly independent한 Subset인 동시에 Span했을 때 V를 Generate하는 벡터이다.

Zero-vector는 basis가 될 수 없는데, 이는 당연히 span했을 때 V를 Generate할 수 없을 뿐더러

Zero-vector를 Basis로 삼는 순간, 그 집합은 바로 Linearly dependent해지기 때문에 Basis가 어떠한 경우에도 될 수 없다.

* * *

Ex. 6) S = {(2, -3, 5), ... (7,2,0)}이 있을 때 얘들이 R^3을 Generate 할 수 있는가?

-> 가감법 하다보면 Independent한 Vector가 3개 있다. 따라서 R^3을 Generate 할 수 있다.

* * *

## Dimension

Dimension 은 Basis라는 집합안에 들어있는 원소들의 개수이다.

즉, Basis 들로 Span 했을 때 나오는 차원의 수를 뜻한다고 보여진다.

V = {0} -> dim(V) = 0
V = F^4 -> dim(V) = 4
V = M_2x3(F) -> dim(V) = 6

마지막 예제가 이해가 잘 안갈 수도 있는데, 이는 이 행렬이 표현 가능한 자리의 수를 대략적으로 따져보면 이해가 쉽다.
[1,0,0    [0,1,0   [0,0,1    [0,0,0    [0,0,0    [0,0,0
 0,0,0] ,  0,0,0] , 0,0,0] ,  1,0,0] ,  0,1,0] ,  0,0,1] 과 같이 표현 할 수 있는 자리의 수가 6개지?

 각 행렬은 서로서로 Independent 하기 때문에, 6차원을 표현할 수 있는 것이다.

 만약에 basis가 P_n(F) = {1,x,...,x^n} 일 때 몇차원이 될까?

 basis들은 서로 Independent하므로 n+1 차원이 된다.

 그럼 Field = Complex number , V = Complex number 일 때 차원은 ?

 dim(V) = 1, basis = {1}이 된다. 왜냐하면 Field가 복소수 차원이므로, 기저인 1에다가 Field에 속하는 스칼라값을 곱해주면 되기 때문

 (이게 햇갈린다면 벡터 공간이 스칼라 * 벡터 및 벡터 + 벡터로 구성된다는 사실을 떠올려보기)

 만약 Field = Real number, V = Complex number일 때는?

div(V) = 2, basis = {1, i}가 된다. 왜냐하면 Field가 실수 차원이므로, 벡터 공간의 모든 수를 표현하기 위해서는 기저로 i를 가지고 있어야 하기 때문

* * *

Ex 17. W = {(a1, a2, ..., a5)} |a1 + a3+ a5 = 0, a2 = a4| 일 때 차원은 3개이다. 왜냐하면 Constraint가 2개이니까 (이해 필요)

Free variable과 dimension간의 정리 필요.