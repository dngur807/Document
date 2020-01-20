### BufferBlock

> 출처  https://riptutorial.com/ko/csharp/example/10607/bufferblock--t- 
>
>  https://jacking75.github.io/csharp_TPL_DataFlow/ 

FIFO 들어오는 데이터가 나가는 데이터 입니다.



BufferBlock은 T의 인스턴스를 저장하기 위한 제한되지 않거나 제한된 버퍼를 제공합니다.

T의 인스턴스를 블록에 게시할 수 있습니다. 그러면 게시되는 데이터가 블록에 의해 선입 선출(FIFO) 순서로 저장됩니다.

블록에서 수신하면 이전에 저장되었거나 향후 사용할 수 있는 T의 인스턴스를 동기적 또는 비동기적으로 얻을 수 있다.



![image-20200117143721116](../../image\image-20200117143721116.png)

``` 
예
// Hand-off through a bounded BufferBlock<T>
private static BufferBlock<int> _Buffer = new BufferBlock<int>(
    new DataflowBlockOptions { BoundedCapacity = 10 });

// Producer
private static async void Producer()
{
    while(true)
    {
        await _Buffer.SendAsync(Produce());
    }
}

// Consumer
private static async Task Consumer()
{
    while(true)
    {
        Process(await _Buffer.ReceiveAsync());
    } 
}

// Start the Producer and Consumer
private static async Task Run()
{
    await Task.WhenAll(Producer(), Consumer());
}
```





- 범용적인 비동기 메시징 구조체를 나타낸다.

- 복수의 소스에서 쓰는 것이 가능하고 도 복수의 타겟에서 읽을 수도 있다.

- FIFO 큐에 메시지를 저장한다.

- 타겟이 메시지를 큐에서 가져가면 그 메시지는 큐에서 삭제된다.

- 복수의 메시지를 다른 컴포넌트에 전달하고, 그 컴포넌트에서 메시지를 수신하고 싶을 때 편리하다.

- 아래 예는 BufferBlock 오브젝트에 Int32 타입의 복수의 값을 post하고 그 오브젝트에서 값을 읽어들인다.

  

```
// Create a BufferBlock<int> object.
var bufferBlock = new BufferBlock<int>();

// Post several messages to the block.
for (int i = 0; i < 3; i++)
{
   bufferBlock.Post(i);
}

// Receive the messages back from the block.
for (int i = 0; i < 3; i++)
{
   Console.WriteLine(bufferBlock.Receive());
}
```

* TryReceive
  * 정기적으로 폴링할 때 사용하면 좋다
  * 이 메소드를 호출한 스레드를 블럭 시키지 않는다.

```
// Post more messages to the block.
for (int i = 0; i < 3; i++)
{
   bufferBlock.Post(i);
}

// Receive the messages back from the block.
int value;
while (bufferBlock.TryReceive(out value))
{
   Console.WriteLine(value);
}
Output:
   0
   1
   2
```

* Post 예제
  * post2와 post1은 병렬로 실행하므로 순서 보장은 못함.
  * 아래 예제의 task의 완료 순서는 post2, post1, receive

```
// Write to and read from the message block concurrently. 
var post01 = Task.Run(() =>
   {
      bufferBlock.Post(0);
      bufferBlock.Post(1);
   });
var receive = Task.Run(() => 
   {
     for (int i = 0; i < 3; i++)
     {
        Console.WriteLine(bufferBlock.Receive());
     }
   });
var post2 = Task.Run(() => 
   {
     bufferBlock.Post(2);
   });
Task.WaitAll(post01, receive, post2);
Sample output:
   2
   0
   1
```

