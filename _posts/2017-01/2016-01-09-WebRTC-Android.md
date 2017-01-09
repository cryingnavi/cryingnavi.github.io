---
layout: post
title: WebRTC Android
date: "2017-01-09 16:42"
---

WebRTC 를 안드로이드로 빌드 하는 방법에 대해서 알아 본다. 아래 과정을 순서대로 따라하면 된다.

### WebRTC android 참고 문서 및 소스
- [https://webrtc.org/](https://webrtc.org/)
- webrtc 안드로이드 문서
  - [https://webrtc.org/native-code/android/](https://webrtc.org/native-code/android/)
- 소스 주소
  - [https://chromium.googlesource.com/external/webrtc.git](https://chromium.googlesource.com/external/webrtc.git)


### 준비하기
- 글쓴이는 2012년 형 맥프로에서 빌드환경을 구축하였다.
- VirtualBox
  - [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
- ubuntu 16
  - ubuntu-16.04.1-desktop-amd64.iso
  - [https://www.ubuntu.com/desktop](https://www.ubuntu.com/desktop)

### VirtualBox에 ubuntu 설치
- VirtualBox에 우분투를 설치한다.
- 설치시 디스크 용량은 최소 60기가 정도로 잡는다.

### VirtualBox의 ubuntu 설정
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
- so 파일을 추축하는 방법은 아래 두가지가 있다.
- 원본 so 파일 추출
  - [src]/out/Default/libjingle_peerconnection_so.so
- apk 에서 추출하기
  - [src]/out/Default/apks/AppRTCMobile.apk
  - 압축 해제하면 libs 폴더가 존재하고 해당 폴더 안에 so 파일이 존재한다.

### 샘플 프로젝트 위치
- src/webrtc/examples/androidapp

### 샘플 프로젝트 시작하기
- 버츄얼 박스 밖의 원래 사용하고 있는 OS 의 이클립스에서 안드로이드 프로젝트를 만든다
![_config.yml]({{ site.baseurl }}/images/webrtc-android/webrtc-android-sample01-step01.png)
![_config.yml]({{ site.baseurl }}/images/webrtc-android/webrtc-android-sample01-step02.png)
![_config.yml]({{ site.baseurl }}/images/webrtc-android/webrtc-android-sample01-step03.png)
![_config.yml]({{ site.baseurl }}/images/webrtc-android/webrtc-android-sample01-step04.png)
![_config.yml]({{ site.baseurl }}/images/webrtc-android/webrtc-android-sample01-step05.png)
![_config.yml]({{ site.baseurl }}/images/webrtc-android/webrtc-android-sample01-step06.png)

- 프로젝트가 생성되었다면 프로젝트의 전체를 삭제한다.
![_config.yml]({{ site.baseurl }}/images/webrtc-android/webrtc-android-sample01-step07.png)

- webrtc 를 빌드한 우분투로 돌아가서 src/webrtc/examples/androidapp 전체를 공유 폴더로 복사하고 이를 다시 원래 사용하고 있던 OS에서 만든 프로젝트에 복사하여 프로젝트 전체를 덮어쓰기한다.
- 해당 프로젝트에서 libs 폴더를 만든다.
- webrtc 를 빌드한 우분투로 돌아가서 빌드한 apk 를 찾아 압축을 해제한다. 경로 및 파일명은 Arm V7 with Neon, Arm 64 두 개를 빌드 했을 경우 out/Default/apks/AppRTCMobile.apk 와 out_arm64/Default/apks/AppRTCMobile.apk 이다.
- 압축을 해제하고 생긴 lib 폴더를 안에 armeabi-v7a 또는 arm64-v8a 라는 폴더가 있을 것이다. 이는 libjingle_peerconnection_so.so 를 담고 있는 폴더이다. 이를 공유 복사한 다음 이클립스에서 만든 프로젝트내 libs 폴더로 복사한다.
- 다시 out/Default/lib.java/webrtc 폴더 내에서 autobanh_java.jar, base_java.jar, libjingle_peerconnection_java.jar 를 찾아 공유 폴더로 복사하고 다시 jar 파일들을 이클립스에서 만든 프로젝트내 libs 폴더로 복사한다.
- 위 과정을 모두 마치면 아래와 같이 된다.
![_config.yml]({{ site.baseurl }}/images/webrtc-android/webrtc-android-sample01-step08.png)
- 이제 안드로이드 연결 후에 빌드할 수 있다.
