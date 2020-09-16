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



### 4.遍历的分析

如果知道了树的先序、后序遍历结果中的一种，并且也知道中序遍历的结果，可以根据遍历结果进行反推：
（1）根据先序遍历的第一个元素或后序遍历的最后一个元素即可知道原树的根节点，再在中序遍历结果中从根节点断开，左边即为左子树，右边即为右子树；
（2）根据分出的左右子树将先序或后序除开根节点的部分分成两个部分，即为左子树、右子树，对每个子树再进行（1）中的操作，即递归，直到每个子树的大小为1为止。
使用这种方法的前提是必须知道**中序遍历**的结果，和前序、后序遍历结果中的一种。