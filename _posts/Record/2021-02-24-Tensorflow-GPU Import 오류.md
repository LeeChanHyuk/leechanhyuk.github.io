---
title: "[Error] Tensorflow-gpu import 관련 오류"
date: 2021-02-24 22:10:00
author: Leechanhyuk
categories: Record
tags:	Record
cover: "/assets/instacode.png"
toc: true
---

## 오류 발생 상황 

Pytorch만 사용하다 오래간만에 Tensorflow를 사용할 일이 생겼다.

진행하는 도중에, 오랜만에 Import 관련 에러가 났다.

분명 예전에도 겪었던 적 있었던 오류인데, 다시 보니 기억이 잘 나질 않았다.

에러 보자마자, 구글링부터 했다.

지나고보면 참 안좋은 습관인 듯 하다.

에러를 제대로 파악하고, 내 상황을 알아야 필요한 글귀들을 선별적으로 받아들일 수 있을텐데

차근차근 보는 습관을 들이자.

아무튼 이런 상황에 잘 대처하기 위해 오류 및 해결 방법을 기록해둔다.

실행 환경 : Windows 10 / Tensorflow==1.12.0 / Keras==2.1.6 / 외 몇 개 / 프로젝트 가상환경을 따로 만들어서 돌림.

## 오류

<img src="/assets/image/20210224a/error.png" width="450px" height="300px" title="title" alt="title"> 

## 해결 방법

Tensorflow와 Cuda version이 맞지 않았다.

Tensorflow 1.12.0 - Cuda 9.0 - cudnn 7.14를 설치해서 해결했다.

구글링시에 나왔던 다른 오류 해결법은 아래와 같다.

- Pycharm의 버전 업그레이드로 인한 오류 (이 경우는 Prompt로 실행시에는 해결되었다. 즉 IDE의 오류일 수도 있다는 생각만 가지도록하자)

- Visual studio의 재배포 패키치 설치 미완료로 인한 오류 (재설치하면 되는 사람들도 있었다) - Windows에서 Python 사용 시 발생 가능

- CUDA 환경 변수 설정 안했을 때 (특히, 여러 버전을 설치했을 경우에는 꼭 점검해보자.)



