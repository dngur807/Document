### APCU

apcu 관련 기능 사용하려면 

composer에 추가 해야합니다.

```
// 추가 후
"require" : {
	 "ext-apcu": "*"
}
composer update
```

composer update 시 에러 발생

```
Your requirements could not be resolved to an installable set of packages.

  Problem 1
    - The requested PHP extension ext-apcu * is missing from your system. Install or enable PHP's apcu extension.
    
원인 : composer는 의존성 관리도구 이지 설치해주지 않는다.
설치해주고 다시 진행해보자!!
php -m | grep apcu 로 설치되어 있는지 확인해보자
```



