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

# 模块

# datatime 模块

# 文件与文件系统

# OS模块中关于文件/目录常用的函数

# 序列与反序列化
