---
layout: post
title:  "Apache Tomcat 연동"
date:   2015-05-31 23:13:31
categories: jekyll update
---

종종 한개의 서버에 Tomcat을 여러개 띄우는 경우가 있다.

이번에 서버작업을 하면서 실제로 특정 Domain에 따라 실행되는 Tomcat이 다른 서버를 확인하였다.

이를 실제로 처리해주는것은 Apache에 있는 mod_jk 모듈이다.

mod_jk모듈을 구글에서 검색해보면 가장 먼저 나오는 것이 <http://tomcat.apache.org/connectors-doc/> 이다.

들어가서 보면 가장 잘 보이는 문구가 The Apache Tomcat Connector다.

-----

먼저 workers.properties 파일부터 보면

~~~~~~
# Define 1 real worker using ajp13
worker.list=worker1
# Set properties for worker1 (ajp13)
worker.worker1.type=ajp13
worker.worker1.host=localhost
worker.worker1.port=8009
~~~~~~

위는 apache tomcat 사이트에 나와 있는 예제이다. 위의 예제를 보면 먼저 worker.list에 request들을 처리할 tomcat의 리스트를 나열한다. 이름은 마음대로 정해도 된다.

정한 worker들을 4번째줄부터 처럼 worker를 정의 하면된다. 4번째 줄에는 worker의 type(ajp13), worker의 host명 또는 ip, worker의 port를 설정하면된다.

위의 예제 외에 더 많은 옵션이 있으며, worker.list처럼 request를 처리하는 tomcat을 여러개 정의할 수 있다.

-------------

앞에서 말했다시피 mod_jk는 모듈이다. 모듈이기 때문에 apache에 설치가 되어있을 수도 있고, 설치가 되어있지 않을 수도 있다. 따라서 모듈이 설치된 폴더에서 mod_jk의 binary 파일인 mod_jk.so가 있는지 확인해야 한다.

만약에 해당 모듈이 없을시에는 apache 사이트가서 직접 compile하여 만들어도 되고, binary파일을 구해서 복사해 넣어도 된다.

이제는 apache설정 파일을 보겠다.

~~~~~~
# Load mod_jk module
# Update this path to match your modules location
LoadModule    jk_module  libexec/mod_jk.so
# Declare the module for <IfModule directive> (remove this line on Apache 2.x)
AddModule     mod_jk.c
# Where to find workers.properties
# Update this path to match your conf directory location (put workers.properties next to httpd.conf)
JkWorkersFile /etc/httpd/conf/workers.properties
# Where to put jk shared memory
# Update this path to match your local state directory or logs directory
JkShmFile     /var/log/httpd/mod_jk.shm
# Where to put jk logs
# Update this path to match your logs directory location (put mod_jk.log next to access_log)
JkLogFile     /var/log/httpd/mod_jk.log
# Set the jk log level [debug/error/info]
JkLogLevel    info
# Select the timestamp log format
JkLogStampFormat "[%a %b %d %H:%M:%S %Y] "
# Send everything for context /examples to worker named worker1 (ajp13)
JkMount  /examples/* worker1
~~~~~~

주석을 보면 금방 알겠지만 먼저 LoadModule로 시작하는것은 mod_jk모듈을 읽어오는 설정이다. JkWorkersFile 에 이전에 설정해놓은 workers.properties파일의 경로를 설정하면, workers.properties를 읽어온다.
JkMount설정이 써있는 줄을 보면 해당 URI로 들어오는 Request는 workers.properties의 worker1 워커가 처리한다는 뜻이다.

---------

이렇게 설정을 해주면 특정 URI나 Domain, Port에 대해서 처리하는 tomcat(worker)를 지정하여 처리할 수 있다.
