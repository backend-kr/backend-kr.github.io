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

### 팩토리 메소드 패턴
다음은 팩토리 메소드 패턴의 개념을 이해하는 데 필요한 내용입니다.
> 인터페이스를 통해 객체를 생성하지만 팩토리가 아닌 서브 클래스가 해당 객체 생성을 위해 어떤 클래스를 호출할지 결정한다.
> 팩토리 메소드는 인스턴스화가 아닌 상속을 통해 객체를 생성한다.
> 팩토리 메소드 디자인은 유동적이다. 심플 팩토리 메소드와는 다르게 특정 객체 대신 인스턴스나 서브 클래스 객체를 반환할 수 있다.
> 

---

## 예시
![](../../../assets/images/DesignPatterns/facebook.png)

실제 사례를 기반으로 팩토리 메소드를 구현해보자. 페이스북이나 인스타그램 같은 소셜 네트워크에 특정 사용자나 기업의 프로필을 작성하는 경우를 예로 들 수 있다.

프로필에는 여러 섹션이 있다. 페이스북에는 최근 다녀온 여행지에서 찍은 사진을 올릴 수 있다.

두 서비스의 공통적으로 기입해야 하는 개인 정보도 있다.

이렇게 서비스 종류에 따라 알맞은 내용을 포함하는 프로필을 생성하는 프로그램을 구현해보자

---

### [팩토리 메소드] 스크립트 작성
```python
# product.py
class Section:
    def describe(self):
        pass


class PersonalSection(Section):
    def describe(self):
        print("Personal Section")


class AlbumSection(Section):
    def describe(self):
        print("Album Section")


class PatentSection(Section):
    def describe(self):
        print("Patent Section")


class PublicationSection(Section):
    def describe(self):
        print("Publication Section")

# creator.py
class Profile:
    def __init__(self):
        self.sections = []
        self.create_profile()

    def create_profile(self):
        pass

    def get_sections(self):
        return self.sections

    def add_sections(self, section):
        self.sections.append(section)


class instagram(Profile):
    def create_profile(self):
        self.add_sections(PersonalSection())
        self.add_sections(PatentSection())
        self.add_sections(PublicationSection())


class facebook(Profile):
    def create_profile(self):
        self.add_sections(PersonalSection())
        self.add_sections(AlbumSection())


if __name__ == "__main__":
    profile_type = input("생성할 프로필을 선택해 주세요 [Facebook or Instagram]")
    profile = eval(profile_type.lower())()
    print(f"계정 생성중,,,{type(profile).__name__}")
    print(f"계정 섹션 -- {profile.get_sections()}")

```
> ## Output
{% highlight default %}
생성할 프로필을 선택해 주세요 [Facebook or Instagram]Facebook
계정 생성중,,,facebook
계정 섹션 -- [<__main__.PersonalSection object at 0x10b6eca30>, <__main__.AlbumSection object at 0x10b6ecf70>]
{% endhighlight %}

---


### 팩토리 메소드 장점

> * 특정 클래스에 종속적이지 않기 때문에 구현 및 개발이 쉽다.
> 
> * 객체를 생성하는 코드와 활용하는 코드를 분리해 의존성이 줄어든다. 클라이언트는 어떤 인자를 넘겨야하고, 어떤 클래스를 생성해야 하는지 걱정할 필요가 없다.
> 
> * 새로운 클래스를 쉽게 추가할 수 있고, 유지보수가 쉽다.