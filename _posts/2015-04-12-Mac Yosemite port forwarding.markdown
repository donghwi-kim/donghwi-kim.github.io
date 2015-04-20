---
layout: post
title:  "Mac Yosemite port forwarding"
date:   2015-04-12 23:10:00
categories: jekyll update
---
Mac에서 개발하다보면 Tomcat port가 80으로 바뀌지 않아 개발하는데 귀찮음을 느낄때가 많다.

구글에서 Mac에서 8080포트를 80포트로 포트포워딩 하는방법으로 대부분 ipfw 명령어를 사용하는것이 많다.
<a href="https://discussions.apple.com/thread/6645172">하지만 Yosemite에서는 ipfw대신 pf를 사용한다고 한다.(Yosemite firewall: IPFW is gone, Moving to PF)</a>

아래에 쓴내용은 이미 내 MacBook에 적용하여 사용하고 있으며, 해당 출처는 내 선임분께서 알려주신 방법이다.

물론, 구글에서 검색하면 나오는 방법 중 하나이다.

1. sudo vi /etc/pf.anchors/com.test
com.test파일을 /etc/pf.anchors/ 경로에 생성한다. 물론 /etc인 시스템폴더 중 하나를 접근하기 때문에 super user 권한이 필요하기 때문에 sudo를 사용한다.

2. 생성한 파일에 아래 내용으 넣는다.(8080포트를 80 포트로)
rdr pass on lo0 inet proto tcp from any to any port { 80 8080 } -> 127.0.0.1 port 8080
rdr은 redirect의 약자인거 같은데 pf에 사용되는 명령어중 하나이다.
이에 대한 자세한 내용은 <a href="http://www.openbsd.org/faq/pf/rdr.html">http://www.openbsd.org/faq/pf/rdr.html</a>를 참고하면 된다.
간단하게 요약을 하자면, lo0(loopback) 디바이스의 80 또는 8080 포트를 127.0.0.1 8080 으로 리다이렉트 시킨다는 말이다.

3. /etc/pf.conf 파일을 vi에디터로 연다. 물론 해당 파일도 시스템과 관련된 파일이기 때문에 super user의 권한이 필요하다.
sudo vi /etc/pf.conf

4. 해당파일의 rdr-anchor "com.apple/*": 다음줄에 rdr-anchor "com.test"를 추가한뒤, load anchor "com.apple" from "/etc/pf.anchors/com.apple": 다음줄에 load anchor "com.test" from "/etc/pf.anchors/com.test" 를 추가한다.

5. 이제 아래 명령어를 실행한다.
sudo pfctl -f /etc/pfconf
sudo pfctl -e

위의 내용을 따라하다보면 anchor라는것이 자주 보이는데 anchor는 iptables의 ruleset처럼 사용자 또는 시스템에서 정의한 rule들의 집합(table)을 말한다.

맨 아래의 pfctl 명령어의 f옵션은 파라미터로 오는 파일 옵션으로 ruleset을 변경하는 옵션이다.
e옵션은 설정한 필터를 적용한다는 옵션이다.

pfctl 명령어는 <a href="http://www.openbsd.org/cgi-bin/man.cgi/OpenBSD-5.6/man8/pfctl.8?query=pfctl&sec=8&manpath=OpenBSD-5%2e6">다음 사이트</a> 를 참고하면 된다.

anchor에 대한 내용은 <a href="http://www.openbsd.org/faq/pf/anchors.html">다음 사이트</a>를 참고하면 된다. 
