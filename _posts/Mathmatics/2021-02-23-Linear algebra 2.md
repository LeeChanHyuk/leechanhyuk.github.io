---
title: "[Math] Linear algebra 2"
date: 2021-03-03 11:30:06
author: Leechanhyuk
categories: Mathmatics
tags: Mathmatics Linear_argebra
use_math: true
cover: "/assets/instacode.png"
toc: true
---

**Linear algebra 2강**

**본 포스팅은 김영길 교수님의 유튜브 강의를 보고 개인적인 공부 목적으로 적은 것 임을 밝힙니다. 문제가 될 시 삭제하겠습니다.**

* * *

## 목차

1. Vector space
2. **Linear transformation and matrix**
3. System of linear equations
4. Determinant
5. Diagonalization
6. Inner product space

* * *

## 2.1 Linear transformation

 T: V -> W (여기서 V는 Domain(정의역), W는 Codomain(공역)) T는 Linear transformation을 뜻하고

 T가 V -> W의 Linear transformation이 되려면 

 T(x + y) = T(x) + T(y) (Additivity)
 T(cx)    = cT(x)       (homogeneity) 를 모든 x,y in v, c in F에서 성립해야한다. 두 조건을 만족시키는 것을 Super position principle이 성립한다. 고 표현한다.

 즉, addition 및 scalar multiplication을 preserve하는 것을 Linear map(=Linear transformation) 이라고 한다.

* * *

*Properties of Lienar map*

1. T(0) = 0 (Zero vector 의 output은 Zero-vector이다) (즉 해당되는 공간의 벡터는 항등원 및 역원이 존재해야하고, 그렇게 되면 반드시 원점을 지나게 되므로, 당연히 Zero-vector의 Output은 Zero-vector가 된다.)

즉, 아래 그림은 Linear하지 않다.

<img src="/assets/image/20210305/linearmap_ex.png" width="450px" height="300px" title="title" alt="title"> 

2. T(cx + y) = cT(x) + T(y)

3. T(x - y) = T(x) - T(y)

4. T($$sigma_(i=1 to n) aixi) = sigma_(i=1 to n) aiT(xi)) -> 즉 스칼라곱을 미리하고 Linear transformation을 하나, 나중에 하나 같다는 것이다.

* * *

Ex 1. T: R^2 -> R^2

(a1, a2) -> (2a1 + a2, a1)이 LT인가?

2번 성질을 이용해서 풀어봤을 때 성공적이므로 맞다.

* * *

Ex 2. T: R^2 -> R^2

<img src="/assets/image/20210305/linearmap_ex2.png" width="450px" height="300px" title="title" alt="title"> 

위 그림에서 벡터가 세타만큼 회전하는게 LT인지의 증명은 Additivity 및 homogeneity를 증명하면 된다.

* * *

*Reflection*

T: R^2 -> R^2 일 때 (a1, a2) -> (a1, -a2)는 LT인가?

역시 Additivity 및 homogeneity를 증명하면 LT라는 걸 알 수 있다.

* * *

*Projection*

<img src="/assets/image/20210305/linearmap_ex3.png" width="450px" height="300px" title="title" alt="title"> 

이렇게 벡터의 정사영을 내리는 것도 LT이다.

증명은, 어떤 두 벡터의 합의 정사영과 두 벡터 각각의 정사영의 합이 같으면 Additivity가 성립하고

마찬가지로, 한 벡터의 곱의 정사영과, 그 벡터의 정사영의 곱이 같으면 homogeneity가 성립한다는 것을 알 수 있다.

* * *

미분, 및 정적분도 LT이다. (부정적분은 상수 C가 튀어나와버리니까 LT가 아니게되어 버릴 수 있다.)

## Linear transformation some cases

1. Identity transformation
  
 - V -> V 공간에서 x -> x

2. Zero transformation

 - V -> W 공간에서 x -> 0

## Nullspace

 - V -> W LT할 때 W에서 0가 되는 V내 벡터들의 집합을 Nullspace라고 한다.

 - Identity transformation에서는 x -> x 여야 하므로, Nullspace가 되는 집합은 x=0으로만 구성된다.

 - Zero transformation에서는 x -> 0 이어야 하므로, Nullspace가 되는 집합은 V 내 속한 벡터 전체이다.

## image / preimage / Range

- image : X->Y로 LT했을 때 x->y가 된다고 하면, y가 image가 된다.

- pre-image : X->Y로 LT했을 때 x->y가 된다고 하면, x가 pre-image가 된다.

- Range : 치역의 부분집합을 의미한다. 즉 image들의 집합을 의미한다고 생각하면 될 듯.

## Ex 8.
 - I: V -> V 에서, x -> x일 때

 N(I) = {0}이고, R(I)는 그 자체가 된다.

## EX 9.
 - T0 : V -> V 에서, x -> 0 일 때

 N(T0) = V, R(T0) = {0}가 된다. 설명은 필요없겠지?

## Ex 9-1
 
 - T: $R^3 to R^2$

 $(a1, a2, a3) -> (a1 - a2, 2*a3)$ 일 때 Nullspace는 a1 = a2이고, a3=0 일 때의 집합을 의미하므로

 Span(1,1,0)이 T의 Null space가 된다.

여기서 Range는 전체 집합이 된다. 왜냐하면 a1, a2, a3의 조절로 모든 R^2를 만들어낼 수 있으므로

## Thm 2.2

 T: P2(R) -> M_2*2(R) 이 선형적일 때, 

 V의 basis의 집합이 b일 때, R(T) = span(T(B))를 증명.

 증명을 위해서는 서로 속한다는 걸 증명해야 함.

 Span(T(b)) in R(T) 증명

 basis의 image는 R(T) 내에 포함되어 있다. 이는 W의 Subspace이므로 그 부분집합의 (basis의) span 또한 반드시 포함해야하므로 증명된다.

 ## Ex. 10
 T: P_2(R) -> M_2*2(R) Linear 할 때, f(x) = a2*x^2 + a1*x + a0 -> [f(1) - f(2)      0         ] 
                                                                  [     0          f(0)       ]
 basis = {1, x, x^2} 일 때 P_2(R).

 T(f(X)) = T(1) = a2=0, a2=0 이라는 뜻.                                                              

## Nullity and Rank
 - Nullity = Nullspace의 dimension

 - Rank = Range의 dimension

