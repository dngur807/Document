### C# 해쉬테이블 vs 딕셔너리

> 출처  https://hongjinhyeon.tistory.com/87 

C#에서는 KEY와 VALUE를 사용해서 자료를 저장하는 타입이 2가지가 있습니다.

해시테이블과 딕셔너리인데요 사용법은 거의 같은나, 내부적으로 처리하는 기술이 다릅니다.

이 두가지 타입의 기본적인 사용법과 장단점에 대해 알아보겠습니다.



1. 해시 테이블

```
//생성
Hashtable hashtable = new Hashtable();

//자료 추가 ( 박싱이 일어남)
hashtable.Add("Data1", new HongsClass() { Name = "정우혁1", intCount = 1 });
hashtable.Add("Data2", new HongsClass() { Name = "정우혁2", intCount = 2 });

//자료 검색
if (hashtable.ContainsKey("Data1").Equals(true))
{
    HongsClass temp = hashtable["Data1"] as HongsClass; (언박싱 처리)
    Console.WriteLine(temp.Name);
}

//Loop 전체 순회출력
foreach (string NowKey in hashtable.Keys)
{
    HongsClass temp = hashtable[NowKey] as HongsClass;
    Console.WriteLine(temp.Name);
        
}

//결과  OUTPUT
//정우혁1
//정우혁1
//정우혁2
```



[해시테이블의 특징]

```
1. Non-Generic
2. Key와 Value 모두 Object를 입력받는다.
3. 박싱 / 언박싱을 사용합니다.
```



2 .딕셔너리 (Dictionary)

```
//생성- 제네릭 기반 
Dictionary<string, HongsClass> dictionary = new Dictionary<string, HongsClass>();

//자료추가
dictionary.Add("Data1", new HongsClass() { Name = "홍진현1", intCount = 1 });
dictionary.Add("Data2", new HongsClass() { Name = "홍진현2", intCount = 2 });

//자료검색
if (dictionary.ContainsKey("Data1").Equals(true))
{
    Console.WriteLine(dictionary["Data1"].Name);
}

//Loop 전체 순회출력
foreach (HongsClass NowData in dictionary.Values)
{
    Console.WriteLine(NowData.Name);
}

//결과
//홍진현1
//홍진현1
//홍진현2
```



[딕셔너리의 특징]

1. Generic
2. Key와 Value 모두 Strong Type을 입력받는다. (선언시 타입을 입력해줘야한다)
3. 박싱 / 언박싱이 일어나지 않는다.

+딕셔너리는 선언시에 사용할 타입을 미리 설정하기 때문에 입력시나 출력시에도 박싱/언박싱이 일어나지 않습니다.

따라서 입력과 사용에 용이하며, 외부에서도 이 타입을 사용할 때도 타입이 정의되어 있으니 다른 타입으로 형변환을

시도하다가 실패할 염려가 없습니다.



3. 결론

두가지 타입이 사용법은 비슷하지만 내부적인 처리와 수용하는 타입의 형태가 다르므로 필요에 따라서 선택을 해야합니다. 고정적으로 하나의 타입만 입력받을 시에는 딕셔너리를 사용하며, Value에 일정한 형식이 없고 여러 형태를 저장하려면 해시 테이블을 사용해야 합니다.

