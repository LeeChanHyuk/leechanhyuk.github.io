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

  1. data.groupby()

    - data.groupby(['Sex', 'Survived]) --> 모든 데이터를 Sex와 Survived를 통해 나타낸다. 즉 female - survived 한 사람들을 나머지 카테고리에 대해서 나타내고, male -survived ... 이런 식. 즉 Group by에 하나씩 늘 때마다, 축이 하나씩 는다고 생각하면 편할 듯.

    - data.groupby(['Sex', 'Survived])['Survived']는 위처럼 간추려진 데이터에서 또 다시 Survived에 대한 항목만 추리는 것.

    - data.groupby.count(), data.groupby.mean() 등과 같이, 해당 Label로 묶을 수 있는 상태니까 특정 Label에 대해서 count나 mean을 계산 가능하다.

    - data.count()나 data.mean()과 같이 어떤 label에 대해서 묶을 수 없는 경우는 동작하지 않는다.

  2. data.plot.bar()

    - data.plot.bar(), data.plot.pie(), data.plot.kde(), data.plot.hist(bin=20) 등 plot.Graph의 형태로 나타내어 Data를 해당 Graph로써 표현 가능하다.

  3. sns.countplot(x, y, hue, data, order), sns.factorplot(x, y, hue, data, order), sns.violinplot(x, y, hue, data, order)

    - seaborn 라이브러리를 통해 count를 plot하는 함수. 일단 data.plot.bar()보다 조금 더 Colorfull 한 듯.

    - hue parameter는 groupby와 같은 역할을 하여, data를 해당 Label에 맞게 정리하여 Visualization 해준다.

    - factorplot은 일반 선 그래프로 나타내준다. 나머지는 sns.countplot()과 같음.

  4. pd.crosstab(index, columns, margins)

    - Table 형식으로 표현해주는 함수.

    - Index가 세로축, Columns가 가로축을 의미.

    - margins는 총 합의 표시 유무.

  5. data.value_counts()

    - data의 label마다 counts를 반환해준다. data.count()대신 이걸 사용하도록.

    - 주로 data.value_counts().plot.bar()과 같이 사용되곤 한다.

  6. data.loc[y, x]

    - data.loc['Sex'] -> Sex 행의 데이터를 모두 가져옴

    - data.loc[:, 'Survived'] -> Survived 열의 데이터를 모두 가져옴

    - data.loc['Sex', 'Survived'] -> Sex행, Survived 열의 데이터를 모두 가져옴. 

   