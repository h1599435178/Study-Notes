# QQBOT个人开发实践 - 01文件结构与调通



## 前言

为什么选择开发QQBOT？

笔者在一年前开始接触舞萌DX，然后接触到舞萌DX查分器，期间也见到了许多优秀的开源BOT，比如Mirai，Diving-fish-MaiBot，Chieri，提比等。他们的开发者赋予了这些BOT丰富的功能以及能力，不论是对分数的查询，以及BOT的娱乐功能都非常有趣。于是笔者针对Diving-Fish的Github开源[mai-bot](https://github.com/Diving-Fish/mai-bot)进行魔改，随后也是对nonebot2进行了一波研究并部署到了自己的服务器上，也变成了我Python课的期末项目并获取了高分。

随着Bot的功能越来越多，其稳定运行在服务器上的时间也越来越长。然而好景不长，用来部署bot的另一个通信框架：[go-cqhttp(github)](https://github.com/Mrs4s/go-cqhttp)即将迎来它生命中的最后时刻，开发者Mrs4s宣布停止对cq的更新与维护。这对每一个参与开发的bot开发者来说都是一个噩梦，T厂也收到了不少民间开发者的谩骂。

但在不久后，T厂随即发布了官方的[QQBOT](https://q.qq.com/)。于是，开发者们又看到了一丝曙光，QQBOT的原生完美形态正式发布。

目前QQBOT的状态：

| 企业                  | **个人** |
| --------------------- | -------- |
| 群BOT/频道BOT/私聊BOT | 频道BOT  |

暂不清楚个人BOT是否会逐步开放功能，只能说期待一手好消息吧



## 一些信息

未来笔者将全程使用Python语言进行Bot开发（不排除转GO的可能性）。

此文章适合有nonebot2开发经验的开发者，不推荐**无编程经验**小白阅读。

**我的开发环境：**

- Python 3.11.5
- Win11
- PyCharm 2023.2.3

**此处省略注册机器人以及前期的配置流程，包括并不限于：**

- 注册账号
- 创建机器人
- 沙箱配置
- 功能配置
- 基础配置
- 白名单配置等

如需以上配置教程，请参阅[QQBOT官方文档]([QQ 机器人 | QQ机器人文档](https://bot.q.qq.com/wiki/#_1-阅读文档))

在开发时，我们用到最多的就是开发设置中所给出的四个参数：

其中需要接同SDK与BOT是下面三个`AppID`、`AppSecret`、`Token`。

![image-20231201161803490](C:\Users\BVBYD\Desktop\学习文档\QQBot\image-20231201161803490.png)



## 准备工作

### 安装QQBOT环境

官方要求 Python 版本 >= 3.8

打开终端，运行以下命令

```pip
pip install qq-botpy
```

或

```
pip3 install qq-botpy
```

出现以下字样代表安装成功

```
Successfully installed APScheduler-3.10.4 aiohttp-3.9.1 pytz-2023.3.post1 qq-botpy-1.1.2 tzlocal-5.2
```



## 编写代码

### 启动器

新建文件`bot.py`用于启动Bot

```python
import botpy

"""
    @appid: 你管理端页面的appid
    @token: 一样是管理端页面获取
"""

data = {
    'appid': '替换成你的AppID',
    'token': '替换成你的Token'
}


if __name__ == '__main__':
    intents = botpy.Intents(public_guild_messages=True)
    client = botpy.Client(intents=intents)
    client.run(data['appid'], data['token'])


```

### 官方提供的两种订阅事件的方式

#### 方法一：

```python
intents = botpy.Intents() 
client = MyClient(intents=intents)
```

在Intents中填入对应的[参数](https://bot.q.qq.com/wiki/develop/pythonsdk/#参数列表)

例子：

```python
intents = botpy.Intents(public_guild_messages=True, direct_message=True, guilds=True)
```

#### 方法二：

```python
intents = botpy.Intents.none()
```

然后打开对应的订阅([参数列表](https://bot.q.qq.com/wiki/develop/pythonsdk/#参数列表))

```python
intents.public_guild_messages=True
intents.direct_message=True
intents.guilds=True
```

说明：

方法二对应的快捷订阅方式为

1. 订阅所有事件

```python
intents = botpy.Intents.all()
```

2. 订阅所有的公域事件

```python
intents = botpy.Intents.default()
```



### 添加监听文件

新建`/func/sendMsg.py`文件，添加以下官方给出的代码：

```python
"""
    存放一些消息处理函数
"""
import botpy
from botpy.types.message import *

class MyClient(botpy.Client):
    async def on_at_message_create(self, message: Message):
        await self.api.post_message(channel_id=message.channel_id, content="content")

```



### 运行BOT

回到bot.py文件，点击运行

![image-20231202150958052](C:\Users\BVBYD\Desktop\学习文档\QQBot\image-20231202150958052.png)

输出以下字样，则代表BOT运行成功，此时在QQ频道页面@BOT发送消息

![image-20231202151048017](C:\Users\BVBYD\Desktop\学习文档\QQBot\image-20231202151048017.png)

测试成功！



## 写在最后

虽然官方BOT禁止了许多第三方BOT中有趣的功能，但是QQBOT无疑给开发者们带来了极大的便利，许多开发者也不用再冒着”请喝茶”的风险去做第三方BOT了。最后希望BOT社区繁荣发展吧！
