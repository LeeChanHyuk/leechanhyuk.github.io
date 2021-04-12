---
title: "[CV] Focal loss for dense object detection"
date: 2021-03-26 21:05:00
author: Leechanhyuk
categories: paper_Review
tags: Computer_Vision Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

<img src="/assets/image/High-performance/frontdoor.PNG" width="450px" height="300px" title="title" alt="title">


* * *

**Focal loss for dense object detection 논문 리뷰**

 - 본 리뷰는 논문의 구성을 동일하게 따라가지 않고, 제가 이해한대로 정리한 리뷰입니다. 참고하시면 좋을 듯 합니다.

* * *

## 논문 관련 정보

 - Facebook AI Research(FAIR)에서 Publish 한 논문

 - Class-imbalance Problem을 Loss function을 통해 완화시킨 초기의 논문

## 전체 정리

- One-Stage detector(YOLO, SSD 등)란 한 네트워크에서 Region-Proposal - Classification의 전 과정을 진행하는 네트워크를 의미. Two-Stage detector에 비해 상대적으로 ACC가 낮고, 빠름.

- Two-Stage detector(R-CNN 계열 등)란 RP(Region-Proposal) 과정을 분리하여, Proposed region 중 조건에 맞는 일부만 Classification 하는 네트워크를 의미. One-Stage detector에 비해 상대적으로 ACC가 높고, 느림.

- Two-Stage detector는 RP 단계에서 한번 필터링을 거침으로써, 원하는 Object가 있는 곳(Foreground)이 아닌 다른 영역(Background)이 학습에서 큰 영향을 미치지 않도록 디자인.

- 기존의 One-Stage detector는 RP단계에서 필터링 없이, 모든 Proposed region **(아주 극소수의 Foreground, 대다수의 Background로 구성)**을 학습하여, 과도하게 많은 Background를 학습하도록 되어버림.

- 논문에서 제시한 Foreground : Background 비율은 1 : 1000

- 따라서 학습이 Background example에만 집중되어 있어 올바르게 학습을 하지 못함. (Class-imbalanced problem post 참조)

- 저자는 Two-Stage detector보다 ACC가 낮은 Critical한 요인이 Foreground와 Background sample(Proposed region을 의미)의 차이라고 생각함.

- 또한, Sample들 중에 분류 난이도가 Hard한 sample의 학습이 ACC 향상에 기여할 수 있다고 판단. 반대로 말하면, easy negative sample들이 Loss에 좋지 않은 영향을 끼치고 있다고 판단.

- 따라서 대부분의 easy negative들을 학습에서 배제하기 위해서, Loss function에 Probabilistic Term을 넣어서 Hard한 sample에 집중하자는 것이 논문의 주장.

- 본 Loss를 사용하기 적합한 네트워크를 선정(Called RetinaNet), Inference한 결과는 아래 사진과 같다.

<img src="/assets/image/focal_loss/inference_output.PNG" width="450px" height="300px" title="imagenet" alt="imagenet">

## Focal loss

 - 저자가 제안한 Loss는 Binary Cross-Entropy(아래 사진 참조)에 몇가지 Term을 붙인 형식으로 구성되어 있다. (1)

<img src="/assets/image/focal_loss/Binary_cross_entropy.PNG" width="450px" height="300px" title="imagenet" alt="imagenet">
 
 - $p_t$는 위의 Cross-Entropy에도, 추가적으로 붙일 Term에도 사용되므로 간략화를 위해서 따로 정의해 둔 것이다. (2)

<img src="/assets/image/focal_loss/pt.PNG" width="450px" height="300px" title="imagenet" alt="imagenet">

 - Focal loss 식은 아래와 같다. (4)

<img src="/assets/image/focal_loss/focalloss.PNG" width="450px" height="300px" title="imagenet" alt="imagenet">

 - 식을 잘 보면 알 수 있겠지만, Focal loss는 앞에 붙여진 $(1-p_t)$ Term을 통해서 classification 결과가 틀렸을 경우 많은 Loss를, 맞았을 경우 적은 Loss를 산출하게 만들어준다.

 - 이를 통해서, Easy negative sample들의 Loss 개입을 막고, 철저하게 Hard한 Sample들 위주로 학습을 진행시킨다.

 - 또한 $\gamma$ Term을 통해서 추가된 Modulating factor의 영향을 제어할 수 있도록 하였다.

 - 일반적으로 Class-imbalance problem을 다룰 때에는 아래 식과 같이 Positive class인지, Negative class인지에 따라서 Loss를 조절해주는 weighting factor를 추가한다. (3)

<img src="/assets/image/focal_loss/pt.PNG" width="450px" height="300px" title="imagenet" alt="imagenet">

 - Focal loss에서도 식 (4)에 나타나있던 식에 $\alpha$ Term을 덧붙여, Positive / Negative class에 따라서 Loss를 좀 더 조절하도록 하였다.

## Model initialization

 - 일반적으로 Binary Cross Entropy는 Class가 1, -1일 확률이 같도록 초기화되는데, Class imbalance 문제가 큰 Task의 경우에는 이 확률을 실제 sample의 비율과 유사하게 조정하면 학습시에 도움이 된다.

## Class imbalance and Two-stage detectors

 - Two-stage detector는 IoU가 일정 Threshold 이상인 Region만 남기는 방법으로 Positive와 Negative를 구분해 낸다.

 - 구분된 Positive 및 Negative는 alpha-balancing 비율과 유사한 비율로 Mini-batch 내에서 구성된다.

## RetinaNet Architecture
 
 - RetinaNet의 Architecture는 아래와 같다.

<img src="/assets/image/focal_loss/architecture.PNG" width="675px" height="450px" title="imagenet" alt="imagenet">

 - Backbone은 ResNet을 사용했고, FPN을 사용하여 결과로 도출된 값에 Classification 및 Box localization을 수행하는 방식으로 되어있다.

 - 새로운 Network를 디자인하는게 본 논문의 목표는 아니므로, 거기에는 신경을 많이 쓰지 않았다고 한다.

 - FPN 없이 단일 Backbone을 사용하는 경우에는 AP가 낮았다고 한다.

## Anchor

 - Box localization에 사용된 앵커는 RPN(Region Proposal Network)에서 사용된 앵커와 유사한 것을 사용했다.

 - 앵커는 피라미드 레벨인 $P_3 \to P_7$ 까지 $32^2 \to 512^2$의 영역 넓이를 갖는 앵커를 사용했따.

 - 앵커의 aspect ratios 는 {1:2, 1:1, 2:1}, {$2^0, 2^{\frac_{1}_{3}}, 2^{\frac_{2}_{3}}$} 이다.

 - 각 앵커박스에는 K개(클래스 종류 개수)의 Classification target(One-Hot)과 4개의 Box regression target(모서리) 가 할당되어 있다.

 - GT와 IoU가 0.5 이상이면 Positive sample이라고 판단했다.

 - GT와 IoU가 0.4 이하이면 Negative sample이라고 판단했다. 나머지는 무시한다.

## Classification Subset

 - Classification subset은 한 공간에서 A개의 Anchor로 부터 각각 K 클래스일 확률을 계산한다.

 - 모든 Pyramid level에서 본 Subnet의 Parameters를 공유한다.

 - 네트워크 구조는 3x3 conv layers - RELU - KA Filters(Anchor 및 K 클래스일 확률 계산하는 것을 의미)

 - Box regression Subnet과는 Parameters를 공유하지 않는다.


## Inference and Training

 - Initialization : ResNet-50-FPN 및 ResNet-101-FPN을 Backbone으로 실험하였다. ImageNet을 통해 Pretrain 후에 진행하였다.

 - Classification subset의 마지막 Conv layer의 bias를 $b = -\log{(1-\pi)/\pi}$로 세팅하고 진행하였다. $\pi$는 0.01로 세팅했다. 이건, Training의 첫 Iteration에서 Background에 Anchors가 너무 많아지는 것을 방지해준다.

 - Optimization : SGD를 사용하고, Learning Rate는 0.01을 사용했다. Weight decay는 0.0001 / Momentum은 0.9를 사용했고 Localization을 위해서는 smooth L1 Loss를 사용했다.

## Exparimental result

 - COCO Dataset을 사용해서 실험한 결과는 아래와 같다.

<img src="/assets/image/focal_loss/result.PNG" width="675px" height="450px" title="imagenet" alt="imagenet">

 - Classification subset의 마지막 conv layer의 bias의 조절을 하지 않았을 때에는 Diverse 했다.

 - $\alpha$값은 0.75가 최적이었다.

 - $\gamma$ 값은 2가 최적이었다. (아래 그래프 참조)

<img src="/assets/image/focal_loss/gamma.PNG" width="675px" height="450px" title="imagenet" alt="imagenet">

 - SOTA와의 비교는 아래 표와 같다.

<img src="/assets/image/focal_loss/SOTA.PNG" width="675px" height="450px" title="imagenet" alt="imagenet">

## Conclusion

 - Hard example에 집중한 Loss는 매우 간단하고 효율적이다.

 - SOTA를 달성했다.

## 마치며

 - Class imbalance 문제를 접할 수 있었다.

 - Loss function design에 대해서 심층적 고민 기회를 얻을 수 있었다.

 - Detector의 전체적인 종류 및 계보를 한번 더 정리할 수 있었다.

















