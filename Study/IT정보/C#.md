1. 
2. Logging in C# .NET Modern-day Practices: The Complete Guide
   https://michaelscodingspot.com/logging-in-dotnet/

2. Tame your Garbage Collector
   https://prodotnetmemory.com/slides/TameYourGC/#1

3. Collecting and analyzing memory dumps 
   https://devblogs.microsoft.com/dotnet/collecting-and-analyzing-memory-dumps

   이 유틸리티로 만든 파일을 Visual Studio나 Windows의 PerfView 유틸리티로 분석할 수 있습니다.
   그리고 리눅스에서는 sudo 명령 없이도, 그리고 dotnet dump analyze 명령어를 Windows에서도 사용할 수 있게 개선한 닷넷 코어의 자체 메모리 덤프 도구가 새로 업데이트되었습니다. 이 도구를 사용하여 메모리 누수, 사용 현황 파악을 좀 더 쉽게 할 수 있습니다.

4. C언어, 2019 올해의 프로그래밍 언어...상승세 파이썬 제쳐
   http://m.zdnet.co.kr/news_view.asp?article_id=20200110021453&re=zdk#imadnews
   "예측을 뒤엎고 C언어가 인기를 얻은 이유는 사물인터넷(IoT)과 최근 출시하는 다양한 스마트 기기에 적합한 프로그래밍 언어로 주목받았기 때문"

   위 순위는 인기 상승률을 말하는 것으로 C언어가 1위, C#은 2위입니다. 

   2020년 1월 전체 순위는 JAVA가 부동의 1위이고,  C , Python, C++, C#, VB.NET, JS, PHP 순서입니다
   https://jaxenter.com/c-tiobe-2019-166456.html



5. 【Rider】후위 템플릿 사용하기
   http://baba-s.hatenablog.com/entry/2019/09/10/075000 

   글에 있는 git 동영상을 보면 어떤 기능인지 쉽게 이해할 수 있습니다

6. C# 직력화 라이브러리 Ceras 
   https://github.com/rikimaru0345/Ceras

   성능이 MsgPack 보다 더 좋고 사용 방법도 더 쉽습니다.
   Unity에서도 쉽게 사용할 수 있습니다.
   클라이언트는 Unity, 서버는 C#으로 만들 때 사용하면 좋을 것 같습니다.

7. dotnet/diagnostics
   https://github.com/dotnet/diagnostics

   닷넷용 커맨드라인 툴이고, 아래의 3개의 툴이 있습니다.
   dotnet-dump - Dump collection and analysis utility.
   dotnet-trace - Enable the collection of events for a running .NET Core Application to a local trace file.
   dotnet-counters - Monitor performance counters of a .NET Core application in real time.

   dotnet-trace 사용법
   https://jira.gamevilcom2us.com/wiki/display/CCentralServerGUIDE/dotnet-trace

8. Rider 2019.3의 새로운 기능
   https://www.jetbrains.com/ko-kr/rider/whatsnew/

   

9. 닷넷으로 커맨드라인 툴을 만들 때 유용한 라이브러리

   Cocona
   https://github.com/mayuki/Cocona



C#으로 웹어셈블리 프로그래밍이 가능한 Blazor WebAssembly 5월에 출시
https://jira.gamevilcom2us.com/wiki/pages/viewpage.action?pageId=175805552

고성능 웹앱을 만들기 위해 WebAssembly를 사용하는데 C#에서는 Blazor를 사용하여 WebAssembly를 만듭니다.
이 Blazor를 사용하면 HEML5나 CSS 등을 몰라도 복잡하지 않은 웹앱은 C# 코드로 구현할 수 있습니다.







Rider 추천 플러그인 

Awesome Plugins for Rider: UI/UX
https://blog.jetbrains.com/dotnet/2019/06/04/awesome-plugins-rider-uiux/

Awesome Plugins for Rider: Code Editing/Analysis
https://blog.jetbrains.com/dotnet/2019/05/31/awesome-plugins-rider-code-editinganalysis/

Awesome Plugins for Rider: Language Support
https://blog.jetbrains.com/dotnet/2019/05/29/awesome-plugins-rider-language-support/

Fun and entertaining plugins for Rider
https://blog.jetbrains.com/dotnet/2019/07/19/fun-entertaining-plugins-rider/



ASP.NET Core에 NLog 사용하기

https://docs.google.com/document/d/1Ea8qqRjLdI5aYVSV_Lxu4YhbDAPQ5Rc0jAoqY8ipsTM/edit





닷넷 코어 3.0 이상부터 사용할 수 있습니다.

.NET Core의 Diagnostics CLI Tool을 사용하기
https://docs.google.com/document/d/1IGs5K1IbcuWc7ty-6RQQRc6CQ01SK_PFgSSM6MAcl3Q/edit?usp=sharing





NLog - Fluentd 노드로 로그를 보내는 방법
https://jacking75.github.io/csharp_nlog_fluentd/

NLog와 연결될 Fluentd는 Input을 tcp로 합니다.



for, foreach 속도 비교
https://docs.google.com/document/d/1auWKA-NySQrm0VIV9lpDgR9et1HyOHzEpvSH6fogj0E/edit?usp=sharing



https://github.com/jacking75/RandomStringGenerator4DotNet
랜덤 문자열 생성 라이브러리입니다.

임시 비밀번호 같은걸 만들 때 사용하면 유용합니다.

사용 법
var generator = new RandomStringGenerator.StringGenerator();
generator.MinLowerCaseChars = 2;
generator.MinUpperCaseChars = 1;
generator.MinNumericChars = 3;
generator.MinSpecialChars = 2;
generator.FillRest = RandomStringGenerator.CharType.LowerCase;

var token = generator.GenerateString(10);







C#으로 백그라운드로 특정 시간, 특정 기간마다 일을 처리하고 싶을 때 사용하면 좋은 라이브러리입니다.
리눅스의 cron 같은 것을 생각하면 이해하기 쉬울 것 같습니다.

원래 소스(https://github.com/fluentscheduler/FluentScheduler)를 닷넷코어 프로젝트로 변경했습니다
https://github.com/jacking75/FluentScheduler





.NET 고속화 Tips
https://docs.google.com/document/d/1wzlU2D1sDkEYKgtyKmlXByyEpHhJ0rvVpHwgjNMJdW8/edit?usp=sharing

