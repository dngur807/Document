### Task

> 출처 http://www.csharpstudy.com/Threads/task.aspx

Task 클래스는 .Net 4.0 에 도입됐다. 쓰레드 풀로부터 쓰레드를 가져와 비동기 작업을 실행합니다. 

Task 관련 클래스들과 Parallel  클래스를 합쳐 Task Parallel Library(TPL) 이라 부르는데, 이들은 기본적으로 다중 CPU 병렬처리를 염두에 두고 만들었다. 



```csharp
예제
namespace MultiThrdApp
{
    using System;    
    using System.Threading.Tasks;    

    class Program
    {
        static void Main(string[] args)
        {
            // Task.Factory를 이용하여 쓰레드 생성과 시작
            Task.Factory.StartNew(new Action<object>(Run), null);
            Task.Factory.StartNew(new Action<object>(Run), "1st");
            Task.Factory.StartNew(Run, "2nd");

            Console.Read();
        }

        static void Run(object data)
        {            
            Console.WriteLine(data == null ? "NULL" : data);
        }
    }
}

```

---

