---
title: "[ML] Transformer"
date: 2021-04-16 21:05:00
author: Leechanhyuk
categories: paper_Review
tags: Computer_Vision Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

* * *

**Transformer**

* * *

이 글은 허민석님의 영상

https://www.youtube.com/watch?v=mxGCEWOxfe8&t=357s

를 보고 개인적으로 간단하게 정리한 글입니다.

* * *

- Transformer는 RNN 기반으로 동작하지 않음.

- RNN의 유명한 예제는 Encoder-Decoder 모델이 있음.

- Traditional Encoder-Decoder는 Context vector의 길이가 정해져 있어(Neuron의 수가 정해져 있음), 유용성이 떨어짐.

- 이와 같은 점의 해결을 위해, Google에서 Attention 기반의 Mechanism이 등장.

- 어텐션 기반의 모델은, 우선 입력 및 출력의 길이가 고정되어 있지 않고, Encoder-Decoder처럼 Encoder단에서 각 hidden-state에서 상태값을 계산한다.

- 그 후, 각 hidden-state의 값으로 부터, Context vector를 나타내고, 출력단의 각 단어에는 그 전 단어의 출력값 + Context vector가 Input으로 작용해, 긴 단어에도 대응 가능하게 했다. 

- 여기서 착안한게, 이 논문의 핵심이 되는 Decoder단 없이, 앞단. 즉 Attention 부분만 살리면 어떨까? 이다.

- 해당 방법의 적용을 통해서, 학습 시간 및 정확도까지 향상시켰다.

- 
