---
title: Sublime Text 2에서 Sass 사용할 때, 닫히지 않은 주석 때문에 코드 하이라이팅이 잘 되지 않는 문제 해결
author: 안형우
layout: post
permalink: /archives/9481
daumview_id:
  - 40824333
categories:
  - 개발 툴
---
무슨 소리냐 하면, Sass 문법을 Sublime Text 2에서 사용할 수 있는데, 닫는 표시가 없는 CSS 주석을 처리하는 못하고 아래쪽 전부를 주석 처리하는 문제가 있다는 거다. 예컨대 아래처럼 된다는 거다. (캡쳐 뜬 거다.)

<img class="alignnone" alt="" src="/uploads/legacy/sass-comment-example.png" width="561" height="324" />

36행을 보면 주석이 닫혀 있지 않고, 그래서 그 아래쪽은 전부 주석 처리된다는 걸 알 수 있다. `sass-convert`를 이용해서 변환한 CSS는 죄다 저렇게 된다. 즉, 사용자 주석을 닫지 않아도 Sass는 알아서 잘 처리를 하는데, Sublime Text 2의 Sass 확장은 처리를 못하는 거다.

## 해결책 &#8211; 주석을 한 번에 닫아 주자

그래서 찾아 보기도 했는데 뭐 모르겠고, 그냥 실용적인 해결책을 마련했다. 그냥 모든 주석을 닫아 주는 거다. 정규식을 이용하면 간편하다.

단, 여러 줄 주석의 경우 제대로 처리 안 될 수 있으니 수동으로 반드시 확인하길.

일단, Sublime Text 2에서 찾기 바꾸기로 들어간다. (맥은 `Alt+Cmd+F`, 윈도우는 `Ctrl+H`다.)

그리고 정규식을 사용하겠다고 표시한다. (`.*` 라고 씌어 있는 버튼을 누르면 된다. 아래 그림 참고)

<img class="alignnone" alt="" src="/uploads/legacy/sumlime-text-2-regex-find.png" width="347" height="63" />

그리고 찾기 부분에 `/\*(.*)\n`라고 써 주고, 바꾸기 부분에 `/\*$1 \*/\n`라고 써 준다. 그리고 한꺼번에 바꾸기! 그럼 닫기가 없는 모든 주석을 닫아 준다. (아, 그리고 닫기가 있는 주석도 다 닫아 준다. 주의!)

단, 위 정규식은 한 줄짜리 주석만 처리한다. 따라서 여러 줄 주석을 단 경우는 반드시 수동 처리해 줘야 한다. 만약에 **여러줄 주석으로만 주석을 작성한 경우라면 위 정규식을 사용하면 안 된다.**

[2013-03-20 추가] 찾기에 `/\*([^=\n\/\*]+)\n` 이렇게 쓰면 다음 경우를 제외한다.

*   `/*`로 여러 줄 주석을 여는 경우
*   `/* =` 로 시작하는 워드프레스 Twenty Twelve 테마 style.css의 두 줄짜리 주석
*   `*/`로 닫은 주석