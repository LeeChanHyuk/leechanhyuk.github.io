---
title: "[Image recognition] Efficient Net Review"
date: 2020-12-24 18:13:00
author: Leechanhyuk
categories: Paper_review
tags: Computer_Vision Machine_Learning
cover: "/assets/instacode.png"
toc: true
---

<img src="/assets/image/efficientnet/efficientnet_title.png" width="450px" height="300px" title="title" alt="title">


* * *
**본 논문리뷰는 논문 구성을 따르고 있지만, 단지 제 생각을 구성에 맞춰서 적은 것이지**

**실제 논문을 요약하여 적은 것이 아닙니다. 착오 없으시길 바랍니다.**

**Efficient Net 논문 리뷰**

* * *

## Efficient Net에 관한 정보

1. 네트워크의 최적화와 Accuracy를 동시에 신경 쓴 모델
2. 네트워크 최적화 방식을 formulize하여 많은 모델에 도입이 쉬움
3. 논문 publish 당시 Top-1 Accuracy를 달성, 동시에 매우 적은 파라미터로 속도 역시 빠름

<img src="/assets/image/efficientnet/top1.png" width="450px" height="300px" title="MAE" alt="MAE">

## Introduction

Computer vision. Classification이나 Detection과 같은 task에서는 일반적으로 Accuracy를 중시하는 경향이 있습니다.

물론 Accuracy는 성능 평가에 뺄 수 없는 중요한 지표이고, 실사용에서도 아주 큰 부분을 차지한다고 생각하지만

ILSVRC(ImageNet competetion)의 영향인지 지금 껏 본 바로는 **Accuracy 중심의 모델이 중심**이 되어서 소개되고

경량화에 중점을 둔 모델은 그와 별개로, 혹은 특수 목적에 맞춰서 소개되곤 했습니다.

<img src="/assets/image/efficientnet/imagenet.png" width="450px" height="300px" title="imagenet" alt="imagenet">

왜 그런걸까요? 왜 Accuracy와 speed는 분리되어야 했을까요?

그 이유는 간단합니다. 

바로 Accuracy에 영향을 미치는 채널이나 레이어 수를 증가시킨다는 것은 그 만큼 Computation cost가 증가함을 의미하고

이는 곧 Speed의 저하로 이어지기 때문이죠.

따라서 지금까지의(EfficientNet 발표 이전) 논묻들은 모델이 너무 방대해지지 않는 한에서 

레이어 수나 채널 수 혹은 입력 영상의 해상도와 같은 factor들을 경험적으로 조절하고 

네트워크 자체의 전개 방식에서 아이디어를 내어 좋은 결과를 얻곤했습니다.

EfficientNet은 이런 부분에서 착안하여 위에 명시된 factor들을 조절하여 Accuracy를 향상시킴과 동시에

Speed도 같이 가져갈 수 있는 가장 이상적인 factor value를 찾는 것을 목적으로합니다.

또한 모델마다의 특성이 달라질 수 있는 것을 고려하여, 범용적인 사용을 위해 이를 공식화하여 많은 모델에 적용이 가능하도록 하였습니다.

## Proposed Approach - What is the Compound scaling?

<img src="/assets/image/efficientnet/scale.png" width="600px" height="450px" title="title" alt="title">

Introduction에서 말했듯이, Model의 Accuracy를 향상시키기 위해선 Model의 알고리즘적인 측면 뿐만 아니라

Layer의 수(Depth), Channel의 수(Width), 입력 영상 해상도(Resolution)에 대한 고려가 필요합니다.

*Figure 2*는 각 factor 별 특성을 고려한 네트워크를 (a) baseline에 대한 간단한 변형을 통해 

(b), (c), (d)로 각각 나타내고 있습니다.

(e)는 3가지 factor들을 모두 반영한 모습을 보여주고 있는데, 본 논문에서는 3가지 factor에 대한 Ideal value를 찾아

네트워크에 모두 반영하는 것을 Compound Scaling으로 명명하고 이를 공식화하여 나타냅니다.

## Proposed Approach - Problem Formulation

<img src="/assets/image/efficientnet/problem.png" width="450px" height="300px" title="title" alt="title">

사진은 Efficient Network의 Problem formulation을 나타냅니다.

이는 전체 네트워크를 d(for Depth), w(for Width), r(for Resolution) 3가지 factor를 사용해서 network를 resize 하여

Memory와 Flops(초당 부동소숫점 연산)이 허용하는 범위 이내에서 Accuracy를 maximize하는 것을 목적으로 합니다.

또한 본 네트워크에 사용된 Conv Layer들은 똑같은 형태의 레이어를 여러개 연속적으로 사용하고 있으며

해당 레이어들의 Function을 Fi로, Function의 인수에는 factor와 곱해진 입력 영상(X)이 들어가고

레이어들의 조합이 전체 네트워크(N)을 구성하고 있습니다.

## Propose Approach - Scaling Dimensions

앞서서도 설명했지만, 3가지 factor들은 서로가 dependency를 가지고 있어 

3가지를 한꺼번에 고려하여 dimensions를 정하는 방법이 필요합니다.

따라서 논문에서는, scaling에 대한 설명 이전에 각 factor들의 속성에 대하여 먼저 설명합니다.

1. Depth
  - 복잡하고 많은 특징들을 잘 잡아냄
  - 새로운 task에도 좋은 효과를 발휘함
2. Width
  - 크기가 작은 특징들을 잘 잡아냄
  - 크기가 큰 특징들은 잘 잡아내기 힘듬
3. Resoltuion
  - 크기가 작은 특징들을 잘 잡아냄

이러한 factor들의 증감이 네트워크에 어떤 영향을 미치는가를 테스트하기 위해서

동일한 baseline Network를 factor들을 각각 사용해서 scaling 한 후 결과를 분석합니다.

<img src="/assets/image/efficientnet/each.png" width="450px" height="300px" title="title" alt="title">

각각의 결과는 연산속도 대비 Accuracy를 나타내고 있고, 

**factor가 증가함에 따라 Accuracy도 상승하지만 상승폭은 점점 감소하여, 특정 value로 saturation되는 경향**을 보이고 있습니다.

이와 같은 결과를 통해서 단일 factor scaling은 한계가 뚜렷하다는 것을 확인할 수 있습니다.

* * *

<img src="/assets/image/efficientnet/all.png" width="600px" height="450px" title="title" alt="title">

위 그래프는 색깔에 따라서 d와 r을 설정한 후, w값에 따른 Accuracy 상승을 나타냅니다.

처음 파란색 그래프는 d=1.0, r=1.0으로 w만 변화시킨 그래프와 동일한데 역시 accuracy가 saturation 되는 것을 관찰할 수 있고

마지막 빨간색 그래프는 d=2.0, r=1.3으로 설정해둠으로써 3가지 요소들을 다 변화시켜 Accuracy가 훨씬 높아지는 것을 확인할 수 있습니다.

**이로써 3가지 factor들을 balance있게 고려해야한다는 점** 또한 확인할 수 있습니다.

3가지 factor들을 골고루 고려한 실험을 통해서 저자는 경험적으로 가장 최상의 비율을 이끌어낼 수 있는 공식을 발견했는데

해당 공식은 아래와 같습니다.

<img src="/assets/image/efficientnet/formula.png" width="450px" height="300px" title="title" alt="title">

공식은 3가지 factor를 scalar value로 정해두지 않고, a, b, r의 파이의 제곱승을 통해 표현했는데

그 이유는 사용자의 Computer resource가 모두 다르고, 사용 모델의 크기도 모두 다르기때문에

그 부분을 파이를 조정함으로써 상황에 맞게 조절하기 위함입니다.

또한 a에 비해 b와 r는 제곱으로 나타나있는데 그 이유는

a는 depth를 2배 늘리는 것이므로 단순히 연산량이 2배가 되지만

b, 즉 width는 채널의 수가 두배가 되므로, 한 레이어에서의 output을 산출하는데 필요한 연산량이 2배가 됩니다.

또한 그 output이 다음 레이어의 input으로 작용하므로 그 또한 2배라서 총 4배의 연산량이 필요하게 됩니다.

r은 가로가 2배, 세로가 2배가 되어 총 4배의 연산량 증가가 있습니다.

이런 이유때문에 b 및 r에만 제곱승이 붙은 것입니다.

## Architecture

Architecture를 설계할 때 저자는 전체 네트워크에 Compound scaling을 적용하는 대신에 baseline이 되는

Small network에 Compound scaling을 적용하여 upscaling 하는 방식으로 여러개의 architecture를 설계했습니다.

또한 이와 같은 방법은 "Platform-aware neural architecture search for mobile."(Tan et al/2019 CVPR)에서 착안한

"multi-objective neural architecture search"를 사용하였고, 해당 논문에서 사용한 Optimization function을

mobile 기기에 해당하는 변수 하나만 빼고 그대로 차용하였습니다.

<img src="/assets/image/efficientnet/fun.png" width="450px" height="300px" title="title" alt="title">
(w = -0.07)

해당 공식을 적용함에 따라서 Accuracy는 향상시키고, FLOPS는 저하시키는 방향으로 학습하였으며

사용한 Baseline Network는 MBConv 블록을 사용하여 디자인 하였습니다.

Baseline Network는 아래와 같습니다.

<img src="/assets/image/efficientnet/architecture.png" width="400px" height="250px" title="title" alt="title">

Compound scaling은 Resource의 여유를 두기위해 파이는 1로 고정시켜두고

grid search라는 방법(모든 경우의 수를 전부 탐색)을 통해 최적의 factor를 찾았습니다.

해당 값은 a = 1.2, b = 1.1, r = 1.15로 산출되었으며 해당 모델을 EfficientNet-B0로 명명하고

B0를 기반으로 파이 값을 변화시키며 B1 - B7에 해당하는 네트워크들을 디자인하였습니다.

Compound scaling 후의 학습 parameter들은 아래와 같습니다.

* * *

- RMSProp 사용(Decay = 0.9, Momentum = 0.9)

- Batch Norm momentum = 0.99

- Weight Decay = 1e-5

- Learning Rate = 0.256(decays 0.97 every 2.4 epochs)

- SiLU

- Stochastic depth

- Dropout

* * *

아래 figure는 B0 - B7의 값과 이름있는 네트워크들을 비교한 결과입니다.

<img src="/assets/image/efficientnet/result1.png" width="600px" height="450px" title="title" alt="title">

## Experiments

Compound scaling의 효용성을 검증하기 위해, 저자는 ResNet과 MobileNet에 Compound scaling을 적용했습니다.

<img src="/assets/image/efficientnet/result2.png" width="450px" height="300px" title="title" alt="title">

Network를 Upscaling한 만큼 FLOPS는 증가하였지만, 확실하게 Accuracy는 원래 네트워크는 물론이고

하나의 factor만 고려한 것보다 더 높은 결과를 보여줍니다.

* * *

또한 ImageNet의 모델들과의 비교 또한 진행하였고, 결과 Table은 아래와 같습니다.

<img src="/assets/image/efficientnet/transfer_learning.png" width="600px" height="450px" title="title" alt="title">

테이블의 좌측은 Public하게 공개된 모델들과의 비교이고, 우측은 report된 모델들과의 비교입니다.

전체적으로 비슷한 Accuracy라도 훨씬 적은 파라미터를 사용하고 있고

특히 Birdsnap Dataset에서는 Top-1 Accuracy를 달성했으며, G-Pipe와 비교해 8.4배나 적은 파라미터수를 보여줍니다.

<img src="/assets/image/efficientnet/cam.png" width="450px" height="300px" title="title" alt="title">

또한 CAM을 사용한 결과에서는 Feature map의 분석을 통해서 객체의 특징을 좀 더 잘 잡아주는 것을 보여줍니다.

## 마치며

EfficientNet은 알고리즘적으로만 네트워크를 개선하는데 주목하고있던 많은 사람들을 환기시켰을 뿐 아니라

자신의 방법을 통해서 한정적이지만 Top-1 Accuracy를 달성하였고

개발 방법을 공식화하고, 현존하는 모델들에 적용함으로써 그 효과를 정량적으로 입증했다는 점에서

저는 매우 좋은 논문이라는 생각이 들었고, 읽으면서 많은 걸 배웠다고 생각합니다.

리뷰가 누군가에겐 도움이 되었으면 좋겠습니다ㅎㅎ

잘못되거나 틀린점은 언제라도 말씀해주시면 감사히듣고 반영하겠습니다.

감사합니다.
























