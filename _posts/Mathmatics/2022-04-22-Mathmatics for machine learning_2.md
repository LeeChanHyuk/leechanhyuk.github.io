---
title: "[Mathmatics for machine learning] Linear transformations"
date: 2022-03-28 11:30:06
author: Leechanhyuk
categories: Mathmatics
tags: Mathmatics Machine_learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

**Mathmatics for machine learning**

- 본 포스팅은 Mathmatics for learning을 공부하는 과정에서, 기억해야 할 개념이나 햇갈리는 개념만 모아서 정리한 개인 정리용 포스팅입니다.

## 시작하기전, 복습. Vector란?

  - '벡터란 방향과 크기를 나타내는 물리량'이라고 배웠지만, 이는 물리학의 관점에서 Euclidean vector를 의미하는 것.

  - 선형 대수에서는 아래 성질을 나타내는 집합을 Vector space라고 정의하고, 해당 공간에 속하는 개체를 Vector라고 정의한다.

    - 덧셈에 대한 항등원 존재 (Zero-vector의 존재)

    - 덧셈에 대한 역원 존재 (즉, 원점을 지나갈 것)

    - 교환법칙 성립

    - 결합법칙 성립

## Linear combination

  - **모든 벡터는, 해당 벡터가 속한 공간의 기저 벡터 (Basis vector)의 조합으로 이루어 진다.**

  - 모든 벡터는 기저 벡터의 스칼라를 곱한 결과이기 때문에, 벡터 간의 덧셈 또한 기저 벡터에 스칼라를 여러번 더한 것으로 표현 가능하다.

  - 따라서, 모든 벡터 간의 덧셈은, 결합된 벡터가 속한 벡터 공간에 닫혀있다.
  
  - 기본적인 덧셈이 아닌, 선형 결합을 생각해보자.

  - 다른 두 기저 벡터 간의 선형 결합은, 두 기저 벡터가 표현할 수 있는 모든 범위를 나타내고, 이는 하나의 공간을 만들어낸다. 이것을 **Span**이라고 한다.

  - Span에 참여하는 기저 벡터의 숫자가 바로 생성된 벡터 공간의 차원을 의미하고 이를 **Rank**라고 표현한다.

  - 만약 span에 참여하는 벡터들이 각각 완전히 독립된 방향을 가리키고 있다면, 이 벡터들은 서로 **Linearly independent**하다고 표현하고,도출된 벡터 공간에서는 참여한 벡터를 이루는 기저 벡터 수의 합만큼 Rank가 도출된다.

  - 반대로 Span에 참여하는 벡터들 중, 중복된 기저 벡터를 가지고 있는 벡터들을 서로 **Linearly dependent**하다고 표현하고, 결합을 통해 도출된 벡터 공간은 Linearly dependent한 벡터를 제외하고 겹치지 않는 기저 벡터의 수 만큼 Rank가 도출된다.

## Linear transformation

  - Linear transformation은 그냥 함수다. 벡터를 넣으면 벡터가 나오는 함수.

  - Linear transformation에서 Linear하다는 건, 변환 전 후의 원점이 동일하고, 기존에 평행했던 벡터들이 변환 후에도 동일하게 평행해야 한다.

  - 이는 각 벡터 공간의 기저 벡터가 어떻게 변하는지를 살펴보고, 변한 기저 벡터에 원래 벡터를 곱하게 되면, 변환된 벡터가 나오게 된다.

  - 즉, 기저 벡터를 변환시키는 값의 집합을 행렬로 나타내면, 행렬로 Linear transformation을 나타낼 수 있게 되는 것이다.

## Matrix composition

  - 행렬 곱으로 Linear transformation을 나타낼 수 있다고 했다.

  - Linear transfomation이 여러번 반복될 경우, 이는 다중 행렬 곱으로 나타낼 수 있고, 여러 Transformation matrix는 곧, 행렬 곱을 통해서 하나의 Transformation matrix로 나타낼 수 있다.
  
 ## Determinant

  - Determinant는 선형 변환에 의한 영역의 변화를 나타내는 Factor이다.

  - Determinant가 0이라는 것은, 이 행렬과 곱해지는 모든 행렬이 가지는 넓이를 0으로 만드는 것을 의미한다. (Rank를 2로 가정할 때)

 ﻿  - 그렇다면 Determinant가 음수인 것은 어떤걸 의미할까?

  - 그건 바로 영역을 뒤집는 것을 의미한다. 이 때에도 넓이가 변하는 것은 Determinant의 절대값을 의미한다.

  - 3차원에서는 부피를 확장하고 축소하는 것을 의미한다.

  - 행렬식은 Rank가 2인 행렬에서는 ad-bc로 구한다.

  - 그 이유는 ad-bc가 두 벡터가 이루는 평행사변형의 넓이를 나타내기 때문이다.

  - 이 부분이 잘 이해가 가지 않는다면 Youtube에 3blue1brown의 determinant 영상을 찾아보기를 추천한다.