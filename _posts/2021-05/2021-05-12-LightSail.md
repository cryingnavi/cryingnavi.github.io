---
layout: post
title: Putty로 아마존 라이트세일서버에 접속하기
date: "2021-05-10 12:00"
categories: server
tags: [lightsail, aws, putty, terminal]
published: true
---


Putty로 아마존 라이트세일 서버에 접속해보겠습니다. 일단 관리 콘솔에서 생성한 인스턴스에 접속합니다.



## Pem 다운로드
![라이트세일putty](/assets/images/2021-05-12/라이트세일putty1.png)

![라이트세일putty](/assets/images/2021-05-12/라이트세일putty2.png)

<br/>
위 과정을 통해 pem을 다운로드 받습니다.


## 프라이빗키 만들기
puttygen을 실행합니다.
![라이트세일putty](/assets/images/2021-05-12/라이트세일putty3.png)
위와 같이, 다운 받은 pem을 load 하고 Save Private Key를 클릭하여 프라이빗키를 저장합니다. 저장시에는 원하는 이름을 지정하면 됩니다.

## putty로 접속하기
putty를 실행합니다.
![라이트세일putty](/assets/images/2021-05-12/라이트세일putty4.png)
좌측 카테고리에서 SSH > Auth로 이동합니다. 그리고 위에서 만들었던 프라이빗 키를 지정합니다.

![라이트세일putty](/assets/images/2021-05-12/라이트세일putty5.png)
이젠 다시 Session 메뉴로 이동합니다. <br/>
Host Name에 라이트세일의 아이피 주소를 입력하고, Saved Session쪽에 저장할 이름을 지정합니다. 그리고 open을 클릭하면 아래와 같이 접속할 수 있습니다.


![라이트세일putty](/assets/images/2021-05-12/라이트세일putty6.png)
라이트세일의 우분투 인스턴스는 계정 이름이 <strong>ubuntu</strong> 입니다. ubuntu라고 계정이름을 입력하면 최종 접속이 완료됩니다.



