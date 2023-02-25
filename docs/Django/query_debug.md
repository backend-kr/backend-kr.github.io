---
layout: default
title: Query Debug setting
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
이 챕터에서는 장고에서 호출되는 Query를 콘솔에서 log로 확인하는 setting에 대해서 정리합니다.

장고에서는 DataBase API로 ORM을 쓰기 때문에 실제 사용할 때, SQL 쿼리가 어떻게 동작하는지 알고 싶은 경우가 많습니다. 

특히 특정 기능에서 속도가 느린 경우, SQL 쿼리가 어떻게 되는지 먼저 확인하면 매우 좋습니다.

장고에서 extension으로 제공하는 디버깅 툴을 이용해서도 쿼리가 어떻게 수행되는지 볼 수 있지만, 

콘솔 창에서도 쿼리가 수행되는 내역을 볼 수 있습니다. 이번엔 콘솔 창에서 쿼리 수행 내역을 보는 간단한 방법을 알아보겠습니다.

---
## settings.py에 설정 추가
```python
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'formatters': {
        'django.request': {
            '()': 'django.utils.log.ServerFormatter',
            'format': '[%(levelname)s %(asctime)s pid: %(process)d] %(message)s',
        }
    },
    'handlers': {
        'console': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler',
            'formatter': 'django.request',
        }
    },
    'loggers': {
        "django.db.backends": {
            'handlers': ['console'],
            'level': 'DEBUG',
        },
    }
}
```

### 코드 설명

* disable_existing_loggers : 기존 로거를 비활성화
* formatters: 로그에 찍을 포멧 설정 위의 경우 디버깅레벨, 시간, process id, 메세지를 보여주게 설정하였습니다.
* handlers : console 부분의 level을 DEBUG, class는 스트림 핸들러(콘솔 창에 보여주는 핸들러)로 설정
* log level 설명: DEBUG(10) < INFO(20) < WARNING(30) < ERROR(40) < CRITICAL(50) 순입니다. 디폴트는 INFO부터 보여줍니다. -> 설정을 통해서 DEBUG부터 콘솔에 나오게 처리합니다.


### 결과
{% highlight default %}
# DEBUG 포맷 이후에 등장하는 ()의 내부값은 쿼리들이 몇초가 걸렸는지 표시해줍니다. 1초는 1000ms이고, 0.017는 17ms가 됩니다.

[DEBUG 2023-02-25 17:28:29,843 pid: 50] (0.017) SELECT @@SQL_AUTO_IS_NULL; args=None # 17ms
[DEBUG 2023-02-25 17:28:29,851 pid: 50] (0.004) SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED; args=None # 4ms
[DEBUG 2023-02-25 17:28:29,866 pid: 50] (0.013) SELECT VERSION(); args=None # 13ms
[DEBUG 2023-02-25 17:28:32,497 pid: 50] (0.002) SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED; args=None # 2ms
[DEBUG 2023-02-25 17:28:32,505 pid: 50] (0.007) SELECT `django_site`.`id`, `django_site`.`domain`, `django_site`.`name` FROM `django_site` ORDER BY `django_site`.`domain` ASC LIMIT 1; args=() # 7ms
[DEBUG 2023-02-25 17:28:32,523 pid: 50] (0.007) SELECT `django_site`.`id`, `django_site`.`domain`, `django_site`.`name` FROM `django_site` ORDER BY `django_site`.`domain` ASC LIMIT 1; args=() # 7ms
{% endhighlight %}

이후 기존 django 디버깅 모드에서 콘솔에서 ORM을 호출하여도, 쿼리셋이 나오게됩니다.
