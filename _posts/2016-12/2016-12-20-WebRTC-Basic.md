---
layout: post
title: WebRTC Basic
date: "2016-12-20 11:41"
---

### webrtc is

기존에는 플래시와 같은 별도의 플러그인 또는 서드 파티 소프트웨어로 웹 브라우저 상에서 화상 통신과 같은 서비스를 구현했었다. 그러나 WebRTC 가 브라우저에 기본 탑재되면서 이러한 플러그인 없이 웹 브라우저 간에 실시간 커뮤니케이션을 수행할 수 있게 되었다.

WebRTC 란 웹 브라우저만으로 실시간 화상 통신을 구현할 수 있는 API 이다. 이는 웹 브라우저 만으로 Peer 간에 음성, 영상, 파일을 직접 주고 받을 수 있다는 것이며 구글의 행아웃이 대표적인 서비스할 수 있다.

WebRTC 의 비전은 웹어플리케이션에서 비디오 채팅, Peer 간의 데이터 공유를 손쉽게 구현하고 전화기, TV, 컴퓨터가 모두 공통 플랫폼 위에서 대화할 수 있도록 하는 것이다. 웹 브라우저만이라고는 하지만 실제 서비스에 적용된 모습을 살펴보면 웹브라우저, 모바일 네이티브 어플리케이션을 가리지 않는다. 이는 WebRTC 가 오픈소스이며 이를 직접 빌드하여 사용할 수 있기 때문이다.

현재 IETE 에서 관련 프로토콜을 정의 하고 있으며, W3C 에서는 API 에 대한 표준화를 진행하고 있다. 2016년 9월. WebRTC 의 정식 버전인 1.0 버전이 공식 릴리즈 되었다.

WebRTC 의 기능을 간편하게 체험할 수 있는 방법이 있으며  아래 주소를 크롬 또는 파이어 폭스로 접속하여 실행해 볼 수 있다.

[https://apprtc.appspot.com/](https://apprtc.appspot.com/)


![_config.yml]({{ site.baseurl }}/images/webrtc-browser.png)
[2016.12.20] 현재 브라우저 지원현황

- Edge 는 ORTC 형태로 지원되며 2016.12.20 현재, WebRTC와 상호 호환되지 않는다. 호환은 작업은 현재 진행중이다.
- 사파리는 현재 WebRTC를 구현중이며 2017년 릴리즈 될것으로 보인다.


### WebRTC 기반 기술 이해하기
WebRTC 전체 아키텍처 그림을 기반으로 각 용어가 무엇을 의미하는지 살펴본다.

![_config.yml](https://webrtc.org/assets/images/webrtc-public-diagram-for-website.png)

#### Your Web App
WebRTC API 를 활용하여 사용자에 의해 구현된 어플리케이션이다. 해당 어플리케이션은 화상, 음성, 또는 메시징(파일, 텍스트) 서비스를 수행하는 어플리케이션이다.

#### WebRTC API
웹 브라우저 상에서 자바스크립트로 이용되어질 수 있는 WebRTC API를 지칭한다. WebRTC API 는 크게 세 가지로 나눌 수 있다. getUserMedia, PeerConnection, DataChannel이다.

- getUserMedia : 사용자 단말기의 미디어 장치를 액세스할 수 있는 방법을 제공한다. 미디어 장치라 함은 마이크와 웹캠을 의미한다. getUserMedia 를 통해 미디어 장치를 액세스 하게 되면 미디어 스트림 객체를 얻을 수 있으며 이를 PeerConnection 에 전달하여 미디어 스트림을 전송하게 된다.
- PeerConnection : 가장 중요한 API 이면서 Peer 간의 화상과 음성 등을 교환하기 위한 거의 모든 작업을 수행하는 API 이다. 기본적인 기능은 Singal Processing, Security, 비디오 encode/decode, 네트워크와 관련된 NAT Traversal, Packet send/receive, bandwidth estimation 등이 있다.
- DataChannel : Peer 간에 텍스트나 파일을 주고 받을 수 있는 메시징 API 이다. 설정에 따라 SCTP 또는 RTP 로 전송할 수 있다. DataChannel 은 WebSocket 과 같은 수준의 API 를 제공하며 이는 Row Level API 라 할 수 있다. 대용량 파일을 주고받기 위해서는 이 API 를 활용한 어플리케이션 단의 테크닉이 필요하다.

#### WebRTC Native
WebRTC Native 는 C++ 로 작성되어 있으며 오픈소스이다. WebRTC 네이티브는 오디오(Voice Engine), 비디오(Video Engine), 네트워크(Transport) 관련된 기능으로 세 개의 파트로 나뉘어진다

#### Voice Engine
보이스 엔진은 사용자 단말기의 사운드 카드부터 네트워크에 이르는 일련의 과정을 컨트롤 하는 프레임워크이다. iSAC / iLBC / Opus 와 같은 음성 코덱을 지원하며 AEC(에코 캔슬) 과 노이즈 제거 기능을 가지고 있다. (AEC 와 노이즈 관련 기능은 현재 계속적으로 성능이 개선되고 있다.)

#### Video Engine
보이스 엔진과 마찬가지로 사용자 단말기의 카메라에서 부터 네트워크까지 이르는 일련의 기능을 수행하는 프레임워크이다.

#### STUN
STUN 서버는 네트웍 장비의 일환이다. 서로 연결하고자 하는 Peer 들이 NAT나 방화벽 뒤에 존재하는 지 검사하고 이들의 공인 IP 주소를 전달하는 역할을 수행한다.

이는 server / client 모델이며 STUN Client 는 NAT나 방화벽 뒤에 존재하며 STUN 서버는 공인 IP 망에 존재한다. STUN Client 는 STUN 서버에게 나의 공인 IP 주소는 무엇인가 라고 질의 하게 되고 STUN 서버는 이를 찾아 응답하게 된다. 이렇게 찾아진 공인 IP 를 통해서 peer 간의 통신을 설정하게 됩니다.

![_config.yml]({{ site.baseurl }}/images/Webrtc-stun.png)
[STUN]

#### TURN
STUN 을 통해 통신 설정을 시도 했지만 실패하고 Peer 가 결국 서로를 찾지 못 했을 경우 TURN 서버가 Peer 간의 모든 정보를 중계하여 준다. TURN 은 Peer 간에 발생하는 모든 미디어에 대한 일종의 미디어 proxy 서버라 할 수 있다.

Peer 간의 모든 트래픽을 중계해 주어야 하므로 상당한 부하를 감당해 내야만 한다. 그러므로 실제 서비스에서 가장 큰 비용이 드는 부분이다.

- TURN 을 사용하게끔되는 순서는 다음과 같다.
		- PeerConnection 객체는 UDP 로 통신 설정을 시도
		- UDP 실패시 TCP로 시도한다.
		- TCP 마저 실패시 모든 정보는 TURN 서버에 의해 릴레이

![_config.yml]({{ site.baseurl }}/images/Webrtc-turn.png)
[TURN]

#### Candidate
상대가 나에게 접근할 수 있는 네트워크 경로들에 대한 후보들을 말한다. 예를 들어 candidate 에는 TURN을 경유하는 경로, STUN 을 사용하는 경로, 로컬망에서의 접근 경로들이 있다.
PeerConnection 객체를 생성하면 candidate 를 얻을 수 있고 많게는 candidate 가 열대여섯개 정도 검출되어진다.

#### SDP(Session Description Protocol)
PeerConnection 객체를 생성하게 되면 PeerConnection 객체에서 offer SDP, answer sdp 를 얻을 있다. SDP는 미디어에 대한 메타 데이터로 사용할 수 있는 코덱은 무엇들이 있으며, 어떤 프로토콜을 사용하고, 비트레이트는 얼마이며, 밴드위드스는 얼마이다 와 같은 데이터가 텍스트 형태로 명시되어 있다.

Offer SDP 란 연결을 먼저 맺기를 원하는 Peer의 SDP 를 일컫는다. 해당 Peer는 자신의 Offer SDP 를 생성하여 다른 Peer 에게 전달하고 전달받은 Peer 는 Answer SDP 를 생성하여 Offer 에게 전달한다.

#### ICE
각 Peer 가 서로 상대방을 찾기 위해 최적의 NetWork 경로를 찾을 수 있도록 도와 주는 프레임웍이다. ICE 는 STUN 과 TURN 을 활용하여 여러 Candidate 를 검출하고 이들 중 하나를 선택하여 Peer 간 연결을 수행한다.

### Signaling
WebRTC 는 P2P 연결을 통해 직접 통신하지만, 이를 중계해주는 과정이 필요하다. 이를 시그널링 이라 부른다. 그리고 이를 수행하는 서버를 시그널 서버라 칭한다.

WebRTC 명세에는 들어가 있지 않고 표준화된 방법이 존재하지 않으며 어떤 언어로 개발하든 무방하다.

시그널 서버는 개발하고자 하는 서비스의 성격에 맞게 SIP나 XMPP 등의 기존의 시그널링 프로토콜을 사용해도 되고 Ajax long polling이나 websocket 등의 적절한 쌍방향 통신 채널로 이를 구현하면 된다.

그렇지만 시그널링의 핵심은 비동기적으로 발생하는 Peer 들의 정보(SDP, Candidate)를 교환하는 일이다. 그러므로 전이중 통신을 지원하는 websocket 으로 이를 구현하는 것이 가장 적합하다.


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

첫번째 인자로 미디어 비디오와 오디오에 대한 constraints 를 지정할 수 있다. 비디오는 resolution, frame rate 에 대해 지정할 수 있다. 자세한 지정 방법은 [https://webrtchacks.com/how-to-figure-out-webrtc-camera-resolutions/](https://webrtchacks.com/how-to-figure-out-webrtc-camera-resolutions/) 에서 확인할 수 있다.


#### PeerConnection
거의 모든 일을 처리하는 객체이다. signal processing, Security, encode, decode, NAT traversal, packet send/receive, bandwidth estimation etc..

브라우저마다 제공하고 있는 함수의 이름이 다르므로 아래와 같이 표준화 한다.

```
var PeerConnection = (function(){
	var PeerConnection = window.PeerConnection ||
		window.webkitPeerConnection00 ||
		window.webkitRTCPeerConnection ||
		window.mozRTCPeerConnection ||
		window.RTCPeerConnection;
	return PeerConnection;
})();
```

사용법은 아래와 같다.

```
var pc = new PeerConnection({
	iceServers: [
		{"url": "stun:23.21.150.121" },
		{"url": "stun:stun.l.google.com:19302"}
	]}, {
		optional: [
			{ DtlsSrtpKeyAgreement: true },
			{ RtpDataChannels: false }
		]
});
```



### ORTC
