---
title: "[Develop record] Face and eye detector for using EfficientDet"
date: 2021-07-04 11:30:06
author: Leechanhyuk
categories: Develop_record
tags: Develop_record
use_math: true
cover: "/assets/instacode.png"
toc: true
---

# 2021. 07. 25

  - 현재 Regression, Classification, Anchors 함수 학습 및 PPT 화 완료. (Structure.pptx 참조)

  - Feature map을 Anchor로 분할하면, 분할 된 Feature map의 shape이 맞지 않아서, 하나의 Tensor로 나타낼 수 없다.

  - 결국 마지막에는 Feature map의 각 pixel에서 추론을 진행하기 때문에 Reshape 과정이 들어간다는 점을 이용, 각 Feature map마다 따로 Convolution을 진행하고, View 시킨 후에 List에 Append해두고, 나중에 Concatenation 진행한다.

  - Regression, Classification, Anchors, Annotation 전부 순서 및 차원이 같아야 한다.

  - Annotation 같은 경우에는 별도의 절차가 필요할 듯 (**고민 필요**)

  - Feature map을 정사각형으로 만들 필요는 없을 듯