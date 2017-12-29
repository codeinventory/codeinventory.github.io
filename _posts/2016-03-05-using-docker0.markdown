---
layout: post
title:  "Using Docker"
date:   2016-03-05 01:07:01
comments: false
categories: DevOps
tags: devops, docker
---

이번에는 Docker를 실제 사용해보자. Docker 사용 예로, Project Management application인 Redmine을 사용해보자.

간단한 Docker 설명은 이전 POST [STARTING DOCKER](http://goldbassist.github.io/devops/2016/03/02/starting-docker/)를 참조하자.

<br>

### Case 1. Official Installation

Docker를 사용하기 이전에 Redmine의 초기 설치법이 어떠한지 살펴보자.
[Redmine Installation Guide](https://www.redmine.org/projects/redmine/wiki/RedmineInstall)에 따라 아래와 같이 하면 Redmine을 사용할 수 있게 된다. 

1. OS는 Unix, Linux, Mac, Windows 모두 사용 가능
1. Ruby가 필요함
1. MySQL/PostgreSQL/MSSQL/SQLite3 중 1개의 Database가 필요함
1. Redmine 버전에 따라 몇가지는 사용법이 다름
1. Installation procedure
	1. Redmine application Download
	1. Create an empty database and accompanying user
	1. Database connection configuration
	1. Dependencies installation
	1. Session store secret generation
	1. Database schema objects creation
	1. Database default data set
	1. File system permissions
	1. Test the installation
	1. Logging into the application

약 15단계를 거쳐, (말이 15 단계지 그 안에 들어가면 각각 3-5 명령어는 있으니...) Redmine을 설치할 수 있었다.

<br>

### Case 2. Using Docker

이제 Docker를 사용해보자. 이전 포스트를 참고하여 필요한 사항만 나열하였다.

{% highlight Bash %}
$ docker search redmine
NAME                       DESCRIPTION                    STARS     OFFICIAL   AUTOMATED
sameersbn/redmine                                          175                  [OK]
redmine                    Redmine is a flexible project   122       [OK]
bitnami/redmine            Bitnami Docker Image f          3                    [OK]
74th/redmine-all-in-one    Redmine includes hosting S      2                    [OK]
{% endhighlight %}

OFFICIAL이 [OK] 되어있는 버전을 사용해보자.

{% highlight Bash %}
$ docker pull redmine
$ docker run redmine
{% endhighlight %}

[http://192.168.99.100:3000/](http://192.168.99.100:3000/) 로 접속해보자.

무엇인가 로그가 막 나오지만 Web으로 접속 할 수는 없다. container 내에서의 port가 막혀있기 때문이다.

몇가지 옵션을 줘서 해결해보자.

{% highlight Bash %}
$ docker run -d --name redmine_container -p 3000:3000 redmine
{% endhighlight %}

이제 다시 [http://192.168.99.100:3000/](http://192.168.99.100:3000/) 로 접속해보자. Redmine 초기 화면을 볼 수 있을 것이다.

이렇게 Docker를 사용하면, 기존의 능력자들이 build 해논 Docker Image를 가지고 손쉽고 빠르게 Application을 사용할 수 있다.

현업에서도 충분히 배포용도로 사용이 가능하다고 많은 이들이 말하고 있다.


위의 Redmine은 Docker Container의 특성상 Data는 휘발성으로 Container가 지워지는 시점에 Data가 지워진다.

이를 해결하기 위해서는 DB Container를 별도로 두고, Data Directory를 별도로 두어 link해서 사용하면 된다.

Constainer가 지워진 상태이더라도 Data는 보존될 것이다.

이는 Docker Hub에 가서 [Redmine Image 페이지](https://hub.docker.com/_/redmine/)를 참고하자.
