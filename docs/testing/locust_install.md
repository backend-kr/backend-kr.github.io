---
layout: default
title: Locust 실무에 적용하기
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
{: .note-title }
> locust 실행 시 파일명은 반드시 locustfile.py로 생성해야합니다.

{% highlight default %}
locust -f <경로>/locustfile.py
{% endhighlight %}

