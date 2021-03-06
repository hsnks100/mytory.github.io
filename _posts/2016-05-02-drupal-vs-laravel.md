---
title: '드루팔이냐 라라벨이냐'
layout: post
tags:
  - PHP
  - Framework
---

[&lt;노동자 연대&gt;](http://wspaper.org/) 웹사이트를 새로 구축하기로 결정했다. 지금까지는 인트라넷 작업을 하느라 신문 웹사이트는 손을 많이 못 댔다. 그러나 앞으로는 웹사이트에 더 많이 공을 들이려고 계획중이다. 

지금 웹사이트는 2006년에 구현한 것이다. 당시 나는 아직 개발자가 아니었고 다른 사람이 구축한 것이다. 겉모양엔 많은 변화가 있었지만, 안 쪽 코드는 많이 낡았다.

아래는 구현할 CMS와 프레임웍을 고르면서 나름대로 비교한 것을 간략히 적은 것이다.


워드프레스
---------

워드프레스는 훌륭한 CMS다. [블로터](http://bloter.net)나 [레디앙](http://www.redian.org/) 같은 곳은 워드프레스로 구축돼 있고 잘 운영되는 듯하다.

그런데 지난 몇 년 간 워드프레스로 사이트를 구축해 온 나에겐 걸리는 점이 있었다. 내가 '인지 비용'[^fn3]이라고 부르는 단점이다.

Post Type이나 Taxonomy(분류) 같은 것을 커스터마이징할 때 사고에 비용이 좀 든다는 것이 내 생각이다. 예컨대, 워드프레스는 Post Type을 추가하려면 테마나 플러그인에서 아래와 같은 코드를 사용한다(워드프레스 Custom Post Type 예제 코드).

<pre>
add_action( 'init', 'codex_book_init' );
/**
 * Register a book post type.
 *
 * @link http://codex.wordpress.org/Function_Reference/register_post_type
 */
function codex_book_init() {
    $labels = array(
        'name'               => _x( 'Books', 'post type general name', 'your-plugin-textdomain' ),
        'singular_name'      => _x( 'Book', 'post type singular name', 'your-plugin-textdomain' ),
        'menu_name'          => _x( 'Books', 'admin menu', 'your-plugin-textdomain' ),
        'name_admin_bar'     => _x( 'Book', 'add new on admin bar', 'your-plugin-textdomain' ),
        'add_new'            => _x( 'Add New', 'book', 'your-plugin-textdomain' ),
        'add_new_item'       => __( 'Add New Book', 'your-plugin-textdomain' ),
        'new_item'           => __( 'New Book', 'your-plugin-textdomain' ),
        'edit_item'          => __( 'Edit Book', 'your-plugin-textdomain' ),
        'view_item'          => __( 'View Book', 'your-plugin-textdomain' ),
        'all_items'          => __( 'All Books', 'your-plugin-textdomain' ),
        'search_items'       => __( 'Search Books', 'your-plugin-textdomain' ),
        'parent_item_colon'  => __( 'Parent Books:', 'your-plugin-textdomain' ),
        'not_found'          => __( 'No books found.', 'your-plugin-textdomain' ),
        'not_found_in_trash' => __( 'No books found in Trash.', 'your-plugin-textdomain' )
    );

    $args = array(
        'labels'             => $labels,
        'description'        => __( 'Description.', 'your-plugin-textdomain' ),
        'public'             => true,
        'publicly_queryable' => true,
        'show_ui'            => true,
        'show_in_menu'       => true,
        'query_var'          => true,
        'rewrite'            => array( 'slug' => 'book' ),
        'capability_type'    => 'post',
        'has_archive'        => true,
        'hierarchical'       => false,
        'menu_position'      => null,
        'supports'           => array( 'title', 'editor', 'author', 'thumbnail', 'excerpt', 'comments' )
    );

    register_post_type( 'book', $args );
}
</pre>

Post Type을 서너개만 만들어도 이런 코드가 여러 개 생긴다. Post Type에 한글 이름을 붙이는 것도 꽤 성가시다. 

사실 포스트 타입이 다르면 그냥 별도 테이블을 사용하고, 모델도 따로 짜는 게 사고 과정에 잘 맞는다. 분류 같은 경우도 그냥 DB의 `category` 컬럼에 들어 있는 것을 사용하면 된다. 인지 비용이 낮다. 이에 비해 워드프레스는 생각을 약간 틀어야 한다. 나에겐 약간의 왜곡처럼 느껴진다.

관리자단을 커스터마이징하는 것도 같은 측면에서 턱 걸리는 것이 있는 것처럼 느껴졌다. 워드프레스에선 관리자단에 페이지를 하나 추가하려면 함수를 몇 개 추가해야 한다. 아래는 플러그인 페이지를 추가하는 함수다. 맨 마지막 인자값인 `$function`이 콜백 함수고, 이 콜백함수가 페이지의 HTML을 들고 있다. 

    add_plugins_page( $page_title, $menu_title, $capability, $menu_slug, $function);
    
이해하면 이 코드가 그리 어렵지 않긴 한데, 직관적이진 않다. 

이게 내가 워드프레스를 고르기 껄끄러웠던 이유다. 함수를 만들고, 훅을 걸고, 훅에서 함수를 호출해 그 함수가 다른 함수를 호출하고, 마지막에 호출된 함수가 HTML을 렌더링하고... 이게 그리 직관적이진 않다는 것이다.

워드프레스는 블로그 플랫폼으로 출발했다. 그리고 다양한 훅과 필터를 바탕으로 플러그인 생태계를 이룬 뒤 CMS로 발전했다. 오랜 궤적을 그리며 이어 온 생태계는 크나큰 장점이다. 그러나 역설적으로 나에겐 세월의 흔적이 묻어 나는 그 확장이 오히려 단점이 되었다.

사실 워드프레스는 표준화된 웹사이트다. 프로그래밍을 레고 블록들의 조립으로 대체하려는 노력이, 어쩌면 웹 어플리케이션 분야에선 워드프레스로 결실을 맺었다고 볼 수 있을 지도 모르겠다. 여전히 필요에 맞는 사이트를 구현하려면 많은 코드를 직접 짜야 하지만 말이다. 

그래도 관리자 페이지가 기본으로 구현돼 있고, 괜찮은 플러그인이 꽤 있다는 점은 큰 장점이다. 나도 User Role Editor, Google Analyticator, Limit Login Attempts, W3 Total Cache 같은 플러그인을 사용한다. 내가 만든 Uploading downloading non-latin filename 플러그인도 잘 사용하고 있다.

앞으로도 많은 사이트를 워드프레스로 구축하게 될 것이다. 하지만, 이번에 만들 신문 사이트에는 아니라는 결론을 내렸다. 많은 시간을 사이트 개발 자체를 하면서 보내게 될 것이고, 다양하고 유연한 기능들이 많이 들어가야만 하는 사이트다. 인지 비용이 드는 워드프레스는 뭔가 맘에 딱 들지 않았다.


드루팔(Drupal)
--------------

워드프레스를 후보에서 제외한 뒤에는 해외 뉴스 사이트들이 어떤 프레임웍을 사용하는지 살펴 봤는데, [Newscoop](https://www.sourcefabric.org/en/newscoop/) 같은 뉴스CMS보다도 [드루팔](https://www.drupal.org/) 같은 일반화된 CMS를 더 많이 사용했다. 하긴 CMS는 생태계가 훨씬 중요할 테니 그럴 법하다는 생각이 들었다.[^fn4]

뉴스 CMS를 보면서 재밌게 본 것은 [vox.com](http://vox.com/)의 코러스(Chorus)다. 요새 [각광받고 있다](http://www.bloter.net/archives/196694)고 해서 테스트해 보려고 했는데, 웬걸 공개된 것이 아니었다. 다만, 기능을 묘사해 둔 글들을 보니 구현할 기능을 참고하는 데 활용할 수 있겠다는 생각이 들었다.

드루팔은 상당히 유연해 보였다. Content Type과 해당 글을 편집하는 UI를 기본적으로 관리자단에서 관리할 수 있다고 했다. 국내에선 [진보넷](http://act.jinbo.net/drupal/)이 드루팔로 구현돼 있고, 처음 진보넷이 사이트를 드루팔로 리뉴얼했을 때 소개한 글도 본 적이 있었다. [꽤 칭찬을 하는 글이었다.](http://act.jinbo.net/drupal/node/5569) 다만, 2011년인가 테스트해 봤을 때 너무 느리다는 인상이 있었다. 그 땐 초보였으니 느리다고 폐기했었다. 이젠 캐시를 활용하고 웹서버와 PHP 세팅을 잘 하면 된다는 생각을 한다. 

이런 이유로 팀원 중 한 명에게 드루팔의 기본적인 것을 공부하고 발표해 달라고 부탁했다. 짧게 본 소감은 좀더 확장된 CMS로서의 워드프레스 같았다는 것이다. 워드프레스는 코드에서 새로운 Post Type이나 Taxonomy를 등록한다. 드루팔을 그것을 관리자 인터페이스로 처리한다. 써드 파티 모듈들이 이런 Content Type과 Taxonomy에 잘 결합돼 있을 법하다. 

이것을 보고 드루팔이 어쩌면 최선의 선택이 될 수 있지 않을까 하는 생각을 했다. 관리자가 직접 Content Type(워드프레스의 Post Type에 해당하는 것)을 정의하고, 해당 Content의 필드들을 정의하면 관리자 페이지가 만들어진다. 워드프레스는 이 과정에 개발자가 개입해야 한다. 커스텀 포스트 타입 UI 플러그인이 있긴 하지만, 플러그인이다.

즉, 워드프레스보다 직관적으로 느껴졌다.


### 규약... ###

그러나 개발자가 커스터마이징을 하기 시작하면 이야기가 달라지는 것 같다. 유연함은 개발의 어려움과 비례한다고 말할 수도 있을 테니, 유연함이 극대화된 이 CMS는 규약이 많을 것으로 생각할 수 있을 것이고, 과연 그랬다.

드루팔은 모듈과 테마라는 두 범주로 구성돼 있다. 기능은 모듈, 외양은 테마로 처리한다. 기능과 모양이 혼연일체를 이루는 워드프레스보다 구분은 명확해서 좋았다.[^fn1]

하지만 역시 모듈은 훅을 거는 것이었다. CMS에서 기능을 주입하려면 당연한 말이지만, 이 점이 걸렸다.

테마는 영역을 규정하고, 해당 영역으로 블록을 쌓는다. 이 말은 영역별, 블록별로 코드를 작성하고 스타일을 입혀야 한다는 말이 된다. 이 역시 그리 유쾌한 것은 아니었다. 

이에 수반하는 많은 규약들... 워드프레스 만큼이나 많은 규약이 있을 것이고, 초기엔 개발의 상당시간을 규약을 익히는 데 보내야 할 것이다. 그리고 다 구현했더니 뒤늦게 좀더 손쉽게 구현할 수 있는 규약을 발견하는 경우도 있을 것이다.

사실 이게 그렇게 나쁘다고 할 수는 없다. 유연한 CMS라면 당연히 이런 식으로 구성되어야 할 것이다. 하지만, 나로서는 꽤 큰 프로젝트를 앞에 두고 있는 상황에서, 그리고 향후 10년은 관리해야 할 사이트를 개발하는 입장에서, 꽤나 많은 규약에 종속돼 개발을 하는 것은 썩 내키지 않았다.

간단한 사이트를 만들고, 주로 관리를 하게 된다면 드루팔을 사용하는 것이 여러 모로 도움이 될 것이다. 사실 드루팔을 보고, 그 동안 워드프레스로 개발했던 사이트들을 드루팔로 구현했다면 더 좋았겠다는 생각을 했다. 

소프트웨어 개발을 레고 블록 조립처럼 만들려는 시도는 어쩌면 워드프레스보단 드루팔이 더 앞서 있을 지도 모르겠다는 생각이 든다.


라라벨
------

### 즐거움이라는 요소 ###

[소프트웨어 공학의 정의는 "소프트웨어를 최소 비용으로 최소 시간에 개발하는 방법"](http://www.allofsoftware.net/2008/12/blog-post_10.html)이라고 한다. 이 기준에 맞춰 툴을 고르려고 노력했다. 결론은 라라벨이었다. 코드 작성 시간을 꽤나 단축시켜 주는 여러 가지 모듈, 그리고 코드 작성의 즐거움. 두 가지가 결국 라라벨을 고르게 된 이유다. 

드루팔과 라라벨의 생산성을 비교한다면, 어쩌면 드루팔이 좀 앞설 지도 모르겠다. 익숙해지기만 하면 사이트를 뚝딱 만들어 낼 지도 모르겠다는 생각도 든다. 즉, 굳이 드루팔이 아니라 라라벨을 선택한 이유는 생산성보다는 코드 작성의 즐거움 때문이었다. 라라벨이 꽤 기능이 많은 프레임웍이긴 하지만, 그렇다고 해도 드루팔 같은 CMS보단 자유도가 있다.

CMS나 프레임웍을 선택하는 이유는, 단순반복을 툴에 맡기고 개발자는 중요한 비즈니스 로직에 집중하기 위해서라고들 한다. 그런데 내가 워드프레스로 개발을 하면서 느낀 것은 규약을 약간 벗어나면 오히려 꽤 힘들어지는 경우가 있다는 것이다(사실 이건 CMS의 숙명이라고 할 수 있다).

예컨대, 워드프레스를 보면 메뉴를 UI로 조정할 수 있다. 매우 간편하다. 하지만, 메뉴의 코드를 변경하려면 꽤 시간이 걸린다. 그냥 HTML을 작성하면 됐을 것을... 하는 생각이 드는 때도 있다. 실제로 사이트를 만들고, 개발자가 관리자로 있는 경우 워드프레스의 메뉴 기능을 비활성화해 버리고 직접 코드를 작성하는 게 나을 수도 있다.

코드 이그니터는 또 약간 다르다. 경량 프레임웍으로, 객체 모델은 제공하지 않고 DB와 통신하는 추상화된 클래스인 Active Record만 제공하는데, 이것도 쿼리를 날리는 것보다는 꽤 간편했다. 아래처럼 쓰면 2016년 1월 1일 이후 작성된 노트들을 가져온다.

    $rows = $this->db->get_where('notes', ['date >=' => '2016-01-01'])->result();

그리고 규약이 거의 없으니 꽤 코딩할 맛이 났다. 어쩌면 라라벨이 없었다면 코드 이그니터를 선택했을 지도 모르겠다. 물론 코드 이그니터는 제공하는 기능이 많지 않아서 생산성이 CMS보다 꽤 떨어진다. 그래서 뭐라 단정할 수는 없다. 아마 [CakePHP](http://cakephp.org/) 같은 것을 더 뒤져 봤을 것이다. 그러나 현실엔 [라라벨](https://laravel.com/)이 있다.


### 모던 PHP ###

라라벨은 꽤 최근에 나온 프레임웍이다. 허용하는 PHP 최하 버전은 5.5.9로, 워드프레스나 코드 이그니터보다 요구사항이 꽤 높다. Composer와 거의 혼연일체를 이루고 있어서, laravel은 Composer 모듈들의 집합이라고 봐도 될 정도다. 실제로 또다른 PHP 프레임웍인 [Sympony](https://symfony.com/)의 모듈들을 많이 가져와 사용하고 있다.

PHP가 구식이고 기능도 볼품없다는 이야기가 종종 있는데, 이미 버전 5부터는 그런 이야기가 크게 의미있지는 않게 된 듯하고, 더군다다 이제 PHP7이 출시돼 격세지감이 느껴진다. 라라벨은 PHP의 최신 기능들을 잘 활용하고 있다. trait이나 namespace 같은 것들 말이다. 즉, 라라벨은 사용하는 것만으로 모던 PHP의 기운을 느낄 수 있다.[^fn5]


### 커맨드 라인 ###

PHP의 커맨드라인 지원이라는 것은 초기엔 그리 훌륭하지 않았던 것 같다. 인터렉티브 쉘을 호출하는(다른 언어들에선 거의 기본이다) `php -a` 명령어는 5.1에서야 등장했다. 내장 웹서버를 실행하는 `php -S localhost:8000` 같은 명령어는 5.4가 돼서야 사용할 수 있게 됐다. 여하튼, 이제는 PHP가 다른 언어들에 비해 기능이 딸린다고 말할 수는 없는 수준이 된 것 같다는 사실이 중요하다. 라라벨은 이런 철학에 발맞추고 있는 듯하다. 아래는 객체 모델을 생성하는 명령어다.

    php artisan make:model Note
    
이러면 `Note` 모델이 만들어진다. Django나 Rails에서 보던 모습이다.[^fn2]

심지어 `laravel` 패키지를 설치해 `laravel new my-site` 하고 명령을 내릴 수도 있다.

php 내장 웹서버를 이용해 laravel을 돌리는 명령어도 제공한다.

    php artisan serve
    
커맨드라인 명령어를 추가하는 것은 상당히 쉽다. 역시 커맨드라인으로 틀을 생성한 뒤 비즈니스 로직을 작성하면 된다.

    php artisan make:console command_name


### ORM(Object Relation Model) ###

기본적으로 객체 모델을 사용한다는 점도 매력적이었다. 

<pre>
use App\Note;

$note = Note::find(1);
$other_notes = Note::where('id', '!=', '1')->get();
$note->title = 'Changed Title';
$note->save();
</pre>
    
그간 같이 개발한 개발자들이 이런 식의 객체 모델을 만들기 위해 노력하는 모습들을 봐 왔고, 흔히 혼자 시간에 쫓기며 작성한 그 모델들은 제대로 작동하지 않았다. 내부를 파헤치고 이해하고 바꾸느라 시간이 오히려 더 들곤 했다. 하지만 라라벨의 ORM은 수많은 사람들의 공동 작업 성과물이다. 따라서 내가 내부 구조를 들여다 볼 일은 거의 없을 것이다.[^fn6] 탄탄한 객체 모델은 정말로 나를 비즈니스 로직에 집중할 수 있게 해 줄 듯하다.

언어를 고르기 위해서 팀원들끼리 볼 만한 링크를 공유하는 간단한 웹 어플리케이션을 작성해 봤다. ORM 덕에 코드가 줄었고, 말하자면 비즈니스 로직에 집중할 수 있었다.
    
또한, 각 객체 모델이 DB의 어떤 컬럼과 연결되는지 라라벨이 알고 있기 때문에, 아래와 같이 프로퍼티를 활용한 함수도 쓸 수 있다.

    
    App\Tag::whereNoteId($note->id);


### 기타 ###

모던 PHP, 커맨드 라인, ORM 세 가지가 가장 인상적이었지만, 매력적인 게 더 많다. 예컨대, Composer로 설치할 수 있는 Laravel IDE Helper라던가, 이메일 드라이버를 잘 지원한다든가, 로그인 페이지와 자동 로그인 기능을 기본적으로 갖고 있다든가 하는 것들이다.


최종 고민
---------

마지막 고민은 관리자단을 만들어주지 않는 라라벨이 관리자단을 기본적으로 제공하는 드루팔에 비교해 얼마나 효율적일 것인가 하는 점이었다. 그래서 관리자단을 만들어 주는 라라벨 플러그인을 찾아 보기도 했는데, 쓸만한 게 없었다. 깔끔해 보이는 플러그인도 문서가 제대로 갖춰져 있지 않았다. 관리자단이 플러그인에 종속되는 것도 꺼림칙한데 잘 관리되지 않는 느낌을 주는 플러그인을 사용할 수는 없었다.

그런데 모델이 깔끔하게 구현돼 있으면 관리자단을 만드는 것이 그리 어렵지 않을 것 같다는 생각이 들었다. 폼에서 포스트로 요청을 보내면, `*_action.php`에서 받아 배열에 프로퍼티를 일일이 세팅하고, 그걸로 쿼리를 짜고 했던 과정들이 관리자단을 피곤하게 만들었다. 라라벨을 이용하면 이 과정이 훨씬 단축될 것이고, 그렇다면 해 볼 만 하겠다는 생각이 들었다. 아직 검증되진 않았지만, 해 보면 알 수 있을 것이다.

그래서 결론은, **"생산성 차이가 확연하지 않다면 즐겁게 개발할 수 있는 것을 고르자"** 하는 것이었다. 즉, 개발자의 사기를 위해서 내린 결정이다.

개발해 보고 나서 후기를 작성해 보려고 한다. 이상.


    








[^fn1]: 워드프레스에서는 고가의 테마를 사서 많은 커스터마이징을 하고 나면 해당 테마에 종속된 데이터 타입들 때문에 이전비용이 높아진다. 드루팔이라고 전혀 안 그렇진 않을 테지만, 기본적으로 제공하는 Content Type과 테마 정의 덕에 좀더 낫지 않을까 하는 생각이 들기도 했다. 뭐, 워드프레스도 어차피 우리가 직접 구현할 테니 이전 비용 안 나게 개발하면 되지만서도...

[^fn2]: 아, Django를 사용할까도 생각해 봤다. 그런데 팀원들 대부분이 PHP에 가장 익숙한 상황이었고, 그래서 포기했다. 생산성이 중요했다.

[^fn3]: 인지 비용은 이 글에서 내가 그냥 사용하는 말이다. 직관적으로 이해가 잘 되면 인지 비용이 낮다고 표현할 것이고, 그렇지 않은 경우 높다고 표현하는 것이다.

[^fn4]: 이건 내가 조사한 것은 아니고 [팀원](http://gnoownow10.cafe24.com/)이 조사한 것이다. 

[^fn5]: [PHP The Right Way(한국어판)](http://modernpug.github.io/php-the-right-way/) 같은 글을 보면 모던 PHP가 어떤 것인지 잘 설명돼 있다.

[^fn6]: 버그 리포트, 혹은 공부.