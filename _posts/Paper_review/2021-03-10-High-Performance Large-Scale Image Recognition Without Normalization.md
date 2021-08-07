---
title: "[Image recognition] High-Performance Large-Scale Image Recognition Without Normalization Review"
date: 2021-03-10 21:05:00
author: Leechanhyuk
categories: paper_Review
tags: Computer_Vision Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

<img src="/assets/image/High-performance/frontdoor.PNG" width="450px" height="300px" title="title" alt="title">

<img src="/assets/image/High-performance/figure1.png" width="450px" height="300px" title="title" alt="title">

* * *

**High-Performance Large-Scale Image Recognition Without Normalization논문 리뷰**

**본 논문리뷰는 논문 구성을 따르고 있지만, 단지 제 생각을 구성에 맞춰서 적은 것이지**

**실제 논문을 요약하여 적은 것이 아닙니다. 착오 없으시길 바랍니다.**

* * *

## 논문 관련 정보

 - 추가 예정

## Abstract

 - Batch Normalization(BN)은 대부분의 분류 문제에서 필수적이었다.

 - Batch-size에 Dependency가 있고, Example마다 interaction이 있다. (*Inference마다 결과가 다르다는걸 의미하는 건가?*)

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

 - 지금껏 BN을 대체할만한 논문들은 나왔었지만, Test accuracy가 낮거나, Computing cost가 높거나 하는 단점들이 있었다. && Batch size가 너무 크면 unstable 했다.

 - 매우 deep한 resnet은 residual branch의 hidden activation(*?*)을 억제하는 방법으로 Normalize 없이도 성공하였다. 이는 learnable scalar로 구현 가능하다.

## Contribution

 - AGC(Adaptive Gradient Clipping)을 사용해 큰 batch size 및 stronger data augmentations를 가능하게 했다.

 - Normalizer-Free-ResNets(NFNets), sets new state-of-the-art validatijon accuracies on ImageNet competition (86.5%)

 - Through the fine-tuning, the model achieves 89.2%, top-1 accuracy after fine-tuning

## Understanding of batch normalization

 - Batch normalization downscales hidden activation on the residual branch. 
 
   - biases the signal towards the skip path (For efficient optimization)
   
   - 최적화를 돕는다.

 - Batch normalization eliminates mean-shift

   - ReLU와 같은 Activation Function은 평균이 0이 아니다.

   - 결과적으로 Input Feature간의 내적이 0에 가까울 지라도 높은 성능의 독립된 Non-Linearity 직후의 Training Data들의 Normalizer-Free Resnet Activation간의 내적은 크고 양의 값이다.
    (평균이 0이 아니므로 내적하면 0이 아닌 값이 아닌 양수의 값이 나온다는 뜻)

   - 이러한 문제는 네트워크 깊이가 증가함에 따라 폭합적으로 작용하며 네트워크의 깊이에 비례하여 다른 Training Data간의 Activation에     'Meah-Shift'를 초래한다. 이는 Deep Network가 초기화시 모든 데이터를 단일 Label로 예측하는 문제를 야기한다.

   - Batch Normalization은 Activation의 평균을 0로 만들고 mean shift를 제거한다.

- Batch normalization has a regularizing effect

- Batch Normalization allows efficient large-batch training

  - Batch normalization은 Loss의 Landscape를 smooth하게 해준다.

  - 큰 Batch size를 사용하여 학습시에는, 이러한 특징이 필요하다.

## Towards Removing Batch Normalization

 - 본 연구에서는 Normalizer-Free Resnet(NF-ResNEts)의 구조를 차용한다. NF-Resnet은 $h_{i+1}=h_{i}+\alpha f_{i}(h_{i}/\beta_{i})$형태의 Residual Bock을 사용한다.

 <img src="/assets/image/High-performance/residual.png" width="450px" height="300px" title="title" alt="title">

 - **Scaled Weight Standardization (Qiao et al., 2019)**

    - Mean-Shift를 없에기 위하여 가중치를 정규화 해준다.

    <img src="/assets/image/High-performance/a.png" width="450px" height="300px" title="title" alt="title">

    - 활성화 함수도 $\gamma$ 로 Scaled 된다.

 - 이러한 시도들은 배치 사이즈가 작을 때는 배치 정규화 적용한 것에 비해 성능이 좋았지만, 배치가 클때는(e.g 4096 이상) 배치 정규화 성능을 따라 잡기 힘들었다.

## Adaptive Gradient Clipping for Efficient Large-Batch Training
 
 - 배치 정규화를 사용하지 않고도, 큰 사이즈의 배치를 사용하기 위해 Gradient Clipping 전략을 참고 하였다.

 - 조건이 좋지 못한 Loss 함수의 경우(*경사가 거친 모양을 뜻하는 듯*) 혹은 배치사이즈가 클때는 최적의 lr은 maximum stable learning rate에 의해 제약 사항이 생긴다

   - loss의 landscape가 거칠때는 learning rate가 높으면, global minimum을 지나쳐서 local minimum에 빠질 수 있기 때문에 적당히 stable한 learning rate가 필요하고, batch size가 클 때에는 gradient가 안정적이지만 크게 형성되지 않아 learning rate를 올리는 것이 필요한데, 이 때 stable하면서도 효과적인 lr을 찾는게 힘들다..

 - Gradient Clipping이 배치 사이즈를 늘리면서 학습이 가능하게는 하지만, 학습의 안정성은 다소 떨어진다 (Threshold, 모델 깊이, lr 등에 민감함)

## Gradient Clipping

 <img src="/assets/image/High-performance/gradient_clipping.png" width="450px" height="300px" title="title" alt="title">

## Adaptive Gradient Clipping
    
  $$\begin{aligned}&W^{l} \in R^{N \times M}: l^{t h} \text { 번째 계층의 가중치 행렬 }\\&G^{l} \in R^{N \times M}: W^{l} \text { 에 대응하는 기울기 }\\&\text { || } \cdot \|_{F} \text { 는 Frobenius norm }\left(L^{2}\right)\\&\left\|W^{l}\right\|_{F}=\sqrt{\sum_{i}^{N} \sum_{j}^{M}\left(W_{i, j}^{l}\right)^{2}}\end{aligned}$$

  - 비율 $\frac{\left \| G^l \right \|}{\left \| W^l \right \|}$이 경사 하강법 Step에서 원래 가중치 W를 얼마나 변경하는지를 나타 내는 단위가 될 수 있다는 점에 영감을 얻었다.

  - 예를 들어 경사 하강 법을 이용해 (모멘텀 없이) 학습 시킬 경우 $\frac{\left \| \Delta W^l \right \|}{\left \| W^l \right \|}$= h$\frac{\left \| G^l \right \|}{\left \| W^l \right \|}$ 다음과 같이 나타낼 수 있고, $l^{th}$ 번째 계층의 가중치 업데이트는 다음과 같이 주어진다.

    $\delta{W^l} = -hG^l$, h는 learning rate.

  - $l^{th}$ 번째 계층의 기울기는 다음과 같이 Clipping 될 수 있다.

      $$G_{i}^{\ell}\rightarrow\left\{\begin{array}{ll}\lambda \frac{\left\|W_{i}^{\ell}\right\|_{F}^{\star}}{\left\|G_{i}^{\ell}\right\|_{F}} G_{i}^{\ell} & \text { if } \frac{\left\|G_{i}^{\ell}\right\|_{F}}{\left\|W_{i}^{\ell}\right\|_{F}^{\star}}>\lambda \\G_{i}^{\ell} & \text { otherwise. }\end{array}\right$$

  - 하이퍼 파라미터 $\lambda$는 다음과 같이 정의한다.

 <img src="/assets/image/High-performance/b.png" width="450px" height="300px" title="title" alt="title">

  - 최적의 파라미터 값은 옵티마이저, 배치 사이즈에 따라 조금씩 다르다. 하지만, 큰 배치의 경우는 값이 작아야 한다

  - AGC를 이용하면, 큰 배치사이즈(e.g. 4096) 과 데이터 확장(Augmentation)에도 강인함을 보였다.

  - 기존의 NF-ResNet은 배치 사이즈가 커지면, 정확도가 확 떨어지지만, AGC를 더하면 정확도가 유지 된다.

 <img src="/assets/image/High-performance/c.png" width="450px" height="300px" title="title" alt="title">

## Normalizer-Free Architectures with Improved Accuracy and Training Speed

 - 이전 챕터에서 우리는 Adaptive Gradient Clipping 방법이 큰 Batch size를 사용할 수 있게 해주고, 또 strong data augmentation 기법을 사용할 수 있도록 해주는 것을 확인했다.

 - Adaptive Gradient Clipping을 SOTA 모델에 적용해보기 위해서, 서칭해본 결과 SE-ResNeXt-D model (Xie et al 2017, Hu et al., 2018; He et al., 2019) with GELU activations을 사용해보기로 했다.

 - 조건 1. 3x3 conv에서 group width를 128로 설정했다.

 - 조건 2. 각 Stage마다 균일하게 depth를 조정했다.

 <img src="/assets/image/High-performance/depth.png" width="450px" height="300px" title="title" alt="title">

 - 조건 3. Conv layer channel을 균일하게 조정했다. [256, 512, 1024, 2048]

## Experiments

 <img src="/assets/image/High-performance/experiment.png" width="450px" height="300px" title="title" alt="title">

## Conclusion

- 배치 정규화를 적용하지 않고도, 큰 배치에서 배치 정규화를 적용한 모델의 성능을 뛰어 넘는 최초의 모델이다.

- 배치 정규화 적용한 모델과 성능은 비슷하면서도빠르게 학습할 수 있다.

- AGC 기법을 적용한 family models을 만들었다.

- 정규화 없는 모델이 (이미지 넷과 같은 모델을 학습 한 후) Finetuning 할때 되려 더 좋은 성능을 나타낸 다는 것을 보였다.






