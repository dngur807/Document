# 델리게이트

델이게이트 사용 예시

```
http://devkorea.co.kr/bbs/board.php?bo_table=m03_qna&wr_id=36068
[실생활에서 delegate와 비슷한 상황을 말한다면..]
1) 손님이 카페에 갑니다.
2) 손님이 카페에서 주문을 합니다.
3) 손님이 주문하면 점원은 결재를 하고 원격으로 call 하는 기계(이름 몰라요..;;)를 손님에게 줍니다.
    기계안에 delegate로 호출 event를 등록 하고 줬습니다.
4) 카페에서는 누가 주문을 했는지는 모르고 주문번호(기계에 붙은 번호)와 들어온 걸 만듭니다.
5) 점원이 주문한 내용을 다 만들고 나면, 주문자가 누군지는 모르지만 주문번호의 기계를 호출(call) 합니다.
6) 손님은 누가만들었는지는 몰라도 기계가 울리니깐 카운터로 가서 주문한 음료를 받아서 돌아갑니다.


[게임에서라면..]
간단하게 GameManager와 Player가 있다고 하겠습니다.
Player가 죽는 경우의 처리에 대해서 어떻게 처리할 것인가..만 간단히 생각해 보겠습니다.
예를 드는거라 두가지만 간단하게 극단적으로 말하자면..

1) GameManager의 Update에서 Player의 HP를 항상 체크하면서, 혹은 죽었는지를 항상 체크하면서 죽으면 어떤 처리를 하는 방식.(매 Update 주기마다 호출함)

2) GameManager가 Player에게 delegate로 Dead event를 넘겨줍니다. 그 후에 Player 자신이 죽었을 때 Dead event를 발생(1회 호출함)시켜서 GameManager에게 죽었다고 알립니다.


delegate-event를 사용하면, 위에서 보시다시피 2)번이 호출 횟수도 적고 로직상 연관성이나 상하관계를 안가지더라도 무언가 이벤트가 일어났을 때 알려줄 수 있습니다.
```





유니티 이벤트는 이벤트를 쉽게 사용할 수 있도록 만들어둔거임



델리게이트 (대행)가 뭔지 짧게 설명하면 

어떤 목록을 추가해주면 대신 발동해준다.



간단한 예제를 만들자



C# 스크립트 만들고 Calculator 스크립트 만들어보자



계산을 대행해준다. 어떤 내용을 계산할 지 모른다.



일단 델리게이트 없이 계산을 하자



public float Sum(float a , float b)

{

   Debug.Log(a + b);

​	return a + b ;

}

public float Subtract(float a , float b)

{

​	Debug.Log(a - b);

​	return a - b;

}



public float Multiply(float a , float b)

{

​	Debug.Log( a * b);

​	return a * b;

}



void Update()

{



// 즉 계산하는 순간에 어떤 것을 계산할 지 알고 있어야 한다..

​	if (Input.GetKeyDown(KeyCode.Space))

​	{

​		Sum(1,10);

Subtract(1,10);

Multiply(1,10);

​	}

}



문제는 계산기가 어떤계산을 할지 실행 시점에 알아야한다는 거다.

어떤 계산을 할지는 신경쓰면 안되지...

새로 짜지 않으면 변경사항을 적용하기 힘듬



이제 계산합니다 라고 명시해주고

실시간으로 바뀔 수 있게 하자



유언 대리인에 비유된다.



형을 먼저 선언한다 델리게이트 형은 이런 형태만 대신해 주겠다는 것이다.



이 명단에 함수를 등록하는거지



사실은 델리게이트는 함수 포인터를 가지고 있다.

A , B ,C 에 등록하는건 함수 포인터를 가지고 있는 것이다.

기억하고 있다가 발동할 때 그 함수를 호출하는 것이다.



이제 델리게이트 형을 선언해보자

delegate float Calculate(float a ,float b) ; //오브젝트가 아니고 델리게이트 원본이다

위 타입에 함수만 대행해준다는 것이다.

Calculate onCalculate;



// 게임 시작 시 대신해줄 친구들을 등록해준다.

void Start()

{

onCalculate = Sum; // 괄호치면 실제로 사용하는 것이지..

}

이제 업데이트 할 때 발동하자 이건 어떤 녀석이 발동할지는 알지 못한다.



대행 한 친구의 결과 값도 받아 올 수 있다.



한가지만 대리할 수 있는것이 아니라 여러가지를 대행할 수 있습니다.

Oncalculate += Subtract;

기존 명단에 있는 놈은 -로 뺄 수 도 있다.





