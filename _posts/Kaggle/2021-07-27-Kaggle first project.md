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

  [Link](/assets/image/Kaggle/Titanik1.html) - Result in titanik competition.

  [Link](/assets/image/Kaggle/Titanik_ensemble.html) - Result for using ensemble method in titanik competition.

# Review

 - 전체 진행 프로세스
   
   - 1. 데이터셋 확인
  
     - 데이터를 확인하고 분류 계획를 세우며, 결측치 확인 및 처리를 진행한다.

   - 2. EDA (Exploratory Data Analysis)

     - 여러 Feature 간의 상관관계를 분석한다.

   - 3. Feature engineering

     - EDA를 진행하여 얻은 Insight에 기초하여, 쓸모없는 feature들을 제거하고, 수정하는 등의 작업을 진행한다. (One-hot encoding, Categorical split 등)

   - 4. Initialize model

     - 모델을 만들고 최적의 Parameter들을 찾는다. (Grid search등의 방법으로)

   - 5. Prediction

     - 만든 모델을 가지고 결과값을 예측한다.

   - 6. Model evaluation

     - 결과치를 분석하여 모델이 어떤 Feature를 중요하게 사용했는지, 어떤 모델이 성능이 잘 나왔는지, 어떻게 개량해야 할 지 (Ensemble 조합을 바꿔본다던가, Hyper parameter를 수정한다거나)

- 1. 데이터셋 확인

  - 데이터를 불러오고, head()나 Describe()를 통해 전체적인 맥락을 파악한다.

  - isnull()을 이용해 결측치를 체크한다.(결측치가 nan일 경우에는)

  - 결측치가 몇개 되지 않을 경우에는 일반적으로 중앙값이나 평균으로 채운다.

  - 많을 경우에는 주변 Feature 중 Correlation이 높은 feature들 중 주변 값이 비슷하면서 결측치가 아닌 경우의 값을 가지고 오거나, 아니면 모델을 만들어서 결측치를 Prediction을 통해 채울 수도 있다. (이럴 경우에는 Feature enginnering 파트에서 채운다.)

  - 또한 IQR을 계산해서 이상치를 먼저 탐색하고 이상치 이내의 데이터들만 사용하는 것도 좋은 방법인 것 같다.

- 2. EDA (Exploratory Data Analysis)

  - 보통 하나의 Feature마다 해당 Feature가 Target에 미치는 영향을 위주로 탐색한다.

  - 탐색은 Matplotlib의 다양한 plot이나 Seaborn의 다양한 plot (bar, count, factor, violin, dist, heatmap 등) 들을 이용해서 Visualization해서 Insight를 가지기 쉽게 해준다. (Seaborn이 조금 더 이쁜 것 같다.)

  - pandas의 crosstab 또한 유용했다.

  - Dataframe을 특정 항목별로 정리할 때, df.groupby(['Feature'])를 많이 사용했다.

- 3. Feature enginnering

   - 결측치 채우기

   - Feature 내 항목들을 범주형 데이터로 바꾸거나(df.loc을 사용하거나 함수를 Design해서 apply 함수를 사용하거나), str을 num으로 바꾸어주는 등의 작업을 진행하거나, One-hot encoding 등을 진행한다.(pd.get_dummies 활용)

   - 필요없는 Feature들을 drop시킨다. (train.drop(['Feature name']))

- 4. Initialize model

   - 주로 Sklearn 라이브러리 내 모델들을 사용한다. (SVM, RandomForest, MLP 등등)

   - 나중에는 최신 논문에 탑재된 모델들을 사용해야 할 듯 하다.

   - Ensemble을 자주 사용한다. (XGBoost등 Gradient Boosting 계열의 알고리즘이 많이 사용됨.)

- 5. Prediction

   - 학습을 진행하고, testset에다가 prediction을 진행한다.

   - 결과값을 확인하고, Importance feature을 check 한다.(model.feature_importances_)

 - 6. Model evaluation

   - 만들어둔 모델의 Feature importance를 활용해서, 어떤 feature를 사용했을 때 결과가 좋았고, 그 결과가 좋았다면 feature engineering 과정에서 어떤걸 잘 해서 좋았는지, 좋지 않았다면 비교적 결과가 좋았던 feature과 비교해서 어떤 방식을 사용해서 feature engineering 쪽을 진행하였는지 등을 확인해야한다.

   - 또한 ensemble을 활용하였다면 어떤 모델들을 사용했는지, 그리고 feature importance 측면을 생각했을 때, 너무 비슷한 feature들을 중요하게 사용하는 모델을 사용한건 아닌지 (다양한 측면을 고려해야 결과가 잘나온다는 kaggle winner들의 말이 있다.)를 생각해봐야한다.

   - 또한 drop 시킨 feature들의 중요성을 다시 생각해보고, 다른 Feature들이 정말 필요한지, 어떻게 engineering을 고치면 좋을지를 다시 생각해보자.

   - 마지막으로 전체적인 방향성에 대해서 살펴보자. 예를 들자면, 본 Competition에서 중요하게 작용되는 것이 무엇일지. Data noise를 제거하는게 키포인트라서 그에 맞게 Auto-encoder based model을 사용한다거나 하는 Competition의 핵심을 말이다.

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
     
      <img src="/assets/image/titanik/adaboost.png" width="600px" height="400px" title="title" alt="title">
    
    - ### 3.2. Gradient Boost

      - Adaboost 대신에 Gradient descent Algorithm을 적용하여 약-분류기의 Weight를 업데이트 시키는 방식.

      - 느리고, Over fitting의 위험성이 있다.

      <img src="/assets/image/titanik/gboost.png" width="600px" height="400px" title="title" alt="title">

    - ### 3.3. XGBoost

      - XGBoost는 Gradient Boost + Regularization + Early Stopping 이라고 생각하면 된다.

      <img src="/assets/image/titanik/xgboost.png" width="600px" height="400px" title="title" alt="title">
