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

## Diagonal하다 라는 것의 의미

 - 우선 행렬이 diagonal하다는 것은 그 행렬이 LT일 때 의미를 가진다.

 - 만약 $D$라는 행렬이 **Diagonal**하다는 것을 만족하려면

 - $D = [T]_\beta$로 나타낼 때, $T(V_j) = \lambda_j * V_j$가 된다.

 - 이는 즉, $V_j$라는 Input이 있었을 때, $D$라는 diagonal한 LT를 사용하면 이 LT의 Output은 $V_j$의 형식을 유지한 채 그냥 $\lambda_j$를 곱해주는 형식으로 도출된다는 것이다.

 - 역으로 말하자면, $T(V_j) = \lambda_j * V_j$라면, 그 LT는 Diagonalized 되어있다고 볼 수 있다.

 - 여기서 $\lambda_j$를 Eigen value, $V_j$를 Eigen vector라고 표현한다.

## 행렬에서의 Eigen vector

 - 위 예제와 거의 같다.

 - 그냥 $A*V = \lambda * V$일 때, $V$를 eigenvector of $A$라고 표현한다.

 - 대학교 과정에서는(공대 기준) 이렇게 행렬에 대한 Eigen vector만 다룬다.

 - 하지만 결국 행렬이라는 것은 공간과 공간 사이의 Linear operator이므로, 결국 그말이 그말이다. 단지 의미가 제한되게 대학교에서는 배웠고, 여기서는 좀 더 나아간 의미로써 eigen vector 및 eigen value를 학습하는 것이다.

## Thm 5.1

 - $T$의 Eigen vector로만 이루어진 ordered basis $\beta$를 만들 수 있다면 이 때 $T$는 diagonalized 되어있다고 할 수 있다.

 - 이는 eigenvectors로 $V$를 span하고, linearly independent할 때 이렇다고 생각할 수 있다!

## Ex 1.

 - A = $\\begin{pmatrix} 1 & 3 \\\ 4 & 2 \\end{pmatrix}$

 - $V_1 = \\begin{pmatrix} 1 \\\ -1 \\end{pmatrix}$

 - $V_2 = \\begin{pmatrix} 3 \\\ 4 \\end{pmatrix}$

 - $L_A * (V_1)  = \\begin{pmatrix} -2 \\\ 2 \\end{pmatrix}$

 - 이것은 -2 * $V_1$이므로 $\lambda_1 = -2$

 - 마찬가지로 계산하면 $\lambda_2 = 5$가 된다.

## Ex 2.

 - $T: $ = 90도 회전이동

 - 이 Linear operator는 Vector의 방향 자체를 바꿔버린다.

 - 즉, 위의 예제에 $V_1$에 해당하는 Vector가 보존되지 않기 때문에 Eigen vector도 Eigen value도 물론 없는 것이다.

## Thm 5.2.

 - $(A - \lambda * I) V = 0$ 이므로, $(A - \lambda I)$는 Invertible하면 안된다."(Det = 0) 왜냐하면 저게 Invertable하다는 것은 $V$가 Zero-vector가 되어버린다는 것이니까.

 - 또한 이렇게 Det=0인 Matrix를 Singular matrix라고 한다.

## Def.

 - Characteristic polynomial of $A = Det(A - t * I)$

 - 항상 eigen value, vector는 squared matrix에서 나온다.

 - nxn의 matrix일 경우 물론 degree는 n

 - $t^n$의 coefficient는 $(-1)^n$ (원소 - t 가 n번 곱해지니까)

## Finding eigen value

 - Characteristic polynomial을 사용하는 것으로 간단하게 Eigen value를 계산 가능하다.

 - $t$ 대신에 $\lambda$를 넣어서 계산하면 된다. 아래는 예제이다.

 A =$\\begin{pmatrix} 1 & 1 \\\ 4 & 1 \\end{pmatrix}$
 $det(A - \lambda * I) = \lambda^2 - 2 * \lambda - 3$
 
 - 따라서 $\lambda =  3 or -1$이 된다.

## Calculate the eigen vector

 - $v$를 Eigen vector, $\lambda$를 Eigen value라고 하였을 때

 - $T(v) = \lambda v$ 이므로

 - $(T - \lambda I)V = 0$

 - 이므로, $V$는 $(T-\lambda I)$의 Null space가 된다. 

## Ex 6.

 - $T = \\begin{pmatrix} 1 & 1 \\\ 4 & 1 \\end{pmatrix}$

 - $\lambda_1 = 3$, $\lambda_2 = -1$ 일 때

 - 우선 $\lambda_1$에 대한 Eigen vector를 구하면

 - $T-\lambda_1 I$는 $\\begin{pmatrix} -2 & 1 \\\ 4 & -2 \\end{pmatrix}$
 
 $\\begin{pmatrix} -2 & 1 \\\ 4 & -2 \\end{pmatrix} * \\begin{pmatrix} x1 \\\ x2 \\end{pmatrix} = \\begin{pmatrix} 0 \\\ 0 \\end{pmatrix}$

 이므로

 $\\begin{pmatrix} x1 \\\ x2 \\end{pmatrix} = t * \\begin{pmatrix} 1 \\\ 2 \\end{pmatrix}$

 가 된다.

 - 따라서 $\lambda_1$의 Eigen vector는 $\\begin{pmatrix} 1 \\\ 2 \\end{pmatrix}$가 된다.

 - $\lambda_2$의 경우에도 똑같이 구해주면 $\\begin{pmatrix} 1 \\\ -2 \\end{pmatrix}$가 된다.

 - 이 EigenVector를 Q의 Column으로 쓰면, $D = Q^-1 A Q$ = $\\begin{pmatrix} \lambda_1 & 0 \\\ 0 & \lambda_2 \\end{pmatrix}$ (이해 필요)

 - 이것을 행렬의 Eigen decomposition이라고 한다.

 - 이 행렬을 잘 보면, 이건 Diagonal matrix의 형태를 띄고있다.

 - 이는 임의의 원소 예를 들면 matrix[$y_1 \ y_2$]와 곱해졌다고 할 때

 - 이것의 Output은 coupling이 전혀 없다. (서로 독립적이다).

 - 따라서 Eigen-vector를 사용하면 임의의 시스템을 decoupling할 수 있다고 볼 수 있다.




 