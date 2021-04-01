---
title: "[ML] Batch normalization 개요 및 고찰"
date: 2021-03-11 21:05:00
author: Leechanhyuk
categories: paper_Review
tags: Computer_Vision Machine_Learning
cover: "/assets/instacode.png"
toc: true
---

<img src="/assets/image/Batch_Normalization/frontdoor.PNG" width="450px" height="300px" title="title" alt="title">


* * *

## 시작하기전에

본 포스팅은 "Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift" 논문을 토대로

많은 분들의 포스팅 및 영상을 본 후 정리하는 글입니다. 잘못된 것이 있으면 말씀해주시면 감사히 잘 듣겠습니다.

## Batch normalization의 배경
 
 - 딥러닝 학습은 순차적 학습이기 때문에, 앞 단의 잘못된 결과가 뒷단으로 갈수록 더욱더 잘못된 결과를 초래하는 경우가 잦음. (Internal covariate shift)

 - 윗 줄의 문제에 따라서 Learning rate의 제한이 있었음

 - 가중치 초기화(Weight initialization)에 rough해짐.

 - Gradient exploding / vanishing과 같은 문제들이 존재

## Batch normalization이 뭐냐?

 - 학습시 데이터들을 정규화함으로써, 각 Layer의 Input의 distribution을 유사하게 변환하여 학습을 안정적으로 반들어서 전반적인 성능을 향상시키는 방법.

## Batch normalization의 장점
 
 - 전반적으로 Loss가 평탄해지므로, 더욱 높은 Learning rate를 사용 가능하다 (즉, 규제효과가 있다.). -> 학습 속도가 빨라진다는 장점으로 이어진다.

 - 학습 후 Performance 또한 상승한다.

## Batch normalization의 단점

 - 계산량이 늘어난다. (Mean, Variance 계산 등)

 - 각 Mini-batch가 갖는 독립성을 완화시킨다. (학습되어야 할 특징들을 덜 하게 만든다.)

 - Train시와 Test시에 성능 차이가 발생한다.

 ## 참고 의견

 - "How Does Batch Normalization Help Optimization?" (2018, NeurIPS) 에서는 Batch normalization 저자가 주장한 것과는 다르게 Batch normalization이 Internal covariate shift를 줄이지도 않고
 줄인다고 해도, 이건 성능 향상에 critical 하지 않다고 주장.

 - 주장 2) Batch normalization이 성능을 향상시키는 것은 loss를 평탄하게 만들기 때문에 좀 더 큰 step에서도 Gradient의 direction은 유지되기 때문에 그렇다고 주장. (Global minimum을 더 잘 찾을 수 있다는 의미로 받아들였다.)

 - 주장에 따른 근거로는 Normalization 전 후의 레이어의 비교 사진 및 레이어에 Noise를 추가해서 학습시켰을 때에도 (분포가 흐트러졌을 때에도) 학습이 잘 된다는 것을 근거로 제시.






