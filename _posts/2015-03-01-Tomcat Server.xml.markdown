---
layout:	post
title:	"Tomcat server.xml 간단한 설명"
date:	2015-03-08 23:00:00
categories:	jekyll update
---
하루종일 고생해서 설정을 못한 tomcat의 설정파일인 server.xml에 대해 간단하게 분석해보겠다.

개발을 할때 항상 IDE에 의존 하다보니 정작 서버를 세팅할때 어떻게 세팅하는지 잘 몰라서 기본적인 부분도 세팅방법을 몰랐다.

이번에 고생을 하면서 기본적이지만 알게된 부분에 대해 적겠다.

server.xml의 위치는 $CATALINA_HOME/conf/server.xml이며 $CATALINA_HOME은 환경변수에 정의된 tomcat의 절대 경로다.

Server는 한개 이상의 Service Element를 가지고 있으며, 지정된 포트로 shutdown 명령을 listen한다.
Service는 하나의 Engine을 공유하는 한개 이상의 Connector를 가지고 있다.
Engine은 Request를 처리하는 host로 넘기는 entry point이다.

Host가 서버를 설정하면서 가장 중요했던 부분인거 같다. 프로젝트의 배포된 위치와 설정을 하는데 가장 애먹고 있는 부분이다.
속성중에는 appBase라는것이 있는데 이름에서 알 수 있듯이 application의 기본 디렉토리이다. 기본적으로 webapps로 설정되어 있다.
deployOnStartup flag는 Tomcat 시동시 자동으로 배포할것인지에 대한 flag이다.
unpackWARs는 WAR(Web Application aRchive)파일이 압축된 하나의 파일이 아닌 디렉토리 구조를 원할경우, true로 셋팅하면되고, WAR파일의 압축된 하나의 파일로 Web Application을 실행할경우 false로 셋팅하면 된다.
autoDeploy flag는 주기적으로 새로운 web application이나 수정된 web application이 있는지 주기적으로 확인할것인지에 대한 flag이다. 보통 IDE를 쓰면 해당 flag가 true로 되어있어, application을 수정하게되면 자동으로 web application이 재배포된다.

Host에 내부 Element로 Context라는 부분을 볼 수 있는데 Context는 각각의 다른 web application에 대해 연관된 가상 host이다. eclipse에서 보면 보통 project의 이름에 따라 해당 Context를 설정해준다.
Context가 정의 되어 있으면 appBase가 정의되어있지 않거나, deployOnStartup과 autoDeploy가 false이어야만 한다.
Context의 속성으로는 path가 있는데 path로 명시된 Request들을 매칭되는 web application으로 request들을 처리하며, 해당 path는 반드시 unique해야한다. docBase는 보통 해당 application의 위치나 WAR파일의 위치를 말한다.

개발 PC에서 개발을 계속하면서 IDE에서 자동으로 해당 설정파일들을 셋팅해주다 보니, 실질적으로 작동하는 서버에 셋팅을 못해 하루종일 고생하고 있다. 웹개발자로서 웹을 구동시키는 Apache, Tomcat, Nginx의 기본적인 설정은 알고 있어야 할거 같다.

계속해서 서버를 셋팅해보면서, 알게되는 부분을 계속해서 글을 올리도록 하겠다.