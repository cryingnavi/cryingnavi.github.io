---
layout: post
title: WebRTC 화면 공유하기
date: "2020-10-15 10:22"
categories: webrtc
tags: [webrtc]
---

WebRTC에서 화면을 공유하는 방법입니다.

## API
아래와 같이 화면에 대한 미디어 스트림을 얻을 수 있다.
```
navigator.mediaDevices.getDisplayMedia({
	audio: true,
	video: true
}).then(function(stream){
	//success
}).catch(function(e){
	//error;
});
```

옵션으로 audio: true를 전달하면, 오디오를 함께 공유할 수 있다(화면 공유 창에서 audio를 체크해야한다.)

![getDisplayMedia](/assets/images/2020-10-15/getDisplayMedia.png)



그러나 크롬탭이 아닌 화면 전체 또는 특정 어플리케이션만 공유한다면, 오디오 체크 부분이 활성화 되지 않을 수 있다.
그럼 다음과 같이 각각 미디어 스트림을 얻어내어 결합해야 한다.


```
navigator.mediaDevices.getUserMedia({
	audio: true
}).then(function(audioStream){
	//오디오 스트림을 얻어냄

	navigator.mediaDevices.getDisplayMedia({
		audio: true,
		video: true
	}).then(function(screenStream){
		//스크린 공유 스트림을 얻어내고 여기에 오디오 스트림을 결합함
		screenStream.addTrack(audioStream.getAudioTracks()[0]);
	}).catch(function(e){
		//error;
	});
}).catch(function(e){
	//error;
});
```

screenStream.addTrack(audioStream.getVideoTracks()[0])와 같이 addTrack함수를 이용할 수 있다.

