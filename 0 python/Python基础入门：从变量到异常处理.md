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
