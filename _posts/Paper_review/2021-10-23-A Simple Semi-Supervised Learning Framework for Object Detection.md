---
title: "[Object detection] A Simple Semi-Supervised Learning Framework for Object Detection"
date: 2021-10-23 21:05:00
author: Leechanhyuk
categories: Paper_review
tags: Computer_vision
use_math: true
cover: "/assets/instacode.png"
toc: true
---

* * *

**A Simple Semi-Supervised Learning Framework for Object Detection**

<img src="/assets/image/soft_teacher/front.png" width="600px" height="450px" title="title" alt="title">

# 0. Abstract

  - Semi-supervised learning (SSL) 은 model의 performance를 올릴 가능성이 크다.

  - 현재까지 SSL은 대부분 Image classification에 관해서만 이루어졌다.

  - STAC를 제안하고, 이는 Object detection task에서 활용되며, 주로 high-confident pseudo-label 및 data augmentation을 기반으로 동작한다.

  - COCO Dataset 및 VOC07 Dataset을 활용하여 평가하였고, 기존 모델의 AP(Average Precision)을 크게 향상시켰다.

  <img src="/assets/image/soft_teacher/figure1.png" width="600px" height="450px" title="title" alt="title">

# 1. Introduction

  - 기존의 SSL은 대부분 "Consistency-based self training", Pseudo-label 및 stochastic data augmentation을 이용해서 훈련하는 방법, 을 이용해서 훈련했다.

  - 따라서, SSL은 Data augmentation의 발전에 큰 영향을 받아왔고, 최근의 Rand augmentation or CTAugment 등에도 큰 영향을 받았다.

  - Pseudo-label을 사용한다는 것이, detection에는 적용하기 힘들었기에, 대부분의 SSL은 Classification에 적용되었다.

  - 따라서, detection task는 대부분 충분한 annotation data가 존재하는 경우에만 수행되었다.

  - 따라서 STAC에서는 pseudo-label 및 consistency regularization (about Data augmentation)을 활용하여, 2-stage 구조로 네트워크를 design했다 (Noisy student에 영향을 받음).

  - 첫 번째 단계에서는, detector를 labeled-image로 훈련시키고, 해당 모델에 unlabeled-image를 feed 시킨 후에 NMS 거치고, 그 중에서도 High threshold 조건을 만족시키는 bounding box만 추려냈다.

  - 두 번째 단계에서는, labeled-image 및 pseudo-labeled-image를 섞고, strong-data-augmentation도 적용하여 detector 모델을 새롭게 훈련시킨다.

  - 또한, 두 번째 단계에서 적용되는 data-augmentation은 global-color-transformation, global-box level geometric transformation을 새롭게 제안하고 적용했다.

  - 이 모델은 COCO Dataset 및 PASCAL VOC Dataset을 통해 평가했다.

  - 평가는 labeled-image를 1, 2, 5, 10% 사용했을 때의 경우로 나누어서 진행했다.

  - VOC07의 Trainval을 labeled-dataset으로, VOC12 및 MS-COCO를 unlabeled-dataset으로 활용했다.

  <img src="/assets/image/soft_teacher/figure2.png" width="600px" height="450px" title="title" alt="title">

  - Curiosity) 기존 Teacher model보다 AP가 높나?

  - Idea) 결국, 여기서는 classification SSL에서 사용되던 방법을 그대로 가져와서, NMS + High threshold filtering을 거친게 모델에서는 주요한 contribution인거지?

  - ## 1.1. Contribution

    - State-of-the-art SSL Strategy 및 Data augmentation을 통한 regularization 방식을 적용하여 STAC Network를 만들었다.

    - 두 가지의 hyperparameter만 사용했다. pseudo-label을 결정지을 confidence threshold와 pseudo-training-loss의 반영 비율을 결정지을 weight value.

    - SSL을 평가하기 위한, 새로운 Metric을 제안했다.

# 2. Related work

  - 생략

# 3. Methodology

  - ## 3.1. Background: Unsupervised Loss in SSL

    - Unsupervised loss를 정하는 것은, SSL에서는 매우 중요하다.

    - 아래의 식은 기존의 image classification SSL에서 자주 사용되던 K-way classification에 관련된 식이다.

    <img src="/assets/image/soft_teacher/equation1.png" width="450px" height="300px" title="title" alt="title">

    - 식 중, x는 image, w(x)는 x input에 대한 weight value, l은 q와 p의 거리 (L2-Distance 혹은 cross-entropy로 측정), p는 prediction value, q는 prediction target, $\theta$는 prediction시 사용된 hyperparameter를 뜻한다.

    - 즉, 보통의 방식 (결과값과 Ground truth값의 비교)로 학습하는데, L2-Norm이나 cross-entropy 쓰고, input에 따라서 weight 주겠다! 이거다.

    - 해당 loss에서 prediction과 weight는 아래 식으로 규정된다.
    
    <img src="/assets/image/soft_teacher/equation2.png" width="450px" height="300px" title="title" alt="title">

    - 즉, prediction target은 결과 class 중에서 max값을 갖는 class를 사용하고, weight는 threshold보다 높으면 1을 아니면 0을 줘서, threshold 이상의 sample만 사용하겠다는 것이다.

    - 정리하자면, 하나의 모델로 훈련 및 pseudo-labeling을 진행하는데, pseudo-labeling은 weak augmentation을 적용한 결과, 그리고 훈련에서는 더 강한 augmentation을 적용하여 훈련시키는 것.

    - Noisy student가 위의 architecture와 다른 점은, 하나의 네트워크가 아닌 pseudo-label을 만드는데 teacher network를 사용했다는 것.

  - ## 3.2. STAC: SSL for object detection.

    - STAC, Self-Training and the Augmentation driven Consistency Regularization, 를 제안한다.

    - STAC의 훈련 과정은 아래와 같다.

    <img src="/assets/image/soft_teacher/process.png" width="450px" height="300px" title="title" alt="title">

    - ### 3.2.1. Training a teacher model

      - Teacher network로는 Faster-R-CNN을 사용한다.

      - Supervised-Unsupervised loss를 모두 반영하기 위해서, 아래의 식을 통해서 학습시켰다.

      <img src="/assets/image/soft_teacher/equation3.png" width="450px" height="300px" title="title" alt="title">

      - 여기서 i는 mini-batch 내의 anchor를 의미하고, p는 anchor의 class, t는 anchor의 regression value를 의미한다.

    - ### 3.2.2. Generating pseudo label

      - Teacher model의 output에 NMS를 적용해서 Pseudo-label을 만들어냈다.

      - NMS를 써서 비슷한 Bounding box를 제거하니까, 좀 더 이점이 있지만 그것만으로는 부족해서 threshold를 적용해서, 좀 더 confidence가 높은 sample만 반영시켰다.

    - ### 3.2.3. Unsupervised loss

      - Loss는 아래 식과 같다.

      <img src="/assets/image/soft_teacher/equation4.png" width="450px" height="300px" title="title" alt="title">
      
      - 즉, supervised learning에서는 data augmentation 없이, weight 없이 반영 + unsupervised learning에서는 data augmentation + weight를 반영하여 학습하겠다는 것.

      - 또한, confidence threshold는 0.9로, unsupervised weight는 [1, 2]로 정하는게 좋았다고 한다.

    - ### 3.2.4. Data augmentation strategy

      - Image classification을 위한 data augmentation 방식은 많이 만들어졌는데, detection을 위한건 잘 없다.

      - 저자는, RandAugmentation을 box-level transformation까지 search space를 확장시켜서 반영했다. Cutout과 함께.

      - 반영한 data augmentation 방법은 아래와 같다.

      <img src="/assets/image/soft_teacher/augmentation.png" width="450px" height="300px" title="title" alt="title">

      - 실제로는, Global color transformation + (Global geometric transformation + Box-level transformation 중 하나) + Cutout을 적용했다.

      <img src="/assets/image/soft_teacher/figure3.png" width="450px" height="300px" title="title" alt="title">

# 4. Experiments

  <img src="/assets/image/soft_teacher/table1.png" width="450px" height="300px" title="title" alt="title">

  <img src="/assets/image/soft_teacher/table2.png" width="450px" height="300px" title="title" alt="title">

  <img src="/assets/image/soft_teacher/table3.png" width="450px" height="300px" title="title" alt="title">

  <img src="/assets/image/soft_teacher/table4.png" width="450px" height="300px" title="title" alt="title">
  
  <img src="/assets/image/soft_teacher/table5.png" width="450px" height="300px" title="title" alt="title">

  <img src="/assets/image/soft_teacher/figure4.png" width="450px" height="300px" title="title" alt="title">

  <img src="/assets/image/soft_teacher/figure5.png" width="450px" height="300px" title="title" alt="title">