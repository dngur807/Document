### Attribute 활용

>  https://yongho1037.tistory.com/518 

애트리뷰트는 코드에 대한 부가 정보를 기록하고 읽을 수 있는 기능

주석은 사람이 읽고 쓰는 정보라고 하면, 

애트리뷰트는 사람이 작성하고 컴퓨터가 읽는다는 것입니다.

```
예를 들면 C# 라이브러리를 사용하는데, 해당 라이브러리에 문제가 발생해서,
수정을 했는데, 수정전 버전을 쓰는 사람한테 알려줄 때 사용 경고 메세지를 보내서
```



### 사용법

```
[애트리뷰트_이름(애트리뷰트 매개 변수)]
public void MyMethod()
{}

class MyClass
{
	[Obsolete("OldMethod는 폐기되었습니다. NewMethod() 를 이용하세요.")]
	public void OldMethod()
	{}
}
```

