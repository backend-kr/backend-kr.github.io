---
layout: default
title: Locust 실무에 적용하기 - 3 (Docker로 관리하기)
parent: Locust
grand_parent: Testing
---
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}
---

## Locust DockerFile로 구성하기

> Why Use DockerFile?
> 
- 필자들은 최근 서버 과부화 테스트를 진행하게 되었다. 이때 서로의 환경이 달라서 같은 코드이지만 실행이 안되는 문제가 발생하였고,
추가적으로 여러대의 서버를 실행시켜 과부화 테스트를 진행해야하는데 매번 locust를 따로 띄워야하는 귀찮음에 Dockerfile과 docker-compose파일로 관리하게 되었다.
  

### DockerFile 구성

{% highlight default %}
FROM python:3.7

WORKDIR /usr/src/app 

RUN pip install --no-cache-dir --upgrade pip \
    && pip install locust
{% endhighlight %}

### docker-compose.yml 구성

{% highlight default %}
services:
  server-test-0:
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - ./locustfile-0.py:/usr/src/app/test.py
    ports:
      - "9000:8089"
    command: locust -f test --host http://172.16.0.253:8080 --users 4 --spawn-rate 4

  server-test-1:
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - ./locustfile-1.py:/usr/src/app/test.py
    ports:
      - "9001:8089"
    command: locust -f test --host http://172.16.0.253:8081 --users 4 --spawn-rate 4
{% endhighlight %}

### 코드 설명

> [command]
> 
>  locust -f <파일명>으로 실행시킬 locust를 실행시켜준다. --host, --users, --spawn-rate를 지정하여 실행할때 default값을 주입시켜준다.


### 마치며
설치,실행, dockerfile 작업까지 진행해보았다. 

직접 해보면 매우 간단하게 부하 테스트를 할 수 있다는 것을 바로 체감할 수 있다! 

위 설명한 기능들 이외에 locust가 다양한 설정 및 기능들을 제공하니 자세한 사항은 아래 `reference`를 확인하면 된다.

### reference

> [Locust Documentation]

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

   [Locust Documentation]: <https://docs.locust.io/en/stable/index.html>

