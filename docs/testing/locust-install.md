---
layout: default
title: Locust 실무에 적용하기 - 1 (코드)
parent: Locust
grand_parent: Testing
---
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}
---
## Locust Install

{: .note-title }
{% highlight default %}
pip install locust
{% endhighlight %}


## Run Locust

> 8089 port로 기본적으로 열립니다.


{% highlight default %}
locust -f <경로>/<파일 이름>.py
{% endhighlight %}
> 현재 위치에 locustfile.py 라는 파일이 존재하면

{% highlight default %}
locust
{% endhighlight %}

> --host 등 커맨드 옵션을 알고 싶다면
{% highlight default %}
locust --help
{% endhighlight %}


## 스크립트 작성

```python
class AccountBehavior(TaskSet):
    @task
    def get_account_number_detail(self):
        account_number = 123445152
        self.client.get(f'/asset/{account_number}')

    @task(weight=2)
    def home(self):
        self.client.get('/')

    def on_start(self):
        print("START LOCUST")

    def on_stop(self):
        print("STOP LOCUST")


class LocustUser(HttpUser):
    host = "https://locust.load-test.com"
    tasks = [AccountBehavior]
    wait_time = between(1, 4)
```

### 코드 설명

> [HttpUser]
> 
>  서버에 부하를 가할 주체를 나타낸다. tasks 애트리뷰트에 선언된 작업 또는 @task 데코레이터가 붙여진 작업를 수행한다.
> 
> [TaskSet]
> 
> 유저가 수행할 작업들을 하나의 클래스로 만들어, tasks 애트리뷰트에 선언된 작업들 또는 @task 데코레이터가 붙여진 작업들 중 랜덤으로 수행한다.
> 
> [task]
>
> HttpUser는 @task 데코레이터가 붙은 메서드를 찾아 수행하게 된다. weight 파라미터를 넣어주면 각 테스크마다 가중치를 부여할 수 있다. home이 get_account_number_detail보다 실행될 확률이 2배나 된다.
> 
> [between]
>
> HttpUser나 TaskSet의 wait_time 애트리 뷰트를 사용할 때 해당 함수를 사용할 수 있다. wait_time = between(1, 4) 선언해주면 1초 ~ 4초 사이 간격으로 랜덤하게 작업이 수행된다는 뜻이다.
> 
> [on_start]
> 
> Locust가 실행 후 처리할 로직을 처리합니다.
> 
> [on_stop]
> 
> Locust가 종료 후 처리할 로직을 처리합니다.

