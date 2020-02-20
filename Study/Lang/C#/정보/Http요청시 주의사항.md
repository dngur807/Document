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





실습

웹은 2초 정도 딜레이가 걸린다고 했을 때

 System.Net.ServicePointManager.DefaultConnectionLimit 수에 따라 처리되는 것을 확인 할 수있다.

C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config 의 위치

```csharp
List<Task> listTask = new List<Task>();
System.Net.ServicePointManager.DefaultConnectionLimit = 4;
Console.WriteLine(System.Net.ServicePointManager.DefaultConnectionLimit);
for (int i = 0; i < num; ++i)
{

    listTask.Add(new Task(() =>
                          {
                              jreq.SetField("server_name", i.ToString());
                              var res = Util.HttpRequest("url", jreq.ToString());
                              server_name++;
                              Console.WriteLine(res.response);
                          }));    
}

for (int i = 0; i < num; ++i)
{
    listTask[i].Start();
}
var res2 = Util.HttpRequest("url", jreq.ToString());
Console.WriteLine(res2.IsSuccess);

for (int i = 0; i < num; ++i)
{
    listTask[i].Wait();
}

```





> 출처 http://www.devpia.com/Maeul/Contents/Detail.aspx?BoardID=17&MAEULNo=8&no=178248&ref=178248 댓글

http 컨낵션을 닫지 않아서 생기는 문제

원인은 2가지

http는 1개 서버당 2개 컨넥션만 허용하는게 기본 설정이다.  그걸 관리하기 위해서는 컨넥션 풀이 내부적으로 있습니다. 즉 재사용 됩니다. 소켓 연결을 끊지 않고 다음 요청 (Request) 하고 받고 (Response) 합니다.

이걸 ServicePoint 라고 합니다.

이전 http 연결을 닫지 않으면 사용중으로 마크 되어 컨넥션을 받을 때 까지 대기 하게 됩니다.



만약 단순한 html 페이지가 아니라 다수의 동시 호출이 이루어지는 경우 컨넥션 수를 늘려야할 필요가 있습니다.

System.Net.ServicePointManager.DefaultConnectionLimit = 10;

이 명령으로 커낵션 수를 10개까지 쓰도록 늘릴수 있습니다.



무한히 늘린다고 좋은게 아니고, 시스템간 소켓 통신 연결은 두개만 있어도 충분한것 입니다. 서버의 동시 처리 역량에 맞춰주어야 합니다. (예 CPU 코어수만큼)



두번 째 원인은 세션락입니다.

웹서버 수준에서 같은 세션으로 들어오는 요청에 대해 잠금을 수행합니다.

Authorization 이 같다면 같은 사용자로 보고, 해당 사용자 리소스 즉 세션을 잠금 합니다. 더 이상의 요청을 모두 보류 시켜 데드락 처럼 보이게 됩니다. 또한 서버 수준에서도 세션당 연결 수 를 제한할 수 있습니다.



