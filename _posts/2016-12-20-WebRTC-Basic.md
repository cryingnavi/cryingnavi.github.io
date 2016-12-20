---
layout: post
title: WebRTC Basic
---

### webrtc is
WebRTC 는 웹기반으로 플러그인 없이 실시간 미디어 통신을 구현할 수 있는 표준이다. 기존에는 웹에서 실시간 화상/음성 통신을 수행하기 위해서는 ActiveX 나 flash 와 같은 형태의 플러그인이 필요했었다. 그러나 WebRTC 가 브라우저에 기본 탑재되면서 플러그인 없이 Javascript API 만으로도 브라우저에서 실시간 화상/음성 통신을 구현할 수 있게 되었다. 
또한 WebRTC를 Android, IOS 형태로 빌드 할 수 있으며 이를 직접 빌드하면, 모바일 네이티브에서도 사용할 수 있다.
현재 IETE 에서 관련 프로토콜을 정의 하고 있으며, W3C 에서는 API 에 대한 표준화를 진행하고 있다. 2016년 9월. WebRTC 의 정식 버전인 1.0 버전이 공식 릴리즈 되었다.


![_config.yml]({{ site.baseurl }}/images/webrtc-browser.png)
[2016.12.20] 현재 브라우저 지원현황

- Edge 는 ORTC 형태로 지원되며 2016.12.20 현재, WebRTC와 상호 호환되지 않는다. 호환은 작업은 현재 진행중이다.
- 사파리는 현재 WebRTC를 구현중이며 2017년 릴리즈 될것으로 보인다.

### 주요 용어 정리
- STUN
- TURN
- Candidate
- SDP(Session Description Protocol)
- ICE

### 시그널링

### Main API's
- getUserMedia
- PeerConnection
- DataChannel

### ORTC