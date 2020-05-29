# python

## 零、输入/输出 input/print

占位符：%s %d %f %x %%，%s把任意数据类型转为字符串处理。

## 一、基础
### 1.数据类型 变量 计算 转换
整型/浮点型/字符串（str）/字节类型（bytes）/布尔型/空值

布尔值：True/False；布尔运算：and/or/not ><=

空值None是python的特殊值·

列表/字典/自定义数据类型

python是动态语言，即变量本身的类型不固定，仅是一个标记。

python没有常量，只有约定大写变量为常量，但并不完全禁止改动。

```py
“”“
多行注释
”“”
demo = '\t\n\u32'   # 字符类型           
demo = r''          # 字符类型，表示''内部的字符串不转译
demo = b'ABC'       # 字节类型，其每个字符都只占一个字节
# 动态语言：变量就是变量，没有类型及其检查，指向的内容有类型

a = ‘abc’
b = a
a = ‘efg’   # b仍然为‘abc’
# python赋值语句可以理解为取右值的地址给左值？

result = 10 / 3     # result是浮点数，即使恰好整除
result = 10 // 3    # result是整数，//称为地板除
result = 10 % 3     # 取余数

ival = int(demo)    # str转int
demo = str(ival)    # int转str
```
### 2.字符编码
python3中，字符串以Unicode编码。
ord()函数获取字符的整数表示，char()函数吧编码转成对应字符。
可以使用十六进制编写字符，如'\u4e2d\u6587' 中文。

终坚持使用UTF-8编码对str和bytes进行转换，避免乱码。
对代码源文件指定用utf-8编码（编辑器也要设置UTF-8 without BOM编码才行）：
```py
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

'abc'.encode('ascii')   # b'abc'
# 纯英文str可以用ASCII编码为bytes，内容是一样的;
# 含有中文的str可以用UTF-8编码为bytes;
# 含有中文的str无法用ASCII编码，因为中文编码的范围超过了ASCII编码的范围，Python会报错。在bytes中，无法显示为ASCII字符的字节，用\x##显示;

b'efg'.decode('ascii')  # 'efg'
# 从网络或磁盘上读取了字节流，读到的数据是bytes
# 要把bytes变为str，用decode()
# 如果bytes中包含无法解码的字节，decode()报错(可用errors='ignore'忽略错误的字节)

len('abc')      # 3，计算字符数量
len('中文'.encode('utf-8')) # 6，一个中文字符常占用3个字节。

'{0}, {1}! {2:.2f}'.format('hi', 'work', 3.14)  # {n} 占位符
# hi, work! 3.14
```

### 3.列表 list 及 元组 tuple
元组 tuple 初始化后不可修改，其他同list，比list更安全，尽量使用tuple
```py
names = ['jim', 'sam', 'wim']   # list类型
names[0]        # 'jim'，确保索引不越界，最后一个是len-1
names[-1]       # 'wim'
len[names]      # 3
names.append('aren')    # 追加
names.insert(1, 'li')   # 插入
names.pop()             # 末尾弹出
names.pop(1)            # 指定弹出

items = ['jim', 23, True, ['another'], names]
items[4][0]     # 'jim'

names = ('jim', 'sam', 'wim')   # tuple类型
items = ()      # 空元组
items = (1)     # 整数1
items = (1,)    # 一元组
items = (1, 2, [3, 4])  # 可变的list的元组
```

### 4.字典 dict 及 集合 set
```py
mydict = {'jim': 23, 'sam': 12, 'wim': 53}
mydict['jim']           # 23
mydict['liz']           # 异常报错
mydict.get('liz')       # None
mydict.get('liz', -1)   # -1
mydict.pop('jim')       # 23, 删除
# dict 内部存放顺序和放入顺序无关
# dict的key必须是不可变对象，比如：list不可作为key
# 内部使用哈希算法计算value的存储位置

myset = set([23, 12, 53])   # set是不重复的key的无序的集合，用一个list初始化
myset.add(1)
myset.remove(12)
```
### 5.python的可变与不可变对象
变量指向对象，可变对象可变，变后还是同一对象；不可变对象一般通过运算得到另一对象；

不变对象的数据不会被修改，多任务环境不需要加锁。

None也是不变对象；
### 6.顺序语句
1. if-elif-else
2. for-in
3. break/continue

```py
if a > 100:
    print('a > 100')
elif a > 10
    print('a > 10')

for item in items:  # items可以是list tuple dict set等
    print(item)

for x in range(10): # 内置range函数，生成[1, 10]整数序列
    print(x)
```

### 7.函数
python有很多内置函数，如abs(num), max(num, ...), 类型转换函数int/float/str/bool。
可以使用help(abs)查看帮助信息。
传入的参数的数量或类型不对，报错TypeError。
函数名是指向函数对象的引用，可以赋值给一个变量。

```py
def myfun(x，y=2):      # y提供默认值
    if not isinstance(x, (int, float)):     # type check
        raise TypeError('bad operand type')
    print(x)
    return x
    #return             # =return None
    # return x, y       # 返回多个值，实际是返回一个tuple类型，只不过省略了括号，且多个值可以被tuple赋值

def myfun(mylist=[]):
# 多次调用myfun()导致默认参数的值就变了，因为默认参数是一个变量对象
# 默认值一般指向不变对象，需要指向可变对象时，使用默认值None
    mylist.append('demo')
    return 

# 如果def myfun(list_or_tuple), 调用可简化省略一层括号；
#   调用时如果入参是list/tuple，前面加*，表示把list/tuple的所有元素作为可变参数传入。

# 如果def myfun(*myvar):            # 可变参数，可以传入0个或任意个参数，myvar当作tuple处理

# 如果def myfun(name, age, **kw):   # name,age是必选参数； kw是关键字参数，函数可以接受更多的参数，以key=value的形式的。

# 如果def myfun(name, age, *, city, job)        # *后的参数是命名关键字参数，如果是*myvar，则不用*做间隔符号
#   命名关键字参数必须传入参数名；必选参数是按位置来的必填参数；缺少*，python无法解析命名关键字参数和必选参数；
#   命名关键字参数常用默认值，简化调用

# 函数参数的顺序必须是必选参数、默认参数、可变参数、命名关键字参数、关键字参数
def myfun(name, age, loc='bj', *args, city, job, **kw):
    return 0
# 通过tuple/dict类型，可调用myfun函数

# 对于任意函数都可以通过def func(*args, **kw)的形式调用；

def myfun():        # 定义一个空函数
    pass            # pass是占位符

if age > 18:
    pass

# python函数允许递归，但要注意栈溢出问题
# 可以通过尾递归解决栈溢出问题，尾递归会被优化成循环处理
#   尾递归要求return语句返回调用本身或不包含表达式的不变对象。
def myfunc(n):
    return myfunc_iter(n, 1)
def myfunc_iter(num, product):
    if num == 1:
        return product
    return myfunc_iter(num - 1, num * product)
```

### 8.高级特性
python认为代码越少越好。越少，开发效率才能越高。python的高级特性就是为了少写代码。
```py
# 切片
listobj[1:3]       # 取从下标1开始，到下标3前止的元素切片
listobj[:3]        # 取从下标0开始，到下标3前止的元素切片（0可省略）
listobj[-3:-1]     # 从下标-3开始，到下标-1前止的元素切片
listobj[-3:]       # 从下标-3开始，到最后一个的元素切片
listobj[:10:2]     # 前10个元素，每两个取一个
listobj[::2]       # 所有元素，每两个取一个
listobj[:]         # list复制
(3, 2, 4, 5)[:3]    # tuple做切片，仍为tuple
'abcdefg'[:3]       # 字符串切片，仍为字符串（python没有substring操作，只有切片）

# 迭代
for item in listobj
for key in dictobj
for value in dictobj
for key, value in dictobj.items()
for ch in 'abc'
#   python可迭代对象判定
from collections import Iterable
isinstance(obj, Iterable)   #  返回True则可迭代
#   list转化索引-元素对
for idx, item in enumerate(listobj)
for x, y in [(1, 2), (2, 2), (3, 1)]

# list comprehensions 列表补全表达式
list(range(1, 11))  # 补全为从1到11之前的列表
[x * x for x in range(1, 11)]   # 以for间隔的表达式和循环体，创建list
[x * x for x in range(1, 11) if x % 2 == 0] # 带有if判定
[m + n for m in 'abc' for n in 'ABC']   # 双层for循环
[d for d in os.listdir('.')] # import os，os.listdir可以列出文件和目录
[k + '=' + v for k, v in dictobj.items()]
[s.lower() for s in listobj]
[x if x % 2 == 0 else -x for x in range(1, 11)] # 需要else的时候，if-else前置，反装饰器效果？

# 生成器
#   列表补全表达式占用空间大；生成器在运行时循环中生成
#   生成器就是把列表补全表达式的[]替换成()
g = (x * x for x in range(1, 11))
next(g) # 获得下一个返回值，边界后跑出StopIteration错误，几乎从不用next，而是用for
for item in g   # 生成器可迭代
#   如果一个函数定义中包含关键字yield，就不是不同函数，而是一个生成器
#   如斐波那契数列函数（无法用列表补全表达式）
def fib(_max):
    n, a, b = 0, 0, 1
    while n < _max:
        yield b         # next(gerator_obj)每运行到此，返回一次
        a, b = b, a + b
        n = n + 1
    return 'done'

# 迭代器
#   可迭代对象：集合数据类型（list/tuple/dict/set/str）和生成器
#   集合数据类型不是迭代器，可以被next()函数调用的且返回下一个值的对象称为迭代器iterator
#   迭代器判定
from collections import Iterator
isinstance(obj, Iterator)   #  返回True则是迭代器
#   可迭代对象转迭代器
iter(iterable_obj)
#   迭代器是一个数据流，可以看作是有序序列（不知道其长度），却不是序列对象
#   迭代器可以表达一个无限的数据流，如自然数，但序列对象list是不能存储所有的自然数
```

## 二、函数式编程
funtion programming -> funtional programming，函数式编程是一种抽象程度高的编程范式，纯粹的函数式编程的函数没有变量。
纯函数，输入确定，输出就是确定的。使用变量的函数则不是。
函数式编程甚至可以把函数作为参数或返回值。

python认为越高级语言，越抽象，越贴近计算而不是计算机，执行效率固然低？
### 1.高阶函数
变量可以指向函数，函数的参数接收变量 -> 一个函数接收另一个函数做参数，即高阶函数(high-order-function)。
```py
# e.g.1. map/reduce
#   map(function, iterable, ...)
#   reduce(function, iterable[, initializer])
map(f, [x1, x2, ..., xn]) = [f(x1), f(x2), ..., f(xn)]
#   map把f作用在iterable对象上，返回一个iterator
reduce(f, [x1, x2, x3, ..., xn]) = f(f(...f(f(x1, x2), x3)...), xn)
#   reduce把f递归作用在iterable对象上，返回值
reduce(lambda x, y: x * 10 + y, map(char2num, '12345'))
#   '12345'转整数

# e.g.2. filter
#   filter(function, iterable)
filter(f, [x1, x2, ..., xn]) = [x1, x2, ..., xm]
#   过滤序列
#   质数生成器 -- begin
def _odd_iter():
    n = 1
    while True:
        n = n + 2
        yield n

def _not_divisible(n):
    return labda x: x % n > 0

def primes():
    yield 2
    it = _odd_iter()    # 初始序列
    while True:
        n = next(it)    # 返回下一个数
        yield n
        it = filter(_not_divisible(n), it) # 构造新序列，嵌套的filter

for n in primes():
    if n < 1000:
        print(n)
    else:
        break
#   质数生成器 -- end

# e.g.3. sorted
#   sorted(iterable, cmp=None, key=None, reverse=False)
sorted([x1, x2, ..., xn], key=f) = iterable-obj-sorted-by-f
```

### 2.返回函数
```py
def lazy_fn(*args): # 定义返回函数
    def fn():
        retval = f(args)
        return retval
    return fn
fun = lazy_fn(1, 2, 3)  # 每次调用返回一个新函数，即使入参相同
fun()   # 执行

def lazy_fn():
    fns = []
    for i in range(1, 4):
        def f():
            return i*i
        fns.append(f)
    return fns
f1, f2, f3 = lazy_fn()
f1()    # 9
f2()    # 9
f3()    # 9
#   局部变量i被修改成3
#   返回函数不要引用任何循环变量，会发生不符预期的情况
```

### 3.匿名函数
```py
lambda x: x*x
# =
def fn(x)
    return x*x
```

### 4.装饰器
```py
def fn():
    pass
fn.__name__ # 'fn'

# 装饰器，返回一个高阶函数（执行了更多）
def log(text):  # 入参装饰
    def decorator(func):   # 装饰器
        @functools.wraps(func)  #   把原始func属性保存，执行后，修复回来（import functools）
        def wrapper(*argc, **kw):
            print('%s'%func.__name__)
            return func(*argc, **kw)
        return wrapper
    return decorator
@log('test')        # 装饰器调用，相当于fn = log('test')(fn)
def fn():
    pass

```

### 5.偏函数
```py
import functools
int2 = functools.partial(int, base=2)   # 返回一个新函数，封装了int函数，并参数base默认为2
max2 = functools.partial(max, 100)
#   偏函数实际是将默认参数打包成**kw/*argc的形式 作为 原函数的参数
```

## 三、模块与包
python里一个py文件就是一个模块。模块提高了代码的可维护性，可扩展性。

编写模块不用考虑与其他模块命名冲突，但尽量避免与内置函数冲突。
python按目录来组织模块，称为包管理。每个包都会有一个__init__.py文件（必须存在，否则就是普通目录），可以是空文件，以目录名为模块名。

模块和包命名要遵守python变量命名规范，并避免和系统模块冲突。
python交互环境中，执行import命令可以查看包是否存在。

模块的搜索路径优先顺序：当前目录 > 内置模块目录 > 第三方模块。
可以通过修改sys.path或设置环境变量PYTHONPATH修改。

模块里（或者py文件里）一般以python脚本声明、utf-8编码声明为第一二行，后续第一个字符串被视为该模块的文档注释。__author__变量用于标记写者。

python的作用域，python约定以'_'开头的为内部变量/函数，其他为公开的。__xxx__形式的为特殊变量，如__doc__是当前模块注释。
### 1.系统模块
```py
import sys
sys.argv    # 第一个元素是模块名，后续为调用参数
sys.path    # 打印模块搜索路径，可以append修改

if __name__ == '__main__'   # 判定当前py文件为运行模块，常用于执行测试
```

### 2.第三方模块
python使用pip做包管理。
python有丰富的第三方库，如图形库Pillow、mysql驱动库、web框架flask、科学计算numpy等。
Anaconda是一个python数据处理和科学计算平台，内置了很多第三方库，且自带python环境。

## 四、面向对象
类是从实例抽象出来的模版
```py
# 基础
class Student(object):  # Student类，继承自object
    attr = 'study'      # 类属性，所有实例公用；
    #   stu.attr会优先使用实例属性，再使用类属性；也可以Student.attr访问；应尽量避免类属性与实例属性重名；
    #   通过Student.attr修改类属性？
    def __init__(self, name, score):    # 特殊方法__init__，定义了入参，则创建实例必须满足入参要求
        self.__name = name  # 实例属性，以__为前缀，成为一个私有变量，外部不能直接访问（可通过_Student__name访问）
        self._score = score # 实例属性，以_为前缀，约定为私有变量，但是外部可以访问
        self.attr = 'test'
    def printout(self): # 类的方法
        print('%s: %s' % (self.name, self.score))

sam = Student('sammy', 60)  # 实例，每个实例内存地址不同
sam.test = 'test'   # python允许实例变量绑定任何数据，只和实例有关，和其类无关
sam.__name = '134'  # 是额外绑定的数据，而不是类成员

# 继承 -> 复用、多态
#   达到对扩展开放，对修改封闭
class SeniorStu(Student):
    pass

lizi = Senior('lizi', 54)   # 必须也满足基类实例化入参要求
isinstance(lizi, Student)   # True

def printout(obj):  # 定义了printout的函数的类的实例可以作为入参
    obj.printout()
#   python的多态不要求继承，任何实现了函数要求的实例都可以
#   python：看起来像鸭子，走起路像鸭子，就是鸭子

# type函数，判定对象的类型（及types包，types函数需要import）
type(123)           # class 'int'
type('123')         # class 'str'
type(None)          # type(None) 'NoneType'
type(abs)           # class 'builtin_function_or_method'
type(a)             # class '__main__.Animail'
type(123) == type(456)  # 以下皆True
type(123) == int
type(fn) == types.FunctionType
type(abs) == types.BuiltinFunctionType
type(lambda x: x) == types.LambdaType
type((x for x in range(10))) == types.GeneratorType

# isinstance函数，Base -> Derived，以下默认返回True
isinstance(derived, Base)
isinstance([1, 2, 3], (list, tuple))    # 判定是list或tuple

# dir函数，获得一个对象的所有属性和方法
dir('abc')      # ['__add__', '__class__', ..., '__subclasshook__', 'capitalize', 'casefold', ..., 'zfill']
#   len('abc')内部实际调用对象的__len__()方法，任何一个实现了__len__()的类的实例都可以用len(obj)调用
#   str类型的普通函数这样调用：'AbC'.lower()

# getattr()/setattr()/hasattr()，适用于变量和方法
hasattr(obj, 'mbr')     # 判定obj是否有属性mbr
setattr(obj, 'mbs', 9)  # 设置obj.mbs = 9
getattr(obj, 'mbs')     # 返回obj.mbs的值，不存在则抛出异常AttributeError
getattr(obj, 'mbt', 404)    # 返回obj.mbt，不存在则返回404

# python设计了一系列内置函数，用于剖析对象。如果知道了对象信息，就不要去剖析了。
#   这些函数常用于接口处理入参判定？

# 属性动态绑定与__slots__（属性白名单？）
from types import MethodTypes
obj.mtd = MethodType(fn, obj)       # 实例动态绑定函数属性，仅当前实例可用，obj.mtd()
Obj.mtd = fn                        # 类动态绑定函数属性，所有实例都可使用
class Cls(object):
    __slot__ = ('mbr', 'mtd')       # 限制类及实例的动态绑定；白名单外的属性动态绑定抛出异常；对派生的子类不起作用
    pass
```
### 1.多重继承
### 2.定制类
### 3.元类