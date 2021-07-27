---
title: "Anaconda virtual environment export and import"
date: 2021-07-27 16:59:00
author: Leechanhyuk
categories: Record
tags:	Record
use_math: true
cover: "/assets/instacode.png"
toc: true
---

# Conda environment export

  - 1. Activate the virtual environment.

  - 2. Type "conda env export > filename.yaml"

  - 3. The filename.yaml is created in the base folder.

  - 4. You can check your env_ name, channels, dependencies, libraries in your filename.yaml.
  
  - <img src="/assets/image/export/result.png" width="200px" height="300px" title="MAE" alt="MAE"> 

# Conda environment import

  - 1. Type "conda env create -f filename.yaml" (If you have a environment which is same with your filename, this command can be refused. If so, you must delete the environment first).

  - 2. Activate the environment that you create and check the libraries in your environment.

  - <img src="/assets/image/export/result2.png" width="200px" height="300px" title="MAE" alt="MAE"> 


