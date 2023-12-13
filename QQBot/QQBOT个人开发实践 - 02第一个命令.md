# QQBOT个人开发实践 - 02 BOT的第一个命令

## 前言

如今，对话机器人我认为大可分为两类，其中一种是`模型驱动型`，而QQBOT则是`命令驱动型`。

这篇就来做我们BOT的第一个命令。

## 尝试

首先，在QQBOT管理平台页面配置一个新的指令，如图：

![image-20231204094954784](C:\Users\BVBYD\Desktop\学习文档\QQBot\image-20231204094954784.png)

接着，根据上一篇文章的目录结构，我们在func文件夹中新建weather.py文件，并删除sendMsg.py文件。随后，在bot.py文件中添加以下代码：

```python
class MyClient(botpy.Client):
    async def on_at_message_create(self, message: Message):
        # 注册指令handler
        handlers = [hello, weather]
        for handler in handlers:
            if await handler(api=self.api, message=message):
                return
```

这段代码用作注册指令用，大概的实现原理与nonebot中@on_command和@handler用法差不多，都是通过装饰器来做一个闭包，然后执行命令。具体原理可以参考Python教程闭包和装饰器。

接下来，编写命令的方法：

```python
_log = logging.get_logger()
@Commands(name=("你好", "hello"))
async def hello(api: BotAPI, message: Message, params=None):
    _log.info(params)
    # 第一种用reply发送消息
    await message.reply(content='宝宝')
    # 第二种用api.post_message发送消息
    await api.post_message(channel_id=message.channel_id, content='宝宝', msg_id=message.id)
    return True
```

官方一共给出两种回复消息的方式，任选其一即可。

而命令驱动则是使用@Commands装饰器来完成。这里我们可以看一下@Commands的代码：

```python
class Commands:
    """
    指令装饰器

    Args:
      name (Union[tuple, str]): 字符串元组或单个字符串。
    """

    def __init__(self, name: Union[tuple, str]):
        self.commands = name

    def __call__(self, func):
        @wraps(func)
        async def decorated(*args, **kwargs):
            api: BotAPI = kwargs["api"]
            message: Message = kwargs["message"]
            if isinstance(self.commands, tuple):
                for command in self.commands:
                    if command in message.content:
                        # 分割指令后面的指令参数
                        params = message.content.split(command)[1].strip()
                        return await func(api=api, message=message, params=params)
            elif self.commands in message.content:
                # 分割指令后面的指令参数
                params = message.content.split(self.commands)[1].strip()
                return await func(api=api, message=message, params=params)
            else:
                return False

        return decorated
```

注意：

```python
@Commands(name=('a','b'))
```

装饰器中name只能传**字符串元组**或者**单个字符串**，其意义是配置多条命令或者单条命令。装饰器必须位于方法的**上面一行**。这条装饰器通过输入a或b都能触发下方的方法。

```python
@Commands(name='hello')
```

当name仅为一个字符串时，只有输入这个字符串才能触发。



## 第一次运行

此时我们运行bot.py测试一下功能

```bash
[INFO]	(client.py:159)_bot_login	[botpy] 登录机器人账号中...
[INFO]	(client.py:178)_bot_init	[botpy] 程序启动...
[INFO]	(connection.py:59)multi_run	[botpy] 最大并发连接数: 1, 启动会话数: 1
[INFO]	(client.py:236)bot_connect	[botpy] 会话启动中...
[INFO]	(gateway.py:110)ws_connect	[botpy] 启动中...
[INFO]	(gateway.py:136)ws_identify	[botpy] 鉴权中...
[INFO]	(gateway.py:80)on_message	[botpy] 机器人「KumaBot-测试中」启动成功！
[INFO]	(gateway.py:217)_send_heart	[botpy] 心跳维持启动...
```

此时我们在频道中输入`@BOT 你好`可以成功触发指令。

![image-20231205095336498](C:\Users\BVBYD\Desktop\学习文档\QQBot\image-20231205095336498.png)

这里带不带`/`都一样，因为在装饰器里规定了，接收到"你好"和"hello"就会触发指令。

![image-20231205095517975](C:\Users\BVBYD\Desktop\学习文档\QQBot\image-20231205095517975.png)

此时我们可以观察到控制台中通过logging函数输出了params，是我们的方法名hello。

```
[INFO]	(bot.py:23)hello	
```

有关log的打印我还没有开始研究，在未来不久会出一篇博客说明一下log的作用。

也有些同学会在控制台输出timeout，这可能是官方的一个bug，尚未解决，回头我也会研究一下这个问题

```
[WARNING]	(http.py:188)request	请求超时，请求连接: https://api.sgroup.qq.com/channels/******/messages
[WARNING]	(http.py:188)request	请求超时，请求连接: https://api.sgroup.qq.com/channels/******/messages
```



## 2023-12-6 编辑

放弃原生Py-QQBOT开发，bug过多，后续将转为Nonebot-Adapter-QQ继续进行开发。感谢开发者频道群老哥提出的建议和帮助



