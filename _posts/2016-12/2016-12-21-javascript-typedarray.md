---
layout: "post"
title: "Javascript-TypedArray"
date: "2016-12-21 11:41"
---

Javascript 에서는 원시 이진 데이터를 컨트롤 할 수 있다. 이를 Typed Array 라 한다.
이를 사용하면 텍스트나 파일을 바이트 형태로 전송할 수 있다.

WebRTC나 WebSocket 의 경우 파일을 Base64 형태의 문자열로 변환하여 보낼 수도 있으나 이럴 경우 대용량 파일이 문제가 된다. 어마어마한 양의 Base64 문자열을 안정적으로 핸들링하기가 여간 어려운것이 아니다. 이와 같은 경우 파일을 바이트로 변환하여 송수신할 수 있다.

TypedArray 를 사용하기 위해서는 두가지만 기억하면 된다. 바로 버퍼와 뷰이다. 버퍼는 길이가 정해진 이진 데이터 버퍼이다. 다시 말해 데이터가 담길 그릇의 크기를 정하는 것이다. 뷰는 버퍼에 데이터를 읽거나 쓸 수 있도록 로우 레벨 인터페이스를 제공한다.

여기서는 버퍼와 뷰의 사용법 외에 한글을 인코딩하는 방법에 대해서도 설명한다.


### 자료형

| 이름 | 범위    | 설명     | Type    |
|:--------|:-------|:--------|:--------|
| Int8Array | -128 ~ 127 | 부호있는 8비트 정수 | char |
| Uint8Array | 0 ~ 255 | 부호없는 8비트 정수 | unsigned char |
| Int16Array | -32,768 ~ 32,767 | 부호있는 16비트 정수 | short |
| Uint16Array | 0 ~ 65,535 | 부호없는 16비트 정수 |	unsigned short |
| Int32Array | -2,147,483,648 ~ 2,147,483,647 |	부호있는 32비트 정수 | int |
| Uint32Array | 0 ~ 4,294,967,295 | 부호없는 32비트 정수 | unsigned int |
| Float32Array | -3.4 x 10의 38승 ~ 3.4 x 10의 38승 | 32-bit IEEE floating point number | float |
| Float64Array | -1.79 x 10의 308승 ~ 1.79 x 10의 308승 | 64-bit IEEE floating point number | double |


### Buffer
일단 8 바이트 크기의 버퍼를 생성하는 방법은 아래와 같다.

```
var buf = new ArrayBuffer(8);
```

### View
뷰는 다음과 같이 생성한다.

```
var view = new DataView(buf);
```

DataView 는 다양한 형태의 데이터를 읽고 쓸 수 있다. 뷰를 생성했다면, 뷰의 메소드들을 사용할 수 있다. 뷰의 get/set 메소드들은 아래와 같은 것들이 있다.

#### set

| 이름 | 설명 |
| -------- | -------- |
| setInt8 | 1 바이트 크기의 value 를 설정한다 |
| setUint8 | 1 바이트 크기의 value 를 설정한다 |
| setInt16 | 2 바이트 크기의 value 를 설정한다 |
| setUint16 | 2 바이트 크기의 value 를 설정한다 |
| setInt32 | 4 바이트 크기의 value 를 설정한다 |
| setUint32 | 4 바이트 크기의 value 를 설정한다 |
| setFloat32 | 4 바이트 크기의 value 를 설정한다 |
| setFloat64 | 8 바이트 크기의 value 를 설정한다 |

#### get

| 이름 | 설명 |
| ----- | ----- |
| getInt8 | 1 바이트 크기의 value 를 반환한다 |
| getUint8 | 1 바이트 크기의 value 를 반환한다 |
| getInt16 | 2 바이트 크기의 value 를 반환한다 |
| getUint16 | 2 바이트 크기의 value 를 반환한다 |
| getInt32 | 4 바이트 크기의 value 를 반환한다 |
| getUint32 | 4 바이트 크기의 value 를 반환한다 |
| getFloat32 | 4 바이트 크기의 value 를 반환한다 |
| getFloat64 | 8 바이트 크기의 value 를 반환한다 |

### 데이터 읽고 쓰기
생성한 뷰에 데이터를 써보겠다.

```
view.setInt8(0, 1);
view.setInt8(1, 2);
view.setInt8(2, 3);
view.setInt8(3, 4);
view.setInt8(4, 5);
view.setInt8(5, 6);
view.setInt8(6, 7);
view.setInt8(7, 8);
```

첫밴째 인자는 값을 설정하는 버퍼의 위치이다. 그리고 두번째 인자는 해당 위치에 설정되는 값이다.

```
view.getInt8(0); //0번째 위치의 값을 반환. 1
view.getInt8(1); //1번째 위치의 값을 반환. 2
view.getInt8(2); //2번째 위치의 값을 반환. 3
view.getInt8(3); //3번째 위치의 값을 반환. 4
view.getInt8(4); //4번째 위치의 값을 반환. 5
view.getInt8(5); //5번째 위치의 값을 반환. 6
view.getInt8(6); //6번째 위치의 값을 반환. 7
view.getInt8(7); //7번째 위치의 값을 반환. 8
```

첫번째 인자로 위치를 지정하면 해당 위치의 값을 반환한다.

2 바이트를 차지하도록 값을 설정해 보도록 하겠다.

```
view.setInt16(0, 32001);
view.setInt16(2, 32002);
view.setInt16(4, 32003);
view.setInt16(6, 32004);
```

```
view.getInt16(0);
view.getInt16(2);
view.getInt16(4);
view.getInt16(6);
```
0 번째부터 2바이트를 읽으므로 위치를 0, 2, 4, 6 으로 지정하였다.


### 특정 형태의 데이터 뷰 생성하기

```
var view = new Uint8Array(buf);
```

해당 view 는 unsigned char 형태의 데이터만 읽거나 쓸 수 있다. 고로 해당 뷰는 0 ~ 255 까지의 데이터만을 설정할 수 있는 것이다.

해당 뷰에 데이터를 쓰기 위해서는 아래와 같이 한다.
```
view[0] = 1;
view[1] = 2;
view[2] = 3;
view[3] = 4;
view[4] = 5;
view[5] = 6;
view[6] = 7;
view[7] = 8;
```

뷰에서 데이터를 읽기 위해서는 아래와 같이 한다.

```
view[0]; //1
view[1]; //2
view[2]; //3
view[3]; //4
view[4]; //5
view[5]; //6
view[6]; //7
view[7]; //8
```

### 한글 인코딩하기
Uint8Array 로 생성한 뷰에 한글을 데이터로 설정하려면 어떻게 해야하는 것일까? 일단 charCodeAt 메소드를 이용하여 문자의 유니코드 값을 반환받는다. 그런데 영문이 아닌 경우 유니코드 값은 255 가 넘어 갈 것이다. 자료형이 표현할 수 있는 범위를 넘어서는 것이다. 이 경우 Uint8Array 의 뷰에 2 바이트를 차지하도록 데이터를 설정해야하는 것이다.

```
var view = new Uint8Array(buf);
view[0] = xxx;
view[1] = xxx;
//위 두바이트가 한글자이다.
```

위와 같이 설정하려면 어떻게 해야할까? 이는 비트연산과 논리곱을 통해 수행할 수 있다.

```
var text = "가나다라마바사";
var buf = new ArrayBuffer(text.length * 2);
var view = new Uint8Array(buf);
var unicode = 0;
var index = 0;

for(var i=0; i<text.length; i++){
  unicode = text.charCodeAt(i);
  view[index] = unicode >>> 8;
  view[index + 1] = unicode & 0xFF;
  index = index + 2;
}
```

우선 각 문자에 대한 유니코드 값을 반환받는다. "가"의 경우 10진수 유니코드 값은 44032 이다. 이를 오른쪽으로 1바이트만큼 비트연산을 수행하면 172 가 된다. 이를 2 바이트의 첫번째 바이트에 저장한다. 그리고 유니코드 값에 255를 논리곱을 수행하여 두번째 바이트에 저장한다. 이렇게 하면 첫번째 바이트에는 172, 두번째 바이트에는 0 이 저장되었을 것이다.

이를 다시 원래 문자로 읽기 위해서는 다음과 같이 한다.

```
var text2 = "";
for(var i=0; i<view.length; i=i+2){
  unicode = (view[i] * 255) + view[i] + view[i + 1];
  text2 = text2 + String.fromCharCode(unicode);
}
```

공식은 다음과 같다.
```
(비트연산의 값 * 255) + 비트연산의 값 + 논리곱의 값
```

데이터뷰를 사용하여 읽을 경우, 2바이트를 한꺼번에 읽을 수 있다. 위 공식을 적용한 것과 같은 결과를 반환한다.

```
var text2 = "";
var dataView = new DataView(buf);
for(var i=0; i<dataView.byteLength; i=i+2){
  unicode = dataView.getUint16(i);
  text2 = text2 + String.fromCharCode(unicode);
}
```
