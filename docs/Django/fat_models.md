---
layout: default
grand_parent: Django
parent: Django 비즈니스 로직 구성
title: Fat Models
---
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}
   
---
### Fat Models란?

Fat model이란 말 그대로 모델에 비즈니스 로직을 넣어 크게 만든다는 뜻입니다.

MVC의 기본 설계 패턴은 Fat Model, Thin Controller이다. Model에 기능을 모아두고, Controller은 중개역할만 하는거다.

이 경우, 코드의 중복이 줄어들고, 객체지향적으로 코드를 설계하기 편하다는 장점이 있다. 

코드를 보면서 이해해봅시다.


{: .warning }
설명을 위해 급하게 짠 코드이므로 양해 먼저 부탁드립니다.

> ### 요점 : 모델 내부 함수를 통해서 비즈니스 로직이 구현 가능하다.

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
        bank = serializer.save()
        bank.set_guide(request.data)
        return Response(bank, status=status.HTTP_201_CREATED)
 
# models.py
 
class Bank(TimeModelMixin, Account):
    account = models.ForeignKey(Account, on_delete=models.CASCADE) # 계좌번호
    vendor_name = models.CharField(max_length=200, blank=True, default='') # 은행명
    vendor_code = models.CharField(max_length=200, blank=True, default='') # 은행사 코드
 
    def set_guide(self, data):
        # 입력된 나라에 따라 저장될 상태 분기
        if data.get('country') == 'kr':
            self.vendor_name = '신한은행'
            self.vendor_code = '신한'
        else:
            self.vendor_name = 'Shinhan'
            self.vendor_code = 'SHIN'
 
        self.save()
    ...

```

위 코드를 보면 model에 비즈니스 로직이 들어가 있는걸 볼 수 있습니다.

`view`에서 비즈니스 로직을 처리하지 않고, model에서 처리하므로 view가 간결해지는 장점이 존재하지만,

서비스가 커지게 되면 model에 많은 비지니스 로직이 들어가 복잡하게 되고, 

다른 model에서 상속을 하는 경우가 있다면 모든 비지니스 로직을 상속하게 됩니다. 

이런 경우 추상화(Abstarct)가 되어있지 않다면 사이드 이슈가 생길 수도 있습니다.

결국 프로젝트, 비즈니스 로직이 커지고 복잡해지게 되면 model을 분리해야 하는 레거시 코드가 될 수도 있습니다. 

{: .note }
위 문제는 [Django Proxy Mode]을 사용하여 해결 할 수 있습니다.


[Django Proxy Mode]: https://www.benlopatin.com/using-django-proxy-models/