---
title: "[Tip] Library installation method (ZeroMQ Installation)"
date: 2021-03-04 14:33:00
author: Leechanhyuk
categories: Tip
tags:	Tip
cover: "/assets/instacode.png"
toc: true
---

## 환경

 - Windows 10 / Visual studio 2017

## 방법

 - VCPKG 사용

 - Cmake 사용

 - 둘 중 어떤 방법을 사용해도 무방

## VCPKG

 1. 폴더 생성

 2. git clone https://github.com/Microsoft/vcpkg.git

 3. cd vcpkg

 4. bootstrap-vcpkg

 5. vcpkg.exe 파일 생성 확인

 6. 설치된 폴더를 환경 변수에 등록

 7. vcpkg search package_name 을 검색해서 원하는 패키지 명을 선정

 8. 32 bit -> vcpkg install package_name / 64 bit -> vcpkg install package_Name:x64-windows

## CMake (GUI) 

 1. CMake 설치 (https://cmake.org/)

 2. CMake로 설치해야 하는 파일 다운 (폴더 내에 Cmakelist.txt가 있어야 실행 가능) (예시로 ZMQ 설치를 진행 https://github.com/zeromq/libzmq)

 3. Cmake-gui 실행

 4. Source code 경로 설정 / Binaries 경로 설정

 <img src="/assets/image/20210304/cmake.png" width="450px" height="300px" title="MAE" alt="MAE">
 
 5. Configure -> Yes 클릭

 <img src="/assets/image/20210304/configure.png" width="450px" height="300px" title="MAE" alt="MAE">

 6. Generator 설정 (visual_Studio version 및 운영체제에 따라 설정)

 <img src="/assets/image/20210304/generater.png" width="450px" height="300px" title="MAE" alt="MAE">

 7. Installation 설정 (Dynamic library 방식 사용 예정이라 Build_STATIC 옵션 제거한 상태)

 <img src="/assets/image/20210304/done.png" width="450px" height="300px" title="MAE" alt="MAE">

 8. Generate 버튼 클릭

 9. 해당 폴더 / build 폴더 접속

 10. sln파일 실행

 11. Debug 모드 클릭 - ALL_BUILD 빌드 / Release 모드 클릭 - ALL_BUILD 빌드

 12. 이제 build 폴더 내에 보면 lib 폴더 내에 lib 파일들 있고, bin 폴더 내에 dll 파일들 있음.

 13. 참고로 Include 폴더는 build 폴더 밖에, 맨 처음에 받은 설치하려는 폴더 내에 있음

 14. 설정은 "[Tip] Static-Dynamic Library / Visual studio setting for Library" 글을 참조하기

 15. Done

 참고로 동적 라이브러리 방식을 사용할 때에는 Cmake 방식이 좀 더 잘되는 듯 하다.

 



