> https://jacking75.github.io/csharp_HttpRequest/

### C# - Http Request

* 닷넷프레임워크에서는 서버로의 동 접수가 기본으로 최대 2로 제한 되어 있다. 이 때문에 웹 서비스 클라에서 웹 서비스등을 호출할 때 처리량을 올리기 위해서는 멀티스레드에서 웹서비스를 호출하여도 액티브한 최대 동접수는 2로 됩니다.
* 싱글 스레드로 통신 할 때와 비교하면 처리량(시간당 처리량)은 향상 되지만 처리 스레드 수를 늘려도 바로 그 결과를 얻을 수 없다.

* 서버로의 동시 접속수가 최대 2개로 제한된 것을 늘리기 위해서 애플리케이션 구성 파일 (machine.config 파일도 가능) 에 connectionManagement 요소를 아래와 같이 추가하여 최대 수 를 늘릴 수 있다.

```
<configuration>
<system.net>
  <connectionManagement>
    <add address="*" maxconnection="20"/>
  </connectionManagement>
</system.net>
</configuration>
```

* 보안을 고려해서 address는 제한을 해제하고 싶은 서버 URL 만을 지정하면 좋습니다.

  ```
  <configuration>
    <system.net>
      <connectionManagement>
        <add address = “http://www.yahoo.co.jp” maxconnection = “4″ />
        <add address = “*” maxconnection = “2″ />
      </connectionManagement>
    </system.net>
  </configuration>
  ```

* 설정 파일이 아닌 코드로 설정 할 수도 있다.

```
System.Net.ServicePointManager.DefaultConnectionLimit
```



```
string PostData = “mac=70-71-BC-CC-64-D6&svcCd=3ON&ip=124.137.60.50”; 
var httpWebRequest = (System.Net.HttpWebRequest)
System.Net.WebRequest.Create(RequestUrl); 
byte[] sendData = UTF8Encoding.UTF8.GetBytes(PostData); 
httpWebRequest.ContentType = “application/x-www-form-urlencoded; c
harset=UTF-8”; httpWebRequest.Method = “POST”; 
httpWebRequest.ContentLength = sendData.Length;

using (var requestStream = httpWebRequest.GetRequestStream()) { requestStream.Write(sendData, 0, sendData.Length);
requestStream.Close();
var httpResponse = (System.Net.HttpWebResponse)httpWebRequest.GetResponse();
using (var streamReader = new StreamReader(httpResponse.GetResponseStream()))
{ 
var result = streamReader.ReadToEnd();
} 
}

```

