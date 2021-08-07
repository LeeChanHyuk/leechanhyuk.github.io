---
title: "[Object detection] You Only Learn One Representation: Unified Network for Multiple Tasks Review [ENG]"
date: 2021-07-16 21:05:00
author: Leechanhyuk
categories: Paper_review
tags: Computer_vision
use_math: true
cover: "/assets/instacode.png"
toc: true
---

* * *

**you only learn one representation: unified network for multiple tasks paper review**

<img src="/assets/image/yolor/front.png" width="600px" height="450px" title="title" alt="title">

# 0. Abstract

  - People understand the world through vision, hearing, and so on. So human has huge database making intuition even if it's unseen task.

  - The unified network consisted by implicit and explicit knowledge can perform multiple task on catching pysical meaning.

# 1. Introduction

  - Unlike CNN, a human's implicit knowledge can help to perform various tasks.

  - In this paper, the knowledge directly corresponded with observation is called **"Explicit knowledge"** and, the knowledge that has nothing to do with observation is called **"Inplicit knowledge"** in this paper.

  - The proposed network is unified by implicit knowledge and explicit knowledge and, enable to the learned model to contain a general representation.

  - The proposed network is constructed by compressive sensing and deep learning.

# 1.1. Contribution

  - The authors propose a unified network that can accomplish various tasks and the network effectively improves the performance of the model with a very small amount of additional cost.

  - The authors introduce a various things into the implicit knowledge learning process and verify their effectiveness.

  - The authors discussed the ways of multiple tools as a tool to model implicit knowledge.

  - The authors confirmed that the proposed implicit representation learned can accurately correspond to a specific physical characteristic.

  - Combined with state-of-the-art methods, the proposed model achieved comparable accuracy as Scaled-YOLOv4-P7 on object detection and the inference speed has been increased 88%.

<img src="/assets/image/yolor/figure2.png" width="600px" height="450px" title="title" alt="title">

# 2. Related work

  - The related work introduce about Explicit deep learning and implicit deep learning and knowledge modeling.

  - ## 2.1. Explicit deep learning

     - Transformer, Non-local networks, Commonly used explicit deep learning.

  - ## 2.2. Implicit deep learning

     - The main categories of implicit deep learning are neural representations and deep equilibrium models.

     - Neural representations: To obtain the parameterized continueos mapping representation of discrete inputs to perform different tasks.

     - Deep equilibrium models: To transform implicit learning into a residual form neural networks.

  - ## 2.3. Knowledge modeling

     - The main categories of knowledge modeling are sparse representations and memory networks.

     - Sparse representations: Describe the specific thing in low dimension than original.

     - Memory networks: To combine various forms of embedding to form memory.

# 3. How implicit knowledge work?

  - In this section, the method of implicit knowledge as constant tensor can be applied to various tasks is mainly introduced.

  - ## 3.1. Manifold space reduction

     - The good representation should be able to find an appropriate projection in the manifold space.

     - So, like figure 3, if the target categories are classified enough, the inner product between implicit representation and projection vector can effectively achieve various tasks.

     <img src="/assets/image/yolor/figure3.png" width="400px" height="300px" title="title" alt="title">

  - ## 3.2. Kernel space alignment

     - The figure 4-(a), (b) illustrates an example of kernel space misalignment in multi-task and aligned example.

     - The misalignment can be deal with translation, rotation, and scaling of output feature and implicit representation.

     <img src="/assets/image/yolor/figure4.png" width="400px" height="300px" title="title" alt="title">

  - ## 3.3. More functions

     - Implicit knowlenge can be extended into many more functions.

     - The one of the example is that neural network can predict the offset of center coordinate.

     - It can be done by a multiplication of hyperparameter to anchors.

     - Figure 5 illustrates the examples of the functions able to be applied by implicit knowledge.

     <img src="/assets/image/yolor/figure5.png" width="400px" height="300px" title="title" alt="title">

# 4. Implicit knowledge in our unified networks

  - In this section, the comparison between conventional networks and proposed unified networks.

  - ## 4.1. Formulation of implicit knowledge

    - ### 4.1.1. Conventional networks

       <img src="/assets/image/yolor/equation1.png" width="400px" height="300px" title="title" alt="title">
     
       - $x$ is observation, $\theta$ is the set of parameters of a neural network, $f_{\theta}$ represents operation of the neural network, $\epsilon$ is the error term, and $y$ is the garget of given task.
 
       - In the conventional networks, the different observation project to the same point (=the same task) if the target of different observation is same. Figure 6-(a) illustrates the problem. 
 
       - For the general purpose neural network, the obtained representation must be able to depict the all purpose. But that is impossible with a trivial mathematical method (One-Hot vector, theshold of Euclidean distance... and so on.). Figure 6-(b) depict the impossible state of an above problem.

       - Figure 6(c) depicts the solution the error term $\epsilon$ to find solutions for different tasks.

       <img src="/assets/image/yolor/figure6.png" width="400px" height="300px" title="title" alt="title">

    - ### 4.1.2. Unified networks
       
       - The following equation is about the unified objective function with implicit knowledge and explicit knowledge.

       <img src="/assets/image/yolor/eqaution2.png" width="400px" height="300px" title="title" alt="title">

       - $\epsilon_{ex} and \epsilon_{im}$ are the operations which modeling the explicit error and implicit error from observation x and latent code z (The feature from encoder).

       - $g_{\pi}$ is a task specific operation that serves to combine or select information from explicit knowledge and explicit knowledge.

       <img src="/assets/image/yolor/equation3.png" width="400px" height="300px" title="title" alt="title">

       - equation 2 can be depicted as equation 3. The Star means the operators that can combine $f_{\theta}$ and $g_{\theta}$. And in this work, the sort of operators are addition, multiplication, concatenation.
       
   - ## 4.2. Modeling implicit knowledge

     - The implicit knowledge can be modeled in the Vector / Matrix / Tensor.

     - ### 4.2.1. Neural Network.

        - The weight matrix can be used for linear combination or non-linaarization with z as the prior of implicit knowledge.

        - The weight matrix can be substitute by more complex neural network or Markov chain.
     
     - ### 4.2.2. Matrix Factorization

       <img src="/assets/image/yolor/equation8.png" width="400px" height="300px" title="title" alt="title">

        - The $Z$ is the implicit prior basis and the $c$ is the coefficient for forming implicit representation.

        - The $C$ can be sparse constraint or non-negative constraint for matrix factorization (NMF) form.

       <img src="/assets/image/yolor/figure7.png" width="400px" height="300px" title="title" alt="title">
   
   - ## 4.3. Training

     - Assuming that our model dos not have any prior implicit knowledge at the beginning, that is to say, it will not have ny effect on explicit representation $f_{\theta}(x)$.

     - When the combining operator star ∈ {addition, concatenation}, the initial implicit prior z ∼ N(0, σ).

     - When the combining operator star ∈ {addition, concatenation}, the initial implicit prior z ∼ N(0, σ).

     - Here, σ is a very small value which is close to zero. As for z and φ, they both are trained with backpropagation algorithm during the training process.
   
   - ## 4.4. Inference

     - Since implicit knowledge is irrelevant with observation $x$, the model $g_{\pi}$ can be constant tensor before inference phase.
       So, implicit information has no effect on computational cost. (Why..?)
     
     - If the operation is multiplication, and the subsequant layer is conv layer, it can be integrated like equation (9).

       <img src="/assets/image/yolor/figure9.png" width="400px" height="300px" title="title" alt="title">
     
     - If the operation is addition, and the subsequant layer is conv layer, no activation layer, it can be integrated like equation (10).

       <img src="/assets/image/yolor/equation10.png" width="400px" height="300px" title="title" alt="title">

# 5. Experiments

  - The experiments is conducted with the MSCOCO dataset which is including object detection, instance segmentation, and so on..!

  - ## 5.1. Experimental setup

     - The implicit knowledge in this experiments, is three aspect.

     - The content is, 1. Feature alignment for FPN, 2. Prediction refinement, 3. Multi-task learning in a single model.

     - The baseline model is YOLOv4-CSP and introduce the implicit knowledge into the model at the position pointed by the arrow in Figure 8. The hyper parameter is set as default of baseline model.

       <img src="/assets/image/yolor/figure8.png" width="400px" height="300px" title="title" alt="title">

  - ## 5.2. Feature alignment for FPN

     - The implicit representation for feature alignment in each FPN layer improves all of the AP. The result is shown in table 1.

       <img src="/assets/image/yolor/table1.png" width="400px" height="300px" title="title" alt="title">

  - ## 5.3. Prediction refinement for object detection

     - The table 2 shows the result of adding implicit knowledge in prediction refinement.

       <img src="/assets/image/yolor/table2.png" width="400px" height="300px" title="title" alt="title">
     
     - In overall objective function of object detection model has position, objectness, class information. So the implicit knowledge is already reflected in almost object detection model.
   
  - ## 5.4. Canonical representaion for multi-task

     - In normal case, for multi-task, the cost function is designed with joint optimization process and this process can make worse the total accuracy.

     - In this experiement, only implicit knowledge reflect the multi-task aspect instead of joint optimization process.

       <img src="/assets/image/yolor/table3.png" width="400px" height="300px" title="title" alt="title">

  - ## 5.5. Implicit modeling with different operators

     - In the implicit knowledge for feature alignment experiment, the addition and concatenation improve performance, while the multiplicaiton makes worse in accuracy.

     - In the implicit knowledge for prediction refinement experiment, the concatenation cannot be stated bacause of change of channel.

     - So, in prediction refinement experiment, applying multiplication is better than applying addition. Because the multiplication can set anchor owns a larger optimization space but addition can affect to coordinate bounded by grid.

     - Table 4 shows the result of this experiment.   

       <img src="/assets/image/yolor/table4.png" width="400px" height="300px" title="title" alt="title">

       <img src="/assets/image/yolor/table5.png" width="400px" height="300px" title="title" alt="title">
   
  - ## 5.6. Modeling implicit knowledge in different ways

     - The result of modeling with neural networks and matrix factorization is shown in table 6.

       <img src="/assets/image/yolor/table6.png" width="400px" height="300px" title="title" alt="title">

  - ## 5.7. Analysis of implicit models

     - In this section, the parameters, FLOPs, and the learning process of model with/ w/o implicit knowledge is described in table 7 and figure 11.

       <img src="/assets/image/yolor/table7.png" width="400px" height="300px" title="title" alt="title">

       <img src="/assets/image/yolor/figure11.png" width="400px" height="300px" title="title" alt="title">

  - ## 5.8. Implicit knowledge for object detection

     - The table 8 shows the effect of introducing implicit knowledge.

       <img src="/assets/image/yolor/table8.png" width="400px" height="300px" title="title" alt="title">

     - The table 9 shows the comparision of state-of-the-art.

       <img src="/assets/image/yolor/table9.png" width="400px" height="300px" title="title" alt="title">

# 6. Conclusion

  - The authors show how to construct a unified network that integrates implicit knowledge and explicit knowledge which is very effective for multi-task learning.

# 7. Impression

  - I understand about what is the implicit knowledge and how to reflect the knowledge to original model. However, i don't know about how the implicit knowledge can be constant tensor in detail..

  - The advantage of this model is to make possible to multi-task learning but there is no result about other than object detection.

  - It was difficult to understand about implicit learning because there were too few empirical explanation in the mathematical equations of implicit learning.
     
