---
layout: post
title:  "Starting Docker"
date:   2016-03-01 17:35:01
comments: false
categories: DevOps
tags: devops, docker
---

Docker를 언급하는 곳이 몇 있길래 docker가 뭔가 하고 둘러봤는데 이게 맘에 드는 기술이다. 물론 내 현업에서는 시기상조(어쩌면 평생 이런 분야에 접근하지 않을지도.)이지만.. 이를 통해 S/W 배포가 용이해질 것 같아 조금 봐 보기로 했다.

### About Docker

Docker를 이해하기 위해서 아래의 Site를 탐구한다.

아래의 Site를 통해 OSX용으로 인스톨 한다.

* [Docker Official Site](https://www.docker.com)
* ['가장 빨리 만나는 Docker'의 저자 Blog](http://pyrasis.com)

<br>

### Docker의 시작

Docker의 Base는 Linux Kernel이다. 때문에,

* Linux Kernel을 사용하는 OS는 그 자체가 DOCKER_HOST로 동작한다.
    * Redhat, Debian ...
* Linux Kernel을 사용하지 않는 OS는 Virtual Machine을 이용하여 Linux Kernel을 동작시킨다. 그 위에서 모든 것이 작동한다.
    * OSX, Windows ...
    * [Docker Official Site](https://www.docker.com)에서 알려주듯이 OSX에 Docker를 인스톨하면 모든 것이 준비되어있다.

OSX에서 Docker Host에 접속하기 위해서는 아래와 같이 하면 된다.

* `Launchpad` 에서 `Docker Quickstart Terminal` 을 실행

열린 command 창에서 실제 실행된 명령어를 볼 수 있다.

<br>

### Starting Docker

* 사용할 Base Image 찾기
{% highlight Bash %}
$ docker search ubuntu
{% endhighlight %}

* Image 받기
{% highlight Bash %}
$ docker pull ubuntu:latest
{% endhighlight %}

* 컨테이너 생성하기
{% highlight Bash %}
$ docker run -i -t --name hello ubuntu /bin/bash
{% endhighlight %}

* 컨테이너 정지하기
{% highlight Bash %}
$ docker stop hello
{% endhighlight %}

* 정지된 컨테이너 시작하기
{% highlight Bash %}
$ docker start hello
{% endhighlight %}

* 컨테이너 재시작하기
{% highlight Bash %}
$ docker restart hello
{% endhighlight %}

