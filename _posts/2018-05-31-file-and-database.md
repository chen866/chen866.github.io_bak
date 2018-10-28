---
layout: post
title: 文件存在数据库和硬盘里那个快？
tags:
  - C#
lang: zh-Hans
---


<!--more-->

今天领导(?)想知道文件存在数据库里快，还是存在硬盘里面快,让写一个程序展示一下，尽管想用其他语言(python多好)，但是考虑到具体司情，还是用C#吧  
因为暂时不需要验证文件，所以先用分卷压缩制作了3个特定大小的文件，文件大小分别是1M、10M、20M，就以大小命名，并去除文件扩展名和放在C盘下面新建的一个文件夹里方便拼写路径  
新建的是C#控制台程序，SQL Server数据库本地直连，因为图省事所以在本地直连的数据库，所以程序是在公司测试数据库所在的服务器上跑的  
使用`Stopwatch`计算的程序运行时间，使用SqlConnection连接的数据库。  
> 好坑啊最开始创建的是.NETCore控制台程序而不是.NETFrameWork控制台程序，以至于一直导不进去SQLConnection的库，引以为戒  
刚开始for循环的次数是100次，运行了几次就快10个G了好心疼固态，调成了10次。
在我电脑上很奇怪三个写入本地的时间都是一样的，在想是不是因为固态或者磁盘缓冲区之类的原因。  
没什么说的了，上图

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-05-31-1.png)

上代码  

```csharp  
using System;
using System.Data;
using System.Data.SqlClient;
using System.Diagnostics;
using System.IO;

namespace ConsoleApp1
{
    class Program
    {
        private static SqlConnection _conn = null;

        static SqlConnection conn
        {
            /*if (conn == null)
            {
                conn = new SqlCeConnection(connsql);
            }*/
            get { return _conn ?? (_conn = new SqlConnection(connsql)); }
        }

        // 数据库连接字符串,database设置为自己的数据库名，以Windows身份验证
        static string connsql =
            "Data Source=.;Initial Catalog=DBName;Integrated Security=SSPI;"; 

        static void Main(string[] args)
        {
            test();
            Console.WriteLine("===========>ENTER TO EXIT<===========");
            Console.ReadLine();
        }

        static void test()
        {
            //文件路径
            //string[] myComputerFilePath = { "D:\\1M", "D:\\10M", "D:\\20M" };
            string[] myComputerFilePath = {"C:\\0\\1M", "C:\\0\\10M", "C:\\0\\20M"};
            
            //Test1
            Console.WriteLine("读文件成字节流");

            Stopwatch stopwatch = new Stopwatch();
            foreach (string file in myComputerFilePath)
            {
                stopwatch.Start(); //  开始监视代码运行时间

                //  需要测试的代码
                for (int i = 0; i < 100; i++)
                {
                    byte[] b = FileToByte(file);
                }

                stopwatch.Stop(); //  停止监视
                TimeSpan timespan = stopwatch.Elapsed; //  获取当前实例测量得出的总时间
                double milliseconds = timespan.TotalMilliseconds; //  总毫秒数
                Console.WriteLine(file + "--->" + milliseconds + "毫秒");
            }


            //Test2
            Console.WriteLine("写字节流为文件");

            foreach (string file in myComputerFilePath)
            {
                byte[] filebyte = FileToByte(file);
                Directory.CreateDirectory(file + "s").Create();
                stopwatch.Start(); //  开始监视代码运行时间

                //  需要测试的代码
                for (int i = 0; i < 10; i++)
                {
                    ByteToFile(filebyte, file + "s\\" + i + "M");
                }

                stopwatch.Stop(); //  停止监视
                TimeSpan timespan = stopwatch.Elapsed; //  获取当前实例测量得出的总时间
                double milliseconds = timespan.TotalMilliseconds; //  总毫秒数
                Console.WriteLine(file + "--->" + milliseconds + "毫秒");
            }

            //Test3
            Console.WriteLine("字节流存至数据库");

            foreach (string file in myComputerFilePath)
            {
                byte[] filebyte = FileToByte(file);
                stopwatch.Start(); //  开始监视代码运行时间

                //  需要测试的代码
                for (int i = 0; i < 10; i++)
                {
                    ByteToDatabase(file + "s\\" + i + "M", filebyte);
                }

                stopwatch.Stop(); //  停止监视
                TimeSpan timespan = stopwatch.Elapsed; //  获取当前实例测量得出的总时间
                double milliseconds = timespan.TotalMilliseconds; //  总毫秒数
                Console.WriteLine(file + "--->" + milliseconds + "毫秒");
            }

            //Test4
            Console.WriteLine("从数据库读取字节流");

            foreach (string file in myComputerFilePath)
            {
                stopwatch.Start(); //  开始监视代码运行时间

                //  需要测试的代码
                for (int i = 0; i < 10; i++)
                {
                    byte[] filebyte = DatabaseToByte(file + "s\\" + i + "M");
                }

                stopwatch.Stop(); //  停止监视
                TimeSpan timespan = stopwatch.Elapsed; //  获取当前实例测量得出的总时间
                double milliseconds = timespan.TotalMilliseconds; //  总毫秒数
                Console.WriteLine(file + "--->" + milliseconds + "毫秒");
            }
        }

        /// <summary>
        /// 写字节流为文件
        /// </summary>
        /// <param name="Files"></param>
        /// <param name="path"></param>
        static void ByteToFile(byte[] Files, string path)
        {
            if (File.Exists(path))
            {
                File.Create(path);
            }

            BinaryWriter bw = new BinaryWriter(File.Open(path, FileMode.OpenOrCreate));
            bw.Write(Files);
            bw.Close();
        }

        /// <summary>
        /// 读文件成字节流
        /// </summary>
        /// <param name="path"></param>
        /// <returns></returns>
        static byte[] FileToByte(string path)
        {
            FileStream fs = new FileInfo(path).OpenRead();
            BinaryReader br = new BinaryReader(fs);
            byte[] byData = br.ReadBytes((int) fs.Length);
            return byData;
        }

        /// <summary>
        /// 字节流存至数据库
        /// </summary>
        /// <param name="name"></param>
        /// <param name="bytes"></param>
        static void ByteToDatabase(string name, byte[] bytes)
        {
            conn.Open();
            string str = "insert into test (name,content) values('"+name+"',@file)";
            SqlCommand mycomm = new SqlCommand(str, conn);
            mycomm.Parameters.Add("@file", SqlDbType.Binary, bytes.Length);
            mycomm.Parameters["@file"].Value = bytes;
            mycomm.ExecuteNonQuery();
            conn.Close();
        }

        /// <summary>
        /// 从数据库读取字节流
        /// </summary>
        /// <param name="conn"></param>
        /// <returns></returns>
        static byte[] DatabaseToByte(string name)
        {
            conn.Open();
            string str = "select top 1 * from test where name='" + name + "';";
            SqlDataAdapter sda = new SqlDataAdapter(str, conn);
            DataTable myds = new DataTable();
            sda.Fill(myds);
            conn.Close();
            return (byte[]) myds.Rows[0]["content"];
        }
    }
}
```