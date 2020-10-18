---
layout: post
title: VirtualBox Shared Folder
date: "2016-12-26 03:10"
categories: etc
tags: [virtualbox]
---

VirtualBox 에서 공유 폴더를 설정하는 방법에 관하여 알아본다.



### VirtualBox 관리자에서 공유폴더 설정
![sharedfolder](/assets/images/2016-12-26/virtualbox-sharedfolder.png)
[설정]

### 우분투 실행 후 아래 명령어 입력

```
sudo apt-get update
sudo apt-get install build-essential linux-headers-$(uname -r)
sudo apt-get install virtualbox-guest-x11
```

### 게스트 확장 설치
- 우분투가 실행되고 있는 상태에서 상단의 device 에서 Insert Guest Additions CD Image 선택
- 우분투에서 비밀번호를 묻는 창이 뜨면 입력 후 설치


### 공유 폴더 생성

```
mkdir Shared
```

### mount

- mount -t vboxsf -o uid=1000,gid=1000,dmode=0755,fmode=0755 관리자에서 설정한 공유폴더 이름 우분투에서 생성한 공유폴더 경로

```
mount -t vboxsf -o uid=1000,gid=1000,dmode=0755,fmode=0755 Shared /home/USER아이디/Shared
```
