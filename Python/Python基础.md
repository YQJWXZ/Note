# Python基础

## 1. 字面量

### 1.1 常用的值类型
1. 数字(Number)
2. 字符串(String)
3. 列表(List)  有序的可变序列
4. 元组(Tuple)  有序的不可变序列
5. 集合(Set)  无序不重复集合
6. 字典(Dictionary)  无序Key-Value集合

## 2. 变量

## 3. 数据类型
> type(被查看类型的数据)

1. 数据类型转换
* int(x)
* float(x)
* str(x)

## 4. 标识符

## 5. 运算符

## 6. 字符串

1. 字符串的三种定义方式
* 单引号定义 ``name = '黑马程序员'``
* 双引号定义 ``name = "黑马程序员"``
* 三引号定义 ``name = """黑马程序员"""``
> 使用转义字符\解除引号的效用

2. 字符串的拼接

3. 字符串格式化

```python
name = "黑马程序员"
message = "学IT就来 %s" % name  # 多个变量占位，变量要用括号括起来，并按照占位的顺序填入
print(message)

s_ = '''
%表示我要占位
s表示将变量变成字符串放入占位的地方
'''

```
4. 格式化数字精度控制
> m.n

m: 控制宽度
.n: 控制小数点精度，会进行小数的四舍五入

5. 快速格式化字符串
> f"内容{变量}"

## 7. 数据输入
> input()

## 8. 判断语句

## 9. 循环语句
1. range语句
* range(num) ``获取一个从0开始，到num结束的数字序列(不含num本身)``
* range(num1, num2) ``获取一个从num1开始，到num2结束的数字序列(不含num2本身)``
* range(num1, num2, step) 

## 10. 函数
```python
def 函数名(传入参数):
    函数体
    return 返回值
```

1. 函数说明文档
```python
def fun(x, y):
    """
    函数说明：
    :param x: 形参x的说明
    :param y: 形参y的说明
    :return: 返回值的说明
    """
    函数体
    return 返回值
```

## 11. 数据容器

1. 列表(list)
> 列表可以一次存储多个数据，且可以为不同的数据类型，支持嵌套
* 查找元素在列表的索引 ``my_list.index("hello")``
* 插入元素 ``my_list.insert(1, "hello")``
* 追加单个元素 ``my_list.append(4)``
* 追加一批元素 ``my_list.extend([4,5,6])``
* 删除元素 ``del my_list[2] || my_list.pop(2)``  
* 删除某元素在列表中的第一个匹配项 ``my_list.remove("hello")``
* 清空列表内容 ``my_list.clear()``
* 统计某元素在列表中的数量 ``my_list.count("hello")``
* 统计列表中的全部元素数量 ``my_list.len()``

2. 元组(tuple)
> 变量名称 = tuple()
> 
> 元组如果只有一个数据，这个数据后面要加逗号  t = ('Hello', )

3. 字符串
> 同元组一样，字符串是一个无法修改的数据容器
* 通过下表索引取值 ``my_str[ ]``
    * 从前往后，下标从0开始
    * 从后往前，下标从-1开始
* 字符串的替换 ``my_str.replace("it", "程序")``
* 字符串的分割 ``my_str.split(分隔符)``
* 字符串的规整操作
  * 去前后空格 ``my_str.strip()``
  * 去前后指定字符串 ``my_str.strip(字符串)``
* 统计字符串中某字符串的出现次数 ``my_str.count(字符串)``
* 统计字符串的长度 ``my_str.len()``

4. 序列
> 内容连续、有序，可使用下标索引的一类数据容器
>
> 切片：从一个序列中，取出一个子序列
> 
> 语法：序列[起始下标:结束下标:步长]

5. 集合(set)
> 变量名称 = set()
>
> 集合是无序的，去重，不支持下标索引访问
* 添加元素 ``my_set.add("Python")``
* 移除元素 ``my_set.remove("Pyhton")``
* 从集合中随机取出元素 ``my_set.pop()``
* 清空集合 ``my_set.clear()``
* 取出两个集合的差集 ``集合1.difference(集合2) 集合1有而集合2没有的``、
* 消除2个集合的差集 ``集合1.difference_update(集合2) 在集合1内，删除和集合2相同的元素``
* 2个集合合并 ``集合1.union(集合2) 将集合1和集合2组合成一个新集合``

6. 字典(dict)
> {key:value, key:value, ...., key:value}

* 嵌套字典
* 从嵌套字典中获取数据
* 获取指定Key对应的Value值 ``字典[Key]``
* 添加、更新、删除(pop)元素、清空字典
* 获取全部的Key ``字典.keys()``

## 12. 函数的多返回值
* 按照返回值的顺序，写对应顺序的多个变量接收即可
* 变量之间用逗号隔开
* 支持不同类型的数据return
```python
def test_return():
    return 1, "Hello", True

x, y, z = test_return()
print(x)
print(y)
print(z)
```

## 13. 函数的多种传参

1. 位置参数：调用函数时根据函数定义的参数位置来传递参数
```python
def user_info(name, age, gender):
    print("xxxxxx")

user_info('TOM', 20, '男')
```

2. 关键字参数：函数调用时通过"键=值"形式传递参数
```python
def user_info(name, age, gender):
    print("xxxxx")

# 关键字传参
user_info(name="小明", age=20, gender="男")

# 可以不按照固定顺序
# 可以和位置参数混用，位置参数必须在前，且匹配参数顺序
```

3. 缺省参数：用于定义函数，为参数提供默认值
```python
def user_info(name, age, gender='男'):
    print("xxxxxx")

user_info('TOM', 20)
user_info('Rose', 18, '女')
```

4. 不定长参数：可变参数，用于不确定调用的时候会传递多少个参数
* 位置传递
```python
def user_info(*args):
    print(args)

# 传进的所有参数都会被args变量收集，它会根据传进参数的位置合并为一个元素，args是元组类型，这就是位置传递
    
user_info('TOM')
user_info('TOM', 18)
```
* 关键字传递
```python
def user_info(**kwargs):
    print(kwargs)

# 参数是"键=值"形式的情况下，所有的"键=值"都会被kwargs接受，同时会根据"键=值"组成字典
    
user_info(name='TOM', age=18, id=110)
```

5. 匿名函数
* 函数作为参数传递  ``计算逻辑的传递，而非数据的传递``
```python
def test_func(compute):
    result = compute(1, 2)
    print(result)

def compute(x, y):
    return x + y

test_func(compute)
```
* lambda匿名函数
> lambda 传入参数: 函数体(一行代码)
```python
def test_func(compute):
    result = compute(1, 2)
    print(result)

test_func(lambda x, y: x + y)
```

## 14. 文件

1. 文件的读取操作
* 打开文件 open(name, mode, encoding) ``f=open('python.txt', 'r', encoding='UTF-8')``
* 读取文件
  * .read(num)  ``读取到num个字节的结果``
  * .readlines()  ``一次性读取全部内容，返回一个列表``
  * .readline()  ``一次读取一行内容``
* 关闭文件 close()
* with open语法
```python
with open("python.txt", "r")as f:
    f.readlines()

# 通过with open的语句块中对文件进行操作
# 可以在操作完成后自动关闭close文件，避免遗忘掉close方法
```

2. 文件的写出操作 ``f=open('python.txt', 'w', encoding='UTF-8')``
* 文件写入 f.write('hello world')
* 内容刷新 f.flush()
> 直接调用write，内容并未真正写入文件，而是会积攒在程序的内存中，称之为缓冲区
> 
> 当调用flush的时候，内容会真正写入文件
> 
> 这样做是避免频繁的操作硬盘，导致效率下降

3. 文件的追加操作 ``f=open('python.txt', 'a', encoding='UTF-8')``

## 15. 异常

1. 捕获异常
```
try:
    可能发生错误的代码
except NameError as e:
    如果出现异常执行的代码
[else:]
    如果没有异常要执行的代码
[finally:]
    无论是否异常都要执行的代码
```

2. 异常的传递性

## 16. 模块
> [from 模块名] import [模块 | 类 | 变量 | 函数 | *] [as 别名]

1. 自定义模块

2. __main__变量：只有当程序是直接执行的才会进入if内部, 如果是被导入的, 则if无法进入

3. __all__变量: 可以控制import *的时候那些功能是可以导入的

## 17. 包

> 从物理上看，包就是一个文件夹，在该文件夹下包含了一个__init__.py文件，该文件夹可用于包含多个模块文件
> 
> 从逻辑上看，包的本质依然是模块

1. 第三方包
* 科学计算 ``numpy``
* 数据分析 ``pandas``
* 大数据计算 ``pyspark、apache-flink``
* 图形可视化 ``matplotlib、pyecharts``
* 人工智能 ``tensorflow``

2. 安装第三方包
> pip install 第三方包

## 18. json数据
> Python数据转化为json数据: json.dumps(data, ensure_ascii=False)
> 
> json数据转化为Python数据：json.loads(data)
```python
import json

data = [{"name": "张大山", "age": 11}, {"name": "王大锤", "age": 13}, {"name": "赵小虎", "age": 16},]

json_str = json.dumps(data, ensure_ascii= False) 
print(type(json_str))
print(json_str)
```

## 19. 对象

## 20. 类
```
class 类名称:
    类的属性

    类的行为

创建对象类：
    对象 = 类名称
```
```python
class Student:
    name = None
    age = None

# self：表示类对象本身的意思，只有通过self，成员方法才能访问类的成员变量
# self出现在形参列表中，但是不占用参数位置，无需理会
    def say_hi(self):
        print(f"Hello, {self.name}")


stu = Student()
stu.name = "World"
stu.say_hi()
```

1. 成员方法的定义语法
```
def 方法名(self, 形参1, ......, 形参N):
    方法体
```

2. 构造方法
> __init__()方法，称之为构造方法
> 
> 在创建类对象(构造类)的时候，会自动执行，将传入参数自动传递给__init__方法使用
```python
class Student:
    # name = None
    # age = None
    # tel = None

    def __init__(self, name, age, tel):
        self.name = name
        self.age = age
        self.tel = tel

stu = Student("World", 31, "124364575676")
```

3. 魔术方法：内置的类方法
* __init__  构造方法
* __str__  字符串方法
* __lt__  小于大于符号比较
* __le__  小于等于、大于等于符号比较
* __eq__  =符号比较

## 21. 封装

1. 私有成员
> 私有成员变量：变量名以__开头(2个下划线)
> 
> 私有成员方法：方法名以__开头(2个下划线)

2. 私有成员无法被类对象使用，但是可以被其他的成员使用

## 22. 继承
```
class 类名(父类名1, 父类名2, ...., 父类名N):

  类内容体
```

1. pass关键字
> pass是占位语句，用来保证函数(方法)或类定义的完整性，表示无内容，空的意思
```python
class MyPhone(Phone, NFCReader, RemoteControl):
    pass
```

2. 复写和使用父类成员

调用父类同名成员：``使用被复写的父类的成员``
```
方式1：
  父类名.成员变量
  父类名.成员方法(self)
  
方式2：
  super().成员变量
  super().成员方法()
```

## 23. 类型注解

1. 变量的类型注解
> 变量: 类型

``元组类型设置类型详细注解，需要将每一个元素都标记出来``
> my_tuple: tuple[str, int, bool] = ("world", 666, True)

``字典类型设置类型详细注解，需要2个类型，第一个是Key，第二个是Value``
> my_dict: dict[str, int] = {"World", 666}

2. 注释中的类型注解
``# type: 类型``

3. 函数(方法)的类型注解
* 形参注解 
```
def 函数方法名(形参名：类型, 形参名: 类型,.....):
  pass
```
* 返回值注解
```
def 函数方法名(形参: 类型, ....., 形参: 类型) -> 返回值类型:
  pass
```

4. Union类型定义联合类型注解
```python
from typing import Union

my_list: list[Union[str, int]] = [1, 2, "world", "hello"]
my_dict: dict[str, Union[str, int]] = {"name": "world", "age": 31}

def func(data: Union[int, str]) -> Union[int, str]:
    pass
```

## 24. 多态

## 25. 抽象类(接口)
* 父类用来确定有哪些方法
* 具体的方法实现，由子类自行决定 
```python
class Animal:
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        print("汪汪汪")

class Cat(Animal):
    def speak(self):
        print("喵喵喵")
```

## 26. SQL

1. 数据操作
```python
from pymysql import Connection

# 获取到MySQL数据库的连接对象
conn = Connection(
  host='localhost',
  port=3306,
  user='root'
  password='123456'
)

# 获取游标对象
cursor = conn.cursor()
conn.select_db("test")

# 使用游标对象，执行sql语句
cursor.execute("CREATE TABLE test_pymysql(id INT, info VARCHAR(255))")

# 关闭到数据库的连接
conn.close()
```

2. 数据插入
```python
from pymysql import Connection

# 获取到MySQL数据库的连接对象
conn = Connection(
  host='localhost',
  port=3306,
  user='root'
  password='123456'
  autocommit=True  # 设置为自动提交
)

# 获取游标对象
cursor = conn.cursor()
conn.select_db("test")


cursor.execute("insert into test values(10001, 'World', 31, '男')")

# 通过commit确认
conn.commit()

# 关闭到数据库的连接
conn.close()
```

## 27. PySpark

1. 构建PySpark执行环境入口对象  ``类SparkContext的类对象``
```python
from pyspark import SparkConf, SparkContext

# 创建SparkConf类对象
conf = SparkConf.setMaster("local[*]").setAppName("test_spark_app")

# 基于SparkConf类对象创建SparkContext类对象
sc = SparkContext(conf = conf)

print(sc.version)

# 停止SparkContext对象的运行
sc.stop()
```

2. 数据输入
> PySpark支持多种数据的输入，在输入完成后，都会得到一个RDD类的对象
* RDD对象：弹性分布式数据集
* Python数据容器转RDD对象
```python
from pyspark import SparkConf,SparkContext
import os
os.environ['PYSPARK_PYHON'] = "D:/dev/python/python.exe"

conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")

sc = SparkContext(conf = conf)

rdd = sc.parallelize(数据容器对象)

# 输出RDD的内容
print(rdd.collect())
```
* 读取文件转RDD对象
```python
from pyspark import SparkConf,SparkContext
import os
os.environ['PYSPARK_PYHON'] = "D:/dev/python/python.exe"

conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")

sc = SparkContext(conf = conf)

rdd = sc.textFile(文件路径)

# 输出RDD的内容
print(rdd.collect())
```

3. 数据计算
* map方法(算子): 将RDD的数据一条条处理(处理的逻辑：基于map算子中接收的处理函数), 返回新的RDD  ``rdd.map(func)``
* flatMap方法: 对RDD执行map操作，然后进行解除嵌套操作
* reduceByKey算子：针对KV型RDD，自动按照key分组，然后根据提供的聚合逻辑，完成组内数据(value)的聚合操作  ``rdd.reduceByKey(func)``
* filter算子：过滤想要的数据进行保留 ``rdd.filter(func)``
* distinct算子：对RDD数据进行去重，返回新的RDD ``rdd.distinct()``
* sortBy算子：对RDD数据进行排序，基于你指定的排序依据 ``rdd.sortBy(func, ascending=False, numPartitios=1)``
> func：(T) -> U: 告知按照热点的中的哪个数据进行排序，比如lambda x: x[1] 表示按照rdd中的第二列元素进行排序

4. 数据输出
* 输出为Python对象
  * collect算子：将RDD各个分区内的数据，统一收集到Driver中，形成一个list对象
  * reduce算子：对RDD数据集按照你传入的逻辑进行聚合
  * take算子：取RDD的前N个元素，组合成list返回
  * count算子
* 输出到文件
  * saveAsTextFile算子：将RDD的数据写入到文本文件中  ``rdd.saveAsTextFile("D:/output")``
> 输出的结果是一个文件夹，有几个分区就输出多少个结果文件
```
修改rdd分区：
法一：SparkConf对象设置属性全局并行度为1
conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
conf.set("spark.default.parallelism", "1")
sc = SprakContext(conf=conf)

法二：创建RDD的时候设置(parallelize方法传入numSlices参数为1)
rdd1 = sc.parallelize([1, 2, 3, 4, 5], numSlices=1)
rdd2 = sc.parallelize([1, 2, 3, 4, 5], 1)
```

## 28. 闭包

> 在函数嵌套的前提下，内部函数使用了外部函数的变量，并且外部函数返回了内部函数，我们把这个使用外部函数变量的内部函数称为闭包

优点：
* 无需定义全局变量就可实现通过函数，持续的访问、修改某个值
* 闭包使用的变量的所用于在函数内，难以被错误的调用修改
```python
def account_create(initial_amount=0):
    def atm(num, deposit=True):
        nonlocal initial_amount
        if deposit:
            initial_amount+=num
            print(f"存款:+{num}, 账户余额:{initial_amount}")
        else:
            initial_amount-=num
            print(f"取款:-{num}, 账户余额:{initial_amount}")
    return atm

fn = account_create()
fn(300)
fn(200)
fn(300, False)
```

## 29. 装饰器

> 装饰器其实也是一种闭包，其功能就是在不破坏目标函数原有的代码和功能的前提下，为目标函数增加新功能
```python
def outer(func):
    def inner():
        print("睡觉了")
        func()
        print("起床了")
    return inner

@outer
def sleep():
    import random
    import time
    print("睡觉中....")
    time.sleep(random.randint(1, 5))

sleep()
```

## 30. 多线程
> threading.Thread([group [, target, [, name [, args [, kwargs]]]]])
```python
import time
import threading

def sing():
    while True:
        print("在唱歌")
        time.sleep(1)

def dance(msg):
    while True:
        print(msg)
        time.sleep(1)

if __name__ == '__main__':
    sing_thread = threading.Thread(target=sing)
    dance_thread = threading.Thread(target=dance, args="在跳舞")
    # dance_thread = threading.Thread(target=dance, kwargs = {"msg":"在跳舞"})
    sing_thread.start()
    dance_thread.start()
```

## 31. 网络编程

1. Socket服务端编程
```python
import socket

# 创建socket对象
socket_server = socket.socket()

# 绑定socket_server到指定ip和地址
socket_server.bind("localhost", 8888)

# 服务端开始监听端口
socket_server.listen(1)

# 接收客户端连接，获得连接对象
conn, address = socket_server.accept()
print(f"接收到客户端连接，连接来自:{address}")

# 客户端连接后，通过recv方法，接收客户端发送的消息
while True:
    data = conn.recv(1024).decode("UTF-8") # recv方法的返回值是字节数组，可以通过decode方法使用UTF-8解码为字符串
    if data == 'exit'
        break
    print("接收到发送来的数据："{data})

# 回复消息
msg = input("你好").encode("UTF-8")
conn.send(msg)

# 关闭连接
conn.close()
socket_server.close()
```

2. Socket客户端编程
```python
import socket

# 创建socket对象
socket_client = socket.socket()

# 连接到服务端
socket_client.connect("localhost", 8888)

# 发送消息
while True:
    send_msg = input("请输入要发送的消息")
    if send_msg = 'exit':
        # 通过特殊标记来确保可以退出无限循环
        break
    socket_client.send(send_msg.encode("UTF-8"))

# 接收返回消息
while True:
    send_msg = input("请输入要发送的消息").encode("UTF-8")
    socket_client.send(send_msg)
    
    recv_data = socket_client.recv(1024)
    # recv方法是阻塞式的，即不接收到返回，就卡在这里等待
    print("服务端回复消息为：", recv_data.decode("UTF-8"))

# 关闭连接
socket_client.close()
```

## 32. 正则表达式
> import re
* match方法: 从头匹配  ``re.match(匹配规则，被匹配字符串)``
* search方法：搜索匹配  ``re.search(匹配规则，被匹配字符串)``
* findall方法: 搜索全部匹配  ``re.findall(匹配规则，被匹配字符串)``

1. 元字符匹配

```python
import re

s = "heima @@ python cast3"
re.findall(r'[a-zA-Z]',s)
```

## 33. 递归

1. os模块
* os.listdir: 列出指定目录下的内容
* os.path.isdir: 判断给定路径是否是文件夹，是返回True
* os.path.exists: 判断给定路径是否存在，存在返回True 