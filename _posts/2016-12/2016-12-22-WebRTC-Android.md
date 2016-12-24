---
layout: post
title: WebRTC Android
date: "2016-12-22 14:10"
---

WebRTC 를 안드로이드로 빌드 하는 방법에 대해서 알아 본다.


### 준비하기
- VirtualBox
  - https://www.virtualbox.org/wiki/Downloads
- ubuntu 16
  - ubuntu-16.04.1-desktop-amd64.iso
  - https://www.ubuntu.com/desktop


### VirtualBox 설치


### VirtualBox에 ubuntu 설치
- 설치시 디스크 용량은 최소 60 기가 정도로 잡는다.

### ubuntu 설정
- 시스템 -> 프로세서 -> CPU 2개
- 디스플레이 -> 화면 -> 비디오 메모리 128MB
- 공유폴더설정

```
mount -t vboxsf -o uid=1000,gid=1000,dmode=0755,fmode=0755 Shared /home/USER아이디/Shared
```

### WebRTC
- https://webrtc.org/
- webrtc 안드로이드 문서
  - https://webrtc.org/native-code/android/
- 소스 주소
  - https://chromium.googlesource.com/external/webrtc.git


### 컴파일 환경
#### git 설치

```
apt-get install git
```

### 폴더생성

```
mkdir webrtc
cd webrtc
```


### Depot Tools clone

```
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
```

### Depot Tools 임시 Path 등록

```
export PATH=`pwd`/depot_tools:"$PATH"
```

### 타겟 지정

```
export GYP_DEFINES="OS=android"
```

### 소스 다운로드

```
fetch --nohooks webrtc_android
```

```
gclient sync
```

- 네트워크에 따라 몇 시간 걸림
- 중간에 라이센스 관련하여 y/n 을 물어본다.
- Android Tool 설치 시 Google-Play SDK 설치 라이센스 동의 묻는 부분의 대략 11번 항목에서 멈춰 서고 화면에는 보이지 않지만(y/n 을 물어봄) y 를 치고 엔터를 치면 계속 진행함


### 다운로드 에러시 (거의 에러 발생)
- 만약 나머지 라이센스 항목이 나오면서 오류가 나는 경우에는 다음과 같이 조치한다.
- ./setup_links.py 파일을 실행하면 심볼릭 링크가 생성한다. 실행 후 ll 명령어 실행으로 심볼릭 링크가 확인 되지 않으면 ./setup_links.py 를 다시 실행한다.

```
cd src
./setup_links.py
```

- 컴파일시 필요라이브러리 설치를 설치하고 gclient sync 로 다시 다운로드 한다.

```
./build/install-build-deps.sh  --no-chromeos-fonts
./build/install-build-deps-android.sh
gclient sync
```

### 소스 빌드

- Arm V7 with Neon : armeabi-v7a (arm 계열의 32비트)

```
gn gen out/Default --args='target_os="android" target_cpu="arm" is_debug=false'
ninja -C out/Default AppRTCDemo
```

- Arm 64 : arm64-v8a (arm 계열의 64비트)

```
gn gen out_arm64/Default --args='target_os="android" target_cpu="arm64" is_debug=false'
ninja -C out_arm64/Default AppRTCDemo
```

- x86

```
gn gen out_x86/Default --args='target_os="android" target_cpu="x86" is_debug=false'
ninja -C out_x86/Default AppRTCDemo
```

- x64

```
gn gen out_x64/Default --args='target_os="android" target_cpu="x64" is_debug=false'
ninja -C out_x64/Default AppRTCDemo
```

- 안드로이드는 Arm V7 with Neon, Arm 64 두개만 하면 된다.
- 각각 지정한 폴더 밑으로 빌드본이 생긴다. out/Default, out_arm64/Default

### 빌드 실패시
- 깔끔하게 우분투 설치 과정부터 다시 한다.


### Android 프로젝트에 포함시키기

#### so 파일 추축하기

#### jar 파일 추출하기


계속 쓸 예정...
