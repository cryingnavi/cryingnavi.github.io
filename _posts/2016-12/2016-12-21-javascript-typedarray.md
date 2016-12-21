---
layout: "post"
title: "Javascript-TypedArray"
date: "2016-12-21 11:41"
---

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |


Javascript 에서는 원시 이진 데이터를 컨트롤 할 수 있다. 이를 Typed Array 라 한다.
이를 사용하면 텍스트나 파일을 바이트 형태로 전송할 수 있다.

WebRTC나 WebSocket 의 경우 파일을 Base64 형태의 문자열로 변환하여 보낼 수도 있으나 이럴 경우 대용량 파일이 문제가 된다. 어마어마한 양의 Base64 문자열을 안정적으로 핸들링하기가 여간 어려운것이 아니다.

이와 같은 경우 파일을 바이트로 변환하여 송수신할 수 있다.

TypedArray 를 사용하기 위해서는 두가지만 기억하면 된다. 바로 버퍼와 뷰이다.

버퍼는 길이가 정해진 이진 데이터 버퍼이다. 다시 말해 데이터가 담길 그릇의 크기를 정하는 것이다.

뷰는 버퍼에 데이터를 읽거나 쓸 수 있도록 로우 레벨 인터페이스를 제공한다.

여기서는 버퍼와 뷰의 사용법 외에 한글을 인코딩하는 방법에 대해서도 설명한다.

### 자료형
형식   |범위    |설명     |C에 해당하는 Type
----- | ----- | ----- | ------
Int8Array|-128 ~ 127|부호있는 8비트 정수|char
Uint8Array|0 ~ 255|부호없는 8비트 정수|unsigned char
Int16Array|-32,768 ~ 32,767|부호있는 16비트 정수|short
Uint16Array|0 ~ 65,535|부호없는 16비트 정수|	unsigned short
Int32Array|-2,147,483,648 ~ 2,147,483,647|	부호있는 32비트 정수|int
Uint32Array|0 ~ 4,294,967,295|부호없는 32비트 정수|unsigned int
Float32Array|-3.4 x 10의 38승 ~ 3.4 x 10의 38승|32-bit IEEE floating point number|float
Float64Array|-1.79 x 10의 308승 ~ 1.79 x 10의 308승|64-bit IEEE floating point number|double



### Buffer
일단 16바이트 크기의 버퍼를 생성하는 방법은 아래와 같다.

```
var buf = new ArrayBuffer(16);
```

이제 16 바이트 크기의 데이터를 저장할 수 있는 버퍼가 생성된 것이다.

### View
뷰는 다음과 같이 생성한다.

```
var view = new DataView(buf);
```

### 데이터 쓰기
뷰를 생성했다면, 뷰의 메소드들을 사용할 수 있다. 뷰의 get/set 메소드드들은 아래와 같은 것들이 있다.

#### set
이름|설명
-----|-----
setInt8|
setUint8|
setInt16|
setUint16|
setInt32|
setUint32|
setFloat32|
setFloat64|

#### get
이름|설명
-----|-----
getInt8|
getUint8|
getInt16|
getUint16|
getInt32|
getUint32|
getFloat32|
getFloat64|

### 한글 인코딩하기


계속 쓸 예정....
