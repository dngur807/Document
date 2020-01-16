### ConcurrentBag

> 출처   http://www.csharpstudy.com/DS/dynamic-array.aspx 

.NET 4.0 부터 멀티쓰레딩 환경에서 리스트를 보다 간편하게 사용할 수 있는 새로운 클래스

리스트와 비슷하게 객체들의 컬렉션을 저장하는데, List<T> 와 달리 입력 순서를 보장하지는 않는다. 



```
Add() : 데이터 추가

데이터 읽기
foreach 문
TryPeek() : 데이터를 읽기만 함
TryTake() : 데이터를 읽은 후 요소를 삭제
```



특징 

멀티쓰레드가 동시에 액세스할 수 있는데, 

ex) ThreadA와 ThreadB가 Bag에 데이터를 쓸 때 , 

ThreadA 가  1,2,3 넣고 , B가 4,5,6dmf sjgdmaus

Bag은 다시 읽을 때 자신이 쓴 ,1,2,3 을 우선순위로 먼저 읽고 다음 나머지 다른 쓰레드에 입력된 요소(4,5,6)을 읽게 된다.



 아래 예제에서 첫번째 쓰레드는 100개의 숫자를 ConcurrentBag에 넣게 되고, 동시에 두번째 쓰레드는 1초마다 10회에 걸쳐 해당 ConcurrentBag의 내용을 출력하는 것이다. 

```
using System;
using System.Collections;
using System.Collections.Concurrent; // ConcurrentBag
using System.Threading;
using System.Threading.Tasks;

namespace ConcurrentApp
{
    class Program
    {
        static void Main(string[] args)
        {
            var bag = new ConcurrentBag<int>();

            // 데이타를 Bag에 넣는 쓰레드
            Task t1 = Task.Factory.StartNew(() =>
            {
                for (int i = 0; i < 100; i++)
                {
                    bag.Add(i);
                    Thread.Sleep(100);
                }
            });

            // Bag에서 데이타를 읽는 쓰레드
            Task t2 = Task.Factory.StartNew(() =>
            {
                int n = 1;               
                // Bag 데이타 내용을 10번 출력함
                while (n <= 10)
                {                    
                    Console.WriteLine("{0} iteration", n);
                    int count = 0;

                    // Bag에서 데이타 읽기
                    foreach (int i in bag)
                    {
                        Console.WriteLine(i);
                        count++;
                    }
                    Console.WriteLine("Count={0}", count);

                    Thread.Sleep(1000);
                    n++;
                }
            });

            // 두 쓰레드가 끝날 때까지 대기
            Task.WaitAll(t1, t2);
        }
    }
}
        
```

