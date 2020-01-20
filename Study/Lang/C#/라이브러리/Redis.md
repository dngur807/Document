#### C# Redis 사용

> 참고  http://egloos.zum.com/sweeper/v/3157497 

NuGet 패키지 관리자

```
StackExchange.Redis 설치
```

```
// ConnectionMultiplexer는 클러스터 구성 환경을 수용한다.
ConnectionMultiplexer redis =
                   ConnectionMultiplexer.Connect(redisServerIP + ",allowAdmin=true,password=...");
                   
IDatabase db = redis.GetDatabase();
                     
```



불편한점

StackExchange.Redis는 모든 오브젝트에 대해 byte[] 로 만든 뒤 읽고 ㅅ쓴다.

그래서 일일이 serialize / deserialize 를 해줘야한다.

귀찮기도하고 JSON serializer 라이브러리를 별도로 유지해야 한다는 부담이 있다.

이건을 개선한것이

  [StackExchange.Redis.Extensions](http://sweeper.egloos.com/3157731)  이다... 나중에 살펴보자

