---
title: "[Concept summary] AUROC란?"
date: 2021-10-04 18:13:00
author: Leechanhyuk
categories: Machine_Learning
tags: Machine_Learning
cover: "/assets/instacode.png"
toc: true
---

AUROC (Area Under Receiver Operating Characteristic) Curve

# AUROC Curve란?

  - AUROC Curve란 Area Under Receiver Operating Characteristic Curve를 줄여서 부르는 말로써, ROC Curve의 아래 쪽 넓이를 의미하며 이진분류기의 성능을 평가할 때 자주 사용됩니다.

  - 아래는 ROC Curve를 나타냅니다. 여러 개의 선이 있는데, 우리의 모델은 초록색 선이라고 가정하겠습니다.

  - <img src="/assets/image/auroc/auroc.png" width="450px" height="300px" title="title" alt="title"> 

  - 이 때, AUROC는 ROC Curve 아래쪽의 면적이니까 AUROC는 초록색으로 색칠한 면적이 되겠죠?

  - <img src="/assets/image/auroc/auroc2.png" width="450px" height="300px" title="title" alt="title"> 

  - 그렇다면 저 그래프의 x축(False Positive Rate) 과 y축(True Positive Rate)은 뭐고, 해당 그래프는 어떻게 형성되는 걸까요?

  - 
  
# Reference

 - https://glassboxmedicine.com/2019/02/23/measuring-performance-auc-auroc/

