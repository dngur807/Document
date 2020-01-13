### C# 태스크 (Task)

> 출처  https://qzqz.tistory.com/260 



Task 클래스는 비동기에 대한 기능을 사용할 수 있습니다.

동기 코드는

메서드를 호출한 뒤에 이 메서드의 실행이 완전히 종료되어야 다음 메서드를 호출할 수 있습니다.



비동기 코드는

메서드를 호출한  뒤에 메서드의 종료를 기다리지 않고 바로 다음 코드를 실행합니다.



Task 클래스는 인스턴스를 생성할 때 Action 델리게이트를 넘겨받습니다.

```
Action SomeAction = () =>
{
    Thread.Sleep(1000);  // 1초 딜레이
    Console.WriteLine("Printed asynchronously");
};

Task myTask = new Task(SomeAction);
myTask.Start(); // 무명 함수를 비동기로 호출

Console.WriteLine("Printed synchronously");

myTask.Wait(); // myTask 비동기 호출이 완료될 때까지 기다림

// Printed synchronously
// Printed asynchronously
```

 Start() 메서드 말고 Run() 메서드를 사용할 수도 있습니다. (조금 더 일반적인 방법) 



```
// Task의 생성과 시작을 한번에 함
var myTask = Task.Run( () =>
    {
        Thread.Sleep(1000);  // 1초 딜레이
        Console.WriteLine("Printed asynchronously");
    }
);

Console.WriteLine("Printed synchronously");

myTask.Wait(); // myTask 비동기 호출이 완료될 때까지 기다림
```



### Task<TResult> 클래스

 코드의 비동기 실행 결과를 주는 Task<TResult> 클래스입니다. 

```
var myTask = Task<List<int>>.Run(
    () =>
    {
        Thread.Sleep(1000);
    
        List<int> list = new<int>();
        list.Add(3);
        list.Add(4);
        list.Add(5);    

        return list;    // TResult 형식의 결과를 반환
    }
);

var myList = new List<int>();

myList.Add(0);
myList.Add(1);
myList.Add(2);

// myTask.Result 프로퍼티는 비동기 작업이 완료되어야 반환되어 Wait() 메서드를 호출하지 않아도 되지만
// Wait() 메서드는 항상 호출 하는것이 좋습니다.
myTask.Wait();
myList.AddRange(myTask.Result.ToArray()); // myList 요소는 0, 1, 2, 3, 4, 5
```





### Parallel 클래스 (병렬 처리)

병렬 처리는 하나의 작업을 여러 작업자가 나눠서 수행한 뒤 다시 하나의 결과로 만드는 것을 말합니다.

 이전 포스팅의 예제는 특정 범위 안에 있는 모든 소수를 찾는 프로그램이었습니다.
각 Task 객체는 동시에 작업을 수행하고 결과를 반환합니다.



  Parallel 클래스는 For(), Foreach() 등의 메서드를 제공하면서
이전 Task<TResult>를 이용한 병렬처리를 훨씬 쉽게 구현할 수 있도록 해줍니다.

```
void SomeMethod(int i)
{
    Console.WriteLine(i);
}

Parallel.For(0, 100, SomeMethod);
```

