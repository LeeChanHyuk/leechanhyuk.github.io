---
title: "[Tip] Ubuntu 어댑터를 찾을 수 없습니다."
date: 2021-06-15 16:59:00
author: Leechanhyuk
categories: Git
tags:	Record
use_math: true
cover: "/assets/instacode.png"
toc: true
---

# Thinkpad notebook에서 어뎁터를 찾을 수 없습니다 문제 해결 방법.

  - 내 Thinkpad에서 '어뎁터를 찾을 수 없습니다' 문제가 발생했다.

  - 첫 설치 이후에 뜨는건 랜카드를 못잡아서라고쳐도, 멀쩡히 잘 사용하다가 갑자기 안되는 경우가 발생했다.

  - 구글링으로 얻은 솔루션을 업로드해둔다.

  - 1. OS 업데이트 후 해보자.

  - 2. 아래 방법을 시도해보자.

  ```python
    sudo apt update
    sudo apt install git build-essential
    git clone https://git.kernel.org/pub/scm/linux/kernel/git/iwlwifi/backport-iwlwifi.git
    cd backport-iwlwifi/
    make defconfig-iwlwifi-public
    sed -i 's/CPTCFG_IWLMVM_VENDOR_CMDS=y/# CPTCFG_IWLMVM_VENDOR_CMDS is not set/' .config
    make -j4
    sudo make install
    sudo modprobe iwlwifi
  ```