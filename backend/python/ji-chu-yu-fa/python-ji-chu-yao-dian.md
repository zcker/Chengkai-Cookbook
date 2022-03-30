---
cover: >-
  https://images.unsplash.com/photo-1646551387209-18632aa0bad3?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE2NDg0MzQ5NTk&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 🏹 python基础要点

## 1.1 Python安装及环境配置

使用anaconda来驱动Python

## 1.2 PyCharm安装

使用virtualenv environment

在anaconda3 文件夹中python.exe

## 1.3 Python介绍

实质是逻辑思维

通达信等有专业的金融语言，缺点为太简单，功能少

## 1.4 用PyCharm写第一个Python程序

配置环境后，简单的写了一个爬虫

## 1.5 基本类型和计算

```python
exit()  # 这样就退出执行了
print() #之间需要换行
print(1, 2, 3)  # 理论上是可以输出无限个元素，你给多少它终端上就输出多少
print(stock_count, type(stock_count))  # type()函数的作用是输出变量的类型
stock_change = .0156  # 浮点数，某个股票涨了1.56%，小数点之前的0可以省略，.0156和0.0156是一样的。
market_capital = 2.33E11  # 市值，可以使用科学技术发来表示很大的数字
空值：只有一个，None。大小写敏感。表示没有值的值
取余数的操作：%
乘方操作：**
布尔运算：and和or
```

## 1.6 Python中的列表-list

```python
list是具有顺序的一组对象，其中的元素不需要是同类型
print(list_var[-2])  # 输出最后第二个位置的元素。
超出长度会报错 IndexError: list index out of range
print(list_var[3:8])  # list[a:b]，从第a个位置开始，一直到第b个位置之前的那些元素
print(list_var[1:7:3])  # list[a:b:c]，每c个元素，选取其中的第一个
print(list_var1 + list_var2)  # 两个list相加
print(1 in list_var)  # 判断1元素，是否在list_var中出现
list常见操作：len，max，min
del list_var[0]  # 删除位置0的那个元素
print(list_var.index(2)) #查找一个元素的在list中的位置
append,在后方增加一个元素,也可增加list
list_var.extend([6, 'seven', [8], '九']) # 两个list合并
逆序、排序:list_var.reverse()和list_var.sort()
排序方法2：print(sorted(list_var))
```

## 1.7 Python中的字典-dict

```python
具有一系列成对的对象。一个叫做key，一个叫做value。其中的元素(包括key和value)不需要是同类型,且无顺序
print(dict_var['sh600004'])  # 获取'sh000001'这个key对应的名称
print(dict_var.get('sh600004'))  # 效果同上
print('sh600000' in dict_var)	#判断一个key是不是在dict里面
输出一个dict中所有的key和value
print(dict_var.keys())  # 输出所有的key
print(dict_var.values())  # 输出所有的value
```

## 1.8 Python中的字符串-str

```python
使用\对特殊字符进行转义。转义也可以用于表达不可见字符，例如tab符号：\t'
如果要表达\本身，也需要转义，写为\\
在字符串的开始加r（Raw String），使得字符串中不发生转义
startswith、endswith:判断字符串是否是以参数开头
print('不' in name)  # 判断字符串中是否包含'邢'
print('邢不行, 邢不行量化'.replace('邢不行', '西蒙斯'))  # 会替换所有的
print(info.split(', '), type(info.split(', ')))
输出：['sh600000', 'sz000001', 'sh600004'] <class 'list'>
print(''.join(list_var))：反向结合起来，''中可以写'-',','等符号
print(phone.strip())  # 去除两边的空格
把字符串当做list
print(name[0])
print(name[:3])
即list有的都可以用
```

## 1.9 条件语句介绍

```python
# 口语化的条件表达
变量名 = 满足条件时候要显示的内容 if 条件 else 不满足条件的时候要显示的内容
status = '涨停' if change >= 0.1 else '没有涨停'
```

## 1.10 循环语句介绍

```python
# range的用法有：
1. range(N)：得到[0, 1, 2, 3, ..., N-1]
2. range(a, b)：得到[a, a+1, ..., b - 1]。这边要注意如果a>=b的话，得到的是[]
print(list(range(10)))  # 强制转为list才能print

for code in ['sh600000', 'sh600002', 'sh600003', 'sz000001']:
for number in range(10 + 1):

# 口语化的循环表达
变量名 = [针对循环中临时变量名的操作语句 for 循环中临时变量名 in 列表]
比如：change_list_rounded = [round(change, 4) for change in change_list]
等于：
change_list_rounded = []
for change in change_list:
    change_rounded = round(change, 4)
    change_list_rounded.append(change_rounded)

while用break跳出循环
```

## 1.11 Python中的函数

```python
函数调用的时候，建议把参数名写全，注意顺序
可以使用隐式参数传递
但是默认参数必须填，仅附加参数会报错
```

## 1.12 异常处理

```python
try:
    执行相关语句1
except Exception as e:
    执行相关语句2
else:
    执行相关语句3

raise ValueError('程序报错！')

综合：
while True:
    try:  # 尝试做以下事情
        buy()
    except Exception as e:  # 如果因为各种原因报错
        print(e)  # 把exception输出出来
        print('警告！下单出错，停止1秒再尝试')
        try_count += 1
        time.sleep(1)
        if try_count >= max_try_count:
            print('超过最大尝试次数，下单失败')
            # 此处需要执行相关程序，通知某些人
            break
        else:
            continue
    else:  # 如果没有报错
        try_count = 0
        print('下单成功了')
        break
```
