---
title: "[Geometry] Automatic Panoramic Image Stitching using Invariant Features"
date: 2021-12-07 21:05:00
author: Leechanhyuk
categories: Paper_review
tags: Multiview_geometry
use_math: true
cover: "/assets/instacode.png"
toc: true
---

* * *

**Automatic Panoramic Image Stitching using Invariant Feautres review**

<img src="/assets/image/multiview_geometry/front.png" width="600px" height="450px" title="title" alt="title">

# 0. Abstract

  - Panorama stitching에 관련된 이전 연구들은 1D (Single axis of rotation)에서는 많이 다루었지만, 2D나 Multi-row에서는 사람의 보조와 같은 restriction 없이는 제대로 이루어지지 않았다.

  - 본 연구에서는 stitching을 multi-image matching problem으로 formalization하여 풀고자 시도하였기에, 본 연구는 image ordering, orientation, scale, illumination에 민감하다.

# Introduction

  - Camera matrix, homography parameters를 기반으로 하는 기존 정합 방식들은 (Canon, Realviz) initialization process (Aligh, ordering)이 필요했지만, 제안하는 방식은 initialization 없이도 성공적인 정합이 가능하다.

  - Image stitching 연구는 Registration과 같은 initialization을 모두 사용하는 **direct 방식**과 Initialization 없이 feature만 가지고 정합하여, Invariant property가 좀 부족한 feature based method로 나눠진다.

  - 본 논문에서는 Invariant한 feature based method 및 efficient한 bundle adjustment method를 사용하여 아래와 같은 이점을 얻는다.

    1. Rotation, zoom, illumination 조건 변화에 강건하다.

    2. Panorama stitching 문제를 multi-image matching problem으로 formulate함으로써, dis-ordered images도 잘 정합할 수 있다.

    3. Multi-band blending을 통해서 seamless한 output을 만들어낸다.

# Feature matching

  - SIFT는 Local gradients의 Orientation histogram을 바탕으로 similarity를 계산하기 때문에, edge가 slightly move되더라도 descriptor에는 영향을 주지 않을 수 있다.

  - 추가적으로 Gradient를 이용함으로써 illumination을, normalize를 이용함으로써 gain을 방지할 수 있다.

  - SIFT는 Rotation, scale change에 invariant하지만 orientation이나 zoom에는 대응할 수 없다.

  - 카메라의 광축을 기준으로 camera가 rotate한다고 가정했을 때, transformation group은 homography group으로 나타낼 수 있고, 각 카메라를 rotation vector $\theta = [\theta_1, \theta_2, \theta_3], f (Focal_length)$로 파라미터화해서 나타낼 수 있다.

  - 이미지 i 및 j의 homography는 아래 식을 통해 얻어 낼 수 있다.

  <img src="/assets/image/multiview_geometry/equation1.png" width="600px" height="450px" title="title" alt="title">

  - 각 파라미터는 equation2, 3와 같이 나타난다.

  <img src="/assets/image/multiview_geometry/equation2.png" width="600px" height="450px" title="title" alt="title">

  <img src="/assets/image/multiview_geometry/equation3.png" width="600px" height="450px" title="title" alt="title">

  - 이상적인 경우에는 위와 같은 방식으로 homography를 얻을 수 있겠지만, equation 4와 같은 조금의 이미지 위치 변화만 일어났을 경우, equation 5와 같은 affine matrix를 통해 정합 가능함을 의미하고, 이런 상황에서는 SIFT가 적합하다.

  <img src="/assets/image/multiview_geometry/equation4.png" width="600px" height="450px" title="title" alt="title">

  <img src="/assets/image/multiview_geometry/equation5.png" width="600px" height="450px" title="title" alt="title">

# Image matching

  - Image matching의 목적은 모든 이미지의 pair 및 특징점 matching을 진행하는 것이다.

  - Image geometry를 구하기 위해서는 적은 수의 overlapping image만 있으면 된다.

  - 본 논문에서는, 원본 이미지와 matching point가 충분히 있는 image 6장을 사용하여, feature extraction을 진행하고, RANSAC을 이용하여 Refine한 후에 Homography를 구했다.

  - ## Robust Homography Estimation using RANSAC

    - 


