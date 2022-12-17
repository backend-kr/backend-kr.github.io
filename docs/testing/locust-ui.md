---
layout: default
title: Locust 실무에 적용하기 - 2 (UI 설명)
parent: Locust
grand_parent: Testing
---
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}
---
## 설정 페이지
![](../../assets/images/testing/init.png)

* ### Number of users
최종적으로 트래픽을 만들어내는 유저 수
* ### Spawn rate 
몇초에 한번씩 유저가 추가되는지
* ### Host 
요청보낼 타겟 서버 주소
* ### Start swarming(부하 테스트가 진행)
요청수, 실패 수, 종류별 응답 시간 등을 볼 수 있다
![](../../assets/images/testing/start.png)

## 요청 페이지
![](../../assets/images/testing/failure.png)
* 상단 오른편에는 실시간으로 업데이트 되는 현재상태, RPS 정보, 실패비율등이 나타납니다.

## 실시간 결과 페이지
![](../../assets/images/testing/chart.png)
* 상단에서 Chart 패널을 클릭하면 TPS와 Response Times등 시각화 자료도 볼 수 있습니다.

![](../../assets/images/testing/chart_1.png)
![](../../assets/images/testing/chart_2.png)
