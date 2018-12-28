---
layout: article
title: 每天推送鸡汤
tags:
  - python
lang: zh-Hans
---

<!--more-->

前几日在掘金上看到一篇文章：[100行代码，每天早上喝一碗鸡汤](https://juejin.im/post/5bb1dd23e51d456f087ba7c6)

大意是通过高德api获取一个固定城市的天气，通过金山词霸的api获取每日推送的鸡汤文，看着挺不错的样子，我就打算研究一下，原文中有的地方我就不再重复了，记录下copy的过程。

## api准备

首先，作者在文章下面贴上了完整代码，完整代码中的Server酱的推送地址和高德api url中的key是需要我们自己在[Server酱](http://sc.ftqq.com/3.version)和[高德开放平台](https://lbs.amap.com/dev/)上面自行申请替换，都是免费的配额足够自用了。  
高德查询天气api还需要城市编码，可以通过高德开放平台上面发布的城市编码表查询所需要查询天气的城市编码。

## 定时任务框架APScheduler

定时发送使用的APScheduler，APScheduler支持多种方式存储任务队列，默认使用的是在内存中存放，看起来挺好玩的，我使用的是sqlite方式，运行后可以打开数据库查看任务队列；  

APScheduler框架也挺有意思的，比如说它的工作流程：

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-10-28-01.png)

> 参考教程：[花10分钟让你彻底学会Python定时任务框架apscheduler](https://www.jianshu.com/p/919a7627cafe)

中间碰到比较坑的是服务器时间，我是在下午调试的，还以为服务器时间使用的是不是24小时制，部署了很多次都未按照预期运行，直到我灵光一闪，想到了时区问题。。。

> 纽约和北京的时差相差12个小时

知道真相的我眼泪掉下来。。。  
翻翻[APScheduler的用户指南](https://apscheduler.readthedocs.io/en/latest/)，找到一个叫`timezone`的构造参数，尝试使用下：

```python
tz = pytz.timezone('Asia/Shanghai')
scheduler = BlockingScheduler(timezone=tz)
```

嗯，完美运行，又涨姿势了

## 部署

`nohup`命令，后台运行进程，并输出日志到nohup.out文件中：  
```shell
nohup python3 -u heartsoup.py >> nohup.out 2>&1 &
```

查看当前后台运行进程：  
```shell
ps -aux|grep heartsoup.py| grep -v grep | awk '{print $2}'
```

根据PID关闭相应进程：  
```shell
kill -9 [PID]
```

## 推送

推送使用的是Server酱的微信群组推送，和单独推送使用起来大差不差；  
又向里面加入了钉钉群机器人的推送。

## 效果图

钉钉：  
![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-10-28-02.png)

微信：  
![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-10-28-04.png)  
![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-10-28-03.png)

## 总结

总的来说，还是挺好玩的一个东西；  
美中不足的是因为推送时候使用的是Markdown，获取天气只能把地址位置固定，不能根据用户位置进行调整，有待优化；  
微信Server酱推送以后消息只能在服务器端保存3天，3天前的消息都不能查看了，有待优化。

## 具体代码

```python
#!/usr/bin/python3
# -*- coding: utf-8 -*-

import requests,json,sys,os,time,pytz
from apscheduler.schedulers.blocking import BlockingScheduler

soup_url = "http://open.iciba.com/dsapi/"
weather_url = "http://restapi.amap.com/v3/weather/weatherInfo?city=城市编码&key=高德地图key&extensions=all"
dd_urls = ["https://oapi.dingtalk.com/robot/send?access_token=机器人token"]
# Server酱群组推送
group_push_key = "key"
group_push_url = "https://pushbear.ftqq.com/sub"

tz = pytz.timezone('Asia/Shanghai')
scheduler = BlockingScheduler(timezone=tz)

def get_time():
    """
    获取当前时间
    """
    return time.strftime("%Y-%m-%d %H:%M:%S", time.localtime())

def get_soup():
    """
    获取鸡汤
    """
    response = requests.get(soup_url)
    json_data = response.json()
    date = json_data['dateline']
    content = json_data['content']
    note = json_data['note']
    picture = json_data['picture']
    picture2 = json_data['picture2']
    translation = json_data['translation']
    return date, content, note, picture, translation,picture2

def get_weather():
    """
    获取天气
    """
    response = requests.get(weather_url)
    json_data = response.json()
    if json_data['status'] == '1':
        return json_data['forecasts'][0]['casts'][0]
    else:
        print(get_time() + " 天气获取失败：" + json_data['info'])
        return None

def make_dd_soup(soup,weather):
    """
    制作鸡汤 钉钉
    """
    if weather is None:
        weather = get_weather()
    msg= {
     "msgtype": "actionCard",
     "actionCard": {
        "title": "[每日鸡汤]",
         "text": "![]({picture})  \n#### {date}\n\n#### 天气 {weather}，温度{nighttemp}℃ ~ {daytemp}℃。\n\n*{content}*\n\n#### {note}\n\n{translation}\n\n######  \n".format(date=soup[0], weather=weather['dayweather'] if weather['dayweather'] == weather['nightweather'] else weather['dayweather'] + "转" + weather['nightweather'],
            nighttemp=weather['nighttemp'], daytemp=weather['daytemp'],content=soup[1],note=soup[2],picture=soup[5],translation=soup[4]),
        "hideAvatar": "0",
        "btnOrientation": "0",
        "singleTitle" : "",
        "singleURL" : ""
        }
    }
    return msg

def make_wx_soup(soup,weather):
    """
    制作鸡汤 微信
    """
    if weather is None:
        weather = get_weather()
    title = "早上好！"
    desp = "#### {date}\n\n天气：{weather}，温度{nighttemp}℃ ~ {daytemp}℃。\n\n*{content}*\n\n{note}\n\n![]({picture})\n\n{translation}".format(date=soup[0],
                weather=weather['dayweather'] if weather['dayweather'] == weather['nightweather'] else weather['dayweather'] + "转" + weather['nightweather'],
                nighttemp=weather['nighttemp'],
                daytemp=weather['daytemp'],
                content=soup[1],
                note=soup[2],
                picture=soup[5],
                translation=soup[4])
    return title,desp

def push_wx(text=None, desp=""):
    """
    推送消息到微信
    """
    params = {
        "text": text,
        "desp": desp
    }
    response = requests.get(server_url, params=params)
    json_data = response.json()
    if json_data['errno'] == 0:
        print(get_time() + " 推送成功。")
    else:
        print("{0} 推送失败：{1} \n {2}".format(get_time(), json_data['errno'], json_data['errmsg']))

def push_wx_group(text, desp):
    """
    推送消息到微信群组
    """
    params = {
        'sendkey': group_push_key,
        'text': text,
        'desp': desp
    }
    response = requests.post(group_push_url, params=params)
    json_data = response.json()
    if json_data['code'] == 0:
        print(get_time() + ' 微群 ' + json_data['data'])
    else:
        print("{0} 微群 推送失败：{1} \n {2}".format(get_time() + json_data['message'], json_data['data']))

def push_dd(dd_url,msg):
    """
    推送消息到钉钉
    """
    headers = {'Content-Type': 'application/json;charset=utf-8'}
    response = requests.post(dd_url,data=json.dumps(msg),headers=headers)
    json_data = response.json()
    if json_data['errcode'] == 0:
        print(get_time() + " 钉钉 推送成功。")
    else:
        print("{0} 钉钉 推送失败：{1} \n {2}\n{3}".format(get_time(), json_data['errcode'], json_data['errmsg']),dd_url)

def work():
    # 获取数据
    soup = get_soup()
    weather = get_weather()
    # 生成
    dd_msg = make_dd_soup(soup,weather)
    wx_msg = make_wx_soup(soup,weather)
    # 推送 钉钉
    for url in dd_urls:
        push_dd(url,dd_msg)
    # 推送 微信群组
    push_wx_group(wx_msg[0],wx_msg[1])

if __name__ == '__main__':
    url = sys.argv[1] if len(sys.argv) > 1 else 'sqlite:///jobs.sqlite'
    scheduler.add_jobstore('sqlalchemy',url=url) 
    # 使用sqlite存储jobs table默认名为apscheduler_jobs
    print(get_time() + " 开始执行任务")
    scheduler.add_job(work, 'cron', day_of_week='0-6', hour=8, minute=00, second=00,id='data_id')
    scheduler.start()

```
