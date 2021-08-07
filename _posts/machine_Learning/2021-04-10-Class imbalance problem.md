---
title: "[Concept summary] Class-imbalance problem"
date: 2021-04-10 11:30:06
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

 - 이에 대한 해결책으로는 크게 Hard sampling, Soft sampling, Generative methods 총 3가지 방법이 있습니다.

## Hard sampling

 - Object detection 에서 Class-imbalance 문제를 다룰 때 자주 사용되는 방법입니다.

 - 주어진 데이터셋에서 Positive, Negative samples를 선택함으로써 Loss에 반영될 sample들을 선택하여 imbalance 문제를 다룹니다.

 - 조건에 따라 학습되지 않는 데이터가 있다는게 특징입니다.

## Hard sampling - 1. Random sampling

  - 가장 기본이 되는 Hard-sampling은 Random sampling입니다. Original Foreground- Background의 분포를 유지한채 샘플링합니다.

  - Random sampling은 R-CNN 계열에서 Positive sample이 부족할 때, Negative sample중 Random하게 선택하는데 사용됩니다.

  - IoU나 Loss value와 같은 Input box로써의 속성을 고려할 때, 더 좋은 방법들이 있는 것으로 알려져있습니다.

## Hard sampling - 2. Hard-example mining methods

  - 이 방법은 좀 더 학습하기 어려운 특징들을 학습할 때 Detector의 학습이 잘 된다는 가정을 바탕으로하는 방법입니다.

  - 일종의 Bootstrap 방법이라고 볼 수 있습니다.

## Hard sampling - 2.1 Hard-negative mining

  - 동작 방법은 우선 학습을 통해 initialization model을 만들고, 만드는 과정에서 Confidense score가 높았던 Negative 데이터들을 데이터셋에 추가하여 new classifier를 훈련시킵니다.

  - 정리하자면, 학습 시 Background 데이터에서 오분류되었던 Sample, 즉 False positive 중 가장 크게 잘못 분류한 데이터를 데이터셋에 추가하여 훈련하는 방법을 뜻합니다.

  - 이를 Iterative하게 동작시킴으로써 본 방법은 동작합니다.
  
  <img src="/assets/image/Class_imbalance/negative.png" width="450px" height="300px" title="MAE" alt="MAE">
 
## Hard sampling - 2.2. OHEM (Online Hard Example Mining) (CVPR, 2016)

  - Hard-example mining methods의 전체 과정이 너무 느린 점을 개선하기 위해 등장했습니다.

  - 모든 Region proposal(논문에서는 Selective search)들을 forward pass 한 후, 높은 Loss를 가지는 Region에 대해서만 학습시키는 방법입니다.
 
  - Sampling 과정이 없기 때문에, Hard-negative mining 보다 2배 빠른 결과를 보입니다. 

  - 추가적인 메모리 소모(Readonly model)와 학습 속도가 느린 단점이 있습니다.

  <img src="/assets/image/Class_imbalance/ohem.PNG" width="675px" height="450px" title="MAE" alt="MAE">

## Hard sampling - 2.3 IoU Lower Bound (CVPR, 2017)

  - GT와의 IoU가 높게 측정된 Negative sample을 Hard sample로 생각하고, 일정 이상의 IoU를 가진 negavie sample들만 학습에 반영시킨 방법입니다.

  - Positive sample과 Negative sample의 비율을 정해두고 시행합니다.

  - Fast-R-CNN이 대표적인 예시

  <img src="/assets/image/Class_imbalance/iou.png" width="300px" height="200px" title="MAE" alt="MAE">

## Soft sampling

 - Hard sampling과는 달리, 전체 데이터에 관해서 학습합니다.

 - 조건에 따라, 데이터 마다의 학습 정도는 다를 수 있습니다.

 - 여기서 Sampling scale은 $w_i$로 나타냅니다.

## Soft sampling - 1. Focal loss (ICCV, 2017)

 - Sample들의 예측율에 반비례하게 학습시키는 방법입니다.

 - 즉, 학습이 쉬워 예측율이 높게 나오면 Loss에 영향을 덜 주고, 학습이 어려워 예측율이 낮게 나오면 Loss에 영향을 많이 주는 방법입니다.

 - Cross-Entropy Loss 앞에 조절 항을 붙이는 식으로 Loss function을 디자인합니다.

 - $P_s$는 해당 Sample의 예측율을 뜻합니다. $/gamma$는 조절 항의 Loss 기여 정도를 조정하는 Parameter인데, 논문에서는 2로 설정하였습니다.

 <img src="/assets/image/Class_imbalance/focal_loss.PNG" width="450px" height="300px" title="MAE" alt="MAE">
  
## Soft sampling - 2. GHM (Gradient Harmonizing Mechanism) (AAAI, 2019)

 - 대부분의 Easy sample들은 비슷한 Gradient norm를 가진다는데서 착안한 방법입니다.

 - 유사한 Gradient를 가지는 sample이 많을 수록, 해당 sample로부터 도출되는 Loss에 penalty를 줍니다.

 - 아래 공식에서 $BB_i$는 특정 sample, $G(BB_i)$는 해당 특정 sample의 gradient norm에 가까운 sample들의 숫자를 의미합니다. m은 batch 내 전체 bounding box를 의미합니다.

 <img src="/assets/image/Class_imbalance/ghm.PNG" width="450px" height="300px" title="MAE" alt="MAE">

## Soft sampling - 3. PISA (PrIme Sample Attention) (CVPR, 2019)

 - 각 클래스의 Positive sample과 GT간의 IoU를 계산하여 IoU와 비례하게 전체 sample의 rank를 계산합니다.

 - Normalized rank를 아래 식을 통해 계산합니다. $u_i$가 normalized rank, $n_j$는 해당 클래스(j 번째)의 sample 개수, $r_i$는 해당 sample(i 번째)의 rank를 뜻합니다.

 <img src="/assets/image/Class_imbalance/pisa1.PNG" width="450px" height="300px" title="MAE" alt="MAE">

 - 아래식을 통해, sampling scale factor를 계산합니다. $\beta$ 및 $\gamma$ 는Normalized rank가 얼마나 반영될지를 결정짓는 parameter입니다.

 <img src="/assets/image/Class_imbalance/pisa2.PNG" width="450px" height="300px" title="MAE" alt="MAE">

 - 결과적으로 GT와의 IoU가 높은 Positive sample이 더 크게 반영되어서, 마치 robust estimation과 유사한 느낌을 줍니다.

 - 본 네트워크는, OHEM을 Positive sample에 적용했을 때 보다 성능이 뛰어났으며, 본 네트워크와 OHEM 결합하였을 때 더 뛰어난 결과를 보였다고 합니다.
  
 - 직접적으로 precision 결과를 높인 것은 아니지만, IoU를 더 잘 찾게되어서 전체적인 성능 향상이 일어났다고 합니다.

## Soft sampling - 4. Generalized focal loss (CVPR, 2020)

  - Classification 및 Localization을 통합한 Loss(QFL) 및 새로운 Localization function(DFL)의 사용으로 performance를 향상시킨 방법입니다.

  - 앞선 PISA나 IoU-based method들이 너무 Localization쪽에만 치중되어 있어서 성능이 좋지 않다고 비판하였습니다.

  - QFL (Quality Focal Loss)

<img src="/assets/image/Class_imbalance/qfl.PNG" width="540px" height="360px" title="title" alt="title">

  - DFL (Distribution Focal Loss)

<img src="/assets/image/Class_imbalance/dfl.PNG" width="540px" height="360px" title="title" alt="title">

  - GFL (Generalized Focal Loss) = QFL + DFL

<img src="/assets/image/Class_imbalance/gfl.PNG" width="540px" height="360px" title="title" alt="title">

## Foreground-Foreground class imbalance

 - 이것은 Class간의 불균형이 전부 foreground class에서 나타났을 때를 의미합니다.

 - 주로, 데이터셋 자체가 편중되어있거나 (=Dataset-level imbalance), Sampling 방법에 문제가 있을 때 (Mini-batch-level imbalance) 나타납니다.

 - 이것은 데이터셋 자체를 균등하게 제작하거나, Batch를 만들때 OFB(Online Foreground Balanaced) 샘플링 등을 사용하는데, 상당히 직관적인 문제라 다루지 않겠습니다.

