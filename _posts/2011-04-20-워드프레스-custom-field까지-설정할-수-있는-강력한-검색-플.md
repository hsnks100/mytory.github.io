---
title: '[워드프레스] custom field까지 설정할 수 있는 강력한 검색 플러그인 : Relevanssi &#8211; A Better Search'
author: 안형우
layout: post
permalink: /archives/1127
aktt_notify_twitter:
  - yes
daumview_id:
  - 36748163
categories:
  - WordPress
tags:
  - WordPress Plugin
---
**글의 요지 : Relevanssi &#8211; A Better Search 플러그인을 이용하면 다양한 검색 조건을 넣을 수 있다. 특히, 특정 Custom Field와 특정 카테고리 등을 검색에 잡히게 하거나 제외하게 하는 등의 세부사항을 설정할 수 있다. 이 플러그인은 무료로 제공되며, 더 강력한 기능을 가진 프리미엄 버전도 있다. 한글 사용자들이 쓰려면 DB에서 인코딩을 바꿔 줘야 할 수도 있다.**

워드프레스를 이용해서 웹사이트 제작을 하고 있다. CMS를 사용하니 분명한 장점이 있다. 매번 반복해야 하는 페이징, 본문 불러오기, 리스트 만들기, 입력하기 등등의 작업을 굳이 하지 않아도 된다는 점은 정말 매력적이다.

그러나 분명한 단점도 있다. CMS의 규칙을 벗어나는 것을 하나 하려고 하면 온갖 규칙에 얽매여서 제대로 하기가 힘들어질 때가 있는 것이다. 물론, CMS를 하나의 언어라고 생각하면 문제는 쉬워진다. CMS의 각종 함수에 통달해 있다면 기능을 추가하는 것이 그다지 어렵지 않을 것이기 때문이다.

워드프레스를 이요한 개발을 처음 해보는 나는 각종 함수에 통달해 있지 않았다. 다음 난제에 부딪혔다.

## 난제

워드프레스는 post_meta 라는 테이블을 이용해서 포스트에 추가로 들어가는 컬럼을 무한히 확장할 수 있다. 컬럼을 무한히 추가하는 방식이 아니라 key=value 쌍으로 데이터를 저장한다.

예컨대 출판사 사이트를 만든다고 해 보자. 워드프레스에 기본으로 들어가는 정보는 제목과 본문이다. 그럼 저자, 역자, 저자 소개, 역자 소개, 책 소개, 목차 등의 정보는? 바로 post_meta에 저장하면 된다. 그리고 그걸 쉽게 만들어 주는 플러그인이 바로 [Custom Field Template][1] 이다. (이를 설명한 글은 : 워드프레스 플러그인 “Custom Field Template” ― 강력한 사용자정의 필드 tool)

이것을 이용해서 내가 개발하는 웹사이트도 잘 만들었다. 그런데?

문제가 생겼다. 요구사항이 추가됐는데, 검색시 다른 정보들은 검색하지 말고 책 제목과 저자만 검색하게 해 달라는 것! 두둥~ 이것은 post의 기본 정보 중 제목과 custom field들 중 일부만을 검색해 달라는 말이다.

일단 워드프레스의 기본 검색은 custom field를 검색하지 못한다. 따라서 직접 검색 쿼리를 짜거나, 플러그인을 사용해야 한다.

## 플러그인 뒤지기

그래서 플러그인을 뒤지기 시작했고, 다양한 검색 플러그인을 테스트해 봤다.

*   Search API: 작동 안 함
*   Search Everything: 모든 것을 검색하지만, custom field의 일부만 설정할 수 있는 기능이 없음.
*   WP Custom Fields Search: custom field를 하나씩만 검색할 수 있고, 동시에 몇 가지의 custom field를 검색할 방법이 없음.
*   Search Unleashed: 강력한 기능을 갖추고 있지만 최신 버전의 워드프레스에서 제대로 작동하지 않음
*   wpSearchMu: php5가 필요하며, Mu라는 워드프레스 프로젝트의 일부인데, Mu 프로젝트는 워드프레스 3.0 버전으로 넘어오면서 중단된 듯함.
*   Search By Category 2.0.1: 작동안함
*   yd-search-functions: 검색 스타일만 바꿔 주는 플러그인임
*   Search Engine query in WordPress: 검색엔진을 통해 블로그에 들어왔을 때, 검색한 단어를 표시해 주는 플러그인인 듯함. 우리가 찾는 기능과 무관.
*   Better Search: 원하는 기능 없음
*   Advanced Category Excluder: 카테고리를 지정해서 검색해 주기만 하는 플러그인임.

이런 시행착오를 거친 끝에 결국 Relevanssi &#8211; A Better Search 플러그인을 찾아 냈다.

## Relevanssi &#8211; A Better Search 플러그인의 특징

이 부분은 Relevanssi &#8211; A Better Search 플러그인 페이지의 소개를 번역한 부분이다.

**주요 특징**

*   검색 결과를 날짜순이 아니라 중요도순으로 정렬한다.
*   퍼지 검색 지원 : 단어의 일부만 맞고, 전체가 완벽히 맞지 않아도 검색된다.[역자 주: 퍼지 검색은 인공지능 검색으로, 사용자가 애매한 단어를 쳐도 적당한 다른 놈을 골라서 뿌려 주는 것을 말하는 듯함. 참고: [위키피디아 퍼지 논리][2]]
*   하나의 검색어만 맞아도 나오게 할 수도 있고(OR 검색), 모든 단어가 맞아야 나오게 할 수도 있다.(AND 검색)
*   따옴표와 함께 검색할 수 있다. 예를 들면, &#8220;이 문장을 검색하라&#8221;
*   검색어 주변 문장을 발췌해서 보여주는 것을 다양하게 설정할 수 있다. 원하는 부분을 강조할 수도 있다.
*   검색 결과를 클릭했을 때, 문서 본문에서 검색어를 강조할 수 있다.
*   댓글, 태그, 카테고리, 커스텀 필드를 검색할 수 있다.

**고급 기능**

*   제목, 태그, 댓글의 검색 중요도를 설정할 수 있다.
*   쿼리를 저장하고, 인기 검색어와 최근 검색어를 보여줄 수 있다. 검색 횟수는 제외.
*   hidden 변수나 플러그인 세팅을 이용해서 카테고리와 태그 중 일부만으로 검색 영역을 제한할 수 있음.
*   사용자 정의 포스트 타입과 사용자 정의 taxonomy를 인덱스할 수 있음.[인덱스한다는 건 검색을 용이하게 하기 위해 최적화한 데이터를 DB에 저장한다는 것을 말함]
*   [WPML multi-language plugin][3]을 자동으로 지원함.
*   검색 결과를 원하는대로 조작할 수 있도록 고급 필터를 제공함.

## 라이센스

이 부분 역시 라이센스 부분을 번역한 것이다.

Relevanssi는 레귤러 버전과 프리미엄 버전이 있다. 레귤러 Relevanssi는 공짜고 앞으로도 그럴 것이다. Relevanssi 프리미엄은 비용을 지불해야 하지만 더 새로운 기능을 사용할 수 있다. 표준 Relevanssi는 버그만 지속적으로 수정할 것이지만, 프리미엄 버전은 계속 새 기능이 추가된다. 또한 표준 Relevanssi에 대한 지원은 전적으로 내 기분과 가용 시간에 달려있지만, 프리미엄 버전 구입비에는 지원 비용이 포함돼 있다.

**프리미엄 버전의 특징(프리미엄 버전에만 해당함)**

*   대용량 데이터베이스에서 검색 속도 개선
*   철자 오류 수정과 &#8220;혹시 이걸 검색하신 건가요?&#8221; 기능 개선.
*   유저 프로필 검색과 인덱스.
*   포스트 타입에 따른 검색 중요도 설정.
*   외부 검색 엔진 유입시 검색어에 대한 하이라이트 기능.

## 사용법

사용법은 간단하다. 설정은 보면 알 수 있게 돼 있으니 패스한다. 개발자면 누구나 설정 가능할 것이라고 본다.

다만 두 가지, 좀 까다로운 부분만 설명하겠다.

일단 퍼지 검색 항목. 퍼지 검색은 인공지능 검색으로, 꼭 검색어와 일치하지 않더라도 대충 비슷한 놈까지 불러 주는 검색이다. 이게 한국어에서는 제대로 작동할 리 만무하다. 따라서 끄는 편이 속편할 듯하다. 이상한 검색 결과를 보고 싶지 않다면 말이다. (물론 각자 테스트해 보고 판단하기 바란다.)

다음, 이 검색엔진은 인덱싱을 기반으로 작동한다. 따라서 맨 위에 있는 인덱싱 버튼을 눌러 줘야 작동한다. 그런데, 골때리는 게 어떤 경우 DB가 latin을 기본 인코딩으로 생성되기 때문에 한글을 인식하지 못한다. 따라서 DB의 테이블을 죄다 utf8이나 euckr로 변경해 줘야 한다. 그래야 인덱싱이 제대로 된다.

인덱싱이 제대로 됐는지는 설정 페이지의 맨 밑에 가 보면 알 수 있다.

따로 코드를 적어 주거나 할 필요는 없다. 기본 검색창에서 잘 돌아 간다.

이상이다.

 [1]: http://wordpress.org/extend/plugins/custom-field-template/
 [2]: http://ko.wikipedia.org/wiki/%ED%8D%BC%EC%A7%80_%EB%85%BC%EB%A6%AC
 [3]: http://wpml.org/