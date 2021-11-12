---
title: "[Object detection] Relation Networks for Object Detection Review"
date: 2021-11-12 21:05:00
author: Leechanhyuk
categories: Paper_review
tags: Computer_vision
use_math: true
cover: "/assets/instacode.png"
toc: true
---

* * *

**Relation Networks for Object Detection review**

<img src="/assets/image/relationnet/front.png" width="600px" height="450px" title="title" alt="title">

# 0. Abstract

  - Object detection에서 object간의 relation이 중요하다는 건 이미 대두되었지만, 지금까지의 연구들은 모두 객체만 식별할 뿐 이었다.

  - 본 논문에서는 one-instance detection 보다 set-of-instance detection을 통해 object set에 관한 geometry 및 appearance relation을 분석하여, 결과적으로는 object recognition 및 duplicate object removal에서 좋은 효과를 보였다.

# 1. Introduction

  - 최근, R-CNN 기반의 State-of-the-art study들은 sparse한 proposal으로부터, hand-crafted parameter (Anchor configuration, NMS)을 기반으로 instance별로 class와 bounding box를 prediction했다.

  - 비전 분야에서는 object간의 relation을 찾는 것이 중요한 task로 여겨졌었지만, deep-learning 대두 이래에는 object instance만을 찾는 연구들이 주류를 이루었다.

  - 실제로 이미지 내의 객체들은 다른 categories, scale을 갖기 때문에, object간의 relation을 특정 짓는건 어렵다.

  - 본 연구는 NLP(Natural Language Processing)에서 자주 사용되는 특정 단어가 주어진 문장의 구성 요소들과 어떠한 관계를 가지고 있는지를 학습할 수 있는 attention module로부터 영향을 받았다.

  - 제안하는 네트워크는 attention 기반으로 이루어져있으며, appearance based, relative geometry based weight를 사용하여 object recognition을 수행함으로써, translation invariant 한 특성을 가지고자 했다.

  - 해당 모듈은 object relation module이라 불리며, 기존의 attention과 같이 parallel하게 동작하며, input과 output의 dimension을 일치시켜 어떤 네트워크에도 쉽게 적용 가능하도록 했다.

  - Figure 1에 나타난 것 처럼, object relation module은 instance recognition 및 duplicate removal 단게에서 활용되며, NMS 등을 사용하지 않고 Network 기반으로 해당 작업을 수행하는 만큼, End-to-end 방식으로 볼 수 있다.

  <img src="/assets/image/relationnet/figure1.png" width="600px" height="450px" title="title" alt="title">

  - 제안하는 모듈의 적용을 통해, 기존의 CNN network의 문제점들을 많이 해결할 수 있었고, 이는 object detection 만이 아닌, segmentation, action recognition 등에도 잘 적용될 수 있다.

# 2. Related works

  - ## 2.1. Object relation in post processing

    - 이전 많은 연구분야에서는 object relationship을 post processing으로 적용해왔지만, deep-learning이 대두 된 이후에는 conv-net의 큰 receptive field가 가지는 incorporated contextual information 때문에 object간의 relationship을 추론하기가 힘들었다.

  - ## 2.2. Sequential relation modeling

    - LSTM이나 Spatial memory network(SMN)을 사용하여 object relation을 sequential하게 파악하고자 했지만, 이 방법은 SOTA Detector에 관하여 테스트되지 않은 한계점이 있다.

  - ## 2.3. Human centered scenarios

    - Human-object relation을 파악하려했으나, 너무 specific한 target에만 적용되어서 범용성이 부족하다.

  - ## 2.4. Duplicate removal

    - Duplicate removal을 위해, NMS는 Object score와 coordinate간의 관계를 자연스럽게 배울 수 있지만, hand-crafted parameter가 사용되는 단점이 있다.

    - GossipNet에서는 set-of-instance간의 관계를 파악하여 duplicate removal을 시행하는 방식으로써, 제안하는 네트워크와 가장 유사하지만, 실험 부분에서 미진한 부분이 많았다.

  - 제안하는 네트워크는 computational cost 면에서도, end-to-end가 가능하다는 면에서도 좀 더 유연하고 효과적인 네트워크라고 할 수 있다.

# 3. Object relation module

  - 이번 챕터에서는 Scaled dot product attention을 이용하여 object relation을 수행하는 과정을 나타낸다.

  - ## 3.1. Appearance weight

    - Relation module의 두 가지 weight 중 하나인 appearance weight이다.

    - image category에 따라서 appearance feature를 구하는 방법이 달라진다.

    - 아래 식을 통해 Object m과 n에 관하여 appearance 유사도를 scaled-dot product로 구한다.

    <img src="/assets/image/relationnet/equation4.png" width="600px" height="450px" title="title" alt="title">

    - 여기서 $f_A$는 object의 appearance feature를, $d_k$는 key의 dimension을, $w_A^{mn}$은 object m과 n의 유사도 weight를 나타낸다.
    (이 과정이 잘 이해가 안간다면, attention 진행 과정에 관해 공부하고 오길 바란다.)

  - ## 3.2. Geometry weight

    - Geometry weight를 구하는 과정은 두 과정으로 나뉘고, 아래 식은 그 두 과정을 통합한 식이다.

    <img src="/assets/image/relationnet/equation5.png" width="600px" height="450px" title="title" alt="title">

    1. Translation 및 scale transformations를 위한 high-dimensional projection.

      - 우선 n, m object의 bounding box의 x, y, w, h를 log function을 사용하여 정규화한다.

      <img src="/assets/image/relationnet/log.png" width="600px" height="450px" title="title" alt="title">

      - 정규화된 4-d vector는 different wavelength의 sin 및 cos function을 이용하는 [49]의 representation method를 사용하여 high-dimension으로 projection 한다.

    2. Trimming and non-linearity

      - 오직 geometric relationship만 고려하기 위해, zero-trimming을 시행한다. ($W_G$를 통해서 trimming 된다.)

      - Non-linearity를 위해 ReLU function을 사용한다.

  - ## 3.3. Total weight

    - $W^{mn}$은 앞서 구한 appearance based weight와 geometry based weight를 모두 사용하여 이미지 내의 모든 object 중에 object m이 n과 얼마나 관련성이 높은지를 확률으로써 나타내는 weight이다.

      <img src="/assets/image/relationnet/equation3.png" width="600px" height="450px" title="title" alt="title">

  - ## 3.4. Relation feature

    - Object m이 n에 미치는 relation을 통합적으로 나타내기 위해 각 feature에 관해서 weighted sum을 하는 방식은 아래와 같다.

    <img src="/assets/image/relationnet/equation2.png" width="600px" height="450px" title="title" alt="title">

    - $f_R(n)$는 relation feature를, $W_v, w^{mn}, f^m_A$는 각각 attention value, total weight, appearance feature를 나타낸다.

  - ## 3.5. Total relation feature

    - Relation feature는 하나만 사용하는 것이 아니라, 여러 개를 사용해서 이미지 내의 다양한 객체와의 관계를 골고루 고려하고자 하였고, 최종 출력에서는 하나로 concatenation해서 사용하였다. 아래 식은 해당 사항을 나타낸다.

    <img src="/assets/image/relationnet/equation6.png" width="600px" height="450px" title="title" alt="title">

    - 본 과정은 아래의 algorithm 1을 통해서도 나타난다.

    <img src="/assets/image/relationnet/algorithm1.png" width="600px" height="450px" title="title" alt="title">

    - 그림으로 나타내면 아래의 figure 2와 같이 나타난다.

    <img src="/assets/image/relationnet/figure2.png" width="600px" height="450px" title="title" alt="title">

    - figure 2를 통해 전체 과정을 다시 한번 더 설명하자면, appearance feature와 query, key, value weight와의 곱을 통해서 query, key, value를 만들어낸다.

    - 만들어 낸 query, key 정보를 scaled-dot production하여서 object n, m 사이의 관계성을 weight로 나타낸다.

    - 또한 geometry information을 high-dimension으로 projection하고 그 정보를 object n, m 사이의 관계성 weight와 같이 고려하여서, 이미지내의 object 중에, m과 n이 얼마나 관련성이 높은지를 확률로써 나타낸다.

    - 마지막으로, object n과 나머지 object들에 관한 관련 weight를 확률을 곱함으로써 weighted-sum을 취하고 이를 하나의 value로 나타낸다.

    - 여러 개의 relation module을 통해 도출된 여러 개의 attention value를 합쳐서 나온 value를 원래 appearance feature와 add해서, 결과적으로는 relation information을 가지고 있는 feature를 만들어 낸다.

# 4. Relation networks for object detection

  - ## 4.1. Review of object detection pipeline

    - 최근 네트워크들은 R-CNN 계열의 네트워크와 유사하며, 일반적으로 총 4가지 단계로 구성된다.

    1. Generate full image features

    2. Generate regional features (From Region Proposal Network)

    3. Instance recognition

    4. Duplicate removal.

    - Relation network를 통해서는 3번 및 4번 과정을 수행할 수 있고, jointly training하는 과정을 통해 accuracy를 향상시킬 수 있고, end-to-end 학습이 가능한 장점이 있다.

    - 또한, Faster-R-CNN, FPN, DCN처럼 RPN을 사용하고, 그 후 2개의 fc layer로 prediction을 수행하는 네트워크 들에 대해서 Relation network를 적용해보았고 결과가 좋아짐을 확인했다.

  - ## 4.2. Relation for instance recognition

    - Relation module은 input과 output의 dimension이 같기 때문에, 2개의 fc layer 사이에 relation module을 끼워 넣는 식으로 하여 instance recognition 성능을 향상시켰다.

    - 즉, equation (9)에서 equation (10)처럼 만들어서 성능 향상시켰다는 의미이다.

    <img src="/assets/image/relationnet/equation9.png" width="600px" height="450px" title="title" alt="title">

    <img src="/assets/image/relationnet/equation10.png" width="600px" height="450px" title="title" alt="title">

  - ## 4.3. Relation for duplicate removal

    <img src="/assets/image/relationnet/figure3.png" width="600px" height="450px" title="title" alt="title">

    - 각 object는 1024-dimension vector (FC layer output), classification score와 bounding box information을 input으로 0에서 1사이의 duplicate sample이 맞는지 아닌지를 나타내는 확률값을 반환한다.

    - Duplicate removal network는 크게 3단계로 구성되어 있다.

    1. 1024-d feature와 classification score를 합쳐서 appearance feature를 만든다.

    2. 모든 object의 appearance feature를 relation module을 통해 transformation 시킨다.

    3. Relation network의 output을 linear layer와 activation layer (sigmoid)를 거치게 하여, 최종 확률 값을 반환하게 한다.

    - ### 4.3.1. Rank feature

      - classification score는 value 그대로 사용되기 보다, 이미지 내의 모든 object의 score를 내림차순으로 두었을 때, 그 rank를 사용한다.

      - 결국에 이 classification score와 1024-d feature는 128-d vector로 변형되어 결과적으로는 relation module의 $f_A$, 즉 appearance feature로 사용된다.

    - ### 4.3.2. Which object is correct?

      - 기존 COCO Dataset의 evaluation metric은 특정 threshold를 정하고, IoU가 그 이상 나오면 correct하다고 판단하는 metric이다.

      - 특정 threshold를 사용하여 training 할 경우, evaluation 시, training 시와 다른 threshold를 사용하게 된다면 성능이 좋지 않게 나오게 된다. (Table 4 참조)

      - 따라서 본 네트워크에서는 threshold를 {0.5, 0.6, 0.7, 0.8, 0.9}를 모두 사용해서, detection score를 산출하고, 산출된 5개의 score를 average해서 최종 detection score를 매긴다.

    - ### 4.3.3. Training

      - 최종 final score는 binary cross entropy를 사용한다. 다만 학습에 참여하는 sample들은 대부분 duplicate sample이고, correct sample은 1% 정도 밖에 안된다.
      그럼에도 학습은 성공적이었는데, 그 이유는 duplicate sample을 올바르게 분류했을 경우, loss는 매우 작아서 학습에 크게 반영이 안되고, 오히려 잘못 분류한 sample에 대해서만 큰 loss가 생기기 때문에, duplicate sample이 많다고 해서 이게 큰 영향을 주지는 않기 때문이다.(Focal loss와는 상황이 조금 다른게, 우선 background : foreground의 비율은 1:10000정도로 기억하는데, 여기서는 한번 걸러진 객체의 duplicate : correct를 비교하기 때문에 비율이 1:100정도 밖에 안되어서 loss에 큰 영향은 미치지 않는 듯 싶다.)

  - ## 4.4. End-to-End object detection

    - 학습에 사용된 loss는 region proposal loss, instance recognition loss, duplicate classification loss 총 3가지이고, 균등하게 최종 반영되었다.

# 5. Experiments

  - Backbone은 resnet, dataset은 COCO를 사용했다.

  - ## 5.1. Relation for instance recognition

    - Instance recognition 부분만 단독으로 보기 위해, NMS를 사용하여 결과를 측정했다.

    <img src="/assets/image/relationnet/table1.png" width="600px" height="450px" title="title" alt="title">

    - 결과는 좌측부터, 아무런 제안 방법도 추가하지 않았을 경우, geometric feature를 추가했을 경우, relation module 1개 사용했을 경우, 2개 사용했을 경우를 나타낸다.

    - 파라미터 수와 AP의 관계성을 보기위한 실험도 시행했다.

    <img src="/assets/image/relationnet/table2.png" width="600px" height="450px" title="title" alt="title">

    - Duplicate removal에 관한 실험도 시행했다.

    <img src="/assets/image/relationnet/table3.png" width="600px" height="450px" title="title" alt="title">

    <img src="/assets/image/relationnet/table4.png" width="600px" height="450px" title="title" alt="title">

    - Table 4는 training 및 testing시 detection threshold 설정이 다를 경우, 성능 하락이 있다는 것을 나타내는 결과값이다.

    - 제안 네트워크는 영향을 덜 받는다.

  - ## 5.3. End-to-End object detection

    <img src="/assets/image/relationnet/table5.png" width="600px" height="450px" title="title" alt="title">

    - 위 결과는 기존 네트워크 + Soft NMS / 기존 네트워크 + RM + Soft NMS / 기존 네트워크 + RM (Instance recognition) + RM (Duplicate classification)의 결과를 나타낸다.

    - 적용한 세 가지 네트워크에서 모두 성능 향상을 보여준다.

# 6. 후기

  - 정말 정말 안읽어지는 논문이었다. 한 챕터 읽고 쉬었다가 또 읽고 쉬었다가.. 몇 시간을 썼는지 모르겠다.. 논문 순서도 엉망인 것 같았고 챕터는 또 왜 저렇게 분류해뒀는지.. 아무튼 힘들었다.