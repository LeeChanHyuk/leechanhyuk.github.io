---
title: "[Discussion] FLOPS vs FLOPs"
date: 2021-06-04 16:59:00
author: Leechanhyuk
categories: Discussion
tags:	Discussion
use_math: true
cover: "/assets/instacode.png"
toc: true
---

# FLOPS와 FLOPs의 차이

 - FLOPS : **FL**oating point **OP**erations per **S**econd

 - 1초 동안 얼마나 많은 부동 소수점 연산을 처리할 수 있는가?

 - 컴퓨팅 파워(하드웨어)의 성능을 나타내는 지표로써 쓰인다.

 - FLOPs : **FL**oating point **OP**eration**S**

 - 부동 소수점 연산이 몇 번 시행되었는가?

 - 딥러닝에서 모델의 크기를 나타내는데 주로 사용된다.

 - 물론, 적을 수록 더 경량화된 모델을 의미한다.

# FLOPs가 적으면 Inference가 더 빠른가?

 - 이 부분에 대해서는 '모바일 장치에서 캐시 및 메모리 접근이 CNN 성능에 미치는 영향 분석', 한국소프트웨어종합학술대회, 2018 논문을 참조해서 적는다.

 - http://nyx.skku.ac.kr/wp-content/uploads/2018/12/18-719.pdf 

 <img src="/assets/image/FLOPS/one.PNG" width="450px" height="300px" title="title" alt="title">

 - 위 사진대로, FLOPs와 Inference time은 비례하지만, 항상 그 비례 상수가 같지는 않다.

 - 그 이유는, 논문에 따르면 FLOPs 이외에도 메모리 접근 비용 / 병렬 처리 수준등의 다른 변수들이 있기 때문에 FLOPs 하나 만으로는 네트워크를 다양한 면에서 최적화했다고 보기는 힘들다.

 -  