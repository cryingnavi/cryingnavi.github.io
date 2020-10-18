---
layout: post
title: Canvas Recording
date: "2016-12-27 13:16"
categories: javascript
tags: [canvas, recording, blob, MediaRecorder]
---

캔버스를 레코딩하여 비디오 파일(webm)로 다운로드하는 방법에 대해서 알아본다.

캔버스 레코딩은 캔버스 태그의 captureStream 인터페이스와 MediaRecorder API 를 활용하여 수행할 수 있다. [MediaRecorder](https://developer.mozilla.org/ko/docs/Web/API/MediaRecorder) 는 Canvas 뿐 아니라 미디어 스트림을 사용하는 WebRTC 등에도 적용할 수 있다.


### 캔버스 태그
```
<canvas id="canvas" width="800" height="500"></canvas>
```

### 캔버스 객체 생성
```
var canvas = document.getElementById("canvas");
```

### 미디어 스트림 생성
```
var stream = canvas.captureStream();
```

### 미디어레코더 생성
```
var rec = new MediaRecorder(stream);
```

### 미디어 레코더 메소드
- 레코딩을 수행하기 위한 메소드는 아래 두가지가 있다.

| 이름 | 설명    |
|:--------|:-------|
| start | 레코딩을 시작한다 |
| stop | 레코딩을 중단한다 |


### 미디어 이벤트
- 레코딩을 수행하기 위한 이벤트는 목록은 아래와 같다.

| 이름 | 설명    |
|:--------|:-------|
| onstart | 레코딩이 시작되면 호출되는 이벤트이다 |
| onstop | 레코딩이 중단되면 호출되는 이벤트이다 |
| ondataavailable | 레코딩이 시작되면 주기적으로 호출되어 캡처한 미디어 데이터를 얻을 수 있는 이벤트이다 |


### 레코딩하기
```
var chunks = []; //레코딩 데이터를 담을 배열

//ondataavailable 이벤트가 주기적으로 호출되면서 chunks 배열에 레코딩 데이터를 쌓음
rec.ondataavailable = function(){
  chunks.push(e.data);
};

//3초 주기로 레코딩을 시작함. 3초마다 ondataavailable 가 호출된다.
rec.start(3000);
```

- start 메소드 호출시 인자로 timeslice 을 전달한다. 예를 들어 3000 을 전달하면 미디어 레코더 객체는 3초동안 레코딩 데이터를 수집하여 ondataavailable 를 호출한다.
- 그러나 만약 3초동안 캔버스에 아무런 변화가 없다면 ondataavailable 는 호출되지 않는다.


### 레코딩 중단하기

```
rec.stop();
```

### 레코딩 결과 다운로드 하기

```
rec.onstop = function(){
  var encodeData = new Blob(chunks, {type: "video/webm"});

  var doc = document,
    link = doc.createElementNS("http://www.w3.org/1999/xhtml", "a"),
    event = doc.createEvent("MouseEvents");

  link.href = URL.createObjectURL(encodeData);
  link.download = "file.webm";

  event.initEvent("click", true, false);
  link.dispatchEvent(event);

  chunks = [];
}
```
