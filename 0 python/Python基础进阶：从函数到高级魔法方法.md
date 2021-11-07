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

## 函数的返回值

## 变量作用域

## Lambda 的定义

## Lambda 的应用

# 类与对象

# 魔法方法

# 模块

# datatime 模块

# 文件与文件系统

# OS模块中关于文件/目录常用的函数

# 序列与反序列化
