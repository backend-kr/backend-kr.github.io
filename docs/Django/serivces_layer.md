---
layout: default
grand_parent: Django
parent: Django 비즈니스 로직 구성
title: Service Layer
---

{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}
   
---
### Service Layer란?

Service Layer란 모델과 뷰 사이에 비즈니스 로직을 담당하는 개념을 추가하는 것을 의미한다.

Service Layer가 추가되면, 모델에는 객체의 속성만 관리하게 된다.


{: .warning }
설명을 위해 급하게 짠 코드이므로 양해 먼저 부탁드립니다.

> ### 요점 : 계층간 데이터 교환을 위해 객체를 생성하여 내부에서 비즈니스 로직을 처리할 수 있습니다.

```python
# views.py
class BankViewSet(mixins.CreateModelMixin,
                       mixins.ListModelMixin,
                       GenericViewSet):
    """
        - 계좌 생성 및 취소 관련 api
    """
 
    queryset = Bank.objects.all()
    serializer_class = BankSerializer
 
    def create(self, request, *args, **kwargs) -> Response:
        serializer = self.get_serializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        create_account = serializer.save()
        
        # Serivice 쪽에서 비지니스 로직을 처리
        _bank = Bank()
        _bank.set_guide(create_account)
        
        return Response(_bank, status=status.HTTP_201_CREATED)
 
# services.py
class Bank(object):
    
    def set_guide(self, create_account):
        # 입력된 나라에 따라 저장될 상태 분기
        if data.get('country') == 'kr':
            create_account.vendor_name = '신한은행'
            create_account.vendor_code = '신한'
        else:
            create_account.vendor_name = 'Shinhan'
            create_account.vendor_code = 'SHIN'
 
        create_account.save(updated_fields=['vendor_name', 'vendor_code'])
    ...

```

{: .note }
service layer를 구별하여 코드를 관리하면 Test code와 Unit Test 자체도 간단하게 구현할 수 있습니다.


비즈니스 로직을 상위와 같이 service Layer에서 처리하게 되면 model, view + serializer가 모두 깔끔해집니다. 

Django에선 항상 비즈니스 로직의 위치가 정형화 되어 있지 않습니다. 회사마다 Code Convention이 있다면, 그 기준을 따르겠지만, 없다면 service Layer로 처리해보는 것도 
나쁘지 않을 것 같습니다.

{: .note }
Django에 Service Layer를 두는 몇가지 방식은 다음과 같다.

> * Form / Serializer가 Service Layer를 대신한다.

> * utils 모듈에 비즈니스 로직을 추가해서 사용한다. 

> * service 모듈에 비즈니스 로직을 추가해서 사용한다.


