# 入门体验小游戏：盲猜数字
```python
import random
guess=random.randint(1,101)
i=1
while True:
  print ("第%d次猜，请输入一个整数数字："%(i))
  try:
    temp=int(input())
    i+=1
  except ValueError :
    print ("输入无效")
    continue
  if temp==guess:
    print ("恭喜你猜对了，就是这个数",guess)
    break;
  elif (temp>guess):
    print ("大了")
  elif (temp<guess):
    print ("小了")
```

# 变量/运算符与数据类型
## 注释
单行注释
```python
# 这是一个注释
print("Hello world")
# Hello world
```
多行注释
```python
'''
这是多行注释，用三个单引号
这是多行注释，用三个单引号
这是多行注释，用三个单引号
'''
print("Hello china")
# Hello china
"""
这是多行注释，用三个双引号
这是多行注释，用三个双引号
这是多行注释，用三个双引号
"""
print("hello china")
# hello china
```

## 运算符
算术运算符
操作符 | 名称 | 示例
:---:|:---:|:---:
`+` | 加 | `1 + 1`
`-` | 减 | `2 - 1`
`*` | 乘 | `3 * 4`
`/` | 除 | `3 / 4`
`//`| 整除（地板除）| `3 // 4`
`%` | 取余| `3 % 4`
`**`| 幂 | `2 ** 3`
```python
print(3 % 2) # 1
print(11 / 3) # 3.6666666666666665
print(11 // 3) # 3
print(2 ** 3) # 8
```

比较运算符
操作符 | 名称 | 示例
:---:|:---:|:---:
`>` |大于| `2 > 1`
`>=`|大于等于| `2 >= 4`
`<` |小于| `1 < 2`
`<=`|小于等于| `5 <= 2`
`==`|等于| `3 == 4`
`!=`|不等于| `3 != 5`
```python
print(1 > 3) # False
print(2 < 3) # True
print(1 == 1) # True
print(1 != 1) # False
```

逻辑运算符
操作符 | 名称 | 示例
:---:|:---:|:---:
`and`|与| `(3 > 2) and (3 < 5)`
`or` |或| `(1 > 3) or (9 < 2)`
`not`|非| `not (2 > 1)`
```python
print((3 > 2) and (3 < 5)) # True
print((1 > 3) and (2 < 1)) # False
print((1 > 3) or (3 < 5)) # True
```

位运算符
操作符 | 名称 | 示例
:---:|:---:|:---:
`~` |按位取反|`~4`
`&` |按位与  |`4 & 5`
`|` |按位或  |`4 | 5`
`^` |按位异或|`4 ^ 5`
`<<`|左移    |`4 << 2`
`>>`|右移    |`4 >> 2`
```python
# 按位非操作 ~
~ 1 = 0
~ 0 = 1

# 按位与操作 &
1 & 1 = 1
1 & 0 = 0
0 & 1 = 0
0 & 0 = 0

# 按位或操作 |
1 | 1 = 1
1 | 0 = 1
0 | 1 = 1
0 | 0 = 0

# 按位异或操作 ^
1 ^ 1 = 0
1 ^ 0 = 1
0 ^ 1 = 1
0 ^ 0 = 0

# 按位左移操作 <<
00 00 10 11 -> 11
11 << 3
---
01 01 10 00 -> 88

# 按位右移操作 >>
00 00 10 11 -> 11
11 >> 2
---
00 00 00 10 -> 2

print(bin(4))  # 0b100
print(bin(5))  # 0b101
print(bin(~4), ~4)  # -0b101 -5
print(bin(4 & 5), 4 & 5)  # 0b100 4
print(bin(4 | 5), 4 | 5)  # 0b101 5
print(bin(4 ^ 5), 4 ^ 5)  # 0b1 1
print(bin(4 << 2), 4 << 2)  # 0b10000 16
print(bin(4 >> 2), 4 >> 2)  # 0b1 1
```

三元运算符
```python
x, y = 4, 5
small = x if x < y else y
print(small)  # 4
```

其他运算符
操作符 | 名称 | 示例
:---:|:---:|:---:
`in`|存在| `'A' in ['A', 'B', 'C']`
`not in`|不存在|`'h' not in ['A', 'B', 'C']`
`is`|是| `"hello" is "hello"`
`not is`|不是|`"hello" is not "hello"`
```python
letters = ['A', 'B', 'C']
if 'A' in letters:
    print('A' + ' exists') # A exists
if 'h' not in letters:
    print('h' + ' not exists') # h not exists

a = "hello"
b = "hello"
print(a is b, a == b)  # True True
print(a is not b, a != b)  # False False
```

注意：
- is, is not 对比的是两个变量的内存地址
- ==, != 对比的是两个变量的值
- 比较的两个变量，指向的都是地址不可变的类型（str等），那么is，is not 和 ==，！= 是完全等价的
- 对比的两个变量，指向的是地址可变的类型（list，dict，tuple等），则两者是有区别的

运算符的优先级
| 运算符 | 描述 |
|-----|-----|
| ** | 指数（最高优先级）   |
| ~+- | 按位翻转，一元加号和减号 |
| * / % // | 乘，除，取模和取整除）   |
| + - | 加法减法 |
| >> << | 右移，左移运算符 |
|&| 位‘AND’|
| ^\| | 位运算符  |
| <=<>>=| 比较运算符 |
| <>==!= | 等于运算符  |
| =%=/=//=-=+=*=**= | 赋值运算符 |
| is is not | 身份运算符   |
| in not in | 成员运算符 |
| not and or | 逻辑运算符   |

```python
print(-3 ** 2)  # -9
print(3 ** -2)  # 0.1111111111111111
print(1 << 3 + 2 & 7)  # 0
print(-3 * 2 + 5 / -2 - 4)  # -12.5
print(3 < 4 and 4 < 5)  # True
```

## 变量和赋值
```python
# 在使用变量之前，需要对其先赋值
teacher = "老马的程序人生"
print(teacher)  # 老马的程序人生

# 变量名可以包括字母、数字、下划线、但变量名不能以数字开头
first = 2
second = 3
third = first + second
print(third)  # 5

# Python 变量名是大小写敏感的，foo != Foo
myTeacher = "老马的程序人生"
yourTeacher = "小马的程序人生"
ourTeacher = myTeacher + ',' + yourTeacher
print(ourTeacher)  # 老马的程序人生,小马的程序人生
```

## 数据类型与转换
基本类型：整型、浮点型、布尔型
容器类型：字符串、元组、列表、字典和集合

类型 | 名称 | 示例
:---:|:---:|:---:
int | 整型 `<class 'int'>`| `-876, 10`
float | 浮点型`<class 'float'>`| `3.149, 11.11`
bool | 布尔型`<class 'bool'>` | `True, False`

整型
```python
a = 1031
print(a, type(a))
# 1031 <class 'int'>
```

浮点型
```python
print(1, type(1))
# 1 <class 'int'>

print(1., type(1.))
# 1.0 <class 'float'>

a = 0.00000023
b = 2.3e-7
print(a)  # 2.3e-07
print(b)  # 2.3e-07

# 保留小数点后n位
import decimal
from decimal import Decimal

b = Decimal(1) / Decimal(3)
print(b)

decimal.getcontext().prec = 4
c = Decimal(1) / Decimal(3)
print(c) # 0.3333
```

布尔型
```python
print(True + True)  # 2
print(True + False)  # 1
print(True * False)  # 0
```

获取类型信息
- type() 不会认为子类是一种父类类型，不考虑继承关系
- isinstance() 会认为子类是一种父类类型，考虑继承关系（推荐）
```python
print(isinstance(1, int))  # True
print(isinstance(5.2, float))  # True
print(isinstance(True, bool))  # True
print(isinstance('5.2', str))  # True
```

类型转换
- 转换为整型 `int(x, base=10)`
- 转换为字符串 `str(object='')`
- 转换为浮点型 `float(x)`
```python
print(int('520'))  # 520
print(int(520.52))  # 520
print(float('520.52'))  # 520.52
print(float(520))  # 520.0
print(str(10 + 10))  # 20
print(str(10.1 + 5.2))  # 15.3
```

## print()函数
- 将对象以字符串表示的方式格式化输出到流文件对象file里。其中所有非关键字参数都按`str()`方式进行转换为字符串输出；
- 关键字参数`sep`是实现分隔符，比如多个参数输出时想要输出中间的分隔字符；
- 关键字参数`end`是输出结束时的字符，默认是换行符`\n`；
- 关键字参数`file`是定义流输出的文件，可以是标准的系统输出`sys.stdout`，也可以重定义为别的文件；
- 关键字参数`flush`是立即把内容输出到流文件，不作缓存。
```python
print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)

shoplist = ['apple', 'mango', 'carrot', 'banana']
print("This is printed without 'end'and 'sep'.")
for item in shoplist:
    print(item) # 换行

# This is printed without 'end'and 'sep'.
# apple
# mango
# carrot
# banana

shoplist = ['apple', 'mango', 'carrot', 'banana']
print("This is printed with 'end='&''.")
for item in shoplist:
    print(item, end='&') # 不换行
print('hello world')

# This is printed with 'end='&''.
# apple&mango&carrot&banana&hello world

shoplist = ['apple', 'mango', 'carrot', 'banana']
print("This is printed with 'sep='&''.")
for item in shoplist:
    print(item, 'another string', sep='&')

# This is printed with 'sep='&''.
# apple&another string
# mango&another string
# carrot&another string
# banana&another string
```

# 位运算

## 利用位运算实现快速计算
通过 `<<`，`>>` 快速计算2的倍数问题。

```python
n << 1 -> 计算 n*2
n >> 1 -> 计算 n/2，负奇数的运算不可用
n << m -> 计算 n*(2^m)，即乘以 2 的 m 次方
n >> m -> 计算 n/(2^m)，即除以 2 的 m 次方
1 << n -> 2^n
```

通过 `^` 快速交换两个整数。
```python
a ^= b
b ^= a
a ^= b
```

通过 `a & (-a)` 快速获取`a`的最后为 1 位置的整数。
```python
00 00 01 01 -> 5
&
11 11 10 11 -> -5
---
00 00 00 01 -> 1

00 00 11 10 -> 14
&
11 11 00 10 -> -14
---
00 00 00 10 -> 2
```

## 利用位运算实现整数集合
一个数的二进制表示可以看作是一个集合（0 表示不在集合中，1 表示在集合中）。

比如集合 `{1, 3, 4, 8}`，可以表示成 `01 00 01 10 10` 而对应的位运算也就可以看作是对集合进行的操作。

元素与集合的操作：
```python
a | (1<<i)  -> 把 i 插入到集合中
a & ~(1<<i) -> 把 i 从集合中删除
a & (1<<i)  -> 判断 i 是否属于该集合（零不属于，非零属于）
```

集合之间的操作：
```python
a 补   -> ~a
a 交 b -> a & b
a 并 b -> a | b
a 差 b -> a & (~b)
```

## python 输出负数的补码
- Python中`bin`一个负数（十进制表示），输出的是它的原码的二进制表示加上个负号，巨坑
- Python中的整型是补码形式存储的
- Python中整型是不限制长度的不会超范围溢出

所以为了获得负数（十进制表示）的补码，需要手动将其和十六进制数`0xffffffff`进行按位与操作，再交给`bin()`进行输出，得到的才是负数的补码表示。
```python
print(bin(3))  # 0b11
print(bin(-3))  # -0b11

print(bin(-3 & 0xffffffff))  
# 0b11111111111111111111111111111101

print(bin(0xfffffffd))       
# 0b11111111111111111111111111111101

print(0xfffffffd)  # 4294967293
```

# 条件语句

## if 语句
```python
if expression:
    expr_true_suite
```

```python
if 2 > 1 and not 2 > 3:
    print('Correct Judgement!')

# Correct Judgement!
```

## if-else 语句
```python
if expression:
    expr_true_suite
else:
    expr_false_suite
```

```python
temp = input("猜一猜小姐姐想的是哪个数字？")
guess = int(temp) # input 函数将接收的任何数据类型都默认为 str。
if guess == 666:
    print("你太了解小姐姐的心思了！")
    print("哼，猜对也没有奖励！")
else:
    print("猜错了，小姐姐现在心里想的是666！")
print("游戏结束，不玩儿啦！")
```

## if-elif-else 语句
```python
if expression1:
    expr1_true_suite
elif expression2:
    expr2_true_suite
    .
    .
elif expressionN:
    exprN_true_suite
else:
    expr_false_suite
```

```python
temp = input('请输入成绩:')
source = int(temp)
if 100 >= source >= 90:
    print('A')
elif 90 > source >= 80:
    print('B')
elif 80 > source >= 60:
    print('C')
elif 60 > source >= 0:
    print('D')
else:
    print('输入错误！')
```

## assert 关键词
`assert`这个关键词我们称之为“断言”，当这个关键词后边的条件为 False 时，程序自动崩溃并抛出`AssertionError`的异常，在进行单元测试时，可以用来在程序中置入检查点，只有条件为 True 才能让程序正常工作
```python
my_list = ['lsgogroup']
my_list.pop(0)
assert len(my_list) > 0

# AssertionError
```

# 循环语句

## while 循环
```python
while 布尔表达式:
    代码块
```

```python 
count = 0
while count < 3:
    temp = input("猜一猜小姐姐想的是哪个数字？")
    guess = int(temp)
    if guess > 8:
        print("大了，大了")
    else:
        if guess == 8:
            print("你太了解小姐姐的心思了！")
            print("哼，猜对也没有奖励！")
            count = 3
        else:
            print("小了，小了")
    count = count + 1
print("游戏结束，不玩儿啦！")
```

```python
string = 'abcd'
while string:
    print(string)
    string = string[1:]

# abcd
# bcd
# cd
# d
```

## while-else 循环
```python
while 布尔表达式:
    代码块
else:
    代码块
```
当`while`循环正常执行完的情况下，执行`else`输出，如果`while`循环中执行了跳出循环的语句，比如 `break`，将不执行`else`代码块的内容。 

```python
count = 0
while count < 5:
    print("%d is  less than 5" % count)
    count = count + 1
else:
    print("%d is not less than 5" % count)
    
# 0 is  less than 5
# 1 is  less than 5
# 2 is  less than 5
# 3 is  less than 5
# 4 is  less than 5
# 5 is not less than 5
```

```python
count = 0
while count < 5:
    print("%d is  less than 5" % count)
    count = 6
    break
else:
    print("%d is not less than 5" % count)

# 0 is  less than 5
```

## for 循环
```python
for 迭代变量 in 可迭代对象:
    代码块
```

```python
for i in 'ILoveLSGO':
    print(i, end=' ')  # 不换行输出

# I L o v e L S G O
```

```python
member = ['张三', '李四', '刘德华', '刘六', '周润发']
for each in member:
    print(each)

# 张三
# 李四
# 刘德华
# 刘六
# 周润发

for i in range(len(member)):
    print(member[i])

# 张三
# 李四
# 刘德华
# 刘六
# 周润发
```

```python 
dic = {'a': 1, 'b': 2, 'c': 3, 'd': 4}

for key, value in dic.items():
    print(key, value, sep=':', end=' ')
    
# a:1 b:2 c:3 d:4 
```

```python
dic = {'a': 1, 'b': 2, 'c': 3, 'd': 4}

for key in dic.keys():
    print(key, end=' ')
    
# a b c d 
```

```python
dic = {'a': 1, 'b': 2, 'c': 3, 'd': 4}

for value in dic.values():
    print(value, end=' ')
    
# 1 2 3 4
```

## for-else 循环
```python
for 迭代变量 in 可迭代对象:
    代码块
else:
    代码块
```
当`for`循环正常执行完的情况下，执行`else`输出，如果`for`循环中执行了跳出循环的语句，比如 `break`，将不执行`else`代码块的内容，与`while - else`语句一样

```python
for num in range(10, 20):  # 迭代 10 到 20 之间的数字
    for i in range(2, num):  # 根据因子迭代
        if num % i == 0:  # 确定第一个因子
            j = num / i  # 计算第二个因子
            print('%d 等于 %d * %d' % (num, i, j))
            break  # 跳出当前循环
    else:  # 循环的 else 部分
        print(num, '是一个质数')

# 10 等于 2 * 5
# 11 是一个质数
# 12 等于 2 * 6
# 13 是一个质数
# 14 等于 2 * 7
# 15 等于 3 * 5
# 16 等于 2 * 8
# 17 是一个质数
# 18 等于 2 * 9
# 19 是一个质数
```

## range() 函数
```python
range([start,] stop[, step=1])
```
- `step=1` 表示第三个参数的默认值是1。
- `range` 这个BIF的作用是生成一个从`start`参数的值开始到`stop`参数的值结束的数字序列，该序列包含`start`的值但不包含`stop`的值。

```python
for i in range(2, 9):  # 不包含9
    print(i)

# 2
# 3
# 4
# 5
# 6
# 7
# 8
```

```python
for i in range(1, 10, 2):
    print(i)

# 1
# 3
# 5
# 7
# 9
```

## enumerate()函数
```python
enumerate(sequence, [start=0])
```
- sequence：一个序列、迭代器或其他支持迭代对象。
- start：下标起始位置。
- 返回 enumerate(枚举) 对象

```python
seasons = ['Spring', 'Summer', 'Fall', 'Winter']
lst = list(enumerate(seasons))
print(lst)
# [(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
lst = list(enumerate(seasons, start=1))  # 下标从 1 开始
print(lst)
# [(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
```

```python
languages = ['Python', 'R', 'Matlab', 'C++']
for language in languages:
    print('I love', language)
print('Done!')
# I love Python
# I love R
# I love Matlab
# I love C++
# Done!


for i, language in enumerate(languages, 2):
    print(i, 'I love', language)
print('Done!')
# 2 I love Python
# 3 I love R
# 4 I love Matlab
# 5 I love C++
# Done!
```

## break 语句
```python
import random
secret = random.randint(1, 10) #[1,10]之间的随机数

while True:
    temp = input("猜一猜小姐姐想的是哪个数字？")
    guess = int(temp)
    if guess > secret:
        print("大了，大了")
    else:
        if guess == secret:
            print("你太了解小姐姐的心思了！")
            print("哼，猜对也没有奖励！")
            break
        else:
            print("小了，小了")
print("游戏结束，不玩儿啦！")
```

## continue 语句
```python
for i in range(10):
    if i % 2 != 0:
        print(i)
        continue
    i += 2
    print(i)

# 2
# 1
# 4
# 3
# 6
# 5
# 8
# 7
# 10
# 9
```

## pass 语句
`pass`是空语句，不做任何操作，只起到占位的作用，其作用是为了保持程序结构的完整性。尽管`pass`语句不做任何操作，但如果暂时不确定要在一个位置放上什么样的代码，可以先放置一个`pass`语句，让代码可以正常运行。
```python
def a_func():

# SyntaxError: unexpected EOF while parsing
```

```python
def a_func():
    pass
```

## 推导式
列表推导式
```python
[ expr for value in collection [if condition] ]
```

```python
x = [-4, -2, 0, 2, 4]
y = [a * 2 for a in x]
print(y)
# [-8, -4, 0, 4, 8]

x = [i ** 2 for i in range(1, 10)]
print(x)
# [1, 4, 9, 16, 25, 36, 49, 64, 81]

x = [(i, i ** 2) for i in range(6)]
print(x)
# [(0, 0), (1, 1), (2, 4), (3, 9), (4, 16), (5, 25)]

x = [i for i in range(100) if (i % 2) != 0 and (i % 3) == 0]
print(x)
# [3, 9, 15, 21, 27, 33, 39, 45, 51, 57, 63, 69, 75, 81, 87, 93, 99]

a = [(i, j) for i in range(0, 3) for j in range(0, 3)]
print(a)
# [(0, 0), (0, 1), (0, 2), (1, 0), (1, 1), (1, 2), (2, 0), (2, 1), (2, 2)]

x = [[i, j] for i in range(0, 3) for j in range(0, 3)]
print(x)
# [[0, 0], [0, 1], [0, 2], [1, 0], [1, 1], [1, 2], [2, 0], [2, 1], [2, 2]]
x[0][0] = 10
print(x)
# [[10, 0], [0, 1], [0, 2], [1, 0], [1, 1], [1, 2], [2, 0], [2, 1], [2, 2]]

a = [(i, j) for i in range(0, 3) if i < 1 for j in range(0, 3) if j > 1]
print(a)
# [(0, 2)]
```

元组推导式
```python
( expr for value in collection [if condition] )
```

```python
a = (x for x in range(10))
print(a)
# <generator object <genexpr> at 0x0000025BE511CC48>

print(tuple(a))
# (0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
```

字典推导式
```python
{ key_expr: value_expr for value in collection [if condition] }
```

```python
b = {i: i % 2 == 0 for i in range(10) if i % 3 == 0}
print(b)
# {0: True, 3: False, 6: True, 9: False}
```

集合推导式
```python
{ expr for value in collection [if condition] }
```

```python
c = {i for i in [1, 2, 3, 4, 5, 5, 6, 4, 3, 2, 1]}
print(c)
# {1, 2, 3, 4, 5, 6}
```

`next(iterator[, default])`
Return the next item from the iterator. If default is given and the iterator is exhausted, it is returned instead of raising StopIteration.
```python
e = (i for i in range(10))
print(e)
# <generator object <genexpr> at 0x0000007A0B8D01B0>

print(next(e))  # 0
print(next(e))  # 1

for each in e:
    print(each, end=' ')

# 2 3 4 5 6 7 8 9
```

# 异常处理
## Python 标准异常总结

- BaseException：所有异常的 **基类**
- Exception：常规异常的 **基类**
- StandardError：所有的内建标准异常的基类
- ArithmeticError：所有数值计算异常的基类
- FloatingPointError：浮点计算异常
- <u>OverflowError</u>：数值运算超出最大限制
- <u>ZeroDivisionError</u>：除数为零
- <u>AssertionError</u>：断言语句（assert）失败
- <u>AttributeError</u>：尝试访问未知的对象属性
- EOFError：没有内建输入，到达EOF标记
- EnvironmentError：操作系统异常的基类
- IOError：输入/输出操作失败
- <u>OSError</u>：操作系统产生的异常（例如打开一个不存在的文件）
- WindowsError：系统调用失败
- <u>ImportError</u>：导入模块失败的时候
- KeyboardInterrupt：用户中断执行
- LookupError：无效数据查询的基类
- <u>IndexError</u>：索引超出序列的范围
- <u>KeyError</u>：字典中查找一个不存在的关键字
- <u>MemoryError</u>：内存溢出（可通过删除对象释放内存）
- <u>NameError</u>：尝试访问一个不存在的变量
- UnboundLocalError：访问未初始化的本地变量
- ReferenceError：弱引用试图访问已经垃圾回收了的对象
- RuntimeError：一般的运行时异常
- NotImplementedError：尚未实现的方法
- <u>SyntaxError</u>：语法错误导致的异常
- IndentationError：缩进错误导致的异常
- TabError：Tab和空格混用
- SystemError：一般的解释器系统异常
- <u>TypeError</u>：不同类型间的无效操作
- <u>ValueError</u>：传入无效的参数
- UnicodeError：Unicode相关的异常
- UnicodeDecodeError：Unicode解码时的异常
- UnicodeEncodeError：Unicode编码错误导致的异常
- UnicodeTranslateError：Unicode转换错误导致的异常

异常体系内部有层次关系，Python异常体系中的部分关系如下所示：
![](https://img-blog.csdnimg.cn/20200710131404548.png)

## Python 标准警告总结
- Warning：警告的基类
- DeprecationWarning：关于被弃用的特征的警告
- FutureWarning：关于构造将来语义会有改变的警告
- UserWarning：用户代码生成的警告
- PendingDeprecationWarning：关于特性将会被废弃的警告
- RuntimeWarning：可疑的运行时行为(runtime behavior)的警告
- SyntaxWarning：可疑语法的警告
- ImportWarning：用于在导入模块过程中触发的警告
- UnicodeWarning：与Unicode相关的警告
- BytesWarning：与字节或字节码相关的警告
- ResourceWarning：与资源使用相关的警告

## try-except 语句
```python
try:
    检测范围
except Exception[as reason]:
    出现异常后的处理代码
```

- 首先，执行`try`子句（在关键字`try`和关键字`except`之间的语句），如果没有异常发生，忽略`except`子句，`try`子句执行后结束。
- 如果在执行`try`子句的过程中发生了异常，那么`try`子句余下的部分将被忽略。如果异常的类型和`except`之后的名称相符，那么对应的`except`子句将被执行。最后执行`try - except`语句之后的代码。
- 如果一个异常没有与任何的`except`匹配，那么这个异常将会传递给上层的`try`中。

```python
try:
    f = open('test.txt')
    print(f.read())
    f.close()
except OSError:
    print('打开文件出错')
# 打开文件出错

try:
    f = open('test.txt')
    print(f.read())
    f.close()
except OSError as error:
    print('打开文件出错\n原因是：' + str(error))
# 打开文件出错
# 原因是：[Errno 2] No such file or directory: 'test.txt'

try:
    int("abc")
    s = 1 + '1'
    f = open('test.txt')
    print(f.read())
    f.close()
except OSError as error:
    print('打开文件出错\n原因是：' + str(error))
except TypeError as error:
    print('类型出错\n原因是：' + str(error))
except ValueError as error:
    print('数值出错\n原因是：' + str(error))
# 数值出错
# 原因是：invalid literal for int() with base 10: 'abc'

dict1 = {'a': 1, 'b': 2, 'v': 22}
try:
    x = dict1['y']
except LookupError:
    print('查询错误')
except KeyError:
    print('键错误')
else:
    print(x)
# 查询错误

dict1 = {'a': 1, 'b': 2, 'v': 22}
try:
    x = dict1['y']
except KeyError:
    print('键错误')
except LookupError:
    print('查询错误')
else:
    print(x)
# 键错误

try:
    s = 1 + '1'
    int("abc")
    f = open('test.txt')
    print(f.read())
    f.close()
except (OSError, TypeError, ValueError) as error:
    print('出错了！\n原因是：' + str(error))
# 出错了！
# 原因是：unsupported operand type(s) for +: 'int' and 'str'
```

## try-except-finally 语句
```python
try:
    检测范围
except Exception[as reason]:
    出现异常后的处理代码
finally:
    无论如何都会被执行的代码
```

```python
def divide(x, y):
    try:
        result = x / y
        print("result is", result)
    except ZeroDivisionError:
        print("division by zero!")
    finally:
        print("executing finally clause")


divide(2, 1)
# result is 2.0
# executing finally clause
divide(2, 0)
# division by zero!
# executing finally clause
divide("2", "1")
# executing finally clause
# TypeError: unsupported operand type(s) for /: 'str' and 'str'
```

## try-except-else 语句
```python
try:
    检测范围
except:
    出现异常后的处理代码
else:
    如果没有异常执行这块代码
```

```python
try:
    检测范围
except(Exception1[, Exception2[,...ExceptionN]]]):
   发生以上多个异常中的一个，执行这块代码
else:
    如果没有异常执行这块代码
```

```python
try:
    fh = open("testfile.txt", "w")
    fh.write("这是一个测试文件，用于测试异常!!")
except IOError:
    print("Error: 没有找到文件或读取文件失败")
else:
    print("内容写入文件成功")
    fh.close()

# 内容写入文件成功
```

## raise 语句
Python 使用`raise`语句抛出一个指定的异常
```python
try:
    raise NameError('HiThere')
except NameError:
    print('An exception flew by!')
    
# An exception flew by!
```
