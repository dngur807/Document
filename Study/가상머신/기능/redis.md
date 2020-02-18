### redis 트랜잭션

> 출처 https://rocksea.tistory.com/319

**참고사이트**

\- http://redis.io/topics/transactions#cas

\- http://dol9.tistory.com/209



Redis Transaction 사용법

```
MULTI : Transaction 시작 선언

DISCARD : Transaction 취소 (Rollback과 다름. 자세한 내용은 참고사이트 참조) 

EXEC : Transaction Commit

WATCH : 특정 Key 변경 감시

UNWATCH : 모든 WATCH 취소
```



  Senario 1. MULTI + EXEC을 이용한 Transaction 실행.

```
127.0.0.1:6379[1]> MULTI
OK
127.0.0.1:6379[1]> SET rocksea 1
QUEUED
127.0.0.1:6379[1]> INCR rocksea
QUEUED
127.0.0.1:6379[1]> INCR rocksea
QUEUED
127.0.0.1:6379[1]> EXEC
1) OK
2) (integer) 2
3) (integer) 3
127.0.0.1:6379[1]> GET rocksea
"3"
변수를 선언하고 값을 증가시킨 뒤 EXEC을 이용하여 Commit하였더니
값이 증가하였음을 확인 할 수 있었다.
```

  Senario 2. MULTI + DISCARD 을 이용한 Transaction 취소.

```
127.0.0.1:6379[1]> SET rocksea 1
OK
127.0.0.1:6379[1]> MULTI
OK
127.0.0.1:6379[1]> INCR rocksea
QUEUED
127.0.0.1:6379[1]> INCR rocksea
QUEUED
127.0.0.1:6379[1]> DISCARD
OK
127.0.0.1:6379[1]> get rocksea
"1"
127.0.0.1:6379[1]>

마찬가지로 변수를 선언하고 값을 증가시킨 뒤 DISCARD를 이용하여 취소하였더니
증가했던 Command가 취소되었음을 확인 할 수 있었다.
```

**Senario 3. WATCH를 이용한  Optimistic Locking.**

Optimistic Locking은 일반적인 RDBMS의 Locking과는 다르다.

보통의 RDBMS의 경우 Transaction을 잡고있으면 Locking이 되어 버리는 반면

Redis의 Locking은 Sequential 하게 요청을 수행하도록 보장해 주는 역할을 한다.  

```
127.0.0.1:6379[1]> WATCH rocksea
OK
127.0.0.1:6379[1]> MULTI
OK
127.0.0.1:6379[1]> INCR rocksea
QUEUED
127.0.0.1:6379[1]> EXEC
1) (integer) 6
rocksea key값이 변하지 않았기 때문에 성공적으로 Transaction의 Sequence를 보장한다.
```



**실행 실패의 예**

```
127.0.0.1:6379[1]> WATCH rocksea
OK
127.0.0.1:6379[1]> INCR rocksea
(integer) 7
127.0.0.1:6379[1]> MULTI
OK
127.0.0.1:6379[1]> INCR rocksea
QUEUED
127.0.0.1:6379[1]> EXEC
(nil)
이미 WATCH를 시작 후 값이 변경 된 후라 Transaction을 적용하더라도 요청은 실패한다.
그 이유는 WATCH 시 Redis 내부에서 REDIS_DIRTY_CAS ( Check And Set ) 값을 
체크하여 commit or rollback의 여부를 판단하기 때문이다. 이는 다른 Client에서 값을 
변경하여도 동일하게 작용한다.
```





**Client A**

```
127.0.0.1:6379[1]> WATCH rocksea
OK
127.0.0.1:6379[1]> MULTI
OK
```

**Client B**

```
127.0.0.1:6379[1]> INCR rocksea
(integer) 12
```

**Client A**

```
127.0.0.1:6379[1]> INCR rocksea
QUEUED
127.0.0.1:6379[1]> EXEC
(nil)
127.0.0.1:6379[1]> get rocksea
"12"
```

```
일반적인 RDBMS였다면 ACID를 보장하기 위해 Client B에서 접근 시 Lock이 걸렸겠지만 

그렇지 않고 변수값을 변경 후 바로 적용되었다. 오히려 Transaction의 주체인 Client A에서

Discard되어 요청이 취소되었음을 확인 할 수 있었다. 이렇듯 WATCH는 일반적인 

RDBMS와 다르게 Key값의 변화에 따라 Sequential한 요청을 보장하는 장치로써 

동작하게 된다.

상황에 따라 Sequence를 보장해야 되는 경우 유용하게 사용 할 수 있을듯 하다.
```









### Hashes 관련 명령어

> 출처 https://realmojo.tistory.com/172

HSET 하나의 객체에 여러 개의 변수를 담을 수 있는 구조 하고 생각하자

* hset : key에 저장된 해시 필드를 설정합니다.
* hget : key 필ㄷ즈에 저장된 값을 불러온다.
* 시간복잡고 O(1)

