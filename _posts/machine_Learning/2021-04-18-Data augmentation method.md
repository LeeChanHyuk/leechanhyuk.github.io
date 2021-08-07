---
title: "[Concept summary] Data Augmentation"
date: 2021-04-18 11:30:06
author: Leechanhyuk
categories: Machine_Learning
tags: Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

## 시작하기전에

 - 본 포스팅은 EfficientNet V2 내 활용되었던 Data augmentation technique 이해를 위해, 간략하게 작성된 포스팅입니다.
 
 - RandAugment: Practical automated data augmentation with a reduced search space, 2019, CVPR

 - mixup: Beyond Empirical Risk Minimization, 2017, CVPR 논문을 참조하여 개인 정리 목적으로 기술한 포스팅입니다.

## 배경

 - Supervised Learning을 수행할 때, 우리는 보통 Dataset(In sample)을 통해서 우리가 짠 모델을 훈련시킴으로써 현실의 문제 (Out sample)를 해결하고자 한다.

 - 그 말은, 우리는 가고자 하는 목표치는 철저하게 Dataset에 맞춰져 있고, 아무리 Regularization을 통해서 학습을 한다 한들, 실제 우리가 목표로 하는 Target function과의 차이는 있을 수 밖에 없다.

 - 하지만 우리가 가진 Dataset에는 한계가 있고, 모든 경우의 수를 전부 담는 것은 불가능에 가까울 뿐더러 막대한 비용과 시간이 소요된다.

 - 여기서 착안하여 나온 것이 바로 Data augmentation이다.

## Data augmentation 이란?

 - Data augmentation이란 말 그대로, 데이터 증강. 즉, 가상의 데이터를 만들어 내는 것을 뜻한다.

 - 주된 방법으로는 Dataset의 Sample들에 Transformation을 취해서 새로운 데이터를 만들어 내는 방법을 사용한다.

 - 물론 이 또한 본래 Dataset의 Distribution에서 크게 떨어진 데이터를 만들기는 힘들지만, 기존 Data set과 비교했을 때는 확연히 Generalization 측면에서 나아갔다고 볼 수 있다.

## 기존 연구

 - Rand augment 이전의 augmentation 기법들 중, 성능이 뛰어났던 방법은 Auto-augment, Fast-Auto-augment 등이다.

 - 위 방법들은 강화학습 기반의 방법을 통해서 데이터 셋 생성에 최적화 된 Policy를 찾는 방법들이다.

 - Proxi task에서 찾은 Solution을, 큰 task에 적용할 수 있다는 가정을 기반으로 한다.

 - Search space가 넓어, 학습이 매우 오래걸리는 단점이 있다.

 - Model 및 Data set size에 맞게 Regularization strength (Magnitude)를 조절할 수 없는 단점 또한 있다.

## Rand Augment - method

 - 실제 Training에서 본 Network가 같이 학습될 수 있도록 Parameter수를 적게 가져감.

 - 14개의 Augmentation Transformation method들 중에 무작위로 선택하여 N번 적용

   <img src="/assets/image/data_augmentation/1.png" width="450px" height="300px" title="title" alt="title">

 - 즉, 최대 $K^N$개의 Policy를 나타낼 수 있음.

 - 각각의 Transformation의 정도는 M(Magnitude) Parameter를 통해 적용

 - 각 Transformation은 Range가 정해져있고, 그 Range안에서 Magnitude에 따라서 선형적으로 Value 값을 결정함.

 - Magnitude Range는 실험 결과 파트 참조

 - Magnitude 선택 방식은 Random, Constant, Linearly incresing 모두 실험

    <img src="/assets/image/data_augmentation/2.png" width="450px" height="300px" title="title" alt="title">

     → 모두 결과가 잘 나옴. 비교적 Computational Cost가 낮은 Constant Magnitude를 가지고 실험

 - Grid search를 사용하여 실험

## Rand Augment - experiment

 - Magnitude와 Network size, Training set size의 상관 관계를 알아보기 위한 실험을 수행

 - Magnitude 범위는 Wide-Resnet의 경우에는 [1:30]까지. 모델의 크기와 상관 관계가 있으므로 Optimal Range는 제시되어 있지 않음.

 - Wide-Resnet을 사용하여 Parameter 조정으로 Network size에 변화를 주며 실험

 <img src="/assets/image/data_augmentation/3.png" width="450px" height="300px" title="title" alt="title">

 - 실험 결과, Network size, Training set size 모두 Magnitude와 비례하는 결과 도출

## Mixup - Motivation

 - 일반적인 Supervised Learning은 Dataset에 Dependency가 생김.

 - 여러 규제 방법을 사용하더라도 한계점이 명확.

 - 결국에는 Dataset의 Distribution에 변화를 주어야 함.

## Mixup - effect

 - Generalization 효과 (For out of sample).

 - Reduces corrupted labels.

 - Robustness to adversial example.

## Mixup - method

 - Mixup 이란 두 샘플을 일정한 비율로 섞어서, 새로운 샘플을 만들어내는 방법.

 <img src="/assets/image/data_augmentation/4.png" width="450px" height="300px" title="title" alt="title">

 - 비율 $\lambda$는 Beta distribution으로 부터 가져옴

 <img src="/assets/image/data_augmentation/5.png" width="450px" height="300px" title="title" alt="title">

 - Pytorch Code

 <img src="/assets/image/data_augmentation/6.png" width="450px" height="300px" title="title" alt="title">

 - 아래 그림과 같이 Label도 soft-labeling을 사용해 label corruption을 Reduce시킨다.

 <img src="/assets/image/data_augmentation/7.png" width="450px" height="300px" title="title" alt="title">

 - Mixup은 Mini-batch를 뽑고, 뽑을 때 마다 Mixup을 적용하여 Sample을 늘리는 식으로 동작.

## Mixup - experiment

 - Resnet 관련 모델을 활용하여 실험, ERM 방식(Empirical Risk Minimization)보다 Mixup을 활용한 방식이 ACC면 (좌측 그래프)에서나 Gradient norm (우측 그래프) 면에서 더 뛰어남

 - 또한 \lambda의 값이 0.5 근처에 있을 때, 가장 불안정한 모습을 통해 Beta distribution을 사용한 이유가 추론 가능
 
 <img src="/assets/image/data_augmentation/8.png" width="450px" height="300px" title="title" alt="title">

## In EfficientNet V2

 - 논문 내에는 위에 기재한 두 테크닉을 아래와 같은 Hyper-parameter를 사용하여 Data-augmentation을 수행하였다.

 <img src="/assets/image/data_augmentation/9.png" width="450px" height="300px" title="title" alt="title">

## 마치며

 - Data augmentation에 많은 기법이 있다는 사실을 처음 알았다. 사용해볼 가치는 충분할 듯 하다.