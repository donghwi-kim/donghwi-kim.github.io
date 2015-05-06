---
layout: post
title:  "jenkins plugin"
date:   2015-05-02 19:13:31
categories: jekyll update
---
Jenkins서버를 이전하게 될일이 생겼었다.<br>
기존에 NCloud에 Jenkins서버를 생성해 두었었는데, ToastCloud로 이전해야 했다.<br>
Jenkins서버를 이전하면서 단순히 war파일과 Jenkins관련된 파일을 모두 복사하면 해결될 수도 있는 일이다.<br>
기존 Jenkins의 버전이 현재 Stable버전인 <a href="http://mirrors.jenkins-ci.org/war-stable/latest/jenkins.war">1.596.2.</a> 버전만 webapp에서 교체해도 될일이었지만, Jenkins서버를 셋팅해보고 싶기도하고, 여러가지 환경이 달라 직접 설정을 해보았다.
<br>
무엇보다도 가장 힘들었던것은 ACL이 가장 큰 문제였다. 하지만 시간만 있으면 해결될 문제들이었다.<br>
Jenkins를 제대로 모르는 상태에서 서버이전을 하다보니, Jenkins UI에서만 설정들을 복사하다보니 몇몇 Plugin들을 설치를 할 수 없었다.<br>
사내 Plugin도 있고, 가동중인 Jenkins Plugin목록에도 뜨지 않는 Plugin들이 있었다.<br>
분명히 Plugin을 검색하면 존재는 하는데 Jenkins Plugin목록에서만 뜨지 않는 Plugin들이 존재하였다.<br>
그래서 Jenkins directory자체를 복사할까 생각을 했더니, 버전마다 지원이 가능한 Plugin도 있고 Plugin의 최신버전이 있는데, 구버전을 신규 Jenkins서버에 설치하기에는 Jenkins서버를 이전할 이유가 없기 때문에 Plugin을 설치 하는 방법을 찾아야했다.<br>
그래서 먼저 Jenkins Plugin관련하여 구글에서 검색하다가 대부분 Jenkins Wiki로 연결되는것을 확인 할 수 있었다.<br>
분명히 Jenkins Wiki에 나와있듯이 존재하는 Plugin인데 설치 목록에서는 뜨지 않아서 당황했었다.<br>
그래서 다시 <a href="https://jenkins-ci.org">Jenkins 홈페이지</a>로와서 천천히 확인해보기로 하였다.<br>
Jenkins 홈페이지로 가서 WIKI페이지로 가면 왼쪽 탭에 Plugins라는 항목을 볼 수 있을것이다.<br>
해당 항목을 천천히 읽다보면 By hand부분을 보면 Download Site를 보면 Plugin목록을 볼 수 있을것이다.<br>
해당 <a href="http://updates.jenkins-ci.org/download/plugins/">링크</a>를 가면 내가 원하던 Plugin을 찾을 수 있을 것이다. 만약 해당 목록에도 없다면, 사내 Plugin이거나 공개되지 않은 Plugin 일 가능성이 높다.<br>
By hand부분을 자세히 읽어보면 *.hpi 또는 *.jpi 를 JENKINS_HOME/plugins 폴더(UI에서 Plugin을 설치하면 해당 폴더에 Plugin파일이 설치된다)에 넣으면 된다고 써있다.<br>
해당 폴더에 설치하려는 Plugin을 다운로드 링크에서 찾아서 복사하여 Jenkins server를 reload시키면 된다.<br>
scp 또는 파일 전송 명령어를 일일이 치기가 귀찮다면, Jenkins UI에서 Plugin설치하는 방법 중 수동으로 하는 방법이 있을것이다. 해당 파일들을 모두 업로드 시키고 마찬가지로 Jenkins를 reload를 시키면 정상적으로 plugin 들이 설치되어 있는것을 확인할 수 있다.
