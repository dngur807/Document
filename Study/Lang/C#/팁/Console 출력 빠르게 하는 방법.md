

>  https://docs.google.com/document/d/1fbjXng3cAK4qxAOiJJXSkCocYQMItj10kCuXM1FTgSQ/edit 

출처: http://baba-s.hatenablog.com/entry/2020/01/20/083000
기존 방법**using** System;

**internal** **static** **class** Program
{
  **private** **static** **void** Main()
  {
    **for** ( **int** i = 0; i < 10000; i++ )
    {
      Console.WriteLine( i );
    }
  }
}

고속화

**using** System;
**using** System.IO;

**internal** **static** **class** Program
{
  **private** **static** **void** Main()
  {
    **var** sw = **new** StreamWriter( Console.OpenStandardOutput() )
    {
      AutoFlush = **false**,
    };
    Console.SetOut( sw );

​    **for** ( **int** i = 0; i < 10000; i++ )
​    {
​      Console.WriteLine( i );
​    }
  }
}