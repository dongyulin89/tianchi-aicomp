# 函数与Lambda 表达式
## 函数的定义
- 函数以`def`关键词开头，后接函数名和圆括号()。
- 函数执行的代码以冒号起始，并且缩进。
- return [表达式] 结束函数，选择性地返回一个值给调用方。不带表达式的return相当于返回`None`。
```python
def functionname (parameters):
       "函数_文档字符串"
        function_suite
        return [expression]
```

## 函数的调用
```python
def printme(str):
    print(str)

printme("我要调用用户自定义函数!")  # 我要调用用户自定义函数!
printme("再次调用同一函数")  # 再次调用同一函数
temp = printme('hello') # hello
print(temp)  # None
```

## 函数文档
```python
def MyFirstFunction(name):
    "函数定义过程中name是形参"
    # 因为Ta只是一个形式，表示占据一个参数位置
    print('传递进来的{0}叫做实参，因为Ta是具体的参数值！'.format(name))


MyFirstFunction('老马的程序人生')  
# 传递进来的老马的程序人生叫做实参，因为Ta是具体的参数值！

print(MyFirstFunction.__doc__)  
# 函数定义过程中name是形参

help(MyFirstFunction)
# Help on function MyFirstFunction in module __main__:
# MyFirstFunction(name)
#    函数定义过程中name是形参
```

## 函数参数
- 位置参数 (positional argument)
- 默认参数 (default argument)
- 可变参数 (variable argument)
- 关键字参数 (keyword argument)
- 命名关键字参数 (name keyword argument)
- 参数组合

### 位置参数
```python
def functionname(arg1):
       "函数_文档字符串"
       function_suite
       return [expression]
```
`arg1` - 位置参数 ，这些参数在调用函数 (call function) 时位置要固定。

### 默认参数
```python
def functionname(arg1, arg2=v):
       "函数_文档字符串"
       function_suite
       return [expression]
```
`arg2 = v` - 默认参数 = 默认值，调用函数时，默认参数的值如果没有传入，则被认为是默认值。
默认参数一定要放在位置参数 <b>后面</b>，不然程序会报错。

```python
def printinfo(name, age):
    print('Name:{0},Age:{1}'.format(name, age))


printinfo(age=8, name='小马')  # Name:小马,Age:8
```
Python 允许函数调用时参数的顺序与声明时不一致，因为 Python 解释器能够用参数名匹配参数值

### 可变参数
```python
def functionname(arg1, arg2=v, *args):
       "函数_文档字符串"
       function_suite
       return [expression]
```
`*args` - 可变参数，可以是从零个到任意个，自动组装成元组。
加了星号（*）的变量名会存放所有未命名的变量参数。

```python
def printinfo(arg1, *args):
    print(arg1)
    for var in args:
        print(var)


printinfo(10)  # 10
printinfo(70, 60, 50)
# 70
# 60
# 50
```

### 关键字参数
```python
def functionname(arg1, arg2=v, args, *kw):
       "函数_文档字符串"
       function_suite
       return [expression]
 ```
 `**kw` - 关键字参数，可以是从零个到任意个，自动组装成字典
 
 ```python
 def printinfo(arg1, *args, **kwargs):
    print(arg1)
    print(args)
    print(kwargs)


printinfo(70, 60, 50)
# 70
# (60, 50)
# {}
printinfo(70, 60, 50, a=1, b=2)
# 70
# (60, 50)
# {'a': 1, 'b': 2}
```

### 命名关键字参数
```python
def functionname(arg1, arg2=v, args, *, nkw, *kw):
       "函数_文档字符串"
       function_suite
       return [expression]
```

`*, nkw` - 命名关键字参数，用户想要输入的关键字参数，定义方式是在nkw 前面加个分隔符 `*`
如果要限制关键字参数的名字，就可以用「命名关键字参数」
使用命名关键字参数时，要特别注意不能缺少参数名
 
```python
def printinfo(arg1, *, nkw, **kwargs):
    print(arg1)
    print(nkw)
    print(kwargs)


printinfo(70, nkw=10, a=1, b=2)
# 70
# 10
# {'a': 1, 'b': 2}

printinfo(70, 10, a=1, b=2)
# TypeError: printinfo() takes 1 positional argument but 2 were given
```
没有写参数名nwk，因此 10 被当成「位置参数」，而原函数只有 1 个位置函数，现在调用了 2 个，因此程序会报错。

### 参数组合
在 Python 中定义函数，可以用位置参数、默认参数、可变参数、命名关键字参数和关键字参数，这 5 种参数中的 4 个都可以一起使用，但是注意，参数定义的顺序必须是：
- 位置参数、默认参数、可变参数和关键字参数。
- 位置参数、默认参数、命名关键字参数和关键字参数。

要注意定义可变参数和关键字参数的语法：
- `*args` 是可变参数，`args` 接收的是一个 `tuple`
- `**kw` 是关键字参数，`kw` 接收的是一个 `dict`

命名关键字参数是为了限制调用者可以传入的参数名，同时可以提供默认值。定义命名关键字参数不要忘了写分隔符 `*`，否则定义的是位置参数。

警告：虽然可以组合多达 5 种参数，但不要同时使用太多的组合，否则函数很难懂。


## 函数的返回值
```python
def add(a, b):
    return a + b
print(add(1, 2))  # 3
print(add([1, 2, 3], [4, 5, 6]))  # [1, 2, 3, 4, 5, 6]

def back():
    return [1, '小马的程序人生', 3.14]
print(back())  # [1, '小马的程序人生', 3.14]

def back():
    return 1, '小马的程序人生', 3.14
print(back())  # (1, '小马的程序人生', 3.14)

def printme(str):
    print(str)
temp = printme('hello') # hello
print(temp) # None
print(type(temp))  # <class 'NoneType'>
```

## 变量作用域
- Python 中，程序的变量并不是在哪个位置都可以访问的，访问权限决定于这个变量是在哪里赋值的。
- 定义在函数内部的变量拥有局部作用域，该变量称为局部变量。
- 定义在函数外部的变量拥有全局作用域，该变量称为全局变量。
- 局部变量只能在其被声明的函数内部访问，而全局变量可以在整个程序范围内访问。

```python
def discounts(price, rate):
    final_price = price * rate
    return final_price

old_price = float(input('请输入原价:'))  # 98
rate = float(input('请输入折扣率:'))  # 0.9
new_price = discounts(old_price, rate)
print('打折后价格是:%.2f' % new_price)  # 88.20
```
- 当内部作用域想修改外部作用域的变量时，就要用到`global`和`nonlocal`关键字了。

```python
num = 1

def fun1():
    global num  # 需要使用 global 关键字声明
    print(num)  # 1
    num = 123
    print(num)  # 123

fun1()
print(num)  # 123
```

### 内嵌函数
```python
def outer():
    print('outer函数在这被调用')

    def inner():
        print('inner函数在这被调用')

    inner()  # 该函数只能在outer函数内部被调用

outer()
# outer函数在这被调用
# inner函数在这被调用
```

### 闭包
- 闭包是函数式编程的一个重要的语法结构，是一种特殊的内嵌函数。
- 如果在一个内部函数里对外层非全局作用域的变量进行引用，那么内部函数就被认为是闭包。
- 通过闭包可以访问外层非全局作用域的变量，这个作用域称为 <b>闭包作用域</b>。

```python
def funX(x):
    def funY(y):
        return x * y

    return funY

i = funX(8)
print(type(i))  # <class 'function'>
print(i(5))  # 40
```

闭包的返回值通常是函数
```python
def make_counter(init):
    counter = [init]

    def inc(): counter[0] += 1

    def dec(): counter[0] -= 1

    def get(): return counter[0]

    def reset(): counter[0] = init

    return inc, dec, get, reset


inc, dec, get, reset = make_counter(0)
inc()
inc()
inc()
print(get())  # 3
dec()
print(get())  # 2
reset()
print(get())  # 0
```

如果要修改闭包作用域中的变量则需要 nonlocal 关键字
```python
def outer():
    num = 10

    def inner():
        nonlocal num  # nonlocal关键字声明
        num = 100
        print(num)

    inner()
    print(num)


outer()

# 100
# 100
```

### 递归
如果一个函数在内部调用自身本身，这个函数就是递归函数
```python
# 利用循环
n = 5
for k in range(1, 5):
    n = n * k
print(n)  # 120

# 利用递归
def factorial(n):
    if n == 1:
        return 1
    return n * factorial(n - 1)


print(factorial(5)) # 120
```

斐波那契数列 `f(n)=f(n-1)+f(n-2), f(0)=0 f(1)=1`
```python
# 利用循环
i = 0
j = 1
lst = list([i, j])
for k in range(2, 11):
    k = i + j
    lst.append(k)
    i = j
    j = k
print(lst)  
# [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55]

# 利用递归
def recur_fibo(n):
    if n <= 1:
        return n
    return recur_fibo(n - 1) + recur_fibo(n - 2)


lst = list()
for k in range(11):
    lst.append(recur_fibo(k))
print(lst)  
# [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```

设置递归的层数，Python默认递归层数为 100
```python
import sys

sys.setrecursionlimit(1000)
```

## Lambda 的定义
在 Python 里有两类函数：
- 第一类：用 `def` 关键词定义的正规函数
- 第二类：用 `lambda` 关键词定义的匿名函数

Python 使用 `lambda` 关键词来创建匿名函数，而非`def`关键词，它没有函数名，其语法结构如下：
```python
lambda argument_list: expression
```
- `lambda` - 定义匿名函数的关键词。
- `argument_list` - 函数参数，它们可以是位置参数、默认参数、关键字参数，和正规函数里的参数类型一样。
- `:`- 冒号，在函数参数和表达式中间要加个冒号。
- `expression` - 只是一个表达式，输入函数参数，输出一些值。

注意：
- `expression` 中没有 return 语句，因为 lambda 不需要它来返回，表达式本身结果就是返回值。
- 匿名函数拥有自己的命名空间，且不能访问自己参数列表之外或全局命名空间里的参数。

```python
def sqr(x):
    return x ** 2

print(sqr)
# <function sqr at 0x000000BABD3A4400>

y = [sqr(x) for x in range(10)]
print(y)
# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

lbd_sqr = lambda x: x ** 2
print(lbd_sqr)
# <function <lambda> at 0x000000BABB6AC1E0>

y = [lbd_sqr(x) for x in range(10)]
print(y)
# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]


sumary = lambda arg1, arg2: arg1 + arg2
print(sumary(10, 20))  # 30

func = lambda *args: sum(args)
print(func(1, 2, 3, 4, 5))  # 15
```

## Lambda 的应用
`函数式编程`是指代码中每一块都是不可变的，都由纯函数的形式组成。这里的纯函数，是指函数本身相互独立、互不影响，对于相同的输入，总会有相同的输出，没有任何副作用

非函数式编程
```python
def f(x):
    for i in range(0, len(x)):
        x[i] += 10
    return x

x = [1, 2, 3]
f(x)
print(x)
# [11, 12, 13]
```

函数式编程
```python
def f(x):
    y = []
    for item in x:
        y.append(item + 10)
    return y

x = [1, 2, 3]
f(x)
print(x)
# [1, 2, 3]
```

匿名函数 常常应用于`函数式编程`的`高阶函数 (high-order function)`中，主要有两种形式：
- 参数是函数 (filter, map)
- 返回值是函数 (closure)

如，在 `filter`和`map`函数中的应用：
- `filter(function, iterable)` 过滤序列，过滤掉不符合条件的元素，返回一个迭代器对象，如果要转换为列表，可以使用 `list()` 来转换。
```python
odd = lambda x: x % 2 == 1
templist = filter(odd, [1, 2, 3, 4, 5, 6, 7, 8, 9])
print(list(templist))  # [1, 3, 5, 7, 9]
```

- `map(function, *iterables)` 根据提供的函数对指定序列做映射。
```pyghon
m1 = map(lambda x: x ** 2, [1, 2, 3, 4, 5])
print(list(m1))  
# [1, 4, 9, 16, 25]

m2 = map(lambda x, y: x + y, [1, 3, 5, 7, 9], [2, 4, 6, 8, 10])
print(list(m2))  
# [3, 7, 11, 15, 19]
```

除了 Python 这些内置函数，我们也可以自己定义高阶函数
```python
def apply_to_list(fun, some_list):
    return fun(some_list)

lst = [1, 2, 3, 4, 5]
print(apply_to_list(sum, lst))
# 15

print(apply_to_list(len, lst))
# 5

print(apply_to_list(lambda x: sum(x) / len(x), lst))
# 3.0
```

# 类与对象
`对象 = 属性 + 方法`
- 对象是类的实例。换句话说，类主要定义对象的结构，然后我们以类为模板创建对象。类不但包含方法定义，而且还包含所有实例共享的数据。
- 封装：信息隐蔽技术

我们可以使用关键字 `class` 定义 Python 类，关键字后面紧跟类的名称、分号和类的实现
```python
class Turtle:  # Python中的类名约定以大写字母开头
    """关于类的一个简单例子"""
    # 属性
    color = 'green'
    weight = 10
    legs = 4
    shell = True
    mouth = '大嘴'

    # 方法
    def climb(self):
        print('我正在很努力的向前爬...')

    def run(self):
        print('我正在飞快的向前跑...')

    def bite(self):
        print('咬死你咬死你!!')

    def eat(self):
        print('有得吃，真满足...')

    def sleep(self):
        print('困了，睡了，晚安，zzz')


tt = Turtle()
print(tt)
# <__main__.Turtle object at 0x0000007C32D67F98>

print(type(tt))
# <class '__main__.Turtle'>

print(tt.__class__)
# <class '__main__.Turtle'>

print(tt.__class__.__name__)
# Turtle

tt.climb()
# 我正在很努力的向前爬...

tt.run()
# 我正在飞快的向前跑...

tt.bite()
# 咬死你咬死你!!

# Python类也是对象。它们是type的实例
print(type(Turtle))
# <class 'type'>
```

继承：子类自动共享父类之间数据和方法的机制
```python
class MyList(list):
    pass

lst = MyList([1, 5, 2, 7, 8])
lst.append(9)
lst.sort()
print(lst)

# [1, 2, 5, 7, 8, 9]
```

多态：不同对象对同一方法响应不同的行动
```python
class Animal:
    def run(self):
        raise AttributeError('子类必须实现这个方法')

class People(Animal):
    def run(self):
        print('人正在走')

class Pig(Animal):
    def run(self):
        print('pig is walking')

class Dog(Animal):
    def run(self):
        print('dog is running')

def func(animal):
    animal.run()

func(Pig())
# pig is walking
```

## self
Python 的 `self` 相当于 C++ 的 `this` 指针
```python
class Test:
    def prt(self):
        print(self)
        print(self.__class__)

t = Test()
t.prt()
# <__main__.Test object at 0x000000BC5A351208>
# <class '__main__.Test'>
```

类的方法与普通的函数只有一个特别的区别 —— 它们必须有一个额外的第一个参数名称（对应于该实例，即该对象本身），按照惯例它的名称是 `self`。在调用方法时，我们无需明确提供与参数 `self` 相对应的参数。
```python
class Ball:
    def setName(self, name):
        self.name = name

    def kick(self):
        print("我叫%s,该死的，谁踢我..." % self.name)

a = Ball()
a.setName("球A")
b = Ball()
b.setName("球B")
c = Ball()
c.setName("球C")
a.kick()
# 我叫球A,该死的，谁踢我...
b.kick()
# 我叫球B,该死的，谁踢我...
```

## Python的魔法方法
据说，Python 的对象天生拥有一些神奇的方法，它们是面向对象的 Python 的一切...
它们是可以给你的类增加魔力的特殊方法...
如果你的对象实现了这些方法中的某一个，那么这个方法就会在特殊的情况下被 Python 所调用，而这一切都是自动发生的...
类有一个名为`__init__(self[, param1, param2...])`的魔法方法，该方法在类实例化时会自动调用。

```python
class Ball:
    def __init__(self, name):
        self.name = name

    def kick(self):
        print("我叫%s,该死的，谁踢我..." % self.name)


a = Ball("球A")
b = Ball("球B")
c = Ball("球C")
a.kick()
# 我叫球A,该死的，谁踢我...
b.kick()
# 我叫球B,该死的，谁踢我...
```

## 私有变量
在 Python 中定义私有变量只需要在变量名或函数名前加上“__”两个下划线，那么这个函数或变量就会为私有的了

- 类的私有属性实例
```python
class JustCounter:
    __secretCount = 0  # 私有变量
    publicCount = 0  # 公开变量

    def count(self):
        self.__secretCount += 1
        self.publicCount += 1
        print(self.__secretCount)


counter = JustCounter()
counter.count()  # 1
counter.count()  # 2
print(counter.publicCount)  # 2

# Python的私有为伪私有
print(counter._JustCounter__secretCount)  # 2 
print(counter.__secretCount)  
# AttributeError: 'JustCounter' object has no attribute '__secretCount'
```

- 类的私有方法实例
```python
class Site:
    def __init__(self, name, url):
        self.name = name  # public
        self.__url = url  # private

    def who(self):
        print('name  : ', self.name)
        print('url : ', self.__url)

    def __foo(self):  # 私有方法
        print('这是私有方法')

    def foo(self):  # 公共方法
        print('这是公共方法')
        self.__foo()


x = Site('老马的程序人生', 'https://blog.csdn.net/LSGO_MYP')
x.who()
# name  :  老马的程序人生
# url :  https://blog.csdn.net/LSGO_MYP

x.foo()
# 这是公共方法
# 这是私有方法

x.__foo()
# AttributeError: 'Site' object has no attribute '__foo'
```

## 继承
Python 同样支持类的继承，派生类的定义如下所示
```python
class DerivedClassName(BaseClassName):
       statement-1
              .
              .
              .
       statement-N
```

`BaseClassName`（基类名）必须与派生类定义在一个作用域内。除了类，还可以用表达式，基类定义在另一个模块中时这一点非常有用
```python
class DerivedClassName(modname.BaseClassName):
       statement-1
              .
              .
              .
       statement-N
```

如果子类中定义与父类同名的方法或属性，则会自动覆盖父类对应的方法或属性
```python
# 类定义
class people:
    # 定义基本属性
    name = ''
    age = 0
    # 定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0

    # 定义构造方法
    def __init__(self, n, a, w):
        self.name = n
        self.age = a
        self.__weight = w

    def speak(self):
        print("%s 说: 我 %d 岁。" % (self.name, self.age))


# 单继承示例
class student(people):
    grade = ''

    def __init__(self, n, a, w, g):
        # 调用父类的构函
        people.__init__(self, n, a, w)
        self.grade = g

    # 覆写父类的方法
    def speak(self):
        print("%s 说: 我 %d 岁了，我在读 %d 年级" % (self.name, self.age, self.grade))


s = student('小马的程序人生', 10, 60, 3)
s.speak()
# 小马的程序人生 说: 我 10 岁了，我在读 3 年级
```

注意：如果上面的程序去掉：`people.__init__(self, n, a, w)`，则输出：` 说: 我 0 岁了，我在读 3 年级`，因为子类的构造方法把父类的构造方法覆盖了。
```python
import random

class Fish:
    def __init__(self):
        self.x = random.randint(0, 10)
        self.y = random.randint(0, 10)

    def move(self):
        self.x -= 1
        print("我的位置", self.x, self.y)


class GoldFish(Fish):  # 金鱼
    pass


class Carp(Fish):  # 鲤鱼
    pass


class Salmon(Fish):  # 三文鱼
    pass


class Shark(Fish):  # 鲨鱼
    def __init__(self):
        self.hungry = True

    def eat(self):
        if self.hungry:
            print("吃货的梦想就是天天有得吃！")
            self.hungry = False
        else:
            print("太撑了，吃不下了！")
            self.hungry = True


g = GoldFish()
g.move()  # 我的位置 9 4
s = Shark()
s.eat() # 吃货的梦想就是天天有得吃！
s.move()  
# AttributeError: 'Shark' object has no attribute 'x'
```

解决该问题可用以下两种方式：
- 调用未绑定的父类方法`Fish.__init__(self)`
```python
class Shark(Fish):  # 鲨鱼
    def __init__(self):
        Fish.__init__(self)
        self.hungry = True

    def eat(self):
        if self.hungry:
            print("吃货的梦想就是天天有得吃！")
            self.hungry = False
        else:
            print("太撑了，吃不下了！")
            self.hungry = True
```

- 使用super函数`super().__init__()`
```python
class Shark(Fish):  # 鲨鱼
    def __init__(self):
        super().__init__()
        self.hungry = True

    def eat(self):
        if self.hungry:
            print("吃货的梦想就是天天有得吃！")
            self.hungry = False
        else:
            print("太撑了，吃不下了！")
            self.hungry = True
```

Python 虽然支持多继承的形式，但我们一般不使用多继承，因为容易引起混乱
```python
class DerivedClassName(Base1, Base2, Base3):
       statement-1
              .
              .
              .
       statement-N
```
需要注意圆括号中父类的顺序，若是父类中有相同的方法名，而在子类使用时未指定，Python 从左至右搜索，即方法在子类中未找到时，从左到右查找父类中是否包含方法
```python
# 类定义
class People:
    # 定义基本属性
    name = ''
    age = 0
    # 定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0

    # 定义构造方法
    def __init__(self, n, a, w):
        self.name = n
        self.age = a
        self.__weight = w

    def speak(self):
        print("%s 说: 我 %d 岁。" % (self.name, self.age))


# 单继承示例
class Student(People):
    grade = ''

    def __init__(self, n, a, w, g):
        # 调用父类的构函
        People.__init__(self, n, a, w)
        self.grade = g

    # 覆写父类的方法
    def speak(self):
        print("%s 说: 我 %d 岁了，我在读 %d 年级" % (self.name, self.age, self.grade))


# 另一个类，多重继承之前的准备
class Speaker:
    topic = ''
    name = ''

    def __init__(self, n, t):
        self.name = n
        self.topic = t

    def speak(self):
        print("我叫 %s，我是一个演说家，我演讲的主题是 %s" % (self.name, self.topic))


# 多重继承
class Sample01(Speaker, Student):
    a = ''

    def __init__(self, n, a, w, g, t):
        Student.__init__(self, n, a, w, g)
        Speaker.__init__(self, n, t)

# 方法名同，默认调用的是在括号中排前地父类的方法
test = Sample01("Tim", 25, 80, 4, "Python")
test.speak()  
# 我叫 Tim，我是一个演说家，我演讲的主题是 Python

class Sample02(Student, Speaker):
    a = ''

    def __init__(self, n, a, w, g, t):
        Student.__init__(self, n, a, w, g)
        Speaker.__init__(self, n, t)

# 方法名同，默认调用的是在括号中排前地父类的方法
test = Sample02("Tim", 25, 80, 4, "Python")
test.speak()  
# Tim 说: 我 25 岁了，我在读 4 年级
```

## 组合
```python
class Turtle:
    def __init__(self, x):
        self.num = x

class Fish:
    def __init__(self, x):
        self.num = x

class Pool:
    def __init__(self, x, y):
        self.turtle = Turtle(x)
        self.fish = Fish(y)

    def print_num(self):
        print("水池里面有乌龟%s只，小鱼%s条" % (self.turtle.num, self.fish.num))

p = Pool(2, 3)
p.print_num()
# 水池里面有乌龟2只，小鱼3条
```

## 类/类对象/实例对象
![类对象和实例对象](https://img-blog.csdnimg.cn/20191007090316462.png)

类对象：创建一个类，其实也是一个对象也在内存开辟了一块空间，称为类对象，类对象只有一个。
```python
class A(object):
       pass
```
实例对象：就是通过实例化类创建的对象，称为实例对象，实例对象可以有多个。
```python
class A(object):
    pass

# 实例化对象 a、b、c都属于实例对象。
a = A()
b = A()
c = A()
```

类属性：类里面方法外面定义的变量称为类属性。类属性所属于类对象并且多个实例对象之间共享同一个类属性，说白了就是类属性所有的通过该类实例化的对象都能共享。
```python
class A():
    a = 0  #类属性
    def __init__(self, xx):
        A.a = xx  #使用类属性可以通过 （类名.类属性）调用。
```

实例属性：实例属性和具体的某个实例对象有关系，并且一个实例对象和另外一个实例对象是不共享属性的，说白了实例属性只能在自己的对象里面使用，其他的对象不能直接使用，因为`self`是谁调用，它的值就属于该对象。
```python
# 创建类对象
class Test(object):
    class_attr = 100  # 类属性

    def __init__(self):
        self.sl_attr = 100  # 实例属性

    def func(self):
        print('类对象.类属性的值:', Test.class_attr)  # 调用类属性
        print('self.类属性的值', self.class_attr)  # 相当于把类属性 变成实例属性
        print('self.实例属性的值', self.sl_attr)  # 调用实例属性


a = Test()
a.func()

# 类对象.类属性的值: 100
# self.类属性的值 100
# self.实例属性的值 100

b = Test()
b.func()

# 类对象.类属性的值: 100
# self.类属性的值 100
# self.实例属性的值 100

a.class_attr = 200
a.sl_attr = 200
a.func()

# 类对象.类属性的值: 100
# self.类属性的值 200
# self.实例属性的值 200

b.func()

# 类对象.类属性的值: 100
# self.类属性的值 100
# self.实例属性的值 100

Test.class_attr = 300
a.func()

# 类对象.类属性的值: 300
# self.类属性的值 200
# self.实例属性的值 200

b.func()
# 类对象.类属性的值: 300
# self.类属性的值 300
# self.实例属性的值 100
```

注意：属性与方法名相同，属性会覆盖方法。
```python
class A:
    def x(self):
        print('x_man')

aa = A()
aa.x()  # x_man
aa.x = 1
print(aa.x)  # 1
aa.x()
# TypeError: 'int' object is not callable
```

## 绑定
Python 严格要求`方法需要有实例才能被调用`，这种限制其实就是 Python 所谓的`绑定`概念。

Python 对象的数据属性通常存储在名为`.__ dict__`的字典中，我们可以直接访问`__dict__`，或利用 Python 的内置函数`vars()`获取`.__ dict__`。
```python
class CC:
    def setXY(self, x, y):
        self.x = x
        self.y = y

    def printXY(self):
        print(self.x, self.y)

dd = CC()
print(dd.__dict__)
# {}

print(vars(dd))
# {}

print(CC.__dict__)
# {'__module__': '__main__', 'setXY': <function CC.setXY at 0x000000C3473DA048>, 'printXY': <function CC.printXY at 0x000000C3473C4F28>, '__dict__': <attribute '__dict__' of 'CC' objects>, '__weakref__': <attribute '__weakref__' of 'CC' objects>, '__doc__': None}

dd.setXY(4, 5)
print(dd.__dict__)
# {'x': 4, 'y': 5}

print(vars(CC))
# {'__module__': '__main__', 'setXY': <function CC.setXY at 0x000000632CA9B048>, 'printXY': <function CC.printXY at 0x000000632CA83048>, '__dict__': <attribute '__dict__' of 'CC' objects>, '__weakref__': <attribute '__weakref__' of 'CC' objects>, '__doc__': None}

print(CC.__dict__)
# {'__module__': '__main__', 'setXY': <function CC.setXY at 0x000000632CA9B048>, 'printXY': <function CC.printXY at 0x000000632CA83048>, '__dict__': <attribute '__dict__' of 'CC' objects>, '__weakref__': <attribute '__weakref__' of 'CC' objects>, '__doc__': None}
```

## 内置函数（BIF）
- `issubclass(class, classinfo)` 方法用于判断参数 class 是否是类型参数 classinfo 的子类。
- 一个类被认为是其自身的子类。
- `classinfo`可以是类对象的元组，只要class是其中任何一个候选类的子类，则返回`True`。
```python
class A:
    pass

class B(A):
    pass

print(issubclass(B, A))  # True
print(issubclass(B, B))  # True
print(issubclass(A, B))  # False
print(issubclass(B, object))  # True
```

- `isinstance(object, classinfo)` 方法用于判断一个对象是否是一个已知的类型，类似`type()`。
- `type()`不会认为子类是一种父类类型，不考虑继承关系。
- `isinstance()`会认为子类是一种父类类型，考虑继承关系。
- 如果第一个参数不是对象，则永远返回`False`。
- 如果第二个参数不是类或者由类对象组成的元组，会抛出一个`TypeError`异常。
```python
a = 2
print(isinstance(a, int))  # True
print(isinstance(a, str))  # False
print(isinstance(a, (str, int, list)))  # True

class A:
    pass

class B(A):
    pass

print(isinstance(A(), A))  # True
print(type(A()) == A)  # True
print(isinstance(B(), A))  # True
print(type(B()) == A)  # False
```

- `hasattr(object, name)`用于判断对象是否包含对应的属性。
```python
class Coordinate:
    x = 10
    y = -5
    z = 0

point1 = Coordinate()
print(hasattr(point1, 'x'))  # True
print(hasattr(point1, 'y'))  # True
print(hasattr(point1, 'z'))  # True
print(hasattr(point1, 'no'))  # False
```

- `getattr(object, name[, default])`用于返回一个对象属性值。
```python
class A(object):
    bar = 1

a = A()
print(getattr(a, 'bar'))  # 1
print(getattr(a, 'bar2', 3))  # 3
print(getattr(a, 'bar2'))
# AttributeError: 'A' object has no attribute 'bar2'
```
这个例子很酷！
```python
class A(object):
    def set(self, a, b):
        x = a
        a = b
        b = x
        print(a, b)

a = A()
c = getattr(a, 'set')
c(a='1', b='2')  # 2 1

```

- `setattr(object, name, value)`对应函数 `getattr()`，用于设置属性值，该属性不一定是存在的。
```python
class A(object):
    bar = 1

a = A()
print(getattr(a, 'bar'))  # 1
setattr(a, 'bar', 5)
print(a.bar)  # 5
setattr(a, "age", 28)
print(a.age)  # 28
```

- `delattr(object, name)`用于删除属性。
```python
class Coordinate:
    x = 10
    y = -5
    z = 0

point1 = Coordinate()

print('x = ', point1.x)  # x =  10
print('y = ', point1.y)  # y =  -5
print('z = ', point1.z)  # z =  0

delattr(Coordinate, 'z')

print('--删除 z 属性后--')  # --删除 z 属性后--
print('x = ', point1.x)  # x =  10
print('y = ', point1.y)  # y =  -5

# 触发错误
print('z = ', point1.z)
# AttributeError: 'Coordinate' object has no attribute 'z'
```

- `class property([fget[, fset[, fdel[, doc]]]])`用于在新式类中返回属性值。
    - `fget` -- 获取属性值的函数
    - `fset` -- 设置属性值的函数
    - `fdel` -- 删除属性值函数
    - `doc` -- 属性描述信息
```python
class C(object):
    def __init__(self):
        self.__x = None

    def getx(self):
        return self.__x

    def setx(self, value):
        self.__x = value

    def delx(self):
        del self.__x

    x = property(getx, setx, delx, "I'm the 'x' property.")

cc = C()
cc.x = 2
print(cc.x)  # 2

del cc.x
print(cc.x)
# AttributeError: 'C' object has no attribute '_C__x'
```

# 魔法方法
魔法方法总是被双下划线包围，例如`__init__`。
魔法方法是面向对象的 Python 的一切，如果你不知道魔法方法，说明你还没能意识到面向对象的 Python 的强大。
魔法方法的“魔力”体现在它们总能够在适当的时候被自动调用。

魔法方法的第一个参数应为`cls`（类方法） 或者`self`（实例方法）。
- `cls`：代表一个类的名称
- `self`：代表一个实例对象的名称

## 基本的魔法方法
- `__init__(self[, ...])` 构造器，当一个实例被创建的时候调用的初始化方法
```python
class Rectangle:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def getPeri(self):
        return (self.x + self.y) * 2

    def getArea(self):
        return self.x * self.y

rect = Rectangle(4, 5)
print(rect.getPeri())  # 18
print(rect.getArea())  # 20
```

- `__new__(cls[, ...])` 在一个对象实例化的时候所调用的第一个方法，在调用`__init__`初始化前，先调用`__new__`。
    - `__new__`至少要有一个参数`cls`，代表要实例化的类，此参数在实例化时由 Python 解释器自动提供，后面的参数直接传递给`__init__`。
    - `__new__`对当前类进行了实例化，并将实例返回，传给`__init__`的`self`。但是，执行了`__new__`，并不一定会进入`__init__`，只有`__new__`返回了，当前类`cls`的实例，当前类的`__init__`才会进入。
```python
class A(object):
    def __init__(self, value):
        print("into A __init__")
        self.value = value

    def __new__(cls, *args, **kwargs):
        print("into A __new__")
        print(cls)
        return object.__new__(cls)


class B(A):
    def __init__(self, value):
        print("into B __init__")
        self.value = value

    def __new__(cls, *args, **kwargs):
        print("into B __new__")
        print(cls)
        return super().__new__(cls, *args, **kwargs)


b = B(10)

# 结果：
# into B __new__
# <class '__main__.B'>
# into A __new__
# <class '__main__.B'>
# into B __init__

class A(object):
    def __init__(self, value):
        print("into A __init__")
        self.value = value

    def __new__(cls, *args, **kwargs):
        print("into A __new__")
        print(cls)
        return object.__new__(cls)


class B(A):
    def __init__(self, value):
        print("into B __init__")
        self.value = value

    def __new__(cls, *args, **kwargs):
        print("into B __new__")
        print(cls)
        return super().__new__(A, *args, **kwargs)  # 改动了cls变为A


b = B(10)

# 结果：
# into B __new__
# <class '__main__.B'>
# into A __new__
# <class '__main__.A'>
```

- 若`__new__`没有正确返回当前类`cls`的实例，那`__init__`是不会被调用的，即使是父类的实例也不行，将没有`__init__`被调用。
利用`__new__`实现单例模式
```python
class Earth:
    pass

a = Earth()
print(id(a))  # 260728291456
b = Earth()
print(id(b))  # 260728291624

class Earth:
    __instance = None  # 定义一个类属性做判断

    def __new__(cls):
        if cls.__instance is None:
            cls.__instance = object.__new__(cls)
            return cls.__instance
        else:
            return cls.__instance

a = Earth()
print(id(a))  # 512320401648
b = Earth()
print(id(b))  # 512320401648
```

- `__new__`方法主要是当你继承一些不可变的 class 时（比如`int, str, tuple`）， 提供给你一个自定义这些类的实例化过程的途径。
```python
class CapStr(str):
    def __new__(cls, string):
        string = string.upper()
        return str.__new__(cls, string)

a = CapStr("i love lsgogroup")
print(a)  # I LOVE LSGOGROUP
```

- `__del__(self)` 析构器，当一个对象将要被系统回收之时调用的方法。
> Python 采用自动引用计数（ARC）方式来回收对象所占用的空间，当程序中有一个变量引用该 Python 对象时，Python 会自动保证该对象引用计数为 1；当程序中有两个变量引用该 Python 对象时，Python 会自动保证该对象引用计数为 2，依此类推，如果一个对象的引用计数变成了 0，则说明程序中不再有变量引用该对象，表明程序不再需要该对象，因此 Python 就会回收该对象。
>
> 大部分时候，Python 的 ARC 都能准确、高效地回收系统中的每个对象。但如果系统中出现循环引用的情况，比如对象 a 持有一个实例变量引用对象 b，而对象 b 又持有一个实例变量引用对象 a，此时两个对象的引用计数都是 1，而实际上程序已经不再有变量引用它们，系统应该回收它们，此时 Python 的垃圾回收器就可能没那么快，要等专门的循环垃圾回收器（Cyclic Garbage Collector）来检测并回收这种引用循环。
```python
class C(object):
    def __init__(self):
        print('into C __init__')

    def __del__(self):
        print('into C __del__')

c1 = C()
# into C __init__
c2 = c1
c3 = c2
del c3
del c2
del c1
# into C __del__
```

- `__str__(self)`:
    - 当你打印一个对象的时候，触发`__str__`
    - 当你使用`%s`格式化的时候，触发`__str__`
    - `str`强转数据类型的时候，触发`__str__`

- `__repr__(self)`：
    - `repr`是`str`的备胎
    - 有`__str__`的时候执行`__str__`,没有实现`__str__`的时候，执行`__repr__`
    - `repr(obj)`内置函数对应的结果是`__repr__`的返回值
    - 当你使用`%r`格式化的时候 触发`__repr__`
```python
class Cat:
    """定义一个猫类"""

    def __init__(self, new_name, new_age):
        """在创建完对象之后 会自动调用, 它完成对象的初始化的功能"""
        self.name = new_name
        self.age = new_age

    def __str__(self):
        """返回一个对象的描述信息"""
        return "名字是:%s , 年龄是:%d" % (self.name, self.age)
        
    def __repr__(self):
        """返回一个对象的描述信息"""
        return "Cat:(%s,%d)" % (self.name, self.age)

    def eat(self):
        print("%s在吃鱼...." % self.name)

    def drink(self):
        print("%s在喝可乐..." % self.name)

    def introduce(self):
        print("名字是:%s, 年龄是:%d" % (self.name, self.age))

# 创建了一个对象
tom = Cat("汤姆", 30)
print(tom)  # 名字是:汤姆 , 年龄是:30
print(str(tom)) # 名字是:汤姆 , 年龄是:30
print(repr(tom))  # Cat:(汤姆,30)
tom.eat()  # 汤姆在吃鱼....
tom.introduce()  # 名字是:汤姆, 年龄是:30
```

`__str__(self)` 的返回结果可读性强。也就是说，`__str__` 的意义是得到便于人们阅读的信息，就像下面的 '2019-10-11' 一样。
`__repr__(self)` 的返回结果应更准确。怎么说，`__repr__` 存在的目的在于调试，便于开发者使用。
```python
import datetime

today = datetime.date.today()
print(str(today))  # 2019-10-11
print(repr(today))  # datetime.date(2019, 10, 11)
print('%s' %today)  # 2019-10-11
print('%r' %today)  # datetime.date(2019, 10, 11)
```

## 算术运算符
类型工厂函数，指的是“不通过类而是通过函数来创建对象”。
```python
class C:
    pass

print(type(len))  # <class 'builtin_function_or_method'>
print(type(dir))  # <class 'builtin_function_or_method'>
print(type(int))  # <class 'type'>
print(type(list))  # <class 'type'>
print(type(tuple))  # <class 'type'>
print(type(C))  # <class 'type'>
print(int('123'))  # 123

# 这个例子中list工厂函数把一个元祖对象加工成了一个列表对象。
print(list((1, 2, 3)))  # [1, 2, 3]
```

- `__add__(self, other)`定义加法的行为：`+`
- `__sub__(self, other)`定义减法的行为：`-`
```python
class MyClass:

    def __init__(self, height, weight):
        self.height = height
        self.weight = weight

    # 两个对象的长相加，宽不变.返回一个新的类
    def __add__(self, others):
        return MyClass(self.height + others.height, self.weight + others.weight)

    # 两个对象的宽相减，长不变.返回一个新的类
    def __sub__(self, others):
        return MyClass(self.height - others.height, self.weight - others.weight)

    # 说一下自己的参数
    def intro(self):
        print("高为", self.height, " 重为", self.weight)


def main():
    a = MyClass(height=10, weight=5)
    a.intro()

    b = MyClass(height=20, weight=10)
    b.intro()

    c = b - a
    c.intro()

    d = a + b
    d.intro()

if __name__ == '__main__':
    main()

# 高为 10  重为 5
# 高为 20  重为 10
# 高为 10  重为 5
# 高为 30  重为 15
```

- `__mul__(self, other)`定义乘法的行为：`*`
- `__truediv__(self, other)`定义真除法的行为：`/`
- `__floordiv__(self, other)`定义整数除法的行为：`//`
- `__mod__(self, other)` 定义取模算法的行为：`%`
- `__divmod__(self, other)`定义当被 `divmod()` 调用时的行为
- `divmod(a, b)`把除数和余数运算结果结合起来，返回一个包含商和余数的元组`(a // b, a % b)`。
```python
print(divmod(7, 2))  # (3, 1)
print(divmod(8, 2))  # (4, 0)
```

- `__pow__(self, other[, module])`定义当被 `power()` 调用或 `**` 运算时的行为
- `__lshift__(self, other)`定义按位左移位的行为：`<<`
- `__rshift__(self, other)`定义按位右移位的行为：`>>`
- `__and__(self, other)`定义按位与操作的行为：`&`
- `__xor__(self, other)`定义按位异或操作的行为：`^`
- `__or__(self, other)`定义按位或操作的行为：`|`

## 反算数运算符
反运算魔方方法，与算术运算符保持一一对应，不同之处就是反运算的魔法方法多了一个“r”。当文件左操作不支持相应的操作时被调用。

- `__radd__(self, other)`定义加法的行为：`+`
- `__rsub__(self, other)`定义减法的行为：`-`
- `__rmul__(self, other)`定义乘法的行为：`*`
- `__rtruediv__(self, other)`定义真除法的行为：`/`
- `__rfloordiv__(self, other)`定义整数除法的行为：`//`
- `__rmod__(self, other)` 定义取模算法的行为：`%`
- `__rdivmod__(self, other)`定义当被 divmod() 调用时的行为
- `__rpow__(self, other[, module])`定义当被 power() 调用或 `**` 运算时的行为
- `__rlshift__(self, other)`定义按位左移位的行为：`<<`
- `__rrshift__(self, other)`定义按位右移位的行为：`>>`
- `__rand__(self, other)`定义按位与操作的行为：`&`
- `__rxor__(self, other)`定义按位异或操作的行为：`^`
- `__ror__(self, other)`定义按位或操作的行为：`|`

`a + b`
这里加数是`a`，被加数是`b`，因此是`a`主动，反运算就是如果`a`对象的`__add__()`方法没有实现或者不支持相应的操作，那么 Python 就会调用`b`的`__radd__()`方法。
```python
class Nint(int):
    def __radd__(self, other):
        return int.__sub__(other, self) # 注意 self 在后面

a = Nint(5)
b = Nint(3)
print(a + b)  # 8
print(1 + b)  # -2
```

## 增量赋值运算符
- `__iadd__(self, other)`定义赋值加法的行为：`+=`
- `__isub__(self, other)`定义赋值减法的行为：`-=`
- `__imul__(self, other)`定义赋值乘法的行为：`*=`
- `__itruediv__(self, other)`定义赋值真除法的行为：`/=`
- `__ifloordiv__(self, other)`定义赋值整数除法的行为：`//=`
- `__imod__(self, other)`定义赋值取模算法的行为：`%=`
- `__ipow__(self, other[, modulo])`定义赋值幂运算的行为：`**=`
- `__ilshift__(self, other)`定义赋值按位左移位的行为：`<<=`
- `__irshift__(self, other)`定义赋值按位右移位的行为：`>>=`
- `__iand__(self, other)`定义赋值按位与操作的行为：`&=`
- `__ixor__(self, other)`定义赋值按位异或操作的行为：`^=`
- `__ior__(self, other)`定义赋值按位或操作的行为：`|=`

## 一元运算符
- `__neg__(self)`定义正号的行为：`+x`
- `__pos__(self)`定义负号的行为：`-x`
- `__abs__(self)`定义当被`abs()`调用时的行为
- `__invert__(self)`定义按位求反的行为：`~x`

## 属性访问
- `__getattr__(self, name)`: 定义当用户试图获取一个不存在的属性时的行为。
- `__getattribute__(self, name)`：定义当该类的属性被访问时的行为（先调用该方法，查看是否存在该属性，若不存在，接着去调用`__getattr__`）。
- `__setattr__(self, name, value)`：定义当一个属性被设置时的行为。
- `__delattr__(self, name)`：定义当一个属性被删除时的行为。
```python
class C:
    def __getattribute__(self, item):
        print('__getattribute__')
        return super().__getattribute__(item)

    def __getattr__(self, item):
        print('__getattr__')

    def __setattr__(self, key, value):
        print('__setattr__')
        super().__setattr__(key, value)

    def __delattr__(self, item):
        print('__delattr__')
        super().__delattr__(item)


c = C()
c.x
# __getattribute__
# __getattr__

c.x = 1
# __setattr__

del c.x
# __delattr__
```

## 描述符
描述符就是将某种特殊类型的类的实例指派给另一个类的属性。
- `__get__(self, instance, owner)`用于访问属性，它返回属性的值。
- `__set__(self, instance, value)`将在属性分配操作中调用，不返回任何内容。
- `__del__(self, instance)`控制删除操作，不返回任何内容。
```python
class MyDecriptor:
    def __get__(self, instance, owner):
        print('__get__', self, instance, owner)

    def __set__(self, instance, value):
        print('__set__', self, instance, value)

    def __delete__(self, instance):
        print('__delete__', self, instance)

class Test:
    x = MyDecriptor()

t = Test()
t.x
# __get__ <__main__.MyDecriptor object at 0x000000CEAAEB6B00> <__main__.Test object at 0x000000CEABDC0898> <class '__main__.Test'>

t.x = 'x-man'
# __set__ <__main__.MyDecriptor object at 0x00000023687C6B00> <__main__.Test object at 0x00000023696B0940> x-man

del t.x
# __delete__ <__main__.MyDecriptor object at 0x000000EC9B160A90> <__main__.Test object at 0x000000EC9B160B38>
```

## 定制序列
协议（Protocols）与其它编程语言中的接口很相似，它规定你哪些方法必须要定义。然而，在 Python 中的协议就显得不那么正式。事实上，在 Python 中，协议更像是一种指南。

**容器类型的协议**
- 如果说你希望定制的容器是不可变的话，你只需要定义`__len__()`和`__getitem__()`方法。
- 如果你希望定制的容器是可变的话，除了`__len__()`和`__getitem__()`方法，你还需要定义`__setitem__()`和`__delitem__()`两个方法。

编写一个不可改变的自定义列表，要求记录列表中每个元素被访问的次数。
```python
class CountList:
    def __init__(self, *args):
        self.values = [x for x in args]
        self.count = {}.fromkeys(range(len(self.values)), 0)

    def __len__(self):
        return len(self.values)

    def __getitem__(self, item):
        self.count[item] += 1
        return self.values[item]


c1 = CountList(1, 3, 5, 7, 9)
c2 = CountList(2, 4, 6, 8, 10)
print(c1[1])  # 3
print(c2[2])  # 6
print(c1[1] + c2[1])  # 7

print(c1.count)
# {0: 0, 1: 2, 2: 0, 3: 0, 4: 0}

print(c2.count)
# {0: 0, 1: 1, 2: 1, 3: 0, 4: 0}
```

- `__len__(self)`定义当被`len()`调用时的行为（返回容器中元素的个数）。
- `__getitem__(self, key)`定义获取容器中元素的行为，相当于`self[key]`。
- `__setitem__(self, key, value)`定义设置容器中指定元素的行为，相当于`self[key] = value`。
- `__delitem__(self, key)`定义删除容器中指定元素的行为，相当于`del self[key]`。

编写一个可改变的自定义列表，要求记录列表中每个元素被访问的次数。
```python
class CountList:
    def __init__(self, *args):
        self.values = [x for x in args]
        self.count = {}.fromkeys(range(len(self.values)), 0)

    def __len__(self):
        return len(self.values)

    def __getitem__(self, item):
        self.count[item] += 1
        return self.values[item]

    def __setitem__(self, key, value):
        self.values[key] = value

    def __delitem__(self, key):
        del self.values[key]
        for i in range(0, len(self.values)):
            if i >= key:
                self.count[i] = self.count[i + 1]
        self.count.pop(len(self.values))

c1 = CountList(1, 3, 5, 7, 9)
c2 = CountList(2, 4, 6, 8, 10)
print(c1[1])  # 3
print(c2[2])  # 6
c2[2] = 12
print(c1[1] + c2[2])  # 15
print(c1.count)
# {0: 0, 1: 2, 2: 0, 3: 0, 4: 0}
print(c2.count)
# {0: 0, 1: 0, 2: 2, 3: 0, 4: 0}
del c1[1]
print(c1.count)
# {0: 0, 1: 0, 2: 0, 3: 0}
```

## 迭代器
- 迭代是 Python 最强大的功能之一，是访问集合元素的一种方式。
- 迭代器是一个可以记住遍历的位置的对象。
- 迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。
- 迭代器只能往前不会后退。
- 字符串，列表或元组对象都可用于创建迭代器
```python
string = 'lsgogroup'
for c in string:
    print(c)

'''
l
s
g
o
g
r
o
u
p
'''

for c in iter(string):
    print(c)
```

```python
links = {'B': '百度', 'A': '阿里', 'T': '腾讯'}
for each in links:
    print('%s -> %s' % (each, links[each]))
    
'''
B -> 百度
A -> 阿里
T -> 腾讯
'''

for each in iter(links):
    print('%s -> %s' % (each, links[each]))
```

- 迭代器有两个基本的方法：`iter()` 和 `next()`。
- `iter(object)` 函数用来生成迭代器。
- `next(iterator[, default])` 返回迭代器的下一个项目。
- `iterator` -- 可迭代对象
- `default` -- 可选，用于设置在没有下一个元素时返回该默认值，如果不设置，又没有下一个元素则会触发 `StopIteration` 异常。
```python
links = {'B': '百度', 'A': '阿里', 'T': '腾讯'}

it = iter(links)
while True:
    try:
        each = next(it)
    except StopIteration:
        break
    print(each)

# B
# A
# T

it = iter(links)
print(next(it))  # B
print(next(it))  # A
print(next(it))  # T
print(next(it))  # StopIteration
```

把一个类作为一个迭代器使用需要在类中实现两个魔法方法 `__iter__()` 与 `__next__()` 。
- `__iter__(self)`定义当迭代容器中的元素的行为，返回一个特殊的迭代器对象， 这个迭代器对象实现了 `__next__()` 方法并通过 `StopIteration` 异常标识迭代的完成。
- `__next__()` 返回下一个迭代器对象。
- `StopIteration` 异常用于标识迭代的完成，防止出现无限循环的情况，在 `__next__()` 方法中我们可以设置在完成指定循环次数后触发 `StopIteration` 异常来结束迭代。
```python
class Fibs:
    def __init__(self, n=10):
        self.a = 0
        self.b = 1
        self.n = n

    def __iter__(self):
        return self

    def __next__(self):
        self.a, self.b = self.b, self.a + self.b
        if self.a > self.n:
            raise StopIteration
        return self.a

fibs = Fibs(100)
for each in fibs:
    print(each, end=' ')

# 1 1 2 3 5 8 13 21 34 55 89
```

## 生成器
- 在 Python 中，使用了 `yield` 的函数被称为生成器（generator）。
- 跟普通函数不同的是，生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器。
- 在调用生成器运行的过程中，每次遇到 `yield` 时函数会暂停并保存当前所有的运行信息，返回 `yield` 的值, 并在下一次执行 `next()` 方法时从当前位置继续运行。
- 调用一个生成器函数，返回的是一个迭代器对象。
```python
def myGen():
    print('生成器执行！')
    yield 1
    yield 2
    
myG = myGen()
for each in myG:
    print(each)

'''
生成器执行！
1
2
'''

myG = myGen()
print(next(myG))  
# 生成器执行！
# 1

print(next(myG))  # 2
print(next(myG))  # StopIteration
```

用生成器实现斐波那契数列
```python
def libs(n):
    a = 0
    b = 1
    while True:
        a, b = b, a + b
        if a > n:
            return
        yield a

for each in libs(100):
    print(each, end=' ')

# 1 1 2 3 5 8 13 21 34 55 89
```

# 模块
模块是一个包含所有你定义的函数和变量的文件，其后缀名是`.py`。模块可以被别的程序引入，以使用该模块中的函数等功能。这也是使用 Python 标准库的方法。

1. 容器 -> 数据的封装
2. 函数 -> 语句的封装
3. 类 -> 方法和属性的封装
4. 模块 -> 程序文件

创建一个 hello.py 文件
```python
# hello.py
def hi():
	print('Hi everyone, I love lsgogroup!')
```

## 命名空间
命名空间因为对象的不同，也有所区别，可以分为如下几种：
1. `内置命名空间（Built-in Namespaces）`：Python 运行起来，它们就存在了。内置函数的命名空间都属于内置命名空间，所以，我们可以在任何程序中直接运行它们，比如 id() ,不需要做什么操作，拿过来就直接使用了。
2. `全局命名空间（Module：Global Namespaces）`：每个模块创建它自己所拥有的全局命名空间，不同模块的全局命名空间彼此独立，不同模块中相同名称的命名空间，也会因为模块的不同而不相互干扰。
3. `本地命名空间（Function & Class：Local Namespaces）`：模块中有函数或者类，每个函数或者类所定义的命名空间就是本地命名空间。如果函数返回了结果或者抛出异常，则本地命名空间也结束了。
![image](https://user-images.githubusercontent.com/44680953/140784135-8f6032b3-446e-47b5-850a-65d92a908509.png)

程序在查询上述三种命名空间的时候，就按照从里到外的顺序，即：Local Namespaces --> Global Namesspaces --> Built-in
Namesspaces。
```python
import hello

hello.hi() # Hi everyone, I love lsgogroup!
hi() # NameError: name 'hi' is not defined
```

## 导入模块
创建一个模块 TemperatureConversion.py
```python
# TemperatureConversion.py
def c2f(cel):
	fah = cel * 1.8 + 32
	return fah

def f2c(fah):
	cel = (fah - 32) / 1.8
	return cel
```

 第一种：import 模块名
 ```python
import TemperatureConversion

print('32摄氏度 = %.2f华氏度' % TemperatureConversion.c2f(32))
print('99华氏度 = %.2f摄氏度' % TemperatureConversion.f2c(99))
# 32摄氏度 = 89.60华氏度
# 99华氏度 = 37.22摄氏度
 ```
 
第二种：from 模块名 import 函数名
```python
from TemperatureConversion import c2f, f2c

print('32摄氏度 = %.2f华氏度' % c2f(32))
print('99华氏度 = %.2f摄氏度' % f2c(99))
# 32摄氏度 = 89.60华氏度
# 99华氏度 = 37.22摄氏度
```

下面的方式不推荐
```python
from TemperatureConversion import *

print('32摄氏度 = %.2f华氏度' % c2f(32))
print('99华氏度 = %.2f摄氏度' % f2c(99))
# 32摄氏度 = 89.60华氏度
# 99华氏度 = 37.22摄氏度
```

第三种：import 模块名 as 新名字
```python
import TemperatureConversion as tc

print('32摄氏度 = %.2f华氏度' % tc.c2f(32))
print('99华氏度 = %.2f摄氏度' % tc.f2c(99))
# 32摄氏度 = 89.60华氏度
# 99华氏度 = 37.22摄氏度
```

## if __name__ == '__main__'
对于很多编程语言来说，程序都必须要有一个入口，而 Python 则不同，它属于脚本语言，不像编译型语言那样先将程序编译成二进制再运行，而是动态的逐行解释运行。也就是从脚本第一行开始运行，没有统一的入口。

假设我们有一个 const.py 文件，内容如下：
```python
PI = 3.14

def main():
	print("PI:", PI)

main()
# PI: 3.14
```

现在，我们写一个用于计算圆面积的 area.py 文件，`area.py`文件需要用到`const.py`文件中的 PI 变量。从`const.py`中，我们把 PI 变量导入`area.py`：
```python
from const import PI

def calc_round_area(radius):
 	return PI * (radius ** 2)
	
def main():
 	print("round area: ", calc_round_area(2))

main()
'''
PI: 3.14
round area: 12.56
'''
```

我们看到`const.py`中的 main 函数也被运行了，实际上我们不希望它被运行，因为`const.py`提供的 main 函数只是为了测试常量定义。这时`if __name__ == '__main__'`派上了用场，我们把`const.py`改一下，添加`if __name__ == "__main__"`：
```python
PI = 3.14

def main():
 	print("PI:", PI)
	
if __name__ == "__main__":
 	main()
```

运行 const.py，输出如下：
```python
PI: 3.14
```

运行 area.py，输出如下：
```python
round area: 12.56
```

`__name__` ：是内置变量，可用于表示当前模块的名字。
```python
import const

print(__name__)
# __main__

print(const.__name__)
# const
```

由此我们可知：如果一个`.py`文件（模块）被直接运行时，其`__name__`值为`__main__`，即模块名为`__main__`。所以，`if __name__ =='__main__'`的意思是：当`.py`文件被直接运行时，`if __name__ == '__main__'`之下的代码块将被运行；当`.py`文件以模块形式被导入时，`if __name__ == '__main__'`之下的代码块不被运行。

## 搜索路径
当解释器遇到 import 语句，如果模块在当前的搜索路径就会被导入。
```python
import sys
print(sys.path)

# ['C:\\ProgramData\\Anaconda3\\DLLs', 'C:\\ProgramData\\Anaconda3\\lib', 'C:\\ProgramData\\Anaconda3', 'C:\\ProgramData\\Anaconda3\\lib\\site-packages',...]
```

我们使用 import 语句的时候，Python 解释器是怎样找到对应的文件的呢？
这就涉及到 Python 的搜索路径，搜索路径是由一系列目录名组成的，Python 解释器就依次从这些目录中去寻找所引入的模块。
这看起来很像环境变量，事实上，也可以通过定义环境变量的方式来确定搜索路径。
搜索路径是在 Python 编译或安装的时候确定的，安装新的库应该也会修改。搜索路径被存储在 sys 模块中的 path 变量中。

## 包（package）
包是一种管理 Python 模块命名空间的形式，采用"点模块名称"。
创建包分为三个步骤：
1. 创建一个文件夹，用于存放相关的模块，文件夹的名字即包的名字。
2. 在文件夹中创建一个 __init__.py 的模块文件，内容可以为空。
3. 将相关的模块放入文件夹中。

不妨假设你想设计一套统一处理声音文件和数据的模块（或者称之为一个"包"）。
现存很多种不同的音频文件格式（基本上都是通过后缀名区分的，例如： .wav，.aiff，.au），所以你需要有一组不断增加的模块，用来在不同的格式之间转换。
并且针对这些音频数据，还有很多不同的操作（比如混音，添加回声，增加均衡器功能，创建人造立体声效果），所以你还需要一组怎么也写不完的模块来处理这些操作。
这里给出了一种可能的包结构（在分层的文件系统中）:
```python
sound/ 顶层包
 	__init__.py 初始化 sound 包
 	formats/ 文件格式转换子包
		__init__.py
		wavread.py
		wavwrite.py
		aiffread.py
		aiffwrite.py
		auread.py
		auwrite.py
 		...
	effects/ 声音效果子包
		__init__.py
		echo.py
		surround.py
		reverse.py
		...
	filters/ filters 子包
		__init__.py
		equalizer.py
		vocoder.py
		karaoke.py
		...
```

在导入一个包的时候，Python 会根据 sys.path 中的目录来寻找这个包中包含的子目录。
目录只有包含一个叫做 __init__.py 的文件才会被认作是一个包，最简单的情况，放一个空的 __init__.py 就可以了。

导入子模块 sound.effects.echo, 他必须使用全名去访问:
```python
import sound.effects.echo

sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)
```
同样会导入子模块: echo，并且他不需要那些冗长的前缀:
```python
from sound.effects import echo

echo.echofilter(input, output, delay=0.7, atten=4)
```
这种方法会导入子模块: echo，并且可以直接使用他的 echofilter() 函数:
```python
from sound.effects.echo import echofilter

echofilter(input, output, delay=0.7, atten=4)
```
注意当使用`from package import item`这种形式的时候，对应的`item`既可以是包里面的`子模块（子包）`，或者包里面定义的其他名称，比如函数，类或者变量。

设想一下，如果我们使用 `from sound.effects import *` 会发生什么？
Python 会进入文件系统，找到这个包里面所有的子模块，一个一个的把它们都导入进来。
导入语句遵循如下规则：如果包定义文件 `__init__.py` 存在一个叫做 `__all__` 的列表变量，那么在使用 `from package import *` 的时候就把这个列表中的所有名字作为包内容导入。

这里有一个例子，在 `sounds/effects/__init__.py` 中包含如下代码：
```python
__all__ = ["echo", "surround", "reverse"]
```
这表示当你使用 `from sound.effects import *` 这种用法时，你只会导入包里面这三个子模块。
如果 `__all__ `真的没有定义，那么使用 `from sound.effects import *` 这种语法的时候，就不会导入包 `sound.effects` 里的任何子模块。他只是把包 `sound.effects` 和它里面定义的所有内容导入进来（可能运行 `__init__.py` 里定义的初始化代码）。

这会把 `__init__.py` 里面定义的所有名字导入进来。并且他不会破坏掉我们在这句话之前导入的所有明确指定的模块。
```python
import sound.effects.echo
import sound.effects.surround
from sound.effects import *
```
这个例子中，在执行 `from...import` 前，包 `sound.effects` 中的 `echo` 和 `surround` 模块都被导入到当前的命名空间中了。

通常我们并不主张使用 `*` 这种方法来导入模块，因为这种方法经常会导致代码的可读性降低。

# datatime 模块
datetime 是 Python 中处理日期的标准模块，它提供了 4 种对日期和时间进行处理的类：datetime、date、time 和 timedelta。

## datatime 类
```python
class datetime(date):
	def __init__(self, year, month, day, hour, minute, second, microsecond, tzinfo)
		pass	
	def now(cls, tz=None):
		pass
	def timestamp(self):
		pass
	def fromtimestamp(cls, t, tz=None):
		pass
	def date(self):
		pass
	def time(self):
		pass
	def year(self):
		pass
	def month(self):
		pass
	def day(self):
		pass
	def hour(self):
		pass
	def minute(self):
		pass
	def second(self):
		pass
	def isoweekday(self):
		pass
	def strftime(self, fmt):
		pass
	def combine(cls, date, time, tzinfo=True):
		pass
```

1. `datetime.now(tz=None) `获取当前的日期时间，输出顺序为：年、月、日、时、分、秒、微秒。
2. `datetime.timestamp()` 获取以 1970年1月1日为起点记录的秒数。
3. `datetime.fromtimestamp(tz=None)` 使用 unixtimestamp 创建一个 datetime。

如何创建一个 datetime 对象？
```python
import datetime

dt = datetime.datetime(year=2020, month=6, day=25, hour=11, minute=23, second=59)
print(dt) # 2020-06-25 11:23:59
print(dt.timestamp()) # 1593055439.0

dt = datetime.datetime.fromtimestamp(1593055439.0)
print(dt) # 2020-06-25 11:23:59
print(type(dt)) # <class 'datetime.datetime'>

dt = datetime.datetime.now()
print(dt) # 2020-06-25 11:11:03.877853
print(type(dt)) # <class 'datetime.datetime'>
```

1. `datetime.strftime(fmt)` 格式化 datetime 对象。
|符号 | 说明|
| ---- | ---- |
|%a | 本地简化星期名称（如星期一，返回 Mon）|
|%A | 本地完整星期名称（如星期一，返回 Monday）|
|%b | 本地简化的月份名称（如一月，返回 Jan）|
|%B | 本地完整的月份名称（如一月，返回 January）|
|%c | 本地相应的日期表示和时间表示 |
|%d | 月内中的一天（0-31）|
|%H | 24小时制小时数（0-23）|
|%I | 12小时制小时数（01-12）|
|%j | 年内的一天（001-366）|
|%m | 月份（01-12）|
|%M | 分钟数（00-59）|
|%p | 本地A.M.或P.M.的等价符|
|%S | 秒（00-59）|
|%U | 一年中的星期数（00-53）星期天为星期的开始|
|%w | 星期（0-6），星期天为星期的开始|
|%W | 一年中的星期数（00-53）星期一为星期的开始|
|%x | 本地相应的日期表示|
|%X | 本地相应的时间表示|
|%y | 两位数的年份表示（00-99）|
|%Y | 四位数的年份表示（0000-9999）|
|%Z | 当前时区的名称（如果是本地时间，返回空字符串）|
|%% | %号本身|

## data 类

## time 类

## timedelta 类

# 文件与文件系统

## 打开文件

## 文件对象方法

## 简洁的 with 语句

# OS模块中关于文件/目录常用的函数

# 序列与反序列化
