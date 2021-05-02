---
title: "[Linear algebra] Relationship between Matrix and Linear Transformation"
date: 2021-04-16 11:30:06
author: Leechanhyuk
categories: Mathmatics
tags: Mathmatics Linear_argebra
use_math: true
cover: "/assets/instacode.png"
toc: true
---

 - Linear Transformation을 행렬로 나타내려면 어떻게 해야할까?

 - LT(Linear Transformaiton)은 같은 차원끼리의 Transformation에도 적용되지만, 행렬과 연관지어 생각했을 때, 특히 햇갈리는 부분은 다른 차원으로의 LT가 햇갈리기 십상이다.

 - B라는 2차원 공간을 C라는 3차원 공간으로 이동시키는 T라는 LT가 있다고 가정하자.
 
 - 그리고 T라는 LT를 A라는 행렬으로 표현하고, 원래 공간인 B는 x, y라는 원소를 갖는 행렬 B로 표현하고, C라는 3차원 공간은 C라는 행렬로 표현하여 아래와 같은 행렬곱이 있다고 해보자.

 $A = \\begin{pmatrix} a & b \\\ c & d \\\ e & f\\end{pmatrix}$ $B = \\begin{pmatrix} x1 \\\ y1 \\end{pmatrix}$ $C = \\begin{pmatrix} ax1 + by1 \\\ cx1 + dy1 \\\ ex1 + fy1 \\end{pmatrix}$

 - 우리는 이를 $AB = C$라고 표현할 수 있을 것이다.

 - LT에와는 반대로, 행렬 연산은 데이터보다 앞쪽에 배치하여 곱한다. (이유는 본래 데이터를 열벡터로 나타내기 위함이다.)

 - A는 (3x2) 행렬, B는 (2x1) 행렬이기 때문에, 결과 행렬 C는 (3x1) 행렬이 된다.

 - 차원만 따져봤을 때에도 A라는 행렬이 B라는 2차원 데이터를 C라는 3차원 데이터로 바꾼다는 것을 알 수 있다.

 - 곱해지는 과정을 하나하나 따져보면, A의 1행 * B의 1열, A의 2행 * B의 1열... 과 같은 식으로 진행된다.

 - 보면 B의 데이터는 하나의 좌표 (1열) 으로 구성되어 있고, 데이터는 A의 각 행과 곱해져서 결과 행렬의 행을 이루게 된다.

 - 이는 결국 A의 행이 LT의 한 요소라고 생각하면, 그 한 요소가 모든 입력 데이터와 곱해져서 그 결과 값을 결과 행렬의 행에 순차적으로 저장하는 꼴이 된다.

 - 결과적으로, 결과 행렬 C의 각 행은 B라는 모든 데이터와 LT(A 행렬)의 일부분을 곱해서 이루어지고, 각 행이 모여서 3차원 행렬 C를 이루게 만든다.

 - 정리하자면, A의 각 행은 LT의 일부분을, A 전체는 전체 LT를
 
 - B의 각 열은(예제에서는 하나였지만) 각 데이터를
 
 - C의 각 행은 B의 데이터가 A라는 LT의 일부분(A의 각 행)을 만나서 도출되는 결과값을 의미한다. 즉, LT의 결과값을 의미한다.