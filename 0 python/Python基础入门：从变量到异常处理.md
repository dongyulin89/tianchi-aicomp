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

# 变量和赋值
```h饿
```fu之
```
