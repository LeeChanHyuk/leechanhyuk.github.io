---
title: "[Image recognition] Grounded Language-Image Pre-training"
date: 2022-01-04 21:05:00
author: Leechanhyuk
categories: Paper_review
tags: Object_detection Pharse_grounding
use_math: true
cover: "/assets/instacode.png"
toc: true
---

* * *

**Grounded Language Image pre-training review**

<img src="/assets/image/grounded_language/front.png" width="600px" height="450px" title="title" alt="title">

# 0. Abstract

  - object-level, language aware, semantic-rich visual representations을 위해 object detection - phrase grounding unified network은 GLIP (Grounded language-image pre-training) model을 제안

  - 제안하는 모델은 Detection 및 grounding accuracy를 둘 다 향상 가능하고, self-training 방식을 위해 massive한 image-text pair data를 leverage 할 수 있다.

  - 27M grounding data (3M Human annotated), 24M web-crawled image-text pair data로 훈련하여, COCO 및 LVIS Datasets에서 49.8AP, 26.9AP를 달성했다.

  - COCO Training data로 훈련 후에는 60.8 AP, 61.5 AP를 달성했다.

  - Object detection에 관한 13개의 task 에서, 1-shot transfer learning으로 fully-supervised dynamic head와 비슷한 결과를 냈다.

  - **Idea) Image-text pair로 훈련시킨게 왜 각 task에서 더 성능이 좋지? 두 task 간에 필요로 하는 weight는 서로 다를텐데, 어떻게 상호보완적인 결과가 되는걸까?**

# 1. Introduction

  - Visual recognition의 기존 model들은 pre-determined sample 위주로 학습하여 generalization 성능이 떨어진다.

  - CLIP은 image-text pair data가 image-level visual representation을 학습하는데 좋다는 것을, 학습 후 image classification 혹은 text-image retrieval task에 transfer learning한 결과가 좋다는 것을 보여줌으로써 입증하였다. 하지만 fine-grained feature가 필요로 되는 task (object detection, segmentation ..)에는 부족함이 있었다. (*CLIP이 어떤 네트워크이고, 현 모델과의 차이점 조사 필요*)

  - **본 논문에서는 *Phrase grounding*이 object-level, language-aware, semantic-rich visual representation task에 transfer-learning시에 효과적임을 입증하고자 한다.**

  - *Parase grounding: Object - sentence간의 fine-grained correspondence를 확인하는 작업을 의미*

  - 제안하는 방법은 object detection 및 phrase grounding을 통합함으로써, object detection을 phrase grounding으로, phrase grounding을 object detection task로 보는 방법이다.

  ## 1.1 Contribution

    - 1. Unifying detection and grounding by reformulating object detection as phrase grounding

      <img src="/assets/image/grounded_language/fig1.png" width="600px" height="450px" title="title" alt="title">

      - Detection model의 input을 일반 image에서, 일반 image + image description sentence로 변경하는 방법을 사용했다.
      
      - 또한 dual-encoder 형식을 취하여 두 feature을 곱하는 방식으로 하나의 feature로 통합시키고자 하였으며, 이 방식을 통해 모든 object detection model에 본 방식을 적용 가능하게했다.

      - CLIP은 마지막에 dot product layer를 통해서 두 특징을 고려하는 반면에, 본 architecture에서는 deep fusion 방식을 사용했다.

      - 결과적으로 볼 때, detection 및 grounding side 모두 성능이 향상되었다.

    - 2. Scaling up visual concepts with massive image-text data

      - GLIP 학습을 위해, 27M grounding data(human-annotated fine-grained data (3M) + web crawled data (24M))에 NLP parse를 teacher model로 삼고 pseudo annotations을 진행하여 data를 refine했다.

      - figure 2에서 확인 가능한 것처럼, 본 방식을 통하면 pre-determined label이 아닌 경우에도 prediction이 가능해 LVIS 및 13개의 down=stream task에 관해서 큰 성능 향상을 이끌어 냈다.

      - COCO 2017 val, test에서는 60.8 AP 및 61.5 AP를 이루어냈다.

    - 3. Transfer learning wigh GLIP: one model for all

      - 앞선 두 contribution인, grounding reformulation 및 semantic-rich pre-training을 통해 human annotations 없이도 COCO 및 LVIS에서 좋은 성능을 이끌어냈고, 13개의 object detection 관련 task에서도 좋은 결과를 이끌어냈다.

      - 또한 Object 365를 통해 10-shot supervised baseline보다 GLIP-L로 1-shot training 한 결과가 비슷한 수치가 도출되었다.

      - 또한 GLIP 모델 전체가 아닌, 특정 기능을 수행하는 부분만 부분적으로 fine-tuning 한 경우에도 결과가 뛰어났다.
