---
layout: post
title: Javascript 싱글톤 패턴
date: "2021-07-03 23:00"
categories: javascript
tags: [javascript, js, design pattern, es5, ecma, singleton]
published: true
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

자바스크립트에서는 객체 리터널로 선언된 객체가 싱글톤이라고 볼 수 있습니다. 해당 객체를 소프트웨어 전체에서 고유하게 유지하기만 하면됩니다.

'''
const Animal = { };
'''


### 싱글톤 객체 생성 패턴
싱글톤 객체를 생성하는 다양한 패턴이 있을 수 있습니다. 여기서는 총 세개의 패턴을 볼 예정입니다. 첫번째로 클로저를 이용한 단순한 형태, 두번째는 즉시 실행 함수를 이용한 방법, 그리고 세번째는 내부 클래스를 이용한 방법입니다.

#### 클로저를 이용한 방법
'''
let Animal = function () {
    const animal = this;
    Animal = function () {
        return animal;
    }

    this.run = function (){

    };
}

let o1 = new Animal();
let o2 = new Animal();

console.log(o1 === o2);

Animal 생성자가 처음 호출되면 내부에서 <code>const animal = this;</code> 클로저 변수를 선언합니다. 그리고 Animal 생성자 함수를 새로운 생성자 함수로 변경합니다. 해당 함수는 <code>const animal</code> 클로저 변수를 반환합니다.  
객체 리터널만으로도 싱글톤 객체를 생성할 수 있으니, 해당 방법은 그다지 유용한거 같지는 않습니다. 다음으로 더 유용한 방법을 보겠습니다.
'''

#### 즉시 실행 함수를 이용한 방법
'''
const Animal = (()=>{
    return {
        run () {

        }
    };
})();
'''

Animal 함수는 run메소드를 가지는 객체를 반환하여 할당 됩니다. 이제 Animal은 그 자체로 싱글톤 객체 입니다.


### Private Member
싱글톤 객체에서 private과 public 멤버들이 각각 필요할 수 있습니다. 예전에는 아래와 같이 앞에 _를 붙임으로써 이 멤버는 private라 규약을 정했던 적도 있습니다.
'''
let Animal = {
    _privateMember1: function () {},
    _privateMember2: function () {},
    publicMember1: function () {},
    publicMember2: function () {}
};
'''
_privateMember1, _privateMember2가 실제로는 접근이 가능하므로 진정한 private이라고 볼 수는 없습니다. 이는 다음과 같이 개선할 수 있습니다.

'''
const Animal = (()=>{
    let privateMember1 = function () {

    }
    let privateMember2 = function () {
        
    }
    return {
        run () {

        },
        publicMember () {

        }
    };
})();
'''

이제 privateMember1, privateMember2에는 접근이 불가능하고 run과 publicMember는 접근이 가능한 public이 됩니다.


### 내부 클래스 사용한 생성 패턴
이제 위 private 멤버 변수를 가지면서 내부 클래스를 통해 싱글톤 객체를 생성하는 방법을 알아보겠습니다.

'''
const Animal = (()=>{
    let instance = null;
    const InnerClass = function () {
        
    }

    InnerClass.prototype.run = function () { };

    let privateMember1 = function () { };

    return {
        getInstance () {
            if (instance === null) {
                instance = new InnerClass();
            }
            return instance;
        }
    };
})();

const o1 = Animal.getInstance();
const o2 = Animal.getInstance();

console.log(o1 === o2);
'''


### 네임스페이스
마지막으로 네임스페이스에 관해 알아봅니다.  지금은 모듈 시스템을 이용하여 파일 기반의 모듈로 로직을 구분하여 사용합니다. 이전에는 파일로 구분하여도 전역 영역을 오염시킬 염려가 많았습니다.  
이 때, 네임스페이스를 사용해서 로직을 구분할 수 있습니다. 네임스페이스는 기능을 중점으로 나눌 수도 있으며 페이지 기반으로 나눌 수도 있습니다.  
싱글톤 객체는 객체 하위에 또 다른 객체를 둠으로써 그 자체로 네임스페이스의 역할을 수행할 수 있습니다. 또한, 위에서도 설명했듯이, 객체 리터널을 생성함만으로도 싱글톤 객체를 생성할 수 있습니다.


- 기능 중심으로 네임스페이스 생성하기
'''
let MyNameSpace = { };
MyNameSpace.Common = { };
MyNameSpace.Common.log = { };
MyNameSpace.Common.Http = { };
MyNameSpace.Error = { };
'''


- 페이지 중심으로 네임스페이스 생성하기
'''
let MyNameSpace = { };
MyNameSpace.Page1 = { };
MyNameSpace.Page2 = { };
MyNameSpace.Page3 = { };
'''

과거 널리 사용되는 SPA 프레임웍이 없을때, 자체 구현하던 SPA 프레임웍에서는 위와 같이 페이지 단위의 구분하여 사용하기도 했습니다.


다음에는 팩토리 패턴에 대해 알아봅니다.
