---
title: 'SVG 활용 2 &#8211; 일러스트레이터를 이용해서 SVG 파일 만들기'
author: 녹풍(綠風, Windgreen)
layout: post
permalink: /archives/11175
daumview_id:
  - 50143023
categories:
  - 기타
  - 웹 퍼블리싱
tags:
  - Program
  - SVG 활용
  - Web Design
  - 마법 나무 테마
---
이 글은 [블로그 디자인을 개편하면서 얻은 경험을 공유하는 글][1]이다. 첫 번째로, [SVG 활용에 대한 글][2]을 여러 편으로 나눠서 쓰고 있다.

## SVG 파일 만들기

우선 SVG를 만들어야 했다. 이 작업에는 Adobe Illustrator를 사용했다.

[내 로고에 사용한 마법 나무][3]는 istockphoto에서 구매한 그림인데, 구입해서 다운받은 이미지는 eps 포맷으로 돼 있었다. 그건 간단하게 svg로 변환해서 저장할 수 있었다. eps도 벡터 기반이므로 일러스트레이터에서 불러들인 다음 새 이름으로 저장을 하면 된다.

![SVG로 저장하는 방법 - Save as...][4]

새 이름으로 저장(Save as&#8230;)을 선택한 다음 하단에 있는 포맷에서 SVG를 선택하자. 그러면 아래처럼 SVG 옵션을 선택하게 나오는데, 옵션은 하나씩 살펴 보면 될 테고, 내 경우엔 별다른 변경 없이 그냥 저장하니까 잘 됐다.

![SVG로 저장할 때 설정 장면][5]

복잡한 건 소셜 아이콘을 만드는 과정이었다. 맘에 드는 심플한 소셜 아이콘 세트를 찾을 수 없었는데, SVG로 된 건 더더욱 찾을 수가 없었다. 그래서 해당 사이트의 로고를 다운받아서 일일이 제작을 해야 했다. 다행히 사이트 로고는 대부분 벡터 이미지도 제공하고 있었다.

## 비트맵 이미지를 벡터로 변환

벡터로 된 로고를 제공하지 않는 경우에는 일러스트레이터의 Image Trace 기능을 이용했다. 이 기능을 이용하면 일반 이미지를 벡터로 변환해 준다. 이 과정에서 이미지가 단순화되므로 실사는 별로다. 로고 아이콘처럼 선이 분명하고 색상수가 한정된 게 적당하다. 요즘 유행하는 플랫 스타일은 모양이 단순하고 선이 분명하기 때문에 딱 맞다고 할 수 있다.

![일러스트레이터의 이미지 트레이스 기능][6]

일러스트레이터에서 이미지를 불러들인 다음 선택을 하면 위쪽 도구 모음에 &#8216;Image Trace&#8217; 버튼이 나온다. 버튼 옆의 삼각형을 누르면 다양한 벡터 변환 방법이 나온다. 이걸 하나씩 해 보면서 변환하면 된다. 난 단색 이미지를 다뤘으므로 &#8216;3 Colors&#8217;로 변환했다.

원본 이미지 사이즈가 충분히 커야 변환이 제대로 된다는 점도 알아 두자.

이미지 트레이스 기능을 이용해 이미지를 벡터로 변환했는데, 각 개체가 충분히 나뉜 것 같지 않다면, 나눠야 할 개체를 선택한 다음 **Object > Expand&#8230;**를 눌러 보자. 아마 운이 좋으면 더 나뉠 것이다.

## 텍스트를 벡터로 변환

텍스트는 기본적으로 벡터기 때문에 일러스트레이터의 벡터 개체로 변환하는 것도 간단하다. 글자를 쓰고, 텍스트 개체를 선택한 후 마우스 우클릭을 하면 &#8216;Create Outline&#8217;이라는 메뉴가 나온다. 그걸 누르면 벡터 개체로 변한다. 아래 이미지를 참고하라.

![Create Outline 메뉴 모습][7]

텍스트를 벡터로 변환하면 텍스트 상자 하나가 통으로 벡터 개체가 되는데, 역시 마우스 우클릭을 해서 &#8216;Ungroup&#8217;을 누르면 낱자로 하나하나 선택할 수 있게 된다.

![ungroup 메뉴 모습][8]

## 벡터 개체 색 변경

SVG의 색 변경은 CSS로도 할 수 있지만 일러스트레이터에서 기본색을 지정해 두는 게 나을 것이다.

색을 변경하기 전에 해야 할 것이 있다. 일러스트레이터는 기본적으로 CMYK 모드를 사용한다. 이건 인쇄용 잉크 색상 모드다. 우리는 이미지를 웹에서 사용할 것이니 RGB 모드로 변경해 줘야 한다. 당장 내 블로그의 에메랄드색도 CMYK로는 표현이 안 되더라. 지정이 안 되서 한참 헤맸다. RGB 모드 변경은 **File > Document Color Mode**에서 찾을 수 있다. **RGB Mode**로 변경해 주자.

![RGB Mode로 변경하는 모습][9]

모드를 변경한 뒤, 개체를 선택하고 좌측 도구모음의 하단에 있는 색상중 좌측에 있는 것을 더블클릭하자. 그러면 색상표 창이 뜨고, 채울 색을 고를 수 있게 된다. 채워 줄 색을 고르고 확인을 누르면 선택한 개체의 색이 변하는 것을 볼 수 있다.

![색상을 채우는 모습][10]

개체에 외곽선을 넣고 싶다면 좌측 도구모음의 맨 하단에 있는 두 색상 중 우측에 있는 것을 더블클릭해서 색을 골라 주면 된다. 외곽선의 두께는 상단 도구바에서 설정할 수 있다.

## 벡터 개체 합치기, 빼기 등

Pathfinder 기능을 이용하면 개체 두 개를 합치거나, 한 개체에서 다른 개체를 빼거나 할 수 있다. 예컨대, 도넛 모양을 만들려면 원 두 개를 그린 다음 큰 원에서 작은 원을 빼면 되는 거다. 눈사람 모양을 만들려면 원 두 개를 만든 다음 합치면 되는 거고 말이다. 그런 걸 해 주는 게 Pathfinder다. 내 블로그의 파비콘을 만들 때 이 기능을 이용했다.

Pathfinder 기능 상자는 아래처럼 생겼다. 보이지 않는다면 **Window > Pathfinder** 메뉴를 눌러서 띄운다.

![Pathfinder 윈도우 모습][11]

위 이미지에서 나는 &#8216;Shape Modes&#8217;에 있는 기능 정도만 이용했다. 좌측부터 순서대로 합치는 기능, 한 개체에서 다른 개체를 빼는 기능, 교집합만 남기는 기능, 교집합만 빼는 기능이다.

## 사이즈 조정하기

SVG 사이즈 역시 CSS로 조정할 수 있지만 기본 이미지 사이즈도 지정해 주는 게 좋을 거다. 일단 Ctrl+A로 개체를 모두 선택한 뒤, 상단 도구바 우측의 &#8216;Trnasform&#8217;을 누른다. 그러면 아래처럼 사이즈를 조정할 수 있는 창이 뜬다. 당연히 한 개체만 선택해서 사이즈를 조정할 수도 있다.

![Transform 윈도우 모습][12]

단위는 pt로 돼 있는데, px이라고 생각하면 된다.

개체의 사이즈만 조정하는 게 아니라 전체 캔버스의 사이즈를 조정해야 한다. 일러스트레이터에서는 캔버스를 아트보드라고 부른다. Shift+O 키를 누르면 마우스로 아트보드의 사이즈를 조정할 수 있게 되는데, 그렇게 할 것 없이 개체에 딱 맞게 아트보드의 사이즈를 줄이도록 하는 기능을 이용하자.

메뉴에서 **Object > Art Boards > Fit to Artwork Bounds**를 누르면 개체들에 딱 맞게 아트보드의 사이즈가 줄어든다.

## 저장한 SVG 파일 정돈하기

일러스트레이터에서 SVG 파일로 저장을 한 뒤 텍스트 에디터에서 열어 보면 xml 문서라는 걸 알 수 있다. 그러니까 텍스트 에디터에서 좀 정돈을 해 주자.

일단 불필요한 용량을 차지하는 주석을 제거하자. 이 파일은 일러스트레이터에서 만들었다는 주석이 달려 있을 것이다. 지우자.

`svg` 태그에는 `id` 값이 매겨져 있을 것이다. 이것도 필요 없다. 지우자.

`g` 태그는 그룹을 짓는 요소다. 안에 있는 것만 남기고 그냥 지워도 무방하다. 지우려면 지우자.

`svg` 태그 안에 `desc` 태그를 넣어서 이미지 설명을 간단히 넣자. `img`의 `alt` 같은 것이라고 생각하면 된다.

마지막으로 필요하면 개체에 `class`를 매기자. 그런데 이것도 바깥에서 SVG를 감싸는 HTML 요소에 걸면 된다.

웹에서 사용할 수 있는 [SVG Optimizer][13]도 있다. 이걸 쓰면 용량을 줄일 수 있다.

## SVG 파일을 PNG로 변환하기

**File > Export**를 선택하면 PNG로 변환할 수 있다. Resolution을 선택하게 되는데, 72ppi를 선택하면 SVG에서 지정한 사이즈와 같은 사이즈로 PNG도 나온다. 당연히 Resolution이 높아질수록 Export하는 PNG 이미지 사이즈도 커진다.

## 나가며

이정도면 간단한 SVG 이미지 제작은 가능할 거다. 일단 이번 글은 여기서 마치고 다음 글에서는 웹페이지에 SVG 이미지를 넣는 다양한 방법을 알아 볼 것이다.

 [1]: http://mytory.local/archives/tag/%eb%a7%88%eb%b2%95-%eb%82%98%eb%ac%b4-%ed%85%8c%eb%a7%88
 [2]: http://mytory.local/archives/tag/svg-%ed%99%9c%ec%9a%a9
 [3]: http://www.istockphoto.com/stock-illustration-3561299-magic-tree-amp-birdie.php
 [4]: http://dl.dropboxusercontent.com/u/15546257/blog/mytory/svg/save-as-svg.png
 [5]: http://dl.dropboxusercontent.com/u/15546257/blog/mytory/svg/save-as-svg-setting.png
 [6]: http://dl.dropboxusercontent.com/u/15546257/blog/mytory/svg/image-trace-for-svg.png
 [7]: http://dl.dropboxusercontent.com/u/15546257/blog/mytory/svg/illustrator-create-outline.png
 [8]: http://dl.dropboxusercontent.com/u/15546257/blog/mytory/svg/illustrator-ungroup.png
 [9]: http://dl.dropboxusercontent.com/u/15546257/blog/mytory/svg/illustrator-rgb-mode.png
 [10]: http://dl.dropboxusercontent.com/u/15546257/blog/mytory/svg/illustrator-fill.png
 [11]: http://dl.dropboxusercontent.com/u/15546257/blog/mytory/svg/illustrator-pathfinder-window.png
 [12]: http://dl.dropboxusercontent.com/u/15546257/blog/mytory/svg/illustrator-transform.png
 [13]: http://petercollingridge.appspot.com/svg-optimiser