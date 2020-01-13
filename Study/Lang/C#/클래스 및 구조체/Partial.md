### Partial 클래스 및 메서드

>  참고 https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods 
>
>  https://storycompiler.tistory.com/215 
>
> 

### Partial 클래스

partial class 에 대해 탐구해보도록 하겠습니다.

정의할 때 한군데가 아닌 복수의 장소에서 class 를 정의할 수 있도록 지원

```
partial class Korea { public int south; } 
partial class Korea { protected int north; }
```



- Code Generator 자동 생성

  윈도우 폼을 생성할 때 

  Code generator는 partical class를 Form.cs 과 Form1.Designer.cs 파일에 나눠 정의

  Code generator는 클래스 내부에서 개발자가 손 댈 필요없는 부분ㅇ르 떼어 Form1.Designer.cs에 넣고 개발자 손길 필요한 부분만 Form1.cs에 놓아 코드에 집중하도록 지원

- 다수의 개발자의 개발

  다수의 개발자가 한 클래스를 개발하는 경우 

  개발자들은 단위 기능으로 역할 분담할 것

  이 경우 partial class를 사용하여 여러 파일에 개발하면

  한 파일에서 작업할 때 발생할 수 있는 conflict를 막을 수 있다.

- partial method 사용

  다수의 개발자 한 클래스를 개발할 때,

  partial class 의 한 부분에서는 함수선언만 하고, 다른 부분에서는 함수 정의

  partial class의 한 부분이 추상클래스이고 , 다른 부분이 구현 클래스

  만약 partial method가 정의되지 않았다면,

  컴파일 이전에 선언된 method를 제외하고 컴파일을 하게 됩니다.



