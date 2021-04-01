---
title: "[CV] High-Performance Large-Scale Image Recognition Without Normalization"
date: 2021-03-10 21:05:00
author: Leechanhyuk
categories: paper_Review
tags: Computer_Vision Machine_Learning
cover: "/assets/instacode.png"
toc: true
---

<img src="/assets/image/High-performance/frontdoor.PNG" width="450px" height="300px" title="title" alt="title">


* * *

**High-Performance Large-Scale Image Recognition Without Normalization논문 리뷰**

**본 논문리뷰는 논문 구성을 따르고 있지만, 단지 제 생각을 구성에 맞춰서 적은 것이지**

**실제 논문을 요약하여 적은 것이 아닙니다. 착오 없으시길 바랍니다.**

* * *

## 논문 관련 정보

 - 추가 예정

## Abstract

 - Batch Normalization(BN)은 대부분의 분류 문제에서 필수적이었다.

 - Batch-size에 Dependence가 있고, Example마다 interaction이 있다. (*Inference마다 결과가 다르다는걸 의미하는 건가?*)

 - Batchnorm 없이 학습을 시도했지만, Batchnorm을 사용한 결과에는 미치지 못했다.

 - Batchnorm이 없으면, 데이터 셋이 커지거나, LR이 커질 때 Unstable했다.

 - Adaptive gradient clipping을 사용

 - 훈련 모델이 EfficientNet-B7 Accuracy에 match했다. (EfficientNet 84.4% vs this 86.5%)

 - 훈련 시간은 8.7배 빨랐다.

 - Fine-tuning 후에 더욱 성능 향상을 관찰 가능 했다. (89.2%)

## Introduction

 - ResNet은 Batch normalization을 사용하여 높은 성능을 달성했다.

 - 단점 1. Batch normalization은 메모리를 많이 소모하고, Gradient의 계산 등이 느리다.

 - 단점 2. BN을 사용했을 경우 Training 및 Inference시에 성능 차이가 난다.

 - 단점 3. Mini batch상에서 training example 사이의 independency를 없앤다.

 - 단점 3. BN은 하드웨어에 Dependency가 있고, 특히 분산학습시에 subtle implementation errors를 초래한다.

 - Batch 내의 training example 사이의 interaction이 network가 cheat하게 하기 때문에, 특정 작업에는 사용할 수 없다. (*training example 사이의 interaction이 뭐지?*) -> 뒷 문장에는 information leakage에 취약하다는게 나오므로, 아무래도 특정 알고리즘 학습 시 데이터끼리의 cheat가 제대로 된 training을 막을 수 있다는 의미로 보이기는 한다..

 - 한 batch가 large variance를 띄면, 성능이 저하된다.

 - Batch size가 너무 작으면 BN시에 성능 저하가 있다.

 - BN은 한계가 있다.

 - 지금껏 BN을 대체할만한 논문들은 나왔었지만, Test accuracy가 낮거나, Computing cost가 높거나 하는 단점들이 있었다.

 - 매우 deep한 resnet은 hidden activation(*?*)을 억제하는 방법으로 Normalize 없이도 성공하였다.

 - 











