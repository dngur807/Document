### Interlocked 클래스

> 출처 https://docs.microsoft.com/ko-kr/dotnet/api/system.threading.interlocked?view=netframework-4.8

다중 스레드에서 공유하는 변수에 대한 원자 단위 연산을 제공

```
public static class Interlocked
```

`메소드`

 Exchange : 원래 값을 반환하고 지정된 값으로 설정





예제

```csharp
using System;
using System.Threading;

namespace InterlockedExchange_Example
{
    class MyInterlockedExchangeExampleClass
    {
        //0 for false, 1 for true.
        private static int usingResource = 0;

        private const int numThreadIterations = 5;
        private const int numThreads = 10;

        static void Main()
        {
            Thread myThread;
            Random rnd = new Random();

            for(int i = 0; i < numThreads; i++)
            {
                myThread = new Thread(new ThreadStart(MyThreadProc));
                myThread.Name = String.Format("Thread{0}", i + 1);
            
                //Wait a random amount of time before starting next thread.
                Thread.Sleep(rnd.Next(0, 1000));
                myThread.Start();
            }
        }

        private static void MyThreadProc()
        {
            for(int i = 0; i < numThreadIterations; i++)
            {
                UseResource();
            
                //Wait 1 second before next attempt.
                Thread.Sleep(1000);
            }
        }

        //A simple method that denies reentrancy.
        static bool UseResource()
        {
            //0 indicates that the method is not in use.
            if(0 == Interlocked.Exchange(ref usingResource, 1))
            {
                Console.WriteLine("{0} acquired the lock", Thread.CurrentThread.Name);
            
                //Code to access a resource that is not thread safe would go here.
            
                //Simulate some work
                Thread.Sleep(500);

                Console.WriteLine("{0} exiting lock", Thread.CurrentThread.Name);
            
                //Release the lock
                Interlocked.Exchange(ref usingResource, 0);
                return true;
            }
            else
            {
                Console.WriteLine("   {0} was denied the lock", Thread.CurrentThread.Name);
                return false;
            }
        }
    }
}  
```

