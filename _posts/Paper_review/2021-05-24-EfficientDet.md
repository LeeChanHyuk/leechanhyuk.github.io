---
title: "[ML] EfficientDet : Scalable and Efficient Object Detection"
date: 2021-05-24 21:05:00
author: Leechanhyuk
categories: paper_Review
tags: Computer_Vision Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

* * *

**EfficientDet : Scalable and Efficient Object Detection 리뷰**

<img src="/assets/image/EfficientDet/front.PNG" width="600px" height="450px" title="title" alt="title">

본 리뷰는 논문의 세세한 요약보다는 논문을 제가 보기 쉽게 요약한 포스팅입니다.

* * *

<img src="/assets/image/EfficientDet/figure1.PNG" width="600px" height="450px" title="title" alt="title">

## Abstract

 - Model efficiency는 computer vision 분야에서 계속해서 중요하게 다뤄져왔다.

 - 본 논문에서는, Object detection을 위한 Architecture에 대해 연구하고, efficiency를 향상시킬 때 중요한 Key 몇가지를 제안한다.

 - 쉽고 빠른 Multi-scale feature fusion을 위해 BiFPN을 제안한다.

 - Resolution, depth, width for all backbone, feature network, and box/class prediction networks를 uniformly scale하는 Compound scaling을 제안한다.

 - EfficientDet은 resource constraints에 크게 제약받지 않는다.

 - COCO에서 SOTA를 달성(52.2 AP), 52M Parameters, 325B FLOPS.

## Introduction

 - SOTA Detectors는 Object detection acc에 집중해서 Cost efficiency에 대한 고려가 부족하다.

 - 이 점은 Resource contraint가 강한 실제 분야(Robotics or self-driving car..)에 해당 모델 사용을 힘들게 만들었다.

 - Efficientcy를 강조한 모델은 ACC가 낮았고, 그 중 대부분은 Resource에 대해 강하게 제약되었기 때문에, 여전히 실제 산업에 대한 적용은 힘들었다.

 - 그럼 어떻게 ACC와 Efficiency 둘 다를 잡을 수 있을까?

 - 본 논문에서는 One-stage detector paradigm에 따라서, Backbone, feature fusion, class/box network에 대한 구조적인 측면을 고민했다.

 - 핵심적인 문제는 두 가지가 있었다.

 - **Challenge 1 : Efficient multi-scale feature fusion**

 - 다른 Input feature는 다른 Resolution에서 왔음에도 불구하고, 기존의 FPN은 input Feature를 그냥 더하는 구조였다.

 - 따라서 Output-feature에는 여러 Input features가 균일하게 반영되지 못했다.

 - 제안하는 BiFPN은 Learnable한 weight로 다른 Input features의 중요성?을 배우고, Top-down, Bottom-up 방식으로 반복 적용되며 multi-scale feature fusion을 가능하게 한다.

 - **Challenge 2 : Model scaling**

 - 기존 모델을은 Acc 향상을 위해 규모가 큰 모델이나 High resolution image를 사용하곤 했다.

 - 우리는 Feature network, box/class prediction network를 같이 scaling 하는 것이 Efficiency 및 Accuracy를 동시에 잡기위해 중요하다는 것을 알았다.

 - EfficientNet에서 착안하여, EfficientDet은 Resolution, depth, width for all backbone, feature network, box/class prediction network를 전부 Scaling했다.

 - EfficientDet은 EfficientNet을 Backbone으로 사용했다.

 - EfficientDet은 Figure 1에 나온 어떤 모델보다 가볍고, 특히 D7은 52.2AP, 52M Parameteers and 325B FLOPS와 함께 SOTA를 달성했다.

 - EfficientDet은 기존의 Detectors와 비교해서 3x-8x 빠르다.

 - 모델을 조금 수정해서, EfficientDet은 Semantic segmentation Pascal VOC 2012에서 81.74%의 mIOU Accuracy를 18B FLOPS와 함께 달성했다. (DeepLabV3+보다 1.7% 정확하게, 9.8배 적은 FLOPs와 함께)

## 찾아봐야 할 것

 - FLOPs, FLOPS의 차이.

 - Prediction network란 Bounding box regression 단을 말하는 건가?

 - AP (Average Precision)의 정확한 의미.

## Related work

 - **One-stage detectors**

 - Region proposal이 따로 분리되어 있지 않은 Detectors를 의미.

 - Focal loss, Yolo, Overheat 등이 속함.

 - 본 논문도 여기에 속함.

 - **Multi-Scale feature representations**

 - 

