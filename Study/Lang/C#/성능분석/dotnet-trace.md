> https://docs.microsoft.com/ko-kr/dotnet/core/diagnostics/dotnet-trace#synopsis
>
> https://www.jetbrains.com/ko-kr/profiler/features/



### 설명

`dotnet-trace` 도구 :

* 크로스 플랫폼 .NEY Core 도구입니다.
* 네이티브 프로파일러 없이 실행 중인 프로세스의 .NET Core 추적을 수집할 수 있도록 합니다.
* .NET Core 런타임의 크로스 플랫폼 `EventPipe` 기술을 기반으로 하여 빌듣되었습니다.
* Windows.Linux 또는 macOS에서 동일한 환경을 제공



### 설치

```
dotnet tool install --global dotnet-trace
```



### dotnet-trace collect

실행 중인 프로세스에서 진단 추적을 수집합니다.

```
.NET 프로세스를 확인하려면 아래 명령어를 실행해서 PID 를 얻을 수 있다.
dotnet-trace list-processes
dotnet-trace ps(윈도우)
```

```
.NET Core 어플리케이션을 추적할 수 있습니다.
dotnet-trace collect -p <PID>
dotnet-trace collect --format speedscope -p <PID>  (https://www.speedscope.app/)
```



### dotnet-trace 에서 캡처한 추적 보기

윈도우에서 .nettrace 파일들은 ETW나 LTTng로 수집 된 추적 로그들과 마찬가지로 `PerfView`라는 분석 툴로 볼 수 있습니다. 



