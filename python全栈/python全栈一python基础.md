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



