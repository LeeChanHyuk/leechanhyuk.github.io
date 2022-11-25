---
title: "[Linear algebra] 개념 정리_test"
date: 2022-09-23 11:30:06
author: Leechanhyuk
categories: Mathmatics
tags: Mathmatics Machine_learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

# 시작하기전에

  - 본 포스팅은 제가 Linear algebra에 관한 전체적인 개념을 **개인 개념 정리용**으로 간략히 정리한 글입니다.

  - 주로, 3Blue1Brown [링크](https://www.youtube.com/c/3blue1brown) 및 김영기 교수님의 강의 [링크](https://www.youtube.com/channel/UCOBAmYX1K_bVjEPMCP5uTKQ)가 토대가 되었습니다.

# 본론

## 선형대수학과 행렬에 관해

  - 세상의 많은 것들은 방정식으로 풀 수 있어.

  - 경영, 통계, 인공지능 등 많은 분야의 핵심적인 일들에는 방정식이 꼭 사용되지.

  - 이 방정식들을 선형 대수학 적으로 표현하게되면 이를 Linear system of equations라고 해.

  - 이런 방정식들은 행렬을 통해 보기도 좋게, 계산하기도 편하게 간소화 할 수 있어
  
  - 예를 들자면 $ax+by=c$랑 $ex+fy=g$라는 식이 있을 때, 이는 행렬을 통해서 아래와 같이 나타낼 수 있어.

  - $\\begin{pmatrix} a & b \\\ e & f \\end{pmatrix} * \\begin{pmatrix} c \\\ g \\end{pmatrix}$

  - 또 이건 $A\overrightarrow{x}=\overrightarrow{v}$와 같이 나타낼 수 있고, 이런식으로 나타내면 Matrix A를 통해 
  $\overrightarrow{x}$ 가 $\overrightarrow{v}$ 로 변환될 수 있다는 것 또한 쉽게 확인할 수 있지.

  - 위와 같은 예시들을 보면 알겠지만, Matrix는 어떠한 데이터 그 자체로도 사용되지만, 선형대수학에서는 주료 '변환 (Transformation)'에 초점을 맞추고 있어.

  - 따라서 이 matrix가 원본 데이터에 곱해졌을 때, 어떠한 변화가 발생하는지에 관해서 주로 다루고 있다는 걸 염두에 두면 좋을거야.

## 선형적이란?

  - 선형적이라는건 뭘까?

  - 선형적이라는건 일반적으로 중첩의 원리를 따른다는 것을 의미해.

  - 중첩의 원리란, 2개 이상의 Transformation이 있을 때, 두 Transformation의 순서와 관계없이 결과 값이 일정한 것을 의미해. 수식으로 나타내자면, 아래와 같이 나타낼 수 있겠지.

        1. T(CX, CY) = CT(X) + CT(Y)
        2. T(CX) = CT(X)
    
  - 이게 가지는 의미는, 어떠한 input에 가해지는 Transformation의 종류들만 알 수 있다면 output도 예측이 가능하다는데 있어.

## 선형독립? 비독립?

  - 어떠한 행렬이 있을 때, 행렬의 각 열이 서로 Linearly independent하다, dependent하다라고 자주 말하곤 하지.

  - 이건 결국 한 행렬 내에서 특정 열을 나머지 열의 조합으로 만들어 낼 수 있는가 없는가에 달려 있어.

  - 즉, 1열과 2열을 더하고 빼고 지지고 볶았더니 3열이 나왔다. 이건 3열이 1열과 2열에 관해 Linear dependent한거야.

  - 이것의 의미는 1열과 2열의 vector로 span 한 공간에 3열이 속한다는 의미야. 즉, 3열이 고유한 특징을 가지지 못한 vector라는 말이지.

  - 그렇다면 이 행렬이 Linearly dependent한지 independent한지 어떻게 알 수 있을까? 숫자를 일일히 넣어보면서 중학생때처럼 소거법으로 풀고 그래야하나?

  - 물론, 가우스 소거법이라는 좋은 방법을 우리는 이미 대학교에서 배웠겠지만, determinant라는 좋은 방법을 사용하면 좀 더 쉽게 알아볼 수 있어.

  - A 행렬에 관한 Determinant는 det(A)와 같이 표현하고, 그 원소인 a, b, c, d에 관해서 ad-bc라는 식으로 도출 돼.

  - 결론부터 말하자면 determinant가 0가 아니면 linearly independent, 0이면 linearly dependent야.

  - 그 이유는 determinant가 transformation 후의 결과를 함축하고 있기 때문인데, 아래와 같이 식이 주어졌다고 생각해보자.

  - $\\begin{pmatrix} a & b \\\ c & d \\end{pmatrix} * \\begin{pmatrix} x \\\ y \\end{pmatrix} = \\begin{pmatrix} e \\\ f \\end{pmatrix}$

  - 이 때, a와 d는 $\overrightarrow{x}$ 의 x축 y축과 비례해서 $\overrightarrow{v}$ 의 x축 y축에 영향을 주고, b와 c는 $\overrightarrow{x}의 x축 y축에 diagonal하게 $\overrightarrow{v}의 x축 y축에 영향을 주지.

  - 즉, ad-bc는 기존 축으로의 영향 - 대각선 축으로의 영향을 의미하는거고, 그 값이 같을 때는 $\overrightarrow{x}$ 가 원래의 차원을 유지하지 못하고, 더 낮은 차원으로 projection 된다는 것을 의미해.

  - 이건 결국 처음 linearly dependent, independent할 때 span의 결과값과도 유사하지? 거기서도 dependent하면 span 했을 때 열의 개수만큼의 차원을 만들어내지 못했잖아.

  - 이것도 유사하게 transformation 했을 때, 그 차원을 유지하지 못하게 되므로 linearly independent 하다는 것을 알 수 있게 되는거야.

  - ad 및 bc의 영향에 관한게 이해가 잘 안간다면 대표적인 shear transformation을 나타내는 아래와 같은 행렬을 봐.

  - $\\begin{pmatrix} 1 & 1 \\\ 0 & 1 \\end{pmatrix}$

  - a, d외에 b에 1이 들어있지? 따라서 diagonal한 방향으로의 transfomation도 일어나기 때문에 기울여지는거야.

  - 이런 transformation의 결과 차원을 나타내는 용어가 바로 Rank야.

  - 기존 차원과 동일하게 Rank가 유지되는 경우는 Full-Rank라고 하고, 이 때 Zero-vector로 projection 될 수 있는 애들은 차원이 축소되지 않으니까 Zero-vector 밖에 없겠지.

  - 반대로 Full-rank가 아닐 때, vector를 zero-vector로 transformation 하는 vector들의 집합은 null-space라고 표현해.

  - 아직 잘 모르겠다면, 이 [링크](https://www.youtube.com/watch?v=Ip3X9LOh2dk&list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab&index=6) 를 참고하면 좋을 것 같아.

## 고유 벡터? 고유 값?

  - 고유 벡터와 고유 값도 공학 쪽에서는 참 많이 등장하는 애들이지.

  - $A  \overrightarrow{v} = \lambda \overrightarrow{v}$ 에서 v가 바로 고유 벡터고, $\lambda$ 가 고유 값이야.

  - 이게 가지는 의미는 결국 기존 transformation 행렬인 A를 v라는 고유 벡터로 단순화 시킬 수 있다는 것을 뜻 해.

  - 즉, A의 고유 벡터를 모두 합치면 A가 일으키는 변화들을 모두 포괄할 수 있다는 것이 되고, 이는 결국 A라는 행렬의 차원을 축소하여 나타낼 수 있다는 것을 뜻해.

  - Determinant는 이런 고유 값, 고유 벡터를 찾는데도 자주 사용이 되는데, $(A - \lambda) \overrightarrow{v}$ 로 나타냈을 때 determinant가 0이 되는 애들이 바로 고유값이 돼.

  - 그 이유는 물론 determinant가 0이 되는건 linearly independent하다는 뜻이고, 이는 Transformation 시 차원의 축소가 일어난다는 뜻이니까, 더 낮은 차원의 Vector로 projection 가능하다는 것을 의미하기 때문이지.

  - 참고로, Basis-vector를 통해서 eigen-vector를 만들면 대각선 성분 외에는 모두 0이 되는데, 이런걸 diagonal matrix라고 해.

  - 여기서의 eigen-vector는 물론 basis-vector가 될거고, 고유 값은 Transformation의 결과 변화 량을 의미하므로 대각 성분 그 자체가 되겠지.

## 끝내면서

  - 간단히 중요 개념에 관한 것들만 정리해보았다.