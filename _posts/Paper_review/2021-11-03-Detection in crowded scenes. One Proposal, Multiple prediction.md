---
title: "[Object detection] Detection in Crowded Scenes: One Proposal, Multiple Predictions"
date: 2021-11-03 21:05:00
author: Leechanhyuk
categories: Paper_review
tags: Computer_vision
use_math: true
cover: "/assets/instacode.png"
toc: true
---

* * *

**Detection in Crowded Scenes: One Proposal, Multiple Predictions review**

<img src="/assets/image/multipleprediction/front.png" width="600px" height="450px" title="title" alt="title">

# 0. Abstract

  - 겹쳐있는 Object를 잘 탐지하는 것을 목표로 하는 논문.

  - Proposal-based object detector를 제안

  - 기존 네트워크처럼 하나의 prediction에서 하나의 instance만 추론하는 것이 아닌, EMD Loss 및 Set NMS를 사용하여 multiple-prediction을 진행한다.

  - FPN-Res 50을 사용하여 Crowded-Human dataset에서 4.9% AP Gain을 얻음.

  - COCO와 같은 Crowded 하지 않은 Dataset에서도 성능 향상이 일어났음을 통해 Robust한 method를 제안한다고 주장.

# 1. Introduction

  - Proposal-based method는 One-stage, Two-stage 관계없이 사용되어 왔고, method는 다시 Anchor-based method, Learnable method(RPN)로 나뉜다.

  - Proposal-based method는 COCO 혹은 Pascal-VOC Dataset에서 좋은 성능을 냈지만, figure 1과 같이 crowded한 object detection에서는 성능이 좋지 않았다.

  <img src="/assets/image/multipleprediction/figure1.png" width="600px" height="450px" title="title" alt="title">

  - 성능이 좋지 않았던 이유는, 겹치는 object들이 매우 유사한 feature를 공유한다는 점과, NMS를 통해서 쉽게 제거될 수 있다는 점을 들 수 있다.

  - 이러한 이슈를 다루었던 다른 연구들이 있었지만, 그 연구들은 Object가 너무 많이 겹치거나, 아에 덜 겹치는 경우에는 성능이 좋지 않았다.

  - 본 논문에서는, 이러한 이슈를 하나의 instance만 inference하는게 아니라, set-of-instance를 inference 하는 방식으로 overlapped object를 검출해낸다.

  <img src="/assets/image/multipleprediction/figure2.png" width="600px" height="450px" title="title" alt="title">

  - 이 방식을 통해서, detector는 여러개의 object를 분별적으로 학습하기보다, 같은 object의 set의 feature를 학습하는 방식으로 진행한다.

  - EMD Loss 및 Set NMS, Refinement module의 사용으로 본 method를 성공적으로 구현했다.

  - 본 method를 통해, Crowded dataset은 물론, COCO와 같은 generalized dataset에서도 성능 향상을 이끌어냈다.

# 2. Background

  - 기존의 proposal-based detector는 아래와 같은 과정을 거쳐갔다.

  1. **Proposal-box generation** from Anchor, selective search, Region proposal network (RPN).

  2. **Instance prediction**, prediction box가 Ground truth 영역에 속하는지 아닌지를 판단하고, 맞으면 bounding box를 refine하는 과정으로 진행한다.

  - 따라서 위 과정은 많은 prediction들이 하나의 ground truth bounding box에 assign됨을 의미하고, 해당 결과를 효과적으로 refine하기 위해서 NMS가 필요한 것이다.

  - ## 2.1. Advanced NMS

    - 기존의 NMS는 Object가 crowded 하지 않다는 가정 하에만 효과적이어서 improve 된 NMS들이 등장했다.

    - Soft NMS, Softer NMS는 Confidence score가 낮은 애들을 바로 버리기보다는, confidence score를 decay하는 방식을 선택했다.

    - [30] 에서는Quadratic Binary Optimization을 사용하여, distribution of ground truth-size에 따른 이점을 가져왔다. 하지만 이 방식은 case에 따라서는 잘 작동하지 않는 경우도 있었다. *(공부필요)*.

    - [18, 25]에서는 data-dependent한 network를 NMS에 적용해서 효과를 보았지만, 너무 Cost가 많이 들어가는 단점이 있었다.

    - [23, 17]은 bounding box에 따라 different한 threshold를 적용하였다. 이 경우에는 IoU/density estimation을 위해서 추가적인 network가 필요하였고, 많이 겹치는 bounding box는 분별해내지 못했다.

  - ## 2.2. Loss functions for crowded detection.

    - 드물게, Loss function design을 통해 crowded detection 문제를 극복하려는 사례들이 있었다.

    - Aggregation loss, repulsion loss는 proposal이 GT와 근접하게 가도록 하거나, Multiple Ground Truths에는 추가적인 페널티를 주는 방법을 도입했다.

    - 이런 방법들은 여전히 NMS를 필요로하고, overlapped instance에서는 recall이 떨어지는 문제점이 있었다.

  - ## 2.3. Re-scoring.

    - 기존 detector들이 자주 사용하던 방법은, proposed-bounding box가 GT와 최대한 매칭되도록 하여 성능을 향상시키는 방식을 사용하였고, 이는 Many-to-one 관계를 초래해 NMS의 사용을 불가피하게 만들었다.

    - 따라서 기존 연구들 중, one-to-one 관계가 되어 NMS를 제거할 수 있도록 loss function을 design 한 연구들이 있었다. 하지만 이 연구들은 prediction 하나당 GT 하나를 매칭하는 방법을 사용하였기 때문에, prediction이 Optimal한 GT에 matching되지 못하여 result가 ambiguous해지는 경향성이 드러났다.

    - RelationNet에서는 앞에 언급된 one-to-one network의 문제를 해결하기 위하여, 추가적으로 proposal 간의 관계성을 modeling하는 방법을 사용했다. 하지만 Overlapped object 하나 하나를 다른 label로 분류하려고 하는 방식 때문에, crowded human dataset에서는 좋은 효과를 나타내지 못했다.

    - 결과적으로, crowded object를 하나하나 detection하는 것은 기본적으로 너무 어렵다. 따라서 본 논문에서는, **multiple instance prediction**을 매 proposal마다 진행하는 것으로 이를 해결하고자 한다.

# 3. Our Approach: Multiple Instance Prediction

  - 제안하고자 하는 방법은 아래와 같은 두 가지 발견 사항에 기초하고 있다.

    1. Object들이 많이 겹쳐있는 것들에 관한 고려

    2. 하나의 proposal이 다른 proposal과 겹쳐있으면, 그 object는 다른 object와 겹쳐서 보일 확률이 높다.

  - 따라서, 하나의 object만 prediction하기보다 set-of-object를 prediction 하는 방법을 사용하기로 함.

  <img src="/assets/image/multipleprediction/equation1.png" width="600px" height="450px" title="title" alt="title">

  - $b_i$는 each proposal box, $g_i$는 each ground truth box, $G(b_i)$는 set-of-ground truth instances, $\theta$는 positive sample이 되기 위한 threshold

  <img src="/assets/image/multipleprediction/figure2.png" width="600px" height="450px" title="title" alt="title">

  - figure 2(b)에서는 Highly overlapped boxes를 예시로 들어서, single-instance-detection (figure-2(a)) 및 group-instance-detection (figure-2(b))를 비교해보고 있다.

  - ## 3.1. Instance set prediction

    - 기존의 detection 방식은 각 instance마다 class label과 bounding box coordinate를 예측했다.

    - 반면에 