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

This formula $f(x) = x^2$ is an example.
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


