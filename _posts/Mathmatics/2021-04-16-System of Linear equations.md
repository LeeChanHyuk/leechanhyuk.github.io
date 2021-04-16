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

## System of Linear Equation이란?

 -  우리 말로는 연립방정식.

 - $Ax = \beta$일 때, 이 식의 Solution이 존재하면 이 System을 Consistent하다. 존재하지 않으면 Inconsistent하다고 표현한다.

 - Homogeneous equation의 경우에는 Solution이 항상 존재한다. 왜냐하면 Zero-vector를 항상 Solution으로 포함하니까. ($Ax = 0$을 Homogeneous equation이라고 한다.)

 - Homogeneous equation의 경우에는 A라는 System이 있을 때, A의 Nullspace가 Solution이 될 것이므로 $Nullspace = Dimension - Rank(A)$가 된다.