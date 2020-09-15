# 计算机基础

## 一、计算机的基本概念

### 1.计算机是什么：

计算机，俗称电脑，是现代一种用于高速计算的电子**计算**机器。
特点：数值计算、逻辑计算、存储记忆等功能。

### 2.计算机的组成

硬件：鼠标、显示器、CPU、硬盘…，看得见、摸得着。
软件：QQ、PyCharm、浏览器…,看不见、摸不着。
软件就是一系列按照特定顺序组织的**计算机数据**和**特定指令**的集合。

## 二、计算机语言

### 1.计算机语言的概念

数字、字符和语法规则组成了计算机中的各种指令（或者各种语句）。
计算机语言就是用于人与计算机之间通讯的语言。

### 2.计算机语言的发展

机器语言 → 汇编语言 → 高级计算机语言
根据转换的时机不同，高级计算机语言又分为：

- 编译型语言
  **C语言**等：
  x（源码） → 编译 → y（编译后的机器码）
  特点：执行速度快，将整个代码全部编译后再执行，但是跨平台比较差。
  用于开发大型数据库等。
- 解释型语言
  **Java、Python**等：
  x（源码） → 解释器（虚拟机） → 解释执行
  特点：执行速度比较慢，因为逐行解释再执行，但是跨平台性好。
  用于开发脚本、接口等。

## 三、交互方式

### 1.交互方式种类

#### （1）命令行的交互方式TUI

#### （2）图形界面化的交互方式GUI

interface：
接口，即软件的各种功能选项，且针对不同的用户有不同的接口和权限。

#### 交互模式的打开方式

TUI（基于Windows系统）
win键+R键 → 出现运行窗口 → 输入cmd → 回车。
命令行结构：
版本号+版权声明
**>**：
命令提示符，后面直接输入指令。
软盘驱动器：早期，分别为A盘和B盘，是重要的存储介质，A盘一般使用3.5英寸软盘，B盘一般使用5.25英寸软盘。
软盘特点：内存小，读取速度慢，容易丢失，寿命很短，难以满足需求。

### 2.Dos命令

.表示当前目录
..表示上一级目录
**cd**：切换
**del**：删除
**rd**：清空
**md**：创建目录

## 四、文本文件和字符集

文本文件一般分为两种：
**纯文本**：只能保存单一内容（不能保存图片、字体颜色…）
**富文本**：可以保存文本以外的内容（有道笔记、word文档…）
在开发的时候采用纯文本开发。

将字符转换成二进制的过程称为编码（Encode）；
将二进制转换成字符的过程称为解码（Decode）。
编码和解码时所采用的规则成为字符集
常见的字符集：
**ASCII：**
美国用，使用7位对美国的常用的字符进行编码，包含128个字符。
**ISO-8859-1：**
欧洲用，使用8位，包含256个字符。
**GBK：**
国标码，我国的编码
**Unicode：**
万国码，包含世界所有的语言和符号，分为多种实现方式，如utf-8、utf-16、utf-32等。
最常见的是utf-8。
utf-8为1-5字节，utf-16为2-4个字节，utf-32为4个字节。

字节Byte：计算机用于**计量存储和传输容量**的一种计量单位。
1个字节 = 8位二进制，
一个英文字母（不区分大小写）占1个字节，一个中文汉字占2个字节。
符号：英文标点占1个字节，如中文标点占2个字节如“.”占1个字节，“。”占2个字节。
字符：计算机中使用的字母、数字、字和符号，如1、2、3、a、b、c、@、#、￥等。
字符、字节、字的关系：
2个字符对应1个字，1个字等于2个字节，1个字符等于1个字节。

## 五、进制

生活中：十进制
计算机：二进制，二进制基本单位是8位，还有八进制、十六进制等。
出现除二进制的其他进制的原因：
进制越大，其表现形式越短，可以更方便地表示数据。

### 1.进制间的转换

二进制和十进制之间的转换：
十进制 -> 二进制：
原理：对十进制进行除2运算。
举例：
5 / 2 = 2…1
2 / 2 = 1…0
1 / 2 = 0…1
所以为 101。

二进制 → 十进制：
原理：二进制乘以二次幂的过程。
举例：
1 * 20 + 0 * 21 + 1 * 22 = 1 + 0 + 4 = 5
如果要转换成十六进制，要先转换成二进制。
90 → 0101,1010
5 10(A)
90 = 0x5A

### 2.进制的计数

十进制：满十进一
十进制共有10个数字0、1、2、3、4、5、6、7、8、9
十进制计数：0、1、2、3…9、10、11、12…19、20、21…39、40…
二进制：满二进一
二进制中有2个数字0、1
二进制技术：0、1、10、11、100、101…1000、1001…
八进制：满八进一
八进制共有8个数字0、1、2、3、4、5、6、7
八进制计数：
0、1、2、3、4、5、6、7、10、11、12…17、20、21…、27、30、31…
十六进制：满十六进一
十六进制共有16个数字0、1、2、3、4、5、6、7、8、9、A、B、C、D、E、F
十六进制计数：
0、1、2、3…9、A、B、C、D、E、F、10、11、12、13…19、1A、1B…

### 3.数据间的换算

二进制的计算：
内存中每一个最小单位成为1bit（位），bit是计算机中最小的单位，byte是我们可以操作的最小单位。
8bit=1Byte（字节）
1024Byte=1KB（千字节）
1024kb=1MB（兆字节）
1024MB=1GB（吉字节）
1024GB=1TB（太字节）

## 六、环境变量

环境变量就是操作系统中的一些变量。

### 1.环境变量操作

#### 查看环境变量

Windows系统：
右键此电脑选择属性 → 左侧选择高级系统配置 → 选择环境变量。
环境变量分为两个部分：
**用户变量**和**系统变量**

#### 添加环境变量

通过新建按钮添加环境变量；一个环境变量有多个值时，值与值之间用英文分号（;）分开。

#### 修改环境变量

通过编辑按钮来修改环境变量。

#### 删除环境变量

通过删除按钮来删除环境变量。

### 2.path环境变量

path环境变量：保存的是单个的路径。
在命令行输入一个命令或访问一个文件时，系统会在当前目录下寻找，如果有就直接打开或者执行，如果没有，会在path环境变量的路径中依次寻找，直到找到为止，如果path环境变量中没有找到该路径，则报错。

# Python基本常识

## 一、Python基本概念

Python概念：
是一门易于学习、功能强大的编程语言，效率高、能简单又有效地实现面向对象编程，简洁的语法与动态输入特性，解释性语言的本质，在多种领域与绝大多数平台都能进行脚本编写和应用快速开发工作。

**创始人：Guido von Rossum(吉多·范罗苏姆)**，尊称龟叔，于1989年基于abc语言开发出Python，英语名称意为**蟒蛇**。
Java：1991年开发，由Sun团队开发维护。

Python特点：**简单、易于学习、自由且开放、跨平台、可嵌入性、丰富的库**。
可嵌入性：可以在其他语言如C、C++中嵌入Python脚本。
口号：人生苦短，我用Python！ ***Life is short, I use Python!\***

## 二、Python语言的发展及应用

### 1.常规的软件开发

Android：Java → Kotlin
iOS：OC → Swift

### 2.科学计算

绘制2D、3D图像。

### 3.自动化运维

### 4.云计算

### 5.Web开发

Djongo、Flask、Tornado等框架。

### 6.网络爬虫开发

**scrapy爬虫框架**。

### 7.数据分析

### 8.人工智能

机器学习、神经网络、深度学习…

※※**Python之禅：**※※

> The Zen of Python, by Tim Peters
> Beautiful is better than ugly.
> Explicit is better than implicit.
> Simple is better than complex.
> Complex is better than complicated.
> Flat is better than nested.
> Sparse is better than dense.
> Readability counts.
> Special cases aren’t special enough to break the rules.
> Although practicality beats purity.
> Errors should never pass silently.
> Unless explicitly silenced.
> In the face of ambiguity, refuse the temptation to guess.
> There should be one-- and preferably only one --obvious way to do it.
> Although that way may not be obvious at first unless you’re Dutch.
> Now is better than never.
> Although never is often better than *right* now.
> If the implementation is hard to explain, it’s a bad idea.
> If the implementation is easy to explain, it may be a good idea.
> Namespaces are one honking great idea – let’s do more of those!

**中文翻译及解释**

> Python之禅 by Tim Peters
> 优美胜于丑陋（Python 以编写优美的代码为目标）
> 明了胜于晦涩（优美的代码应当是明了的，命名规范，风格相似）
> 简洁胜于复杂（优美的代码应当是简洁的，不要有复杂的内部实现）
> 复杂胜于凌乱（如果复杂不可避免，那代码间也不能有难懂的关系，要保持接口简洁）
> 扁平胜于嵌套（优美的代码应当是扁平的，不能有太多的嵌套）
> 间隔胜于紧凑（优美的代码有适当的间隔，不要奢望一行代码解决问题）
> 可读性很重要（优美的代码是可读的）
> 即便假借特例的实用性之名，也不可违背这些规则（这些规则至高无上）
> 不要包容所有错误，除非你确定需要这样做（精准地捕获异常，不写 except:pass 风格的代码）
> 当存在多种可能，不要尝试去猜测
> 而是尽量找一种，最好是唯一一种明显的解决方案（如果不确定，就用穷举法）
> 虽然这并不容易，因为你不是 Python 之父（这里的 Dutch 是指 Guido ）
> 做也许好过不做，但不假思索就动手还不如不做（动手之前要细思量）
> 如果你无法向人描述你的方案，那肯定不是一个好方案；反之亦然（方案测评标准）
> 命名空间是一种绝妙的理念，我们应当多加利用（倡导与号召）

## 三、Python环境搭建

### 1.Python解释器分类：

CPython:官方，用C语言编写；
**PyPy**：用Python语言编写的解释器，最常用 ；
IronPython：用.net语言编写；
JPython：用Java语言编写。

### 2.Python安装

#### 下载相关文件：

帮助文件；
嵌入式文件；
**可执行文件**（即安装包，通常下载该文件）；
联网安装文件。
都可能涉及到64与32位的选择。

#### 安装包安装：

- 立即安装
- 自定义安装

选择：是否安装文档；是否安装pip工具包；是否安装IDLE开发工具（Python自带）；是否安装Python测试。
高级选项：是否把Python添加到环境变量。
安装后可以在新打开的Dos命令窗口中输入`python`命令来检测是否安装成功。

## 四、Python的第一个程序

### 1.用文本文件

建立test.txt，里边写入`print('Hello World')`，保存退出，在命令行中切到test.txt所在路径，并执行`python test.txt`，便会打印出`Hello World`。

### 2.用python文件

在Python IDE中建立test.py或者将test.txt的后缀改为.py，在其中写入`print('Hello World')`，在命令行中直接输入`test.py`，也会打印`Hello World`。

## 五、PyCharm的安装和配置

官方网站：
http://www.jetbrains.com/pycharm/

1. 下载安装包后进行安装，并打开PyCharm，并创建项目，选择编译器为之前安装的Python版本。
2. 创建Python文件，然后右键运行，会在控制台有相应输出。
3. 在PyCharm中可以对主题、字体、快捷键、是否自动更新、自动导包、设置头文件信息、编码格式等进行修改和**个性化设置**。

# python基本数据类型

## 一、几个概念

### 1.表达式

表达式，是由数字、算符、数字分组符号（括号）、自由变量和约束变量等以能求出数值的有意义排列方法所得的组合。
用通俗的话讲，表达式就是类似于数学公式的东西。
表达式不会对程序产生影响。

### 2.语句

语句，是一个语法上自成体系的单位，它由一个词或语法上有关联的一组词构成。
语句会对程序产生影响。

### 3.程序

程序就是由一条一条的语句和一条一条的表达式组成的。

### 4.函数

函数是专门用来完成特定功能的语句。
形如：`xxx()`。
**参数**：函数可以不添加参数，也可以添加一个或多个参数，如有多个参数，应用“,”隔开。
**返回值**：不是所有函数都有返回值。
函数的分类：
**内置函数**：由Python解释器提供，可以直接使用；
**自定义函数**：自己根据需要定义的函数。

## 二、标识符

Python语言由关键字、标识符、注释、变量和数值、运算符、语句、函数、序列等8个部分组成；
Python的8个关键要素为数据类型、对象引用、组合数据类型、逻辑操作符、控制流语句、算数操作符、输入/输出和函数的创建与调用。

### 1.关键字

Python自己使用，我们不能再自己定义使用的词。
可以在console中执行`import keyword`，再执行`keyword.kwlist`查看Python的关键字。

### 2.标识符

开发人员在程序中自定义的一些符号的名称，例如变量名、类名、函数名等。

#### 组成：

由26个英文字母（大小写）、数字0-9、符号_$等组成。

#### 规则：

（1）标识符可以包含字母、数字、_，但是不能使用数字做开头，例如name1、name_、_name等；
（2）不能使用关键字和保留字作为标识符。

#### 命名方式：

**命名原则：见名识意。**
命名法：
**小驼峰**：第一个单词以小写字母开始，后边单词首字母大写，如myName、aDog；
**大驼峰**：每一个单词的首字母大写，如FirstName、LastName；
**下划线**：用下划线来连接两个有含义的单词，如get_url,buffer_name。

## 三、整数和小数

### 1.整数

即整型，如a=1、b=2等，都是int类型。
※计算机中存储的数值不是无穷大，有一定的范围；
※遇到比较大的数，可以每隔几位用下划线_分割，如123_456_789等。

### 2.小数

即浮点型，如a=1.2、b=0.09。
※浮点数是有误差的，如Python中计算0.1+0.2=0.30000000000000004，而不是0.3，这是因为计算机中运算用的是二进制数字，而在Python中输入的是十进制数字，在Python运行时将十进制转化为二进制时会产生误差，从而使计算产生误差。

## 四、布尔类型和空值

### 1.布尔类型

布尔型只有两个值**True**和**False**，基本都是用于逻辑判断。
※布尔值实际上也属于整型，True相当于1，False相当于0。

### 2.空值

即**None**，是常量，表示数据是一个空值。

## 五、字符串和转义字符

### 1.字符串

字符串就是由字符、数字、下划线组成的一串字符，如’Hello’、“World”。
※字符串用单引号或双引号包含，但是但单双引号必须成对使用，不能混用，并且相同引号之间不能嵌套。
※可由`type()`函数来检测字符串的类型。

### 2.转义字符

即“\”,使字符在Python中单独出现的意义消失，使得以转义字符开头的字符序列具有不同于该字符序列单独出现时的语义。
\‘表示’
\“表示”
\t表示制表符tab
\n表示换行符
\表示反斜杠
※可以在字符串前加字母’r’使其具有最原本的意义。

## 六、长字符串

对于较长的字符串，在每一行后加反斜杠\可使不同行组成一个字符串；
对于多行的字符串，需要用三重引号表示，这样可以换行，同时保留字符串中的格式，如

```python
'''ascii(object)
As repr(), return a string containing a printable representation of 
an object, but escape the non-ASCII characters in the string returned 
by repr() using \x, \u or \U escapes. 
This generates a string similar to that returned by repr() in Python 2.
'''
123456
```

即会保留字符串在文档中的格式并换行。

## 七、格式化字符串

### 方式一 拼字符串：

只有字符串与字符串才能拼接，如`print('s = '+s)` 。
可以进行类型转换，如执行`6+int('6')`,会输出12。

### 方式二 多个参数：

传入多个参数，如`print('s =',s)`。

### 方式三 占位符

创建字符串时可以在字符串中指定占位符。
**%s 字符
%d 整数**
例如`print('I love %s' % 'Python')`，`print('I love %s and %s' % ('Python','Java'))`。
**注意：占位数必须和后边的参数个数相同，否则会报错。**

### 方式四 “新式”字符串格式化

```python
s='Python'
print('I love {0}'.format(s))
12
```

### 方式五 字符串插值f-Strings

```python
s1='Python'
s2='Java'
print(f'I love {s1} and {s2}')
123
```

※如需了解字符串的详细用法，可参考https://blog.csdn.net/ning13481193737/article/details/80948501。

## 八、字符串的其他操作

### 1.字符串长度

用`len()`函数。

### 2.字符串运算

用`s*20`即可将字符串s打印20编。

### 3.字符串包含

用in来判断一个字符串是否在另一个字符串中，如用`a in b`来判断a是否包含于b。

### 4.求最大值和最小值

用`max()`和`min()`函数。

### 5.求字符在ASCII表中的十进制数值

用`ord()`函数，如`print(ord('A'))`。

### 6.分割字符串

用`split()`函数，返回列表。
如

```python
s='I love Python'
s.split(' ')
12
```

### 7.拼接字符串

用`join()`方法，如`'_'.join(s)`。

### 8.去掉空格

`strip()`去掉字符串左右两边的空格；
`lstrip()`去掉字符串左边的空格；
`rstrip()`去掉字符串右边的空格。
如

```python
s=' I love Python '
print(s.strip())
print(s.lstrip())
print(s.rstrip())
1234
```

### 9.字符串大小写

`upper()`全部大写；
`lower()`全部小写；
`capitalize()`首字母大写；
`isupper()`判断是否大写；
`islower()`判断是否小写。

例如：

```python
s='I love Python'
print(s.upper())
print(s.capitialize())
print(s.islower())
1234
```

## 九、变量

### 1.什么时候定义变量

当数据不确定、需要对数据进行存储时，就需要定义一个变量来完成存储。

### 2.什么是变量

变量就是计算机内存中的一块区域，存储规定范围内的值。
※值可以改变，通俗地讲，变量就是给数据取名字；
※同时，变量名也要符合标识符的命名规则；
※通过`id()`来查不同变量的存储在内存中的地址；
※**两个变量相等**和**两个变量是同一个对象**是两个概念。

### 3.变量的运算

```python
a=10
b=4
print(a+b)
123
```

总结：只要在运算过程中含有浮点数，那么返回的就是一个浮点数。

# Python基础之4.运算符

## 一、算术运算符

### 1.运算符：

用于执行程序代码运算，会针对一个以上操作数项目来进行运算。
例如：2+3，其操作数是2和3，运算符是“+”。

### 2.算数运算符

表现形式：

#### （1）加法运算符+

例如:

```python
x=1+2
print(x)
12
```

如果是两个字符串相加，则会进行拼串操作。

#### （2）减法运算符-

```python
x=5-2
print(x)
12
```

字符串不能进行相减，会报错。

#### （3）乘法运算符*

```python
x=2*2
print(x)
12
```

两个字符串不能进行相乘，但是字符串可以和数字相乘，返回一个被重复若干次数的字符串。

#### （4）除法运算符/

```python
x=6/2
print(x)
12
```

做除法运算时，总会返回一个浮点型数值，且0不能作除数，会报错。

#### （5）整除运算符//

```python
x=7//3
print(x)
12
```

#### （6）取余运算符%

```python
x=7%3
print(x)
12
```

#### （7）幂运算符**

```python
x=2**3
print(x)
12
```

x = a ** 0.5即为对a开平方。

## 二、赋值运算符

概念：等号“=”可以将右侧的值赋值给左侧的变量。
例如：

```python
x=10
print(x)
12
```

**x+=a等价于x=x+a
x-=a等价于x=x-a
x\*=a等价于x=x\*a
x/=a等价于x=x/a
x//=a等价于x=x//a
x%=a等价于x=x%a**
例如：

```python
x=2
x+=3
print(x)
123
x=5
x%=2
print(x)
123
```

## 三、比较运算符

比较运算符是用来比较两个值之间的关系，总会返回一个布尔类型的值，如果关系成立，返回True，关系不成立则返回False。
`r=10>20 print(r)`
会打印出False。
\>运算符：比较左侧值是否大于右侧值；
<运算符：比较左侧值是否小于右侧值；
\>=运算符：比较左侧值是否大于或等于右侧值；
<=运算符：比较左侧值是否小于或等于右侧值;
==运算符：比较两个对象的值是否相等；
!=运算符：比较两个对象的值是否不相等。
例如：

```python
r=2>True
print(r)
12
```

比较字符串：

```python
r='2'>'1'
print(r)
12
```

打印True

```python
r='2'>'11'
print(r)
12
```

打印True

```python
r='a'>'b'
print(r)
12
```

打印False

```python
r='ab'>'b'
proint(r)
12
```

打印False

```python
r='2'>'20'
print(r)
12
```

打印False
字符串比较是逐位比较，若比较出大小关系则返回结果，否则进入字符串下一位比较。
※==只能比较两个对象的值是否相等，不能判断两个对象是否为同一个对象；
要用is来判断两个对象是否为同一个对象，判断的依据是对象的id值。
例如：

```python
r=0 is False
print(r)
12
```

打印False

```python
r=1 is not True
print(r)
12
```

打印True
**内存中存储的是对象的id、类型、值，==比较的是值，is比较的是id。**

## 四、逻辑运算符

主要用来进行逻辑判断。

### 1.逻辑非not

```python
a=True
a=not a
print(a)
123
```

对a进行非运算
not可以对符号右侧的值进行非运算。

```python
a=None
a=not a
print(a)
123
```

None为空值。
对于非布尔值，非运算会先将其先转化为布尔值，再做取反运算。
对于0、None、空字符串和其他一些表示空性的值会转化成False，剩下的转化为True。

### 2.逻辑与and

```python
a=True and True
print(a)
12
```

打印True

```python
a=True and False
print(a)
12
```

打印False

```python
a=False and True
print(a)
12
```

打印False

```python
a=False and False
print(a)
12
```

打印False
and可以对符号两侧的值进行与运算：
**与运算找False**
只有当符号两侧的值均为True时，才返回True；
只要有一个为False，就会返回False；
如果左边的值为True，则看右边，右边的值即为返回值；
如果左边的值为False，则直接返回False
`True and print('Hello')`打印Hello，
`False and print('Hello')`返回False。

### 3.逻辑或or

```python
a=True or True
print(a)
12
```

打印True

```python
a=True or False
print(a)
12
```

打印True

```python
a=False or True
print(a)
12
```

打印True

```python
a=False or False
print(a)
12
```

打印False
逻辑或可以对符号两侧的值进行或运算：
**或运算找True**
如果两个之中其中有一个为True，就会返回True；
两个值均为False，则返回False；
如果左边的值为True，则不看第二个值，直接返回True；
如果左边的值为False，看右边，右边的值即为返回值
`True or print('Hello')`返回True，
`False or print('Hello')`打印Hello。

## 五、非布尔值的与或运算

与运算：
**如果第一个值是False，则返回第一个值，否则返回第二个值。**

```python
r = 1 and 2
print(r)
12
```

打印2
解释：1不为空值，所以为True

```python
r = 0 and 1
print(r)
12
```

打印0
解释：0为空性值，所以返回0

```python
r = None and 0
print(r)
12
```

打印None
解释：None为空性值，所以直接返回None
或运算：
**如果第一个值是True，则返回第一个值，否则返回第二个值。**

```python
r=1 or 2
print(r)
12
```

打印1

```python
r=0 or 1
print(r)
12
```

打印1

```python
r=1<2<3
print(r)
12
```

打印True

## 六、条件运算符

又称三元运算符
语法为：`语句1 if 表达式 else 语句2`
执行过程：条件运算符在执行时，先对条件表达式进行求值判断，如条件表达式为True，则执行语句1并返回执行结果，为False则执行语句2并返回执行结果。
例如：
`print('Python') if True else print('Java)`打印Python

```python
a=10
b=20
print('a is bigger than b') if a > b else print('b is bigger than a')
123
```

打印b is bigger than a

```python
a=30
b=50
m=a if a > b else b
print(m)
1234
```

打印50

```python
a=20
b=50
c=30
m=a if a>b else b
m=m if m>c else c
print(m)
123456
```

打印50

```python
a=20
b=50
c=30
m=a if (a>b and a>c) else (b if b>c else c)
print(m)
12345
```

打印50

## 七、运算符的优先级

```python
a=2 or 3 and 4
print(a)
12
```

打印2
**解释：and的优先级比or的高，所以先算3 and 4得到4，再算2 or 4得到2。**

```python
a = not 4 > 2 and 5 < 6 or 3 < 4
print(a)
12
```

打印True

```python
a = not (4 > 2 and 5 < 6 or 3 < 4)
print(a)
12
```

打印False，分析同上。
关于运算符的优先级的详细情况和运算符的其他知识可参考https://blog.csdn.net/qq_41573234/article/details/81351693
或查看Python官方文档
https://docs.python.org/3/reference/expressions.html#operator-precedence。

# Python基础之5.条件控制语句

## 一、if语句

语法：

```python
if 条件表达式:
    代码块
12
```

代码块用于保存一组代码，同一个代码块要么都执行，要么都不执行，代码块就是一种伪代码分组的机制。
执行流程：if语句在执行时，会先对条件表达式进行判断：
如果为True，则执行if后的语句，
如果为False，则不执行。

```python
if True:
    print('hello')
12
number = 50
if number > 20:
    print('number is bigger than 20')
123
```

默认情况下if指挥控制紧跟其后的一条语句；
如果希望if可以控制多条语句，则可以在if后跟一个代码块。

```python
if True:
    print('hello')
    print('你好')
123
```

可以用**逻辑运算符连接**判断条件。

```python
num = 30
if num > 20 and num < 40:
    print('num is bigger than 20 and less than 40')
123
```

## 二、input()函数

`input()`函数在执行时，程序会暂停、等待用户输入，直到用户输入，才会继续往下执行。

```python
username = input('Please input your username:')
if username == 'admin':
    print('Welcome')
123
```

还可以进行类型转换，如下：

```python
salary = int(input('Please input your salary'))
if salary <= 2000:
    print('You can be better')
123
```

## 三、if-else语句

语法：

```python
if 条件表达式：
    代码块
else:
    代码块
1234
```

执行流程：
if-else语句在执行时，先对if后的条件表达式进行求值判断：
如果为True，则执行if后的代码块，
如果为False，则执行else后的代码块。

```python
salary = int(input('Please input your salary'))
if salary <= 2000:
    print('You can be better')
else:
    print('You are good')
12345
```

## 四、if-elif-else语句

语法：

```python
if 条件表达式：
    代码块
elif 条件表达式：
    代码块
elif 条件表达式：
    代码块
elif 条件表达式：
    代码块
else：
    代码块
12345678910
```

执行流程：
语句在执行时，会自上向下一次对条件表达式进行求值判断：
如果表达式的结果为True，则执行当前代码块，然后语句结束；
如果表达式的结果为False，则继续向下判断，直到找到True为止；
如果所有的表达式都是False，则执行else后的代码块。
语句中只会有一个代码块被执行。

```python
salary = 30000
if salary >= 30000:
    print('Your salary is bigger than 30000')
elif salary >= 20000:
    print('Your salary is bigger than 20000')
elif salary >= 10000:
    print('Your salary is bigger than 10000')
elif salary >= 5000:
    print('Your salary is bigger than 5000')
else:
    print('You can be better')
1234567891011
```

没有else语句时，可能不会有执行结果。

```python
salary = 2000
if salary >= 30000:
    print('Your salary is bigger than 30000')
elif salary >= 20000:
    print('Your salary is bigger than 20000')
elif salary >= 10000:
    print('Your salary is bigger than 10000')
elif salary >= 5000:
    print('Your salary is bigger than 5000')
123456789
```

if-elif-else语句的表达式的数值一般采用从高到低写，而不能从低到高写。
如

```python
salary = 30000
if salary >= 5000:
    print('Your salary is bigger than 5000')
elif salary >= 10000:
    print('Your salary is bigger than 10000')
elif salary >= 20000:
    print('Your salary is bigger than 20000')
else:
    print('Your salary is bigger than 30000')
123456789
```

可能会总是打印`Your salary is bigger than 5000`，与我们要表达的意思不符，成为**死代码**。
判断月份属于哪个季节：

```python
month = 3
if month > 12 or month < 1:
    print('not exists')
elif month >=3 and month <= 5:
    print(month,'Spring')
elif month >= 6 and month <= 8:
    print(month,'Summer')
elif month >= 9 and month <= 11:
    print(month,'Autumn')
else:
    print(month,'Winter')
1234567891011
```

## 五、if语句练习

### 1.判断奇偶数

```python
num = int(input('Please input a number:'))
if num % 2 == 0:
    print(num,'is an even')
else:
    print(num,'is an odd')
12345
```

### 2.检查年份是否为闰年

```python
year = int(input('Please input a year:'))
if (year % 4 == 0 and year % 100 != 0) or year % 400 == 0:
    print('Leap year')
else:
    print('Ordinary year')
12345
```

### 3.狗与人的年龄对应

条件：狗前两年相当于人10.5岁，然后每增加1年就增加4岁。

```python
dog_age = float(input('Please input the age of dog:'))
person_age = 0
if dog_age > 0:
    if dog_age <= 2:
        person_age = dog_age * 10.5
    else:
        person_age = 10.5 * 2
        person_age += （dog_age - 2) * 4
    print('The dog of {} years old is equal to a person of {} years old'.format(dog_age,person_age))
else:
    print('Error')
1234567891011
```

※此段代码使用了条件判断的**嵌套**。

## 六、while语句

循环：是指定的代码块重复执行指定的次数。
while循环：
语法：

```python
while 条件表达式：
    代码块
else:
    代码块
1234
```

执行流程：
语句在执行时，会对while后的条件表达式进行求值判断：
如果判断结果为True，则执行循环体（代码块）；
循环体执行完毕，继续对条件表达式进行求值判断，以此类推；
直到判断结果为False，则循环中止。
例如：

```python
while True:
    print('hello')
12
```

作为测试，执行之后，一直打印Hello，在几秒内打印了近20万行，如图所示。
**此程序为无限循环，会一直打印，如不强行停止，会进入死循环，很消耗内存。**
![此图为运行结果图，在3、4秒内打印出174756行Hello，强行终止才是程序停止运行，也说明了死循环的可怕](https://img-blog.csdnimg.cn/20191024214502446.jpg?#pic_centerx-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

**循环三要素：**
**初始化表达式**：通过初始化来定义变量，如`i = 0`
**条件表达式**：用来循环执行的条件，如

```python
while i < 20:
    print(i)
12
```

**更新表达式**：修改初始化变量的值，如`i += 1`
完整代码为：

```python
i = 0
while i < 20:
    print(i)
    i += 1
1234
```

且while语句可以和else语句结合使用。

```python
i = 0
while i < 20:
    print(i)
    i += 1
else:
print('i is not less than 20')
123456
```

## 七、while语句练习

### 1.求100（包括100）以内所有偶数之和

**方法一：**

```python
i = 0
result = 0
while i < 100:
    i += 1
    if i % 2 == 0:
        result += i
print('The result is {}'.format(result))
1234567
```

**方法二：**

```python
i = 0
result = 0
while i < 100:
    i += 2
    result += i
print('The result is {}'.format(result))
123456
```

### 2.求100以内（包括100）所有9的倍数之和

**方法一：**

```python
i = 0
result = 0
while (i+9) < 100:
    i += 9
    result += i
print('The result is {}'.format(result))
123456
```

**方法二：**

```python
i = 9
result = 0
while i < 100:
    result += i
    i += 9
print('The result is {}'.format(result))
```

# Python基础之6.条件控制语句练习

## 一、循环嵌套简单应用

### 1.求1000以内所有的水仙花数

**水仙花数**：是指一个n位数（n>=3），每个位数上的数字的m次幂之和等于它本身。
如对于153：13 + 53 + 33=153。

```python
#1.获取1000以内所有三位数
i = 100
while i < 1000:
    #假设i的百位数是a，十位数是b,个位数是c。
    a = i // 100
    b = (i // 10) % 10
    #求b第二种方法
    #b = (i - a *100) // 10
    c = i % 10
    #2.判断是否为水仙花数
    if a ** 3 + b ** 3 + c ** 3 == i:
        print(i)
    i += 1
12345678910111213
```

打印：

```python
153
370
371
407

12345
```

### 2.获取用户输入的任意数，判断其是否为质数

**质数**：只能被1和它本身整除的数就是质数，如2、3、5、7、11、13等。

```python
num = int(input('Please input a number which is bigger than 1:'))
# 2.定义变量
i = 2
#创建一个标记用来记录num是否是质数，默认为质数
flag = 1
while i < num:
    #满足下面条件，则num不是质数（逆向思维）
    if num % i == 0:
        #一旦符合此判断条件，则证明num不是质数
        flag = 0
    i += 1

if flag:
    print(num,'is a prime')
else:
    print(num,'is not a prime')
    
1234567891011121314151617
```

## 二、循环嵌套

**循环嵌套**：在一个循环中出现另一个循环的情况叫**循环嵌套**，前者为外层循环，后者为内层循环，内层循环嵌套在外层循环中。

### 1.要求：在控制台输出5行“*”，每行5个。

方法一：

```python
print('*****\n' * 5)
1
```

方法二：

```python
i = 0
while i < 5:
    print('*****')
    i += 1
1234
```

方法三：

```python
#外层循环控制高度（列），内层循环控制宽度（行）
i = 0
while i < 5:
    j = 0
    while j < 5:
        print('*',end='')
        j += 1
    print()
    i += 1
123456789
```

### 2.打印三角

要求：打印出以下图形

```python
*
**
***
****
*****
12345
```

方法一：

```python
i = 0
while i < 5:
    j = 0
    while j < i + 1:
        print('*',end='')
        j += 1
    print()
    i += 1
12345678
```

方法二：

```python
i = 0
while i < 5:
    i += 1
    print('*' * i)
1234
```

### 3.打印倒三角

要求：打印出以下图形

```python
*****
****
***
**
*
12345
```

方法一：

```python
i = 0
while i < 5:
    j = 0
    while j < 5 - i:
        print('*',end='')
        j += 1
    print()
    i += 1
12345678
```

方法二：

```python
i = 0
while i < 5:
    print('*' * (5 - i))
    i += 1
1234
```

上述的示例中即出现了嵌套循环。

### 4.在控制台输出九九乘法表

```python
#创建外层循环控制高度
i = 0
while i < 9:
    i += 1
    #创建内层循环控制图形宽度
    j = 0
    while j < i:
        j += 1
        print(f'{j}×{i}={i*j}',end='\t')
    print()
    
1234567891011
```

打印：
![程序运行产生的九九乘法表](https://img-blog.csdnimg.cn/20191025214009378.jpg#pic_center)

## 三、continue和break

**continue**：
可以用来跳过当次循环，如

```python
i = 0
while i < 6:
    i += 1
    if i == 2:
        continue
    print(i)
else:
    print('hello')
12345678
```

输出

```python
1
3
4
5
6
hello
123456
```

**break**：
用来立即退出循环语句，包括else语句，如

```python
i = 0
while i < 6:
    i += 1
    if i == 2:
        break
    print(i)
else:
    print('hello')
12345678
```

输出

```python
1
1
```

练习：
猜数字游戏，随机1-10个数字，如果猜对则给出正确，如果没有猜对则给出错误，给用户9次机会，最终结果要求用户一次都猜不对，即1、2、3、4、5、6、7、8、9、10共10个数字，9次都猜错，最后给出正确的数字。

思路：
可以先逐个收集用户猜过的数字，收集到9个数字，然后找出1-10中哪一个数不在这9个数中，就是我们需要的数字。

方法一：

```python
import random
#定义一个列表，用来存储用户猜过的数字
lst = []
i = 0
while i < 9:
    number = int(input('Please input a number between 1 and 10:'))
    #把用户猜过的数字添加到列表中
    lst.append(number)
    print('Sorry,you are wrong!!!')
    i += 1

while True:
    #number_x between 1 and 10
    number_x = random.randint(1,10)
    if number_x in lst:
        continue
    else:
        break
print('The right number is',number_x)
12345678910111213141516171819
```

方法二：

```python
import random
#定义一个列表，用来存储用户猜过的数字
lst = []
i = 0
while i < 9:
    number = int(input('Please input a number between 1 and 10:'))
    #把用户猜过的数字添加到列表中
    lst.append(number)
    print('Sorry,you are wrong!!!')
    i += 1

j = 1
while j < 11:
    if j in lst:
        j+=1
        continue
    else:
        break
print('The right number is',j)
12345678910111213141516171819
```

## 三、质数求解的完善与优化

### 1.求100以内所有质数

```python
i = 2
while i <= 100:
    #创建一个标记
    flag = 1
    #判断i是否为质数：先获取所有可能成为i的因数的数
    j = 2
    while j < i:
        #判断i是否能被j整除,如果可以，则i不是质数
        if i % j == 0:
            flag = 0
        j += 1
    if flag:
        print(i,end='\t')
    i += 1
1234567891011121314
```

### 2.在程序中加入时间

对程序运行进行计时：

```python
from time import *
#获取程序开始时间
start = time()
i = 2
while i <= 10000:
    #创建一个标记
    flag = 1
    #判断i是否为质数：先获取所有可能成为i的因数的数
    j = 2
    while j < i:
        #判断i是否能被j整除,如果可以，则i不是质数
        if i % j == 0:
            flag = 0
        j += 1
    if flag:
        print(i,end='\t')
    i += 1
#获取程序结束时间
end = time()
print()
print('The program runs for',end-start,'second')
123456789101112131415161718192021
```

程序运行时间如下图：
![i上限为10000时未优化的运行时间](https://img-blog.csdnimg.cn/20191025220500373.jpg#pic_center)
如将上限改为100000时，运行时间如下图：
![i上限为100000时未优化的运行时间](https://img-blog.csdnimg.cn/20191025220619743.jpg#pic_center)
很显然，运行时间扩大了不止10倍，达到了100倍之多。

### 3.质数第一次优化

```python
from time import *
#获取程序开始时间
start = time()
i = 2
while i <= 10000:
    #创建一个标记
    flag = 1
    #判断i是否为质数：先获取所有有可能成为i的因数的数
    j = 2
    while j < i:
        #判断i是否能被j整除,如果可以，则i不是质数
        if i % j == 0:
            flag = 0
            #一旦进入判断证明i不是质数，内层循环根本没有必要再继续执行
            break
        j += 1
    if flag:
        print(i,end='\t')
    i += 1
#获取程序结束时间
end = time()
print()
print('The program runs for',end-start,'second')
1234567891011121314151617181920212223
```

※在代码行`flag = 0`后加入break，会减少很多不必要的循环次数，从而减少执行时间，提升运行效率。
程序运行时间如下图：
![i上限为10000时进行第一次优化后的运行时间](https://img-blog.csdnimg.cn/20191025220719240.jpg#pic_center)
如将上限改为100000时，运行时间如下图：
![i上限为100000时进行第一次优化后的运行时间](https://img-blog.csdnimg.cn/20191025220826181.jpg#pic_center)
运行时间也扩大了近100倍。

### 4.质数第二次优化

```python
from time import *
#获取程序开始时间
start = time()
i = 2
while i <= 10000:
    #创建一个标记
    flag = 1
    #判断i是否为质数：先获取所有有可能成为i的因数的数
    j = 2
    #没有必要对从2到i的数都进行判断
    while j <= i ** 0.5:
        #判断i是否能被j整除,如果可以，则i不是质数
        if i % j == 0:
            flag = 0
            #一旦进入判断证明i不是质数，内层循环根本没有必要再继续执行
            break
        j += 1
    if flag:
        print(i,end='\t')
    i += 1
#获取程序结束时间
end = time()
print()
print('The program runs for',end-start,'second')
123456789101112131415161718192021222324
```

※对于一个素数，也没有必要从2到它本身进行判断，因为假如它是和数，对于一个逐渐增大的因数，也必然会有一个从这个数本身逐渐减小的数与之对应，它们相乘即为这个数本身，所以只需要判断到这个数的平方根即可，又可很大程度上节省时间、提高效率。
程序运行时间如下图：
![i上限为10000时进行第二次优化后的运行时间](https://img-blog.csdnimg.cn/20191025221028821.jpg#pic_center)
如将上限改为100000时，运行时间如下图：
![i上限为100000时进行第二次优化后的运行时间](https://img-blog.csdnimg.cn/20191025221125823.jpg#pic_center)
运行时间也扩大了不止10倍。
为了更直观地体现优化与否的差异，我将i的上限分别设为10000和100000，并针对未优化、第一次优化、第二次优化进行运行并统计时间，结果如下图，

| 程序类型 | 未优化    | 第一次优化 | 第二次优化 |
| -------- | --------- | ---------- | ---------- |
| 10000    | 7.8387s   | 0.8537s    | 0.0479s    |
| 100000   | 947.2531s | 76.5400    | 0.9218s    |

很明显，进行优化后的程序执行时间**大大缩短、效率大大提高**，因此对于一个好的程序来说，重要的不仅仅是怎么将其写出来，还有更为重要甚至起决定作用的就是程序优化，这是一个逐渐积累的过程，在经历了足够多的代码之后，优化代码的意识和经验都会大大提升。

# Python基础之7.数据结构-列表

## 一、列表的基本操作

### 1.列表的创建

列表：Python中最基本也是最常用的数据结构之一，列表中的每一个元素被分配一个数字作为索引（即下标），用来表示该元素在列表内所排的位置，第1个元素索引是0，第2个索引是1，依次类推。
列表是一个**有序可重复**的集合。
创建列表：

- 方式一：用函数方式

  ```python
  l = list(iterable)
  1
  ```

  **可迭代**：可以用for循环遍历即可迭代；
  参数中传入可迭代的类型如字符串、列表、元组、字典、集合等。
  ※在PyCharm可按CTRL键并将鼠标光标直到函数上查看该函数的Python官方说明。

- 方式二：用方括号[]

  ```python
  l = [1,2,3,4,5]
  print(l)
  12
  ```

### 2.访问列表内的元素

列表从0开始为它的每一个元素顺序创建下标索引，直到总长度减1。要访问它的某一个元素，以方括号加下标值的方式即可。
※注意要确保索引不越界，一旦访问的索引超过范围，会抛出异常，一定要记得最后一个元素的索引是`len(list)-1`.
※并且索引必须为整型，其他类型会报错。
如

```python
lst = [1,2,3]
num = lst[1]
print(num)
123
```

会打印出2。

### 3.修改元素的值

```python
lst[1] = 'hello'
print(lst)
12
```

打印

```python
[1, 'hello', 3, 4, 5]
1
```

※不能赋值给列表中不存在的索引。

### 4.删除值

- 方式一：用`del`

  ```python
  del lst[0]
  1
  ```

- 方式二：根据值进行删除

  ```python
  lst.remove(3)
  1
  ```

  删除值为3的这个元素

- 方式三：根据索引删除

  ```python
  value = lst.pop()
  print(value)
  12
  ```

  该方法有返回值，为删除的元素的值；
  可以传参数，如无参数默认弹出最后一个元素，有删除则弹出指定索引的元素。
  如传参数为：

  ```python
  value = lst.pop(2)
  print(value)
  12
  ```

  即删除第3个元素。

## 二、列表的特殊操作

### 1.列表组合

直接将两个列表相加，是列表的拼接，不是对应位置相加。

```python
l1 = [1,2,3]
l2 = [4,5,6]
l3 = l1 + l2
print(l3)
1234
```

打印：

```python
[1, 2, 3, 4, 5, 6]
1
```

也可用
`l3 = l1.__add__(l2)`,此为魔法方法，为面向对象的方法。

### 2.列表的乘法

```python
l4 = l1 * 3
1
```

相当于

```python
l4 = l1.__mul__(3)
1
```

是将列表l1重复3次，成为一个列表；
两个列表不能直接相乘，否则会报错。

### 3.判断元素是否在列表中

用关键词`in`：

```python
print(2 in l1)
1
```

### 4.迭代列表中的每个元素

```python
for i in l1:
    print(i)
12
```

## 三、列表的常用方法和内置方法

### 1.列表长度

```python
l1 = [1,2,3]
print(len(l1))
12
```

相当于

```python
print(l1.__len__())
1
```

### 2.最大值与最小值

```python
l1 = [1,2,3]
print(max(l1),min(l1))
12
```

## 3.类型转换

```python
lst = list('str')
1
```

属于强制类型转换

```python
print(type(lst))
1
```

※`type()`函数可以查看变量的类型。

### 4.列表排序

#### （1）反转

```python
lst = [1,3,2,4]
lst.reverse()
print(lst)
123
```

打印出

```python
[4, 2, 3, 1]
1
```

很显然，`reverse()`函数对列表进行了改变，不再是原来的列表。
**in place**：修改对象本身。

#### （2）排序

```python
lst.sort()
1
```

为默认**升序**

```python
lst.sort(reverse=True)
1
```

为**降序**
排序只能对元素为同种类型的值进行排序，不能对不同类型的值进行排序，要么全为数值型，要么均为字符串。

### 5.切片

切片即为对序列进行截取，选取序列中的某一段。
语法为：

```python
list[start : end : step]
1
```

step为步长，区间为**左闭右开**。

```python
lis = ['a','b','c','d','e']
print(lis[1:3])
12
```

打印

```python
['b', 'c']
1
```

※省略end取到末尾，省略start从头开始到当前，省略所有复制列表。
※步长为负数时反方向取，即从尾到头取。

### 6.列表的内置方法

#### （1）append()

添加一个元素到末尾

```python
lis = ['a','b','c','d','e']
lis.append('e')
print(lis)
123
```

打印

```python
['a', 'b', 'c', 'd', 'e', 'e']
1
```

如参数为列表，则列表作为一个元素添加到之前的列表末尾。

#### （2）count()

对传入的元素进行计数

```python
lis = ['a','b','c','d','e']
print(lis.count('a'))
12
```

结果为1。

#### （3）extend()

在列表末尾一次性追加另一个序列中的多个值（用新序列扩展原来的列表）

```python
lis = ['a','b','c','d','e']
lis.extend(['f','g'])
print(lis)
123
```

打印出

```python
['a', 'b', 'c', 'd', 'e', 'f', 'g']
1
```

#### （4）insert()

根据索引的位置添加元素

```python
lis = ['a','b','c','d','e']
lis.insert(2,'g')
print(lis)
123
```

打印出

```python
['a', 'b', 'g', 'c', 'd', 'e']
1
```

#### （5）copy()

浅复制

```python
lis = ['a','b','c','d','e']
lst = lis.copy()
print(lst)
123
```

相当于`lst = lis[:]`

#### （6）clear()

清空列表

# Python基础之8.数据结构-元组和字典

## 一、元组的基本使用

### 1.基本定义

序列：计算机中有序地排列数据的一种数据结构。
序列分为**可变**和**不可变**两种。
元组：序列中的一种，即tuple，属于**不可变**序列，没有添加、删改等方法。

**选择列表和元组的原则**：
一般，希望数据不改变时用元组，其他情况都用列表。

### 2.创建元组

用`()`创建元组。

```python
my_tuple = (1,2,3,4,5)
print(my_tuple)
print(type(my_tuple))
123
```

输出

```python
(1, 2, 3, 4, 5)
<class 'tuple'>
12
```

### 3.访问元素

用`tuple[index]`。

```python
my_tuple = (1,2,3,4,5)
print(my_tuple[3])
12
```

输出

```python
4
1
```

给元素赋值会报错，如

```python
my_tuple = (1,2,3,4,5)
my_tuple[3] = 6
print(my_tuple)
123
```

报错`TypeError: 'tuple' object does not support item assignment`。
※如果元组非空，它里面至少有一个元素：

```python
my_tuple = 1,
print(my_tuple)
12
```

打印

```python
(1,)
1
```

可以分别访问元素并赋值，即元组的解包：
**解包**指的是将元组每一个元都赋值给一个变量。

```python
tpl = (1,2,3,4,5)
a,b,c,d,e = tpl
print(f'a={a}\nb={b}\nc={c}\nd={d}\ne={e}')
123
```

输出

```python
a=1
b=2
c=3
d=4
e=5
12345
```

解包的运用：
可运用于变量值的交换，如

```python
a = 1
b = 2
a,b = b,a
print(f'a={a}\nb={b}')
1234
```

打印

```python
a=2
b=1
12
```

在对元素解包时，必须要保证变量的个数与元组的长度一致，否则会报错，如：

```python
tpl = (1,2,3,4,5)
a,b = tpl
print(f'a={a}\nb={b}')
123
```

会报错`ValueError: too many values to unpack (expected 2)`。
※当变量和元组中的元素数量不一致，剩余元素用列表表示，可以在变量前面加一个`*`，这样变量将会获取元组中剩余的元素，并且只能加一个`*`，不能加多个`*`；
※并且列表的位置可以与元组的`任意连续区间`对应。

```python
tpl = (1,2,3,4,5)
a,b,*c = tpl
print(f'a={a}\nb={b}\nc={c}')
123
```

打印

```python
a=1
b=2
c=[3, 4, 5]
123
```

又如，

```python
tpl = (1,2,3,4,5)
*a,b,c = tpl
print(f'a={a}\nb={b}\nc={c}')
123
```

打印

```python
a=[1, 2, 3]
b=4
c=5
123
```

> 本人本科学生，热爱Python、爬虫、数据分析和大数据等，愿与各位读者一起交流，建了一个QQ交流群，群里有一些Python和其他领域的学习资料，也有很多志同道合的朋友，可以点击 [![Python极客部落](https://pub.idqqimg.com/wpa/images/group.png)963624318](https://qm.qq.com/cgi-bin/qm/qr?k=rgE7cwG7OGHgfEucpRIQoSlYCTOEkmEr&jump_from=webapi) 进群获取资料和交流，也可以扫码加群：
> ![Python极客部落群聊二维码](https://img-blog.csdnimg.cn/20200818150858294.png#pic_center)

## 二、两对概念

### 1.可变对象与不可变对象

Python中，不可变类型包括：
int 、float、 字符串str 、元组tuple、bool。
可变类型包括：
列表list、集合set、字典dict。

每个对象在内存中保存了3个数据：
**id（标识）
type（类型）
value（值）**
举例：
列表`a = [1,2,3]`，在内存中的一定区域，会分别保存一个对象的id（假设为0x111）、type（class list）、value（[1，2，3]）。
对序列中的元素进行改变即对值进行改变。

- 改对象：
  如a[0] = 10，则a变为[10,2,3]
  这个操作是通过变量修改对象里面的值，不会改变变量指向的操作。
- 改变量：
  如`a = [4,5,6]`，即a发生了改变，假设id变为0x211，此时为id（0x211）、type（class list）、value（[4，5，6]）
  对变量重新赋值，会**改变变量所指向的对象**。

修改前：

```python
a = [1,2,3]
print(a,id(a))
12
```

打印

```python
[1, 2, 3] 1843535938056
1
```

改对象后：

```python
a[0] = 10
print(a,id(a))
12
```

打印

```python
[10, 2, 3] 1843535938056
1
```

显然，此时a对象**未发生改变**。
改变量后：

```python
a = [4,5,6]
print(a,id(a))
12
```

打印

```python
[4, 5, 6] 1384044909640
1
```

显然，此时a对象**发生改变**。
将a赋给新变量b：

```python
a = [1,2,3]
b = a
b[0] = 10
print('a:',a,id(a))
print('b:',b,id(b))
12345
```

打印

```python
a: [10, 2, 3] 1963325084232
b: [10, 2, 3] 1963325084232
12
```

显示，a和b的值和id完全一致。
解释：
生成a时，a对应的值为id（假设为0x111）、type（class list）、value（[1，2，3]），
将a赋给b时，其实只是**将b指向了a对应的内存对象的内存地址**，并**未分配**新的内存空间给b，a和b共用一个内存地址，所以a和b打印出来的结果一致。

```python
a = [1,2,3]
b = a
b = [10,2,3]
print('a:',a,id(a))
print('b:',b,id(b))
12345
```

打印

```python
a: [1, 2, 3] 2260109775432
b: [10, 2, 3] 2260109775944
12
```

此时b和a不一致，因为b是通过重新赋值得到的新列表，系统会重新分配一个内存地址，所以id会不同。

### 2.==和is

`==`和`!=`比较的是对象的**值是否相等**；
`is`和`is not`比较的是对象的**id是否相等**。

```python
a = [1,2,3]
b = [1,2,3]
print(a == b)
print(a is b)
1234
```

打印

```python
True
False
12
```

!=和is not的用法类似。

## 三、字典的概念和基本使用

### 1.定义

字典即**dict**，是一种新的数据结构，也可以叫映射（mapping）。
字典的作用：
存储对象的容器。

列表存储数据性能很好，但是查询数据性能很差；
字典中每一个元素都有唯一的名字，通过这个名字可以快速查找到指定的元素，这个唯一的名字称为键（key），通过key可以查找到值（value），所以字典也称为键值对（key-value），每个字典可以有多个键值对，每个键值对称为一项。
**字典中键和值的类型**：
值可以是任意对象；
键可以是任意不可变的对象，包括int、str、bool、tuple等。

### 2.创建字典

#### （1）用{}

语法：`{key1:value1,key2:value2,...}`。

```python
d = {'name':'Tom','age':'20','gender':'male'}
print(d,type(d))
12
```

打印

```python
{'name': 'Tom', 'age': '20', 'gender': 'male'} <class 'dict'>
1
```

字典的**键不能重复**，如有重复，则后边的会覆盖前边的。
如

```python
d = {'name':'Tom','age':'20','gender':'male','name':'Jerry'}
print(d,type(d))
12
```

打印

```python
{'name': 'Jerry', 'age': '20', 'gender': 'male'} <class 'dict'>
1
```

即前边的Tom被覆盖。
字典还可以跨行写，如

```python
d = {
    'name':'Tom',
    'age':'20',
    'gender':'male'
    }
print(d,type(d))
123456
```

输出与之前相同。

#### （2）用dict()函数

方式一：

```python
d = dict(name = 'Tom',age=20,gender='male')
print(d)
12
```

打印

```python
{'name': 'Tom', 'age': 20, 'gender': 'male'}
1
```

方式二：

```python
d = dict([('name','Tom'),('age',20)])
print(d,type(d))
12
```

打印

```python
{'name': 'Tom', 'age': 20} <class 'dict'>
1
```

解释：
`dict()`函数可以将一个包双值子序列转化为字典。
双值序列：即序列中只有两个值，如`[3,4]`、`('name','hello)`等。
子序列：如果序列中的元素也是序列，称这个元素为子序列，如`[(1,2)]`即为子序列。

### 3.根据键来获取值

```python
d = {
    'name':'Tom',
    'age':'20',
    'gender':'male'
    }
print(d['name'],d['age'],d['gender'])
123456
```

打印

```python
Tom 20 male
1
```

## 四、字典的常见用法

### 1.len()

获取字典的长度即字典中键值对的个数。

```python
d = dict([('name','Tom'),('age',20)])
print(len(d))
12
```

结果为`2`。

### 2.in、not in

检查字典中是否含有或不含有指定的键。

```python
d = dict([('name','Tom'),('age',20)])
print('hello' in d)
12
```

打印

```python
False
1
```

### 3.获取字典里面的值

语法：`d[key]`。

```python
d = dict([('name','Tom'),('age',20)])
print(d['name'])
12
```

打印

```python
Tom
1
```

如将键赋值给一个变量，则通过变量访问时不需要引号，如

```python
d = {'name':'Tom','age':'20','gender':'male'}
b = 'name'
print(d[b])
123
```

打印

```python
Tom
1
```

如果键不存在会报错，即**KeyError**：

```python
d = {'name':'Tom','age':'20','gender':'male'}
print(d['hello'])
12
```

打印

```python
KeyError: 'hello'
1
```

为避免抛出异常，可以用`get()`方法
语法：`get([key,default])`。
根据键来获取字典中的值，也可以指定一个默认值，作为第二个参数，这样当获取不到键的时候会返回默认值。
如

```python
d = {'name':'Tom','age':'20','gender':'male'}
print(d.get('hello','no such key'))
12
```

打印

```python
no such key
1
```

### 4.修改字典

语法：`d[key] = value`。
如果存在则覆盖，不存在则添加。

```python
d = {'name':'Tom','age':'20','gender':'male'}
d['name'] = 'Jerry'
d['phone'] = '010-11111111'
print(d)
1234
```

打印

```python
{'name': 'Jerry', 'age': '20', 'gender': 'male', 'phone': '010-11111111'}
1
```

### 5.setdefault()函数

可以向字典当中添加key-value：
如果key已经存在于字典中，则返回key对应的值，不会对字典做任何操作；
如果key不存在，则向字典中添加这个key，并设置value。

key存在时：

```python
d = {'name':'Tom','age':'20','gender':'male'}
result = d.setdefault('name','Jerry')
print(d,result)
123
```

打印

```python
{'name': 'Tom', 'age': '20', 'gender': 'male'} Tom
1
```

key不存在时：

```python
d = {'name':'Tom','age':'20','gender':'male'}
result = d.setdefault('phone','010-11111111')
print(d,'\n',result)
123
```

打印

```python
{'name': 'Tom', 'age': '20', 'gender': 'male', 'phone': '010-11111111'} 
 010-11111111
12
```

### 6.update()方法

将其他字典中的key-value添加到当前的字典中

```python
d1 = {'a':1,'b':2,'c':3}
d2 = {'d':4,'e':5,'f':6}
d1.update(d2)
print(d1)
1234
```

打印

```python
{'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5, 'f': 6}
1
```

如存在相同的键，则覆盖：

```python
d1 = {'a':1,'b':2,'c':3}
d2 = {'b':4,'e':5,'f':6}
d1.update(d2)
print(d1)
1234
```

打印

```python
{'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6}
1
```

### 7.删除

- 方式一：`del`
  删除字典中的key和value

  ```python
  d = {'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6}
  del d['a']
  del d['b']
  print(d)
  1234
  ```

  打印

  ```python
  {'c': 3, 'e': 5, 'f': 6}
  1
  ```

- 方式二：`d.popitem()`
  随机删除字典中的一个键值对，一般情况会删除最后一个键值对。
  删除之后会将key-value作为返回值，返回值为元组。

  ```python
  d = {'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6}
  result = d.popitem()
  print('result =',result)
  print(d)
  1234
  ```

  打印

  ```python
  result = ('f', 6)
  {'a': 1, 'b': 4, 'c': 3, 'e': 5}
  12
  ```

- 方式三：`d.pop([,default])`
  根据key删除字典中的键值对，返回的是被删除的value值

  ```python
  d = {'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6}
  result = d.pop('b')
  print('result =',result)
  print(d)
  1234
  ```

  打印

  ```python
  result = 4
  {'a': 1, 'c': 3, 'e': 5, 'f': 6}
  12
  ```

  如被删除的键不存在，则报错。

  ```python
  d = {'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6}
  result = d.pop('z')
  print('result =',result)
  print(d)
  1234
  ```

  抛出异常：`KeyError: 'z'`
  可以通过在pop中添加第二个参数指定默认值，如果指定，当删除不存在的键时，会返回默认值。

  ```python
  d = {'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6}
  result = d.pop('z','no such key')
  print('result =',result)
  print(d)
  1234
  ```

  打印

  ```python
  result = no such key
  {'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6}
  12
  ```

- 方式四：`d.clear()`
  用来清空字典。

  ```python
  d = {'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6}
  d.clear()
  print(d)
  123
  ```

  结果为：`{}`。

### 8.浅复制（浅拷贝）

`copy()`：用于将字典浅复制，即创建已有字典的副本。

```python
d = {'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6}
d2 = d
print(d,id(d))
print(d2,id(d2))
1234
```

打印

```python
{'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6} 1997496449288
{'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6} 1997496449288
12
```

d和d2指向同一个对象。
当d中的值改变时，d2中的值也随之改变。

```python
d = {'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6}
d2 = d
d['a'] = 7
print(d,id(d))
print(d2,id(d2))
12345
```

打印

```python
{'a': 7, 'b': 4, 'c': 3, 'e': 5, 'f': 6} 2121296378120
{'a': 7, 'b': 4, 'c': 3, 'e': 5, 'f': 6} 2121296378120
12
```

用`copy()`方法：

```python
d = {'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6}
d2 = d.copy()
print(d,id(d))
print(d2,id(d2))
1234
```

打印

```python
{'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6} 3102906251528
{'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6} 3102906251608
12
```

d和d2指向不同对象。
当d中的值改变时，d2中的值不会改变。

```python
d = {'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6}
d2 = d.copy()
d['a'] = 7
print(d,id(d))
print(d2,id(d2))
12345
```

打印

```python
{'a': 7, 'b': 4, 'c': 3, 'e': 5, 'f': 6} 2430767003912
{'a': 1, 'b': 4, 'c': 3, 'e': 5, 'f': 6} 2430767003992
12
```

当字典的值为字典时，

```python
d = {'a': {'name':'Tom','age':'20','gender':'male'}, 'b': 4, 'c': 3, 'e': 5, 'f': 6}
d2 = d.copy()
print(d,id(d))
print(d2,id(d2))
1234
```

打印

```python
{'a': {'name': 'Tom', 'age': '20', 'gender': 'male'}, 'b': 4, 'c': 3, 'e': 5, 'f': 6} 2220628113672
{'a': {'name': 'Tom', 'age': '20', 'gender': 'male'}, 'b': 4, 'c': 3, 'e': 5, 'f': 6} 2220628114392
12
```

当d中的值的值改变时，如

```python
d = {'a': {'name':'Tom','age':'20','gender':'male'}, 'b': 4, 'c': 3, 'e': 5, 'f': 6}
d2 = d.copy()
d['a']['name'] = 'Jerry'
print(d,id(d))
print(d2,id(d2))
12345
```

打印

```python
{'a': {'name': 'Jerry', 'age': '20', 'gender': 'male'}, 'b': 4, 'c': 3, 'e': 5, 'f': 6} 2104435669336
{'a': {'name': 'Jerry', 'age': '20', 'gender': 'male'}, 'b': 4, 'c': 3, 'e': 5, 'f': 6} 2104435670056
12
```

**复制**：即创建已有对象的副本，则原对象改变，副本应该不发生改变，这才是正常的复制。
修改d时，d中的’a’下的’name’的值也发生了改变，此即浅复制，只会复制字典本身，字典中还有字典不会被复制。
总结：**浅复制只会复制字典本身，如果字典中还有字典，是不会被复制的**。

## 五、遍历字典

### 1.keys()

该方法会返回字典所有的键。

```python
d = {'name':'Tom','age':'20','gender':'male'}
for key in d.keys():
    print(d[key])
123
```

打印

```python
Tom
20
male
123
```

### 2.values()

返回一个序列，保存字典的值。

```python
d = {'name':'Tom','age':'20','gender':'male'}
for value in d.values():
    print(value)
123
```

打印

```python
Tom
20
male
123
```

### 3.items()

返回字典中所有的项，会返回一个序列，序列中包含**双值子序列**，双值分别为字典中的key和value。

```python
d = {'name':'Tom','age':'20','gender':'male'}
for key,value in d.items():
    print(key,'=',value)
123
```

打印

```python
name = Tom
age = 20
gender = male     
```

# Python基础之9.数据结构-集合与函数

## 一、集合的基本操作

集合，即set，和列表基本一致。
不同点为：
1.集合只能**存储不可变对象**；
2.集合中存储的对象是**无序**的，不是按照元素插入的顺序来保存；
3.集合中不能出现**重复**的元素。

### 1.创建集合

- 方式一：使用`{}`来创建集合

  ```python
  s = {1,2,3,4}
  print(s,type(s))
  12
  ```

  打印

  ```python
  {1, 2, 3, 4} <class 'set'>
  1
  ```

  当集合中插入新数据时

  ```python
  s = {10,1,2,3,4}
  print(s,type(s))
  12
  ```

  打印

  ```python
  {1, 2, 3, 4, 10} <class 'set'>
  1
  ```

  结果不是按照插入的顺序来保存的，即无序。
  当出现重复元素时

  ```python
  s = {10,1,2,3,4,1,1,3,3,4,4,4}
  print(s,type(s))
  12
  ```

  打印

  ```python
  {1, 2, 3, 4, 10} <class 'set'>
  1
  ```

  即会过滤掉重复的元素。
  当集合中出现可变对象时，会报错

  ```python
  s = {[10,1,2,3,4],1,1,3,3,4,4,4}
  print(s,type(s))
  12
  ```

  打印

  ```python
  TypeError: unhashable type: 'list'
  1
  ```

- 方式二：用`set()`函数来创建集合
  创建空的字典用`{}`，但是创建空的集合不能用{}，要用`set()`。
  `set()`函数可以将序列和字典转化成集合。

  ```python
  s = set([10,1,2,3,4,1,1,2,2])
  print(s,type(s))
  12
  ```

  打印

  ```python
  {1, 2, 3, 4, 10} <class 'set'>
  1
  ```

  又如

  ```python
  s = set('Python')
  print(s,type(s))
  12
  ```

  打印

  ```python
  {'y', 'n', 'o', 'P', 'h', 't'} <class 'set'>
  1
  ```

  当参数为字典时，

  ```python
  s = set({'a':'Python','b':'Java'})
  print(s,type(s))
  12
  ```

  打印

  ```python
  {'b', 'a'} <class 'set'>
  1
  ```

  ※使用`set()`函数将字典转化为集合的时候，只会包含字典的键。

### 2.集合的访问

**集合不能通过索引访问**，会报错

```python
s = {1,2,'a','b','3'}
print(s[0])
12
```

打印

```python
TypeError: 'set' object is not subscriptable
1
```

可将集合转化成列表，再访问，如

```python
s = {1,2,'a','b','3'}
s = list(s)
print(s[0])
123
```

打印

```python
1
1
```

### 3.集合的使用

**（1）in和not in**：
用来检查集合中的元素，结果返回一个布尔值

```python
s = {1,2,'a','b','3'}
print('b' in s)
12
```

打印

```python
True
1
```

**（2）len()**：
获取集合中元素的个数（长度）

```python
s = {1,2,'a','b','3'}
print(len(s))
12
```

打印`5`。

-**（3）add()**：
可以向集合中添加元素

```python
s = {1,2,'a','b','3'}
s.add(3)
s.add(4)
print(s)
1234
```

打印

```python
{'a', 1, 2, '3', 3, 4, 'b'}
1
```

**（4）update()**:
将一个集合中的元素添加到当前集合中

```python
s = {1,2,'a','b','3'}
s2 = set('Python')
s.update(s2)
print(s)
1234
```

打印

```python
{'3', 1, 2, 'o', 'b', 't', 'y', 'P', 'n', 'a', 'h'}
1
```

**（5）pop()**:
随机删除集合中的一个元素，返回值为被删除的元素。

```python
s = {1,2,'a','b','3'}
result = s.pop()
print(s)
print(result)
1234
```

打印

```python
{'a', 2, 'b', '3'}
1
12
```

**（6）remove()**:
删除集合中的指定元素

```python
s = {1,2,'a','b','3'}
s.remove('b')
print(s)
123
```

打印

```python
{1, 2, '3', 'a'}
1
```

**（7）clear()**：
清空集合

```python
s = {1,2,'a','b','3'}
s.clear()
print(s)
打印
1234
set()
```

## 二、集合的基本运算

### 1.交集运算&

```python
s = {1,2,3,4,5}
s2 = {3,4,5,6,7}
result = s&s2
print(result)
1234
```

打印

```python
{3, 4, 5}
1
```

### 2.并集运算|

```python
s = {1,2,3,4,5}
s2 = {3,4,5,6,7}
result = s|s2
print(result)
1234
```

打印

```python
{1, 2, 3, 4, 5, 6, 7}
1
```

### 3.差集运算-

```python
s = {1,2,3,4,5}
s2 = {3,4,5,6,7}
result = s-s2
print(result)
1234
```

打印

```python
{1, 2}
1
```

当s和s2交换顺序时

```python
s = {1,2,3,4,5}
s2 = {3,4,5,6,7}
result = s2-s
print(result)
1234
```

打印

```python
{6, 7}
1
```

### 4.异或集^

```python
s = {1,2,3,4,5}
s2 = {3,4,5,6,7}
result = s2^s
print(result)
1234
```

打印

```python
{1, 2, 6, 7}
1
```

### 5.<=

检查一个集合是否是另一个集合的子集，结果返回一个布尔值

```python
a = {1,2,3}
b = {1,2,3,4,5}
result = a <= b
print(result)
1234
```

打印

```python
True
1
```

### 6.<

检查一个集合是否是另一个集合的真子集，结果返回一个布尔值

```python
a = {1,2,3}
b = {1,2,3}
result = a < b
print(result)
1234
```

打印

```python
False
1
```

### 7.>=

检查一个集合是否是另一个集合的超集，结果返回一个布尔值

```python
a = {1,2,3}
b = {1,2,3,4,5}
result = b >= a
print(result)
1234
```

打印

```python
True
1
```

### 8.>

检查一个集合是否是另一个集合的真超集，结果返回一个布尔值

```python
a = {1,2,3}
b = {1,2,3}
result = a > b
print(result)
1234
```

打印

```python
False
1
```

## 三、函数的介绍和基本使用

### 1.定义

表现形式：function
对象是内存中专门用来存储数据的一块区域。
函数也是一个**对象**。

函数出现的原因：
便于程序维护；
便于共享使用。

函数可以用来**保存一些可执行的代码**，并且在需要的时候对这些语句进行多次调用。
函数只有被调用才会被使用。

### 2.创建函数

语法：

```python
def 函数名([形参1,形参2,形参2,...]):
    代码块
12
```

例如：

```python
def fn():
    print('First Function')
print(fn)
123
```

打印

```python
<function fn at 0x000002292FF23F78>
1
```

即为函数保存的内存地址。
被调用时，

```python
def fn():
    print('First Function')
fn()
123
```

打印

```python
First Function
1
```

上例中`fn`是函数对象，`fn()`调用函数。

### 3.函数的参数

定义函数时，可以在后边的括号里定义数量不定的形参，形参之间用逗号隔开。
形参，即形式参数，相当于在函数内部定义了变量，但是并没有赋值。
实参，即实际参数，函数定义时指定了形参，那么调用函数的时候也必须传递实参，实参将会赋值给对应的形参，有几个形参就传几个实参。

```python
def sum(a,b):
    print('a =',a)
    print('b =',b)
sum(11,23)
1234
```

打印

```python
a = 11
b = 23
12
```

又如

```python
def sum(a,b):
    print('a + b =',a+b)
sum(11,23)
123
```

打印

```python
a + b = 34
1
```

## 四、函数参数的使用

### 1.参数默认值

定义形参时，可以为形参指定一个默认值：
指定了默认值以后，如果用户传递了参数，默认值不会起作用；
如果用户没有传递，则默认值生效。
指定默认值时，

```python
def fn(a,b,c=10):
    print('a =',a)
    print('b =',b)
    print('c =',c)
fn(1,2,3)
12345
```

打印

```python
a = 1
b = 2
c = 3
123
```

c不为默认值10，而是指定的值3。
未指定默认值时，

```python
def fn(a,b,c=10):
    print('a =',a)
    print('b =',b)
    print('c =',c)
fn(1,2)
12345
```

打印

```python
a = 1
b = 2
c = 10
123
```

c为默认值10。

### 2.位置传参和关键字传参

**位置传参**就是将对应位置的实参传递给对应位置的形参。
也可以不按照形参定义的顺序去传参，而根据参数名来传递参数，即**关键字传参**。

关键字传参练习如下：

```python
def fn(a,b,c=10):
    print('a =',a)
    print('b =',b)
    print('c =',c)
fn(b=2,a=3,c=5)
12345
```

打印

```python
a = 3
b = 2
c = 5
123
```

关键字传参可以和位置传参混合使用：

```python
def fn(a,b,c=10):
    print('a =',a)
    print('b =',b)
    print('c =',c)
fn(2,1,c=5)
12345
```

打印

```python
a = 2
b = 1
c = 5
123
```

混合使用时，**位置参数必须位于关键字参数前面**，并且位置参数个数要与非默认值的参数个数一致，否则会报错。

### 3.实参的类型

函数在调用的时候，解释器不会检查实参的类型，实参类型可以是任意的对象。
如

```python
def fn(a,b,c=10):
    print('a =',a)
    print('b =',b)
    print('c =',c)

def fn2(a):
    print('a =',a)
fn2(fn)
打印
123456789
a = <function fn at 0x000002323DD53EE8>
1
```

又如

```python
def fn(a,b):
    print(a+b)
fn('123',456)
123
```

打印

```python
TypeError: can only concatenate str (not "int") to str
1
```

参数类型不同，报错
再如

```python
def fn(a):
    a = 30
    print('a =',a)
c = 10
fn(c)
print('c =',c)
123456
```

打印

```python
a = 30
c = 10
12
```

易知，在函数中对形参进行赋值不会影响其他的变量。

```python
def fn(a):
    a[0] = 5
    print('a = ',a)
c = [1,2,3]
fn(c)
print('c =',c)
123456
```

打印

```python
a =  [5, 2, 3]
c = [5, 2, 3]
12
```

解释：形参在执行时，通过形参去修改对象，会影响到指向该对象的变量。
如不希望改a影响到c，可以用`copy()`方法。
即

```python
def fn(a):
    a[0] = 5
    print('a = ',a)
c = [1,2,3]
fn(c.copy())
print('c =',c)
123456
```

打印

```python
a =  [5, 2, 3]
c = [1, 2, 3]
12
```

还有一种方法

```python
def fn(a):
    a[0] = 5
    print('a = ',a)
c = [1,2,3]
fn(c[:])
print('c =',c)
123456
```

输出结果相同。
**总结**：传递的是可变对象时，但又不希望函数内部的操作影响到函数外部，可以传递参数的副本，不可变对象不用管。

## 五、不定长参数和参数解包

### 1.不定长参数

在定义函数时，可以在形参前面加一个`*`，这样这个形参将会获取到所有的实参值，并将所有形参保存到一个元组中。

```python
def sum(*a):
    print(a,type(a))
sum()
123
```

打印

```python
() <class 'tuple'>
1
```

有多个参数时，

```python
def sum(*a):
    print(a,type(a))
sum(1,2,3,4,5,6)
123
```

打印

```python
(1, 2, 3, 4, 5, 6) <class 'tuple'>
1
```

即参数的打包过程，将所有参数保存到一个元组中。

```python
def sum(*a):
    r = 0
    for n in a:
        r += n
    print(r)
sum(1,2,3,4,5,6)
123456
```

即实现求多个数的和，结果为21。

```python
def fn(a,b,*c):
    print('a =',a)
    print('b =',b)
    print('c =',c)
fn(1,2,3,4,5,6)
12345
```

打印

```python
a = 1
b = 2
c = (3, 4, 5, 6)
123
```

可变长参数不是必须位于最后，但是带*的参数后的参数必须以关键字的形式传参。

```python
def fn(*a,b,c):
    print('a =',a)
    print('b =',b)
    print('c =',c)
fn(1,2,3,4,b=5,c=6)
12345
```

打印

```python
a = (1, 2, 3, 4)
b = 5
c = 6
123
```

如果在形参的开头直接为*，则要求所有的参数要以关键字形式传递。

```python
def fn(*,a,b,c):
    print('a =',a)
    print('b =',b)
    print('c =',c)
fn(a=4,b=5,c=6)
12345
```

打印

```python
a = 4
b = 5
c = 6
123
```

形参可以接受其他的关键字参数，它会将这些参数统一保存到一个字典中，字典的key是参数名，value是参数的值，形参只能有一个，并且必须位于最后面。

```python
def fn(a,b,**c):
    print('a =',a)
    print('b =',b)
    print('c =',c)
fn(a=4,b=5,d=1,e=2,f=3)
12345
```

打印

```python
a = 4
b = 5
c = {'d': 1, 'e': 2, 'f': 3}
123
```

### 2.参数解包

参数解包是将序列中的参数化为单个参数传入，要求序列中的元素个数必须和形参一致。

```python
def fn(a,b,c):
    print('a =',a)
    print('b =',b)
    print('c =',c)
t = (1,2,3)
fn(*t)
123456
```

打印

```python
a = 1
b = 2
c = 3
123
```

并且可传入字典解包

```python
def fn(a,b,c):
    print('a =',a)
    print('b =',b)
    print('c =',c)
d = {'a':1,'b':2,'c':3}
fn(**d)
123456
```

打印

```python
a = 1
b = 2
c = 3
```

# Python基础之10.函数

## 一、函数的返回值

上一节，求任意数的和代码如下：

```python
def fn(*nums):
    #定义变量保存结果
    result = 0
    #便利元组，将元组中的元素累加
    for n in nums:
        result += n
    print(result)

fn(1,2,3,4)
123456789
```

但是有时候并不需要对结果进行打印，而是进行一些其他的处理，这时候就需要**返回值**。
返回值就是函数返回的结果。
返回值可以直接使用，也可以通过一个变量来接收函数返回值的结果。
如

```python
def fn():
    return 100
r= fn()
print(r)
1234
```

打印`100`。
return后面可以跟任意的对象，甚至是一个函数。
如

```python
def fn():
    def fn2():
        print('Python')
    return fn2
r= fn()
print(r)
123456
```

打印`<function fn.<locals>.fn2 at 0x0000022AF4782EE8>`，即返回了一个函数。
又如

```python
def fn():
    def fn2():
        print('Python')
    return fn2

r= fn()
r()
print(r)
12345678
```

打印
`Python <function fn.<locals>.fn2 at 0x000001ECEE8A2EE8>`
如果仅仅写一个return（后边不加其他值）或者不写return，相当于`return None`。
如

```python
def fn2():
    return
r = fn2()
print(r)
1234
```

打印`None`。

在函数中，return后面的代码都不会执行，return一旦执行函数自动结束。

```python
def fn3():
    print('Hello')
    return
    print('ABC')
fn3()
12345
```

打印`Hello`，没有打印出ABC。
`return`和`break`的比较：

```python
def fn4():
    for i in range(5):
        if i == 3:
        #break用于退出当前循环
            break
        print(i)
    print('Loop ends')
fn4()
12345678
```

打印

```python
0
1
2
Loop ends
1234
def fn4():
    for i in range(5):
        if i == 3:
        #return用来结束函数
            return
        print(i)
    print('Loop ends')
fn4()
12345678
```

打印

```python
0
1
2
123
```

没有打印出`Loop ends`。

计算求和返回结果并运用：

```python
def fn(*nums):
    #定义变量保存结果
    result = 0
    #便利元组，将元组中的元素累加
    for n in nums:
        result += n
    return result

r = fn(1,2,3,4)
print(r+6)
12345678910
```

打印`16`

```python
def fn5():
    return 10
print(fn5)
print(fn5())
1234
```

打印出

```python
<function fn5 at 0x000002997A442F78>
10
12
```

`print(fn5)`是函数对象，实际上是在打印函数对象；
`print(fn5())`是在调用函数，实际上是在打印`fn5()`函数的返回值。

> 本人本科学生，热爱Python、爬虫、数据分析和大数据等，愿与各位读者一起交流，建了一个QQ交流群，群里有一些Python和其他领域的学习资料，也有很多志同道合的朋友，可以点击 [![Python极客部落](https://pub.idqqimg.com/wpa/images/group.png)963624318](https://qm.qq.com/cgi-bin/qm/qr?k=rgE7cwG7OGHgfEucpRIQoSlYCTOEkmEr&jump_from=webapi) 进群获取资料和交流，也可以扫码加群：
> ![Python极客部落群聊二维码](https://img-blog.csdnimg.cn/20200818150858294.png#pic_center)

## 二、文档字符串、函数的作用域和命名空间

### 1.文档字符串

`help()函数`：用于查询Python函数的用法，一般是在定义函数时注释中编写。

语法：`help(函数对象)`**（函数对象不加括号）**

```python
help(print)
1
```

打印

```python
Help on built-in function print in module builtins:

print(...)
    print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)
    
    Prints the values to a stream, or to sys.stdout by default.
    Optional keyword arguments:
    file:  a file-like object (stream); defaults to the current sys.stdout.
    sep:   string inserted between values, default a space.
    end:   string appended after the last value, default a newline.
    flush: whether to forcibly flush the stream.
1234567891011
```

文档字符串示例：

```python
def fn(a:int,b:str,c:bool) -> int:
    '''
    the parameters of the function:
    :param a: a is used to ...
    :param b: b is used to ...
    :param c: c is used to ...
    :return:
    '''
    return 10

help(fn)
1234567891011
```

打印

```python
Help on function fn in module __main__:

fn(a: int, b: str, c: bool) -> int
    the parameters of the function:
    :param a: a is used to ...
    :param b: b is used to ...
    :param c: c is used to ...
    :return:
12345678
```

### 2.函数的作用域

作用域就是指变量生效的区域。

```python
a = 20
def fn():
    #a定义在函数内部，它的作用域就是在函数内部，函数外部是无法访问的。
    a = 10
    print('Inside the function,a = ',a)
fn()
print('Outside the function,a = ',a)
1234567
```

打印

```python
Inside the function,a =  10
Outside the function,a =  20
12
```

又如

```python
b = 20
def fn():
    #a定义在函数内部，它的作用域就是在函数内部，函数外部是无法访问的。
    a = 10
    print('Inside the function,a = ',a)
    print('Inside the function,b = ', b)
fn()
print('Outside the function,b = ',b)
12345678
```

打印

```python
Inside the function,a =  10
Inside the function,b =  20
Outside the function,b =  20
123
```

在Python中有两种作用域：
**全局作用域**：
在程序执行时创建，在程序结束时销毁，所有函数以外的区域都是全局作用域。
在全局作用域中定义的变量都属于全局变量，全局变量可以在程序中的任何位置访问。
**函数作用域**：
在函数调用时创建，在调用结束时销毁。
函数每调用一次就会产生一个新函数的作用域。
在函数作用域中定义的变量都是局部变量，它只能在函数内部被访问。

在函数内部定义的局部变量可以访问函数外部的变量的值，如有多个同名变量，则访问最近的变量，函数外部的变量不能访问函数内部的变量的值。

```python
def fn2():
    a = 30
    def fn3():
        print('Inside fn3(),a =',a)
    fn3()
fn2()
123456
```

打印`Inside fn3(),a = 30`
再如

```python
def fn2():
    a = 30
    def fn3():
        a = 40
        print('Inside fn3(),a =',a)
    fn3()
fn2()
1234567
```

打印`Inside fn3(),a = 40`
再如

```python
a = 20
def fn2():
    def fn3():
        a = 10
        print('Inside fn3(),a =',a)
    fn3()
fn2()
1234567
```

打印`Inside fn3(),a = 10`

```python
a = 20
def fn2():
    a = 10
    print('Inside fn2(),a =',a)
fn2()
print('Outside fn2(),a =',a)
123456
```

打印

```python
Inside fn2(),a = 10
Outside fn2(),a = 20
12
```

如果需要在函数内部修改全局变量，则需要使用`global`关键字，来声明变量。

```python
a = 20
def fn2():
    #声明函数内部使用a是全局变量，此时再修改a时，就是修改全局变量。
    global a
    a = 10
    print('Inside fn2(),a =',a)
fn2()
print('Outside fn2(),a =',a)
12345678
```

打印

```python
Inside fn2(),a = 10
Outside fn2(),a = 10
12
```

### 3.命名空间

**命名空间**实际上就是一个**字典**，专门用来存储变量的字典。
函数`locals()`用来获取当前作用域的命名空间，返回的是一个字典，把所有的变量存到该字典中。
如果在全局作用域中，调用`locals()`则获取全局作用域命名空间，如果在函数作用域中调用`locals()`则获取函数命名空间。

```python
s = locals()
print(s)
12
```

打印

```python
{'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x0000016F7E71CB88>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, '__file__': 'Demo.py', '__cached__': None, 's': {...}}
1
```

又如

```python
a = 20
s = locals()
print(s)
print(s['a'])
1234
```

打印

```python
{'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x0000024D0E2DCB88>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, '__file__': 'Demo.py', '__cached__': None, 'a': 20, 's': {...}}
20
12
```

向字典中添加一个键值对，即增加一个变量。

```python
a = 20
s = locals()
print(s)
s['c'] = 200
print(s)
12345
```

打印

```python
{'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x0000022917BECB88>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, '__file__': 'Demo.py', '__cached__': None, 'a': 20, 's': {...}}
{'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x0000022917BECB88>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, '__file__': 'Demo.py', '__cached__': None, 'a': 20, 's': {...}, 'c': 200}
12
```

在函数中定义变量：

```python
def fn4():
    a = 10
    s = locals()
    print(s)
fn4()
12345
```

打印`{'a': 10}`

```python
def fn4():
    a = 10
    s = locals()
    #可以通过操作s来操作函数的命名空间，但是不建议这么做！！
    s['b'] = 20
    print(s)
fn4()
1234567
```

打印`{'a': 10, 'b': 20}`

## 三、递归

### 1.递归基本概念和使用

练习：尝试求出10的阶乘`10!`。
用循环：

```python
result = 1
for i in range(1,11):
    result *= i
print(result)
1234
```

打印`3628800`。
要求：创建一个函数求任意数的阶乘：

```python
def factorial(n):
    result = 1
    for i in range(1,n+1):
        result *= i
    return result
print(factorial(10))
123456
```

打印`3628800`。

递归：
即递归式函数；
递归简单说就是**自己调用自己**，递归式函数就是在函数中调用自己。
递归时解决问题的一种方式，思想是将大的问题拆分成小的问题，直到不能再拆分。
递归式函数的两个条件：
1.**基线条件**：
问题可以分解成最小的问题，当满足基线条件时，递归不再执行。
2.**递归条件**：
将问题继续分解的条件。

```python
def recursion(n):
    #基线条件
    if n == 1:
        return 1
    #递归条件
    return n * recursion(n - 1)
print(recursion(10))
1234567
```

打印`3628800`

### 2.递归练习递归练习

#### 创建一个函数，来为任意数作任意次幂运算

```python
def exponentiation(n,i):
    #n为底数，i为指数
    #基线条件，指数i为1
    if i == 1:
        return n
    #递归条件
    return n * exponentiation(n,i - 1)
print(exponentiation(2,4))
12345678
```

打印`16`。

当然，还有更简单的方法

```python
def exponentiation(n,i):
    return n ** i
print(exponentiation(2,4))
123
```

即运用Python自带的幂运算符。

#### 创建一个函数，用来检查任意的字符串是否为回文字符串，如果是返回True，不是，返回False

**回文字符串**：即字符串从前往后看和从后往前看是一样的，如123321、abcba。
方法：先检查第一个字符和最后一个字符是否一致，如果不一致不是回文字符串，如果一致，则看剩余部分是不是回文串。

```python
def palindrome(s):
    #基线条件，如果字符串长度小于2，则这个字符串是回文串
    if len(s) < 2:
        return True
    elif s[0] != s[-1]:
        return False
    #递归条件
    return palindrome(s[1:-1])
print(palindrome('Python'))
123456789
```

打印`False`。

## 四、高级函数

**特点**：
1.接受一个或多个函数作为参数；
2.将函数作为返回值。
满足任意一个特点即为高级函数。

练习：定义一个函数，将指定列表中的所有偶数，保存到新的列表中返回。

```python
l = [1,2,3,4,5,6,7,8,9,10]
def evenlist(lst):
    new_list = []
    for item in lst:
        #判断item的奇偶
        if item % 2 == 0:
            new_list.append(item)
    return new_list
print(evenlist(l))
123456789
```

打印`[2, 4, 6, 8, 10]`

用函数判断奇偶时：

```python
l = [1,2,3,4,5,6,7,8,9,10]
def evenlist(lst):
    def even(i):
        if i % 2 == 0:
            return True
    new_list = []
    for item in lst:
        #判断item的奇偶
        if even(item):
            new_list.append(item)
    return new_list
print(evenlist(l))
123456789101112
```

结果相同。

可在外函数中同时添加多个内部函数，但是这不灵活的，需要将每个函数都写出来。
当使用函数作为参数时，实际上是将**相应函数的代码**传递进了目标函数。

```python
l = [1,2,3,4,5,6,7,8,9,10]
def even(i):
    if i % 2 == 0:
        return True
def tri(i):
    if i % 3 == 0:
        return True
def evenlist(func,lst):
    new_list = []
    for item in lst:
        #判断item的奇偶
        if func(item):
            new_list.append(item)
    return new_list
print(evenlist(even,l))
print(evenlist(tri,l))
12345678910111213141516
```

即为高级函数，打印

```python
[2, 4, 6, 8, 10]
[3, 6, 9]
```

# Python基础之11.函数（2）

## 一、匿名函数

### 1.filter()函数

内置函数`filter()`，参数中传入可迭代的结构，即`filter(function,iterable)`，可以从序列中过滤出符合条件的元素，保存到一个新的序列中。
参数function：传递实现过滤功能的函数；
参数iterable：需要过滤的序列。

返回值：过滤后的新序列。

```python
l = [1,2,3,4,5,6,7,8,9,10]
def tri(n):
    if n % 3 == 0:
        return True
print(list(filter(tri,l)))
12345
```

打印出`[3, 6, 9]`。

### 2.匿名函数：

又称lambda表达式。
lambda函数表达式专门用来创建一些简单的函数，是创建函数的另外一种形式。
语法：`lambda 参数列表:返回值`。
如

```python
result = lambda a,b:a+b
print(result)
12
```

打印

```python
<function <lambda> at 0x0000024C52FF2EE8>
1
```

可见，函数没有名字，所以叫匿名函数。
传入参数

```python
result = lambda a,b:a+b
print(result(10,20))
12
```

打印`30`，这与

```python
def fn(a,b):
    return a + b
print(fn(10,20))
123
```

效果是一样的，但是更简洁。

```python
l = [1,2,3,4,5,6,7,8,9,10]
tri = lambda i:i%3==0
r = filter(tri,l)
print(list(r))
1234
```

与上面`tri()`函数的效果是一致的。
lambda匿名函数俗称**语法糖**，可以减少代码量和优化程序，减少内存消耗。

### 2.map()函数：

可以对可迭代对象中的所有元素做指定操作，然后将其添加到一个新的可迭代对象并返回。
匿名函数一般都是作为参数使用，其他地方一般不用。

```python
l = [1,2,3,4,5,6,7,8,9,10]
r = map(lambda i:i+1,l)
print(list(r))
123
```

打印`[2, 3, 4, 5, 6, 7, 8, 9, 10, 11]`

### 3.sort()方法：

该方法用来对列表中的元素进行排序，直接默认比较列表中元素的大小。
在该方法中可以接受一个关键字参数，即一个函数作为参数。

```python
l = [34,554,67,897,436]
l.sort()
print(l)
123
```

打印`[34, 67, 436, 554, 897]`
同时可传递参数来对字符串长度进行比较。

```python
l = ['ssd','adsd','frfrrffr','frfrsfrfs','rd']
l.sort(key=len)
print(l)
123
```

打印`['rd', 'ssd', 'adsd', 'frfrrffr', 'frfrsfrfs']`。
还可以根据类型转换后的值进行比较：

```python
l = ['34',554,'67',897,436]
l.sort(key=int)
print(l)
123
```

打印`['34', '67', 436, 554, 897]`

### 4.sorted()函数：

返回一个新的列表。

```python
l = ['34',554,'67',897,436]
l2 = sorted(l,key=int)
print(l)
print(l2)
1234
```

打印

```python
['34', 554, '67', 897, 436]
['34', '67', 436, 554, 897]
12
```

## 二、闭包

闭包将函数作为返回值返回，也是一种高级函数。

```python
def fn():
    #函数内部再定义一个函数
    def inner():
        print('In fn2()')
    #将inner()函数作为返回值返回
    return inner
print(fn())
1234567
```

打印`<function fn.<locals>.inner at 0x000001E69ABC2EE8>`

```python
def fn():
    #函数内部再定义一个函数
    def inner():
        print('In fn2()')
    #将inner()函数作为返回值返回
    return inner
r = fn()
r()
12345678
```

打印`In fn2()`。
解释：r其实就是`fn()`返回的函数inner对象，再调用，就会打印，所以这个函数总是能返回`fn()`函数内部的变量，如下：

```python
def fn():
    a  = 10
    #函数内部再定义一个函数
    def inner():
        print('In fn2()')
        print(a)
    #将inner()函数作为返回值返回
    return inner
r = fn()
r()
12345678910
```

打印

```python
In fn2()
10
12
```

r是调用`fn()`后返回的函数，是在`fn()`内部定义的，并不是全局函数，所以这个函数总是能访问到`fn()`函数内部的变量。
闭包的好处：
通过闭包可以创建一些只有当前函数可以访问到的对象（可以将一些私有的数据藏到闭包中）。
应用：

```python
nums = []
def average(n):
    nums.append(n)
    return sum(nums)/len(nums)
print(average(10))
print(average(20))
print(average(30))
1234567
```

打印

```python
10.0
15.0
20.0
123
```

不好的地方：nums是全局变量，会被任意访问到，可能会出现被修改、覆盖，从而导致本身的nums改变导致错误。
所以形成闭包：

```python
def op_avg():
    nums = []
    def average(n):
        nums.append(n)
        return sum(nums)/len(nums)
    return average
avg = op_avg()
print(avg(10))
print(avg(20))
print(avg(30))
12345678910
```

结果与上面相同。
总结：
**形成闭包的条件**：
1.函数嵌套；
2.将内部函数作为返回值返回；
3.内部函数必须使用外部函数的变量。
**闭包可以确保数据的安全。**

## 三、装饰器

### 1.装饰器的出现背景

```python
def add(a,b):
    #求任意两个数的和
    print('Start...')
    r = a + b
    print('End...')
    return r

def multiply(a,b):
    # 求任意两个数的积
    print('Start...')
    r = a * b
    print('End...')
    return r

r = add(1,2)
print(r)
12345678910111213141516
```

打印

```python
Start...
End...
3
123
```

这是添加了提示开始和结束的输出，但是如果函数较多，会出现一些问题：
※如果要修改的函数很多，修改起来很麻烦；
※不方便后期维护；
※会违反开闭原则（OCP）。
**开闭原则**：在开发的时候，要求可以开发对程序的扩展，但是要关闭对程序的修改。

```python
def fn():
    print('In fn()')

def fn2():
    print('Start...')
    fn()
    print('End...')

fn2()
123456789
```

打印

```python
Start...
In fn()
End...
123
```

又如

```python
def add(a,b):
    #求任意两个数的和
    r = a + b
    return r

def new_add(a,b):
    print('Start...')
    r = add(a,b)
    print('End...')
    return r

r = new_add(1,2)
print(r)
12345678910111213
```

打印

```python
Start...
End...
3
123
```

很显然，会出现一定问题，每扩展一个函数会定义一个新的函数，很麻烦。

### 2.装饰器的使用

```python
def start_end():
    #用来对其他函数进行扩展，使其他函数可以在执行前打印开始和结束
    #创建一个函数
    def new_func():
        pass
    #返回函数
    return new_func
f = start_end()
f2 = start_end()
print(f)
print(f2)
1234567891011
```

打印

```python
<function start_end.<locals>.new_func at 0x0000018368DE2EE8>
<function start_end.<locals>.new_func at 0x0000018368DFB1F8>
12
```

扩展new_func后：

```python
def fn():
    print('In fn()')

def start_end():
    # 用来对其他函数进行扩展，使其他函数可以在执行前打印开始和结束
    # 创建一个函数
    def new_func():
        print('Start...')
        fn()
        print('End...')
    #返回函数
    return new_func

f = start_end()
f()
123456789101112131415
```

打印

```python
Start...
In fn()
End...
123
```

但是new_func是固定的、不能改变，失去了意义，所以可以在函数参数中传入被扩展的函数。

```python
def fn():
    print('In fn()')

def start_end(func):
    #用来对其他函数进行扩展，使其他函数可以在执行前打印开始和结束
    #创建一个函数
    def new_func():
        print('Start...')
        func()
        print('End...')
    #返回函数
    return new_func

f = start_end(fn)
f()
123456789101112131415
```

打印

```python
Start...
In fn()
End...
123
```

当被扩展的函数有参数时，

```python
def add(a,b):
    #求任意两个数的和
    r = a + b
    return r

def start_end(func):
    #用来对其他函数进行扩展，使其他函数可以在执行前打印开始和结束
    #创建一个函数
    def new_func(a,b):
        print('Start...')
        result = func(a,b)
        print('End...')
        return result
    #返回函数
    return new_func

f = start_end(add)
r = f(1,2)
print(r)
12345678910111213141516171819
```

打印

```python
Start...
End...
3
123
```

需要对任何函数进行扩展时，

```python
def add(a,b):
    #求任意两个数的和
    r = a + b
    return r

def fn():
    print('In fn()')

def start_end(func):
    #用来对其他函数进行扩展，使其他函数可以在执行前打印开始和结束
    #创建一个函数
    def new_func(*args,**kwargs): #*args接收所有位置参数，**kwargs接收所有关键字参数
        print('Start...')
        result = func(*args,**kwargs)
        print('End...')
        return result
    #返回函数
    return new_func
f = start_end(add)
f2 = start_end(fn)
r = f(1,2)
print(r)
f2()
1234567891011121314151617181920212223
```

打印

```python
Start...
End...
3
Start...
In fn()
End... 
123456
```

总结：像上面的`start_end(func)`这类的函数即称之为装饰器。
通过装饰器可以在不修改原来函数的基础之上来对函数进行扩展，在开发过程中，都是通过对装饰器来扩展函数的功能。

# Python基础之12.面向对象

## 一、面向对象简介

### 1.面向对象基本概念

面向对象**OOP**，即Object Oriented Programming。
什么是对象：
对象就是**内存**中存储指定数据的一块区域。
实际上对象就是一个容器，专门用来存数据。
程序运行的通俗解释：
代码存在硬盘，CPU处理代码，CPU和硬盘之间有内存，解释器将代码交给内存，CPU再从内存读取。

### 2.面向对象的结构

#### id（标识）

用来标识对象的唯一性，每个对象都有唯一的id，每个id指向一个内存地址值。
id由解释器生成，其实就是对象的内存地址。

#### type（类型）

类型决定了对象有哪些功能。
可以通过`type()`函数来查看对象的类型。

#### value（值）

值就是对象中存储的具体数据，分为：

- 可变对象：
  **列表list、集合set、字典dict**。
- 不可变对象：
  **数值类型int、float，字符串str，元组tuple，布尔型bool**。

### 3.从面向过程到面向对象

所谓**面向对象**，简单理解就是语言中所有的操作都是通过对象来进行的。
**面向过程 → 面向对象**：
面向过程的典型代表是C语言，是早期语言的特点，是将一件事分成一个个步骤，并用函数分别实现。

面向对象是一种思考问题的方式，是一种思想，将事物变得简单化。

对**面向对象**的理解：
（1）可以让复杂事务变得简单化，人由执行者过渡到指挥者；
（2）具备封装、继承和多态。

举例——孩子吃瓜：
（1）妈妈穿衣服穿鞋出门
（2）妈妈骑电动车
（3）妈妈到超市门口停好电动车
（4）妈妈买瓜
（5）妈妈结账
（6）妈妈骑电动车回家
（7）到家孩子吃瓜
这是一个典型的面向过程的例子，在现实中没问题，但是在程序中会出现问题，在复用、修改时会出现很多问题。

面向过程的思想：
将一个功能分解成一个一个的小步骤。

- 优点
  比较符合人的思维，编写起来比较容易；
- 缺点
  这种编程方式只适用于一个功能，我们要实现别的功能的时候，往往需要编写新的代码，复用性较低。

面向对象就是**让孩子吃瓜**，而不管具体怎么实现，细节在对象内部。

- 优点
  容易维护、方便复用；
- 缺点
  不符合人的思维，编写时比较麻烦。

## 二、类(class)的简介

目前所用到的对象都是Python内置的对象，如int、float、str等。
类简单理解就是一张图纸，在程序中我们需要根据类来创建对象。

```python
a = int(10)
print(a)
12
```

打印`10`。

定义一个类：
自己定义类时，类名要以大写开头。
语法：

```python
class 类名([父类])：
    代码块
12
```

如

```python
class MyClass():
    pass
print(MyClass)
123
```

打印`<class '__main__.MyClass'>`

```python
class MyClass():
    pass
#用MyClass创建了一个对象mc，mc是MyClass的实例
mc = MyClass()
print(mc,type(mc))
12345
```

打印`<__main__.MyClass object at 0x000001C9EB32AA88> <class '__main__.MyClass'>`
解释：用MyClass创建了一个对象mc，mc是MyClass的实例。

```python
class MyClass():
    pass
#用MyClass创建了一个对象mc，mc是MyClass的实例
mc = MyClass()
mc2 = MyClass()
mc3 = MyClass()
mc4 = MyClass()
#用来检查一个对象是否是一个类的实例
result = isinstance(mc,MyClass)
result2 = isinstance(mc2,int)
result3 = isinstance(mc3,type(mc))
result4 = isinstance(mc4,type(mc4))
print(result,result2,result3,result4)
12345678910111213
```

打印`True False True True`
`isinstance()`用来检查一个对象是否是一个类的实例。

## 三、对象的基本介绍

### 1.对象的创建流程

类也是一个对象，类是一个用来创建对象的对象。

```python
class MyClass():
    pass
print(id(MyClass),type(MyClass))
123
```

打印`1720742711128 <class 'type'>`
显然，类是一个**type类型**的对象。
创建类的实例即对象时，实例并没有值，是空的，所对应的类也是没有值的，也是空类。
可以向对象中添加变量，对象中的变量称为属性。

```python
class MyClass():
    pass
mc = MyClass()
mc.name = 'Tom'
print(mc.name)
12345
```

打印`Tom`
即属性是添加到实例中，而不是到类中，如`print(MyClass.name)`会报错。

### 2.对象的定义

类和对象都是对现实生活中事物或程序内容的抽象。
实际上，所有的事物都是由两部分组成的：
**※数据（属性）**
**※行为（方法）**

在类的代码中，可以定义变量和函数；
在类中定义的变量将会成为所有实例的公共属性，所有实例都可以访问这些变量。
如

```python
class Person():
    a = 10
    b = 20
#创建Person的实例
p1 = Person()
p2 = Person()
print(p1.a,p2.b)
1234567
```

打印`10 20`

加入有意义的属性：

```python
class Person():
    name = 'Tom'
#创建Person的实例
p1 = Person()
p2 = Person()
print(p1.name)
123456
```

打印`Tom`
在类中可以定义函数，类中的函数称为**方法**。
这些方法通过该类的实例都可以访问。

```python
class Person():
    name = 'Tom'
    def speak(self):
        print('Hello')
#创建Person的实例
p1 = Person()
p2 = Person()
print(p1.name)
p2.speak()
123456789
```

打印

```python
Tom
Hello
12
```

如果是函数，有几个形参，就传几个实参；
如果是方法，默认传递一个参数，即类中的方法至少要定义一个形参，如上面`speak()`的`self`参数。

## 四、属性和方法

属性和方法的查找流程：
调用一个对象的属性和方法时，解析器会在当前对象中寻找是否有该属性和方法，如果有，直接返回；
如果没有，去当前对象的类对象中寻找，如果有，则返回类对象中的属性值和方法，如果没有，则报错。

```python
class Person():
    name = 'Tom'
    def speak(self):
        print('Hello')
#创建Person的实例
p1 = Person()
p2 = Person()
p1.name = 'John'
print(p1.name,p2.name)
123456789
```

打印`John Tom`。
类对象和实例对象中都可以保存属性和方法：
如果属性和方法是所有类的实例共享的，则应该保存到类对象中；
如果属性和方法是某个实例独有的，则应该保存到实例对象中；
一般，属性保存到实例对象中，方法保存到类对象方法中。

探究`self`：

```python
class Person():
    name = ''
    def speak(self,name):
        print(self)

#创建Person的实例
p1 = Person()
p2 = Person()
p1.name = 'John'
p2.name = 'Tom'
p1.speak(p1.name)
p2.speak(p2.name)
123456789101112
```

打印

```python
<__main__.Person object at 0x0000020D3D325188>
<__main__.Person object at 0x0000020D3D3251C8>
12
```

即默认传递的参数，就是调用方法的对象本身，明明为`self`。
所以，要想实现对象的方法分别调用自己的属性，可以如下：

```python
class Person():
    name = ''
    def speak(self,name):
        print('Hello, I\'m %s'%name)
#创建Person的实例
p1 = Person()
p2 = Person()
p1.name = 'John'
p2.name = 'Tom'
p1.speak(p1.name)
p2.speak(p2.name)
1234567891011
```

打印

```python
Hello, I'm John
Hello, I'm Tom
12
```

另一种方法：

```python
class Person():
    name = ''
    def speak(self):
        print('Hello, I\'m %s'%self.name)
#创建Person的实例
p1 = Person()
p2 = Person()
p1.name = 'John'
p2.name = 'Tom'
p1.speak()
p2.speak()
1234567891011
```

效果与前一种方法是一样的。

# Python基础之13.面向对象（2）

## 一、特殊方法

```python
class Person:
    name = 'Tom'

    def speak(self):
        print('Hello,I\'m %s'%self.name)

p1 = Person()
p2 = Person()
p2.speak()
123456789
```

打印`Hello,I'm Tom`
类中没有定义属性时，手动添加属性

```python
class Person:
    def speak(self):
        print('Hello,I\'m %s'%self.name)

p1 = Person()
p2 = Person()
#手动向对象添加属性
p1.name = 'Tom'
p2.name = 'Jerry'
p1.speak()
p2.speak()
1234567891011
```

打印

```python
Hello,I'm Tom
Hello,I'm Jerry
12
```

如未手动添加属性时，

```python
class Person:
    def speak(self):
        print('Hello,I\'m %s'%self.name)

p1 = Person()
p2 = Person()
p1.speak()
p2.speak()
12345678
```

便会报错：`AttributeError: 'Person' object has no attribute 'name'`
在上例中，对于Person这个类name属性是必须的，并且对于每一个对象name属性的值是不一样的，讲name属性手动添加，很容易出错。

### 1.特殊方法：

即魔术方法，都是以__开头、__结尾的方法。
特殊方法不需要我们调用，会在特殊的时候自己调用。
学习特殊方法：
※特殊方法什么时候调用；
※特殊方法有什么作用。

```python
class Person:
    def __init__(self):
        print('hello')

    def speak(self):
        print('Hello,I\'m %s'%self.name)

p1 = Person()
p1.__init__()
123456789
```

打印

```python
hello
hello
12
```

hello打印了两次，因为类的实例创建时，会自动调用了一次，再手动调用了一次，所以调用了两次。

```python
class Person:
    def __init__(self):
        print('hello')

    def speak(self):
        print('Hello,I\'m %s'%self.name)

p1 = Person()
p2 = Person()
p3 = Person()
p4 = Person()
1234567891011
```

打印

```python
hello
hello
hello
hello
1234
```

可以说，每创建一个实例对象，特殊方法就执行一次。

**对象的创建流程**：

```python
p1 = Person()
1
```

（1）创建了一个变量；
（2）在内存中创建了一个新的对象；
（3)执行类中的代码块的代码，并且**只在类中执行一次**；
（4）`__init__()`方法执行。
即如类的定义中除特殊方法还有别的代码语句，则其他语句先执行，如，

```python
class Person:
    print('In class Person')
    def __init__(self):
        print('hello')

    def speak(self):
        print('Hello,I\'m %s'%self.name)

p1 = Person()
p2 = Person()
p3 = Person()
p4 = Person()
123456789101112
```

打印

```python
In class Person
hello
hello
hello
hello
12345
```

`__init__()`方法会在对象创建以后立即执行，向新创建的对象初始化属性,

```python
class Person:
    def __init__(self):
        print(self)

    def speak(self):
        print('Hello,I\'m %s'%self.name)

p1 = Person()
p2 = Person()
p3 = Person()
p4 = Person()
1234567891011
```

打印

```python
<__main__.Person object at 0x000002E1EEC36708>
<__main__.Person object at 0x000002E1EEC36EC8>
<__main__.Person object at 0x000002E1EEC36F08>
<__main__.Person object at 0x000002E1EEC36F48>
1234
```

即打印self即打印出对象,self代表对象本身。

```python
class Person:
    def __init__(self):
        self.name = 'Tom'

    def speak(self):
        print('Hello,I\'m %s'%self.name)

p1 = Person()
p1.speak()
123456789
```

打印`Hello,I'm Tom`
为了解决之前出现过的问题，可以在`__init__()`方法中添加参数，通过self向新建的对象初始化对象。

```python
class Person:
    def __init__(self,name):
        self.name = name

    def speak(self):
        print('Hello,I\'m %s'%self.name)

p1 = Person()
p1.speak()
123456789
```

初始化时未添加name，会报错
`TypeError: __init__() missing 1 required positional argument: 'name'`
所以必须在初始化时即添加，

```python
class Person:
    def __init__(self,name):
        self.name = name

    def speak(self):
        print('Hello,I\'m %s'%self.name)

p1 = Person('Tom')
print(p1.name)
p2 = Person('Jerry')
print(p2.name)
p1.speak()
p2.speak()
12345678910111213
```

打印

```python
Tom
Jerry
Hello,I'm Tom
Hello,I'm Jerry
1234
```

### 2.类的基本结构：

```python
class 类名([父类]):
    公共属性
    #对象的初始化方法
    __init__(self,...):
        ...
    #其他的方法
    def method(self,...):
        ...
12345678
```

## 二、练习

要求：尝试定义一个车的类。

```python
class Car():
    '''属性：name、color
    方法：run()、didi()'''
    def __init__(self,name,color):
        self.name = name
        self.color = color

    def run(self):
        print('Car in running...')

    def didi(self):
        print('%s didi'%self.name)

c = Car('daben','white')
print(c.name,c.color)
c.run()
c.didi()
1234567891011121314151617
```

打印

```python
daben white
Car in running...
daben didi
123
```

定义对象时，希望对象能发挥一些作用，应该保证数据的安全性和有效性，

```python
class Car():
    '''属性：name、color
    方法：run()、didi()'''
    def __init__(self,name,color):
        self.name = name
        self.color = color

    def run(self):
        print('Car in running...')

    def didi(self):
        print('%s didi'%self.name)

c = Car('daben','white')
#修改属性
c.name = 'aodi'
print(c.name,c.color)
c.run()
c.didi()
12345678910111213141516171819
```

结果就会改变

```python
aodi white
Car in running...
aodi didi
123
```

要增加数据的安全性，要做到：
1.属性不能随意修改，有确实要改的需要才改，没有真实的需要才改；
2.属性不能改为任意的值，要保证有效。

## 三、封装

封装是面向对象的三大特性之一（另外两个是继承、多态）。
封装指的是隐藏对象中一些不希望被外部访问到的属性和方法。

```python
class Dog:
    def  __init__(self,name):
        self.name = name

d = Dog('erha')
d.name = 'kaisa'
print(d.name)
1234567
```

打印`kaisa`
此例中name属性即未被隐藏，会被任意修改。
如何修改对象中的一个属性：
将对象的属性名修改为外部不知道的名字。

```python
class Dog:
    def  __init__(self,name):
        self.hidden_name = name

    def speak(self):
        print('Hello,I\'m %s'%self.hidden_name)

d = Dog('erha')
d.name = 'kaisa'
d.speak()
12345678910
```

打印`Hello,I'm erha`
此时不能修改，要想修改，

```python
class Dog:
    def  __init__(self,name):
        self.hidden_name = name

    def speak(self):
        print('Hello,I\'m %s'%self.hidden_name)

d = Dog('erha')
d.hidden_name = 'kaisa'
d.speak()
12345678910
```

打印`Hello,I'm kaisa`，这又不是封装，成了最初的形式。
如何获取（修改）对象当中的属性：
通过提供getter()和setter()方法来访问和修改属性。

```python
class Dog:
    def  __init__(self,name):
        self.hidden_name = name

    def speak(self):
        print('Hello,I\'m %s'%self.hidden_name)

    def get_name(self):
        #获取对象的属性
        return self.hidden_name

    def set_name(self,name):
        #修改对象属性
        self.hidden_name = name

d = Dog('erha')
print(d.get_name())
d.set_name('kaisa')
print(d.get_name())
d.speak()
1234567891011121314151617181920
```

打印

```python
erha
kaisa
Hello,I'm kaisa
123
```

使用封装确实在一定程度上增加了程序的复杂性，
但是它**确保了数据的安全性**：
1.隐藏了属性名，使调用者无法任意修改对象中的属性。
2.增加了getter和setter方法，可以很好地控制属性是否是可读的：
如果希望属性是可读的，则可以直接去掉setter方法；
如果希望属性不能被完结访问，则可以直接去掉getter方法。
3.使用setter方法设置属性，可以增加数据的验证，确保数据的值是正确的。
虽然性，但是增加了程序的安全性。
4.可以在读取属性和设置属性的方法中做一些其他的操作。

```python
class Dog:
    def  __init__(self,name,age):
        self.hidden_name = name
        self.hidden_age = age

    def speak(self):
        print('Hello,I\'m %s'%self.hidden_name)

    def get_name(self):
        #获取对象的属性
        return self.hidden_name

    def set_name(self,name):
        #修改对象属性
        self.hidden_name = name

    def get_age(self):
        #获取对象的属性
        return self.hidden_age

    def set_age(self,age):
        #修改对象属性
        self.hidden_age = age

d = Dog('erha',5)
print(d.get_age())
d.set_age(-5)
print(d.get_age())
12345678910111213141516171819202122232425262728
```

打印

```python
5
-5
12
```

显然，把狗的年龄设为负值程序是没报错的，但是是没有实际意义的，所以我们可以增加对数据的判断。

```python
class Dog:
    def  __init__(self,name,age):
        self.hidden_name = name
        self.hidden_age = age

    def speak(self):
        print('Hello,I\'m %s'%self.hidden_name)

    def get_name(self):
        #获取对象的属性
        return self.hidden_name

    def set_name(self,name):
        #修改对象属性
        self.hidden_name = name

    def get_age(self):
        #获取对象的属性
        return self.hidden_age

    def set_age(self,age):
        #修改对象属性
        #判断再处理
        if age > 0:
            self.hidden_age = age

d = Dog('erha',5)
print(d.get_age())
d.set_age(-5)
print(d.get_age())
123456789101112131415161718192021222324252627282930
```

打印

```python
5
5
12
```

## 四、封装进阶

```python
class Person():
    def __init__(self,name):
        self.hidden_name = name

    def get_name(self):
        return self.hidden_name

    def set_name(self,name):
        self.hidden_name = name

p = Person('Tom')
p.set_name('Jerry')
print(p.get_name())
12345678910111213
```

打印`Jerry`，即属性值会改变。hidden_name改为__name

```python
class Person():
    def __init__(self,name):
        self.__name = name

    def get_name(self):
        return self.__name

    def set_name(self,name):
        self.__name = name

p = Person('Tom')
print(p.get_name())
123456789101112
```

打印`Tom`

```python
class Person():
    def __init__(self,name):
        self.__name = name

    def get_name(self):
        return self.__name

    def set_name(self,name):
        self.__name = name

p = Person('Tom')
print(p.__name)
123456789101112
```

会报错`AttributeError: 'Person' object has no attribute '__name'`

```python
class Person():
    def __init__(self,name):
        self.__name = name

    def get_name(self):
        return self.__name

    def set_name(self,name):
        self.__name = name

p = Person('Tom')
p.__name = 'Jerry'
print(p.get_name())
12345678910111213
```

打印还是`Tom`，没有被修改。
总结：可以为对象的属性使用双下划线开头，`__xxx`。
双下划线开头的属性是悐的隐藏属性，隐藏属性只能在类的内部访问，无法通过外部访问。
隐藏属性是Python自动为属性改了一个名字，实际改的名字是:_类名_属性名_：
__name -> _Person__name

```python
class Person():
    def __init__(self,name):
        self.__name = name

    def get_name(self):
        return self.__name

    def set_name(self,name):
        self.__name = name

p = Person('Tom')
print(p.get_name())
p._Person__name = 'Jerry'
print(p.get_name())
1234567891011121314
```

打印

```python
Tom
Jerry
12
```

即`_Person__name`又可以对属性进行修改。
在写代码的时候，可以用一个下划线，既容易书写，外部能访问，且不易被修改。
一般情况下：使用_开头的属性都是私有属性，没有特殊情况不要修改。

## 五、装饰器@property

用来将get()方法转化为对象的属性。
添加property装饰器以后，就可以像调用属性一样调用方法。

```python
class Person():
    def __init__(self,name):
        self._name = name

    def name(self):
        return self._name

p = Person('Tom')
print(p.name())
123456789
```

打印`Tom`
用了`property`装饰器以后,

```python
class Person():
    def __init__(self,name):
        self._name = name

    @property
    def name(self):
        print('The method is run')
        return self._name

p = Person('Tom')
print(p.name)
1234567891011
```

输出结果为：

```python
The method is run
Tom
12
```

加入setter后，

```python
class Person():
    def __init__(self,name):
        self._name = name

    @property
    def name(self):
        print('The method is run')
        return self._name

    @name.setter
    def name(self,name):
        self._name = name

p = Person('Tom')
print(p.name)
p.name = 'Jerry'
print(p.name)
1234567891011121314151617
```

打印

```python
The method is run
Tom
The method is run
Jerry
```

# Python基础之14.面向对象（3）

## 一、继承的简介

### 1.引入

```python
class Doctor():
    name = ''
    age = ''
    def treat(self):
        print('treat a patient')

class soldier():
    name = ''
    age = ''

    def protect(self):
        print('Protect the people')
123456789101112
```

以上定义了两个类，但是又重复的属性name、age，如果每定义一个类都要分别定义这两个属性，很重复冗余，所以可以抽象出一个类来封装它们的公共属性和方法，即它们的父类这些类都`继承`自它们的父类。
如

```python
class Person():
    name = ''
    age = ''
123
```

### 2.继承的特点：

1.提高了代码的复用性；
2.让类与类之间产生联系，有了这个联系，才有了多态。
继承是面向对象的三大特性之一，其他两个特性是封装、多态。

定义一个动物类

```python
class Animal:
    def run(self):
        print('Running...')

    def sleep(self):
        print('Sleeping...')

a = Animal()
a.run()
123456789
```

再定义一个狗类，

```python
class Dog:
    def run(self):
        print('Running...')

    def sleep(self):
        print('Sleeping...')
123456
```

这样定义狗类，有大量重复性代码，表现较差，我们应该使狗类继承自动物类，继承动物类的属性和方法。

```python
class Dog(Animal):

    pass

d = Dog()
d.run()
d.sleep()
1234567
```

打印

```python
Running...
Sleeping...
12
```

在定义类时可以在类名＋括号，括号内的指定的是当前类的父类（超类、基类，super）。

```python
class Dog(Animal):
    def Housekeep(self):
        print('Housekeeping...')

d = Dog()
d.run()
d.sleep()
d.Housekeep()
r1 = isinstance(d,Dog)
r2 = isinstance(d,Animal)
print(r1,r2)
1234567891011
```

打印

```python
Running...
Sleeping...
Housekeeping...
True True
1234
```

显然，d既是Dog的实例，也是Animal的实例，所以Dog也继承自了Animal。
如果在创建类的时候省略了父类，则默认父类是object；
object是所有类的父类，所有类都继承自object。
`issubclass()`检查一个类是否是另一个类的子类。

```python
class Animal:
    def run(self):
        print('Running...')

    def sleep(self):
        print('Sleeping...')

class Dog(Animal):
    def Housekeep(self):
        print('Housekeeping')

r1 = issubclass(Dog,Animal)
r2 = issubclass(Animal,object)
print(r1,r2)
1234567891011121314
```

证明了所有类都是object的子类，所有对象都是object的实例。

## 二、方法的重写

如果在子类中有和父类重名的方法，通过子类的实例去调用方法时，会调用子类的方法而不是父类的方法，这个特点称为方法的重写（覆盖，override）。
如，

```python
class Animal:
    def run(self):
        print('Running...')

    def sleep(self):
        print('Sleeping...')

class Dog(Animal):
    def run(self):
        print('Dog is running...')

    def sleep(self):
        print('Dog is sleeping...')

    def Housekeep(self):
        print('Dog is housekeeping')

d = Dog()
d.run()
12345678910111213141516171819
```

打印`Dog is running...`
下边再详细说明，

```python
class A(object):
    def test(self):
        print('A...')

class B(A):
    pass

class C(B):
    pass

c = C()
c.test()
123456789101112
```

打印`A...`。
类B中加入方法test()后，

```python
class A(object):
    def test(self):
        print('A...')

class B(A):
    def test(self):
        print('B...')

class C(B):
    pass

c = C()
c.test()
12345678910111213
```

打印`B...`
类C中加入方法test()后，

```python
class A(object):
    def test(self):
        print('A...')

class B(A):
    def test(self):
        print('B...')

class C(B):
    def test(self):
        print('C...')

c = C()
c.test()
1234567891011121314
```

打印`C...`。
当我们调用一个对象时，会有先去当前对象寻找是否有该方法，如果有直接调用；如果没有，则取当前对象的父类中寻找，如果有，直接调用父类中的方法，如果没有，则去父类的父类寻找，如果有直接调用，以此类推，直到找到object，如果依然没有则报错。

## 三、super()

父类中所有的方法都会被子类继承，包括特殊方法，如`__init__()`等，也可以重写特殊方法。

```python
class Animal:
    def __init__(self,name):
        self._name = name

    def run(self):
        print('Running...')

    def sleep(self):
        print('Sleeping...')

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self,name):
        self._name = name

class Dog(Animal):
    def run(self):
        print('Dog is running...')

    def sleep(self):
        print('Dog is sleeping...')

    def Housekeep(self):
        print('Dog is housekeeping')

d = Dog()
d.run()
123456789101112131415161718192021222324252627282930
```

会抛出异常，`TypeError: __init__() missing 1 required positional argument: 'name'`
因为初始化没有传入参数，因为父类初始化要传入name这个参数，子类也必须传入。
传入参数后,

```python
class Animal:
    def __init__(self,name):
        self._name = name

    def run(self):
        print('Running...')

    def sleep(self):
        print('Sleeping...')

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self,name):
        self._name = name

class Dog(Animal):
    def run(self):
        print('Dog is running...')

    def sleep(self):
        print('Dog is sleeping...')

    def Housekeep(self):
        print('Dog is housekeeping')

d = Dog('erha')
print(d.name)
123456789101112131415161718192021222324252627282930
```

打印`erha`
调用setter方法修改属性，

```python
class Animal:
    def __init__(self,name):
        self._name = name

    def run(self):
        print('Running...')

    def sleep(self):
        print('Sleeping...')

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self,name):
        self._name = name

class Dog(Animal):
    def run(self):
        print('Dog is running...')

    def sleep(self):
        print('Dog is sleeping...')

    def Housekeep(self):
        print('Dog is housekeeping')

d = Dog('erha')
d.name = 'demu'
print(d.name)
12345678910111213141516171819202122232425262728293031
```

打印`demu`。
如果需要在子类中定义新的属性时，即要扩展属性时，

```python
class Animal:
    def __init__(self,name):
        self._name = name

    def run(self):
        print('Running...')

    def sleep(self):
        print('Sleeping...')

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self,name):
        self._name = name

class Dog(Animal):
    def __init__(self,name,age):
        self._name = name
        self._age = age

    def run(self):
        print('Dog is running...')

    def sleep(self):
        print('Dog is sleeping...')

    def Housekeep(self):
        print('Dog is housekeeping')

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, age):
        self._age = age

d = Dog('erha',10)
print(d.name)
print(d.age)
12345678910111213141516171819202122232425262728293031323334353637383940414243
```

打印

```python
erha
10
12
```

可以直接调用父类中的`__init__()`来初始化父类中的属性，

```python
class Animal:
    def __init__(self,name):
        self._name = name

    def run(self):
        print('Running...')

    def sleep(self):
        print('Sleeping...')

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self,name):
        self._name = name

class Dog(Animal):
    def __init__(self,name,age):
        Animal.__init__(self,name)
        self._age = age

    def run(self):
        print('Dog is running...')

    def sleep(self):
        print('Dog is sleeping...')

    def Housekeep(self):
        print('Dog is housekeeping')

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, age):
        self._age = age

d = Dog('erha',10)
print(d.name)
print(d.age)
12345678910111213141516171819202122232425262728293031323334353637383940414243
```

即Dog类中的初始化方法中，name属性通过Animal的初始化调用获取，可以将父类的多个属性传给子类，但是继承的父类是固定的，这是需要用suoer()动态地继承父类属性。
super()可以用来获取当前类的父类，并且通过suoer()返回的对象，调用父类方法时不需要传入self。

```python
class Animal:
    def __init__(self,name):
        self._name = name

    def run(self):
        print('Running...')

    def sleep(self):
        print('Sleeping...')

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self,name):
        self._name = name

class Dog(Animal):
    def __init__(self,name,age):
        super().__init__(name)
        self._age = age

    def run(self):
        print('Dog is running...')

    def sleep(self):
        print('Dog is sleeping...')

    def Housekeep(self):
        print('Dog is housekeeping')

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, age):
        self._age = age

d = Dog('erha',10)
print(d.name)
print(d.age)
12345678910111213141516171819202122232425262728293031323334353637383940414243
```

执行结果与之前相同。

## 四、多重继承

语法：类名._*bases*_
可以用来获取当前类的所有直接父类。

```python
class A(object):
    def test(self):
        print('A...')

class B(A):
    def test2(self):
        print('B...')

class C(B):
    pass

print(C.__bases__)
print(B.__bases__)
print(A.__bases__)
1234567891011121314
```

打印

```python
(<class '__main__.B'>,)
(<class '__main__.A'>,)
(<class 'object'>,)
123
```

在Python中支持多重继承，即可以为一个类同时指定多个父类。

```python
class A(object):
    def test(self):
        print('A...')

class B(object):
    def test2(self):
        print('B...')

class C(A,B):
    pass

print(C.__bases__)
123456789101112
```

打印`(<class '__main__.A'>, <class '__main__.B'>)`。

```python
class A(object):
    def test(self):
        print('A...')

class B(object):
    def test2(self):
        print('B...')

class C(A,B):
    pass

c = C()
c.test()
c.test2()
1234567891011121314
```

打印

```python
A...
B...
12
```

显然，子类可以同时继承多个父类的方法。
但是一般在实际中没有特殊情况，尽量避免使用多重继承，因为多重继承会让代码过于复杂；
如果多个父类中有同名的方法，则会在第一个父类中寻找，找不到再继续往下寻找…
如，

```python
class A(object):
    def test(self):
        print('A...test')

class B(object):
    def test(self):
        print('B...test')

    def test2(self):
        print('B...')

class C(A,B):
    pass

c = C()
c.test()
12345678910111213141516
```

会打印`A...test`
如果A中没有test()方法，

```python
class A(object):
    pass

class B(object):
    def test(self):
        print('B...test')

    def test2(self):
        print('B...')

class C(A,B):
    pass

c = C()
c.test()
123456789101112131415
```

会打印`B...test`

## 五、多态

多态是面向对象的三大特征之一。
多态字面理解即多种形态，在面向对象中指一个对象可以以不同的形态呈现。

```python
class A(object):
    def __init__(self,name):
        self._name = name

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self,name):
        self._name = name


class B(object):
    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, name):
        self._name = name

def speak(obj):
    print('Hello,%s' % obj.name)

a = A('Tom')
b = B('Jerry')
speak(a)
speak(b)
1234567891011121314151617181920212223242526272829303132
```

打印

```python
Hello,Tom
Hello,Jerry
12
```

定义一个新类C，但是没有name属性时，

```python
class A(object):
    def __init__(self,name):
        self._name = name

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self,name):
        self._name = name


class B(object):
    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, name):
        self._name = name

class C(object):
    pass
def speak(obj):
    print('Hello,%s' % obj.name)

a = A('Tom')
b = B('Jerry')
c= C()
speak(a)
speak(b)
speak(c)
123456789101112131415161718192021222324252627282930313233343536
```

会抛出异常，`AttributeError: 'C' object has no attribute 'name'`，
增加类型检查，

```python
class A(object):
    def __init__(self,name):
        self._name = name

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self,name):
        self._name = name


class B(object):
    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, name):
        self._name = name

class C(object):
    pass

def speak(obj):
    print('Hello,%s' % obj.name)

def test_speak(obj):
    '''类型检查'''
    if isinstance(obj,A):
        print('Hello,%s' % obj.name)

a = A('Tom')
b = B('Jerry')
c= C()
test_speak(a)
test_speak(b)
test_speak(c)
123456789101112131415161718192021222324252627282930313233343536373839404142
```

打印`Hello,Tom`。
在`test_speak()`这个函数中做了类型检查，也就是obj是A类型的对象的时候，才可以正常执行，其他类型的对象无法使用该函数。
这个函数其实就是违反了多态。
违反了多态的函数，只适合于一种类型的对象，无法适用于其他类型的对象，导致函数的适用性非常差。
多态使面向对象的编程更加具有`灵活性`。

```python
l = [1,2,3]
s = 'python'
print(len(l),len(s))
123
```

如上，len()函数既可以得到list的长度，又可以获取str的长度，
len()就可以检查不同对象类型的长度就是面向对象的特征之一——多态。
len()的底层实现：
之所以len()函数能获取长度，是因为这些对象中有一个特殊方法__len__()方法；
换句话说，只要对象中有__len__()方法，就可以通过len()方法来获取对象的长度。

```python
class B(object):
    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, name):
        self._name = name

b = B('Jerry')
print(len(b))
1234567891011121314
```

会报错`TypeError: object of type 'B' has no len()`。
在B中加入`__len__()`方法后，

```python
class B(object):
    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, name):
        self._name = name
    
    def __len__(self):
        return 1

b = B('Jerry')
print(len(b))
1234567891011121314151617
```

会打印`1`

**面向对象的三大特征**：
1.封装：确保对象中的数据更安全；
2.继承：保证对象的可扩展性；
3.多态：保证了程序的灵活性。

## 六、类中的属性和方法

类属性是直接在类中定义的属性，类属性可以通过类或类的实例访问；
通过实例对象添加的属性属于实例属性。

```python
class A(object):
    #类属性
    count = 0

a = A()
print(A.count)
print(a.count)
1234567
```

打印

```python
0
0
12
```

修改实例的属性，

```python
class A(object):
    #类属性
    count = 0

a = A()
a.count = 5
print('In class,',A.count)
print('In instance,',a.count)
12345678
```

打印

```python
In class, 0
In instance, 5
12
```

修改类中的属性，

```python
class A(object):
    #类属性
    count = 0

a = A()
A.count = 5
print('In class,',A.count)
print('In instance,',a.count)
12345678
```

打印

```python
In class, 5
In instance, 5
12
```

可总结出：类属性只能通过类对象来修改，无法通过实例对象来修改；

```python
class A(object):
    def __init__(self):
        #实例属性
        self.name = 'Tom'

a = A()
print(a.name)
1234567
```

打印`Tom`
在初始化中定义的属性为实例属性，只能通过实例属性来访问和修改，类对象无法访问和修改。
如，

```python
class A(object):
    def __init__(self):
        #实例属性
        self.name = 'Tom'

a = A()
print(A.name)
1234567
```

报错`AttributeError: type object 'A' has no attribute 'name'`
在类中定义的以self为第一个参数的方法都是实例方法；
实力方法在调用时，Python会将调用对象作为self传入；

```python
class A(object):
    def __init__(self):
        #实例属性
        self.name = 'Tom'
    def test(self):
        #实例方法
        print('in test')
a = A()
a.test()
123456789
```

打印`in test`
类对象调用时，

```python
class A(object):
    def __init__(self):
        #实例属性
        self.name = 'Tom'
    def test(self):
        #实例方法
        print('in test')
a = A()
A.test()
123456789
```

报错`TypeError: test() missing 1 required positional argument: 'self'`
但是通过类对象调用时，传入实例对象多为方法的参数不会报错。
即通过类对象调用方法时，不会自动穿self，必须手动传self。

```python
class A(object):
    def __init__(self):
        #实例属性
        self.name = 'Tom'
    def test(self):
        #实例方法
        print('in test')
a = A()
A.test(a)
123456789
```

输出结果相同。
即`a.test()`等价于`A.test(a)`。
可以通过**classmethod**装饰器来调用类方法；
类方法的第一个参数是cls，也会被自动传递，cls就是当前的类对象。

```python
class A(object):
    count = 0
    def __init__(self):
        #实例属性
        self.name = 'Tom'
    def test(self):
        #实例方法
        print('in test')
    @classmethod
    def test2(cls):
        #实例方法
        print('in test2',cls)

a = A()
A.test2()
123456789101112131415
```

打印`in test2 <class '__main__.A'>`
调用类属性，

```python
class A(object):
    count = 0
    def __init__(self):
        #实例属性
        self.name = 'Tom'
    def test(self):
        #实例方法
        print('in test')
    @classmethod
    def test2(cls):
        #实例方法
        print('in test2',cls)
        print(cls.count)

a = A()
A.test2()
12345678910111213141516
```

打印

```python
in test2 <class '__main__.A'>
0
12
```

通过实例调用类方法，

```python
class A(object):
    count = 0
    def __init__(self):
        #实例属性
        self.name = 'Tom'
    def test(self):
        #实例方法
        print('in test')
    @classmethod
    def test2(cls):
        #实例方法
        print('in test2',cls)
        print(cls.count)

a = A()
a.test2()
12345678910111213141516
```

结果相同。
可总结：类方法可以通过类对象调用，也可通过实例方法调用。
`A.test2()`等价于`a.test2()`。
静态方法：
用**staticmethod**装饰器来调用类方法

```python
class A(object):
    count = 0
    def __init__(self):
        #实例属性
        self.name = 'Tom'
    def test(self):
        #实例方法
        print('in test')
    @classmethod
    def test2(cls):
        #实例方法
        print('in test2',cls)
        print(cls.count)
    @staticmethod
    def test3():
        print('in test3')

a = A()
A.test3()
a.test3()
1234567891011121314151617181920
```

打印

```python
in test3
in test3
12
```

静态方法，基本上是一个和当前类无关的方法，它只是一个保存到当前类中的函数；
静态方法一般是工具方法，和当前类无关。

# Python基础之15.异常处理

## 一、异常的简介

### 1.异常定义

程序在运行过程中不可避免会出现一些错误，比如使用了没有被赋值过的变量、除0、使用了不存在的索引等等。
如执行`print(a)`，会抛出`NameError: name 'a' is not defined`；
执行`print(1/0)`，会抛出`ZeroDivisionError: division by zero`。
这些错误在程序中即称为异常。
程序在运行过程中一旦出现异常会导致程序立即终止，异常后面的代码都不会执行。

### 2.处理异常

程序出现异常，目的不是要程序立即终止；
Python中设置异常的目的时再出现异常时，编写代码对异常进行处理。
处理异常语法：**try_except_else**语句

```python
try:
    代码块(可能出现错误的语句)
except:
    代码块(出现错误以后的处理方式)
else:
    代码块(没有错误时要执行的语句)
123456
```

如，在try代码块中没有错误时，

```python
print('Python')
try:
    print(1/1)
except:
    print('Error in Try')
print('hello')
123456
```

结果为，

```python
Python
1.0
hello
123
```

在try代码块中有错误时

```python
print('Python')
try:
    print(1/0)
except:
    print('Error in Try')
print('hello')
123456
```

打印

```python
Python
Error in Try
hello
123
```

后边有else语句时且try代码块中没有错误时，

```python
print('Python')
try:
    print(1/1)
except:
    print('Error in Try')
else:
    print('No error in Try')
print('hello')
12345678
```

结果为

```python
Python
1.0
No error in Try
hello
1234
```

后边有else语句时且try代码块中有错误时，

```python
print('Python')
try:
    print(1/0)
except:
    print('Error in Try')
else:
    print('No error in Try')
print('hello')
12345678
```

打印

```python
Python
Error in Try
hello
123
```

## 二、异常的传播

当在函数中出现异常时，如果在函数中对异常进行了处理，则异常不会再传播；
如果函数中没有对异常进行处理，则异常继续向函数调用处传播。
如，

```python
def fn():
    print('hello.......')
    print(1/0)
fn()
12345
```

会出现两次报错，

```python
Traceback (most recent call last):
  File "xxx/Demo.py", line 16, in <module>
    fn()
  File "xxx/Demo.py", line 14, in fn
    print(1 / 0)
ZeroDivisionError: division by zero
123456
```

对异常进行处理后，

```python
def fn():
    print('hello.......')
    try:
        print(1 / 0)
    except:
        pass

fn()
12345678
```

打印`hello.......`。
如果函数调用处处理了异常，则不再传播；
如果没有处理则继续向调用处传播；
直到传递到全局作用域，如果依然没有处理，则程序终止,并显示异常信息。

```python
def fn():
    print('in fn')
    print(1/0)

def fn2():
    print('in fn2')
    fn()

def fn3():
    print('in fn3')
    fn2()

fn3()
12345678910111213
```

打印

```python
in fn3
in fn2
in fn
Traceback (most recent call last):
  File "xxx/Demo.py", line 34, in <module>
    fn3()
  File "xxx/Demo.py", line 32, in fn3
    fn2()
  File "xxx/Demo.py", line 28, in fn2
    fn()
  File "xxx/Demo.py", line 24, in fn
    print(1/0)
ZeroDivisionError: division by zero
12345678910111213
```

即抛出了ZeroDivisionError，这是一个**异常对象**。

## 三、异常对象

当程序运行过程中出现异常以后，所有的异常信息会被专门保存到一个异常对象中；
异常传播时，实际上就是异常对象抛给了调用处。
如NameError类专门处理变量的错误。

```python
print('Before Exception')
try:
    print(a)
    print(1/0)
except:
    print('Handling Exception')
print('After Exception')
1234567
```

打印结果为

```python
Before Exception
Handling Exception
After Exception
123
```

except后增加异常对象时，

```python
print('Before Exception')
try:
    print(a)
    print(1/0)
except NameError:
    print('NameError Exception')
print('After Exception')
1234567
```

结果为

```python
Before Exception
NameError Exception
After Exception
123
```

但是当except语句中的异常对象未与try语句块中的异常类型匹配时，

```python
print('Before Exception')
try:
    print(1/0)
except NameError:
    print('NameError Exception')
print('After Exception')
123456
```

打印

```python
Traceback (most recent call last):
  File "xxx/Demo.py", line 37, in <module>
    print(1/0)
ZeroDivisionError: division by zero
1234
```

出现报错，意即except后的异常对象只能捕获与之对应的异常，不能捕获其他异常。

```python
print('Before Exception')
try:
    print(1/0)
except NameError:
    print('NameError Exception')
except ZeroDivisionError:
    print('ZeroDivisionError Exception')
print('After Exception')
12345678
```

打印

```python
Before Exception
ZeroDivisionError Exception
After Exception
123
```

即此时捕获到`ZeroDivisionError`，try语句块后边可以同时跟多个except语句块，来捕获多个异常。
如果except后面不跟任何内容，此时会捕获到所有的异常；
如果except后面跟着一个异常类型，此时只会捕获该类型的异常。
Exception是所有异常类型的父类；
如果except后跟的是Exception，它会捕获到所有的异常。

```python
print('Before Exception')
try:
    print(a)
    print(1/0)
except Exception:
    print('in Exception')
print('After Exception')
1234567
```

结果为

```python
Before Exception
in Exception
After Exception
123
```

即Exception可以捕获所有的异常，并且可将异常对象赋值给另一个对象，来进行打印以识别异常类型。

```python
print('Before Exception')
try:
    print(1/0)
except Exception as e:
    print(e,type(e))
print('After Exception')
123456
```

打印

```python
Before Exception
division by zero <class 'ZeroDivisionError'>
After Exception
123
```

可据此得出异常内容和异常类型。

### finally语句

无论是否出现异常都会执行。
没有异常时，

```python
print('Before Exception')
try:
    print(1/1)
finally:
    print('Hello')
print('After Exception')
123456
```

打印结果为，

```python
Before Exception
1.0
Hello
After Exception
1234
```

有异常但无except语句块时，

```python
print('Before Exception')
try:
    print(1/0)
finally:
    print('Hello')
print('After Exception')
123456
```

打印结果为，

```python
Traceback (most recent call last):
  File "xxx/Demo.py", line 37, in <module>
    print(1/0)
ZeroDivisionError: division by zero
Before Exception
Hello
123456
```

有异常且有except语句块时，

```python
print('Before Exception')
try:
    print(1/0)
except:
    print('In Exception')
finally:
    print('Hello')
print('After Exception')
12345678
```

打印

```python
Before Exception
In Exception
Hello
After Exception
1234
```

显然，有无异常时均执行了finally语句块中的语句。
当try语句块中有异常时，如果没有except语句，先执行finally语句块，再回到try语句块报错；
如果有except语句块，则先执行except语句块，再执行finally语句块。

### 异常的完整语法：

```python
try:
    代码块(可能出现错误的语句)
except 异常类型1 as 异常名:
    代码块(出现错误以后的处理方式)
except 异常类型2 as 异常名:
    代码块(出现错误以后的处理方式)
except 异常类型3 as 异常名:
    代码块(出现错误以后的处理方式)
...
else:
    代码块(没有错误时要执行的语句)
finally:
    代码块(是否有异常都会执行)
12345678910111213
```

## 四、自定义异常对象

### 1.抛出异常

使用raisre语句来抛出异常，后面需要跟一个异常类或异常实例。

```python
def add(a,b):
    r = a + b
    return r

print(add(-1,2))
12345
```

打印结果为`1`。
现在定义函数只计算正数，如果a、b中有负数，就向调用处抛异常。

```python
def add(a,b):
    if a < 0 or b < 0:
        raise Exception
    r = a + b
    return r

print(add(-1,2))
1234567
```

此时会报错

```python
Traceback (most recent call last):
  File "xxx/Demo.py", line 48, in <module>
    print(add(-1,2))
  File "xxx/Demo.py", line 44, in add
    raise Exception
Exception
123456
```

同时，可自定义异常类型，便于直观得出异常信息

```python
def add(a,b):
    if a < 0 or b < 0:
        raise Exception('NegativeError')
    r = a + b
    return r

print(add(-1,2))
1234567
```

此时为

```python
Traceback (most recent call last):
  File "xxx/Demo.py", line 48, in <module>
    print(add(-1,2))
  File "xxx/Demo.py", line 44, in add
    raise Exception('NegativeError')
Exception: NegativeError
123456
```

抛出异常的目的，时告诉调用者调用时可能出现问题，以便自己进行处理。

### 2.定义异常类

只需要继承Exception，就可以了。

```python
class MyError(Exception):
    pass

def add(a,b):
    if a < 0 or b < 0:
        raise MyError('NegativeError')
    r = a + b
    return r

print(add(-1,2))
12345678910
```

结果为

```python
Traceback (most recent call last):
  File "xxx/Demo.py", line 53, in <module>
    print(add(-1,2))
  File "xxx/Demo.py", line 49, in add
    raise MyError('NegativeError')
__main__.MyError: NegativeError
123456
```

抛出了MyError异常，并给出了异常提示信息。

# Python全栈（二）数据结构和算法之1.算法和数据结构引入

## 一、算法的引入

### 1.运行时间

如果 a+b+c=1000，且 a2+b2=c2（a、b、c 为自然数），如何求出所有a、b、c可能的组合?
可以用枚举法，简单，但是计算量大。
需要用至少三个循环来完成。

```python
import time
start = time.time()
for a in range(1001):
    for b in range(1001):
        for c in range(1001):
            if a**2+b**2 == c**2 and a+b+c==1000:
                print('a = %d,b = %d,c = %d'%(a,b,c))
end = time.time()
print('The time is %f seconds'%(end-start))
123456789
```

运行结果

```python
a = 0,b = 500,c = 500
a = 200,b = 375,c = 425
a = 375,b = 200,c = 425
a = 500,b = 0,c = 500
The time is 1317 seconds
12345
```

显然，运行用了很长时间。
其实，枚举法也是一种**算法**，该算法的执行次数为1000的三次方，效率很低。

### 2.算法的概念

算法是计算机处理信息的本质。
算法是独立存在的一种**解决问题**的方法和**思想**。
算法的五大特性：
**输入；
输出；
有穷性；
确定性；
可行性。**
对上面程序可以进行一定优化：

```python
import time
start = time.time()
for a in range(1001):
    for b in range(1001-a):
        c = 1000 - a - b
        if a**2+b**2 == c**2:
            print('a = %d,b = %d,c = %d'%(a,b,c))
end = time.time()
print('The time is %f seconds'%(end-start))
123456789
```

运行结果

```python
a = 0,b = 500,c = 500
a = 200,b = 375,c = 425
a = 375,b = 200,c = 425
a = 500,b = 0,c = 500
The time is 0.992053 seconds
12345
```

显然比上面的运行时间要短得多得多。

可以大致得到一个结论：
实现算法程序的执行时间可以反应出算法的效率，即算法的优劣。
算法的效率衡量：
**执行时间（在一定程度上）反映算法效率**。
但是并不是绝对的，
由于：
（1）测试结果非常依赖测试环境：
硬件不同会对结果造成很大影响。
（2）测试结果受数据规模和数据本身的影响较大：
如对于排序，如数据本身是有序的，可能执行时间就会相对较短，同时数据规模较小时，测试时间不一定能真实反映算法的性能。
所以每台机器执行总时间不一定一致，但是执行的基本运算的次数大体相同。

## 二、时间复杂度

### 1.时间复杂度大O

第一个例子中，执行的次数为：
T = 1000 * 1000 * 1000 * 2

1000到2000时
T = 2000 * 2000 * 2000 * 2

1000换成N时，
T = n * n * n * 2 = n ^ 3 * 2 = f(n) * 2
即T(n) = f(n) * 2

**T(n) = O(f(n))**
其中，n表示数据规模的大小，T(n)表示代码执行的时间，f(n)代表每行代码的执行次数总和，O表示T(n)与f(n)成正比。
此即为大O时间复杂度表示法，不是代码真正的执行时间，而是代码执行时间随数据规模增长的变化趋势，也叫渐进时间复杂度，简称**时间复杂度**。

### 2.时间复杂度分析

方法：

#### 只关心循环执行次数最多的一段代码

```python
def cal(n):
    sum = 0
    i = 1
    for i in range(n+1):
        sum += i
        return sum
123456
```

函数内部，前两行是常量级的执行时间，与n的大小无关，可忽略，关注循环次数最多的那一个即for循环，for循环执行了n次，所以时间复杂度为O(n)。

#### 加法原则

总复杂度等于量级最大的那段代码的复杂度。

```python
def cal(n):
    sum = 0
    i = 1
    for i in range(100):
        sum += i
    for i in range(n+1):
        sum += i
    for i in range(n+1):
        for i in range(n + 1):
            pass
12345678910
```

第一个for循环为常数循环，复杂度为**O(1)**，第二个for为单层循环，为**O(n)**，第三个for循环为双层嵌套循环，时间复杂度为**O(n2)**。
如`T(n) = max(O(1),O(n),O(n^2)) = O(n^2)`。
分析算法时，存在几种可能：
（1）**最优时间复杂度**：
最少需要多少基本操作
`li = [1,2,3,4,5]`
列表有序，则常数时间，O(1)。

（2）**平均时间复杂度**：
平均需要多少基本操作。

（3）**最坏时间复杂度**：
最多需要多少基本操作。
列表无序时，时间复杂度与元素个数正相关，即O(n)。
最优时间复杂度和平均时间复杂度价值不大，未提供太多有用的信息，因此主要关注算法的最坏情况，亦即**最坏时间复杂度**。

### 3.常见时间复杂度

常见的时间复杂度包括 **O(1)、O(logn)、O(n)、O(nlogn)、O(n2)、O(2n)** 等。

#### 列表比较：

| 执行次数实例 | 复杂度    | 非正式术语 |
| ------------ | --------- | ---------- |
| 10           | O(1)      | 常数阶     |
| 3n+2         | O(n)      | 线性阶     |
| 2n2+3n+1     | O(n2)     | 平方阶     |
| 2log2n+10    | O(log2n)  | 对数阶     |
| 2nlog2n+2n+3 | O(nlog2n) | nlog2n阶   |
| 2n           | O(2n)     | 指数阶     |

#### 绘图说明：

![各种时间复杂度图形比较](https://img-blog.csdnimg.cn/20191106221730417.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
再分析：

```python
def cal(n):
    for i in range(n):
        for j in range(i):
            print(i,j)
1234
```

要计算1+2+3+…+(n-1)=n*(n-1)/2，所以也为**O(n2)**。

## 三、Python内置类型性能分析

**timeit**模块用来测试一段代码的执行时间：
三个参数：
**stmt, setup, timer**
即`class timeit.Timer(stmt='',setup='',timer='')`
**Timer**:测试小段代码执行时间的类;
**stmt**:要测试的代码语句;
**setup**:运行代码时需要的设置;
**timer**:定时器参数,与平台有关。

```python
#分析列表添加的执行效率
from timeit import Timer
def test1():
    l = []
    for i in range(1000):
        l = l + [i]

def test2():
    l = []
    for i in range(1000):
        #末尾追加
        l.append(i)

def test3():
    #列表推导式
    l = [i for i in range(1000)]

def test4():
    l = list(range(1000))

def test5():
    l = []
    for i in range(1000):
        #从头添加
        l.insert(0,i)

#函数要加引号
t1 = Timer('test1()','from __main__ import test1')
print('add',t1.timeit(number=1000))
t2 = Timer('test2()','from __main__ import test2')
print('append',t2.timeit(number=1000))
t3 = Timer('test3()','from __main__ import test3')
print('list derivation',t3.timeit(number=1000))
t4 = Timer('test4()','from __main__ import test4')
print('list range',t4.timeit(number=1000))
t5 = Timer('test5()','from __main__ import test5')
print('insert',t5.timeit(number=1000))
12345678910111213141516171819202122232425262728293031323334353637
```

打印

```python
add 1.223421629
append 0.06038822500000007
list derivation 0.030709197000000188
list range 0.012542454999999952
insert 0.2713445080000001
12345
```

易知，第一种方式最慢，`insert()`方法稍快，`append()`更快，其他的也较快。

## 四、数据结构的引入

数据存储方式的不同，会导致需要不同的算法进行处理，其执行效率也会不同。
例如：
在用Python中的类型来存储一个班的学生信息时，可以考虑通过列表和字典两种方式：
在通过学生姓名获取其信息时，如通过列表，则要遍历这个列表，其时间复杂度为O(n)(假设学生规模为n)；
而通过字典存储时，可以将学生姓名作为字典的键，学生信息作为值，进行查询时不需要遍历即可快速获取到学生信息，时间复杂度为O(1)。

这说明数据存储方式的不同的确会导致需要的算法不同，用算法解决问题的效率也会不同。
数据是一个抽象的概念，将其分类后得到程序设计语言的基本类型，如常见的int、float、char等。
数据元素之间不是独立的，存在特定的关系，这些关系便是结构。

定义：
**数据结构**指数据对象中数据元素之间的关系，即数据组织的方式。
Python中，数据结构分为：
内置数据结构：Python已经实现，不需要我们再去定义，如列表、元组、字典等；
扩展数据结构：Python并未定义，需要我们自己去定义实现这些数据的组织方式，如栈、队列等。

**程序 = 数据结构 + 算法**
总结：算法是为了解决实际问题而设计的，数据结构是算法需要处理问题的**载体**。

**抽象数据类型（ADT）**：
指一个数据类型以及定义在此数据模型上的一组操作，即把数据类型和数据类型上的运算捆绑在一起，进行封装。
引入抽象数据类型的目的是把数据类型的表示和数据类型上运算的实现与这些数据类型和运算在程序中的引用隔开，使它们相互独立。

# Python全栈（二）数据结构和算法之2.顺序表

## 引入：

几个问题：
1.列表的下标为什么从0开始；
2.为什么列表append比insert快；
3.列表append之后，id值为什么不变，即地址不变。

## 一、内存、类型和连续存储

内存：
单位：1字节=8位
有一个int值1，32位电脑中1个整型int占4个字节，1转换成8位为**0000 0001**，对应有一个内存地址。
有多个整型时，如7、21、39，在内存中分别存放，地址不是连续的，如0x01、ox09、0x17，
如果放在一起，如7为0x01，则21、39分别为0x05、0x09是连续存储的，
取39时，如果知道7的地址0x01，则39的位置为0x(01+2*4)=0x09。
类型决定了数据在计算机中占多大的存储单位，如一个int占4个字节。
如图：
![内存和存储](https://img-blog.csdnimg.cn/20191108164316519.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_1,color_FFFFFF,t_70)



## 二、顺序表的基本形式

### 1.基本顺序表

`li = [200,389,78,12，3]`
存储li时，会在内存申请5个连续的地址，存放这5个数，分别对应着5个连续的地址，此即**基本顺序表**。
**基本顺序表：**
元素连续存储，每个元素所占的存储单元大小固定相同，下标是逻辑地址。
`Loc(ei) = Loc(e0) + c*i`
其中，Loc(ei)是待确定元素地址，Loc(e0)是起始地址，i表示第i个元素，c表示存储单元大小。
li指向首个元素的地址。
基本顺序表存储的数据类型是一样的。

### 2.元素外置顺序表

列表存储的数据类型不一样时，如
`li = [12,'ab',1.11,1000]`
先保存12，有一个内存地址，再保存字符串，但是不是连续的，再保存浮点数，和后边的整型，都不是连续保存的，再将它们的**内存地址**连续保存在内存的其他区域，存储的地址也会占用4位，内存地址的地址是连续的，此即**元素外置顺序表**。
**元素外置顺序表：**
将实际数据元素另行存储，而顺序表中各单元位置保存对应元素的地址信息（即链接）。由于每个链接所需的存储量相同，通过上述公式，可以计算出元素链接的存储位置，而后顺着链接找到实际存储的数据元素。
这时c不再是数据元素的大小，而是存储一个链接地址所需的存储量，这个量通常很小。
外置顺序表也被称为对实际数据的索引，这是最简单的索引结构。
li指向首个元素地址的对应地址。
顺序表两种基本形式图示：
![基本顺序表和元素外置顺序表](https://img-blog.csdnimg.cn/20191108164636283.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
两种基本形式的简单存储方式示意：
![两种基本形式](https://img-blog.csdnimg.cn/20191108170238542.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
**解答1：**
**列表的下标为什么从0开始**：
下表最确切的定义是**偏移（offfset）**，a[k]表示偏移k个存储单元大小的位置。
如果下表从0开始，则a[k]的内存地址为：
`a[k]_address = base_address + k * type_size`
如果下表从1开始，则a[k]的内存地址会变为：
`a[k]_address = base_address + (k-1)*type_size`
对比可知，从1开始编号，每次都多进行了一次减法运算，CPU多进行了一次减法指令。数组作为非常基础的数据结构，通过下标随机访问数组元素又是其非常基础的编程操作，效率的优化就要尽可能做到极致。为了**减少一次减法运算**，数组从0开始编号，而不是从1开始编号。

## 三、顺序表的实现方式和实现原理

### 1.顺序表的结构

**表头信息+数据区**
即顺序表的完整信息包括两部分：

- 表中的数据集合
- 为实现正确操作而需记录的信息
  包括元素存储区的容量和当前表已有的元素个数。

**顺序表的两种基本实现形式**：

- 一体式结构
  存储表信息单元与元素存储区以连续的方式安排在一块存储区里，两部分数据的整体构成一个完整的顺序表对象。
  结构整体性强，易于管理，但顺序表创建后，元素存储区就固定了。
  表头也会有内存地址，且与元素地址是相连的。
- 分离式结构
  表对象里只保存与整个表有关的信息（即容量和元素个数），实际数据元素存放在另一个独立的元素存储区里，通过链接与基本表对象关联。
  表头除了存表头信息和数据区，还保存了首个元素的地址，用以指向数据区，数据区和表头信息的地址不是连续的。
  添加元素时，数据区会重新生成，但是表头地址不会变，只是改变存储的数量和表头所存的地址，列表指向表头不改变，所以改变列表不会改变内存地址。
  也说明列表等数据结构使用的是**分离式结构**。

如图：
![一体式和分离式结构](https://img-blog.csdnimg.cn/20191108164729616.png#pic_center)

### 2.元素存储区替换

一体式结构由于顺序表信息区与数据区连续存储在一起，所以若想更换数据区，则只能整体搬迁，即整个顺序表对象（指存储顺序表的结构信息的区域）改变了；
分离式结构若想更换数据区，只需将表信息区中的数据区链接地址更新即可，而该顺序表对象不变。

### 3.元素存储区扩充

采用一体式结构的顺序表，进行扩充时，如不存在空白元素区，不能直接在最后一个元素后添加，只能重新开一个内存区域，用来存储原来的元素和新增的元素，列表所指向的地址也随着数据区的改变而改变。
采用分离式结构的顺序表，若将数据区更换为存储空间更大的区域，则可以在不改变表对象的前提下对其数据存储区进行了扩充，所有使用这个表的地方都不必修改。只要程序的运行环境（计算机系统）还有空闲存储，这种表结构就不会因为满了而导致操作无法进行。人们把采用这种技术实现的顺序表称为**动态顺序表**，因为其容量可以在使用中动态变化。

两种实现方式扩充原理的对比：
![扩充方式对比](https://img-blog.csdnimg.cn/20191108170448554.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
两种扩充策略：
**线性增长**：
每次扩充增加固定数目的存储位置，如每次扩充增加10个元素位置。
特点：节省空间，但是扩充操作频繁，操作次数多。
**成倍增长**：
每次扩充容量加倍，如每次扩充增加一倍存储空间。
特点：减少了扩充操作的执行次数，但可能会浪费空间资源。**以空间换时间**，推荐的方式。

### 4.顺序表的实现

Python中的list和tuple两种类型采用了顺序表的实现技术。
tuple是不可变类型，即不变的顺序表，不支持改变的操作，因此采用**一体式**的结构。

**list的基本实现技术：**
list是一种元素个数可变的线性表，可以添加和删除元素，并在各种操作中维持已有元素的顺序（即保序）。
特征：
※基于下标（位置）的高效元素访问和更新，时间复杂度为O(1)。
list采用顺序表结构，表中元素保存在一块连续的存储区中。
※允许任意加入元素，而且在不断加入元素的过程中，表对象的标识（id值）不变。
采用**分离式**结构实现，是一种**动态顺序表**，能更换元素存储区，并且为保证更换存储区时list对象的标识id不变。

**解答2：**
**列表append之后，id值为什么不变，即地址不变**：
list是采用分离式结构实现的**动态顺序表**，表对象里只保存与整个表有关的信息（即容量和元素个数），实际数据元素存放在另一个独立的元素存储区里，append是对独立的元素存储区进行操作，不会改变表对象，也就不会改变列表对象的地址了。

## 四、顺序表的操作

### 1.增加元素

(1)表尾端加入元素append
时间复杂度：O(1)
(2)非保序的元素插入（不常见）insert
时间复杂度：O(1)
(3)**保序**的元素插入insert
时间复杂度：O(n)
保序：即插入元素，原先的顺序不变。
如图，
![增加元素](https://img-blog.csdnimg.cn/20191108164849947.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
**解答3：**
**为什么列表append比insert快**：
append时间复杂度为`O(1)`，insert（一般为保序）复杂度为`O(n)`，当然append更快。

### 2.删除元素

(1)表尾删除元素pop
时间复杂度：O(1)
(2)非保序的元素删除（不常见）remove
时间复杂度：O(1)
(3)保序的元素删除
时间复杂度：O(n)
如图，
![删除元素](https://img-blog.csdnimg.cn/20191108164916357.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 五、单向链表介绍和变量标识的本质

### 1.为什么需要链表

顺序表的构建需要预先知道数据大小来申请连续的存储空间，而在进行扩充时又需要进行数据的搬迁，所以使用起来并不是很灵活。
链表结构可以充分利用计算机内存空间，实现灵活的**动态内存管理**。

### 2.链表的定义

链表是一种常见的基础的数据结构，是一种**线性表**，不像顺序表一样连续存储数据，而是在每个节点（数据存储单元）里存放下一个节点的位置信息（即地址）。
分为**单链表、双向链表、循环链表**。
**线性表：顺序表+链表**。

### 3.单链表

单链表，即单向链表，是链表中最简单的形式，每个节点包括两个域，**元素域**和**链接域**。
链接指向链表中的下一个节点，顺序单一，最后一个节点的链接域指向空值。
**节点 = 数据区 + 链接区**
如图，
![单链表](https://img-blog.csdnimg.cn/20191108164955275.png#pic_center)
其中，
**表元素域elem**用来存放具体的数据；
**链接域next**用来存放下一个节点的位置（python中的标识）；
**变量p**指向链表的头节点（首节点）的位置，从p出发能找到表中的任意节点。
单链表存储的简单示意：
![单链表存储](https://img-blog.csdnimg.cn/2019110817073058.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 4.Python变量标识的本质

变量交换：

```python
#a的地址指向10
a = 10
#b的地址指向20
b = 20
#a指向20，b指向10
a,b = b,a
123456
```

变量标识的本质就是**指向变量所表示的对象的地址**，如例子中就是a、b交换了所指向的地址。

# Python全栈（二）数据结构和算法之3.链表的实现

## 一、节点的实现&链表的初始化

节点组成部分：**item next**

### 1.定义节点类

```python
class SingleNode(object):
    '''the node of single_link_list'''
    def __init__(self,item):
        '''初始化'''
        #item存放数据元素
        self.item = item
        #next是下一个节点的标识
        self.next = None
12345678
```

### 2.定义单链表并初始化

链表为空：
`__head == None`
链表不为空：
`__head = nodex`。

```python
class SingleLinkList(object):
    '''单链表'''
    def __init__(self,node = None):
        self.__head = node
1234
```

### 3.单链表的操作

`is_empty()` 链表是否为空
`length()` 链表长度
`travel()` 遍历整个链表
`append(item)` 链表尾部添加元素
`add(item)` 链表头部添加元素
`insert(pos, item)` 指定位置添加元素
`search(item)` 查找节点是否存在
`remove(item)` 删除节点

## 二、单链表中的遍历&长度&末尾添加

### 1.获取长度：

用游标遍历`cur = cur.next`，
最后指向None`cur == None`，即停止.
用count计数，即每遍历一个节点，`count += 1`.

```python
def length(self):
    '''链表长度'''
    #cur即游标，指向首节点，用来遍历
    cur = self.__head
    count = 0
    #当未到达尾部时
    while cur != None:
        count += 1
        #将游标向后移动一位
        cur = cur.next
    return count
1234567891011
```

### 2.遍历链表：

指标每次指向下一个节点
应该先打印出首节点，再移向下一个节点。

```python
def travel(self):
    '''遍历链表'''
    cur = self.__head
    while cur != None:
        print(cur.item,end=' ')
        cur = cur.next
123456
```

### 3.append尾部追加元素

先创建节点，再让游标向后遍历，直到`cur.next = node`
要先判断链表是否为空。

```python
def append(self,item):
    '''链表尾部添加元素'''
    #创建一个保存数据的节点
    node = SingleNode(item)
    #判断链表是否为空，若为空，则将__head指向新节点
    if self.is_empty():
        self.__head = node
    #若不为空，则找到尾部
    else:
        cur = self.__head
        while cur.next != None:
            #找到最后一个节点
            cur = cur.next
        #将尾节点的next指向新节点
        cur.next = node
123456789101112131415
```

## 三、单链表中的头部添加&指定位置添加

### 1.add头部添加元素

先将要添加的节点指向头节点，再将head指向要添加的节点。
![头部插入元素](https://img-blog.csdnimg.cn/20191110205607371.png#pic_center)

```python
def add(self,item):
    '''链表头部添加元素'''
    # 创建一个保存数据的节点
    node = SingleNode(item)
    #将新节点的链接区指向头节点
    node.next = self.__head
    #将链表的头节点指向新节点
    self.__head = node
12345678
```

### 2.insert指定位置添加元素

新节点指向当前节点的下一个节点`node.next = cur.next`，当前节点指向新节点`cur.next = node`
![指定位置插入元素](https://img-blog.csdnimg.cn/20191110205659787.png#pic_center)

```python
def insert(self,pos,item):
    '''指定位置添加元素'''
    #若指定位置为第一个位置之前，则指向头部插入
    if pos <= 0:
        self.add(item)
    #若指定位置超过链表尾部，则执行尾部插入
    elif pos > self.length() - 1:
        self.append(item)
    #找到指定位置
    else:
        node = SingleNode(item)
        # pre用来指向指定位置pos的前一个位置pos-1，初始从头节点开始移动到指定位置
        pre = self.__head
        count = 0
        while count < pos - 1:
            count += 1
            pre = pre.next
        #将新节点node的next指向插入位置的节点
        node.next = pre.next
        #将插入位置的前一个节点的next指向新节点
        pre.next = node
123456789101112131415161718192021
```

## 四、单链表的搜索和删除

### 1.查找

要遍历到最后一个节点后的None，如果没有找到，返回False。

```python
def search(self,item):
    '''查找节点是否存在'''
    cur = self.__head
    while cur != None:
        #判断第一个节点
        if cur.item == item:
            return True
        #未找到，继续向后移动
        else:
            cur = cur.next
    return False
1234567891011
```

### 2.remove删除指定元素

将被删除节点的上一个节点指向被删除节点的下一个节点，进行判断。
![删除指定元素](https://img-blog.csdnimg.cn/2019111020580962.png#pic_center)

```python
def remove(self,item):
    '''删除节点'''
    cur = self.__head
    pre = None
    while cur != None:
        #找到指定元素
        if cur.item == item:
            #如果第一个就是删除的节点，将头指针指向头结点的后一个节点
            if cur == self.__head:
                self.__head = cur.next
            #否则，将删除位置前一个节点的next指向删除位置的后一个节点
            else:
                pre.next = cur.next
            break
        #未找到，继续移动
        else:
            pre = cur
            cur = cur.next
123456789101112131415161718
```

### 3.完整实现

整个节点和单链表的完整实现和测试代码为：

```python
#单链表
#节点的实现
class SingleNode(object):
    '''the node of single_link_list'''
    def __init__(self,item):
        '''item存放数据元素'''
        self.item = item
        '''next是下一个节点的标识'''
        self.next = None

class SingleLinkList(object):
    '''单链表'''
    def __init__(self,node = None):
        self.__head = node
    
    def is_empty(self):
        '''判断是否为空'''
        return self.__head == None

    def length(self):
        '''链表长度'''
        #cur即游标，指向首节点，用来遍历
        cur = self.__head
        count = 0
        #当未到达尾部时
        while cur != None:
            count += 1
            #将游标向后移动一位
            cur = cur.next
        return count


    def travel(self):
        '''遍历链表'''
        cur = self.__head
        while cur != None:
            print(cur.item,end=' ')
            cur = cur.next

    def append(self,item):
        '''链表尾部添加元素'''
        #创建一个保存数据的节点
        node = SingleNode(item)
        #判断链表是否为空，若为空，则将__head指向新节点
        if self.is_empty():
            self.__head = node
        #若不为空，则找到尾部
        else:
            cur = self.__head
            while cur.next != None:
                #找到最后一个节点
                cur = cur.next
            #将尾节点的next指向新节点
            cur.next = node

    def add(self,item):
        '''链表头部添加元素'''
        # 创建一个保存数据的节点
        node = SingleNode(item)
        #将新节点的链接区指向头节点
        node.next = self.__head
        #将链表的头节点指向新节点
        self.__head = node

    def insert(self,pos,item):
        '''指定位置添加元素'''
        #若指定位置为第一个位置之前，则指向头部插入
        if pos <= 0:
            self.add(item)
        #若指定位置超过链表尾部，则执行尾部插入
        elif pos > self.length() - 1:
            self.append(item)
        #找到指定位置
        else:
            node = SingleNode(item)
            # pre用来指向指定位置pos的前一个位置pos-1，初始从头节点开始移动到指定位置
            pre = self.__head
            count = 0
            while count < pos - 1:
                count += 1
                pre = pre.next
            #将新节点node的next指向插入位置的节点
            node.next = pre.next
            #将插入位置的前一个节点的next指向新节点
            pre.next = node

    def remove(self,item):
        '''删除节点'''
        cur = self.__head
        pre = None
        while cur != None:
            #找到指定元素
            if cur.item == item:
                #如果第一个就是删除的节点，将头指针指向头结点的后一个节点
                if cur == self.__head:
                    self.__head = cur.next
                #否则，将删除位置前一个节点的next指向删除位置的后一个节点
                else:
                    pre.next = cur.next
                break
            #未找到，继续移动
            else:
                pre = cur
                cur = cur.next

    def search(self,item):
        '''查找节点是否存在'''
        cur = self.__head
        while cur != None:
            #判断第一个节点
            if cur.item == item:
                return True
            #未找到，继续向后移动
            else:
                cur = cur.next
        return False

#测试
#定义一个值为100的节点
node = SingleNode(100)
s = SingleLinkList()
s.append(1)
s.append(2)
s.append(3)
s.append(4)
s.append(5)
s.add(6)
s.insert(-1,7)
s.travel()
s.insert(3,8)
s.insert(10,9)
s.remove(7)
s.remove(3)
s.remove(9)
s.travel()
print(s.is_empty())
print(s.length())
s.travel()
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138
```

运行结果为：

```python
7 6 1 2 3 4 5 6 1 8 2 4 5 False
6
6 1 8 2 4 5 
123
```

## 五、链表与顺序表的对比

链表没有顺序表随机读取的优点，同时增加了节点的指针域，增大了空间开销，但是对内存的使用相对更灵活。

| 操作            | 链表 | 顺序表 |
| --------------- | ---- | ------ |
| 访问元素        | O(n) | O(1)   |
| 头部插入/删除   | O(1) | O(n)   |
| 尾部插入/删除   | O(n) | O(1)   |
| 在中间插入/插入 | O(n) | O(1)   |

链表的主要耗时操作时遍历查找，删除和插入操作本身时间复杂度很小，为**O(1)**；
顺序表查找很快，主要耗时的操作是拷贝覆盖。

### 写单链表的思考、启发和建议：

1. 考虑各种特殊情况，如为空链表时插入、插入的位置为负；
2. 对于遍历最后的判断条件，根据不同的操作要求分别判断，如`cur != None`和`cur.next != None`等，还要注意其他边界条件；
3. 引入有助于理解和实现的辅助工具，如`cur`和`pre`等，理解其意义有助于更好地实现；
4. 可以举例画图，有助于深入思考和理解。

### 利用链表实现LRU缓存算法

缓存是一种提高数据读取性能的技术，在硬件设计、软件开发中都有着非常广泛的应用，如CPU缓存、数据库缓存、浏览器缓存等等，Redis、MongoCache等等。
缓存的大小有限，当缓存被用满时，哪些数据应该被清理出去，哪些数据应该被保留？这就需要缓存淘汰策略来决定。常见的策略有三种：先进先出策略FIFO（First In，First Out）、最少使用策略LFU（Least Frequently Used）、最近最少使用策略LRU（Least Recently Used）。
实现的简单思路：
维护一个有序单链表，越靠近链表尾部的结点是越早之前访问的。当有一个新的数据被访问时，我们从链表头开始顺序遍历链表：
1.如果此数据之前已经被缓存在链表中了，我们遍历得到这个数据对应的结点，并将其从原来的位置删除，然后再插入到链表的头部。
2.如果此数据没有在缓存链表中，又可以分为两种情况：
如果此时缓存未满，则将此结点直接插入到链表的头部；
如果此时缓存已满，则链表尾结点删除，将新的数据结点插入链表的头部。

# Python全栈（二）数据结构和算法之4.单向循环链表的实现

## 一、单项循环链表长度&判空实现

### 1.循环链表定义

是一种特殊的单链表，唯一的区别是：
单链表的尾结点指针指向空地址，表示这就是最后的结点了；
循环链表的尾结点指针是指向链表的头结点。

### 2.is_empty()

与单链表一致。

```python
def is_empty(self):
    '''判断是否为空'''
    return self.__head == None
123
```

### 3.length()

`cur.next = self.__head`时循环结束，且因最后一次循环不会执行，所以count不会加1，所以count要哦才能够1开始计数。
并且为空时，直接返回0。

```python
def length(self):
    '''链表长度'''
    #判断是否为空链表
    if self.is_empty():
        return 0
    # cur即游标，指向首节点，用来遍历
    cur = self.__head
    count = 1
    # 当未到达尾部时
    while cur.next != self.__head:
        count += 1
        # 将游标向后移动一位
        cur = cur.next
    return count
1234567891011121314
```

## 二、单项循环链表遍历、头插法&尾插法

### 1.遍历travel()

`cur.next = self.__head`时循环结束。
空链表时直接返回。

```python
def travel(self):
    '''遍历链表'''
    #链表为空时
    if self.is_empty():
        return
    cur = self.__head
    while cur.next != self.__head:
        print(cur.item, end=' ')
        cur = cur.next
    #退出循环时，cur指向尾节点，单独打印
    print(cur.item)
1234567891011
```

### 2.头部添加add()：

将新节点指向head的下一个节点，head指向新节点，尾节点指向新节点，第三步顺序可以在第一或第二步。
为空链表时，__head指向新节点，新节点指向自己（即__head）。

```python
def add(self, item):
    '''链表尾部添加元素'''
    # 创建一个保存数据的节点
    node = Node(item)
    # 判断链表是否为空，若为空，则将__head指向新节点，新节点指向头部
    if self.is_empty():
        self.__head = node
        node.next = self.__head #等价于node.next = node
    #若不为空
    else:
        cur = self.__head
        #找到尾部
        while cur.next != self.__head:
            cur = cur.next
        #新节点指向__head
        node.next = self.__head
        #__head指向新节点
        self.__head = node
        # 将尾节点指向新节点，此句可以在self.__head = node或node.next = self.__head之前
        cur.next = node
1234567891011121314151617181920
```

### 3.append()尾部添加

找到尾节点：
尾节点指向新节点，新节点指向头部。
也要判断是否为空链表。

```python
def append(self, item):
    '''链表尾部添加元素'''
    # 创建一个保存数据的节点
    node = Node(item)
    #判断是否为空链表
    if self.is_empty():
        self.__head = node
        node.next = self.__head  # 等价于node.next = node
    else:
        cur = self.__head
        # 找到尾节点
        while cur.next != self.__head:
            cur = cur.next
        # 尾节点指向新节点
        cur.next = node
        #新节点指向__head
        node.next = self.__head
1234567891011121314151617
```

## 三、单项循环链表删除&搜索

### 1.insert()

新节点指向目标位置，目标位置前一个位置指向新节点。

```python
def insert(self, pos, item):
    '''指定位置添加元素'''
    # 若指定位置为第一个位置之前，则执行头部插入
    if pos <= 0:
        self.add(item)
    # 若指定位置超过链表尾部，则执行尾部插入
    elif pos > self.length() - 1:
        self.append(item)
    # 找到指定位置
    else:
        node = Node(item)
        cur = self.__head
        count = 0
        while count < pos - 1:
            count += 1
            cur = cur.next
        # 将新节点node的next指向插入位置的节点
        node.next = cur.next
        # 将插入位置的前一个节点的next指向新节点
        cur.next = node
1234567891011121314151617181920
```

### 2.search()

如果为空，直接返回False。
并且在循环后判断尾节点是否符合。

```python
def search(self, item):
    '''查找节点是否存在'''
    if self.is_empty():
        return False
    cur = self.__head
    while cur.next != self.__head:
        # 判断第一个节点
        if cur.item == item:
            return True
        # 未找到，继续向后移动
        else:
            cur = cur.next
    #循环退出，cur指向尾节点
    if cur.item == item:
        return True
    return False
12345678910111213141516
```

### 3.remove()

如果删除的节点为头节点：
如果不止头节点一个节点，__head指向当前的下一个节点；
如果只有head一个节点，则__head指向None。
如果删除的节点不是头节点：
`pre.next = cur.next`，并且需要循环，同时要对尾节点进行判断。

```python
def remove(self, item):
    '''删除节点'''
    #如果链表为空，直接返回
    if self.is_empty():
        return
    cur = self.__head
    pre = None
    #头节点的元素就是要删除的元素
    if cur.item == item:
        #链表中的节点不止一个
        if cur.next != self.__head:
            while cur.next != self.__head:
                cur = cur.next
            #循环结束，cur指向尾节点
            cur.next = self.__head.next
            self.__head = cur.next
        #只有一个节点
        else:
            self.__head = None
    #头节点不是要删除的元素
    else:
        while cur.next != self.__head:
            if cur.item == item:
                #删除
                pre.next = cur.next
                return
            #继续往后遍历
            else:
                pre = cur
                cur = cur.next
        #要删除的节点是最后一个元素时
        if cur.item == item:
            pre.next = cur.next #pre.next = self.__head也可
123456789101112131415161718192021222324252627282930313233
```

整个单项循环链表的实现如下：

```python
class Node(object):
    '''节点'''
    def __init__(self,item):
        '''初始化'''
        self.item = item
        #next是下一个节点的标识
        self.next = None

class SinCycLinkList(object):
    '''循环链表'''
    def __init__(self,node = None):
        self.__head = None
        if node:
            node.next = node

    def is_empty(self):
        '''判断是否为空'''
        return self.__head == None

    def length(self):
        '''链表长度'''
        #判断是否为空链表
        if self.is_empty():
            return 0
        # cur即游标，指向首节点，用来遍历
        cur = self.__head
        count = 1
        # 当未到达尾部时
        while cur.next != self.__head:
            count += 1
            # 将游标向后移动一位
            cur = cur.next
        return count

    def travel(self):
        '''遍历链表'''
        #链表为空时
        if self.is_empty():
            return
        cur = self.__head
        while cur.next != self.__head:
            print(cur.item, end=' ')
            cur = cur.next
        #退出循环时，cur指向尾节点，单独打印
        print(cur.item)

    def add(self, item):
        '''链表头部添加元素'''
        # 创建一个保存数据的节点
        node = Node(item)
        # 判断链表是否为空，若为空，则将__head指向新节点，新节点指向头部
        if self.is_empty():
            self.__head = node
            node.next = self.__head #等价于node.next = node
        #若不为空
        else:
            cur = self.__head
            #找到尾部
            while cur.next != self.__head:
                cur = cur.next
            #新节点指向__head
            node.next = self.__head
            #__head指向新节点
            self.__head = node
            # 将尾节点指向新节点，此句可以在self.__head = node或node.next = self.__head之前
            cur.next = node


    def append(self, item):
        '''链表尾部添加元素'''
        # 创建一个保存数据的节点
        node = Node(item)
        #判断是否为空链表
        if self.is_empty():
            self.__head = node
            node.next = self.__head  # 等价于node.next = node
        else:
            cur = self.__head
            # 找到尾节点
            while cur.next != self.__head:
                cur = cur.next
            # 尾节点指向新节点
            cur.next = node
            #新节点指向__head
            node.next = self.__head

    def insert(self, pos, item):
        '''指定位置添加元素'''
        # 若指定位置为第一个位置之前，则指向头部插入
        if pos <= 0:
            self.add(item)
        # 若指定位置超过链表尾部，则执行尾部插入
        elif pos > self.length() - 1:
            self.append(item)
        # 找到指定位置
        else:
            node = Node(item)
            cur = self.__head
            count = 0
            while count < pos - 1:
                count += 1
                cur = cur.next
            # 将新节点node的next指向插入位置的节点
            node.next = cur.next
            # 将插入位置的前一个节点的next指向新节点
            cur.next = node

    def remove(self, item):
        '''删除节点'''
        #如果链表为空，直接返回
        if self.is_empty():
            return
        cur = self.__head
        pre = None
        #头节点的元素就是要删除的元素
        if cur.item == item:
            #链表中的节点不止一个
            if cur.next != self.__head:
                while cur.next != self.__head:
                    cur = cur.next
                #循环结束，cur指向尾节点
                cur.next = self.__head.next
                self.__head = cur.next
            #只有一个节点
            else:
                self.__head = None
        #头节点不是要删除的元素
        else:
            while cur.next != self.__head:
                if cur.item == item:
                    #删除
                    pre.next = cur.next
                    return
                #继续往后遍历
                else:
                    pre = cur
                    cur = cur.next
            #要删除的节点是最后一个元素时
            if cur.item == item:
                pre.next = cur.next #pre.next = self.__head也可

    def search(self, item):
        '''查找节点是否存在'''
        if self.is_empty():
            return False
        cur = self.__head
        while cur.next != self.__head:
            # 判断第一个节点
            if cur.item == item:
                return True
            # 未找到，继续向后移动
            else:
                cur = cur.next
        #循环退出，cur指向尾节点
        if cur.item == item:
            return True
        return False

s = SinCycLinkList()
print(s.is_empty())
print(s.length())
s.add(1)
s.add(2)
s.add(3)
s.add(4)
s.travel()
s.append(5)
s.append(6)
s.insert(-1,7)
s.insert(12,8)
s.insert(5,9)
s.travel()
print(s.search(3))
print(s.search(10))
s.remove(6)
s.travel()
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176
```

对单项循环链表进行测试，结果如下：

```python
True
0
4 3 2 1
7 4 3 2 1 9 5 6 8
True
False
7 4 3 2 1 9 5 8
1234567
```

### 小结：

单项循环链表的实现要结合单链表，要对它们的相同和不同之处进行比较。
很显然，单向循环链表实现的时候要考虑的情况比单链表更多，也更复杂，要在对比中学习，举一反三。
并且在每一个方法中`cur.next != self.__head`这个循环判断条件都出现了，所以找到实现的边界条件极为重要。
同时，要考虑各种特殊情况，比如链表为空，只有一个节点，对尾节点的处理等情况。

# Python全栈（二）数据结构和算法之5.双向链表的实现和栈的实现

## 一、双向链表的定义、判断是否为空、长度&遍历

### 1.定义

单向链表只有一个方向，每个节点只有一个后继节点next指向下一个节点；
双向链表支持两个方向，每个节点不止有一个后继指针next指向后面的节点，还有一个前驱指针prev指向前面的节点。
双向链表占内存更大，`用空间换时间`。
当前节点的前一个节点称为前驱结点，后一个节点称为后继节点。
双向链表的示意图
![双向链表的示意图](https://img-blog.csdnimg.cn/20191113201617808.png)

### 2.is_empty():

```python
def is_empty(self):
    '''判断是否为空'''
    return self.__head == None
123
```

### 3.length():

count = 0开始计时，循环条件和单链表一样，即`cur != None`。

```python
def length(self):
    '''链表长度'''
    #判断是否为空链表
    if self.is_empty():
        return 0
    # cur即游标，指向首节点，用来遍历
    cur = self.__head
    count = 0
    # 当未到达尾部时
    while cur != None:
        count += 1
        # 将游标向后移动一位
        cur = cur.next
    return count
1234567891011121314
```

### 4.travel():

循环条件是`cur != None`。

```python
def travel(self):
    '''遍历链表'''
    #链表为空时
    if self.is_empty():
        return
    cur = self.__head
    while cur != None:
        print(cur.item, end=' ')
        cur = cur.next
123456789
```

## 二、双向链表的插入

### 1.add():

新节点的next指向首节点`node.next = self.__head`；
原来头节点指向新节点（实现双向）`self.__head.prev = node`；
__head指向新节点`self.__head = node`。

```python
def add(self, item):
    '''链表头部添加元素'''
    # 创建一个保存数据的节点
    node = Node(item)
    # 判断链表是否为空，若为空，则将__head指向新节点，新节点指向头部
    if self.is_empty():
        self.__head = node
    #若不为空
    else:
        #将node的next指向__head的头节点
        node.next = self.__head
        #将__head的prev指向node
        self.__head.prev = node
        #将__haed指向node
        self.__head = node
123456789101112131415
```

### 2.append()

遍历找到尾节点
循环条件：`while cur.next != None`
尾节点指向新节点：`cur.next = node`
新节点的prev指向cur：`node.prev = cur`。

```python
def append(self, item):
    '''链表尾部添加元素'''
    # 创建一个保存数据的节点
    node = Node(item)
    #判断是否为空链表
    if self.is_empty():
        self.__head = node
        node.next = self.__head  # 等价于node.next = node
    else:
        cur = self.__head
        # 找到尾节点
        while cur.next != None:
            cur = cur.next
        # 尾节点指向新节点
        cur.next = node
        #新节点的prev指向cur
        node.prev = cur
1234567891011121314151617
```

### 3.insert()

双向链表不需要pre指针。
将node的prev指向cur：`node.prev = cur`；
将node的nextcur的下一个节点：`node.next = cur.next`；
将cur的下一个节点的prev指向node：`cur.next.prev = node`；
将cur的下一个节点指向node：`cur.next = node`。

```python
def insert(self, pos, item):
    '''指定位置添加元素'''
    # 若指定位置为第一个位置之前，则指向头部插入
    if pos <= 0:
        self.add(item)
    # 若指定位置超过链表尾部，则执行尾部插入
    elif pos > self.length() - 1:
        self.append(item)
    # 找到指定位置
    else:
        node = Node(item)
        cur = self.__head
        count = 0
        while count < pos - 1:
            count += 1
            cur = cur.next
        #循环结束，找到插入位置的前一个节点。
        #将node的prev指向cur
        node.prev = cur
        #将node的nextcur的下一个节点（与前一步顺序可换）
        node.next = cur.next
        #将cur的下一个节点的prev指向node
        cur.next.prev = node
        #将cur的下一个节点指向node
        cur.next = node
12345678910111213141516171819202122232425
```

插入操作
![插入操作](https://img-blog.csdnimg.cn/20191113205055450.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 三、双向链表的查找、删除

### 1.查找

```python
def search(self, item):
    '''查找节点是否存在'''
    if self.is_empty():
        return False
    cur = self.__head
    while cur != None:
        # 判断第一个节点
        if cur.item == item:
            return True
        # 未找到，继续向后移动
        else:
            cur = cur.next
    return False
12345678910111213
```

### 2.删除

如果首结点的元素就是要删除的元素：
链表中只有一个节点：`self.__head = None`
链表中有多个节点：`cur.next.prev = None`,`self.__head = cur.next`。
如果首结点的元素不是要删除的元素：`cur.next.prev = cur.prev`，`cur.prev.next = cur.next`；

```python
def remove(self, item):
    '''删除节点'''
    #如果链表为空，直接返回
    if self.is_empty():
        return
    else:
        cur = self.__head
        pre = None
        #头节点的元素就是要删除的元素
        if cur.item == item:
            #链表中只有一个节点
            if cur == None:
                self.__head = None
            #有多个节点
            else:
                cur.next.prev = None
                self.__head = cur.next
            return
        #头节点不是要删除的元素
        while cur != None:
            if cur.item == item:
                #删除
                if cur.next:
                    cur.next.prev = cur.prev
                cur.prev.next = cur.next
                break
            cur = cur.next
123456789101112131415161718192021222324252627
```

删除操作
![删除操作](https://img-blog.csdnimg.cn/20191113205933612.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
整个双向链表的实现如下，

```python
class Node(object):
    '''节点'''
    def __init__(self,item):
        '''初始化'''
        self.item = item
        #next是下一个节点的标识
        self.next = None
        #prev是上一个节点的标识
        self.prev = None

class DoubleLinkList(object):
    '''双向链表'''
    def __init__(self,node = None):
        self.__head = node

    def is_empty(self):
        '''判断是否为空'''
        return self.__head == None

    def length(self):
        '''链表长度'''
        #判断是否为空链表
        if self.is_empty():
            return 0
        # cur即游标，指向首节点，用来遍历
        cur = self.__head
        count = 0
        # 当未到达尾部时
        while cur != None:
            count += 1
            # 将游标向后移动一位
            cur = cur.next
        return count

    def travel(self):
        '''遍历链表'''
        #链表为空时
        if self.is_empty():
            return
        cur = self.__head
        while cur != None:
            print(cur.item, end=' ')
            cur = cur.next

    def add(self, item):
        '''链表头部添加元素'''
        # 创建一个保存数据的节点
        node = Node(item)
        # 判断链表是否为空，若为空，则将__head指向新节点，新节点指向头部
        if self.is_empty():
            self.__head = node
        #若不为空
        else:
            #将node的next指向__head的头节点
            node.next = self.__head
            #将__head的prev指向node
            self.__head.prev = node
            #将__haed指向node
            self.__head = node


    def append(self, item):
        '''链表尾部添加元素'''
        # 创建一个保存数据的节点
        node = Node(item)
        #判断是否为空链表
        if self.is_empty():
            self.__head = node
            node.next = self.__head  # 等价于node.next = node
        else:
            cur = self.__head
            # 找到尾节点
            while cur.next != None:
                cur = cur.next
            # 尾节点指向新节点
            cur.next = node
            #新节点的prev指向cur
            node.prev = cur

    def insert(self, pos, item):
        '''指定位置添加元素'''
        # 若指定位置为第一个位置之前，则指向头部插入
        if pos <= 0:
            self.add(item)
        # 若指定位置超过链表尾部，则执行尾部插入
        elif pos > self.length() - 1:
            self.append(item)
        # 找到指定位置
        else:
            node = Node(item)
            cur = self.__head
            count = 0
            while count < pos - 1:
                count += 1
                cur = cur.next
            #循环结束，找到插入位置的前一个节点。
            #将node的prev指向cur
            node.prev = cur
            #将node的nextcur的下一个节点（与前一步顺序可换）
            node.next = cur.next
            #将cur的下一个节点的prev指向node
            cur.next.prev = node
            #将cur的下一个节点指向node
            cur.next = node

    def remove(self, item):
        '''删除节点'''
        #如果链表为空，直接返回
        if self.is_empty():
            return
        else:
            cur = self.__head
            pre = None
            #头节点的元素就是要删除的元素
            if cur.item == item:
                #链表中只有一个节点
                if cur == None:
                    self.__head = None
                #有多个节点
                else:
                    cur.next.prev = None
                    self.__head = cur.next
                return
            #头节点不是要删除的元素
            while cur != None:
                if cur.item == item:
                    #删除
                    if cur.next:
                        cur.next.prev = cur.prev
                    cur.prev.next = cur.next
                    break
                cur = cur.next

    def search(self, item):
        '''查找节点是否存在'''
        if self.is_empty():
            return False
        cur = self.__head
        while cur != None:
            # 判断第一个节点
            if cur.item == item:
                return True
            # 未找到，继续向后移动
            else:
                cur = cur.next
        return False

if __name__ == '__main__':
    d = DoubleLinkList()
    print(d.is_empty())
    print(d.length())
    d.travel()
    d.add(1)
    d.add(2)
    d.add(3)
    d.append(4)
    d.append(5)
    d.append(6)
    d.travel()
    d.insert(-1,7)
    d.insert(3,8)
    d.insert(10,9)
    d.travel()
    print(d.search(3))
    print(d.search(10))
    d.remove(2)
    d.remove(100)
    d.travel()
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168
```

进行测试，结果如下，

```python
True
0
3 2 1 3 2 1 4 5 6 7 3 2 8 1 4 5 6 9 True
False
7 3 8 1 4 5 6 9 
12345
```

## 四、栈的实现

### 1.定义

栈：后进先出，先进后出，只能在栈顶入或者出。
栈是一种“操作受限”的线性表，只允许在一端插入和删除数据，在满足先进后出、后进先出的特性时，应该使用栈。
如图，
![栈的示意](https://img-blog.csdnimg.cn/20191114205504950.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 2.实现

用顺序表实现的栈成为顺序栈，用链表实现的栈叫做链式栈。
顺序栈的实现相对简单，因为对于栈的操作可以通过对顺序表的操作方法实现，不用自己定义，直接调用即可，相对简单。
整个栈的实现如下，

```python
class Stack(object):
    '''栈，用列表的方法实现'''
    def __init__(self):
        self.__items = []

    def is_empty(self):
        '''判断栈是否为空'''
        return self.__items == []

    def push(self,item):
        '''添加一个新的元素item到栈顶'''
        self.__items.append(item)

    def pop(self):
        '''弹出栈顶元素'''
        return self.__items.pop()

    def peek(self):
        '''返回栈顶元素'''
        #判断是否为空
        if self.is_empty():
            return None
        else:
            return self.__items[-1]

    def size(self):
        '''返回栈的元素个数'''
        return len(self.__items)

    def travel(self):
        '''遍历'''
        for item in self.__items:
            print(item)

if __name__ == '__main__':
    s = Stack()
    s.push(1)
    s.push(2)
    s.push(3)
    s.pop()
    print(s.size())
    print(s.is_empty())
    print(s.peek())
    s.travel()
1234567891011121314151617181920212223242526272829303132333435363738394041424344
```

并进行测试，结果如下，

```python
2
False
2
1
2
12345
```

### 3.简单应用：

浏览器的前进后退：
当你依次访问完一串页面a-b-c之后，点击浏览器的后退按钮，就可以查看之前浏览过的页面b和a。当你后退到页面a，点击前进按钮，就可以重新查看页面b和c。但是，如果你后退到页面b后，点击了新的页面d，那就无法再通过前进、后退功能查看页面c了。
如图，
![浏览器的前进后退](https://img-blog.csdnimg.cn/20191113220023474.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

# Python全栈（二）数据结构和算法之6.队列的实现和冒泡排序的实现

## 一、队列和队列的实现

### 1.定义

特点：先进者先出。
操作：
入队`enqueue`：放一个数据到队列尾部；
出队`dequeue`：从队列头部取一个元素。
队列也是一种操作受限制的线性表数据结构。
包括一些额外特性的队列，比如循环队列、阻塞队列、并发队列等，在很多偏底层系统、框架、中间件的开发中起着关键作用。

### 2.队列的实现

队列的实现也是用顺序表list来实现，相关操作与list的操作相关，实现如下，

```python
class Queue(object):
    def __init__(self):
        '''初始化'''
        self.__items = []

    def enqueue(self,item):
        '''
        :param item:向队列中添加一个item元素
        :return: None
        '''
        self.__items.append(item)

    def dequeue(self):
        '''
        :param item
        :return:第一个元素
        '''
        self.__items.pop(0)

    def is_empty(self):
        '''
        判断一个队列是否为空
        :return: True or False
        '''
        return self.__items == []

    def size(self):
        '''
        :return:队列中的元素个数
        '''
        return len(self.__items)

    def travel(self):
        '''
        遍历
        :return:
        '''
        for item in self.__items:
            print(item)

q = Queue()
print(q.is_empty())
q.enqueue(1)
q.enqueue(2)
q.enqueue(3)
q.travel()
q.dequeue()
q.dequeue()
q.travel()
print(q.size())
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
```

并进行测试，打印结果为，

```python
1
2
3
3
1
12345
```

### 3.双端队列及其实现

全名**double-ended queue**，一种具有队列和栈的性质的数据结构。
结构如图，
![双端队列示意](https://img-blog.csdnimg.cn/20191117214933932.png)
元素可以从两端弹出，其限定插入和删除操作在队列的两端进行，可以在队列任意一端入队和出队。

实现与队列类似，

```python
class Deque(object):
    '''双端队列'''
    def __init__(self):
        self.__items = []

    def is_empty(self):
        '''判断双端队列是否为空'''
        return self.__items == []

    def size(self):
        '''
        返回队列中的元素个数
        :return:
        '''
        return len(self.__items)

    def add_front(self,item):
        '''从队头添加一个item元素'''
        self.__items.insert(0,item)

    def add_rear(self,item):
        '''从队尾添加一个元素'''
        self.__items.append(item)

    def remove_front(self):
        '''从队尾删除一个元素'''
        return self.__items.pop(0)

    def remove_rear(self):
        '''从队尾删除一个元素'''
        return self.__items.pop()

    def travel(self):
        '''
        遍历
        :return:
        '''
        for item in self.__items:
            print(item)

if __name__ == '__main__':
    d = Deque()
    print(d.is_empty())
    d.add_front(1)
    d.add_rear(2)
    d.travel()
    d.remove_front()
    print(d.remove_rear())
    print(d.size())
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849
```

进行测试，打印结果为，

```python
True
1
2
2
0
12345
```

### 4.阻塞队列：

input()函数调用时等待输入，即为阻塞。
阻塞队列在队列基础上增加了阻塞操作：
在队列为空的的时候，从队头取数据会被阻塞，因为此时还没有数据可取，直到队列中有了数据才能返回；
如果队列已经满了，那么插入数据的操作就会被阻塞，直到队列中有空闲位置后再插入数据，然后再返回。
应用：CPU资源分配：
CPU资源是有限的，任务的处理速度与线程个数并不是线性正相关。相反，过多的线程反而会导致CPU频繁切换，处理性能下降。所以，线程池的大小一般都是综合考虑要处理任务的特点和硬件环境，来事先设置的。
如果采用链表：无限排队的无界队列 响应时间，比较敏感不合适；
如果采用顺序表：有界的队列 如果超过 直接拒绝 响应时间 敏感比较合适。

## 二、排序算法的分析

### 1.排序算法简介

最经典的排序算法：
**冒泡排序
插入排序
选择排序
归并排序
快速排序
希尔排序**

分析排序算法的角度：
※算法的执行效率：
▲时间复杂度；
▲比较次数和交换（移动）次数。
※排序算法的内存消耗：
原地排序算法：特指时间复杂度为O(1)的排序算法。
※排序算法的稳定性：
稳定性是指，如果待排序的序列中存在值相等的元素，经过排序之后，相等元素之间原有的先后顺序不变。

### 2.递归回顾

递归是一种算法，很对数据结构和算法的实现都要用到算法，如DFS深度优先搜索、前中后序二叉树遍历等。
递归的三个条件：

1. 一个问题的解可以分成几个子问题的解；
2. 这个问题与分解之后的子问题，除了数据规模，求解思路完全一样；
3. 存在递归终止条件。
   在Python中，递归最大深度为1000，超过1000会抛出异常。
   简单举例：

```python
def f(n):
    if n == 1:
        return 1
    else:
        return f(n - 1) + 1

print(f(5))
1234567
```

打印`5`
再如

```python
def factorial(n):
    '''阶乘'''
    if n == 1:
        return 1
    else:
        return factorial(n - 1) * n

print(factorial(20))
12345678
```

打印`2432902008176640000`。
用递归的方式输出l=[‘jack’,(‘tom’,23),27,‘rose’,(14,55,67)]列表中内的每一个元素输出：

```python
def list_p(l):
    if isinstance(l,(str,int)):
        print(l)
    else:
        for item in l:
            list_p(item)

l=['jack',('tom',23),27,'rose',(14,55,67)]
list_p(l)
123456789
```

会打印

```python
jack
tom
23
27
rose
14
55
67
12345678
```

### 3.冒泡排序：

冒泡排序（Bubble Sort），重复地遍历要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来。遍历数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。
这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端。
实现思路：

- 比较相邻的元素，如果第一个比第二个大（升序），就交换他们两个；
- 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数；
- 针对所有的元素重复以上的步骤，除了最后一个；
- 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。
  用直观的图形分析如下，
  ![冒泡原理](https://img-blog.csdnimg.cn/20191117212925256.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
  进行一次循环后，会对除了最后一个元素的其他元素进行第二次循环，再比较，直到完全有序。
  用更形象的动图分析，
  ![冒泡排序动态分析](https://img-blog.csdnimg.cn/20191117212842747.gif)

#### 实现：

**方法一：**

```python
def bubble_sort_1(alist):
    '''用循环控制，冒泡排序'''
    for o in range(len(alist)-1,0,-1):
        for i in range(o):
            if alist[i] > alist[i+1]:
                alist[i],alist[i+1]=alist[i+1],alist[i]

if __name__ == '__main__':
    li = [54, 26, 93, 17, 77, 31, 44, 55, 20]
    bubble_sort_1(li)
    print(li)
1234567891011
```

打印`[17, 20, 26, 31, 44, 54, 55, 77, 93]`。
**方法二：**

```python
def bubble_sort_2(alist):
    n = len(alist)
    #外层循环控制循环的次数
    for o in range(len(alist)-1):
        #内层循环为当前循环，i为所需比较的次数
        for i in range(n - 1 - o):
            if alist[i] > alist[i+1]:
                alist[i],alist[i+1]=alist[i+1],alist[i]

if __name__ == '__main__':
    li = [54, 26, 93, 17, 77, 31, 44, 55, 20]
    bubble_sort_2(li)
    print(li)
12345678910111213
```

结果与方法一相同。
**方法三：用递归实现**

```python
def bubble_sort_3(alist):
    '''递归实现'''
    n = len(alist)
    flag = True
    for i in range(n - 1):
        if alist[i] > alist[i + 1]:
            flag = False
            break
        else:
            continue
    if flag:
        return alist
    else:
        for i in range(n - 1):
            if alist[i] > alist[i+1]:
                alist[i],alist[i+1]=alist[i+1],alist[i]
    return bubble_sort_3(alist[:-1]) + [alist[-1]]

if __name__ == '__main__':
    li = [54, 26, 93, 17, 77, 31, 44, 55, 20]
    new_list = bubble_sort_3(li)
    print(new_list)
12345678910111213141516171819202122
```

打印结果为`[17, 20, 26, 31, 44, 54, 55, 77, 93]`，与前两种方法相同，但是前两种方法是直接对原列表进行操作，使列表成为有序，原列表被改变，不带返回值，用递归实现的方法使被操作后的列表返回，返回有序的列表，是带有返回值的。

冒泡排序的分析：

- **冒泡排序是稳定的**：
  在冒泡排序中，只有交换才可以改变两个元素的前后顺序。为了保证冒泡排序算法的稳定性，当有相邻的两个元素大小相等的时候，我们不做交换，相同大小的数据在排序前后不会改变顺序，所以冒泡排序是稳定的排序算法。
- **时间复杂度**：
  最好情况下，要排序的数据已经是有序的了，只需要进行一次冒泡操作进行比较，就结束了，所以最好情况时间复杂度是O(n)；而最坏的情况是，要排序的数据刚好是倒序排列的，我们需要进行n次冒泡操作，所以最坏情况时间复杂度为O(n2)。

# Python全栈（二）数据结构和算法之7.选择排序、插入排序和希尔排序的实现

## 一、选择排序

原理：首先在未排序序列中找到最小（最大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小元素，然后放到已排序序列的末尾，以此类推，直到所有元素排序完毕。
过程和演示如下：
![过程](https://img-blog.csdnimg.cn/2019111921335889.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191119213504747.gif#pic_center)

### 实现：

```python
def selection_sort(li):
    '''选择排序'''
    n = len(li)
    #选择次数
    for i in range(n - 1):
        #记录最小值的位置
        min_index = i
        #从min_index位置到末尾找到最小值对应的下标，与min_index交换
        for j in range(min_index,n):
            if li[j] < li[min_index]:
                min_index = j
        #交换
        li[i],li[min_index] = li[min_index],li[i]

if __name__ == '__main__':
    li = [54, 26, 93, 17, 77, 31, 44, 55, 20]
    selection_sort(li)
    print(li)
123456789101112131415161718
```

进行测试，结果为`[17, 20, 26, 31, 44, 54, 55, 77, 93]`。

### 算法分析：

1.选择排序的主要优点与数据移动有关。如果某个元素位于正确的最终位置上，则它不会被移动。选择排序每次交换一对元素，它们当中至少有一个将被移到其最终位置上，因此对n个元素的表进行排序总共进行至多**(n-1)**次交换。在所有的完全依靠交换去移动元素的排序方法中，选择排序属于非常好的一种。
2.选择排序是不稳定的。选择排序是给每个位置选择当前元素最小的，比如给第一个位置选择最小的，在剩余元素里面给第二个元素选择第二小的，依次类推，直到第n-1个元素，第n个元素不用选择了，因为只剩下它一个最大的元素了。那么，在一趟选择，如果当前元素比一个元素小，而该小的元素又出现在一个和当前元素相等 的元素后面，那么交换后稳定性就被破坏了。举个例子，序列5 8 5 2 9，第一遍选择第1个元素5会和2交换，那么原序列中2个5的相对前后顺序就被破坏了，所以选择排序不是一个稳定的排序算法。

## 二、插入排序：

插入排序（Insertion Sort），是一种简单直观的排序算法。它的工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。插入排序在实现上，在从后向前扫描过程中，需要反复把已排序元素逐步向后挪位，为最新元素提供插入空间。
实现原理：
首先，我们将数组中的数据分为两个区间，已排序区间和未排序区间。初始已排序区间只有一个元素，就是数组的第一个元素。插入算法的核心思想是取未排序区间中的元素，在已排序区间中找到合适的插入位置将其插入，并保证已排序区间数据一直有序。重复这个过程，直到未排序区间中元素为空，算法结束。
如下图所示，要排序的数据是 4，5，6，1，3，2，其中左侧为已排序区间，右侧是未排序区间。
![过程](https://img-blog.csdnimg.cn/20191119213547656.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
动态演示如下，
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019111921365534.gif#pic_center)

### 实现：

**方法一**：

```python
def insert_sort_1(li):
    n = len(li)
    #从第二个位置，即下标为1的位置开始向前插入
    for i in range(1,n):
        #从第i个元素，向前比较，如果小于前一个元素，则交换
        for j in range(i,0,-1):
            if li[j] < li[j-1]:
                #交换
                li[j],li[j-1] = li[j-1],li[j]

if __name__ == '__main__':
    li = [54, 26, 93, 17, 77, 31, 44, 55, 20]
    insert_sort_1(li)
    print(li)
1234567891011121314
```

进行测试，结果为`[17, 20, 26, 31, 44, 54, 55, 77, 93]`。
**方法二**：

```python
def insert_sort_2(li):
    n = len(li)
    #从第二个位置，即下标为1的位置开始向前插入
    for i in range(1,n):
        j = i
        #从第i个元素，向前比较，如果小于前一个元素，则交换
        while j > 0:
            if li[j] < li[j-1]:
                #交换
                li[j],li[j-1] = li[j-1],li[j]
            j -= 1

if __name__ == '__main__':
    li = [54, 26, 93, 17, 77, 31, 44, 55, 20]
    insert_sort_2(li)
    print(li)
12345678910111213141516
```

结果与方法一相同。

### 算法分析：

从代码中可以看出一共遍历了n-1 + n-2 + … + 2 + 1 = n * (n-1) / 2 = 0.5 * n ^ 2 - 0.5 * n，那么时间复杂度是**O(N2)**；
因为在有序部分元素和待插入元素相等的时候，可以将待插入的元素放在前面，所以插入排序是稳定的。
是稳定的。

## 三、希尔排序

是插入排序的一种，也称缩小增量排序，是直接排序算法的一种更高效的改进版本。
希尔排序是非稳定排序算法。
希尔排序(Shell Sort)是插入排序的一种，也称缩小增量排序，是直接插入排序算法的一种更高效的改进版本。希尔排序通过将比较的全部元素分为几个区域来提升插入排序的性能。这样可以让一个元素可以一次性地朝最终位置前进一大步。然后算法再取越来越小的步长进行排序，算法的最后一步就是普通的插入排序，但是到了这步，需排序的数据几乎是已排好的了（此时插入排序较快）。
希尔排序的**实质就是分组的插入排序**。
工作原理：
将需要排序的序列划分为若干个较小的序列，对这些序列进行直接插入排序，每次排序后用更短的步长、更长的序列循环进行，通过这样的操作可使需要排序的数列基本有序，最后再使用一次直接插入排序。
在希尔排序中首先要解决的是**怎样划分序列**，对于子序列的构成不是简单地分段，而是采取将相隔某个增量的数据组成一个序列。一般选择增量的规则是：取上一个增量的一半作为此次子序列划分的增量，一般初始值元素的总数量。
对于序列`[54, 26, 93, 17, 77, 31, 44, 55, 20]`，其排序过程大致如下图，
![希尔排序](https://img-blog.csdnimg.cn/20191119213952132.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 实现：

方法一，

```python
def shell_sort_1(li):
    # 初始步长
    n = len(li)
    gap = n // 2
    # 按补偿进行插入排序
    while gap > 0:
        for i in range(gap,n,gap):
            # 插入排序
            while i >= gap:
                if li[i-gap] > li[i]:
                    li[i - gap], li[i] = li[i],li[i-gap]
                i -= gap
        # 得到新的步长
        gap = gap // 2

if __name__ == '__main__':
    li =  [54, 26, 93, 17, 77, 31, 44, 55, 20]
    shell_sort_1(li)
    print(li)
12345678910111213141516171819
```

打印结果为`[17, 20, 26, 31, 44, 54, 55, 77, 93]`。
方法二：

```python
def shell_sort_2(li):
    #初始步长
    n = len(li)
    gap = n // 2
    while gap > 0:
        #按补偿进行插入排序
        for i in range(gap,n,gap):
            #插入排序
            while i >= gap and li[i - gap] > li[i]:
                li[i - gap], li[i] = li[i], li[i - gap]
                i -= gap
        gap = gap // 2

if __name__ == '__main__':
    li =  [54, 26, 93, 17, 77, 31, 44, 55, 20]
    shell_sort_2(li)
    print(li)
1234567891011121314151617
```

结果与方法一相同。
方法三：

```python
def shell_sort_3(li):
    # 初始步长
    n = len(li)
    gap = n // 2
    # 按补偿进行插入排序
    while gap > 0:
        for i in range(n-1,gap-1,-1):
            j = i
            while j >= gap:
                # 插入排序
                if li[j - gap] > li[j]:
                    li[j - gap],li[j] = li[j],li[j - gap]
                j -= gap
        # 得到新的步长
        gap = gap // 2

if __name__ == '__main__':
    li =  [54, 26, 93, 17, 77, 31, 44, 55, 20]
    shell_sort_3(li)
    print(li)
1234567891011121314151617181920
```

结果与前两种方法相同，实现希尔排序的方式还有很多，个人比较喜欢第三种，因为很接近希尔排序的定义，方便理解。

### 算法分析：

1.希尔排序是按照不同步长对元素进行插入排序，当刚开始元素很无序的时候，步长最大，所以插入排序的元素个数很少，速度很快；当元素基本有序了，步长很小，插入排序对于有序的序列效率很高；
2.由于多次插入排序，一次插入排序是稳定的，不会改变相同元素的相对顺序，但在不同的插入排序过程中，相同的元素可能在各自的插入排序中移动，最后其稳定性就会被打乱，所以，希尔排序是不稳定的。

# Python全栈（二）数据结构和算法之8.快速排序、归并排序和二分查找的实现

## 一、快速排序

### 1.定义

又称划分交换排序（partition-exchange sort），通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

### 2.工作原理

1. 从数列中挑出一个元素，称为"基准"（pivot）；
2. 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区结束之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
3. 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。

递归的最底部情形，是数列的大小是零或一，也就是永远都已经被排序好了。虽然一直递归下去，但是这个算法总会结束，因为在每次的迭代（iteration）中，它至少会把一个元素摆到它最后的位置去。
图示分析如图，
![快速排序过程](https://img-blog.csdnimg.cn/20191121202313268.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
动态演示如下，
![动态演示](https://img-blog.csdnimg.cn/2019112120311241.gif)

### 3.算法实现

```python
def quick_sort(li,start,end):
    '''快速排序'''
    # 递归的退出条件
    if start >= end:
            return
        
    low = start
    high = end
    # 设定起始元素为要寻找位置的基准元素
    mid = li[low]
    while low < high:
        # 如果low与high未重合，high指向的元素不比基准元素小，则high向左移动
        while low < high and li[high] >= mid:
            high -= 1
        # 将high指向的元素放到low的位置上
        li[low] = li[high]
        # 如果low与high未重合，low指向的元素比基准元素小，则low向右移动
        while low < high and li[low] <= mid:
            low += 1
        # 将low指向的元素放到high的位置上
        li[high] = li[low]
    # 退出循环后，low与high重合，此时所指位置为基准元素的正确位置
    # 将基准元素放到该位置
    li[low] = mid
    #对前面的元素进行快排
    quick_sort(li,start,low - 1)
    #对后面的元素进行快排
    quick_sort(li,low + 1,end)

if __name__ == '__main__':
    li = [54, 26, 93, 17, 77, 31, 44, 55, 20]
    quick_sort(li,0,len(li) - 1)
    print(li)
123456789101112131415161718192021222324252627282930313233
```

进行测试，结果为`[17, 26, 31, 54, 54, 54, 55, 77, 93]`。

### 4.算法分析

时间复杂度：
最优的情况下空间复杂度为：**O(logN)**；
最差的情况下空间复杂度为：**O(N^2)**。
稳定性：
因为在快速排序的时候，即使待排序元素与基数相等也需要移动待排序元素的位置使得有序，所以快速排序是**不稳定**的。

## 二、归并排序

### 1.定义

归并排序是采用**分治法**的一个非常典型的应用，即分而治之。归并排序的思想就是先递归分解数组，再合并数组。

### 2.工作原理

将数组分解最小之后，然后合并两个有序数组，基本思路是比较两个数组的最前面的数，谁小就先取谁，取了后相应的指针就往后移一位。然后再比较，直至一个数组为空，最后把另一个数组的剩余部分复制过来即可。
动态演示为
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191121203148174.gif#pic_center)

### 3.算法实现

方法一：

```python
def merge_sort(li):
    '''归并排序'''
    # 如果列表长度小于1,不在继续拆分，返回列表
    if len(li) <= 1:
        return li
    #二分分解
    mid_index = len(li) // 2
    left_li = merge_sort(li[:mid_index])
    right_li = merge_sort(li[mid_index:])
    l_index = 0
    r_index = 0
    result = []
    #循环条件
    while l_index < len(left_li) and r_index < len(right_li):
        #left_li和right_li的当前元素进行比较，根据情况分别插入result，索引加1
        if left_li[l_index] < right_li[r_index]:
            result.append(left_li[l_index])
            l_index += 1
        else:
            result.append(right_li[r_index])
            r_index += 1
    #将left_li剩余部分加入数组
    result += left_li[l_index:]
    #将right_li剩余部分加入数组
    result += right_li[r_index:]
    #返回结果
    return result

if __name__ == '__main__':
    li = [54, 26, 93, 17, 77, 31, 44, 55, 20]
    result = merge_sort(li)
    print(result)
1234567891011121314151617181920212223242526272829303132
```

进行测试，打印结果为`[17, 20, 26, 31, 44, 54, 55, 77, 93]`。
方法二：

```python
def merge_sort(li):
    # 如果列表长度小于1,不在继续拆分
    if len(li) <= 1:
        return li
    # 二分分解
    mid_index = len(li) // 2
    left = merge_sort(li[:mid_index])
    right = merge_sort(li[mid_index:])
    # 合并
    return merge(left,right)

def merge(left, right):
    '''合并操作，将两个有序数组left[]和right[]合并成一个大的有序数组'''
    # left与right的下标指针
    l_index, r_index = 0, 0
    result = []
    while l_index < len(left) and r_index < len(right):
        if left[l_index] < right[r_index]:
            result.append(left[l_index])
            l_index += 1
        else:
            result.append(right[r_index])
            r_index += 1

    result += left[l_index:]
    result += right[r_index:]
    return result

if __name__ == '__main__':
    li = [54, 26, 93, 17, 77, 31, 44, 55, 20]
    result = merge_sort(li)
    print(result)
1234567891011121314151617181920212223242526272829303132
```

结果与方法一相同，但是此方法是分成两个子方法来实现，第一个函数进行分解，第二个函数进行合并，实质和方法一一样，只是形式不同。

### 4.算法分析

时间复杂度：
每次合并操作的时间复杂度是O(N)，而递归的深度是log2N，所以总的时间复杂度是O(**N\*lgN**)。
稳定性：
因为交换元素时，可以在相等的情况下做出不移动的限制，所以归并排序是**稳定**的。

### 5.常见排序算法效率比较

如下所示，
![算法比较](https://img-blog.csdnimg.cn/20191121203230293.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 三、二分查找

### 1.搜索定义

搜索是在一个项目集合中找到一个特定项目的算法过程。搜索通常的答案是真的或假的，因为该项目是否存在。 搜索的几种常见方法：顺序查找、二分法查找、二叉树查找、哈希查找。

### 2.二分查找定义

二分查找又称折半查找，优点是比较次数少，查找速度快，平均性能好；其缺点是要求**待查表为有序表**，且插入删除困难。因此，折半查找方法适用于不经常变动而查找频繁的有序列表。

### 3.工作原理

1. 首先，假设表中元素是按升序排列，将表中间位置记录的关键字与查找关键字比较，如果两者相等，则查找成功；
2. 否则利用中间位置记录将表分成前、后两个子表，如果中间位置记录的关键字大于查找关键字，则进一步查找前一子表，否则进一步查找后一子表；
3. 重复以上过程，直到找到满足条件的记录，使查找成功，或直到子表不存在为止，此时查找不成功。

图示如下，
![二分查找](https://img-blog.csdnimg.cn/20191121203302811.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 4.算法实现

方法一，

```python
def binary_search_1(li,item):
    '''普通二分查找'''
    first = 0
    last = len(li) - 1
    while first <= last:
        mid = (first + last) // 2
        if li[mid] == item:
            return True
        elif item < li[mid]:
            last = mid - 1
        else:
            first = mid + 1
    return False

if __name__ == '__main__':
    #li为有序列表
    li = [17, 20, 26, 31, 44, 54, 55, 77, 93]
    result1 = binary_search_1(li, 17)
    result2 = binary_search_1(li, 54)
    result3 = binary_search_1(li, 100)
    print(result1,result2,result3)
123456789101112131415161718192021
```

打印结果为`True True False`。

方法二，

```python
def binary_search_2(li,item):
    '''递归实现二分查找'''
    if len(li) == 0:
        return False
    else:
        mid = len(li) // 2
        if li[mid] == item:
            return True
        else:
            if item < li[mid]:
                return binary_search_2(li[:mid],item)
            else:
                return binary_search_2(li[mid+1:],item)

if __name__ == '__main__':
    #li为有序列表
    li = [17, 20, 26, 31, 44, 54, 55, 77, 93]
    result1 = binary_search_2(li, 17)
    result2 = binary_search_2(li, 54)
    result3 = binary_search_2(li, 100)
    print(result1,result2,result3)
123456789101112131415161718192021
```

此方法为递归方法实现，结果与方法一相同。

### 5.算法分析

二分查找算法，在n个元素的数组中查找一个数，情况最遭时，需要(log2n)步，所以二分查找的时间复杂度是**O(log2n)**

# Python全栈（二）数据结构和算法之9.树与树的算法

## 一、树的概念

### 1.定义

树是一种**抽象数据结构**（ADT），用来模拟具有树状结构性质的数据集合。
树是由n（n>=1）个有限节点组成一个具有层次关系的集合。把它叫做“树”是因为它看起来像一棵倒挂的树，也就是说它是根朝上，而叶朝下的。
![树的结构](https://img-blog.csdnimg.cn/20191123165321854.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 2.特点：

- 每个节点有零个或多个子节点；
- 没有父节点的节点称为根节点；
- 每个非根节点有且只有一个父节点；
- 除了根节点，每个子节点可以分为多个不相交的子树。
  如图，
  ![节点及其关系](https://img-blog.csdnimg.cn/2019112316540663.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
  A 节点就是 B 节点的**父节点**，B 节点是 A 节点的**子节点**。B、C、D 这三个节点的父节点是同一个节点，所以它们之间互称为**兄弟节点**。我们把没有父节点的节点叫作**根节点**，也就是图中的节点 E。我们把没有子节点的节点叫作**叶子节点**或者**叶节点**，比如图中的 G、H、I、J、K、L 都是叶子节点。

### 3.度量概念：

高度（Height）：节点到叶子节点的最长路径。
树的高度即根节点的高度。
深度（Depth）：根节点到这个节点所经历的边的个数。
层数（Level）：节点的深度+1。
如下图所示，
![度量](https://img-blog.csdnimg.cn/20191123165434473.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 4.种类：

- **无序树**：树中任意节点的子节点之间没有顺序关系，也称为自由树。

- 有序树

  ：树中任意节点的子节点之间有顺序关系。

  又进一步分为，

  - 二叉树

    ：每个节点最多含有两个子树的树称为二叉树。

    - **完全二叉树**：对于一颗二叉树，假设其深度为d(d>1)。除了第d层外，其它各层的节点数目均已达最大值，且第d层所有节点从左向右连续地紧密排列，这样的二叉树被称为完全二叉树，其中**满二叉树**的定义是所有叶节点都在最底层的完全二叉树。
    - **平衡二叉树**（AVL树）：当且仅当任何节点的两棵子树的高度差不大于1的二叉树。
    - **排序二叉树**（二叉查找树、二叉搜索树、有序二叉树，Binary Search Tree）。

  - **霍夫曼树**：带权路径最短的二叉树称为哈夫曼树或最优二叉树，常用于信息编码。
  - **B树**：一种对读写操作进行优化的自平衡的二叉查找树，能够保持数据有序，拥有多余两个子树。

### 5.树的应用场景：

- xml，html等的结构其实就是树的结构，编写的适合会用到树；
- 路由协议使用了树的算法；
- mysql数据库索引；
- 文件系统的目录结构；
- 很多经典的AI算法其实都是树搜索，此外机器学习中的decision tree也是树结构。

## 二、二叉树及其概念

### 1.二叉树定义

二叉树每个节点最多有两个子节点，分别是左子节点和右子节点。不过，二叉树并不要求每个节点都有两个子节点，有的节点只有左子节点，有的节点只有右子节点。
如下图，
![二叉树结构](https://img-blog.csdnimg.cn/20191123170006651.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
其中，编号 2 的二叉树中，叶子节点全都在最底层，除了叶子节点之外，每个节点都有左右两个子节点，这种二叉树就叫作满二叉树。
编号 3 的二叉树中，叶子节点都在最底下两层，最后一层的叶子节点都靠左排列，并且除了最后一层，其他层的节点个数都要达到最大，这种二叉树叫作完全二叉树。

### 2.存储与表示

（1）顺序存储：
将数据结构存储在固定的数组中。
特点：在遍历速度上有一定的优势，但因所占空间比较大，是非主流二叉树。
如图
![顺序存储](https://img-blog.csdnimg.cn/20191123170047428.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
（2）链式存储：
每个节点有三个字段，其中一个存储数据，另外两个是指向左右子节点的指针。
特点：只要找到根节点，就可以通过左右子节点的指针，把整棵树都串起来。是常用的存储方式。
如图
![链式存储](https://img-blog.csdnimg.cn/20191123170116689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 三、二叉树的实现

### 1.节点的实现

通过Node类中定义三个属性，分别为item本身的值，还有litem和ritem。

```python
class Node(object):
    '''节点类'''
    def __init__(self,item,litem = None,ritem=None):
        self.item = item
        self.litem = litem
        self.ritem = ritem
123456
```

### 2.树的创建和添加

创建一个树的类，并给一个root根节点，一开始为空，随后添加节点。

```python
class Tree(object):
    '''树类'''
    def __init__(self,root=None):
        self.root = root

    def add(self,item):
        '''添加节点'''
        #创建节点
        node = Node(item)
        #如果树是空的，则对根节点赋值
        if self.root == None:
            self.root = node
        else:
            queue = []
            # 根节点放到队列中
            queue.append(self.root)
            #对所有的节点进行层次遍历
            while queue:
                #弹出队列的第一个元素
                cur_node = queue.pop(0)
                if cur_node.litem == None:
                    cur_node.litem = node
                    return
                elif cur_node.ritem == None:
                    cur_node.ritem = node
                    return
                else:
                    #如果左右子树都不为空，加入队列继续判断
                    queue.append(cur_node.litem)
                    queue.append(cur_node.ritem)
123456789101112131415161718192021222324252627282930
```

添加实际上是按照**广度优先顺序**添加的。

### 3.树的遍历

树的遍历是树的一种重要操作。
遍历是指对树中所有结点的信息的访问，即依次对树中每个结点访问一次且仅访问一次，我们把这种对所有节点的访问称为遍历（traversal）。
树的两种重要的遍历方式是深度优先遍历和广度优先遍历,深度优先一般用**递归**，广度优先一般用**队列**。
一般情况下能用递归实现的算法大部分也能用堆栈来实现。

#### 广度优先遍历（层次遍历）

从树的root开始，从上到下从从左到右遍历整个树的节点。
实现如下，

```python
def breadth_travel(self):
    '''广度优先遍历'''
    if self.root == None:
        return
    else:
        queue = []
        # 根节点放到队列中
        queue.append(self.root)
        while queue:
            #当前节点
            node = queue.pop(0)
            print(node.item,end=' ')
            if node.litem != None:
                queue.append(node.litem)
            if node.ritem != None:
                queue.append(node.ritem)
12345678910111213141516
```

#### 深度优先遍历

对于一颗二叉树，沿着树的深度遍历树的节点，尽可能深地搜索树的分支。
深度遍历有重要的三种方法，它们之间的不同在于访问每个节点的次序不同。这三种遍历分别为先序遍历（preorder）、中序遍历（inorder）和后序遍历（postorder）。

##### 先序遍历：

先访问根节点，然后递归使用先序遍历访问左子树，再递归使用先序遍历访问右子树，即
**根节点->左子树->右子树**
实现如下

```python
def preorder(self,node):
    '''递归实现先序遍历'''
    if node == None:
        return
    print(node.item,end=' ')
    self.preorder(node.litem)
    self.preorder(node.ritem)
1234567
```

##### 中序遍历：

递归使用中序遍历访问左子树，然后访问根节点，最后再递归使用中序遍历访问右子树，即
**左子树->根节点->右子树**
实现如下

```python
def inorder(self,node):
    '''递归实现中序遍历'''
    if node == None:
        return
    self.inorder(node.litem)
    print(node.item,end=' ')
    self.inorder(node.ritem)
1234567
```

##### 后序遍历：

在后序遍历中，我们先递归使用后序遍历访问左子树和右子树，最后访问根节点，即
**左子树->右子树->根节点**
实现如下

```python
def postorder(self,node):
    '''递归实现后序遍历'''
    if node == None:
        return
    self.postorder(node.litem)
    self.postorder(node.ritem)
    print(node.item, end=' ')
1234567
```

先序、终须、后序的记忆方式可根据**根节点的遍历先后**进行区别，最先遍历即为先序遍历，中间遍历则为中序遍历，最后遍历则为后序遍历。

完整实现代码如下，

```python
class Node(object):
    '''节点类'''
    def __init__(self,item,litem = None,ritem=None):
        self.item = item
        self.litem = litem
        self.ritem = ritem

class Tree(object):
    '''树类'''
    def __init__(self,root=None):
        self.root = root

    def add(self,item):
        '''添加节点'''
        #创建节点
        node = Node(item)
        #如果树是空的，则对根节点赋值
        if self.root == None:
            self.root = node
        else:
            queue = []
            # 根节点放到队列中
            queue.append(self.root)
            #对所有的节点进行层次遍历
            while queue:
                #弹出队列的第一个元素
                cur_node = queue.pop(0)
                if cur_node.litem == None:
                    cur_node.litem = node
                    return
                elif cur_node.ritem == None:
                    cur_node.ritem = node
                    return
                else:
                    #如果左右子树都不为空，加入队列继续判断
                    queue.append(cur_node.litem)
                    queue.append(cur_node.ritem)

    def breadth_travel(self):
        '''广度优先遍历'''
        if self.root == None:
            return
        else:
            queue = []
            # 根节点放到队列中
            queue.append(self.root)
            while queue:
                #当前节点
                node = queue.pop(0)
                print(node.item,end=' ')
                if node.litem != None:
                    queue.append(node.litem)
                if node.ritem != None:
                    queue.append(node.ritem)

    def preorder(self,node):
        '''递归实现先序遍历'''
        if node == None:
            return
        print(node.item,end=' ')
        self.preorder(node.litem)
        self.preorder(node.ritem)

    def inorder(self,node):
        '''递归实现中序遍历'''
        if node == None:
            return
        self.inorder(node.litem)
        print(node.item,end=' ')
        self.inorder(node.ritem)

    def postorder(self,node):
        '''递归实现后序遍历'''
        if node == None:
            return
        self.postorder(node.litem)
        self.postorder(node.ritem)
        print(node.item, end=' ')

if __name__ == '__main__':
    t = Tree()
    t.add(0)
    t.add(1)
    t.add(2)
    t.add(3)
    t.add(4)
    t.add(5)
    t.add(6)
    t.add(7)
    t.add(8)
    t.breadth_travel()
    print()
    t.preorder(t.root)
    print()
    t.inorder(t.root)
    print()
    t.postorder(t.root)
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879808182838485868788899091929394959697
```

打印结果为

```python
0 1 2 3 4 5 6 7 8 
0 1 3 7 8 4 2 5 6 
7 3 8 1 4 0 5 2 6 
7 8 3 4 1 5 6 2 0
1234
```

显然，4种遍历方法得到的结果都不完全一致。
再用下图进行展示，
![4种遍历方法](https://img-blog.csdnimg.cn/20191123170252799.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 4.遍历的分析

如果知道了树的先序、后序遍历结果中的一种，并且也知道中序遍历的结果，可以根据遍历结果进行反推：
（1）根据先序遍历的第一个元素或后序遍历的最后一个元素即可知道原树的根节点，再在中序遍历结果中从根节点断开，左边即为左子树，右边即为右子树；
（2）根据分出的左右子树将先序或后序除开根节点的部分分成两个部分，即为左子树、右子树，对每个子树再进行（1）中的操作，即递归，直到每个子树的大小为1为止。
使用这种方法的前提是必须知道**中序遍历**的结果，和前序、后序遍历结果中的一种。

# Python全栈（三）数据库优化之1.数据库简介

## 一.数据库的介绍

### 1.传统数据记录的缺点：

- 不易保存；
- 备份困难；
- 查找不便。

### 2.现代化手段–文件：

对于数据容量较大的数据，不能够很好的满足，而且性能较差；
不易扩展。

### 3.数据库：

- 持久化存储（数据库是一种特殊的文件）；
- 读写速度**极高**；
- 保证数据的**有效性**；
- 对程序支持性非常好，容易扩展。
- 我们看到的数据库的呈现方式是这样的，
  ![数据库](https://img-blog.csdnimg.cn/20191125215614417.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
  在网页上展现出来可能是这样的，
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20191125215631150.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 二.数据库

### 1.相关名词

- 数据行（记录）
- 数据列（字段）
- 数据表（数据行的集合）
- 数据库（数据表的集合）
- 主键（能够**唯一**标识某个记录的字段）
  ![ju](https://img-blog.csdnimg.cn/20191125215722909.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 2.MySQL简介：

一个关系型数据库管理系统，目前属于Oracle旗下产品。
特点：

- 使用C和C++编写，并使用了多种编译器进行测试，保证源代码的可移植性；
- 支持多种操作系统，如Linux、Windows、AIX、FreeBSD、HP-UX、MacOS、NovellNetware、OpenBSD、OS/2 Wrap、Solaris等；
- 为多种编程语言提供了API，如C、C++、Python、Java、Perl、PHP、Eiffel、Ruby等；
- 支持多线程，充分利用CPU资源；
- 优化的SQL查询算法，有效地提高查询速度；
- 提供多语言支持，常见的编码如GB2312、BIG5、UTF8；
- 提供TCP/IP、ODBC和JDBC等多种数据库连接途径；
- 提供用于管理、检查、优化数据库操作的管理工具；
- 大型的数据库。可以处理拥有上千万条记录的大型数据库；
- 支持多种存储引擎；
- MySQL 软件采用了双授权政策，它分为社区版和商业版，由于其体积小、速度快、总体拥有成本低，尤其是开放源码这一特点，一般中小型网站的开发都选择MySQL作为网站数据库；
- MySQL使用标准的SQL数据语言形式；
- Mysql是可以定制的，采用了GPL协议，你可以修改源码来开发自己的Mysql系统；
- 在线DDL更改功能；
- 复制全局事务标识；
- 复制无崩溃从机；
- 复制多线程从机。

## 三、MySQL安装和配置

### 1.直接安装

下载地址：https://www.mysql.com/downloads
安装可参考：https://jingyan.baidu.com/article/0aa223751ed91188cc0d643f.html

### 2.集成安装

- phpstudy
- xampp

phpstudy：集成安装，可用于MySQL服务的**开启和停止**。
安装建议：
直接安装步骤繁琐，建议使用集成安装方式，使用也更方便。

### 3.图形化管理工具

- phpMyAdmin
- Navicat
- SQLyog

图形化管理工具：MySQL服务端开启服务后，是以命令行的方式操作数据库的，不方便，图形化工具可以更方便地操作数据库。
SQLyog的安装过程可参考：https://blog.csdn.net/lihua5419/article/details/73881837
使用建议：
图形化管理工具是为了辅助操作的，如果在现实工作中用图形化工具会导致**安全**问题，所以实际学习和工作中用的更多的是通过**语句**来操作。

# Python全栈（三）数据库优化之2.数据库的操作与数据表的操

## 一、SQL介绍&常见的数据类型

### 1.SQL定义

SQL是结构化查询语言，是一种用来操作**RDBMS**(关系型数据库管理系统)的数据库语言。
当前关系型数据库都支持使用SQL语言进行操作，也就是说可以通过SQL操作oracle，sql server,mysql等关系型数据库。

SQL语句分为：
DQL数据查询语言：
用于对数据进行查询，如select；
DML数据操作语言：
对数据进行增加、修改、删除，如insert、udpate、delete；
DDL数据定义语言：
进行数据库、表的管理等，如create、drop。
重点是数据的**CURD**（增删改查）。

### 2.数据的完整性：

在表中为了更加准确的存储数据，保证数据的正确有效，可以在创建表的时候，为表添加一些强制性的验证，包括数据字段的类型、约束。

#### 常见的数据类型：

整型int；
小数decimal:
decimal表示浮点数，如decimal(5,2)表示共存5位数，小数占2位.
字符串varchar、char：
varchar是可变长度的字符串，如varchar(3)，填充’ab’时会存储’ab’；
char是固定长度字符串，如char(3)，填充’ab’时会存储’ab '（ab后有一个空格）；
字符串text用于存储大文本，当字符数大于4000时推荐使用。
※对于图片、音频、文本等二进制文件，不存储在数据库中，而是上传到某个服务器上，然后在表中存储这个文件的保存地址。
日期时间date、time、datetime。
枚举类型enum：
把所有可能列举出来，如性别只有2种男、女，星期只有周一到周日7种。

#### 数值类型：

![数值类型](https://img-blog.csdnimg.cn/20191130001759126.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

#### 字符串：

![字符串](https://img-blog.csdnimg.cn/20191130001859977.png)

#### 日期时间类型：

![日期时间](https://img-blog.csdnimg.cn/2019113000191575.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 二、数据库约束&数据库简单操作

### 1.约束

主键primary key：物理上存储的顺序，取值唯一,可以设置自增，并且只能是主键设置自增，其他字段不能设置自增，且主键不能设置默认值；
非空not null：此字段不允许填写空值；
无符号unsigned：没有负值；
自增auto_increment：主键自动增加；
唯一unique：此字段的值不允许重复；
默认default：当不填写此值时会使用默认值，如果填写时以填写为准；
外键foreign key：对关系字段进行约束，当为关系字段填写值时，会到关联的表中查询此值是否存在，如果存在则填写成功，如果不存在则填写失败并抛出异常。

### 2.数据库操作

操作都是基于命令行的,SQL语句后需要用**分号结尾**。
数据库的连接：

```mysql-sql
--连接数据库
mysql -u root -proot --不安全,密码直接显示，不推荐
12
```

结果：

```mysql-sql
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 94
Server version: 5.7.26 MySQL Community Server (GPL)

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
1234567891011121314
```

或者

```mysql-sql
mysql -uroot -p
1
Enter password: ****
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 97
Server version: 5.7.26 MySQL Community Server (GPL)

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
1234567891011121314
```

此时会提示输入密码，密码默认为root。
退出数据库

```mysql-sql
--退出数据库
exit --或者quit
12
```

打印

```mysql-sql
mysql> exit
Bye
12
```

查看所有数据库

```mysql-sql
--查看所有数据库
show databases;
12
```

打印

```mysql-sql
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test               |
+--------------------+
5 rows in set (0.01 sec)
12345678910
```

显示数据库版本

```mysql-sql
--显示数据库版本
select version();
12
```

打印

```mysql-sql
+-----------+          
| version() |          
+-----------+          
| 5.7.26    |          
+-----------+          
1 row in set (0.00 sec)
                       
mysql>                 
12345678
```

显示时间

```mysql-sql
--查询时间
select now();
12
```

打印

```mysql-sql
+---------------------+
| now()               |
+---------------------+
| 2019-11-29 16:47:56 |
+---------------------+
1 row in set (0.00 sec)
123456
```

创建数据库

```mysql-sql
--创建数据库
create database pythontest;
12
```

打印

```mysql-sql
Query OK, 1 row affected (0.00 sec)
1
```

创建数据库时设置编码方式

```mysql-sql
--创建数据库（设置编码方式）
create database pythontest charset=utf8;
12
```

查看创建数据库的命令

```mysql-sql
--查看创建数据库的语句
show create database pythontest;
12
```

打印

```mysql-sql
+------------+---------------------------------------------------------------------------------------------+
| Database   | Create Database                                                                             |
+------------+---------------------------------------------------------------------------------------------+
| pythontest | CREATE DATABASE `pythontest` /*!40100 DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci */ |
+------------+---------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)                                                                                     
123456
```

使用数据库

```mysql-sql
--使用数据库
use test;
12
```

打印

```mysql-sql
Database changed
1
```

查看当前使用的数据库

```mysql-sql
--查看当前使用的数据库
select database();
12
```

打印

```mysql-sql
+------------+         
| database() |         
+------------+         
| test       |         
+------------+         
1 row in set (0.00 sec)
123456
```

删除数据库
这一语句需要慎用，

```mysql-sql
--删除数据库
drop database test;
12
```

打印

```mysql-sql
Query OK, 1 row affected (0.01 sec)
1
```

## 三、数据表的操作和数据插入

### 1.数据表的操作

查看当前数据库中所有表

```mysql-sql
--查看当前数据库中所有的表
show tables;
12
```

打印

```mysql-sql
+----------------+     
| Tables_in_demo |     
+----------------+     
| test           |     
+----------------+     
1 row in set (0.00 sec)
123456
```

创建表

```mysql-sql
--create table tablename(字段 约束[,字段 约束 类型]);
create table demo1(
    id int,
    name varchar(30)
);
12345
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
1
```

查看表结构

```mysql-sql
--desc 数据表的名字；
desc demo1;
12
```

打印

```mysql-sql
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | YES  |     | NULL    |       |
| name  | varchar(30) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
1234567
--创建students表（id、name、age、high、gender、cls_id）
create table students(
    id int not null primary key auto_increment,
    name varchar(30),
    age tinyint unsigned default 18,
    height decimal(5,2),
    gender enum('male','female','secret') default 'secret',     --存数据时，只能存male、female、secret三个值，不能存其他的值。
    cls_id int
);
123456789
```

打印

```mysql-sql
Query OK, 0 rows affected (0.00 sec)
1
```

查看表的创建语句

```mysql-sql
-- show create table 表名字;
show create table students;
12
```

打印

```mysql-sql
+----------+-------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------+                                          
| Table    | Create Table                                                                                          
                                                                                                                   
                                                                                                                   
                                                                        |                                          
+----------+-------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------+                                          
| students | CREATE TABLE `students` (                                                                             
  `id` int(11) NOT NULL AUTO_INCREMENT,                                                                            
  `NAME` varchar(30) COLLATE utf8_unicode_ci DEFAULT NULL,                                                         
  `age` tinyint(3) unsigned DEFAULT '18',                                                                          
  `height` decimal(5,2) DEFAULT NULL,                                                                              
  `gender` enum('male','female','secret') COLLATE utf8_unicode_ci DEFAULT 'secret',                                
  `cls_id` int(11) DEFAULT NULL,                                                                                   
  PRIMARY KEY (`id`)                                                                                               
) ENGINE=MyISAM DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci |                                                     
+----------+-------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------+                                          
1 row in set (0.00 sec)                                                                                            
1234567891011121314151617181920212223242526
```

创建classes表（id、name）：

```mysql-sql
create table classes(
    id int primary key not null auto_increment,
    name varchar(30)
);
1234
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
1
```

修改表-添加字段

```mysql-sql
-- alter table 表名 add 列名 类型;
alter table students add birthday date;
12
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

修改表-修改字段（不重命名）

```mysql-sql
-- alter table 表名 modify 列名 类型及约束;
alter table students modify birthday date default '1990-1-1';
12
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

修改表-修改字段（重命名）

```mysql-sql
-- alter table 表名 change 原名 新名 类型及约束
alter table students change birthday birth date default '1990-1-1';
12
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

修改表-删除字段

```mysql-sql
-- alter table 表名 drop 列名;
alter table students drop height;
12
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

删除表或者数据库

```mysql-sql
drop table 表名;
drop database 数据库;
12
```

### 2.数据插入

#### 全列插入

```mysql-sql
--insert [into] 表名 values(...);
--向classes中插入一个班级
insert into classes values(1,'class1');
select * from classes;
1234
```

打印

```mysql-sql
Query OK, 1 row affected (0.00 sec)

+----+--------+
| id | name   |
+----+--------+
|  1 | class1 |
+----+--------+
1 row in set (0.00 sec)
12345678
```

主键字段是自增的，用0、null、default来占位。

```mysql-sql
--向students表插入一个学生信息
insert into students values(0,'Tom',18,1,1,'1999-9-9');
insert into students values(null,'Jerry',19,1,1,'1999-10-9');
insert into students values(default,'Nancy',17,1,2,'1999-8-9');
select * from students;
12345
```

打印

```mysql-sql
Query OK, 1 row affected (0.00 sec)                     
                                                        
Query OK, 1 row affected (0.00 sec)                     
                                                        
Query OK, 1 row affected (0.00 sec)                     
                                                        
+----+-------+------+--------+--------+------------+    
| id | NAME  | age  | gender | cls_id | birth      |    
+----+-------+------+--------+--------+------------+    
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |    
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |    
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |    
+----+-------+------+--------+--------+------------+    
3 rows in set (0.00 sec)                                
1234567891011121314
```

枚举类型插入,下标是从1开始。

```mysql-sql
insert into students values(default,'John',19,1,1,'1999-7-9');
select * from students;
12
```

打印

```mysql-sql
Query OK, 1 row affected (0.00 sec)                  
                                                     
+----+-------+------+--------+--------+------------+ 
| id | NAME  | age  | gender | cls_id | birth      | 
+----+-------+------+--------+--------+------------+ 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 | 
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 | 
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 | 
|  4 | John  |   19 | male   |      1 | 1999-07-09 | 
+----+-------+------+--------+--------+------------+ 
4 rows in set (0.00 sec)                             
1234567891011
```

#### 部分插入

非空字段必须要指定值，否则要指定默认值。

```mysql-sql
--insert into 表名(列1,...) values(值1,...)
insert into students(id,name,gender) values(default,'Tony',1);
12
```

打印

```mysql-sql
Query OK, 1 row affected (0.00 sec)
1
```

#### 多行插入

```mysql-sql
insert into students values
(default,'John',19,1,2,'1999-7-9'),
(default,'Rose',19,2,2,'1999-7-9')
;
1234
```

打印

```mysql-sql
Query OK, 2 rows affected (0.01 sec)
Records: 2  Duplicates: 0  Warnings: 0
```

# Python全栈（三）数据库优化之3.数据的修改和删除及数据的条件查询

## 一、修改&删除&简单查询

### 1.修改

update 表名 set 列1=值1,列2=值2,… where 条件;

```mysql-sql
--不加where，全部修改
update students set name = 'Jack';
12
--加where，修改符合条件的记录
update students set name = 'Jack' where name = 'John';
12
update students set name = 'John',gender = 'secret' where name = 'Jack';
1
```

### 2.删除

#### 物理删除

delete from 表名 where 条件;

```mysql-sql
--删除的是表中的全部数据
delete from demo1;
12
```

打印

```mysql-sql
Query OK, 2 rows affected (0.00 sec)
1
--删除的是表中符合条件的数据
delete from students where id = 5;
12
```

打印

```mysql-sql
Query OK, 1 row affected (0.01 sec)
1
```

删除的是表中的数据，数据被真实地删除，删除操作后被删除的数据已不存在。

#### 逻辑删除

`is_delete`字段表示是否删除，相当于对原有数据增加一个标识，并未真正删除数据。
这样能保存原有的数据，因为在互联网时代数据有巨大的价值，不应该被任意删除。
查找：

```mysql-sql
select * from students where is_delete = 0;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+ 
| id | NAME  | age  | gender | cls_id | birth      | is_delete | 
+----+-------+------+--------+--------+------------+-----------+ 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 | 0         | 
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 | 0         | 
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 | 0         | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 | 0         | 
|  6 | John  |   16 | secret |      1 | 1999-05-09 | 0         | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 | 0         | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
|  9 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 10 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 11 | Rose  |   19 | female |      2 | 1999-07-09 | 0         | 
| 12 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 13 | Rose  |   19 | female |      2 | 1999-07-09 | 0         | 
| 14 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 15 | Rose  |   19 | female |      2 | 1999-07-09 | 0         | 
| 16 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 17 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 18 | Rose  |   19 | female |      2 | 1999-07-09 | 0         | 
| 19 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 20 | Rose  |   19 | female |      2 | 1999-07-09 | 0         | 
| 21 | John  |   19 | secret |      1 | 1999-07-09 | 0         | 
| 22 | John  |   19 | secret |      1 | 1999-07-09 | 0         | 
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 | 0         | 
| 24 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 25 | John  |   16 | secret |      1 | 1999-05-09 | 0         | 
| 26 | John  |   19 | secret |      2 | 1999-07-09 | 0         | 
| 27 | Rose  |   19 | female |      2 | 1999-07-09 | 0         | 
+----+-------+------+--------+--------+------------+-----------+ 
26 rows in set (0.00 sec)                                        
12345678910111213141516171819202122232425262728293031
```

此时删除即修改is_delete字段：

```mysql-sql
update students set is_delete = 1 where id = 7;
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.00 sec)
Rows matched: 1  Changed: 0  Warnings: 0
12
```

### 3.MySQL简单查询

（1）查询所有列：

```mysql-sql
select * from students;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 |
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
26 rows in set (0.01 sec)
12345678910111213141516171819202122232425262728293031
```

（2）去除重复字段的查询：
消除重复行：distinct字段

```mysql-sql
select distinct name from students;
1
```

打印

```mysql-sql
+-------+               
| name  |               
+-------+               
| Tom   |               
| Jerry |               
| Nancy |               
| John  |               
| Rose  |               
| Tony  |               
+-------+               
6 rows in set (0.00 sec)
1234567891011
```

distinct关注的是整个行是否重复。

```mysql-sql
select distinct name,age from students;
1
```

打印

```mysql-sql
+-------+------+        
| name  | age  |        
+-------+------+        
| Tom   |   18 |        
| Jerry |   19 |        
| Nancy |   17 |        
| John  |   19 |        
| John  |   16 |        
| Rose  |   19 |        
| Tony  |   18 |        
+-------+------+        
7 rows in set (0.00 sec)
123456789101112
```

（3）查询指定列：
select 列1,列2,… from 表名;

```mysql-sql
select name,age,gender from students;
1
```

打印

```mysql-sql
+-------+------+--------+ 
| name  | age  | gender | 
+-------+------+--------+ 
| Tom   |   18 | male   | 
| Jerry |   19 | male   | 
| Nancy |   17 | male   | 
| John  |   19 | secret | 
| John  |   16 | secret | 
| John  |   19 | secret | 
| John  |   19 | secret | 
| John  |   19 | secret | 
| John  |   19 | secret | 
| Rose  |   19 | female | 
| John  |   19 | secret | 
| Rose  |   19 | female | 
| John  |   19 | secret | 
| Rose  |   19 | female | 
| John  |   19 | secret | 
| John  |   19 | secret | 
| Rose  |   19 | female | 
| John  |   19 | secret | 
| Rose  |   19 | female | 
| John  |   19 | secret | 
| John  |   19 | secret | 
| Tony  |   18 | male   | 
| John  |   19 | secret | 
| John  |   16 | secret | 
| John  |   19 | secret | 
| Rose  |   19 | female | 
+-------+------+--------+ 
26 rows in set (0.01 sec) 
12345678910111213141516171819202122232425262728293031
```

（4）可以为表、字段等重命名，

```mysql-sql
select name as n,gender from students as stu;
1
```

打印

```mysql-sql
+-------+--------+       
| n     | gender |       
+-------+--------+       
| Tom   | male   |       
| Jerry | male   |       
| Nancy | male   |       
| John  | secret |       
| John  | secret |       
| John  | secret |       
| John  | secret |       
| John  | secret |       
| John  | secret |       
| Rose  | female |       
| John  | secret |       
| Rose  | female |       
| John  | secret |       
| Rose  | female |       
| John  | secret |       
| John  | secret |       
| Rose  | female |       
| John  | secret |       
| Rose  | female |       
| John  | secret |       
| John  | secret |       
| Tony  | male   |       
| John  | secret |       
| John  | secret |       
| John  | secret |       
| Rose  | female |       
+-------+--------+       
26 rows in set (0.00 sec)
12345678910111213141516171819202122232425262728293031
```

通常在对**多个表**进行联合查询时，会对表重命名。
as关键字表示在查询的结果中显示时，替换掉原来的名称，而不是将字段替换，只是显示时发生了变化，不能被引用，否则会报错。

```mysql-sql
select name as n,gender from students where n = 'Tom';
1
```

打印

```mysql-sql
ERROR 1054 (42S22): Unknown column 'n' in 'where clause'
1
```

即报错，字段别名不能在where条件中使用。

## 二、条件查询-比较&逻辑运算符

### 1.比较运算符：

（1）**>** 大于

```mysql-sql
select * from students where age > 18;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+  
| id | NAME  | age  | gender | cls_id | birth      | is_delete |  
+----+-------+------+--------+--------+------------+-----------+  
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |  
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |  
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |  
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 |  
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 |  
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
+----+-------+------+--------+--------+------------+-----------+  
21 rows in set (0.01 sec)                                         
1234567891011121314151617181920212223242526
```

（2）**<** 小于

```mysql-sql
select * from students where age < 18;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
3 rows in set (0.01 sec)                                        
12345678
```

（3）**>=** 大于等于

```mysql-sql
select * from students where age >= 18;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+ 
| id | NAME  | age  | gender | cls_id | birth      | is_delete | 
+----+-------+------+--------+--------+------------+-----------+ 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
+----+-------+------+--------+--------+------------+-----------+ 
23 rows in set (0.00 sec)                                        
12345678910111213141516171819202122232425262728
```

（4）**<=** 小于等于

```mysql-sql
select * from students where age <= 18;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
5 rows in set (0.00 sec)                                        
12345678910
```

（5）**=** 等于

```mysql-sql
select * from students where age = 18;
1
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+
| id | NAME | age  | gender | cls_id | birth      | is_delete |
+----+------+------+--------+--------+------------+-----------+
|  1 | Tom  |   18 | male   |      1 | 1999-09-09 |         1 |
| 23 | Tony |   18 | male   |      1 | 1990-01-01 |         1 |
+----+------+------+--------+--------+------------+-----------+
2 rows in set (0.00 sec)
1234567
```

（6）**!=\**或\**<>** 不等于

```mysql-sql
select * from students where name != 'John';
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 |
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
10 rows in set (0.00 sec)                                       
123456789101112131415
select * from students where name <> 'John';
1
```

结果与前面相同。

### 2.逻辑运算符

（1）**and**和

```mysql-sql
select * from students where age > 18 and age < 28;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
21 rows in set (0.00 sec)                                       
1234567891011121314151617181920212223242526
select * from students where age > 18 and gender = 'female';
1
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+
| id | NAME | age  | gender | cls_id | birth      | is_delete |
+----+------+------+--------+--------+------------+-----------+
| 11 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 13 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 15 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 18 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 20 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 27 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
+----+------+------+--------+--------+------------+-----------+
6 rows in set (0.00 sec)                                       
1234567891011
```

（2）**or**或

```mysql-sql
select * from students where id < 4 or is_delete = 0;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 |
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
3 rows in set (0.00 sec)                                        
12345678
```

（3）**not**取反

```mysql-sql
select * from students where not (age = 18 and gender = 'female');
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 |
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
26 rows in set (0.00 sec)
12345678910111213141516171819202122232425262728293031
```

又如

```mysql-sql
select * from students where (not age <= 18) and gender = 2;
1
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+
| id | NAME | age  | gender | cls_id | birth      | is_delete |
+----+------+------+--------+--------+------------+-----------+
| 11 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 13 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 15 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 18 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 20 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 27 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
+----+------+------+--------+--------+------------+-----------+
6 rows in set (0.00 sec)
1234567891011
```

等价于

```mysql-sql
select * from students where age > 18 and gender = 2;
1
```

结果与前者相同。
用**括号**解决优先级问题，会使代码可读性更高。

## 三、条件查询-模糊&范围查询&空判断

### 1.模糊查询：

（1）**like**关键字
**%**：替换0个或多个，即任意多个字符
**_**：替换1个字符

```mysql-sql
select * from students where name like 'J%';
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+ 
| id | NAME  | age  | gender | cls_id | birth      | is_delete | 
+----+-------+------+--------+--------+------------+-----------+ 
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
+----+-------+------+--------+--------+------------+-----------+ 
17 rows in set (0.00 sec)                                        
12345678910111213141516171819202122
select * from students where name like '%o%';
1
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+
| id | NAME | age  | gender | cls_id | birth      | is_delete |
+----+------+------+--------+--------+------------+-----------+
|  1 | Tom  |   18 | male   |      1 | 1999-09-09 |         1 |
|  4 | John |   19 | secret |      1 | 1999-07-09 |         1 |
|  6 | John |   16 | secret |      1 | 1999-05-09 |         1 |
|  7 | John |   19 | secret |      1 | 1999-07-09 |         1 |
|  8 | John |   19 | secret |      2 | 1999-07-09 |         1 |
|  9 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 10 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 11 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 12 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 13 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 14 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 15 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 16 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 17 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 18 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 19 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 20 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
| 21 | John |   19 | secret |      1 | 1999-07-09 |         1 |
| 22 | John |   19 | secret |      1 | 1999-07-09 |         1 |
| 23 | Tony |   18 | male   |      1 | 1990-01-01 |         1 |
| 24 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 25 | John |   16 | secret |      1 | 1999-05-09 |         1 |
| 26 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 27 | Rose |   19 | female |      2 | 1999-07-09 |         1 |
+----+------+------+--------+--------+------------+-----------+
24 rows in set (0.00 sec)                                      
1234567891011121314151617181920212223242526272829
```

查询字段值长度为3的记录：

```mysql-sql
select * from students where name like '___';
1
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+
| id | NAME | age  | gender | cls_id | birth      | is_delete |
+----+------+------+--------+--------+------------+-----------+
|  1 | Tom  |   18 | male   |      1 | 1999-09-09 |         1 |
+----+------+------+--------+--------+------------+-----------+
1 row in set (0.00 sec)
123456
```

查询长度至少为4的数据值：

```mysql-sql
select * from students where name like '____%';
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
25 rows in set (0.00 sec)
123456789101112131415161718192021222324252627282930
```

（2）**rlike**匹配正则表达式：

```mysql-sql
select * from students where name rlike '^J.*';
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
17 rows in set (0.00 sec)
12345678910111213141516171819202122
select * from students where name rlike '^J.*y$';
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
1 row in set (0.00 sec)
123456
```

### 2.范围查询：

（1）**in**表示在一个非连续的范围内

```mysql-sql
select * from students where age in (18,34);
1
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+
| id | NAME | age  | gender | cls_id | birth      | is_delete |
+----+------+------+--------+--------+------------+-----------+
|  1 | Tom  |   18 | male   |      1 | 1999-09-09 |         1 |
| 23 | Tony |   18 | male   |      1 | 1990-01-01 |         1 |
+----+------+------+--------+--------+------------+-----------+
2 rows in set (0.00 sec)
1234567
select * from students where name in ('Tom','John');
1
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+
| id | NAME | age  | gender | cls_id | birth      | is_delete |
+----+------+------+--------+--------+------------+-----------+
|  1 | Tom  |   18 | male   |      1 | 1999-09-09 |         1 |
|  4 | John |   19 | secret |      1 | 1999-07-09 |         1 |
|  6 | John |   16 | secret |      1 | 1999-05-09 |         1 |
|  7 | John |   19 | secret |      1 | 1999-07-09 |         1 |
|  8 | John |   19 | secret |      2 | 1999-07-09 |         1 |
|  9 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 10 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 12 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 14 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 16 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 17 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 19 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 21 | John |   19 | secret |      1 | 1999-07-09 |         1 |
| 22 | John |   19 | secret |      1 | 1999-07-09 |         1 |
| 24 | John |   19 | secret |      2 | 1999-07-09 |         1 |
| 25 | John |   16 | secret |      1 | 1999-05-09 |         1 |
| 26 | John |   19 | secret |      2 | 1999-07-09 |         1 |
+----+------+------+--------+--------+------------+-----------+
17 rows in set (0.00 sec)                                      
12345678910111213141516171819202122
```

（2）**not in**表示不在一个非连续的范围内

```mysql-sql
select * from students where age not in (18,34);
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+  
| id | NAME  | age  | gender | cls_id | birth      | is_delete |  
+----+-------+------+--------+--------+------------+-----------+  
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 |  
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |  
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |  
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |  
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |  
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 |  
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 |  
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |  
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 |  
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |  
+----+-------+------+--------+--------+------------+-----------+  
24 rows in set (0.00 sec)                                         
1234567891011121314151617181920212223242526272829
```

（3）**between … and …** 表示在一个连续的范围内

```mysql-sql
select * from students where id between 3 and 8;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
5 rows in set (0.00 sec)
12345678910
select * from students where (id between 3 and 8) and gender = 1;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
1 row in set (0.00 sec)
123456
```

（4）**not between … and …** 表示不在一个连续的范围内

```mysql-sql
select * from students where age not between 18 and 34;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete |
+----+-------+------+--------+--------+------------+-----------+
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |
+----+-------+------+--------+--------+------------+-----------+
3 rows in set (0.00 sec)                                        
12345678
```

等价于

```mysql-sql
select * from students where not age between 18 and 34;
1
```

### 3.空判断:

判空**is null**，不能用“=”。

```mysql-sql
select * from students where name is null;
1
```

打印

```mysql-sql
Empty set (0.00 sec)
1
select * from students where name is not null;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+ 
| id | NAME  | age  | gender | cls_id | birth      | is_delete | 
+----+-------+------+--------+--------+------------+-----------+ 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 
|  2 | Jerry |   19 | male   |      1 | 1999-10-09 |         1 | 
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
|  6 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
|  9 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 10 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 11 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 13 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 18 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 20 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 
+----+-------+------+--------+--------+------------+-----------+ 
26 rows in set (0.00 sec)                                        
12345678910111213141516171819202122232425262728293031
```

## 三、聚合函数

### 1.count()总数

```mysql-sql
select count(*) from students;
1
```

打印

```mysql-sql
+----------+
| count(*) |
+----------+
|       26 |
+----------+
1 row in set (0.00 sec)
123456
select count(*) as numofMale from students where gender = 1;
1
```

打印

```mysql-sql
+-----------+          
| numofMale |          
+-----------+          
|         4 |          
+-----------+          
1 row in set (0.00 sec)
123456
select count(*) as numofFemale from students where gender = 2;
1
```

打印

```mysql-sql
+-------------+
| numofFemale |
+-------------+
|           6 |
+-------------+
1 row in set (0.00 sec)
123456
```

### 2.max()最大值

```mysql-sql
select max(age) from students;
1
```

打印

```mysql-sql
+----------+
| max(age) |
+----------+
|       19 |
+----------+
1 row in set (0.00 sec)
123456
select max(id) from students where gender = 2;
1
```

打印

```mysql-sql
+---------+
| max(id) |
+---------+
|      27 |
+---------+
1 row in set (0.00 sec)
123456
```

### 3.min()最小值

```mysql-sql
select min(age) from students where is_delete = 1;
1
```

打印

```mysql-sql
+----------+
| min(age) |
+----------+
|       16 |
+----------+
1 row in set (0.00 sec)
123456
```

### 4.sum()求和

```mysql-sql
select sum(age) from students where gender = 1;
1
```

打印

```mysql-sql
+----------+           
| sum(age) |           
+----------+           
|       72 |           
+----------+           
1 row in set (0.00 sec)
123456
```

对于非数值型字段，如果全部数据均为字符型，求和为0，如果有部分为数字，会将所有数值进行相加。

```mysql-sql
select sum(name) from students;
1
```

打印

```mysql-sql
+-----------+
| sum(name) |
+-----------+
|         0 |
+-----------+
1 row in set, 26 warnings (0.01 sec)
123456
```

### 5.avg()平均值

默认保留4位小数，可以用`round(num,n)`可以使num保留n位小数。

```mysql-sql
select avg(age) from students where gender = 2 and is_delete = 1;
1
```

打印

```mysql-sql
+----------+           
| avg(age) |           
+----------+           
|  19.0000 |           
+----------+           
1 row in set (0.00 sec)
123456
select round(avg(age),2) from students where gender = 2 and is_delete = 1;
1
```

打印

```mysql-sql
+-------------------+  
| round(avg(age),2) |  
+-------------------+  
|             19.00 |  
+-------------------+  
1 row in set (0.00 sec)
```

# Python全栈（三）数据库优化之4.数据库查询（分组、排序、分页、连接查询、子查询）

## 一、分组与分组后的筛选

### 1.分组

关键字：group by
将查询结果按照1个或多个字段进行分组，字段值相同的为一组；
可用于单个字段分组，也可用于多个字段分组
语法：`select ... from 表 group by 组别;`

```mysql-sql
select name from students group by gender;
1
```

打印

```mysql-sql
ERROR 1055 (42000): Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'demo.students.NAME' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
1
```

报错，并且没有实际意义，即select后接的是能区分这个组的字段，一般是聚合函数。

```mysql-sql
select gender from students group by gender;
1
```

打印

```mysql-sql
+--------+              
| gender |              
+--------+              
| male   |              
| female |              
| secret |              
+--------+              
3 rows in set (0.01 sec)
12345678
```

统计每个组的数量

```mysql-sql
select gender,count(*) from students group by gender;
1
```

打印

```mysql-sql
+--------+----------+   
| gender | count(*) |   
+--------+----------+   
| male   |        4 |   
| female |        6 |   
| secret |       17 |   
+--------+----------+   
3 rows in set (0.00 sec)
12345678
```

换别名

```mysql-sql
select gender as sex,count(*) from students group by gender;
1
```

打印

```mysql-sql
+--------+----------+
| sex    | count(*) |
+--------+----------+
| male   |        4 |
| female |        6 |
| secret |       17 |
+--------+----------+
3 rows in set (0.00 sec)
12345678
```

查找各个组的最值

```mysql-sql
select gender as sex,max(age) from students group by gender;
1
```

打印

```mysql-sql
+--------+----------+   
| sex    | max(age) |   
+--------+----------+   
| male   |       19 |   
| female |       19 |   
| secret |       19 |   
+--------+----------+   
3 rows in set (0.00 sec)
12345678
```

查看组内信息：
`group_concat(...)`字段，可以作为一个输出字段来使用；
表示分组之后，根据分组结果，使用group_concat()来放置每一组的某字段的值的集合，相对于对字符串进行拼接。

```mysql-sql
select gender as sex,group_concat(name) from students group by gender;
1
```

打印

```mysql-sql
+--------+--------------------------------------------------------------------------------------+
| sex    | group_concat(name)                                                                   |
+--------+--------------------------------------------------------------------------------------+
| male   | Tom,Jerry,Nancy,Tony                                                                 |
| female | Rose,Rose,Rose,Rose,Rose,Rose                                                        |
| secret | John,John,John,John,John,John,John,John,John,John,John,John,John,John,John,John,null |
+--------+--------------------------------------------------------------------------------------+
3 rows in set (0.00 sec)
12345678
```

查看多个字段

```mysql-sql
select gender as sex,group_concat(name,'-',age) from students group by gender;
1
```

打印

```mysql-sql
+--------+---------------------------------------------------------------------------------------------------------
--------------------------------+                                                                                  
| sex    | group_concat(name,'-',age)                                                                              
                                |                                                                                  
+--------+---------------------------------------------------------------------------------------------------------
--------------------------------+                                                                                  
| male   | Tom-18,Jerry-19,Nancy-17,Tony-18                                                                        
                                |                                                                                  
| female | Rose-19,Rose-19,Rose-19,Rose-19,Rose-19,Rose-19                                                         
                                |                                                                                  
| secret | John-19,John-16,John-19,John-19,John-19,John-19,John-19,John-19,John-19,John-19,John-19,John-19,John-19,
John-19,John-16,John-19,null-18 |                                                                                  
+--------+---------------------------------------------------------------------------------------------------------
--------------------------------+                                                                                  
3 rows in set (0.00 sec)                                                                                           
123456789101112131415
```

### 2.分组之后的筛选

分组之后根据条件进行筛选不能再用where语句，要用having语句；
having 条件表达式用来分组查询后指定一些条件来输出查询结果。

```mysql-sql
--查询性别分组中人数大于5的组别
select gender,count(*) from students group by gender having count(*) > 5;
12
```

打印

```mysql-sql
+--------+----------+   
| gender | count(*) |   
+--------+----------+   
| female |        6 |   
| secret |       17 |   
+--------+----------+   
2 rows in set (0.00 sec)
1234567
--查询男生女生人数大于2的姓名
select gender,count(*),group_concat(name) from students group by gender having count(*) > 2;
12
```

打印

```mysql-sql
+--------+----------+--------------------------------------------------------------------------------------+
| gender | count(*) | group_concat(name)                                                                   |
+--------+----------+--------------------------------------------------------------------------------------+
| male   |        4 | Tom,Jerry,Nancy,Tony                                                                 |
| female |        6 | Rose,Rose,Rose,Rose,Rose,Rose                                                        |
| secret |       17 | John,John,John,John,John,John,John,John,John,John,John,John,John,John,John,John,null |
+--------+----------+--------------------------------------------------------------------------------------+
3 rows in set (0.01 sec)
12345678
--查询平均年龄超过18岁的姓名，以及姓名
select gender,avg(age),group_concat(name) from students group by gender having avg(age) > 18;
12
```

打印

```mysql-sql
+--------+----------+--------------------------------------------------------------------------------------+
| gender | avg(age) | group_concat(name)                                                                   |
+--------+----------+--------------------------------------------------------------------------------------+
| female |  19.0000 | Rose,Rose,Rose,Rose,Rose,Rose                                                        |
| secret |  18.5882 | John,John,John,John,John,John,John,John,John,John,John,John,John,John,John,John,null |
+--------+----------+--------------------------------------------------------------------------------------+
2 rows in set (0.00 sec)
1234567
```

having作用和where一样，但having只能用于group by。

## 二、排序

为了方便查看数据，可以对数据进行排序。
关键字：order by
语法：`select * from 表名 order by 列1 asc|desc [,列2 asc|desc,...];`
将行数据按照列1进行排序，如果某些行列1的值相同时，则按照列2排序，以此类推；
asc从小到大排列，即升序，是默认值；
desc从大到小排列，即降序。

```mysql-sql
--查询年龄在18-26岁之间的男同学，按照年龄从小到大排序
select * from students where (age between 18 and 26) and gender =1 order by age;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
3 rows in set (0.01 sec)
12345678
```

与

```mysql-sql
select * from students where (age between 18 and 26) and gender =1 order by age asc;
1
```

效果一样。

```mysql-sql
--查询年龄在18-26岁之间的女同学，按照身高从低到高排序
select * from students where (age between 18 and 26) and gender =1 order by height;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
3 rows in set (0.00 sec)                                                 
12345678
```

order by排序多个字段：
排序的字段相同时，再按照后边的字段进行排序。

```mysql-sql
--查询年龄在18-26岁之间的男同学，按照身高从高到低排序，身高相同时按照年龄从小到大排序
select * from students where (age between 18 and 26) and gender =1 order by height desc,age asc;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+ 
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height | 
+----+-------+------+--------+--------+------------+-----------+--------+ 
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 | 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 | 
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 | 
+----+-------+------+--------+--------+------------+-----------+--------+ 
3 rows in set (0.00 sec)                                                  
12345678
--按照年龄从大到小、身高从低到高排序
select * from students order by age desc,height asc;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
27 rows in set (0.00 sec)
1234567891011121314151617181920212223242526272829303132
```

## 三、分页与制作分页

### 1.分页简介

当数据量过大时，在一页中查看数据是一件非常麻烦的事情，我们可以限制查出来的数据个数，即限制显示条数，这就是分页。
语法：`select * from 表名 limit start,count;`
–start起始的位置，从0开始，count个数，表示从start开始，获取count条数据。

```mysql-sql
select * from students limit 2;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
2 rows in set (0.01 sec)
1234567
```

显示了前2条数据。

```mysql-sql
select * from students limit 5,5;
1
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+--------+
| id | NAME | age  | gender | cls_id | birth      | is_delete | height |
+----+------+------+--------+--------+------------+-----------+--------+
|  7 | John |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |
|  8 | John |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
|  9 | John |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |
| 10 | John |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |
| 11 | Rose |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |
+----+------+------+--------+--------+------------+-----------+--------+
5 rows in set (0.00 sec)                                                
12345678910
```

查询了第6-10条数据。

### 2.制作分页

```mysql-sql
--每页显示2个，第1页
select * from students limit 0,2;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
2 rows in set (0.00 sec)                                                 
1234567
--每页显示2个，第2页
select * from students limit 2,2;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
2 rows in set (0.00 sec)                                                 
1234567
--每页显示2个，第3页
select * from students limit 4,2;
12
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+--------+
| id | NAME | age  | gender | cls_id | birth      | is_delete | height |
+----+------+------+--------+--------+------------+-----------+--------+
|  6 | John |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |
|  7 | John |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |
+----+------+------+--------+--------+------------+-----------+--------+
2 rows in set (0.00 sec)
1234567
--每页显示2个，第4页
select * from students limit 6,2;
12
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+--------+
| id | NAME | age  | gender | cls_id | birth      | is_delete | height |
+----+------+------+--------+--------+------------+-----------+--------+
|  8 | John |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
|  9 | John |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |
+----+------+------+--------+--------+------------+-----------+--------+
2 rows in set (0.00 sec)
1234567
```

可总结出规律：
第n页：`limit (n-1)*每页显示个数`，但是不能在SQL语句中直接使用公式，要提前算出数值。
limit语句只能放在**SQL语句最后**，只有先查询到数据才能分页。

```mysql-sql
select gender from students group by gender limit 0,2;
1
```

打印

```mysql-sql
+--------+              
| gender |              
+--------+              
| male   |              
| female |              
+--------+              
2 rows in set (0.00 sec)
1234567
```

## 四、连接查询

当查询结果的列来源于多张表时，需要将多张表连接成一个大的数据集，再选择合适的列返回。
在MySQL中支持三种类型的连接查询：

### 1.内连接查询：

查询的结果为两个表匹配到的数据，如下图所示：
![内连接](https://img-blog.csdnimg.cn/20191203201129337.png#pic_center)
语法：`select ... from 表A inner join 表B on 表1.列 = 表2.列;`

```mysql-sql
select * from students inner join classes;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+----+--------+ 
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height | id | name   | 
+----+-------+------+--------+--------+------------+-----------+--------+----+--------+ 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |  1 | class1 | 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |  2 | class2 | 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |  3 | class3 | 
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |  1 | class1 | 
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |  2 | class2 | 
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |  3 | class3 | 
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |  1 | class1 | 
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |  2 | class2 | 
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |  3 | class3 | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |  1 | class1 | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |  2 | class2 | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |  3 | class3 | 
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |  1 | class1 | 
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |  2 | class2 | 
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |  3 | class3 | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |  1 | class1 | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |  2 | class2 | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |  3 | class3 | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  1 | class1 | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  2 | class2 | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  3 | class3 | 
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |  1 | class1 | 
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |  2 | class2 | 
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |  3 | class3 | 
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |  1 | class1 | 
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |  2 | class2 | 
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |  3 | class3 | 
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |  1 | class1 | 
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |  2 | class2 | 
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |  3 | class3 | 
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  1 | class1 | 
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  2 | class2 | 
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  3 | class3 | 
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |  1 | class1 | 
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |  2 | class2 | 
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |  3 | class3 | 
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  1 | class1 | 
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  2 | class2 | 
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  3 | class3 | 
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |  1 | class1 | 
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |  2 | class2 | 
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |  3 | class3 | 
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  1 | class1 | 
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  2 | class2 | 
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  3 | class3 | 
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |  1 | class1 | 
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |  2 | class2 | 
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |  3 | class3 | 
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |  1 | class1 | 
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |  2 | class2 | 
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |  3 | class3 | 
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |  1 | class1 | 
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |  2 | class2 | 
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |  3 | class3 | 
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |  1 | class1 | 
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |  2 | class2 | 
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |  3 | class3 | 
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |  1 | class1 | 
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |  2 | class2 | 
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |  3 | class3 | 
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |  1 | class1 | 
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |  2 | class2 | 
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |  3 | class3 | 
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |  1 | class1 | 
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |  2 | class2 | 
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |  3 | class3 | 
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |  1 | class1 | 
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |  2 | class2 | 
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |  3 | class3 | 
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |  1 | class1 | 
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |  2 | class2 | 
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |  3 | class3 | 
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |  1 | class1 | 
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |  2 | class2 | 
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |  3 | class3 | 
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |  1 | class1 | 
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |  2 | class2 | 
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |  3 | class3 | 
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |  1 | class1 | 
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |  2 | class2 | 
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |  3 | class3 | 
+----+-------+------+--------+--------+------------+-----------+--------+----+--------+ 
81 rows in set (0.00 sec)                                                               
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283848586
```

得到的是两个表的笛卡儿积，但是一般无意义。

```mysql-sql
--查询对应班级的学生及班级信息
select * from students inner join classes on students.cls_id = classes.id;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+----+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height | id | name   |
+----+-------+------+--------+--------+------------+-----------+--------+----+--------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |  1 | class1 |
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |  3 | class3 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |  2 | class2 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |  1 | class1 |
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |  3 | class3 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |  1 | class1 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  2 | class2 |
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |  3 | class3 |
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |  3 | class3 |
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |  1 | class1 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  2 | class2 |
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |  1 | class1 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  2 | class2 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |  2 | class2 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |  2 | class2 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |  2 | class2 |
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |  3 | class3 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |  2 | class2 |
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |  3 | class3 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |  1 | class1 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |  1 | class1 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |  1 | class1 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |  2 | class2 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |  1 | class1 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |  2 | class2 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |  2 | class2 |
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |  1 | class1 |
+----+-------+------+--------+--------+------------+-----------+--------+----+--------+
27 rows in set (0.00 sec)                                                              
1234567891011121314151617181920212223242526272829303132
```

去除重复信息

```mysql-sql
--查询对应班级的学生及班级信息
select students.*,classes.name from students inner join classes on students.cls_id = classes.id;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+--------+ 
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height | name   | 
+----+-------+------+--------+--------+------------+-----------+--------+--------+ 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 | class1 | 
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 | class3 | 
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 | class2 | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 | class1 | 
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 | class3 | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 | class1 | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | class2 | 
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 | class3 | 
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 | class3 | 
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 | class1 | 
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | class2 | 
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 | class1 | 
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | class2 | 
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 | class2 | 
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | class2 | 
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 | class2 | 
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 | class3 | 
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 | class2 | 
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 | class3 | 
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 | class1 | 
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 | class1 | 
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 | class1 | 
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 | class2 | 
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 | class1 | 
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 | class2 | 
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 | class2 | 
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 | class1 | 
+----+-------+------+--------+--------+------------+-----------+--------+--------+ 
27 rows in set (0.00 sec)                                                          
1234567891011121314151617181920212223242526272829303132
```

换别名

```mysql-sql
--查询对应班级的学生及班级信息
select s.*,c.name from students as s inner join classes as c on s.cls_id = c.id;
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height | name   |
+----+-------+------+--------+--------+------------+-----------+--------+--------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 | class1 |
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 | class3 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 | class2 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 | class1 |
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 | class3 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 | class1 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | class2 |
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 | class3 |
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 | class3 |
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 | class1 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | class2 |
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 | class1 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | class2 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 | class2 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | class2 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 | class2 |
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 | class3 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 | class2 |
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 | class3 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 | class1 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 | class1 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 | class1 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 | class2 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 | class1 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 | class2 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 | class2 |
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 | class1 |
+----+-------+------+--------+--------+------------+-----------+--------+--------+
27 rows in set (0.00 sec)                                                         
1234567891011121314151617181920212223242526272829303132
--查询对应班级的学生及班级信息
select c.name,s.* from students as s inner join classes as c on s.cls_id = c.id;
12
```

打印

```mysql-sql
+--------+----+-------+------+--------+--------+------------+-----------+--------+
| name   | id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+--------+----+-------+------+--------+--------+------------+-----------+--------+
| class1 |  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
| class3 |  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
| class2 |  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |
| class1 |  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| class3 |  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |
| class1 |  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |
| class2 |  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class3 |  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |
| class3 | 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |
| class1 | 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |
| class2 | 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class1 | 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |
| class2 | 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |
| class2 | 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |
| class3 | 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |
| class2 | 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |
| class3 | 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |
| class1 | 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |
| class1 | 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| class1 | 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |
| class2 | 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |
| class1 | 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |
| class2 | 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |
| class2 | 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |
| class1 | 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |
+--------+----+-------+------+--------+--------+------------+-----------+--------+
27 rows in set (0.00 sec)                                                         
1234567891011121314151617181920212223242526272829303132
```

加入排序

```mysql-sql
select c.name,s.* from students as s inner join classes as c on s.cls_id = c.id order by c.name;
1
```

打印

```mysql-sql
+--------+----+-------+------+--------+--------+------------+-----------+--------+
| name   | id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+--------+----+-------+------+--------+--------+------------+-----------+--------+
| class1 | 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |
| class1 |  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
| class1 |  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |
| class1 | 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |
| class1 | 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |
| class1 | 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| class1 | 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |
| class1 |  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| class1 | 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |
| class1 | 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |
| class2 | 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |
| class2 |  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |
| class2 | 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |
| class2 | 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |
| class2 | 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |
| class2 | 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |
| class2 |  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |
| class2 | 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class3 |  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
| class3 | 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |
| class3 |  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |
| class3 | 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |
| class3 | 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |
| class3 |  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |
+--------+----+-------+------+--------+--------+------------+-----------+--------+
27 rows in set (0.00 sec)                                                         
1234567891011121314151617181920212223242526272829303132
select c.name,s.* from students as s inner join classes as c on s.cls_id = c.id order by c.name,s.id asc;
1
```

打印

```mysql-sql
+--------+----+-------+------+--------+--------+------------+-----------+--------+
| name   | id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+--------+----+-------+------+--------+--------+------------+-----------+--------+
| class1 |  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
| class1 |  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| class1 |  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |
| class1 | 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |
| class1 | 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |
| class1 | 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |
| class1 | 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| class1 | 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |
| class1 | 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |
| class1 | 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |
| class2 |  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |
| class2 |  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |
| class2 | 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| class2 | 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |
| class2 | 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |
| class2 | 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |
| class2 | 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |
| class2 | 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |
| class3 |  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
| class3 |  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |
| class3 |  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |
| class3 | 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |
| class3 | 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |
| class3 | 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |
+--------+----+-------+------+--------+--------+------------+-----------+--------+
27 rows in set (0.00 sec)
1234567891011121314151617181920212223242526272829303132
```

### 2.左连接查询

查询的结果为两个表匹配到的数据，左表特有的数据，对于右表中不存在的数据使用null填充，如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191203201107744.png#pic_center)
语法：`select ... from 表A left join 表B on 表1.列 = 表2.列;`
以第一个表为主，应用比右连接更多。

```mysql-sql
select * from students left join classes on students.cls_id = classes.id;
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height | id   | name   |
+----+-------+------+--------+--------+------------+-----------+--------+------+--------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |    1 | class1 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |    1 | class1 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |    1 | class1 |
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |    1 | class1 |
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |    1 | class1 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |    1 | class1 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |    1 | class1 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |    1 | class1 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |    1 | class1 |
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |    1 | class1 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |    2 | class2 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |    2 | class2 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |    2 | class2 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |    2 | class2 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |    2 | class2 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |    2 | class2 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |    2 | class2 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |    2 | class2 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |    2 | class2 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |    2 | class2 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |    2 | class2 |
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |    3 | class3 |
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |    3 | class3 |
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |    3 | class3 |
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |    3 | class3 |
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |    3 | class3 |
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |    3 | class3 |
+----+-------+------+--------+--------+------------+-----------+--------+------+--------+
27 rows in set (0.01 sec)
1234567891011121314151617181920212223242526272829303132
select * from students as s left join classes as c on s.cls_id = c.id having c.id is null;
1
```

打印

```mysql-sql
Empty set (0.00 sec)
1
```

与

```mysql-sql
select * from students as s left join classes as c on s.cls_id = c.id where c.id is null;
1
```

效果相同。
连接后筛选用的关键字是having，也可以用where，但是建议用having。

```mysql-sql
select c.name,s.* from students as s left join classes as c on s.cls_id = c.id having c.id is null and s.is_delete = 1;
1
```

打印

```mysql-sql
Empty set (0.00 sec)
1
```

### 3.右连接

查询的结果为两个表匹配到的数据，右表特有的数据，对于左表中不存在的数据使用null填充，如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019120320104157.png#pic_center)
语法：`select ... from 表A right join 表B on 表1.列 = 表2.列;`
以第二个表为主，应用比左连接更少，一般可以转化成左连接。

```mysql-sql
select * from students right join classes on students.cls_id = classes.id;
1
```

打印

```mysql-sql
+------+-------+------+--------+--------+------------+-----------+--------+----+--------+
| id   | NAME  | age  | gender | cls_id | birth      | is_delete | height | id | name   |
+------+-------+------+--------+--------+------------+-----------+--------+----+--------+
|    1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 |    165 |  1 | class1 |
|    2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 |    157 |  3 | class3 |
|    3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 |    165 |  2 | class2 |
|    4 | John  |   19 | secret |      1 | 1999-07-09 |         1 |    168 |  1 | class1 |
|    6 | John  |   16 | secret |      3 | 1999-05-09 |         1 |    165 |  3 | class3 |
|    7 | John  |   19 | secret |      1 | 1999-07-09 |         1 |    176 |  1 | class1 |
|    8 | John  |   19 | secret |      2 | 1999-07-09 |         1 |    165 |  2 | class2 |
|    9 | John  |   19 | secret |      3 | 1999-07-09 |         1 |    153 |  3 | class3 |
|   10 | John  |   19 | secret |      3 | 1999-07-09 |         1 |    165 |  3 | class3 |
|   11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 |    169 |  1 | class1 |
|   12 | John  |   19 | secret |      2 | 1999-07-09 |         1 |    165 |  2 | class2 |
|   13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 |    158 |  1 | class1 |
|   14 | John  |   19 | secret |      2 | 1999-07-09 |         1 |    165 |  2 | class2 |
|   15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |    183 |  2 | class2 |
|   16 | John  |   19 | secret |      2 | 1999-07-09 |         1 |    165 |  2 | class2 |
|   17 | John  |   19 | secret |      2 | 1999-07-09 |         1 |    173 |  2 | class2 |
|   18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 |    165 |  3 | class3 |
|   19 | John  |   19 | secret |      2 | 1999-07-09 |         1 |    159 |  2 | class2 |
|   20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 |    176 |  3 | class3 |
|   21 | John  |   19 | secret |      1 | 1999-07-09 |         1 |    157 |  1 | class1 |
|   22 | John  |   19 | secret |      1 | 1999-07-09 |         1 |    168 |  1 | class1 |
|   23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 |    173 |  1 | class1 |
|   24 | John  |   19 | secret |      2 | 1999-07-09 |         1 |    175 |  2 | class2 |
|   25 | John  |   16 | secret |      1 | 1999-05-09 |         1 |    164 |  1 | class1 |
|   26 | John  |   19 | secret |      2 | 1999-07-09 |         1 |    160 |  2 | class2 |
|   27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 |    171 |  2 | class2 |
|   28 | null  |   18 | secret |      1 | 1990-01-01 |         0 |    169 |  1 | class1 |
+------+-------+------+--------+--------+------------+-----------+--------+----+--------+
27 rows in set (0.02 sec)                                                                
1234567891011121314151617181920212223242526272829303132
```

## 五、子查询

即嵌套查询，在一个select语句中嵌套另一个select语句，被嵌入的select语句称之为子查询语句。

```mysql-sql
--查询最高的男生信息
select * from students where height = (
    select max(height) from students where gender = 1) and gender = 1;
123
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+--------+  
| id | NAME | age  | gender | cls_id | birth      | is_delete | height |  
+----+------+------+--------+--------+------------+-----------+--------+  
| 23 | Tony |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |  
+----+------+------+--------+--------+------------+-----------+--------+  
1 row in set (0.00 sec)                                                   
123456
--查询高于平均身高的信息
select * from students where height > (select avg(height) from students);
12
```

打印

```mysql-sql
+----+------+------+--------+--------+------------+-----------+--------+
| id | NAME | age  | gender | cls_id | birth      | is_delete | height |
+----+------+------+--------+--------+------------+-----------+--------+
|  4 | John |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
|  7 | John |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |
| 11 | Rose |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |
| 15 | Rose |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |
| 17 | John |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |
| 20 | Rose |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |
| 22 | John |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| 23 | Tony |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |
| 24 | John |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |
| 27 | Rose |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |
| 28 | null |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |
+----+------+------+--------+--------+------------+-----------+--------+
11 rows in set (0.01 sec)                                               
12345678910111213141516
```

### 列级子查询

与内连接效果一样。

```mysql-sql
--查询学生的班级号能够对应的学生信息
select * from students where cls_id in (select id from classes);
12
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 |
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 |
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 |
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 |
| 12 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| 13 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 158.00 |
| 14 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| 15 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 183.00 |
| 16 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 |
| 17 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 173.00 |
| 18 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 165.00 |
| 19 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 159.00 |
| 20 | Rose  |   19 | female |      3 | 1999-07-09 |         1 | 176.00 |
| 21 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 157.00 |
| 22 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 |
| 23 | Tony  |   18 | male   |      1 | 1990-01-01 |         1 | 173.00 |
| 24 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 175.00 |
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |
| 26 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 160.00 |
| 27 | Rose  |   19 | female |      2 | 1999-07-09 |         1 | 171.00 |
| 28 | null  |   18 | secret |      1 | 1990-01-01 |         0 | 169.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
27 rows in set (0.00 sec)
1234567891011121314151617181920212223242526272829303132
--查询最大年龄的女性id
select id,name from students where age = (select max(age) from students where gender = 2) and gender = 2;
12
```

打印

```mysql-sql
+----+------+
| id | name |
+----+------+
| 11 | Rose |
| 13 | Rose |
| 15 | Rose |
| 18 | Rose |
| 20 | Rose |
| 27 | Rose |
+----+------+
6 rows in set (0.00 sec)
1234567891011
```

# Python全栈（三）数据库优化之5.MySQL自关联、外键与Python操作MySQL

## 一、自关联

引入：省市区三级联动数据
导入外部.sql文件的数据：
从SQLyog选择文件导入并执行SQL语句，导入了三张表（省、市、县区）的数据。
读者需要练习使用省市区数据执行脚本可以打开文末的网盘链接进行下载。

```mysql-sql
-查询湖南省
select * from provinces where province = '湖南省';
12
```

打印

```mysql-sql
+----+------------+-----------+
| id | provinceid | province  |
+----+------------+-----------+
| 18 |     430000 | 湖南省    |
+----+------------+-----------+
1 row in set (0.00 sec)
123456
-查询湖南省下面的市
select * from cities where provinceid = 430000;
12
```

打印

```mysql-sql
+-----+--------+--------------------------------+------------+
| id  | cityid | city                           | provinceid |
+-----+--------+--------------------------------+------------+
| 186 | 430100 | 长沙市                         | 430000     |   
| 187 | 430200 | 株洲市                         | 430000     |   
| 188 | 430300 | 湘潭市                         | 430000     |   
| 189 | 430400 | 衡阳市                         | 430000     |   
| 190 | 430500 | 邵阳市                         | 430000     |   
| 191 | 430600 | 岳阳市                         | 430000     |   
| 192 | 430700 | 常德市                         | 430000     |   
| 193 | 430800 | 张家界市                       | 430000     |    
| 194 | 430900 | 益阳市                         | 430000     |   
| 195 | 431000 | 郴州市                         | 430000     |   
...         
+-----+--------+--------------------------------+------------+
14 rows in set, 5 warnings (0.00 sec)                         
12345678910111213141516
```

嵌套子查询

```mysql-sql
select * from provinces as p inner join cities as c on p.provinceid = c.provinceid having p.province = '湖南省';
1
```

打印

```mysql-sql
+----+------------+-----------+-----+--------+--------------------------------+------------+   
| id | provinceid | province  | id  | cityid | city                           | provinceid |   
+----+------------+-----------+-----+--------+--------------------------------+------------+   
| 18 |     430000 | 湖南省    | 186 | 430100 | 长沙市                         | 430000     |         
| 18 |     430000 | 湖南省    | 187 | 430200 | 株洲市                         | 430000     |         
| 18 |     430000 | 湖南省    | 188 | 430300 | 湘潭市                         | 430000     |         
| 18 |     430000 | 湖南省    | 189 | 430400 | 衡阳市                         | 430000     |         
| 18 |     430000 | 湖南省    | 190 | 430500 | 邵阳市                         | 430000     |         
| 18 |     430000 | 湖南省    | 191 | 430600 | 岳阳市                         | 430000     |         
| 18 |     430000 | 湖南省    | 192 | 430700 | 常德市                         | 430000     |         
| 18 |     430000 | 湖南省    | 193 | 430800 | 张家界市                       | 430000     |          
| 18 |     430000 | 湖南省    | 194 | 430900 | 益阳市                         | 430000     |         
| 18 |     430000 | 湖南省    | 195 | 431000 | 郴州市                         | 430000     |         
...
+----+------------+-----------+-----+--------+--------------------------------+------------+   
14 rows in set, 170 warnings (0.00 sec)                                                        
12345678910111213141516
```

查询areas一张表

```mysql-sql
-查询湖南省
select * from areas where name = '湖南';
12
```

打印

```mysql-sql
+----+-----+--------+------+
| id | pid | name   | type |
+----+-----+--------+------+
| 14 |   1 | 湖南   |    1 |
+----+-----+--------+------+
1 row in set (0.00 sec)
123456
-查询湖南省下面的市
select * from areas where pid = 14;
12
```

打印

```mysql-sql
+-----+-----+-----------+------+
| id  | pid | name      | type |
+-----+-----+-----------+------+
| 197 |  14 | 长沙      |    2 |  
| 198 |  14 | 张家界    |    2 |   
| 199 |  14 | 常德      |    2 |  
| 200 |  14 | 郴州      |    2 |  
| 201 |  14 | 衡阳      |    2 |  
| 202 |  14 | 怀化      |    2 |  
| 203 |  14 | 娄底      |    2 |  
| 204 |  14 | 邵阳      |    2 |  
| 205 |  14 | 湘潭      |    2 |  
| 206 |  14 | 湘西      |    2 |  
...
+-----+-----+-----------+------+
14 rows in set (0.00 sec)       
12345678910111213141516
-查询长沙 市下面的区
select * from areas where pid = 197;
12
```

打印

```mysql-sql
+------+-----+-----------+------+
| id   | pid | name      | type |
+------+-----+-----------+------+
| 1647 | 197 | 岳麓区    |    3 |
| 1648 | 197 | 芙蓉区    |    3 |
| 1649 | 197 | 天心区    |    3 |
| 1650 | 197 | 开福区    |    3 |
| 1651 | 197 | 雨花区    |    3 |
| 1652 | 197 | 开发区    |    3 |
| 1653 | 197 | 浏阳市    |    3 |
| 1654 | 197 | 长沙县    |    3 |
| 1655 | 197 | 望城县    |    3 |
| 1656 | 197 | 宁乡县    |    3 |
+------+-----+-----------+------+
10 rows in set (0.00 sec)
123456789101112131415
```

内连接查询

```mysql-sql
-查询湖南省下面的市
select * from areas as p inner join areas as c on p.id = c.pid having p.name = '湖南';
12
```

打印

```mysql-sql
+----+-----+--------+------+-----+-----+-----------+------+  
| id | pid | name   | type | id  | pid | name      | type |  
+----+-----+--------+------+-----+-----+-----------+------+  
| 14 |   1 | 湖南   |    1 | 197 |  14 | 长沙      |    2 |      
| 14 |   1 | 湖南   |    1 | 198 |  14 | 张家界    |    2 |       
| 14 |   1 | 湖南   |    1 | 199 |  14 | 常德      |    2 |      
| 14 |   1 | 湖南   |    1 | 200 |  14 | 郴州      |    2 |      
| 14 |   1 | 湖南   |    1 | 201 |  14 | 衡阳      |    2 |      
| 14 |   1 | 湖南   |    1 | 202 |  14 | 怀化      |    2 |      
| 14 |   1 | 湖南   |    1 | 203 |  14 | 娄底      |    2 |      
| 14 |   1 | 湖南   |    1 | 204 |  14 | 邵阳      |    2 |      
| 14 |   1 | 湖南   |    1 | 205 |  14 | 湘潭      |    2 |      
| 14 |   1 | 湖南   |    1 | 206 |  14 | 湘西      |    2 |      
...    
+----+-----+--------+------+-----+-----+-----------+------+  
14 rows in set (0.00 sec)                                    
12345678910111213141516
```

此即自关联，同一个表关联查询。
自连结其实就是连结查询，需要两张表，只不过它的左表（主表）和右表（子表）都是自己。在做自连接查询的时候是自己链接自己，分别给主表和子表取别名，再附加条件执行。

```mysql-sql
-查询长沙市下面的区
select * from areas as c inner join areas as a on c.id = a.pid having c.name = '哈尔滨';
12
```

打印

```mysql-sql
+-----+-----+--------+------+------+-----+-----------+------+ 
| id  | pid | name   | type | id   | pid | name      | type | 
+-----+-----+--------+------+------+-----+-----------+------+ 
| 197 |  14 | 长沙   |    2 | 1647 | 197 | 岳麓区    |    3 |      
| 197 |  14 | 长沙   |    2 | 1648 | 197 | 芙蓉区    |    3 |      
| 197 |  14 | 长沙   |    2 | 1649 | 197 | 天心区    |    3 |      
| 197 |  14 | 长沙   |    2 | 1650 | 197 | 开福区    |    3 |      
| 197 |  14 | 长沙   |    2 | 1651 | 197 | 雨花区    |    3 |      
| 197 |  14 | 长沙   |    2 | 1652 | 197 | 开发区    |    3 |      
| 197 |  14 | 长沙   |    2 | 1653 | 197 | 浏阳市    |    3 |      
| 197 |  14 | 长沙   |    2 | 1654 | 197 | 长沙县    |    3 |      
| 197 |  14 | 长沙   |    2 | 1655 | 197 | 望城县    |    3 |      
| 197 |  14 | 长沙   |    2 | 1656 | 197 | 宁乡县    |    3 |      
+-----+-----+--------+------+------+-----+-----------+------+ 
10 rows in set (0.01 sec)                                     
123456789101112131415
```

## 二、外键

一个健壮的数据库一定有很好的参照完整性。为了保证数据的完整性，将两张表之间的数据建立关系，因此就需要在表中添加外键约束。
创建两张表：

```mysql-sql
create table classes(
    id int(4) not null primary key,
    name varchar(36)
);
create table student(
    sid int(4) not null primary key,
    sname varchar(36),
    gid int(4) not null
);
123456789
```

添加外键：
语法：`alert table 表名 add constraint 外键名字 foreign key(外键字段名) references 外表表名(主键字段名)`;
对student表添加外键

```mysql-sql
alter table student add constraint fk_gid foreign key(gid) references classes(id);
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

引入外键之后，外键列只能插入参照列存在的值，参照列被参照的值不能被删除，这就保证了数据的参照完整性。
验证外键的作用：

```mysql-sql
insert into student (sid, sname, gid) values(1, 'Tom', 1);
1
```

打印

```mysql-sql
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`demo1125`.`student`, CONSTRAINT `fk_gid` FOREIGN KEY (`gid`) REFERENCES `classes` (`id`))
1
```

很显然插入失败，是外键限制，应该先在classes中插入一个班级，才能在学生中插入记录。

```mysql-sql
insert into classes values(1,'class1');
1
```

打印

```mysql-sql
Query OK, 1 row affected (0.01 sec)
1
```

此时再执行之前插入失败的语句

```mysql-sql
insert into student (sid, sname, gid) values(1, 'Tom', 1);
1
```

打印

```mysql-sql
Query OK, 1 row affected (0.00 sec)
1
```

即插入成功。
删除数据时，比如删除1班

```mysql-sql
delete from classes where id = 1;
1
```

打印

```mysql-sql
ERROR 1451 (23000): Cannot delete or update a parent row: a foreign key constraint fails (`demo1125`.`student`, CONSTRAINT `fk_gid` FOREIGN KEY (`gid`) REFERENCES `classes` (`id`))
1
```

即删除失败
要想删除班级，这里应该先删除班级为1的学生

```mysql-sql
delete from student where sid = 1;
1
```

打印

```mysql-sql
Query OK, 1 row affected (0.01 sec)
1
```

此时再执行之前删除失败的语句

```mysql-sql
delete from classes where id = 1;
1
```

打印

```mysql-sql
Query OK, 1 row affected (0.00 sec)
1
```

即删除成功。
在MySQL中，有4种针对存在外键的数据表进行操作时的设置：Cascade（级联）、No Action（不做操作）、Restrict（限制，默认）、Set null（设为空）。
删除外键约束：
`alter table 表名 drop foreign key 外键名;`
PS：有可能在加入外键约束后插入数据不会报错，这是因为MySQL安装时默认用的表引擎是MyISAM，而MyISAM是不支持外键的，如图，
![引擎](https://img-blog.csdnimg.cn/20191206192146654.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
要想解决这个问题，可以在当前的表设置引擎为InnoDB、PBXT或SolidDB，但这只是修改了这一个数据库，下次建新的数据库默认引擎还是MyISAM，我们可以在MySQL的安装目录下的配置文件my.ini中的 [mysqld] 下面加入`default-storage-engine=INNODB`（其他支持外键的引擎也可），再重启Mysql服务器即可，小编在这块也遇到了问题，很久都没能解决，最后请教老师成功解决了。
可参考https://www.cnblogs.com/lqcdsns/p/7858279.html。

## 三、MySQL和Python交互

### 1.数据准备

创建数据库和数据表

```mysql-sql
--创建京东数据库
create database jingdong charset = utf8;
12
```

打印

```mysql-sql
Query OK, 1 row affected (0.00 sec)
1
--创建商品goods数据表
create table goods(
    id int unsigned primary key auto_increment not null,
    name varchar(150) not null,
    cate_name varchar(40) not null,
    brand_name varchar(40) not null,
    price decimal(10,3) not null default 0,
    is_show tinyint not null default 1,
    is_saleoff tinyint not null default 0
);
12345678910
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
1
```

插入数据

```mysql-sql
-- 向goods表中插入数据
insert into goods values(0,'r510vc 15.6英寸笔记本','笔记本','华硕','3399',default,default); 
insert into goods values(0,'y400n 14.0英寸笔记本电脑','笔记本','联想','4999',default,default);
insert into goods values(0,'g150th 15.6英寸游戏本','游戏本','雷神','8499',default,default); 
insert into goods values(0,'x550cc 15.6英寸笔记本','笔记本','华硕','2799',default,default); 
insert into goods values(0,'x240 超极本','超级本','联想','4880',default,default); 
insert into goods values(0,'u330p 13.3英寸超极本','超级本','联想','4299',default,default); 
insert into goods values(0,'svp13226scb 触控超极本','超级本','索尼','7999',default,default); 
insert into goods values(0,'ipad mini 7.9英寸平板电脑','平板电脑','苹果','1998',default,default);
insert into goods values(0,'ipad air 9.7英寸平板电脑','平板电脑','苹果','3388',default,default); 
insert into goods values(0,'ipad mini 配备 retina 显示屏','平板电脑','苹果','2788',default,default); 
insert into goods values(0,'ideacentre c340 20英寸一体电脑 ','台式机','联想','3499',default,default); 
insert into goods values(0,'vostro 3800-r1206 台式电脑','台式机','戴尔','2899',default,default); 
insert into goods values(0,'imac me086ch/a 21.5英寸一体电脑','台式机','苹果','9188',default,default); 
insert into goods values(0,'at7-7414lp 台式电脑 linux ）','台式机','宏碁','3699',default,default); 
insert into goods values(0,'z220sff f4f06pa工作站','服务器/工作站','惠普','4288',default,default); 
insert into goods values(0,'poweredge ii服务器','服务器/工作站','戴尔','5388',default,default); 
insert into goods values(0,'mac pro专业级台式电脑','服务器/工作站','苹果','28888',default,default); 
insert into goods values(0,'hmz-t3w 头戴显示设备','笔记本配件','索尼','6999',default,default); 
insert into goods values(0,'商务双肩背包','笔记本配件','索尼','99',default,default); 
insert into goods values(0,'x3250 m4机架式服务器','服务器/工作站','ibm','6888',default,default); 
insert into goods values(0,'商务双肩背包','笔记本配件','索尼','99',default,default);
12345678910111213141516171819202122
```

打印

```mysql-sql
Query OK, 1 row affected (0.01 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
                                    
Query OK, 1 row affected (0.00 sec) 
1234567891011121314151617181920212223242526272829303132333435363738394041
```

数据表大致如下

```mysql-sql
+----+---------------------------------------+---------------------+------------+-----------+---------+-----
+                                                                                                           
| id | name                                  | cate_name           | brand_name | price     | is_show | is_s
|                                                                                                           
+----+---------------------------------------+---------------------+------------+-----------+---------+-----
+                                                                                                           
|  1 | r510vc 15.6英寸笔记本                 | 笔记本              | 华硕       |  3399.000 |       1 |          0    
|                                                                                                           
|  2 | y400n 14.0英寸笔记本电脑              | 笔记本              | 联想       |  4999.000 |       1 |          0      
|                                                                                                           
|  3 | g150th 15.6英寸游戏本                 | 游戏本              | 雷神       |  8499.000 |       1 |          0    
|                                                                                                           
|  4 | x550cc 15.6英寸笔记本                 | 笔记本              | 华硕       |  2799.000 |       1 |          0    
|                                                                                                           
|  5 | x240 超极本                           | 超级本              | 联想       |  4880.000 |       1 |          0  
|                                                                                                           
|  6 | u330p 13.3英寸超极本                  | 超级本              | 联想       |  4299.000 |       1 |          0    
|                                                                                                           
|  7 | svp13226scb 触控超极本                | 超级本              | 索尼       |  7999.000 |       1 |          0    
|                                                                                                           
|  8 | ipad mini 7.9英寸平板电脑             | 平板电脑            | 苹果       |  1998.000 |       1 |          0      
|                                                                                                           
|  9 | ipad air 9.7英寸平板电脑              | 平板电脑            | 苹果       |  3388.000 |       1 |          0      
|                                                                                                           
| 10 | ipad mini 配备 retina 显示屏          | 平板电脑            | 苹果       |  2788.000 |       1 |          0     
...                                                                                                 
+----+---------------------------------------+---------------------+------------+-----------+---------+-----
+                                                                                                           
21 rows in set (0.00 sec)                                                                                   
                                                                                                            
123456789101112131415161718192021222324252627282930
```

### 2.数据表拆分

数据表拆分是一种思想，将大表拆分成很多小表，可以增加复用、提高效率。

创建 “商品分类” 表

```mysql-sql
--创建种类表
create table goods_cates(
    id int unsigned primary key auto_increment not null,
    name varchar(40) not null
);
12345
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
1
--将种类插入种类表
insert into goods_cates (name) select cate_name from goods group by cate_name;
12
```

打印

```mysql-sql
Query OK, 7 rows affected (0.01 sec)
Records: 7  Duplicates: 0  Warnings: 0
12
```

将查询的数据插入新表不需要values字段，否则会报错。

```mysql-sql
--修改goods表
update goods as g inner join goods_cates as c on g.cate_name = c.name set g.cate_name = c.id;
select * from goods;
123
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
Rows matched: 21  Changed: 0  Warnings: 0
12
```

此时，goods表为

```mysql-sql
+----+---------------------------------------+-----------+------------+-----------+---------+------------+ 
| id | name                                  | cate_name | brand_name | price     | is_show | is_saleoff | 
+----+---------------------------------------+-----------+------------+-----------+---------+------------+ 
|  1 | r510vc 15.6英寸笔记本                 | 5         | 华硕       |  3399.000 |       1 |          0 |        
|  2 | y400n 14.0英寸笔记本电脑              | 5         | 联想       |  4999.000 |       1 |          0 |          
|  3 | g150th 15.6英寸游戏本                 | 4         | 雷神       |  8499.000 |       1 |          0 |        
|  4 | x550cc 15.6英寸笔记本                 | 5         | 华硕       |  2799.000 |       1 |          0 |        
|  5 | x240 超极本                           | 7         | 联想       |  4880.000 |       1 |          0 |      
|  6 | u330p 13.3英寸超极本                  | 7         | 联想       |  4299.000 |       1 |          0 |        
|  7 | svp13226scb 触控超极本                | 7         | 索尼       |  7999.000 |       1 |          0 |        
|  8 | ipad mini 7.9英寸平板电脑             | 2         | 苹果       |  1998.000 |       1 |          0 |         
|  9 | ipad air 9.7英寸平板电脑              | 2         | 苹果       |  3388.000 |       1 |          0 |         
| 10 | ipad mini 配备 retina 显示屏          | 2         | 苹果       |  2788.000 |       1 |          0 |        
...
+----+---------------------------------------+-----------+------------+-----------+---------+------------+ 
21 rows in set (0.00 sec)                                                                                  
12345678910111213141516
```

### 3.Python操作MySQL

#### Python-MySQL安装

在Windows操作系统上安装：
Python3:`pip install pymysql`
Python2:`pip install MySQLdb`
Linux（Ubuntu）安装：https://www.jianshu.com/p/d84cdb5e6273

#### 操作步骤

- 开始
- 创建connection
- 获取cursor
- 执行查询、执行命令、获取数据、处理数据
- 关闭cursor
- 关闭connection
- 结束

和文件操作类似。
如图
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcua2FuY2xvdWQuY24vZWQvMGMvZWQwYzExNTlhODJhZDNlNzg4YWM1MWM1N2M0NjI2ZjVfODc1eDU2Ny5wbmc?x-oss-process=image/format,png)
（1）Connection 对象
用于建立与数据库的连接
创建对象：调用**connect()** 方法。

```python
conn=connect(参数列表)
参数host：连接的mysql主机，如果本机是'localhost'
参数port：连接的mysql主机的端口，默认是3306
参数database：数据库的名称
参数user：连接的用户名
参数password：连接的密码
参数charset：通信采用的编码方式，推荐使用utf8
1234567
```

根据导入库的方式，具体可分为两种连接方式
方式一

```python
import pymysql
con = pymysql.connect(host = 'localhost',port=3306,database='jingdong',user='root',password = 'root',charset = 'utf8')
12
```

方式二

```python
from pymysql import *
conn = connect(host = 'localhost',port=3306,database='jingdong',user='root',password = 'root',charset = 'utf8')
12
```

对象的方法：

- commit()提交；
- cursor()返回Cursor对象,用于执行sql语句并获得结果；
- close()关闭连接。

（2）Cursor对象
用于执行sql语句,经常使用的语句为select、insert、update、delete
获取Cursor对象:调用Connection对象的cursor()方法。

```python
cur = conn.cursor()
1
```

对象的方法

- `execute(operation [, parameters ])`执行语句，返回受影响的行数，主要用于执行insert、update、delete语句，也可以执行create、alter、drop等语句；
- fetchone()执行查询语句时，获取查询结果集的第一个行数据，返回一个元组；
- fetchmany(n)执行查询语句时，获取查询结果集的n行数据，一行构成一个元组，再将这些元组装入一个元组返回；
- fetchall()执行查询时，获取结果集的所有行，一行构成一个元组，再将这些元组装入一个元组返回；
- close()关闭 先关闭游标,在关闭链接。

使用python连接数据库并代码实现查询数据库中的数据示例：

```python
from pymysql import *

#连接数据库
conn = connect(host='127.0.0.1',port=3306,db='jingdong',user='root',passwd='root',charset='utf8')

#获取游标对象
cur = conn.cursor()

#执行SQL语句，获取查询结果的记录数
r = cur.execute('select * from goods;')
print(r)

#得到数据
#获取一条数据
print(cur.fetchone())
print(cur.fetchone())
print(cur.fetchone())
#获取多条数据
print(cur.fetchmany(3))
#获取全部数据，是指游标之后的全部，而不一定是整个表的所有数据
print(cur.fetchall())
print(cur.fetchone())

#关闭游标对象
cur.close()

#关闭连接
conn.close()
12345678910111213141516171819202122232425262728
```

打印结果为

```python
21
(1, 'r510vc 15.6英寸笔记本', '5', '华硕', Decimal('3399.000'), 1, 0)
(2, 'y400n 14.0英寸笔记本电脑', '5', '联想', Decimal('4999.000'), 1, 0)
(3, 'g150th 15.6英寸游戏本', '4', '雷神', Decimal('8499.000'), 1, 0)
((4, 'x550cc 15.6英寸笔记本', '5', '华硕', Decimal('2799.000'), 1, 0), (5, 'x240 超极本', '7', '联想', Decimal('4880.000'), 1, 0), (6, 'u330p 13.3英寸超极本', '7', '联想', Decimal('4299.000'), 1, 0))
((7, 'svp13226scb 触控超极本', '7', '索尼', Decimal('7999.000'), 1, 0), (8, 'ipad mini 7.9英寸平板电脑', '2', '苹果', Decimal('1998.000'), 1, 0), (9, 'ipad air 9.7英寸平板电脑', '2', '苹果', Decimal('3388.000'), 1, 0), (10, 'ipad mini 配备 retina 显示屏', '2', '苹果', Decimal('2788.000'), 1, 0), (11, 'ideacentre c340 20英寸一体电脑 ', '1', '联想', Decimal('3499.000'), 1, 0), (12, 'vostro 3800-r1206 台式电脑', '1', '戴尔', Decimal('2899.000'), 1, 0), (13, 'imac me086ch/a 21.5英寸一体电脑', '1', '苹果', Decimal('9188.000'), 1, 0), (14, 'at7-7414lp 台式电脑 linux ）', '1', '宏碁', Decimal('3699.000'), 1, 0), (15, 'z220sff f4f06pa工作站', '3', '惠普', Decimal('4288.000'), 1, 0), (16, 'poweredge ii服务器', '3', '戴尔', Decimal('5388.000'), 1, 0), (17, 'mac pro专业级台式电脑', '3', '苹果', Decimal('28888.000'), 1, 0), (18, 'hmz-t3w 头戴显示设备', '6', '索尼', Decimal('6999.000'), 1, 0), (19, '商务双肩背包', '6', '索尼', Decimal('99.000'), 1, 0), (20, 'x3250 m4机架式服务器', '3', 'ibm', Decimal('6888.000'), 1, 0), (21, '商务双肩背包', '6', '索尼', Decimal('99.000'), 1, 0))
None
1234567
```

在执行`cur.fetchall()`后游标到数据最后位置，已查询完毕，此时再`cur.fetchone()`会返回None，要想再次查询再执行一次`r = cur.execute('select * from goods;')`即可。
在查询完毕后，要先关闭游标，再关闭连接。
在连接数据库时，一般要异常处理，如上边代码可修改为

```python
from pymysql import *

try:
    #连接数据库
    conn = connect(host='127.0.0.1',port=3306,db='jingdong',user='root',passwd='root',charset='utf8')

    #获取游标对象
    cur = conn.cursor()

    #执行SQL语句，获取查询结果的记录数
    r = cur.execute('select * from goods;')
    print(r)

    #得到数据
    #获取一条数据
    print(cur.fetchone())
    print(cur.fetchone())
    print(cur.fetchone())
    #获取多条数据
    print(cur.fetchmany(3))
    #获取全部数据，是指游标之后的全部，而不一定是整个表的所有数据
    print(cur.fetchall())
    print(cur.fetchone())

    #关闭游标对象
    cur.close()

    #关闭连接
    conn.close()

except Exception as e:
    print("Error %d:%s"%(e.args[0],e.args[1]))
1234567891011121314151617181920212223242526272829303132
```

查询结果与之前相同。
省市区数据脚本链接：
https://pan.baidu.com/s/1IkODeq4N_drQYWiysUuKjA
提取码: p39a。

# Python全栈（三）数据库优化之6.Python操作MySQL和视图

## 一、封装MySQL-DB类

用面向对象的思想，封装DB类。
实现初始化（连接）、查询、关闭等操作。

```python
from pymysql import *

class MyDB(object):
    def __init__(self):
        self.conn()

    def conn(self):
        '''连接'''
        try:
            self.conn = connect(
                host='127.0.0.1',
                port=3306,
                db='demo',
                user='root',
                passwd='root',
                charset='utf8'
            )
        except Exception as e:
            print(e)

    def get_one(self):
        '''单个记录查询'''
        sql = 'select * from students;'
        #获取游标
        cur = self.conn.cursor()
        #执行SQL语句
        cur.execute(sql)
        #获取执行结果
        result = cur.fetchone()
        cur.close()
        return result

    def get_many(self):
        '''多个记录查询'''
        sql = 'select * from students;'
        #获取游标
        cur = self.conn.cursor()
        #执行SQL语句
        cur.execute(sql)
        #获取执行结果
        result = cur.fetchmany()
        cur.close()
        return result

    def get_all(self):
        '''所有记录查询'''
        sql = 'select * from students;'
        #获取游标
        cur = self.conn.cursor()
        #执行SQL语句
        cur.execute(sql)
        #获取执行结果
        result = cur.fetchall()
        cur.close()
        return result

    def __del__(self):
        '''关闭连接，释放'''
        self.conn.close()

def main():
    obj = MyDB()
    res1 = obj.get_one()
    print(res1)
    res2 = obj.get_all()
    for r in res2:
        print(r)

if __name__ == '__main__':
    main()
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970
```

进行实例化并测试得，

```python
(1, 'Tom', 18, 'male', 1, datetime.date(1999, 9, 9), 1, Decimal('165.00'))
(1, 'Tom', 18, 'male', 1, datetime.date(1999, 9, 9), 1, Decimal('165.00'))
(2, 'Jerry', 19, 'male', 3, datetime.date(1999, 10, 9), 1, Decimal('157.00'))
(3, 'Nancy', 17, 'male', 2, datetime.date(1999, 8, 9), 1, Decimal('165.00'))
(4, 'John', 19, 'secret', 1, datetime.date(1999, 7, 9), 1, Decimal('168.00'))
(6, 'John', 16, 'secret', 3, datetime.date(1999, 5, 9), 1, Decimal('165.00'))
(7, 'John', 19, 'secret', 1, datetime.date(1999, 7, 9), 1, Decimal('176.00'))
(8, 'John', 19, 'secret', 2, datetime.date(1999, 7, 9), 1, Decimal('165.00'))
(9, 'John', 19, 'secret', 3, datetime.date(1999, 7, 9), 1, Decimal('153.00'))
(10, 'John', 19, 'secret', 3, datetime.date(1999, 7, 9), 1, Decimal('165.00'))
...
1234567891011
```

## 二、练习-商品表的查询

需求：
输入1：查询所有商品
输入2：查询所有商品种类
输入3：查询所有品牌
输入4：添加商品种类
输入5：退出
代码如下，

```python
from pymysql import *
class Jd(object):
    def __init__(self):
        '''初始化'''
        #连接数据库
        self.conn = connect(
            host='127.0.0.1',
            port=3306,
            db='jingdong',
            user='root',
            passwd='root',
            charset='utf8'
        )
        #获取游标
        self.cur = self.conn.cursor()

    @staticmethod
    def print_menu():
        '''静态方法，不需要self，单独定义打印菜单函数，解耦，降低耦合性'''
        print('--jd shop--')
        print('1-查询所有商品')
        print('2-查询所有种类')
        print('3-查询所有品牌')
        print('4-添加商品种类')
        print('5-退出')
        num = input('请输入一个数字：')
        return num

    def execute_sql(self,sql):
        self.cur.execute(sql)
        result = self.cur.fetchall()
        for item in result:
            print(item)

    def show_all_goods(self):
        '''查询所有商品'''
        sql = 'select * from goods;'
        self.execute_sql(sql)

    def show_all_cates(self):
        '''查询所有种类'''
        sql = 'select * from goods_cates;'
        self.execute_sql(sql)

    def show_all_brands(self):
        sql = 'select distinct brand_name from goods;'
        self.execute_sql(sql)

    def add_cate(self):
        name = input('请输入新的商品分类名：')
        sql = 'insert into goods_cates(name) values("%s")' % name
        self.cur.execute(sql)
        print('Adding cates successfully!')

    def run(self):
        while True:
            num = self.print_menu()
            if num == '1':
                self.show_all_goods()
            elif num == '2':
                self.show_all_cates()
            elif num == '3':
                self.show_all_brands()
            elif num == '4':
                self.add_cate()
            elif num == '5':
                print('Bye-Bye')
                break

    def __del__(self):
        self.cur.close()
        self.conn.close()

def main():
    jd = Jd()
    jd.run()

if __name__ == '__main__':
    main()
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879
```

进行实例化并测试得，

```python
--jd shop--
1-查询所有商品
2-查询所有种类
3-查询所有品牌
4-添加商品种类
5-退出
请输入一个数字：1
(1, 'r510vc 15.6英寸笔记本', '5', '华硕', Decimal('3399.000'), 1, 0)
(2, 'y400n 14.0英寸笔记本电脑', '5', '联想', Decimal('4999.000'), 1, 0)
(3, 'g150th 15.6英寸游戏本', '4', '雷神', Decimal('8499.000'), 1, 0)
(4, 'x550cc 15.6英寸笔记本', '5', '华硕', Decimal('2799.000'), 1, 0)
(5, 'x240 超极本', '7', '联想', Decimal('4880.000'), 1, 0)
(6, 'u330p 13.3英寸超极本', '7', '联想', Decimal('4299.000'), 1, 0)
(7, 'svp13226scb 触控超极本', '7', '索尼', Decimal('7999.000'), 1, 0)
(8, 'ipad mini 7.9英寸平板电脑', '2', '苹果', Decimal('1998.000'), 1, 0)
(9, 'ipad air 9.7英寸平板电脑', '2', '苹果', Decimal('3388.000'), 1, 0)
(10, 'ipad mini 配备 retina 显示屏', '2', '苹果', Decimal('2788.000'), 1, 0)
(11, 'ideacentre c340 20英寸一体电脑 ', '1', '联想', Decimal('3499.000'), 1, 0)
(12, 'vostro 3800-r1206 台式电脑', '1', '戴尔', Decimal('2899.000'), 1, 0)
(13, 'imac me086ch/a 21.5英寸一体电脑', '1', '苹果', Decimal('9188.000'), 1, 0)
(14, 'at7-7414lp 台式电脑 linux ）', '1', '宏碁', Decimal('3699.000'), 1, 0)
(15, 'z220sff f4f06pa工作站', '3', '惠普', Decimal('4288.000'), 1, 0)
(16, 'poweredge ii服务器', '3', '戴尔', Decimal('5388.000'), 1, 0)
(17, 'mac pro专业级台式电脑', '3', '苹果', Decimal('28888.000'), 1, 0)
(18, 'hmz-t3w 头戴显示设备', '6', '索尼', Decimal('6999.000'), 1, 0)
(19, '商务双肩背包', '6', '索尼', Decimal('99.000'), 1, 0)
(20, 'x3250 m4机架式服务器', '3', 'ibm', Decimal('6888.000'), 1, 0)
(21, '商务双肩背包', '6', '索尼', Decimal('99.000'), 1, 0)
--jd shop--
1-查询所有商品
2-查询所有种类
3-查询所有品牌
4-添加商品种类
5-退出
请输入一个数字：2
(1, '台式机')
(2, '平板电脑')
(3, '服务器/工作站')
(4, '游戏本')
(5, '笔记本')
(6, '笔记本配件')
(7, '超级本')
--jd shop--
1-查询所有商品
2-查询所有种类
3-查询所有品牌
4-添加商品种类
5-退出
请输入一个数字：3
('华硕',)
('联想',)
('雷神',)
('索尼',)
('苹果',)
('戴尔',)
('宏碁',)
('惠普',)
('ibm',)
--jd shop--
1-查询所有商品
2-查询所有种类
3-查询所有品牌
4-添加商品种类
5-退出
请输入一个数字：4
请输入新的商品分类名：minipads
Adding cates successfully!
--jd shop--
1-查询所有商品
2-查询所有种类
3-查询所有品牌
4-添加商品种类
5-退出
请输入一个数字：5
Bye-Bye
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475
```

这个类种定义了一个**静态方法**，不需要传入self参数，因为这个print_menu()方法与类无实际联系，所以定义为静态方法；
同时运用了**解耦**的思想，降低耦合性，能独立出方法块的尽量抽离出独立的方法，减少依赖性，写程序时要求高内聚、低耦合。

## 三、封装类中数据库的新增

在封装类的数据库中进行数据的新增、更新、删除。
执行插入语句后，

```python
from pymysql import *

def operate_db():
    global conn
    #连接数据库
    conn = connect(
        host='127.0.0.1',
        port=3306,
        db='demo1125',
        user='root',
        passwd='root',
        charset='utf8'
    )
    #获取游标
    cur = conn.cursor()
    sql = 'insert into student(sname) values("Jack");'
    cur.execute(sql)
    cur.close()
    conn.close()

if __name__ == '__main__':
    operate_db()
12345678910111213141516171819202122
```

再查询数据库并没有看到新增的数据，说明数据并没有被成功插入，此时需要提交事务。
如下

```python
from pymysql import *

def operate_db():
    global conn
    #连接数据库
    conn = connect(
        host='127.0.0.1',
        port=3306,
        db='demo1125',
        user='root',
        passwd='root',
        charset='utf8'
    )
    #获取游标
    cur = conn.cursor()
    sql = 'insert into student(sname) values("Jack");'
    cur.execute(sql)
    # sql = 'insert into student(name) values("Jackson");'
    # cur.execute(sql)
    #提交事务
    conn.commit()
    cur.close()
    conn.close()

if __name__ == '__main__':
    operate_db()
1234567891011121314151617181920212223242526
```

此时，再去查询，发现已经有了一条刚刚插入的数据，并且id并不连续，即未成功提交事务的数据也会占用id，如下所示

```python
+-----+-------+-----+
| sid | sname | gid |
+-----+-------+-----+
|   3 | Jack  |   1 |
|   4 | Jack  |   1 |
|   5 | Jack  |   1 |
|   8 | Jack  |   1 |
+-----+-------+-----+
12345678
```

5过了就直接到8了，id自增是不可逆的。
提交事务：
只有提交之后，数据才会被真正地添加到数据库中，数据库才会发生相应的更改。
同时执行多条插入语句时

```python
from pymysql import *

def operate_db():
    global conn
    #连接数据库
    conn = connect(
        host='127.0.0.1',
        port=3306,
        db='demo1125',
        user='root',
        passwd='root',
        charset='utf8'
    )
    #获取游标
    cur = conn.cursor()
    sql = 'insert into student(sname) values("Jack");'
    cur.execute(sql)
    sql = 'insert into student(sname) values("Jackson");'
    cur.execute(sql)
    #提交事务
    conn.commit()
    cur.close()
    conn.close()

if __name__ == '__main__':
    operate_db()
1234567891011121314151617181920212223242526
```

查询到

```mysql-sql
+-----+---------+-----+
| sid | sname   | gid |
+-----+---------+-----+
|   3 | Jack    |   1 |
|   4 | Jack    |   1 |
|   5 | Jack    |   1 |
|   8 | Jack    |   1 |
|   9 | Jack    |   1 |
|  10 | Jackson |   1 |
+-----+---------+-----+
12345678910
```

当第2条SQL语句出错时，

```python
from pymysql import *

def operate_db():
    global conn
    #连接数据库
    conn = connect(
        host='127.0.0.1',
        port=3306,
        db='demo1125',
        user='root',
        passwd='root',
        charset='utf8'
    )
    #获取游标
    cur = conn.cursor()
    sql = 'insert into student(sname) values("Jack");'
    cur.execute(sql)
    sql = 'insert into student(name) values("Jackson");'
    cur.execute(sql)
    #提交事务
    conn.commit()
    cur.close()
    conn.close()

if __name__ == '__main__':
    operate_db()
1234567891011121314151617181920212223242526
```

此时，数据为

```mysql-sql
+-----+---------+-----+ 
| sid | sname   | gid | 
+-----+---------+-----+ 
|   3 | Jack    |   1 | 
|   4 | Jack    |   1 | 
|   5 | Jack    |   1 | 
|   8 | Jack    |   1 | 
|   9 | Jack    |   1 | 
|  10 | Jackson |   1 | 
+-----+---------+-----+ 
6 rows in set (0.00 sec)
1234567891011
```

可知，在第2条打起来语句有错的情况下，第1条语句也为成功插入。
可以加入异常处理

```python
from pymysql import *

def operate_db():
    global conn
    try:
        #连接数据库
        conn = connect(
            host='127.0.0.1',
            port=3306,
            db='demo1125',
            user='root',
            passwd='root',
            charset='utf8'
        )
        #获取游标
        cur = conn.cursor()
        sql = 'insert into student(sname) values("Jack");'
        cur.execute(sql)
        sql = 'insert into student(name) values("Jackson");'
        cur.execute(sql)
        #提交事务
        conn.commit()
        cur.close()
        conn.close()
    except Exception as e:
        print(e.args[0],e.args[1])
        conn.commit()

if __name__ == '__main__':
    operate_db()
123456789101112131415161718192021222324252627282930
```

打印`1054 Unknown column 'name' in 'field list'`。
此时再查询，

```mysql-sql
+-----+---------+-----+ 
| sid | sname   | gid | 
+-----+---------+-----+ 
|   3 | Jack    |   1 | 
|   4 | Jack    |   1 | 
|   5 | Jack    |   1 | 
|   8 | Jack    |   1 | 
|   9 | Jack    |   1 | 
|  10 | Jackson |   1 | 
|  22 | Jack    |   1 | 
+-----+---------+-----+ 
7 rows in set (0.01 sec)
123456789101112
```

显然，第1条语句被成功执行并插入，但是id已不再是从刚才的id自增1，而是间隔了很多个值，这样就能达到将前边正确的SQL语句插入的目的，即部分提交；
如果要使得如果在发生异常时，所有的操作都撤回，应该使用回滚，即**rollback**。

```python
from pymysql import *

def operate_db():
    global conn
    try:
        #连接数据库
        conn = connect(
            host='127.0.0.1',
            port=3306,
            db='demo1125',
            user='root',
            passwd='root',
            charset='utf8'
        )
        #获取游标
        cur = conn.cursor()
        sql = 'insert into student(sname) values("Jack");'
        cur.execute(sql)
        sql = 'insert into student(name) values("Jackson");'
        cur.execute(sql)
        #提交事务
        conn.commit()
        cur.close()
        conn.close()
    except Exception as e:
        print(e.args[0],e.args[1])
        #回滚
        conn.rollback()

if __name__ == '__main__':
    operate_db()
12345678910111213141516171819202122232425262728293031
```

此时查询数据库，可得

```mysql-sql
+-----+---------+-----+
| sid | sname   | gid |
+-----+---------+-----+
|   3 | Jack    |   1 |
|   4 | Jack    |   1 |
|   5 | Jack    |   1 |
|   8 | Jack    |   1 |
|   9 | Jack    |   1 |
|  10 | Jackson |   1 |
|  22 | Jack    |   1 |
+-----+---------+-----+
1234567891011
```

显然，这时即便第1条SQL语句即便未出错，但是因为第2条语句出错，导致回滚操作使所有的操作撤回，数据表未增加。

```python
from pymysql import *

def operate_db():
    global conn
    try:
        #连接数据库
        conn = connect(
            host='127.0.0.1',
            port=3306,
            db='demo1125',
            user='root',
            passwd='root',
            charset='utf8'
        )
        #获取游标
        cur = conn.cursor()
        sql = 'insert into student(sname) values("Tom");'
        cur.execute(sql)
        sql = 'insert into student(sname) values("Tommy");'
        cur.execute(sql)
        #提交事务
        conn.commit()
        cur.close()
        conn.close()
    except Exception as e:
        print(e.args[0],e.args[1])
        #回滚
        conn.rollback()


if __name__ == '__main__':
    operate_db()
1234567891011121314151617181920212223242526272829303132
```

此时插入两条正常数据

```mysql-sql
+-----+---------+-----+
| sid | sname   | gid |
+-----+---------+-----+
|   3 | Jack    |   1 |
|   4 | Jack    |   1 |
|   5 | Jack    |   1 |
|   8 | Jack    |   1 |
|   9 | Jack    |   1 |
|  10 | Jackson |   1 |
|  22 | Jack    |   1 |
|  26 | Tom     |   1 |
|  27 | Tommy   |   1 |
+-----+---------+-----+
12345678910111213
```

插入成功，显然id并未从22开始，说明rollback即使未插入数据，但是还是占用了id。
总结：
只要`cur.execute(sql)`被正常执行，即SQL语句正确，id就会自增，未被正确执行，就不会自增，而不管数据是否被插入和是否提交事务；
回滚时，正确执行的使id增加，未被正确执行的使id不变；
全部执行成功时，id增加；
全部都不提交：rollback，id部分增加
提交一部分：commit，id部分增加。
数据的更新、删除操作与新增类似。

## 四、视图

### 1.问题提出

对于复杂的查询，往往是有多个数据表进行关联查询而得到，如果数据库因为需求等原因发生了改变，为了保证查询出来的数据与之前相同，则需要在多个地方进行修改，维护起来非常麻烦。

### 2.视图定义

通俗地讲，视图就是一条select语句执行后返回的结果集，即虚拟的表。
我们在创建视图的时候，主要的工作就落在创建这条SQL查询语句上。
视图是对若干张基本表的引用，一张虚表，查询语句执行的结果，不存储具体的数据（基本表数据发生了改变，视图也会跟着改变）。

### 3.视图操作

**创建视图**
语法：`create view 视图名称 as select语句;`
一般的查询语句：

```mysql-sql
select p.*,c.name as cname from areas as p inner join areas as c on p.id = c.pid having p.name = '湖南'
1
```

打印

```mysql-sql
+----+-----+--------+------+-----------+
| id | pid | name   | type | cname     |
+----+-----+--------+------+-----------+
| 14 |   1 | 湖南   |    1 | 长沙      |    
| 14 |   1 | 湖南   |    1 | 张家界    |     
| 14 |   1 | 湖南   |    1 | 常德      |    
| 14 |   1 | 湖南   |    1 | 郴州      |    
| 14 |   1 | 湖南   |    1 | 衡阳      |    
| 14 |   1 | 湖南   |    1 | 怀化      |    
| 14 |   1 | 湖南   |    1 | 娄底      |    
| 14 |   1 | 湖南   |    1 | 邵阳      |    
| 14 |   1 | 湖南   |    1 | 湘潭      |    
...  
+----+-----+--------+------+-----------+
14 rows in set (0.01 sec)               
123456789101112131415
```

运用查询到的结果创建视图

```mysql-sql
create view v_p_c as select p.*,c.name as cname from areas as p inner join areas as c on p.id = c.pid having p.name = '湖南';
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
1
```

**查看视图**
语法：`show tables;`

```mysql-sql
show tables;
1
```

打印

```mysql-sql
+--------------------+
| Tables_in_demo1125 |
+--------------------+
| areas              |
| cities             |
| classes            |
| provinces          |
| student            |
| v_p_c              |
+--------------------+
6 rows in set (0.00 sec)
1234567891011
```

很明显，此时已经增加了一个新表即视图v_p_c。
**使用视图**

```mysql-sql
select * from v_p_c;
1
```

打印

```mysql-sql
+----+-----+--------+------+-----------+
| id | pid | name   | type | cname     |
+----+-----+--------+------+-----------+
| 14 |   1 | 湖南   |    1 | 长沙      |
| 14 |   1 | 湖南   |    1 | 张家界    |
| 14 |   1 | 湖南   |    1 | 常德      |
| 14 |   1 | 湖南   |    1 | 郴州      |
| 14 |   1 | 湖南   |    1 | 衡阳      |
| 14 |   1 | 湖南   |    1 | 怀化      |
| 14 |   1 | 湖南   |    1 | 娄底      |
| 14 |   1 | 湖南   |    1 | 邵阳      |
| 14 |   1 | 湖南   |    1 | 湘潭      |
| 14 |   1 | 湖南   |    1 | 湘西      |
...
+----+-----+--------+------+-----------+
14 rows in set (0.01 sec)
12345678910111213141516
```

**删除视图**
语法：`drop view 视图名称;`
例如：

```mysql-sql
drop view v_p_c;
1
```

**视图修改**
有下列内容之一，视图不能做修改：

- select子句中包含distinct；
- select子句中包含组函数；
- select语句中包含group by子句；
- select语句中包含order by子句；
- where子句中包含相关子查询；
- from子句中包含多个表；
- 如果视图中有计算列，则不能更新；
- 如果基表中有某个具有非空约束的列未出现在视图定义中，则不能做insert操作。

可知，视图基本不允许修改。

```mysql-sql
update v_p_c set cname ='长沙市' where cname ='长沙;'
1
```

打印

```mysql-sql
ERROR 1288 (HY000): The target table v_p_c of the UPDATE is not updatable
1
```

报错，因为视图本来就是为了查询创建的，而不是为了修改而创建，所以视图修改是没有必要的，也不允许。
原表中的数据改变时：

- 如改变的数据未在select语句的条件中出现，视图中的数据也会相应发生改变；
- 如改变的数据在select语句的条件中出现，视图中的数据也会相应发生改变，因为查询的条件可能因为数据的修改而不再满足，使得查询到的数据量减少或增加。
  可以删除视图，但是不能删除视图中的数据，只能对视图进行查询操作。

**视图的作用**

- 方便操作，特别是查询操作；
- 减少复杂的SQL语句，就像一个函数，提高了重用性，增强代码可读性；
- 对数据库重构，却不影响程序的运行；
- 隐藏数据，提高了安全性能，可以对不同的用户提供不同的视图；
- 让数据更加清晰。

一般在**SQL语句较复杂**时使用视图来简化操作。

# Python全栈（三）数据库优化之7.MySQL高级-事务、索引、账户管理和存储引擎介绍

## 一、事务

### 1.事务引出

事务广泛用于订单系统、银行系统等多种场景。
例如：
A用户和B用户是银行的储户，现在A要给B转账500元，那么需要做以下几件事：
检查A的账户余额>500元；
A账户中扣除500元;
B账户中增加500元。
正常的流程走下来，A账户扣了500，B账户加了500；
那如果A账户扣了钱之后，系统出故障了，A会白白损失了500，而B也没有收到本该属于他的500。
以上的案例中，隐藏着一个前提条件：A扣钱和B加钱，要么同时成功，要么同时失败。事务的需求就在于此。

### 2.事务定义

事务是一个操作序列，这些操作要么都执行，要么都不执行，它是一个不可分割的工作单位。
例如，银行转帐工作：从一个帐号扣款并使另一个帐号增款，这两个操作要么都执行，要么都不执行。所以，应该把他们看成一个事务。
事务是数据库维护数据一致性的单位，在每个事务结束时，都能保持数据一致性。
那么至少需要三个步骤：
（1）检查账户1的余额高于或者等于100美元；
（2）从账户1余额中减去100美元；
（3）在帐户2余额中增加100美元。

```mysql-sql
begin;
update bank1 set money = money - 100 where id = 1;
12
```

此时没有commit，查询时数据不会发生变化，即体现了事物的隔离性。

```mysql-sql
update bank2 set money = money + 100 where id = 1;
commit;
12
```

此时两个表中的数据均发生变化，一个减少100，一个增加100。
以上两步操作组成一个完整的事务。
开启事务后每次执行SQL语句后不会自动提交，要提交才能操作成功。

### 3.事务的四大特性（ACID）

（1）**原子性**（Atomicity）：
一个事务被视为一个不可分割的最小工作单元，整个事务中的所有操作要么全部提交成功，要么全部失败回滚，对于一个事务来说，不可能只执行其中的一部分操作，这就是事务的原子性。
（2）**一致性**（Consistency）：
数据库总是从一个一致性的状态转换到另一个一致性的状态。
在前面的例子中，一致性确保了，即使在执行两条update语句之间时系统崩溃，银行账户中也不会损失100美元，因为事务最终没有提交，所以事务中所做的修改也不会保存到数据库中。
（3）**隔离性**（Isolation）：
通常来说，一个事务所作的修改在最终提交以前，对其他事务是不可见的。
在前面的例子中，当执行完第一update语句、第四条语句还未开始时，此时有另外的一个账户汇总程序开始运行，则其看到帐户1的余额并没有被减去100美元。
在一个命令行中，开启一个事务

```mysql-sql
begin;
update goods set name = 'palyer' where id = 3;
12
```

未提交
在另一个命令行中执行另一个操作

```mysql-sql
update goods set name = 'Player' where id = 3;
1
```

此时会出现堵塞，因为前一个命令行未提交事务，即前一个在操作的时候，其他对该数据库的操作会被锁定，从而保证了数据的一致性。
（4）**持久性**（Durability）：
一旦事务提交，则其所做的修改会永久保存到数据库。
此时即使系统崩溃，修改的数据也不会丢失。

### 4.事务操作

表的引擎类型必须是innodb类型才可以使用事务，这是mysql表的默认引擎。
**开启事务**：
语法：`start transation;`或者`begin;`
开启事务后执行修改命令，变更会维护到本地缓存中，而不维护到物理表中。
**提交事务**：
语法：`commit;`
将缓存中的数据变更维护到物理表中。
**回滚事务**：
语法：`rollback;`
放弃缓存中变更的数据。
**练习**：
现在操作在一个事务中1给2转100，同时另一个事务中1给3转100.
数据如下

```mysql-sql
+----+-----+
| id | num |
+----+-----+
|  1 | 100 |
|  2 | 200 |
|  3 |   0 |
+----+-----+
1234567
```

操作过程如图
![事务操作](https://img-blog.csdnimg.cn/20191210221056134.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
事务1提交后，事务2对id1的更改会报错，因此需要回滚，从而保证了数据的一致性。
修改数据的命令会自动的触发事务，包括insert、update、delete；
如果对数据库的操作涉及多次数据的修改，并且成功一起成功，否则一起会滚到之前的数据，需要在SQL语句中有手动开启事务。

## 二、索引

### 1.引入

在图书馆中是如何找到一本书的？
一般的应用系统对比数据库的读写比例在**10:1**左右(即有10次查询操作时有1次写的操作)，而且插入操作和更新操作很少出现性能问题，遇到最多、最容易出问题还是一些复杂的查询操作，所以查询语句的优化显然是重中之重。
当数据库中数据量很大时，查找数据会变得很慢，所以引入索引进行优化。

### 2.定义

索引是一种**特殊的文件**(InnoDB数据表上的索引是表空间的一个组成部分)，它们包含着对数据表里所有记录的引用指针。
通俗地说，数据库索引就像一本书前面的目录，它们包含着对数据表里所有记录的引用指针。
**索引的目的**：
索引的目的在于提高查询效率，可以类比字典，如果要查“mysql”这个单词，我们肯定需要定位到m字母，然后从下往下找到y字母，再找到剩下的sql。
**索引原理**：
生活中随处可见索引的例子，如词典、火车站的车次表、图书的目录等。它们的原理都是一样的，通过不断的缩小想要获得数据的范围来筛选出最终想要的结果，同时把随机的事件变成顺序的事件，也就是我们总是通过同一种查找方式来锁定数据。
数据库也是一样，但显然要复杂许多，因为不仅面临着等值查询，还有范围查询(>、<、between、in)、模糊查询(like)、并集查询(or)等等。
如下图
![索引原理](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcua2FuY2xvdWQuY24vN2UvZjgvN2VmOGViNWUxNjJlMTg4MDIzZjYwMmQyODY2NDBiOTVfNjI0eDMwMC5qcGc#pic_center?x-oss-process=image/format,png)
索引使用的数据结构一般是**B树**。
索引分为：

- 单值索引：单个字段；
- 复合索引：多个字段；
- 唯一索引：列的值必须唯一，应用于身份证号、电话号等。

### 3.索引的使用

查看索引：
语法：`show index from 表名;`

```mysql-sql
show index from money;
1
```

打印

```mysql-sql
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| money |          0 | PRIMARY  |            1 | id          | A         |           3 |     NULL | NULL   |      | BTREE      |         |               |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
1 row in set (0.01 sec)
123456
```

主键会自动创建索引；
创建索引：
语法：`create index 字段名称 on 表名(字段名称(长度));`

```mysql-sql
create index idx_num on money(num);
1
```

再查看索引

```mysql-sql
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| money |          0 | PRIMARY  |            1 | id          | A         |           3 |     NULL | NULL   |      | BTREE      |         |               |
| money |          1 | idx_num  |            1 | num         | A         |           2 |     NULL | NULL   |      | BTREE      |         |               |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
2 rows in set (0.00 sec)
1234567
```

如果指定字段是字符串，需要指定长度，建议长度与定义字段时的长度一致；
字段类型如果不是字符串，可以不填写长度部分。
创建唯一索引：

```mysql-sql
create unique index idx_num on money(num);
1
```

删除索引：
语法：`drop index 索引名称 on 表名;`

### 4.索引练习

创建测试表test：

```mysql-sql
create table test(title varchar(10));
1
```

使用python程序向表中加入十万条数据:

```python
from pymysql import *
def main():
    conn = connect(
        host='127.0.0.1',
        port=3306,
        db='demo',
        user='root',
        passwd='root',
        charset='utf8')
    cur = conn.cursor()
    for i in range(100000):
        cur.execute('insert into test values("ts-%d")' % i)
    conn.commit()

if __name__ == '__main__':
    main()
12345678910111213141516
```

开启运行时间监测：

```mysql-sql
set profiling=1;
1
```

查找第100000条数据：

```mysql-sql
select * from test where title='ts-99999';
1
```

打印

```mysql-sql
+----------+
| title    |
+----------+
| ts-99999 |
+----------+
1 row in set (0.04 sec)
123456
```

查看执行的时间：

```mysql-sql
show profiles;
1
```

打印

```mysql-sql
+----------+------------+-------------------------------------------+
| Query_ID | Duration   | Query                                     |
+----------+------------+-------------------------------------------+
|        1 | 0.03794450 | select * from test where title='ts-99999' |
+----------+------------+-------------------------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

为表title_index的title列创建索引：

```mysql-sql
create index title_index on test(title(10));
1
```

再次执行查询语句：

```mysql-sql
select * from test where title='ts-99999';
1
```

打印

```mysql-sql
+----------+
| title    |
+----------+
| ts-99999 |
+----------+
1 row in set (0.00 sec)
123456
```

再次查看执行的时间:

```mysql-sql
show profiles;
1
```

打印

```mysql-sql
+----------+------------+---------------------------------------------+
| Query_ID | Duration   | Query                                       |
+----------+------------+---------------------------------------------+
|        1 | 0.03794450 | select * from test where title='ts-99999'   |
|        2 | 0.26895350 | create index title_index on test(title(10)) |
|        3 | 0.00031875 | select * from test where title='ts-99999'   |
+----------+------------+---------------------------------------------+
3 rows in set, 1 warning (0.00 sec)
12345678
```

显然，建立了索引后查询速度有了很大提升，即建立索引可以大幅提高查询效率。
适合建立索引的情况：

- 主键自动建立索引；
- 频繁作为查询条件的字段应该建立索引；
- 查询中与其他表关联的字段,外键关系建立索引；
- 在高并发的情况下创建复合索引；
- 查询中排序的字段,排序字段若通过索引去访问将大大提高排序速度 (建立索引的顺序跟排序的顺序保持一致)。

不适合建立索引的情况：

- 频繁更新的字段不适合建立索引；
- where条件里面用不到的字段不创建索引；
- 表记录太少,当表中数据量超过三百万条数据,可以考虑建立索引；
- 数据重复且平均的表字段,比如性别,国籍。

## 三、账户管理

在生产环境下操作数据库时，绝对不可以使用root账户连接，而是创建特定的账户，授予这个账户特定的操作权限，然后连接进行操作，主要的操作就是数据的**crud**。
MySQL账户体系：
根据账户所具有的权限的不同，MySQL的账户可以分为以下几种：

- 服务实例级账号：启动了一个mysqld，即为一个数据库实例；如果某用户如root,拥有服务实例级分配的权限，那么该账号就可以删除所有的数据库、连同这些库中的表；
- 数据库级别账号：对特定数据库执行增删改查的所有操作；
- 数据表级别账号：对特定表执行增删改查等所有操作；
- 字段级别的权限：对某些表的特定字段进行操作；
- 存储程序级别的账号：对存储程序进行增删改查的操作。
  账户的操作主要包括创建账户、删除账户、修改密码、授权权限等。

### 授予权限：

需要使用实例级账户登录后操作，以root为例
主要操作包括：

#### （1）查看所有用户：

所有用户及权限信息存储在mysql数据库的user表中；
查看user表的结构：

```mysql-sql
use mysql;
desc user;
12
```

打印

```mysql-sql
+------------------------+-----------------------------------+------+-----+-----------------------+-------+   
| Field                  | Type                              | Null | Key | Default               | Extra |   
+------------------------+-----------------------------------+------+-----+-----------------------+-------+   
| Host                   | char(60)                          | NO   | PRI |                       |       |   
| User                   | char(32)                          | NO   | PRI |                       |       |   
| Select_priv            | enum('N','Y')                     | NO   |     | N                     |       |   
| Insert_priv            | enum('N','Y')                     | NO   |     | N                     |       |   
| Update_priv            | enum('N','Y')                     | NO   |     | N                     |       |   
| Delete_priv            | enum('N','Y')                     | NO   |     | N                     |       |   
| Create_priv            | enum('N','Y')                     | NO   |     | N                     |       |   
| Drop_priv              | enum('N','Y')                     | NO   |     | N                     |       |   
| Reload_priv            | enum('N','Y')                     | NO   |     | N                     |       |   
| Shutdown_priv          | enum('N','Y')                     | NO   |     | N                     |       |   
| Process_priv           | enum('N','Y')                     | NO   |     | N                     |       |   
| File_priv              | enum('N','Y')                     | NO   |     | N                     |       |   
| Grant_priv             | enum('N','Y')                     | NO   |     | N                     |       |   
| References_priv        | enum('N','Y')                     | NO   |     | N                     |       |   
| Index_priv             | enum('N','Y')                     | NO   |     | N                     |       |   
| Alter_priv             | enum('N','Y')                     | NO   |     | N                     |       |   
| Show_db_priv           | enum('N','Y')                     | NO   |     | N                     |       |   
| Super_priv             | enum('N','Y')                     | NO   |     | N                     |       |   
| Create_tmp_table_priv  | enum('N','Y')                     | NO   |     | N                     |       |   
| Lock_tables_priv       | enum('N','Y')                     | NO   |     | N                     |       |   
| Execute_priv           | enum('N','Y')                     | NO   |     | N                     |       |   
| Repl_slave_priv        | enum('N','Y')                     | NO   |     | N                     |       |   
| Repl_client_priv       | enum('N','Y')                     | NO   |     | N                     |       |   
| Create_view_priv       | enum('N','Y')                     | NO   |     | N                     |       |   
| Show_view_priv         | enum('N','Y')                     | NO   |     | N                     |       |   
| Create_routine_priv    | enum('N','Y')                     | NO   |     | N                     |       |   
| Alter_routine_priv     | enum('N','Y')                     | NO   |     | N                     |       |   
| Create_user_priv       | enum('N','Y')                     | NO   |     | N                     |       |   
| Event_priv             | enum('N','Y')                     | NO   |     | N                     |       |   
| Trigger_priv           | enum('N','Y')                     | NO   |     | N                     |       |   
| Create_tablespace_priv | enum('N','Y')                     | NO   |     | N                     |       |   
| ssl_type               | enum('','ANY','X509','SPECIFIED') | NO   |     |                       |       |   
| ssl_cipher             | blob                              | NO   |     | NULL                  |       |   
| x509_issuer            | blob                              | NO   |     | NULL                  |       |   
| x509_subject           | blob                              | NO   |     | NULL                  |       |   
| max_questions          | int(11) unsigned                  | NO   |     | 0                     |       |   
| max_updates            | int(11) unsigned                  | NO   |     | 0                     |       |   
| max_connections        | int(11) unsigned                  | NO   |     | 0                     |       |   
| max_user_connections   | int(11) unsigned                  | NO   |     | 0                     |       |   
| plugin                 | char(64)                          | NO   |     | mysql_native_password |       |   
| authentication_string  | text                              | YES  |     | NULL                  |       |   
| password_expired       | enum('N','Y')                     | NO   |     | N                     |       |   
| password_last_changed  | timestamp                         | YES  |     | NULL                  |       |   
| password_lifetime      | smallint(5) unsigned              | YES  |     | NULL                  |       |   
| account_locked         | enum('N','Y')                     | NO   |     | N                     |       |   
+------------------------+-----------------------------------+------+-----+-----------------------+-------+   
45 rows in set (0.00 sec)                                                                                     
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
```

主要字段说明：
Host表示允许访问的主机；
User表示用户名；
authentication_string表示密码，为加密后的值。
查看所有用户：

```mysql-sql
select host,user,authentication_string from user;
1
+-----------+---------------+-------------------------------------------+
| host      | user          | authentication_string                     |
+-----------+---------------+-------------------------------------------+
| localhost | root          | *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B |
| localhost | mysql.session | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
| localhost | mysql.sys     | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
| 127.0.0.1 | Tom           | *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9 |
| localhost | Tom2          | *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9 |
+-----------+---------------+-------------------------------------------+
5 rows in set (0.00 sec)
12345678910
```

#### （2）创建账户、授权

需要使用实例级账户登录后操作，以root为例
常用权限主要包括：create、alter、drop、insert、update、delete、select
如果分配所有权限，可以使用all privileges
语法：
`grant 权限列表 on 数据库 to '用户名'@'访问主机' identified by '密码';`

```mysql-sql
grant select on jingdong.* to 'Leo'@'localhost' identified by '123456';
--创建一个Mary的账号，密码为12345678，可以任意电脑进行链接访问, 并且对jd数据库中的所有表拥有所有权限
grant all privileges on jd.* to "Mary"@"%" identified by "12345678"
--刷新权限
flush privileges;
12345
```

说明：
可以操作数据库的所有表，方式为:jingdong.*；
访问主机通常使用 百分号% 表示此账户可以使用任何ip的主机登录访问此数据库；
访问主机可以设置成 localhost或具体的ip，表示只允许本机或特定主机访问。
查看用户有哪些权限：

```mysql-sql
show grants for Leo@localhost;
1
```

打印

```mysql-sql
+---------------------------------------------------+
| Grants for Leo@localhost                          |
+---------------------------------------------------+
| GRANT USAGE ON *.* TO 'Leo'@'localhost'           |
| GRANT SELECT ON `jingdong`.* TO 'Leo'@'localhost' |
+---------------------------------------------------+
2 rows in set (0.00 sec)                           
1234567
```

使用新创建的用户登录：

```mysql-sql
mysql -uLeo -p
1
```

查看数据库：

```mysql-sql
show databases;
1
```

打印

```mysql-sql
+--------------------+
| Database           |
+--------------------+
| information_schema |
| jingdong           |
+--------------------+
2 rows in set (0.00 sec)
1234567
```

进行删除操作

```mysql-sql
use jingdong;
drop table goods;
12
```

打印

```mysql-sql
ERROR 1142 (42000): DROP command denied to user 'Leo'@'localhost' for table 'goods'
1
```

报错，命令拒绝，因为只给Leo了查看权限，所以不能进行增加、修改、删除操作。

## 四、数据库存储引擎简介

### 1.MySQL体系结构：

- 连接层
- 服务层（核心）
- 存储引擎层
- 文件系统

如图：
![MySQL体系结构](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcua2FuY2xvdWQuY24vNDAvMDIvNDAwMjgyZTUzMTU1ODQ0NTM2ZWIyYzAyNjc2MGZkZGZfMTEwMng3MTcucG5n?x-oss-process=image/format,png)
**服务层**：
服务层是MySQL的核心，MySQL的核心服务层都在这一层，查询解析、SQL执行计划分析、SQL执行计划优化、查询缓存、以及跨存储引擎的功能都在这一层实现：存储过程，触发器，视图等。
下图是服务层的内部结构：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcua2FuY2xvdWQuY24vMmMvNzEvMmM3MWI1OTZkMDRkMjliOTlkNjYxMjg1NTNhNDNkNmNfMTA5Nng2MzUucG5n?x-oss-process=image/format,png)
**存储引擎层**：
负责MySQL中数据的存储与提取。
服务器中的查询执行引擎通过API与存储引擎进行通信，通过接口屏蔽了不同存储引擎之间的差异。
MySQL采用插件式的存储引擎。MySQL为我们提供了许多存储引擎，每种存储引擎有不同的特点。我们可以根据不同的业务特点，选择最适合的存储引擎。
如果对于存储引擎的性能不满意，可以通过修改源码来得到自己想要达到的性能。例如阿里巴巴的X-Engine，为了满足企业的需求facebook与google都对InnoDB存储引擎进行了扩充。

### 2.存储引擎操作

查看存储引擎：

```mysql-sql
show engines;
1
```

打印

```mysql-sql
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                        | Transactions | XA   | Savepoints |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES        |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables                          | NO           | NO   | NO         |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables      | NO           | NO   | NO         |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to it disappears) | NO           | NO   | NO         |
| MyISAM             | YES     | MyISAM storage engine                                          | NO           | NO   | NO         |
| CSV                | YES     | CSV storage engine                                             | NO           | NO   | NO         |
| ARCHIVE            | YES     | Archive storage engine                                         | NO           | NO   | NO         |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                             | NO           | NO   | NO         |
| FEDERATED          | NO      | Federated MySQL storage engine                                 | NULL         | NULL | NULL       |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
9 rows in set (0.01 sec)                                                                                            
1234567891011121314
```

查看默认的存储引擎：

```mysql-sql
show variables like '%storage_engines%';
1
```

打印

```mysql-sql
+--------------------------+-------+
| Variable_name            | Value |
+--------------------------+-------+
| disabled_storage_engines |       |
+--------------------------+-------+
1 row in set, 1 warning (0.00 sec)
```

# Python全栈（三）数据库优化之8.MySQL高级-存储引擎和基准测试

## 一、MySQL引擎之MyISAM

面对不同的需求，选择不同的存储引擎。

```mysql-sql
create table enginetest(id int) engine='myisam';
1
```

### 1.定义

MyISAM是MySQL5.5之前版本的默认存储引擎。
MyISAM存储引擎表由MYD（数据文件）和MYI（索引文件）组成。

### 2.特性

锁的作用：
管理共享资源的并发访问；
实现事务的隔离性。
锁的类型：

- 共享锁（读锁）：
  针对同一份数据,多个读操作可以同时进行而不会互相影响。
- 独占锁（写锁）：
  当前写操作没有完成前,它会阻断其他写锁和读锁。

锁的粒度：

- 表级锁
- 行级锁

MyISAM存储引擎的特性：
（1）并发性与锁级别：
使用表级锁，并发性较低；
（2）表损坏修复：
check语句检查表的状态，repair语句来修复表的状态。

```mysql-sql
check table test;
1
```

打印

```mysql-sql
+-----------+-------+----------+----------+
| Table     | Op    | Msg_type | Msg_text |
+-----------+-------+----------+----------+
| demo.test | check | status   | OK       |
+-----------+-------+----------+----------+
1 row in set (0.20 sec)
123456
repair table test;
1
```

打印

```mysql-sql
+-----------+--------+----------+----------+
| Table     | Op     | Msg_type | Msg_text |
+-----------+--------+----------+----------+
| demo.test | repair | status   | OK       |
+-----------+--------+----------+----------+
1 row in set (0.31 sec)
123456
```

（3）MyISAM表支持数据压缩：
命令：`myisampack -b -f myIsam.MYI`
为Linux命令，Windows不支持。
存储引擎限制：
版本 < MySQL5.0时默认表大小为4G,如存储大表则要修改MAX_Rows和AVG_ROW_LENGTH;
版本 > MySQL5.0时默认支持256TB。

### 3.适用场景：

（1）非事务型应用：
不支持事务场景适合。
（2）只读类应用：
读取较快，适合于只读应用。

## 二、MySQL引擎之InnoDB

### 1.定义

InnoDB是MySQL5.5之后版本的默认存储引擎。
InnoDB支持事务的ACID特性。
InnoDB使用表空间进行数据存储：
`innodb_file_per_table`的值表示存储的表空间类型：
on：独立表空间，tablename.ibd，是默认的；
off：系统表空间，ibdaraX（X是一个数字）。
通过以下语句查看数据库的表空间类型：

```mysql-sql
show variables like 'innodb_file_per_table';
1
```

打印

```mysql-sql
+-----------------------+-------+
| Variable_name         | Value |
+-----------------------+-------+
| innodb_file_per_table | ON    |
+-----------------------+-------+
1 row in set, 1 warning (0.01 sec)
123456
```

通过以下语句修改类型：

```mysql-sql
set global innodb_file_per_table=off;
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.00 sec)
1
```

命令行修改，只是临时修改，不会永久性改变，重启数据库会恢复默认；
配置文件修改，会永久修改。
系统表空间和独立表空间的选择：
系统表空间会产生IO瓶颈，刷新数据的时候是顺序进行的，因此会产生文件的IO瓶颈；
独立表空间可以同时向多个文件刷新数据，一般情况下会选择独立表空间。

### 2.InnoDB存储引擎的特性：

（1）支持事务的ACID特性；
（2）支持行级锁，可以最大程度地支持并发。

MyISAM和InnoDB的对比：
如图
![对比](https://img-blog.csdnimg.cn/20191215201409511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 三、MySQL引擎之CSV

### 1.文件系统存储特点：

数据为以文本方式存储在文件中；
.csv文件存储表内容；
.csm文件存储表的元数据如表状态和数据量；
.frm文件存储数据表结构信息。

### 2.特点：

（1）以csv格式进行数据存储：
可以通过Excel或其他对csv文件编辑的方式对数据进行编辑，编辑后在命令行中通过`flush tables;`命令进行刷新数据再查看，但是一般不建议通过这种方式编辑数据，因为在其他方式（如命令行、可视化界面）编辑等同时存在时会产生同步的延迟和数据错误等问题。
（2）不支持自增。
（3）不支持索引。
（4）不支持空字段。

### 3.使用场景：

适合作为数据交换的中间表。

## 四、MySQL引擎之Memory

### 1.定义

也称HEAP存储引擎，数据保存在内存中，如果MySQL服务重启数据会丢失，数据表结构会保存下来。

### 2.功能特点

（1）支持HASH索引和BTREE索引：
hash索引一般用于等值查找，btree一般用于范围查找。
Memory支持两种索引，其他引擎只支持btree，所以可为Memory数据表指定索引类型。
查看表索引：

```mysql-sql
show index from test_memory;
1
```

打印

```mysql-sql
+-------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table       | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+-------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| test_memory |          0 | PRIMARY  |            1 | id          | NULL      |           0 |     NULL | NULL   |      | HASH       |         |               |
+-------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
1 row in set (0.01 sec)
123456
```

指定新的索引类型：

```mysql-sql
create index idx_name using btree on test_memory(name(31));
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

再次查看索引

```mysql-sql
show index from test_memory;
1
```

打印

```mysql-sql
+-------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table       | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+-------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| test_memory |          0 | PRIMARY  |            1 | id          | NULL      |           0 |     NULL | NULL   |      | HASH       |         |               |
| test_memory |          1 | idx_name |            1 | name        | A         |        NULL |     NULL | NULL   | YES  | BTREE      |         |               |
+-------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
2 rows in set (0.00 sec)
1234567
```

（2）所有字段都为固定长度varchar(10)=char(10)；
（3）不支持BLOB和TEXT等大字段；
（4）Memory存储引擎使用表级锁。

### 3.选择存储引擎

除非需要使用到某些InnoDB不具备的特性，并且没有其他办法可以代替，否则都应该优先选择InnoDB引擎，并且一般情况下**不要混用**。
应用举例：

- 事务：
  如果应用需要事务支持，那么InnoDB(或者XtraDB)是目前最稳定并且经过验证的选择；
- 备份：
  如果可以定期地关闭服务器来执行备份，那么备份的因素可以忽略；反之，如果需要在线热备份，那么选择InnoDB就是基本的要求。
- 崩溃恢复：
  MyISAM崩溃后发生损坏的概率比InnoDB要高很多，而且恢复速度也要慢。

### 4.应用举例：

日志型应用：
MyISAM，因为大部分只读和插入，速度较快。
只读或大部分情况只读的表：
MyISAM，读取较快。
订单处理：
InnoDB，涉及到事务和资金处理。

## 五、基准测试

### 1.定义

基准测试是一种测量和评估软件性能指标的活动，用于建立某个时刻的性能基准,以便当系统发生软硬件变化时重新进行基准测试以评估变化对性能的影响。
基准测试是针对系统设置的一种压力测试。

### 2.基准测试特点

基准测试特点：
直接、简单、易于比较,用于评估服务器的处理能力；
可能不关心业务逻辑,所使用的查询和业务的真实性可以和业务环境没关系。
压力测试特点：
对真实的业务数据进行测试,获得真实系统所能承受的压力；
需要针对不同主题,所使用的数据和查询也是真实用到的。
基准测试是**简化的压力测试**。

### 3.基准测试的目的

建立MySQL服务器的性能基准线,确定当前MySQL服务器运行情况,确定优化之后的效果；
模拟比当前系统更高的负载,已找出系统的扩展瓶颈,可以增加数据库并发,观察QPS(每秒处理的查询数),TPS(每秒处理的事务数)变化,确定并发量与性能最优的关系；
测试不同的硬件、软件和操作系统配置；
证明新的硬件设备是否配置正确。

### 4.如何进行基准测试

对整个系统进行基准测试：
优点：

- 能够测试整个系统的性能,包括web服务器缓存、数据库等；
- MySQL并不总是出现性能问题的瓶颈,如果只关注MySQL可能忽略其他问题,能反映出系统中各个组件接口间的性能问题，体现真实性能状况。

缺点：

- 测试设计复杂,消耗时间长。

对MySQL进行基准测试：
优点：

- 测试设计简单,所耗费时间短；

缺点：

- 无法全面了解整个系统的性能基线。

### 5.MySQL基准测试的常见指标

（1）TPS：
单位时间内处理的事务数
MySQL中查询语句为

```mysql-sql
--提交事务速率查询
show global status like 'Com_commit';
12
```

打印

```mysql-sql
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| Com_commit    | 0     |
+---------------+-------+
1 row in set (0.01 sec)
123456
--回滚事务速率查询
show global status like 'Com_rollback';
12
```

打印

```mysql-sql
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| Com_rollback  | 0     |
+---------------+-------+
1 row in set (0.00 sec)
123456
```

（2）QPS：
单位时间内处理的查询数
MySQL中查询语句为

```mysql-sql
show global status like 'Question%';
1
```

打印

```mysql-sql
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| Questions     | 436   |
+---------------+-------+
1 row in set (0.00 sec)  
123456
```

### 6.MySQL基准测试工具

#### mysqlslap：

MySQL5.5以后自带，可以模拟服务器负载,并输出相关统计信息。
常用参数说明：

- auto-generate-sql 由系统自动生成SQL脚本进行测试
- auto-generate-sql-add-autoincrement 在生成的表中增加自增ID
- auto-generate-sql-load-type 指定测试中使用的查询类型 读写或者混合,默认是混合
- auto-generate-sql-write-number 指定初始化数据时生成的数据量
- concurrency 指定并发线程的数量 1,10,50,200
- engine 指定要测试表的存储引擎,可以用逗号分割多个存储引擎
- no-drop 指定不清理测试数据
- iterations 指定测试运行的次数 指定了这个不能指定no-drop
- number-of-queries 指定每一个线程执行的查询数量
- debug-info 指定输出额外的内存及CPU统计信息
- number-int-cols 指定测试表中包含的INT类型列的数量
- number-char-cols 指定测试表中包含的varchar类型的数量
- create-schema 指定了用于执行测试的数据库的名字
- query 用于指定自定义SQL的脚本
- only-print 并不运行测试脚本,而是把生成的脚本打印出来
  测试举例：
  `mysqlslap --concurrency=1,50,100,200 --iterations=3 --number-int-cols=5 --number-char-cols=5 --auto-generate-sql --aut o-generate-sql-add-autoincrement --engine=myisam,innodb --number-of-queries=10 --create-schema=test –uroot -proot | more`
  结果为

```bash
Benchmark
        Running for engine myisam
        Average number of seconds to run all queries: 0.000 seconds
        Minimum number of seconds to run all queries: 0.000 seconds
        Maximum number of seconds to run all queries: 0.000 seconds
        Number of clients running queries: 1
        Average number of queries per client: 10

Benchmark
        Running for engine myisam
        Average number of seconds to run all queries: 0.036 seconds
        Minimum number of seconds to run all queries: 0.031 seconds
        Maximum number of seconds to run all queries: 0.047 seconds
        Number of clients running queries: 50
        Average number of queries per client: 0

Benchmark
        Running for engine myisam
        Average number of seconds to run all queries: 0.056 seconds
        Minimum number of seconds to run all queries: 0.046 seconds
        Maximum number of seconds to run all queries: 0.062 seconds
        Number of clients running queries: 100
        Average number of queries per client: 0

Benchmark
        Running for engine myisam
        Average number of seconds to run all queries: 0.140 seconds
        Minimum number of seconds to run all queries: 0.109 seconds
        Maximum number of seconds to run all queries: 0.187 seconds
        Number of clients running queries: 200
        Average number of queries per client: 0

Benchmark
        Running for engine innodb
        Average number of seconds to run all queries: 0.010 seconds
        Minimum number of seconds to run all queries: 0.000 seconds
        Maximum number of seconds to run all queries: 0.016 seconds
        Number of clients running queries: 1
        Average number of queries per client: 10

Benchmark
        Running for engine innodb
        Average number of seconds to run all queries: 0.401 seconds
        Minimum number of seconds to run all queries: 0.063 seconds
        Maximum number of seconds to run all queries: 1.063 seconds
        Number of clients running queries: 50
        Average number of queries per client: 0

Benchmark
        Running for engine innodb
        Average number of seconds to run all queries: 0.458 seconds
        Minimum number of seconds to run all queries: 0.125 seconds
        Maximum number of seconds to run all queries: 1.125 seconds
        Number of clients running queries: 100
        Average number of queries per client: 0

Benchmark
        Running for engine innodb
        Average number of seconds to run all queries: 0.313 seconds
        Minimum number of seconds to run all queries: 0.313 seconds
        Maximum number of seconds to run all queries: 0.313 seconds
        Number of clients running queries: 200
        Average number of queries per client: 0
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263
```

#### sysbench:

可参考[https://www.cnblogs.com/kismetv/archive/2017/09/30/7615738.htm](https://www.cnblogs.com/kismetv/archive/2017/09/30/7615738.html)

# Python全栈（三）数据库优化之9.MySQL高级-explain分析SQL语句

## 一、影响服务器性能的几个方面

### 1.硬件条件原因

- 服务器硬件：
  内存大小等。
- 服务器的操作系统：
  不同系统（Linux、Windows）。
- 数据库存储引擎的选择：
  根据不同的需求选择不同的存储引擎。
- 数据库参数配置：
  不同的参数配置会影响到性能。
- 数据库结构设计和SQL语句：
  拆表后，小表的查询效率更快。

### 2.SQL本身性能下降原因

- 查询语句写的不好：
  语句太长，有太多连接。
- 索引失效：
  建立的索引未用上。
- 关联查询太多join：
  有太多连接，降低性能。
- 服务器调优及各个参数设置。

### 3.SQL语句加载顺序

手写SQL代码的顺序：

```mysql-sql
select distinct 
    <select _list>
from 
    <left_table>
join  <right_table> on <join_codition>
where
    <where_condition>
group by
    <group_by_list>
having
    <having_condition>
order by
    <order_by_condition>
limit <limit number>
1234567891011121314
```

机器读取执行代码的顺序：
![机器执行顺序](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcua2FuY2xvdWQuY24vY2IvOWUvY2I5ZTgxM2I1ZTRmYjUyMzNlYzUwMWJiNDJjNTdmMmRfNDAweDMzMC5wbmc#pic_center?x-oss-process=image/format,png)
如图
![SQL解析](https://img-blog.csdnimg.cn/20191216164812562.png)

### 4.MySQL常见瓶颈

- CPU:
  CPU在饱和的时候一般发生在数据装入内存或从磁盘读取数据的时候；
- IO:
  磁盘I/O瓶颈发生在装入数据远大于内存容量的时候；
- 服务器硬件的性能瓶颈：
  内存、硬盘大小等。

## 二、explain分析SQL

### 1.基本定义

使用explain关键字可以**模拟优化器**执行SQL查询语句,从而知道MySQL是如何处理SQL语句的。

### 2.explain的作用：

- 表的读取顺序；
- 数据读取操作的操作类型；
- 哪些索引可以使用；
- 哪些索引被实际使用；
- 表之间的引用；
- 每张表有多少行被优化器查询。

### 3.explain的使用

语法：

```mysql-sql
explain + SQL语句;
1
```

例如

```mysql-sql
explain select * from students;
1
```

打印

```mysql-sql
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table    | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+-------+
|  1 | SIMPLE      | students | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   27 |   100.00 | NULL  |
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
123456
```

### 4.explain字段解释

#### id：

表的读取顺序：
select查询的顺序号,包含一组数字,表示查询中执行select子句或操作表的顺序。
两种情况:
（1）id相同,执行顺序由上至下：
例如：

```mysql-sql
explain select test2.* from test1,test2,test3 where test1.id = test2.id and test1.id = test3.id;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+--------+---------------+---------+---------+---------------+------+----------+-------------+
| id | select_type | table | partitions | type   | possible_keys | key     | key_len | ref           | rows | filtered | Extra       |
+----+-------------+-------+------------+--------+---------------+---------+---------+---------------+------+----------+-------------+
|  1 | SIMPLE      | test1 | NULL       | index  | PRIMARY       | PRIMARY | 4       | NULL          |    1 |   100.00 | Using index |
|  1 | SIMPLE      | test2 | NULL       | eq_ref | PRIMARY       | PRIMARY | 4       | demo.test1.id |    1 |   100.00 | Using index |
|  1 | SIMPLE      | test3 | NULL       | eq_ref | PRIMARY       | PRIMARY | 4       | demo.test1.id |    1 |   100.00 | Using index |
+----+-------------+-------+------------+--------+---------------+---------+---------+---------------+------+----------+-------------+
3 rows in set, 1 warning (0.00 sec)                                                                                                   
12345678
```

（2）id不同,如果是子查询,id的序号会递增,id值越大优先级越高,越先被执行：
例如：

```mysql-sql
 explain select test2.* from test2 where id = (select id from test1 where id = (select test3.id from test3 where test3.other_columns = ''));
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------------+
| id | select_type | table | partitions | type  | possible_keys | key     | key_len | ref   | rows | filtered | Extra       |
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------------+
|  1 | PRIMARY     | test2 | NULL       | const | PRIMARY       | PRIMARY | 4       | const |    1 |   100.00 | Using index |
|  2 | SUBQUERY    | test1 | NULL       | const | PRIMARY       | PRIMARY | 4       | const |    1 |   100.00 | Using index |
|  3 | SUBQUERY    | test3 | NULL       | ALL   | NULL          | NULL    | NULL    | NULL  |    1 |   100.00 | Using where |
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------------+
3 rows in set, 1 warning (0.00 sec)                                                                                                  
12345678
```

显然，根据id的大小，表读取的顺序依次是test3、test1、test2，先执行子查询，再执行父查询，从底层到上层依次执行。
（3）id相同不同：
id时同时执行，不同时id越大越先被执行。

#### select_type：

数据读取操作的操作类型：
（1）simple：
简单的select查询,查询中不包含子查询或者union；
（2）primary：
查询中若包含任何复杂的子部分,最外层查询则被标记，就是最后加载的那一层查询；
（3）subquery：
在select或where列表中包含了子查询；
（4）union：
union是把两个表的查询结果连接起来。
若第二个select出现在union之后,则被标记为union，

```mysql-sql
explain select * from test2 union select * from test4;
1
```

打印

```mysql-sql
+----+--------------+------------+------------+-------+---------------+---------+---------+------+------+----------+-----------------+ 
| id | select_type  | table      | partitions | type  | possible_keys | key     | key_len | ref  | rows | filtered | Extra           | 
+----+--------------+------------+------------+-------+---------------+---------+---------+------+------+----------+-----------------+ 
|  1 | PRIMARY      | test2      | NULL       | index | NULL          | PRIMARY | 4       | NULL |    1 |   100.00 | Using index     | 
|  2 | UNION        | test4      | NULL       | index | NULL          | PRIMARY | 4       | NULL |    1 |   100.00 | Using index     | 
| NULL | UNION RESULT | <union1,2> | NULL       | ALL   | NULL          | NULL    | NULL    | NULL | NULL |     NULL | Using temporary 
+----+--------------+------------+------------+-------+---------------+---------+---------+------+------+----------+-----------------+ 
3 rows in set, 1 warning (0.01 sec)                                                                                                                                                                                                   
12345678
```

只有两张表的**字段个数相同**才能使用。
（5）union result：
从union表获取结果的select。

#### table：

显示这一行的数据是关于哪张表的。

#### partitions：

查询访问的分区。

#### type：

显示SQL的执行效率，包括ALL，index，range，ref，eq_ref，const、system，NULL等。
从最好到最差依次是：
system > const > eq_ref > ref > range > index > ALL。
（1）system：
表只有一行记录(等于系统表),这是const类型的特例，意义不大。
例如

```mysql-sql
explain select * from mysql.db;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+---------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table | partitions | type    | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+-------+------------+---------+---------------+------+---------+------+------+----------+-------+
|  1 | SIMPLE      | db    | NULL       | system  | NULL          | NULL | NULL    | NULL |    6 |   100.00 | NULL  |
+----+-------------+-------+------------+---------+---------------+------+---------+------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)                                                                                                                                                                                                                                                                                 
123456
```

（2）const：
表示通过索引一次就找到了,const用于比较primary key，使用较少。
例如

```mysql-sql
explain select * from test1 where id = 1;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type  | possible_keys | key     | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | test1 | NULL       | const | PRIMARY       | PRIMARY | 4       | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.01 sec)                                                                                                                                                                                                                                                                                
123456
```

（3）eq_ref：
唯一索引扫描,对于每个索引键,表中只有一条记录与之匹配
例如

```mysql-sql
explain select * from test1,test2 where test1.id = test2.id;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+--------+---------------+---------+---------+---------------+------+----------+-------------+
| id | select_type | table | partitions | type   | possible_keys | key     | key_len | ref           | rows | filtered | Extra       |
+----+-------------+-------+------------+--------+---------------+---------+---------+---------------+------+----------+-------------+
|  1 | SIMPLE      | test1 | NULL       | ALL    | PRIMARY       | NULL    | NULL    | NULL          |    1 |   100.00 | NULL        |
|  1 | SIMPLE      | test2 | NULL       | eq_ref | PRIMARY       | PRIMARY | 4       | demo.test1.id |    1 |   100.00 | Using index |
+----+-------------+-------+------------+--------+---------------+---------+---------+---------------+------+----------+-------------+
2 rows in set, 1 warning (0.01 sec)                                                                                                                                                                                                                                                                              
1234567
```

（4）ref：
非唯一性索引扫描,返回匹配某个单独值的所有行,本质上也是一种索引访问,它返回所有匹配某个单独值的行
例如

```mysql-sql
create index idx_col1_col2 on test3(col1,col2);
explain select * from test3 where col1 = 'test';
12
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+---------------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key           | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+---------------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | test3 | NULL       | ref  | idx_col1_col2 | idx_col1_col2 | 93      | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+---------------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)                                                                                                                                                                                                                                                                             
123456
```

（5）range：
只检索给定范围的行,使用一个索引来选择行，where语句中包含between…and…、in…条件时即为range，一般where语句中的字段是主键时才可能出现range。
例如

```mysql-sql
explain select * from test1 where id between 1 and 10;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type  | possible_keys | key     | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | test1 | NULL       | range | PRIMARY       | PRIMARY | 4       | NULL |    1 |   100.00 | Using where |
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)                                                                                                                                                                                                                                                                            
123456
```

（6）index：
Full Index Scan,index与ALL区别为index类型只遍历索引树。
例如

```mysql-sql
explain select id from test1;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+ 
| id | select_type | table | partitions | type  | possible_keys | key     | key_len | ref  | rows | filtered | Extra       | 
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+ 
|  1 | SIMPLE      | test1 | NULL       | index | NULL          | PRIMARY | 4       | NULL |    1 |   100.00 | Using index | 
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+ 
1 row in set, 1 warning (0.01 sec)                                                                                                                                                                                                                                                                                                                                                                    
123456
```

（7）ALL：
遍历全表找到匹配的行。
例如

```mysql-sql
explain select * from test1;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
|  1 | SIMPLE      | test1 | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)                                                                                                                                                                                                                                                                                                                                                                  
123456
```

#### possible_keys：

显示可能应用在这张表中的索引,一个或多个。

#### key：

实际使用的索引，如果为null,则没有使用索引。
例如

```mysql-sql
explain select col1,col2 from test3;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+---------------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type  | possible_keys | key           | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+-------+---------------+---------------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | test3 | NULL       | index | NULL          | idx_col1_col2 | 186     | NULL |    1 |   100.00 | Using index |
+----+-------------+-------+------------+-------+---------------+---------------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)                                                                                                                                                                                                                                                                                                                                                                                                                                                                 
123456
```

覆盖索引，查询列要被所建的索引覆盖，上例中查询的列col1、col2即被创建的索引idx_col1_col2覆盖。
查询中若使用了覆盖索引,则该索引仅出现在key列表中。

#### key_len：

表示索引中使用的字节数,可通过该列计算查询中使用的索引长度。
在不损失精确性的情况下,长度越短越好。
key_len显示的值为索引字段的最大可能长度,**并非实际使用长度**,即key_len是根据表定义计算而得,不是通过表内检索出的。
索引长度计算：

```bash
varchr(n)变长字段且允许NULL：
n*(Character Set：utf8=3,gbk=2,latin1=1)+1(NULL)+2(变长字段)；
varchr(n)变长字段且不允许NULL：
n*(Character Set：utf8=3,gbk=2,latin1=1)+2(变长字段)；
char(n)固定字段且允许NULL：
n*(Character Set：utf8=3,gbk=2,latin1=1)+1(NULL)；
char(n)固定字段且不允许NULL：
n*(Character Set：utf8=3,gbk=2,latin1=1)。
12345678
```

上例中，字符集为utf8为3，允许非空1，可变长2，并且有2个字段，所以有
key_len=(30×3+1+2)×2=186。

#### ref：

显示索引哪一列被使用到了。
在id表的读取顺序部分示例的结果中，ref列的第2、3个值为demo.test1.id，表示demo数据库中的test1表中的id字段被用到。

#### rows：

根据表统计信息及索引选用情况,大致估算出找到所需的记录所需要读取的行数。
如图
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcua2FuY2xvdWQuY24vYmMvYTMvYmNhM2RlNWZkZTU1ZGYwMGJhOTY3ZDU4Y2M4MThlNDJfOTcyeDMwMS5wbmc#pic_center?x-oss-process=image/format,png)

#### Extra：

包含不适合在其他列中显示但十分重要的额外信息。
（1）Using filesort：
说明mysql会对数据使用一个外部的索引排序,而不是按照表内的索引顺序进行读取,MySQL中无法利用索引完成的排序操作称为"文件排序"，说明需要进行优化。
例如

```mysql-sql
drop index idx_col1_col2 from test3;
create index idx_col1_col2_col3 on test3(col1,col2,col3);
explain select * from test3 where col1 = 'test' order by col3;
123
```

打印

```mysql-sql
+----+-------------+-------+------------+------+--------------------+--------------------+---------+-------+------+----------+---------------------------------------+
| id | select_type | table | partitions | type | possible_keys      | key                | key_len | ref   | rows | filtered | Extra                                 |
+----+-------------+-------+------------+------+--------------------+--------------------+---------+-------+------+----------+---------------------------------------+
|  1 | SIMPLE      | test3 | NULL       | ref  | idx_col1_col2_col3 | idx_col1_col2_col3 | 93      | const |    1 |   100.00 | Using index condition; Using filesort |
+----+-------------+-------+------------+------+--------------------+--------------------+---------+-------+------+----------+---------------------------------------+
1 row in set, 1 warning (0.00 sec)                                                                                                                                                                                                                                                                                                                                                                                                                                                                 
123456
```

显然，此时出现了filesort，需要进行优化。
例如

```mysql-sql
explain select * from test3 where col1 = 'test' order by col2,col3;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+--------------------+--------------------+---------+-------+------+----------+-----------------------+
| id | select_type | table | partitions | type | possible_keys      | key                | key_len | ref   | rows | filtered | Extra                 |
+----+-------------+-------+------------+------+--------------------+--------------------+---------+-------+------+----------+-----------------------+
|  1 | SIMPLE      | test3 | NULL       | ref  | idx_col1_col2_col3 | idx_col1_col2_col3 | 93      | const |    1 |   100.00 | Using index condition |
+----+-------------+-------+------------+------+--------------------+--------------------+---------+-------+------+----------+-----------------------+
1 row in set, 1 warning (0.01 sec)                                                                                                                                                                                                                                                                                                                                                                                                                                                                 
123456
```

此时不再有filesort。
（2）Using temporary：
使用临时表保存中间结果,MySQL在对查询结果排序时使用临时表，一般出现在使用了order by、group by的时候，性能极差。
例如

```mysql-sql
explain select * from test3 where col1 in ('te','st') group by col2;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+--------------------+------+---------+------+------+----------+----------------------------------------------+
| id | select_type | table | partitions | type | possible_keys      | key  | key_len | ref  | rows | filtered | Extra                                        |
+----+-------------+-------+------------+------+--------------------+------+---------+------+------+----------+----------------------------------------------+
|  1 | SIMPLE      | test3 | NULL       | ALL  | idx_col1_col2_col3 | NULL | NULL    | NULL |    1 |   100.00 | Using where; Using temporary; Using filesort |
+----+-------------+-------+------------+------+--------------------+------+---------+------+------+----------+----------------------------------------------+                                                                                                                                                                                                                                                                                                                                                                                                                                                                
1 row in set, 1 warning (0.01 sec)
123456
```

显然，同时出现了filesort、temporary，需要优化。
例如

```mysql-sql
explain select * from test3 where col1 in ('te','st') group by col1,col2;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+--------------------+--------------------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type  | possible_keys      | key                | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+-------+--------------------+--------------------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | test3 | NULL       | index | idx_col1_col2_col3 | idx_col1_col2_col3 | 279     | NULL |    1 |   100.00 | Using where |
+----+-------------+-------+------------+-------+--------------------+--------------------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)                                                                                                                                                                                                                                                                                                                                                                                                                           
123456
```

此时filesort、temporary均消失，我们应该尽量按照**建立索引的顺序**进行分组、排序。
（3）Using index：
使用了索引,避免了全表扫描。
（4）Using where：
使用了where过滤。
（5）Using join buffer：
使用了连接缓存。
（6）impossible where：
不可能的条件,where子句的值总是false。
例如

```mysql-sql
explain select * from test3 where id = 1 and id = 2;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra            |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------+
|  1 | SIMPLE      | NULL  | NULL       | NULL | NULL          | NULL | NULL    | NULL | NULL |     NULL | Impossible WHERE |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------+
1 row in set, 1 warning (0.01 sec)                                                                                    
```

# Python全栈（三）数据库优化之10.MySQL高级-表优化和索引优化

## 一、单表优化

建表：

```mysql-sql
create table article(
    id int unsigned not null primary key auto_increment,
    author_id int unsigned not null,
    category_id int unsigned not null,
    views int unsigned not null,
    comments int unsigned not null,
    title varchar(255) not null,
    content text not null
);
123456789
```

打印

```mysql-sql
Query OK, 0 rows affected (0.05 sec)
1
```

插入数据：

```mysql-sql
insert into article(`author_id`,`category_id`,`views`,`comments`,`title`,`content`) values 
(1,1,1,1,'1','1'),
(2,2,2,2,'2','2'),
(1,1,3,3,'3','3');
1234
```

打印

```mysql-sql
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0
12
```

查询category_id为1且comments大于1的情况下,views最多的article_id：

```mysql-sql
select * from article where category_id = 1 and comments > 1 order by views desc limit 1;
1
```

打印

```mysql-sql
+----+-----------+-------------+-------+----------+-------+---------+
| id | author_id | category_id | views | comments | title | content |
+----+-----------+-------------+-------+----------+-------+---------+
|  3 |         1 |           1 |     3 |        3 | 3     | 3       |
+----+-----------+-------------+-------+----------+-------+---------+
1 row in set (0.00 sec)
123456
```

explain语句分析

```mysql-sql
explain select * from article where category_id = 1 and comments > 1 order by views desc limit 1;
1
```

打印

```mysql-sql
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-----------------------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                       |
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-----------------------------+
|  1 | SIMPLE      | article | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    3 |    33.33 | Using where; Using filesort |
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-----------------------------+
1 row in set, 1 warning (0.01 sec)                                                                                                        
123456
```

type为all，全表扫描，且Using filesort，需要进行优化。
优化尝试：

```mysql-sql
--建立复合索引
create index idx_article_ccv on article(category_id,comments,views);
explain select * from article where category_id = 1 and comments > 1 order by views desc limit 1;
123
```

打印

```mysql-sql
+----+-------------+---------+------------+-------+-----------------+-----------------+---------+------+------+----------+---------------------------------------+
| id | select_type | table   | partitions | type  | possible_keys   | key             | key_len | ref  | rows | filtered | Extra                                 |
+----+-------------+---------+------------+-------+-----------------+-----------------+---------+------+------+----------+---------------------------------------+
|  1 | SIMPLE      | article | NULL       | range | idx_article_ccv | idx_article_ccv | 8       | NULL |    1 |   100.00 | Using index condition; Using filesort |
+----+-------------+---------+------------+-------+-----------------+-----------------+---------+------+------+----------+---------------------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

显然，type变成了range并且出现了Using index condition，但是Using filesort仍然存在，未解决问题，改变条件语句进行尝试可知，views字段索引是多余的，所以在索引中去掉views，再次尝试。

```mysql-sql
drop index idx_article_ccv on article;
--重新建索引
create index idx_article_cv on article(category_id,views);
explain select * from article where category_id = 1 and comments > 1 order by views desc limit 1;
1234
```

打印

```mysql-sql
+----+-------------+---------+------------+------+----------------+----------------+---------+-------+------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys  | key            | key_len | ref   | rows | filtered | Extra       |
+----+-------------+---------+------------+------+----------------+----------------+---------+-------+------+----------+-------------+
|  1 | SIMPLE      | article | NULL       | ref  | idx_article_cv | idx_article_cv | 4       | const |    2 |    33.33 | Using where |
+----+-------------+---------+------------+------+----------------+----------------+---------+-------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)                                                                                                    
123456
```

此时type为ref，且Using filesort未出现，达到优化效果。

## 二、双表优化

建表：

```mysql-sql
--商品类别
create table class(
    id int unsigned not null primary key auto_increment,
    card int unsigned not null
);

--图书表
create table book(
    bookid int unsigned not null auto_increment primary key,
    card int unsigned not null
);
1234567891011
```

打印

```mysql-sql
Query OK, 0 rows affected (0.02 sec)

Query OK, 0 rows affected (0.01 sec)
123
```

插入随机数据：
MySQL中，rand()函数返回随机小数（0-1），floor()函数返回小数向下取整的结果。

```mysql-sql
--向class表插入数据
insert into class(card) values(floor(rand()*100));
--向book表插入数据
insert into book(card) values(floor(rand()*100));
1234
```

反复执行多次可以插入多条数据。
联合查询：

```mysql-sql
select * from class left join book on class.card = book.card;
1
```

打印

```mysql-sql
+----+------+--------+------+
| id | card | bookid | card |
+----+------+--------+------+
| 10 |    6 |      9 |    6 |
| 12 |   13 |     10 |   13 |
|  1 |   12 |   NULL | NULL |
|  2 |   43 |   NULL | NULL |
|  3 |   78 |   NULL | NULL |
|  4 |   63 |   NULL | NULL |
|  5 |   81 |   NULL | NULL |
|  6 |   15 |   NULL | NULL |
|  7 |   32 |   NULL | NULL |
|  8 |   18 |   NULL | NULL |
|  9 |   92 |   NULL | NULL |
| 11 |   51 |   NULL | NULL |
| 13 |   16 |   NULL | NULL |
| 14 |   39 |   NULL | NULL |
| 15 |   47 |   NULL | NULL |
| 16 |   21 |   NULL | NULL |
| 17 |   63 |   NULL | NULL |
| 18 |   53 |   NULL | NULL |
| 19 |   78 |   NULL | NULL |
| 20 |   31 |   NULL | NULL |
+----+------+--------+------+
20 rows in set (0.00 sec)    
12345678910111213141516171819202122232425
```

性能测试

```mysql-sql
explain select * from class left join book on class.card = book.card;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                                              |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
|  1 | SIMPLE      | class | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   20 |   100.00 | NULL                                               |
|  1 | SIMPLE      | book  | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   20 |   100.00 | Using where; Using join buffer (Block Nested Loop) |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
2 rows in set, 1 warning (0.00 sec)
1234567
```

type为all，全表扫描，且出现Using join buffer连接缓存，需要优化。
先向book表加索引

```mysql-sql
--向右表加索引
create index idx_book_card on book(card);
explain select * from class left join book on class.card = book.card;
123
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+---------------+---------+-----------------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key           | key_len | ref             | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+---------------+---------+-----------------+------+----------+-------------+
|  1 | SIMPLE      | class | NULL       | ALL  | NULL          | NULL          | NULL    | NULL            |   20 |   100.00 | NULL        |
|  1 | SIMPLE      | book  | NULL       | ref  | idx_book_card | idx_book_card | 4       | demo.class.card |    1 |   100.00 | Using index |
+----+-------------+-------+------------+------+---------------+---------------+---------+-----------------+------+----------+-------------+
2 rows in set, 1 warning (0.00 sec)
1234567
```

此时，book表的type变为ref，且Using join buffer消失。
向class表加索引

```mysql-sql
drop index idx_book_card on book;
--向左表加索引
create index idx_class_card on class(card);
explain select * from class left join book on class.card = book.card;
1234
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+----------------+---------+------+------+----------+----------------------------------------------------+
| id | select_type | table | partitions | type  | possible_keys | key            | key_len | ref  | rows | filtered | Extra                                              |
+----+-------------+-------+------------+-------+---------------+----------------+---------+------+------+----------+----------------------------------------------------+
|  1 | SIMPLE      | class | NULL       | index | NULL          | idx_class_card | 4       | NULL |   20 |   100.00 | Using index                                        |
|  1 | SIMPLE      | book  | NULL       | ALL   | NULL          | NULL           | NULL    | NULL |   20 |   100.00 | Using where; Using join buffer (Block Nested Loop) |
+----+-------------+-------+------------+-------+---------------+----------------+---------+------+------+----------+----------------------------------------------------+
2 rows in set, 1 warning (0.00 sec)                                                                                                                                       
1234567
```

此时只是class的type变为index，其他未变化。
我们可得出，左连接往右表加索引优化效果更好，因为左连接时左表全部包含在内，而右表只是包含部分数据；
同理，右连接往左表建立索引。
驱动表：
mysql中指定了连接条件时，满足查询条件的记录行数少的表为驱动表；如未指定查询条件，则扫描行数少的为驱动表。mysql优化器就是以小表驱动大表的方式来决定执行顺序的。

```mysql-sql
--指定class表先载入
explain select * from class straight_join book on class.card = book.card;
12
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                                              |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
|  1 | SIMPLE      | class | NULL       | ALL  | NULL          | NULL | NULL    | NULL |  100 |   100.00 | NULL                                               |
|  1 | SIMPLE      | book  | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 1000 |    10.00 | Using where; Using join buffer (Block Nested Loop) |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
2 rows in set, 1 warning (0.00 sec)                                                                                                                                                                                                                                                                           
1234567
--指定book表先载入
explain select * from book straight_join class on class.card = book.card;
12
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                                              |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
|  1 | SIMPLE      | book  | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 1000 |   100.00 | NULL                                               |
|  1 | SIMPLE      | class | NULL       | ALL  | NULL          | NULL | NULL    | NULL |  100 |    10.00 | Using where; Using join buffer (Block Nested Loop) |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
2 rows in set, 1 warning (0.00 sec)                                                                                                                                                                                                                                                                           
1234567
```

`straight_join`实现强制多表的载入顺序，选择其左边的表先载入；而join语法是根据“哪个表的结果集小，就以哪个表为驱动表”来决定谁先载入的
使用STRAIGHT_JOIN一定要慎重，因为啊部分情况下认为指定的执行顺序并不一定会比优化引擎要靠谱。

## 三、三表优化

建立新表并插入多条数据

```mysql-sql
--手机
create table phone(
    phoneid int unsigned not null primary key auto_increment,
    card int unsigned not null
);
12345
insert into phone(card) values(floor(rand()*20));
1
```

进行测试

```mysql-sql
create index idx_book_card on book(card);
create index idx_phone_card on phone(card);
explain select * from class left join book on class.card = book.card left join phone on book.id = phone.id;
123
```

打印

```mysql-sql
+----+-------------+-------+------------+------+----------------+----------------+---------+-----------------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys  | key            | key_len | ref             | rows | filtered | Extra       |
+----+-------------+-------+------------+------+----------------+----------------+---------+-----------------+------+----------+-------------+
|  1 | SIMPLE      | class | NULL       | ALL  | NULL           | NULL           | NULL    | NULL            |  100 |   100.00 | NULL        |
|  1 | SIMPLE      | book  | NULL       | ref  | idx_book_card  | idx_book_card  | 4       | demo.class.card |    1 |   100.00 | Using index |
|  1 | SIMPLE      | phone | NULL       | ref  | idx_phone_card | idx_phone_card | 4       | demo.book.card  |    1 |   100.00 | Using index |
+----+-------------+-------+------------+------+----------------+----------------+---------+-----------------+------+----------+-------------+
3 rows in set, 1 warning (0.00 sec)
12345678
```

## 四、索引优化

### 1.建表：

```mysql-sql
create table staffs(
    id int primary key auto_increment,
    name varchar(24) not null default "",
    age int not null default 0,
    pos varchar(20) not null default "",
    add_time timestamp not null default CURRENT_TIMESTAMP 
)charset utf8;
create table user(
    id int not null auto_increment primary key,
    name varchar(20) default null,
    age int default null,
    email varchar(20) default null
) engine=innodb default charset=utf8;
12345678910111213
```

打印

```mysql-sql
Query OK, 0 rows affected (0.02 sec)

Query OK, 0 rows affected (0.01 sec)
123
```

### 2.插入数据：

```mysql-sql
insert into staffs(`name`,`age`,`pos`,`add_time`) values('z3',22,'manager',now());
insert into staffs(`name`,`age`,`pos`,`add_time`) values('July',23,'dev',now());
insert into staffs(`name`,`age`,`pos`,`add_time`) values('2000',23,'dev',now());
insert into user(name,age,email) values('1aa1',21,'b@163.com');
insert into user(name,age,email) values('2aa2',22,'a@163.com');
insert into user(name,age,email) values('3aa3',23,'c@163.com');
insert into user(name,age,email) values('4aa4',25,'d@163.com');
1234567
```

打印

```mysql-sql
Query OK, 1 row affected (0.01 sec)

Query OK, 1 row affected (0.00 sec)

Query OK, 1 row affected (0.01 sec)

Query OK, 1 row affected (0.00 sec)

Query OK, 1 row affected (0.00 sec)

Query OK, 1 row affected (0.00 sec)
1234567891011
```

### 3.建立复合索引：

```mysql-sql
create index idx_staffs_nameAgePos on staffs(name,age,pos);
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

### 4.口诀：

> 全值匹配我最爱,最左前缀要遵守；
> 带头大哥不能死,中间兄弟不能断；
> 索引列上少计算,范围之后全失效；
> like百分写最右,覆盖索引不写星；
> 不等空值还有or,索引失效要少用；
> varchar引号不可丢,SQL高级也不难。

解释：

#### 全值匹配我最爱：

查询使用的字段和建立的索引对应的字段完全匹配。

```mysql-sql
explain select * from staffs where name = 'July';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-------+
| id | select_type | table  | partitions | type | possible_keys         | key                   | key_len | ref   | rows | filtered | Extra |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | staffs | NULL       | ref  | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 74      | const |    1 |   100.00 | NULL  |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.01 sec)
123456
```

此时key_len为74，只用到了name。
增加一个字段age：

```mysql-sql
explain select * from staffs where name = 'July' and age = 23;
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------+------+----------+-------+
| id | select_type | table  | partitions | type | possible_keys         | key                   | key_len | ref         | rows | filtered | Extra |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------+------+----------+-------+
|  1 | SIMPLE      | staffs | NULL       | ref  | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 78      | const,const |    1 |   100.00 | NULL  |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
123456
```

此时key_len变为78，索引多了一个age。
再增加字段pos：

```mysql-sql
explain select * from staffs where name = 'July' and age = 23 and pos = 'dev';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------------+------+----------+-------+
| id | select_type | table  | partitions | type | possible_keys         | key                   | key_len | ref               | rows | filtered | Extra |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------------+------+----------+-------+
|  1 | SIMPLE      | staffs | NULL       | ref  | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 140     | const,const,const |    1 |   100.00 | NULL  |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
123456
```

此时key_len变为140，索引多了一个pos。
索引的个数变化，个数越多索引越长，查询范围越小，越快，第3种情况即为全值匹配，建立索引的字段和where条件句中的字段完全匹配。

#### 最左前缀要遵守：

查询从索引的最左前列开始，并且不跳过索引中的列：

##### 带头大哥不能死：

建立索引的第一个字段不能缺失，即查询从索引的最左前列开始。

```mysql-sql
explain select * from staffs where age = 23 and pos = 'dev';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | staffs | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    3 |    33.33 | Using where |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
123456
```

显然没有Using index condition，即没有使用到索引。

##### 中间兄弟不能断：

建立索引的中间字段不能缺失，即不能跳过索引中的列。

```mysql-sql
explain select * from staffs where name = 'July' and pos = 'dev';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-----------------------+
| id | select_type | table  | partitions | type | possible_keys         | key                   | key_len | ref   | rows | filtered | Extra                 |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-----------------------+
|  1 | SIMPLE      | staffs | NULL       | ref  | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 74      | const |    1 |    33.33 | Using index condition |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-----------------------+
1 row in set, 1 warning (0.01 sec)
123456
```

key_len为74，说明用到了1个索引，即只用到了name，索引中的字段在where条件句中中间的字段不能缺失，否则缺失后面的字段索引将失效，如例中的pos字段即失效。

#### 索引列上少计算：

不能**在where条件语句的索引列**上直接进行数值计算、调用函数。

```mysql-sql
explain select * from staffs where lower(name) = 'july';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | staffs | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    3 |   100.00 | Using where |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)
123456
```

type为all，全表扫描，索引失效。

```mysql-sql
explain select * from staffs where name = 'July' and age - 1 = 22;
1
```

打印

```mysql-sql
 id | select_type | table  | partitions | type | possible_keys         | key                   | key_len | ref   | rows | filtered | Extra                 |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-----------------------+
|  1 | SIMPLE      | staffs | NULL       | ref  | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 74      | const |    1 |   100.00 | Using index condition |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-----------------------+
1 row in set, 1 warning (0.01 sec)
12345
```

key_len为74，只用到了1个索引，说明age索引失效，要想使用索引，可以在等号右边进行计算。

```mysql-sql
create index idx_add_yime on staffs(add_time);
explain select * from staffs where date(add_time) = '2019-12-16';
12
```

打印

```mysql-sql
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | staffs | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    3 |   100.00 | Using where |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)                                                                                       
123456
```

即使用函数会使索引失效。

#### 范围之后全失效：

```mysql-sql
explain select * from staffs where name = 'july' and age > 25 and pos = 'dev';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+-------+-----------------------+-----------------------+---------+------+------+----------+-----------------------+
| id | select_type | table  | partitions | type  | possible_keys         | key                   | key_len | ref  | rows | filtered | Extra                 |
+----+-------------+--------+------------+-------+-----------------------+-----------------------+---------+------+------+----------+-----------------------+
|  1 | SIMPLE      | staffs | NULL       | range | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 78      | NULL |    1 |    33.33 | Using index condition |
+----+-------------+--------+------------+-------+-----------------------+-----------------------+---------+------+------+----------+-----------------------+
1 row in set, 1 warning (0.01 sec)
123456
```

有2个索引字段有效，即name、age有效，age为范围，所以之后的pos失效。

#### like百分写最右：

%、_写到字符串最右边，type为range。

```mysql-sql
explain select * from staffs where name like '%july%';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+ 
| id | select_type | table  | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       | 
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+ 
|  1 | SIMPLE      | staffs | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    3 |    33.33 | Using where | 
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+ 
1 row in set, 1 warning (0.01 sec)                                                                                        
123456
```

模糊查询（字段值首位都有%），为全表查询，未使用索引。

```mysql-sql
explain select * from staffs where name like 'july%';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+-------+-----------------------+-----------------------+---------+------+------+----------+-----------------------+
| id | select_type | table  | partitions | type  | possible_keys         | key                   | key_len | ref  | rows | filtered | Extra                 |
+----+-------------+--------+------------+-------+-----------------------+-----------------------+---------+------+------+----------+-----------------------+
|  1 | SIMPLE      | staffs | NULL       | range | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 74      | NULL |    1 |   100.00 | Using index condition |
+----+-------------+--------+------------+-------+-----------------------+-----------------------+---------+------+------+----------+-----------------------+
1 row in set, 1 warning (0.01 sec)
123456
```

字段值尾有%，能用到索引。

```mysql-sql
explain select * from staffs where name like '%july';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | staffs | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    3 |    33.33 | Using where |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)
123456
```

显然，%位于字段值首时，又变为全表扫描，索引失效。
_与%的效果相同。

```mysql-sql
create index idx_user_nameage on user(name,age);
explain select name from user where name like '%aa%';
12
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+------------------+---------+------+------+----------+--------------------------+
| id | select_type | table | partitions | type  | possible_keys | key              | key_len | ref  | rows | filtered | Extra                    |
+----+-------------+-------+------------+-------+---------------+------------------+---------+------+------+----------+--------------------------+
|  1 | SIMPLE      | user  | NULL       | index | NULL          | idx_user_nameage | 68      | NULL |    4 |    25.00 | Using where; Using index |
+----+-------------+-------+------------+-------+---------------+------------------+---------+------+------+----------+--------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

select后的字段都必须为含有索引的字段的组合，才能使用索引，用全局索引或搜索引擎解决问题。

```mysql-sql
explain select email from user where name like '%aa%';
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | user  | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    4 |    25.00 | Using where |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)
123456
```

email字段不含有索引，所以为全表扫描，不能使用索引。

#### 覆盖索引不写星：

select不能用*，一般根据需要用什么字段select就接什么字段。

```mysql-sql
explain select name from staffs where name = 'july' and age = 23 and pos = 'dev';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------------+------+----------+-------------+ 
| id | select_type | table  | partitions | type | possible_keys         | key                   | key_len | ref               | rows | filtered | Extra       | 
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------------+------+----------+-------------+ 
|  1 | SIMPLE      | staffs | NULL       | ref  | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 140     | const,const,const |    1 |   100.00 | Using index | 
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------------------+------+----------+-------------+ 
1 row in set, 1 warning (0.01 sec)                                                                                                                              
123456
```

#### 不等空值还有or,索引失效要少用：

```mysql-sql
explain select * from staffs where name != 'july';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys         | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | staffs | NULL       | ALL  | idx_staffs_nameAgePos | NULL | NULL    | NULL |    3 |    66.67 | Using where |
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)                                                                                                                            
123456
```

即为全表扫描，且未使用索引。
可以用不等号（>、<）解决这个问题。

```mysql-sql
explain select * from staffs where name is null;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra            |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------+
|  1 | SIMPLE      | NULL  | NULL       | NULL | NULL          | NULL | NULL    | NULL | NULL |     NULL | Impossible WHERE |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------+
1 row in set, 1 warning (0.01 sec)                                                                                                                        
123456
```

因为建表时标记name为非空，所以这里为Impossible WHERE。

```mysql-sql
explain select * from staffs where name is not null;
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys         | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | staffs | NULL       | ALL  | idx_staffs_nameAgePos | NULL | NULL    | NULL |    3 |    66.67 | Using where |
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)                                                                                                                       
123456
```

也为全表扫描。

```mysql-sql
explain select * from staffs where name = 'july' or name = 'tom';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys         | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | staffs | NULL       | ALL  | idx_staffs_nameAgePos | NULL | NULL    | NULL |    2 |   100.00 | Using where |
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)                                                                                                                         
123456
```

#### varchar引号不可丢,SQL高级也不难：

```mysql-sql
explain select * from staffs where name = '2000';
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-------+
| id | select_type | table  | partitions | type | possible_keys         | key                   | key_len | ref   | rows | filtered | Extra |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | staffs | NULL       | ref  | idx_staffs_nameAgePos | idx_staffs_nameAgePos | 74      | const |    1 |   100.00 | NULL  |
+----+-------------+--------+------------+------+-----------------------+-----------------------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.01 sec)                                                                                                                       
123456
```

没有引号时

```mysql-sql
explain select * from staffs where name = 2000;
1
```

打印

```mysql-sql
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys         | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | staffs | NULL       | ALL  | idx_staffs_nameAgePos | NULL | NULL    | NULL |    2 |    50.00 | Using where |
+----+-------------+--------+------------+------+-----------------------+------+---------+------+------+----------+-------------+
1 row in set, 3 warnings (0.00 sec)                                                                                                                          
123456
```

即没有引号时MySQL也能隐式转换为char型，可以正常运行实现功能、查询到数据，但是索引会失效，成为全表扫描，降低查询效率。
可得：虽然对varchar可以进行隐式类型转化，但是会导致索引失效。

### 5.练习：

假设index(a,b,c)，如下图
![索引练习](https://img-blog.csdnimg.cn/20191218142606589.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 五、排序优化

### 1.分析

- 观察,至少跑一天,看看生产的慢SQL情况，问题复现；
- 开启慢查询日志,设置阙值,比如超过5秒钟的就是慢SQL,并抓取出来；
- explain + 慢SQL分析；
- show profile查询SQL在MySQL服务器里面的执行细节；
- 进行SQL数据库服务器的参数调优(运维 or DBA来做)。

### 2.永远小表驱动大表

小表驱动大表，即小的数据集驱动大的数据集。
类比循环

```python
for i in range(5):
    for j in range(1000):
        pass

for i in range(1000):
    for j in range(5):
        pass
1234567
```

如果小的循环在外层，对于数据库连接来说就只连接5次，进行5000次操作，如果1000在外，则需要进行1000次数据库连接，从而浪费资源，增加消耗。这就是为什么要小表驱动大表。
可参考https://blog.csdn.net/akunshouyoudou/article/details/90518955。

# Python全栈（三）数据库优化之11.MySQL高级-排序优化、慢查询日志、批量插入数据和Show Profile

## 一.排序优化

### 1.定义

即order by优化。
order by子句,尽量使用**index方式**排序,避免使用filesort方式排序。

### 2.建表，插入测试数据：

```mysql-sql
create table tbla(
    age int,
    birth timestamp not null
);

insert into tbla(age,birth) values(22,now());
insert into tbla(age,birth) values(23,now());
insert into tbla(age,birth) values(24,now());
12345678
```

打印

```mysql-sql
Query OK, 0 rows affected (0.02 sec)

Query OK, 1 row affected (0.01 sec)

Query OK, 1 row affected (0.00 sec)

Query OK, 1 row affected (0.00 sec)
1234567
```

### 3.建立索引：

```mysql-sql
create index idx_tbla_agebrith on tbla(age,birth);
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

### 4.分析：

```mysql-sql
explain select * from tbla where age > 30 order by age;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+--------------------------+
| id | select_type | table | partitions | type  | possible_keys     | key               | key_len | ref  | rows | filtered | Extra                    |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+--------------------------+
|  1 | SIMPLE      | tbla  | NULL       | range | idx_tbla_agebrith | idx_tbla_agebrith | 5       | NULL |    1 |   100.00 | Using where; Using index |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+--------------------------+
1 row in set, 1 warning (0.01 sec)
123456
```

未出现Using filesort。

```mysql-sql
explain select * from tbla where age > 30 order by age,birth;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+--------------------------+
| id | select_type | table | partitions | type  | possible_keys     | key               | key_len | ref  | rows | filtered | Extra                    |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+--------------------------+
|  1 | SIMPLE      | tbla  | NULL       | range | idx_tbla_agebrith | idx_tbla_agebrith | 5       | NULL |    1 |   100.00 | Using where; Using index |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+--------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

仍然未出现Using filesort。

```mysql-sql
explain select * from tbla where age > 30 order by birth;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
| id | select_type | table | partitions | type  | possible_keys     | key               | key_len | ref  | rows | filtered | Extra                                    |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
|  1 | SIMPLE      | tbla  | NULL       | range | idx_tbla_agebrith | idx_tbla_agebrith | 5       | NULL |    1 |   100.00 | Using where; Using index; Using filesort |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

此时出现了Using filesort。

```mysql-sql
explain select * from tbla where age > 30 order by birth,age;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
| id | select_type | table | partitions | type  | possible_keys     | key               | key_len | ref  | rows | filtered | Extra                                    |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
|  1 | SIMPLE      | tbla  | NULL       | range | idx_tbla_agebrith | idx_tbla_agebrith | 5       | NULL |    1 |   100.00 | Using where; Using index; Using filesort |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
1 row in set, 1 warning (0.01 sec)
123456
```

也出现了Using filesort。
可以得出，order by也遵循最佳左前缀法则。
改变查询条件：

```mysql-sql
explain select * from tbla where birth > '2019-01-01 00:00:00' order by birth;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+------------------------------------------+
| id | select_type | table | partitions | type  | possible_keys | key               | key_len | ref  | rows | filtered | Extra                                    |
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+------------------------------------------+
|  1 | SIMPLE      | tbla  | NULL       | index | NULL          | idx_tbla_agebrith | 9       | NULL |    3 |    33.33 | Using where; Using index; Using filesort |
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+------------------------------------------+
1 row in set, 1 warning (0.01 sec)
123456
```

也出现了文件内排序，即是否出现文件内排序与where条件的字段无关，与order by后边的字段有关，order by关心的是后边接的用于排序的字段，而不是where条件句中的字段。

```mysql-sql
explain select * from tbla where birth > '2019-01-01 00:00:00' order by age desc,birth asc;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+------------------------------------------+
| id | select_type | table | partitions | type  | possible_keys | key               | key_len | ref  | rows | filtered | Extra                                    |
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+------------------------------------------+
|  1 | SIMPLE      | tbla  | NULL       | index | NULL          | idx_tbla_agebrith | 9       | NULL |    3 |    33.33 | Using where; Using index; Using filesort |
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+------------------------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

可得出，order by后边有多个字段时，如果排序方式不一致时也会出现文件内排序。

```mysql-sql
explain select * from tbla where birth > '2019-01-01 00:00:00' order by age desc,birth desc;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+--------------------------+
| id | select_type | table | partitions | type  | possible_keys | key               | key_len | ref  | rows | filtered | Extra                    |
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+--------------------------+
|  1 | SIMPLE      | tbla  | NULL       | index | NULL          | idx_tbla_agebrith | 9       | NULL |    3 |    33.33 | Using where; Using index |
+----+-------------+-------+------------+-------+---------------+-------------------+---------+------+------+----------+--------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

显然，order by后边有多个字段时，如果排序方式一致时不会出现文件内排序。

#### 分析总结：

MySQL支持两种方式的排序,filesort和index。
index效率高,MySQL扫描索引本身完成排序；filesort方式效率较低。
order by在两种情况下,会使用index方式排序：

- order by语句使用索引最左前列；
- 使用where子句与order by子句条件组合满足索引最左前列。
  如

```mysql-sql
explain select * from tbla where age > 30 order by birth;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
| id | select_type | table | partitions | type  | possible_keys     | key               | key_len | ref  | rows | filtered | Extra                                    |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
|  1 | SIMPLE      | tbla  | NULL       | range | idx_tbla_agebrith | idx_tbla_agebrith | 5       | NULL |    1 |   100.00 | Using where; Using index; Using filesort |
+----+-------------+-------+------------+-------+-------------------+-------------------+---------+------+------+----------+------------------------------------------+
1 row in set, 1 warning (0.01 sec)
123456
```

会出现文件内排序。

```mysql-sql
explain select * from tbla where age = 30 order by birth;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+-------------------+-------------------+---------+-------+------+----------+--------------------------+
| id | select_type | table | partitions | type | possible_keys     | key               | key_len | ref   | rows | filtered | Extra                    |
+----+-------------+-------+------------+------+-------------------+-------------------+---------+-------+------+----------+--------------------------+
|  1 | SIMPLE      | tbla  | NULL       | ref  | idx_tbla_agebrith | idx_tbla_agebrith | 5       | const |    1 |   100.00 | Using where; Using index |
+----+-------------+-------+------------+------+-------------------+-------------------+---------+-------+------+----------+--------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

去掉范围后，age和birth可以组成最佳左前缀，无文件内排序。

### 5.filesort算法：

filesort有两种算法：双路排序和单路排序。

#### 双路排序：

MySQL4.1之前是使用双路排序,字面意思就是**两次扫描磁盘**,最终得到数据,读取行指针和order by列,对他们进行排序,然后扫描已经排序好的列表,按照列表中的值重新从列表中读取对应的数据输出。

#### 单路排序：

单路排序,从磁盘读取查询需要的所有列,按照order by列在buffer对他们进行排序,然后扫描排序后的列表进行输出,它的效率更快一些,避免了第二次读取数据,并且把**随机IO变成了顺序IO**,但是它会使用更多的空间。

在数据量超过缓存区大小sort_buffer_size的时候，可能出现单路排序性能低于双路排序的情况，需要进行优化：

#### 优化策略：

调整MySQL参数：

- 增加sort_buffer_size参数设置；
- 增大max_lenght_for_sort_data参数的设置。

### 6.提高order by的速度：

- order by时select * 是一个大忌,只写需要的字段：
  当查询的字段大小总和小于max_length_for_sort_data而且排序字段不是text|blob类型时,会用改进后的算法–单路排序；
  两种算法的数据都有可能超出sort_buffer的容量,超出之后,会创建tmp文件进行合并排序,导致多次I/O。
- 尝试提高sort_buffer_size。
- 尝试提高max_length_for_sort_data。

group by的用法、分析、优化与order by类似。

### 7.练习

索引`a_b_c(a,b,c)`：

| SQL语句                                    | 是否会出现filesort |
| ------------------------------------------ | ------------------ |
| order by a,b                               | 不会               |
| order by a,b,c                             | 不会               |
| order by a desc,b desc,c desc              | 不会               |
| where a = const order by b,c               | 不会               |
| where a = const and b = const order by c   | 不会               |
| where a = const and b > const order by b,c | 不会               |
| order by a asc,b desc,c desc               | 会                 |
| where g = const order by b,c               | 会                 |
| where a = const order by c                 | 会                 |
| where a = const order by a,d               | 会                 |

## 二、慢查询日志

### 1.定义

MySQL的慢查询日志MySQL提供的一种日志记录,它用来记录在MySQL中**响应时间超过阙值**的语句。
具体指运行时间超过long_query_time值的SQL,则会被记录到慢查询日志中。
如long_query_time的默认值为10,意思是运行10秒以上（>10秒，即不包括10秒）的语句。
由它来查看哪些SQL超出了我们的最大忍耐时间值,比如一条SQL执行超过5秒钟,我们就算是慢SQL,希望能收集超过5秒的SQL,结合之前explain进行全面分析。

### 2.使用

默认情况下,MySQL数据库没有开启慢查询日志,需要我们手动来设置这个参数；
如果不是调优需要的话,一般不建议启动该参数,因为开启慢查询日志会或多或少带来一定的**性能影响**，影响效率。
慢查询日志支持将日志记录写入文件。
查看慢查询日志状态：

```mysql-sql
show variables like '%slow_query_log%';
1
```

打印

```mysql-sql
+---------------------+----------------------------------------------------------------------------+
| Variable_name       | Value                                                                      |
+---------------------+----------------------------------------------------------------------------+
| slow_query_log      | OFF                                                                        |
| slow_query_log_file | E:\MySQL\phpstudy_pro\Extensions\MySQL5.7.26\data\xxx.log |
+---------------------+----------------------------------------------------------------------------+
2 rows in set, 1 warning (0.01 sec)
1234567
```

slow_query_log_file是slow_query_log的保存路径，可以在配置文件中修改。
slow_query_log默认关闭，可通过下面命令开启：

```mysql-sql
set global slow_query_log = 1;
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
1
```

再次查询

```mysql-sql
show variables like '%slow_query_log%';
1
```

打印

```mysql-sql
+---------------------+----------------------------------------------------------------------------+
| Variable_name       | Value                                                                      |
+---------------------+----------------------------------------------------------------------------+
| slow_query_log      | ON                                                                         |
| slow_query_log_file | E:\MySQL\phpstudy_pro\Extensions\MySQL5.7.26\data\xxx.log |
+---------------------+----------------------------------------------------------------------------+
2 rows in set, 1 warning (0.00 sec)
1234567
```

以上在命令行中的修改都是临时的，重启MySQL会恢复默认，要想永久修改，要在配置文件my.ini中修改，并重启服务才能生效。
查看响应时间阙值：

```mysql-sql
show variables like '%long_query_time%';
1
```

打印

```mysql-sql
+-----------------+-----------+
| Variable_name   | Value     |
+-----------------+-----------+
| long_query_time | 10.000000 |
+-----------------+-----------+
1 row in set, 1 warning (0.01 sec)
123456
```

可知，时间阙值默认为10秒。
重新设置阙值

```mysql-sql
set global long_query_time = 3;
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.00 sec)
1
```

此时再次查看阙值

```mysql-sql
show variables like '%long_query_time%';
1
```

打印

```mysql-sql
+-----------------+----------+
| Variable_name   | Value    |
+-----------------+----------+
| long_query_time | 3.000000 |
+-----------------+----------+
1 row in set, 1 warning (0.01 sec)
123456
```

如果未改变，可以新打开一个命令行，连接数据库即可看到变为3秒。
此时进行测试

```mysql-sql
select sleep(4);
1
```

打印

```mysql-sql
+----------+
| sleep(4) |
+----------+
|        0 |
+----------+
1 row in set (4.00 sec)
123456
```

sleep()函数可以实现阻塞功能，阻塞给定的时间，可以实现用于测试慢查询的时间。
此时可查看慢查询日志文件，可看到之前执行的慢SQL以及被记录到日志：

```mysql-sql
E:\MySQL\phpstudy_pro\COM\..\Extensions\MySQL5.7.26\\bin\mysqld.exe, Version: 5.7.26 (MySQL Community Server (GPL)). started with:
TCP Port: 3306, Named Pipe: MySQL
Time                 Id Command    Argument
# Time: 2019-12-19T11:22:04.811013Z
# User@Host: root[root] @ localhost [::1]  Id:    13
# Query_time: 4.000478  Lock_time: 0.000000 Rows_sent: 1  Rows_examined: 0
use demo;
SET timestamp=1576754524;
select sleep(4);
123456789
```

一般情况下不应该开启慢查询日志，在测试环境下可以使用，在生产环境下可以使用。

### 3.慢查询日志工具

mysqldumpslow，一般是在Linux系统下使用。

| 符号 | 意义                     |
| ---- | ------------------------ |
| s    | 表示按照何种方式排序     |
| c    | 访问次数                 |
| l    | 锁定时间                 |
| r    | 返回记录                 |
| t    | 查询时间                 |
| al   | 平均锁定时间             |
| ar   | 平均返回记录数           |
| t    | 即为返回前面多少条的数据 |

例如，
得到返回记录集最多的10个SQL：

```mysql-sql
mysqldumpslow -s r -t 10 D:/phpStudy/PHPTutorial/MySQL/slow_log.txt;
1
```

得到访问次数最多的10个SQL：

```mysql-sql
mysqldumpslow -s c -t 10 D:/phpStudy/PHPTutorial/MySQL/slow_log.txt;
1
```

查询有多少条慢SQL语句：

```mysql-sql
show global status like '%slow_queries%';
1
```

打印

```mysql-sql
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| Slow_queries  | 3     |
+---------------+-------+
1 row in set (0.01 sec)
123456
```

易知，此时有3条慢SQL语句。

## 三、批量插入数据

### 1.创建表：

```mysql-sql
--部门表
create table dept(
            id int primary key auto_increment,
            deptno mediumint not null,
            dname varchar(20) not null,
            loc varchar(13) not null
)engine=innodb default charset=gbk;
--员工表
create table emp(
            id int primary key auto_increment,
            empno mediumint not null,   
            ename varchar(20) not null, 
            job varchar(9) not null, 
            mgr mediumint not null,  
            hiredate DATE not null,  
            sal decimal(7,2) not null, 
            comm decimal(7,2) not  null, 
            deptno mediumint not null   
)engine=innodb default charset=gbk;
12345678910111213141516171819
```

打印

```mysql-sql
Query OK, 0 rows affected (0.03 sec)

Query OK, 0 rows affected (0.02 sec)
123
```

### 2.创建函数



——定界符、分隔符。DELIMITER——定界符、分隔符。DELIMITER

：开始；
END $$：结束。
随机产生字符串：



```mysql-sql
--开始符
DELIMITER $$
--创建函数
CREATE FUNCTION rand_string(n INT) RETURNS VARCHAR(255)
BEGIN
--声明变量
DECLARE chars_str VARCHAR(100) DEFAULT 'abcdefghijklmnopqrstuvwxyzABCDEFJHIJKLMNOPQRSTUVWXYZ';
DECLARE return_str VARCHAR(255) DEFAULT '';
DECLARE i INT DEFAULT 0;
--开始循环
WHILE i < n DO
--contact连接字符串
SET return_str = CONCAT(return_str,SUBSTRING(chars_str,FLOOR(1+RAND()*52),1));
SET i = i + 1;
--结束循环
END WHILE;
RETURN return_str;
--结束符
END $$
12345678910111213141516171819
```

随机产生部门编号：

```mysql-sql
DELIMITER $$
CREATE FUNCTION rand_num() RETURNS INT(5)
BEGIN
DECLARE i INT DEFAULT 0;
SET i = FLOOR(100+RAND()*10);
RETURN i;
END $$
1234567
```

创建函数，如果报错：`this function has none of DETERMINISTIC...` 时设置参数

```mysql-sql
set global log_bin_trust_function_creators=1;
1
```

### 3.创建存储过程

存储过程用于批量处理数据。
为emp创建存储过程、插入数据：

```mysql-sql
DELIMITER $$
CREATE PROCEDURE insert_emp(IN START INT(10),IN max_num INT(10))
BEGIN
DECLARE i INT DEFAULT 0;
--关闭自动提交，最后一起提交
SET autocommit = 0;
REPEAT
SET i = i + 1;
--调用函数插入
INSERT INTO emp(empno,ename,job,mgr,hiredate,sal,comm,deptno) VALUES ((START+i),rand_string(6),'SALESMAN',0001,CURDATE(),2000,400,rand_num());
UNTIL i = max_num
END REPEAT;
--一起提交
COMMIT;
END $$
123456789101112131415
```

为dept创建存储过程、插入数据：

```mysql-sql
DELIMITER $$
CREATE PROCEDURE insert_dept(IN START INT(10),IN max_num INT(10))
BEGIN
DECLARE i INT DEFAULT 0;
SET autocommit = 0;
REPEAT
SET i = i + 1;
INSERT INTO dept(deptno,dname,loc) VALUES ((START + i),rand_string(10),rand_string(8));
UNTIL i = max_num
END REPEAT;
COMMIT;
END $$
123456789101112
```

建议在可视化界面里执行函数和存储过程，因为可以多行同时输入并同时执行，不会产生语法错误；
如果在命令行里执行，要逐行输入，不可一次性复制粘贴，会出现语法错误，并且在`DELIMITER $$`语句后MYSQL的结束符变为"$$"，要再次执行`DELIMITER ;`才会使结束符变为";"，执行一般的SQL语句。

### 4.调用存储过程

调用存储过程实现批量插入数据：

```mysql-sql
delimiter ; 
call insert_dept(100,10);
    
call insert_emp(100001,500000);
1234
```

打印

```mysql-sql
Query OK, 0 rows affected (0.02 sec)

Query OK, 0 rows affected (55.29 sec)
123
```

## 四、Show Profile进行SQL分析

### 1.定义

Show Profile是MySQL提供可以用来分析当前会话中语句执行的资源消耗情况,可以用于SQL的调优的测量。
默认情况下,参数处于关闭状态,并保存最近15次的运行结果。

### 2.Show Profile分析步骤：

- 是否支持,看看当前MySQL版本是否支持；
- 开启功能,默认是关闭,使用前需要开启。

版本 > 5.5均支持Show Profile。
查看状态：

```mysql-sql
show variables like 'profiling';
1
```

打印

```mysql-sql
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| profiling     | OFF   |
+---------------+-------+
1 row in set, 1 warning (0.01 sec)
123456
```

改变状态

```mysql-sql
--set profiling = on;
set profiling = 1;
12
```

打印

```mysql-sql
Query OK, 0 rows affected, 1 warning (0.00 sec)
1
```

再次查看状态：

```mysql-sql
show variables like 'profiling';
1
```

打印

```mysql-sql
+---------------+-------+         
| Variable_name | Value |         
+---------------+-------+         
| profiling     | ON    |         
+---------------+-------+         
1 row in set, 1 warning (0.00 sec)
123456
```

此时已经打开。
进行几次查询：

```mysql-sql
select * from students group by age limit 3; 
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
3 rows in set (0.01 sec)
12345678
select * from students order by age limit 3; 
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height |
+----+-------+------+--------+--------+------------+-----------+--------+
| 25 | John  |   16 | secret |      1 | 1999-05-09 |         1 | 164.00 |
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 |
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 |
+----+-------+------+--------+--------+------------+-----------+--------+
3 rows in set (0.00 sec)
12345678
select * from students; 
1
```

打印

```mysql-sql
+----+-------+------+--------+--------+------------+-----------+--------+ 
| id | NAME  | age  | gender | cls_id | birth      | is_delete | height | 
+----+-------+------+--------+--------+------------+-----------+--------+ 
|  1 | Tom   |   18 | male   |      1 | 1999-09-09 |         1 | 165.00 | 
|  2 | Jerry |   19 | male   |      3 | 1999-10-09 |         1 | 157.00 | 
|  3 | Nancy |   17 | male   |      2 | 1999-08-09 |         1 | 165.00 | 
|  4 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 168.00 | 
|  6 | John  |   16 | secret |      3 | 1999-05-09 |         1 | 165.00 | 
|  7 | John  |   19 | secret |      1 | 1999-07-09 |         1 | 176.00 | 
|  8 | John  |   19 | secret |      2 | 1999-07-09 |         1 | 165.00 | 
|  9 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 153.00 | 
| 10 | John  |   19 | secret |      3 | 1999-07-09 |         1 | 165.00 | 
| 11 | Rose  |   19 | female |      1 | 1999-07-09 |         1 | 169.00 | 
... 
+----+-------+------+--------+--------+------------+-----------+--------+ 
27 rows in set (0.00 sec)                                                 
12345678910111213141516
select * from emp group by id%10 limit 15000; 
1
```

打印

```mysql-sql
+----+--------+--------+----------+-----+------------+---------+--------+--------+
| id | empno  | ename  | job      | mgr | hiredate   | sal     | comm   | deptno |
+----+--------+--------+----------+-----+------------+---------+--------+--------+
| 10 | 100011 | fIaaad | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    102 |
|  1 | 100002 | APwPJb | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    100 |
|  2 | 100003 | uKqxof | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    106 |
|  3 | 100004 | fxYAIL | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    106 |
|  4 | 100005 | jUWYeC | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    104 |
|  5 | 100006 | wUkqYU | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    105 |
|  6 | 100007 | hZDRyS | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    108 |
|  7 | 100008 | SJBNmU | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    107 |
|  8 | 100009 | ccemXd | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    104 |
|  9 | 100010 | nQrmha | SALESMAN |   1 | 2019-12-18 | 2000.00 | 400.00 |    106 |
+----+--------+--------+----------+-----+------------+---------+--------+--------+
10 rows in set (1.64 sec)                                                         
123456789101112131415
```

查看各个SQL语句的执行时间：

```mysql-sql
show profiles;
1
```

打印

```mysql-sql
+----------+------------+----------------------------------------------+
| Query_ID | Duration   | Query                                        |
+----------+------------+----------------------------------------------+
|        1 | 0.00027575 | select * from students group by age limit 3  |
|        2 | 0.00029700 | select * from students order by age limit 3  |
|        3 | 0.00021350 | select * from students                       |
|        4 | 1.65845325 | select * from emp group by id%10 limit 15000 |
+----------+------------+----------------------------------------------+
4 rows in set, 1 warning (0.00 sec)
123456789
```

查看单个SQL语句的执行情况

```mysql-sql
show profile cpu,block io for query 1;
1
```

打印

```mysql-sql
+----------------------+----------+----------+------------+--------------+---------------+
| Status               | Duration | CPU_user | CPU_system | Block_ops_in | Block_ops_out |
+----------------------+----------+----------+------------+--------------+---------------+
| starting             | 0.000057 | 0.000000 |   0.000000 |         NULL |          NULL |
| checking permissions | 0.000009 | 0.000000 |   0.000000 |         NULL |          NULL |
| Opening tables       | 0.000017 | 0.000000 |   0.000000 |         NULL |          NULL |
| init                 | 0.000048 | 0.000000 |   0.000000 |         NULL |          NULL |
| System lock          | 0.000005 | 0.000000 |   0.000000 |         NULL |          NULL |
| optimizing           | 0.000002 | 0.000000 |   0.000000 |         NULL |          NULL |
| optimizing           | 0.000002 | 0.000000 |   0.000000 |         NULL |          NULL |
| statistics           | 0.000013 | 0.000000 |   0.000000 |         NULL |          NULL |
| preparing            | 0.000010 | 0.000000 |   0.000000 |         NULL |          NULL |
| statistics           | 0.000020 | 0.000000 |   0.000000 |         NULL |          NULL |
| preparing            | 0.000009 | 0.000000 |   0.000000 |         NULL |          NULL |
| executing            | 0.000012 | 0.000000 |   0.000000 |         NULL |          NULL |
| Sending data         | 0.000005 | 0.000000 |   0.000000 |         NULL |          NULL |
| executing            | 0.000002 | 0.000000 |   0.000000 |         NULL |          NULL |
| Sending data         | 0.001247 | 0.000000 |   0.000000 |         NULL |          NULL |
| end                  | 0.000003 | 0.000000 |   0.000000 |         NULL |          NULL |
| query end            | 0.000006 | 0.000000 |   0.000000 |         NULL |          NULL |
| closing tables       | 0.000001 | 0.000000 |   0.000000 |         NULL |          NULL |
| removing tmp table   | 0.000156 | 0.000000 |   0.000000 |         NULL |          NULL |
| closing tables       | 0.000006 | 0.000000 |   0.000000 |         NULL |          NULL |
| freeing items        | 0.000038 | 0.000000 |   0.000000 |         NULL |          NULL |
| cleaning up          | 0.000015 | 0.000000 |   0.000000 |         NULL |          NULL |
+----------------------+----------+----------+------------+--------------+---------------+
22 rows in set, 1 warning (0.00 sec)
123456789101112131415161718192021222324252627
```

可以看出，在主要操作之外，删除临时表removing tmp table用时相对较长。
进行验证：

```mysql-sql
explain select * from students group by age limit 3;
1
```

打印

```mysql-sql
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+---------------------------------+
| id | select_type | table    | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                           |
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+---------------------------------+
|  1 | SIMPLE      | students | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   27 |   100.00 | Using temporary; Using filesort |
+----+-------------+----------+------------+------+---------------+------+---------+------+------+----------+---------------------------------+
1 row in set, 1 warning (0.01 sec)
123456
```

出现Using temporary，确实用到了临时表，先创建临时表，将查询到的数据存到临时表，再从临时表查询数据，再删除临时表，所以会耗费很多时间。
再查看一个SQL语句

```mysql-sql
show profile cpu,block io for query 4;
1
```

打印

```mysql-sql
+----------------------+----------+----------+------------+--------------+---------------+
| Status               | Duration | CPU_user | CPU_system | Block_ops_in | Block_ops_out |
+----------------------+----------+----------+------------+--------------+---------------+
| starting             | 0.000067 | 0.000000 |   0.000000 |         NULL |          NULL |
| checking permissions | 0.000005 | 0.000000 |   0.000000 |         NULL |          NULL |
| Opening tables       | 0.002077 | 0.000000 |   0.000000 |         NULL |          NULL |
| init                 | 0.000030 | 0.000000 |   0.000000 |         NULL |          NULL |
| System lock          | 0.000011 | 0.000000 |   0.000000 |         NULL |          NULL |
| optimizing           | 0.000003 | 0.000000 |   0.000000 |         NULL |          NULL |
| statistics           | 0.000022 | 0.000000 |   0.000000 |         NULL |          NULL |
| preparing            | 0.000009 | 0.000000 |   0.000000 |         NULL |          NULL |
| Creating tmp table   | 0.000032 | 0.000000 |   0.000000 |         NULL |          NULL |
| Sorting result       | 0.000003 | 0.000000 |   0.000000 |         NULL |          NULL |
| executing            | 0.000001 | 0.000000 |   0.000000 |         NULL |          NULL |
| Sending data         | 1.659538 | 1.515625 |   0.062500 |         NULL |          NULL |
| Creating sort index  | 0.000060 | 0.000000 |   0.000000 |         NULL |          NULL |
| end                  | 0.000004 | 0.000000 |   0.000000 |         NULL |          NULL |
| query end            | 0.000007 | 0.000000 |   0.000000 |         NULL |          NULL |
| removing tmp table   | 0.000008 | 0.000000 |   0.000000 |         NULL |          NULL |
| query end            | 0.000003 | 0.000000 |   0.000000 |         NULL |          NULL |
| closing tables       | 0.000077 | 0.000000 |   0.000000 |         NULL |          NULL |
| freeing items        | 0.000061 | 0.000000 |   0.000000 |         NULL |          NULL |
| cleaning up          | 0.000015 | 0.000000 |   0.000000 |         NULL |          NULL |
+----------------------+----------+----------+------------+--------------+---------------+
20 rows in set, 1 warning (0.00 sec)                                                      
12345678910111213141516171819202122232425
```

出现了Sorting result，说明存在文件内排序。
进行验证

```mysql-sql
explain select * from emp group by id%10 limit 15000;
1
```

打印

```mysql-sql
+----+-------------+-------+------------+------+---------------+------+---------+------+---------+----------+---------------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows    | filtered | Extra                           |
+----+-------------+-------+------------+------+---------------+------+---------+------+---------+----------+---------------------------------+
|  1 | SIMPLE      | emp   | NULL       | ALL  | PRIMARY       | NULL | NULL    | NULL | 1395176 |   100.00 | Using temporary; Using filesort |
+----+-------------+-------+------------+------+---------------+------+---------+------+---------+----------+---------------------------------+
1 row in set, 1 warning (0.00 sec)
123456
```

出现了Using filesort，说明确实存在文件内排序，需要进行优化。
**type参数：**

| 参数        | 意义                       |
| ----------- | -------------------------- |
| all         | 显示所有的开销信息         |
| block io    | 显示块IO相关开销           |
| cpu         | 显示CPU相关开销信息        |
| ipc         | 显示发送和接收相关开销信息 |
| memory      | 显示内存相关开销信息       |
| page faults | 显示页面错误相关开销信息   |

**参数注意**：

| 参数                         | 意义                                  |
| ---------------------------- | ------------------------------------- |
| converting HEAP to MyISAM    | 查询结果太大,内存都不够用了往磁盘上搬 |
| Creating tmp table           | 创建临时表                            |
| Copying to tmp table on disk | 把内存中临时表复制到磁盘,危险         |
| locked                       |                                       |

### 3.全局查询日志

把每一条SQL语句记录下来，量大，无必要。
开启命令：

```mysql-sql
set global general_log = 1;
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.01 sec)
1
```

将SQL语句写到表中：

```mysql-sql
set global log_output = 'TABLE';
1
```

打印

```mysql-sql
Query OK, 0 rows affected (0.00 sec)
1
```

所编写的SQL语句,会记录到MySQL库里的genral_log表。
执行一条SQL语句，如

```mysql-sql
select * from students;
1
```

再查看全局查询日志：

```mysql-sql
select * from mysql.general_log;
1
```

打印

```mysql-sql
+----------------------------+------------------------------+-----------+-----------+--------------+---------------------------------------------------------------+
| event_time                 | user_host                    | thread_id | server_id | command_type | argument                                                      |
+----------------------------+------------------------------+-----------+-----------+--------------+---------------------------------------------------------------+
| 2019-12-18 21:47:51.376592 | root[root] @ localhost [::1] |        10 |         1 | Query        | use `mysql`                                                   |
| 2019-12-18 21:47:52.914806 | root[root] @ localhost [::1] |        10 |         1 | Query        | show full tables from `mysql` where table_type = 'BASE TABLE' |
| 2019-12-18 21:48:02.936032 | root[root] @ localhost [::1] |        10 |         1 | Query        | describe `mysql`.`general_log`                                |
| 2019-12-18 21:48:02.937085 | root[root] @ localhost [::1] |        10 |         1 | Query        | show index from `mysql`.`general_log`                         |
| 2019-12-18 21:48:05.235971 | root[root] @ localhost [::1] |        10 |         1 | Query        | select * from `mysql`.`general_log` limit 0, 1000             |
...
| 2019-12-18 22:07:08.593162 | root[root] @ localhost [::1] |        19 |         1 | Query        | show profile                                                  |
| 2019-12-19 20:38:20.901277 | root[root] @ localhost [::1] |        24 |         1 | Query        | select * from demo                                            |
| 2019-12-19 20:38:37.884983 | root[root] @ localhost [::1] |        24 |         1 | Query        | select * from studens                                         |
| 2019-12-19 20:38:43.057866 | root[root] @ localhost [::1] |        24 |         1 | Query        | select * from students                                        |
| 2019-12-19 20:39:55.765410 | root[root] @ localhost [::1] |        24 |         1 | Query        | select * from mysql.general_log                               |
+----------------------------+------------------------------+-----------+-----------+--------------+---------------------------------------------------------------+
28 rows in set (0.00 sec)
```

# Python全栈（三）数据库优化之12.MySQL高级-数据库锁和数据库分区

## 一、数据库锁

锁是计算机协调多个进程或线程并发访问某一资源的机制。
根据对数据操作的粒度，分为表锁、行锁、间隙锁。

### 1.表锁

更偏读。
偏向MyISAM存储引擎,开销小,加锁快;
无死锁,因为整个表都被锁了，别人不能访问；
锁定粒度大,发送锁冲突的概率最高,并发度低。
表锁举例：
创建表并插入数据：

```sql
create table mylock(
    id int not null primary key auto_increment,
    name varchar(20)
)engine myisam;

insert into mylock(name) values('a');
insert into mylock(name) values('b');
insert into mylock(name) values('c');
insert into mylock(name) values('d');
insert into mylock(name) values('e');
12345678910
```

打印

```sql
Query OK, 0 rows affected (0.01 sec)

Query OK, 1 row affected (0.01 sec)

Query OK, 1 row affected (0.00 sec)

Query OK, 1 row affected (0.00 sec)

Query OK, 1 row affected (0.00 sec)

Query OK, 1 row affected (0.00 sec)
1234567891011
```

手动增加表锁：
语法：
`lock table 表名字 read(write),表名字2 read(write);`

```sql
lock table mylock read,book write;
1
```

打印

```sql
Query OK, 0 rows affected (0.01 sec)
1
```

查看所有数据库的所有表，同时可以查看创建的锁：

```sql
show open tables;
1
```

打印

```sql
+--------------------+------------------------------------------------------+--------+-------------+
| Database           | Table                                                | In_use | Name_locked |
+--------------------+------------------------------------------------------+--------+-------------+
| performance_schema | events_waits_summary_by_user_by_event_name           |      0 |           0 |
| performance_schema | events_waits_summary_global_by_event_name            |      0 |           0 |
| performance_schema | events_transactions_summary_global_by_event_name     |      0 |           0 |
| performance_schema | replication_connection_status                        |      0 |           0 |
| mysql              | time_zone_leap_second                                |      0 |           0 |
| mysql              | columns_priv                                         |      0 |           0 |
| performance_schema | metadata_locks                                       |      0 |           0 |
| performance_schema | status_by_user                                       |      0 |           0 |
| demo               | mylock                                               |      1 |           0 |
| mysql              | time_zone_transition_type                            |      0 |           0 |
| performance_schema | events_statements_current                            |      0 |           0 |
| performance_schema | prepared_statements_instances                        |      0 |           0 |
| performance_schema | events_statements_history_long                       |      0 |           0 |
| mysql              | db                                                   |      0 |           0 |
| performance_schema | file_instances                                       |      0 |           0 |
| performance_schema | events_stages_summary_by_user_by_event_name          |      0 |           0 |
| performance_schema | memory_summary_by_thread_by_event_name               |      0 |           0 |
...
| performance_schema | events_stages_history                                |      0 |           0 |
| mysql              | time_zone                                            |      0 |           0 |
| mysql              | slave_master_info                                    |      0 |           0 |
| mysql              | proxies_priv                                         |      0 |           0 |
| performance_schema | events_statements_summary_by_host_by_event_name      |      0 |           0 |
| demo               | book                                                 |      1 |           0 |
| performance_schema | socket_instances                                     |      0 |           0 |
| performance_schema | host_cache                                           |      0 |           0 |
| performance_schema | status_by_account                                    |      0 |           0 |
| performance_schema | events_statements_summary_by_account_by_event_name   |      0 |           0 |
| performance_schema | memory_summary_by_account_by_event_name              |      0 |           0 |
...
+--------------------+------------------------------------------------------+--------+-------------+
110 rows in set (0.00 sec)
1234567891011121314151617181920212223242526272829303132333435
```

显然，mylock表和book表的In_use值为1.
此时在同一命令行中读取

```sql
select * from mylock;
1
```

打印

```sql
+----+------+           
| id | name |           
+----+------+           
|  1 | a    |           
|  2 | b    |           
|  3 | c    |           
|  4 | d    |           
|  5 | e    |           
+----+------+           
5 rows in set (0.00 sec)
12345678910
```

此时再打开一个命令行，执行相同的命令：

```sql
select * from mylock;
1
```

打印

```sql
+----+------+           
| id | name |           
+----+------+           
|  1 | a    |           
|  2 | b    |           
|  3 | c    |           
|  4 | d    |           
|  5 | e    |           
+----+------+           
5 rows in set (0.00 sec)
12345678910
```

在命令行一中更新数据

```sql
update mylock set name = 'a2' where id = 1;
1
```

打印

```sql
ERROR 1099 (HY000): Table 'mylock' was locked with a READ lock and can't be updated
1
```

即读锁限制了对表的更新。
在命令行二中执行相同命令

```sql
update mylock set name = 'a2' where id = 1;
1
```

会发生堵塞，直到命令行一中解锁

```sql
unlock tables;
1
```

打印

```sql
Query OK, 0 rows affected (0.00 sec)
1
```

命令行二中打印

```sql
Query OK, 0 rows affected (57.79 sec)
Rows matched: 1  Changed: 0  Warnings: 0
12
```

即堵塞了57.79秒。
在命令行一中锁定mylock表之后，再在命令行二中查询其他表能正常查询，但是在命令行一中不能再查询其他表，否则会报错，要想读其他表，必须先对mylock解锁。
并且在命令行一关闭后，对mylock的锁自动关闭，其他命令行可对mylock表进行操作。

```sql
select * from book;
1
```

打印

```sql
ERROR 1100 (HY000): Table 'book' was not locked with LOCK TABLES
1
```

在命令行一中加写锁

```sql
lock table mylock write;
1
```

打印

```sql
Query OK, 0 rows affected (0.00 sec)
1
```

查询数据

```sql
select * from mylock;
1
```

打印

```sql
+----+------+           
| id | name |           
+----+------+           
|  1 | a2   |           
|  2 | b    |           
|  3 | c    |           
|  4 | d    |           
|  5 | e    |           
+----+------+           
5 rows in set (0.00 sec)
12345678910
```

能正常查询数据。
在命令行二中执行相同命令：

```sql
select * from mylock;
1
```

发生堵塞。
在命令行二查询其他表能正常查询到。
在命令行一中更新数据

```sql
update mylock set name = 'a4' where id = 1;
1
```

打印

```sql
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0
12
```

在命令行二中执行相同操作

```sql
update mylock set name = 'a4' where id = 1;
1
```

也会发生堵塞，直到命令行一释放写锁。
释放表锁：
语法：
`unlock tables;`
表锁总结：
MyISAM在执行查询语句(select)前,会自动给涉及的所有表加读锁,在执行增删改操作前,会自动给涉及的表加写锁：

- 对MyISAM表的读操作(加读锁),不会阻塞其他进程对同一表的读请求,但会阻塞对同一表的写请求。只有当读锁释放后,才会执行其他进行的写操作。即读锁会堵塞写，但是不会堵塞读。
- 对MyISAM表的写操作(加写锁),会阻塞其他进程对同一表的读和写操作,只有当写锁释放后,才会执行其他进程的读写操作。即写锁会堵塞写和读。

```sql
show status like 'table%';
1
```

打印

```sql
+----------------------------+-------+ 
| Variable_name              | Value | 
+----------------------------+-------+ 
| Table_locks_immediate      | 112   | 
| Table_locks_waited         | 0     | 
| Table_open_cache_hits      | 0     | 
| Table_open_cache_misses    | 0     | 
| Table_open_cache_overflows | 0     | 
+----------------------------+-------+ 
5 rows in set (0.01 sec)               
12345678910
```

Table_locks_immediate表示产生表级锁的次数，Table_locks_waited表示表级锁发生冲突时等待的次数，这两个值较高，存在较高的表级锁竞争。

### 2.行锁

更偏写。
偏向InnoDB存储引擎,开销大,加锁慢,会出现死锁；
锁定粒度最小,发生锁冲突的概率最低,并发度也最高。
InnoDB与MyISAM的最大不同点,是支持事务,采用了行级锁。
行锁举例：
创建表并插入数据

```sql
create table test_innodb_lock(a int(11),b varchar(16))engine=innodb;

insert into test_innodb_lock values(1,'b2');
insert into test_innodb_lock values(3,'3');
insert into test_innodb_lock values(4,'4000');
insert into test_innodb_lock values(5,'5000');

create index idx_test_innodb_a on test_innodb_lock(a);

create index idx_test_innodb_b on test_innodb_lock(b);
12345678910
```

打印

```sql
Query OK, 0 rows affected (0.02 sec)  
                                      
Query OK, 1 row affected (0.01 sec)   
                                      
Query OK, 1 row affected (0.00 sec)   
                                      
Query OK, 1 row affected (0.00 sec)   
                                      
Query OK, 1 row affected (0.00 sec)   
                                      
Query OK, 0 rows affected (0.02 sec)  
Records: 0  Duplicates: 0  Warnings: 0
                                      
Query OK, 0 rows affected (0.02 sec)  
Records: 0  Duplicates: 0  Warnings: 0              
123456789101112131415
```

两个命令行关闭自动提交：

```sql
set autocommit = 0;
1
```

打印

```sql
Query OK, 0 rows affected (0.00 sec)            
1
```

在命令行一中更新数据：

```sql
update test_innodb_lock set b = '4001' where a = 4;
1
```

打印

```sql
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0          
12
```

在命令行一中查询，

```sql
select * from test_innodb_lock;
1
```

打印

```sql
+------+------+
| a    | b    |
+------+------+
|    1 | b2   |
|    3 | 1024 |
|    4 | 4001 |
|    5 | 1024 |
+------+------+
4 rows in set (0.00 sec)
123456789
```

数据已经改变
此时在命令行二中查询，

```sql
select * from test_innodb_lock;
1
```

打印

```sql
+------+------+         
| a    | b    |         
+------+------+         
|    1 | b2   |         
|    3 | 1024 |         
|    4 | 1024 |         
|    5 | 1024 |         
+------+------+         
4 rows in set (0.01 sec) 
123456789
```

会发现未改变，需要在两个命令行中commit，然后再在命令行二中查询，

```sql
select * from test_innodb_lock;
1
```

打印

```sql
+------+------+         
| a    | b    |         
+------+------+         
|    1 | b2   |         
|    3 | 1024 |         
|    4 | 4001 |         
|    5 | 1024 |         
+------+------+         
4 rows in set (0.00 sec)
123456789
```

在命令行一中执行

```sql
update test_innodb_lock set b = '4002' where a = 4;
1
```

打印

```sql
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0          
12
```

此时在命令行二中执行

```sql
update test_innodb_lock set b = '4003' where a = 4;
1
```

会发生堵塞
在命令行一中提交后，命令行二打印

```sql
Query OK, 1 row affected (1 min 17.33 sec)
Rows matched: 1  Changed: 1  Warnings: 0
12
```

此时两个命令行分别查询，值分别为4002、4003，出现不一致的情况，再在两个命令行同时提交后，再次查询，数据一致，均为4003。
两个命令行中修改不同行的数据，不会发生冲突。
varchar不加引号：

- 索引失效，全表扫描；
- 由行锁变为表锁，可能阻塞。

在命令行一中执行

```sql
update test_innodb_lock set a = 5 where b = 4000;
1
```

打印

```sql
Query OK, 0 rows affected (0.00 sec)
Rows matched: 1  Changed: 0  Warnings: 0        
12
```

再在命令行二中执行

```sql
update test_innodb_lock set b = '4003' where a = 4;
1
```

会发生堵塞。

#### 如何分析行锁定：

通过检查innodb_row_lock状态变量来分析系统上的行锁争夺情况。

```sql
show status like 'innodb_row_lock%';
1
```

打印

```sql
+-------------------------------+-------+
| Variable_name                 | Value |
+-------------------------------+-------+
| Innodb_row_lock_current_waits | 0     |
| Innodb_row_lock_time          | 77325 |
| Innodb_row_lock_time_avg      | 77325 |
| Innodb_row_lock_time_max      | 77325 |
| Innodb_row_lock_waits         | 1     |
+-------------------------------+-------+
5 rows in set (0.00 sec)      
12345678910
```

### 各个状态量的说明

| 状态量                        | 说明                                                   |
| ----------------------------- | ------------------------------------------------------ |
| Innodb_row_lock_current_waits | 当前正在等待锁定的数量                                 |
| Innodb_row_lock_time          | 从系统启动到现在锁定的总时间长度，值越大，锁定时间越长 |
| Innodb_row_lock_time_avg      | 每次等待所花费平均时间                                 |
| Innodb_row_lock_time_max      | 从系统启动到现在等待最长的一次所花费的时间             |
| Innodb_row_lock_waits         | 系统启动后到现在总共等待的次数                         |

### 3.间隙锁

在命令行一中：

```sql
update test_innodb_lock set b = '1024' where a > 1 and a < 6;
1
```

打印

```sql
Query OK, 1 row affected (0.00 sec)
Rows matched: 3  Changed: 1  Warnings: 0      
12
```

在命令行二中

```sql
insert into test_innodb_lock values(2,'2000');
1
```

发生堵塞，在命令行一中commit才能插入成功。
此即间隙锁。
当我们用范围条件而不是相等条件检索数据,并请求共享或排他锁时,innodb会给符合条件的已有数据记录的索引项加锁, 对于键值在条件范围内但并不存在的记录,叫做"间隙"；
innodb也会对这个"间隙"加锁,这种锁机制就是所谓的间隙锁。
间隙锁的危害：

- 因为SQL执行过程中通过范围查找的话,他会锁定整个范围内所有的索引值,即使这个键值并不存在；
- 间隙锁有一个比较致命的弱点,就是当锁定以为范围键值之后,即使某些不存在的键值也会被无辜的锁定,而造成在锁定的时候无法插入锁定键值范围内的任何数据。在某些场景下这可能会对性能造成很大的危害。

**如何锁定一行**：
`select * from test_innodb_lock where a = 8 for update;`

## 二、MySQL分区表

### 1.定义

分区表的特点：
在逻辑上为一个表,在物理上存储在多个文件中。
查看MySQL是否支持分区：

```sql
show plugins;
1
```

打印

```sql
+----------------------------+----------+--------------------+---------+---------+
| Name                       | Status   | Type               | Library | License |
+----------------------------+----------+--------------------+---------+---------+
| binlog                     | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| mysql_native_password      | ACTIVE   | AUTHENTICATION     | NULL    | GPL     |
| sha256_password            | ACTIVE   | AUTHENTICATION     | NULL    | GPL     |
| CSV                        | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| MEMORY                     | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| InnoDB                     | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| INNODB_TRX                 | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_LOCKS               | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
| INNODB_LOCK_WAITS          | ACTIVE   | INFORMATION SCHEMA | NULL    | GPL     |
...
| MyISAM                     | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| MRG_MYISAM                 | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| PERFORMANCE_SCHEMA         | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| ARCHIVE                    | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| BLACKHOLE                  | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| FEDERATED                  | DISABLED | STORAGE ENGINE     | NULL    | GPL     |
| partition                  | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| ngram                      | ACTIVE   | FTPARSER           | NULL    | GPL     |
+----------------------------+----------+--------------------+---------+---------+
44 rows in set (0.01 sec)                                                               
1234567891011121314151617181920212223
```

创建表

```sql
create table `login_log`(
    login_id int(10) unsigned not null comment '登录用户id',
    login_time timestamp not null default current_timestamp,
    login_ip int(10) unsigned not null comment '登录类型'
)engine=innodb default charset=utf8 partition by hash(login_id) partitions 4;
12345
```

打印

```sql
Query OK, 0 rows affected (0.06 sec)      
1
```

分为4个区，此时在数据文件中有4个文件login_log#p#p0.ibd、login_log#p#p1.ibd、login_log#p#p2.ibd、login_log#p#p3.ibd。
分区键：
分区引入了分区键的概念,分区键用于根据某个区间值、特定值、或者HASH函数值执行数据的聚集,让数据根据规则分布在不同的分区中。
分区类型：

- RANGE分区
- LIST分区
- HASH分区

无论哪种分区类型,要么分区表上没有主键/唯一键,要么分区表的主键/唯一键都必须包括分区键,也就是说不能使用主键/唯一字段之外的其他字段分区。

#### 分区表的原理

分区的主要目的是将数据按照一个较粗的粒度分在不同的表中， 这样可以将相关的数据存放在一起，而且如果想一次性删除整个分区的数据也很方便；
对用户而言,分区表是一个独立的逻辑表，但是底层MySQL将其分成多个物理子表，这对用户来说是透明的，每一个 分区表都会使用一个独立的表文件；
创建表时使用partition by子句定义每个分区存放的数据，执行查询时,优化器会根据分区定义过滤那些没有我们需要数据的分区,这样查询只需要查询所需要数据在的分区即可。

### 2.RANGE分区：

RANGE分区特点：

- 根据分区键值的范围把数据行存储到表的不同分区中；
- 多个分区的范围要连续,但是不能重叠；
- 分区不包括上限,取不到上限值。
  建立RANGE分区：

```sql
create table `login_log_range`(
    login_id int(10) unsigned not null comment '登录用户ID',
    login_time timestamp not null default CURRENT_TIMESTAMP,
    login_ip int(10) unsigned not null comment '登录ip'
)engine=innodb 
partition by range(login_id)(
partition p0 values less than(10000),   #实际范围0-9999
partition p1 values less than(20000),   #实际范围10000-19999
partition p2 values less than(30000),   #实际范围20000-29999
partition p3 values less than maxvalue  #存储大于30000的数据
);
1234567891011
```

打印

```sql
Query OK, 0 rows affected (0.04 sec)     
1
```

此时插入一条login_id大于30000的数据：

```sql
insert into login_log_range values(30001,now(),1)
1
```

打印

```sql
Query OK, 1 row affected (0.01 sec)      
1
```

分析SQL语句，查看分区

```sql
explain select * from login_log_range where login_id = 30001;
1
```

打印

```sql
+----+-------------+-----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table           | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | login_log_range | p3         | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | Using where |
+----+-------------+-----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)     
123456
```

显示位于p3分区。
查看id为500的记录对应的分区：

```sql
explain select * from login_log_range where login_id = 500;
1
```

打印

```sql
+----+-------------+-----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table           | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | login_log_range | p0         | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | Using where |
+----+-------------+-----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)     
123456
```

显示位于p0分区。
查看所有分区：

```sql
explain select * from login_log_range;
1
```

打印

```sql
+----+-------------+-----------------+-------------+------+---------------+------+---------+------+------+----------+-------+ 
| id | select_type | table           | partitions  | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra | 
+----+-------------+-----------------+-------------+------+---------------+------+---------+------+------+----------+-------+ 
|  1 | SIMPLE      | login_log_range | p0,p1,p2,p3 | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | NULL  | 
+----+-------------+-----------------+-------------+------+---------------+------+---------+------+------+----------+-------+ 
1 row in set, 1 warning (0.01 sec)                                                                                               
123456
```

RANGE分区使用场景：

- 分区键为日期或是时间类型；
- 经常运行包含分区键的查询,MySQL可以很快的确定只有某一个或某些分区需要扫描,例如检索商品login_id小于10000的记录数,MySQL只需要扫描p0分区即可；
- 定期按分区范围清理历史数据。

删除分区：

```sql
alter table login_log_range drop partition p0;
1
```

打印

```sql
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0  
12
```

### 3.HASH分区

HASH分区的特点：

- 根据MOD(分区键,分区值)的值把数据行存储到表的不同分区内；
- 数据可以平均地分布在各个分区中；
- HASH分区的键值必须是一个INT类型的值,或是通过函数可以转为INT类型。

建立HASH分区表：

```sql
create table `login_log_hash`(
    login_id int(10) unsigned not null comment '登录用户ID',
    login_time timestamp not null default CURRENT_TIMESTAMP,
    login_ip int(10) unsigned not null comment '登录ip'
)engine=innodb default charset=utf8 partition by hash(login_id) partitions 4;
12345
```

或者

```sql
create table `login_log_hash`(
    login_id int(10) unsigned not null comment '登录用户ID',
    login_time timestamp not null default CURRENT_TIMESTAMP,
    login_ip int(10) unsigned not null comment '登录ip'
)engine=innodb default charset=utf8 partition by hash(UNIX_TIMESTAMP(login_time)) partitions 4;
12345
```

打印

```sql
Query OK, 0 rows affected (0.04 sec)
1
```

插入数据

```sql
insert into login_log_hash values(1,now(),1);
insert into login_log_hash values(2,now(),2);
insert into login_log_hash values(3,now(),3);
insert into login_log_hash values(4,now(),4);
insert into login_log_hash values(5,now(),5);
12345
```

打印

```sql
Query OK, 1 row affected (0.00 sec)
                                   
Query OK, 1 row affected (0.00 sec)
                                   
Query OK, 1 row affected (0.00 sec)
                                   
Query OK, 1 row affected (0.00 sec)
                                   
Query OK, 1 row affected (0.00 sec)
123456789
```

查看分区

```sql
explain select * from login_log_hash where login_id = 1;
explain select * from login_log_hash where login_id = 2;
explain select * from login_log_hash where login_id = 3;
explain select * from login_log_hash where login_id = 4;
explain select * from login_log_hash where login_id = 5;
12345
```

打印

```sql
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table          | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | login_log_hash | p1         | ALL  | NULL          | NULL | NULL    | NULL |    2 |    50.00 | Using where |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)                                                                                               
                                                                                                                                 
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table          | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | login_log_hash | p2         | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | Using where |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)                                                                                               
                                                                                                                                 
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table          | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | login_log_hash | p3         | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | Using where |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)                                                                                               
                                                                                                                                 
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table          | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | login_log_hash | p0         | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | Using where |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)                                                                                               
                                                                                                                                 
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table          | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | login_log_hash | p1         | ALL  | NULL          | NULL | NULL    | NULL |    2 |    50.00 | Using where |
+----+-------------+----------------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)                                                                                                                                                                                      
12345678910111213141516171819202122232425262728293031323334
```

显然，每条数据位于对应的分区。
查看所有分区：

```sql
explain select * from login_log_hash;
1
```

打印

```sql
+----+-------------+----------------+-------------+------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table          | partitions  | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+----------------+-------------+------+---------------+------+---------+------+------+----------+-------+
|  1 | SIMPLE      | login_log_hash | p0,p1,p2,p3 | ALL  | NULL          | NULL | NULL    | NULL |    4 |   100.00 | NULL  |
+----+-------------+----------------+-------------+------+---------------+------+---------+------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
123456
```

hash分区为全表扫描。

### 4.LIST分区

LIST分区特点：

- 按分区键取值的列表进行分区；
- 同范围分区一样,各分区的列表值不能重复；
- 每一行数据必须能找到对应的分区列表,否则数据插入失败。
  建立LIST分区

```sql
create table `login_log_list`(
    login_id int(10) unsigned not null comment '登录用户ID',
    login_time timestamp not null default CURRENT_TIMESTAMP,
    login_ip int(10) unsigned not null comment '登录ip',
    login_type int(10) not null
)engine=innodb 
partition by list(login_type)(
partition p0 values in(1,3,5,7,9),    
partition p1 values in(2,4,6,8)   
);
12345678910
```

打印

```sql
Query OK, 0 rows affected (0.03 sec)
1
```

插入值

```sql
insert into login_log_list values(1,now(),2,1);
1
```

打印

```sql
Query OK, 1 row affected (0.00 sec)
1
```

可以正常插入。
插入一条login_type列表中没有的数据

```sql
insert into login_log_list values(1,now(),2,10);
1
```

打印

```sql
ERROR 1526 (HY000): Table has no partition for value 10
1
```

会报错，即没有10对应的分区。

## 三、练习–如何选择合适的分区方式

业务场景：

- 用户每次登录都会记录到日志表中；
- 用户登录日志保存一年,一年后可以删除。

```sql
create table login_lg_log(
    login_id int(10) not null,
    login_time datetime not null default current_timestamp,
    login_ip int(10) not null
)engine = INNODB
partition by range(year(login_time))(
    partition p0 values less than(2015),
    partition p1 values less than(2016),
    partition p2 values less than(2017),
    partition p3 values less than(2018)
);
1234567891011
```

打印

```sql
Query OK, 0 rows affected (0.03 sec)
1
```

插入数据

```sql
insert into login_lg_log values
(1,'2015-01-01',1),
(2,'2015-01-02',1),
(3,'2016-01-01',1),
(4,'2016-01-01',1),
(5,'2017-01-01',1),
(6,'2017-01-01',1),
(7,'2017-01-01',1)
;
123456789
```

打印

```sql
Query OK, 7 rows affected (0.00 sec)
Records: 7  Duplicates: 0  Warnings: 0
12
```

查看分区

```sql
select table_name,partition_name,partition_description,table_rows from information_schema.partitions where table_name = 'login_lg_log';
1
```

打印

```sql
+--------------+----------------+-----------------------+------------+
| table_name   | partition_name | partition_description | table_rows |
+--------------+----------------+-----------------------+------------+
| login_lg_log | p0             | 2015                  |          0 |
| login_lg_log | p1             | 2016                  |          2 |
| login_lg_log | p2             | 2017                  |          2 |
| login_lg_log | p3             | 2018                  |          3 |
+--------------+----------------+-----------------------+------------+
4 rows in set (0.00 sec)
123456789
```

添加分区

```sql
alter table login_lg_log add partition(partition p4 values less than(2019));
1
```

打印

```sql
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

删除分区

```sql
alter table login_lg_log drop partition p0;
1
```

打印

```sql
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
12
```

### 数据归档：

新建一个和要归档的表的结构一样的表：

```sql
create table login_lg_log_test(
    login_id int(10) not null,
    login_time datetime not null default current_timestamp,
    login_ip int(10) not null
)engine = INNODB;
12345
```

打印

```sql
Query OK, 0 rows affected (0.01 sec)
1
```

分区迁移：

```sql
alter table login_lg_log exchange partition p1 with table login_lg_log_test;
1
```

打印

```sql
Query OK, 0 rows affected (0.01 sec)
1
```

查看两个表：

```sql
select * from login_lg_log;
1
```

打印

```sql
+----------+---------------------+----------+
| login_id | login_time          | login_ip |
+----------+---------------------+----------+
|        3 | 2016-01-01 00:00:00 |        1 |
|        4 | 2016-01-01 00:00:00 |        1 |
|        5 | 2017-01-01 00:00:00 |        1 |
|        6 | 2017-01-01 00:00:00 |        1 |
|        7 | 2017-01-01 00:00:00 |        1 |
+----------+---------------------+----------+
5 rows in set (0.00 sec)                     
12345678910
```

即原表中p1分区的数据被移除。

```sql
select * from login_lg_log_test;
1
```

打印

```sql
+----------+---------------------+----------+
| login_id | login_time          | login_ip |
+----------+---------------------+----------+
|        1 | 2015-01-01 00:00:00 |        1 |
|        2 | 2015-01-02 00:00:00 |        1 |
+----------+---------------------+----------+
2 rows in set (0.00 sec)
1234567
```

新表中即为原表p1分区的数据。
使用分区表的注意事项：

- 结合业务场景选择分区键,避免跨分区查询；
- 对分区表进行查询最好在where从句中包含分区键；
- 具有主键或唯一索引的表,主键或唯一索引必须是分区键的一部分。

### 分区适用场景

- 表非常大，无法全部存在内存,或者只在表的最后有热点数据,其他都是历史数据；
- 分区表的数据更容易维护，可以对独立的分区进行独立的操作；
- 分区表的数据可以分布在不同的机器上,从而高效使用资源；
- 可以使用分区表来避免某些特殊的瓶颈；
- 可以备份和恢复独立的分区。

# Python全栈（三）数据库优化之13.MySQL高级-主从复制和数据库总结

## 一、主从复制

### 1.目的和解决的问题：

目的：读写分离，一个数据库只负责读，一个只负责写。
解决的问题：

- 数据分布:随意停止或开始复制，并在不同地理位置分布数据备份；
- 负载均衡:降低单个服务器的压力；
- 故障切换:帮助应用程序避免单点失败；
- 升级测试:可以使用更高版本的MySQL作为从库。

### 2.基本原理：

如图
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcua2FuY2xvdWQuY24vN2IvMjEvN2IyMWJhMzBmODdlZWFlYTE1ZGI1OTZhZmEzMDhkNjNfNTgzeDQyMS5wbmc#pic_center?x-oss-process=image/format,png)

### 3.复制的三步骤：

（1）master将改变记录到二进制日志，这些记录过程叫做二进制日志事件binary log events；
（2）slave将master的binary log events拷贝到它的中继日志；
（3）slave重做中继日志中的事件,将改变应用到自己的数据库中。
MySQL复制是**异步**的且**串行**的。

### 4.复制的基本原则：

- 每个slave只有一个master；
- 每个slave只能有一个唯一的服务器ID；
- 每个master可以有多个salve。

### 5.一主一从常见配置：

- MySQL版本一致且后台服务可以运行；
- 主从主机可以相互通信；
- 主从配置都在[mysqld]结点下,都是小写。

主机（Windows系统）配置my.ini：

```
server_id=2 #[必须]主服务器唯一ID，不能与从机重复
log-bin=自己本地的路径/mysqlbin（例如mysql-bin） #[必须]启用二进制日志
log-err = 自己本地的路径/mysqlerr #[可选]启用错误日志
123
```

从机（Linux Ubuntu系统）配置mysqld.cnf(/etc/mysql/mysql.conf.d/mysqld.cnf)：

```
bind-address=0.0.0.0 #让任何IP地址连接
server_id=1 #[必须]主服务器唯一ID
log-bin=自己本地的路径/mysqlbin（例如/var/log/mysql/mysql-bin.log） #[可选]启用二进制日志
123
```

修改过配置文件之后，要重启MySQL服务：

```shell
service mysql restart
1
```

关闭Linux防火墙：

```shell
service iptables stop
1
```

关闭Windows防火墙：
关闭**专用网络**防火墙。
在Windows主机上建立账户并授权slave：

```sql
create user 'slave'@'从机数据库IP' identified with mysql_native_password by 'password';
#例如create user 'zhangsan'@'192.168.1.1' identified with mysql_native_password by '123456';
12
```

授权并查看状态：

```sql
grant replication slave on *.* to 'slave'@'从机数据库IP' identified by 'password';
#例如grant replication slave on *.* to 'zhangsan'@'192.168.1.1' identified by '123456';

show master status;
1234
```

记录下File和position的值。

### 6.配置Linux从机

```
change master to master_host = '192.168.0.161',
master_user = 'jerry',
master_password = '123456',
master_log_file = 'binlog.000004',
master_log_pos= 908;
12345
```

### 7.测试是否配置成功

```
start slave;  #启动从服务器复制功能

show slave status;
123
```

下面两个参数都是yes,则说明主从配置成功：
`slave_io_running:yes`；
`slave_sql_running:yes`。

主从复制总结：
当读数据和写数据都是对同一个数据库进行操作时，可能会遇到性能瓶颈，所以新增一个与原数据库相同的数据库作为从数据库，即备份，实现**读写分离**（主数据库写，从数据库读），既能提高读写效率，又能提高安全性，但是会产生一定延迟。

## 二、MySQL操作规范

### 1.命名规范

- 表名建议使用有业务意义的英文词汇，必要时可加数字和下划线，并以英文字母开头。
- 库、表、字段全部采用小写：
  MySQL在Linux下默认是区分大小写的，而在Windows下不区分大小写。因此，防止出现问题，建议都设置为小写。
- 避免用 MySQL 的保留字。
- 命名（包括表名、列名）禁止超过 30 个字符。
- 临时库、表名必须以tmp为前缀，并以日期为后缀，如：tmp_shop_info_20200120。
- 备份库、表必须以bak为前缀，并以日期为后缀，如：bak_shop_info_20200120。
- 索引命名：
  非唯一索引必须按照"**idx_字段名称**"或"**idx_表名_字段名称**"进行命名；
  唯一索引必须按照"**uniq_字段名称**"或"**uniq_表名_字段名称**"进行命名。

### 2.设计规范

- 主键：
  表必须有主键；
  不使用更新频繁的列做主键；
  尽量不选择字符串列做主键；
  不使用 UUID（不重复的字符串）、MD5 HASH 做主键；
  默认使用非空的唯一键。
- 如无特殊要求，建议都使用 InnoDB 引擎。
- 默认使用 utf8mb4 字符集，数据排序规则使用 utf8mb4_general_ci：
  utf8mb4 为万国码，无乱码风险；与 utf8 编码相比，utf8mb4 能支持 Emoji 表情。
- 所有表、字段都需要增加 comment 来描述此表、字段所表示的含义：
  如`data_status TINYINT NOT NULL DEFAULT '1' COMMENT '1代表记录有效，0代表记录无效'`。
- 尽可能不使用 TEXT、BLOB 类型：
  原因：会浪费更多的磁盘和内存空间，非必要的大量大字段查询会淘汰掉热数据，导致内存命中率急剧降低，影响数据库性能。如果实在有某个字段过长需要使用TEXT、BLOB类型，则建议独立出来一张表，用主键来对应，避免影响原表的查询效率。字段用多大就取多大，避免资源浪费和性能下降。
- 单表列数目建议小于30。

### 3.SQL语句规范

- 避免隐式转换：
  varchar要加引号。
- 尽量不使用`select *`,只select需要的字段：
  读取不需要的列会增加CPU、IO、NET消耗，并且不能有效的利用覆盖索引。使用`SELECT *`容易在增加或者删除字段后导致程序报错。
- 建议将子查询转换为关联查询。
- 建议应用程序捕获SQL异常，并有相应处理：
  可以避免数据库攻击。

### 4.行为规范

- 批量导入、导出数据必须提前通知DBA协助观察；
- 不在业务高峰期批量更新、查询数据库；
- 删除表或者库要求尽量先重命名rename、备份，观察几天，确定对业务没影响，再drop

## 三、数据库基础总结

### 1.数据类型

- 整数：tinyint、smallint、mediumint、int、bigint；
- 实数：float、double；
- 字符串：varchar、char、text、blob；
- 日期时间：timestamp、datetime。

### 2.列属性

- 自增auto_increment；
- 默认值default；
- 非空not null；
- 零填充zerofill；
- 无符号unsigned。

### 3.数据库操作

连接数据库：

```shell
mysql -uroot -p #更安全
或者
mysql -u root -proot
123
```

退出数据库：

```sql
exit
--或者quit
12
```

查看所有数据库：

```sql
show databases;
1
```

创建数据库：

```sql
#创建数据库
create database 数据库名字 charset = utf8;
#查看创建数据库的命令
show create database 数据库名字;
#使用数据库
use mydatabase;
123456
```

### 4.数据库表操作

查看当前数据库中所有表：

```sql
show tables;
1
```

创建表：

```sql
create table 数据表名字(字段 类型 约束[,字段 类型 约束]);
1
```

查看表：

```sql
desc demo1;
1
```

查看表的创建语句：

```sql
show create table salary;
1
```

### 5.DML数据库管理语言

新增：

```sql
#全列插入：
insert [into] 表名 values(...);
#部分插入：
insert into 表名(列1 ,...) values(值1 ,...);
1234
```

修改：

```sql
update 表名 SET 字段1=新值,字段2=新值[where 条件];
update 表名 SET 字段1=新值,字段2=新值;
12
```

删除：

```sql
delete from 表名 [where条件];
delete from 表名; #清空表中的数据
12
```

查询：
where子句：=、>、<
逻辑运算符：and、or、not
模糊查询：%、_
范围查询：in、between…and…
空判断：is null、is not null
聚合函数：count、max、min、sum、avg
分组：group by
排序：asc、desc
分页：limit

## 四、MySQL-视图、索引、事务、引擎总结

### 1.视图

（1）作用：
视图是**虚拟的表**，只包含动态检索数据的查询，不包含数据；
简化操作,隐藏细节，保护数据；
对视图的更新会作用于基表，一般不更新。

（2）视图操作
定义视图：

```sql
create view 视图名称 as select语句;
1
```

查看视图：

```sql
show tables;
1
```

使用视图：

```sql
select * from v_stu_score;
1
```

删除视图：

```sql
drop view 视图名称;
1
```

### 2.索引

（1）索引类型：
唯一索引：具有唯一性约束，不允许为空；
主键索引：特殊的唯一索引，不允许有空值；
单值索引：单列；
复合索引：将多个列组合在一起创建索引，可以覆盖多个列。
（2）索引操作：
创建索引：

```sql
create [unique] index 索引名称 on 表名(字段名称(长度));
1
```

查看索引：

```sql
show index from 表名;
1
```

删除索引：

```sql
drop index 索引名称 on 表名;
1
```

（3）索引对性能的影响：

- 大大减少服务器需要扫描的数据量；
- 帮助服务器避免排序和临时表；
- 将随机I/O变顺序I/O；
- 大大提高查询速度，降低写的速度，占用磁盘。

（4）索引的使用场景：

- 对于非常小的表，大部分情况下全表扫描效率更高；
- 中到大型表，索引非常有效；
- 特大型的表，建立和使用索引的代价将随之增长,可以使用分区技术来解决。

（5）唯一索引与主键索引的区别：

- 一个表只能有一个主键索引，可以有很多个唯一索引；
- 主键索引一定是唯一索引，唯-索引不是主键索引；
- 主键可以与外键构成参照完整性约束，防止数据不一致。

（6）不建立索引的情况：

- 频繁更新的字段不适合建立素引；
- where条件里面用不到的字段不创建索引；
- 表记录太少，当表中数据量超过三百万条数据，可以考虑建立素引；
- 数据重复且平均的表字段，比如性别、国籍。

### 3.事务

（1）ACID特性：

- A原子性
- C一致性
- I隔离性
- D持久性

（2）事务操作
开启事务：

```sql
begin;
start transaction;
12
```

提交事务：

```sql
commit;
1
```

回滚事务：

```sql
rollback;
1
```

### 4.存储引擎

（1）InnoDB：
支持事务，支持行级锁，支持外键。
（2）MyISAM：
不支持事务和表级锁，不支持外键。
（3）CSV：
以文本形式存储，不支持索引和自增。
（４）Memory：
存储在内存中，服务重启后数据丢失。
（５）选择：
根据事务、崩溃恢复、备份三个特性选择。

## 五、MySQL优化总结

### 1.分析慢SQL方法

（1）记录慢查询日志

- 开启慢查询日志`set global slow_ query_ log = 1;`
- 设置阙值，默认是10秒`set global long_ query_ time = 3;`
- 查看多少条慢SQL`show global status like "%slow_queries%';`

（2）使用explain
用法：`explain + sql`
（3）使用show profile

- `set profiling = 1;`
- `show profiles;`
- `show profile cpu,block io for query 临时表ID;`

### 2.SQL语句优化

- 选择正确的存储引擎；
- 优化字段的数据类型；
- 为搜索字段添加索引；
- 避免使用`select *`，从数据库里读出越多的数据，查询就会变得越慢；
- 尽可能使用**NOT NULL**。

### 3.SQL注释

| 符号  | 举例                                                    |
| ----- | ------------------------------------------------------- |
| #     | SELECT * FROM USER WHERE NAME = ‘1aa1’ #and age = 22    |
| –空格 | SELECT * FROM USER WHERE NAME = ‘1aa1’ – and age = 22   |
| /**/  | SELECT * FROM USER WHERE NAME = ‘1aa1’ /*and age = 22*/ |

### 4.分区

（1）原理
分区的主要目的是将数据按照一个较粗的粒度分在不同的表中, 这样可以将相关的数据存放在一起，而且如果想一次性删除整个分区的数据也很方便。
对用户而言,分区表是-一个独立的逻辑表，但是底层MySQL将其分成多个物理子表，这对用户来说是透明的，每一个 分区表都会使用一个独立的表文件。
（2）方式

- RANGE分区
- LIST分区
- HASH分区

（3）适用场景

- 表非常大，无法全部存在内存,或者只在表的最后有热点数据,其他都是历史数据；
- 分区表的数据更容易维护，可以对独立的分区进行独立的操作；
- 分区表的数据可以分布在不同的机器上,从而高效使用资源；
- 可以使用分区表来避免某些特殊的瓶颈；
- 可以备份和恢复独立的分区。

### 5.主从复制

（1）原理

- 在主库上把数据更改记录到二进制日志；
- 从库将主库的日志复制到自己的中继日志；
- 从库读取中继日志中的事件,将其重放到从库数据中。

（2）解决的问题

- 数据分布；
- 负载均衡；
- 故障切换；
- 升级测试。

### 6*.SQL查询的安全方案

- 使用预处理语句防止SQL注入；
- 写入数据库的数据要进行特殊字符的转义；
- 查询错误信息不要返回给用户，将错误记录到日志