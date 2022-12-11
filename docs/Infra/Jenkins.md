---
layout: default
title: Jenkins
parent: Infra
---
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}
   
---
# Jenkins란?
이 장에선 Docker환경에서 Jenkins 빌드 자동화 구축에 대해서 알아보겠습니다.
젠킨스는 소프트웨어 개발 시 지속적으로 통합 서비스를 제공하는 툴입니다. CI(Continuous Integration) 툴 이라고 표현한다.

![img](https://www.crowdstrike.com/wp-content/uploads/2018/10/Jenkins-blog-image-1.jpg)

## 젠킨스가 주는 이점

개발중인 프로젝트에서 커밋은 매우 빈번히 일어나기 때문에 커밋 횟수만큼 빌드를 실행하는 것이 아니라 자동으로 빌드와 배포를 수행하게 처리하여
커밋과 동시에 실제 서버에 배포를 도와줍니다. 대표적인 장점은 아래와 같습니다.

- ### Build 자동화의 확립
  
  커밋과 동시에 서버에 자동으로 배포되는 환경을 통해서 로컬에서 개발한 코드를 실제 서버에서 즉각적으로 확인할 수 있다.
  
- ### 각종 배치 작업의 간략화
  
  프로젝트를 진행 하다보면 어플리케이션의 작업 외적으로 배치 작업이 필요한 경우가 있다. 젠킨스 내부의 스케줄러를 사용하면
간단하게 소스코드의 수정 없이 간단하게 조작할 수 있다.
  

# How to Install

이 글에서의 Jenkins 설치는 docker가 설치 되어있는 ubuntu 18.04 환경에서 진행합니다.

글의 시작은 Docker 명령어로 시작하되, 마지막엔 `docker-compose.yml`로 정리할 예정이다.

## Docker Hub을 통한 Jenkins 이미지 가져오기
{% highlight default %}
$ docker pull jenkins/jenkins:lts
{% endhighlight %}

위 명령어를 통해서 Jenkins LTS(Long Term Support) 버전의 이미지를 다운로드 받습니다.
LTS 이미지로 설치하는 이유는 가장 안정화 되어있는 버전이기 때문에 LTS로 설치하였습니다.

## Jenkins 컨테이너 띄우기
{% highlight default %}
$ sudo docker run -d -p 4000:8080 -v `pwd`/jenkins_home:/var/jenkins_home --name jenkins -u root jenkins/jenkins:lts
{% endhighlight %}

#### 명령어 알아보기
{: .no_toc }

{: .note-title }
> Docker 명령어 정리
>
> -d: 컨테이너를 데몬으로 띄웁니다.
> 
> -p 4000:8080 : 컨테이너 외부와 내부 포트를 포워딩합니다. 좌측이 호스트 포트, 우측이 컨테이너 포트입니다.(필자들은 8080포트를 다른곳에서도 사용할 예정이기 때문에 
  중복을 피하기 위해서 4000번을 외부 포트로 사용하였습니다.)
> 
> v 'pwd'/jenkins:/var/jenkins_home: 도커 컨테이너의 데이터는 컨테이너가 종료되면 휘발됩니다. 
  도커 컨테이너의 데이터를 보존하기 위한 여러 방법이 존재하는데, 그 중 한 방법이 볼륨 마운트입니다. 
  이 옵션을 사용하여 젠킨스 컨테이너의 /var/jenkins_home 이라는 디렉토리를 호스트의 <현재 디렉토리>/jenkins_home와 마운트하고 데이터를 보존할 수 있습니다.
  이것을 하는 이유는, Jenkins 설치 시 ssh 키값 생성, 저장소 참조 등을 용이하게 하기 위함입니다.
> 
> -u root : 컨테이너가 실행될 리눅스의 사용자 계정을 root 로 명시합니다. 이 명령어가 없으면 -v를 사용할 시 permission denied가 발생할 수 있습니다.

## Jenkins 최초 로그인

도커를 사용하여 젠킨스 컨테이너가 성공적으로 띄워졌다면, EC2의 퍼블릭 IP를 통해 외부에서 접속할 수 있을 것 입니다.
localhost:[지정한 포트]로 접속하면 아래와 같은 화면이 보일 것 입니다.
  ```yaml
  EX) localhost:1111
      필자의 경우 상위 -p 옵션을 사용하여 1111포트로 접속을 하도록 명시하였다
  ````

![img](https://hudi.blog/static/960b1726cd3b94778b3a7bf6dba2b159/7c958/unlock-jenkins.png)

처음 접속을 위해선 Jenkins에서 제공해주는 inital admin password를 사용해야하는데, 해당 정보는 우리가 생성한 docker container 로그에서 확인할 수 있다.
{% highlight default %}
$ sudo docker logs jenkins
{% endhighlight %}

- 마지막 jenkins는 우리가 컨테이너를 생성할 때 만들어둔 컨테이너의 이름이다. [--name jenkins]

이제 로그를 보면서 최초 비밀번호를 확인해보자.
```yaml
2022-12-11 13:12:51.941+0000 [id=44]	INFO	jenkins.install.SetupWizard#init: 

*************************************************************
*************************************************************
*************************************************************

Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation:

55a9819ea38247b39c83a324760ae431  ==> 우리에게 필요한 비밀번호 정보이다.

This may also be found at: /var/jenkins_home/secrets/initialAdminPassword

*************************************************************
*************************************************************
*************************************************************

2022-12-11 13:13:01.148+0000 [id=44]	INFO	jenkins.InitReactorRunner$1#onAttained: Completed initialization
2022-12-11 13:13:01.181+0000 [id=24]	INFO	hudson.lifecycle.Lifecycle#onReady: Jenkins is fully up and running
```

혹은 아래의 명령으로 jenkins 컨테이너 내부에 직접 접속하여, 
/var/jenkins_home/secrets/initialAdminPassword 파일의 내용을 조회하는 방법도 있습니다.
{% highlight default %}
$ sudo docker exec -it jenkins /bin/bash
$ cat /var/jenkins_home/secrets/initialAdminPassword
{% endhighlight %}

마지막으로 필자는 Install sugeested plugins를 사용하여 설치를 진행하였습니다.

![img](https://hudi.blog/static/34033c88d26a34735801aa2c4d16bf9a/d9349/customize-jenkins.png)

## docker-compose.yml로 모든 작업 통합하기
{: .no_toc }

{: .note-title }
> Docker compose로 모든 작업 한번에 끝내기!
{% highlight default %}
version: '3'
  services:
    jenkins:
      restart: always
      image: jenkins/jenkins:lts
      ports:
        - 4000:8080
      volumes:
        - ./jenkins_home:/var/jenkins_home
{% endhighlight %}

- 마지막으로 상위 내용을 한번에 빌드까지 해버릴 수 있게 docker-compose 파일로 생성하였습니다.
필요하시면 그대로 사용하셔도 무방합니다.