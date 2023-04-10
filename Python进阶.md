# Python进阶内容

高阶函数、装饰器--functools、生成器--functools,contextlib,with,greenlet,gevent,eventlet、描述符--werkzeug,bottle、元类、多线程--threading.RLock,daemon thread,Queue,logging,线程池。

## 高阶函数

> 以函数作为参数的函数：
>
> > ```python
> > def f(func, **params):
> >     pass
> > ```

### 内置的高阶函数：

#### sorted

> 对可迭代[^1]内容(如列表)排序，并返回排序结果，此操作不会对原内容做修改，而是返回新的内容

```python
sorted(iter, key=func, reverse=bool)  # 按照func方法对内容的每个元素进行操作，以此结果为依据进行排序，顺序默认为正序(从小到大)，在reverse=True时为反序(从大到小),如：
print(sorted([2, 4, -3, -1], key=abs))  # 按列表元素的绝对值对列表进行排序
>>> [-1, 2, -3, 4]
```

[^1]: 可迭代对象(Iterable):实现了iter()方法或getitem()方法的对象，可以通过for..in遍历的，如：List、String、Tuple、Dict、文件对象等，可通过 `isinstance(obj, Iterable)` 来判断

#### filter

> 对可迭代内容过滤，并返回一个filter对象(迭代器)，可使用 转换 来使用结果

```python
filter(func, iter)  # 按照func方法对内容的每个元素进行判断，True则保留，False则去除，得到想要的结果，如：
def select(i):
    if isinstance(i, int):
        return True
    else:
        return False

print(filter(select, ['time', 2, 'date', 12]))
>>> <filter object at 0x000001FD7882E808>
print(list(filter(select, ['time', 2, 'date', 12])))
>>> [2, 12]
```

#### map

> 对可迭代对象的各元素执行指定的方法，并返回一个map对象，可使用 转换 来使用结果

```python
map(func, iter)  # 按照func方法对内容的每个元素执行，获得结果，如：
def make(i):
    return i*2 if i > 1 else i-5

print(map(make, [3, 1, 4, -2, 5]))
>>> <map object at 0x00000222A2DD3F88>
print(list(map(make, [3, 1, 4, -2, 5])))
>>> [6, -4, 8, -7, 10]
```

#### reduce

> 需要从functools库导入
>
> 对可迭代对象的各元素执行指定的方法(在迭代执行过程中积累处理结果)，并返回最终结果

```python
reduce(func, iter)  # 按照func方法对内容的每个元素执行，获得最终结果，如：
from functools import reduce
def count(x, y):
    return x + y

print(reduce(count, [1, 2, 3, 4, -5, 6]))
>>> 11
```

#### partial

> 偏函数，通过原函数创建指定默认值的函数

```python
partial(func, param)  # 将func函数的默认值替换设置为param并返回新函数，如：
from functools import partial
def count(x, y):
    return x + y

new_count = partial(count, y=6)
print(new_count(3))
>>> 9
```

**附：**

##### 函数的参数

> 函数参数可包含：
>
> > 普通形参：a(传值时为必选实参)
> >
> > 默认值参数[实际也是普通形参]：a=2(传入时为可选实参)
> >
> > 位置参数：*args(以元组形式解包传参)
> >
> > 关键字参数：**kwags(以字典形式解包传参)

不同类别的参数在函数声明中有放置顺序：

普通形参，默认值参数，位置参数，关键字参数

- 默认值参数：

  ​	若以函数外变量为默认值参数赋值，默认值仅计算一次，当默认值为列表、字典或类实例等时，会产生与该规则不同的结果(以上次结果为输入)

- 位置参数：

  ​	必须在普通参数、默认值参数之后，将传入参数以元组形式解包，赋给对应位置的形参

- 关键字参数：

  ​	必须在普通参数、默认值参数、位置参数之后，传入参数以字典形式解包，键对应参数名来传值

### 匿名函数

​	从实际使用来说，它像是一种表达式，其实则是无函数名的函数，因此无法通过调用名重复使用

#### lambda

> 对参数进行操作并将结果返回

```python
list(map(lambda x: x * 2, [2, 3]))  # 将输入参数x乘以2得到结果后返回
>>> [4, 6]

def shape(a):
	return lambda: a ** 2  # 作为返回值

print(shape(2))
>>> <function shape.<locals>.<lambda> at 0x000001F78BA6D5E8>
print(shape(2)())
>>> 4
```

### 迭代器

> 可以通过next函数不断地获取其下一个值，直至返回 `StopIteration` 异常

所有可迭代对象(具备迭代能力的对象)均可通过 `iter` 函数去获取它的迭代器，可通过for循环访问：

```python
array = ['a', 'b', 'c']  # 可迭代对象
it = iter(array)  # 迭代器
next(it)  # 迭代
>>> 'a'
next(it)
>>> 'b'
```

所有迭代器均实现了 `__iter__` 和 `__next__` 方法：

```python
class Test:
	def __init__(self, a, b):
		self.a = a
		self.b = b
	
	def __iter__(self):
		return self
	
	def __next__(self):
		self.a += 1
		if self.a > self.b:
			raise StopIteration()
		return self.a
```

迭代器的类型：

```python
from collections.abc import Iterator
```

### 生成器

> 一种特殊的迭代器，只能迭代一遍，其元素在每次迭代时生成，并非所有元素保存在内存中

- 通过生成器表达式创建生成器：(类似于列表推导式)

  ```python
  yi = (x**x for x in range(1, 10))
  print(yi)
  >>> <generator object <genexpr> at 0x000001ED4E0498A0>
  ```

- 通过 `yield` 编写生成器函数：

   `yield` ：类似于 `return` ，但 `yield` 返回的是一个生成器，函数在运行到 `yield` 时获得一个元素，当再次运行到 `yield` 同理，直至结束

  ```python
  def yi(x, n=100):
  	while abs(x) < n:
  		yield x
  		x += x
  yi_1 = yi(2, 100)
  for item in yi_1:
      print(item)
  ```

### 装饰器

> 为函数添加额外的功能而不影响函数额主体功能，实现基础是函数可作为参数传递给另一个函数，一个函数可以将另一个函数作为返回值；本质上是一个函数

```python
from datetime import datetime
def log(func):
	def decorator(*args, **kwargs):
		print('log test ' + func.__name__ + 'has been called at ' + datetime.now().strftime('%Y-%m-%d %H:%M:%S'))
		return func(*args, **kwargs)
	return decorator
```

使用：

```python
@log  # @为Python语法糖
def add(a, b):
    return a + b
```

装饰后其本质相当于

```python
add = log(add)
add(a, b)
print(add.__name__)
>>> decorator
```

## 面向对象编程(OOP)

类(Class)|实例(Instance)：类是抽象模板，实例是根据类模板创建出来的具体的“对象”

- 抽象

  ​	对特定实例抽取共同的特征、行为形成一个抽象类型；在抽象类型中属性就相当于特征，行为就相当于方法

- 封装

  ​	用类将数据(属性)和基于数据的操作(方法)封装在一起(类中)，隐藏内部数据，对外提供公共的访问接口

- 继承

  ​	在子类中使用父类中定义的属性和方法作为子类的属性和方法，可以单继承或多继承

- 多态

  ​	一类事物可以有多种形态，与多态性不同，多态性指不同功能的方法可以使用相同的名字

#### Python类

> 关键词：`class` ，`object` 类是所有自定义类的父类，创建类需要继承父类 `object` (Python3时可以不写)
>
> 命名：大驼峰命名法 -- 每个单词第一个字母大写；私有属性用 `_*` / `__*` 开头

------

**Python中一切皆对象**：类本身是对象，类的实例也是对象，凡是可以通过一对 `()` 来调用的都是可调用对象

> 通过 `callable()` 函数可判断一个对象是否是可调用对象
>
> - 通常实例对象都是**不可**调用对象，但若在类中实现了 `__call__` 方法就可将实例对象转换为可调用对象

创建类时会自动创建它的内置类方法或属性，Python内置 `dir` 函数可查看对象的全部属性和方法：

```python
class Mine:
	"""人 -- 对象"""

	def __init__(self, name, sex, age, id):
		self.name = name  # 姓名
		self.sex = sex  # 性别
		self.age = age  # 年龄
		self.id = id  # id

	def __info(self):
		return f'姓名：{self.name}\t|性别：{self.sex}\t|年龄：{self.age}\t|id：{self.id}\t'
dir(Mine)  # 查看Mine类的全部属性和方法
>>> ['_Mine__info', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getstate__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
```

 **`self`** ：表示实例对象自身，类方法第一个参数必须表示实例化对象的引用，通常用 `self` 来表示，在类方法中使用类的属性/方法时可通过 `self.**` 的方式引用

**私有属性和方法**：Python约定一个下划线表示外部不应该直接调用，但仍可以调用到，两个下划线时外部调用不到(并不绝对)，双下划线约束的属性或方法可通过 `对象名._classname__attr` 的方式获取(强烈不推荐)，如 `sx._Mine__info()` 

**类属性**：可直接使用类访问，无需实例化，在类外只有公有的类属性(未在任何方法内的)才可被直接访问

**类方法**：可通过类名直接访问，无需实例化，用装饰器 `@classmethod` 装饰，第一个参数是类对象自身 `cls` (并非实例对象 `self` )，可访问类属性

**静态方法**：可不需要类或对象参数，用装饰器 `@staticmethod` 装饰，一般静态方法不需要类中参数参与，仅与类有一点关系，放在类中可更好地组织代码逻辑

| 类型     | 第一参数              | 内容中可引用                       |
| -------- | --------------------- | ---------------------------------- |
| 实例方法 | 强制为实例对象 `self` | 类属性、实例属性、类方法、实例方法 |
| 类方法   | 强制为类对象 `cls`    | 类属性、类方法                     |
| 静态方法 | 可无                  | 类属性                             |

------

```python
@property
def id(self):
   return self.__id
@id.setter
def id(self, id):
   self.__id = id
@id.deleter
def id(self):
   del self.__id
```

 **`@property`** 装饰器可将一个方法当作一个属性来使用，获得、修改对象的一个属性(必须先有`@property`装饰器，类必须继承 `object` 父类)

- 有 `@property` ：只读

- 有 `@property` 、 `@*.setter` ：可读可写

- 有 `@property` 、 `@*.setter` 、 `@*.deleter` ：可读可写可删

 **`__slots__`** 属性可用于限制绑定属性，定义一个实例对象后可以任意绑定属性，通过 `__slots__` 可将允许绑定的属性以元组形式定义，此属性不继承给子类，但当子类定义该属性后，父类中的此属性值也会被子类继承进去

------

-  `__init__` ：构造函数，初始化设置，创建实例时自动执行此方法，常用于新建实例时给对象绑定属性，当把类当作装饰器使用时， `__init__` 只能接受被装饰的函数这一个参数

   **注意： `__init__` 是在对象创建完成之后执行的，并不是用来创建对象的必备函数，创建对象的实际函数为 `__new__(cls, [...])` ，可以不必在自定义类中实现， `__new__` 中甚至可以指定是否执行 `__init__` **

-  `__new__(cls, [...])` ：为object类所具备，用于创建实例对象并返回该对象，必须有返回值如 `super().__new__(cls)` / `object.__new__(cls)`

-  `__del__` ：析构函数，释放对象时执行，即删除，销毁对象

-  `__repr__` ：获取对象的字符串形式表示(面向解释器，通常为合法Python代码)，转换时执行

-   `__str__` ：获取对象的字符串形式表示(面向用户，更简洁)

-  `__doc__` ：获取类的文档字符串(注释说明)

-  `__class__` ：类对象的类型

-  `__dict__` ：类的属性、信息组成的字典

-  `__name__` ：获得类名

-  `__module__` ：类所属的模块

-  `__bases__` ：类的所有父类组成的元组

-  `__getattribute__` ：获得对象属性值

-  `__getattr__` ：获得对象属性值

   | 异同点   | `__getattribute__(self, attr)`           | `__getattr__(self, attr)`          |
   | -------- | ---------------------------------------- | ---------------------------------- |
   | 调用条件 | 调用实例对象属性(无论属性是否存在)时触发 | 获取不存在的实例对象属性时触发     |
   | 参数     | 实例对象自身self，实例对象属性attr       | 实例对象自身self，实例对象属性attr |
   | 返回值   | 必须有                                   | 必须有                             |
   | 作用     | 控制访问权限                             | 访问不存在属性时进行自定义         |

    `__getattribute__` 比 `__getattr__` 优先调用，若调用 `__getattribute__` 有结果就不会再调用 `__getattr__` 

-  `__call__` ：让实例对象可调用，实例对象被调用时执行

   ```python
   def __call__(self):
       print(f'调用对象，给你我的名字')
       return self.name
   ```

-  `__iter__` ：让类成为可迭代对象

   ```python
   def __iter__(self):
       return self
   ```

-  `__next__` ：让类可通过 `for` 来循环遍历

   ```python
   def __next__(self):
       if self.data > 5:
           raise StopIteration
       else:
           self.data += 1
           return self.data
   ```

------

 **继承** 的多个类中具有同名方法时，子类调用此方法，可通过 `*.mro()` 来查看类的继承顺序，即调用方法的解析顺序，但这样的继承会使得程序的可理解性变差，因此更推荐设计时使用 `Mixin` 思想，即一个子类只继承一个主要的类，其他功能拆分出来，单独功能命名为一个 `类Mixin` 

-  `super()` ：超类(父类)的引用，常见于子类的 `__init__` 方法中对父类初始化方法的调用

## 多进程与多线程

### 多进程

> 多进程的模块： `multiprocessing` 	类： `Process` 

```python
# 1.导包
from multiprocessing import Process

# 2.创建进程
# Process类创建实例对象p，参数target为要执行的目标函数，参数args为目标函数的参数值(元组形式)
# 此对象为一个子任务，运行此子任务(start)时系统会创建一个子进程
p = Process(target=func, args=(tuple, ))

# 3.启动子进程
p.start()

# 4.等待子进程执行完毕后继续执行父进程
# 控制执行过程的手段，非必要
p.join()
```

- `os` 包中的 `getpid()` 方法可获得当前进程id

实例：

  ```python
  import time
  from multiprocessing import Process
  
  def make_add(x, y):
     time.sleep(1)
     print(f'取得{x}与{y}的和：{x+y}')
  
  def main():
     array = [(1, 2), (2, 3), (3, 4), (4, 5), (5, 6), (6, 7), (7, 8)]
     process = []
     for i in array:
        p = Process(target=make_add, args=i)
        process.append(p)
        p.start()
     for i in process:
        i.join()
  
  
  if __name__ == '__main__':
     main()
  ```

### 进程间通信

> 进程具有独立的运行空间
>
> 进程通信的模块： `multiprocessing` 
>
> 管道类： `Pipe` 		队列类： `Queue` 

Pipe:

```python
# 1.导包
from multiprocessing import Pipe

# 2.创建管道
# Pipe类的实例为元组，包含两个连接器，即一个管道的两“端”，参数bool为是否可双向
# 发用send(data)，收用recv()，关用close()
conn_1, conn_2 = Pipe(bool)

# 3.发送接收(不同进程中)
# 进程1：
conn_1.sent('hello')
# 进程2：
data = conn_2.recv()
```

实例：

```python
from multiprocessing import Process, Pipe
def send(p):
	conn_1, conn_2 = p
	for i in range(1, 11):
		conn_1.send(i)
		print(f'发出：{i}')
	conn_1.send('No')
	print(f'发完咧')

def recv(p):
	conn_1, conn_2 = p
	while True:
		data = conn_2.recv()
		print(f'收到：{data}')
		if data == 'No':
			print('收完咧')
			break

def main():
	p = Pipe(True)
	Process(target=send, args=(p,)).start()
	Process(target=recv, args=(p,)).start()

if __name__ == '__main__':
   main()
```

Queue:

> 通信过程实际传递的是对象的引用，若担心值被修改，建议采用不可修改的数据结构或一个对象的深拷贝

```python
# 1.导包
from multiprocessing import Queue

# 2.创建队列
# Queue类创建实例化对象queue，已包含必要的锁，参数maxsize为队列最大容量(限制能容纳的元素数量)
# 发用put()，收用get()，关用close()，判空用empty()、full()、qsize()
queue = Queue(maxsize)

# 3.发送接收(不同进程中)
# 进程1：参数block为是否非阻塞
queue.put(data, block=bool)
# 进程2：参数block为是否非阻塞，参数timeout为等待超时时间
queue.get(bolck=bool, timeout=time)
```

-  `get()` 后其他进程就接收不到此数据了

-  **block** 和 **timeout** 参数：

  | block | timeout | 操作                                                         |
  | ----- | ------- | ------------------------------------------------------------ |
  | False | 任意    | 遇到放满/取空时，直接引发queue.Full/Empty异常                |
  | True  | None    | 遇到放满/取空时，一直等待直到有数据被取出/放入时才执行       |
  | True  | n(秒)   | 遇到放满/取空时，等待n秒，若还放满/取空，则引发queue.Full/Empty异常 |

实例：

```python
from multiprocessing import Process, Queue
from _queue import Empty

def producer(q):
	i = 1
	while True:
		if q._maxsize - q.qsize() == 1:
			q.put('F', block=True)
			print('放满了')
			break
		else:
			q.put(i, block=True)
			print(f'放入:{i}')
			i += 1

def consumer(q):
	while True:
		try:
			data = q.get(block=False, timeout=0.5)
			print(f'收到：{data}')
			if data == 'F':
				print('收完了')
				break
		except Empty:
			pass

def main_queue():
	q = Queue(maxsize=10)
	Process(target=producer, args=(q,)).start()
	Process(target=consumer, args=(q,)).start()
```

