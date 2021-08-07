---
title: "[Concept summary] Cross-Entropy"
date: 2021-03-26 18:13:00
author: Leechanhyuk
categories: Machine_Learning
tags: Computer_Vision Machine_Learning
cover: "/assets/instacode.png"
toc: true
---

Cross-Entropy (CE)

------------------------------------------------------

## 시작하기전에

 - 개인 공부 목적으로 정리한 글입니다. 설명이 부족할 수 있습니다.

## Cross-Entropy란?

 - Loss function중 하나. Input 값(=Model의 Output값)이 일반적으로는 확률로써 들어가기 때문에

 Activation function이 주로 Sigmoid나 Softmax가 된다.

$\Sigma(L_i*Log(S_i))$

<img src="/assets/image/Cross_Entropy/-log.PNG" width="450px" height="300px" title="title" alt="title">

-Log(x) 의 개형이다.

x로는 확률값이 들어가기 때문에 그림에 나타나있는 부분만 보면 된다.

x가 0이되면, 무한대에 가깝게 되고, 1이되면 0이되는걸 확인할 수 있을 것이다.

Li, 즉 정답이 [0 1]^T 라고 할 때, 즉 Label이 두 개 있는 Binary classification일 때, 두 번째 원소를 예측하는 Task라고 하자.

만약 정답을 맞췄으면, [0 1] * [-log([0 1])] 이 되면, 0*무한대 + 1*0이 되어서 loss가 0이 된다.

만약 정답을 틀렸으면, [0 1] * [-log([1 0])] 이 되면, 0*0 + 1*무한대가 되어서 loss가 무한대가 된다.

실제로는 확률이 들어가니까 One-hot encoding을 하지 않는 이상 왠만하면 loss가 무한대가 되진 않겠지만, 중요한 점은 잘 맞췄을 때는 loss가 0에 가까워지고, 못 맞췄을 때는 loss가 무한대에 가까워 진다는 점이다.

이러한 과정을 통해서, **Label 값이 정해진 것에 대해서만 학습을 진행한다.** 이는 Binary classification이 아니라 Multi-Class classification (Label의 종류가 여러가지 인 것)이나

Multi-Label classification (한 image 내에 Label이 여러개인 것) 에도 동일하게 적용이 된다.

여기서 의문점이 들 수가 있다. Label 값이 정해진 것에 대해서만 학습을 진행한다면, Region proposal을 진행하는 detection 문제에서는 어떻게 적용되는거지?

Region proposal을 진행하는 Network나 알고리즘을 사용하는 모델에서는, Labeling 되지 않는 것들을 나타내는 클래스를 별도로 명시해둔다.

따라서 Cross-Entropy로 학습을 진행하더라도 문제가 없는 것이다. 다만, 여기서 고려해야할 것은 YOLO나 SSD와 같은 One-Stage network의 경우에는 Proposal된 Region들을

별도로 걸러내는 과정이 포함되어 있지 않기 때문에 Class-imbalance (클래스 불균형) 문제가 발생하여, Negative sample에 지나치게 편중된 학습이 진행될 수 있다.

이를 해결하기 위한 방법으로는, Focal loss처럼 loss function 자체에 Negative sample의 Loss 간섭도를 낮추거나, 아니면 어떠한 필터링을 추가하여 Proposal 된 Region들을 걸러내는 방법이 있겠다.




