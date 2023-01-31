---
layout: default
title: 팩토리(Factory)
parent: 생성 패턴
grand_parent: DesignPattern
---
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}
---
## 의도

객체지향 프로그래밍에서 팩토리란 다른 클래스의 객체를 생성하는 클래스를 일컫는다.

클라이언트는 특정 인자와 함께 메소드를 호출하고 팩토리는 해당 객체를 생성하고 반환한다.

## 예시
![](../../../assets/images/DesignPatterns/factory.png)

자동차나 인형 같은 완구를 제조하는 공장을 예로 들어보자.

이 업체는 자동차 장난감만 제조해왔지만 인형에 대한 시장의 수요가 늘어나는 상황을 고려해 급히 인형도 제조하기로 결정했다.

이러한 상황이 팩토리 패턴에 적합한 상황이다.

### 팩토리 패턴의 종류
> * 심플 팩토리 패턴 : 인터페이스는 객체 생성 로직을 숨기고 객체를 생성한다.
> 
> * 팩토리 메소드 패턴 : 인터페이스를 통해 객체를 생성하지만 서브 클래스가 객체 생성에 필요한 클래스를 선택한다.
> 
> * 추상 팩토리 패턴 : 추상 팩토리는 객체 생성에 필요한 클래스를 노출하지 않고 객체를 생성하는 인터페이스다. 내부적으로 다른 팩토리 객체를 생성한다.
> 
 
---

### 심플 팩토리 패턴
심플 팩토리 패턴은 팩토리 메소드 패턴과 추상 팩토리 패턴을 이해하기 위한 기본 개념이다.


### [심플 팩토리 패턴] 스크립트 작성
```python
from abc import ABCMeta


class Animal(metaclass=ABCMeta):
    def do_say(self):
        pass

class Dog(Animal):
    def do_say(self):
        print("멍 멍 !")

class Cat(Animal):
    def do_say(self):
        print("야옹!")

class AnimalFactory(object):
    def make_sound(self, object_type):
        return eval(object_type)().do_say()
    
if __name__ == "__main__":
    # The client code.
    animal_factory = AnimalFactory()
    animal = input("Dog or Cat???") # Cat
    animal_factory.make_sound(animal)
```

> ## Output
{% highlight default %}
Dog or Cat???Cat[입력값]
야옹!
{% endhighlight %}

### 코드 설명

> 
> Animal 인터페이스를 사용해 Cat과 Dog라는 상품 객체를 생성하고 각 동물의 울음소리를 출력하는 do_say() 함수를 구현한다.
> `AnimalFactory` 팩토리에는 make_sound() 함수가 있고 클라이언트가 전달한 인자에 따라 해당 Animal 인스턴스가 생성되고 출력된다.
> 

---