---
title: "[Error]Internal error"
date: 2021-02-24 22:10:00
author: Leechanhyuk
categories: Tip
tags:	Tip
cover: "/assets/instacode.png"
toc: true
---

## 오류 발생 상황

실행 환경 : Windows 10 / Tensorflow==1.8.0 / Keras==2.1.6 사용 / 가상 환경을 따로 만들어서 실행

CPU : I7 - 9850M / GPU : T2000 Quadro

## 오류

InternalError (see above for traceback): Blas SGEMM launch failed : m=802816, n=64, k=32 발생

## 해결 방법

Tensorflow와 CUDA의 Version compativity를 위해서 CUDA 10.1 -> CUDA 9.0으로 Downgrade 했었는데

CUDA 9.0에는 패치가 4개 있었는데, 그 패치들을 수행하지 않아 생긴 오류로 보인다.

나는 4개의 패치를 성공적으로 수행하고 나서는 오류가 발생하지 않았다.

구글링 해 얻은 관련 결과는 아래와 같다.

- 동시에 실행되고 있는 GPU를 사용하고 있는 Process들을 Shotdown한다.

- GPU 사용량을 줄인다. (GPU관련 프로세스가 두 개 이상 실행되어 있을 때 사용량이 전체 용량을 초과하는 경우인 듯하다.)

- Batch size를 줄인다. (역시 GPU 사용량을 줄이기 위해서이다.)









류