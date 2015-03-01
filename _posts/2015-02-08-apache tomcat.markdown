---
layout:	post
title:	"apache, apache tomcat을 연동하는 이유"
date:	2015-02-08 22:00:00
categories:	jekyll update
---
apache tomcat

apache tomcat은 아파치소프퉤어 재단의 Application서버이다. Java Servlet을 실행시키고 JSP가 포함된 코드를 웹페이지로 만들어준다.

apache와 apache tomcat의 차이는 무엇이고 왜 개발을 할때 tomcat을 사용하면서 개발을 하는가에 대해 아무런 의문없이 개발을 하였다.

이번에 apache 와 tomcat의 차이를 알아보면서 apache가 프로젝트에서 하는 역할과 tomcat이 하는 역할은 서로 다르다는것을 알았다.

apache는 Apache재단의 웹 서버이다.
웹서버(Web Server)라고 하면 보통 많은 사람들이 Apache 나 Apache Tomcat이나 둘다 웹서버를 구동하는것인데 어떤것은 Web Server라고 하고 어떤것은 WAS(Web Application Server)라고 하는지 이해가 잘 안갈것이다.

나도 처음에는 이 2개로 나누는 이유를 잘 몰랐다.
Web Server는 정적인 구성요소를 처리하는데 주로 사용되고, WAS(Web Application Server)는 주로 동적인 구성요소를 처리하는데 사용된다고만 알고 있었다.

하지만 내가 조금이나마 아는부분도 틀렸던 것이다.(맞는 것이기도 하지만)

Apache Tomcat은 5.5 버전 이후로 Apache native library를 지원을 하면서 정적인 구성요소에 대한 처리에 대해서도 Apache Http와 같은 효율이 나오게 되었다.(<a href="http://www.tomcatexpert.com/blog/2010/03/24/myth-or-truth-one-should-always-use-apache-httpd-front-apache-tomcat-improve-perform">http://www.tomcatexpert.com/blog/2010/03/24/myth-or-truth-one-should-always-use-apache-httpd-front-apache-tomcat-improve-perform</a>)

그럼에도 불구하고 대부분의 많은 서버들이 tomcat 과 apache를 같이 사용하고 있다. 그 이유는 뭘까?

대부분의 이유는 아마도 다른 기능이나 모듈을 사용해야할 필요가 있거나, tomcat만으로는 필요한 설정을 다 적용하기 어려운부분이 있다고 한다.
tomcat은 java servlet만 지원을 하기 때문에 다른 php나 다른 웹서버 언어를 사용 해야할 경우에 apache가 필요 하다.
이외에도 다른 이유가 더 있을테지만, 검색해보면서 알게 된 내용은 이정도 이다.

앞으로 시간이 될때 웹개발자로서 꼭 알아야할 apache에 대해서 공부해봐야겠다.

참고한 사이트: 
<a href="http://toby.epril.com/?p=1125">http://toby.epril.com/?p=1125</a>

<a href="http://nomore7.tistory.com/5">http://nomore7.tistory.com/5</a>
