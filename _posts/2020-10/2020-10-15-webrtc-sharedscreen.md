---
layout: post
title: WebRTC 화면 공유하기
date: "2020-10-15 10:22"
---

WebRTC에서 화면 공유하는 방법을 알아본다.

## API
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
