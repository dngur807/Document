### RecursiveIteratorIterator

> https://www.php.net/manual/en/class.recursiveiteratoriterator.php
>
> [https://gracefullight.dev/2019/04/12/RecursiveDirectoryIterator-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0/](https://gracefullight.dev/2019/04/12/RecursiveDirectoryIterator-사용하기/)

디렉토리 순회하고 싶을 때 사용합니다.



디렉토리 순회를 한다고하면 무슨 매소드를 쓸까??

```
// 쉘실행하고?
exec('find dir');

// 아니면 for문과 scandir?
scandir('dir');

위 방법은 간단하지면 /home/gracefullight/tmp/**/*.bak 와 같은 중첩된 디렉토리 파일의 데이터를 가져오려면 엄청난 if/else 처리가 들어갈 것이다.
```



### RecursiveDirectoryIterator

오토로딩을 하기 위해 필수로 들어가 있는 Standard PHP Library엔 파일 순회에 사용할 수 있는 이터레이터 클래스가 들어가 있다.

```
// $path 하위를 가져오고 .. 와 . 는 제외한다.
$directory = new RecursiveDirectoryIterator($path, RecursiveDirectoryIterator::SKIP_DOTS);
$iterator = new RecursiveIteratorIterator($directory);

foreach ($iterator as $file) {
    $file->getPathname();
    $file->getMTime();
}
```

`$file`은 [SplFileInfo](https://www.php.net/manual/en/class.splfileinfo.php) 이다.