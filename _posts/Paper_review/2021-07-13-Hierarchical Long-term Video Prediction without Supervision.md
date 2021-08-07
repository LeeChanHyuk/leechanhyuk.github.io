---
title: "[Video prediction] Hierarchical Long-term Video Prediction without Supervision Review"
date: 2021-07-13 21:05:00
author: Leechanhyuk
categories: paper_Review
tags: Computer_Vision Machine_Learning
use_math: true
cover: "/assets/instacode.png"
toc: true
---

* * *

**Hierarchical Long-term Video Prediction without Supervision 리뷰**

<img src="/assets/image/hierarchical_video/front.PNG" width="600px" height="450px" title="title" alt="title">

본 리뷰는 논문의 세세한 요약보다는 논문을 제가 보기 쉽게 요약한 포스팅입니다.

* * *

# 0. Abstract

 - 기존의 연구들은 짧은 가로축 영역에서만 Video를 만드는 것에 성공했었다.

 - 제안 네트워크에서는 Encoder가 Input frame을 Encode하여, 미래의 High-level encoding을 예측한다.

 - 또한 Decoder는 Encoder의 Output으로부터 Future Image를 만들어낸다.

 - Decoder는 fore-ground object를 mask 한다.

 - 기존 연구들과는 다르게, Encoder와 Decoder를 Jointly-Training 한다.

 - Human dataset에서 20초 후의 미래를 예측함으로써 존 연구들보다 더 좋은 결과를 내었다.

# 1. Introduction

 - 기존 연구들은 Interaction을 위한 Intelligent-agent의 제작을 위해 미래의 state를 예측하는데 많은 관심을 보여왔다.

 - 유명한 연구들은 Recursive한 방법을 사용해서 예측을 수행했다.

 - 기존 방법들은 자동으로 Variation의 주된 Factor의 Dynamics를 확인해야했기 때문에 Pixel-level의 Noise에 취약했다.

 - 이런 방법들은 Long-term으로 갈 수록 결과가 좋지 않았다.

 - Hierarchical method (e.g using Face Landmark)들은 High-level input $\to$ High-level output으로 가는 구조이다.

 - Pixel-level prediction 보다 더 Long-term에 적합했다.

 - 하지만 High-level input이 필요한 만큼, 많은 양의 Annotation data가 필요했다.

 - 제안 네트워크는 Predicted frame은 Input으로 사용하지 않는다. *(Decoder는?)*

# 2. Contribution

 - An unsupervised approach for discovering high-level features necessary for long-term future prediction

 - A joint training strategy for generating high-level features from low-level features and low-level features from high-level features simultaneously.

 - Use of adversarial training in feature space for improved high-level feature discovery and generation.

 - Long-term pixel-level video prediction for about 20 seconds into the future for the Human 3.6M dataset.

# 3. Related work

  - ## 3.1. Patch-level prediction

    - Video prediction을 위한 최초의 방법

    - 예측 값이 blockiness (이미지가 checker board처럼 가로 세로 줄이 생기는 현상) image로 나타나는 단점이 있었다.

  - ## 3.2. Frame-level prediction on realistic videos

    - 좀 더 후에는, Convolution layer로 Encoder 및 Decoder를 구성하여 Predict하는 방법들이 제안되었다.

    - 한 연구는, 픽셀 단위의 예측을 한 후에 지역적으로 평균을 취하여 예측을 하는 방법을 사용했다. (L2 Loss 사용)
  
    - 한 연구는, adversarial training 방법을 사용했다.

    - 한 연구는, Video 내 Motion 및 Content를 decompose 함으로써 prediction을 진행하여, 이전 연구들 보다 더 좋은 성과를 내었다.

    - 한 연구는, 차영상으로 Learning을 진행하여 Prediction 하였다.

    - 한 연구는, Stochatic video prediction을 진행하였다.

  - ## 3.3. Long-term prediction

    - 한 연구는, Action conditional encoder-decoder architecture를 사용하여 Video games를 예측하였지만 실 환경에서의 적용은 불가능했다.

    - 한 연구는, Long-term prediction이 가능한 hierarchical method를 사용하였지만, Landmark등의 High-level Annnotation data가 많이 필요했다.

# 4. Background

 - Hierarchical video prediction model을 사용한 한 연구는 High-level feature space에서 video dynamics를 modeling 함으로써 이전 Prediction의는Blur를 감소시켰다.

 - 여기서 사용한 방법은, LSTM을 사용해 초반 C개의 Context frame에서 예측을 진행하고, 예측한 값을 바탕으로 더 미래의 값을 예측한다.

 - 여기서 Input은 Predicted value **(High-level features - Landmark)** 및 Hidden state, Output은 value, Hidden state 이다.

 <img src="/assets/image/hierarchical_video/FIGURE1.PNG" width="400px" height="300px" title="title" alt="title">

 - 추가적으로 본 연구에서는 오직 Hidden state에 의해서만 Prediction 될 수 있도록 $\hat{p_{t-1}}$의 Auto-regressive connection을 제거했다고 한다.

 - 그 후 VAN(Visual Analogy Network)를 적용하여 t시점의 Image를 만들기 위해 Output 및 C 시점의 Input의 Gaussian heatmap의 차이를 C시점의 Input에 적용함으로써 t시점의 이미지를 만들어낸다.

 - 아래 식은 VAN의 과정을 나타낸다.

 <img src="/assets/image/hierarchical_video/FIGURE2.PNG" width="400px" height="300px" title="title" alt="title">

 - 제안 방법은 t 시점의 이미지 복원을 위해 Moving object의 Pixel을 찾기 쉽게 만들어 VAN의 Input으로 삼음으로써 Prediction을 진행한다.

# 5. Method

 - ## 5.1. Network Architecture

    <img src="/assets/image/hierarchical_video/figure3.PNG" width="400px" height="300px" title="title" alt="title">
   
    - $e_{t-1}$은 Input image $I^{t-1}$을 Encoder network에 통과시킨 Output이다.
   (논문에는 $I^t$라고 되어있는데 오타인 듯 하다.)

    - $\hat{e_{t}}$은 LSTM으로부터 예측된 Feature를 의미한다.

    - 위 값들을 이용하여 아래의 식을 통해 예측을 계속 진행한다.

    <img src="/assets/image/hierarchical_video/figure4.PNG" width="400px" height="300px" title="title" alt="title">

    - $f_{enc} : R^d \to R^{s \cdot s \cdot m}$ (Convolutional network for transfer Feature vector to feature tensor)

    - $f_{img} : R^{h \cdot w \cdot c} \to R^{s \cdot s \cdot m}$ (convolutional network for transfer input image to feature tensor)

    - $f_{dec} : R^{s \cdot s \cdot m} \to R^{h \cdot w \cdot c}$ (Deconvolutional network that maps a feature tensor into an image)

    <img src="/assets/image/hierarchical_video/figure5.PNG" width="400px" height="300px" title="title" alt="title">

    - $f_{diff}$ : x, y의 차를 구함

    - [...] : Concatenation in depth channel

    - $f_{analogy} : R^{s \cdot s \cdot 2m} \to R^{s \cdot s \cdot m}$ : Mapping function

    - 즉, 첫 이미지와 첫 feature vector의 차와 t 시점에서 예측되는 feature vector를 concatenation시키고, t 시점에서 예측되는 feature vector와 더한 후에 decoding을 진행한다는 것이다.

    - 위 과정으로 도출된 $I^{-}_t, M_t$를 통해 목표로 하는 이미지인 $\hat{I_t}$를 만들어낸다.

    - 즉, 정리하자면 첫 이미지와 예측하기를 원하는 시점간의 차영상과 예측되는 시점의 주요 feature vector들을 가지고 예측되는 이미지와 그 가중치를 VAN을 통해 예측해내고, 가중치를 바탕으로 첫 이미지와 예측 이미지간의 합성을 통해 예측 이미지를 정확하게 만드는 것이다.
  
  - ## 5.2. Training objective

    - 제안 네트워크에서는 이전 연구와는 다르게 LSTM과 VAN을 Jointly training 시킨다.

    - ### 5.2.1. END-TO-END PREDICTION

      - 첫 번째 방법은 Predicted image와 실제 Image 간에 L2 Loss를 적용하여 Minimize하는 식으로 End-to-End로 학습하는 방법이다.

      - 과정은 아래 사진과 같다.

      <img src="/assets/image/hierarchical_video/figure6.PNG" width="600px" height="450px" title="title" alt="title">
   
      - 본 방법을 사용했을 때는 Prediction image에 Blur가 있었다고 한다.

    - ### 5.2.2. ENCODER PREDICTOR WITH ANALOGY MAKING

      - 일반 L2 Loss를 사용하는 방법의 대안으로는, encoder 또한 같이 훈련시키는 방법이 있다.

      - Encoder의 훈련은 아래 식과 같이 이루어진다.

      <img src="/assets/image/hierarchical_video/figure7.PNG" width="400px" height="300px" title="title" alt="title">

      - 식은 예측 이미지와 실제 이미지의 L2 및 예측되는 encoder output과 실제 encoder output의 L2 Loss를 Minimize하는 식으로 되어있다.

      - 각각의 부분을 띠로 Gradient descent등읗 활용해서 학습시킬 수도 있었지만, summantion 후 Minimize하는 것이 가장 효과가 좋았다고 한다.

      - 또한 식에서의 $\alpha$는 encoder 훈련 강도를 조절하는 Hyperparameter인데, 이게 없으면 predictor 및 encoder가 둘 다 zero-vector를 초래할 수 있다고 한다.

      - 아래 그림은 제안 방법의 Flow를 나타낸다.

      <img src="/assets/image/hierarchical_video/figure8.PNG" width="600px" height="450px" title="title" alt="title">

      - Figure 2의 붉은색 선은 VAN의 학습을, 파란색 선은 Encoder와 Predictor를 학습시킨다.

      - 기존 방식과의 가장 큰 차이점은 학습이 두 가지 부분으로 나누어진다는 것 외에도, VAN의 학습에 참여하는 Input이 Predicted value가 아닌 Ground truth value의 encoding output이라는 점에 있다.

      - 또한 이 방식의 장점은 VAN이 Ground truth frame으로부터 Training 되기 때문에 좀 더 Pixel을 sharp하게 예측가능하다는 것.

      - 물론, Inference시에는 $e_t$대신에 $\hat{e_t}$를 사용한다.

      - 저자들은 이 방법을 EPVA라고 칭한다.

      - EPVA에서는 $\alpha$가 1e-7부터 점진적으로 커지며 학습되는 것이 효과가 좋았다고 한다.

    - ### 5.2.3.**EPVA** WITH ADVERSARIAL LOSS IN PREDICTOR

      - EPVA의 단점은 Prediction 시에 Blurry effect를 초래할 수 있는 L2 Loss를 사용한다는 점이다.

      - 여기에 저자들은 Adverserial Loss를 Predicted encoding 및 Ground truth encoding을 분별하는데 적용했다.

      <img src="/assets/image/hierarchical_video/figure9.PNG" width="400px" height="300px" title="title" alt="title">

      - Predictor에 Gaussian noise variable (*Variable?*) 을 추가하여 학습하였더니 효과가 좋았다고 한다.

      - 게다가 여기에 VAN 역시 훈련시키기 위해서 VAN의 Encoder (???) Output과 predicted encoding을 concatenation하고, VAN의 Encoder Output과 실제 encoding을 concatenation시켜서 역시 discriminator에 넣고 훈련시킴으로써 VAN의 Encoder 역시 훈련시켰다.
     (VAN의 Encoder?)
 
# 6. Experiments

  - 실험은 Human 3.6M dataset 및 Bouncing shape에 관한 Toy dataset으로 실험을 진행하였다.

  - ## 6.1. Long-term Prediction on a Toy Dataset

    - 저자는 Toy dataset으로 제안 방법 및 기존 연구 중 하나로 Training 시켰다.

    - Training은 16-Frames를 예측하도록 했고, Evaluating은 1000-Frames에 대해서 진행하였다.

    - 평가는 정량적 평가를 위해, 예측된 객체의 Shape과 Color가 제대로 유지 되는지를 천 번의 iteration을 통해 측정하였다.

    <img src="/assets/image/hierarchical_video/table1.PNG" width="400px" height="300px" title="title" alt="title">
    
    - 결과는 기존 방법에 비해 훨씬 뛰어난 성능을 보이는 것을 확인할 수 있다. 또한 저자들은 Predicted image를 Figure 3을 통해 나타내었다.

    <img src="/assets/image/hierarchical_video/figure11.PNG" width="400px" height="300px" title="title" alt="title">

    - Figure 3을 통해, 1000 이상의 time-step이 지나도 형태가 잘 유지되는 것을 확인할 수 있다.

    - **저자는 해당 단락 마지막에 1000번 이상의 Time-step이 지나도 Location을 잘 찾는 것은 비 현실적이라고 하고 있지만, 적어도 기존 알고리즘보다 뛰어나다면, Figure 3에 256~258 time-step을 제시한 것 처럼 기존 알고리즘보다 더 Location을 잘 찾는 Time-step을 제시할 수 있었지 않을까 하는 생각이 든다.**

  - ## 6.2. Long-term Prediction on Human 3.6M

    - Human dataset에 대해서는 데이터 셋의 세부 종류에 따라서 각 64개의 이미지를 사용하여 FPS를 6.25로 맞춘 후에 Training을 진행하였다.

    - 32개의 Frames를 예측하도록 훈련하였으며, 실제로는 126개 이상의 Frame을 예측한 결과를 논문에서는 제시하고 있다.

    - 제안 방법은 시작 부분으로부터 0.8초까지의 Frames를 Context 삼아서 20초 가량을 Prediction 가능한 결과를 보였다.

    - EPVA Network의 Encoder 부분은 Pre-trained VGG Net을 사용하였다.

    - 다른 모델과의 비교를 통해, 데이터셋에 관한 파라미터와 그 외 실험 환경을 통일하고 실험을 진행하였다.

    - 그럼에도, 다른 모델과의 비교가 완벽히 공정하지는 않았다고 설명하고 있다.

    - Figure 5는 제안 방법과 기존 연구들을 비교하며, 또한 encoder 및 Predictor의 foreground mask를 나타내고 있다.
   
    <img src="/assets/image/hierarchical_video/figure12.PNG" width="600px" height="450px" title="title" alt="title">

    - Figure 5를 통해, 제안 방법이 기존 방법들 보다는 Localtional 하게나 Shape, Color 면에서 우수하다는 것을 확인할 수 있다.

    - E2E 방식 및 CDNA 방식은 매우 빠르게 Blur 되어버린다. (L2 Loss의 영향으로 추정)

    - ### 6.2.1. PERSON DETECTOR EVALUATION

      - 제안 모델의 사람 형상 복원 정도의 테스트를 위해서 저자는 MobileNet을 COCO Dataset으로 Training 시키고, Prediction image를 판별함으로써 그 정도를 테스트하였다.

      - 테스트 결과는 0~1의 범위로 나타내었고, 이를 'Person score'로 명명하였다.

      - 훈련시킨 Detector의 Ground truth에 대한 Person score는 0.4였다. (너무 낮은 거 아닌가..?)

      - 제안 방법 (EPVA Adversarial Version)은 다른 방법들에 비해서 Person score의 일정함 및 높음을 보였다.

      <img src="/assets/image/hierarchical_video/figure13.PNG" width="400px" height="300px" title="title" alt="title">

    - ### 6.2.2. HUMAN EVALUATION

      - 제안 모델의 Prediction Realistic 평가를 위해 사람에게 직접 두 비디오 중, 어떤게 진짜에 가까운가를 평가하게 했다.

      <img src="/assets/image/hierarchical_video/table2.PNG" width="600px" height="450px" title="title" alt="title">

      - 결과적으로는 EPVA가 다른 모델보다 더, EPVA-Adversarial이 EPVA보다 더 좋은 결과를 내었다.
    
    - ### 6.2.3. POSE REGRESSION FROM LEARNED FEATURES

      - Encoder feature를 통해 Human pose regression 하는데에도 실험을 진행하였다.

      - Encoder의 Output feature를 2-Layer-MLP를 통과시켜서 Regression을 진행하는 방식으로 실험을 진행하였다.

      - 비교대상은 Encoder의 Baseline인 VGG NET으로, 결과는 VGG NET보다 9% 향상된 결과를 보였다.

  - ## 6.3. Ablation Studies

    - 저자는 VAN의 사용이 Prediction quality를 향상시킨다는 것을 증명하기 위해서 VAN을 다른 Decoder로 대체하고 실험을 진행하였다.

    - 이 Decoder는 오직 Encoder의 Output에만 Access 가능하게 하였고, 첫 번째 Frame은 관찰 불가능하게 하였다. (학습에서 배제한 듯)

    - VAN을 사용했을 때에는, 사람 영역을 나타내는 Mask를 제대로 만들어서, First frame에 해당 사람 이미지가 대체되지 않게 만들었다.

    - VAN을 사용하지 않았을 때에는, 32 Frame을 넘어가면 Human region을 제대로 가리지 못해서 First frame으로 부터 사람 영역이 대체되게 만들었다.

    - 또한 EPVA와 E2E 방식을 결합해봤는데, 이건 Output이 blur되는 결과를 초래했다.

    - 또한 Context frame을 5에서 10으로 증가시켜봤지만, 이건 long-term prediction의 성능을 improve시키지는 못했다.

# 7. Conclusion

  - High-level structure annotation이 필요없는 hierarchical long-term video prediction을 제안했다.

  - 제안하는 EPVA 방식은 가끔 Prediction이 사라지는 단점이 있지만, 기존 방법들 보다 Long-term에서 뛰어난 결과를 보인다.

  - EPVA-Adversarial version은 기존 방법들보다 더욱 실감나는 Video를 만들어냈다.

  - Future work로는 feature space에 다른 방법들을 더욱 더 적용해 볼 것이다.