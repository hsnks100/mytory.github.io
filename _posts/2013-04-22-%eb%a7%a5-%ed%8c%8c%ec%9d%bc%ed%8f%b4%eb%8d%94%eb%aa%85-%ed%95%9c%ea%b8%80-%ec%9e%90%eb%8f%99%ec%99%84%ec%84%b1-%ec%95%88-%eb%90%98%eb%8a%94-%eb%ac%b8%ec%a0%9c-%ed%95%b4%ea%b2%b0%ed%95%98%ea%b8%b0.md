---
title: 맥 터미널 파일/폴더명 한글 자동완성 안 되는 문제 해결하기
author: 녹풍(綠風, Windgreen)
layout: post
permalink: /archives/9967
daumview_id:
  - 43477381
categories:
  - 서버단
tags:
  - shell
---
shell에서 폴더명이랑 파일명을 일일이 칠 필요가 없이 첫글자 조금만 치고 탭 키를 누르면 자동완성이 된다는 사실을 알았을 때 그동안 속았다는 기분이었다. (도스는 쉘은 아니지만;; 여튼간에 얼마나 오랫동안 나는 도스에서 파일명을 일일이 쳐 왔던가.)

그런데, 리눅스에서는 한글도 자동완성이 잘 되는데, 유독 맥의 터미널에서는 한글 자동완성이 되지 않는 것이다. 그냥 참고 살다가 요새 터미널을 자주 사용하게 되어, 해결책을 찾아 봤다. 역시나 해결책은 있었다. 무려 세 가지나 있었다. [뭔가 패치를 적용하는 해결책][1]도 있고 [입력기를 설치하는 해결책][2]도 있는 것 같던데, 나는 bash를 판올림해서 해결했다. (근데 뭔가 패치를 적용하는 해결책은 도무지 뭘 어떻게 하라는 건지 모르겠다.)

나는 OSX 라이언(10.8.3)에 bash 4.2로 업그레이드였고, 성공했다.

## bash란 무엇인가?

터미널에서 cp, mv, find 등의 명령어를 사용하는데, 이건 명령어 세트다. 이런 명령어 세트를 제공하는 것이 shell이다. 여러 가지 쉘이 있어서 자기 맘에 드는 것을 갖다 사용하면 된다고 한다. (내가 뭐 맘에 드는 걸 갖다 사용해 본 적은 없으니까.)

bash는 그중 가장 널리 사용되는 쉘이라고 한다. Borune-again shell의 약자라고;; 여튼 그래서 이 놈도 여전히 지속적으로 개발이 되고 있다.

난 개발자가 되기 전엔, GUI가 있는데 뭐하러 터미널을 사용하나 하고 생각했었다. 리눅스를 조금 사용하게 됐을 때도 GUI가 더 편했고, GUI로 할 수 있는 것은 다 GUI로 하려고 했다. 그런데 결국 알게 된 것은, shell을 사용할 때 더 편한 게 분명히(!) 있다는 것이었다. bash 같은 게 요새도 개발되고 있고 계속 업그레이드되고 있다는 사실을 알게 됐을 땐 조금 충격이기도 했다. 세상은 넓고 배울 건 많구나. 여튼간에, 내가 다운받은 bash 4.2는 미러 서버의 ftp에 올라와 있는 압축 파일의 날짜가 2011년 2월 13일이다. 아마 그쯤 출시됐겠지. 그러니까 CUI는 구닥다리가 전혀 아니고, 계속 개발되고 있는 최신 프로그램인 거다. 와우.

여튼 이놈을 업그레이드한다는 게 개념이 잘 이해되지 않을 수도 있다는 생각이 들어서 설명을 좀 했고, 다음으로 넘어가서 업그레이드를 해 보자.

## 업그레이드해 보자

bash는 일단 [bash 공식 웹사이트][3]에 가서 다운을 받는다. 무슨 폴더가 나올 텐데, 버전 제일 높은 놈 tar.gz을 다운받으면 된다. tar.gz.sig 파일은 아마 시그너쳐 파일이라고, 파일 무결성을 검사하는 데 사용하는 파일일 거다.

다음으로는, 다음 글을 보고 스텝을 따라한다. 영어지만 차분히 해석해 보면 될 것이다. 쉘을 사용하는 사람들이라면 여튼 이정도는 보고 따라할 수 있을 거라 생각한다. 시행착오도 좀 겪을 수 있을 것이고. ^^ 그럼 이만.

[Change to new BASH Shell (4.1) for Mac OS X][4]

&nbsp;

 [1]: http://psg9.egloos.com/2609961
 [2]: http://www.albireo.net/threads/13938/
 [3]: http://www.gnu.org/software/bash/bash.html
 [4]: http://techscienceinterest.blogspot.kr/2010/05/change-to-new-bash-shell-41-for-mac-os.html