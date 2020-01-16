### where 키워드

>  https://m.blog.naver.com/PostView.nhn?blogId=beaqon&logNo=221301092125&proxyReferer=https%3A%2F%2Fwww.google.com%2F 

일반화 프로그래밍에서 형식매개 변수 T에 제약을 걸 수 있는 방법이 있다.

where 키워드를 사용하는 것이다. where T : 클래스 이름 으로 설정을 해버리면 그 클래스의 하위 클래스만 T로 사용할 수 있게 제약이 걸린다.



> where 키워드 사용

```
class MyList<T> where T : MyClass{}
```

일반화 클래스를 선언할 때 앞에 where T 를 적은 후 제한이 될 클래스 이름을 적어주면 된다.

위 코드의 경우 MyClass 를 상속하는 타입만 T의 자리에 들어올수 있습니다.



> 제약에 따른 다양한 형식

![image-20200115170704241](../../\image\image-20200115170704241.png)