---
title: "[Curiosity] Faster-r-cnn Anchor 개념 의문점 정리"
date: 2021-05-04 11:30:06
author: Leechanhyuk
categories: Machine_Learning
tags: Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

Faster-r-cnn anchor 의문점 정리

- RPN 구성 : Input image -> Backbone -> 1 padding -> 3x3 convolution (여기서 Anchor 적용) -> 1x1 convolution -> Classification 및 Bounding box regression에 대한 feature map (WxHxkx2 및 WXhxKx4) 생성 -> Classificaiton 결과가 좋은 상위 K개의 Anchor를 남김 -> K개의 Anchor에 NMS를 적용 -> Bounding box 도출

- 의문점 1 : 3x3 convolution에서 어떻게 Anchor를 적용하는가? Anchor의 개수는 논문을 예시로 들면 9개인데, Feature map 내 한 픽셀에 대해서 9개의 Convolution 결과를 Average 취하는 것도 아니야, Feature map의 채널 수가 9배로 늘어나는 것도 아니야.

- 의문점 2 : 만약 Anchor로 Feature map을 직접 만드는게 아니라, 그냥 그 값을 가지고 있는다고 하면 어떻게 학습이 되지? Back propagation이 일어날 수가 없잖아 feature map이 없는데.

- One-stage와의 차이점 : 역시 Anchor를 모두 학습시키느냐, 안그러면 IoU를 봐서 부분적으로 학습시키느냐가 차이인 듯. 만약 마지막 단에서 IoU를 보고 학습시키면? Two-stage보다 좋지않은점이 있는가?



