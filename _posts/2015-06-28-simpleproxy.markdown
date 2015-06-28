---
layout: post
title:  "simpleproxy"
date:   2015-06-28 23:13:31
categories: jekyll update
---

개발을 진행하면서 서버를 셋팅하다 보면, 종종 부딪히게 되는 부분은 Network ACL이다.
Application ACL의 경우 해당 ACL을 관리하는 관리자에게 문의하여, 급한 사정을 말하면서 허가해 달라는 부탁을 하면 된다.
하지만 Network ACL의 경우는 다르다. 물론 Network ACL을 관리하는 툴을 통해 간단하게 바꿀수 있는 환경도 존재할것이다.
대규모 인프라를 관리하는 곳에서의 Network ACL은 해당 ACL을 열어줘도 되는지에 대해서 고려할것이고 생각보다 시간이 많이 걸린다.

하지만 상황에 따라 매우 급한경우가 있다.
이럴때 유용하게 사용할 수 있는 방법이 simpleproxy프로그램이다.
이 프로그램은 단순히 우회를 시켜주는 프로그램이다.
외국 사이트를 접하다보면, 구글 검색에서는 뜨지만 종종 해당 웹페이지를 로딩을 하지 못할때가 있다.
그럴때면 Chrom Extension으로 있는 zenmate라는 확장프로그램을 사용해서 해당 웹페이지에 접근할 수 있다.

웹 proxy는 이미 많은 사이트 또는 프로그램들이 있지만, 웹으로 열려있는 포트인 80 또는 8080 443에 대한 포트만 접근 할 수 있다.
하지만 우리가 주로 필요한것은 Application에 지정한 특정포트이다.
이 특정 포트에 proxy하기 위해서 리눅스에서 기본적으로 제공해주는 명령어를 통해서 할 수 있다고 한다.(어떻게 하는지에 대해서는 해당 부분에 대한 공부를 해봐야할거같다.)
특정포트에 대해서 우회를 하고 싶은데 명령어를 공부하기에는 너무 촉박하다면 simpleproxy프로그램을 사용하면된다.
해당 프로그램은 GNU라이센스이기 때문에 라이센스에 대해 고려해보고 사용해야 할 것이다.

해당 프로그램을 받기 위해서 먼저 [http://sourceforge.net/projects/simpleproxy/](http://sourceforge.net/projects/simpleproxy/) 해당 링크에서 받아서 binary파일을 생성한다.
대부분 서버셋팅하는데서 사용하기 때문에 터미널에서 해당 프로그램의 소스를 받아서 binary를 만들고 직접 실행해보겠다.
먼저 다운로드 링크가 sourceforge에 있기 때문에 로컬 pc에서 해당 프로그램을 받은뒤에 scp로 전송하여 프로그램을 전송하거나 binary를 생성한뒤 binary를 전송하면 될것이다.

먼저 가장 최근 버전인 3.4버전의 압축을 풀고 설명서대로 binary를 생성해보겠다.
![simple proxy 압축해제](/img/simpleproxy/simpleproxy압축.png)
README를 읽어보면 간단한 소개와 설치하는 방법이 매우 간단하게 나와있다.
![simple proxy README](/img/simpleproxy/simpleproxyREADME.png)
![simple proxy 설치1](/img/simpleproxy/simpleproxy설치1.png)
![simple proxy 설치2](/img/simpleproxy/simpleproxy설치2.png)
설치를 한뒤에 simpleproxy를 실행해보겠다.
![simple proxy 실행](/img/simpleproxy/simpleproxy실행.png)
위와 같이 simpleproxy 명령어에 -L은 로컬 포트를 지정하고, -R은 대상 서버와 포트를 지정하면 로컬로 들어오는 포트에 대한 요청을 대상 서버 포트로 우회시켜준다.
![simple proxy 적용예제1](/img/simpleproxy/simpleproxy적용예제1.png)
![simple proxy 적용예제2](/img/simpleproxy/simpleproxy적용예제2.png)

이와 같이 급하게 Network ACL이 필요한경우, 이미 ACL 이 등록되어있는 서버로 simpleproxy를 통해서 우회하여 급한불을 끌 수 있다.
다만 proxy이기 때문에 부가적으로 생기는 부작용등이 생길 수 있다.(속도가 느려질 수 있다.)
