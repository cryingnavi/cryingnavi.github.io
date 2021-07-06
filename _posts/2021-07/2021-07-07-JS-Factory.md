---
layout: post
title: Javascript 팩토리 패턴
date: "2021-07-07 23:00"
categories: javascript
tags: [javascript, js, design pattern, es5, ecma, factory]
published: false
---


## 팩토리
객체를 생성하는 일을 다른 곳에 맡기어 대신 객체를 생성하게끔 하는 패턴입니다. 자바같은 OOP언어에서는 인터페이스를 활용하여 인터페이스를 구현하는 클래스들과 이들의 객체를 생성하는 클래스를 분리하게 됩니다. 이를 통해 클라이언트 코드와 분리함으로써 결합도를 낮추게 됩니다.