---
title: "[CV] You Only Learn One Representation: Unified Network for Multiple Tasks"
date: 2021-07-16 21:05:00
author: Leechanhyuk
categories: paper_Review
tags: Computer_vision Multiple_task
use_math: true
cover: "/assets/instacode.png"
toc: true
---

* * *

**You Only Learn One Representation: Unified Network for Multiple Tasks Paper review**

<img src="/assets/image/yolor/front.png" width="600px" height="450px" title="title" alt="title">

# 0. Abstract

  - People understand the world through vision, hearing, and so on. So human has huge database making intuition even if it's unseen task.

  - The unified network consisted by implicit and explicit knowledge can perform multiple task on catching pysical meaning.

# 1. Introduction

  - Unlike CNN, a human's implicit knowledge can help to perform various tasks.

  - In this paper, the knowledge directly corresponded with observation is called **"Explicit knowledge"** and, the knowledge that has nothing to do with observation is called **"Inplicit knowledge"** in this paper.

  - The proposed network is unified by implicit knowledge and explicit knowledge and, enable to the learned model to contain a general representation.

  - The proposed network is constructed by compressive sensing and deep learning.

# 2. Contribution

  - The authors propose a unified network that can accomplish various tasks and the network effectively improves the performance of the model with a very small amount of additional cost.

  - The authors introduce a various things into the implicit knowledge learning process and verify their effectiveness.

  - The authors discussed the ways of multiple tools as a tool to model implicit knowledge.

  - The authors confirmed that the proposed implicit representation learned can accurately correspond to a specific physical characteristic.

  - Combined with state-of-the-art methods, the proposed model achieved comparable accuracy as Scaled-YOLOv4-P7 on object detection and the inference speed has been increased 88%.

<img src="/assets/image/yolor/figure2.png" width="600px" height="450px" title="title" alt="title">

# 3. Related work

  - The related work introduce about Explicit deep learning and implicit deep learning and knowledge modeling.

  - ## 3.1. Explicit deep learning

     - Transformer, Non-local networks, Commonly used explicit deep learning.

  - ## 3.2. Implicit deep learning

     - The main categories of implicit deep learning are neural representations and deep equilibrium models.

     - Neural representations: To obtain the parameterized continueos mapping representation of discrete inputs to perform different tasks.

     - Deep equilibrium models: To transform implicit learning into a residual form neural networks.

  - ## 3.3. Knowledge modeling

     - The main categories of knowledge modeling are sparse representations and memory networks.

     - Sparse representations: Describe the specific thing in low dimension than original.

     - Memory networks: To combine various forms of embedding to form memory.

# 4. How implicit knowledge work?

  - In this section, the method of implicit knowledge as constant tensor can be applied to various tasks is mainly introduced.

  - ## 4.1. Manifold space reduction

     - The good representation should be able to find an appropriate projection in the manifold space.

     - So, like figure 3, if the target categories are classified enough, the inner product between implicit representation and projection vector can effectively achieve various tasks.

     <img src="/assets/image/yolor/figure3.png" width="600px" height="450px" title="title" alt="title">

  - ## 4.2. Kernel space alignment

     - The figure 4-(a), (b) illustrates an example of kernel space misalignment in multi-task and aligned example.

     - The misalignment can be deal with translation, rotation, and scaling of output feature and implicit representation.

     <img src="/assets/image/yolor/figure4.png" width="600px" height="450px" title="title" alt="title">

  - ## 4.3. More functions

     - Implicit knowlenge can be extended into many more functions.

     - The one of the example is that neural network can predict the offset of center coordinate.

     - It can be done by a multiplication of hyperparameter to anchors.

     - Figure 5 illustrates the examples of the functions able to be applied by implicit knowledge.

     <img src="/assets/image/yolor/figure5.png" width="600px" height="450px" title="title" alt="title">



     
