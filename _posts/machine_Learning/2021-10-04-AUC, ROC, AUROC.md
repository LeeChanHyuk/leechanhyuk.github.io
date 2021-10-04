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

  - 그림 1 (Reference 1)

  - <img src="/assets/image/auroc/auroc.png" width="450px" height="300px" title="title" alt="title"> 

  - 이 때, AUROC는 ROC Curve 아래쪽의 면적이니까 AUROC는 초록색으로 색칠한 면적이 되겠죠?

  - 그림 2 (Reference 2)

  - <img src="/assets/image/auroc/auroc2.png" width="450px" height="300px" title="title" alt="title"> 

  - 그렇다면 저 그래프의 x축(False Positive Rate) 과 y축(True Positive Rate)은 뭐고, 해당 그래프는 어떻게 형성되는 걸까요?

# Confusion Matrix

  - Confusion matrix란, 실제 정답을 어떻게 분류했는가에 따라서 나올 수 있는 경우의 수를 행렬의 형태로 나타낸 것을 의미합니다.

  - <img src="/assets/image/auroc/confusionmatrix.png" width="450px" height="300px" title="title" alt="title"> 

  - matrix에서 행은 실제 정답(Ground Truth)값을, 열은 모델이 예측한 값을 의미합니다.

  - 실제 값이 Positive이고 에측한 값도 Positive라면, 올바르게 예측했으므로 TP(True Positive)

  - 실제 값이 Positive인데 예측한 값이 Negative라면, 올바르지 못하게 예측했으므로 FP(False Positive)

  - 실제 값이 Negative인데, 예측한 값이 Negative라면 올바르게 예측했으므로 TN(True Negative)

  - 실제 값이 Negative인데, 예측한 값이 Positive라면 올바르지 못하게 예측했으므로 FN(False Negative)라고 표기합니다.

  - 이 값을 바탕으로 우리가 궁금했던 TPR(True Positive Rate), FPR(False Positive Rate)를 한번 확인해 보겠습니다.

  - ## TPR

    - $\frac{TP}{TP+FN} = \frac{실제 positive인데, positive라고 분류한 경우}{모든 positive 경우}

    - 즉, TPR은 실제 positive인 경우 중에, 제대로 positive라고 분류한 비율을 의미합니다.

  - ## FPR

    - $\frac{FP}{TN+FP} = \frac{실제 negative인데, positive라고 분류한 경우}{모든 negative 경우}

    - 즉, FPR은 실제 negative인 경우 중에, 잘못 positive라고 분류한 비율을 의미합니다.

  - 즉, TPR과 FPR은 모두 positive라고 분류했지만 잘 분류되었냐, 잘못 분류되었냐를 나타내는 것이라고 생각하시면 됩니다.

  - 이 때, Positive, negative를 가르는 threshold를 decision threshold라고 합니다.

  - 이 threshold를 0부터 1까지 변화시키면서 TPR과 FPR의 값을 한 그래프로 나타낸 것이 바로 ROC Curve입니다.

# 마무리

 - 아래 그림은 AUROC Curve에 decision threshold를 0부터 1까지 0.1의 간격으로 나타내었습니다.

 - <img src="/assets/image/auroc/auroc3.png" width="450px" height="300px" title="title" alt="title"> 

 - TPR / FPR 둘 다 decision threshold가 낮아지면 그 만큼 수치가 높아지고, 높아지면 수치가 낮아지는 경향성을 보입니다.

 - decision threshold를 높였을 때, TPR이 FPR에 비해서 수치가 덜 낮아진다면, 그건 model의 성능이 좋은 것을 뜻하며 그래프는 좀 더 상향될 것이고

 - decision threshold를 높였을 때, TPR이 FPR에 비해서 비슷하게, 아니면 더 낮아진다면 그것은 model의 성능이 좋지 않은 것을 뜻하며, 그래프는 좀 더 하향될 것입니다.

 - 따라서 AUROC는 model의 성능에 따라서, 특정 decision threshold일 때 그래프의 y축이 얼마나 높은지가 넓이를 결정짓고, 그 넓이는 모델의 성능을 나타내게 됩니다. 
  
# Reference

 - 1. https://glassboxmedicine.com/2019/02/23/measuring-performance-auc-auroc/

 - 2. https://towardsai.net/p/data-science/how-to-evaluate-you-model-using-the-confusion-matrix

