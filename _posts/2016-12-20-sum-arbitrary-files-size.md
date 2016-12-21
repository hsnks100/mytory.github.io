---
title: '[bash] 특정 파일들의 용량 합산'
author: 안형우
layout: post
tags:
  - shell
---

리눅스 터미널에서는 파이프 기능을 이용해서 다양한 짓을 할 수 있다. `|`(파이프 명령어)는 앞선 명령어의 표준출력[^fn1]을 뒤의 명령어의 인자값으로 넘겨 주는 역할을 한다.

이걸 이용해서 css 파일들의 용량만 합산하고 싶다면 아래처럼 사용한다.

    ls -al *.css | tr -s ' ' | cut -d ' ' -f 5 | sed 's/^/.+/' | bc | tail -1

1. `tr`은 글자를 바꿔주는 프로그램이다. `-s`는 반복되는 글자를 한 글자로 줄여 준다. 그래서 띄어쓰기 여러 개 된 것을 하나로 줄여 주는 역할을 하는 것이다.
2. `cut`은 라인에서 특정 섹션을 지워 주는 역할을 한다. `-d`는 delimiter의 약자로 구분자를 지정해 주는 것이고, `-f`는 field의 약자로 특정 필드만 남기라는 뜻이다. 숫자를 적어 준다.
3. `sed`는 '텍스트를 필터링하고 변경하는 스트림 에디터(stream editor for filtering and transforming text)'인데, 정규식을 바탕으로 문자를 치환할 때 사용한다. 나는 보통 대용량 DB 파일에서 문자를 치환할 때 사용했는데, 여기서는 모든 숫자들 앞에 `.+` 문자를 붙여 주는 용도로 사용한다.
4. `bc`는 '임의의 정확도 계산 언어(An arbitrary precision calculator language)'인데, 그냥 계산기로 쓴다. `bc`를 실행하고 `.+10` 엔터, `.+20` 엔터 해 봐라. 그러면 숫자가 누적해서 더해지는 것을 볼 수 있을 것이다.
5. `tail`은 표준출력을 맨 마지막에서 몇 줄 잘라 주는 역할을 하는 프로그램이다. `-숫자`를 붙이면 해당 숫자만큼 잘라서 보여 준다. `-1`을 붙인 건 마지막 한 줄만 필요하기 때문이다.

물론 탐색기에서 파일을 찍으면 상태 표시줄에 합산한 용량이 나온다. 이건 그냥 bash 공부라고 생각하자.

[^fn1]: 대충 말하자면, 에러 말고 제대로 나오는 출력. 표준 입력, 표준 출력, 표준 에러 세 가지 표준 스트림이 있다.