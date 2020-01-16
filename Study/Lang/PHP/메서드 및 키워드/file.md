### PHP fopen

> 참고  https://www.php.net/manual/en/function.fopen.php 

fopen은 파일 생성 및 읽을 때 사용합니다.

```
파일 읽기
$handle = fopen("user_info.csv", "r");
파일 존재하지 않을 경우 false 리턴
성공시 파일 포인터 리소스를 반환합니다.
```



### fgetcsv 

>  https://www.php.net/manual/en/function.fgetcsv.php 

csv 파일을 읽어서 처리하는 메서드

한 행식 CSV 형식으로 구문 분석하고 배열을 리턴한다는 점 이외는 fgets() 와 유가합니다.

```
csv 첫줄 읽기
$headers = fgetcsv($handle);
```





