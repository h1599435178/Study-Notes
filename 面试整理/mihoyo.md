# 米哈游测开面经整理



# 八股

## 1. http请求和响应包括哪些内容，http请求有几种方式

### 1.1 请求报文

```header
GET http://www.example.com/ HTTP/1.1
Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cache-Control: max-age=0
Host: www.example.com
If-Modified-Since: Thu, 17 Oct 2019 07:18:26 GMT
If-None-Match: "3147526947+gzip"
Proxy-Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 xxx

param1=1&param2=2

```

1. 请求方式
2. URL
3. 协议版本
4. headers
5. 请求体

### 1.2 响应报文

```
HTTP/1.1 200 OK
Age: 529651
Cache-Control: max-age=604800
Connection: keep-alive
Content-Encoding: gzip
Content-Length: 648
Content-Type: text/html; charset=UTF-8
Date: Mon, 02 Nov 2020 17:53:39 GMT
Etag: "3147526947+ident+gzip"
Expires: Mon, 09 Nov 2020 17:53:39 GMT
Keep-Alive: timeout=4
Last-Modified: Thu, 17 Oct 2019 07:18:26 GMT
Proxy-Connection: keep-alive
Server: ECS (sjc/16DF)
Vary: Accept-Encoding
X-Cache: HIT

<!doctype html>
<html>
<head>
    <title>Example Domain</title>
	// 省略... 
</body>
</html>
```

1、协议版本

2、状态码

3、响应状态

4、响应头

5、响应体



### 1.3 http请求方式

**1、GET**

**2、POST**

**GET和POST的区别**（摘自[http请求中get和post方法的区别 ](https://zhuanlan.zhihu.com/p/275695831)）：

1. GET请求参数放在URL中，POST放在请求体中
2. POST比GET请求要慢，其中原因如下：

- 相同情况下，POST有更多的请求头，如content-type
- post请求的过程：
  （1）浏览器请求tcp连接（第一次握手）
  （2）服务器答应进行tcp连接（第二次握手）
  （3）浏览器确认，并发送post请求头（第三次握手，这个报文比较小，所以http会在此时进行第一次数据发送） 
  （4）服务器返回100 Continue响应
  （5）浏览器发送数据
  （6）服务器返回200 OK响应
  get请求的过程：
  （1）浏览器请求tcp连接（第一次握手）
  （2）服务器答应进行tcp连接（第二次握手）
  （3）浏览器确认，并发送get请求头和数据（第三次握手，这个报文比较小，所以http会在此时进行第一次数据发送）
  （4）服务器返回200 OK响应
  也就是说，目测get的总耗是post的2/3左右，这个口说无凭，网上已经有网友进行过测试。     
- 在获取重复数据时，GET比POST效率高，因为GET会把静态数据做缓存。

**面试怎么答**

1. POST不会直接显示在URL中，相对比较安全
2. POST发送的数据多，GET受URL长度限制（最多1024字节）
3. GET快
4. POST发送的数据类型多
5. 使用场景不一样，POST多用于向服务器提交，更改数据。GET一般用于查询，获取数据

**3、HEAD**

**4、PUT**

**5、DELETE**

**6、CONNECT**

**7、OPTIONS**

**8、TRACE**



## 2. 发一次GET请求有几个数据包

GET一个，GET可以一次发送Headers和Data。

POST只能分两次，先发Headers，等服务器返回100后再发送Data。



## 3. 常用的Linux命令

| 命令      | 功能                          |
| --------- | ----------------------------- |
| cd        | 导航                          |
| ls        | 列出文件和文件夹              |
| ll        | 列出文件及信息                |
| touch     | 创建文件                      |
| cp        | copy，复制文件或文件夹        |
| rm        | 删除文件                      |
| mv        | 移动文件                      |
| mkdir     | 创建文件夹                    |
| chmod     | 更改文件权限                  |
| vim       | 使用vim编辑文件               |
| cat       | 查看文件详情                  |
| ps        | 查看当前进程及PID             |
| which     | 查找PATH下的可执行文件        |
| whereis   | 查找所有文件                  |
| head/tail | 查看文件的前/后10行，-n可设置 |
| grep      | 匹配文件内容                  |
| whoami    | 显示当前用户名                |
| find      | 匹配正则搜索文件              |
| ifconfig  | 查看网络状态                  |

## 4. 进程与线程的区别

1. 进程是系统分配资源的最小单位，线程是系统调度的最小单位
2. 一个进程包含一个或多个线程
3. 多个进程间的内存是独立的，一个进程中的多个线程拥有共享内存
4. 进程挂了，不会影响其他进程，线程挂了会影响整个进程



## 5.InnoDB特点

- 支持ACID事务
- 行级锁定
- 读写阻塞与事务隔离级别相关
- 既能缓存索引又能缓存数据
- 支持外键
- InnoDB更消耗资源，读取速度没有MyISAM快
- 在InnoDB中存在着缓冲管理，通过缓冲池，将索引和数据全部缓存起来，加快查询的速度；
- 对于InnoDB类型的表，其数据的物理组织形式是聚簇表。所有的数据按照主键来组织。数据和索引放在一块，都位于B+数的叶子节点上；

### 	5.1 ACID：

​	原子性（Atomicity）

​	一致性（Consistency）

​	独立（隔离）性（Isolation）

​	持久性（Durability）



## 6. MySQL explain有哪些比较重要的信息？

### 6.1 EXPLAIN有什么用

使用EXPLAIN关键字可以模拟优化器执行SQL查询语句，从而知道MySQL是如何处理你的SQL语句的。分析你的查询语句或是表结构的性能瓶颈。

### 6.2 使用方式

EXPLAIN + SQL语句

```sql
EXPLAIN SELECT * FROM table
```

### 6.3 **通过EXPLAIN，我们可以分析出以下结果：**

- 表的读取顺序
- 数据读取操作的操作类型
- 哪些索引可以使用
- 哪些索引被实际使用
- 表之间的引用
- 每张表有多少行被优化器查询



## 7. 简述KCP协议

KCP（KuaiChuan Protocol）是一种用户空间的可靠 UDP 协议。它是由中国开发者林晓斌创建的，旨在提供一个在不可靠的 UDP 上实现可靠性传输的协议。



### 7.1 KCP的特性（来自GPT）

- **可靠性**：KCP 旨在通过在用户空间实现类似 TCP 的可靠性，但在 UDP 上运行，从而提供更低的延迟和更好的自定义配置。
- **拥塞控制：**KCP 也具有拥塞控制机制，但相对于 TCP 更加轻量，适应于高延迟、不稳定网络环境。
- **延迟：**KCP 旨在通过在 UDP 上提供可靠性来降低延迟，使其成为一种适用于实时应用的协议。
- **场景：**适用于对延迟和可靠性都有较高要求的场景，例如在线游戏、实时通信等。丢包率高的网络环境下KCP的优点才会显示出来。如果不丢包，那么TCP和KCP的效率不会差别很大，可能就是少了连接建立/关闭而已。一般来讲，在公网上传输的都可以使用，特别是对实时性要求较高的程序，如LOL。



## 8.http和https的区别

### 8.1 安全性

- http使用明文传输，https经过加密
- https 有TLS/SSL加密协议
- https在http上层添加了一层安全套接字层保护数据

### 8.2 端口

- http用80，https一般用443

### 8.3 证书

- https有机构颁发的CA证书，要钱

### 8.4 搜索引擎

- 理论上，搜索引擎会优先给https协议优化搜索排名（SEO）



## 9. 在Fiddler中，抓包http和https有什么区别

就是有个加密，可能会有加密乱码。 在HTTPS抓包时，你可能会看到SSL/TLS握手的过程，其中包括协商加密算法、交换密钥等步骤。



## 10. 状态码

HTTP标准的状态码一般是100-599

- 1XX（信息状态码）：一般是TCP握手时会发送
- 2XX（成功）：
  - 200 OK：请求成功，服务器成功处理请求
  - 201 Create： 请求成功，有一个新资源依据请求的需要而建立
  - 204 No Content： 服务器处理请求但无返回内容
- 3XX（重定向）
  - 301 Moved Permanently：请求资源永久移动到了新位置
  - 302 Found：资源临时从不同的URI响应请求
  - 304 Not Modified：资源未被修改，可以使用缓存的版本
- 4XX（客户端错误）
  - 400 Bad Request： 请求的语法错误
  - 401 Unauthorized： 身份验证出错、需要身份验证
  - 403 Forbidden： 服务器拒绝请求，无权限（？
  - 404 Not Found：服务器找不到请求的资源
- 5XX（服务器错误）
  - 500 Internal Server Error：服务器错误
  - 502 Bad Gateway： 网关或代理服务器收到无效响应
  - 503 Service Unavailable：服务器目前不可用（服务器更新、停服等）



## 11. JMeter 断言方法

1. 响应断言
2. Json断言
3. HTML断言
4. XPath断言

### 	11.1 其他断言方法

- 断言是否相等Equal/NotEqual

- 断言是否为真True/False

- 断言是否为空IsNone/IsNotNone

  等等



## 12. 向页面提交图片出错，从哪几个方面查错(Bug定位)

- 确保网络通畅
- 检查图片后缀名是否符合上传要求，文件头、尾是否正确，确认图片是否损坏
- 使用F12工具，查看响应状态及错误码
- 若无错误码，查看请求头的`Content-Type`是否为`multipart/form-data`
- 查看JS的响应控制台是否出错
- 检查前后端代码是否出错
- 检查图片大小是否超出后端限制
- 检查接口URL是否错误
- 如果有跨域，查看跨域的配置是否有问题
- 使用其他环境进行测试
- 向后端请求查看错误日志获取错误信息



## 13. 你是一名测试人员，有一项报错，你认为是后端同学的问题，后端同学觉得不是他的问题，要怎么解决

开放性，别太离谱即可

1. 分析自身
2. 重看项目需求
3. 指导复现
4. 沟通意见
5. 提供有力证据
6. 寻求同级/上级帮助



## 14. 如何优化SQL（来自知乎）

- 减少数据访问： 设置合理的字段类型，启用压缩，通过索引访问等减少磁盘IO
- 返回更少的数据： 只返回需要的字段和数据分页处理 减少磁盘io及网络io
- 减少交互次数： 批量DML操作，函数存储等减少数据连接次数
- 减少服务器CPU开销： 尽量减少数据库排序操作以及全表查询，减少cpu 内存占用
- 利用更多资源： 使用表分区，可以增加并行操作，更大限度利用cpu资源

总结三点:

- 最大化利用索引；
- 尽可能避免全表扫描；
- 减少无效数据的查询；







# 手撕

## 1. 去除字符串两边的空格

解法1：split函数

```python
str.split(' ')
```

解法2：递归

```python
def delspace(str1):
    arr = str1
    if str1[0] == ' ':
        arr = str1[1:]
        return delspace(arr)
    elif str1[-1] == ' ':
        arr = str1[:-1]
        return delspace(arr)
    else:
        return arr
```



## 2. 有一个数组，求其中两个数字乘积最大的组合，并设计测试用例

```python
arr = list(map(int, input().split()))
arr = sorted(arr,reverse=True)
print(arr[0]*arr[1])
```



## 3. 斐波那契数列

解法1：迭代

```python
def fibonacci():
    l = [1, 1]
    while l[-1]<100:
        l.append(l[-1]+l[-2])
    return l

print(fibonacci())
```

解法2： 迭代

```python
def fib():
    a = 1
    n = 0
    while n<100:
        tmp = n
        n=n+a
        a=tmp
        print(n, end=' ')

fib()
```

解法3： 递归

```python
import sys

print(sys.getrecursionlimit())
def fibb(n):
    sys.setrecursionlimit(10000)
    if n==1 or n==2:
        return 1
    else:
        return fibb(n-1)+fibb(n-2)

print(fibb(10))
```



## 4. SQL中WHERE和ORDER BY如何创建索引

```

```



## 5. 手撕冒泡排序

给定列表[5,2,1,6,3]，按从小到大方式排序，要求使用冒泡排序

```python
def bubble_sort(arr):
    # 外层控制需要多少次冒泡操作
    for i in range(len(arr)):
        # 内层控制相邻元素交换
        for j in range(len(arr) - i - 1):
            if arr[j] < arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
    return arr


arr = list(map(int, input().split()))
arr_after = bubble_sort(arr)
print(arr_after)
```



## 6. 手撕快排

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    else:
        pivot = arr[0]
        less = [x for x in arr[1:] if x <= pivot]
        greater = [x for x in arr[1:] if x > pivot]
        # 分治思想，递归
        return quick_sort(less) + [pivot] + quick_sort(greater)


arr = list(map(int, input().split()))
arr_after = quick_sort(arr)
print(arr_after)
```



## 7. 不用排序，不导包，怎么找到无序列表中的中位数

基于快速选择算法思想，需要认真看一下

```python
def partition(arr, low, high):
    """
    快速选择算法中的分区函数，用于确定枢轴的最终位置。
    """
    pivot = arr[high]
    i = low - 1

    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            # 交换元素，将小于等于枢轴的元素移动到左侧
            arr[i], arr[j] = arr[j], arr[i]

    # 将枢轴放到正确的位置
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1

def quick_select(arr, low, high, k):
    """
    使用快速选择算法找出列表中第 k 小的元素。
    """
    if low <= high:
        partition_index = partition(arr, low, high)

        if partition_index == k:
            # 如果枢轴的位置恰好是 k，说明找到了第 k 小的元素
            return arr[partition_index]
        elif partition_index < k:
            # 如果枢轴的位置小于 k，继续在右侧找第 k 小的元素
            return quick_select(arr, partition_index + 1, high, k)
        else:
            # 如果枢轴的位置大于 k，继续在左侧找第 k 小的元素
            return quick_select(arr, low, partition_index - 1, k)

def find_median(arr):
    """
    找出无序列表的中位数。
    """
    n = len(arr)
    if n % 2 == 1:
        # 如果列表长度为奇数，则中位数的下标为 n // 2
        return quick_select(arr, 0, n - 1, n // 2)
    else:
        # 如果列表长度为偶数，则中位数的下标为 (n // 2 - 1) 和 (n // 2) 的平均值
        left = quick_select(arr, 0, n - 1, n // 2 - 1)
        right = quick_select(arr, 0, n - 1, n // 2)
        return (left + right) / 2

# 示例用法
unsorted_list = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5]
median = find_median(unsorted_list)
print("中位数:", median)

```



## 8. 两个列表组成字典

1. 相邻两个组成Key和value拼接

```python
"""
    等长列表
"""
list1 = ['name', 'age', 'gender']
list2 = ['bob', 23, 'male']
# 1 zip方法
dic = dict(zip(list1, list2))
print(dic)

# 2 推导式
dic = {list1[i]: list2[i] for i in range(min(len(list1), len(list2)))}
print(dic)
```

```python
"""
    非等长列表
"""
list3 = ['name', 'age', 'gender']
list4 = ['bob', 23]
# 1 zip
dic = dict(zip(list3, list4))
print(dic)

# 2 推导式
dic = {list1[i]: list2[i] for i in range(min(len(list3), len(list4)))}
print(dic)

# 3 默认值替代
value = "Unknown"
dic = {k: list4[i] if i < len(list4) else value for i, k in enumerate(list3)}
print(dic)
```



2. 一组key，一组value拼接
