---
layout:	post
title:	"iTerm2 Broadcast Input"
date:	2015-06-12 22:00:00
categories:	jekyll update
---
서버를 설치 및 셋팅을 하다보면 종종 여러개의 동일한 서버를 동일한 환경으로 설정을 만들어야 하거나 서버를 여러대 증설하여 이전 상태와 동일하게 설치를 해야하는 경우가 있다.

이럴때 항상 느끼지만 키보드를 미러링해서 모든 입력을 각 터미널마다 같은 입력을 해주는 프로그램이 있었으면 하는 바램이 생긴다.

실제로 iTerm2에는 Broadcast Input이라는 기능이 있다. 기능의 이름에서 알 수 있듯이 Input을 Broadcasting할 수 있다는 뜻이다.

보이는것과 같이 Shell menu => Broadcast Input이라는 기능이 있다. 보이는것과 같이 해당 기능에는 4가지의 옵션이 있다.

![](/img/iTerm2/BroadcastOption.png)

* Send Input to Current Session Only
* Broadcast Input to All Panes in All Tabs
* Broadcast Input to All Panes in Current Tab
* Toggle Broadcast Input to Current Session
* Show Backgroud Pattern Indicator

아무런 설정없이 기본 Terminal을 열게 되면 Send Input to Current Session Only 이 체크되어 있을것이다.(체크가 안되어 있는 경우도 있는데 왜그런지 잘 모르겠다.)

Send Input to Current Session Only옵션은 현재 열려 있는 터미널에 대해서만 Keyboard의 Input을 적용한다는 뜻이다. 즉, 일반적으로 터미널을 열었을때 하나의 터미널에 대해서만 사용하는것과 같은 상태이다.

먼저 다음 옵션들을 설명하기전에 Show Background Pattern Indicator에 대해서 설명하겠다. 해당 옵션은 현재 Keyboard Broadcast 옵션이 적용되는 터미널에 대해서 배경화면에 인지할 수 있는 패턴을 표시한다는 것이다. 해당 옵션을 켜놓고 다음 옵션들을 적용해보면 알 수 있을 것이다.

Broadcast Input to All Panes in All Tabs 옵션은 현재 Pane(같은 Window)에 대해서 모든 Tab의 Terminal에 대하여 Broadcast Input을 한다는 것이다.

![](/img/iTerm2/BroadCastAllTab.png)

즉 아래와 같이 1개의 Window에 대해서 여러탭에 여러개의 Terminal이 있어도, Window에 있는 모든 Terminal에 대해서 동일한 Keyboard 입력을 받게 된다는것이다.

![](/img/iTerm2/BroadCastCurrentTab.png)

이와 다르게 Broadcast Input to All Panes in Current Tab은 이름에서 보이듯이 현재 탭에 대해서만 Broadcast Input을 한다는것이다.

![](/img/iTerm2/BroadCastCurrentTab2.png)

마지막으로 Toggle Broadcast Input to Current Session은 토글시킨 Terminal에 대해서만 Broadcast Input을 한다는 것이다.

![](/img/iTerm2/BroadCastToggled.png)

위의 옵션만 잘 사용한다면, 서버셋팅하는데 시간을 N배로 줄일수 있을것이다.