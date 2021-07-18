---
title: "[Tip] Static-Dynamic Library / Visual studio setting for Library"
date: 2021-03-04 14:33:00
author: Leechanhyuk
categories: Record
tags:	Record
cover: "/assets/instacode.png"
toc: true
---

## 환경 

 - Windows 10 / Visual studio 2017

## library_Categories

 - 라이브러리는 원래 코드와 결합하는 방식에 따라서 정적 라이브러리(static_Library)와 동적 라이브러리(dynamic_Library)로 나뉜다.

## static_Library

 - 라이브러리의 동작 코드를 본 코드와 통합하여 작동하는 방식. 

 - 환경에 구애받지 않고 실행 가능

 - 라이브러리의 개수가 늘어나면, 프로그램의 크기도 커짐

 - 메모리 공간 효율 저하 (프로그램마다 라이브러리를 메모리에 올려버리니까)
 
## dynamic_Library

 - 본 코드에서 필요시 사용할 수 있도록 최소한의 정보만 본 코드에서 가지고 있음

 - 프로그램 실행 시 해당 라이브러리 코드를 메모리에 올리고, 본 코드에서는 그곳에 접근해서 동작을 수행하는 방식.

 - 메모리 효율 향상

## static_Library - visual_Studio_Settings

1. 해당 코드에 필요한 헤더 파일이나 코드 파일경로를 추가 (C/C++ - Additional Include Directories)

 <img src="/assets/image/20210304a/Additional.png" width="450px" height="300px" title="MAE" alt="MAE"> 

2. Runtime Library 방식 수정 (C/C++ - Code Generation - MT(Release) / MTd(Debug))

 <img src="/assets/image/20210304a/Runtime.png" width="450px" height="300px" title="MAE" alt="MAE"> 

3. 라이브러리 링킹을 위한 경로 설정 (VC++ Directories / Linker-General - Library Directories / Additional Library Directories)

 <img src="/assets/image/20210304a/linker.png" width="450px" height="300px" title="MAE" alt="MAE"> 

4. 라이브러리 종속성 파일 설정 (Linker - Input - Additional Dependencies)

 <img src="/assets/image/20210304a/input.png" width="450px" height="300px" title="MAE" alt="MAE"> 

## dynamic_Library - visual_Studio_Settings

1. 해당 코드에 필요한 헤더 파일이나 코드 파일경로를 추가 (C/C++ - Additional Include Directories)

 <img src="/assets/image/20210304a/Additional.png" width="450px" height="300px" title="MAE" alt="MAE"> 

2. Runtime Library 방식 수정 (C/C++ - Code Generation - MD(Release) / MDd(Debug))

 <img src="/assets/image/20210304a/Runtime_MD.png" width="450px" height="300px" title="MAE" alt="MAE"> 

3. 라이브러리 링킹을 위한 경로 설정 (VC++ Directories / Linker-General - Library Directories / Additional Library Directories)
 
 <img src="/assets/image/20210304a/linker.png" width="450px" height="300px" title="MAE" alt="MAE"> 

4. 라이브러리 종속성 파일 설정 (Linker - Input - Additional Dependencies)

 <img src="/assets/image/20210304a/input.png" width="450px" height="300px" title="MAE" alt="MAE"> 

5. 환경 변수 설정 (중요) (내 PC - 속성 - 고급 시스템 설정 - 환경변수 - PATH 수정 - 해당 라이브러리 DLL 경로 추가) or (구성 속성 - 일반 - 디버깅 내에서 프로젝트 마다의 환경 설정을 해줘도 무방)

 <img src="/assets/image/20210304a/Environment.png" width="450px" height="300px" title="MAE" alt="MAE"> 




