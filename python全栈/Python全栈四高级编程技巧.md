# Python全栈（四）高级编程技巧之1.类与对象深度

## 一、鸭子类型和多态

多态的概念是应用于Java和C#这一类强类型语言中，而Python崇尚“**鸭子类型**”。
动态语言调用实例方法时不检查类型，只要方法存在，参数正确，就可以调用。这就是动态语言的“鸭子类型”，它并不要求严格的继承体系，一个对象只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子。

```python
a = [1,2]
b = [3,4]
c = (5,7)
d = {7,8}

a.extend(b)
print(a)
for item in c:
    print(item)
123456789
```

打印

```python
[1, 2, 3, 4]
5
7
123
```

`extend(self,iterable)`方法的源码是

```python
def extend(self, *args, **kwargs): # real signature unknown
    """ Extend list by appending elements from the iterable. """
    pass
123
```

并没有实现具体的代码，底层是用C语言封装的。
**iterable**：
可迭代的对象，可以用for。
**extend**方法的参数是可迭代对象，所以列表、元组、集合都能作为**extend**方法的参数。

```python
a = [1,2]
b = [3,4]
c = (5,7)
d = {7,8}

a.extend(c)
print(a)
b.extend(d)
print(b)
123456789
```

打印

```python
[1, 2, 5, 7]
[3, 4, 8, 7]
12
```

**多态**：
定义时的类型和运行时的类型不一样，此时就成为多态。

```python
class Cat(object):
    def say(self):
        print('I am a cat')


class Dog(object):
    def say(self):
        print('I am a dog')


class Duck(object):
    def say(self):
        print('I am a duck')


animal_list = [Cat, Dog, Duck]
for animal in animal_list:
    animal().say()
123456789101112131415161718
```

打印

```python
I am a cat
I am a dog
I am a duck
123
```

## 二、抽象基类(abc模块)

### 1.定义

抽象基类（abstract base class,ABC）：
抽象基类就是类里定义了纯虚成员函数的类。纯虚函数只提供了接口，并**没有具体实现**。
换句话说，抽象基类就是定义各种方法而不做具体实现的类，任何继承自抽象基类的类必须实现这些方法，否则无法实例化。
特点：

- 抽象基类不能被实例化(不能创建对象)，通常是作为基类供子类继承（不能被实例化）；
- 子类中重写虚函数，实现具体的接口（方法在子类中必须重写）。

### 2.应用场景

#### 判断某个对象的类型

例如：Sized判断某个类是否实现了`__len__()`方法。

```python
from collections.abc import Sized

class Demo(object):
    def __init__(self,name):
        self.name = name

    def __len__(self):
        #魔法方法，加入才能使用len()方法
        return len(self.name)

    def test(self):
        pass

d = Demo(['Tom','Jack'])
print(len(d))
#hasattr()方法用于判断对象是否具有某个方法
print(hasattr(d,'__len__'))
print(hasattr(d,'test'))
print(isinstance(d,Demo))
print(isinstance(d,Sized))
1234567891011121314151617181920
```

打印

```python
2
True
True
True
1234
```

#### 强制某个子类必须实现某些方法

强制子类必须实现某个方法：

```python
class CacheBase(object):
    def get(self, key):
        pass

    def set(self, key, value):
        pass


class RedisBase(CacheBase):
    pass


r = RedisBase()
r.get('Tom')
1234567891011121314
```

**方法一：父类方法中抛出异常**

```python
class CacheBase(object):
    def get(self,key):
        # pass
        raise ValueError

    def set(self,key,value):
        # pass
        raise NotImplementedError

class RedisBase(CacheBase):
    pass

r = RedisBase()
r.get('Tom')
1234567891011121314
```

运行会报错

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 89, in <module>
    r.get('Tom')
  File "xxx/demo.py", line 79, in get
    raise ValueError
ValueError
123456
```

重写父类中的方法：

```python
class CacheBase(object):
    def get(self,key):
        # pass
        raise ValueError

    def set(self,key,value):
        # pass
        raise NotImplementedError

class RedisBase(CacheBase):
    # pass
    #重写父类中的方法
    def get(self,key):
        pass

r = RedisBase()
r.get('Tom')
1234567891011121314151617
```

再次运行无报错。
**方法二：父类继承抽象基类，方法加装饰器**
父类继承抽象基类使子类必须继承父类指定的所有抽象方法。

```python
import abc

class CacheBase(metaclass=abc.ABCMeta):
    @abc.abstractmethod
    def get(self,key):
        pass
        # raise ValueError

    @abc.abstractmethod
    def set(self,key,value):
        pass
        # raise NotImplementedError

class RedisBase(CacheBase):
    #重写父类中的方法
    def get(self,key):
        pass

r = RedisBase()
r.get('Tom')
1234567891011121314151617181920
```

运行会报错

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 94, in <module>
    r = RedisBase()
TypeError: Can't instantiate abstract class RedisBase with abstract methods set
1234
```

子类继承父类的所有方法后

```python
import abc

class CacheBase(metaclass=abc.ABCMeta):
    @abc.abstractmethod
    def get(self,key):
        pass
        # raise ValueError

    @abc.abstractmethod
    def set(self,key,value):
        pass
        # raise NotImplementedError

class RedisBase(CacheBase):
    #重写父类中的方法
    def get(self,key):
        pass

    def set(self,key,value):
        pass

r = RedisBase()
r.get('Tom')
1234567891011121314151617181920212223
```

再次运行不报错。
加了装饰器`@abc.abstractmethod`的父类方法必须被子类继承重写。

## 三、两对概念辨析

### 1.isinstance和type

```python
i = 1
s = 'hello'
print(isinstance(i,int))
print(isinstance(s,int))
print(isinstance(s,str))
12345
```

打印

```python
True
False
True
123
```

`isinstance()`方法会**考虑类的继承关系**。

```python
class A(object):
    pass

class B(A):
    pass

b = B()
print(isinstance(b,B))
print(isinstance(b,A))
123456789
```

打印

```python
True
True
12
```

`type()`**没有考虑类的继承关系**。

```python
class A(object):
    pass

class B(A):
    pass

b = B()
print(type(b) is B)
print(type(b) is A)
123456789
```

打印

```python
True
False
12
```

### 2.类变量和对象变量

类属性可以向上查找，实例属性不能向下查找。

```python
class A:
    #类属性
    aa = 1

    #实例方法
    def __init__(self,x,y):
        #实例属性
        self.x = x
        self.y = y

a = A(1,2)
print(a.x,a.y,a.aa)
try:
    print(A.x)
except Exception as e:
    print(e.args[0])

A.aa = 11
#相当于在实例化时增加self.aa = 22
a.aa = 22
print(a.aa)
print(A.aa)

b = A(1,2)
print(b.aa)
12345678910111213141516171819202122232425
```

打印

```python
1 2 1
type object 'A' has no attribute 'x'
22
11
11
12345
```

## 四、MRO算法查找顺序和自省机制

### 1.MRO算法

```python
class A(object):
    name = 'Corley'

    def __init__(self):
        self.name = 'cl'

a =A()
print(a.name)
print(A.name)
123456789
```

打印

```python
cl
Corley
12
```

#### （1）Python2.2之前的算法:金典类

![Python2.2之前的算法:金典类](https://img-blog.csdnimg.cn/20200129191319728.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
DFS(deep first search)：A->B->D->C->E

```python
class D(object):
    pass

class B(D):
    pass

class E(object):
    pass

class C(E):
    pass

class A(B,C):
    pass

print(A.__mro__)
12345678910111213141516
```

打印

```python
(<class '__main__.A'>, <class '__main__.B'>, <class '__main__.D'>, <class '__main__.C'>, <class '__main__.E'>, <class 'object'>)
1
```

顺序为A->B->D->C->E->object。

#### （2）Python2.2版本之后,引入了BFS(广度优先搜索)

![BFS(广度优先搜索)](https://img-blog.csdnimg.cn/20200129191424408.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
BFS:A->B->C->D

```python
class D(object):
    pass

class B(D):
    pass

class C(D):
    pass

class A(B,C):
    pass

print(A.__mro__)
12345678910111213
```

打印

```python
(<class '__main__.A'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.D'>, <class 'object'>)
1
```

顺序为A->B->C->D->object。

#### （3）在Python2.3之后,Python采用了C3算法

Python新式类继承的C3算法：https://www.cnblogs.com/blackmatrix/p/5644023.html。

### 2.自省机制

自省是通过一定的机制查询到对象的内部结构。
Python中比较常见的自省（**introspection**）机制(函数用法)有：
`dir()`，`type()`, `hasattr()`, `isinstance()`。
通过这些函数，我们能够在程序运行时得知对象的类型，判断对象是否存在某个属性，访问对象的属性。

```python
class Person(object):
    name = 'Corley'


class Student(Person):
    def __init__(self,school_name):
        self.school_name = school_name

user = Student('CUFE')
#__dict__不包括父类属性
print(user.__dict__)
print(user.name)
print(dir(user))
12345678910111213
```

打印

```python
{'school_name': 'CUFE'}
Corley
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'name', 'school_name']
123
```

又如

```python
a = [1,2]
try:
    print(a.__dict__)
except Exception as e:
    print(e.args[0])
print(dir(a))
print(list.__dict__)
1234567
```

打印

```python
'list' object has no attribute '__dict__'
['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
{'__repr__': <slot wrapper '__repr__' of 'list' objects>, '__hash__': None, '__getattribute__': <slot wrapper '__getattribute__' of 'list' objects>, '__lt__': <slot wrapper '__lt__' of 'list' objects>, '__le__': <slot wrapper '__le__' of 'list' objects>, '__eq__': <slot wrapper '__eq__' of 'list' objects>, '__ne__': <slot wrapper '__ne__' of 'list' objects>, '__gt__': <slot wrapper '__gt__' of 'list' objects>, '__ge__': <slot wrapper '__ge__' of 'list' objects>, '__iter__': <slot wrapper '__iter__' of 'list' objects>, '__init__': <slot wrapper '__init__' of 'list' objects>, '__len__': <slot wrapper '__len__' of 'list' objects>, '__getitem__': <method '__getitem__' of 'list' objects>, '__setitem__': <slot wrapper '__setitem__' of 'list' objects>, '__delitem__': <slot wrapper '__delitem__' of 'list' objects>, '__add__': <slot wrapper '__add__' of 'list' objects>, '__mul__': <slot wrapper '__mul__' of 'list' objects>, '__rmul__': <slot wrapper '__rmul__' of 'list' objects>, '__contains__': <slot wrapper '__contains__' of 'list' objects>, '__iadd__': <slot wrapper '__iadd__' of 'list' objects>, '__imul__': <slot wrapper '__imul__' of 'list' objects>, '__new__': <built-in method __new__ of type object at 0x00007FFDDE682D30>, '__reversed__': <method '__reversed__' of 'list' objects>, '__sizeof__': <method '__sizeof__' of 'list' objects>, 'clear': <method 'clear' of 'list' objects>, 'copy': <method 'copy' of 'list' objects>, 'append': <method 'append' of 'list' objects>, 'insert': <method 'insert' of 'list' objects>, 'extend': <method 'extend' of 'list' objects>, 'pop': <method 'pop' of 'list' objects>, 'remove': <method 'remove' of 'list' objects>, 'index': <method 'index' of 'list' objects>, 'count': <method 'count' of 'list' objects>, 'reverse': <method 'reverse' of 'list' objects>, 'sort': <method 'sort' of 'list' objects>, '__doc__': 'Built-in mutable sequence.\n\nIf no argument is given, the constructor creates a new empty list.\nThe argument must be an iterable if specified.'}
123
```

易知，列表没有`__dict__`属性，列表对象有`__dict__`。

## 五、super函数

在类的继承中，如果重定义某个方法，该方法会覆盖父类的同名方法。
但有时，我们希望能同时实现父类的功能，这时，我们就需要调用父类的方法了，可通过使用**super**来实现。

```python
class A(object):
    def __init__(self):
        print('A')

class B(A):
    def __init__(self):
        print('B')
        #继承父类方法，打印出A
        super().__init__() #Python2中为super(B,self).__init__()

b = B()
1234567891011
```

打印

```python
B
A
12
```

### 思考：

#### 为什么重写了B的构造函数，还要去调用super？

```python
class People(object):
    def __init__(self,name,age,weight):
        self.name = name
        self.age = age
        self.weight = weight

    def speak(self):
        print('%s says: I\'m %d years old'%(self.name,self.age))


class Student(People):
    def __init__(self,name,age,weight,grade):
        super().__init__(name,age,weight)
        self.grade = grade

s = Student('Tom',18,30,3)
s.speak()
1234567891011121314151617
```

打印

```python
Tom says: I'm 18 years old
1
```

或者用父类类名来初始化子类，同时需要self参数。

```python
class People(object):
    def __init__(self,name,age,weight):
        self.name = name
        self.age = age
        self.weight = weight

    def speak(self):
        print('%s says: I\'m %d years old'%(self.name,self.age))


class Student(People):
    def __init__(self,name,age,weight,grade):
        People.__init__(self,name,age,weight)
        self.grade = grade

    def speak(self):
        print('%s says: I\'m %d years old, and I\'m in grade %d' % (self.name, self.age,self.grade))

s = Student('Tom',18,30,3)
s.speak()
1234567891011121314151617181920
```

打印

```python
Tom says: I'm 18 years old, and I'm in grade 3
1
```

结论：
调用super的作用是：
**节省实例化参数代码，避免重复；
扩展属性**。

#### super执行顺序是怎样的？

```python
class A(object):
    def __init__(self):
        print('A')

class B(A):
    def __init__(self):
        print('B')
        super().__init__()

class C(A):
    def __init__(self):
        print('C')
        super().__init__()

class D(B,C):
    def __init__(self):
        print('D')
        super().__init__()

d = D()
print(D.__mro__)
123456789101112131415161718192021
```

打印

```python
D
B
C
A
(<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)
12345
```

总结：
super调用父类中的方法不是按照一般的方法调用的，而是按照**MRO算法**来调用的，其顺序也是MRO算法的顺序。
又如

```python
class A(object):
    def __init__(self):
        print('A')

class B(A):
    def __init__(self):
        print('B')
        super().__init__()

class C(A):
    def __init__(self):
        print('C')
        super().__init__()

class E(object):
    def __init__(self):
        print('E')

class D(B,C,E):
    def __init__(self):
        print('D')
        super().__init__()

d = D()
print(D.__mro__)
12345678910111213141516171819202122232425
```

打印

```python
D
B
C
A
(<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class '__main__.E'>, <class
```

# Python全栈（四）高级编程技巧之2.类与对象深度问题与解决技巧

## 一、派生内置不可变类型并修改其实例化行为

引入：
我们想自定义一种新类型的元组,对于传入的可迭代对象,我们只保留其中int类型且值大于0的元素，例如:
`IntTuple([2,-2,'jr',['x','y'],4]) => (2,4)`
如何继承内置tuple 实现IntTuple？
自定义IntTuple：

```python
class IntTuple(tuple):
    def __init__(self,iterable):
        for i in iterable:
            if isinstance(i,int) and i > 0:
                super().__init__(i)

int_t = IntTuple([2,-2,'cl',['x','y'],4])
print(int_t)
12345678
```

打印

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 7, in <module>
    int_t = IntTuple([2,-2,'cl',['x','y'],4])
  File "xxx/demo.py", line 5, in __init__
    super().__init__(i)
TypeError: object.__init__() takes exactly one argument (the instance to initialize)
123456
```

问题：self对象到底是谁创建的：

```python
class A:
    def __new__(cls, *args, **kwargs):
        print('A.__new__',cls,args)
        return object.__new__(cls)

    def __init__(self,*args):
        print('A.__init__')

a = A(1,2)
123456789
```

打印

```python
A.__new__ <class '__main__.A'> (1, 2)
A.__init__
12
```

易知，真正创建对象的是__new__方法，**self对象是通过__new__创建的**。
上述示例默认继承object，也可以继承自其他类，__new__方法返回的是super().**new**(cls)。

```python
class B:
    pass

class A(B):
    def __new__(cls, *args, **kwargs):
        print('A.__new__',cls,args)
        return super().__new__(cls)

    def __init__(self,*args):
        print('A.__init__')

a = A(1,2)
123456789101112
```

运行结果与前者相同，但最好采用第一种方式。
A的实例化可以转化为另两行等价的代码：

```python
class A:
    def __new__(cls, *args, **kwargs):
        print('A.__new__',cls,args)
        return object.__new__(cls)

    def __init__(self,*args):
        print('A.__init__')

# a = A(1,2)相当于下面两行代码
a = A.__new__(A,1,2)
A.__init__(a,1,2)
1234567891011
```

创建列表转化等价代码：

```python
l = list.__new__(list,'abc')
print(l)
list.__init__(l,'abc')
print(l)
1234
```

打印

```python
[]
['a', 'b', 'c']
12
```

但是元组不同：

```python
t = tuple.__new__(tuple,'abc')
print(t)
tuple.__init__(t,'abc')
print(t)
1234
```

打印

```python
('a', 'b', 'c')
('a', 'b', 'c')
12
```

在第一步调用__new__方法时就已经实例化生成了元组。
这也解释了在最开始自定义IntTuple时__init__(self,iterable)会报错：
在__init__(self,iterable)之前调用__new__方法时元组已经被创建好了，并且元组是不能被修改的，所以会报错，不能实现预期效果。
对代码的修改：

```python
class IntTuple(tuple):
    def __new__(cls,iterable):
        #生成器
        r = (i for i in iterable if isinstance(i,int) and i > 0)
        return super().__new__(cls,r)

int_t = IntTuple([2,-2,'cl',['x','y'],4])
print(int_t)
12345678
```

打印

```python
(2, 4)
1
```

实现了预期功能。

## 二、创建大量实例节省内存

问题提出：
在游戏中,定义了玩家类player,每有一个在线玩家,在服务器内则有一个player的实例,当在线人数很多时,将产生大量实例(百万级)，如何降低这些大量实例的内存开销？
解决方案：
定义类的__slots__属性,声明实例有哪些属性(**关闭动态绑定**)。

```python
class Player1(object):
    def __init__(self,uid,name,status=0,level=1):
        self.uid = uid
        self.name = name
        self.status = status
        self.level = level

class Player2(object):
    __slots__ = ('uid','name','status','level')
    def __init__(self,uid,name,status=0,level=1):
        self.uid = uid
        self.name = name
        self.status = status
        self.level = level

p1 = Player1('0001','Tom')
p2 = Player2('0002','Tom')
print(len(dir(p1)))
print(len(dir(p2)))
print(dir(p1))
print(dir(p2))
print(set(dir(p1)) - set(dir(p2)))
12345678910111213141516171819202122
```

打印

```python
30
29
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'level', 'name', 'status', 'uid']
['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__slots__', '__str__', '__subclasshook__', 'level', 'name', 'status', 'uid']
{'__dict__', '__weakref__'}
12345
```

显然，p1和p2的所有属性相比较，p2比p1少了属性，其中__weakref__是**弱引用**，__dict__是**动态绑定属性**，主要的内存浪费就在于此。

```python
class Player1(object):
    def __init__(self,uid,name,status=0,level=1):
        self.uid = uid
        self.name = name
        self.status = status
        self.level = level

p1 = Player1('0001','Tom')
p1.x = 6
p1.__dict__['y'] = 7 #p1.__dict__本身就是字典
print(p1.__dict__)
1234567891011
```

打印

```python
{'uid': '0001', 'name': 'Tom', 'status': 0, 'level': 1, 'x': 6, 'y': 7}
1
```

可知，增加了属性x和y，此即动态绑定，即可以随意向对象添加属性。
Python可以通过这种动态绑定添加属性，但是这种方式是及其浪费内存的，添加__slots__后可消除动态绑定、解决这个问题。

```python
class Player1(object):
    def __init__(self,uid,name,status=0,level=1):
        self.uid = uid
        self.name = name
        self.status = status
        self.level = level

class Player2(object):
    __slots__ = ('uid','name','status','level')
    def __init__(self,uid,name,status=0,level=1):
        self.uid = uid
        self.name = name
        self.status = status
        self.level = level

p1 = Player1('0001','Tom')
p2 = Player2('0002','Tom')
p1.x = 6
p2.y = 7
print(p1.__dict__)
print(p2.__dict__)
123456789101112131415161718192021
```

打印

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 73, in <module>
    p2.y = 7
AttributeError: 'Player2' object has no attribute 'y'
1234
```

即不允许再向Player2的实例任意添加属性。
导入sys模块查看具体内存：

```python
import sys

class Player1(object):
    def __init__(self,uid,name,status=0,level=1):
        self.uid = uid
        self.name = name
        self.status = status
        self.level = level

p1 = Player1('0001','Tom')
p1.x = 6
print(p1.__dict__)
print(sys.getsizeof(p1.__dict__))
print(sys.getsizeof(p1.uid))
print(sys.getsizeof(p1.name))
print(sys.getsizeof(p1.status))
print(sys.getsizeof(p1.level))
print(sys.getsizeof(p1.x))
123456789101112131415161718
```

打印

```python
112
53
52
24
28
28
123456
```

不允许对实例动态修改__slots__：

```python
class Player1(object):
    def __init__(self,uid,name,status=0,level=1):
        self.uid = uid
        self.name = name
        self.status = status
        self.level = level

class Player2(object):
    __slots__ = ('uid','name','status','level')
    def __init__(self,uid,name,status=0,level=1):
        self.uid = uid
        self.name = name
        self.status = status
        self.level = level

p1 = Player1('0001','Tom')
p2 = Player2('0002','Tom')
p1.x = 6
p1.__dict__['y'] = 7
p2.__slots__ = ('uid','name','status','level','y')
print(p1.__dict__)
print(p2.__dict__)
12345678910111213141516171819202122
```

打印

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 74, in <module>
    p2.__slots__ = ('uid','name','status','level','y')
AttributeError: 'Player2' object attribute '__slots__' is read-only
1234
```

报错，__slots__是只读的。
导入库tracemalloc跟踪内存的使用：

```python
import tracemalloc

class Player1(object):
    def __init__(self,uid,name,status=0,level=1):
        self.uid = uid
        self.name = name
        self.status = status
        self.level = level

class Player2(object):
    __slots__ = ('uid','name','status','level')
    def __init__(self,uid,name,status=0,level=1):
        self.uid = uid
        self.name = name
        self.status = status
        self.level = level

tracemalloc.start()
p1 = [Player1('0001','Tom',2,3) for _ in range(100000)]
p2 = [Player1('0001','Tom',2,3) for _ in range(100000)]
end = tracemalloc.take_snapshot()
top = end.statistics('lineno')

for stat in top[:10]:
    print(stat)
12345678910111213141516171819202122232425
```

打印

```python
xxx/demo.py:59: size=21.4 MiB, count=399992, average=56 B
xxx/demo.py:74: size=6274 KiB, count=100003, average=64 B
xxx/demo.py:73: size=6274 KiB, count=100001, average=64 B
xxx/demo.py:75: size=432 B, count=1, average=432 B
xxx\Python\Python37\lib\tracemalloc.py:532: size=64 B, count=1, average=64 B
12345
```

`end.statistics()`的参数为’lineno’时是对占内存的代码逐行分析：
59行和73行是对p1进行分析，占内存为21.4 MiB（__dict__属性内存）+6274 KiB=27.5MiB；
74行是对p2进行分析：6274 KiB=6.1MiB。
`end.statistics()`的参数为’filename’时是对整个文件内存进行分析：
此时需要对p1和p2分别分析：
对p1：

```python
import tracemalloc

class Player1(object):
    def __init__(self,uid,name,status=0,level=1):
        self.uid = uid
        self.name = name
        self.status = status
        self.level = level

class Player2(object):
    __slots__ = ('uid','name','status','level')
    def __init__(self,uid,name,status=0,level=1):
        self.uid = uid
        self.name = name
        self.status = status
        self.level = level

tracemalloc.start()
p1 = [Player1('0001','Tom',2,3) for _ in range(100000)]
# p2 = [Player1('0001','Tom',2,3) for _ in range(100000)]
end = tracemalloc.take_snapshot()
# top = end.statistics('lineno')
top = end.statistics('filename')

for stat in top[:10]:
    print(stat)
1234567891011121314151617181920212223242526
```

打印

```python
xxx/demo.py:0: size=16.8 MiB, count=299994, average=59 B
xxx\Python\Python37\lib\tracemalloc.py:0: size=64 B, count=1, average=64 B
12
```

对p2：

```python
import tracemalloc

class Player1(object):
    def __init__(self,uid,name,status=0,level=1):
        self.uid = uid
        self.name = name
        self.status = status
        self.level = level

class Player2(object):
    __slots__ = ('uid','name','status','level')
    def __init__(self,uid,name,status=0,level=1):
        self.uid = uid
        self.name = name
        self.status = status
        self.level = level

tracemalloc.start()
# p1 = [Player1('0001','Tom',2,3) for _ in range(100000)]
p2 = [Player1('0001','Tom',2,3) for _ in range(100000)]
end = tracemalloc.take_snapshot()
# top = end.statistics('lineno')
top = end.statistics('filename')

for stat in top[:10]:
    print(stat)
1234567891011121314151617181920212223242526
```

打印

```python
xxx/demo.py:0: size=16.8 MiB, count=299994, average=59 B
xxx\Python\Python37\lib\tracemalloc.py:0: size=64 B, count=1, average=64 B
12
```

## 三、Python中的with语句

文件处理：

```python
try:
    f = open('test.txt','w')
    raise KeyError
except KeyError as e:
    print('Key Error')
    f.close()
except IndexError as e:
    print('Index Error')
    f.close()
except Exception as e:
    print('Error')
    f.close()
finally:
    print('end')
    f.close()
123456789101112131415
```

打印

```python
Key Error
end
12
```

代码中有多个except显得很冗余重复，所以引入with语句：

```python
with open('test.txt','r') as f:
    f.read()
12
```

类关于上下文处理的尝试：

```python
class Sample(object):
    def demo(self):
        print('This is a demo')

with Sample() as sample:
    sample.demo()
123456
```

打印

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 105, in <module>
    with Sample() as sample:
AttributeError: __enter__
1234
```

报错AttributeError（没有__enter__方法），这个方法与上下文处理有关。
改进-增加__enter__方法：

```python
class Sample(object):
    def __enter__(self):
        print('start')
        return self

    def demo(self):
        print('This is a demo')

with Sample() as sample:
    sample.demo()
12345678910
```

打印

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 109, in <module>
    with Sample() as sample:
AttributeError: __exit__
1234
```

报错AttributeError,没有__exit__方法。
改进-增加__exit__方法：

```python
class Sample(object):
    #获取资源
    def __enter__(self):
        print('start')
        return self

    def demo(self):
        print('This is a demo')
    
    #释放资源
    def __exit__(self, exc_type, exc_val, exc_tb):
        print('end')

with Sample() as sample:
    sample.demo()
123456789101112131415
```

打印

```python
start
This is a demo
end
123
```

此时正常运行。
易知，要想用上下文处理器来处理类，要在类中增加__enter__和__exit__方法。
类中的3个方法可类比文件处理中的打开文件、处理文件、关闭文件，代替了异常处理（try…except…）语句。
__exit__的参数探索：
正常情况下：

```python
class Sample(object):
    def __enter__(self):
        print('start')
        return self

    def demo(self):
        print('This is a demo')

    def __exit__(self, exc_type, exc_val, exc_tb):
        print('exc_type:',exc_type)
        print('exc_val:',exc_val)
        print('exc_tb:',exc_tb)
        print('end')

with Sample() as sample:
    sample.demo()
12345678910111213141516
```

打印

```python
start
This is a demo
exc_type: None
exc_val: None
exc_tb: None
end
123456
```

有异常时：

```python
class Sample(object):
    def __enter__(self):
        print('start')
        return self

    def demo(self):
        print('This is a demo')

    def __exit__(self, exc_type, exc_val, exc_tb):
        #异常类
        print('exc_type:',exc_type)
        #异常值
        print('exc_val:',exc_val)
        #追踪信息
        print('exc_tb:',exc_tb)
        print('end')

with Sample() as sample:
    sample.sample()
12345678910111213141516171819
```

打印

```python
Traceback (most recent call last):
start
  File "xxx/demo.py", line 119, in <module>
exc_type: <class 'AttributeError'>
    sample.sample()
exc_val: 'Sample' object has no attribute 'sample'
AttributeError: 'Sample' object has no attribute 'sample'
exc_tb: <traceback object at 0x00000210286C8388>
end
123456789
```

即__exit__三个参数携带了有关异常处理的信息。
contextlib库简化上下文管理器：

```python
import contextlib

@contextlib.contextmanager
def file_open(filename):
    #相当于__enter__
    print('file open')
    yield {}
    # 相当于__exit__
    print('file close')

with file_open('test.txt') as f:
    print('file operation')
123456789101112
```

打印

```python
file open
file operation
file close
123
```

此即上下文管理器协议；
file_open函数中必须要用yield，不能用return，否则会报错。

## 四、创建可管理的对象属性

在面向对象编程中,我们把方法看做对象的接口。
直接访问对象的属性可能是不安全的,或设计上不够灵活,但是使用调用方法在形式上不如访问属性简洁。
调用方法：

```python
A.get_key()  #访问器
A.set_key()  #设置器
12
```

这种方式较麻烦。
属性访问：

```python
A.key
A.key = 'xxx'
12
```

这种方式不安全。
寻找形式上属性访问、实质上调用方法的方式：

```python
class A:
    def __init__(self,age):
        self.age = age

    def get_age(self):
        return self.age

    def set_age(self,age):
        if not isinstance(age,int):
            raise TypeError('Type Error')
        self.age = age

a =A(18)
a.set_age('20')
1234567891011121314
```

打印

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 147, in <module>
    a.set_age('20')
  File "xxx/demo.py", line 143, in set_age
    raise TypeError('Type Error')
TypeError: Type Error
123456
```

当设置年龄不为整型时会抛出异常，但是较麻烦。
property两种调用方式：
（1）property(fget=None, fset=None, fdel=None, doc=None)：

```python
class C(object):
    def getx(self): return self._x
    def setx(self, value): self._x = value
    def delx(self): del self._x
    x = property(getx, setx, delx, "I'm the 'x' property.")
12345
```

例如：

```python
class A:
    def __init__(self,age):
        self.age = age

    def get_age(self):
        return self.age

    def set_age(self,age):
        if not isinstance(age,int):
            raise TypeError('Type Error')
        self.age = age

    R = property(get_age,set_age)

a =A(18)
#set
a.R = 20
#get
print(a.R)
12345678910111213141516171819
```

打印

```python
20
1
```

（2）加装饰器

```python
class C(object):
    @property
    def x(self):
        "I am the 'x' property."
        return self._x
    @x.setter
    def x(self, value):
        self._x = value
    @x.deleter
    def x(self):
        del self._x
1234567891011
```

例如：

```python
class A:
    def __init__(self,age):
        self.age = age

    def get_age(self):
        return self.age

    def set_age(self,age):
        if not isinstance(age,int):
            raise TypeError('Type Error')
        self.age = age

    R = property(get_age,set_age)

    @property
    def S(self):
        return self.age

    @S.setter
    def S(self,age):
        if not isinstance(age,int):
            raise TypeError('Type Error')
        self.age = age

a =A(18)
#set
a.S = 20
#get
print(a.S)
1234567891011121314151617181920212223242526272829
```

打印

```python
20
1
```

## 五、类支持比较操作

有时我们希望自定义类的实例间可以使用>、<、<=、>=、==、!=符号进行比较,例如,有一个矩形的类,比较两个矩形的实例时,比较的是它们的面积。

```python
a = 1
b = 2
print(a > b)
print(a < b)
print(a.__ge__(b))
print(a.__lt__(b))
c = 'abc'
d = 'abd'
print(c > d)
print(c < d)
print(c.__ge__(d))
print(c.__lt__(d))
e = {1,2,3}
f = {1,2}
g = {1,4}
#集合比较的是包含，包含则为True，否则为False
print(e > f)
print(e > g)
123456789101112131415161718
```

打印

```python
False
True
False
True
False
True
False
True
True
False
12345678910
```

在类中实现比较：

```python
class Rect(object):
    def __init__(self,w,b):
        self.w = w
        self.b = b

    def area(self):
        return self.w * self.b

    def __str__(self):
        return '(%s,%s)' % (self.w,self.b)

    def __lt__(self, other):
        return self.area() < other.area()

    def __gt__(self, other):
        return self.area() > other.area()


rect1 = Rect(1,2)
rect2 = Rect(3,4)
print(rect1 < rect2)
123456789101112131415161718192021
```

打印

```python
True
1
```

__lt__和__gt__方法可省去其中一个，也能实现大小的比较，如：

```python
class Rect(object):
    def __init__(self,w,b):
        self.w = w
        self.b = b

    def area(self):
        return self.w * self.b

    def __str__(self):
        return '(%s,%s)' % (self.w,self.b)

    def __lt__(self, other):
        return self.area() < other.area()


rect1 = Rect(1,2)
rect2 = Rect(3,4)
print(rect1 > rect2)
123456789101112131415161718
```

打印

```python
False
1
```

原理解释：
在类Rect中只实现了小于比较，Python内部实现了自动转化`rect2 < rect1`，而事实上该不等式不成立，所以为False，所以返回False。
但是要实现等于或者其他比较，还需要在类中实现相应的方法，很麻烦。
导入库对类加装饰器：

```python
from functools import total_ordering

@total_ordering
class Rect(object):
    def __init__(self,w,b):
        self.w = w
        self.b = b

    def area(self):
        return self.w * self.b

    def __str__(self):
        return '(%s,%s)' % (self.w,self.b)

    def __lt__(self, other):
        return self.area() < other.area()

    def __eq__(self, other):
        return self.area() == other.area()


rect1 = Rect(1,2)
rect2 = Rect(3,4)
print(rect1 > rect2)
print(rect1 < rect2)
print(rect1 >= rect2)
print(rect1 <= rect2)
print(rect1 == rect2)
print(rect1 != rect2)
1234567891011121314151617181920212223242526272829
```

打印

```python
False
True
False
True
False
True
123456
```

即此时只需要在类中实现__lt__和__eq__两个方法即可实现所有类型的比较。
两个类之间比较：

```python
from functools import total_ordering
import math

@total_ordering
class Rect(object):
    def __init__(self,w,b):
        self.w = w
        self.b = b

    def area(self):
        return self.w * self.b

    def __str__(self):
        return '(%s,%s)' % (self.w,self.b)

    def __lt__(self, other):
        return self.area() < other.area()

    def __eq__(self, other):
        return self.area() == other.area()

class Cricle(object):
    def __init__(self,r):
        self.r = r

    def area(self):
        return math.pi * pow(self.r,2)

    def __lt__(self, other):
        return self.area() < other.area()

    def __eq__(self, other):
        return self.area() == other.area()

rect = Rect(1,2)
c = Cricle(1)
print(rect > c)
print(rect < c)
print(rect == c)
123456789101112131415161718192021222324252627282930313233343536373839
```

打印

```python
False
True
False
123
```

对比较进行优化：用抽象基类简化代码

```python
from functools import total_ordering
import math
import abc

@total_ordering
class Shape(metaclass=abc.ABCMeta):
    @abc.abstractmethod
    def area(self):
        pass

    def __lt__(self, other):
        return self.area() < other.area()

    def __eq__(self, other):
        return self.area() == other.area()


class Rect(Shape):
    def __init__(self,w,b):
        self.w = w
        self.b = b

    def area(self):
        return self.w * self.b


class Cricle(Shape):
    def __init__(self,r):
        self.r = r

    def area(self):
        return math.pi * pow(self.r,2)


rect = Rect(1,2)
c = Cricle(1)
print(rect > c)
print(rect < c)
print(rect == c)
123456789101112131415161718192021222324252627282930313233343536373839
```

执行结果与之前相同。

## 六、环状数据结构中管理内存

垃圾回收：

```python
class A(object):
    #被释放时执行
    def __del__(self):
        print('del')

a = A()
print('after a = A()')
a2 = a
print('after a2 = a')
a2 = None
print('after a2 = None')
a = None
print('after a = None')
12345678910111213
```

打印

```python
after a = A()
after a2 = a
after a2 = None
del
after a = None
12345
```

易知，在a被改变时A的实例被释放回收；
垃圾回收机制针对的是引用计数。
双向循环链表：

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None

    def add_right(self, node):
        self.right = node
        node.left = self

    def __str__(self):
        return 'Node:<%s>' % self.data

    def __del__(self):
        print('in __del__: delete %s' % self)

def create_linklist(n):
    head = current = Node(1)
    for i in range(2, n + 1):
        node = Node(i)
        current.add_right(node)
        current = node
    return head

#创建节点1000次
head = create_linklist(1000)
#释放节点
head = None

import time
for _ in range(1000):
    time.sleep(1)
    print('run...')
input('wait...')
12345678910111213141516171819202122232425262728293031323334
```

![双向链表](https://img-blog.csdnimg.cn/20200130180029173.png#pic_center)
在命令行执行Python文件，并强制结束时，会打印

```python
run...
run...
run...
run...
run...
run...
run...
Traceback (most recent call last):
  File "demo.py", line 267, in <module>
    time.sleep(1)
KeyboardInterrupt
in __del__: delete Node:<999>
in __del__: delete Node:<1000>
in __del__: delete Node:<1>
in __del__: delete Node:<296>
in __del__: delete Node:<297>
in __del__: delete Node:<2>
...
in __del__: delete Node:<993>
in __del__: delete Node:<994>
in __del__: delete Node:<995>
in __del__: delete Node:<996>
in __del__: delete Node:<997>
in __del__: delete Node:<998>
123456789101112131415161718192021222324
```

即回收时会调用__del__。
弱引用：

```python
import weakref

class A(object):
    def __del__(self):
        print('del')

a = A()
print('after a = A()')
a2 = weakref.ref(a)
print('after a2 = weakref.ref(a)')
a3 = a2()
print('after a3 = a2()')
del a3
print('after del a3')
1234567891011121314
```

打印

```python
after a = A()
after a2 = weakref.ref(a)
after a3 = a2()
after del a3
del
12345
```

弱引用会在所有执行结束后再调用__del__；
弱引用不占用引用计数，Python中的垃圾回收是根据引用计数来的。
a2即是弱引用，a3是真实的引用。
![a2即是弱引用，a3是真实的引用](https://img-blog.csdnimg.cn/20200130175856670.png#pic_center)
上述双向链表可修改小部分代码即可变为弱引用：

```python
import weakref

class Node:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None

    def add_right(self, node):
        self.right = node
        # node.left = self
        node.left = weakref.ref(self)

    def __str__(self):
        return 'Node:<%s>' % self.data

    def __del__(self):
        print('in __del__: delete %s' % self)

def create_linklist(n):
    head = current = Node(1)
    for i in range(2, n + 1):
        node = Node(i)
        current.add_right(node)
        current = node
    return head

#创建节点1000次
head = create_linklist(1000)
#释放节点
head = None

import time
for _ in range(1000):
    time.sleep(1)
    print('run...')
input('wait...')
12345678910111213141516171819202122232425262728293031323334353637
```

打印

```python
in __del__: delete Node:<1>
in __del__: delete Node:<2>
in __del__: delete Node:<3>
in __del__: delete Node:<4>
in __del__: delete Node:<5>
in __del__: delete Node:<6>
...
in __del__: delete Node:<995>
in __del__: delete Node:<996>
in __del__: delete Node:<997>
in __del__: delete Node:<998>
in __del__: delete Node:<999>
in __del__: delete Node:<1000>
run...
run...
run...
run...
run...
run...
...
1234567891011121314151617181920
```

此时与之前的运行结果不一样，先调用__del__释放对象再运行后面的代码。

# Python全栈（四）高级编程技巧之3.垃圾回收机制和性能分析

## 一、通过实例方法名字的字符串调用方法

实例方法名字的字符串调用方法需要实现不同类的**统一接口**。

```python
s = 'abc123'
print(s.find('123'))
print('find:',getattr(s,'find'))
#字符串有find方法
print('find:',getattr(s,'find')('123'))
#字符串没有pop方法，加入默认值None
print('pop:',getattr(s,'pop',None))
1234567
```

打印

```python
3
find: <built-in method find of str object at 0x000001DCB858CD30>
find: 3
pop: None
1234
```

有三个图形类Circle、Triangle、Rectangle（在Shape.py文件中，和demo.py在同一目录下）：

```python
class Triangle:
    def __init__(self,a,b,c):
        self.a,self.b,self.c = a,b,c
    
    def get_area(self):
        a,b,c = self.a,self.b,self.c
        p = (a+b+c)/2
        return (p * (p-a)*(p-b)*(p-c)) ** 0.5

class Rectangle:
    def __init__(self,a,b):
        self.a,self.b = a,b
    
    def getArea(self):
        return self.a * self.b
        
class Circle:
    def __init__(self,r):
        self.r = r
    
    def area(self):
        return self.r ** 2 * 3.14159
12345678910111213141516171819202122
```

它们都有一个获取图形面积的方法，但是方法名字不同，我们可以实现一个统一的获取面积的函数，使用每种方法名进行尝试，调用相应类的接口。
**实现统一接口**：

```python
from Shape import *

shape1 = Triangle(3,4,5)
shape2 = Rectangle(4,6)
shape3 = Circle(3)

def get_area(shape):
    method_name = ['get_area','getArea','area']
    for name in method_name:
        r = getattr(shape,name,None)
        if r:
            return r()


print(get_area(shape1))
print(get_area(shape2))
print(get_area(shape3))
1234567891011121314151617
```

打印

```python
6.0
24
28.27431
123
```

通过**map()** 函数映射进一步简化：

```python
from Shape import *

shape1 = Triangle(3,4,5)
shape2 = Rectangle(4,6)
shape3 = Circle(3)

def get_area(shape):
    method_name = ['get_area','getArea','area']
    for name in method_name:
        r = getattr(shape,name,None)
        if r:
            return r()

shape_list = [shape1,shape2,shape3]
area_list = list(map(get_area,shape_list))
print(area_list)
12345678910111213141516
```

打印

```python
[6.0, 24, 28.27431]
1
```

## 二、垃圾回收机制

### 1.垃圾回收

Python 中一切皆对象，因此，你所看到的一切变量，本质上都是对象的一个指针。
那么，怎么知道一个对象，是否永远都不能被调用了呢？
就是当这个对象的引用计数（指针数）为0的时候，说明这个对象永不可达，自然它也就成为了垃圾，需要被回收。
`a = 1000`，当执行`del a`时，a的引用计数变为0，被回收。

```python
import os
import psutil

# 显示当前 python 程序占用的内存大小
def show_memory_info(hint):
    pid = os.getpid()
    p = psutil.Process(pid)

    info = p.memory_full_info()
    memory = info.uss / 1024. / 1024
    print('{} memory used: {} MB'.format(hint, memory))


def func():
    show_memory_info('initial')
    a = [i for i in range(10000000)]
    show_memory_info('after a created')


func()
show_memory_info('finished')
123456789101112131415161718192021
```

此代码在Windows下运行会报错，在Linux下才能正常运行返回结果：

```python
initial memory used: 9.546875 MB
after a created memory used: 277.4453125 MB
finished memory used: 3.640625 MB
123
```

在调用`func()`函数时内存急剧上升，调用完成之后回到初始水平。
解释：
a是在`func()`函数内部生成的局部变量，函数返回之后局部变量的引用会被销毁，a的引用计数变为0，触发垃圾回收机制，内存被释放。
将a声明为全局变量后，函数返回后a不会被回收，内存不会被释放：

```python
import os
import psutil

# 显示当前 python 程序占用的内存大小
def show_memory_info(hint):
    pid = os.getpid()
    p = psutil.Process(pid)

    info = p.memory_full_info()
    memory = info.uss / 1024. / 1024
    print('{} memory used: {} MB'.format(hint, memory))


def func():
    #声明全局变量
    global a
    show_memory_info('initial')
    a = [i for i in range(10000000)]
    show_memory_info('after a created')


func()
show_memory_info('finished')
1234567891011121314151617181920212223
```

打印：

```python
initial memory used: 9.8828125 MB
after a created memory used: 264.01953125 MB
finished memory used: 261.640625 MB
123
```

函数中将a返回并且无变量接收时，内存被释放：

```python
import os
import psutil

# 显示当前 python 程序占用的内存大小
def show_memory_info(hint):
    pid = os.getpid()
    p = psutil.Process(pid)

    info = p.memory_full_info()
    memory = info.uss / 1024. / 1024
    print('{} memory used: {} MB'.format(hint, memory))


def func():
    show_memory_info('initial')
    a = [i for i in range(10000000)]
    show_memory_info('after a created')
    return a


func()
show_memory_info('finished')
12345678910111213141516171819202122
```

打印：

```python
initial memory used: 9.8515625 MB
after a created memory used: 279.5234375 MB
finished memory used: 3.609375 MB
123
```

函数中将a返回并且有变量接收时，内存不会被释放：

```python
import os
import psutil

# 显示当前 python 程序占用的内存大小
def show_memory_info(hint):
    pid = os.getpid()
    p = psutil.Process(pid)

    info = p.memory_full_info()
    memory = info.uss / 1024. / 1024
    print('{} memory used: {} MB'.format(hint, memory))


def func():
    show_memory_info('initial')
    a = [i for i in range(10000000)]
    show_memory_info('after a created')
    return a


r = func()
show_memory_info('finished')
12345678910111213141516171819202122
```

打印：

```python
initial memory used: 9.75 MB
after a created memory used: 270.71484375 MB
finished memory used: 268.1328125 MB
123
```

### 2.引用计数

```python
import sys

a = []
#getrefcount()查看变量的引用次数，函数本身也算一次
print(sys.getrefcount(a))

def func(a):
    print(sys.getrefcount(a))

func(a)
12345678910
```

打印：

```python
2
4
12
```

**解释**：
函数的形参a算1次，函数内部引用1次，加之前的2次，共4次。
手动释放内存：
**方式一**：用`del`

```python
a = 3
del a
12
```

**方式二**：导入gc模块调用方法

```python
import gc
import os
import psutil

# 显示当前 python 程序占用的内存大小
def show_memory_info(hint):
    pid = os.getpid()
    p = psutil.Process(pid)
    info = p.memory_full_info()
    memory = info.uss / 1024. / 1024
    print('{} memory used: {} MB'.format(hint, memory))


show_memory_info('initial')
a = [i for i in range(10000000)]
show_memory_info('after a created')

del a
#清除没有引用的对象
gc.collect()

show_memory_info('finish')
12345678910111213141516171819202122
```

打印：

```python
1
```

易知，a被清除后，内存被释放。

### 3.循环引用

问题：
引用计数为0是垃圾回收机制启动的充分必要条件吗？
如果有两个对象，它们互相引用，并且不再被别的对象所引用，那么它们应该被垃圾回收吗？

```python
import os
import psutil


# 显示当前 python 程序占用的内存大小
def show_memory_info(hint):
    pid = os.getpid()
    p = psutil.Process(pid)

    info = p.memory_full_info()
    memory = info.uss / 1024. / 1024
    print('{} memory used: {} MB'.format(hint, memory))


def func():
    show_memory_info('initial')
    a = [i for i in range(10000000)]
    b = [i for i in range(10000000)]
    show_memory_info('after a, b created')
    a.append(b)
    b.append(a)

func()
show_memory_info('finished')
123456789101112131415161718192021222324
```

打印：

```python
1
```

显然，此时a和b的引用计数已经为0，但是并没有触发垃圾回收机制、内存没有被释放，需要手动销毁。
可得：
引用计数为0不是垃圾回收机制启动的充分必要条件，是**充分非必要**条件。

```python
import os
import psutil
import gc

# 显示当前 python 程序占用的内存大小
def show_memory_info(hint):
    pid = os.getpid()
    p = psutil.Process(pid)

    info = p.memory_full_info()
    memory = info.uss / 1024. / 1024
    print('{} memory used: {} MB'.format(hint, memory))


def func():
    show_memory_info('initial')
    a = [i for i in range(10000000)]
    b = [i for i in range(10000000)]
    show_memory_info('after a, b created')
    a.append(b)
    b.append(a)

func()
gc.collect()
show_memory_info('finished')
12345678910111213141516171819202122232425
```

打印：

```python
1
```

除了循环引用，还有可能存在引用环，即多个对象之间闭环式引用。

### 4.调试内存泄漏

虽然有了自动回收机制，但这也不是万能的，难免还是会有漏网之鱼。内存泄漏是我们不想见到的，而且还会严重影响性能。有没有什么好的调试手段呢？
它就是**objgraph**，一个非常好用的可视化引用关系的包。在这个包中，我主要推荐两个函数，第一个是`show_refs()`，它可以生成清晰的引用关系图。

```python
import objgraph

a = [1, 2, 3]
b = [4, 5, 6]

a.append(b)
b.append(a)

objgraph.show_refs([a])
123456789
```

打印：

```python
Graph written to xxx\Temp\objgraph-mh9h0c86.dot (8 nodes)
Graph viewer (xdot) and image renderer (dot) not found, not doing anything else
12
```

运行会生成临时的.dot文件，可以在文件目录中找到，内容如下所示：

```bash
digraph ObjectGraph {
  node[shape=box, style=filled, fillcolor=white];
  o2187464298312[fontcolor=red];
  o2187464298312[label="list\n4 items"];
  o2187464298312[fillcolor="0,0,1"];
  o2187464298312 -> o2187464300232;
  o2187464298312 -> o140728380322112;
  o2187464298312 -> o140728380322080;
  o2187464298312 -> o140728380322048;
  o2187464300232[label="list\n4 items"];
  o2187464300232[fillcolor="0,0,0.766667"];
  o2187464300232 -> o2187464298312;
  o2187464300232 -> o140728380322208;
  o2187464300232 -> o140728380322176;
  o2187464300232 -> o140728380322144;
  o140728380322112[label="int\n3"];
  o140728380322112[fillcolor="0,0,0.766667"];
  o140728380322080[label="int\n2"];
  o140728380322080[fillcolor="0,0,0.766667"];
  o140728380322048[label="int\n1"];
  o140728380322048[fillcolor="0,0,0.766667"];
  o140728380322208[label="int\n6"];
  o140728380322208[fillcolor="0,0,0.533333"];
  o140728380322176[label="int\n5"];
  o140728380322176[fillcolor="0,0,0.533333"];
  o140728380322144[label="int\n4"];
  o140728380322144[fillcolor="0,0,0.533333"];
}

1234567891011121314151617181920212223242526272829
```

可以选择文件转换工具实现格式转换，生成直观的图片，如https://onlineconvertfree.com/zh/。
最后得到的图形为
![objgraph-mh9h0c86.dot](https://img-blog.csdnimg.cn/20200131182628134.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
小结：

- 垃圾回收是 Python 自带的机制，用于自动释放不会再用到的内存空间；
- 引用计数是其中最简单的实现，不过切记，这只是充分非必要条件，因为循环引用需要通过不可达判定，来确定是否可以回收；
- Python 的自动回收算法包括标记清除和分代收集，主要针对的是循环引用的垃圾收集；
- 调试内存泄漏方面， objgraph 是很好的可视化分析工具。

## 三、调试和性能分析

### 1.用pdb进行代码调试

在程序中相应的地方打印是调试程序的一个常用手段，但这只适用于小型程序。因为你每次都得重新运行整个程序，或是一个完整的功能模块，才能看到打印出来的变量值。如果程序不大，每次运行都非常快，那么使用`print()`，的确是很方便的。

#### 使用pdb

首先，要启动 pdb 调试，我们只需要在程序中加入`import pdb`和`pdb.set_trace()`这两行代码就行了。

```python
import pdb

a = 1
b = 2
pdb.set_trace()
c = 3
print(a + b + c)
1234567
```

这时，我们就可以执行，在 IDE 断点调试器中可以执行的一切操作，比如打印，语法是"p"。

```python
> xxx\demo.py(128)<module>()
-> c = 3
(Pdb) p a
1
(Pdb) p b
2
(Pdb) p c
*** NameError: name 'c' is not defined
(Pdb) n
> xxx\demo.py(129)<module>()
-> print(a + b + c)
(Pdb) p c
3
(Pdb) l
124  	
125  	a = 1
126  	b = 2
127  	pdb.set_trace()
128  	c = 3
129  ->	print(a + b + c)
[EOF]
(Pdb) n
6
--Return--
> xxx\demo.py(129)<module>()->None
-> print(a + b + c)
(Pdb) n
123456789101112131415161718192021222324252627
```

输入命令说明：

- p：打印
- n：继续执行代码到下一行
- l：列举出当前代码行上下的 11 行源代码，方便开发者熟悉当前断点周围的代码状态
- s：step into 的意思，进入相对应的代码内部
- q：退出

除了这些常用命令，还有许多其他的命令可以使用，可以参考官方文档：[https://docs.python.org/3/library/pdb.html#module-pdb%EF%BC%89](https://docs.python.org/3/library/pdb.html#module-pdb）)。
再如

```python
import pdb

a = 1
b = 2

def func():
    print('enetr func()')

pdb.set_trace()
c = 3
func()
print(a + b + c)
123456789101112
```

打印：

```python
> xxx\demo.py(132)<module>()
-> c = 3
(Pdb) n
> xxx\demo.py(133)<module>()
-> func()
(Pdb) s
--Call--
> xxx\demo.py(128)func()
-> def func():
(Pdb) n
> xxx\demo.py(129)func()
-> print('enetr func()')
(Pdb) n
enetr func()
--Return--
> xxx\demo.py(129)func()->None
-> print('enetr func()')
(Pdb) n
> xxx\demo.py(134)<module>()
-> print(a + b + c)
(Pdb) n
6
--Return--
> xxx\demo.py(134)<module>()->None
-> print(a + b + c)
(Pdb) n
1234567891011121314151617181920212223242526
```

### 2.用cProfile进行性能分析

除了要对程序进行调试，性能分析也是每个开发者的必备技能。
日常工作中，我们常常会遇到这样的问题：在线上，我发现产品的某个功能模块效率低下，延迟高，占用的资源多，但却不知道是哪里出了问题。这时，对代码进行profile就显得异常重要了。
这里所谓的**profile**，是指对代码的每个部分进行动态的分析，比如准确计算出每个模块消耗的时间等。
运用递归思想，计算斐波拉契数列，并测试一下这段代码总的效率以及各个部分的效率：

```python
import cProfile

def fib(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib(n-1) + fib(n-2)

def fib_seq(n):
    res = []
    if n > 0:
        res.extend(fib_seq(n-1))
    res.append(fib(n))
    return res

cProfile.run('fib_seq(30)')
123456789101112131415161718
```

打印：

```python
         7049218 function calls (96 primitive calls) in 1.599 seconds

   Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    1.599    1.599 <string>:1(<module>)
7049123/31    1.599    0.000    1.599    0.052 demo.py:138(fib)
     31/1    0.000    0.000    1.599    1.599 demo.py:146(fib_seq)
        1    0.000    0.000    1.599    1.599 {built-in method builtins.exec}
       31    0.000    0.000    0.000    0.000 {method 'append' of 'list' objects}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
       30    0.000    0.000    0.000    0.000 {method 'extend' of 'list' objects}
123456789101112
```

**参数说明**：

| 参数            | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| ncalls          | 是指相应代码 / 函数被调用的次数                              |
| tottime         | 是指对应代码 / 函数总共执行所需要的时间（注意，并不包括它调用的其他代码 / 函数的执行时间） |
| tottime percall | 就是上述两者相除的结果，也就是tottime / ncalls               |
| cumtime         | 则是指对应代码 / 函数总共执行所需要的时间，这里包括了它调用的其他代码 / 函数的执行时间 |
| cumtime percall | 则是 cumtime 和 ncalls 相除的平均结果。                      |

## 四、经典的参数错误

### 1.不可变类型和可变类型

在Python中，对象分为可变对象和不可变对象：
对于不可变对象，如数值型对象

```python
i = 5
print(id(i))
i += 1
print(id(i))
1234
```

打印：

```python
140728286736768
140728286736800
12
```

在改变值后，相当于重新赋值，id改变，是不同的对象。
对于可变对象，如列表

```python
a = [1,2,3]
print(id(a))
a.append(4)
print(id(a))
1234
```

打印：

```python
2808636133960
2808636133960
12
```

在改变对象的值后，是对原对象进行操作，id不变，是同一个对象。
不可变类型和可变类型：

- 不可变类型：
  以int类型为例，实际上 i += 1 并不是真的在原有的int对象上+1，而是重新创建一个value为6的int对象，i引用自这个新的对象。
- 可变类型：
  以list为例，list在append之后，还是指向同个内存地址，因为list是可变类型，可以在原处修改。

```python
def add(a,b):
    a += b
    return a

a = 1
b = 2
c = add(a,b)
print(c)
print(a,b)

a = [1,2]
b = [3,4]
c = add(a,b)
print(c)
print(a,b)      

a = (1,2)
b = (3,4)
c = add(a,b)
print(c)
print(a,b)
123456789101112131415161718192021
```

打印：

```python
3
1 2
[1, 2, 3, 4]
[1, 2, 3, 4] [3, 4]
(1, 2, 3, 4)
(1, 2) (3, 4)
123456
```

易知，在调用`add()`函数后：
a、b为数值型值时，调用add()函数后a和b的值不变；
a、b为列表时，调用add()函数后a改变、b不变；
a、b为元组时，调用add()函数后a和b的值不变。
**解释**：
参数为数值型值和元组等不可变类型时，是不可变的，函数内部是对变量重新赋值，是新的对象；
参数为列表等可变类型时，是可变的，函数内部对其进行了值的改变。

### 2.小整数对象池

在命令行等交互环境中

```python
>>> a = 1
>>> b = 1
>>> print(a is b)
True
>>> print(a == b)
True
>>>
>>> c = 2222
>>> d = 2222
>>> print(c is d)
False
>>> print(c == d)
True
12345678910111213
```

整型的值较小时，是提前创建好的，可以直接使用，所以`a is b`的值为True；
整型的值较大时，是没有创建的，需要自己创建，所以两次赋值创建了两个不同的对象，所以`a is b`的值为False。
小整数对象池的范围是 **-5 - 256**。

# Python全栈（四）高级编程技巧之4.元类编程、迭代器和生成器

## 一、__getattr__和__getattribute__魔法函数

```python
from datetime import date


class User:
    def __init__(self, name, birthday):
        self.name = name
        self.birthday = birthday


if __name__ == "__main__":
    user = User("corley", date(year=2020, month=1, day=1))
    print(user.name)
123456789101112
```

打印

```python
corley
1
```

当打印不存在的属性时，会报错：

```python
from datetime import date


class User:
    def __init__(self, name, birthday):
        self.name = name
        self.birthday = birthday


if __name__ == "__main__":
    user = User("corley", date(year=2020, month=1, day=1))
    print(user.age)
123456789101112
```

打印

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 18, in <module>
    print(user.age)
AttributeError: 'User' object has no attribute 'age'
1234
```

此时需要加入`__getattr__`：

```python
from datetime import date


class User:
    def __init__(self, name, birthday):
        self.name = name
        self.birthday = birthday

    def __getattr__(self, item):
        print("not find attr")


if __name__ == "__main__":
    user = User("corley", date(year=2020, month=1, day=1))
    print(user.age)
123456789101112131415
```

打印

```python
not find attr
None
12
```

可得：
None表示打印`user.age`时调用`__getattr__`方法，该方法没有返回值，所以打印None；
`__getattr__`魔法方法是在查找不到属性的时候调用。
同时可打印`__getattr__`的参数item：

```python
from datetime import date


class User:
    def __init__(self, name, birthday):
        self.name = name
        self.birthday = birthday

    def __getattr__(self, item):
        print(item, "not find attr")


if __name__ == "__main__":
    user = User("corley", date(year=2020, month=1, day=1))
    print(user.age)
123456789101112131415
```

打印

```python
age not find attr
None
12
```

即表示没有找到User对象的age属性。
在初始化方法的参数中增加info字典，初始化实例时info里添加age键值对：

```python
from datetime import date


class User:
    def __init__(self, name, birthday, info = {}):
        self.name = name
        self.birthday = birthday

    def __getattr__(self, item):
        print(item, "not find attr")


if __name__ == "__main__":
    user = User("corley", date(year=2020, month=1, day=1),info={'age':18})
    print(user.age)
123456789101112131415
```

打印

```python
age not find attr
None
12
```

仍然查找不到age属性，要想能查找到age属性，需要改进代码：

```python
from datetime import date


class User:
    def __init__(self, name, birthday, info = {}):
        self.name = name
        self.birthday = birthday
        self.info = info

    def __getattr__(self, item):
        return self.info[item]


if __name__ == "__main__":
    user = User("corley", date(year=2020, month=1, day=1),info={'age':18})
    print(user.age)
12345678910111213141516
```

打印

```python
18
1
```

此时即可访问到字典中的属性age。
如果要访问的属性在字典中不存在时，会报和字典的箭不存在一样的错误：

```python
from datetime import date


class User:
    def __init__(self, name, birthday, info = {}):
        self.name = name
        self.birthday = birthday
        self.info = info

    def __getattr__(self, item):
        return self.info[item]


if __name__ == "__main__":
    user = User("corley", date(year=2020, month=1, day=1),info={'age':18})
    print(user.ages)
12345678910111213141516
```

打印

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 16, in <module>
    print(user.ages)
  File "xxx/demo.py", line 11, in __getattr__
    return self.info[item]
KeyError: 'ages'
123456
```

解决办法：

- 方法一：改进`__getattr__`方法里访问字典的方式

```python
from datetime import date


class User:
    def __init__(self, name, birthday, info = {}):
        self.name = name
        self.birthday = birthday
        self.info = info

    def __getattr__(self, item):
        return self.info.get(item)


if __name__ == "__main__":
    user = User("corley", date(year=2020, month=1, day=1),info={'age':18})
    print(user.ages)
12345678910111213141516
```

打印

```python
None
1
```

如果找不到相应的键，会返回None。

- 方法二：增加`__getattribute__`方法

```python
from datetime import date


class User:
    def __init__(self, name, birthday, info = {}):
        self.name = name
        self.birthday = birthday
        self.info = info

    def __getattr__(self, item):
        return self.info.get(item)

    def __getattribute__(self, item):
        return "corley"


if __name__ == "__main__":
    user = User("corley", date(year=2020, month=1, day=1),info={'age':18})
    print(user.age)
    print(user.ages)
    print(user.birthday)
123456789101112131415161718192021
```

打印

```python
corley
corley
corley
123
```

易知：
不管访问User对象的什么属性，不管属性是否存在，都返回的是`__getattribute__`方法的返回内容；
进一步可知，`__getattribute__`方法在`__getattr__`方法之前执行，不要轻易重写。

## 二、属性描述符

### 1.属性描述符分析

```python
class User:
    def __init__(self, age):
        self.age = age

    def get_age(self):
        return str(self.age) + 'years old'

    def set_age(self, age):
        if not isinstance(age, int):
            raise TypeError('Type Error')
        self.age = age
1234567891011
```

该类可以对年龄属性进行判断，不过不是整型，则抛出异常；
但是显然这只是对一个属性进行判断，如果User类中有多个属性都需要判断，那么就需要写多个方法，就很麻烦，我们需要对方法进行复用。这个时候就要用到属性描述符。
**属性描述符**：
在类中实现 `__get__` 、`__set__` 、`__del__` 中的一个方法，即可构成属性描述符。

```python
class IntField(object):
    def __get__(self, instance, owner):
        print('__get__')

    def __set__(self, instance, value):
        print('__set__')

    def __del__(self, instance):
        pass


class User:
    age = IntField()


user = User()
#set
user.age = 30
#get
print(user.age)
1234567891011121314151617181920
```

打印

```python
__set__
__get__
None
Exception ignored in: <function IntField.__del__ at 0x0000023DDEDC6C18>
TypeError: __del__() missing 1 required positional argument: 'instance'
12345
```

并没有打印出30，但是`__get__`和`__set__`方法被调用，并且先调用`__set__`方法。

```python
class IntField(object):
    def __get__(self, instance, owner):
        print('__get__')

    def __set__(self, instance, value):
        print('__set__')
        print(instance)
        print(value)

    def __del__(self, instance):
        pass


class User:
    age = IntField()


user = User()
#set
user.age = 30
#get
print(user.age)
12345678910111213141516171819202122
```

打印

```python
__set__
Exception ignored in: <function IntField.__del__ at 0x000001FCFE446C18>
<__main__.User object at 0x000001FCFE488388>
30
TypeError: __del__() missing 1 required positional argument: 'instance'
__get__
None
1234567
```

易知，`__set__`方法的参数value即User对象属性赋值的值，在执行`user.age = 30`时调用`__set__`方法方法并传参数值。
进一步改进–加入属性值判断：

```python
class IntField(object):
    def __get__(self, instance, owner):
        print('__get__')
        return self.value

    def __set__(self, instance, value):
        print('__set__')
        if not isinstance(value,int):
            raise TypeError('Type Error')
        self.value = value

    def __del__(self, instance):
        pass


class User:
    age = IntField()


user = User()
#set
user.age = 30
#get
print(user.age)
123456789101112131415161718192021222324
```

打印

```python
__set__
__get__
30
Exception ignored in: <function IntField.__del__ at 0x0000018F39F76C18>
TypeError: __del__() missing 1 required positional argument: 'instance'
12345
```

此时能打印出正常的属性值，这就是属性描述符。
属性描述符分为数据描述符（有`__get__`方法和`__set__`方法）和非数据描述符（只有`__get__`方法，较少），这里是数据描述符。
非数据描述符的定义如下：

```python
class NoneDataIntField:
    def __get__(self, instance, owner):
        pass
123
```

如果传入的不是整型值，就会报错：

```python
class IntField(object):
    def __get__(self, instance, owner):
        print('__get__')
        return self.value

    def __set__(self, instance, value):
        print('__set__')
        if not isinstance(value,int):
            raise TypeError('Type Error')
        self.value = value

    def __del__(self, instance):
        pass


class User:
    age = IntField()


user = User()
#set
user.age = '30'
#get
print(user.age)
123456789101112131415161718192021222324
```

打印

```python
__set__
Traceback (most recent call last):
  File "xxx/demo.py", line 56, in <module>
    user.age = '30'
  File "xxx/demo.py", line 43, in __set__
    raise TypeError('Type Error')
TypeError: Type Error
Exception ignored in: <function IntField.__del__ at 0x0000027003A66C18>
TypeError: __del__() missing 1 required positional argument: 'instance'
123456789
```

据此可对整型进行判断。

### 2.属性查找顺序

> user = User(), 那么user.age 顺序如下:
>
> （1） 如果"age"是出现在User或其基类的__dict__中, 且age是data descriptor,那么调用其__get__方法,
> 否则
>
> （2） 如果"age"出现在user的__dict__中, 那么直接返回 obj.**dict**[‘age’],否则
>
> （3） 如果"age"出现在User或其基类的__dict__中
>
> （3.1） 如果age是non-data descriptor,那么调用其__get__方法， 否则
>
> （3.2） 返回 **dict**[‘age’]
>
> （4）如果User有__getattr__方法，调用__getattr__方法，否则
>
> （5） 抛出AttributeError

**优先级最高的是属性描述符**，下面进行简单的证明：

```python
class IntField(object):
    def __get__(self, instance, owner):
        print('__get__')
        return self.value

    def __set__(self, instance, value):
        print('__set__')
        if not isinstance(value,int):
            raise TypeError('Type Error')
        self.value = value

    def __del__(self, instance):
        pass


class User:
    age = IntField()


user = User()
#set
user.age = 30
user.__dict__['age'] = 18
#get
print(user.age)
12345678910111213141516171819202122232425
```

打印

```python
Exception ignored in: <function IntField.__del__ at 0x0000023854326C18>
TypeError: __del__() missing 1 required positional argument: 'instance'
__set__
__get__
30
12345
```

易知，即便`user.__dict__['age']`在`user.age`后重新赋值18，返回的还是30，所以优先级最高的是属性描述符。

## 三、自定义元类

**元类**：
是创建类的类，即type类。

### 1.动态创建类

```python
def create_class(name):
    if name == 'user':
        class User:
            def __str__(self):
                return 'user'
        return User
    elif name == 'student':
        class Student:
            def __str__(self):
                return 'student'
        return Student

if __name__ == '__main__':
    myclass = create_class('user')
    obj = myclass()
    print(obj)
    print(type(obj))
1234567891011121314151617
```

打印

```python
user
<class '__main__.create_class.<locals>.User'>
12
```

可以动态地创建User或者Student类。

### 2.使用type创建类

查看`type()`函数的源码：

```python
def __init__(cls, what, bases=None, dict=None): # known special case of type.__init__
    """
    type(object_or_name, bases, dict)
    type(object) -> the object's type
    type(name, bases, dict) -> a new type
    # (copied from class doc)
    """
    pass
12345678
```

可知type()函数有两种用法：
（1）`type(object) -> the object's type`：
参数传入一个对象，返回这个对象的类型；
（2）`type(name, bases, dict) -> a new type`：
给定一些参数，返回一个新类型，即创建类：

- 第一个参数：name表示类名称，字符串类型
- 第二个参数：bases表示继承对象（父类），元组类型，单元素使用逗号
- 第三个参数：dict表示属性构成的字典，这里可以填写类属性、类方式、静态方法，采用字典格式，key为属性名，value为属性值

由第二种用法可知，type还可以动态的创建类`type(类名,由父类组成的元组,包含属性的字典)`。
例如

```python
User = type('User',(),{})
obj = User()
print(obj)
123
```

打印

```python
<__main__.User object at 0x000001F17D88AB88>
1
```

向类中添加属性：

```python
User = type('User',(),{'name':'corley'})
obj = User()
print(obj)
print(obj.name)
1234
```

打印

```python
<__main__.User object at 0x00000221F1D06688>
corley
12
```

向类中添加方法：
像添加属性一样添加方法

```python
def info(self):
    return self.name


User = type('User',(),{'name':'corley','info':info})
obj = User()
print(obj.info())
1234567
```

打印

```python
corley
1
```

当方法名改变时，对象实例调用的方法名不需要改变，因为调用方法时调用的是字典里对应的键：

```python
def infos(self):
    return self.name


User = type('User',(),{'name':'corley','info':infos})
obj = User()
print(obj.info())
1234567
```

依然能正常运行，打印

```python
corley
1
```

再如

```python
def infos(self):
    return self.name

def get_age(self):
    self.age = 18
    return self.age

def __init__(self):
    self.sex = 'male'


User = type('User',(),{'name':'corley','info':infos,'age':get_age,'sex':__init__})
obj = User()
print(obj.info())
print(obj.age())
print(obj.sex)
print(obj.sex())
1234567891011121314151617
```

打印

```python
corley
18
<bound method __init__ of <__main__.User object at 0x0000020619AE8408>>
None
1234
```

可知，魔法方法和普通方法不一样，不适合在`type()`创建类时使用。
继承关系：

```python
def infos(self):
    return self.name

def get_age(self):
    self.age = 18
    return self.age


class BaseClass(object):
    def test(self):
        return 'base class'

User = type('User',(BaseClass,),{'name':'corley','info':infos,'age':get_age})
user = User()
print(user.test())
123456789101112131415
```

打印

```python
base class
1
```

再考虑继承多个类：

```python
def infos(self):
    return self.name

def get_age(self):
    self.age = 18
    return self.age


class BaseClass(object):
    def test(self):
        return 'base class'


class BaseClass1(object):
    def test1(self):
        return 'base class1'

User = type('User',(BaseClass,BaseClass1,),{'name':'corley','info':infos,'age':get_age})
user = User()
print(user.test())
print(user.test1())
123456789101112131415161718192021
```

打印

```python
base class
base class1
12
```

魔法方法也可以继承：

```python
class BaseClass(object):
    def test(self):
        return 'base class'

    def __str__(self):
        return 'This is a test'


class BaseClass1(BaseClass):
    def test1(self):
        return 'base class1'

b = BaseClass1()
print(b)
1234567891011121314
```

打印

```python
This is a test
1
```

## 四、metaclass属性

如果一个类中定义了`metalass = xxx`，Python就会用元类的方式来创建类。
在Python2和Python3中不同：
在Python2中：

```python
#把创建的类的所有属性大写
def upper_attr():
    pass

class Foo(object):
    __metaclass__ = upper_attr
123456
```

在Python3中：

```python
#把创建的类的所有属性大写
def upper_attr(class_name, class_parents,class_attr):
    new_attr = {}
    for name,value in class_attr.items():
        print(name)
        if not name.startswith('_'):
            new_attr[name.upper()] = value
    return type(class_name,class_parents,new_attr)

class Foo(object, metaclass=upper_attr):
    name = 'corley'


f = Foo()
print(hasattr(Foo,'name'))
print(hasattr(Foo,'NAME'))
12345678910111213141516
```

打印

```python
__module__
__qualname__
name
False
True
12345
```

显然可得，Foo类有NAME属性，没有name属性；
在类的实例化过程中，**metaclass**是优于object的，所以根据`upper_attr`创建对象的。
进一步验证：

```python
class Demo(object):
    def __new__(cls, *args, **kwargs):
        pass


class MetaClass(type):
    def __new__(cls, *args, **kwargs):
        pass


class User(Demo, metaclass=MetaClass):
    pass


obj = User()
123456789101112131415
```

进行断点测试Debug，结果如下
![元类断点测试](https://img-blog.csdnimg.cn/20200202110942855.gif)
可知，实例化User类时，直接进入MetaClass，没有进入Demo，显然元类的优先级高于继承的父类。
即类进行实例化时，首先寻找metaclass，如果没有再寻找普通的继承关系。

## 五、迭代器和生成器

### 1.迭代器

**迭代**：
通过for循环遍历对象的每一个元素的过程。
Python的for语法功能非常强大，可以遍历任何可迭代的对象。
在Python中，**list**、**tuple**、**string**、**dict**、**set**和**bytes**等都是可以迭代的数据类型。
**迭代器**：
是一种可以被遍历的对象，并且能作用于`next()`函数。迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。
迭代器**只能往后遍历不能回溯**，不像列表，随时可以取后面的数据，也可以返回头取前面的数据。

```python
from collections.abc import Iterable,Iterator

#可迭代
print(isinstance(list(),Iterable))
#迭代器
print(isinstance(list(),Iterator))
123456
```

打印

```python
True
False
12
```

说明列表是可迭代的，但不是迭代器。
**解释**：
列表实现了`__iter__`方法，但是没有实现`__next__`方法。
将列表变成迭代器-调用`iter()`方法：

```python
l = [1,2,3,4]
it = iter(l)
print(it)
print(type(it))
1234
```

打印

```python
<list_iterator object at 0x0000026CA6F2CD48>
<class 'list_iterator'>
12
```

显然，it是迭代器类型。
用`next()`方法来遍历取值：

```python
l = [1,2,3,4]
it = iter(l)
print(it)
print(type(it))
print(next(it))
print(next(it))
print(next(it))
print(next(it))
print(next(it))
123456789
```

打印

```python
<list_iterator object at 0x00000178A8BBD848>
<class 'list_iterator'>
1
2
3
4
Traceback (most recent call last):
  File "xxx/demo.py", line 186, in <module>
    print(next(it))
StopIteration
12345678910
```

易知，当遍历完之后，再遍历，会报错。
还可以用for循环来遍历：

```python
l = [1,2,3,4]
it = iter(l)

for i in it:
    print(i)
12345
```

打印

```python
1
2
3
4
1234
```

### 2.生成器

有时候，序列或集合内的元素的个数非常巨大，如果全制造出来并放入内存，对计算机的压力是非常大的，这时候需要生成器来解决。
从Python2.2起，生成器提供了一种简洁的方式帮助返回列表元素的函数来完成简单和有效的代码。
生成器类似于返回值为数组的一个函数，这个函数可以接受参数，可以被调用，但是，不同于一般的函数会一次性返回包括了所有数值的数组，生成器**一次只能产生一个值**，这样消耗的内存数量将大大减小，而且允许调用函数可以很快的处理前几个返回值，因此生成器看起来像是一个函数，但是表现得却像是迭代器。它基于**yield**指令，允许暂停函数并立即返回结果，此函数保存其执行上下文，如果需要，可立即继续执行。
生成器是一个特殊的程序，可以被用作控制循环的迭代行为，Python中生成器是迭代器的一种，使用yield返回值函数，每次调用yield会暂停，而可以使用`next()`函数和`send()`函数恢复生成器。



```python
g = (x for x in range(10))
print(g)
print(next(g))
print(next(g))
print('for start')
for i in g:
    print(i)
1234567
```

打印

```python
<generator object <genexpr> at 0x00000218AB6137C8>
0
1
for start
2
3
4
5
6
7
8
9
123456789101112
```

生成器的遍历取值是和迭代器类似的。
定义斐波拉契数列：

```python
def fibonacci():
    a = 0
    b = 1
    for i in range(5):
        print(a)
        a, b = b, a + b

fibonacci()
12345678
```

打印

```python
0
1
1
2
3
12345
```

使用生成器：

```python
def fibonacci():
    print('--func start--')
    a = 0
    b = 1
    for i in range(5):
        print('--1--')
        yield a
        print('--2--')
        a, b = b, a + b
        print('--3--')
    print('--func end--')

f = fibonacci()
print(next(f))
print(next(f))
print(next(f))
print(next(f))
print(next(f))
123456789101112131415161718
```

打印

```python
--func start--
--1--
0
--2--
--3--
--1--
1
--2--
--3--
--1--
1
--2--
--3--
--1--
2
--2--
--3--
--1--
3
12345678910111213141516171819
```

由执行结果可知，每调用一次next()函数，会执行到`yield a`部分，打印出当前的a值暂停，直到下一次再执行`next()`函数时，才会执行`yield a`下面的`print('--2--')`和`print('--3--')`；
循环5次后，并没有执行`print('--func end--')`，需要再调用一次`next()`函数才会打印 **–func end–**，但是此时由于生成器已遍历到尾部，所以会报错，如下

```python
def fibonacci():
    print('--func start--')
    a = 0
    b = 1
    for i in range(5):
        print('--1--')
        yield a
        print('--2--')
        a, b = b, a + b
        print('--3--')
    print('--func end--')

f = fibonacci()
print(next(f))
print(next(f))
print(next(f))
print(next(f))
print(next(f))
print(next(f))
12345678910111213141516171819
```

打印

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 210, in <module>
    print(next(f))
StopIteration
--func start--
--1--
0
--2--
--3--
--1--
1
--2--
--3--
--1--
1
--2--
--3--
--1--
2
--2--
--3--
--1--
3
--2--
--3--
--func end--
1234567891011121314151617181920212223242526
```

对改程序进行断点测试Debug，更直观地查看执行过程如下：
![生成器](https://img-blog.csdnimg.cn/20200202111628115.gif)

#### 应用：生成器读取大文件

文件300G,文件比较特殊，只有一行，分隔符是 **{|}**。

```python
def readlines(f,newline):
    buf = ""
    while True:
        while newline in buf:
            pos = buf.index(newline)
            yield buf[:pos]
            buf = buf[pos + len(newline):]
        #每次读取大小
        chunk = f.read(4096*10)
        #读到文件末尾
        if not chunk:
            yield buf
            break
        #拼接字符串
        buf += chunk

with open('demo.txt') as f:
    for line in readlines(f,"{|}"):
        print(line)
12345678910111213141516171819
```

打印

```python
123
abc
987
zyx
1234
```

其中，demo.txt的内容为：

> 123{|}abc{|}987{|}zyx

# Python全栈（四）高级编程技巧之5.Socket编程-基本概念、UDP发送与接收数据

## 一、IP地址介绍和分类

### 1.IP介绍

目的：
用来**标记**网络上的一台电脑或其他设备。
IP传输数据（局域网）的流程如下：
![数据传输流程](https://img-blog.csdnimg.cn/20200202173322897.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
查看IP地址：

- Windows：

```shell
ipconfig
1
```

打印

```shell
Windows IP 配置


无线局域网适配器 本地连接* 1:

   媒体状态  . . . . . . . . . . . . : 媒体已断开连接
   连接特定的 DNS 后缀 . . . . . . . :

无线局域网适配器 本地连接* 12:

   媒体状态  . . . . . . . . . . . . : 媒体已断开连接
   连接特定的 DNS 后缀 . . . . . . . :

以太网适配器 VMware Network Adapter VMnet1:

   连接特定的 DNS 后缀 . . . . . . . :
   本地链接 IPv6 地址. . . . . . . . : fe80::694f:1a98:xxxx:xxxx%x
   自动配置 IPv4 地址  . . . . . . . : 169.254.xxx.xxx
   子网掩码  . . . . . . . . . . . . : 255.255.0.0
   默认网关. . . . . . . . . . . . . :

以太网适配器 VMware Network Adapter VMnet8:

   连接特定的 DNS 后缀 . . . . . . . :
   本地链接 IPv6 地址. . . . . . . . : fe80::d146:2496:xxxx:xxxx%xx
   自动配置 IPv4 地址  . . . . . . . : 169.254.xxx.xxx
   子网掩码  . . . . . . . . . . . . : 255.255.0.0
   默认网关. . . . . . . . . . . . . :

无线局域网适配器 WLAN:

   连接特定的 DNS 后缀 . . . . . . . :
   本地链接 IPv6 地址. . . . . . . . : fe80::f55a:3b0f:xxx:xxx%xx
   IPv4 地址 . . . . . . . . . . . . : 192.168.xxx.xxx
   子网掩码  . . . . . . . . . . . . : 255.255.255.0
   默认网关. . . . . . . . . . . . . : 192.168.0.1
123456789101112131415161718192021222324252627282930313233343536
```

其中，
无线局域网适配器是网卡；
无线局域网适配器 WLAN是本机IP（内网IP），与外网IP如222.215.xxx.xxx不一致；
以太网适配器 VMware Network Adapter VMnet是虚拟机网卡。

- linux（以Ubuntu为例）：

```shell
ifconfig
1
```

打印

```shell
ens33: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether 00:0c:29:69:xx:xx  txqueuelen 1000  (以太网)
        RX packets 6  bytes 552 (552.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ens34: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether 00:0c:29:69:xx:xx  txqueuelen 1000  (以太网)
        RX packets 6  bytes 552 (552.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (本地环回)
        RX packets 343  bytes 25689 (25.6 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 343  bytes 25689 (25.6 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
12345678910111213141516171819202122
```

在Ubuntu中关闭网卡的命令是

```shell
ifconfig ens33 down
1
```

开启的命令是

```shell
ifconfig ens33 up
1
```

### 2.IP地址分类

（1）**根据版本分类**
如图
![IP根据版本分类](https://img-blog.csdnimg.cn/20200202173435620.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
IPv4有256*256*256*256 = 232 = 4294967296个不同可能，现已耗尽；
IPv6有2128个不同可能。
（2）**根据根据网络号和主机号分类**
举例：
192.168.0.161中，
C类：192.168.0作为网络号时，相同表示在同一网络中，161是主机号，范围是0-255，同一网络号下的主机号是唯一的；
B类：192.168作为网络号；
A类：192作为网络号。
如图
![IP地址分类](https://img-blog.csdnimg.cn/20200202173724958.jpg#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 二、端口介绍

### 1.端口简介

端口是为了识别不同的应用程序而分配给不同的应用的。
举例演示QQ、微信发消息的过程如下
![Port](https://img-blog.csdnimg.cn/20200202173859141.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 2.端口分类

- 知名端口：

  范围是从0到1023。

  - 80端口分配给HTTP服务
  - 21端口分配给FTP服务

- 动态端口：
  范围是从1024-65535。

## 三、Socket的简单使用

### 1.TCP/IP协议

TCP/IP协议是**Transmission Control Protocol/Internet Protocol**的简写，即传输控制协议/因特网互联协议，又名网络通讯协议，是Internet最基本的协议、Internet国际互联网络的基础，由网络层的IP协议和传输层的TCP协议组成。
TCP/IP定义了电子设备如何连入因特网，以及数据如何在它们之间传输的标准。协议采用了4层的层级结构，每一层都呼叫它的下一层所提供的协议来完成自己的需求。
TCP/IP网络模型四层模型从根本上和OSI七层网络模型是一样的，只是合并了其中的某些层：
![TCP/IP网络模型四层模型](https://img-blog.csdnimg.cn/20200202174147777.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 2.Socket

socket又称**套接字**,应用程序通常通过**套接字**向网络发出请求或者应答网络请求,使主机间或者一台计算机上的进程间可以通讯。
白话说，socket就是两个节点为了互相通信,而在各自家里装的一部电话。
TCP/IP各层之间传输数据的过程：
如图
![TCP/IP各层之间传输数据](https://img-blog.csdnimg.cn/20200202174454792.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
增加Socket（在应用层和传输层之间）之后传输数据的过程：
如图
![Socket传输数据](https://img-blog.csdnimg.cn/20200202174510551.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
Python中，socket模块的源码：

```python
def __init__(self, family=-1, type=-1, proto=-1, fileno=None):
    # For user code address family and type values are IntEnum members, but
    # for the underlying _socket.socket they're just integers. The
    # constructor of _socket.socket converts the given argument to an
    # integer automatically.
12345
```

参数中
family是协议族，默认**AF_INET**是IPv4，**AF_INET6**是IPv6；
type是套接字类型，包括TCP和UDP两种，默认**SOCK_STREAM**是流式套接字，属于TCP，**SOCK_DGRAM**是数据报套接字，属于UDP。

#### socket的简单使用：

socket的使用过程：

- 创建套接字
- 使用套接字收/发数据
- 关闭套接字

```python
import socket

#创建套接字
s = socket.socket(family=socket.AF_INET,type=socket.SOCK_DGRAM)
#使用套接字收发数据
pass
#关闭套接字
s.close()
12345678
```

过程可类比文件处理。

## 四、UDP发送与接收

### 1.UDP发送数据

```python
import socket

def main():
    #创建UDP套接字
    udp_socket = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
    #发送数据
    udp_socket.sendto('hello',('192.168.0.100',8080))
    #关闭套接字
    udp_socket.close()

if __name__ == '__main__':
    main()
123456789101112
```

打印

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 21, in <module>
    main()
  File "xxx/demo.py", line 16, in main
    udp_socket.sendto('hello',('192.168.0.161',8080))
TypeError: a bytes-like object is required, not 'str'
123456
```

报错，`sendto()`函数参数需要字节型数据，需要对原代码进行改动：

```python
import socket

def main():
    #创建UDP套接字
    udp_socket = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
    #发送数据
    udp_socket.sendto(b'hello',('192.168.0.100',8080))
    #关闭套接字
    udp_socket.close()

if __name__ == '__main__':
    main()
123456789101112
```

此时无打印输出，需要用可视化工具对发送的数据进行显示，这里选择**NetAssist**网络调试助手，可点击https://download.csdn.net/download/CUFEECR/12131323下载。
NetAssist进行配置（协议类型、主机地址和端口号与代码中一致）
![NetAssist配置](https://img-blog.csdnimg.cn/2020020217574171.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
点击**打开**按钮，运行代码，即可看到
![结果1](https://img-blog.csdnimg.cn/2020020217591078.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
代码中参数更改后，要在NetAssist中同步

```python
import socket

def main():
    #创建UDP套接字
    udp_socket = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
    #发送数据
    udp_socket.sendto(b'hello world',('127.0.0.1',8080))
    udp_socket.sendto(b'hello Corley', ('127.0.0.1', 8080))
    #关闭套接字
    udp_socket.close()

if __name__ == '__main__':
    main()
12345678910111213
```

显示
![结果2](https://img-blog.csdnimg.cn/20200202175945656.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
但是发生内容只能是ASCII字符，否则会报错

```python
import socket

def main():
    #创建UDP套接字
    udp_socket = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
    #发送数据
    udp_socket.sendto(b'你好',('localhost',8080))
    #关闭套接字
    udp_socket.close()

if __name__ == '__main__':
    main()
123456789101112
```

打印

```python
  File "xxx/demo.py", line 16
    udp_socket.sendto(b'你好',('localhost',8080))
                     ^
SyntaxError: bytes can only contain ASCII literal characters.
1234
```

需要进行编码

```python
import socket

def main():
    #创建UDP套接字
    udp_socket = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
    #发送数据
    send_data = '你好'
    udp_socket.sendto(send_data.encode('gbk'),('localhost',8080))
    #关闭套接字
    udp_socket.close()

if __name__ == '__main__':
    main()
12345678910111213
```

显示
![结果3](https://img-blog.csdnimg.cn/20200202180003493.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
代码进行优化–不间断发送

```python
import socket

def main():
    # 创建UDP套接字
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    # 循环发送数据
    while True:
        send_data = input('Please enter data to send:')
        if send_data == 'exit':
            break
        send_data = send_data.encode('gbk')
        udp_socket.sendto(send_data,('localhost',8080))
    # 关闭套接字
    udp_socket.close()

if __name__ == '__main__':
    main()
1234567891011121314151617
```

显示
![结果4](https://img-blog.csdnimg.cn/20200202180031697.gif)
发送数据时还可以绑定端口，使发送数据的端口保持不变。

```python
import socket

def main():
    # 创建UDP套接字
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    # 绑定本地信息,不绑定则随机分配
    bind_addr = ('', 7890)  # IP字符串为空时代表本机地址中的任何一个
    udp_socket.bind(bind_addr)
    # 循环发送数据
    while True:
        send_data = input('Please enter data to send:')
        if send_data == 'exit':
            break
        send_data = send_data.encode('gbk')
        udp_socket.sendto(send_data,('192.168.0.100',8080))
    # 关闭套接字
    udp_socket.close()

if __name__ == '__main__':
    main()
1234567891011121314151617181920
```

结果如下
![发送指定端口](https://img-blog.csdnimg.cn/20200202180100200.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
端口为指定端口。

### 2.UDP接收数据

UDP接收数据的过程：

- 创建套接字
- 绑定本地信息(IP和端口)
- 接收数据
- 显示或处理数据
- 关闭套接字

发送数据前，需要对NetAssist进行配置如下
![NetAssist发送配置](https://img-blog.csdnimg.cn/20200202180323611.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
接收数据尝试

```python
import socket

def main():
    # 创建UDP套接字
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    #接收数据
    recv_data = udp_socket.recvfrom(1024) #参数1024表示接收的最大字节数
    print(recv_data)
    # 关闭套接字
    udp_socket.close()

if __name__ == '__main__':
    main()
12345678910111213
```

打印

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 23, in <module>
    main()
  File "xxx/demo.py", line 17, in main
    recv_data = udp_socket.recvfrom(1024) #参数1024表示接收的最大字节数
OSError: [WinError 10022] 提供了一个无效的参数。
123456
```

报错，需要绑定本地信息。
端口绑定问题：
如果程序运行时，没有绑定端口，那么操作系统会自动分配一个端口给程序；
而且同一端口，不能用两次。

```python
import socket

def main():
    # 创建UDP套接字
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    #绑定本地信息
    bind_addr = ('',7789) #IP字符串为空时代表本机地址中的任何一个
    udp_socket.bind(bind_addr)
    #接收数据
    recv_data = udp_socket.recvfrom(1024) #参数1024表示接收的最大字节数
    print(recv_data)
    # 关闭套接字
    udp_socket.close()

if __name__ == '__main__':
    main()
12345678910111213141516
```

运行，然后在NetAssist中发送数据，可得

```python
(b'1', ('192.168.0.100', 8080))
1
```

易知，接收到的数据为元组形式，其中，
第一个值为接收到的数据；
第二个值为(发送方的IP,端口)。
进一步优化代码–实时接收

```python
import socket

def main():
    # 创建UDP套接字
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    #绑定本地信息
    bind_addr = ('',7789) #IP字符串为空时代表本机地址中的任何一个
    udp_socket.bind(bind_addr)
    while True:
        #接收数据
        recv_data = udp_socket.recvfrom(1024) #参数1024表示接收的最大字节数
        data = recv_data[0]
        send_addr = recv_data[1]
        print('%s:%s'%(str(send_addr),data.decode('gbk')))
    # 关闭套接字
    udp_socket.close()

if __name__ == '__main__':
    main()
12345678910111213141516171819
```

显示
![结果6](https://img-blog.csdnimg.cn/20200202182313789.gif)
如果`recvfrom()`的参数指定过小,超过发送的数据的大小,则会报错

```python
import socket

def main():
    # 创建UDP套接字
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    #接收数据
    recv_data = udp_socket.recvfrom(1024) #参数1024表示接收的最大字节数
    print(recv_data)
    # 关闭套接字
    udp_socket.close()

if __name__ == '__main__':
    main()
12345678910111213
```

打印

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 46, in <module>
    main()
  File "xxx/demo.py", line 40, in main
    recv_data = udp_socket.recvfrom(10) #参数10表示接收的最大字节数
OSError: [WinError 10040] 一个在数据报套接字上发送的消息大于内部消息缓冲区或其他一些网络限制，或该用户用于接收数据报的缓冲区比数据报小。
123456
```

### 3.应用–生成UDP聊天器

步骤：

- 创建套接字 套接字是可以同时收发数据的
- 发送数据
- 接收数据

```python
import socket


def send_data(udp_socket):
    '''发送数据'''
    send_data = input('Please enter data to send:')
    send_data = send_data.encode('gbk')
    dest_ip = input('Please enter destination IP:')
    dest_port = int(input('Please enter destination PORT:'))
    udp_socket.sendto(send_data,(dest_ip,dest_port))

def recv_data(udp_socket):
    '''接收数据'''
    recv_data = udp_socket.recvfrom(1024)
    data = recv_data[0]
    send_addr = recv_data[1]
    print('%s:%s' % (str(send_addr), data.decode('gbk')))

def main():
    # 创建UDP套接字，同时接收、发送数据
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    # 绑定本地信息
    bind_addr = ('', 7788)  # IP字符串为空时代表本机地址中的任何一个
    udp_socket.bind(bind_addr)
    # 循环发送、接收数据
    while True:
        #发送
        send_data(udp_socket)
        #接收
        recv_data(udp_socket)
    # 关闭套接字
    udp_socket.close()


if __name__ == '__main__':
    main()
123456789101112131415161718192021222324252627282930313233343536
```

显示
![结果7](https://img-blog.csdnimg.cn/20200202182337349.gif#pic_center)
提示：
此时要关闭NetAssist，否则可能会因占用端口而发生程序不能正常向下执行的情况。

# Python全栈（四）高级编程技巧之6.Socket编程-TCP客户端和服务端

## 一、TCP介绍

### 1.概念

TCP协议，即传输控制协议，是一种面向连接的、可靠的、基于字节流的传输层通信协议。
TCP通信需要经过**创建连接**、**数据传送**、**终止连接**三个步骤。
TCP通信模型中，在通信开始之前，一定要先建立相关连接，才能发送数据。

### 2.TCP特点：

- 面向连接：
  通信双方必须先建立连接才能进行数据的传输。
- 可靠传输：
  - TCP采用发送应答机制
  - 超时重传：
    启动定时器，在规定时间内未收到，则重新发送，直到收到。
  - 错误校验
  - 流量控制和阻塞管理

### 3.TCP和UDP的比较

- TCP面向连接;UDP是无连接的，即发送数据之前不需要建立连接.
- TCP提供可靠的服务。也就是说，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达;UDP尽最大努力交付，即不保证可靠交付.
- UDP具有较好的实时性，工作效率比TCP高，适用于对高速传输和实时性有较高的通信或广播通信。
- 每一条TCP连接只能是点到点的;UDP支持一对一，一对多，多对一和多对多的交互通信。
- TCP对系统资源要求较多，UDP对系统资源要求较少。
- TCP严格区分客户端和服务端，UDP不一定区分。

UDP通信和TCP通信的过程如下：
**UDP通信**：
![UDP通信](https://img-blog.csdnimg.cn/20200203151013379.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
**TCP通信**：
![TCP通信](https://img-blog.csdnimg.cn/20200203151038427.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
形象地说：
UCP类比于写信，别人不一定能收到；
TCP类比于打电话，可以获知对方是否收到。

## 二、TCP客户端构建

客户端：是需要被服务的一方；
服务器端：就是提供服务的一方。
TCP客户端构建流程

- 创建socket
- 连接服务器
- 收发数据(最大接收2014个字节)
- 关闭套接字

NetAssist进行配置如下
![客户端配置](https://img-blog.csdnimg.cn/20200203151206783.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
此时，NetAssist作为服务端，Python运行作为客户端。

```python
import socket


def main():
    #创建TCP套接字
    tcp_client = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    #连接服务器
    server_ip = input('Server IP:')
    server_port = int(input('Server Port:'))
    tcp_client.connect((server_ip,server_port))
    #接收数据
    recv_data = tcp_client.recv(1024)
    print(recv_data)
    #关闭套接字
    tcp_client.close()

if __name__ == '__main__':
    main()
123456789101112131415161718
```

显示
![结果1](https://img-blog.csdnimg.cn/2020020315154239.gif#pic_center)
数据进行解码：

```python
import socket


def main():
    #创建TCP套接字
    tcp_client = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    #连接服务器
    server_ip = input('Server IP:')
    server_port = int(input('Server Port:'))
    tcp_client.connect((server_ip,server_port))
    #接收数据
    recv_data = tcp_client.recv(1024).decode('gbk')
    print(recv_data)
    #关闭套接字
    tcp_client.close()

if __name__ == '__main__':
    main()
123456789101112131415161718
```

显示
![结果 2](https://img-blog.csdnimg.cn/20200203151621728.gif#pic_center)
增加发送数据：

```python
import socket


def main():
    #创建TCP套接字
    tcp_client = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    #连接服务器
    server_ip = input('Server IP:')
    server_port = int(input('Server Port:'))
    tcp_client.connect((server_ip,server_port))
    #发送数据
    send_data = input('Data to send:')
    tcp_client.send(send_data.encode('gbk'))
    #接收数据
    recv_data = tcp_client.recv(1024).decode('gbk')
    print(recv_data)
    #关闭套接字
    tcp_client.close()

if __name__ == '__main__':
    main()
123456789101112131415161718192021
```

显示
![结果3](https://img-blog.csdnimg.cn/20200203151703672.gif#pic_center)

## 三、TCP服务端构建流程

TCP服务端构建过程：

- socket创建套接字
- bind绑定IP和port
- listen使套接字变为可以被动连接
- accept等待客户端的连接
- recv/send接收发送数据
- 关闭套接字

NetAssist进行配置如下
![服务器端配置](https://img-blog.csdnimg.cn/20200203151852333.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
此时，NetAssist作为客户端，Python运行作为服务器端；
在代码未运行时，NetAssist点击连接按钮连接不了，因为此时服务器端还未开启，应该先运行服务器端、再连接。

```python
import socket


def main():
    #创建TCP监听套接字
    tcp_server = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    #绑定信息
    tcp_server.bind(('',7890))
    #默认套接字主动变被动
    tcp_server.listen(128)
    #等待，创建服务套接字
    new_client_socket,client_addr = tcp_server.accept()
    print(client_addr)


if __name__ == '__main__':
    main()
1234567891011121314151617
```

显示
![结果4](https://img-blog.csdnimg.cn/20200203151933615.gif#pic_center)
TCP监听套接字负责等待新的客户端连接；
产生的服务套接字负责收发数据。
进一步完善代码：

```python
import socket


def main():
    #创建TCP监听套接字
    tcp_server = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    #绑定信息
    tcp_server.bind(('',7890))
    #默认套接字主动变被动
    tcp_server.listen(128)
    #等待，创建服务套接字
    new_client_socket,client_addr = tcp_server.accept()
    print(client_addr)
    #接收数据
    recv_data = new_client_socket.recv(1024)
    print(recv_data.decode('gbk'))
    #发送数据
    send_data = input('Data to send:').encode('gbk')
    new_client_socket.send(send_data)
    new_client_socket.close()
    tcp_server.close()


if __name__ == '__main__':
    main()
12345678910111213141516171819202122232425
```

显示
![结果5](https://img-blog.csdnimg.cn/20200203152015751.gif#pic_center)

## 四、TCP服务端为多个客户端服务

### 1.代码分析

服务器端代码`tcp_server.accept()`部分会阻塞，阻塞直到有客户端连接；
`new_client_socket.recv()`部分会阻塞，直到客户端有数据发送或者客户端断开连接解堵塞。

### 2.多客户端实现

```python
import socket


def main():
    # 创建TCP监听套接字
    tcp_server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 绑定信息
    tcp_server.bind(('', 7890))
    #为多个客户端服务
    while True:
        # 默认套接字主动变被动
        tcp_server.listen(128)
        # 等待，创建服务套接字
        new_client_socket, client_addr = tcp_server.accept()
        print(client_addr)
        # 接收数据
        recv_data = new_client_socket.recv(1024)
        print(recv_data.decode('gbk'))
        # 发送数据
        send_str = 'Hello, ' +client_addr[0]
        send_data = send_str.encode('gbk')
        new_client_socket.send(send_data)
        new_client_socket.close()
    tcp_server.close()


if __name__ == '__main__':
    main()
12345678910111213141516171819202122232425262728
```

显示
![结果6](https://img-blog.csdnimg.cn/20200203152115681.gif#pic_center)
显然，此时可以连接多个客户端，但是每个客户端只能发送一次数据，再发送会产生异常，自动断开连接，要想多次服务，还需要进一步改进：

```python
import socket


def main():
    # 创建TCP监听套接字
    tcp_server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 绑定信息
    tcp_server.bind(('', 7890))
    #为多个客户端服务
    while True:
        # 默认套接字主动变被动
        tcp_server.listen(128)
        # 等待，创建服务套接字
        new_client_socket, client_addr = tcp_server.accept()
        print(client_addr)
        #为客户端提供多次服务
        while True:
            # 接收数据
            recv_data = new_client_socket.recv(1024)
            # 发送数据
            if recv_data.decode('gbk'):
                print(recv_data.decode('gbk'))
                send_str = 'Hello, ' +client_addr[0]
                send_data = send_str.encode('gbk')
                new_client_socket.send(send_data)
            #无数据，退出循环，关闭连接
            else:
                break
        new_client_socket.close()
    tcp_server.close()


if __name__ == '__main__':
    main()
12345678910111213141516171819202122232425262728293031323334
```

显示
![结果7](https://img-blog.csdnimg.cn/20200203152156754.gif#pic_center)
易知，可以实现为多个客户端提供服务，但是一次只能为一个客户端提供服务，不能同时为多个客户端服务。

## 五、应用–文件下载器

**TCP客户端实现思路**：

- 创建套接字
- 连接服务器
- 获取下载文件名称
- 发送下载文件下载请求
- 接收服务端发送过来的数据
- 保存文件
- 关闭套接字

**tcp_client.py**：

```python
import socket


def main():
    #创建套接字
    tcp_client_socket = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    # 连接服务器
    server_ip = input('Server IP:')
    server_port = int(input('Server Port:'))
    tcp_client_socket.connect((server_ip,server_port))
    #获取下载文件名称
    file_name = input('Input File Name:')
    #发送文件下载请求
    tcp_client_socket.send(file_name.encode('gbk'))
    #接收文件
    recv_data = tcp_client_socket.recv(1024*1024)
    #保存文件
    with open('Received ' + file_name,'wb') as f:
        f.write(recv_data)
    #关闭套接字
    tcp_client_socket.close()

if __name__ == '__main__':
    main()
123456789101112131415161718192021222324
```

用NetAssist作服务器端，进行测试，结果
![结果8](https://img-blog.csdnimg.cn/20200203152232755.gif#pic_center)
**TCP服务器端实现思路**：

- 创建套接字
- 绑定信息
- 主动变被动
- 等待客户端连接
- 收发数据
- 关闭套接字

**tcp_server.py**：

```python
import socket


def send_file_to_client(new_client_socket):
    file_name = new_client_socket.recv(1024).decode('gbk')
    try:
        with open(file_name,'rb') as f:
            content = f.read()
            new_client_socket.send(content)
    except:
        print('No such File:%s' % file_name)
        return


def main():
    # 创建TCP监听套接字
    tcp_server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 绑定信息
    tcp_server.bind(('', 7890))
    # 默认套接字主动变被动
    tcp_server.listen(128)
    # 等待，创建服务套接字
    new_client_socket, client_addr = tcp_server.accept()
    print(client_addr)
    # 给客户端返回文件内容
    send_file_to_client(new_client_socket)
    #关闭客户端
    new_client_socket.close()
    tcp_server.close()


if __name__ == '__main__':
    main()
123456789101112131415161718192021222324252627282930313233
```

服务器端和客户端运行起来，可得结果：
![结果9-1](https://img-blog.csdnimg.cn/20200203152317783.gif#pic_center)
显示生成新文件的动态过程：
![结果9-2](https://img-blog.csdnimg.cn/20200203152400515.gif#pic_center)
易知，运行并连接后，在客户端输入文件名，服务器端读写本地文件并发给客户端进行保存生成**Received test.py**文件。

# Python全栈（四）高级编程技巧之7.Python多任务-线程的介绍和使用

## 一、多任务的介绍

有很多的场景中的事情是同时进行的，比如开车的时候手和脚共同来驾驶汽车，再比如唱歌跳舞也是同时进行的。
程序中模拟多任务：

```python
import time


def sing():
    for i in range(3):
        print('Singing for %d' % i)
        time.sleep(1)


def dance():
    for i in range(3):
        print('Dancing for %d' % i)
        time.sleep(1)


if __name__ == '__main__':
    sing()
    dance()
123456789101112131415161718
```

显示
![result1](https://img-blog.csdnimg.cn/20200204122302871.gif#pic_center)
整个代码执行时间为6秒，两个函数执行是按照先后顺序执行的，不能实现同时执行，此时需要多任务，可以使多个任务同时进行。
对多任务的理解：
![多任务](https://img-blog.csdnimg.cn/20200204122412945.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
多任务是指多个任务**同时**进行。
在实际应用中，多个应用“同时”运行时，并不是真正地同时运行，而是按照**时间片轮转**的原理进行的。

- 并发：
  假的多任务，CPU数小于当前执行的任务数；
- 并行：
  真的多任务，CPU数大于等于当前执行的任务数。

线程举例说明：

```python
import threading
import time


def demo():
    print('Hello World')
    time.sleep(1)


def main():
    for i in range(5):
        t = threading.Thread(target=demo)
        t.start()


if __name__ == '__main__':
    main()
1234567891011121314151617
```

显示
![result2](https://img-blog.csdnimg.cn/20200204122510786.gif#pic_center)
其中，`main()`函数中为主线程，`demo()`函数为子线程；
**target**参数是需要执行的任务。
对前一段代码进行修改：

```python
import time
import threading


def sing():
    for i in range(3):
        print('Singing for %d' % i)
        time.sleep(1)


def dance():
    for i in range(3):
        print('Dancing for %d' % i)
        time.sleep(1)


if __name__ == '__main__':
    t1 = threading.Thread(target=sing)
    t2 = threading.Thread(target=dance)
    t1.start()
    t2.start()
123456789101112131415161718192021
```

显示
![result3](https://img-blog.csdnimg.cn/20200204122558199.gif#pic_center)
显然，此时，`sing()`和`dance()`两个函数是同时执行的，此即为多任务。
在代码中，t1和t2两个线程是同时执行、不分先后顺序的。
子线程结束之后主线程才会结束，下面进行验证：

```python
import threading
import time


def demo():
    for i in range(5):
        print('Hello World')
        time.sleep(1)


def main():
    t = threading.Thread(target=demo)
    t.start()
    print('Main Thread')


if __name__ == '__main__':
    main()
123456789101112131415161718
```

显示：
![result4](https://img-blog.csdnimg.cn/20200204122632549.gif#pic_center)
显然，主线程在等待子线程执行结束再结束。
要想主线程直接执行结束，可增加**守护线程**：

```python
import threading
import time


def demo():
    for i in range(5):
        print('Hello World')
        time.sleep(1)


def main():
    t = threading.Thread(target=demo)
    #守护线程
    t.setDaemon(True)
    t.start()
    print('Main Thread')


if __name__ == '__main__':
    main()
1234567891011121314151617181920
```

显示
![result5](https://img-blog.csdnimg.cn/20200204122651571.gif#pic_center)
显然，只打印了一次**Hello World**，主线程并没有等子线程执行结束再结束，而是直接执行结束。
守护线程不会等子线程结束，而是直接结束。
进一步改进–主线程等待子线程结束之后再继续运行：

```python
import threading
import time


def demo():
    for i in range(5):
        print('Hello World')
        time.sleep(1)


def main():
    t = threading.Thread(target=demo)
    t.start()
    # 等待子线程执行结束，主线程继续向下运行
    t.join()
    print('Main Thread')


if __name__ == '__main__':
    main()
1234567891011121314151617181920
```

显示：
![result6](https://img-blog.csdnimg.cn/20200204122713355.gif#pic_center)
显然，主线程等待子线程打印完全部**Hello World**之后才打印**Main Thread**。
并且，子线程还可以创建子线程

```python
import threading
import time

def demo1():
    print('In demo1')


def demo():
    for i in range(5):
        print('Hello World')
        time.sleep(1)
        t2 = threading.Thread(target=demo1)
        t2.start()


def main():
    t = threading.Thread(target=demo)
    t.start()
    # 等待子线程执行结束，主线程继续向下运行
    t.join()
    print('Main Thread')


if __name__ == '__main__':
    main()
12345678910111213141516171819202122232425
```

显示：
![result7](https://img-blog.csdnimg.cn/20200204122731810.gif#pic_center)
易知，子线程的子线程也能正常运行，并且穿插在子线程的运行过程中运行。

## 二、查看线程数量

在基础Python中，`enumerate()`函数可以为列表自动创建索引：

```python
l = ['a','b','c']
for i,item in enumerate(l):
    print(i,item)
123
```

打印

```python
0 a
1 b
2 c
123
```

在多任务中，`enumerate()`函数可以查看当前线程的数量：

```python
import threading


def demo1():
    for i in range(5):
        print('--demo1-- %d' % i)


def demo2():
    for i in range(5):
        print('--demo2-- %d' % i)


def main():
    t1 = threading.Thread(target=demo1)
    t2 = threading.Thread(target=demo2)
    t1.start()
    t2.start()
    #获取当前程序所有的线程
    print(threading.enumerate())


if __name__ == '__main__':
    main()
123456789101112131415161718192021222324
```

打印

```python
--demo1-- 0
--demo1-- 1
--demo1-- 2
--demo1-- 3
--demo1-- 4
--demo2-- 0
--demo2-- 1
[<_MainThread(MainThread, started 23280)>, <Thread(Thread-2, started 27600)>]
--demo2-- 2
--demo2-- 3
--demo2-- 4
1234567891011
```

易知，有两个线程：主线程和线程2。
但是再运行的结果可能是

```python
--demo1-- 0
--demo1-- 1
--demo1-- 2
--demo1-- 3
--demo1-- 4
--demo2-- 0
--demo2-- 1
--demo2-- 2
--demo2-- 3
--demo2-- 4
[<_MainThread(MainThread, started 10816)>]
1234567891011
```

此时，只有主线程一个线程。
**解释**：
线程运行是没有先后顺序的，谁先抢占到内存资源就会先执行，所以线程数是变化的。
进一步改变代码：

```python
import threading
import time


def demo1():
    for i in range(5):
        print('--demo1-- %d' % i)
        time.sleep(0.5)


def demo2():
    for i in range(10):
        print('--demo2-- %d' % i)
        time.sleep(0.5)


def main():
    t1 = threading.Thread(target=demo1)
    t2 = threading.Thread(target=demo2)
    t1.start()
    t2.start()
    # 获取当前程序所有的线程
    while True:
        print(threading.enumerate())
        if len(threading.enumerate()) <= 1:
            break
        time.sleep(1)


if __name__ == '__main__':
    main()
12345678910111213141516171819202122232425262728293031
```

显示：
![result8](https://img-blog.csdnimg.cn/20200204122819343.gif#pic_center)
易知，开始时3个线程，执行5次后`demo1()`线程执行结束，还有两个线程，10次后`demo2()`也结束，只有主线程1个线程，while循环结束，主线程也结束。

## 三、验证子线程的创建与执行

当调用Thread的时候，不会创建线程；
当调用Thread创建出来的实例对象的`start()`方法的时候，才会创建线程以及开始运行这个线程。

```python
import threading


def demo():
    for i in range(5):
        print('demo')


def main():
    print(threading.enumerate())
    t1 = threading.Thread(target=demo)
    print(threading.enumerate())
    t1.start()
    print(threading.enumerate())


if __name__ == '__main__':
    main()
123456789101112131415161718
```

其中一次执行结果为：

```python
[<_MainThread(MainThread, started 28344)>]
[<_MainThread(MainThread, started 28344)>]
demo
demo
demo
[<_MainThread(MainThread, started 28344)>, <Thread(Thread-1, started 30844)>]
demo
demo
12345678
```

显然，可得，在调用`Thread()`时不会创建子线程，只有在调用`start()`方法后才会创建线程并执行。

## 四、继承Thread类创建线程

继承类创建线程尝试：

```python
import threading
import time


class A(object):
    def demo(self):
        for i in range(5):
            print('demo')
            time.sleep(0.5)


class B(object):
    def demo1(self):
        for i in range(5):
            print('demo1')
            time.sleep(0.5)


if __name__ == '__main__':
    t1 = threading.Thread(target=A().demo)
    t2 = threading.Thread(target=B().demo1)
    t1.start()
    t2.start()
1234567891011121314151617181920212223
```

打印

```python
demo
demo1
demo1
demo
demo1
demo
demo1
demo
demo1
demo
12345678910
```

虽然运用了类，但是与之前直接调用方法无本质区别，我们需要做的是继承自Thread类来创建线程。

```python
import threading


class A(threading.Thread):
    # 该方法必须要重写且不能变方法名
    def run(self):
        for i in range(5):
            print(i)


if __name__ == '__main__':
    t1 = A()
    t1.start()
12345678910111213
```

打印

```python
0
1
2
3
4
12345
```

类继承自Thread时，`run()`方法必须要重写，并且不能改名字。
验证：

```python
import threading


class A(threading.Thread):
    def run1(self):
        for i in range(5):
            print(i)

    def demo(self):
        for i in range(5):
            print(i)


if __name__ == '__main__':
    t1 = A()
    t1.start()
12345678910111213141516
```

运行改代码，无输出结果，不能实现预期的效果，因此必须要有`run()`方法。

## 五、线程间通信和传参

### 1.线程间通信

多线程共享全局变量又称为线程间通信。
问题：
在函数内部修改全局变量必须要声明global吗？

```python
num = 100
li = [11,22]


def demo1():
    global num
    num += 100


def demo2():
    li.append(33)


def demo3():
    try:
        li += [44]
    except:
        print('error')


def demo4():
    global li
    li += [55]


print(num)
demo1()
print(num)

print(li)
demo2()
print(li)

print(li)
demo3()
print(li)

print(li)
demo4()
print(li)
12345678910111213141516171819202122232425262728293031323334353637383940
```

打印

```python
100
200
[11, 22]
[11, 22, 33]
[11, 22, 33]
error
[11, 22, 33]
[11, 22, 33]
[11, 22, 33, 55]
123456789
```

易知，数值型变量要想在函数内部改变，必须声明global；
列表在函数内部通过`append()`函数改变时，可以不声明global，但是要想通过直接相加的方式改变，必须要声明global。
解释：
列表通过直接相加的方式会改变原列表的指向，即原列表增加元素后会成为新的列表；
通过`append()`方法改变列表的元素后不改变原列表的指向，即还是原来的列表对象。
从而可得：
在函数中是否需要加global来改变全局变量，需要看该变量的指向是否改变，如果改变，则需要加global，反之，如果未改变指向、仅仅是修改了指向的空间中的数据，则不需要加global。
从而也就回答了问题：
在函数内部修改全局变量不一定要声明global，要看是否对全局变量的指向进行了修改。

```python
import threading
import time


num = 100


def demo1():
    global num
    num += 1
    print('demo1---%d' % num)


def demo2():
    print('demo2---%d' % num)


def main():
    t1 = threading.Thread(target=demo1)
    t2 = threading.Thread(target=demo2)
    t1.start()
    time.sleep(1)
    t2.start()
    time.sleep(1)
    print('main---%d' % num)


if __name__ == '__main__':
    main()
1234567891011121314151617181920212223242526272829
```

显示：
![result9](https://img-blog.csdnimg.cn/20200204122856236.gif#pic_center)
显然，三次打印都是101。
**解释**：
在t1开始后，`time.sleep(1)`执行，所以不继续向下执行，而是执行`demo1()`中的代码，由于num声明了global，使num自增1，变为101，所以打印101；
1秒之后，执行`demo2()`，此时num已经变为101，所以会打印出101；
在过1秒后，`main()`函数中，打印出num为101。
可知，多线程是共享全局变量的，在一个线程改变的全局变量在其他线程中也能`同步使用`。

### 2.多线程参数args

```python
import threading
import time


num = [11,22]


def demo1(num):
    num.append(33)
    print('demo1---%s' % str(num))


def demo2(num):
    print('demo2---%s' % str(num))


def main():
    t1 = threading.Thread(target=demo1, args=(num,))
    t2 = threading.Thread(target=demo2, args=(num,))
    t1.start()
    time.sleep(1)
    t2.start()
    time.sleep(1)
    print('main---%s' % str(num))


if __name__ == '__main__':
    main()
12345678910111213141516171819202122232425262728
```

打印

```python
demo1---[11, 22, 33]
demo2---[11, 22, 33]
main---[11, 22, 33]
```

# Python全栈（四）高级编程技巧之8.Python多任务-线程(下)和进程介绍

## 一、共享全局变量资源竞争

一个线程写入、一个线程读取没问题，如果有两个线程对某一资源同时写入时，就可能会产生**资源竞争**。

```python
import threading


num = 0


def demo1(count):
    global num
    for i in range(count):
        num += 1
    print('demo1--%d' % num)


def demo2(count):
    global num
    for i in range(count):
        num += 1
    print('demo2--%d' % num)


def main():
    t1 = threading.Thread(target=demo1, args=(100,))
    t2 = threading.Thread(target=demo2, args=(100,))
    t1.start()
    t2.start()
    print('main--%d' % num)


if __name__ == '__main__':
    main()
123456789101112131415161718192021222324252627282930
```

打印

```python
demo1--100
demo2--200
main--200
123
```

当参数进一步扩大时：

```python
import threading


num = 0


def demo1(count):
    global num
    for i in range(count):
        num += 1
    print('demo1--%d' % num)


def demo2(count):
    global num
    for i in range(count):
        num += 1
    print('demo2--%d' % num)


def main():
    t1 = threading.Thread(target=demo1, args=(1000000,))
    t2 = threading.Thread(target=demo2, args=(1000000,))
    t1.start()
    t2.start()
    print('main--%d' % num)


if __name__ == '__main__':
    main()
123456789101112131415161718192021222324252627282930
```

打印

```python
main--181091
demo2--1209597
demo1--1410717
123
```

解释：
num为全局变量，最开始为0，在`demo1()`、`demo2()`两个子线程中有for循环，在每个子线程的循环中：
（1）先获取num的值；
（2）再把获取到的num加1；
（3）并把结果保存到num。
当执行时，在一个子线程执行到第（2）步时，还未保存，此时轮转到另一个子线程，此时由于第一个子线程还未执行到第（3）步，为保存num，所以num的值为0，执行到第（2）步，也加1，此时回到第一个子线程执行第（3）步保存num，为1，再到第二个子线程执行第三步也保存，所以也为1。
上述现象是一个概率性问题，当循环次数越多时，发生的概率越大，因此循环次数为100时，未发生此现象，循环次数为1000000时，有较明显的现象。
此时即发生了**资源竞争**。
对执行过程进行验证（查看字节码）：

```python
import dis


def add_num(a):
    a += 1


print(dis.dis(add_num))
12345678
```

打印

```python
 36           0 LOAD_FAST                0 (a)
              2 LOAD_CONST               1 (1)
              4 INPLACE_ADD
              6 STORE_FAST               0 (a)
              8 LOAD_CONST               0 (None)
             10 RETURN_VALUE
None
1234567
```

说明：
（1）**LOAD_FAST**：加载a；
（2）**LOAD_CONST**：加载常量值1；
（3）**INPLACE_ADD**：执行add加法；
（4）**STORE_FAST**：赋值给a。
显然，这个过程与之前解释资源竞争时子线程执行时的过程类似。

## 二、互斥锁和死锁

### 1.互斥锁

当多个线程几乎同时修改某一个共享数据的时候，需要进行同步控制；某个线程要更改共享数据时，先将其锁定，此时资源的状态为**锁定**，其他线程不能改变，只到该线程释放资源，将资源的状态变成**非锁定**，其他的线程才能再次锁定该资源。
互斥锁保证了每次只有一个线程进行写入操作，从而保证了多线程情况下**数据的正确性**。

```python
import threading


num = 0
# 创建一个互斥锁
mutex = threading.Lock()


def demo1(count):
    global num
    # 加锁
    mutex.acquire()
    for i in range(count):
        num += 1
    # 解锁
    mutex.release()
    print('demo1--%d' % num)


def demo2(count):
    global num
    # 加锁
    mutex.acquire()
    for i in range(count):
        num += 1
    # 解锁
    mutex.release()
    print('demo2--%d' % num)


def main():
    t1 = threading.Thread(target=demo1, args=(1000000,))
    t2 = threading.Thread(target=demo2, args=(1000000,))
    t1.start()
    t2.start()
    print('main--%d' % num)


if __name__ == '__main__':
    main()
12345678910111213141516171819202122232425262728293031323334353637383940
```

打印

```python
main--166589
demo1--1000000
demo2--2000000
123
```

此时，子线程1和子线程2的打印正常，但是主线程的打印结果仍然无规律。
**解释**：
执行t1.start()和t2.start()后，两个子线程运行，此时会继续向下执行，主线程会打印此时num的值，由于子线程仍可能未运行结束，即num的值未完成相加到2000000，所以会最先打印main，等到子线程1执行完打印1000000，释放锁，再运行子线程2，打印2000000。
进一步改进：

```python
import threading
import time


num = 0
# 创建一个互斥锁
mutex = threading.Lock()


def demo1(count):
    global num
    # 加锁
    mutex.acquire()
    for i in range(count):
        num += 1
    # 解锁
    mutex.release()
    print('demo1--%d' % num)


def demo2(count):
    global num
    # 加锁
    mutex.acquire()
    for i in range(count):
        num += 1
    # 解锁
    mutex.release()
    print('demo2--%d' % num)


def main():
    t1 = threading.Thread(target=demo1, args=(1000000,))
    t2 = threading.Thread(target=demo2, args=(1000000,))
    t1.start()
    t2.start()
    time.sleep(2)
    print('main--%d' % num)


if __name__ == '__main__':
    main()
123456789101112131415161718192021222324252627282930313233343536373839404142
```

打印

```python
demo1--1000000
demo2--2000000
main--2000000
123
```

此时，执行`t1.start()`和`t2.start()`后暂停2秒，足够子线程1和子线程2执行完毕，所以先打印，最后主线程打印出num最后的值2000000。

### 2.死锁

在线程间共享多个资源的时候，如果两个线程分别占有一部分资源并且同时等待对方的资源，就会造成死锁。

```python
import threading
import time

class MyThread1(threading.Thread):
    def run(self):
        # 对mutexA上锁
        mutexA.acquire()

        # mutexA上锁后，延时1秒，等待另外那个线程 把mutexB上锁
        print(self.name+'----do1---up----')
        time.sleep(1)

        # 此时会堵塞，因为这个mutexB已经被另外的线程抢先上锁了
        mutexB.acquire()
        print(self.name+'----do1---down----')
        mutexB.release()

        # 对mutexA解锁
        mutexA.release()

class MyThread2(threading.Thread):
    def run(self):
        # 对mutexB上锁
        mutexB.acquire()

        # mutexB上锁后，延时1秒，等待另外那个线程 把mutexA上锁
        print(self.name+'----do2---up----')
        time.sleep(1)

        # 此时会堵塞，因为这个mutexA已经被另外的线程抢先上锁了
        mutexA.acquire()
        print(self.name+'----do2---down----')
        mutexA.release()

        # 对mutexB解锁
        mutexB.release()

mutexA = threading.Lock()
mutexB = threading.Lock()

if __name__ == '__main__':
    t1 = MyThread1()
    t2 = MyThread2()
    t1.start()
    t2.start()
123456789101112131415161718192021222324252627282930313233343536373839404142434445
```

显示：
![result1](https://img-blog.csdnimg.cn/2020020419514510.gif#pic_center)
显然， 两个线程陷入相互等待的死锁，程序不能正常运行，直到手动停止程序或占满内存程序崩溃。

**避免死锁**：

- 程序设计时要尽量避免
- 添加超时时间

同时，创建的锁在加锁之后只能在释放之后才能再次加锁，否则也会陷入死锁等待，如下

```python
import threading
import time


num = 0
# 创建一个互斥锁
mutex = threading.Lock()


def demo1(count):
    global num
    # 加锁
    mutex.acquire()
    mutex.acquire()
    for i in range(count):
        num += 1
    # 解锁
    mutex.release()
    mutex.acquire()
    print('demo1--%d' % num)


def demo2(count):
    global num
    # 加锁
    mutex.acquire()
    for i in range(count):
        num += 1
    # 解锁
    mutex.release()
    print('demo2--%d' % num)


def main():
    t1 = threading.Thread(target=demo1, args=(100,))
    t2 = threading.Thread(target=demo2, args=(100,))
    t1.start()
    t2.start()
    time.sleep(2)
    print('main--%d' % num)


if __name__ == '__main__':
    main()
1234567891011121314151617181920212223242526272829303132333435363738394041424344
```

显示：
![result2](https://img-blog.csdnimg.cn/20200204195422346.gif#pic_center)
易知，主线程一直在等待子线程执行结束，但是子线程`demo1()`两次枷锁、陷入死锁。
要想**重用锁对象**，需要使用**RLock**：

```python
import threading
import time


num = 0
# 创建一个可重入锁
mutex = threading.RLock()


def demo1(count):
    global num
    # 加锁
    mutex.acquire()
    mutex.acquire()
    for i in range(count):
        num += 1
    # 解锁
    mutex.release()
    mutex.release()
    print('demo1--%d' % num)


def demo2(count):
    global num
    # 加锁
    mutex.acquire()
    for i in range(count):
        num += 1
    # 解锁
    mutex.release()
    print('demo2--%d' % num)


def main():
    t1 = threading.Thread(target=demo1, args=(100,))
    t2 = threading.Thread(target=demo2, args=(100,))
    t1.start()
    t2.start()
    time.sleep(2)
    print('main--%d' % num)


if __name__ == '__main__':
    main()
1234567891011121314151617181920212223242526272829303132333435363738394041424344
```

显示：
![result3](https://img-blog.csdnimg.cn/2020020419544216.gif#pic_center)

#### 死锁扩展–银行家算法

**背景知识**：
一个银行家如何将一定数目的资金安全地借给若干个客户，使这些客户既能借到钱完成要干的事，同时银行家又能收回全部资金而不至于破产，这就是银行家问题。这个问题同操作系统中资源分配问题十分相似：银行家就像一个操作系统，客户就像运行的进程，银行家的资金就是系统的资源。
**问题描述**：
一个银行家拥有一定数量的资金，有若干个客户要贷款。每个客户须在一开始就声明他所需贷款的总额。若该客户贷款总额不超过银行家的资金总数，银行家可以接收客户的要求。客户贷款是以每次一个资金单位（如1万RMB等）的方式进行的，客户在借满所需的全部单位款额之前可能会等待，但银行家须保证这种等待是有限的，可完成的。
例如：有三个客户C1，C2，C3，向银行家借款，该银行家的资金总额为10个资金单位，其中C1客户要借9各资金单位，C2客户要借3个资金单位，C3客户要借8个资金单位，总计20个资金单位。某一时刻的状态如图所示：
![银行家算法](https://img-blog.csdnimg.cn/20200204195656463.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
对于a图的状态，按照安全序列的要求，我们选的第一个客户应满足该客户所需的贷款小于等于银行家当前所剩余的钱款，可以看出只有C2客户能被满足：C2客户需1个资金单位，小银行家手中的2个资金单位，于是银行家把1个资金单位借给C2客户，使之完成工作并归还所借的3个资金单位的钱，进入b图。同理，银行家把4个资金单位借给C3客户，使其完成工作，在c图中，只剩一个客户C1，它需7个资金单位，这时银行家有8个资金单位，所以C1也能顺利借到钱并完成工作。最后（见图d）银行家收回全部10个资金单位，保证不赔本。那麽客户序列{C1，C2，C3}就是个安全序列，按照这个序列贷款，银行家才是安全的。否则的话，若在图b状态时，银行家把手中的4个资金单位借给了C1，则出现不安全状态：这时C1，C3均不能完成工作，而银行家手中又没有钱了，系统陷入僵持局面，银行家也不能收回投资。

综上所述，银行家算法是从当前状态出发，逐个按安全序列检查各客户谁能完成其工作，然后假定其完成工作且归还全部贷款，再进而检查下一个能完成工作的客户，…。如果所有客户都能完成工作，则找到一个安全序列，银行家才是安全的。

## 三、线程同步-Condition

背景：

> 小度：小爱同学；
> 小爱同学：我在呢；
> 小度：现在几点了？
> 小爱同学：你猜猜现在几点了

代码实现尝试：

```python
import threading


class XiaoAi(threading.Thread):
    def __init__(self):
        super().__init__(name='小爱同学')

    def run(self):
        print('{}:我在呢'.format(self.name))


class XiaoDu(threading.Thread):
    def __init__(self):
        super().__init__(name='小度')

    def run(self):
        print('{}:小爱同学'.format(self.name))


if __name__ == '__main__':
    xiaoai = XiaoAi()
    xiaodu = XiaoDu()
    xiaodu.start()
    xiaoai.start()
123456789101112131415161718192021222324
```

打印

```python
小度:小爱同学
小爱同学:我在呢
12
```

进一步扩展尝试：

```python
import threading


class XiaoAi(threading.Thread):
    def __init__(self):
        super().__init__(name='小爱同学')

    def run(self):
        print('{}:我在呢'.format(self.name))
        print('{}:你猜猜现在几点了'.format(self.name))


class XiaoDu(threading.Thread):
    def __init__(self):
        super().__init__(name='小度')

    def run(self):
        print('{}:小爱同学'.format(self.name))
        print('{}:现在几点了？'.format(self.name))


if __name__ == '__main__':
    xiaoai = XiaoAi()
    xiaodu = XiaoDu()
    xiaodu.start()
    xiaoai.start()
1234567891011121314151617181920212223242526
```

打印

```python
小度:小爱同学
小度:现在几点了？
小爱同学:我在呢
小爱同学:你猜猜现在几点了
1234
```

显然，没有达到我们想要的效果。
加锁再次尝试：

```python
import threading


class XiaoAi(threading.Thread):
    def __init__(self,lock):
        super().__init__(name='小爱同学')
        self.lock = lock

    def run(self):
        self.lock.acquire()
        print('{}:我在呢'.format(self.name))
        self.lock.release()
        self.lock.acquire()
        print('{}:你猜猜现在几点了'.format(self.name))
        self.lock.release()


class XiaoDu(threading.Thread):
    def __init__(self,lock):
        super().__init__(name='小度')
        self.lock = lock


    def run(self):
        self.lock.acquire()
        print('{}:小爱同学'.format(self.name))
        self.lock.release()
        self.lock.acquire()
        print('{}:现在几点了？'.format(self.name))
        self.lock.release()


if __name__ == '__main__':
    mutex = threading.Lock()
    xiaoai = XiaoAi(mutex)
    xiaodu = XiaoDu(mutex)
    xiaodu.start()
    xiaoai.start()
1234567891011121314151617181920212223242526272829303132333435363738
```

打印

```python
小度:小爱同学
小度:现在几点了？
小爱同学:我在呢
小爱同学:你猜猜现在几点了
1234
```

结果和之前一样，还是没有达到想要的效果，用锁是达不到效果的。
用线程同步尝试：
**Condition**类中有两个方法：
`__enter__()`调用的是Lock的`acquire()`方法；
`__exit__()`调用的是Lock的`release()`方法。
由此可知，**Condition**对象可以用上下文处理器处理。

```python
import threading


class XiaoAi(threading.Thread):
    def __init__(self,cond):
        super().__init__(name='小爱同学')
        self.cond = cond

    def run(self):
        with self.cond:
            self.cond.wait()
            print('{}:我在呢'.format(self.name))
            self.cond.notify()
            self.cond.wait()
            print('{}:你猜猜现在几点了'.format(self.name))
            self.cond.notify()



class XiaoDu(threading.Thread):
    def __init__(self,cond):
        super().__init__(name='小度')
        self.cond = cond


    def run(self):
        self.cond.acquire()
        print('{}:小爱同学'.format(self.name))
        self.cond.notify()
        self.cond.wait()
        print('{}:现在几点了？'.format(self.name))
        self.cond.notify()
        self.cond.wait()
        self.cond.release()



if __name__ == '__main__':
    cond = threading.Condition()
    xiaoai = XiaoAi(cond)
    xiaodu = XiaoDu(cond)
    xiaodu.start()
    xiaoai.start()
12345678910111213141516171819202122232425262728293031323334353637383940414243
```

显示：
![result4](https://img-blog.csdnimg.cn/20200204200010983.gif#pic_center)
由于Condition类实现了`__enter__()`和方法，所以可以用上下文处理器进行处理，即用with打开、关闭对象，这和`self.cond.acquire()`和`self.cond.release()`的效果是一样的。
显然，程序执行阻塞，这是因为子线程的调用顺序有问题，进行调试：

```python
import threading


class XiaoAi(threading.Thread):
    def __init__(self,cond):
        super().__init__(name='小爱同学')
        self.cond = cond

    def run(self):
        with self.cond:
            print(4)
            self.cond.wait()
            print(5)
            print('{}:我在呢'.format(self.name))
            self.cond.notify()
            self.cond.wait()
            print('{}:你猜猜现在几点了'.format(self.name))
            self.cond.notify()



class XiaoDu(threading.Thread):
    def __init__(self,cond):
        super().__init__(name='小度')
        self.cond = cond


    def run(self):
        self.cond.acquire()
        print('{}:小爱同学'.format(self.name))
        print(1)
        self.cond.notify()
        print(2)
        self.cond.wait()
        print(3)
        print('{}:现在几点了？'.format(self.name))
        self.cond.notify()
        self.cond.wait()
        self.cond.release()



if __name__ == '__main__':
    cond = threading.Condition()
    xiaoai = XiaoAi(cond)
    xiaodu = XiaoDu(cond)
    xiaodu.start()
    xiaoai.start()
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748
```

显示：
![result5](https://img-blog.csdnimg.cn/20200204200029792.gif#pic_center)
易知，执行顺序是：
先执行XiaoDu类的`print('{}:小爱同学'.format(self.name))`和`cond.notify()`，xiaodu一直`cond.wait()`，然后执行XiaoAi的`cond.acquire()`，也一直`cond.wait()`，两者均在等待，陷入死锁。
交换xiaodu和xiaoai的线程创建和执行顺序即可：

```python
import threading


class XiaoAi(threading.Thread):
    def __init__(self,cond):
        super().__init__(name='小爱同学')
        self.cond = cond

    def run(self):
        with self.cond:
            self.cond.wait()
            print('{}:我在呢'.format(self.name))
            self.cond.notify()
            self.cond.wait()
            print('{}:你猜猜现在几点了'.format(self.name))
            self.cond.notify()



class XiaoDu(threading.Thread):
    def __init__(self,cond):
        super().__init__(name='小度')
        self.cond = cond


    def run(self):
        self.cond.acquire()
        print('{}:小爱同学'.format(self.name))
        self.cond.notify()
        self.cond.wait()
        print('{}:现在几点了？'.format(self.name))
        self.cond.notify()
        self.cond.wait()
        self.cond.release()



if __name__ == '__main__':
    cond = threading.Condition()
    xiaoai = XiaoAi(cond)
    xiaodu = XiaoDu(cond)
    xiaoai.start()
    xiaodu.start()
12345678910111213141516171819202122232425262728293031323334353637383940414243
```

打印

```python
小度:小爱同学
小爱同学:我在呢
小度:现在几点了？
小爱同学:你猜猜现在几点了
1234
```

三个子线程的尝试：

```python
import threading


class XiaoAi(threading.Thread):
    def __init__(self,cond):
        super().__init__(name='小爱同学')
        self.cond = cond

    def run(self):
        with self.cond:
            self.cond.wait()
            print('{}:我在呢'.format(self.name))
            self.cond.notify()
            self.cond.wait()
            print('{}:你猜猜现在几点了'.format(self.name))
            self.cond.notify()


class XiaoDu(threading.Thread):
    def __init__(self,cond):
        super().__init__(name='小度')
        self.cond = cond

    def run(self):
        self.cond.acquire()
        print('{}:小爱同学，天猫精灵'.format(self.name))
        self.cond.notify()
        self.cond.wait()
        print('{}:现在几点了？'.format(self.name))
        self.cond.notify()
        self.cond.wait()
        self.cond.release()


class Jingling(threading.Thread):
    def __init__(self,cond):
        super().__init__(name='天猫精灵')
        self.cond = cond

    def run(self):
        with self.cond:
            self.cond.wait()
            print('{}:我也在呢'.format(self.name))
            self.cond.notify()
            self.cond.wait()
            print('{}:你猜猜现在几点了'.format(self.name))
            self.cond.notify()

if __name__ == '__main__':
    cond = threading.Condition()
    xiaoai = XiaoAi(cond)
    xiaodu = XiaoDu(cond)
    jingling = Jingling(cond)
    xiaoai.start()
    jingling.start()
    xiaodu.start()
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556
```

打印

```python
小度:小爱同学，天猫精灵
小爱同学:我在呢
天猫精灵:我也在呢
小度:现在几点了？
小爱同学:你猜猜现在几点了
天猫精灵:你猜猜现在几点了
123456
```

## 四、多任务版UDP聊天

**实现思路**：

- 创建套接字
- 绑定本地信息
- 获取对方IP和端口
- 发送、接收数据
- 创建两个线程，并执行功能

NetAssist配置如下：
![UDP配置](https://img-blog.csdnimg.cn/2020020420020261.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
代码实现：

```python
import socket
import threading


def recv_msg(udp_socket):
    '''接收数据'''
    while True:
        recv_data = udp_socket.recvfrom(1024)
        print(recv_data[0].decode('gbk'))


def send_msg(udp_socket, dest_ip, dest_port):
    '''发送数据'''
    while True:
        send_data = input('Inoput data to send:')
        udp_socket.sendto(send_data.encode('gbk'), (dest_ip, dest_port))


def main():
    #创建套接字
    udp_socket = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
    #绑定
    udp_socket.bind(('',7890))
    # 获取对方的IP和端口
    dest_ip = input('Input IP:')
    dest_port = int(input('Input Port:'))
    t_recv = threading.Thread(target=recv_msg, args=(udp_socket, ))
    t_send = threading.Thread(target=send_msg, args=(udp_socket, dest_ip, dest_port))
    t_recv.start()
    t_send.start()


if __name__ == '__main__':
    main()
12345678910111213141516171819202122232425262728293031323334
```

演示效果：
![result6](https://img-blog.csdnimg.cn/20200204200236251.gif)

## 五、进程介绍

### 1.概念辨析

- 进程：
  **正在运行**的代码+用到的资源；
- 程序：
  没有执行的代码，是**静态**的。

### 2.进程的状态

![进程的状态](https://img-blog.csdnimg.cn/20200204200438356.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
主要包括**就绪、运行、等待**三个状态，是一个循环往复的过程。

### 3.使用进程实现多任务

**multiprocessing**模块就是跨平台的多进程模块，提供了一个**Process类**来代表一个进程对象，这个对象可以理解为是一个独立的进程，可以执行另外的事情。
线程测试：

```python
import threading
import time


def demo1():
    for _ in range(5):
        print('--1--')
        time.sleep(1)


def demo2():
    for _ in range(5):
        print('--2--')
        time.sleep(1)


def main():
    t1 = threading.Thread(target=demo1)
    t2 = threading.Thread(target=demo2)
    t1.start()
    t2.start()


if __name__ == '__main__':
    main()
12345678910111213141516171819202122232425
```

打印：

```python
--1--
--2--
--1--
--2--
--1--
--2--
--1--
--2--
--1--
--2--
12345678910
```

改为进程：

```python
import multiprocessing
import time


def demo1():
    for _ in range(5):
        print('--1--')
        time.sleep(1)


def demo2():
    for _ in range(5):
        print('--2--')
        time.sleep(1)


def main():
    p1 = multiprocessing.Process(target=demo1)
    p2 = multiprocessing.Process(target=demo2)
    p1.start()
    p2.start()


if __name__ == '__main__':
    main()
12345678910111213141516171819202122232425
```

打印：

```python
--1--
--2--
--1--
--2--
--1--
--2--
--1--
--2--
--1--
--2--
12345678910
```

显然，进程和线程的代码实现是相似的，结果也是很近似的。
但是线程和进程是有很大不同的：
观察任务管理器：
是线程时，
![任务管理器-线程](https://img-blog.csdnimg.cn/20200204200659290.png#pic_center)
此时只有**一个**Python进程。
是进程时，
![任务管理器-进程](https://img-blog.csdnimg.cn/20200204200723123.png#pic_center)
此时有**两个**Python进程，分别代表代码中的两个子进程。
一个主进程和两个子进程，**子进程将主进程的代码复制**，再执行，会造成资源浪费，但是会比单进程效率要快。
在**Ubuntu**中运行：

```python
import os
import time

#创建子线程
pid = os.fork()
print('Hello World')

#判断是子线程
if pid == 0:
    print('s_fork:{},f_fork:{}'.format(os.getpid(),os.getppid()))
else:
    print('f_fork:{}'.format(os.getpid()))
123456789101112
```

执行（Windows运行会报错）结果为

```python
Hello World
f_fork:2180
Hello World
s_fork:2181,f_fork:2180
1234
```

打印了两次Corley。
解释：
`os.fork()`复制父进程代码创建了一个子进程，父进程和子进程分别执行一次，先后打印出**Hello World**，并分别进入if判断，两次打印id。

# Python全栈（四）高级编程技巧之9.Python多任务-进程

## 一、线程和进程之间的对比

### 1.代码解析

```python
import os
import time

#创建子线程
pid = os.fork()
print('Hello World')

#判断是子线程
if pid == 0:
    print('s_fork:{},f_fork:{}'.format(os.getpid(),os.getppid()))
else:
    print('f_fork:{}'.format(os.getpid()))
123456789101112
```

进一步解释：
主进程`os.fork()`创建一个子进程，此时先执行主进程，pid不为0，执行else后的语句；
再执行创建的子进程，pid返回值为0，所以执行if后的语句。
关于`os.fork()`的详细解释：
`os.fork()`函数是用来新建进程。但它只在POSIX系统上可用，在windows版的Python中，os模块没有定义`os.fork()`函数。
`os.fork()`函数创建进程的过程是这样的:
程序每次执行时，操作系统都会创建一个新进程来运行程序指令。进程还可调用`os.fork()`，要求操作系统新建一个进程。父进程是调用`os.fork()`函数的进程，父进程所创建的进程叫子进程。每个进程都有一个不重复的进程ID号称为pid，它对进程进行标识。子进程与父进程完全相同，子进程从父进程继承了多个值的拷贝，如全局变量和环境变量。两个进程的唯一区别是fork的返回值。子进程接收返回值0，而父进程接收子进程的pid作为返回值。一个现有进程可以调用`fork()`函数创建一个新进程，由fork创建的新进程被称为子进程（child process）。`fork()`函数被调用一次但返回两次。两次返回的唯一区别是子进程中返回0值而父进程中返回子进程ID。对于程序，只要判断fork的返回值，就知道自己是处于父进程还是子进程中。
更进一步的解释可参考https://www.jianshu.com/p/e8f5b828cce0。

### 2.线程、进程对比

（1）**运行原理对比**：

- 进程：
  当执行`p.start()`时，主进程复制一份代码到子进程，执行多任务；
  能够完成多任务，例如一台电脑上可以同时运行多个QQ。
  **补充**：
  不一定是多少个子进程就复制多少份代码，如果各个进程之间不修改公有变量，则可以共享一份代码，涉及到**写时拷贝**。
  ![进程](https://img-blog.csdnimg.cn/20200205163815168.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
- 线程：
  当执行`t.start()`时，多个子线程共同执行一份代码；
  能够完成多任务，例如一个QQ中的多个聊天窗口。
  ![线程](https://img-blog.csdnimg.cn/20200205163841994.png#pic_center)

（2）**根本区别**：
进程是操作系统**资源分配**的基本单位，而线程是**任务调度**和执行的基本单位。
（3）**开销区别**：
进程之间切换开销很大，线程之间切换开销很小。
线程可以看作一个轻量级的进程，同一类线程共享代码和数据空间。
![线程和进程](https://img-blog.csdnimg.cn/20200205163651442.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 二、进程间通信-Queue

进程间不能直接通信，要想实现两个进程间的通信，可以增加一个中间变量，如通过文件，一个进程写，另一个读，但是这种方式效率很低，一般通过队列**Queue**进行。
队列的特点是**先进先出**。

```python
from multiprocessing import Queue


# 创建队列，最多存放3条数据
q = Queue(3)
# 存数据
q.put(1)
q.put('Corley')
q.put([11,22])
print('finished')
12345678910
```

打印

```python
finished
1
```

再尝试

```python
from multiprocessing import Queue


# 创建队列，最多存放3条数据
q = Queue(3)
# 存数据
q.put(1)
q.put('Corley')
q.put([11,22])
q.put({'name':'Corley'})
print('finished')
1234567891011
```

显示
![result1](https://img-blog.csdnimg.cn/20200205163945498.gif#pic_center)
显然，当超过3条数据时会发生堵塞，只能强制停止。
改进代码：

```python
from multiprocessing import Queue


# 创建队列，最多存放3条数据
q = Queue(3)
# 存数据
q.put(1)
q.put('Corley')
q.put([11,22])
# 直接抛出异常
q.put_nowait({'name':'Corley'})
print('finished')
123456789101112
```

打印

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 11, in <module>
    q.put_nowait({'name':'Corley'})
  File "xxxx\Python\Python37\lib\multiprocessing\queues.py", line 129, in put_nowait
    return self.put(obj, False)
  File "xxxx\Python\Python37\lib\multiprocessing\queues.py", line 83, in put
    raise Full
queue.Full
12345678
```

显示队列已满，不能再添加元素。
取数据：

```python
from multiprocessing import Queue


# 创建队列，最多存放3条数据
q = Queue(3)
# 存数据
q.put(1)
q.put('Corley')
q.put([11,22])
print(q.get())
print(q.get())
print(q.get())
print('finished')
12345678910111213
```

打印

```python
1
Corley
[11, 22]
finished
1234
```

按照**先进先出**的顺序取数据。
再尝试：

```python
from multiprocessing import Queue


# 创建队列，最多存放3条数据
q = Queue(3)
# 存数据
q.put(1)
q.put('Corley')
q.put([11,22])
print(q.get())
print(q.get())
print(q.get())
# 堵塞
print(q.get())
print('finished')
123456789101112131415
```

显示：
![result2](https://img-blog.csdnimg.cn/20200205164018386.gif#pic_center)
显然，数据取完后再取也会发生堵塞。
改进代码：

```python
from multiprocessing import Queue


# 创建队列，最多存放3条数据
q = Queue(3)
# 存数据
q.put(1)
q.put('Corley')
q.put([11,22])
print(q.get())
print(q.get())
print(q.get())
# 堵塞，直接抛出异常
print(q.get_nowait())
print('finished')
123456789101112131415
```

打印

```python
Traceback (most recent call last):
1
Corley
[11, 22]
  File "xxx/demo.py", line 14, in <module>
    print(q.get_nowait())
  File "xxxx\Python\Python37\lib\multiprocessing\queues.py", line 126, in get_nowait
    return self.get(False)
  File "xxxx\Python\Python37\lib\multiprocessing\queues.py", line 107, in get
    raise Empty
_queue.Empty
1234567891011
```

显示队列为空。
其他方法使用：

```python
from multiprocessing import Queue


# 创建队列，最多存放3条数据
q = Queue(3)
# 存数据
q.put(1)
q.put('Corley')
# 判断队列是否为满
print(q.full())
q.put([11,22])
print(q.qsize())
print(q.get())
print(q.get())
print(q.get())
print(q.empty())
print('finished')
1234567891011121314151617
```

打印

```python
False
3
1
Corley
[11, 22]
True
finished
1234567
```

队列在进程通信方面的实现原理如下：
![Queue](https://img-blog.csdnimg.cn/20200205163924719.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
下载和处理数据实现了分离，在一定程度上实现了**解耦**。
队列用于进程通信尝试：

```python
import multiprocessing


def download(q):
    '''下载数据'''
    lis = [11, 22, 33]
    for item in lis:
        q.put(item)
    print('Download complete and save to queue')


def analyse(q):
    '''分析数据'''
    analyse_data = list()
    while True:
        data = q.get()
        analyse_data.append(data)
        if q.empty():
            break
    print(analyse_data)


def main():
    # 创建队列
    q = multiprocessing.Queue()
    p1 = multiprocessing.Process(target=download, args=(q,))
    p2 = multiprocessing.Process(target=analyse, args=(q,))
    p1.start()
    p2.start()


if __name__ == '__main__':
    main()
123456789101112131415161718192021222324252627282930313233
```

打印

```python
Download complete and save to queue
[11, 22, 33]
12
```

## 三、多进程共享全局变量

进程之间共享变量尝试：

```python
import multiprocessing


a = 1


def demo1():
    global a
    a += 1


def demo2():
    # 打印出的值为2，则是共享的
    print(a)


if __name__ == '__main__':
    p1 = multiprocessing.Process(target=demo1)
    p2 = multiprocessing.Process(target=demo2)
    p1.start()
    p2.start()
123456789101112131415161718192021
```

打印

```python
1
1
```

打印出的结果为1，说明**进程之间全局变量是不共享的**，与线程不同。
用普通队列尝试：

```python
import multiprocessing
from queue import Queue


def demo1(q):
    q.put('a')


def demo2(q):
    data = q.get()
    print(data)


if __name__ == '__main__':
    q = Queue()
    p1 = multiprocessing.Process(target=demo1, args=(q,))
    p2 = multiprocessing.Process(target=demo2, args=(q,))
    p1.start()
    p2.start()
12345678910111213141516171819
```

打印

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 93, in <module>
    p1.start()
  File "xxxx\Python\Python37\lib\multiprocessing\process.py", line 112, in start
    self._popen = self._Popen(self)
  File "xxxx\Python\Python37\lib\multiprocessing\context.py", line 223, in _Popen
    return _default_context.get_context().Process._Popen(process_obj)
  File "xxxx\Python\Python37\lib\multiprocessing\context.py", line 322, in _Popen
    return Popen(process_obj)
  File "xxxx\Python\Python37\lib\multiprocessing\popen_spawn_win32.py", line 89, in __init__
    reduction.dump(process_obj, to_child)
  File "xxxx\Python\Python37\lib\multiprocessing\reduction.py", line 60, in dump
    ForkingPickler(file, protocol).dump(obj)
TypeError: can't pickle _thread.lock objects
1234567891011121314
```

报错，属于类型错误。
改进–线程调用`run()`方法即可达到效果：

```python
import multiprocessing
from queue import Queue


def demo1(q):
    q.put('a')


def demo2(q):
    data = q.get()
    print(data)


if __name__ == '__main__':
    q = Queue()
    p1 = multiprocessing.Process(target=demo1, args=(q,))
    p2 = multiprocessing.Process(target=demo2, args=(q,))
    p1.run()
    p2.run()
12345678910111213141516171819
```

打印

```python
a
1
```

此时正常运行得到结果，但是已经不属于多进程了，因为调用的不是`run()`方法，所以普通队列不能实现真正的线程间通信，要实现**跨线程通信**，需要使用`multiprocessing.Queue`。
缺失参数异常处理尝试：

```python
import multiprocessing


def demo1(q):
    try:
        q.put('a')
    except Exception as e:
        print(e.args[0])


def demo2(q):
    try:
        data = q.get()
        print(data)
    except Exception as e:
        print(e.args[0])


if __name__ == '__main__':
    q = multiprocessing.Queue()
    try:
        p1 = multiprocessing.Process(target=demo1)
        p2 = multiprocessing.Process(target=demo2)
        p1.start()
        p2.start()
    except Exception as e:
        print(e.args[0])
123456789101112131415161718192021222324252627
```

打印

```python
Process Process-1:
Traceback (most recent call last):
  File "xxxx\Python\Python37\lib\multiprocessing\process.py", line 297, in _bootstrap
    self.run()
  File "xxxx\Python\Python37\lib\multiprocessing\process.py", line 99, in run
    self._target(*self._args, **self._kwargs)
TypeError: demo1() missing 1 required positional argument: 'q'
Process Process-2:
Traceback (most recent call last):
  File "xxxx\Python\Python37\lib\multiprocessing\process.py", line 297, in _bootstrap
    self.run()
  File "xxxx\Python\Python37\lib\multiprocessing\process.py", line 99, in run
    self._target(*self._args, **self._kwargs)
TypeError: demo2() missing 1 required positional argument: 'q'
1234567891011121314
```

显然，此时报错的位置不是在自己写的代码中，而是在Python自己的**lib\multiprocessing\process.py**中报错，属于Process类的参数错误。

## 四、进程池

### 1.进程池介绍

当需要创建的子进程数量不多时，可以直接利用multiprocessing中的Process动态生成多个进程，但是如果是上百甚至上千个目标，手动的去创建的进程的工作量巨大，此时就可以用到multiprocessing模块提供的Pool类。
初始化Pool时，可以指定一个最大进程数，当有新的请求提交到Pool中时，如果池还没有满，那么就会创建一个新的进程用来执行该请求，但是如果池中的进程数已经达到指定的最大值，那么该请求就会等待，直到池中有进程结束，才会用之前的进程来执行新的任务。
原理如下图所示：
![线程池](https://img-blog.csdnimg.cn/20200205164718774.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
尝试：

```python
from multiprocessing import Pool
import os, time, random


def work(msg):
    t_start = time.time()
    print('%s开始执行,进程号为%d' % (msg, os.getpid()))

    time.sleep(random.random() * 2)
    t_stop = time.time()
    print(msg, "执行完成,耗时%0.2f seconds" % (t_stop - t_start))


def demo():
    pass


if __name__ == '__main__':
    po = Pool(3)  # 定义一个进程池，包含3个进程
    for i in range(0, 10):
        # 添加任务，有10个任务
        po.apply_async(work, (i,))
    print("--start--")
    # 关闭进程池，不再接收新的任务请求
    po.close()
    # 等待子进程执行完成，主进程再结束
    po.join()
    print("--end--")
12345678910111213141516171819202122232425262728
```

显示：
![result3](https://img-blog.csdnimg.cn/20200205164744964.gif#pic_center)
易知，最开始有0、1、2三个进程执行，0执行完成后3开始执行，1执行完成后4开始执行，3执行完成后5开始执行，一直循环，直到所有进程完成。
修改进程池参数–包含4个进程：

```python
from multiprocessing import Pool
import os, time, random


def work(msg):
    t_start = time.time()
    print('%s开始执行,进程号为%d' % (msg, os.getpid()))

    time.sleep(random.random() * 2)
    t_stop = time.time()
    print(msg, "执行完成,耗时%0.2f seconds" % (t_stop - t_start))


def demo():
    pass


if __name__ == '__main__':
    po = Pool(4)  # 定义一个进程池，包含3个进程
    for i in range(0, 10):
        # 添加任务，有10个任务
        po.apply_async(work, (i,))
    print("--start--")
    # 关闭进程池，不再接收新的任务请求
    po.close()
    # 等待子进程执行完成，主进程再结束
    po.join()
    print("--end--")
12345678910111213141516171819202122232425262728
```

显示：
![result4](https://img-blog.csdnimg.cn/20200205164806318.gif#pic_center)
显然，进程池有4个进程时，执行效率更高，因为同时有4个进程在执行。
`po.close()`执行后不能再添加任务：

```python
from multiprocessing import Pool
import os, time, random


def work(msg):
    t_start = time.time()
    print('%s开始执行,进程号为%d' % (msg, os.getpid()))

    time.sleep(random.random() * 2)
    t_stop = time.time()
    print(msg, "执行完成,耗时%0.2f seconds" % (t_stop - t_start))


def demo():
    pass


if __name__ == '__main__':
    po = Pool(4)  # 定义一个进程池，包含3个进程
    for i in range(0, 10):
        # 添加任务，有10个任务
        po.apply_async(work, (i,))
    print("--start--")
    # 关闭进程池，不再接收新的任务请求
    po.close()
    po.apply_async(demo)
    # 等待子进程执行完成，主进程再结束
    po.join()
    print("--end--")
1234567891011121314151617181920212223242526272829
```

打印

```python
--start--
Traceback (most recent call last):
  File "xxx/demo.py", line 151, in <module>
    po.apply_async(demo)
  File "xxxx\Python\Python37\lib\multiprocessing\pool.py", line 362, in apply_async
    raise ValueError("Pool not running")
ValueError: Pool not running
1234567
```

要添加任务，必须要在`po.close()`之前添加。

### 2.进程池中进程间的通信

用`multiprocessing.Queue`进行尝试：

```python
import multiprocessing

def demo1(q):
    q.put('a')


def demo2(q):
    data = q.get()
    print(data)


if __name__ == '__main__':
    q = multiprocessing.Queue()
    po = multiprocessing.Pool()
    po.apply_async(demo1,(q,))
    po.apply_async(demo2, (q,))
    po.close()
    po.join()
123456789101112131415161718
```

打印

```python
1
```

打印为空，即没有实现进程池的进程间通信。
进行测试：

```python
import multiprocessing

def demo1(q):
    print(1)
    q.put('a')


def demo2(q):
    print(2)
    data = q.get()
    print(data)


if __name__ == '__main__':
    q = multiprocessing.Queue()
    po = multiprocessing.Pool()
    po.apply_async(demo1,(q,))
    po.apply_async(demo2, (q,))
    po.close()
    po.join()
1234567891011121314151617181920
```

打印

```python
1
```

还是为空，进一步进行异常处理：

```python
import multiprocessing

def demo1(q):
    try:
        q.put('a')
    except Exception as e:
        print(e.args[0])



def demo2(q):
    try:
        data = q.get()
        print(data)
    except Exception as e:
        print(e.args[0])


if __name__ == '__main__':
    q = multiprocessing.Queue()
    po = multiprocessing.Pool()
    po.apply_async(demo1,(q,))
    po.apply_async(demo2, (q,))
    po.close()
    po.join()
12345678910111213141516171819202122232425
```

打印

```python
1
```

还是为空，显然可得，`demo1()`和`demo2()`两个函数根本没有执行，所以进程池的进程间通信不能用`multiprocessing.Queue`，要用`multiprocessing.Manager().Queue`。
再次尝试：

```python
import multiprocessing

def demo1(q):
    try:
        q.put('a')
    except Exception as e:
        print(e.args[0])



def demo2(q):
    try:
        data = q.get()
        print(data)
    except Exception as e:
        print(e.args[0])


if __name__ == '__main__':
    q = multiprocessing.Manager().Queue()
    po = multiprocessing.Pool()
    po.apply_async(demo1,(q,))
    po.apply_async(demo2, (q,))
    po.close()
    po.join()
12345678910111213141516171819202122232425
```

打印

```python
a
1
```

此时正常运行。
进程池里的进程，如果出现异常，不会主动抛出，所以在用进程池实现多任务时尽量增加异常处理`try...except...`。
**总结**：
现在有3种队列：
（1）`queue.Queue`：
不能完成进程之间的通信；
（2）`multiprocessing.Queue`：
实现进程间的通信；
（3）`multiprocessing.Manager().Queue`：
实现进程池间的进程通信。

## 五、应用–多任务文件夹复制

**实现思路**：

- 获取用户要复制的文件夹名
- 创建新的文件夹
- 获取文件夹中所有待拷贝的文件名字
- 创建进程池
- 添加拷贝任务

代码初步实现：

```python
import multiprocessing
import os


def copy_file(index,file_name,new_folder,old_folder):
    '''文件拷贝'''
    print('%2d:File to Copy:%s' % (index + 1,file_name))
    # 读取旧文件
    old_file = open(old_folder + '/' + file_name,'rb')
    content = old_file.read()
    old_file.close()
    # 保存到新的文件夹
    # new_file = open(new_folder + '/' + file_name, 'wb')
    # new_file.write(content)
    # new_file.close()
    with open(new_folder + '/' + file_name, 'wb') as f:
        f.write(content)
    print('%2d:File %s Copy Finished' % (index + 1,file_name))


def main():
    # 获取用户要复制的文件夹名
    old_folder = input('Inout Directory Name:')
    # 创建新的文件夹
    new_folder = old_folder + '--copy'
    if not os.path.exists(new_folder):
        os.mkdir(new_folder)
    # 获取文件夹中所有待拷贝的文件名字
    file_list = os.listdir(old_folder)
    # 创建进程池
    po = multiprocessing.Pool(5)
    # 添加拷贝任务
    for index,file_name in enumerate(file_list):
        po.apply_async(copy_file,args=(index,file_name,new_folder,old_folder))
    po.close()
    po.join()


if __name__ == '__main__':
    main()
12345678910111213141516171819202122232425262728293031323334353637383940
```

打印

```python
Inout Directory Name:test
 2:File to Copy:_collections_abc.py
 1:File to Copy:_bootlocale.py
 5:File to Copy:_dummy_thread.py
 3:File to Copy:_compat_pickle.py
 4:File to Copy:_compression.py
 2:File _collections_abc.py Copy Finished
 6:File to Copy:_markupbase.py
 5:File _dummy_thread.py Copy Finished
 1:File _bootlocale.py Copy Finished
 3:File _compat_pickle.py Copy Finished
 4:File _compression.py Copy Finished
 6:File _markupbase.py Copy Finished
 7:File to Copy:_osx_support.py
 8:File to Copy:_pydecimal.py
 7:File _osx_support.py Copy Finished
 9:File to Copy:_pyio.py
 8:File _pydecimal.py Copy Finished
10:File to Copy:_py_abc.py
11:File to Copy:__future__.py
 9:File _pyio.py Copy Finished
11:File __future__.py Copy Finished
12:File to Copy:__phello__.foo.py
10:File _py_abc.py Copy Finished
12:File __phello__.foo.py Copy Finished
12345678910111213141516171819202122232425
```

进一步扩展–增加进度显示

```python
import multiprocessing
import os
import random
import time


def copy_file(index,file_name,new_folder,old_folder,q):
    '''文件拷贝'''
    print('%2d:File to Copy:%s' % (index + 1,file_name))
    # 读取旧文件
    old_file = open(old_folder + '/' + file_name,'rb')
    content = old_file.read()
    old_file.close()
    # 保存到新的文件夹
    # new_file = open(new_folder + '/' + file_name, 'wb')
    # new_file.write(content)
    # new_file.close()
    with open(new_folder + '/' + file_name, 'wb') as f:
        f.write(content)
    q.put(file_name)
    print('%2d:File %s Copy Finished' % (index + 1,file_name))
    time.sleep(random.random() * 5)


def main():
    # 获取用户要复制的文件夹名
    old_folder = input('Inout Directory Name:')
    # 创建新的文件夹
    new_folder = old_folder + '--copy'
    if not os.path.exists(new_folder):
        os.mkdir(new_folder)
    # 获取文件夹中所有待拷贝的文件名字
    file_list = os.listdir(old_folder)
    # 创建进程池
    po = multiprocessing.Pool(5)
    # 创建队列
    q = multiprocessing.Manager().Queue()
    # 添加拷贝任务
    for index,file_name in enumerate(file_list):
        po.apply_async(copy_file,args=(index,file_name,new_folder,old_folder,q))
    po.close()
    # 增加进度条
    file_count = len(file_list)
    copy_count = 0
    while True:
        q.get()
        copy_count += 1
        print('Copy Process：%2.2f%%' % (copy_count / file_count * 100.0))
        # 退出循环
        if copy_count >= file_count:
            break
    po.join()


if __name__ == '__main__':
    main()
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556
```

显示
![result5](https://img-blog.csdnimg.cn/20200205164938269.gif#pic_center)

# Python全栈（四）高级编程技巧之10.Python多任务-协程

## 一、生成器-send方法

### 1.同步、异步

- 同步：
  是指代码调用**IO操作**时,必须等待IO操作完成才返回的调用方式。
- 异步：
  是指代码调用**IO操作**时,不必等IO操作完成就返回的调用方式。
  同步异步比较如下：
  ![比较](https://img-blog.csdnimg.cn/20200207182824974.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 2.堵塞、非堵塞

- 阻塞：

  从调用者的角度出发，如果在调用的时候，被卡住，不能再继续向下运行，需要等待，就说是阻塞。

  堵塞的例子有：

  - 多个用户同时操作数据库和锁机制
  - Socket的`accept()`方法
  - `input()`

- 非阻塞:
  从调用者的角度出发，如果在调用的时候，没有被卡住，能够继续向下运行，无需等待，就说是非阻塞。

### 3.生成器的send()方法

之前讲到生成器：

```python
def create_fib(num):
    a, b = 0, 1
    current_num = 0
    while current_num < num:
        yield a
        a, b = b, a + b
        current_num += 1


g = create_fib(5)
print(next(g))
print()
for i in g:
    print(i)
1234567891011121314
```

打印

```python
0

1
1
2
3
123456
```

假如生成器中有返回值，要想获取返回值：

```python
def create_fib(num):
    a, b = 0, 1
    current_num = 0
    while current_num < num:
        yield a
        a, b = b, a + b
        current_num += 1
    return 'hello'


g = create_fib(5)


while True:
    try:
        ret = next(g)
        print(ret)
    except Exception as e:
        print(e.args[0])
        break
1234567891011121314151617181920
```

打印

```python
0
1
1
2
3
hello
123456
```

显然，hello在异常处理的`except`语句中获取到并打印出来。
`send()`方法有一个参数，该参数指定的是上一次被挂起的yield语句的返回值。
用`send()`方法启动生成器：

```python
def create_fib(num):
    a, b = 0, 1
    current_num = 0
    while current_num < num:
        yield a
        a, b = b, a + b
        current_num += 1
    return 'hello'


g = create_fib(5)


print(g.send(None))
print(g.send('hello'))
123456789101112131415
```

打印

```python
0
1
12
```

当第一次调用的是`send()`方法时，传入的参数只能是None，否则会报错：

```python
def create_fib(num):
    a, b = 0, 1
    current_num = 0
    while current_num < num:
        yield a
        a, b = b, a + b
        current_num += 1
    return 'hello'


g = create_fib(5)


print(g.send('hello'))
print(g.send('world'))
123456789101112131415
```

打印

```python
Traceback (most recent call last):
  File "xxx/demo.py", line 14, in <module>
    print(g.send('hello'))
TypeError: can't send non-None value to a just-started generator
1234
```

显然，第一次调用`send()`时必须传入None，即第一次调用的不是`next()`时，那么调用`send()`的参数必须是**None**。
`send()`方法可以和`next()`方法结合使用：

```python
def create_fib(num):
    a, b = 0, 1
    current_num = 0
    while current_num < num:
        yield a
        a, b = b, a + b
        current_num += 1
    return 'hello'


g = create_fib(5)


print(next(g))
print(g.send('hello'))
123456789101112131415
```

打印

```python
0
1
12
```

当对`yield a`进行赋值时：

```python
def create_fib(num):
    a, b = 0, 1
    current_num = 0
    while current_num < num:
        result = yield a
        print(result)
        a, b = b, a + b
        current_num += 1
    return 'hello'


g = create_fib(5)


print(next(g))
print(g.send('hello'))
12345678910111213141516
```

打印

```python
0
hello
1
123
```

打印出了hello，对hello的打印进行验证：

```python
def create_fib(num):
    a, b = 0, 1
    current_num = 0
    while current_num < num:
        result = yield a
        print('result-->', result)
        a, b = b, a + b
        current_num += 1
    return 'hello'


g = create_fib(5)


print(next(g))
print(g.send('hello'))
12345678910111213141516
```

打印

```python
0
result--> hello
1
123
```

显然，是通过打印result打印出来的。
解释：
执行`result = yield a`时，停在此处，先执行`yield a`返回打印出0，在调用`send()`方法时，将hello赋值给整个`yield a`，即赋值给result，继续向下执行，第二次循环打印出1。
再次测试：

```python
def create_fib(num):
    a, b = 0, 1
    current_num = 0
    while current_num < num:
        result = yield a
        print('result-->', result)
        a, b = b, a + b
        current_num += 1
    return 'hello'


g = create_fib(5)


print(next(g))
print(g.send('hello'))
print(g.send('world'))
1234567891011121314151617
```

打印

```python
0
result--> hello
1
result--> world
1
12345
```

生成器的`close()`方法使用：

```python
def create_fib(num):
    a, b = 0, 1
    current_num = 0
    while current_num < num:
        result = yield a
        print('result-->', result)
        a, b = b, a + b
        current_num += 1
    return 'hello'


g = create_fib(5)


print(next(g))
# 关闭生成器
g.close()
print(g.send('hello'))
print(g.send('world'))
12345678910111213141516171819
```

打印

```python
0
Traceback (most recent call last):
  File "xxx/demo.py", line 18, in <module>
    print(g.send('hello'))
StopIteration
12345
```

即调用`close()`方法后，生成器关闭、迭代停止，不能再向下迭代，即不能再调用`next()`、`send()`方法。

## 二、使用yield完成多任务和yield from

### 1.使用yield完成多任务

使用yield实现多任务测试：

```python
import time


def task1():
    while True:
        print('---1---')
        time.sleep(0.1)
        yield


def task2():
    while True:
        print('---2---')
        time.sleep(0.1)
        yield


def main():
    t1 = task1()
    t2 = task2()
    while True:
        next(t1)
        next(t2)


if __name__ == '__main__':
    main()
123456789101112131415161718192021222324252627
```

显示：
![result1](https://img-blog.csdnimg.cn/20200207201938712.gif#pic_center)
实现了交替运行、实现多任务的效果，并且消耗的资源比线程、进程更少。

### 2.yield from的使用

`itertools.chain`可以实现对多个可迭代对象输出结果：

```python
from itertools import chain


lis = [1, 2, 3]
dic = {
    'name':'Corley',
    'age':18
}

for value in chain(lis, dic, range(5,10)):
    print(value)
1234567891011
```

打印

```python
1
2
3
name
age
5
6
7
8
9
12345678910
```

并且可以对`itertools.chain`对象强制转化为列表：

```python
from itertools import chain


lis = [1, 2, 3]
dic = {
    'name':'Corley',
    'age':18
}

print(list(chain(lis, dic, range(5,10))))
for value in chain(lis, dic, range(5,10)):
    print(value)
123456789101112
```

打印

```python
[1, 2, 3, 'name', 'age', 5, 6, 7, 8, 9]
1
2
3
name
age
5
6
7
8
9
1234567891011
```

可以使用`yield`实现同样的功能：

```python
lis = [1, 2, 3]
dic = {
    'name':'Corley',
    'age':18
}


def my_chain(*args, **kwargs):
    for my_iterable in args:
        for value in my_iterable:
            yield value


for value in my_chain(lis, dic, range(5,10)):
    print(value)
123456789101112131415
```

打印

```python
1
2
3
name
age
5
6
7
8
9
12345678910
```

python3.3新加了`yield from`语法。
使用`yield from`可以实现同样的效果：

```python
lis = [1, 2, 3]
dic = {
    'name':'Corley',
    'age':18
}


def my_chain(*args, **kwargs):
    for my_iterable in args:
        yield from my_iterable


for value in my_chain(lis, dic, range(5,10)):
    print(value)
1234567891011121314
```

执行结果与前者相同，即`yield from`相当于一个for循环。
`yield`和`yield from`对比：

```python
def generator1(lis):
    yield lis


def generator2(lis):
    yield from lis


lis = [1, 2, 3, 4, 5]

for i in generator1(lis):
    print(i)

for i in generator2(lis):
    print(i)
123456789101112131415
```

打印

```python
[1, 2, 3, 4, 5]
1
2
3
4
5
123456
```

生成器中传入参数求和：

```python
# 子生成器
def generator_1():
    total = 0
    while True:
        x = yield
        print('add --', x)
        if not x:
            break
        total += x
    return total


# 委托生成器
def generator_2():
    while True:
        total = yield from generator_1()  # 子生成器
        print('sum is --', total)


# 调用方
def main():
    g1 = generator_1()
    g1.send(None)
    g1.send(2)
    g1.send(3)
    g1.send(None)


if __name__ == '__main__':
    main()
123456789101112131415161718192021222324252627282930
```

打印

```python
Traceback (most recent call last):
add -- 2
add -- 3
add -- None
  File "xxx/demo.py", line 121, in <module>
    main()
  File "xxx/demo.py", line 112, in main
    g1.send(None)
StopIteration: 5
123456789
```

即通过**generator_1**不能实现功能。
用**generator_2**进行尝试：

```python
# 子生成器
def generator_1():
    total = 0
    while True:
        x = yield
        print('add --', x)
        if not x:
            break
        total += x
    return total


# 委托生成器
def generator_2():
    while True:
        # yield from建立调用方和子生成器的通道
        total = yield from generator_1()  # 子生成器
        print('sum is --', total)


# 调用方
def main():
    g2 = generator_2()
    g2.send(None)
    g2.send(2)
    g2.send(3)
    g2.send(None)


if __name__ == '__main__':
    main()
12345678910111213141516171819202122232425262728293031
```

打印

```python
add -- 2
add -- 3
add -- None
sum is -- 5
1234
```

实现了功能。
说明和解释：
子生成器：`yield from`后的`generator_1()`生成器函数是子生成器；
委托生成器：`generator_2()`是程序中的委托生成器，它负责委托子生成器完成具体任务；
调用方：`main()`是程序中的调用方，负责调用委托生成器。
`yield from`建立了调用方和子生成器的通道，借助委托生成器，`send()`函数传的值通过`yield from`传给子生成器中；
`yield from`省去了很多异常处理。

## 三、协程-使用greenlet&gevent完成多任务

### 1.协程概念

协程，又称微线程，是Python中另外一种实现多任务的方式，只不过是比线程占用（需要的资源）更小的执行单元。
Python中的协程大概经历了如下三个阶段：

- （1）最初的生成器变形yield/send
- （2）`yield from`
- （3）在最近的Python3.5版本中引入async/await关键字

协程自带CPU上下文，通过`yield`保存运行状态，才能恢复CPU上下文程序。

### 2.使用greenlet完成多任务

安装模块：

```powershell
pip install greenlet
1
```

greenlet使用：

```python
from greenlet import greenlet
import time


def demo1():
    while True:
        print('---demo1---')
        gr2.switch()
        time.sleep(0.5)


def demo2():
    while True:
        print('---demo2---')
        gr1.switch()
        time.sleep(0.5)


gr1 = greenlet(demo1)
gr2 = greenlet(demo2)
gr1.switch()
123456789101112131415161718192021
```

显示：
![result 2](https://img-blog.csdnimg.cn/20200207202047911.gif#pic_center)
易知，协程利用程序的IO来切换任务，用greenlet模块需要人工手动切换。

### 3.使用gevent完成多任务

安装模块：

```powershell
pip install gevent
1
```

使用gevent进行尝试：

```python
import gevent
import time


def f1(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        time.sleep(0.5)


def f2(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        time.sleep(0.5)


def f3(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        time.sleep(0.5)


g1 = gevent.spawn(f1, 5)
g2 = gevent.spawn(f2, 5)
g3 = gevent.spawn(f3, 5)

g1.join()
g2.join()
g3.join()
1234567891011121314151617181920212223242526272829
```

打印

```python
<Greenlet at 0x25f41185378: f1(5)> 0
<Greenlet at 0x25f41185378: f1(5)> 1
<Greenlet at 0x25f41185378: f1(5)> 2
<Greenlet at 0x25f41185378: f1(5)> 3
<Greenlet at 0x25f41185378: f1(5)> 4
<Greenlet at 0x25f41185598: f2(5)> 0
<Greenlet at 0x25f41185598: f2(5)> 1
<Greenlet at 0x25f41185598: f2(5)> 2
<Greenlet at 0x25f41185598: f2(5)> 3
<Greenlet at 0x25f41185598: f2(5)> 4
<Greenlet at 0x25f411856a8: f3(5)> 0
<Greenlet at 0x25f411856a8: f3(5)> 1
<Greenlet at 0x25f411856a8: f3(5)> 2
<Greenlet at 0x25f411856a8: f3(5)> 3
<Greenlet at 0x25f411856a8: f3(5)> 4
123456789101112131415
```

显然未达到预期的效果实现多任务。
进行改进–使用`gevent.sleep()`：

```python
import gevent
import time


def f1(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        gevent.sleep(0.5)


def f2(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        gevent.sleep(0.5)


def f3(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        gevent.sleep(0.5)


g1 = gevent.spawn(f1, 5)
g2 = gevent.spawn(f2, 5)
g3 = gevent.spawn(f3, 5)

g1.join()
g2.join()
g3.join()
1234567891011121314151617181920212223242526272829
```

打印

```python
<Greenlet at 0x2518fda5378: f1(5)> 0
<Greenlet at 0x2518fda5598: f2(5)> 0
<Greenlet at 0x2518fda56a8: f3(5)> 0
<Greenlet at 0x2518fda5378: f1(5)> 1
<Greenlet at 0x2518fda5598: f2(5)> 1
<Greenlet at 0x2518fda56a8: f3(5)> 1
<Greenlet at 0x2518fda5378: f1(5)> 2
<Greenlet at 0x2518fda5598: f2(5)> 2
<Greenlet at 0x2518fda56a8: f3(5)> 2
<Greenlet at 0x2518fda5378: f1(5)> 3
<Greenlet at 0x2518fda5598: f2(5)> 3
<Greenlet at 0x2518fda56a8: f3(5)> 3
<Greenlet at 0x2518fda5378: f1(5)> 4
<Greenlet at 0x2518fda5598: f2(5)> 4
<Greenlet at 0x2518fda56a8: f3(5)> 4
123456789101112131415
```

此时实现了多任务。
再次测试–假如`time.sleep(2)`：

```python
import gevent
import time


def f1(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        gevent.sleep(0.5)


def f2(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        gevent.sleep(0.5)


def f3(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        gevent.sleep(0.5)

print('--1--')
g1 = gevent.spawn(f1, 5)
print('--2--')
time.sleep(2)
g2 = gevent.spawn(f2, 5)
print('--3--')
g3 = gevent.spawn(f3, 5)
print('--4--')

g1.join()
g2.join()
g3.join()
123456789101112131415161718192021222324252627282930313233
```

显示：
![result 3](https://img-blog.csdnimg.cn/20200207202134639.gif#pic_center)
显然，`time.sleep()`并没有影响到gevent的运行，在`sleep()`之后才开始执行。
改成`gevent.sleep()`效果就会不同：

```python
import gevent
import time


def f1(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        gevent.sleep(0.5)


def f2(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        gevent.sleep(0.5)


def f3(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        gevent.sleep(0.5)

print('--1--')
g1 = gevent.spawn(f1, 5)
print('--2--')
gevent.sleep(2)
g2 = gevent.spawn(f2, 5)
print('--3--')
g3 = gevent.spawn(f3, 5)
print('--4--')

g1.join()
g2.join()
g3.join()
123456789101112131415161718192021222324252627282930313233
```

显示：
![result4](https://img-blog.csdnimg.cn/20200207202200348.gif#pic_center)
即影响到了协程的执行，在实际中会用耗时的IO操作代替`gevent.sleep()`。
如果代码中存在大量的`time.sleep()`等耗时操作代码，不用全部手动改为`gevent.sleep()`，可以使用模块中的类实现：

```python
import gevent
import time
from  gevent import monkey


# 将程序中用到的耗时操作转换为gevent中实现的模块
monkey.patch_all() # 相当于打补丁


def f1(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        time.sleep(0.5)


def f2(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        time.sleep(0.5)


def f3(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        time.sleep(0.5)

print('--1--')
g1 = gevent.spawn(f1, 5)
print('--2--')
time.sleep(2)
g2 = gevent.spawn(f2, 5)
print('--3--')
g3 = gevent.spawn(f3, 5)
print('--4--')

g1.join()
g2.join()
g3.join()
1234567891011121314151617181920212223242526272829303132333435363738
```

显示：
![result5](https://img-blog.csdnimg.cn/20200207202224569.gif#pic_center)
可以实现一样的效果。

### 4.gevent简单应用

```python
import gevent
from gevent import monkey
monkey.patch_all()
import requests


def download(url):
    print('to get:%s' % url)
    res = requests.get(url)
    data = res.text
    print('Got:', len(data), url)


g1 = gevent.spawn(download, 'http://www.baidu.com')
g2 = gevent.spawn(download, 'https://www.csdn.net/')
g3 = gevent.spawn(download, 'https://stackoverflow.com')

g1.join()
g2.join()
g3.join()
1234567891011121314151617181920
```

显示：
![result6](https://img-blog.csdnimg.cn/20200207202258540.gif#pic_center)
`import requests`必须在`monkey.patch_all()`之后，否则会有警告信息。
进一步简化代码：

```python
import gevent
from gevent import monkey
monkey.patch_all()
import requests


def download(url):
    print('to get:%s' % url)
    res = requests.get(url)
    data = res.text
    print('Got:', len(data), url)


gevent.joinall([
    gevent.spawn(download, 'http://www.baidu.com'),
    gevent.spawn(download, 'https://www.csdn.net/'),
    gevent.spawn(download, 'https://stackoverflow.com')
])
123456789101112131415161718
```

执行结果与之前相同。
可得，协程是并发的，因为是属于单线程完成多个任务。

### 5.进程、线程和协程对比

- 进程是**资源分配**的单位；
- 线程是**操作系统调度**的单位；
- 进程切换需要的资源很大、效率很低；
- 线程切换需要的资源一般、效率一般（在不考虑GIL的情况下）；
- 协程切换任务资源很小、效率高；
- 多进程、多线程根据CPU核数不同可能是并行的，但是协程是在一个线程中，所以是并发。

# Python全栈（四）高级编程技巧之11.Python全局解释器锁和高级编程技巧总结

## 一、Python-GIL（全局解释器锁）

面试题：

> 描述Python GIL的概念,以及它对Python多线程的影响

在Linux中执行
（1）主线程死循环

```python
while True:
    pass
12
```

通过**htop**查看CPU使用情况，相当于Windows中的任务管理器。
显示：
![主线程](https://img-blog.csdnimg.cn/2020020910142090.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
运行之后CPU占满，利用率100%。
（2）2个线程死循环

```python
import threading

# 子线程死循环
def test():
    while True:
        pass

t1 = threading.Thread(target=test)
t1.start()

# 主线程死循环
while True:
    pass
12345678910111213
```

显示：
![2线程](https://img-blog.csdnimg.cn/20200209101138876.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
运行之后，如果是双核，两个CPU各占一半，利用率50%。
解释：
两个线程是交叉运行的，任意时刻只有一个线程处于运行状态。
（3）2个进程死循环

```python
import multiprocessing

def deapLoop():
    while True:
        pass
        
# 子进程死循环
p1 = multiprocessing.Process(target=deapLoop)
p1.start()

# 主进程死循环
while True:
    pass
12345678910111213
```

显示：
![2进程](https://img-blog.csdnimg.cn/20200209101839207.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
运行之后，如果是双核，CPU都占满，利用率100%。
解释：
进程是真正利用多核的并发优势，多个CPU同时利用。

Python代码需要通过Python解释器翻译成计算机语言，才能在CPU中处理运行。
解释器有CPython（默认）、JPython（Java语言编写）、IPython等。
GIL只出现在CPython中。

**参考答案**：

- Python语言和GIL没有关系，仅仅是由于历史原因在CPython虚拟机，难以移除GIL。
- GIL即全局解释器锁，每个线程在执行的过程都需要先获取GIL，保证同一时刻只有一个线程可以执行代码。
- 线程释放GIL锁的情况：
  在IO操作等可能会引起阻塞的system call之前，可以暂时释放GIL，但在执行完毕后，必须重新获取GIL，Python3使用计时器。
- Python使用多进程是可以利用多核的CPU资源的。
- 多线程爬取比单线程性能有提升,因为遇到IO阻塞会自动释放GIL锁。

## 二、高级编程技巧总结

### 1.区别可变数据类型和不可变数据类型

从对象的**内存地址**方向：

- 可变数据类型：
  内存不变，值可以改变，如列表、字典、集合等。
- 不可变数据类型：
  值变化时，内存地址也跟着变化，如整型、字符串、元组、布尔型等。

### 2.Python垃圾回收机制

Python中可以不事先声明变量而直接赋值。
垃圾回收有3种方式：

- 引用计数机制：
  当引用计数为0时，自动将变量回收。
  可能会存在循环引用。
- 标记清除
- 分代回收
  引用计数回顾：

```python
import sys

a = []
print(sys.getrefcount(a))


def func(a):
    print(sys.getrefcount(a))


func(a)

123456789101112
```

打印

```python
2
4
12
```

### 3.Python中函数或成员变量包含单下划线前缀结尾和双下划线前缀结尾的区别

- 单下划线前缀结尾:
  _代表保护变量，不希望更改；
- 双下划线前缀结尾：
  __代表私有成员（私有属性和私有方法），类外不能访问。

测试：

```python
class Person(object):
    def __init__(self):
        self.__age = 12
        self._age = 13

    def _set_age(self, age):
        self.__age = age


if __name__ == '__main__':
    p = Person()
    try:
        # 不能直接访问私有属性
        print(p.__age)
    except:
        # 通过该方式访问私有属性，不建议
        print(p._Person__age)
    print(p._age)
123456789101112131415161718
```

打印：

```python
12
13
12
```

### 4.判断一个对象是函数还是方法

```python
from types import MethodType, FunctionType


class Demo(object):
    def __init__(self):
        pass

    def foo(self):
        pass


def foo2():
    pass


print(isinstance(Demo().foo, FunctionType))
print(isinstance(foo2, FunctionType))
print(isinstance(Demo().foo, MethodType))
print(isinstance(foo2, MethodType))
12345678910111213141516171819
```

打印

```python
False
True
True
False
1234
```

### 5.super函数的用法

super：
调用父类的方法，是按照**MRO**算法调用的。

```python
class A(object):
    def __init__(self):
        print('A')


class B(A):
    def __init__(self):
        print('B')
        super(B, self).__init__()


class C(A):
    def __init__(self):
        print('C')
        super().__init__()


class D(B, C):
    def __init__(self):
        print('D')
        super().__init__()


if __name__ == '__main__':
    d = D()
    print(D.__mro__)
1234567891011121314151617181920212223242526
```

打印

```python
D
B
C
A
(<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)
12345
```

### 6.使用isinstance和type的区别

isinstance和type是用来查看对象类型的。

- isinstance**考虑类**的继承关系
- type**不会考虑类**的继承关系

### 7.创建大量实例节省内存

用`__slots__`（列表、元组等）；
单例模式：
只实例化一次。

### 8.上下文管理器

类中实现`__enter__`和`__exit__`方法。

```python
class Sample(object):
    def __enter__(self):
        # 获取资源
        print('Start')
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        # 释放资源
        print('End')

    def demo(self):
        print('demo')


with Sample() as s:
    s.demo()
12345678910111213141516
```

打印

```python
Start
demo
End
123
```

### 9.判断一个对象中是否具有某个属性

用`hasattr(object, attr)`。

```python
class Demo(object):
    name = 'Corley'
    def run(self):
        return 'hello'


d = Demo()

print(hasattr(d, 'name'))
print(hasattr(d, 'run'))
print(hasattr(d, 'age'))
1234567891011
```

打印

```python
True
True
False
123
```

### 10.property动态属性的使用

- 装饰器
- `property(get, set, ...)`

### 11.使用type创建自定义类

`type(name, bases, attr)`
name：类名；
bases：继承对象的元组；
attr：类属性、类方法、静态方法的字典。

### 12.生成器的创建

- `yield`关键字创建
- 生成器表达式()

启动生成器：
\- `next()`方法
\- `send()`方法：
如果前面没有调用`next()`时，第一次调用`send()`必须传None。

### 13.TCP和UDP的区别

- TCP面向连接，UDP无连接
- TCP安全可靠，UDP不保证可靠交付
- UDP实时性更好，适用于高速传输
- TCP点到点，UDP支持一对一、一对多、多对多
- TCP对系统资源要求较高，UDP相对要求较低

### 14.TCP服务端通信流程

- 创建套接字
- 绑定信息
- listen主动变被动
- accept等待客户端连接
- 收发数据
- 关闭套接字

### 15.创建线程的两种方式

- 普通创建–`threading.Thread()`
- 类的继承–继承`threading.Thread`

例如：

```python
import threading
import time

class A(threading.Thread):
    def __init__(self, name):
        super().__init__(name=name)

    def run(self) -> None:
        for i in range(5):
            print(i)


if __name__ == '__main__':
    a = A('a')
    a.start()
123456789101112131415
```

打印

```python
0
1
2
3
4
12345
```

### 16.线程资源竞争，以及解决方案

线程之间共享资源，一个线程可能在运行的时候暂停，另一个线程执行并占用资源，导致竞争资源。
解决：
加互斥锁。

```python
# 创建锁
mutex = threading.Lock()
rmutex = threading.RLock() # 可重入锁
# 上锁
mutex.acquire()
# 解锁
mutex.release()
1234567
```

可能会出现死锁。

### 17.死锁出现的原因

两个对象之间互相等待对方释放锁。

### 18.进程之间的通信，以及进程池中的进程通信

- 进程之间的通信：
  `multiprossing.Queue`
- 进程池中的进程通信：
  `multiprocessing.Manager().Queue`

### 19.同步、异步、阻塞、非阻塞

- 同步：
  多个任务之间有先后顺序
- 异步：
  多个任务之间无先后顺序，可同时执行
- 阻塞：
  停止，不继续向前执行
- 非阻塞：
  不停止，继续向前执行

### 20.进程、线程、协程对比

- 进程：
  资源分配的单位；
  需要的资源较大；
  能真正发挥多核的优势；
  只有进程才可能是并行。
- 线程：
  操作系统调度的单位。
- 协程：
  占有资源最少。

### 21.Python GIL的概念,以及它对Python多线程的影响

GIL：
全局解释器锁，每个线程在执行的过程都需要先获取GIL，保证同一时刻只有一个线程可以执行代码。

在IO操作等可能会引起阻塞的system call之前，可以暂时释放GIL，但在执行完毕后，必须重新获取GIL。