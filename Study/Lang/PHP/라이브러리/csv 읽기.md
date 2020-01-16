### CSV 파일 읽기

csv란 comma separated version 컴마로 구분된 파일입니다.

![image-20200116220334321](../../\image\image-20200116220334321.png)

```
메모장으로 봤을 때 이런식으로 되어 있습니다.
[,id,name,level,exp,]
[,1000,정우혁,1,5,]
[,1001,대충한다,3,45,]
[,1002,그만하자,5,200,]
[,1003,PHP,40,201,]
[,1004,재밌다,30,20000,]
```



![image-20200116222256975](../../\image\image-20200116222256975.png)

```
/**
 * CSV 파일 읽기
 * @param $file_path
 */
function LoadCSV($file_path)
{
    $handle = fopen($file_path , 'r');
    if ($handle === false) {
        return NULL;
    }

    $headers = fgetcsv($handle);
    $start_index = array_search("[", $headers);
    $end_index = array_search("]", $headers);

    array_splice($headers , 0 , $start_index + 1  );
    array_splice($headers , $end_index );

    $headers =  array_map('trim' , $headers);

    $data_list = [];
    $index = array_search("id" , $headers);

    while($row = fgetcsv($handle)) {
        array_splice($row , 0 , $start_index + 1);
        array_splice($row , $end_index);
        $row = array_map('trim' , $row);

        $current = array_combine($headers, $row);
        $data_list[] = $current;
    }

    return $data_list;
}
```

