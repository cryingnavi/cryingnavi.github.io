---
layout: post
title: WebRTC Android
date: "2016-12-22 14:10"
---

WebRTC 를 안드로이드로 빌드 하는 방법에 대해서 알아 본다. 아래 과정을 순서대로 따라하면 된다.

### WebRTC android 참고 문서 및 소스
- https://webrtc.org/
- webrtc 안드로이드 문서
  - https://webrtc.org/native-code/android/
- 소스 주소
  - https://chromium.googlesource.com/external/webrtc.git


### 준비하기
- VirtualBox
  - https://www.virtualbox.org/wiki/Downloads
- ubuntu 16
  - ubuntu-16.04.1-desktop-amd64.iso
  - https://www.ubuntu.com/desktop

### VirtualBox에 ubuntu 설치
- VirtualBox에 우분투를 설치한다.
- 설치시 디스크 용량은 최소 60 기가 정도로 잡는다.

### ubuntu 설정
- 시스템 -> 프로세서 -> CPU 2개
- 디스플레이 -> 화면 -> 비디오 메모리 128MB
- 기타 필요한 설정을 수행한다.
- 공유폴더설정

```
mount -t vboxsf -o uid=1000,gid=1000,dmode=0755,fmode=0755 Shared /home/USER아이디/Shared
```

### git 설치

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
gclient sync
```

- 네트워크에 따라 몇 시간 걸림
- 중간에 라이센스 관련하여 y/n 을 물어본다.

### 필요라이브러리 설치

- 컴파일시 필요라이브러리 설치한다.
- 이는 최초 gclient sync 를 시행하고 그 다음에 수행한다.
- 폰트는 필요없으니 제외 옵션을 추가한다.

```
./build/install-build-deps.sh  --no-chromeos-fonts
./build/install-build-deps-android.sh
```

### 소스 빌드

- Arm V7 with Neon : armeabi-v7a (arm 계열의 32비트)

```
gn gen out/Default --args='target_os="android" target_cpu="arm" is_debug=false'
ninja -C out/Default AppRTCMobile
```

- Arm 64 : arm64-v8a (arm 계열의 64비트)

```
gn gen out_arm64/Default --args='target_os="android" target_cpu="arm64" is_debug=false'
ninja -C out_arm64/Default AppRTCMobile
```

- x86

```
gn gen out_x86/Default --args='target_os="android" target_cpu="x86" is_debug=false'
ninja -C out_x86/Default AppRTCMobile
```

- x64

```
gn gen out_x64/Default --args='target_os="android" target_cpu="x64" is_debug=false'
ninja -C out_x64/Default AppRTCMobile
```

- 안드로이드는 Arm V7 with Neon, Arm 64 두개만 하면 된다.
- 각각 지정한 폴더 밑으로 빌드본이 생긴다. out/Default, out_arm64/Default

### Android 프로젝트에 포함시키기

#### so 파일 추축하기

#### jar 파일 추출하기


계속 쓸 예정...
