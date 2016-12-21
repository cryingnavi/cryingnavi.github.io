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

#### STUN
STUN 서버는 네트웍 장비의 일환이다. 서로 연결하고자 하는 Peer 들이 NAT나 방화벽 뒤에 존재하는 지 검사하고 이들의 공인 IP 주소를 전달하는 역할을 수행한다.
이는 server / client 모델이며 STUN Client 는 NAT나 방화벽 뒤에 존재하며 STUN 서버는 공인 IP 망에 존재한다. STUN Client 는 STUN 서버에게 나의 공인 IP 주소는 무엇인가 라고 질의 하게 되고 STUN 서버는 이를 찾아 응답하게 된다. 이렇게 찾아진 공인 IP 를 통해서 peer 간의 통신을 설정하게 됩니다.

#### TURN
STUN 을 통해 통신 설정을 시도 했지만 실패하고 Peer 가 결국 서로를 찾지 못 했을 경우 TURN 서버가 Peer 간의 모든 정보를 중계하여 준다. TURN 은 Peer 간에 발생하는 모든 미디어에 대한 일종의 미디어 proxy 서버라 할 수 있다. Peer 간의 모든 트래픽을 중계해 주어야 하므로 상당한 부하를 감당해 내야만 한다. 그러므로 실제 서비스에서 가장 큰 비용이 드는 부분이다.
- TURN 을 사용하게끔되는 순서는 다음과 같다.
		- PeerConnection 객체는 UDP 로 통신 설정을 시도
		- UDP 실패시 TCP로 시도한다.
		- TCP 마저 실패시 모든 정보는 TURN 서버에 의해 릴레이

#### Candidate
상대가 나에게 접근할 수 있는 네트워크 경로들에 대한 후보들을 말한다. 예를 들어 candidate 에는 TURN을 경유하는 경로, STUN 을 사용하는 경로, 로컬망에서의 접근 경로들이 있다.
PeerConnection 객체를 생성하면 candidate 를 얻을 수 있고 많게는 candidate 가 열대여섯개 정도 검출되어진다.

#### SDP(Session Description Protocol)
PeerConnection 객체를 생성하게 되면 PeerConnection 객체에서 offer SDP, answer sdp 를 얻을 있다. SDP는 미디어에 대한 메타 데이터로 사용할 수 있는 코덱은 무엇들이 있으며, 어떤 프로토콜을 사용하고, 비트레이트는 얼마이며, 밴드위드스는 얼마이다 와 같은 데이터가 텍스트 형대로 명시되어 있다.

#### ICE ####

### 시그널링



### Main API's

#### getUserMedia
getUserMedia 는 사용자 단말기의 카메라와 마이크에 액세스 할 수 있는 방법을 제공한다. 일단 브라우저마다 제공하고 있는 함수의 이름이 다르므로 아래와 같이 표준화 한다.

```
var UserMedia = (function (){
	var getUserMedia = navigator.getUserMedia ||
		navigator.webkitGetUserMedia ||
		navigator.mozGetUserMedia ||
		navigator.msGetUserMedia;
	return getUserMedia;
})();
```

사용법은 아래와 같다.

```
UserMedia({video: true, audio: true}, function(stream){

}, function(e){

});
```

첫번째 인자로 미디어 비디오와 오디오에 대한 constraints 를 지정할 수 있다. 비디오는 resolution, frame rate 에 대해 지정할 수 있다. 자세한 지정 방법은 [https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia) 에서 확인할 수 있다.


#### PeerConnection
거의 모든 일을 처리하는 객체이다. signal processing, Security, encode, decode, NAT traversal, packet send/receive, bandwidth estimation etc..

#### DataChannel

### ORTC
