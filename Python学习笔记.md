## 数据类型和变量

### 常量

在Python中，通常用全部大写的变量名表示常量：

`PI = 3.14159265359`

### 整数

> 对于很大的数，例如`10000000000`，很难数清楚0的个数。Python允许在数字中间以`_`分隔，因此，写成`10_000_000_000`和`10000000000`是完全一样的。十六进制数也可以写成`0xa1b2_c3d4`。

### 浮点数

> 1.23x10^9就是`1.23e9`

### 复数

```
a=1+2i
a=a+a
```

>`a=2+4i`

### 字符串

> `\n`表示换行，`\t`表示制表符，字符`\`本身也要转义，所以`\\`表示的字符就是`\`
> 
> Python还允许用`r''`表示`''`内部的字符串默认不转义

```
print('''a
b
c```)
```

### 布尔值

```
bool(0)
```

> `False`

### 数据类型转换

## 运算符

### 算术运算符

<img src="https://gitee.com/cpicture/picture-1/raw/master/202202061111537.png" alt="image-20220206111058393" style="zoom:67%;" />

### 位运算符

<img src="https://gitee.com/cpicture/picture-1/raw/master/202202061114888.png" alt="image-20220206111404825" style="zoom:67%;" />

## 字符串与编码

### 字符串与数字的相互转换

```
int("80")
80
int('AB',16)
171
float("80.0")
80.0
```

### 格式化

#### **%S**

```
>>>'hello %s' %'world'
hello world
>>>'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.''
```

#### **format( )**

```
>>>'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
'Hello, 小明, 成绩提升了 17.1%'
>>>'i+i={}'.format(17)
i+i=17
```

#### f-string

```
>>> r = 2.5
>>> s = 3.14 * r ** 2
>>> print(f'The area of a circle with radius {r} is {s:.2f}')
The area of a circle with radius 2.5 is 19.62
```

### 操作字符串

#### 查找字符串

`str.find(sub,[,start[,end]])`

```
>>>s_str='hello world'
>>>s_str.find('l',0,4)
2
```

#### 替换字符串

`str.replace(old,new[,count])`

> `count` 表示个数，如果被省略，替换全部

```
>>>text = "AB CD EF GH IJ"
>>>text.replace('','|',2)
"AB|CD|EF GH IJ"
```

#### 字符串分割

`str.split（sep=None ，maxsplit=-1）`

> 表示：使用`sep`子字符串分割字符串`str`。`maxsplit`是最 大分割次数，如果maxsplit被省略，则表示不限制分割次数。



