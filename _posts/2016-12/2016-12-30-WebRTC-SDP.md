---
layout: post
title: WebRTC SDP
date: "2016-12-30 10:39"
categories: webrtc
tags: [webrtc]
---

WebRTC 의 SDP 에 대해서 자세히 알아본다. SDP란 Session Description Protocol 의 약자로 연결하고자 하는 Peer 서로간의 미디어와 네트워크에 관한 정보를 이해하기 위해 사용된다.

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
- WebRTC 에서 위와 같이 브라우저별 또는 브라우저 버전별 일부 다른 API 사용법을 일관되게 사용하게 해주는 것이 adapter.js 이다. 이를 적용하면 SDP 검출시 Promise 를 일괄적으로 사용할 수 있다.

### SDP 전체 전문 보기
- Offer SDP 의 전문이다.
- 전체 전문을 바탕으로 그 의미를 아는 것들에 대해서만 아래에서 설명한다.

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

- SDP를 생성한 Peer의 식별자. 순서는 아래와 같다.
- user name(생략됨), session id, session version, network type, address type, unicast address 순이다.
- user name 이 - 으로 생략되었지만 문자열 파싱을 통해 어플리케이션에서 user name 을 추가할 수 있다. 이를 추가하면 아래와 같다

```
o=Jack 6137031273746274589 2 IN IP4 127.0.0.1
```

```
s=-
```

- 세션 네임이지만 별도의 세션 네임은 생략되었다.
- 세션 네임이 생략되었지만 어플리케이션에서 이를 추가할 수 있다.

```
t=0 0
```

- 세션이 활성화 되었을 시간을 의미한다. 각각 start time 과 end time 이다. 0 0 은 고정 세션을 의미한다. 이는 세션 만료 없이 영구적임을 의미한다.


```
a=group:BUNDLE audio video data
```

- 번들 그룹핑은 SDP 내에 있는 미디어 라인들간에 관계를 형성 한다. 예를 들어 audio video 라고 기술되어 있다면, 이는 datachannel없이 audio와 video 에 관한 라인들만 있음을 의미한다. 즉 audio, video만 사용됨을 의미한다.



### SDP 상세 설명 (Audio Lines)

```
m=audio 9 UDP/TLS/RTP/SAVPF 111 103 104 9 0 8 126
```

- m 은 미디어라인을 의미한다. 이는 오디오 스트림에 관한 속성들에 대한 정보들을 가지고 있다. 순서는 아래와 같다
- 미디어 타입 (audio)
- 포트 번호 (9)
- 형식을 일컫는다 곧 사용되어지는 프로토콜이다 (UDP/TLS/RTP/SAVPF)
- 브라우저에서 미디어를 보내고 받을 수 있는 미디어 형식(코덱)을 가르키는데 이는 프로파일 번호이다. 프로파일 번호에 해당하는 코덱에 관한 상세 설명은 아래에 있다
- Peer 간의 협상과정에서 앞에 있는 프로파일 번호를 순서대로 적용한다. 예를 들어 111번에 해당하는 코덱이 상호간에 가능한지 확인하고 가능하지 않다면 103번으로 넘어간다.


```
c=IN IP4 0.0.0.0
```

- c 는 실시간 트래픽을 보내고 받을 IP를 제공합니다. 그러나 ICE 에서 이미 IP 가 제공 되고 있으므로 c 라인의 IP 는 사용되지 않는다.

```
a=rtcp:9 IN IP4 0.0.0.0
```

- 이 행은 RTCP에 사용될 IP 및 포트를 지정합니다. RTCP multiplex가 지원되므로 SRTP와 동일한 포트입니다.


```
a=ice-ufrag:eWXl
a=ice-pwd:57FcQfoChjtjaMlHOlp6TPGE
```

- ICE Parameter 이다.
- ICE candidate 가 교환되면, 서로를 확인하는 프로세스가 시작된다.
- ice-ufgra 과 ice-pwd는 해당 프로세스에 사용되어 세션과 관련되지 않는 잠재적인 공격을 받지 않도록 한다.


```
a=fingerprint:sha-256 EB:E1:55:0E:41:99:0C:C0:CC:C8:43:9B:99:11:F9:A1:4D:77:5C:A1:BF:70:78:B0:19:30:04:D8:D3:11:DC:0D
```

- DTLS Parameters 이다.
- DTLS-SRTP 협상에 사용되는 인증서의 해시 함수 (이 경우 sha-256 사용)의 결과이다.

```
a=setup:actpass
```

- DTLS Parameters 이다.
- 이 매개 변수는 Peer가 DTLS 협상을 시작하는 서버 또는 클라이언트가 될 수 있음을 의미합니다. RFC4145에 의해 정의 되었으며 RFC4572에 의해 업데이트되었습니다.


```
a=mid:audio
```

- BUNDLE 행에 사용되는 식별자이다.

```
b=AS:32
```

- 이는 기본으로 검출되는 SDP의 내용이 아니라, 어플리케이션에서 추가한 속성이다.
- available send 의 약자로 전송 가능한 최대 밴드위드스를 뜻한다.
- 오디오에 대해 32 kb/s로 최대 밴드위드스를 설정한다는 의미이다.

```
a=sendrecv
```

- 미디어의 방향을 표시한다.
- SDP 를 생성할 때 사용하는 createOffer, createAnswer 함수의 option 지정을 통해 sendonly, recvonly 형태로 변경할 수 있다.
- MCU 서버를 연결한다든지 혹은 일반적인 서비스 형태가 아닌 경우, 이를 sendonly, recvonly 형태로 선언해야하는 경우도 존재한다.

```
a=rtpmap:111 opus/48000/2
```

- opus 코덱에 관한 기술이다
- 프로파일 번호는 111 이며 48000Hz의 샘플링 주기를 가지며 2채널을 사용한다.
- 프로파일 번호는 m 미디어라인에서 사용되는 프로파일 번호와 일치한다.


```
a=rtpmap:103 ISAC/16000
a=rtpmap:104 ISAC/32000
```

- 각각 ISAC 코덱의 16000Hz 샘플링주기, 32000Hz 샘플링 주기를 뜻한다.


### SDP 상세 설명 (Video Lines)

```
m=video 9 UDP/TLS/RTP/SAVPF 100 101 107 116 117 96 97 99 98
```

- m 은 미디어라인을 의미한다. 이는 비디오 스트림에 관한 속성들에 대한 정보들을 가지고 있다.
- 순서는 오디오와 같다.

### SDP 상세 설명 (Data Lines)

```
m=application 9 DTLS/SCTP 5000
```

- m 은 미디어라인을 의미한다. 이는 데이터 스트림에 관한 속성들에 대한 정보들을 가지고 있다.
- 순서는 오디오와 같다.


### 밴드위드스 설정하기
- createOffer, createAnswer 함수를 통해 SDP 를 생성했다면, 어플리케이션에서 필요에 따라 밴드위드스를 설정할 수 있다
- b=AS 를 설정하는 방법은 아래와 같다.

```
function spdToAddBandwidth(sdp){
  //bundle 검색
  var bundles = sdp.match(/a=group:BUNDLE (.*)?\r\n/);
  if(bundles){
    if(bundles[1]){
      bundles = bundles[1].split(" ");
    }
    else{
      return sdp;
    }
  }
  else{
    return sdp;
  }

  //각 미디어의 밴드위드스 설정값
  var maxab = 32,
    maxvb = 1500,
    maxdb = 1638400;

  //혹시 이미 설정되어 있다면 삭제한다.
  sdp = sdp.replace(/b=AS([^\r\n]+\r\n)/g, "");

  //번들 네임의 경우 브라우저마다 차이가 존재하므로 아래와 같이 처리한다.
  //chrome : audio video data
  //firefox : sdparta_0 sdparta_1 sdparta_2
  for(var i=0; i<bundles.length; i++){
    if(bundles[i] === "audio" || bundles[i] === "sdparta_0"){
      sdp = sdp.replace("a=mid:"+bundles[i]+"\r\n", "a=mid:"+bundles[i]+"\r\nb=AS:" + maxab + "\r\n");
    }else if(bundles[i] === "video" || bundles[i] === "sdparta_1"){
      sdp = sdp.replace("a=mid:"+bundles[i]+"\r\n", "a=mid:"+bundles[i]+"\r\nb=AS:" + maxvb + "\r\n");
    }else if(bundles[i] === "data" || bundles[i] === "sdparta_2"){
      sdp = sdp.replace("a=mid:"+bundles[i]+"\r\n", "a=mid:"+bundles[i]+"\r\nb=AS:" + maxdb + "\r\n");
    }
  }

  return sdp;
}

//사용
var newSdp = spdToAddBandwidth(oldSdp);
```


### 적용 코덱 우선순위 설정하기

- Peer 간의 협상과정에서 서로간에 적용할 수 있는 코덱의 우선순위를 변경 설정할 수 있다.
- 비디오를 예를 들어 VP8 코덱이 아닌 H.264 를 우선 적용하고 싶다면, H.264에 해당하는 프로파일 번호를 가장 앞으로 변경할 수 있다.
- 아래에서 H.264는 107 이다. 107을 100번 앞으로 변경하면 된다.

```
m=video 9 UDP/TLS/RTP/SAVPF 100 101 107 116 117 96 97 99 98
```

```
function sdpToReplacePreferCodec(sdp, mLineReg, preferCodec){
  var mLine,
    newMLine = [],
    sdpCodec,
    mLineSplit,
    reg = new RegExp("a=rtpmap:(\\d+) " + preferCodec + "/\\d+");

  mLine = sdp.match(mLineReg);
  if(!mLine){
    return sdp;
  }

  sdpCodec = sdp.match(reg);
  if(!sdpCodec){
    return sdp;
  }

  mLine = mLine[0];
  sdpCodec = sdpCodec[1];

  mLineSplit = mLine.split(" ");
  newMLine.push(mLineSplit[0]);
  newMLine.push(mLineSplit[1]);
  newMLine.push(mLineSplit[2]);
  newMLine.push(sdpCodec);

  for(var i=3; i<mLineSplit.length; i++){
    if(mLineSplit[i] !== sdpCodec){
      newMLine.push(mLineSplit[i]);
    }
  }

  return sdp.replace(mLine, newMLine.join(" "));
}

var newSdp = sdpToReplacePreferCodec(oldSdp, /m=video(:?.*)?/, "H264");
```
