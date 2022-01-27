---
title: "[Conference paper summary] FG 2021 paper short review in HCI field"
date: 2022-01-26 21:05:00
author: Leechanhyuk
categories: Paper_review
tags: FG_2021
use_math: true
cover: "/assets/instacode.png"
toc: true
---

* * *

**FG 2021 : IEEE International Conference on Automatic Face & Gesture Recognition PAPERs short review**

# 본 포스팅은 FG 2021 Conference 내 Interaction 관련 논문에 관한 개인적인 간략 정리 포스팅임을 알려드립니다.

# 0. Conference attributes

  - Research Impact Score: 7.59

  - Papers published by Top Scientists: 142

  - Contributing Top Scientist : 76

  - Research Ranking (Computer Science) : 40

  - Reference: Research.com

# 1. Paper list

  - 1. [FG, 2021] Beyond the Words. Analysis and Detection of Self-Disclosure Behavior during Robot Positive Psychology Interaction

  - 2. [FG, 2021] Body Gesture and Head Movement Analyses in Duadic Parent-Child Interaction as Indicators of Relationship

  - 3. [FG, 2021] Skeleton-based Action Recognition for Human-Robot Interaction using Self-Attention Mechanism

  - 4. [FG, 2021] The Importance of Qualitative Elements in Subjective Evaluation of Semantic Gestures
  
# 2. Abstract

  - ## 2.1. Beyond the Words. Analysis and Detection of Self-Disclosure Behavior during Robot Positive Psychology Interaction (from MIT Media Lab)

    - 정신적 건강 상태 분석을 위한 비언어적 자기 소개 행동 분석 방법

    - Vocal acoustic features, head orientation and body gestures/movements based multi-modal behavior analysis based on human-robot interaction data

    - Self-disclosure 관련 긍정적인 반응을 81%의 정확도로 분석할 수 있었으며, Human-AI Interaction에서 real-time으로 적용 가능.

  - ## 2.2. Body Gesture and Head Movement Analyses in Duadic Parent-Child Interaction as Indicators of Relationship (from MIT Media Lab)

    - Parent-child nonverbal communication plays a crucial role in understanding their relationships and assessing their interaction styles.

    - The behavior of interaction is divided into child temperament, parenting style, parenting stress, and home literacy environment.

  - ## 2.3. Skeleton-based Action Recognition for Human-Robot Interaction using Self-Attention Mechanism (from Technical University of Chemnitz, Germany)

    - Human-robot interaction에서는 Motion prediction 및 action recognition이 중요하게 작용한다.

    - 기존의 action recognition network들은 skeleton pre-defined structure를 사용하였지만 (미리 정해둔 행동 등), 본 논문에서는 supermarket scenario에서 사용될 수 있는 small-scale action dataset을 제시한다.

    - 분석을 위해 extensive experiments를 시행했다. NTU RGB-D Dataset을 활용

  - ## 2.4. The Importance of Qualitative Elements in Subjective Evaluation of Semantic Gestures (from Glasgow university)

    - Face-to-Face interaction에서는 gesture이 중요한 역할을 담당하며, conversational agents를 만드는데 semantic gesture를 만드는 것은 중요한 challenges이다.

    - Gesture database를 활용한 두 실험을 통해서, viewer의 semantically-related gestures measurement를 진행하였다.

    - 첫 실험에서는 co-speech - perception of energy level의 correlation을 증명하였다.

    - 두 번째 실험에서는 gesture의 semantic information을 measure하였다.

# 3. Summary

   - ## 3.2. Body Gesture and Head Movement Analyses in Duadic Parent-Child Interaction as Indicators of Relationship (from MIT Media Lab)

    - ### 3.2.1. Introduction

      - 많은 연구에서 'Action'은 중요한 cue로써 작용한다.

      - 이전 연구에서는 'Interaction' 연구에서 'Individual'한 개체에 관해서, 그리고 한 가지 측면에 관해서 (i.e.child temperament)만 연구했다.

      - 본 연구에서는 Dyad에 관해서, 다각적인 분석을 통해 분석했다.
    
    - ### 3.2.2. Method

      - Dataset에 제시된 자료들에 대한 검증

      - Parent Head, body, Child Head, body 정보 추출

      - 공간을 parent body, child body, parent-child body, parent head, child head, parent-child head으로 6개로 나눈 후 feature extraction 진행.

      - 각 sub-space의 상위 10% feature만 활용해서 평가를 진행함.

  - ## 3.3. Skeleton-based Action Recognition for Human-Robot Interaction using Self-Attention Mechanism (from Technical University of Chemnitz, Germany)

    - ### 3.3.1. Introduction

      - Action recognition은 Robot이 Human을 보조하는데 있어서 필수적인 기술이다.

      - 본 논문에서는, 3D Skeleton data를 이용하여 Action recognition을 수행한다.

      - 기존 방법 중 RNN을 사용한 방법은 Prediction error가 시간이 갈 수록 누적이 되어 Prediction accuracy가 낮아지는 단점이 있었다.

      - 따라서 본 논문에서는, Feed-forward GCN과 Attention network를 접목한 real-time prediction network를 제시한다.

      - 또한 학습 및 평가를 위해 ASM (Actions in SuperMarket) dataset을 제시했다.
      
      - 그리고 motion과 trajectory feature를 encoding하는 방법을 사용하여, encoding feature 간의 attention을 도출함으로써 feature 간의 correlation을 학습시켰다.

    - ### 3.3.2. Related work

      - #### 3.3.2.1. Human Motion Prediction

        - Human motion prediction에 관한 초기 연구 단계에서는 주로 RNN을 사용했고, Error accumulation이 주된 challenge 였다.

        - Error accumulation을 해결하기 위해서, feed-forward network나 convolutional sequence-to-sequence network를 사용하였지만, long-term은 잘 예측할 수 없는 등의 문제점이 있었다.

        - 또한 Pose를 추정하고 DCT 를 통해서 Transformation 시킨 후에 Graph Convolution Network를 통해서 action을 추정한 연구도 있었다. 하지만 이 또한 Long-term에서는 잘 예측하지 못하는 단점이 있었다.

        - 이러한 점들을 극복하기 위해서, 어떤 논문에서는 attention 기반의 network를 통해 단점을 극복했다.

      - #### 3.3.2.2. Human Action Recognition

        - Human Action Recognition을 위한 초기 연구 단계에서는 추정한 Pose를 기반으로 RNN 혹은 LSTM을 사용했다.

        - 어떤 연구는 Pose 추정 후, pose를 Pseudo 2D Representation image로 만들어서 CNN으로 다시 Feature를 추출하여 LSTM에 넣는 밥법을 사용했다.

        - 어떤 연구는 Pre-defined skeleton feature vector를 GCN에 전파함으로써 Action recognition을 수행하고자 하였지만, feature에 관해서 미리 정해둔 만큼, 학습되지 않은 action을 recognition할 수는 없었다.

        - 또한 최근에는 action과 skeleton sequence를 two stream으로 나누어서 학습시키거나, Spatial-temporal 한 특징을 나누어서 학습시키고 나중에 합치는 그런 연구들이 있었다.

      - ### 3.3.3. Method

        - 본 논문에서는 관찰된 Skeleton pose를 기반으로 GCN을 통해 미래의 pose를 예측하여, Action recognition을 수행한다.

        - #### 3.3.3.1. Input encodings

          - Skeleton sequence는 {현재 프레임의 Joint1 Position - 이전 프레임의 Joint1 Position, ... } 와 같이 구성된다.

          - Feature encoding 방법은 아래와 같다.

            - ##### 3.3.3.1.1. Discrete cosine transformation based encoding

              - Input sequence를 DCT 적용해서 encoding 했다.

            - ##### 3.3.3.1.2. Convolution feature encoding

              - Time-series joint coordinate를 reshape해서 2D Matrix 형태로 만들고, convolution을 적용해서 High-level feature를 추출했다.

        - #### 3.3.3.2. Graph Convolution Network

          - 1. Input을 DCT

          - 2. Adjacency matrix를 활용하는 GCN Network를 Backbone으로 활용

          - 3. InvDCT 후 GT와 비교 수행

        - #### 3.3.3.3. Recognition Head

          - Multi-head attention은 Input component간의 관계성을 학습하는 모듈이다.

          - ##### 3.3.3.3.1. Transformer encoder model

            - GCN의 Output은 Transformer를 거쳐서 classification head (Pool - dense - ReLU - Dropout - dese) layer를 통해 출력을 내보낸다.

          - ##### 3.3.3.3.2. Part based self-attention model

            - GCN 후 Output은 left, right arm, torso, left, right leg로 나누어져서 self-attention을 취하게 된다. 이 때, 각 attention은 병렬로 계산되어 합쳐진다.