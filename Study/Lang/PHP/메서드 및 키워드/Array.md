* ArraySearch

배열에서 특정 값을 찾을 때 사용

```
$pos = array_search("#" , $array);
#값이 시작한 위치
```



* array_splice

```
배열에 offset 부터  length까지 잘라낸다.
array_splice($headers , 0 , $start_index + 1  );
```



* array_map

```
함수를 콜백의 인수로 사용되는 해당 인덱스 적용한 배열 결과 반환
$result_array = array_map('call_back' , array)

<?php
function cube($n)
{
    return ($n * $n * $n);
}

$a = [1, 2, 3, 4, 5];
$b = array_map('cube', $a);
print_r($b);
?>

trim 함수 적용
배열의 각 원소에 trim 함수를 적용
$str = 'alpha,bravo,charlie,delta';
$final = explode( ',', $str );
일단 문자를 배열로 만들자!!

그럼 정상적으로 잘 보이는것 같지만.
컴퓨터에겐 bravo 와 ' bravo' 는 완전히 다른 내용입니다.

이것은 trim 함수로 일치 시켜 주는 것입니다.

```



* array_combine

key 매개변수 배열을 keys키로 사용하고 배열의 값을 해당 값으로 사용하여 배열을 만듭니다.

```
array_combine ( array $keys , array $values ) : array

$a = array('green', 'red', 'yellow');
$b = array('avocado', 'apple', 'banana');
$c = array_combine($a, $b);
print_r($c);
```



