---
layout: post
title:  "mod_access_compat"
date:   2015-03-22 23:31:37
categories: jekyll update
---
종종 보안문제로 인해 몇몇 페이지들에 대해 제한을 둘 필요가 있다.

예를들어 Admin페이지라던가, 테스트중인 페이지는 특정 IP에 대해서만 허가를 해야하고, 나머지 IP에 대해서는 모두 제한을 두어야한다.

이를 Network ACL로 처리할수도 있지만, proxy를 통해서도 접근할 수 있는 방법이 있기 때문에 Network ACL로만 제한을 두어서는 안된다.

또 더군다나, 테스트페이지 같은 경우는 같은 포트인 80포트나 8080포트를 이용하기 때문에 모든 사용자에게 open이 되어있어야 할 포트이기 때문에

Network ACL로 차단하기에는 부족하다.

Apache에는 mod_access_compat이라는 모듈이 있다.

해당 모듈의 이름을 보아도, 접근과 관련된 모듈이라는것을 알 수 있다.

해당 모듈은 현재 2.3 버전 또는 이하의 2.x버전과 호환이 가능하며, 2.3이상의 버전에서는 deprecated되어있으며 mod_authz_host해당 모듈을 권장하고 있다.

mod_access_compat 모듈은 Files, Directory, Location 또는 .haccess파일에 특정 IP나 호스트네임등으로 접근을 제한하거나 허가할 수 있다.

지시자에는 총 4가지가 있다. Allow, Deny, Order, Satisfy

해당 지시자를 쓰기에 앞서, 구체적으로 어떤 Method에 따라 제한을 둘것인지에 대한 설정도 가능하다. 해당 설정은 Limit을 사용하여 Method를 지정할 수 있다.

{% highlight xml lineos %}
<Limit POST PUT DELETE>
  Require valid-user
</Limit>
{% endhighlight %}

{% highlight xml lineos %}
<Limit POST PUT DELETE>
  Order deny, allow
  Deny from all
  Allow from 127.0.0.1
</Limit>
{% endhighlight %}

Allow, Deny지시자는 위와 같이 Allow from xxx.xxx.xxx.xxx(hostname도 가능 또는 DNS) 또는 Deny from xxx.xxx.xxx.xxx 사용하면 된다.

IP로 사용가능한 파라미터로는 특정 IP, IP대역(network mask, CIDR, 부분IP)를 사용할 수 있다.

Order지시자는 지시자의 파라미터 순서에 따라 평가의 순서를 정한다.

예를 들어 Order deny, allow로 지시자를 사용하였다면, deny지시자가 먼저 적용되고, allow 지시자가 적용되는것이다.

즉, 위의 Limit에서 Order deny, allow로 사용하였다면, 먼저 Deny가 평가되기 때문에 모든 ip에 대해 거부한다.

하지만 나중에 평가되는것이 allow이기 때문에 Allow from 127.0.0.1이므로 127.0.0.1 ip에 대해서는 허가를 하게된다. 즉 나중의 것이 앞에 적용된것을 덮어씌우기 때문에 어떤것이 먼저 평가될 것인지 중요한 부분이다.

위에 써있는 Order지시자의 파라미터 순서를 반대로 했다면, 자기자신인 127.0.0.1도 접근을 못할것이다. 하지만 deny, allow순서이기때문에 모든 ip에 거부를 하고 127.0.0.1에 대해서 허가를 한다는것이다.

예를들어 sample.jsp라는 페이지에 대해 나자신과 구글에 대해 허가를 하고 싶다면 다음과 같이 Apache의 httpd conf를 작성하면된다.

{% highlight xml lineos %}
<Location /sample.jsp>
	Order Deny, Allow
	Allow google.com
	Deny from all
	Allow from 127.0.0.1
</Location>
{% endhighlight %}

위에서 순서가 google.com을 허가하고 모든 ip에대해 거부하고 loopback에 대해서 허가를 한다는 순서로 되어있지만, Order의 파라미터가 deny, allow이므로 적용되는 순서는 Deny from all, Allow from google.com, Allow from 127.0.0.1이 되므로 해당 페이지로 허가되는 서버는 자기자신과 구글 도메인으로 등록된 서버가 될것이다.

참고한 사이트
mod_access_compat Doc : <a href="http://httpd.apache.org/docs/2.4/ko/mod/mod_access_compat.html">http://httpd.apache.org/docs/2.4/ko/mod/mod_access_compat.html</a>