---
title: 'loop 안에서 fopen, fclose를 매번 반복하면 얼마나 비효율적일까?'
layout: post
author: 안형우
tags: 
  - php
  - tip
description: '2배 느리지만 대략 10만 분의 2초 차이라서 억 단위 데이터가 아니라면 신경쓰지 않아도 될 듯하다.'
---

대량으로 데이터를 동기화하는 스크립트를 작성할 일이 있었다. 맨 처음에 약 35만 건 정도 되는 데이터를 동기화하고, 이후부턴 매일매일 동기화를 한다. 

클래스를 다 작성하고 보니 내가 루프 안에서 `fopen`과 `fclose`를 하게 만들어 놨다는 것을 발견했다. 아무래도 파일을 열고 닫는 걸 35만 번 반복하면 시간이 상당히 소요되지 않을까? 해당 코드를 루프 밖으로 빼내려면 코드를 상당히 많이 고쳐야 했기 때문에, 간단한 테스트 스크립트를 작성해 봤다.

아래는 우선 루프 밖에서 파일을 열고, 루프 안에서는 쓰기만 한 다음 다시 루프 밖에서 파일을 닫는 테스트 스크립트다. 루프는 35만 회를 돌렸다.

```php
// test1.php
$start = microtime(true);
$handle = fopen('runtime_test.txt', 'a');
for ($i = 0; $i < 350000; $i++) { 
	$random = rand();
	fwrite($handle, date("c") . " [info] [userid=mytory] new user=$random\n");
}
fclose($handle);
echo microtime(true) - $start . 's' . PHP_EOL;
```

아래는 루프 안에서 파일을 열고 닫는 스크립트다.

```php
// test2.php
$start = microtime(true);
for ($i = 0; $i < 350000; $i++) {
        $handle = fopen('runtime_test.txt', 'a');
        $random = rand();
        fwrite($handle, date("c") . " [info] [userid=mytory] new user=$random\n");
        fclose($handle);
}
echo microtime(true) - $start . 's' . PHP_EOL;
```

터미널에서 실행한 결과 test1.php는 7.8초, test2는 15.9초가 소요됐다. 각각 로그 1줄을 쓰는 데 100만 분의 22초, 100만 분의 45초가 소요된 것이다. 2배 차이이므로 억 단위가 되면 시간을 상당히 잡아먹겠지만, 35만 건 수준에서는 고작 8.1초 차이밖에 나지 않으므로 코드는 그냥 두기로 했다.

테스트한 컴퓨터의 저장매체는 ssd였고, CPU는 i5-4210U 1.70GHz다. 요새 웬만한 서버는 ssd니까 굳이 hdd 테스트를 할 필요는 없어 보인다.

또한, 저장 매체의 하드웨어적 특성까지 고려한 테스트는 아니라는 점을 염두에 두자. 오랜 시간 동안으로 친다면 수억 번 `fopen`, `fclose`를 반복하게 될 텐데, 그것이 하드웨어에 무리를 줄지 안 줄지 나는 잘 모른다. 이번 경우는 일회성이기 때문에 이 정도 테스트로 만족했다.
