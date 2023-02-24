---
layout: default
title: ORM CookBook
parent: Django
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
이 챕터에서는 장고의 ORM(객체 관계 매핑) 기능과 모델 기능을 활용하는 다양한 방법을 다룰 예정입니다.

### ORM(Object Relation Mapper)
* 관계형 데이터베이스의 내용을 객체로 변환해서 어플리케이션 소스코드에서 직접 객체를 조작할 수 있도록 구성.

* ORM을 사용해서 개발자는 데이터베이스를 조작하는데 SQL을 사용하지 않고 직접 객체를 사용할 수 있다.

* database의 table은 ORM에서 model로 표현되고 record는 object로 표현된다.

### 장점
* 기존 쿼리문을 작성하는 것보다 효율적이고 가독성이 좋고, 유지 보수에 적합하다.
  
* Django에 구현된 각 RDBMS 별 wrapper 를 통해 RDBMS 의 종류가 어떤 것인가에 상관없이 만들 수 있다.
  
* 직관적인 객체지향 프로그래밍이 가능하다.
  
* 기존의 DB 기반 구성을 객체 기반 구성으로 확장하여, 컴포넌트를 조합하는 방식의 개발이 가능하다.

### 단점
* SQL 구문의 생성을 추상화하여 구현 하였으므로, 복잡한 쿼리의 경우 비효율적으로 SQL 구문이 생성될 수 있다.

### QuerySet
* 전달받은 모델의 객체 목록. 데이터베이스의 여러 레코드(row)를 나타낸다.
  
* 쿼리셋은 데이터베이스로부터 데이터를 읽고, 필터를 걸거나 정렬을 할 수 있다.

---

## Queryset API Map
---

### Queryset을 반환하는 명령어

| 명령어      |    설 명                              |  사용법                             |
|:----------|:-------------------------------------|:-----------------------------------|
| all()   | 해당 모델 테이블의 모든 데이터 조회  | Post.objects.all() |
| filter()     | 특정 조건에 맞는 모든 데이터 조회              | Post.objects.filter(title_contains='Django') |
| exclude()    | 특정 조건을 제외한 모든 데이터 조회              | Post.objects.exclude(title_contains='Django') |
| order_by()      | 특정 조건으로 정렬된 데이터 조회(-를 붙이면 오름차순으로 정렬) | Post.objects.order_by('-created_at')
| values()     | 쿼리셋의 값을 딕셔너리 형태로 반환한다.             | Post.objects.filter(title_contains='Django').values() |
| values_list()      | 쿼리셋의 값을 튜플 형태로 반환한다.              | Post.objects.filter(title_contains='Django').values_list() |

### 단일 데이터 객체를 반환하는 명령어 -> 응답값이 복수로 돌아올시 Error가 발생합니다.

| 명령어      |    설 명                              |  사용법                             |
|:----------|:-------------------------------------|:-----------------------------------|
| get()   | 조건에 맞는 하나의 데이터 조회 | Post.objects.get(id=1) |
| create()     | 하나의 데이터를 생성하고 해당 모델 데이터를 반환         | Post.objects.create(title='Django', context='Good') |
| get_or_create()    | 조건에 맞는 데이터를 조회하고 해당 데이터가 없으면 생성            | Post.objects.get_or_create(title='Django', context='Good') |
| latest()      | 주어진 필드 기준으로 가장 최신 데이터 반환 | Post.objects.latest('-created_at') |
| first()     | 쿼리셋의 가장 첫번째 모델 데이터를 반환, 정렬하지 않은 쿼리셋이면 pk를 기준으로 정렬 후 반환, 만약 없으면 None 반환          | Post.objects.order_by('-created_at').first()  |
| last()      | 쿼리셋 중 가장 마지막 데이터 반환, 없으면 None.              | Post.objects.order_by('-created_at').last()    |
| get_object_or_404()      | 조건에 맞는 하나의 데이터 조회, 없으면 404 Error. 이 명령어를 사용하면, 예외처리를 할 필요가 사라지므로 View에서 사용하기 유용하다.             | get_object_or_404(Post, id=1)    |
| aggregate()      | 필드 전체의 합, 평균, 개수 등을 계산할 때 사용한다.(from django.db.models import Sum)과 함께 사용한다. 이 외 Count, Case 등 다양하게 엮어서 사용할 수 있다.          | Order.objects.aggregate(total_price=Sum('price')    |


### 그 외 명령어

| 명령어      |    설 명                              |  사용법                             |
|:----------|:-------------------------------------|:-----------------------------------|
| exists()   | 쿼리셋에 데이터가 존재하면 True 반환  | Post.objects.get(id=1).exists() |
| count()     | 쿼리셋의 개수를 정수로 반환          | Post.objects.all().count() |
| update()    | 데이터를 수정할 때 사용, 수정된 데이터의 개수를 정수로 반환           | Post.objects.filter(email__icontains='user').update(context='New Data') -> 생성일이 2022년인 모든 데이터의 context 값을 New Data로 바꾼다.|
| delete()      | 데이터를 삭제할 시 사용 | Post.objects.get(id=1) -> Post.delete() |

### 필드 조건 옵션(Field Lookups)

| 명령어      |    설 명                              |  사용법                             |
|:----------|:-------------------------------------|:-----------------------------------|
| __contains   | 대소문자를 구분하여 문자열 포함 여부 확인 | Post.objects.filter(email__contains='Test') |
| __icontains     | 대소문자를 구분하지 않고 문자열 포함 여부 확인      | Post.objects.filter(email__contains='user') |
| __in    | 반복 가능한 객체 안에서의 포함 여부를 확인      | Post.objects.filter(id__in=[1,2,3])|
| __gt      | 초과 여부 확인(greater than) | Post.objects.filter(id__gt=1)  |
| __gte      | 이상 여부 확인(greater than equal) | Post.objects.filter(id__gte=1)  |
| __lt      | 미만 여부 확인(little than) | Post.objects.filter(id__lt=4) |
| __lte      | 이하 여부 확인(little than equal) | Post.objects.filter(id__lte=4) |
| __startswith      | 대소문자를 구분하여 문자열로 시작하는지 여부 확인 | Post.objects.filter(email__startswith='test')  |
| __istartwith      | 대소문자를 구분하지 않고 문자열로 시작하는지 여부 확인 | Post.objects.filter(email__istartwith='test')  |
| __endswith      | 대소문자를 구분하여 문자열로 끝나는지 여부 확인 | Post.objects.filter(email__endswith='Com')  |
| __iendswith      | 대소문자를 구분하지 않고 문자열로 끝나는지 여부 확인 | Post.objects.filter(email__endswith='com') - |
| __range      | range로 제시하는 범위 내에 포함되는지 확인 | Post.objects.filter(created_at__range=(datetime.date(2022,1,1), datetime.date(2023,1,1))  |
| __isnull      | 해당 필드가 Null인지 여부 확인 | Post.objects.filter(emil__isnull=True) |