---
layout: default
title: ELK
parent: Infra
---
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}
   
---
# ELK란?
이 장에선 Docker환경에서 ELK 빌드 자동화 구축에 대해서 알아보겠습니다.
"ELK"는 Elasticsearch, Logstash 및 Kibana, 이 오픈 소스 프로젝트 세 개의 머리글자입니다. 

> **Note**  
* **Elasticsearch**: 검색 및 분석 엔진 (엘라스틱서치는 검색 엔진 이면서 데이터 베이스이기도 하다.)
* **Logstash**: 여러 소스에서 동시에 데이터를 수집하여 변환한 후 Elasticsearch 같은 “stash”로 전송하는 서버 사이드 데이터 처리 파이프라인
* **Kibana**: 사용자가 Elasticsearch에서 차트와 그래프를 이용해 데이터를 시각화 툴


![img](https://images.contentstack.io/v3/assets/bltefdd0b53724fa2ce/blt745c7c0288922a62/5c11edccdf09df047814db29/elk-stack-3-elks-stacked.svg)

## ELK을 더 알아보자!

모든 시스템의 로그를 집계할 수 있습니다. 로그에서 문제를 분석할 수 있을 뿐만 아니라 시스템 사용을 모니터링하고 개선 기회를 찾을 수도 있습니다.

- ### E(Elasticsearch)
  
  엘라스틱서치는 검색 엔진 이면서 데이터 베이스이기도 하다.
  * 인덱싱 indexing
  * 병렬 & 분산 처리
  * Lucene 기반 검색 엔진 (최적화된 자료구조 지원, 스코어링 scoring 등..)
  * REST API
  * DSL (Domain Specific Language)
  
- ### L(Logstash)
  
  로그스태시는 이벤트 수집과 정제를 위한 도구이다.
  * 엘라스틱서치 인덱싱 성능 최적화
  * 로그 가공
  * 데이터 수집 및 가공
  
- ### K(Kibana)
  
  키바나는 시각화와 엘라스틱서치 관리 도구이다.
  * 엘라스틱서치의 UI
  * 시각화, 대시보드
  * 실시간 모니터링, 데이터분석, APM, SIEM 등의 솔루션
  
  ![img](https://static-www.elastic.co/v3/assets/bltefdd0b53724fa2ce/blta64f0cb4ab8c7c1c/5fa31cc94a4abb73ff79b0d5/illustrated-screenshot-hero-kibana.png)

# How to Install
이 글에서의 ELK 구축은 docker를 기반으로 구축하고, Django와 연동하여, 간단하게 APM도 확인할 예정입니다.
마지막은 모든 과정을 하나로 묶은 `docker-compose.yml`로 정리할 예정입니다.

## Github에서 코드를 가져옵니다.
{% highlight default %}
git clone https://github.com/deviantony/docker-elk
{% endhighlight %}
