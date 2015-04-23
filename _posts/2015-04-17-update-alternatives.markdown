---
layout: post
title:  "update-alternatives : Symbolic link관리 명령어"
date:   2015-04-17 23:00:00
categories: jekyll update
---
서버를 셋팅하다보면, 서버를 재사용하거나, 여러개의 프로젝트가 한대의 서버에 올라가는 경우가 있다.(CI서버)
이럴때 어떤 프로젝트에서는 jdk1.6을 사용하고 1.7을 사용하고 여러 버전을 사용하는 경우가 있다.

간혹 JAVA_HOME으로 JAVA의 경로를 설정해줬음에도 불구하고, 실제로 배포 또는 실행하는 계정의 권한이나, 설정때문에 원하지 않은 버전으로 계속 설정될 경우가 있다.
Linux에는 이런 프로그램의 버전관리를 위해(정확하게 말하면 symbolic link를 관리한다) updata-alternatives라는 명령어가 있다.

서버를 잘 다루지 못하는 필자의 경우, JAVA를 yum명령어를 통해 여러개를 설치했었는데,
각각 버전의 경로가 어디에 있는지, JAVA_HOME으로 일일이 설정하기가 어려웠었다. 또한 실제 서비스가 될 서버이기 때문에 기존 jdk를 지우기에 무엇인가가 두려웠다.

jdk설치과정을 제대로 안봤었는지는 몰라도 CentOS에서 jdk가 기본적으로 어디에 설치되는지 구글에서 검색하고 겨우 JAVA_HOME을 설정 할 수 있었다.

하지만, update-alternatives 명령어를 사용하면, symbolic link를 관리하기 쉽다.
먼저 update-alternatives의 도움말을 보기위해 update-alternatives 명령어를 실행하거나 --help옵션으로 확인한다.

![](/img/update-alternatives/update-alternatives--help.png)

물론, update-alternatives 해당 명령어의 버전 또는 환경에 따라 다를 수 있다.
보통 기본적으로 config, display, set, install, remove등의 옵션이 있을것이다.

가장 많이 사용하고, symbolic link를 관리하는데 사용하는 config 옵션의 사용법은 --config <name>이다.
name은 symbolic link를 관리할 이름이다.
update-alternatives --config java 명령어를 치면 해당하는 java의 버전을 보여주고, 설정할 java버전을 선택하면 된다.
이렇게 update-alternatives --config 로 여러개의 symbolic link를 관리할 수 있다.

![](/img/update-alternatives/update-alternatives--config.png)

다른 linux 명령어에 비해 옵션이 적고 옵션이름이 명확하다.
--install link name path priority
install 옵션은 관리할 symbolic link 를 생성하거나, 추가한다.
link는 symbolic link들을 대표할 link를 말한다. name 은 update-alternatives가 관리할 symbolic link들으 대표할 이름이다.
path는 실제 해당 symbolic link의 경로를 말한다. priority는 우선순위(높을수록 우선순위가 높다)를 뜻한다.

--remove name path
name에 해당하는 대표 symbolic link에서 해당하는 path를 제거한다. 모두가 제거되면 대표되는 symbolic link가 사라진다.

--set name path
name에 해당하는 대표 symbolic link를 symbolic link리스트중 하나로 변경한다.(우선순위에 상관 없이)

--auto name
name에 해당하는 대표 symbolic link를 우선순위의 결과에 의해 자동으로 설정한다.

--display name
name에 해당하는 symbolic link리스트를 보여준다.

실제로 해당 명령어를 자주 사용하게 될지는 모르겠지만,
해당 명령어를 알고 있다면, 필자 처럼 java의 버전을 알기위해 어디에 설치되어있을지 모를 java를 인터넷에서 물어볼 일이 없을것이다.