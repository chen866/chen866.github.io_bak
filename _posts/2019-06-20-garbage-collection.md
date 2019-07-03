---
layout: article
title: WPF内存回收
tags:
  - C#
---

<!--more-->
### 问题
之前前员工做的一个WPF项目，运行时程序的占用内存总是不断增长，最后能把电脑的内存空闲内存全部占用，程序变得越来越卡，用户吐槽过好几次了。

### Lierda.WPFHelper
看了看代码，一直都没怎么做过WPF，程序里面大量的使用了定时器，代码全部是重复代码，毫无可读性可言，很多问题，
先给了一个链接  
[[WPF]解决程序运行时间长后内存占用太大(可能是泄漏),加个内存回收释放](https://www.zhaokeli.com/article/8183.html)

看看内容，看看代码，上网找找资料，这两篇文章写的挺详细深入  
[C# Winform应用程序占用内存较大解决方法整理（转）-- SetProcessWorkingSetSize](https://blog.csdn.net/sinat_15155817/article/details/78556603)  
[用SetProcessWorkingSetSize降低内存使用](https://blog.csdn.net/kanbang/article/details/18844453)  

`Lierda.WPFHelper`里的`LierdaCracker`的注释为“利尔达WPF内存压缩类”，其中有一个很明显的成员变量[手动滑稽]
```
exename = "预付费水表管理系统.exe";
```

`Cracker`方法有一个时间参数 默认30s
方法内在新线程内执行`FlushMemory`方法，执行后隔30s再次执行  
`FlushMemory`方法调用`GC.Collect()`方法并使用`GC.WaitForPendingFinalizers()`等待完成后，然后就调用了`SetProcessWorkingSetSize`方法

关于如何降低系统内存占用的资料：
1、使用性能测试工具dotTrace，它能够计算出你程序中那些代码占用内存较多
2、强制垃圾回收
3、多dispose，close
4、用timer，每几秒钟调用：SetProcessWorkingSetSize(Process.GetCurrentProcess().Handle, -1, -1)
5、发布的时候选择Release
6、注意代码编写时少产生垃圾，比如String + String就会产生大量的垃圾，可以用StringBuffer.Append
7、this.Dispose();this.Dispose(True);this.Close();GC.Collect();   
8、注意变量的作用域，具体说某个变量如果只是临时使用就不要定义成成员变量。GC是根据关系网去回收资源的。
9、检测是否存在内存泄漏的情况

总的来说，方法不错，但经过测试以后，在这个程序里面并没有什么用：
1. 那些GC回收不掉的，强制GC回收也没用
2. 程序一直是在前台执行的，`SetProcessWorkingSetSize`方法缓存到硬盘上的数据，很快又会被读出来，还增加了程序的开销

顺便提取了一个工具类：
```csharp
class WPFHelper
{
    /// <summary>开始压缩内存</summary>
    /// <param name="sleepSpan">间隔，单位：秒</param>
    public static void Cracker(int sleepSpan = 30)
    {
        Task.Factory.StartNew(() =>
        {
            while (true)
            {
                try
                {
                    FlushMemory();
                    Thread.Sleep(TimeSpan.FromSeconds((double) sleepSpan));
                }
                catch (Exception ex)
                {
                    //TODO Cracker->FlushMemory Error
                }
            }
        });
    }

    [DllImport("kernel32.dll")]
    private static extern bool SetProcessWorkingSetSize(IntPtr proc, int min, int max);

    private static void FlushMemory()
    {
        GC.Collect();
        GC.WaitForPendingFinalizers();
        if (Environment.OSVersion.Platform != PlatformID.Win32NT)
            return;
        SetProcessWorkingSetSize(Process.GetCurrentProcess().Handle, -1, -1);
    }
    public static void CrackerOnlyGC(int sleepSpan = 30)
    {
        Task.Factory.StartNew(() =>
        {
            while (true)
            {
                try
                {
                    GC.Collect();
                    GC.WaitForPendingFinalizers();
                    Thread.Sleep(TimeSpan.FromSeconds((double)sleepSpan));
                }
                catch (Exception ex)
                {
                    //TODO Error
                }
            }
        });
    }
}
```

### 最终解决
考虑到时间紧迫，果断上报请求支援。于是，之前的部门领导就被分配过来一起负责这个任务。  
在和领导互相讨论和吐槽了两天以后改好了，主要改了两种问题：
1. 定时器（占用CPU）,
    整理内部逻辑，把十几个定时器缩减为一个  
2. 资源释放（内存），`this.Dispose()`、`this.Dispose()`、`this.Dispose()`  

C#里面的资源分为两类：  
- 托管资源：由CLR管理分配和释放的资源，即由CLR里new出来的对象；  
- 非托管资源：不受CLR管理的对象，windows内核对象，如文件、数据库连接、套接字、COM对象等；
 
托管资源在引用计数为0的时候会被自动回收，而非托管资源需要手动处理，调用Dispose进行释放。

释放非托管资源的时候，除了使用Dispose，还可以在使用using。

### using关键字
using关键字有两个主要用途：
1. 作为指令，用于为命名空间创建别名或导入其他命名空间中定义的类型。
2. 作为语句，用于定义一个范围，在此范围的末尾将释放对象。

#### 作为语句
using 语句允许程序员指定使用资源的对象应当何时释放资源。using 语句中使用的对象必须实现 IDisposable 接口。此接口提供了 Dispose 方法，该方法将释放此对象的资源。
1. 可以在 using 语句之前声明对象。
2. 可以在 using 语句之中声明对象。
3. 可以有多个对象与 using 语句一起使用，但是必须在 using 语句内部声明这些对象。

#### 使用规则
1. using只能用于实现了IDisposable接口的类型，禁止为不支持IDisposable接口的类型使用using语句，否则会出现编译错误；
2. **using语句适用于清理单个非托管资源的情况，而多个非托管对象的清理最好以try-finnaly来实现，因为嵌套的using语句可能存在隐藏的Bug。内层using块引发异常时，将不能释放外层using块的对象资源**
3. using语句支持初始化多个变量，但前提是这些变量的类型必须相同
4. 针对初始化多个不同类型的变量时，可以都声明为IDisposable类型

#### using实质
在程序编译阶段，编译器会自动将using语句生成为try-finally语句，并在finally块中调用对象的Dispose方法，来清理资源。所以，using语句等效于try-finally语句

> [using关键字在C#中的3种用法](https://www.cnblogs.com/xiaobiexi/p/6179127.html)