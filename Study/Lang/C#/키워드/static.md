### static

>  https://qzqz.tistory.com/117 

static 은 메서드나 필드가 클래스의 인스턴스가 아닌 클래스 자체에 소속되도록 합니다.



static으로 선언된 필드는 프로그램 전체에 걸쳐 하나만 존재가 가능합니다.

프로그램 전처에 걸쳐 공유해야할 변수가 있다면 static 필드를 사용하면 됩니다.

```
public class MyClass
{
   private int val = 1;
   
   // 인스턴스 메서드
   public int InstRun()
   {
      return val;
   }
   
   // 정적(Static) 메서드
   public static int Run() 
   {
      return 1;
   }
}

public class Client
{
   public void Test()
   {
      // 인스턴스 메서드 호출
      MyClass myClass = new MyClass();
      int i = myClass.InstRun();

      // 정적 메서드 호출
      int j = MyClass.Run();
   }
}
```

