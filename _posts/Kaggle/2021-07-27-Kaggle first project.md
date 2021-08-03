---
title: "[Project 1] Titanik competition"
date: 2021-07-27 11:30:06
author: Leechanhyuk
categories: Kaggle
tags: Kaggle
use_math: true
cover: "/assets/instacode.png"
toc: true
---

# 시작하기전에

  - 개인적으로 공부한 캐글 Competition 관련 복습을 위해 작성한 Jupyter lab 파일들을 올려두는 포스트입니다.

# Titanik competition jupyter lab link.

  [Link](/assets/image/Kaggle/Titanik1.html) 

# Data_visualization_Summary

  - ## 1. data.groupby()

    - data.groupby(['Sex', 'Survived]) --> 모든 데이터를 Sex와 Survived를 통해 나타낸다. 즉 female - survived 한 사람들을 나머지 카테고리에 대해서 나타내고, male -survived ... 이런 식. 즉 Group by에 하나씩 늘 때마다, 축이 하나씩 는다고 생각하면 편할 듯.

    - data.groupby(['Sex', 'Survived])['Survived']는 위처럼 간추려진 데이터에서 또 다시 Survived에 대한 항목만 추리는 것.

    - data.groupby.count(), data.groupby.mean() 등과 같이, 해당 Label로 묶을 수 있는 상태니까 특정 Label에 대해서 count나 mean을 계산 가능하다.

    - data.count()나 data.mean()과 같이 어떤 label에 대해서 묶을 수 없는 경우는 동작하지 않는다.

    <img src="/assets/image/titanik/groupby.png" width="400px" height="300px" title="title" alt="title">

  - ## 2. data.plot.bar()

    - data.plot.bar(), data.plot.pie(), data.plot.kde(), data.plot.hist(bin=20) 등 plot.Graph의 형태로 나타내어 Data를 해당 Graph로써 표현 가능하다.

    <img src="/assets/image/titanik/plotbar.png" width="400px" height="300px" title="title" alt="title">

  - ## 3. sns.barplot(x, y, hue, data, order) sns.countplot(x, (y is the count), hue, data, order), sns.factorplot(x, y, hue, data, order), sns.violinplot(x, y, hue, data, order), sns.distplot(a, bins, hist, ax) , sns.heatmap(data, annot=False)

    - seaborn 라이브러리를 통해 count를 plot하는 함수. 일단 data.plot.bar()보다 조금 더 Colorfull 한 듯.

    - hue parameter는 groupby와 같은 역할을 하여, data를 해당 Label에 맞게 정리하여 Visualization 해준다.

    - factorplot은 일반 선 그래프로 나타내준다. 나머지는 sns.countplot()과 같음.

    <img src="/assets/image/titanik/bar.png" width="400px" height="300px" title="title" alt="title">

    <img src="/assets/image/titanik/factor.png" width="600px" height="400px" title="title" alt="title">

    <img src="/assets/image/titanik/dist.png" width="600px" height="400px" title="title" alt="title">

    <img src="/assets/image/titanik/colormap.png" width="600px" height="400px" title="title" alt="title">

  - ## 4. pd.crosstab(index, columns, margins)

    - Table 형식으로 표현해주는 함수.

    - Index가 세로축, Columns가 가로축을 의미.

    - margins는 총 합의 표시 유무.

    <img src="/assets/image/titanik/crosstab.png" width="600px" height="400px" title="title" alt="title">

  - ## 5. data.value_counts()

    - data의 label마다 counts를 반환해준다. data.count()대신 이걸 사용하도록.

    - 주로 data.value_counts().plot.bar()과 같이 사용되곤 한다.

    <img src="/assets/image/titanik/valuecounts.png" width="600px" height="400px" title="title" alt="title">

  - ## 6. data.loc[y, x]

    - data.loc['Sex'] -> Sex 행의 데이터를 모두 가져옴

    - data.loc[:, 'Survived'] -> Survived 열의 데이터를 모두 가져옴

    - data.loc['Sex', 'Survived'] -> Sex행, Survived 열의 데이터를 모두 가져옴. 

    <img src="/assets/image/titanik/loc.png" width="600px" height="400px" title="title" alt="title">

# Binary classifier

  - ## SVM ('rbf')

    <img src="/assets/image/titanik/rbf.png" width="600px" height="400px" title="title" alt="title">
   
  - ## SVM ('Linear)

    <img src="/assets/image/titanik/linearsvm.png" width="600px" height="400px" title="title" alt="title">

  - ## Logistic regression

    <img src="/assets/image/titanik/logistic.png" width="600px" height="400px" title="title" alt="title">

  - ## Decision Tree

    <img src="/assets/image/titanik/tree.png" width="600px" height="400px" title="title" alt="title">

  - ## K-Neighbor

    <img src="/assets/image/titanik/neighbor.png" width="600px" height="400px" title="title" alt="title">

  - ## Gaussian NB

    <img src="/assets/image/titanik/nb.png" width="600px" height="400px" title="title" alt="title">

  - ## Random Forest

    <img src="/assets/image/titanik/forest.png" width="600px" height="400px" title="title" alt="title">

# Ensemble methods in scikit-learn libraries

  - ## Ensemble이란?

    - 두 개 이상의 모델을 결합시켜 하나의 모델을 만드는 것.

  - ## 1. Voting

    - 다수의 모델의 결과를 투표처럼 사용하여 결과를 추론해 내는 방법.

    - Hard vote: 각 모델이 결과 클래스에 대해서 투표하고, 그를 종합해서 결론을 냄.

    - Soft vote: 각 모델이 결과 클래스를 판정한 확률을 평균내어서, 가장 확률이 높은 것을 결론으로 삼음.

    - Soft vote 방법이 효과가 더 좋다고 알려져 있음.

    <img src="/assets/image/titanik/voting.png" width="600px" height="400px" title="title" alt="title">

  - ## 2. Bagging (Bootstrap Aggregating)

    - 하나의 모델을 복제하여 여러개의 모델로 만들고, 각 모델을 하나의 데이터셋에서 다양하게 샘플링 한 데이터를 가지고 학습시킨다.

    - 이렇게 학습된 모델들을 가지고 Voting을 수행하여 결과를 도출해내는 기법이다.

    - 이 방법에서 데이터 샘플링은 중복을 허용한다.

    <img src="/assets/image/titanik/bagg.png" width="600px" height="400px" title="title" alt="title">

  - ## 3. Boosting

    - ### 3.1. Adaboost

      - 약-분류기부터 시작하여 하나씩 순차적으로 학습하며, 마지막의 분류기를 강-분류기로 학습시키는 것이 목표이다.

      - 학습 과정은 Sequential하게 이루어지며, 지난 Step에서 분류를 실패했던 Sample에 더 높은 가중치를 주는 방식으로 학습하여 정확성을 향상시킨다.

      - 학습 과정이 Sequential한 만큼, 학습 속도가 느린 단점이 있다.
     
      <img src="/assets/image/titanik/adaoost.png" width="600px" height="400px" title="title" alt="title">
    
    - ### 3.2. Gradient Boost

      - Adaboost 대신에 Gradient descent Algorithm을 적용하여 약-분류기의 Weight를 업데이트 시키는 방식.

      - 느리고, Over fitting의 위험성이 있다.

      <img src="/assets/image/titanik/gboost.png" width="600px" height="400px" title="title" alt="title">

    - ### 3.3. XGBoost

      - XGBoost는 Gradient Boost + Regularization + Early Stopping 이라고 생각하면 된다.

      <img src="/assets/image/titanik/xgboost.png" width="600px" height="400px" title="title" alt="title">

   