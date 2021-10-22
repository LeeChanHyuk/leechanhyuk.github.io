---
title: "[Object detection] End-to-End Semi-Supervised Object Detection with Soft Teacher"
date: 2021-10-13 21:05:00
author: Leechanhyuk
categories: Paper_review
tags: Computer_vision
use_math: true
cover: "/assets/instacode.png"
toc: true
---

* * *

**End-to-End Semi-Supervised Object Detection with Soft Teacher**

<img src="/assets/image/soft_teacher/front.png" width="600px" height="450px" title="title" alt="title">

# Abstract

 - End-to-End training과 pseudo labels은 서로 상호보완관게에 있다.

 - 본 논문에서는 두 가지 방법을 사용해 두 가지의 주요 알고리즘을 제안한다.

   1. Soft-teacher mechanism (un-labeled bounding box에 classification score를 통해 weight하는 방법)

   2. Box-jittering approach (For bounding box regression)

 - 제안 방법을 사용해서 기존 네트워크 들의 성능을 성공적으로 향상시켰다.

 - Object365 pre-trained weight를 사용하여 더 높은 mAP를 달성했다.

# Introduction

 - ImageNet은 Deep learning의 인기 열풍에 큰 역할을 했지만, label을 획득하는 작업은 bottle-neck이었다.

 - 지금까지의 pseudo-label 방식은, label이 있는 데이터로 훈련시킨 후에 그 모델로 label이 없는 데이터의 label을 획득하고, 해당 데이터를 사용해 model의 퀄리티를 올리는 방식이었다.

 - ## 제안 방식 하나

    - 본 논문에서는 label이 있는 데이터로 모델을 훈련 시키면서, 다른 모델로 새롭게 label된 데이터를 매 iteration마다 사용하는 방법을 제안한다. (매 batch에 label, un-labeled data는 섞여있다.)

    - 훈련 시킬 모델을 student 모델, pseudo-label을 만들 모델을 teacher 모델이라고 할 때, teacher 모델은 student 모델의 EMA이다. (? 의미 파악 필요)

    - 두 모델은, Training을 거듭해 나가면서 상호 보완적으로 성장하여 큰 효과를 낸다.

 - ## 제안 방식 둘

    - 기존의 pseudo label 방식이 단순히 모델 훈련시에 pseudo-label을 던져주는 식이었다면, 여기서는 teacher-network가 훈련 과정에 직접적으로 개입한다.

    - 이를 Soft-teacher 방식이라고 하며, 이는 모든 bounding-box를 back-ground와 fore-ground로 나누는 것을 의미한다.

    - 나누는 기준은 아래와 같다.
    
      1. 단순히 특정 threshold를 정해두는 것 보다, detection score를 reliability measure로 사용하여 얼마나 배경에 부합하는지를 판단하여 적용하는 방식을 사용했다.

      2. 또한 teacher 모델이 예측한 bound box들의 variance를 reliability measure로 사용하는 방식도 있다.

 - 본 논문에서 제시한 방법을 사용해서, Transformer, Detection network (Faster-R-CNN), Classification network (ResNet) 모두 성능이 향상됨을 확인했다.

# Related work

  - ## Semi-supervised learning in image classification

    - ### Consistency based image classification

      - 이 방식은 unlabeled-images의 prediction이 labeled-image의 prediction과 같다는 것을 학습시켜, regularization loss를 만들어내는 방법이다.

      - 이 방식에는 GAN이나 Augmentation 등이 속한다.

      -  In [29], they develop [11] by ensembling the model itself instead of the model prediction, the so-called exponential mean average (EMA) of the student model. 의미 파악 필요.

    - ### pseudo label based image classification

      - 이 방식은 unlabeled-data에 대해서 우선 classification model로 1차 분류를 하여 pseudo label을 붙이고, 이 pseudo label로 detector를 학습시킨다..

      - 따라서, 일반적인 object detection 방식처럼 탐지 구간을 fore-ground / back-ground로 나누고 regression을 학습하지 않는다.

      - 최근의 연구로 인해, pseudo label을 만드는 것은 weak-augmentation을, detection model을 훈련시키는 것은 strong-augmentation을 적용하는 것이 좋다고 알려졌다.  

  - ## Semi-supervised learning in object detection

    -  Object detection에서도 consistency 및 pseudo label 방식으로 나누어진다 (2-Stage).

    -  다른 방식과는 다르게, 본 논문은 1-stage end-to-end 방식을 사용한다.  

  - ## Object detection

    - One-stage detector과 TWo-stage detector로 나누어진다.

    - 본 논문에서 제안하는 방법은 두 가지 detector에 모두 적용 가능하다.

    - 하지만 지난 연구들과의 공평한비교를 위해 Faster-R-CNN으로 성능을 측정했다.  

# Methodology

  <img src="/assets/image/soft_teacher/figure2.png" width="600px" height="450px" title="title" alt="title">

  - Figure 2는 end-to-end training의 전체적인 과정을 나타낸다.

  - 전체적인 과정을 글로 나타내자면 아래와 같다.

  - ## 전체 과정
      
    1. 특정 비율로 labeled-image와 unlabeled-image를 불러온다.

    2. labeled-image는 student model에 input으로 들어와서 일반적인 과정으로 loss를 구한다.

    3. unlabeled-image는 weakly augmented되어 teacher model에 feed되어 NMS(Non-Maximum-Supression)를 거친 pseudo-box의 class score와 variance를 도출한다.

    4. unlabeled-image는 strongly augmented 되어 student model에 feed되고, pseudo-label과 비교후에 loss를 구한다.

    5. 훈련이 끝난 student model에 EMA를 적용하여 teacher model에 더해준다.

    - 조금 이해가 안되는 점은, 왜 unlabeled-image에만 strong augmentation을 적용하고, labeled-image에는 weak augmentation을 적용하는거지? reference 논문에 나와있는 것 같긴 하다.

  - ## End-to-End Pseudo-Labeling Framework

    - 위의 전체 과정에 따라 훈련을 진행한다.

    - 전체 loss는 아래 식 1과 같다. ($L_s = supervision loss /  \alpha = weight constant for unsupervised loss / L_u = unsupervised loss$)

    <img src="/assets/image/soft_teacher/equation1.png" width="400px" height="300px" title="title" alt="title">

    - 각 loss에 참여한 data sample은 미리 규정한 값에 따라 달라지기 때문에, 그 sample의 비율에 맞춰서 나누어 loss에 반영해준다.
    
    <img src="/assets/image/soft_teacher/equation2.png" width="400px" height="300px" title="title" alt="title">

    - Teacher model에 unlabeled-image가 feed 되고 나서, NMS를 거치고 나서도 background sample이 남아있을 수 있기 때문에, 특정 threshold 보다 높은 sample만 학습에 참여할 수 있다.

  - ## Soft Teacher

    <img src="/assets/image/soft_teacher/figure3.png" width="400px" height="300px" title="title" alt="title">

    - Detector의 performance는 pseudo label에 크게 의존한다.

    - 실제로도, pseudo label을 만들 때 규정하는 background/foreground를 가르는 threshold를 높게 설정했을 때, 결과가 더 좋게 도출되었다.

    - threshold를 0.9로 설정했을 때, precision은 높았지만 recall은 떨어졌다.

    - 이 경우에 만약, pseudo label이 잘못되어서 positive를 negative로 분류하는 경우에는 performance를 낮출 수 있다. (? 왜 detection threshold를 0.9로 설정한 경우에만 이 사항이 해당되는거지? 왜 굳이 경우를 정하지?)

    - 이를 해결하기 위해서, soft-teacher, 즉 richer information을 더 강화하는 네트워크를 사용한다. 이는 pseudo bounding box를 background 및 foreground로 분류한 loss의 합으로 구성된다.

    <img src="/assets/image/soft_teacher/equation4.png" width="400px" height="300px" title="title" alt="title">

    - 이 부분이 좀 햇갈렸는데, 이건 한 이미지 내에서도 여러개의 prediction을 진행할 것이고, 그 중에는 foreground도 background도 있을 텐데, 그 모든 부분을 반영하되, background 부분은 좀 weight를 줘서 loss에 반영하자는 의미로 보인다.

    - 여기서 weight는 아래 식과 같이 규정되며, 이는 배경이 loss에 참여하는 합이 1이 되게 함으로써, positive sample 하나와 같은 양의 정도만 background를 통해서 배우겠다는 것으로 보인다.

    <img src="/assets/image/soft_teacher/equation5.png" width="400px" height="300px" title="title" alt="title">

    - 해당 식의 $r_j$는 reliability score인데, 이는 teacher model의 detection score를 가지고 판단한다. (경험적으로 이 방법을 선택 후 사용했다고 기술되어 있음.)

  - ## Box jittering

    - Figure 3-(b)를 보면, foreground score와 localization score가 항상 비례하는 것은 아니라는 걸 알 수 있다.

    - 이것은 pseudo box의 detection score를 localization에 사용하는 것은 best는 아니란 것을 의미한다.

    - 따라서, 신뢰가능한 reliability measure를 얻기 위해서, 우리는 teacher-network의 prediction 값인 $b_i$를 다시 teacher-network에 넣어서 $\hat{b_{i}}$을 만든다. 본 과정은 아래 식과 같다.

    <img src="/assets/image/soft_teacher/equation7.png" width="400px" height="300px" title="title" alt="title">

    - 이 과정을 여러번 반복하고, 얻어진 bounding box의 variance를 reliability measure로 사용한다.

    <img src="/assets/image/soft_teacher/equation8.png" width="400px" height="300px" title="title" alt="title">
    
    <img src="/assets/image/soft_teacher/equation9.png" width="400px" height="300px" title="title" alt="title">

    - where $σ_k$ is the standard derivation of the k-th coordinate of the refined jittered boxes set $\hat{b_{i,j}}$, $\hat{σ_k}$ is the normalized $σ_k$, $h(b_i)$ and $w(b_i)$ represent the height and width of box candidate bi, respectively.

    - 물론, 이 measure는 작을 수록 여러번의 prediction이 겹친다는거니까 reliability score가 높다.

    - 하지만 본 과정을 training 중에 계속 반복하는 것은 계산량이 너무 커져서, 실제로는 detection score가 0.5 이상인 sample만 진행해서, 관련 속도가 83% 향상되었다.(Background sample이 대다수일거니까 계산량이 많이 줄어드는 것!)

    - figure 3-(c)는 해당 reliable measure과 IoU간의 상관관계를 통해, predicted bounding box의 variance를 통한 추론이 합리적임을 보여준다.

    - 따라서, 정리하자면 bounding box regression은 detection threshold가 0.5 이상인 sample만 사용하니까, 결국은 foreground와 pseudo box를 통해 loss를 계산한다.

    <img src="/assets/image/soft_teacher/equation10.png" width="400px" height="300px" title="title" alt="title">

    - Total, pseudo loss는 아래와 같다.

    <img src="/assets/image/soft_teacher/equation11.png" width="400px" height="300px" title="title" alt="title">

# Experiments

  - <img src="/assets/image/soft_teacher/table2.png" width="400px" height="300px" title="title" alt="title">

  - <img src="/assets/image/soft_teacher/table2.png" width="400px" height="300px" title="title" alt="title">
  
  - <img src="/assets/image/soft_teacher/table3.png" width="400px" height="300px" title="title" alt="title">

  - <img src="/assets/image/soft_teacher/figure4.png" width="400px" height="300px" title="title" alt="title">
  
  - <img src="/assets/image/soft_teacher/table4.png" width="400px" height="300px" title="title" alt="title">

  - <img src="/assets/image/soft_teacher/table5.png" width="400px" height="300px" title="title" alt="title">

  - <img src="/assets/image/soft_teacher/table6.png" width="400px" height="300px" title="title" alt="title">

  - <img src="/assets/image/soft_teacher/table7.png" width="400px" height="300px" title="title" alt="title">

  - <img src="/assets/image/soft_teacher/table8.png" width="400px" height="300px" title="title" alt="title">

  - <img src="/assets/image/soft_teacher/table9.png" width="400px" height="300px" title="title" alt="title">

  - <img src="/assets/image/soft_teacher/table10.png" width="400px" height="300px" title="title" alt="title">