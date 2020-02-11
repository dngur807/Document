### Memcache 설치 및 설정

>  https://www.solanara.net/solanara/memcached 

memcache는 이름 그대로 메모리를 사용해 "캐시" 서비스를 제공해주는 데몬이다.

memcached의 기능은 단순히 키 - 값 쌍으로 메모리에 저장하고 가져오는 기능이 전부지만,

내부적으로 모든 커맨드는 O(1)에 수렴하는 수행시간을 가진다.  또 커스터마이징된 슬랩(Slab) 메모리 할당 알고리즘을 사용해 메모리 관리 효율성을 높여 가동시간이 늘어나도 성능 저하나 메모리 단편화가 적다.

* Memcached 를 사용해 캐시를 구현할 때 지양해야할 점
  * 스토리지에 저장된 파일의 캐시로 사용하지 말아야한다. 파일에 대한 캐시는 OS에서 제공되는 스토리지 캐시를 사용하는 것이 좋다, 만약 memcached에서 캐시하려면 OS의 파일 캐시를 끄는 것도 고려해야 한다. memcached는 파일을 읽고 가공한 것을 캐시하는 것이 좋다고 생각
  * DB 쿼리 결과의 캐시로 사용하지 말아야 한다. 파일 캐시의 경우와 비슷하다 요즘 나오는 DBMS는 쿼리 결과를 캐시하기 때문에 이중으로 캐시할 필요 없다. 쿼리 결과를 가공한 것을 캐시하는 것이 좋다.

* memcached 운용시에는 다음과 같은 것을 주의 운용
  * 시스템 메모리 상황을 상시 몰니터링 해 , 스왑 락이 일어나지 않도록 해야한다. memcached는 스왑이 일어나지 않을 정도로 메모리는 여유있다는 가정하에 개발되기 때문이다. 메모리 부족해 스왑이 일어나 메모리에 저장했던 컨텐츠가 스왑 디스크에 저장되면, 심각한 속도 저하가 있을 것이다.
  * memcached의 히트 / 미스 비율을 상시 모니터링 하자. 당연히 캐시 히트 비율이 높을 수록 좋은 서비스 제공할 수 있다. 히트 / 미스 비율이 낮다면 캐시할 대상을 변경해보는 것도 고려하자, 구체적인 수치는 서비스마다 다르지만 99%이상은 되어야 만족할 만한 성능 향상이 나오지 않을 까 생각한다. 



> 캐시 처리
>
>  성능 향상을 위해 캐시 기법을 사용하는 경우, 캐시에 데이터가 없으면 원본을 찾아 가져와야 서비스를 진행해야 한다. 캐시에 캐시키에 해당하는 데이터가 없다고 해서, 데이터 없음으로 간주하고 서비스하면 안된다는 의미다. 캐시는 어디까지나 성능에만 영향을 주도록 설계되어야 한다. 또한 memcached 처리 중에 오류가 발생해도, 서비스 프로세스는 그대로 진행되도록 서비스를 구성하고 프로그래밍해야 한다. 예를 들어 memcached 가 다운되었어도 서비스는 (비록 성능이 저하되었더라도) 계속 진행되어야 한다. 



**설치**

php 

```
yum --enablerepo=remi-php72 php-pecl-memcache php-pecl-redis
yum -y --enablerepo=remi install redis
yum -y install memcached

systemctl restart memcached
```



**PHP**

---

php 에서 memcached를 사용하기 위한 모듈은 두가지를 제공합니다,.

PECL:memcache , PECL:memcached 이 두가지 모두 memcached 에 접속해 사용한다.

단지 PECL:memcache는 memcached에 직접 접속하는 PHP 모듈이고 

PECL:memcached는 libMemcached를 통해 memcached에 접속하는 모듈이라는 점이 다르다.,



* PECL:memcache 설정

  php.ini 를 수정해 memcache.so 모듈을 로드해야한다.

  ```
  [memcached]
  extension=memcached.so
  ```

  ![image-20200118215327026](../../\image\image-20200115235856242.png)

* 세션 핸들러 변경

  PHP 의 세션 핸들러로 memcached 를 설정할 수 있으며, 저장 경로로 두개 이상의 memcached 데몬을 사용할 수 있다. php 세션 저장을 위해 사용하는 memecached의 키값 은 memc.sess.key 형식으로 정해진다.,

  ​	

  php.ini 파일에 session 생성을 memcache를 쓰도록 한다.

  ```
  /etc/php.ini 파일에서
  ;session.save_handler = files
  session.save_handler = "memcache"
  session.save_path = "127.0.0.1:11211"
  ```

  ![image-20200118220036285](../../\image\image-20200118220036285.png)

```
설정 변경후 
systemctl restart php-fpm
systemctl restart nginx
```



* memcached 관리자

  memcached의 상태를 볼 수 있다. php 로 작성되어 있다. phpmemcacheadmin

  ```
  https://github.com/clickalicious/phpmemadmin
  설치 후 
composer update
  phpmemadmin/app 에 .config.dist 있는데
  .config로 수정
  
  cp .config.dist .config
  
  
  ```
  
  

실행시 주의사항

```
http://localhost(nginx 서버네임)/phpmemadmin/web/index.php?action=1&host=127.0.0.1:11211 

vi /etc/nginx/conf.d/default.conf  servername으로 설정되있음...

```

![image-20200121000209832](../../\image\image-20200121000209832.png)

 

![image-20200121000232203](../../\image\image-20200121000232203.png)









