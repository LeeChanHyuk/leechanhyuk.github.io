---
title: "[ML] Data Augmentation"
date: 2021-04-01 11:30:06
author: Leechanhyuk
categories: Machine_Learning
tags: Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

## 시작하기전에

 - 본 포스팅은 AutoAugment: Learning Augmentation Policies from Data, 2018, CVPR

 - RandAugment: Practical automated data augmentation with a reduced search space, 2019, CVPR

 - mixup: Beyond Empirical Risk Minimization, 2017, CVPR 논문을 참조하여 개인 정리 목적으로 기술한 포스팅입니다.

## 배경

 - Supervised Learning을 수행할 때, 우리는 보통 Dataset(In sample)을 통해서 우리가 짠 모델을 훈련시킴으로써 현실의 문제 (Out sample)를 해결하고자 한다.

 - 그 말은, 우리는 가고자 하는 목표치는 철저하게 Dataset에 맞춰져 있고, 아무리 Regularization을 통해서 학습을 한다 한들, 실제 우리가 목표로 하는 Target function과의 차이는 있을 수 밖에 없다.

 - 하지만 우리가 가진 Dataset에는 한계가 있고, 모든 경우의 수를 전부 담는 것은 불가능에 가까울 뿐더러 막대한 비용과 시간이 소요된다.

 - 여기서 착안하여 나온 것이 바로 Data augmentation이다.

## Data agumentation 이란?

 - Data augmentation이란 말 그대로, 데이터 증강. 즉, 가상의 데이터를 만들어 내는 것을 뜻한다.

 - 주된 방법으로는 Dataset의 Sample들에 Transformation을 취해서 새로운 데이터를 만들어 내는 방법을 사용한다.

 - 물론 이 또한 본래 Dataset의 Distribution에서 크게 떨어진 데이터를 만들기는 힘들지만, 기존 Data set과 비교했을 때는 확연히 Generalization 측면에서 나아갔다고 볼 수 있다.

## 