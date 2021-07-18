---
title: "[CV] Generalized Focal Loss: Learning Qualified and Distributed Bounding Boxes for Dense Object Detection Review"
date: 2021-04-03 21:05:00
author: Leechanhyuk
categories: paper_Review
tags: Computer_Vision Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

<img src="/assets/image/generalized_focal_loss/first.PNG" width="450px" height="300px" title="title" alt="title">

* * *

** Generalized Focal Loss: Learning Qualified and Distributed Bounding Boxes for Dense Object Detection 논문 리뷰**

논문 링크 : https://arxiv.org/pdf/2006.04388.pdf

* * *

## Abstract

 - One-stage detector의 주된 과정은 Classification 및 Localization으로 이루어진다.

 - Classification은 Focal loss에 의해서, Bounding box location estimation은 Dirac delta distribution에 의해서 Optimized 된다.

 - 최근 트렌드는 이 두 작업을 분리해서 진행하는 것이다.

 - 여기서 문제가 발생한다.

   1. Training 과정에서는 Classification 및 Localization을 분리하는데, Test 과정에서는 Joint(Multiplication 등)하여 동작하여 Training 및 Testing의 환경이 다르다.

   2. Dirac delta function은 Localization에서 ambiguity 및 uncertainty가 발생한다.

 - 따라서 본 논문에서는 Training 및 Testing 환경을 일치시키기 위해서, Training 과정에서 Classification 과정에 Localization을 Joint하여 training 시킨다.

 - Continuous Labeling을 통해서 inconsistency를 없애고, flexible한 distribution을 만든다.

 - 위의 과정 수행을 위해서 GFL(Generalized Focal Loss)를 제안한다.

 - State-Of-The-Art를 달성했다.

## Introduction

<img src="/assets/image/generalized_focal_loss/figure1.PNG" width="750px" height="500px" title="title" alt="title">

 - 좌측이 기존 방법, 우측이 제안 방법이다.

 - Classification 및 Localization 과정의 분할은 Training 및 Testing 환경의 불균형을 초래했다. (Inconsistent usage of localization quality estimation and classification score between training and inference)

 - Localization의 Supervision에서 Positive sample만을 고려했다는 점은 Negative sample의 Localizaiton score가 높게 나오는 현상을 초래했다. 이는 NMS(Non-Maximum Supression) 과정 시에 Positive sample보다 더 rank가 높게 나오는 결과를 초래할 수 있어, 전체적인 detection performance가 저하되는 결과를 낳았다.

<img src="/assets/image/generalized_focal_loss/figure2.PNG" width="450px" height="300px" title="title" alt="title">

 - $(a)$는 Negative sample을 Localization rank가 높게 평가되었을 때를 나타내고, $(b)$의 파란색은 Localization process를 분리했을 때 classification scores와 IoU scores 간의 불균형을, 초록색은 제안 방법을 사용하여 균형을 이룬 모습을 나타낸다.

<img src="/assets/image/generalized_focal_loss/figure3.PNG" width="650px" height="500px" title="title" alt="title">

 - 그림에서 흰색 박스는 원래 GT를, 빨간색은 Ambiguous한 Region을, 초록색은 특징이 잘 나타나는 지역을 나타낸다.

 - 이 그림을 통해, 저자는 기존의 Localization 방식 (Dirac Delta Distribution을 활용한 방식) 대신에 제안 방법을 사용할 필요성을 제기한다.

 - 제안 방법은 Classification vector 및 Localization quality factor를 합쳐서 학습하는 방식이다.

 - 직관적으로 생각할 때에도, 좀 더 정확한 Region이 제안되면, Classification score가 높아질 것이고, 이는 전체적인 Prediction score에 영향을 미칠듯 싶다.

 - 이 방식에서 Negative sample에는 quality score를 0을 부여한다. 

 - 이를 위해서 디자인한 GFL(Generalized Focal Loss)는 QFL(Quality Focal Loss) 및 DFL(Distribution Focal Loss)로 이루어진다.

 - QFL은 Sparse한 Hard sample들을 Categori에 따라 0~1까지 Continuous하게 분류하는 value에 관해 학습한다.

 - DFL은 Target bounding box에 대한 Continuous한 location의 probability value에 관해 학습한다.
 
## Related Work

 1. Representation of localization quality
   - NMS, IoU-Net, MSR-CNN, FCOS, IoU Aware은 localization 과정을 분리하여 Training & Testing 환경에 차이가 있다.
   - PISA, IoU Balance는 classification score 및 localization accuracy를 합쳤지만, loss를 classification에 종속적으로 디자인하지 않은 것에서 한계가 있다.(Localization보다 Classification이 주가 되야한다는 의미)

 2. Representation of bounding boxes
   - Dirac delta distribution은 지난 몇 십년동안 bounding box representation에 사용되어 왔다.
   - 최근에는 Gaussian assumption이 예상되는 variance의 prediction을 통해서 uncertainty를 학습하기 위해 사용되고 있다.
   - 현존하는 방법들은 실제 데이터의 distribution을 표현하기에는 너무 simple하다.

## Method

<img src="/assets/image/generalized_focal_loss/figure4.PNG" width="450px" height="300px" title="title" alt="title">

 1. **QFL** (Quality Focal Loss)

   - **Classification과 Localization quality(IoU Score)를 Joint**하여 Loss를 만듬. ("Classification-IoU 로 줄여서 부름)

   - Positive sample은 0~1사이의 값을(IoU Score에 기반함), Negative sample은 0의 값을 갖는다.

   - Multiple-binary classification을 통해 Multi-class implementation을 수행(한 Sample마다 클래스의 개수만큼 이진분류를 진행한다는 뚯인가?)
  
<img src="/assets/image/generalized_focal_loss/qfl.PNG" width="450px" height="300px" title="title" alt="title">
   
   - 해당 식은 조절항인 ${\Vert y-\sigma\Vert}^\beta$ 및 Binary cross entropy와 유사한 항인 $-((1-y) log(1-\sigma) +y\log(\sigma))$의 곱으로 구성되어 있다.

   - figure 4는 기존 방법 및 제안 방법을 나타내는데, 좌측에서 보면 Classification branch에는 one-hot label이, 오른쪽에는 soft one-hot label이 있는 것을 확인할 수 있다.

   - Soft one-hot label이란, IoU score를 뜻하며, 확실하진 않지만, Classification label은 원래 해당 클래스가 1이 되어야하는데, 이건 1에 IoU Score를 곱한 것으로 추정된다.

   - 의문점은, Target IoU Score가 1이 아닌게 이상하다.. 이 부분은 좀 더 살펴봐야할 듯.

   - 이렇게 Classification vector에다가 Localization에 관한 factor를 곱해주는 방식으로 Loss를 선별해낸다.

 2. **DFL** (Distribution Focal Loss)

   - 기존 Bounding box regression model은 아래 식을 통해서 FCN단에서 Regression을 진행하였다.

<img src="/assets/image/generalized_focal_loss/5.PNG" width="450px" height="300px" title="title" alt="title">

   - 기존 방법(Dirac delta, Gaussian) 대신에, 제안 논문은 General distribution $P(x)$를 사용해 target y를 estimation하는 것을 제안했다. 식은 아래와 같다.

<img src="/assets/image/generalized_focal_loss/4.PNG" width="450px" height="300px" title="title" alt="title">
   
   - 또한 범위를 [$y_0, y_n$]에서 {$y_0, y_1, ..., y_n$}으로 세분화하였으며, 따라서 식은 아래와 같이 나타난다. 

<img src="/assets/image/generalized_focal_loss/52.PNG" width="450px" height="300px" title="title" alt="title">











