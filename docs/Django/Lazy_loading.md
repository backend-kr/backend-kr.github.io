---
layout: default
title: Lazy Evaluation
grand_parent: Django
parent: ORM CookBook
has_children: true
---
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}
---

## 들어가기전
---
이 챕터에서는 장고의 Lazy Evaluation(지연연산)에 대해서 다룰 예정입니다.

---
## Lazy Evaluation 이란?

컴퓨터 프로그래밍에서 느긋한 계산법(Lazy Evaluation)은 계산의 결과 값이 필요할 때까지 계산을 늦추는 기법이다.
느긋하게 계산하면 필요없는 계산을 하지 않으므로 실행을 더 빠르게 할 수 있고, 복합 수식을 계산할 때 오류 상태를 피할 수 있고, 무한 자료 구조를 쓸 수 있고, 미리 정의된 것을 이용하지 않고 보통 함수로 제어 구조를 정의할 수 있다.

`위키백과`

즉, Lazy Evaluation은 필요없는 계산을 하지 않고 계산의 결과 값이 실제로 쓰일때까지 그 값의 계산을 뒤로 미루는 방식이다.

---
## Lazy Evaluation(지연연산) & queryset 병합

Django의 모든 Query은 병합이 가능합니다. 

예를 들어 Post 중에 email에 'user'가 들어가고 생성일자가 2022.01.01 이상인 모든 데이터 중 가장 마지막에 들어가는 데이터 중 가장 마지막에 
작성된 데이터를 가져오고 싶다고 할 때 코드는 아래와 같습니다.

{% highlight default %}
# 기본적으로 Django에서 filter를 통한 병합은 AND 연산을 채택하고 있습니다.
Post.objects.filter(email__icontains='user', created_at__gte=datetime.date(2022,1,1)).order_by('-created_at').last() 
{% endhighlight %}

문제는 모든 조건이 하나의 query 연산에 체인으로 묶여있어서 가독성을 매우 떨어트립니다. 

그러므로 체인으로 묶는 것은 지양해야합니다.

하위의 방식으로 처리하여, 가독성을 갖게 수정합니다.

{% highlight default %}
post_query = Post.objects.filter(email__icontains='user', created_at__gte=datetime.date(2022,1,1))
post_query = post_query.order_by('-created_at')
post_query = post_query.last()
{% endhighlight %}

이렇게 여러개로 나누어 작성할 경우 코드가 느려질 수도 있다고 걱정할 수 있지만 그렇지 않습니다.
Django는 기본적으로 Query 연산을 지연 로딩 즉, 지연 연산을 지원하기 때문에 실제로 데이터를 사용할 때까지 연산을 수행하지 않고 대기하고 있기 때문입니다.