---
title: "[Concept summary] Attention & Transformer"
date: 2021-11-08 18:13:00
author: Leechanhyuk
categories: Machine_Learning
tags: Machine_Learning
cover: "/assets/instacode.png"
toc: true
---

## 시작하기전에

  - 본 포스팅은 머리 속 개념 정리를 목적으로, **Attention & Transformer**에 관한 관련 개념들을 간단하게만 정리해본 포스팅입니다.

## 정리사항

  - ### Attention
  
    - Attention mechanism이 등장하기 전, 자연어 처리는 대부분 RNN을 통해 시행되었다.

    - 그 중에서도 특히, Seq2seq network가 주류를 이루고 있었다.

    - seq2seq network란, 입력 Sequence - Context vector - 출력 Sequence의 형태를 가지고 있다.

    - 즉, 입력 sequence의 정보를 context vector에 저장하고, context vector와 각 node의 전 output을 입력으로 받아서 해당 시점의 hidden state 및 output을 배출하는 network이다.

    - 하지만, 하나의 context vector로 모든 input feature의 information을 전부 담기는 너무 작아서, 문장이 길어지면 처음에 입력된 단어가 나중에 입력된 단어에 비해 상대적으로 context vector에서 차지하는 비중이 적어지는 vanishing problem이 대두되었다.

    - 따라서 attention mechanism에서는 하나의 디코더 node에서, 모든 input sequence를 참조하여 예측을 하는 방식을 사용한다.

    - 전체 과정을 간단하게 나타내면 아래와 같다.

      1. 현 디코더 node의 hidden state와 모든 input sequence (길이를 n이라고 하겠다.)의 hidden state를 곱한다.

      2. 결과를 attention score라고 칭하고, 이는 길이 n개의 scalar으로 이루어진다.

      3. n개의 attention score에 softmax를 취하여 각 attention score가 현 디코더의 node의 hidden state와 얼마나 유사성이 짙은 지를 확률과 같이 표현한다. (Attention distribution)

      4. 도출된 n개의 attention distribution을 인코더의 n개의 hidden state와 곱한다.

      5. 결과적으로 인코더의 hidden state들은 현 디코더의 hidden state와 얼마나 관련성이 있는지에 관해 가중치를 곱한 결과값이 된다.

      6. 그 후 모든 결과값들을 더하여, attention value를 만들어낸다. 이는 context vector라고도 불리며, 인코더단의 정보를 해당 디코더 node와의 관계성을 통해 재정립 한 것을 의미한다.

      7. 해당 attention value와 디코더의 hidden state를 concatenation한다.

      8. 해당 결과값을 가중치와 곱한다.

      9. 곱해진 값을 출력층의 입력으로 사용한다.
   