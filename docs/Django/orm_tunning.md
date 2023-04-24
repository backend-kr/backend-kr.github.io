---
layout: default
title: ORM 최적화
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
이 챕터에서는 장고의 ORM의 각 쿼리들과 실제 실무에서 접하는 이슈들을 정리할 예정입니다.

## only vs value?

Django ORM에서 only와 value는 쿼리셋에서 반환되는 필드를 제어하는 데 사용됩니다. 이 둘은 유사하지만 약간의 차이가 있습니다.

`only`는 쿼리셋에서 반환되는 필드를 선택하는 데 사용됩니다. 

즉, `only`로 선택한 필드만이 쿼리 결과로 반환됩니다. `only`를 사용하면 쿼리의 실행 속도를 높일 수 있습니다. 

이것은 데이터베이스에서 불필요한 필드를 가져오지 않으므로 불필요한 리소스를 소비하지 않기 때문입니다.

`value`는 쿼리셋에서 반환되는 필드의 값을 변경하는 데 사용됩니다. 

즉, `value`로 선택한 필드의 값만이 쿼리 결과로 반환됩니다. 

이것은 쿼리 결과에서 특정 필드의 값만을 가져와야 할 때 유용합니다.

```python
from myapp.models import MyModel

# only
# 필요한 필드만 선택하여 가져오는 쿼리셋
qs = MyModel.objects.only('name', 'age')

# qs가 object로 반환됩니다.
for obj in qs:
    print(obj.name, obj.age)
    
# value
# 필드 값만 가져오는 쿼리셋
qs = MyModel.objects.values('name', 'age')

# qs가 dict 형태로 반환됩니다.
for obj in qs:
    print(obj['name'], obj['age'])

# 실행 결과
> John 25
> Mary 30
> David 35
```
only와 value 모두 쿼리 결과에서 필요한 필드만 가져오므로, 

쿼리 실행 속도가 빨라지는 장점이 있습니다. 또한 둘다 쿼리셋이 먼저 실행되고 쿼리 결과가 캐시됩니다.

## only vs value 차이는??

`only`는 데이터베이스에서 불필요한 필드를 가져오지 않기 때문에, 필요한 필드가 많을수록 더 나은 성능을 발휘합니다.  

반면에, `value`는 모든 필드를 가져오고 나서 필요한 필드만 선택하는 것이기 때문에, 필요한 필드가 적을수록 더 나은 성능을 발휘합니다.

또한 `only`는 딱 필요한 필드만 가져오기 때문에 추가적으로 모델안에서 다른 필드를 조회하면 쿼리를 재요청하는 문제가 있기 때문에 필드 사용이 변동이 없을만한 상태에서만

사용하는것을 권장합니다.