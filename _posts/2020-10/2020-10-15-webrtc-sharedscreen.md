---
layout: post
title: WebRTC 화면 공유하기
date: "2020-10-15 10:22"
---

WebRTC에서 화면 공유하는 방법을 알아본다.

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

옵션으로 audio: true를 전달하면, 오디오를 함께 공유할 수 있다. 그러나 맥용 크롬에서는 audio가 공유되지 않는다. 이때에는 각각 미디어 스트림을 얻어내어 결합할 수 있다.


```
navigator.mediaDevices.getUserMedia({
	audio: true
}).then(function(audioStream){
	//success

	navigator.mediaDevices.getDisplayMedia({
		audio: true,
		video: true
	}).then(function(screenStream){
		//success
		screenStream.addTrack(audioStream.getVideoTracks()[0])
	}).catch(function(e){
		//error;
	});
}).catch(function(e){
	//error;
});
```

screenStream.addTrack(audioStream.getVideoTracks()[0])와 같이 addTrack함수를 이용할 수 있다.

