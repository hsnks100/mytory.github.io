---
layout: post
title: "jQuery 틀고정 플러그인 jquery.freezeheader.js"
categories:
- html-css-js
tag:
- jQuery
---

요약: Brent Muir의 [Freeze Header 플러그인](http://brentmuir.com/projects/freezeheader/)이 좋다. 그런데 이건 jQuery 1.5.1을 사용하고, 최신 jQuery에서 작동하지 않는다. [최신 jQuery에서 작동하게 수정한 내 것](https://github.com/mytory/jquery.freezeheader.js)을 사용하면 된다.

--------------

데이터를 많이 다루는 사이트를 만들 때 행을 고정해 달라는 요청을 받게 된다. 엑셀에는 그런 기능이 있으니, 사용자들이 그런 요구를 하는 것은 당연하다. CSS로 구현할 수 있다면 좋겠지만 그러면 아래와 같은 방식으로밖에 구현되지 않는다. 별로다.

<iframe width="420" height="315" src="https://www.youtube.com/embed/LEacNnvC5wQ" frameborder="0" allowfullscreen></iframe>

이번에 입맛에 맞는 플러그인을 찾아 보려고 이거저거 뒤졌다. 일단 엑셀의 '틀고정' 기능을, 영어로는 'Freeze Header'라고 부르더라. 그 키워드로 찾았다. 찾다 보니 'Fixed Header'라는 용어도 사용하던데, 여튼 나는 'Freeze Header'로 찾았다. 

그래서 찾은 게 Brent Muir의 [Freeze Header 플러그인](http://brentmuir.com/projects/freezeheader/)이다. 아래처럼 멋지게 작동한다.

<iframe width="420" height="315" src="https://www.youtube.com/embed/NjtUnrLCu4w" frameborder="0" allowfullscreen></iframe>

흔히 HTML에서 틀고정을 하면 스크롤 박스가 생기는 단점이 있었는데, 이 놈은 스크롤박스 없이 자연스럽게 틀고정을 해 준다.

## jQuery 1.11에서 작동하지 않아 수정했다

그런데 이 플러그인은 jQuery 1.5.1을 기준으로 작성한 플러그인이다. 내가 적용하려는 사이트는 jQuery 1.11을 사용하고 있었는데, 이게 작동하지 않았다. `.attr('rows')` 라는 코드 때문이었다. 

원래 `$('#myTable').attr('rows')` 하는 식으로 쓰면 해당 테이블의 모든 `tr` jQuery 객체를 돌려 주는 놈인데, 이게 jQuery 1.5.1에서는 작동하고, 1.11에서는 작동하지 않는 게 문제였다. 그래서 소스코드를 고쳤다. 아래처럼.

<pre>
$('#myTable').find('tr');
</pre>

그리고 내가 고친 파일을 내 github에 올려 놨다. [mytory/jquery.freezeheader.js](https://github.com/mytory/jquery.freezeheader.js)다. 원저작자에겐 내가 고친 이유와 diff 코드, 그리고 새로 만든 github URL을 메일로 보내 줬다. 뭐, 어쨌든 MIT 라이센스니 github에 올린다고 문제가 되진 않는다.

이상.
