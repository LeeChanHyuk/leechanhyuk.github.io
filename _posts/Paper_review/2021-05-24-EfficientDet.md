---
title: "[ML] EfficientDet : Scalable and Efficient Object Detection"
date: 2021-05-24 21:05:00
author: Leechanhyuk
categories: paper_Review
tags: Computer_Vision Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

* * *

**EfficientDet : Scalable and Efficient Object Detection 리뷰**

<img src="/assets/image/EfficientDet/front.PNG" width="600px" height="450px" title="title" alt="title">

본 리뷰는 논문의 세세한 요약보다는 논문을 제가 보기 쉽게 요약한 포스팅입니다.

* * *

<img src="/assets/image/EfficientDet/figure1.PNG" width="450px" height="300px" title="title" alt="title">

## Abstract

 - Model efficiency는 computer vision 분야에서 계속해서 중요하게 다뤄져왔다.

 - 본 논문에서는, Object detection을 위한 Architecture에 대해 연구하고, efficiency를 향상시킬 때 중요한 Key 몇가지를 제안한다.

 - 쉽고 빠른 Multi-scale feature fusion을 위해 BiFPN을 제안한다.

 - Resolution, depth, width for all backbone, feature network, and box/class prediction networks를 uniformly scale하는 Compound scaling을 제안한다.

 - EfficientDet은 resource constraints에 크게 제약받지 않는다.

 - COCO에서 SOTA를 달성(52.2 AP), 52M Parameters, 325B FLOPS.

## Introduction

 - SOTA Detectors는 Object detection acc에 집중해서 Cost efficiency에 대한 고려가 부족하다.

 - 이 점은 Resource contraint가 강한 실제 분야(Robotics or self-driving car..)에 해당 모델 사용을 힘들게 만들었다.

 - Efficientcy를 강조한 모델은 ACC가 낮았고, 그 중 대부분은 Resource에 대해 강하게 제약되었기 때문에, 여전히 실제 산업에 대한 적용은 힘들었다.

 - 그럼 어떻게 ACC와 Efficiency 둘 다를 잡을 수 있을까?

 - 본 논문에서는 One-stage detector paradigm에 따라서, Backbone, feature fusion, class/box network에 대한 구조적인 측면을 고민했다.

 - 핵심적인 문제는 두 가지가 있었다.

 - **Challenge 1 : Efficient multi-scale feature fusion**

 - 다른 Input feature는 다른 Resolution에서 왔음에도 불구하고, 기존의 FPN은 input Feature를 그냥 더하는 구조였다.

 - 따라서 Output-feature에는 여러 Input features가 균일하게 반영되지 못했다.

 - 제안하는 BiFPN은 Learnable한 weight로 다른 Input features의 중요성?을 배우고, Top-down, Bottom-up 방식으로 반복 적용되며 multi-scale feature fusion을 가능하게 한다.

 - **Challenge 2 : Model scaling**

 - 기존 모델을은 Acc 향상을 위해 규모가 큰 모델이나 High resolution image를 사용하곤 했다.

 - 우리는 Feature network, box/class prediction network를 같이 scaling 하는 것이 Efficiency 및 Accuracy를 동시에 잡기위해 중요하다는 것을 알았다.

 - EfficientNet에서 착안하여, EfficientDet은 Resolution, depth, width for all backbone, feature network, box/class prediction network를 전부 Scaling했다.

 - EfficientDet은 EfficientNet을 Backbone으로 사용했다.

 - EfficientDet은 Figure 1에 나온 어떤 모델보다 가볍고, 특히 D7은 52.2AP, 52M Parameteers and 325B FLOPS와 함께 SOTA를 달성했다.

 - EfficientDet은 기존의 Detectors와 비교해서 3x-8x 빠르다.

 - 모델을 조금 수정해서, EfficientDet은 Semantic segmentation Pascal VOC 2012에서 81.74%의 mIOU Accuracy를 18B FLOPS와 함께 달성했다. (DeepLabV3+보다 1.7% 정확하게, 9.8배 적은 FLOPs와 함께)

## 찾아봐야 할 것

 - FLOPs, FLOPS의 차이.

 - Prediction network란 Bounding box regression 단을 말하는 건가?

 - AP (Average Precision)의 정확한 의미.

## Related work

 - **One-stage detectors**

 - Region proposal이 따로 분리되어 있지 않은 Detectors를 의미.

 - Focal loss, Yolo, Overheat 등이 속함.

 - 본 논문도 여기에 속함.

 - **Multi-Scale feature representations**

 - 이전의 Object detectors는 multi-scale features를 잘 다루는 것이 중요했다.

 - 몇몇 모델은 Backbone network로 부터 추출된 pyramidal feature에 직접적으로 Prediction을 적용하기도 했다.

 - FPN은 Top-down 방식이었다.

 - PANet은 FPN 방식에 Bottom-up 방식을 추가했다.

 - STDL은 Cross-scale feature를 다루기 위해 scale-transfer module을 사용했다.

 - M2det은 Multi-scale의 feature를 섞기 위해 U-Shape의 module을 도입했다.

 - NAS-FPN은 Feature network topology를 자동으로 design하는 방식을 사용하여 효과를 보았다.

 - NAS-FPN은 GPU로 처리할 때, 탐색에(Network architecture) 몇 천시간이 소요되며 결과 네트워크는 일반적이지 않아서 해석하기 힘들다.

 - 본 논문에서는 multi-scale feature fusion을 optimization 시키기 위해서 좀 더 직관적이고 정형화된 방법을 사용한다.

 - **Model scaling**

 - 지금까지는 Acc 향상을 위해, 좀 더 큰 Backbone을 사용했다.

 - 여기서는 EfficientNet에 나온 Compound scaling과 유사한 방법을 사용하겠다.

## BiFPN - Problem Formulation

<img src="/assets/image/EfficientDet/figure2.PNG" width="450px" height="300px" title="title" alt="title">

 - BiFPN : Efficient bidirectional cross-scale connections and weighted feature fusion.

 - Multi-scale features는 $(\overrightarrow{P}_{l_1}^{in}, P_{l_2}^{in}, ...)$와 같이 나타낸다.

 - 각 $P_{L_I}^{in}$은 Level $l_i$의 feature를 나타낸다.

 - 여기서 목표는 여러 Input을 잘 엮어서 Output을 만들어낼 수 있는 Transformation $f$를 찾는 것이다. 물론 식으로 나타내면 $\overrightarrow{P}_out = f(\overrightarrow{P}_{in})$이 된다.

 - figure 2 (a)는 FPN을 나타내는데, 여기서 LEVEL 3-7은 $\overrightarrow{P}^{in} = (P_3^{in}, ..., P_7^{in})$으로 나타낼 수 있다.

 - 또한 각 $P_{i}^{in}$은 resolution이 $\frac{1}{2^i}$를 뜻한다.

 - 예를 들면, Input이 640x640이라고 할 때, LEVEL 3이면 $640/2^3 = 80$이므로 한 변이 80이 되는 것이다. 또한 LEVEL 7이면 한 변의 길이가 5가 되겠지.

 - 아래 공식처럼 FPN의 각 LEVEL의 Output이 나타난다.

<img src="/assets/image/EfficientDet/two.PNG" width="450px" height="300px" title="title" alt="title">

 - 여기서 Resize는 Down-sampling이나 Up-sampling을, Conv는 Convolution layer를 의미한다.

## BiFPN - Cross Scale Connections

 - Figure 2에서도 확인할 수 있듯이, FPN은 단 방향으로만 information flow를 구축한다.

 - PANet은 여기에 Bottom-Up information flow를 추가했다.

 - NAS-FPN은 GPU 기준 몇천시간이 소요되고, Output이 일반적인 형태가 아니기 때문에 네트워크를 해석하거나 변형하기가 힘들다.

 - PANet은 FPN이나 NAS-FPN보다 ACC가 높지만, efficiency가 좋지 않다.

 - Efficiency를 높이기 위해서, PANet에서 Input이 하나 였던, 즉 가장 모서리 부분에 위치했던 Node들을 뺐다.

 - 또한 Cost를 많이 높이지 않으면서도 더 많은 features를 고려하기 위해서, extra-edge를 추가했다.

 - 마지막으로, 좀 더 high-level의 feature fusion을 위해서 BiFPN Layer를 여러번 중첩시켰다.

## BiFPN - Weighted Feature Fusion

 - Input features의 Resolution이 전부 다르다는 사실이 output feature에 동등하게 반영되지 않는다는 사실로 나타남에따라, 각 Input에 Weight를 주어서 각 Input의 importance를 학습하게 했다. (Input의 종류에 따라 Detection 성능을 높이기 위해 Input을 scaling하는 방향으로 학습되기를 기대한 듯 함.)

# Unbounded fusion

  - $O = \Sigma_{i}{w_i} * I_i$

  - $w_i$가 Learnable weights.

  - Input에 Weight를 곱하는식으로 디자인했다. Scaling이 computational cost를 낮추면서도 ACC를 높일 수 있어서 선택했다고 한다.

  - $w_i$는 scalar, vector, multi-dimensional tensor 전부 될 수 있다. (뒤에 나올 BiFPN의 Channel을 증가시키는 부분이 여기인 듯)

  - 하지만 Bound되지 않은 scaling은 Training에 stability를 저하시키므로, weight를 Normalization해주기로 함.

# Softmax-based fusion

  - $O = \Sigma_{i}\frac{e^{w_i}}{\Sigma_{j}{e^{w_j}}}*I_i$

  - 일반적인 Softmax이다. 이거 쓰려했는데 너무 느려서 다른 방법을 썼다고 한다.

# Fast normalized fusion

  - $O = \Sigma_{i}{\frac{w_i}{\epsilon+\Sigma_{j}w_j}}*I_i$

  - Normalize 전에 Relu를 적용시켜 0 이상인 부분만 적용.

  - Softmax에 비해서 30% 빠르다고 한다.

# Integrated feature fusion

    <img src="/assets/image/EfficientDet/three.PNG" width="450px" height="300px" title="title" alt="title">
    
  - LEVEL 6에서의 Feature fusion을 예시로 설명.

  - $P_6^{td}$는 Top-down path에서의 feature fusion 결과.

  - $P_6^{out}$은 Bottom-up path에서의 feature fusion 결과.

  - 각 Node들 간의 Connection의 Input마다 Weights가 할당되어 있는 것을 확인 가능.

  - Efficiency 향상을 위해 depthwise separable convolution을 사용했고, 그 후 BN 및 Activation function을 적용했다.

## EfficientDet

  <img src="/assets/image/EfficientDet/figure3.PNG" width="450px" height="300px" title="title" alt="title">

## EfficientDet Architecture

  - ImageNet Data로 훈련된 EfficientNet을 Backbone으로 사용.

  - 마지막 class and box network weights는 각 LEVEL Features에 대해서 공유하는 방식으로 사용. (각 LEVEL Feature에서 Network output까지 어떻게 이어지는지 확인 필요.)

## Compound scaling

  - Compound coefficient $\pi$를 Backbone network, BiFPN network, class/box network, resolution에 공통적으로 적용.

  - Grid search는 너무 시간이 많이 걸려서, Heuristic하게 Parameter를 결정하기로 함.

## Backbone network

  - EfficientNet B0-B6까지의 coefficient를 동일하게 적용하기로 함.

## BiFPN network

  - {1.2, 1.25, 1.3, 1.35, 1.4, 1.45} 중에서 Grid search를 통해 $\pi$를 결정. 1.35가 베스트.

  - BiFPN의 Channel의 결정은 $W_{bifpn} = 64 * (1.35^\pi)$

  - BiFPN의 Depth의 결정은 $D_{bifpn} = 3 + \pi$

## Box/class prediction network

  - Network의 Channel은 BiFPN의 Channel과 동일하게 유지.

  - Depth는 $D_{box} = D_{class} = 3 + [\pi /3]$

## Input Image Resolution

  - BiFPN 단계에서 $2^n$으로 사이즈가 줄어드므로, Scaling 역시 그를 고려해서 해줌.

  - $R_{input} = 512 + \pi * 128$

  - EfficientDet D0-D7은 각각 $\pi$가 0부터 7까지 할당되었을 때를 의미한다.

  - Table 1은 Scaling configuration을 나타낸다.

  <img src="/assets/image/EfficientDet/table1.PNG" width="450px" height="300px" title="title" alt="title">
  
## Experiments

  <img src="/assets/image/EfficientDet/table2.PNG" width="450px" height="300px" title="title" alt="title">
