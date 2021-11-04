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
