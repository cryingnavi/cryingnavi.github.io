---
layout: post
title: WebRTC SDP
date: "2016-12-30 10:39"
---

WebRTC 의 SDP 에 대해서 자세히 알아본다.

### Offer SDP
- 먼저 연결하고자 하는 Peer 가 만든 SDP 를 일컫는다.
- Offer 를 생성하는 방법은 아래와 같다.

```
RTCPeerConnection.createOffer(successCallback, failureCallback[, options]);
```

- 크롬 51 버전 부터는 Promise 가 적용되었다.

```
RTCPeerConnection.createOffer([options]).then(successCallback).catch(failureCallback);
```

### Answer SDP
- 응답하는 Peer 가 만든 SDP 를 일컫는다.
- Answer 를 생성하는 방법은 아래와 같다.

```
RTCPeerConnection.createAnswer(successCallback, failureCallback[, options]);
```

- 크롬 51 버전 부터는 Promise 가 적용되었다.

```
RTCPeerConnection.createAnswer([options]).then(successCallback).catch(failureCallback);
```

### adapter.js
- 위와 같이 브라우저별 또는 브라우저 버전별 일부 다른 API 사용법을 일관되게 사용하게 해주는 것이 adapter.js 이다.
- 이를 적용하면 SDP 검출시 Promise 를 일괄적으로 사용할 수 있다.

### SDP 전체 전문 보기
- Offer SDP 의 전문이다.
- 전체 전문을 바탕으로 아래에서 한줄 한줄 의미를 파악해 보겠다.

```
v=0
o=- 6137031273746274589 2 IN IP4 127.0.0.1
s=-
t=0 0
a=group:BUNDLE audio video data
a=msid-semantic: WMS vrogvdX9SLeto3IhVm6C1cFS2fRIFqRMlPzd
m=audio 9 UDP/TLS/RTP/SAVPF 111 103 104 9 0 8 126
c=IN IP4 0.0.0.0
a=rtcp:9 IN IP4 0.0.0.0
a=ice-ufrag:eWXl
a=ice-pwd:57FcQfoChjtjaMlHOlp6TPGE
a=fingerprint:sha-256 EB:E1:55:0E:41:99:0C:C0:CC:C8:43:9B:99:11:F9:A1:4D:77:5C:A1:BF:70:78:B0:19:30:04:D8:D3:11:DC:0D
a=setup:actpass
a=mid:audio
b=AS:32
a=extmap:1 urn:ietf:params:rtp-hdrext:ssrc-audio-level
a=extmap:3 http://www.webrtc.org/experiments/rtp-hdrext/abs-send-time
a=sendrecv
a=rtcp-mux
a=rtpmap:111 opus/48000/2
a=rtcp-fb:111 transport-cc
a=fmtp:111 minptime=10;useinbandfec=1
a=rtpmap:103 ISAC/16000
a=rtpmap:104 ISAC/32000
a=rtpmap:9 G722/8000
a=rtpmap:0 PCMU/8000
a=rtpmap:8 PCMA/8000
a=rtpmap:126 telephone-event/8000
a=ssrc:4294317716 cname:rMz9sLiEHfqP6GNx
a=ssrc:4294317716 msid:vrogvdX9SLeto3IhVm6C1cFS2fRIFqRMlPzd dbf5d9a8-8135-4e0d-87f0-d2b6b7d326b3
a=ssrc:4294317716 mslabel:vrogvdX9SLeto3IhVm6C1cFS2fRIFqRMlPzd
a=ssrc:4294317716 label:dbf5d9a8-8135-4e0d-87f0-d2b6b7d326b3
m=video 9 UDP/TLS/RTP/SAVPF 100 101 107 116 117 96 97 99 98
c=IN IP4 0.0.0.0
a=rtcp:9 IN IP4 0.0.0.0
a=ice-ufrag:eWXl
a=ice-pwd:57FcQfoChjtjaMlHOlp6TPGE
a=fingerprint:sha-256 EB:E1:55:0E:41:99:0C:C0:CC:C8:43:9B:99:11:F9:A1:4D:77:5C:A1:BF:70:78:B0:19:30:04:D8:D3:11:DC:0D
a=setup:actpass
a=mid:video
b=AS:1500
a=extmap:2 urn:ietf:params:rtp-hdrext:toffset
a=extmap:3 http://www.webrtc.org/experiments/rtp-hdrext/abs-send-time
a=extmap:4 urn:3gpp:video-orientation
a=extmap:5 http://www.ietf.org/id/draft-holmer-rmcat-transport-wide-cc-extensions-01
a=extmap:6 http://www.webrtc.org/experiments/rtp-hdrext/playout-delay
a=recvonly
a=rtcp-mux
a=rtcp-rsize
a=rtpmap:100 VP8/90000
a=fmtp:100 x-google-start-bitrate=600; x-google-min-bitrate=600; x-google-max-bitrate=1500; x-google-max-quantization=56
a=rtcp-fb:100 ccm fir
a=rtcp-fb:100 nack
a=rtcp-fb:100 nack pli
a=rtcp-fb:100 goog-remb
a=rtcp-fb:100 transport-cc
a=rtpmap:101 VP9/90000
a=rtcp-fb:101 ccm fir
a=rtcp-fb:101 nack
a=rtcp-fb:101 nack pli
a=rtcp-fb:101 goog-remb
a=rtcp-fb:101 transport-cc
a=rtpmap:107 H264/90000
a=rtcp-fb:107 ccm fir
a=rtcp-fb:107 nack
a=rtcp-fb:107 nack pli
a=rtcp-fb:107 goog-remb
a=rtcp-fb:107 transport-cc
a=fmtp:107 level-asymmetry-allowed=1;packetization-mode=1;profile-level-id=42e01f
a=rtpmap:116 red/90000
a=rtpmap:117 ulpfec/90000
a=rtpmap:96 rtx/90000
a=fmtp:96 apt=100
a=rtpmap:97 rtx/90000
a=fmtp:97 apt=101
a=rtpmap:99 rtx/90000
a=fmtp:99 apt=107
a=rtpmap:98 rtx/90000
a=fmtp:98 apt=116
m=application 9 DTLS/SCTP 5000
c=IN IP4 0.0.0.0
a=ice-ufrag:eWXl
a=ice-pwd:57FcQfoChjtjaMlHOlp6TPGE
a=fingerprint:sha-256 EB:E1:55:0E:41:99:0C:C0:CC:C8:43:9B:99:11:F9:A1:4D:77:5C:A1:BF:70:78:B0:19:30:04:D8:D3:11:DC:0D
a=setup:actpass
a=mid:data
b=AS:1638400
a=sctpmap:5000 webrtc-datachannel 1024
```


### SDP 상세 설명 (Global Lines)

```
v=0
```

- SDP의 현재 버전을 일컫는다. 곧 SDP 프로토콜 버전이다.

```
o=- 6137031273746274589 2 IN IP4 127.0.0.1
```

- SDP를 생성한 Peer의 식별자. user name(생략됨), session id, session version, network type, address type, unicast address 순이다.
- user name 이 - 으로 생략되었지만 문자열 파싱을 통해 어플리케이션에서 user name 을 추가할 수 있다.

```
s=-
```

- 세션 네임이지만 별도의 세션 네임은 생략되었다.
- 세션 네임이 생략되었지만 어플리케이션에서 이를 추가할 수 있다.

```
t=0 0
```

- 세션이 활성화 되었을 시간을 의미한다. 각각 start time 과 end time 이다. 0 0 은 고정 세션을 의미한다.


```
a=group:BUNDLE audio video data
```

- 번들 그룹핑은 몇몇 미디어 라인들간에 관계를 형성 한다. 예를 들어 audio video 라고 기술되어 있다면, 이는 datachannel없이 audio video 를 사용하고 있음을 의미한다. 또한 audio 만 기술되어 있다면 SDP내에 audio 에 대한 설명만 존재한다고 할 수 있다.

```
a=msid-semantic: WMS vrogvdX9SLeto3IhVm6C1cFS2fRIFqRMlPzd
```

- 이 라인은 PeerConnection 의 수명동안 식별자를 부여한다.


### SDP 상세 설명 (Audio Lines)

```
m=audio 9 UDP/TLS/RTP/SAVPF 111 103 104 9 0 8 126
```

- m 은 미디어라인을 의미한다. 이는 오디오 스트림에 관한 속성들에 대한 정보들을 가지고 있다. 순서는 아래와 같다
- 미디어 타입 (여기서는 audio)
- UDP/TLS/RTP/SAVPF 는 사용되어지는 프로토콜에 대한 기술이다
- 111 103 104 9 0 8 126 는 브라우저에서 미디어를 보내고 받을 수 있는 미디어 형식을 가리킨다. 해당 번호에 대한 상세 설명은 아래에 나온다.


```
c=IN IP4 0.0.0.0
```

- c 는 연결에 관한 라인이다.




계속 작성...
