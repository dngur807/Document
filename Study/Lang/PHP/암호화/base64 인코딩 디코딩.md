### base64 인코딩 / 디코딩 함수의 특징

> 출처



### 정의

base 64 란 ?

```
base 64는 2진 데이터를 ASCII 코드에 해당하는 문자열로 변경해주는 방식을 말한다.
2진 데이터를 ASCII 형태로 변경하는 것을 base64_encode 가 처리해주며,
ASCII 형태의 데이터를 2진 데이터로 복원하는 것을 base64_decode 가 처리해 줍니다.
```



### 사용방법

```
base64_encode("문자열");
base64_decode("문자열");
```



### 예제

```
$str = "base64_encode";
echo  'base64_encode($str) : '.base64_encode($str).'<br>';
결과 :  base64_encode($str) : YmFzZTY0X2VuY29kZQ==

$str = "YmFzZTY0X2VuY29kZQ==";
echo  'base64_decode($str) : '.base64_decode($str).'<br>';
sha1($str) : base64_decode($str) : base64_encode
```

