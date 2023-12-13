# Python 从命令行小游戏认识**面向对象** - 01创建对象



## 前言

学Python也有一年的时间了，但这一年里很少使用Python来写有关面向对象的脚本或者程序，以至于都快忘记了Python是一个面向对象的语言。最近实习比较闲，藉由此机会来巩固一下Python中面向对象的知识。接下来将通过边学边敲代码边做笔记的方式来完成这个系列。



## 1、什么是面向对象？

把一个事物抽象（abstract，动词）出来，并可以再调用这个抽象**类**，就是面向对象。

面向对象的基本特征：封装、继承、多态。

什么是抽象？

举个例子：

![image-20231130173414078](C:\Users\BVBYD\Desktop\学习文档\Python\image-20231130173414078.png)

这时候，我们通过抽象出来的**属性、方法**再去**new**一个**对象**，这时候就完成了创建对象，这个对象叫做**类的实例**。

有关面向对象的更多理论知识可以去参考其他大牛写的文章，小的这里理解还不是很深，就不献丑了。



## 2、使用Python来创建一个类

首先，新建一个python文件，将它命名为**gameEntity**。

这时，我们就可以新建类了。

gameEntity.py: 

```python
class Player:
    """这里是玩家的类"""
    name = ''
    level = 1
    exp = 0
    dmg = 0
    defence = 0
    hp = 100
    score = dmg * 3 + defence * 2 + hp

    def __init__(self, name, dmg, defence, level, exp, hp, score):
        self.name = name
        self.dmg = dmg
        self.defence = defence
        self.level = level
        self.exp = exp
        self.hp = hp
        self.score = score
```

- 第1行，我们声明了类 **Player**
- 第2行，是类文档字符串，用来阐述类是做什么的。使用 **Player.\__doc__**调用
- 第11行，定义了类的构造方法，每当去创建对象，都会调用这个方法，正如它的名字：**init**ialize。这是个特殊的方法。
- 第3~9行，定义了玩家的属性，分别是名字，等级，经验，伤害，防御力，血量，战力值。
- **self**代表类的实例，self 在定义类的方法时是必须有的，虽然在调用时不必传入相应的参数。相当于Java中的this关键字。



## 3、 创建对象

如何用Python通过类创建对象呢？非常简单！

我们创建第二个python文件：gameMethod.py

```python
class gameMethod():
	# 创建对象实例
	@classmethod
  def initPlayer():
      # 输入姓名
      name = input()
      # 随机初始化攻击和防御
      defence = random.randint(1, 10)
      damage = random.randint(1, 10)
      exp = 0
      hp = 100
      level = 1
      score = gameMethod.calScore(damage, defence, hp)
      p = player(name, damage, defence, level, exp, hp, score)
      # 输出角色初始信息
      gameMethod.printOriginalStatus(p.name, p.dmg, p.defence, p.hp, p.score)
      
	@classmethod
  def printOriginalStatus(name, dmg, defence, hp, score):
      print("角色创建成功！欢迎玩家" + name)
      print("你的初始攻击力为：", dmg)
      print("你的初始防御力为：", defence)
      print("你的初始生命值为：", hp)
      print("你的初始战力为：", score)
```

我们通过调用initPlayer这个方法，就完成了一次对象的创建！

```
输入：
bob
输出：
角色创建成功！欢迎玩家bob
你的初始攻击力为： 4
你的初始防御力为： 6
你的初始生命值为： 100
你的初始战力为： 124
```



## 参考文章：

[Python 面向对象 | 菜鸟教程 (runoob.com)](https://www.runoob.com/python/python-object.html)

[什么是面向对象，它的三个基本特征：封装、继承、多态_面向对象的三个_冰棍hfv的博客-CSDN博客](https://blog.csdn.net/weixin_51201930/article/details/122652397)