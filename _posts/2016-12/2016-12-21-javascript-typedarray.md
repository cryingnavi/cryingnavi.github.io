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
일단 16바이트 크기의 버퍼를 생성하는 방법은 아래와 같다.

```
var buf = new ArrayBuffer(8);
```

이제 8 바이트 크기의 데이터를 저장할 수 있는 버퍼가 생성된 것이다.

### View
뷰는 다음과 같이 생성한다.

```
var view = new DataView(buf);
```

뷰를 생성했다면, 뷰의 메소드들을 사용할 수 있다. 뷰의 get/set 메소드드들은 아래와 같은 것들이 있다.

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


### 한글 인코딩하기


계속 쓸 예정....
