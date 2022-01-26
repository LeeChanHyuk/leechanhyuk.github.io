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

      - 
