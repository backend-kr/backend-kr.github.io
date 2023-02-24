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

장고는 모델-템플릿-뷰(MTV) 프레임워크입니다. 이 책은 그 가운데 ‘모델’에 대해 상세히 다룹니다.

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

### 데이터 객체를 반환하는 명령어

| 명령어      |    설 명                              |  사용법                             |
|:----------|:-------------------------------------|:-----------------------------------|
| get()   | 조건에 맞는 하나의 데이터 조회 | Post.objects.get(id=1) |
| create()     | 하나의 데이터를 생성하고 해당 모델 데이터를 반환         | Post.objects.create(title='Django', context='Good') |
| get_or_create()    | 조건에 맞는 데이터를 조회하고 해당 데이터가 없으면 생성            | Post.objects.get_or_create(title='Django', context='Good') |
| latest()      | 주어진 필드 기준으로 가장 최신 데이터 반환 | Post.objects.latest('-created_at') |
| first()     | 쿼리셋의 가장 첫번째 모델 데이터를 반환, 정렬하지 않은 쿼리셋이면 pk를 기준으로 정렬 후 반환, 만약 없으면 None 반환          | Post.objects.order_by('-created_at').first()  |
| last()      | 쿼리셋 중 가장 마지막 데이터 반환, 없으면 None.              | Post.objects.order_by('-created_at').last()    |