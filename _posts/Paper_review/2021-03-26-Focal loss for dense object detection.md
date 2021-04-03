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

* * *

## 논문 관련 정보

 - 추가 예정

## 정리

- 기존의 One-shot detector들의 학습에 사용된 Sample들에는 fore-ground와 back-ground의 비율이 맞지 않았다. (1000배 가량 차이)

- 따라서 학습이 Background example들에만 너무 집중되어 있어, 올바르게 학습이 되지 않았다.

- Two stage detector(Faster-R-CNN, FPN 등)들은 RPN이나 Selective search와 같은 Proposal 단계를 통해 Fore-ground 및 Back-ground의 비율이 1:3이 되도록 하여 Fore-ground의 비율을 지켰다. (저자는 이게 ACC를 높이는 Critical한 요인이라고 생각했다.)

- Two stage detector에만 있던 데이터를 걸러주는 과정을, Loss function을 통해 싷행해서, 속도와 ACC를 둘다 잡자!

- Loss-function은 기존 Cross-Entropy에 $$(1-P_t)^r$$ term을 추가하여 만들었다.

- Stage를 나누는 기준이 Region proposal이 있느냐 없느냔데, 단지 Proposal을 없애고, 다른 방대한 네트워크를 사용할 경우에 속도가 크게 향상되지 않는다면, 이것이 의미가 있는가?

- Negative sample을 클래스로 두면, Label로 할당되지 않은걸 만날 때 마다 그거에 대해서 학습한다.. 즉, Positive sample을 만날 때에는 나머지가 0이 되어서 학습이 안되겠지만
Negative sample 차례가 되면 그거에 대해서 학습할 수 밖에 없으니까 안되지.














