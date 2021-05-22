---
title: "[Linear algebra] Eigen Value and Eigen Vector"
date: 2021-05-22 11:30:06
author: Leechanhyuk
categories: Mathmatics
tags: Mathmatics Linear_argebra
use_math: true
cover: "/assets/instacode.png"
toc: true
---

> EigenValue and EigenVector

---

 - 본 포스팅은 고유값 및 고유 벡터에 대한 개념을 정리하기 위해 개인적으로 정리한 포스팅입니다.

---

## EigenVector란?

 - 선형 변환이 일어난 뒤에도 방향이 변하지 않는 Vector.

## EigenValue란?

 - 선형 변환이 일어난 뒤, EigenVector의 길이가 변하는 배수.

## 바로 예시

 - $A = \\begin{pmatrix} 2 & 1 \\\ 1 & 2 \\end{pmatrix}$

 - $A * x = \lambda * x$

 - 이 때, $\lambda, Vector x$이 위 같은 식을 만족시킬 때, $\lambda$를 고유값, $x$를 고유 벡터라고 한다.

 - 이를 좌변으로 넘겨주면 $(A-\lambda*I)*x = 0$와 같은 식으로 변형할 수 있는데, 이 때 만약 $x$가 Zero-vector라면?

 - $A$나 $\lambda$나 어떤 값이 들어가도 저 식을 만족하게 되어 버려서, 고유값의 개수는 무한대가 되어 버린다.

 - 따라서, 유한한 개수의 고유값을 가지기 위해서는 $x$가 Zero-vector가 아니어야하고, 이는 곧 $(A-\lambda*I)$가 역행렬을 가지지 않아야 한다는 말과 같다.

 - $(A-\lambda*I)$가 역행렬을 가지지 않는다는건 뭐냐. 바로 저 행렬의 Determinant가 0여야 한다는 말이다.

 - 따라서 $det(A-\lambda*I) = 0$이 된다.

 - 이는 $det(\\begin{pmatrix} 2-\lambda & 1 \\\ 1 & 2-\lambda \\end{pmatrix}) = 0$이고

 - =  $(2-\lambda)^2 - 1 = 0$

 - =  $(\lambda)^2 - 4*(\lambda) + 3 = 0$

 - $(\lambda)_1 = 1, (\lambda)_2 = 3$이다.

 - $(\lambda)_1$을 다시 식에 넣어보면

 - $\\begin{pmatrix} 2 & 1 \\\ 1 & 2 \\end{pmatrix} * \\begin{pmatrix} x_1 \\\ x_2 \\end{pmatrix} = \\begin{pmatrix} x_1 \\\ x_2 \\end{pmatrix}$

 - 이므로, $x_1 = 1, x_2 = -1$이고

 - $(\lambda)_2$를 식에 넣었을 때에는

 - $x_1 = 1, x_2 = 1$이 된다.
  
 - 즉, $A * \\begin{pmatrix} 1 \\\ -1 \\end{pmatrix} = 1 * \\begin{pmatrix} 1 \\\ -1 \\end{pmatrix}$가 되고

 - $A * \\begin{pmatrix} 1 \\\ 1 \\end{pmatrix} = 3 * \\begin{pmatrix} 1 \\\ -1 \\end{pmatrix}$가 되는 것이다.

## 근데 이게 왜 중요해?

 - 선형 변환을 고유 벡터 및 고유 값으로 나타낼 수 있다는 것은, 곱해지는 벡터에 가해지는 연산은, 곱셈으로 한정된다는 것이다.

 - 반대로 말하면, 고유 벡터 및 고유 값으로 나타낼 수 없다는 건, 가해지는 선형 변환이 회전 변환을 포함하고 있다는 것을 역으로 알 수 있다.

 - 이와 연관지어서 어떤 행렬의 Determinant를 분석함으로써 이 행렬이 다른 벡터에게 어떠한 기하적 변환을 가져올지를 미리 알 수 있고

 - 또한 해당 벡터를 오로지 확대 및 축소를 할 때 사용할 행렬 또한 얻을 수 있는 것이다.

