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

  - 단순히 한번 정리해보는게 목적이라, 빠른 시간안에 해보기 위해서 허민석님의 유튜브 영상 (https://www.youtube.com/watch?v=mxGCEWOxfe8&t=6s) 슬라이드를 참조하였습니다. 혹시 문제가 된다면 삭제하도록 하겠습니다.

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
   
  - ### Transformer

    - 기존의 어텐션이 seq2seq에 적용되었던 반면, Transformer는 RNN을 사용하지 않고 순수 어텐션 네트워크로 동작.

    - 인코더 - 디코더의 형태로 구성.

    - 문장 번역을 예시로 들겠다.
        
    - 아래는 허민석님의 Transformer 설명 영상 중, transformer의 전체 네트워크 구성을 나타내는 슬라이드를 첨부하였다.

    <img src="/assets/image/transformer/positionalencoding.png" width="600px" height="900px" title="title" alt="title"> 

      - #### 입력

        - 입력은 번역하고자 하는 문장 내 단어들을 word embedding으로 변환한 것.

        - RNN과는 다르게, 입력이 순차적이 아니므로 포지셔널 인코딩이라는 방법을 사용하여 입력되는 단어의 순서를 알 수 있게한다.

        - 포지셔널 인코딩이란 인코더와 디코더의 입력 모두에 더해지는 벡터로써, sin 혹은 cos 함수를 이용하여 **전체 문장에서 현재 단어의 상대적인 위치**을 나타낼 수 있게 하여 문장 내에서 단어의 위치를 네트워크가 파악할 수 있게 해준다.

        - 삼각함수를 통해 포지셔널 인코딩을 구현함으로써, 위치 정보를 -1 ~ 1사이의 값으로 나타내어 특정 weight 값이 지나치게 커지는 것을 방지할 수 있고, Training data보다 더 긴 문장이 들어와도 잘 대응할 수 있다는 장점을 가진다.

        - 전체 네트워크 구성 중, 흰색 및 회색으로 구성된 벡터가 바로 positional encoding을 의미한다.

      - #### 인코더

        <img src="/assets/image/transformer/encoder.png" width="600px" height="900px" title="title" alt="title"> 

        - 인코더의 목적은 입력된 문장 속 특정 단어가 어떤 단어와 연관성이 짙은지를 탐색하고, 연관성을 행렬의 형태로 나타내는 것이다.

        - 연관성을 찾기 위해서 self-attention 과정을 수행한다. 수행 과정은 아래와 같다. (Self-attention인 이유는, 자기 자신과의 관계도 구하기 때문이다.)

        1. Word embedding과 query, key, value weight와의 행렬 곱을 통해 query, key, value를 구해낸다.

        2. Query 및 Key vector의 곱을 통해 attention score 도출.

        3. attention score를 key dimension으로 나눔. (key dimension이 늘어날 수록, attention score의 값이 너무 커지니까)

        4. softmax를 적용하여, attention score의 distribution을 도출.

        5. 해당 distribution과 Value 값의 곱 연산.

        6. 결과물을 모두 더하여 attention value 산출.

        - 이와 같은 과정을 통해, 한 단어와 문장 속 전체 단어간의 관련성을 함축시킨  하나의 attention value를 만들어낼 수 있게 된다.

        - 트랜스포머 논문에서는 이 attention process를 다중으로 수행한다. (논문에서는 8개)

        - 왜 여러개를 사용해야할까? -> 여러개를 사용하게되면 하나의 정보를 좀 더 다양한 관점에서 분석할 수 있기 때문이다. 마치 model을 ensemble하면 더 좋은 성능이 나오는 것 처럼 말이다.

        - 여러개의 attention value들은 concatenation되어 weight와 곱해지고, 다시 fully connected layer와의 곱을 통하여 최종적으로는 **원래 word embedding과 같은 차원의 output**을 만들어낸다.

        - 각 과정에 information flow를 원활히 하기 위해서, residual connection도 중간 중간에 더해져있다.

        - 인코더는 인풋과 아웃풋의 차원이 같기 때문에, 여러개의 인코더를 붙여서 사용할 수 있으며, 논문에서는 6개의 인코더를 연속적으로 붙여 사용했다.

        - **각 인코더는 가중치를 공유하지 않는다.**

      - #### 디코더

        <img src="/assets/image/transformer/decoder.png" width="600px" height="900px" title="title" alt="title"> 

        - 디코더의 입력은 sos부터 eos까지 RNN에서 사용하던 것과 같은 입력이 들어가고, 추가적으로 Encoder의 output도 사용을 한다.

        - 디코더의 출력은 물론 번역된 단어이며, 한 단어 씩 순차적으로 번역한다.

        - 디코더의 attention은 인코더와 매우 유사하다. 똑같이 multi-head attention 및 fc layer를 적용하는 형태로 되어있다.

        - 다만, 입력에 관한 attention, encoder의 output에 관한 attention이 적용된다.

        - 입력에 관한 attention은 디코더 layer가 아직 순서가 돌아오지 않은 단어를 예측하는 것을 막기 위해서, 입력에 아직 순서가 되지 않은 embedding은 masking 처리를 하고 진행하게 된다.

        - encoder의 output attention은 decoder의 input을 query로, encoder의 output을 key와 value로 나타낸다.

        - 이와 같이 사용하는 이유는, 현재 번역해야 할 단어 (Decoder의 input)가 전체 문장 중 어디와 관련성(Encoder의 output)이 있는지를 나타내기 위함이다.

        - Decoder는 역시 6개가 순차적으로 붙여져서 사용된다.

        - Decoder의 output은 linear layer를 거쳐 softmax를 통해 확률값으로 변형되어 최종 번역된 단어를 출력하게 된다.

## Reference

 - https://www.youtube.com/watch?v=mxGCEWOxfe8&t=6s