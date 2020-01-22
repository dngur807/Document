### PHP 클래스 자동으로 불러오기 (Autoloading)

> 출처 https://edykim.com/ko/post/autoloading-php-classes/

 객체 징향 프로그래밍에 익숙한 개발자라면 하나의 파일에 하나의 클래스를 작성하는 방식에 익숙하다!

다만 php는 다른 언어와 같이 라이브러리를 일괄적으로 불러오는 방법이 없어 위와 같은 접근 방법으로는 require 또는 include를 이용해 수많은 단일 파일을 불러들여야만 했었다.



 PHP5에서는 클래스 또는 인터페이스 등을 호출했을 때 해당 파일을 자동으로 불러올 수 있도록 여러 함수를 제공한다 . 먼저 `__autoload` 함수를 이용한 예제

```
<?php
function __autoload($className){
  include $className . '.php';
}

$foo = new Foo();
$bar = new Bar();
?>
```

위와 같이 함수를 선언하면 new Foo() 와 같이 클래스를 사용하는 순간 해당 클래스명으로 __autoload 함수가 실행, Foo.php 파일을 include 합니다.

다만 __autoload 함수를 spl_autoload_register 함수를 통해 대체될 수 있기 때문에 권장되지 않는다. spl_autoload_register는 다음과 같이 사용한다.

```\
<?php
function my_autoloader($className){
  include 'classes/' . $className . '.class.php';
}

spl_autoload_register('my_autoloader');
?>
```

