---
layout: default
title: 싱글톤(Singleton)
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

어댑터는 호환되지 않는 인터페이스를 가진 객체들이 협업할 수 있도록 하는 구조적 디자인 패턴입니다.

## 예시
![](../../../assets/images/DesignPatterns/single.png)


데이터베이스 기반 애플리케이션에서 싱글톤 패턴을 적용한 사례를 살펴보자

데이터베이스에서 데이터를 읽고 쓰는 클라우드 서비스를 예로 들겠다. 이 클라우드 서비스에는 데이터베이스에 접근하는 여러 모듈이 있다.
UI(웹 앱)에서 직접 DB 연산을 수행하는 API를 호출한다.

여러 서비스가 한 개의 DB를 공유하는 구조다. 안정된 클라우드 서비스를 설계하려면 다음 사항들을 명심해야 한다.

* 데이터베이스 작업 간에 일관성이 유지돼야 한다. 작업 간 충돌이 발생하지 않아야 한다.
* 다수의 DB 연산을 처리하려면 메모리와 CPU를 효율적으로 사용해야 한다.

## 스크립트 작성
```python
class SingletonMeta(type):
    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            instance = super().__call__(*args, **kwargs)
            cls._instances[cls] = instance
        return cls._instances[cls]

class Singleton(metaclass=SingletonMeta):
    def some_business_logic(self):
        """
        비즈니스 로직을 수행하는 함수 생성
        """

if __name__ == "__main__":
    # The client code.

    s1 = Singleton()
    s2 = Singleton()

    if id(s1) == id(s2):
        print("두 인스턴스는 같은 인스턴스 입니다.")
    else:
        print("두 인스턴스는 다른 인스턴스 입니다.")
```

> ## Output
{% highlight default %}
두 인스턴스는 같은 인스턴스 입니다.
{% endhighlight %}
