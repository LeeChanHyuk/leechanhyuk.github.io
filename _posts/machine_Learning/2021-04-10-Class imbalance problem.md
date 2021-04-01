---
title: "[ML] Class-imbalance problem"
date: 2021-04-01 11:30:06
author: Leechanhyuk
categories: Machine_Learning
tags: Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

## 시작하기전에

 - 본 포스팅은 Imbalance Problems in Object Detection: A Review, Kemal et al, IEEE Transactions on Pattern Analysis and Machine Intelligence, 2020. 논문 중 Class-imbalance problem 파트를 발췌 및 참고하여 적은 포스팅입니다.

## Class-imbalance problem (클래스 불균형 문제) 이란?

 - 클래스 불균형 문제는 학습하고자하는 Class들의 Sample의 개수가 균형을 이루지 못하는 문제를 일컫으며 주로 Object detection 분야에서 자주 대두되는 문제입니다.

## Background

 - Object detection 분야에서는 객체 탐지를 위해서 크게 두 가지 방법으로 나누어 시행합니다.

   1. Top-Down approch (상향식 접근법)

   2. Bottom-Up approach (하향식 접근법)

 - Majority를 이루고있는, 유명한 Detection 모델들은 대부분 1번 Top-Down approach를 사용하고 있고 Bottom-Up approach는 최근에 제안되고 있는 추세라고 합니다.

## Background - Top-Down approach

 - Top-Down approach는 크게 Two-stage methods / One-stage methods로 분류할 수 있습니다.

 - **Two-stage methods**란 Region proposal $\to$ Classification 의 단계로 이루어져 있는 네트워크를 의미합니다. 
 
 - Region proposal이란 Object가 있을만한 지역을 탐색하고 걸러주는 역할을 합니다. Anchor, sliding windows 등을 사용하여 수행합니다.

 - 걸러주는 역할이란, 탐지된 negative sample (원하지 않는 클래스에 속한 Sample)을 다음 단계로 Propose 하지 않는 것을 뜻합니다.
 
 - 걸러진 후보군 중에서 Classification을 통해서 accuracy가 일정치 이상 나오게 되었을 때 그곳을 output으로 내보내는 절차를 따르게 됩니다.

 - R-CNN계열 등이 여기에 속합니다.

 - 아래 그림은 One-stage methods를 나타내는 그림입니다.

<img src="/assets/image/Class_imbalance/one_stage.PNG" width="450px" height="300px" title="MAE" alt="MAE">

 - **One-stage methods**란 Propose 단계를 분리하지 않은 채, 탐색을 수행하는 방법을 의미합니다.

 - 걸려주는 Propose 단계가 없는 만큼, 많은 Region들을 분류해야하고, 그에 따라서 수많은 Negative sample들이 분류되게 되고, 이는 수 많은 Negative sample들이 Loss에 영향을 주는 결과로써 이어집니다.

 - 따라서 일반적으로, Class-imbalance 문제는 One-stage methods에서 크게 대두됩니다.

 - YOLO, SSD, Retina-Net 등이 여기에 속합니다.

 - 아래 그림은 Two-stage methods를 나타내는 그림입니다.

<img src="/assets/image/Class_imbalance/two_stage.PNG" width="450px" height="300px" title="MAE" alt="MAE">

## Background - Bottom-Up approach

 - Bottom-Up approach는 Local한 Features를 먼저 찾고, 이로부터 object를 도출해내는 과정을 뜻합니다.

 - 전체 과정은 Key-point estimation $\to$ class-specific hitmaps to predict corners으로 이루어집니다.

 - 찾아진 local features들은 embedding이나 brute-force 알고리즘등을 통해서 group화 됩니다.

 - 대표적인 논문은 Selectnet, Bottom-up object detection by grouping extreme and center points (CVPR, 2019) 등이 있습니다.

 - 아래 그림은 Bottom-Up approach를 나타내는 그림입니다.

<img src="/assets/image/Class_imbalance/bottom_up.PNG" width="450px" height="300px" title="MAE" alt="MAE">

## Sort of Class-imbalance problem

 - 아래 그림은 Class-imbalance problem의 예시를 나타냅니다.

<img src="/assets/image/Class_imbalance/class_imbalance.PNG" width="450px" height="300px" title="MAE" alt="MAE">

 - 본 그림은 Retina-Net을 통해 실험한 결과물로써, 좌측 그림은 Anchor를 통해 Propose 되는 Background 객체 및 Foreground 객체의 개수를 나타냅니다.

 - 우측 그림은 Foreground내에서 탐지되는 Class의 개수를 나타냅니다.

 - 두 그림을 통해서 Class-imbalance problem은 Foreground 끼리, Foreground-Background간에서 발생할 수 있음을 나타냅니다.

## Foreground-Background class-imbalance problem

 - 논문에서는 본 문제에서는, Foreground를 적게 나타난 클래스로, Background를 과도하게 나타난 클래스로 표현합니다.

 - 이 문제는 특정 Labeling 클래스에 속하는 sample들이 많던 적던간에 발생합니다. 이유는 Labeling 되지 않은 Sample들이 문제이기 때문이죠.

 - 이에 대한 해결책으로는 크게 Hard sampling, Soft sampling, Generative methods 총 3가지에 방법이 있습니다.

## Hard sampling

 - Object detection 에서 Class-imbalance 문제를 다룰 때 자주 사용되는 방법입니다.

 - 주어진 데이터셋에서 Positive, Negative samples를 선택함으로써 Loss에 반영될 sample들을 선택하여 imbalance 문제를 다룹니다.

 1. Random sampling

  - 가장 기본이 되는 Hard-sampling은 Random sampling입니다. Original Foreground- Background의 분포를 유지한채 샘플링합니다.

  - Random sampling은 R-CNN 계열에서 Positive sample이 부족할 때, Negative sample중 Random하게 선택하는데 사용됩니다.

  - IOU나 Loss value와 같은 Input box로써의 속성을 고려할 때, 더 좋은 방법들이 있는 것으로 알려져있습니다.

 2. Hard-example mining methods

  - 이 방법은 좀 더 학습하기 어려운 특징들을 학습할 때 Detector의 학습이 잘 된다는 가정을 바탕으로하는 방법입니다.

  - 처음에는 Negative example로 initialization model을 만들고, 그 과정에서 학습에 실패했던 example들로 new classifier를 훈련시킵니다.

  - 즉, False positive, True negative로 분류된 Sample들을 Training data에 넣어서 훈련시킵니다.

  - 위와 같은 방법은 Bootstrap 방법, 관측된 Random sample들 중에 Resample을 하는 방법, 이라고 합니다.

  - Computing resource가 부족한 시절에 제안된 방법이지만, Loss에 기여도가 크다는 점에서 아직도 사용되는 방법입니다.

  - SSD에서는 Negative example들 중에 Highest value를 가진 example들 (Loss 기여도가 큰 example들)을 사용해서 훈련합니다.

 3. OHEM (Online Hard Example Mining)

  - Hard-example mining methods가 전체 과정이 너무 느린 점을 개선하기 위해 등장.

  - 모든 Region proposal을 forward pass 한 후, 높은 Loss를 가지는 Region에 대해서만 학습시키는 방법입니다.

  - Memory 및 Training speed의 문제가 있습니다.

 

  - 아래 그림은 Class-imbalance problem을 다뤘던 대표적인 방법 4가지 및 그에 따른 논문들을 나타냅니다.

<img src="/assets/image/Class_imbalance/methods.PNG" width="450px" height="300px" title="MAE" alt="MAE">


