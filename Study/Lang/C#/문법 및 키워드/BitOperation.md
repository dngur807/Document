### 시프트 연산자

> 출처: https://godffs.tistory.com/86 [.Net 개발자]

시프트 연산자는 값을 진수로 반환하고 비트의 자리수를 옮겨 값을 변경하는 것입니다.

``` 
using System;

public class 시프트연산자
{
    public static void Main()
    {
        int num = 2;
        int result = 0;

        num = 2; //4배
        result = num << 2; // 왼쪽으로 비트를 2칸 이동

        Console.WriteLine("{0}",result);

        num = 40;
        result = num >> 2; // 오른쪽으로 비트를 2칸 이동
        Console.WriteLine("{0}", result);
    }
}
```

![img](../../\image\195DC0154A786A0D01)

그림 11-1은 int num의 값 2를 비트 단위에서 자리 두자리를 바꾸어 4의 자리로
이동하고 다음 마지막 두번재에는 8번 자리로 이동하게 됩니다.
결과값이 8 나오는 것을 확인 할 수 있습니다

![image-20200123112539820](../../image\image-20200123112539820.png)