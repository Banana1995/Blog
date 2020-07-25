# JDB debug

- 使用如下命令启动进程，开启jvm的debug模式，端口可自定义，默认为5005

  > `-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005`

- 使用如下命令，将jdb debugger附到已经运行的jvm上

  >  `jdb -attach 5005`

- 使用如下命令可设置断点

  > - `stop at com.gmy.MyClass:22` *(在MyClass的第22行设置断点)*
  > - `stop in java.lang.String.length` *(在java.land.String.length()方法的起始处设置断点)*
  > - `stop in com.gmy.MyClass.<init>` *(<init> 是 MyClass constructor)*
  > - `stop in com.gmy.MyClass.<clinit>` *(<clinit>MyClass的静态代码块)*

  在设置一个断点时，需要显式指定类的包路径，需要用类的全名称来设定。

- 常见的调试命令如下：

  > - next : 运行到下一步
  >
  > - cont : 往后运行到下一个断点
  >
  > - print ：
  >
  >   - `print MyClass.myStaticField`
  >
  >   - `print myObj.myInstanceField`
  >   - `print i + j + k` *(i, j, k are primities and either fields or local variables)*
  >   - `print myObj.myMethod()` *(if myMethod returns a non-null)*
  >   - `print new java.lang.String("Hello").length()`
  >
  >   **当print指定运行的java表达式未被jvm类加载时，它会报错无法运行。**
  >
  >   例如，当我试图在某个方法的入口执行将某个实例json格式化后print处理时，我使用了如下命令：
  >
  >   `print com.alibaba.fastjson.JSON.toJSONString(myEntity)`。此时产生了个找不到该class的错误。当我将方法继续往下一断点执行，在执行过程中，代码有用到该JSON序列化的类，jvm将该类加载了。之后我再通过如上命令尝试打印出实例的json格式化字符串便成功了。
  >
  > - wherei : 此命令会打印出当前的堆栈信息，告诉你目前断点的位置是在哪
  >
  > - dump ：此命令会告诉你当前线程的dump信息

