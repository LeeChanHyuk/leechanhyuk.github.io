---
title: "[Concept summary] Jaccard (IoU) Loss"
date: 2021-12-19 13:30:06
author: Leechanhyuk
categories: Machine_Learning
tags: Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

# Jaccard loss란?

  - Jaccard loss란 두 집합 사이의 유사도를 측정하는 방법을 loss function으로 이용한 것을 말하며, 계산식은 아래와 같다.

  <img src="/assets/image/jaccard/formula.png" width="600px" height="900px" title="title" alt="title"> 

  - 위 식을 보면 알 수 있겠지만, 두 집단 (Prediction과 Ground truth) 사이의 교집합 / 합집합의 형태로 이루어지며, prediction이 ground truth와 얼마나 많이 겹치는 지를 판단하는 지표이다.

  - 주로 Object detection 혹은 Segmentation에서 활용된다.

  - IoU 계산하는 것과 식이 같다. 따라서 IoU Loss라고 부르기도 한다.

# 실제 사용 방법 및 유의점

  - segmentation_models_pytorch 라이브러리에서 불러와 사용 가능하며, 간단히 import 및 사용 방법은 아래와 같이 나타낼 수 있다.

  ```python
    import segmentation_models_pytorch as smp

    criterion = smp.losses.JaccardLoss(mode = 'multiclass') # mode는 binary, multiclass, multilabel을 지원한다. 원하는 것에 맞춰서 사용하면 된다.
  ```

  - Mode가 Binary 일 경우

    - binary 일 경우, criterion의 인수인 prediction 및 GT Value의 shape이 같아야한다. 
  
      - *Tensor shape = (batch_size, height, width)*

    - binary 니까, class를 나눌 필요가 없고, 따라서 class에 해당하는 channel 또한 없어서 단순히 prediction value가 얼마나 GT와 같은지를 판단한다.

  - Mode가 multiclass 일 경우

    - Prediction의 경우 multi-class 이니까, 마지막에 prediction map 또한 class의 개수에 맞게 나왔을 것이다. 
    
      - *Tensor shape = (batch_size, class_num, height, width), dtype=float*

    - Ground truth의 경우 multi-class 이니까, 각 tensor value가 class가 될 것이다. 
    
      - *Tensor shape = (batch_size, height, width), dtype=long*

  - Mode가 multilabel 일 경우

    - Prediction의 경우 multi-label 이니까, prediction map은 class의 개수에 맞게 나올 것이다.
      
      - *Tensor shape = (batch_size, class_num, height, width), dtype=float*

    - Ground truth의 경우 multi-label 이니까, 하나의 feature map (batch, height, width) 으로는 여러 개의 label을 표현할 수 없다. (이 부분이 이해가 안간다면, multi class 및 multi label의 차이점의 학습이 필요합니다.) 따라서, class channel이 추가되어야 한다.
    
      - *Tensor shape = (batch_size, class_num, height, width), dtype=long*

  - 주의할 점

    - 보통 segmentation에서는 Multi class의 경우 argmax를 통해서 prediction map을 class num으로 바꾸는 경우가 있는데, 여기서는 argmax 하지말고 바로 넣으면 작동한다.

  - forward

    - segmentation_models_pytorch 내의 jaccard forward 사진을 첨부한다.

    <img src="/assets/image/jaccard/forward1.png" width="600px" height="900px" title="title" alt="title"> 

    <img src="/assets/image/jaccard/forward2.png" width="600px" height="900px" title="title" alt="title"> 
