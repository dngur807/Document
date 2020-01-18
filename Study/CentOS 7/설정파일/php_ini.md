### php.ini

php 설정 파일 phpinfo(); 로 위치 확인 가능

세션 설정 ( [https://unabated.tistory.com/entry/%EC%84%B8%EC%85%98-%EC%84%A4%EC%A0%95-phpini](https://unabated.tistory.com/entry/세션-설정-phpini) )

php.ini

```
;session.save_handler = files
session.save_handler = memcache
session.save_path = "127.0.0.1:11211"
```



- session.save_handler = 기본적으로 files 방식 이용. 

  session_module_name()을 통해 현재 설정된 정보를 볼 수 있다. 공유 메모리를 사용하는 MM방식 과 User 방식을 지원

- session.save_path : 세션 파일을 저장할 경로를 의미하고 files 방식에서는 /tmp가 기본적인 파일 저장되는 디렉토리의 절대 경로입니다.

  

