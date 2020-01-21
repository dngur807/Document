### PHP ob_start()

> https://zetawiki.com/wiki/PHP_ob_start()
>
> [https://blog.habonyphp.com/entry/php-%EC%B6%9C%EB%A0%A5-%EB%B2%84%ED%8D%BC-flush-%ED%95%A8%EC%88%98](https://blog.habonyphp.com/entry/php-출력-버퍼-flush-함수)

### 개념

* output buffering start
* 출력 버퍼링을 켬
* 출력 버퍼링이 켜져 있는 동안 헤더를 제외한 스크립트의 모든 출력을 내부 버퍼에 저장하며 실제 전송하지 않음
* ob_start를 여러번 호출하여 중첩이 가능

```
while(ob_end_flush()); // Stop all ob_start()

ob_start();
for ($i = 0; $i < 10; $i++) {
	echo $i;
	echo str_pad("", 4096);
	sleep(1);
	ob_flush();
	flush();
}
ob_end_flush();
```

```
출력 버퍼링은 출력물이 php에 파싱되어 echo나 print 에 의해 브라우저로 출력되는 방법이 일반적인데, 필요에 의해 출력 결과물을 바로 브라우저로 보내지 않고, 내용물을 잠깐 동안 버퍼에 보관해 두었다가 출력이 필요할 곳에 이를 사용할 수 있다.

이를 사용하기 위애서는 ob_start()함수를 호출하고, ob_end_flush로 버퍼링을 비워주면 되는데, 여기서 비운다는 의미는 버퍼에 저장되어 있는 내용을 브라우저로 출력하고, 버퍼를 비운다는 뜻입니다.
```

```

 <?php
 // 출력 버퍼링을 켭니다.
 ob_start();

 // 출력 결과물을 호출합니다.
 test();

 // 버퍼의 내용을 브라우저로 출력하고, 버퍼링을 비워 줍니다.
 // 그리고 출력 버퍼를 종료합니다.
 ob_end_flush();

 function test(){
      echo "php 하보니입니다.";
 }

 // 결과: php 하보니입니다.
```





### PHP 실시간 버퍼링 없이 브라우저 출력

> [https://zetawiki.com/wiki/PHP_%EC%8B%A4%EC%8B%9C%EA%B0%84_%EB%B2%84%ED%8D%BC%EB%A7%81_%EC%97%86%EC%9D%B4_%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80_%EC%B6%9C%EB%A0%A5](https://zetawiki.com/wiki/PHP_실시간_버퍼링_없이_브라우저_출력)

* 출력 과정
  * 프로그램 출력 -> PHP -> Web Server ->Browser
    * echo 를 통해 프로그램에서 출력을 하게되면 브라우저에서 곧장 출력되기를 기대
    * 하지만 버퍼링 등의 문제로 기대와는 달리 곧장 출력 되지 않음

* 방법

  * PHP 버퍼 비활성화
  * 아파치 버퍼 비활성화
  * 캐쉬 비활성화

  ```
  // Turn off output buffering
  ini_set('output_buffering', 'off');
  // Turn off PHP output compression
  ini_set('zlib.output_compression', false);
  
  //Flush (send) the output buffer and turn off output buffering
  while (@ob_end_flush());
  
  // Implicitly flush the buffer(s)
  ini_set('implicit_flush', true);
  ob_implicit_flush(true);
  
  //prevent apache from buffering it for deflate/gzip
  header("Content-type: text/plain");
  // recommended to prevent caching of event data.
  header('Cache-Control: no-cache');
  
  ob_start();
  for ($i = 0; $i < 100; $i++) {
  	echo $i;
  	echo str_pad("", 4096);
  	sleep(1);
  	ob_flush();
  	flush();
  }
  ob_end_flush();
  ```

  