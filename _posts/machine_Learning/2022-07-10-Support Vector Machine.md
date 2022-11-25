---
title: "[Concept summary] Support Vector Machine"
date: 2022-07-10 13:30:06
author: Leechanhyuk
categories: Machine_Learning
tags: Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

# Support Vector Machine이란?

  - 데이터가 어떤 카테고리에 속하는지를 판단하는 분류기

  <img src="/assets/image/svm/data.png" width="600px" height="900px" title="title" alt="title"> 

  - 위 그림처럼 색깔이 같은 각 다중 데이터를 각 카테고리에 맞게 분류하는 선 (데이터의 차원 및 복잡도에 따라 면이 될수도, 다각형이 될 수도 있다.)을 찾는 것이 바로 SVM의 역할. 이 때, 분할하는 plane을 hyperplane이라고 한다.

# Hyperplane 결정 방법

  - Hyperplane은 각 카테고리별로 최대한 많은 데이터를 분류할 수 있는 선에서, 각 데이터와 hyperplane사이의 거리 (margin)이 최대가 되는 plane을 hyperplane으로 정한다.

  - 각 데이터와 hyperplane과의 orthogonal한 거리가 길면 길 수록, 해당 hyperplane은 변수가 생겨도 좀 더 안정적으로 분류할 수 있겠지?

  - 따라서 이 margin을 계산하는 것이 중요하며, hyperplane은 hyperplane의 법선 벡터인 w와 y절편인 b와 데이터 x의 연산이 0가 되는 plane을 hyperplane으로 정한다.
  $<w, x> + b = 0$

# Hard SVM vs Soft SVM

  - 앞서 말한 것들은 Hard SVM

# SVM은 왜 반대로 차원을 확장시키는 방식으로 동작할까요? SVM은 왜 좋을까요?

  - 기본 차원에서도 분류가 가능한 경우도 있지만, 데이터가 복잡한 구조를 가지면 가질 수록 단순한 hyperplane으로는 분류가 어려워진다.
  따라서 데이터를 고차원으로 projection하여 데이터를 다양한 방향에서 바라봄으로써, 그에 맞는 고차원 hyperplane을 찾음으로써 데이터 분류시 최적의 hyperplane을 사용가능하니까.

  - 이는 우리가 딥러닝에서 레이어를 여러개 쌓는 것과 유사하다. 여러개의 레이어 사이에 비선형 Activation function을 넣음으로써 각 레이어를 다중 차원으로 간주하고, 더 정확한 hyperplane을 찾는 것.