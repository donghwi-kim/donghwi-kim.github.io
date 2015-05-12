---
layout: post
title:  "jquery attr prop"
date:   2015-05-09 19:13:31
categories: jekyll update
---
jquery를 사용하다보면 checkbox를 체크, 체크해제할때 attr또는 val으로 셋팅이 안될때 당황한일이있었다.<br>
이것과 관련해서 구글링을 하다보면 attr대신에 prop을 쓰면된다는것을 알 수 있다.<br>
jQuery 1.6버전 이전에는 attr이 prop으로 제어해야할 property value들을 제어할 수 있었는데 이로 인해 정상동작하지 않는 일이 있었다고한다.(<a href="http://api.jquery.com/prop/">http://api.jquery.com/prop/</a> Note아래부분을 읽어보면 나온다.)<br><br>
여기서 attribute와 property의 차이는 무엇일까?<br>
attribute는 HTML 요소의 정보를 전달하는 이름=값 쌍이다.<br>
property는 HTML DOM 트리에서 표현되는 값들이다.<br>
즉, attribute는 HTML 텍스트 문서에 있고, property는 DOM트리에 있다.(원문 : <a href="http://jquery-howto.blogspot.kr/2011/06/html-difference-between-attribute-and.html">http://jquery-howto.blogspot.kr/2011/06/html-difference-between-attribute-and.html</a>).<br>
attr, prop의 차이에 대한 정확한 문서도 없었고, 대부분이 영어 문서이다보니(제대로 의미파악하지도 못한 상태입니다.) 명확하게 어떤것이 attribute이고, property인지에 대해서는 말할 수가 없다.
attr, prop의 차이에 대해 구글링을 해본 결과 대부분의 글들이 HTML 요소에 기록된 속성이라면 attr을, 그렇지 않은 경우에는 prop 을 써야하는지 확인해보라고 써있었다.<br>
좀 더 정확한 정보는 jquery사이트의 <a href="http://api.jquery.com/prop/">prop</a>에 대한 설명을 참고하면 된다.