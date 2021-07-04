---
layout: post
title: Javascript-Singlton
date: "2021-07-03 23:00"
categories: javascript
tags: [javascript, js, design pattern, es5, ecma, singleton]
published: false
---


## 싱글톤
싱글톤이란 소프트웨어 전체에서 인스턴스를 하나만 생성해서 유지하는 디자인 패턴입니다. 인스턴스를 하나만 생성한다는 것에 주목 하면 됩니다. 기존 OOP언어에서는 특정 클래스에서 인스턴스를 생성할 때 처음 호출시에는 객체를 생성하지만 두번째 부터는 생성한 객체를 반환합니다. 다음과 같습니다.

'''
class Animal {
    private static Animal animal = null;

    public static Animal getInstance () {
        if (animal == null) {
            this.animal = new Animal();
        }
        return this.animal;
    }
}

Animal animal = Animal.getInstance();
'''

자바스크립트에서는 객체 리터널로 선언된 객체가 싱긍톤이라고 볼 수 있습니다. 해당 객체를 소프트웨어 전체에서 고유하게 유지하기만 하면됩니다.
