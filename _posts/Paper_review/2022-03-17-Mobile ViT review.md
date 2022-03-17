---
title: "[Image recognition] MoblieViT: Light-Weight, General-Purpose, And Mobile-Friendly Vision Transformer"
date: 2022-03-17 21:05:00
author: Leechanhyuk
categories: Paper_review
tags: ViT Mobilevit Mobile
use_math: true
cover: "/assets/instacode.png"
toc: true
---

* * *

**MoblieViT: Light-Weight, General-Purpose, And Mobile-Friendly Vision Transformer Review**

본 포스팅에서는 MobileVit 논문에 관한 개인적인 논문 정리 겸 리뷰를 작성하였습니다.

# 0. Abstract

  - 본 논문은 페이지 수는 많지만, 명료하고 주장하는 바가 간단하다.

  - CNN의 구조를 띄면서 Transformer를 사용한 네트워크를 만들었고, 두 네트워크의 Inductive bias를 모두 살려 좋은 성능을 이끌어 냈다는 것이 포인트.

  - 최종적으로는 ImageNet-1k에서 78.4%로 Top-1 accuracy를 달성했다.

# 1. Introduction

  - ViT는 너무 무겁고 파라미터 의존도가 높아서, 현실 세상에 문제를 풀기에는 부적합하다.

  -  모바일 기기에서 효과적으로 작동하는 CNN에 비해 ViT는 파라미터 의존도가 높고 학습이 어려우며, Image specific bias가 결여되어 있어 실용성이 낮았다.

  - Flops가 latency 속도를 판별하는 기준이 될 수는 없기 때문에, 본 논문에서는 light-weight, general-purpose, low-latency에 초점을 맞추고 CNN과 transformer의 결합을 통해 모바일 기기에서의 활용성을 향상시켰다.

  - CNN을 통해 local-feature를 transformer를 통해 global feature를 학습시킴으로써, Image net 1k에서 top-1 accuracy (78.4%)를 달성했고, 다른 모바일 네트워크에 백본으로 사용될 때도 mobile vit는 뛰어난 성능을 보였다.


  <img src="/assets/image/mobilevit/figure2.png" width="600px" height="450px" title="title" alt="title">

# 3. Mobile-ViT: A Light-Weight Transformer

  - Transformer based model들은 locality(cnn의 inductive bias)를 가지고 있지 않아서 학습 시 좀 더 많은 파라미터들을 필요로 한다.

  - ## 3.1. MobileVit Architecture

        - ## 3.1.1. MobileViT Block

        <img src="/assets/image/mobilevit/figure1.png" width="600px" height="450px" title="title" alt="title">

            - Mobile vit는 nxn convolution과 point-wise convolution을 통해서 입력을 high-demension으로 projection한다.

            - 일반적으로, long-range dependency를 배우기 위해서는, dilated convolution, self attention 등을 사용하는데, self attention을 사용하는 vit는 spatial inductive bias가 없어서 많은 샘플들을 필요로 한다.

            - Mobile vit에서는 convolution으로 feature를 압축하고, point-wise convolution으로 high-dimension으로 projection 한 후, transformer로 long-range dependency를 배우고, 다시 wxhxc 형태로 변환 후 point-wise convolution으로 변환함으로써 원래 이미지 형태를 보존함으로써 receptive field를 본래 이미지 사이즈만큼 가져간다. (여기에서는 high-dimension으로 projection 한 후에 transformer로 long range dependency를 배우게 한거랑, 다시 원래 이미지 형태로 복원시켜 receptive field를 보존한 것이 포인트)

        - ## 3.1.2. Relationship to convolution

            <img src="/assets/image/mobilevit/figure4.png" width="600px" height="450px" title="title" alt="title">

            - MobileViT는 일반 CNN에서 중간 과정을 high dimension feature based transformer로 바꿈으로써 locality 및 long-range dependency 까지 학습 한다는 것이 차이점이다.

        - ## 3.1,3. Light-weight

            - 기존의 transformer와 다르게 mobileViT가 light-weight가 될 수 있었던 이유는 이미지의 원형을 보존함으로써 image-specific inductive bias를 보존했기 때문에, 더 적은 파라미터로도 충분히 이미지에 관해서 잘 학습할 수 있었기 때문이다.

        - ## 3.1.4. Computational cost

            - Mobile vit는 Delt보다 계산량 측면에서 덜 효율적이지만, 적은 파라미터 수를 통해 적은 Flops를 가져가고, accuracy도 더 높다.

        - ## 3.1.5. Mobile ViT Architecture

            - 기존 Light-weight CNN Network와 유사하게, convolution layer를 조정함으로써 network size에 따라 여러가지 버전을 제시하였다.

  - ## 3.2. Multi-Scale Sampler For Training Efficiency
    
      <img src="/assets/image/mobilevit/figure5.png" width="600px" height="450px" title="title" alt="title">

      - Resolution을 높이면 높일 수록 batch_size가 작아지게 되는데, 본 논문에서는 resolution에 따라 차등적인 batch size를 사용함으로써 효율성을 추구하였다.

# 4. Experimental Results

  - ImageNet-1k validation result는 아래 figure 6와 같다.

  - 가벼운 CNN 및 Backbone으로 자주 사용되는 CNN보다 파라미터 수가 더 적으면서도 결과가 더 좋다.

  <img src="/assets/image/mobilevit/figure6.png" width="600px" height="450px" title="title" alt="title">

  - ViT 계열의 네트워크 및 Mobile ViT에 대한 실험 결과는 아래 figure 7과 같다.

  - 강력한 Augmentation을 적용한 기존 ViT Based model보다 기본적인 Augmentation (Introduced in ResNet)을 사용한 Mobile ViT가 결과가 더 좋았다.

  <img src="/assets/image/mobilevit/figure7.png" width="600px" height="450px" title="title" alt="title">

  - Table 1 및 Table 2에서는 Detection 및 Segmentation performance를 비교하고 있는데, 둘 다 기존 Light-CNN Models 보다 결과가 더 좋다.

  <img src="/assets/image/mobilevit/table1.png" width="600px" height="450px" title="title" alt="title">

  <img src="/assets/image/mobilevit/table2.png" width="600px" height="450px" title="title" alt="title">

  - Figure 8은 Mobile ViT를 Mobile 환경 (IPhone 12)에서 Classification, Detection, Segmentation 분야에서 performance 대비 inference time을 측정한 것이다.

  - 초록색 부분이 Real-time에 들어간 모델들을 나타내고, 빨간색, 주황색 점들은 모델을 Network size별로 나눌 때, 사용한 Convolution filter의 크기가 다른 특징이 있다.

  <img src="/assets/image/mobilevit/figure8.png" width="600px" height="450px" title="title" alt="title">

  - Table 3는 Mobile device에서 Light-weight CNN (MobileNet V2) 및 Transformer 계열의 모델들 (DeIT, PiT)을 비교한 결과이다.

  - 결과는 Transformer 계열의 모델들 보다는 확실히 뛰어나지만, MobileNetv2에 비해서는 뒤쳐진다.

  - 저자는 이 원인을 CNN의 최적화에서 찾고 있다. (CNN은 CUDA에 최적화 되어 있고, Device level optimization - conv layer의 batch normalization fusion과 같은 것들)

  <img src="/assets/image/mobilevit/table3.png" width="600px" height="450px" title="title" alt="title">

# 5. 총평

  - 직관적이고 잘 읽히게 논문을 구성했다.

  - 파라미터 수 및 FLOPS 등 최적화를 잘 시키면서도 좋은 Accuracy를 달성한 점이 인상깊었다.

  - 실험도 다양한 task에서 잘 수행한 것 같다.

  - 하지만 메인으로 주장하는게 Image-specific inductive bias의 중요성인만큼, 그에 관한 실험이 있었다면 더 좋았을 것 같았다. (기존에도 CNN - Transformer 계열의 논문들은 많았으니까.. 그와 차별화 된 점이 이것인 만큼 조금 더 중요하게 짚어줬으면 어땠을까 싶다.)

  - 또한 Introduction에서부터 Real-world problem에 대한 적용이 중요하다고 언급했지만, 실제 모바일에서의 결과는 MobileNetV2보다 느리고 accuracy도 큰 차이가 안나는 것 같아 조금 아쉬웠다.

  - 그래도 재밌게 읽었다.

