### APCU

>  https://www.mynotepaper.com/install-apcu-on-centos-7 



![image-20200115235856242](../../\image\image-20200115235856242.png)

phpinfo() 로 설치 확인



### APCu 관리 페이지

 이것은 선택적 단계입니다. PHP 파일을 공유하고 있습니다. APCu 관리 페이지를 보려면이 파일을 도메인의 공용 html 폴더에 보관해야합니다. [GitHub](https://gist.github.com/mdobydullah/2bc1cca5824eae59117a5a93ed947974) 에서이 파일을 다운로드하십시오 . 



![image-20200116000213383](../../\image\image-20200116000213383.png)





### APCu 구성

 APCu를 쉽게 구성 할 수 있습니다. `/etc/php.ini`파일을 열고 다음 과 같은 값을 추가하십시오. 

```
#Enable/Disable
apc.enabled=1
# Memory Segments
apc.shm_size=512M
## PHP file cache 1 hour ##
apc.ttl=3600
## User cache 2 hour ##
apc.user_ttl=7200
## Garbage collection 1 hour ##
apc.gc_ttl=3600

설정 후 웹 서버 다시 시작
```

