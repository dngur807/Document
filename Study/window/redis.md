### window에 Redis 설치

[https://hwigyeom.ntils.com/entry/Windows-%EC%97%90-Redis-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-1](https://hwigyeom.ntils.com/entry/Windows-에-Redis-설치하기-1)



http://lab.gamecodi.com/board/zboard.php?id=GAMECODILAB_Lecture_series&page=1&sn1=&divpage=1&sn=off&ss=on&sc=on&select_arrange=headnum&desc=asc&no=130







```
서비스 목록 확인
윈도우 + R 눌러 services.msc
레디스 실행중 아니면 시작 눌러 서비스 실해시켜 줍니다.
 C:/Program Files/Redis 에서 redis-cli 실행
 
 keys * 눌러보자
```



---



[Redis(레디스)](http://redis.io/)는 고성능 키-값 형식의 저장소로서 문자열, 리스트, 해시, 셋, 정렬된 셋 형식의 데이터를 지원하는 BSD 라이선스 기반의 오픈소스 NoSQL 입니다.

인스턴스 메시지나 게임 관련 데이터처럼 오래 저장할 필요가 없는 단순한 데이터를 빠르게 처리하는데 적합한 데이터 저장소이며, 데이터를 램(RAM)에 저장하고 디스크에 변경 사항을 기록하는 구조를 사용합니다.
메모리를 저장 공간으로 사용하므로 저장 공간에 제약이 있다는 약점이 있지만, 덕분에 데이터 조작을 매우 빠르게 할 수 있다는 장점이 있으며, 데이터는 키-값 형식으로 문자열, 리스트, 해시, 셋, 정렬된 셋 형식을 저장할 수 있습니다.

## Windows 에 Redis 설치하기

Redis 는 공식적으로 Windows를 지원하지 않지만, Microsoft Open Tech 그룹에서 64비트 기반으로 포팅하여 개발 및 유지보수를 진행하고 있습니다.
64 비트만 지원하는 이유는 32 비트는 지원 가능한 메모리가 4G로 제한이 있으므로 메모리에 데이터를 저장하는 인 메모리 데이터베이스에는 부적합하기 때문일 겁니다.

공식 사이트는 https://msopentech.com/opentech-projects/redis/ 이며, Windows 버전 Redis는 MS Open Tech 그룹의 GitHub 저장소(https://github.com/MSOpenTech/redis)에서 관리되고 있습니다.

### Window용 Redis 설치

설치 미디어나 바이너리 파일은 https://github.com/MSOpenTech/redis/releases 에서 다운로드가 가능하니 다운로드 해주십시오.

설치 파일(msi)이나 바이너리 파일이 포함된 압축 파일을 다운로드 하여 원하는 경로에 설치를 진행하시면 됩니다.
설치 파일로 설치할 경우에는 포트 지정, 방화벽 예외 등록, PATH 등록, 서비스 등록 등을 일괄적으로 해주므로 간편하게 설치를 완료할 수 있습니다.

설치가 완료되면 설치 디렉토리는 아래와 같은 구조입니다.

![img](https://t1.daumcdn.net/cfile/tistory/214E3D4356383E3109)

### 테스트 하기

설치 파일로 설치하신 경우 이미 서비스에 등록을 하셨다면 서버 및 클라이언트 동작여부를 테스트를 위해 잠시 서비스를 중지합니다.

실행 창(Windows Key + R)에서
NET STOP Redis
를 입력하여 Redis 서비스를 중지해 주세요.

이제, Redis 설치 경로에서 redis-server.exe 파일을 실행해 보면 아래와 같이 Redis 서버가 실행되는 것을 확인할 수 있습니다.
Redis 서버가 서비스로 등록되어 실행되고 있지 않다면, Redis를 이용하고자 할 때는 항상 redis-server.exe가 실행되고 있어야 합니다.
redis-server.exe 파일을 실행하면 아래와 같은 화면을 볼 수 있습니다.
기본으로 사용하는 포트와 프로세스 아이디 현재 동작방식(stand alone mode)등을 알 수 있으며 현재 사용하도록 지정된 설정파일이 없어 기본 설정을 이용한다는 것을 알 수 있습니다.

![img](D:\Com2us\Com2usDocument\Server\Game\Fishing\image\22553C4456383E3212)

설정파일을 지정하여 redis-server를 실행하고자 한다면
redis-server redis.windows.conf
와 같이 설정 파일의 경로를 지정해주시면 됩니다.

클라이언트를 이용하여 서버로 접근을 해보려면 서버가 실행되고 있는 상태에서 redis-cli.exe 파일을 실행하시면 됩니다.

![img](D:\Com2us\Com2usDocument\Server\Game\Fishing\image\212A804856383E3212)

redis-cli 를 아무런 옵션을 주지 않고 실행하면 로컬(127.0.0.1) 서버에 기본 포트인 6379 포트를 이용하여 접속하게 됩니다.
ping 을 입력하면 Redis 서버는 PONG 을 반환합니다.

원격 서버에 접속하거나 기본 포트(6379)가 아닌 포트를 이용하는 경우 아래 명령을 이용하여 접속할 수 있습니다.
redis-cli –h host –p port –p password

![img](D:\Com2us\Com2usDocument\Server\Game\Fishing\image\2652F94356383E3416)

간단하게 set 명령어를 이용하여 name 이라는 키에 값으로 문자열을 넣어봤습니다.
문자열 중간에 공백 문자가 있으므로 겹 따옴표로 문자열을 감싸주었습니다. 공백 문자가 없을 경우에는 겹 따옴표는 생략해도 됩니다.
그리고, get 명령어로 name 이라는 키의 값을 가져옵니다.
Redis 에서 실행할 수 있는 명령어의 목록은 http://redis.io/commands 에서 확인할 수 있습니다.

이렇게 Redis 서버가 정상적으로 동작하는 것을 확인하였습니다.

설치 파일로 설치하여 이미 서비스에 등록되어 있다면 중지했던 서비스를 새로 시작하기 위해서 실행 중인 redis-server창을 종료하신 후에 실행 창에서
NET START Redis
를 실행해 주시기 바랍니다.

 

### Redis Windows 서비스로 등록하기

바이너리 압축파일을 다운로드 받았을 경우 서비스로 등록하여 사용하고자 하신다면 아래와 같이 해주시면 됩니다.

우선, Redis를 설치한 경로에서 명령 프롬프트 창을 열어 주신 후
redis-server --service-install redis.windows.conf --loglevel notice
를 입력하여 서비스로 등록할 수 있습니다.

등록된 서비스를 시작/중지할 때는
redis-server --service-start
redis-server --service-stop
를 이용하시면 되며,

서비스 등록을 해제할 때는
redis-server --service-uninstall
로 등록을 취소할 수 있습니다.

등록을 해제하실 때는 자동으로 서비스가 중지되진 않으므로, 등록해제 전 이나 후에 --service-stop 으로 서비스를 중지해주시면 됩니다.

아래 결과를 보시면 --service-install 옵션을 이용하여 redis.windows.conf 설정파일을 이용하도록 설정하여 redis-server를 윈도우 서비스로 등록하였으며, 서비스 실행에 이용되는 계정은 NetworkService 임을 확인하실 수 있습니다.

![img](https://t1.daumcdn.net/cfile/tistory/2106394956383E3517)

![img](D:\Com2us\Com2usDocument\Server\Game\Fishing\image\2714E44B56383E3606)

서비스로 등록 후에 서비스 창에 Redis 라는 이름으로 등록이 되었음을 확인 할 수 있습니다.

서비스 이름을 변경하여 등록하고자 할 때는
redis-server --service-install -service-name MyRedisServer
와 같이 서비스 명을 지정할 수 있습니다.

포트까지 변경하고자 한다면
redis-server --service-install -service-name MyRedisServer -port 10001
이름을 지정한 서비스를 시작할 때는
redis-server –service-start -service-name MyRedisServer
와 같이 서비스 이름을 지정해주시면 됩니다.
서비스 중지 및 해제도 -service-name 옵션을 이용하시면 됩니다.

위와 같은 방식으로 하나의 서버에 여러 개의 이름이 다른 Redis 인스턴스를 등록하여 실행하실 수도 있습니다.

여기까지 Windows에서 Redis 를 설치하는 방법에 대해여 알아봤습니다.
Redis 활용에 대한 자세한 내용은 다음 포스트에서 찾아 뵙겠습니다.



