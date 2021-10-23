---
title: "[Discussion] RELU가 sigmoid보다 더 많이 사용되는 이유는?"
date: 2021-10-23 16:59:00
author: Leechanhyuk
categories: Discussion
tags:	Discussion
use_math: true
cover: "/assets/instacode.png"
toc: true
---

# Background

  - RELU나 Sigmoid는 activation function으로 불리며, 보통은 선형적인 모델 구성으로는 풀 수 없는 XOR 문제와 같은 것들을 풀기위해 모델에 비선형적 요소를 가미해주기 위해 사용되는 함수이다.

  - ## Sigmoid

      <img src="/assets/image/sigmoid_relu/sigmoid.png" width="450px" height="300px" title="title" alt="title">

      - Sigmoid 함수는 위와 같은 형태를 가지는 **비선형함수**이며, Input을 0과 1사이의 값으로 변환시켜준다.

      - 보통 Binary cross-entropy와 결합되어 Binary classification task에 사용된다.

  - ## RELU

      <img src="/assets/image/sigmoid_relu/relu.png" width="450px" height="300px" title="title" alt="title">

      - RELU 함수 역시 위와 같은 형태를 가지는 **비선형함수**이며, Input 중 0이상의 값만 남기는 함수입니다.

      - 최근에는 범용적으로, Sigmoid 보다 더 많이 사용되는 함수입니다.
      
# 왜 sigmoid보다 RELU가 많이 사용될까?

  - sigmoid보다 RELU가 많이 사용되는 이유가 뭘까요?

  - 가장 큰 이유는 바로 Gradient vanishing problem을 해결하기 위함입니다.

  - Gradient vanishing problem이란, backpropagation 단계에서 게속해서 미분을 취하기 때문에, 원래의 정보가 마지막 레이어까지 전파되지 못하여 학습이 골고루 되지 못하는 현상을 가리킵니다.

  - 여기서, 만약 레이어마다 sigmoid를 사용한다면 어떻게 될까요?

  - sigmoid는 입력값을 0~1사이 값으로 만들기 때문에, 매 레이어를 거칠 때 마다 입력값은 기하급수적으로 줄어들게 됩니다.

  - 반면에, relu 같은 경우에는, 입력의 양의 값을 그대로 보존하면서도 비 선형성을 가미할 수 있기 때문에, gradient vanishing problem으로 부터 자유로운 장점이 있기 때문에, sigmoid보다 relu를 많이 사용하는 것입니다.

  - 때로는 맨 마지막 레이어만 sigmoid로 바꾸어, 최종 output은 0~1 사이의 형태로 가져가되, gradient vanishing problem을 해결하는 방법을 사용하기도 합니다.

# Reference

  - Figure 1: https://38402160.tistory.com/39

  - Figure 2: https://pytorch.org/docs/stable/generated/torch.nn.ReLU.html