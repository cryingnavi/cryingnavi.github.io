---
layout: post
title: Raspberry Pi Kiosk
date: "2017-03-05 23:16"
---

라즈베리 파이를 키오스크 모드로 돌리는 방법을 알아본다. 현재 해당 문서는 작성 중이며 틈틈히 작성할 계획이다.

### 라즈비안 다운로드
- [https://www.raspberrypi.org/downloads/raspbian/](https://www.raspberrypi.org/downloads/raspbian/)

### 라즈비안 설치

### 라즈비안 로그인

### 부팅시 화면 회전
```
sudo vi /boot/config.txt
add display_rotate=3 ( 시계 방향으로 90도 회전)

disable_overscan=1
overscan_left=-150
overscan_right=-150
```

### 부팅 로그 감추기
```
sudo vi /boot/cmdline.txt
```

### 부팅 이미지 표시하기
```
sudo apt-get install fbi
vi asplashscreen
```

### 라즈비안 자동 로그인 하기

### 크로미움 설치하기

### 크로미움 전체모드로 실행하기