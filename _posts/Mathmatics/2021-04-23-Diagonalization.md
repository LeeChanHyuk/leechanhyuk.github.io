---
title: "[Linear algebra] Diagonalization"
date: 2021-04-23 11:30:06
author: Leechanhyuk
categories: Mathmatics
tags: Mathmatics Linear_argebra
use_math: true
cover: "/assets/instacode.png"
toc: true
---

**Linear algebra 5강**

**본 포스팅은 김영길 교수님의 유튜브 강의를 보고 개인적인 공부 목적으로 적은 것 임을 밝힙니다. 문제가 될 시 삭제하겠습니다.**

* * *

## 목차

1. Vector space
2. Linear transformation and matrix
3. System of linear equations
4. Determinant
5. **Diagonalization**
6. Inner product space

* * *

## Diagonalization

 - 행렬을 어떻게 대각화 할꺼냐?!

 - LT인 T가 있을 때, 이것의 Ordered basis $\beta$를 찾을 수 있는가?

 - 여기서 $\beta$는 diagonal matrix (Pivot variable외에는 모두 0인 행렬)

 - 이것을 행렬로 표현하면

 - $\beta = (v1, v2, ... , vn)$일 때

 - LT인 T를 $\beta$를 통해 행렬로 표현하면 (=$[T]_\beta$) (=Linear transformation)

   $\\begin{pmatrix} \lambda_1 & 0 & 0 & 0 \\\ 0 & \lambda_2 & 0 & 0 \\\ 0 & 0 & \lambda_3 & 0 \\\ 0 & 0 & 0 & \lambda_4 \\end{pmatrix}$

 - 위 행렬은 물론.Diagonal matrix이므로, Pivot variable 빼고는 모두 0이다.

 - 이것은 즉, $v_j$라는 Input이 있고, T라는 LT가 있을 때, 이것의 OUTPUT은 $0V_1 + 0V_2 + ... + \lambda_j v_j + .. + 0v_n$으로 표현할 수 있고 이것은 $\lambda_j v_j$이다.

 - 왜냐하면 Diagonal matrix이므로, 한 열이는 하나의 원소만 0이 아니고, 나머지는 모두 0이므로.

 - 여기서의 $V_j$를 Eigen vector, $\lambda_j$를 Eigen value라고 한다.

 - 이 Eigen vector는 T를 아주 잘 나타내주고 있다. 즉, Eigen vector를 통해 T를 표현하는 것이 가능하다고 생각하면 될 듯 하다.

## Eigen values and Eigen vector

 - Definition. $T:V \to V$ 인 LT가 있을 때

 - T가 대각화 가능하려면, Ordered basis $\beta$가 diagonal이어야 한다.

 - 이 대각화는 항상 가능한 것이 아니다.  


 