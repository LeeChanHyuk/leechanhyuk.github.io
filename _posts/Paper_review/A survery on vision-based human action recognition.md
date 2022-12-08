---
title: "[Action recognition] A survey on vision-based human action recognition"
date: 2022-12-07 21:05:00
author: Leechanhyuk
categories: Paper_review
tags: ViT Mobilevit Mobile
use_math: true
cover: "/assets/instacode.png"
toc: true
---

* * *

**[Image and Vision Computing] A survey on vision-based human action recognition review**

본 포스팅에서는 Action recognition에 관한 개념 정리 목적으로 리뷰 논문에 관한 요약글을 작성하였습니다.

- # 1. Introduction

  - Offline에서나 Online에서나 Action recognition은 많은 분야에서 응용되고 있다.

  - 본 논문에서는 Action recognition을 수행할 때의 main advantages 및 challenges에 관하여 서술하고자 한다.

  - ## 1.1. Scope of this overview

    - 본 논문에서는 Moeslund et al [90]을 참조하여 person의 movement를 3가지 taxonomies로 정한다.

      - 1. Primitives: Lamb-level (팔, 다리와 같은 부분적 레벨)에서의 atomic한 movement (Ex. Left leg forward)

      - 2. Action: cyclic and whole-body movement (Ex. Running)

      - 3. Activity: Sequential 한 action을 통해 추정되는 movement (Ex. Jumping hurdles)

    - 본 논문에서는 Walking-style 등을 추론하는 Gait estimation과는 반대로, 많은 domain에서 공통적으로 적용할 수 있는 Generalized 한 관점에서 필요한 것들을 서술한다.
  
  - ## 1.2. Surveys and taxonomies

    - 기존 Survey에서의 action recognition은 논문에 따라서 여러 기준으로 나뉘어 서술되곤 한다.

    - 아래와 같은 기준들이 있다.

      - Level: Movement recognition, Activity recognition, Action recognition

      - Methods: 2D approaches, 3D approaches, Recognition

      - Phases: Initialization, Tracking, Pose estimation, Recognition

    - 본 논문에서는 여러 Domain, dataset, evaluation에 따라서 달라지는 Action recognition의 구성 요소에 관해서는 다루지 않는다.

    - 공통적으로 다루어지는 Image representation 및 Action classification에 대해서만 다룬다.

**아직 작성 중인 글입니다.**

