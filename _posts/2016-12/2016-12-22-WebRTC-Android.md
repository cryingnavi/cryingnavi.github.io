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
- [공유폴더설정]({{ site.baseurl }}/VirtualBox-Shared-Folder)



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

### so 파일 추출하기
- 원본 so 파일 추출
  - [src]/out/Default/libjingle_peerconnection_so.so
- 안드로이드 아키텍처 폴더채로 추출
  - [src]/out/Default/apks/AppRTCDemo.apk
  - 압축 해제하여 추출

### 샘플 프로젝트 위치
- src/webrtc/examples/androidapp

### 샘플 프로젝트 시작하기 1
- 버츄얼 박스 밖의 원래 사용하고 있는 OS 의 이클립스에서 안드로이드 프로젝트를 만든다
![_config.yml]({{ site.baseurl }}/images/webrtc-android/webrtc-android-sample01-step01.png)
![_config.yml]({{ site.baseurl }}/images/webrtc-android/webrtc-android-sample01-step02.png)
![_config.yml]({{ site.baseurl }}/images/webrtc-android/webrtc-android-sample01-step03.png)
![_config.yml]({{ site.baseurl }}/images/webrtc-android/webrtc-android-sample01-step04.png)
![_config.yml]({{ site.baseurl }}/images/webrtc-android/webrtc-android-sample01-step05.png)
![_config.yml]({{ site.baseurl }}/images/webrtc-android/webrtc-android-sample01-step06.png)



- src/webrtc/examples/androidapp 전체를 공유 폴더로 복사하고 이를 다시 프로젝트에 복사하여 프로젝트 전체를 덮어쓰기한다.
- libs 폴더를 만들어서 아래 파일처럼 추가한다.(이미지 첨부하기)


### 샘플 프로젝트 시작하기2

계속 쓸 예정...
