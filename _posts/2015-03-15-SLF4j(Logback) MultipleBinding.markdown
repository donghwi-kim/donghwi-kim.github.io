---
layout: post
title:  "SLF4j(Logback) MultipleBinding"
date:   2015-03-15 23:31:37
categories: jekyll update
---
이번주도 서버셋팅을 하면서 생겼었던 문제에 대해서 써보겠다.

이전 서버에 있던 Apache와 Tomcat을 모두 복사하여 사용하였다. 같은 버전을 사용하기 때문에 복사를 하였는데, 이상하게도 Log가 찍히는 경로가 달랐다.

이전에 Log4j와 Logback등을 프로젝트에서 셋팅을 할때 제대로 알지 못하고 제대로 작동하는 셋팅을 보고, 따라하기에 급급하였다.

Log4j, LogBack은 SLF4j를 구현한 구현체이다. 그래서 항상 Log를 남기다보면 slf4j를 볼 수 있다.

이 slf4j는 facade 디자인 패턴을 사용한 로깅 프레임워크이다.

facade패턴이란 여러개의 클래스가 하나의 역할을 수행할때, 대표적인 인터페이스만을 다루는 클래스를 두어 원하는 기능을 처리할수 있게 도와주는 패턴이다.

따라서 SLF4j에서 사용되는 인터페이스만 알고 있다면, Log4j나 Logback이든 SLF4j를 구현하고 있는 구현체라면 같은 문법을 사용하여 로깅할 수 있다.

보통 웹 어플리케이션을 만들때 maven를 통해  dependency를 추가한다.

이번에 겪었던 일은 slf4j와 logback 두개 모두 dependecy를 추가하면서 slf4j로 추가했던 dependency에 log4j를 default로 사용하면서 실질적으로 원했던 logback이 아닌 log4j가 logger로 선택 되면서 발생했었다.

문제의 원인을 파악하기까지 너무나 많은 삽질을 했었다.

가장 처음 의심했던것은 logback의 설정에 사용했던 log 출력포맷과 너무 같았기 때문에, tomcat을 먼저 의심했다. tomcat의 logging.properties를 변경하면서 달라진 부분은 없었다.
logging.properties외에 server.xml을 보면서 tomcat이 문제가 아니라는것을 느꼈다.

내가 설정했던 Logback이 아닌 다른 Log가 작성되는지 도무지 이해가 안갔는데 Log에 남겨진 Log에서 단서가 있지 않을까 싶어서 로그를 자세히 봤다.

Log에 남겨진 단서는 처음 Tomcat구동시에 application-context나 servlet-context에 명시했던 logger가 생성되면서 나오는 Log였다.

나 같은 경우, 따로 에러가 나는것도 아니고 단순히 아래와 비슷한 로그가 찍혔다.

{% highlight console %}
SLF4J: Class path contains multiple SLF4J bindings. 
SLF4J: Found binding in [jar:file:/Users/me/.m2/repository/org/slf4j/slf4j-log4j12/1.5.8/slf4j-log4j12-1.5.8.jar!/org/slf4j/impl/StaticLoggerBinder.class] 
SLF4J: Found binding in [jar:file:/Users/me/.m2/repository/ch/qos/logback/logback-classic/1.0.6/logback-classic-1.0.6.jar!/org/slf4j/impl/StaticLoggerBinder.class] 
SLF4J: See slf4j.org/… for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
{% highlight console	 %}

로그에 정확한 설명은 잘 모르겠지만, 대충 로그를 읽어보면 이해가 된다.

클래스 경로에 여러개의 SLF4J 구현체가 포함되어있고, 발견된 구현체는 slf4j-log4j, logback 2가지이다. 그리고 실질적으로 slf4j에서 바인딩한 구현체는 log4j이다.

따라서 내가 사용하고 싶어서 설정했던 Logback에 있던 설정은 모두 설정은 됬지만, 실제로 사용하는건 log4j이기 때문에 내가 원하는대로 Logging이 되지 않고 있었던것이다.

해당 로그를 보고 compile한 결과물의 폴더를 가봤더니, 실제로 log4j, logback jar파일 2개가 있었다.

해당 2개가 들어간 이유가 무엇일까 생각해보고 pom.xml에서 dependency를 잘못추가한것을 알 수 있었다. 이 multiple binding에 대해서 검색을 해봤더니, 1개의 dependency를 사용하던지,

아니면 pom.xml에 exclude에 필요없는 Logger의 jar파일을 명시하라는것이었다.

먼저, 원인이 정확한것인지 확인하기 위해 jar파일을 먼저 삭제하여 확인하였고, 정확하게 내가 원했던 설정이 반영이 된 로그파일들이 생성되었다.

pom.xml파일을 수정하는것은 프로젝트를 진행하는 동료들과 이 문제를 공유하고 확인해본후 진행할것이다.
