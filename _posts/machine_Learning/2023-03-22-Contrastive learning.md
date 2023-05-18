---
title: "[Concept summary] Contrastive learning"
date: 2023-03-22 14:30:06
author: Leechanhyuk
categories: Machine_Learning
tags: Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

# Contrastive learning 이란?

  - 입력 샘플 간의 비교를 통해 각 클래스의 특성을 학습하는 방법

# Contrastive learning 은 어떻게 진행되나?

  - Classification task를 예로 들자면, Contrastive learning은 기존과 다르게 2개의 Input을 통해 학습

  - Positive pair 간의 거리는 가깝게 측정할 수 있도록, Negative pair는 멀게 측정할 수 있도록 만드는 것이 목적

  - 즉, 하나의 모델이 입력에 따라 올바른 Representation을 생성할 수 있도록 하는 것이 목적인데, 이 때 사용되는 Input은 2개이고, Label은 Positive(1), Negative(0)이거나 두 Pair 간의 특정한 유사도 등이 되는 것이다.

  - 2개의 Input의 Representation을 구하고 두 Represeneation의 유사도를 측정하여, Positive pair일 경우 이 유사도가 높게, Negative pair일 경우 이 유사도가 낮게 만들어주는 Loss를 사용

  - 주로 사용되는 Loss로는 Contrastive loss, NCE, Soft-Nearest Neighbors loss 등이 있다.

