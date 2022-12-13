---
layout: default
title: Locust
parent: Testing
has_children: true
---
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}
---
# Locust란?

![](../../assets/images/testing/locust.png)

이 장에선 스트레스 테스팅 도구(혹은 부하 테스트)들 중 Locust에 대해서 설명하고자 한다.

스트레스 테스팅 도구(Stress testing tools)들은 시스템의 견고성을 테스트하기 위해 높은 트래픽을 일괄로 처리하여, 

지속적인 입력 스트림을 서버로 보내도록 설정할 수 있다. 즉, 동시 사용자 수를 파악하고 서버에서 지원하는 초당 요청 수 
(RPS)를 대략적으로 알기 위함이다.


## Locust가 주는 이점

- #### 구현이 쉬워서 작은 규모 프로젝트에 적합합니다.
- #### python으로 api 테스트 코드를 작성할 수 있습니다.
- #### 설치와 실행이 간단하고 대시보드를 통해 가시적으로 테스트 결과를 볼 수 있습니다.
- #### 여러개의 worker를 이용하여, 부하를 대량으로 발생 시키는 분산 부하 테스트가 가능하다. 
- #### 분산 클러스터 구성 설정이 매우 간단하다. 
