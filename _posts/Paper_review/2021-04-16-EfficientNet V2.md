---
title: "[CV] EfficientNetV2 : Smaller Models and Faster Training"
date: 2021-04-16 21:05:00
author: Leechanhyuk
categories: paper_Review
tags: Computer_Vision Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

<img src="/assets/image/EfficientNetV2/FIRST.PNG" width="450px" height="300px" title="title" alt="title">

* * *

**EfficientNetV2 : Smaller Models and Faster Training Review**

 - 본 리뷰는 논문의 요약보다는 제가 이해한대로 구성하였습니다. 참고하시면 좋을 듯 합니다.

* * *

## Abstract

 - Parameter efficiency 및 Training speed를 Optimization 시키기 위해서 Training-aware neural architecture search 및 scaling 방법을 사용했다.

 - Training 과정에서 Image-up scaling 방법을 사용해서 학습 속도를 올렸지만, ACC의 저하가 있었다.

 - ACC 향상을 위해서 Progressive learning을 사용했다.(Adaptive Regularization) - Dropout이나 Data augmentation을 뜻함.

 - ImageNet 21K로 Pre-training을 하고 각종 데이터셋에 적용했을 때, ImageNet ILSVRC2012에서 87.3% ACC, TOP-1 ACC를 달성했다.

 - ViT보다 2.0% 더 높은 ACC이고, 학습속도는 5x - 11x 정도 더 빨랐다.

<img src="/assets/image/EfficientNetV2/figure1.PNG" width="450px" height="300px" title="title" alt="title">

## Introduction

 - Training efficiency는 최근에 많은 관심을 받아왔고, 각종 SOTA 모델에 적용되었지만 Parameter size가 너무 컸다. (Figure 1-(b) 참조)

 - 이 문제점 해결을 위해 Training-aware neural architecture search (NAS) & Scaling 방법을 사용했다.

 - EfficientNet에서 발견한 3가지 항목은

 1. 큰 이미지를 학습시키는 것은 오래걸린다.

 2. 초반 Layer 층에서 Depth-wise convolutions를 쓰는건 느리다.

 3. 매 Layer에서 scaling을 하는것은 sub-optimal(차선책)이다. 

 - 이러한 점 개선을 위해서 Fused-MBConv, Training aware NAS, scaling 방법을 사용했다.

 - 많은 연구들은 작은 이미지를 사용해서 점진적으로 늘리는 방법을 사용했지만, 모든 이미지 사이즈에 같은 Regularization을 사용했다.

 - 이미지 사이즈마다 다른 Regularization을 사용해야 효과가 좋다. (작은 이미지는 약한 규제를, 큰 이미지는 큰 규제를)

 - 부가 설명을 하자면 큰 이미지는 작은 이미지보다 많은 정보를 가지고 있으므로, 그만큼 Model complexity가 올라가야하고, 이것은 Overfitting을 초래할 수 있다.

 - 저자가 제안하는 방법은, Epoch 초기에는 작은 이미지, 약한 Regularization을 사용하고, 학습을 진행할 수록 더 큰 이미지, 더 강한 Regularization을 사용하는 방법이다.

 - 이미지 resizing은 progressive resizing (*HOWARD, 2018*)을 사용.
 
## Contribution

 - EfficientNetV2 outperform previous models in both training speed and parameter efficiency.

 - The improved method of progressive learning making speeds up on training and improving accuracy is proposed.

 - Faster 11x training speed, better 6.8x paramter eficiency than SOTA.

## Realted work

 - Training and Parameter efficiency (결론 : 소개 된 모델들은 Training speed와 Parameter efficiency를 동시에 잡지는 못했다.)

 - Progressive Training (결론 : Training speed는 올릴 수 있었으나, ACC는 잡지 못했다)

 - Neural Architecture Sarch (NAS) (결론 : 이전 모델들은 FLOPS 올리는데만 집중했지만, 우리는 Training speed와 Parameter efficiency를 둘다 잡았다.)

## EfficientNet V2 Architecture design

 1. Training with very large image sizes is slow

  - EfficientNet V1을 포함한 많은 모델에서 큰 이미지를 다루게 되는데, 이 때 Training 시간이 너무 길다.

  - FixRes에서는 Inference시 보다 더 작은 이미지를 가지고 훈련시켰다.

  - 작은 이미지로 Training 시킨 후, Fine-tuning 하는 식으로, 조금 더 높은 ACC를 얻기도 했다.

  - **제안하는 방법은, Training epochs에 따라서 점진적으로 Image size를 늘리는 방법이다.**

<img src="/assets/image/EfficientNetV2/table2.PNG" width="450px" height="300px" title="title" alt="title">

 
 2. Depthwise convolutions are slow in early layers

  - EfficientNet V1의 Bottle-nect중 또 하나는 extensive depthwise convolutions에서 온다.

  - Depthwise convolution은 일반적인 Convolution 보다 적은 파라미터 수와, FLOPS를 갖는다. 하지만 최신 기술에 비해서 유용하지는 못하다.

  - 따라서, 기존의 Depth-wise conv를 사용했던 MBConv외에 일반 Conv를 사용하는 Fused-MBConv를 사용했다.

  - 초반 1~3 Stage에서는 Fused-MBConv가 Training time을 줄여주는 효과를 발휘했지만, 전체에 다 적용했을 때에는 Training time 및 Parameter size를 증가시키는 영향을 나타냈다.

  - 따라서 MBConv 및 Fused-MBConv를 혼합한 방식을 사용한다.

<img src="/assets/image/EfficientNetV2/figure2.PNG" width="450px" height="300px" title="title" alt="title">

<img src="/assets/image/EfficientNetV2/table3.PNG" width="450px" height="300px" title="title" alt="title">

 3. Equally scaling up every stage is sub-optimal

  - EfficientNet V1의 모든 Layer를 uniform하게 scaling up 하는 것은 (Compound scaling) 모든 Stages에서 좋게 작용한 것은 아니다.

  - 따라서, Scaling up 할 Layer의 선정이 필요하다.

  - 너무 큰 이미지는 Training speed를 늦춘다.

  - Image Size의 최대치 또한 정해두어야 한다.

## EfficientNet V2 Architecture desing - Training-Aware NAS and Scaling

 1. NAS Search

  - For optimization of accuracy, parameter efficiency and training efficiency.

  - Stage based로 Search, MBConv 및 Fused-MBConv의 사용, Number of Layers, kernel size (3x3, 5x5), expansion ratio = {1, 4, 6}

  - 효율적인 최적 환경 탐색을 위해서, NAS에서 불필요한 탐색 영역을 제거함.

  - (1) EfficientNet V1에서 사용되지 않은 Pooling skip-ops에 대한 탐색 영역 제거.

  - (2) 선행 연구에서 이미 탐색 된 Backbone에 대한 Channel size 탐색 영역 제거.

  - 1000개의 모델을 downscale된 이미지 사이즈로 10 epochs 학습시켜 비교.

  - A = Model accuracy, S = normalized training step time, P = parameter size

  - $A * S^W * P^V$ 를 최적화하게 만듬. W는 -0.07, V = -0.05.

 2. EfficientNet V2 Architecture

<img src="/assets/image/EfficientNetV2/table4.PNG" width="450px" height="300px" title="title" alt="title">

  - EfficientNet V1과의 차이점 4가지

    1. Early-layers에서 MBConv 및 Fused-MBConv의 혼용

    2. MBConv에서 사용되는 expansion ratios를 작게 설정

    3. 3x3 kernel size를 사용. 단, Receptive field가 작아지는 것을 생각해서 Layer의 개수를 추가함. (더 뒷단에서는 Feature map size가 작아질 것이고, 그럼 상대적으로 같은 Kernal size라도 Receptive field의 크기가 커진다고 볼 수 있다.)

    4. 마지막 Stride-1 stage를 Parameter size 확장 및 Memory overhead의 가능성 증대로 제거함.

 3. EfficientNet V2 Scaling
 
  - Maximum inference image size를 480으로 규정함.

  - Later stage에 Layer를 추가함.

 4. Training speed comparison

<img src="/assets/image/EfficientNetV2/figure3.PNG" width="450px" height="300px" title="title" alt="title">

  - Figure 3는 Image size를 fix 시켜둔 상태에서 비교를 진행.

  - NFNET은 360 Epoch, 다른 모델들은 전부 350 Epoch를 Training. 이게 최적의 Epochs인지는 탐색 필요.

## Progressive Learning

 - 많은 모델들이 학습 중간에 Image size를 변경하는 방법을 사용했지만, 가끔 ACC Drop이 일어났다.

 - ACC Drop은 Uniform Regularization 때문이다.

 - 이걸 증명하기 위해서, Image size 및 data augmentation을 조정해서 Regularization과 Model complexity 및 Image size 간의 상관관계를 분석해보았다.

 - 결과적으로도 Model complexity나 Image size가 크면 강한 Regularization이 필요했다.

<img src="/assets/image/EfficientNetV2/table5.PNG" width="450px" height="300px" title="title" alt="title">

 - 따라서, 초기에는 작은 이미지, 약한 Regularzation을 사용하고, Epoch이 커질수록 더 큰 이미지, 강한 Regularization을 사용하기로 했다.

 - 아래 사진은 그를 나타낸다.

<img src="/assets/image/EfficientNetV2/figure4.PNG" width="450px" height="300px" title="title" alt="title">

 - Regularization 수치 증가는 Linear하게 이루어진다.

 - 가장 첫 Regularization은 Heuristical하게 정한다.

 - 마지막 Regularization은 Target value로 정한다.

 - 그 사이는 Interpolation해서 진행한다.

<img src="/assets/image/EfficientNetV2/algorithm 1.PNG" width="450px" height="300px" title="title" alt="title">

 - 각 스테이지는 이전 스테이지의 Weights를 상속한다.

 - Transformers와 다르게, 이 네트워크는 Input length와 Weights가 Independent해서 Stage간의 Inherit에 문제가 없다.

 - 본 논문에서 제시한 Progressive Learning 방법은 Dropout, RandAugment, Mixup 방법과 Compatable하다.

 - 따라서 본 논문에서는 Dropout, RandAugment, Mixup 방법을 Regularization 방법으로 사용했다. (사실 Warm Learning Rate 등, Bag of Trick에서 소개되었던 기법들이 몇 개 더 있었다. 이건 논문을 참조하길 바란다.)

 - 사용된 Regularization 기법들 중 햇갈릴만한 부분은 RandAugment, Mixup방법인데, 이것은 Data Augmentation 포스팅을 참조하길 바란다.


## Results


<img src="/assets/image/EfficientNetV2/table7.PNG" width="675px" height="450px" title="title" alt="title">

<img src="/assets/image/EfficientNetV2/figure5.PNG" width="675px" height="450px" title="title" alt="title">

<img src="/assets/image/EfficientNetV2/table8.PNG" width="675px" height="450px" title="title" alt="title">


## Ablation studies

 - EfficientNet V1과 V2의 비교.

 - V1에 Progressive Learning을 적용했을 때, Training speed 및 ACC가 상승함.

<img src="/assets/image/EfficientNetV2/table10.PNG" width="450px" height="300px" title="title" alt="title">

 - V1, V2 둘다 Progressive Learning 없이, Compound scaling만 시행했을 때

<img src="/assets/image/EfficientNetV2/table11.PNG" width="450px" height="300px" title="title" alt="title">

 - Progressive Learning for Diffenent Networks

 - 다른 모델에 Progressive Learning을 적용한 결과이다. 다만, Input size나 Optimizer등 변경시킨 것들이 조금 있다.

<img src="/assets/image/EfficientNetV2/table12.PNG" width="450px" height="300px" title="title" alt="title">

 - 이 결과는 proposed Progressive learning이 더 큰 모델일 수록 더 크게 Training time을 줄여준다는 것을 보여준다.

## Conclusion

 - Training-aware NAS와 Model scaling으로 다른 모델들 보다 더 빠르고 파라미터가 좀 더 효율적으로 작용했다.

 - Improved progressive learning을 적용했다.

 - EfficientNet V1 및 다른 모델들과 비교했을 때, 학습속도는 11배 빨랐고, 파라미터 수는 6.8배 줄어들었다.