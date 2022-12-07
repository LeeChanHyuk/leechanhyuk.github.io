---
title: "[Concept summary] Maximum likelihood estimation"
date: 2022-11-25 14:30:06
author: Leechanhyuk
categories: Machine_Learning
tags: Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

# Likelihood

  - Likelihood란 입력 데이터가 특정 분포에 속할 확률을 뜻한다.

  - 머신러닝 관점에서 말하자면, 모델을 학습시킨다는 것은 입력 데이터 $(X)$ 를 처리하기 위해서 해당 데이터를 잘 표현할 수 있는 우리의 모델 $(\theta)$, 즉 확률 분포를 계속해서 수정해나간다는 것이다.

  - $L(\theta) = p(X\theta)$
  
  - $\theta = (\mu, \sigma)$

# Maximum likelihood estimation

  - 우리는 주어진 데이터셋을 통해서 확률 분포, 즉 우리의 모델을 튜닝해나가야한다.

  - 이를 위해서 데이터를 넣었을 때 Likelihood가 최대가 되는 모델을 선정해야한다. **즉, likelihood를 maximize 해야한다.**
  
  - 이상적인 환경에서 모든 입력 데이터가 독립적이라고 가정했을 때는 아래와 같은 식으로 나타낼 수 있다.

  - $L(\theta)$ $=p(X|\theta)=\Pi^{N}_{n=1}(x_{n}\|\theta)$

  - 우리가 최대값을 찾을 때 변곡점을 찾듯이, 여기서도 미분을 통해서 최대값을 도출할 수 있다.

  - 하지만 위 식의 우변은 모든 항들이 곱셈의 형태로 이루어져 있어서 미분이 쉽지가 않다.

  - 따라서 좌우변에 log를 취해주고, -를 곱해줌으로써 미분과 동시에 maximization 문제를 minimization 문제로 변형하여 prediction value와 label 간의 차이를 최소하하는 쪽으로 학습을 진행한다.

  - 즉, $-ln\,L(\theta)$ $=-\Sigma^{N}_{n=1}\ln\,p(x_{n}\|\theta)$ 로 표현할 수 있다.

  - 이를 **negative log likelihood** 라고 한다.

# Negative log likelihood

  - 그럼 실제로 머신러닝을 학습시킬 때, 이 부분이 어디에 반영되있냐?

  - 이 부분은 바로 Loss function (=Cost function) 부분에 반영되어 있고, Cross-Entropy 혹은 MSE 등이 대표적인 Negative log likelihood를 활용한 function 들이다.

  - Cross-Entropy의 식은 미리 언급한 negative log likelihood의 식과 매우 유사하다.

  - x라는 input이 있을 때 정답을 q(x), prediction 결과를 p(x)라고 할 때, $-\Sigma_{c=1}^{C}\ q(x)\ \log{p(x)}$

  - 유일하게 다른점은 앞에 q(x)라는 label이 곱해진 형태라는 것인데, 이는 likelihood를 최대로 만드는 $\theta$ 와 likelihood의 기댓값을 최대로 만드는 $\theta$ 가 같기 때문이다.

  - 같은 이유는 기댓값을 찾는 과정에서 일종의 각각의 likelihood를 scaling 하더라도 그건 단지 scaling 한 것일 뿐이지 실제 $\theta$ 값 자체가 완전 바뀌어버리는게 아니기 때문이다.

# 총 정리

  - 입력 데이터를 바탕으로 모델을 튜닝하는 것은, 입력 데이터에 적합된 확률 분포를 찾는 것과 같다.

  - 따라서 다중 입력 데이터의 likelihood가 maximize 되는 방향으로 파라미터를 수정하면, 입력 데이터에 최적화 된 확률 분포 (모델)를 찾을 수 있다.

  - 따라서 likelihood를 최대화하는 Maximum likelihood estimation을 수행한다.

  - 하지만 Likelihood를 최대화하는 과정에 미분이 포함되어 있는데, 이 때 다중 입력 데이터로부터 추정된 likelihood들이 모두 곱하는 형태로 주어져 있어서 미분을 수행하기가 어렵기 때문에 log를 붙여줘서 곱셈을 덧셈의 형태로 변환한다.
  
  - 또한 Maximization 문제를 minimization 문제로 바꿔주기 위해서 Negative log likelihood로 변형하여 학습이라는 문제를 푼다.

  - 이와 같은 학습 방법은 Cross-Entropy 혹은 MSE 형태로써 학습에 나타나게 된다.