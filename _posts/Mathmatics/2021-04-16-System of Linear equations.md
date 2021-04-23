---
title: "[Linear algebra] System of Linear Equations"
date: 2021-04-16 11:30:06
author: Leechanhyuk
categories: Mathmatics
tags: Mathmatics Linear_argebra
use_math: true
cover: "/assets/instacode.png"
toc: true
---

**Linear algebra 3강**

**본 포스팅은 김영길 교수님의 유튜브 강의를 보고 개인적인 공부 목적으로 적은 것 임을 밝힙니다. 문제가 될 시 삭제하겠습니다.**

* * *

## 목차

1. Vector space
2. Linear transformation and matrix
3. **System of linear equations**
4. Determinant
5. Diagonalization
6. Inner product space

* * *

## Pivot variable이란?

 - 미리 정리를 해놨어야 하는 개념인데, 지나쳐 버려서 여기에서 정리한다.

 - Pivot variable이란 Gaussian elimination을 통해 행렬을 Echelon Form으로 만들었을 때, (0, 0), (1, 1), (2, 2)등의 대각선 성분을 의미한다.

 - 물론 Echelon Form으로 만드는 과정에서 행과 행, 혹은 열과 열을바꾸거나 하는 식으로 바꾸어서 대각선 행렬을 일단 채워서, 결과적으로 행렬을 계단 형식으로 만든다.

 - 만들고 나서도 대각선 성분 중, 즉 Pivot variable중 0인 곳이 있다면 그것은 이미 존재하는 다른 행 또는 열과 Linearly dependent하다는 의미이다.

 - 반대로 말하면, Echelon Form을 만들었음에도 남아있는 애들은, 서로서로가 Linearly independent하다는 의미와 같고, 이는 Pivot variable의 개수가 Rank와 같다는 것을 의미한다.

 - Pivot variable이 아닌 성분들은 모두 Free variable로 표현한다.

## System of Linear Equation이란?

 - 우리 말로는 연립방정식.

 - $Ax = \beta$일 때, 이 식의 Solution이 존재하면 이 System을 Consistent하다. 존재하지 않으면 Inconsistent하다고 표현한다.

## Homogeneous Equation 이란?

 - $Ax = 0$의 형태를 띄는 Equation을 의미한다.

 - Homogeneous equation의 경우에는 Solution이 항상 존재한다. 왜냐하면 Zero-vector를 항상 Solution으로 포함하니까.

 - Homogeneous equation의 경우에는 A라는 System이 있을 때, A의 Nullspace가 Solution이 될 것이므로 $Nullspace = Dimension - Rank(A)$가 된다.

 - Homogeneous equation의 해를 구하는 것은, x의 Free variable에 임의의 수를 할당하는 것으로 부터 시작된다.

 - Free variable은 각 행을 곱하거나 더하는 과정에서 얼마든지 커지거나 작아질 수가 있어서, 무슨 수를 대입하든 상관이 없다.

 $\\begin{pmatrix} 1 & 2 & 2 & 2 \\\ 0 & 0 & 2 & 4 \\\ 0 & 0 & 0 & 0 \\end{pmatrix}$ $\\begin{pmatrix} P_1 \\\ 1 \\\ P_2 \\\ 0 \\end{pmatrix}$ = $\\begin{pmatrix} 0 \\\ 0 \\\ 0 \\end{pmatrix}$

 - 위 식을 보자. A 행렬에서는 첫째 행의 1과 둘째 행의 2가 Pivot variable이고, 나머지는 모두 Free variable이다. 즉, 이 행렬의 Rank는 2이다.

 - 따라서 x의 Pivot variable 빼고는 모두 임의의 숫자를 대입했다.

 - 이 식을 따라서 쭉 풀어보면 결국에는 해가 $\\begin{pmatrix} -2 \\\ 1 \\\ 0 \\\ 0 \\end{pmatrix}$가 된다.

 - 이것이 바로 Solution, 즉 여기서는 우변이 0이므로 Null space가 된다.

## Non-homogeneous equation

 - Non-homogeneous equation. 즉 $Ax = b$에서 b가 Zero-vector가 아닌 경우, Homogeneous Equation을 먼저 푼다. 그 후에 그 해에 Particular equation을 풀어서 더해준다.

 - 즉, $Ax = b$에서 b의 차원이 n이고, A의 차원이 n이상이면, 이 식은 해가 존재한다. (Linearly independent한 행이 n개는 있어야 해를 구할 수 있잖아)

 - 따라서, Gaussian elimination을 통해서 A를 정리하고(= Echelon Form으로 만들고) , b에도 같은 연산을 해줘야겠지?

 - Homogeneous solution과 다른 해 구하는 방법은, Free variable에 전부 0을 넣고 solution을 구하는 것이다. (이게 잘 이해가 안간다. Homogeneous solution 구할 때에는 0 넣으면 안되나?)

 - 그렇게 해서 Particular solution을 구하고, Homogeneous solution에 더하면 된다.