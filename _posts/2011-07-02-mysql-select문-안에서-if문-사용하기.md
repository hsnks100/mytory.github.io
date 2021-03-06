---
title: '[MySql] Select문 안에서 IF문 사용하기'
author: 안형우
layout: post
permalink: /archives/1416
aktt_notify_twitter:
  - yes
daumview_id:
  - 36706855
categories:
  - 서버단
tags:
  - MySQL
---
추천글을 게시하는데, 시작일과 만료일을 기준으로 게시 여부를 판단하는 로직을 짜고 있었다.

관리자 페이지에서는 한 번에 이 목록을 불러 오되, 사용중인 글과 사용중이지 않은 글을 구분해서 보여 줘야 한다.

사용중인 글들을 위쪽에 표시하고, 그렇지 않은 놈들을 아래쪽에 몰아서 표시해 준다.

당연히 페이징 처리를 해야 한다.

으 썅 어떻게 하지? 처음에는 사용 여부를 DB에 저장해야 하나 하고 생각했다.

그러면 시작일과 만료일, 그리고 use_YN 이라는 이름의 컬럼을 만들어야 했다.

하지만 이것은 데이터 중복이다. 시작일과 만료일을 기준으로 사용여부를 가려낼 수 있는데 굳이 왜 또 사용여부라는 컬럼을 만들어야 하는가?

그러던 중 문득 mysql에서 함수를 사용하면 어떨까 하는 생각이 들었다.

즉, 함수를 이용해 use_YN이라는 가라 컬럼을 생성하면 어떨까 하는 생각이 들었던 것이다.

[MySQL 에서 IF문 사용하기][1] 라는 글이 도움을 줬다. 세 가지 용례를 정리해 둔 게 도움이 됐다.

그리고 그 결과 만든 쿼리다.

<pre>SELECT *, IF(`start_datetime` &lt;= CURDATE() and `end_datetime` &gt;= CURDATE(), &#039;Y&#039;, &#039;N&#039;) use_YN FROM `my_table`</pre>

위 쿼리를 사용하면 시작일과 만료일을 기준으로 현재 사용중인 글에는 Y가, 그렇지 않은 경우에는 N이 찍혀서 콘텐트가 들어온다. 컬럼명은 use_YN이다.

 [1]: http://www.webmadang.net/database/database.do?action=read&boardid=4003&page=1&seq=27