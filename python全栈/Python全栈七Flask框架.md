# Python全栈（七）Flask框架之1.Flask简介与URL和视图介绍

## 一、虚拟环境介绍

### 1.虚拟环境与全局环境

有时候安装了一个Python库，可能在IDE如PyCharm中不能使用，这是因为：
通过pip安装的库默认一般在全局环境中，而PyCharm一般会默认创建虚拟环境，所以两者的环境不一致，导致安装的包不能正常导入使用，解决办法有2种：

- 在PyCharm虚拟环境中安装库，使库位于虚拟环境中
- 将PyCharm的环境设置为全局环境，即我们通常使用的Python，设置为Python的安装目录即可
  设置示意如下：
  ![PyCharm 设置编译环境](https://img-blog.csdnimg.cn/2020040912505946.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

### 2.为什么需要虚拟环境

一般情况下，Python第三方库安装是直接通过`pip install xxx`的方式进行安装的，这样安装会将库安装到系统级的Python环境中。
但是有时可能会面临这样的问题：如果现在用Django 1.10.x写了个网站，但是同时有一个Django 0.9开发的项目需要维护，并且有可能Django 1.10不再兼容Django 0.9的一些语法了，这就需要同时拥有Django 1.10和Django 0.9两套环境，我们可以通过虚拟环境来解决这个问题。

### 3.虚拟环境的安装和简单操作

虚拟环境管理有很多工具，这里我选择pipenv。

#### pipenv的安装

命令：

- Windows下

```bash
pip install pipenv
1
```

- Mac下

```bash
brew install pipenv
1
```

- Linux下

```bash
pip install pipenv
1
```

#### 创建虚拟环境

安装之后即可创建虚拟环境。
创建虚拟环境使用命令`pipenv shell`，如下：
![pipenv shell](https://img-blog.csdnimg.cn/20200409125144218.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

出现图中所标提示`Flask_Framework-rL0Lvhvz`及说明安装成功，此时再运行`pip list`可以看到虚拟环境中默认安装的库：

```bash
Package    Version
---------- -------
pip        20.0.2
setuptools 46.1.3
wheel      0.34.2
12345
```

**不能同时使用**全局环境和虚拟环境的库，只能选择使用其中一个。
虚拟环境默认会安装到系统盘（C盘）下的当前用户目录下的 **.virtualenvs** 目录下，如果想指定安装到其他目录，可以设置系统环境变量，在系统变量中添加变量，变量名为WORKON_HOME，值为需要指定安装的目录，示意如下：
![pipenv 设置安装环境变量](https://img-blog.csdnimg.cn/20200409125302786.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
此时再安装虚拟环境，即会安装到指定的目录下。

虚拟环境安装好之后，需要在PyCharm中设置虚拟环境为当前创建的虚拟环境，即定位选择虚拟环境下的**python.exe**可执行文件，方法与前面相同。

需要在虚拟环境中通过命令`pip install flask`安装Flask，再查看安装的库，结果如下：

```bash
Package      Version
------------ -------
click        7.1.1
Flask        1.1.2
itsdangerous 1.1.0
Jinja2       2.11.1
MarkupSafe   1.1.1
pip          20.0.2
setuptools   46.1.3
Werkzeug     1.0.1
wheel        0.34.2
1234567891011
```

显然，在安装flask时，安装了存在依赖关系的其他库。
如果电脑中同时拥有Python3和Python2，可以指定版本：

```bash
pipenv --three  # 泛指Python3的版本 
pipenv --two    # 泛指Python2的版本 
pipenv --python 3.7 # 指定Python具体版本
123
```

虚拟环境管理

```bash
pipenv shell    # 如果虚拟环境已存在则进入虚拟环境，否则创建并进入虚拟环境
exit            # 退出虚拟环境
pipenv --rm     # 删除整个环境  不会删除pipfile
123
```

#### pipfile和pipfile.lock

在创建虚拟环境后，虚拟环境目录下会生成**pipfile**文件，内容如下：

> [[source]]
> name = “pypi”
> url = “https://pypi.org/simple”
> verify_ssl = true
>
> [dev-packages]
>
> [packages]
> requests = “*”
> django = “*”
>
> [requires] python_version = “3.7”

参数说明：

- url
  可以指定国内pip源，在默认情况使用国外源下载库可能会很慢
- dev-packages
  开发环境
- packages
  生产环境
- django = “*”
  *表示最新版本
- requires
  Python版本

**pipfile.lock**详细记录环境依赖，并且使用了Hash算法以保证完整的对应关系。

如果需要将安装的库记录到Pipfile中，可以使用`pip install --dev 库名`将库安装到开发环境。

在虚拟环境中用run参数运行项目示例如下：

```bash
pipenv run python manage.py runserver
1
```

pipenv有一个缺点：
lock不稳定而且时间非常长，所以安装包的时候记得加上`--skip-lock`，如下：

```bash
pipenv install django --skip-lock
1
```

最后开发完成要提交到仓库的时候再执行`pipenv lock`命令。

## 二、Flask介绍

### 1.Flask简介

flask是一款非常流行的Python Web框架，诞生于2010年，作者是Armin Ronacher，这个项目最初只是作者在愚人节的一个玩笑，后来由于非常受欢迎，逐渐成为一个正式的项目。
flask自2010年发布第一个版本以来，大受欢迎，深得开发者的喜爱，并且在多个公司已经得到了应用，flask能如此流行的原因，可以分为以下几点：

- 微框架、简洁，只做它需要做的，灵活度非常高，给开发者提供了很大的扩展性。

  Flask不会帮开发者做太多的决策，一切都可以按照自己的意愿进行更改。

  - 使用Flask开发数据库的时候，具体是使用SQLAlchemy还是MongoEngine，选择权完全掌握在开发者自己的手中。区别于Django，Django内置了非常完善和丰富的功能，并且如果想替换成开发者想要的，要么不支持，要么非常麻烦。
  - 把默认的Jinija2模板引擎替换成其他模板引擎都是非常容易的。

- Flask和相应的插件写得很好。

- 开发效率非常高，比如使用SQLAlchemy的ORM操作数据库可以节省开发者书写大量sql的时间。

### 2.第一个Flask程序

```python
from flask import Flask

# 传入__name__初始化一个Flask实例
app = Flask(__name__)


# 装饰器，将当前路由映射到指定函数
@app.route('/')
def hello_world():
    return 'hello world'


if __name__ == '__main__':
    app.run()
1234567891011121314
```

打印：

```python
 * Serving Flask app "first_flask" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
123456
```

此时已经创建服务，在浏览器中打开http://127.0.0.1:5000/即可看到：
![flask first program](https://img-blog.csdnimg.cn/20200409125417609.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
并且在开启的服务状态栏下会看到请求的记录，如：

```python
127.0.0.1 - - [09/Apr/2020 07:54:10] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [09/Apr/2020 07:54:10] "GET /favicon.ico HTTP/1.1" 404 -
12
```

说明：

- `@app.route('/')`装饰器映射URL和执行的函数。这个设置将URL映射到指定的函数上，例中指定当前路由为根目录，如果为根目录时也可以不写 **/**，但是尽量写上以示区别。
- `app.run()`是让flask项目运行起来，可以指定主机号和端口号。
  默认的host是**127.0.0.1**，port为**5000**，**host=0.0.0.0**可以让其他电脑也能访问到该网站，port指定访问的端口。

## 三、设置Debug模式

默认情况下flask不会开启DEBUG模式，开启DEBUG模式后，flask会在每次保存代码的时候自动的重新载入代码，并且如果代码有错误，会在终端提示。
在`hello_world()`函数中加入错误代码进行测试：

```python
from flask import Flask

app = Flask(__name__)


# 装饰器，将当前路由映射到指定函数
@app.route('/')
def hello_world():
    result = 1 / 0
    return 'hello world'


if __name__ == '__main__':
    app.run()
1234567891011121314
```

重新运行开启服务后，会发现：
![flask server error](https://img-blog.csdnimg.cn/20200409125506726.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
在日志中也会发现报错：

```python
[2020-04-09 08:07:17,881] ERROR in app: Exception on / [GET]
Traceback (most recent call last):
  File "C:\Users\Lenovo\.virtualenvs\Flask_Framework-rL0Lvhvz\lib\site-packages\flask\app.py", line 2447, in wsgi_app
    response = self.full_dispatch_request()
  File "C:\Users\Lenovo\.virtualenvs\Flask_Framework-rL0Lvhvz\lib\site-packages\flask\app.py", line 1952, in full_dispatch_request
    rv = self.handle_user_exception(e)
  File "C:\Users\Lenovo\.virtualenvs\Flask_Framework-rL0Lvhvz\lib\site-packages\flask\app.py", line 1821, in handle_user_exception
    reraise(exc_type, exc_value, tb)
  File "C:\Users\Lenovo\.virtualenvs\Flask_Framework-rL0Lvhvz\lib\site-packages\flask\_compat.py", line 39, in reraise
    raise value
  File "C:\Users\Lenovo\.virtualenvs\Flask_Framework-rL0Lvhvz\lib\site-packages\flask\app.py", line 1950, in full_dispatch_request
    rv = self.dispatch_request()
  File "C:\Users\Lenovo\.virtualenvs\Flask_Framework-rL0Lvhvz\lib\site-packages\flask\app.py", line 1936, in dispatch_request
    return self.view_functions[rule.endpoint](**req.view_args)
  File "xxx/first_flask.py", line 9, in hello_world
    result = 1 / 0
ZeroDivisionError: division by zero
127.0.0.1 - - [09/Apr/2020 08:07:17] "GET / HTTP/1.1" 500 -
123456789101112131415161718
```

这与显然很麻烦，每次修改之后必须重新运行，而且错误信息在日志中才能看到。
我们可以开启Debug模式，这样每次修改代码后都会载入代码重新运行，并且代码有问题时会显示错误信息。

开启Debug模式有几种方式：

- 在`run()`方法中设置debug参数为True

```python
if __name__ == '__main__':
    app.run(debug=True)
12
```

- 设置app对象实例的属性为True

```python
if __name__ == '__main__':
    app.debug = True
    app.run()
123
```

- 通过配置参数config设置

```python
if __name__ == '__main__':
    app.config.update(DEBUG=True)
    app.run()
123
```

config是继承自字典类型的，所以可以使用字典的`update()`方法。

开启Debug模式测试如下：

```python
from flask import Flask

app = Flask(__name__)


# 装饰器，将当前路由映射到指定函数
@app.route('/')
def hello_world():
    result = 1 / 0
    return 'hello world'


if __name__ == '__main__':
    app.run(debug=True)
1234567891011121314
```

此时再看网页：
![flask error debug](https://img-blog.csdnimg.cn/20200409125711335.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
并且控制台中也会提示已开启Debug模式：

```python
 * Serving Flask app "first_flask" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 313-629-160
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
123456789
```

现在每次修改完代码保存之后，都会自动加载代码重启服务，不需要再手动关闭服务再重启了。

在开启了DEBUG模式后，当程序有异常会进入错误堆栈模式，第一次点击某个堆栈想查看变量值的时候，页面会弹出一个对话框，提示输入PIN值，比如在刚刚启动的项目中的PIN值为313-629-160，输入这个值后，Werkzeug会把这个PIN值作为cookie的一部分保存起来，并在8小时后过期，8小时内不需要再输入PIN值。这样做的目的是为了**提高安全性**，让调试模式下的攻击者更难攻击到本站。

此时可以在报错的网页中进行一些简单的Debug，使用控制台提供的PIN操作示意如下：
![flask webpage debug](https://img-blog.csdnimg.cn/20200409125738975.gif#pic_center)
Debug模式是在开发环境中开启的，开发完成上线之后要关闭Debug模式，因为DEBUG模式会带来非常大的安全隐患。

## 四、配置与配置文件

Flask项目的配置，都是通过**app.config**对象来进行配置的。
比如要配置一个项目处于DEBUG模式下，那么可以使用`app.config['DEBUG'] = True`来进行设置，那么Flask项目将以DEBUG模式运行。
在Flask项目中，有四种方式进行项目的配置。

### 1.直接硬编码

```python
app = Flask(__name__)
app.config['DEBUG'] = True
12
```

硬编码的方式不灵活，不便于进行复用。

### 2.通过update()方法

因为app.config是flask.config.Config的实例，而Config类是继承自dict，因此可以通过`update()`方法进行配置。

```python
app.config.update(
   DEBUG=True,
   SECRET_KEY='...'
)
1234
```

### 3.通过from_object()方法

如果配置项特别多，可以把所有的配置项都放在一个模块中，然后通过加载模块的方式进行配置，假设有一个settings.py模块，专门用来存储配置项的，此你可以通过`app.config.from_object()`方法进行加载，并且该方法既可以接收模块的的字符串名称，也可以模块对象。
有两种形式：

```python
# 1. 通过模块字符串
app.config.from_object('settings')
# 2. 通过模块对象
import settings
app.config.from_object(settings)
12345
```

添加配置文件后，将配置项都放入该文件中，其他文件直接引用该配置文件中的配置项，提高了代码的复用性、降低了耦合度，同时，在配置文件中修改了配置项时，其他代码中均不需要修改，从而提高了代码的灵活性。
新建config.py文件，添加一些配置项如下：

```python
# 设置Debug模式为True
DEBUG = True

# 指定HOST
HOST = '127.0.0.1'
12345
```

在flask文件中导入：

```
from flask import Flask
import config

app = Flask(__name__)


# 装饰器，将当前路由映射到指定函数
@app.route('/')
def hello_world():
    result = 1 / 0
    return 'hello world'


if __name__ == '__main__':
    app.config.from_object(config)
    app.run()
12345678910111213141516
```

再运行，也能开启Debug模式。
也可以通过字符串形式导入：

```python
if __name__ == '__main__':
    app.config.from_object('config')
    app.run()
123
```

此时不需要再导入config模块。

### 4.通过from_pyfile()方法

`app.config.from_pyfile()`方法传入一个文件名，通常是以.py结尾的文件，但也不限于只使用.py后缀的文件。
通过导入Python文件的形式导入配置文件：

```python
if __name__ == '__main__':
    app.config.from_pyfile('config.py')
    app.run()
123
```

`from_pyfile()`方法有一个silent参数，设置为True时，如果配置文件不存在也不会报错；
不仅支持Python格式的配置文件，也支持.ini等格式。

## 五、URL与函数的映射

从前面的例子中，我们可以看到，一个URL要与执行函数进行映射，使用的是`@app.route`装饰器。
`@app.route`装饰器中，可以指定URL的规则来进行更加详细的映射，比如现在要映射一个文章详情的URL，文章详情的URL是`/article/id/`，id有可能为1、2、3…，那么可以通过以下方式：

```python
@app.route('/article/<id>')
def article(id):
   return '%s article detail' % id
123
```

其中，尖括号是固定语法，表示地址中传入的参数，默认的数据类型是字符串。
如果需要限制参数类型，则要写成`限制类型:变量名`，其中限制类型包括以下几种：

- string
  默认的数据类型，接受任何没有斜杠/的字符串。
- int
  整型。
- float
  浮点型。
- path
  和string类似，但是可以传递斜杠/。
- uuid
  uuid类型的字符串。
- any
  可以同时指定多种路径。

新增路由域函数映射测试：

```python
from flask import Flask


app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/corley')
def hello_corley():
    return '这是我的第一个Flask页面'

if __name__ == '__main__':
    app.run(debug=True)
1234567891011121314151617
```

显示：
![flask url view map](https://img-blog.csdnimg.cn/20200409125935667.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，在地址中访问http://127.0.0.1/corley可以访问到，因为在flask中已经定义了。
同时还可以动态传入参数：

```python
from flask import Flask


app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/corley')
def hello_corley():
    return '这是我的第一个Flask页面'

@app.route('/list/<aid>')
def article_list(aid):
    return '这是第{}篇文章'.format(aid)

if __name__ == '__main__':
    app.run(debug=True)
123456789101112131415161718192021
```

显示：
![flask url view map params](https://img-blog.csdnimg.cn/20200409130509106.gif#pic_center)
显然，因为未定义 **/list** 所以不能访问http://127.0.0.1:5000/list；
此时已经可以根据传入的参数动态显示视图，但是并未对数据类型进行限制，可以增加对数据类型的限制：

```python
from flask import Flask


app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/corley')
def hello_corley():
    return '这是我的第一个Flask页面'

@app.route('/list/<int:aid>')
def article_list(aid):
    return '这是第{}篇文章'.format(aid)

if __name__ == '__main__':
    app.run(debug=True)
123456789101112131415161718192021
```

显示：
![flask url view map params int](https://img-blog.csdnimg.cn/20200409130536416.gif#pic_center)
显然，此时参数只能是整型数字了。

一般情况下参数中不能含有 **/**，要想含有 **/**，必须限制为path类型，除了此区别，path与string类型基本一样。
示例如下：

```python
from flask import Flask


app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/corley')
def hello_corley():
    return '这是我的第一个Flask页面'

@app.route('/list/<int:aid>')
def article_list(aid):
    return '这是第{}篇文章'.format(aid)

@app.route('/list/<path:aid>')
def comment_list(aid):
    return '这是第{}个评论'.format(aid)

if __name__ == '__main__':
    app.run(debug=True)
12345678910111213141516171819202122232425
```

显示：
![flask url view map params path](https://img-blog.csdnimg.cn/20200409130606372.gif#pic_center)
显然，如果参数为数字时，匹配`article_list(aid)`函数，如果为字符串类型或者参数中含有 **/** 时匹配`comment_list(aid)`函数。

访问两个路径用同一个函数可以用any来限制，示例如下：

```python
from flask import Flask


app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/corley')
def hello_corley():
    return '这是我的第一个Flask页面'

@app.route('/list/<int:aid>')
def article_list(aid):
    return '这是第{}篇文章'.format(aid)

@app.route('/list/<path:aid>')
def comment_list(aid):
    return '这是第{}个评论'.format(aid)

@app.route('/<any(notice,follow):url_path>/')
def message(url_path):
    if url_path == 'notice':
        return '当前路径是notice'
    else:
        return '当前路径是follow'


if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819202122232425262728293031323334
```

显示：
![flask url view map params any](https://img-blog.csdnimg.cn/20200417170202496.gif#pic_center)
此时定义路由时需要在路径最后添加 **/** 才能正常访问，如`/<any(notice,follow):url_path>/`。

如果不想指定子路径来传递参数，也可以通过 **?=** 的形式来传递参数，例如：`/article?id=xxx`，这种情况下，可以通过`request.args.get('id')`来获取id的值。如果是post方法，则可以通过`request.form.get('id')`来进行获取。
在flask中添加这类的地址参数需要先从flask中导入request，示例如下：

```python
from flask import Flask
from flask import request


app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'hello world'


@app.route('/corley')
def hello_corley():
    return '这是我的第一个Flask页面'

@app.route('/list/<int:aid>')
def article_list(aid):
    return '这是第{}篇文章'.format(aid)

@app.route('/list/<path:aid>')
def comment_list(aid):
    return '这是第{}个评论'.format(aid)

@app.route('/<any(notice,follow):url_path>/')
def message(url_path):
    return url_path

@app.route('/wd')
def baidu_search():
    return request.args.get('keyword')

if __name__ == '__main__':
    app.run(debug=True)
12345678910111213141516171819202122232425262728293031323334
```

显示：
![flask url view map params wd](https://img-blog.csdnimg.cn/20200409130723499.gif#pic_center)
显然，如果`request.args.get()`中传入的参数未在URL中传入，会报错，因为URL未传入keyword参数，所以`request.args.get()`方法的值为空，即视图函数`baidu_search()`返回的值为空，不能渲染，所以会报错。

# Python全栈（七）Flask框架之2.Flask视图和模板

## 一、url_for构造URL和指定地址末尾的/

### 1.通过url_for构造URL

一般我们通过一个URL就可以映射到某一个函数。反过来，知道一个函数时，也可以获得对应的URL，`url_for()`函数可以进行反转、实现这个功能。
`url_for()`函数接收一个及以上的参数，第一个参数是函数名，接收URL规则对应的视图函数名，如果函数中有参数，则将这些参数传入`url_for()`函数第一个参数的后面。

进行测试：

```python
from flask import Flask,url_for

app = Flask(__name__)


@app.route('/')
def index():
    # 根据函数的名字进行反转，得到函数对应的路由
    print(url_for('article_list',aid=2))
    return 'Hello World'


@app.route('/article/<aid>')
def article_list(aid):
    return 'article %s' % aid


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920
```

运行开启服务后访问http://127.0.0.1:5000/，打印

```
127.0.0.1 - - [09/Apr/2020 16:46:24] "GET /article/3 HTTP/1.1" 200 -
127.0.0.1 - - [09/Apr/2020 16:46:30] "GET / HTTP/1.1" 200 -
/article/2
123
```

显然，此时通过`url_for()`方法得到了一个函数对应的路由，并显示在日志中；
有参数时，调用`url_for()`方法也要传入参数。

通过`url_for()`函数来构建URL从而在代码中拼URL的好处有两点：

- 将来如果修改了URL，但没有修改该URL对应的函数名，就可以直接通过`url_for()`函数来获取地址而不用手动替换所有对应的URL。
- `url_for()`函数可以转义一些特殊字符和unicode字符串，这在有时候显得很有用。

进一步测试：

```python
from flask import Flask,url_for

app = Flask(__name__)


@app.route('/')
def index():
    print(url_for('article_list',aid=2))
    print(url_for('notice'))
    print(url_for('follow', fid=2,page=5))
    return 'Hello World'


@app.route('/article/<aid>')
def article_list(aid):
    return 'article %s' % aid

@app.route('/notice')
def notice():
    return 'Notice is as follows'

@app.route('/follows/<fid>')
def follow(fid):
    return 'Follower %s' % fid

if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819202122232425262728
```

再次访问，打印：

```python
/article/2
/notice
/follows/2?page=5
123
```

显然，如果`url_for()`方法中提供的参数多于传入的函数的参数时，以查询参数的形式返回路由。
**斜杠/** 转码测试：

```python
from flask import Flask,url_for

app = Flask(__name__)


@app.route('/')
def index():
    print(url_for('follow', fid=2,page=5,param='/'))
    return 'Hello World'


@app.route('/article/<aid>')
def article_list(aid):
    return 'article %s' % aid

@app.route('/notice')
def notice():
    return 'Notice is as follows'

@app.route('/follows/<fid>')
def follow(fid):
    return 'Follower %s' % fid

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526
```

打印：

```python
127.0.0.1 - - [09/Apr/2020 17:07:09] "GET / HTTP/1.1" 200 -
/follows/2?page=5&param=%2F
12
```

显然，此时 **/** 被编码成ASCII码 **%2F**。

### 2.指定URL末尾的斜杠

有些URL的末尾是有斜杠的，有些URL末尾是没有斜杠的，这其实是两个不同的URL。
例如定义路由`@app.route('/article/')`，当访问一个结尾不带斜线的URL`/article`，会被重定向到带斜线的URL`/article/`上去。
但是在定义路由的时候，如果在末尾没有加上斜杠，在访问的时候又加上了斜杠，这时候就会抛出一个404错误页面。例如`@app.route("/article")`没有在末尾加斜杠，此时只能访问到`/article`，在访问`/article/`的时候，会抛出一个404错误。
因此建议**在定义路由时加上末尾的/**，这样在访问时加 **/** 与否都可以访问到。

## 二、指定HTTP方法

默认情况下定义的路由只能使用GET请求，可以在`@app.route()`中传入一个参数methods来指定本方法支持的HTTP方法。
如果想用post请求，可以使用Postman工具来模拟，可点击https://download.csdn.net/download/CUFEECR/12319280进行下载，点击安装包进行安装，安装完成后直接打开运行，即可进行各种方法的网络请求。
先进行测试：

```python
from flask import Flask,url_for,request

app = Flask(__name__)


@app.route('/')
def index():
    return 'Hello World'


@app.route('/article/<aid>')
def article_list(aid):
    return 'article %s' % aid

@app.route('/login/')
def login():
    return 'login'


if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819202122
```

服务运行之后，在Postman中进行测试，如下：
![flask http method post test](https://img-blog.csdnimg.cn/20200410122133183.gif#pic_center)
显然，此时Post方法不被允许，在路由中添加方法，再测试：

```python
from flask import Flask,url_for,request

app = Flask(__name__)


@app.route('/')
def index():
    return 'Hello World'


@app.route('/article/<aid>')
def article_list(aid):
    return 'article %s' % aid

@app.route('/login/', methods=['GET','POST'])
def login():
    return 'login'


if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819202122
```

再在Postman中测试：
![flask http method post addmethod test](https://img-blog.csdnimg.cn/20200410122201992.gif#pic_center)
显然，装饰器将让login的URL既能支持GET方法又能支持POST方法，此时可以用POST方法请求到。
在给定了methods参数后，就只能用方法列表中的方法请求，如果列表中没有get方法则不能再用get方法进行请求。
还可以传入参数，在flask项目中接收参数。
测试如下：

```python
from flask import Flask,url_for,request

app = Flask(__name__)


@app.route('/')
def index():
    return 'Hello World'


@app.route('/article/<aid>')
def article_list(aid):
    return 'article %s' % aid

@app.route('/login/',methods=['GET','POST'])
def login():
    print(request.form.get('name'))
    return 'login'


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223
```

Postman中请求：
![flask http method post params test](https://img-blog.csdnimg.cn/20200410122336531.gif#pic_center)
查看控制台，会看到：

```python
127.0.0.1 - - [09/Apr/2020 19:00:14] "POST /login/ HTTP/1.1" 200 -
corley
12
```

得到了请求的方法和传递的数据。

接收参数的方法：

- get请求接收参数

```python
request.args.get('xxx')
1
```

- post请求接收参数

```python
request.form.get('xxx')
1
```

## 三、页面跳转和重定向

重定向在页面上的表现就是浏览器从一个页面自动跳转到另外一个页面。
比如用户访问了一个需要权限的页面，但是该用户当前并没有登录，此时应该重定向到登录页面。
重定向分为：

- 永久性重定向
  http的状态码是301，多用于旧网址被废弃了要转到一个新的网址确保用户的访问，一个典型的例子是京东网站，你输入[www.jingdong.com](https://www.jingdong.com/)的时候，会被重定向到[www.jd.com](https://www.jd.com/)，因为**jingdong.com**这个网址已经被废弃了，被改成**jd.com**，所以这种情况下应该用永久重定向。
- 暂时性重定向
  http的状态码是302，表示页面的暂时性跳转。比如访问一个需要权限的网址，如果当前用户没有登录，应该重定向到登录页面，这种情况下，应该用暂时性重定向。

在flask中，重定向是通过`flask.redirect(url, code=302)`这个方法来实现的，url表示需要重定向到的URL，可以结合`url_for()`函数来使用，code表示重定向类型状态码，默认是302也即暂时性重定向，可以修改成301来实现永久性重定向。

模拟访问个人中心页面，未携带name参数而重定向到登录页面：

```python
from flask import Flask,url_for,request,redirect

app = Flask(__name__)


@app.route('/')
def index():
    return 'Hello World'


@app.route('/article/<aid>')
def article_list(aid):
    return 'article %s' % aid

@app.route('/login/')
def login():
    print(request.form.get('name'))
    return 'login'

@app.route('/profile/')
def profile():
    name = request.args.get('name')
    if name:
        return name
    else:
        return redirect('/login')

if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021222324252627282930
```

访问演示如下：
![flask http method redirect  test](https://img-blog.csdnimg.cn/20200410122430666.gif#pic_center)
显然，请求中未携带name参数时会自动重定向到登录页面。
控制台中显示为：

```python
127.0.0.1 - - [09/Apr/2020 19:23:09] "GET /profile/ HTTP/1.1" 302 -
127.0.0.1 - - [09/Apr/2020 19:23:09] "GET /login/ HTTP/1.1" 200 -
None
123
```

显然，请求状态码时302。
但是直接重定向到`/login`显得不够灵活严谨，因为如果登录的地址改变，比如变成`/signin`等，就必须重新修改代码，此时就可以用`url_for()`方法，来传入方法名而不是路由地址，即便地址改变，也可以正常重定向，如下：

```python
from flask import Flask,url_for,request,redirect

app = Flask(__name__)


@app.route('/')
def index():
    return 'Hello World'


@app.route('/article/<aid>')
def article_list(aid):
    return 'article %s' % aid

@app.route('/login/')
def login():
    print(request.form.get('name'))
    return 'login'

@app.route('/profile/')
def profile():
    name = request.args.get('name')
    if name:
        return name
    else:
        # return redirect('/login')
        return redirect(url_for('login'))

if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819202122232425262728293031
```

效果与之前相同。
同时，也可以修改状态码为301，例如`redirect(url_for('login'), code=301)`。

## 四、函数的返回值 - 响应（Response）

视图函数中可以返回以下类型的值：

- Response对象。
- 字符串
  Flask是根据返回的字符串类型重新创建一个**werkzeug.wrappers.Response**对象，Response将该字符串作为主体，状态码为200，MIME类型为text/html，然后返回该Response对象。
- 元组
  传入元组的格式是`(response,status,headers)`，response为一个字符串，status值是状态码，headers是响应头。

如果不是以上三种类型，Flask会通过`Response.force_type(rv,request.environ)`转换为一个请求对象。

之前在函数中返回的都是字符串，现进行返回列表的测试：

```python
from flask import Flask,url_for,request,redirect

app = Flask(__name__)


@app.route('/')
def index():
    return 'Hello World'


@app.route('/article/<aid>')
def article_list(aid):
    return 'article %s' % aid

@app.route('/login/')
def login():
    print(request.form.get('name'))
    return 'login'

@app.route('/profile/')
def profile():
    name = request.args.get('name')
    if name:
        return name
    else:
        # return redirect('/login')
        return redirect(url_for('login'))

@app.route('/about')
def about():
    return ['123']

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526272829303132333435
```

访问http://127.0.0.1:5000/about，会报错：
![flask response list test](https://img-blog.csdnimg.cn/20200410122641250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
提示返回的类型只能是**string、dict、tuple、Response instance或WSGI callable**，其他类型会报错，现换成字典测试：

```python
from flask import Flask,url_for,request,redirect

app = Flask(__name__)


@app.route('/')
def index():
    return 'Hello World'


@app.route('/article/<aid>')
def article_list(aid):
    return 'article %s' % aid

@app.route('/login/')
def login():
    print(request.form.get('name'))
    return 'login'

@app.route('/profile/')
def profile():
    name = request.args.get('name')
    if name:
        return name
    else:
        # return redirect('/login')
        return redirect(url_for('login'))

@app.route('/about')
def about():
    return {'name':'Corley'}

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526272829303132333435
```

显示：
![flask response dict test](https://img-blog.csdnimg.cn/20200410122705550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
也可以返回元组，是元组时，只返回元组的第一个元素；
在一般情况下，返回元组的用法是`return '关于我们',200`，即`return '字符串',状态码`。
返回Response测试：

```python
from flask import Flask,url_for,request,redirect,Response

app = Flask(__name__)


@app.route('/')
def index():
    return 'Hello World'


@app.route('/article/<aid>')
def article_list(aid):
    return 'article %s' % aid

@app.route('/login/')
def login():
    print(request.form.get('name'))
    return 'login'

@app.route('/profile/')
def profile():
    name = request.args.get('name')
    if name:
        return name
    else:
        # return redirect('/login')
        return redirect(url_for('login'))

@app.route('/about')
def about():
    return Response('关于我们')

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526272829303132333435
```

显示：
![flask response response test](https://img-blog.csdnimg.cn/2020041012272758.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
可以看到，给Response传入字符串参数后，返回内容和字符串是类似的；
`Response('关于我们')`相当于`Response('关于我们',status=200,mimetype='text/html')`。
Response的用法是`Response('字符串',状态码,mimetype='')`
也可以用`make_response()`方法创建Response对象并返回，这个方法可以设置额外的数据，比如设置**cookie、header等信息**，测试如下：

```python
from flask import Flask,url_for,request,redirect,Response,make_response

app = Flask(__name__)


@app.route('/')
def index():
    return 'Hello World'


@app.route('/article/<aid>')
def article_list(aid):
    return 'article %s' % aid

@app.route('/login/')
def login():
    print(request.form.get('name'))
    return 'login'

@app.route('/profile/')
def profile():
    name = request.args.get('name')
    if name:
        return name
    else:
        # return redirect('/login')
        return redirect(url_for('login'))

@app.route('/about')
def about():
    return make_response('关于我们')

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526272829303132333435
```

效果与前者是一样的。

## 五、flask模板简介

模板是一个web开发必备的模块，因为在渲染一个网页的时候，并不是只渲染一个纯文本字符串，而是需要渲染一个有富文本标签的页面，这时候就需要使用模板了。
在Flask中，配套的模板是Jinja2，Jinja2的作者也是Flask的作者。这个模板非常强大，并且执行效率高。

函数返回HTML代码测试：

```python
from flask import Flask

app = Flask(__name__)


@app.route('/')
def index():
    return '<input id="kw" name="wd" class="s_ipt" value="" maxlength="255" autocomplete="off">'


if __name__ == '__main__':
    app.run(debug=True)

12345678910111213
```

显示：
![flask template return html test](https://img-blog.csdnimg.cn/20200410122758953.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，是可以解析HTML的，但是这很不利于前后端分离，从而给开发工作带来很大影响。
在一般开发时应该将HTML代码于Python代码分离，将前端代码放入**templates目录**中，再通过`render_template()`方法来渲染模板即可。
在项目目录下创建templates目录，templates目录下创建index.html，如下：

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <input id="kw" name="wd" class="s_ipt" value="" maxlength="255" autocomplete="off">
</body>
</html>
12345678910
```

再测试：

```python
from flask import Flask,render_template

app = Flask(__name__)


@app.route('/')
def index():
    return render_template('index.html')

if __name__ == '__main__':
    app.run(debug=True)

123456789101112
```

显示与之前相同。
`render_template()`方法根据传的值会在默认模板目录templates下寻找匹配模板。
在templates目录下再创建一个子目录profile，里边创建一个模板文件user.html：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>用户中心</title>
</head>
<body>
    <h1>个人中心</h1>
</body>
</html>
12345678910
```

此时要加载user.html必须添加**完整的子文件夹路径**，如下：

```python
from flask import Flask,render_template

app = Flask(__name__)


@app.route('/')
def index():
    return render_template('index.html')


@app.route('/profile/')
def profile():
    return render_template('profile/user.html')

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617
```

显示：
![flask template return template subfolder test](https://img-blog.csdnimg.cn/20200410122843952.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

如果想自定义模板保存目录，可以在初始化FLask时，传入**template_folder参数**来指定指定具体的路径，如下：

```python
app = Flask(__name__, template_folder='./template_files')
1
```

此时你可以将所有模板文件放入template_files文件夹中，但是不建议自定义模板目录。

注意：
在模板代码中尽量不要使用常规的HTML注释，如`<!-- 这是注释 -->`，因为Flask也会解析这种方式注释掉的代码，如果需要使用注释，使用`{# 这是注释 #}`格式的注释。

## 六、flask模板传参

在Flask项目中给模板传入参数：

```python
from flask import Flask,render_template

app = Flask(__name__)


@app.route('/')
def index():
    return render_template('index.html', username='小度', age=18,hobby='Basketball')


@app.route('/profile/')
def profile():
    return render_template('profile/user.html')

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617
```

模板index.html为：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <input id="kw" name="wd" class="s_ipt" value="" maxlength="255" autocomplete="off">
    <h1>{{username}}</h1>
    <h2>{{age}}</h2>
    <h3>{{hobby}}</h3>
</body>
</html>
12345678910111213
```

运行起来之后可以看到：
![flask template pass params test](https://img-blog.csdnimg.cn/20200410122926119.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，传递的参数值被渲染到了页面。
但是当模板中要传递的参数过多的时候，把所有参数放在一个函数中会显得传参部分的代码很臃肿，不是一个好的选择，此时可以进一步改进，使用字典进行包装，并且还可以加两个 ***** 号，来转换成关键字参数。
测试如下：

```python
from flask import Flask,render_template

app = Flask(__name__)


@app.route('/')
def index():
    context = {
        'username':'小度',
        'age':18,
        'hobby':'Basketball'
    }
    return render_template('index.html', context=context)


@app.route('/profile/')
def profile():
    return render_template('profile/user.html')

if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819202122
```

模板中也需要改变：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <input id="kw" name="wd" class="s_ipt" value="" maxlength="255" autocomplete="off">
    <h1>{{context.username}}</h1>
    <h2>{{context.age}}</h2>
    <h3>{{context.hobby}}</h3>
</body>
</html>
12345678910111213
```

此时运行之后与之前效果相同。
当然，模板代码也可以不改变，即直接用username而不是context.username，
此时Python代码应该为：

```python
from flask import Flask,render_template

app = Flask(__name__)


@app.route('/')
def index():
    context = {
        'username':'小度',
        'age':18,
        'hobby':'Basketball'
    }
    return render_template('index.html', **context)


@app.route('/profile/')
def profile():
    return render_template('profile/user.html')

if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819202122
```

即用 ***\*** 修饰context。
context中再嵌套字典测试：

```python
from flask import Flask,render_template

app = Flask(__name__)


@app.route('/')
def index():
    context = {
        'username':'小度',
        'age':18,
        'hobby':{
            'Basketball':'Basketball',
            'Football':'Football',
            'PingPong':'PingPong'
        }
    }
    return render_template('index.html', **context)


@app.route('/profile/')
def profile():
    return render_template('profile/user.html')

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526
```

模板代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <input id="kw" name="wd" class="s_ipt" value="" maxlength="255" autocomplete="off">
    <h1>{{username}}</h1>
    <h2>{{age}}</h2>
    <h4>{{hobby.Basketball}}</h4>
    <h4>{{hobby['Football']}}</h4>
    <h4>{{hobby.PingPong}}</h4>
</body>
</html>
123456789101112131415
```

显示：
![flask template pass params  dict nesting test](https://img-blog.csdnimg.cn/20200410123036304.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
可以看到，嵌套字典取值的方法有2种：

- 属性调用
  例如`hobby.Basketball`。
- 字典取值方法
  例如`hobby['Football']`。

嵌套列表取值测试：

```python
from flask import Flask,render_template

app = Flask(__name__)


@app.route('/')
def index():
    context = {
        'username':'小度',
        'age':18,
        'hobby':{
            'Basketball':'Basketball',
            'Football':'Football',
            'PingPong':'PingPong'
        },
        'books':['Python','Java','PHP']
    }
    return render_template('index.html', **context)


@app.route('/profile/')
def profile():
    return render_template('profile/user.html')

if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021222324252627
```

模板代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <input id="kw" name="wd" class="s_ipt" value="" maxlength="255" autocomplete="off">
    <h1>{{username}}</h1>
    <h2>{{age}}</h2>
    <h4>{{hobby.Basketball}}</h4>
    <h4>{{hobby['Football']}}</h4>
    <h4>{{hobby.PingPong}}</h4>
    <h4>{{books.0}}</h4>
    <h4>{{books[1]}}</h4>
    <h4>{{books.2}}</h4>
</body>
</html>
123456789101112131415161718
```

显示：
![flask template pass params list test](https://img-blog.csdnimg.cn/20200410123120815.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，列表的取值也有两种方法：

- 下标属性取值
  如`books.0`。
- 列表取值方法
  如`books[1]`。

字典和列表的取值都有两种方法，建议使用**属性取值**方法，这在Flask中显得更地道。

# Python全栈（七）Flask框架之3.Flask模板

## 一、Flask模板过滤器

### 1.Jinja2模版内置过滤器

过滤器相当于是一个函数，把当前的变量传入过滤器，过滤器根据自己的功能对变量进行相应的处理，再返回对应的值，并将结果渲染到页面中。
过滤器是通过管道符号 **|** 实现的，例如`{{ name|length }}`返回name的长度。
Jinja2中内置了许多过滤器，常见的如下：

#### abs(value)

返回一个数值的绝对值。
测试如下：

```python
from flask import Flask,render_template

app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    context = {
        'username':'小度',
        'age':-18
    }
    return render_template('index.html', **context)

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617
```

模板index.html代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    <h2>{{age | abs}}</h2>
</body>
</html>
123456789101112
```

显示：
![flask template filter abs](https://img-blog.csdnimg.cn/202004112103067.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，负数被转换成其绝对值。
因为模板文件修改后不一定会重新加载，所以需要配置参数`app.config['TEMPLATE_AUTO_RELOAD'] = True`，当模板文件修改后就会自动更新。

#### default(value, default_value, boolean=false)

如果当前变量没有值，则会使用参数中的值来代替。
例如`name|default('Corley')`中，如果name未在视图函数中定义，就会使用Corley来替代。
参数`boolean=false`默认是在只有这个变量为undefined的时候才会使用default中的值，如果想使用python的形式判断是否为false，则可以传递`boolean=true`，或者使用or来替换。
测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    <h2>{{age | abs}}</h2>
    <h2>{{name | default('这个人很懒，并没有留下签名...')}}</h2>

</body>
</html>
1234567891011121314
```

显示：
![flask template filter default](https://img-blog.csdnimg.cn/20200411210330691.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
因为在Python代码中并未定义和传name参数，所以渲染给定的默认值；
如果在Python中定义了参数，会渲染给定的参数对应的值。
这种情况下只有当name参数未定义的时候才会渲染`default()`的参数值，否则都会渲染name参数对应的值。
也可以在`default()`传入参数`boolean=true`，此时会根据Python的规则来判断name的布尔值是True还是False，如果是True则渲染name的值，如果是False则渲染`default()`中给定的值，测试如下：

```python
from flask import Flask,render_template

app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    context = {
        'username':'小度',
        'age':-18,
        'name1':None,
        'name2':'Corley',
    }
    return render_template('index.html', **context)

if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819
```

模板代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    <h2>{{age | abs}}</h2>
    <h2>{{name1 | default('这个人很懒，并没有留下签名', boolean=true)}}</h2>
    <h2>{{name2 | default('这个人很懒，并没有留下签名', boolean=true)}}</h2>

</body>
</html>
123456789101112131415
```

显示：
![flask template filter default boolean true](https://img-blog.csdnimg.cn/20200411210436962.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，此时`default()`中加了参数`boolean=true`，因为name1为None，其在Python中的布尔值为False，所以会渲染`default()`中给定的值，而name2为一个非空字符串，对应的布尔值为True，所以会渲染name2对应的值。

#### escape(value)

转义字符，会将<、>等特殊符号转义成HTML中的符号，使其成为普通字符串，避免渲染成HTML元素。
例如：`content|escape`或`content|e`。
escape测试如下：

```python
from flask import Flask,render_template

app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    context = {
        'username':'小度',
        'age':-18,
        'name':'Corley',
        'script':'<script>alert("hello")</script>'
    }
    return render_template('index.html', **context)

if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819
```

模板代码为：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    <h2>{{age | abs}}</h2>
    <h2>{{name | default('这个人很懒，并没有留下签名', boolean=true)}}</h2>
    <h2>{{script | escape}}</h2>

</body>
</html>
123456789101112131415
```

显示：
![flask template filter escape](https://img-blog.csdnimg.cn/20200411210508329.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，此时Flask模板渲染时并未将script变量对应的值渲染为Javascript代码，而是当作普通的字符串。
查看网页源代码如下：
![flask template filter escape source](https://img-blog.csdnimg.cn/2020041121053044.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，在渲染时将特殊字符自动转义。
在flask中，默认是开启了自动转义的功能的，所以不使用escape过滤器也是可以的。
这是Jinja2的一种安全机制，可以防止对页面进行篡改、注入等操作。

#### safe(value)

如果开启了全局转义，那么safe过滤器会将变量关掉转义。
例如：`content_html|safe`。
上例中，如果要取消转义，可以关闭escape过滤器，测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    <h2>{{age | abs}}</h2>
    <h2>{{name | default('这个人很懒，并没有留下签名', boolean=true)}}</h2>
    {% autoescape off %}
    <h2>{{script}}</h2>
    {% endautoescape %}
</body>
</html>
12345678910111213141516
```

显示：
![flask template filter escape off](https://img-blog.csdnimg.cn/20200411210551596.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
同时还可以使用safe过滤器达到同样的效果，如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    <h2>{{age | abs}}</h2>
    <h2>{{name | default('这个人很懒，并没有留下签名', boolean=true)}}</h2>
    <h2>{{script | safe}}</h2>
</body>
</html>
1234567891011121314
```

效果与之前相同。
很明显，可以使用safe过滤器渲染HTML代码，如下：

```python
from flask import Flask,render_template

app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    context = {
        'username':'小度',
        'age':-18,
        'name':'Corley',
        'h3':'<h3>Flask</h3>'
    }
    return render_template('index.html', **context)

if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819
```

模板代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    <h2>{{age | abs}}</h2>
    <h2>{{name | default('这个人很懒，并没有留下签名', boolean=true)}}</h2>
    <h2>{{h3 | safe}}</h2>
</body>
</html>
1234567891011121314
```

显示：
![flask template filter escape safe html](https://img-blog.csdnimg.cn/20200411210623990.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

#### first(value)、last(value)和length(value)

`first(value)`返回一个序列的第一个元素，如`names|first`；
`last(value)`返回一个序列的最后一个元素，如`names|last`；
`length(value)`返回一个序列或者字典的长度，如`names|length`。

测试如下：

```python
from flask import Flask,render_template

app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    context = {
        'username':'小度',
        'age':-18,
        'name':'Corley',
        'h3':'<h3>Flask</h3>',
        'books':['Python', 'Java', 'PHP']
    }
    return render_template('index.html', **context)

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920
```

模板代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    <h2>{{age | abs}}</h2>
    <h2>{{name | default('这个人很懒，并没有留下签名', boolean=true)}}</h2>
    <h2>{{h3 | safe}}</h2>
    <h2>{{books | first}}</h2>
    <h2>{{books | last}}</h2>
    <h2>{{books | length}}</h2>
</body>
</html>
1234567891011121314151617
```

显示：
![flask template filter first](https://img-blog.csdnimg.cn/2020041121074260.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
还可以嵌套过滤，求列表中某个字符串的长度：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    <h2>{{age | abs}}</h2>
    <h2>{{name | default('这个人很懒，并没有留下签名', boolean=true)}}</h2>
    <h2>{{h3 | safe}}</h2>
    <h2>{{books | first}}</h2>
    <h2>{{books | last}}</h2>
    <h2>{{books | first | length}}</h2>
</body>
</html>
1234567891011121314151617
```

显示：
![flask template filter first length](https://img-blog.csdnimg.cn/20200411210804969.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

#### lower(value)、upper(value)

分别将字符串转换为小写、大写。
测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    <h2>{{age | abs}}</h2>
    <h2>{{name | default('这个人很懒，并没有留下签名', boolean=true)}}</h2>
    <h2>{{name | lower}}</h2>
    <h2>{{name | upper}}</h2>
</body>
</html>
123456789101112131415
```

显示：
![flask template filter lower upper](https://img-blog.csdnimg.cn/20200411210827363.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

#### replace(value, old, new)

将字符串中old字串替换为new字符串。
测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    <h2>{{age | abs}}</h2>
    <h2>{{name | default('这个人很懒，并没有留下签名', boolean=true)}}</h2>
    <h2>{{name | replace('ley','lin')}}</h2>

</body>
</html>
123456789101112131415
```

显示：
![flask template filter replace](https://img-blog.csdnimg.cn/20200411210850404.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

#### truncate(value,length=255,killwords=False)

截取length长度的字符串。
测试如下：

```python
from flask import Flask,render_template

app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    context = {
        'username':'小度',
        'age':-18,
        'name':'Corley',
        'title':'很“聪明”的小夜灯！靠近时自动亮起、冷暖光可调、还可随身携带'
    }
    return render_template('index.html', **context)

if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819
```

模板代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    <h2>{{age | abs}}</h2>
    <h2>{{name | default('这个人很懒，并没有留下签名', boolean=true)}}</h2>
    <h2>{{title | truncate(length=15)}}</h2>

</body>
</html>
123456789101112131415
```

显示：
![flask template filter truncate](https://img-blog.csdnimg.cn/20200411210924612.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
`truncate()`过滤器一般用于显示较长的标题省略末尾一部分内容；
注意，`truncate()`传入的length必须大于等于3，因为3表示省略号3个点，大于3时剩下的显示变量具体内容，小于3时渲染会报错。

#### striptags(value)

删除字符串中所有的HTML标签，如果出现多个空格，将替换成一个空格。
测试如下：

```python
from flask import Flask,render_template

app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    context = {
        'username':'小度',
        'age':-18,
        'name':'Corley',
        'script':'<script>alert("hello")</script>'
    }
    return render_template('index.html', **context)

if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819
```

模板代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    <h2>{{age | abs}}</h2>
    <h2>{{name | default('这个人很懒，并没有留下签名', boolean=true)}}</h2>
    <h2>{{script | striptags}}</h2>

</body>
</html>
123456789101112131415
```

显示：
![flask template filter striptags](https://img-blog.csdnimg.cn/20200411210958134.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，标签都被过滤掉，只显示内部内容。

#### wordcount(s)

计算一个长字符串中单词的个数。
测试如下：

```python
from flask import Flask,render_template

app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    context = {
        'username':'小度',
        'age':-18,
        'name':'Corley',
        'hello':'hello world',
        'address':'中国 四川省',
    }
    return render_template('index.html', **context)

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920
```

模板代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    <h2>{{age | abs}}</h2>
    <h2>{{name | default('这个人很懒，并没有留下签名', boolean=true)}}</h2>
    <h2>{{hello}}</h2>
    <h2>{{hello | wordcount}}</h2>
    <h2>{{address}}</h2>
    <h2>{{address | wordcount}}</h2>

</body>
</html>
123456789101112131415161718
```

显示：
![flask template filter wordcount](https://img-blog.csdnimg.cn/2020041121102624.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
易知，wordcount是根据空格来计数的，对英文、中文都能识别。

#### 其他过滤器

- `format(value,*arags,**kwargs)`
  格式化字符串。
  例如`{{ "%s" - "%s"|format('Hello',"World") }}`将输出`Hello - World`。
- `join(value,d='xx')`
  将一个序列用d参数的值拼接成字符串。
- `int(value)`
  将值转换为int类型。
- `float(value)`
  将值转换为float类型。
- `string(value)`
  将变量转换成字符串。
- `trim(value)`
  截取字符串前面和后面的空白字符。

### 2.自定义过滤器

除了使用Jinja2模板内置的过滤器，还可以自定义过滤器。

简单示例如下：

```python
from flask import Flask,render_template

app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    context = {
        'username':'小度',
        'age':-18,
        'name':'Corley',
        'hello':'hello world',
        'address':'中国 四川省',
    }
    return render_template('index.html', **context)

@app.template_filter('my_replace')
def cut(value):
    return value.replace(' ', '*')

if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021222324
```

模板代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    <h2>{{age | abs}}</h2>
    <h2>{{name | default('这个人很懒，并没有留下签名', boolean=true)}}</h2>
    <h2>{{hello}}</h2>
    <h2>{{hello | my_replace}}</h2>

</body>
</html>
12345678910111213141516
```

显示：
![flask template filter custom](https://img-blog.csdnimg.cn/20200411211049663.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，空格被替换为*。
自定义过滤器相当于定义函数，同时需要用装饰器`@app.template_filter('my_replace')`来装饰，传入的参数即为过滤器名。

再定义一个过滤器，实现以下功能：
小于1分钟，显示刚刚；
大于1分钟小于1小时，显示XX分钟前；
大于1小时小于24小时，显示XX小时前；
…
测试如下：

```python
from datetime import datetime

from flask import Flask,render_template


app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    context = {
        'username':'小度',
        'age':-18,
        'name':'Corley',
        'hello':'hello world',
        'release_time1':datetime(2020, 4, 11, 9, 30, 0),
        'release_time2':datetime(2020, 4, 11, 12, 30, 0)
    }
    return render_template('index.html', **context)

@app.template_filter('time_handler')
def handle_time(time):
    if isinstance(time, datetime):
        now = datetime.now()
        time_delta = (now - time).total_seconds()
        if time_delta < 60:
            return '刚刚'
        elif time_delta >= 60 and time_delta < 60 * 60:
            return '%d分钟之前' % (time_delta // 60)
        elif time_delta >= 60 * 60 and time_delta < 60 * 60 * 24:
            return '%d小时之前' % (time_delta / 60 // 60)

    else:
        return time

if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021222324252627282930313233343536373839
```

模板代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    <h2>{{age | abs}}</h2>
    <h2>{{name | default('这个人很懒，并没有留下签名', boolean=true)}}</h2>
    <h2>{{hello}}</h2>
    <h2>文章发表时间：{{release_time1}}</h2>
    <h2>文章发表时间：{{release_time1 | time_handler}}</h2>
    <h2>文章发表时间：{{release_time2}}</h2>
    <h2>文章发表时间：{{release_time2 | time_handler}}</h2>
</body>
</html>
123456789101112131415161718
```

显示：
![flask template filter custom timedelta](https://img-blog.csdnimg.cn/20200411211115870.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

## 二、模板控制语句

所有的控制语句都是放在`{% ... %}`中，并且有一个语句`{% endxxx %}`来结束，Jinja中常用的控制语句有`if`、`for...in...`等。

### 1.if判断语句

if语句和Python中类似，可以使用>、<、<=、>=、==和!=来进行判断，也可以通过and、or、not、()来进行逻辑合并操作。
例如：

```html
{% if weather == 'sunny' %}
	出去玩
{% elif weather == 'rainy'  %}
	在家玩
{% else %}
	睡大觉
{% endif %}
1234567
```

if判断语句测试如下：

```python
from datetime import datetime
from flask import Flask, render_template


app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    context = {
        'username':'Corley',
    }
    return render_template('index.html', **context)


if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819
```

模板代码为：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    {% if username == 'Corlin' %}
    <p>{{username}}</p>
    {% else %}
    <p>Current user is not Corlin!!!</p>
    {% endif %}
</body>
</html>
12345678910111213141516
```

显示：
![flask control if](https://img-blog.csdnimg.cn/20200411211147254.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

### 2.for循环语句

for循环可以遍历任何一个序列包括列表、字典、元组，并且可以进行反向遍历。
for循环遍历列表和字典测试：

```python
from datetime import datetime
from flask import Flask,render_template


app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    context = {
        'username': 'Corley',
        'books': ['Python', 'Java', 'PHP'],
        'info': {
            'name': '小度',
            'age': 18,
            'major': 'EC'
        }
    }
    return render_template('index.html', **context)


if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819202122232425
```

模板代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    {% for book in books %}
        <p>{{book}}</p>
    {% endfor %}
    <hr>
    {% for key, value in info.items() %}
        <p>{{key}} : {{value}}</p>
    {% endfor %}
</body>
</html>
123456789101112131415161718
```

显示：
![flask control for](https://img-blog.csdnimg.cn/20200411211313483.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
for循环还可以在序列为空时使用**else语句退出循环**，测试如下：

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    {% for subject in subjects %}
        <p>{{subject}}</p>
    {% else %}
        <em>No Subjects Found</em>
    {% endfor %}
</body>
</html>
12345678910111213141516
```

视图函数中不变，显示：
![flask template for else](https://img-blog.csdnimg.cn/20200411190943794.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
并且Jinja中的for循环还包含变量用来获取当前的遍历状态，常见变量如下：

| 变量          | 描述                                |
| ------------- | ----------------------------------- |
| `loop.index`  | 当前迭代的索引（从1开始）           |
| `loop.index0` | 当前迭代的索引（从0开始）           |
| `loop.first`  | 是否是第一次迭代，返回True或False   |
| `loop.last`   | 是否是最后一次迭代，返回True或False |
| `loop.length` | 序列的长度                          |

获取当前遍历状态测试：

```python
from datetime import datetime

from flask import Flask,render_template


app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    context = {
        'username': 'Corley',
        'books': ['Python', 'Java', 'PHP'],
    }
    return render_template('index.html', **context)


if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021
```

模板代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    {% for book in books %}
        <p>{{loop.index}}--:--{{book}}</p>
    {% endfor %}
    <hr>
    {% for book in books %}
        <p>{{loop.index0}}--:--{{book}}</p>
    {% endfor %}
    <hr>
    {% for book in books %}
        <p>{{loop.first}}--:--{{book}}</p>
    {% endfor %}
</body>
</html>
12345678910111213141516171819202122
```

显示：
![flask control for state](https://img-blog.csdnimg.cn/20200411211537446.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
注意：
**不能用continue和break表达式**来控制循环的执行，它们在模板中控制循环是无效的。

## 三、宏和import语句

### 1.宏的定义和使用

模板中的宏跟python中的函数类似，可以传递参数，但是**不能有返回值**，可以将一些经常用到的代码片段放到宏中，然后把一些不固定的值抽取出来作为参数。
宏定义的语法：

```html
{% macro 宏名字(参数) %}
    xxx
{% endmacro %}
123
```

进行测试：

```python
from flask import Flask,render_template


app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    context = {
        'username': 'Corley',
        'books': ['Python', 'Java', 'PHP'],
    }
    return render_template('index.html', **context)

@app.route('/macro')
def macro():
    return render_template('macro.html')


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223
```

新建模板**macro.html**如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    {% macro input(name, value='', type='text') %}
        <input name="{{name}}" value="{{value}}" type="{{type}}">
    {% endmacro %}
    <table>
        <tr>
            <td>用户名：</td>
            <td>{{input('username')}}</td>
        </tr>
        <tr>
            <td>密码：</td>
            <td>{{input('password', type='password')}}</td>
        </tr>
        <tr>
            <td>提交</td>
            <td>{{input(value='提交', type='submit')}}</td>
        </tr>
    </table>
</body>
</html>
1234567891011121314151617181920212223242526
```

定义的宏抽取出了一个input标签，指定了一些默认参数，那么以后创建input标签的时候，就可以通过宏快速的创建。
显示：
![flask macro test](https://img-blog.csdnimg.cn/20200411211900751.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，`{% macro input(name, value='', type='text') %}`定义了一个类似于函数的结构，传入参数就可以生成对应的模板直接使用。

### 2.import语句

在真实的开发中，会将一些常用的宏单独放在一个文件中，在需要使用的时候，再从这个文件中进行导入。
import语句的用法跟python中的import类似，可以直接`import...as...`，也可以`from...import...`或者`from...import...as...`。

macro.html文件用于专门保存宏：

```html
{% macro input(name, value='', type='text') %}
    <input name="{{name}}" value="{{value}}" type="{{type}}">
{% endmacro %}
123
```

测试如下：

```python
from flask import Flask,render_template


app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    context = {
        'username': 'Corley',
        'books': ['Python', 'Java', 'PHP'],
    }
    return render_template('index.html', **context)


if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819
```

模板代码为：

```html
{% import 'macro.html' as macro %}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    {% for book in books %}
        <p>{{loop.index}}--:--{{book}}</p>
    {% endfor %}
    <hr>
    请输入：{{ macro.input('username') }}
</body>
</html>
123456789101112131415161718
```

显示：
![flask macro import](https://img-blog.csdnimg.cn/20200411211924277.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，访问主页时，渲染导入的宏生成输入框。
也可以用`from xxx import xxx`语句，如下：

```html
{% from 'macro.html' import input %}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    {% for book in books %}
        <p>{{loop.index}}--:--{{book}}</p>
    {% endfor %}
    <hr>
    <p>请输入用户名：{{ input('username') }}</p>
    <p>请输入密码：{{ input('password', type='password') }}</p>
</body>
</html>
12345678910111213141516171819
```

显示：
![flask macro import password](https://img-blog.csdnimg.cn/20200411211946618.gif#pic_center)
用这种导入方式也可以重命名，如`from 'macro.html' import input as inputbox`。
需要注意，导入模板并不会把当前上下文中的变量添加到被导入的模板中，如果需要导入一个需要访问当前上下文变量的宏，有两种可能的方法：

- 显式地传入请求或请求对象的属性作为宏的参数
- 与上下文一起（**with context**）导入宏

在导入宏时用`with context`可以把当前模板的一些参数传给宏所在的模板。
测试如下：

```html
{% from 'macro.html' import input with context %}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    <h1>首页</h1>
    <h2>{{username}}</h2>
    {% for book in books %}
        <p>{{loop.index}}--:--{{book}}</p>
    {% endfor %}
    <hr>
    <p>请输入用户名：{{ input('username') }}</p>
    <p>请输入密码：{{ input('password', type='password') }}</p>
</body>
</html>
12345678910111213141516171819
```

宏文件macro.html中修改：

```html
{% macro input(name, value='', type='text') %}
    <input name="{{name}}" value="{{username}}" type="{{type}}">
{% endmacro %}
123
```

显示：
![flask macro import pass params](https://img-blog.csdnimg.cn/20200411212022310.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
要传递参数，需要在导入宏的时候加上`with context`修饰，这样可以将视图函数中定义的参数传给宏，再渲染到模板中。

## 四、include和set语句

### 1.include语句

include语句可以把一个模板引入到另外一个模板中，类似于把一个模板的代码copy到另外一个模板的指定位置。
测试过程如下：
先在templates目录下**创建common文件夹**，再在common文件夹下**创建header.html和footer.html文件**，
header.html文件如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Header</title>
</head>
<body>
    <ul>
        <li>军事</li>
        <li>科技</li>
        <li>财经</li>
        <li>娱乐</li>
    </ul>
</body>
</html>
123456789101112131415
```

footer.html文件如下：

```html
<footer>
    这是页面底部
</footer>
123
```

再**在templates目录下创建article.html文件**，如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Article</title>
</head>
<body>
    {% include 'common/header.html' %}
    <ul>
        <li>第一篇文章</li>
        <li>第二篇文章</li>
        <li>第三篇文章</li>
    </ul>
    {% include 'common/footer.html' %}
</body>
</html>
12345678910111213141516
```

index.html文件如下：

```html
{% from 'macro.html' import input with context %}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    {% include 'common/header.html' %}
    <h2>{{username}}</h2>
    {% for book in books %}
        <p>{{loop.index}}--:--{{book}}</p>
    {% endfor %}
    <hr>
    <p>请输入用户名：{{ input('username') }}</p>
    <p>请输入密码：{{ input('password', type='password') }}</p>
    {% include 'common/footer.html' %}
</body>
</html>
1234567891011121314151617181920
```

视图函数文件如下：

```python
from flask import Flask,render_template


app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    context = {
        'username': 'Corley',
        'books': ['Python', 'Java', 'PHP'],
    }
    return render_template('index.html', **context)

@app.route('/article/')
def article():
    return render_template('article.html')


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223
```

显示：
![flask include](https://img-blog.csdnimg.cn/20200411212104252.gif#pic_center)

### 2.set赋值语句

一般情况下是在视图函数中定义变量、在模板中引用，但是也可以**在模板中添加变量**，这时候会用到set赋值语句。
测试如下：

```python
from flask import Flask, render_template

app = Flask(__name__)
# 修改模板后，自动重新加载
app.config['TEMPLATE_AUTO_RELOAD'] = True


@app.route('/')
def index():
    context = {
        'username': 'Corley',
        'books': ['Python', 'Java', 'PHP'],
        'name': '小爱'
    }
    return render_template('index.html', **context)


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920
```

模板代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    {%  set name='小度' %}
    <h2>{{username}}</h2>
    <p>{{  name  }}</p>
</body>
</html>
123456789101112
```

显示：
![flask set global](https://img-blog.csdnimg.cn/20200411212131738.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，此时设置的是全局变量，不需要结束标签。
视图函数中未定义name变量时，模板可以渲染set定义的name变量，视图函数中定义了name变量时会覆盖视图函数中的值。
通过set定义全局变量一次只能定义一个，并且赋值语句创建的变量在其之后都是有效的。
如果不想让一个变量影响到全局环境，可以使用**with语句**来创建一个内部的作用域，将set语句放在其中，这样创建的变量只在with代码块中才有效，即相当于创建了一个**局部变量**。
测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    {%  set name='小度' %}
    <h2>{{username}}</h2>
    <p>{{  name  }}</p>
    {% with  %}
        {%  set name='天猫精灵' %}
        <p>{{ name }}</p>
    {% endwith %}

</body>
</html>
1234567891011121314151617
```

显示：
![flask set local](https://img-blog.csdnimg.cn/20200411212243405.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，在with语句内部定义的局部变量，不会受外部的影响。
with语句也可以**直接定义变量**，如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    {%  set name='小度' %}
    <h2>{{username}}</h2>
    <p>{{  name  }}</p>
    {% with name='天猫精灵' %}
        <p>{{ name }}</p>
    {% endwith %}

</body>
</html>
12345678910111213141516
```

这与前者的效果相同，一旦超出with代码块，在该代码块中定义的变量就失效了。
也可以定义字典和列表：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask</title>
</head>
<body>
    {%  set name='小度' %}
    <h2>{{username}}</h2>
    <p>{{  name  }}</p>
    {% with name='天猫精灵' %}
        <p>{{ name }}</p>
    {% endwith %}

    {% with dict = {'name':'Google Home', 'country':'USA'} %}
        <p>{{ dict.name }}</p>
        <p>{{ dict.country }}</p>
    {% endwith %}

    {% with list = ['小度', '小爱', '天猫精灵']  %}
        {% for item in list %}
            <p>{{ item }}</p>
        {% endfor %}
    {% endwith %}
</body>
</html>
1234567891011121314151617181920212223242526
```

显示：
![flask set list dict](https://img-blog.csdnimg.cn/20200411212311444.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

# Python全栈（七）Flask框架之4.Flask模板继承与案例练习

## 一、模版继承

Flask中的模板可以继承，把模板中重复出现的元素抽取出来放在父模板中，子模板再根据自己的需要进行改写。
通常，在父模板中定义公用的部分，通过定义block给子模板开一个口，子模板从父模板中继承并根据需要重写，从而提高了代码的**复用性**。

采用原始的简单复制的方法测试：
创建视图函数Python文件：

```python
from flask import Flask, render_template

app = Flask(__name__)


@app.route('/')
def index():
    return render_template('index.html')


@app.route('/list/')
def course_list():
    return render_template('list.html')

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617
```

创建templates目录，在其下创建index.html和list.html，index.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
    <h1>这是首页</h1>
    <ul>
        <li>课程列表</li>
        <li>课程详情</li>
        <li>视频教程</li>
        <li>关于我们</li>
    </ul>
    <div class="footer">
        网站底部
    </div>
</body>
</html>
12345678910111213141516171819
```

list.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>课程列表</title>
</head>
<body>
    <h1>这是课程列表页面</h1>
    <ul>
        <li>Python</li>
        <li>Java</li>
        <li>PHP</li>
        <li>C++</li>
    </ul>
    <div class="footer">
        网站底部
    </div>
</body>
</html>
12345678910111213141516171819
```

显示：
![flask template inheritance origin](https://img-blog.csdnimg.cn/20200414211221985.gif#pic_center)
显然，两个网页结构相同，并且在模板代码中元素基本相同，有大量的代码重复，可以进行优化。

现使用模板继承进行测试：

```python
from flask import Flask, render_template

app = Flask(__name__)
app.config['TEMPLATES_AUTO_RELOAD'] = True


@app.route('/')
def index():
    return render_template('index.html')


@app.route('/list/')
def course_list():
    return render_template('list.html')


if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819
```

新建base.html即父模板：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>
        {% block title %}
        
        {% endblock %}
    </title>
</head>
<body>
    {% block header %}
        <h1>这是首页</h1>
    {% endblock %}
    {% block content %}
        <ul>
            <li>课程列表</li>
            <li>课程详情</li>
            <li>视频教程</li>
            <li>关于我们</li>
        </ul>
    {% endblock %}
    {% block footer %}
        <div class="footer">
            网站底部
        </div>
    {% endblock %}

</body>
</html>
123456789101112131415161718192021222324252627282930
```

base.html抽取了所有模板都需要用到的元素html、body等公共部分，并且对于所有模板都要用到的样式文件也进行了抽取，同时对于子模板需要重写的地方，比如title、header、content、footer等都定义成了block，子模板继承自base.html后可以根据自己的需要，再具体实现。
让index.html和list.html继承自base.html，index.html如下：

```html
{% extends 'base.html' %}

{% block title %}
    这是首页
{% endblock %}

{# 重写父模板中的block #}
{% block header %}
    <h1>这是子模版首页</h1>
{% endblock %}
12345678910
```

list.html如下：

```html
{% extends 'base.html' %}

{% block title %}
    这是首页
{% endblock %}

{# 重写父模板中的block #}
{% block header %}
    <h1>这是子模版课程列表页面</h1>
{% endblock %}

{% block content %}
        <ul>
            <li>Python</li>
            <li>Java</li>
            <li>PHP</li>
            <li>C++</li>
        </ul>
{% endblock %}
12345678910111213141516171819
```

显示：
![flask template inheritance base](https://img-blog.csdnimg.cn/20200414211304858.gif#pic_center)
显然，在使用模板继承后，代码的复用性明显提高、代码量显著减少；
`{% extends 'base.html' %}`定义了该模板继承的父模板，然后在相应的block中重写所需内容；
在父模板中放入block中的代码子模板继承后可以重写，而父模板中未放入block中的代码子模板只能直接继承不能重写；
模板中的block**不能重名**。

在父模板和子模板中block还**可以嵌套**，测试如下：
base.html：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>
        {% block title %}
        
        {% endblock %}
    </title>
</head>
<body>
    {% block header %}
        <h1>这是首页</h1>
    {% endblock %}
    {% block content %}
        <ul>
            <li>课程列表</li>
            <li>课程详情</li>
            <li>视频教程</li>
            <li>关于我们</li>
        </ul>
    {% endblock %}
    {% block footer %}
        <div class="footer">
            网站底部
        </div>
        {% block footer-link %}
            <label for="">友情链接</label>
        {% endblock %}
    {% endblock %}

</body>
</html>
123456789101112131415161718192021222324252627282930313233
```

index.html：

```html
{% extends 'base.html' %}

{% block title %}
    这是首页
{% endblock %}

{# 重写父模板中的block #}
{% block header %}
    <h1>这是子模版首页</h1>
{% endblock %}

{% block footer %}
    <div class="footer">
        网站底部
    </div>
    {% block footerlink %}
        <label for="">子模版主页友情链接</label>
    {% endblock %}
{% endblock %}
12345678910111213141516171819
```

list.html：

```html
{% extends 'base.html' %}

{% block title %}
    这是首页
{% endblock %}

{# 重写父模板中的block #}
{% block header %}
    <h1>这是子模版课程列表页面</h1>
{% endblock %}

{% block content %}
    <ul>
        <li>Python</li>
        <li>Java</li>
        <li>PHP</li>
        <li>C++</li>
    </ul>
{% endblock %}

{% block footerlink %}
    <label for="">子模版课程列表友情链接</label>
{% endblock %}
1234567891011121314151617181920212223
```

显示：
![flask template inheritance block nesting](https://img-blog.csdnimg.cn/20200414211340802.gif#pic_center)
显然，在父模板中block定义嵌套后，在子模板中重写block时，可以将两层嵌套都引用再重写，如index.html，也可以只引用内部block嵌套并重写，可根据需要选择。
注意：
父模板中的block相当于给子模板提供了一个接口，只有父模板中已经定义了，才能在父模板中实现；
在**子模板中不能扩展代码**，即自己在block之外添加的代码是无效的，子模版中的所有代码都必须在父模板及其定义的block中实现，测试如下：
index.html中添加代码：

```html
{% extends 'base.html' %}

{% block title %}
    这是首页
{% endblock %}

{% block header %}
    <h1>这是子模版首页</h1>
{% endblock %}

{% block footer %}
    <div class="footer">
        网站底部
    </div>
    {% block footerlink %}
        <label for="">子模版主页友情链接</label>
    {% endblock %}
{% endblock %}

<p>这是在block之外的代码</p>
1234567891011121314151617181920
```

显示：
![flask template inheritance outside block](https://img-blog.csdnimg.cn/20200414211521794.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，在block之外的代码并没有被渲染出来。

同时，一个子模板只能继承自一个父模板，不能多继承。
如果在一个地方需要用到另一个block的内容，需要使用`self.block名字`来引用，测试（index.html代码）如下：

```markup
{% extends 'base.html' %}

{% block title %}
    这是首页
{% endblock %}

{% block header %}
    <h1>这是子模版首页</h1>
{% endblock %}

{% block footer %}
    <h1>这是本网页标题---{{ self.title() }}---</h1>
    <div class="footer">
        网站底部
    </div>
    {% block footerlink %}
        <label for="">子模版主页友情链接</label>
    {% endblock %}
{% endblock %}
12345678910111213141516171819
```

显示：
![flask template inheritance block self](https://img-blog.csdnimg.cn/2020042412362432.gif#pic_center)

显然，在footer中引用了title中的内容。

调用父模板中的block时，在改写父模板的同时还想得到父模板中的代码时，可以类比Python中类的继承，使用`super()`函数即调用了父模板中的代码，把父模板中的内容添加到子模板中，测试如下：
index.html代码修改如下：

```html
{% extends 'base.html' %}

{% block title %}
    这是首页
{% endblock %}

{% block header %}
    {{ super() }}
    <h1>这是子模版首页</h1>
{% endblock %}

{% block footer %}
    <div class="footer">
        网站底部
    </div>
    {% block footerlink %}
        <label for="">子模版主页友情链接</label>
    {% endblock %}
{% endblock %}
12345678910111213141516171819
```

显示：
![flask template inheritance super](https://img-blog.csdnimg.cn/20200414211657765.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，是可以继承同时重写父模板的。

## 二、配置静态资源文件

静态文件包括包括CSS样式文件、JavaScript脚本文件、图片文件、字体文件等；
实际的前端开发中会使用大量的静态文件来使得网页更加生动美观，在Jinja2中加载静态文件通过`url_for()`函数来实现，例如

```markup
<link href="{{ url_for('static',filename='css/test.css') }}">
1
```

`url_for()`函数默认会在项目根目录下的static文件夹的css目录下中寻找test.css文件，如果文件存在，则会生成一个相对于项目根目录的`/static/css/test.css`路径，并根据定义的格式渲染网页。
如果不想放在static目录、自己指定目录，可以在初始化Flask时，设置**static_folder参数**，例如`app = Flask(__name__, static_folder='static_files')`，此时会到static_files目录下寻找静态文件。

加载静态文件测试：
在模板文件夹下创建books.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="{{ url_for('static', filename='css/books.css') }}">
    <script src="{{ url_for('static', filename='js/books.js') }}"></script>
    <title>图书列表</title>
</head>
<body>
    <h1>这是图书列表页面</h1>
    <img src="{{ url_for('static', filename='images/monalisa.jpg') }}" alt="蒙娜丽莎的微笑">
    <ul>
        <li>Python</li>
        <li>Java</li>
        <li>PHP</li>
        <li>C++</li>
    </ul>
    <div class="footer">
        网站底部
    </div>
</body>
</html>
12345678910111213141516171819202122
```

在项目目录下创建static目录，下面创建css、js、images三个文件夹，分别用于保存CSS、JavaScript和图片文件，用于保存资源文件，css目录下创建books.css如下：

```css
body {
    background: #4eb8ff;
}
123
```

js目录下创建books.js如下：

```javascript
alert(document.domain);
1
```

找一张图片如monalisa.jpg放在images目录下，进行测试：



<iframe id="527mxvrN-1586870373580" src="https://player.youku.com/embed/XNDYzMjIwMzE5Mg==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

flask模板静态文件加载



显然，此时已加载到css、js、images静态文件。
考虑到模板继承时，css和JavaScript一般在父模板中直接在头部加入，而图片在子模板的对应位置的block中加入，如下：
base.html：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="{{ url_for('static',filename='css/books.css') }}">
    <script src="{{ url_for('static', filename='js/books.js') }}"></script>
    <title>
        {% block title %}
        
        {% endblock %}
    </title>
</head>
<body>
    {% block header %}
        <h1>这是首页</h1>
    {% endblock %}
    {% block content %}
        <ul>
            <li>课程列表</li>
            <li>课程详情</li>
            <li>视频教程</li>
            <li>关于我们</li>
        </ul>
    {% endblock %}
    {% block footer %}
        <div class="footer">
            网站底部
        </div>
        {% block footerlink %}
            <label for="">友情链接</label>
        {% endblock %}
    {% endblock %}

</body>
</html>
1234567891011121314151617181920212223242526272829303132333435
```

index.html：

```html
{% extends 'base.html' %}

{% block title %}
    这是首页
{% endblock %}

{% block header %}
    <h1>这是子模版首页</h1>
    <img src="{{ url_for('static', filename='images/monalisa.jpg') }}" alt="蒙娜丽莎的微笑">
{% endblock %}

{% block footer %}
    <div class="footer">
        网站底部
    </div>
    {% block footerlink %}
        <label for="">子模版主页友情链接</label>
    {% endblock %}
{% endblock %}
12345678910111213141516171819
```

list.html：

```html
{% extends 'base.html' %}

{% block title %}
    这是首页
{% endblock %}

{# 重写父模板中的block #}
{% block header %}
    <h1>这是子模版课程列表页面</h1>
{% endblock %}

{% block content %}
    <ul>
        <li>Python</li>
        <li>Java</li>
        <li>PHP</li>
        <li>C++</li>
    </ul>
{% endblock %}

{% block footerlink %}
    <img src="{{ url_for('static', filename='images/monalisa.jpg') }}" alt="蒙娜丽莎的微笑">
    <label for="">子模版课程列表友情链接</label>
{% endblock %}
123456789101112131415161718192021222324
```

显示：
![flask template static resource inheritance](https://img-blog.csdnimg.cn/20200414212031592.gif#pic_center)
显然，在父模板中定义的CSS样式和JavaScript在子模板中全部继承并渲染。

## 三、模板案例

目标是做一个豆瓣电影的电影、电视剧评分页面图，预期效果如下：
![豆瓣预期效果](https://img-blog.csdnimg.cn/20200414211113649.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
本项目用到的素材可点击https://download.csdn.net/download/CUFEECR/12327362下载测试。

先初步测试：
新建一个文件夹，在其中建立视图函数文件douban_flask.py：

```python
from flask import Flask, render_template

app = Flask(__name__)
app.config['TEMPLATES_AUTO_RELOAD'] = True


@app.route('/')
def index():
    return render_template('index.html')


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314
```

再创建templates文件夹并在其下创建index.html：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="{{ url_for('static', filename='css/base.css') }}">
    <link rel="stylesheet" href="{{ url_for('static', filename='css/item.css') }}">
    <title>豆瓣</title>
</head>
<body>
    <h1>豆瓣影视评分</h1>
    <div class="container">
        <div class="search-group">
            <input type="text" class="search-input">
        </div>
        <div class="item-list-group">
            <div class="item-list-top">
                <span class="module-title">电影</span>
                <a href="#" class="more-btn">更多</a>
            </div>
            <div class="list-group">
                <div class="item-group">
                    <img src="{{ url_for('static', filename='images/绅士们.jpg') }}" alt="" class="thumbnail">
                    <p class="item-title">绅士们</p>
                    <p class="item-rating">
                        {% set rating = 8.4 %}
                        {% set rating_light = ((rating|int)/2)|int %}
                        {% set rating_half = (rating|int)%2 %}
                        {% set rating_gray = 5 - rating_light - rating_half %}
                        {% for foo in range(0, rating_light) %}
                            <img src="{{ url_for('static', filename='images/rate_light.png') }}" alt="">
                        {% endfor %}
                        {% for foo in range(0, rating_half) %}
                            <img src="{{ url_for('static', filename='images/rate_half.png') }}" alt="">
                        {% endfor %}
                        {% for foo in range(0, rating_gray) %}
                            <img src="{{ url_for('static', filename='images/rate_gray.png') }}" alt="">
                        {% endfor %}
                        {{ rating }}
                    </p>
                </div>
                <div class="item-group">
                    <img src="{{ url_for('static', filename='images/饥饿站台.jpg') }}" alt="" class="thumbnail">
                    <p class="item-title">饥饿站台</p>
                    <p class="item-rating">
                        {% set rating = 7.8 %}
                        {% set rating_light = ((rating|int)/2)|int %}
                        {% set rating_half = (rating|int)%2 %}
                        {% set rating_gray = 5 - rating_light - rating_half %}
                        {% for foo in range(0, rating_light) %}
                            <img src="{{ url_for('static', filename='images/rate_light.png') }}" alt="">
                        {% endfor %}
                        {% for foo in range(0, rating_half) %}
                            <img src="{{ url_for('static', filename='images/rate_half.png') }}" alt="">
                        {% endfor %}
                        {% for foo in range(0, rating_gray) %}
                            <img src="{{ url_for('static', filename='images/rate_gray.png') }}" alt="">
                        {% endfor %}
                        {{ rating }}
                    </p>
                </div>
                <div class="item-group">
                    <img src="{{ url_for('static', filename='images/消失吧，群青.jpg') }}" alt="" class="thumbnail">
                    <p class="item-title">消失吧，群青</p>
                    <p class="item-rating">
                        {% set rating = 6.7 %}
                        {% set rating_light = ((rating|int)/2)|int %}
                        {% set rating_half = (rating|int)%2 %}
                        {% set rating_gray = 5 - rating_light - rating_half %}
                        {% for foo in range(0, rating_light) %}
                            <img src="{{ url_for('static', filename='images/rate_light.png') }}" alt="">
                        {% endfor %}
                        {% for foo in range(0, rating_half) %}
                            <img src="{{ url_for('static', filename='images/rate_half.png') }}" alt="">
                        {% endfor %}
                        {% for foo in range(0, rating_gray) %}
                            <img src="{{ url_for('static', filename='images/rate_gray.png') }}" alt="">
                        {% endfor %}
                        {{ rating }}
                    </p>
                </div>
            </div>
        </div>

    </div>
</body>
</html>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283848586
```

创建static目录，在其下创建css目录和images目录，再在css目录下创建base.css和item.css，base.css如下：

```css
*{
    margin: 0;
    padding: 0;
    list-style: none;
    text-decoration: none;
}
.container{
    width: 375px;
    height: 600px;
    background: pink;
}

.search-group{
    padding: 14px 8px;
    background: #41be57;
}
.search-group .search-input{
    background: #fff;
    display: block;
    width: 100%;
    height: 30px;
    border-radius: 5px;
    outline: none;
    border: none;
}
12345678910111213141516171819202122232425
```

item.css如下：

```css
.item-list-group .item-list-top{
    overflow: hidden;
    padding: 10px;
}
.item-list-top .module-title{
    float: left;
}
.item-list-top .more-btn{
    float: right;
}

.list-group{
    overflow: hidden;
    padding: 0 10px;
}
.item-group{
    float: left;
    margin-right: 10px;
}
.item-group .thumbnail{
    display: block;
    width: 100px;
    height: 150px;
}
.item-group .item-title{
    font-size: 12px;
    text-align: center;
}
.item-group .item-rating{
    font-size: 12px;
    text-align: center;
}
.item-rating img{
    width: 10px;
    height: 10px;
}
123456789101112131415161718192021222324252627282930313233343536
```

在images目录下放入所需图片文件，测试如下：
![flask template exercise origin](https://img-blog.csdnimg.cn/20200414212138642.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，达到了预期效果，但是在index.hml代码中重复代码很多，复用性较低，可以进行优化。

现使用宏将显示一部电影信息的HTML代码抽象出来，再传入所需参数，如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="{{ url_for('static', filename='css/base.css') }}">
    <link rel="stylesheet" href="{{ url_for('static', filename='css/item.css') }}">
    <title>豆瓣</title>
</head>
<body>
    {% macro itemGroup(thumbnail, title, rating) %}
        <div class="item-group">
            <img src="{{ thumbnail }}" alt="" class="thumbnail">
            <p class="item-title">{{ title }}</p>
            <p class="item-rating">
                {% set rating_light = ((rating|int)/2)|int %}
                {% set rating_half = (rating|int)%2 %}
                {% set rating_gray = 5 - rating_light - rating_half %}
                {% for foo in range(0, rating_light) %}
                    <img src="{{ url_for('static', filename='images/rate_light.png') }}" alt="">
                {% endfor %}
                {% for foo in range(0, rating_half) %}
                    <img src="{{ url_for('static', filename='images/rate_half.png') }}" alt="">
                {% endfor %}
                {% for foo in range(0, rating_gray) %}
                    <img src="{{ url_for('static', filename='images/rate_gray.png') }}" alt="">
                {% endfor %}
                {{ rating }}
            </p>
        </div>
    {% endmacro %}
    <h1>豆瓣影视评分</h1>
    <div class="container">
        <div class="search-group">
            <input type="text" class="search-input">
        </div>
        <div class="item-list-group">
            <div class="item-list-top">
                <span class="module-title">电影</span>
                <a href="#" class="more-btn">更多</a>
            </div>
            <div class="list-group">
                {{ itemGroup('static/images/绅士们.jpg', '绅士们', 8.4) }}
                {{ itemGroup('static/images/饥饿站台.jpg', '饥饿站台', 7.8) }}
                {{ itemGroup('static/images/消失吧，群青.jpg', '消失吧，群青', 6.7) }}
            </div>
        </div>
    </div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849
```

测试效果与之前相同，此时显然代码缩短了很多，提高了代码的复用性。

但是上面是直接将数据写入HTML中的，真实环境中一般是通过请求数据库获得的，现在模板函数代码中模拟数据的查询与渲染：

```python
from flask import Flask, render_template

app = Flask(__name__)
app.config['TEMPLATES_AUTO_RELOAD'] = True

# 电影数据
movies = [
    {
        'title': u'绅士们',
        'thumbnail': 'static/images/绅士们.jpg',
        'rating': u'8.4',
    },
    {
        'title': u'饥饿站台',
        'thumbnail': u'static/images/饥饿站台.jpg',
        'rating': u'7.8',
    },
    {
        'title': u'消失吧，群青',
        'thumbnail': u'static/images/消失吧，群青.jpg',
        'rating': u'6.7',
    },
    {
        'title': u'再见，妈妈',
        'thumbnail': u'static/images/再见，妈妈.jpg',
        'rating': u'8.1',
    },
    {
        'title': u'狩猎',
        'thumbnail': u'static/images/狩猎.jpg',
        'rating': u'7.3',
    },
    {
        'title': u'隐形人',
        'thumbnail': u'static/images/隐形人.jpg',
        'rating': u'7.3',
    }
]

# 电视剧数据
tvs = [
    {
        'title': u'清平乐',
        'thumbnail': 'static/images/清平乐.jpg',
        'rating': u'8.1',
    },
    {
        'title': u'民国神探',
        'thumbnail': u'static/images/民国神探.jpg',
        'rating': u'7.1',
    },
    {
        'title': u'叹息桥',
        'thumbnail': u'static/images/叹息桥.jpg',
        'rating': u'8.8',
    },
    {
        'title': u'重生',
        'thumbnail': u'static/images/重生.jpg',
        'rating': u'6.6',
    },
    {
        'title': u'陈情令',
        'thumbnail': u'static/images/陈情令.jpg',
        'rating': u'7.7',
    },
    {
        'title': u'锦衣之下',
        'thumbnail': u'static/images/锦衣之下.jpg',
        'rating': u'7.6',
    }
]


@app.route('/')
def index():
    context = {
        'movies': movies,
        'tvs': tvs
    }
    return render_template('index.html', **context)


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283848586
```

模板index.html代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="{{ url_for('static', filename='css/base.css') }}">
    <link rel="stylesheet" href="{{ url_for('static', filename='css/item.css') }}">
    <title>豆瓣</title>
</head>
<body>
    {% macro itemGroup(thumbnail, title, rating) %}
        <div class="item-group">
            <img src="{{ thumbnail }}" alt="" class="thumbnail">
            <p class="item-title">{{ title }}</p>
            <p class="item-rating">
                {% set rating_light = ((rating|int)/2)|int %}
                {% set rating_half = (rating|int)%2 %}
                {% set rating_gray = 5 - rating_light - rating_half %}
                {% for foo in range(0, rating_light) %}
                    <img src="{{ url_for('static', filename='images/rate_light.png') }}" alt="">
                {% endfor %}
                {% for foo in range(0, rating_half) %}
                    <img src="{{ url_for('static', filename='images/rate_half.png') }}" alt="">
                {% endfor %}
                {% for foo in range(0, rating_gray) %}
                    <img src="{{ url_for('static', filename='images/rate_gray.png') }}" alt="">
                {% endfor %}
                {{ rating }}
            </p>
        </div>
    {% endmacro %}
    <h1>豆瓣影视评分</h1>
    <div class="container">
        <div class="search-group">
            <input type="text" class="search-input">
        </div>
        <div class="item-list-group">
            <div class="item-list-top">
                <span class="module-title">电影</span>
                <a href="#" class="more-btn">更多</a>
            </div>
            <div class="list-group">
                {% for movie in movies[:3] %}
                    {{ itemGroup(movie.thumbnail, movie.title, movie.rating) }}
                {% endfor %}
            </div>
        </div>
        <hr>
        <div class="item-list-group">
            <div class="item-list-top">
                <span class="module-title">电视剧</span>
                <a href="#" class="more-btn">更多</a>
            </div>
            <div class="list-group">
                {% for tv in tvs[:3] %}
                    {{ itemGroup(tv.thumbnail, tv.title, tv.rating) }}
                {% endfor %}

            </div>
        </div>
    </div>
</body>
</html>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162
```

显示：
![flask template exercise macro](https://img-blog.csdnimg.cn/20200414212246954.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，已经达到了初步效果。

可以看到，在展示电影和电视剧的模板代码部分，还是有部分重复的代码，因此可以再定义一个宏，如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="{{ url_for('static', filename='css/base.css') }}">
    <link rel="stylesheet" href="{{ url_for('static', filename='css/item.css') }}">
    <title>豆瓣</title>
</head>
<body>
    {% macro itemGroup(thumbnail, title, rating) %}
        <div class="item-group">
            <img src="{{ thumbnail }}" alt="" class="thumbnail">
            <p class="item-title">{{ title }}</p>
            <p class="item-rating">
                {% set rating_light = ((rating|int)/2)|int %}
                {% set rating_half = (rating|int)%2 %}
                {% set rating_gray = 5 - rating_light - rating_half %}
                {% for foo in range(0, rating_light) %}
                    <img src="{{ url_for('static', filename='images/rate_light.png') }}" alt="">
                {% endfor %}
                {% for foo in range(0, rating_half) %}
                    <img src="{{ url_for('static', filename='images/rate_half.png') }}" alt="">
                {% endfor %}
                {% for foo in range(0, rating_gray) %}
                    <img src="{{ url_for('static', filename='images/rate_gray.png') }}" alt="">
                {% endfor %}
                {{ rating }}
            </p>
        </div>
    {% endmacro %}
    {% macro listGroup(type, items) %}
        <div class="item-list-group">
            <div class="item-list-top">
                <span class="module-title">{{ type }}</span>
                <a href="#" class="more-btn">更多</a>
            </div>
            <div class="list-group">
                {% for item in items[:3] %}
                    {{ itemGroup(item.thumbnail, item.title, item.rating) }}
                {% endfor %}
            </div>
        </div>
    {% endmacro %}
    <h1>豆瓣影视评分</h1>
    <div class="container">
        <div class="search-group">
            <input type="text" class="search-input">
        </div>
        {{ listGroup('电影', movies) }}
        <hr>
        {{ listGroup('电视剧', tvs) }}
    </div>
</body>
</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354
```

效果与前者相同。

现进行进一步扩展，将公共部分代码抽象到父模板中，重复代码定义到宏中，进而来实现电影和电视剧详情列表。
视图函数代码如下：

```python
from flask import Flask, render_template, request

app = Flask(__name__)
app.config['TEMPLATES_AUTO_RELOAD'] = True

# 电影数据
movies = [
    {
        'title': u'绅士们',
        'thumbnail': '../static/images/绅士们.jpg',
        'rating': u'8.4',
    },
    {
        'title': u'饥饿站台',
        'thumbnail': u'../static/images/饥饿站台.jpg',
        'rating': u'7.8',
    },
    {
        'title': u'消失吧，群青',
        'thumbnail': u'../static/images/消失吧，群青.jpg',
        'rating': u'6.7',
    },
    {
        'title': u'再见，妈妈',
        'thumbnail': u'../static/images/再见，妈妈.jpg',
        'rating': u'8.1',
    },
    {
        'title': u'狩猎',
        'thumbnail': u'../static/images/狩猎.jpg',
        'rating': u'7.3',
    },
    {
        'title': u'隐形人',
        'thumbnail': u'../static/images/隐形人.jpg',
        'rating': u'7.3',
    }
]

# 电视剧数据
tvs = [
    {
        'title': u'清平乐',
        'thumbnail': '../static/images/清平乐.jpg',
        'rating': u'8.1',
    },
    {
        'title': u'民国神探',
        'thumbnail': u'../static/images/民国神探.jpg',
        'rating': u'7.1',
    },
    {
        'title': u'叹息桥',
        'thumbnail': u'../static/images/叹息桥.jpg',
        'rating': u'8.8',
    },
    {
        'title': u'重生',
        'thumbnail': u'../static/images/重生.jpg',
        'rating': u'6.6',
    },
    {
        'title': u'陈情令',
        'thumbnail': u'../static/images/陈情令.jpg',
        'rating': u'7.7',
    },
    {
        'title': u'锦衣之下',
        'thumbnail': u'../static/images/锦衣之下.jpg',
        'rating': u'7.6',
    }
]


@app.route('/')
def index():
    context = {
        'movies': movies,
        'tvs': tvs
    }
    return render_template('index.html', **context)


@app.route('/list/')
def show_list():
    category = int(request.args.get('category'))
    if category == 1:
        items = movies
        mtype = '电影'
    else:
        items = tvs
        mtype = '电视剧'
    return render_template('list.html', items = items, mtype = mtype)


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283848586878889909192939495969798
```

创建base.html如下：

```html
{% from 'macros.html' import itemGroup, listGroup %}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="{{ url_for('static', filename='css/base.css') }}">
    <link rel="stylesheet" href="{{ url_for('static', filename='css/item.css') }}">
    <title>
        {% block title %}
        
        {% endblock %}
    </title>
</head>
<body>
    {% block body_title %}
        <h1>豆瓣影视评分</h1>
    {% endblock %}
    <div class="container">
        <div class="search-group">
            <input type="text" class="search-input">
        </div>
        {% block content %}

        {% endblock %}
    </div>
</body>
</html>
12345678910111213141516171819202122232425262728
```

创建macro.html保存宏：

```html
{% macro itemGroup(thumbnail, title, rating) %}
    <div class="item-group">
        <img src="{{ thumbnail }}" alt="" class="thumbnail">
        <p class="item-title">{{ title }}</p>
        <p class="item-rating">
            {% set rating_light = ((rating|int)/2)|int %}
            {% set rating_half = (rating|int)%2 %}
            {% set rating_gray = 5 - rating_light - rating_half %}
            {% for foo in range(0, rating_light) %}
                <img src="{{ url_for('static', filename='images/rate_light.png') }}" alt="">
            {% endfor %}
            {% for foo in range(0, rating_half) %}
                <img src="{{ url_for('static', filename='images/rate_half.png') }}" alt="">
            {% endfor %}
            {% for foo in range(0, rating_gray) %}
                <img src="{{ url_for('static', filename='images/rate_gray.png') }}" alt="">
            {% endfor %}
            {{ rating }}
        </p>
    </div>
{% endmacro %}
{% macro listGroup(type, items, category=category) %}
    <div class="item-list-group">
        <div class="item-list-top">
            <span class="module-title">{{ type }}</span>
            <a href="{{ url_for('show_list', category=category) }}" class="more-btn">更多</a>
        </div>
        <div class="list-group">
            {% for item in items[:3] %}
                {{ itemGroup(item.thumbnail, item.title, item.rating) }}
            {% endfor %}

        </div>
    </div>
{% endmacro %}
1234567891011121314151617181920212223242526272829303132333435
```

index.html代码为：

```html
{% extends 'base.html' %}

{% block title %}
    首页
{% endblock %}


{% block body_title %}
    <h1>豆瓣影视评分</h1>
{% endblock %}

{% block content %}
    {{ listGroup('电影', movies, 1) }}
    <hr>
    {{ listGroup('电视剧', tvs, 2) }}
{% endblock %}
12345678910111213141516
```

创建list.html代码为：

```html
{% extends 'base.html' %}

{% block title %}
    {{ mtype }}
{% endblock %}


{% block body_title %}
    <h1>豆瓣{{ mtype }}评分</h1>
{% endblock %}

{% block content %}
    {% for item in items %}
        {{ itemGroup(item.thumbnail, item.title, item.rating) }}
    {% endfor %}
{% endblock %}
12345678910111213141516
```

显示：
![flask template exercise final](https://img-blog.csdnimg.cn/20200414212419247.gif#pic_center)
显然，已经达到了基本的预期效果。

在实现的过程中有关键的几点需要注意：

- 在定义宏时要注意代码重复的范围，并设置恰当的参数；
- 在设计模板继承时，要考虑到各个页面公共的代码，同时要注意在父模板中引用宏，在子模版中直接继承父模板，而不需要再引用宏；
- 在定义宏时，在`listGroup()`部分需要传入参数来判断在主页点击的是电影还是电视剧，从而返回相应的数据，这主要是使用`url_for()`函数进行映射，因为在`show_list()`函数中不存在category参数，所以在请求时会将参数体现到URL中，再在视图函数中通过`request.args.get()`方法来得到选择的category来传递数据。

# Python全栈（七）Flask框架之5.视图高级--类视图和蓝图

## 一、标准类视图及使用

在前面，我们定义视图都是通过route装饰器装饰函数来定义的，一般称之为视图函数。除了这种方式，还可以基于类实现。
类视图支持继承，但是类视图不能跟函数视图一样通过装饰器添加路由，需要通过`app.add_url_rule(url_rule,view_func)`来注册。

### 1.添加url映射规则的其他方法尝试

在之前的代码中，都是通过`@app.route`装饰器来实现url与视图函数的映射的，除了这种方法外，我们也可以通过Flask对象的`add_url_rule()`方法来定义映射规则。

测试（新建一个Python文件class_view.py）如下：

```python
from flask import Flask

app = Flask(__name__)


@app.route('/')
def index():
    return '首页'


def profile():
    return '个人中心'

# 添加url规则
app.add_url_rule('/profile/', view_func=profile)

if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819
```

显示：
![flask class view addurlrule](https://img-blog.csdnimg.cn/20200417190833884.gif#pic_center)
显然，`add_url_rule()`可以定义路由规则。

`add_url_rule()`方法有一个参数endpoint，表示结束点，指定后`url_for()`方法中传入的就不再是视图函数名了，而是指定的endpoint参数值，相当于给url取了一个名字。
通过请求上下文函数可以输出url_for的结果，测试如下：

```python
from flask import Flask, url_for

app = Flask(__name__)


@app.route('/')
def index():
    print(url_for('personal'))
    return '首页'


def profile():
    return '个人中心'


app.add_url_rule('/profile/', endpoint='personal', view_func=profile)

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920
```

此时访问http://127.0.0.1:5000/在控制台会打印`/profile/`。
此时不能再在`url_for()`方法中传入"profile"，否则会报错。

### 2.标准类视图

标准类视图继承自`flask.views.View`，并且类视图中必须重写`dispatch_request()`方法，这个方法类似于视图函数，会返回一个基于Response或者其子类的对象。类视图定义后，需要通过`add_url_rule()`方法和url进行映射，还需要在`as_view()`方法中指定该url的名称，方便`url_for()`函数调用。
定义类视图如下：

```python
from flask import Flask, url_for, views

app = Flask(__name__)


@app.route('/')
def index():
    print(url_for('personal'))
    return '首页'


def profile():
    return '个人中心'


class ListView(views.View):
    def dispatch_request(self):
        return 'class view'

    def demo(self):
        return '测试'


app.add_url_rule('/profile/', endpoint='personal', view_func=profile)
app.add_url_rule('/list/', endpoint='list', view_func=ListView.as_view('list'))

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526272829
```

访问http://127.0.0.1:5000/list/，显示：
![flask class view standard list](https://img-blog.csdnimg.cn/20200417191035437.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
此时，如果要用`url_for()`方法来获取URL，需要注意：
如果`add_url_rule()`中指定了endpoint参数，`url_for()`中传的参数就应该是endpoint参数的值；
如果没有指定，就是`as_view()`方法的值。

在定义视图类时，必须要重写`dispatch_request()`方法，否则会抛出异常，如下：
![flask class view NotImplementedError](https://img-blog.csdnimg.cn/20200417191130963.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
可以查看源码views.py：

```python
def dispatch_request(self):
    """Subclasses have to override this method to implement the
    actual view function code.  This method is called with all
    the arguments from the URL rule.
    """
    raise NotImplementedError()
123456
```

显然，如果不重写`dispatch_request()`方法，会直接抛出**NotImplementedError**错误。

再尝试用类视图传递Json数据：

```python
from flask import Flask, url_for, views, jsonify

app = Flask(__name__)


@app.route('/')
def index():
    return '首页'


def profile():
    return '个人中心'


class JsonView(views.View):
    def get_response(self):
        raise NotImplementedError()

    def dispatch_request(self):
        response = self.get_response()
        return jsonify(response)


class ListJsonView(JsonView):
    def get_response(self):
        return {'username': 'Corley'}


app.add_url_rule('/listjson/', view_func=ListJsonView.as_view('listjson'))

if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021222324252627282930313233
```

显示：
![flask class view standard json](https://img-blog.csdnimg.cn/202004171911571.gif#pic_center)
显然，此时返回页面的是json数据。

返回公共变量测试：

```python
from flask import Flask, url_for, views, jsonify, render_template

app = Flask(__name__)


@app.route('/')
def index():
    return '首页'


class LoginView(views.View):
    def dispatch_request(self):
        self.context = {
            'name': 'Corley'
        }
        return render_template('login.html', **self.context)


class RegisterView(views.View):
    def dispatch_request(self):
        self.context = {
            'name': 'Corley'
        }
        return render_template('register.html', **self.context)


app.add_url_rule('/login/', view_func=LoginView.as_view('login'))
app.add_url_rule('/register/', view_func=RegisterView.as_view('register'))

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526272829303132
```

在templates目录下创建**login.html**和**register.html**，login.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录页面</title>
</head>
<body>
    <h1>这是登录页面</h1>
    <h2>{{ name }}</h2>
</body>
</html>
1234567891011
```

register.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册页面</title>
</head>
<body>
    <h1>这是注册页面</h1>
    <h2>{{ name }}</h2>
</body>
</html>
1234567891011
```

显示：
![flask class view standard public variables](https://img-blog.csdnimg.cn/20200417191242883.gif#pic_center)
显然，可以正常访问登录和注册页面，但是两个视图类中含有相同的变量，可以将其抽取进行优化：

```python
from flask import Flask, url_for, views, jsonify, render_template

app = Flask(__name__)


@app.route('/')
def index():
    return '首页'


class BaseView(views.View):
    '''保存公共变量'''

    def __init__(self):
        super().__init__()
        self.context = {
            'name': 'Corley'
        }


class LoginView(BaseView):
    def dispatch_request(self):
        return render_template('login.html', **self.context)


class RegisterView(BaseView):
    def dispatch_request(self):
        return render_template('register.html', **self.context)


app.add_url_rule('/login/', view_func=LoginView.as_view('login'))
app.add_url_rule('/register/', view_func=RegisterView.as_view('register'))

if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021222324252627282930313233343536
```

将公有变量存储到BaseView，其他类视图再继承自BaseView。

## 二、基于调度方法的类视图

Flask中除了标准类视图，还有一种类视图**flask.views.MethodView**，对每个HTTP方法执行不同的函数，映射到对应方法的同名方法上。

测试如下：

```python
from flask import Flask, url_for, views, jsonify, render_template, request

app = Flask(__name__)


@app.route('/')
def index():
    return '首页'


class BaseView(views.View):
    '''保存公共变量'''

    def __init__(self):
        super().__init__()
        self.context = {
            'name': 'Corley'
        }


class LoginView(views.MethodView):
	# 当客户端通过get方法访问的时候执行
    def get(self):
        return render_template('login.html')

	# 当客户端通过post方法访问的时候执行
    def post(self):
        name = request.form.get('name')
        password = request.form.get('password')
        if name == 'Corley' and password == "123":
            return '登录成功！！！'
        else:
            error =  '账号或密码错误⚠'
            return render_template('login.html', error=error)


class RegisterView(BaseView):
    def dispatch_request(self):
        return render_template('register.html')


app.add_url_rule('/login/', view_func=LoginView.as_view('login'))
app.add_url_rule('/register/', view_func=RegisterView.as_view('register'))

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647
```

在Postman中请求：
![flask class view method login](https://img-blog.csdnimg.cn/20200417191331400.gif#pic_center)
显然，通过get和post请求都能实现登录的效果，代码还可以进行一定的优化：

```python
from flask import Flask, url_for, views, jsonify, render_template, request

app = Flask(__name__)


@app.route('/')
def index():
    return '首页'


class BaseView(views.View):
    '''保存公共变量'''

    def __init__(self):
        super().__init__()
        self.context = {
            'name': 'Corley'
        }


class LoginView(views.MethodView):
    def get(self, error=None):
        return render_template('login.html', error=error)

    def post(self):
        name = request.form.get('name')
        password = request.form.get('password')
        if name == 'Corley' and password == "123":
            return '登录成功！！！'
        else:
            error =  '账号或密码错误⚠'
            return self.get(error)


class RegisterView(BaseView):
    def dispatch_request(self):
        return render_template('register.html')


app.add_url_rule('/login/', view_func=LoginView.as_view('login'))
app.add_url_rule('/register/', view_func=RegisterView.as_view('register'))

if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021222324252627282930313233343536373839404142434445
```

运行效果与前者相同。

类视图的一个缺陷就是比较难用装饰器来装饰，比如需要做权限验证的时候。
此时可以在类视图中定义一个默认属性**decorators**，用于存储装饰器，以后每次调用这个类视图的时候，就会执行这个装饰器。
给视图函数添加装饰器，限制只有登录之后才能访问个人中心，如下：

```python
from flask import Flask, url_for, views, jsonify, render_template, request

app = Flask(__name__)


@app.route('/')
def index():
    return '首页'


def login_required(func):
    '''装饰器，登录之后才能访问个人中心'''
    def wrapper(*args, **kwargs):
        username = request.args.get('username')
        if username:
            return func(*args, **kwargs)
        else:
            return '请先登录再访问'
    return wrapper


@app.route('/profile/')
@login_required
def profile():
    return '个人中心'


if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021222324252627282930
```

显示：
![flask class view method decorator outside](https://img-blog.csdnimg.cn/20200417191503978.gif#pic_center)
很明显，只有登录之后才可以正常访问到个人中心。

通过在decorators列表中添加装饰器实现在类中使用装饰器，测试如下：

```python
from flask import Flask, url_for, views, jsonify, render_template, request

app = Flask(__name__)


@app.route('/')
def index():
    return '首页'


def login_required(func):
    '''装饰器，登录之后才能访问个人中心'''
    def wrapper(*args, **kwargs):
        username = request.args.get('username')
        if username:
            return func(*args, **kwargs)
        else:
            return '请先登录再访问'
    return wrapper


class ProfileView(views.View):
    # 在类中用装饰器
    decorators = [login_required]
    def dispatch_request(self):
        return '个人中心页面'


app.add_url_rule('/profile/', view_func=ProfileView.as_view('profile'))

if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021222324252627282930313233
```

效果与之前相同。

## 三、Flask蓝图的基本使用

之前，我们的代码都是放在一个Python文件里的，不同的功能模块都在一个文件中实现，代码量较少时尚可，随着业务需求的复杂化和代码的增多，项目逐渐变大，会显得很臃肿，也没有条理层次感，这显然不是一个合理的结构。此时就需要定义多个文件（夹），不同的文件（夹）放不同的功能模块，蓝图可以优雅地实现这种需求。
定义了蓝图文件之后，需要在主程序中通过`app.register_blueprint()`方法将这个蓝图注册进url映射中。
简单使用示例如下：
先创建项目根目录，如blue_demo，在下面创建Python文件blue_flask.py如下

```python
from flask import Flask
from blueprints.news import news_bp
from blueprints.book import book_bp

app = Flask(__name__)
app.register_blueprint(news_bp)
app.register_blueprint(book_bp)

@app.route('/')
def index():
    return '这是首页'


if __name__ == '__main__':
    app.run(debug=True)
123456789101112131415
```

再创建blueprints目录，下边创建news.py和book.py，news.py如下：

```python
from flask import Blueprint

news_bp = Blueprint('news', __name__)   # 第一个参数是蓝图名称，一般是当前的Python文件名，第二个参数是蓝图所在的包或模块，一般用__name__变量


@news_bp.route('/news/')
def news():
    return '新闻页面'
12345678
```

book.py如下：

```python
from flask import Blueprint

book_bp = Blueprint('book', __name__)   # 第一个参数是蓝图名称，一般是当前的Python文件名，第二个参数是蓝图所在的包或模块，一般用__name__变量


@book_bp.route('/book/')
def book():
    return '图书列表'


@book_bp.route('/book/detail/<bid>')
def book_detail(bid):
    return '当前图书ID：%s' % bid
12345678910111213
```

运行blue_flask.py之后，显示：
![flask blueprint simple use](https://img-blog.csdnimg.cn/20200417191630727.gif#pic_center)
显然，基本功能已经实现。
现在访问`/news/`、`/book/`，都是在执行news.py、book.py中的视图函数，从而实现了项目的模块化。

如果在blue_flask.py中不能导入blueprints中的文件或者出现警告信息，可以将当前目录设为根目录，如下：
![flask blueprint source root](https://img-blog.csdnimg.cn/20200417191657681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
此时再导入就会出现提示，导入后也不会出现警告信息。
Blueprint对象在实例化时，还可以传入一个参数**url_prefix**，即url前缀，会给当前文件下所有的路由加上指定的前缀，如上面的book.py可以改成

```python
from flask import Blueprint

book_bp = Blueprint('book', __name__, url_prefix='/book')   # 第一个参数是蓝图名称，一般是当前的Python文件名，第二个参数是蓝图所在的包或模块，一般用__name__变量


@book_bp.route('/')
def news():
    return '图书列表'


@book_bp.route('/detail/<bid>')
def book_detail(bid):
    return '当前图书ID：%s' % bid
12345678910111213
```

这与之前的效果是一样的。

## 四、Flask蓝图寻找文件和url_for()寻找路由

### 1.Flask蓝图寻找模板文件

Flask蓝图默认不设置任何模板文件的路径，将会在项目的templates中寻找模板文件。
也可以设置其他的路径，在构造函数Blueprint中有一个**template_folder**参数可以设置模板的路径。
修改news.py如下：

```python
from flask import Blueprint,render_template

news_bp = Blueprint('news', __name__, template_folder='blue_templates')


@news_bp.route('/news/')
def news():
    return render_template('news.hmtl')
12345678
```

修改templates中的news.html如下

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>新闻</title>
</head>
<body>
    <h1>这是模板中的新闻首页</h1>
</body>
</html>
12345678910
```

在blueprints中创建目录blue_templates，在下面创建news.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>新闻</title>
</head>
<body>
    <h1>这是蓝图中的新闻首页</h1>
</body>
</html>
12345678910
```

运行后，显示：
![flask blueprint template](https://img-blog.csdnimg.cn/20200417191726714.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，显示的是项目中的templates目录中的模板，进一步可以总结：
在蓝图中定义的视图函数在寻找模板时，首先会在项目的默认模板目录templates中寻找是否有指定的模板，如果存在则进行渲染，不存在则继续看蓝图文件中初始化Blueprint对象时是否传入了**template_folder**参数来指定模板目录，如果传了则在参数指定的目录中寻找，找到则进行渲染，否则报错，如果没有指定template_folder参数参数也会直接报错。
在实际项目中，一般直接将模板文件放在默认的模板目录templates下。

### 2.Flask蓝图寻找静态文件

默认不设置任何静态文件路径，Jinja2会在项目的static文件夹中寻找静态文件。
也可以设置其他的路径，在初始化蓝图的时候，Blueprint构造函数有一个参数**static_folder**可以指定静态文件的路径。
static_folder可以是相对路径（相对蓝图文件所在的目录），也可以是绝对路径。配置完蓝图后，在模板中引用静态文件应该使用`蓝图名.static`。
测试如下：
先在blue_demo目录下创建static目录，并在static目录下创建news.css如下：

```css
body{
    background: pink;
}
123
```

templates目录下的news.html导入css文件：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="{{ url_for('static', filename='news.css') }}">
    <title>新闻</title>
</head>
<body>
    <h1>这是模板中的新闻首页</h1>
</body>
</html>
1234567891011
```

同时在blueprints目录下创建blue_static目录，下面创建news.css如下：

```css
body{
    background: blue;
}
123
```

运行后可以看到：
![flask blueprint static root](https://img-blog.csdnimg.cn/20200417191802870.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，此时加载的模板文件是templates目录中的news.html，使用的样式文件是static目录下的news.css。

要想使用blue_static目录下的样式文件，需要改动，首先需要在Blueprint初始化时指定静态文件的文件夹：

```python
from flask import Blueprint,render_template

news_bp = Blueprint('news', __name__, static_folder='blue_static')


@news_bp.route('/news/')
def news():
    return render_template('news.html')
12345678
```

templates目录下的news.html导入css文件的路径修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="{{ url_for('news.static', filename='news.css') }}">
    <title>新闻</title>
</head>
<body>
    <h1>这是模板中的新闻首页</h1>
</body>
</html>
1234567891011
```

显示：
![flask blueprint static blue](https://img-blog.csdnimg.cn/20200417191845321.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
此时加载的样式文件即是blueprints目录下的静态文件夹中的样式文件。

### 3.蓝图中使用url_for()方法获取路由

在存在蓝图的情况下使用使用`url_for()`方法，不能直接给路由对应的函数名，需要在前面加上定义该路由所在的蓝图名字，即初始化蓝图时的第一个参数，一般即为该函数所在的Python文件的文件名，使用的格式是`蓝图名称.视图函数名称`。

测试如下：
blue_flask.py如下：

```python
from flask import Flask, url_for
from blueprints.news import news_bp
from blueprints.book import book_bp

app = Flask(__name__)
app.register_blueprint(news_bp)
app.register_blueprint(book_bp)

@app.route('/')
def index():
    print(url_for('news.news'))
    print(url_for('book.book_detail', bid=2))
    return '这是首页'


if __name__ == '__main__':
    app.run(debug=True)
1234567891011121314151617
```

book.py如下：

```python
from flask import Blueprint, url_for

book_bp = Blueprint('book', __name__, url_prefix='/book')


@book_bp.route('/')
def book():
    print(url_for('book.book_detail', bid=3))
    return '图书列表'


@book_bp.route('/detail/<bid>')
def book_detail(bid):
    return '当前图书ID：%s' % bid
1234567891011121314
```

显示：
![flask blueprint urlfor](https://img-blog.csdnimg.cn/20200417191920950.gif#pic_center)
此时查看控制台打印如下：

```python
127.0.0.1 - - [17/Apr/2020 17:41:17] "GET / HTTP/1.1" 200 -
/news/
/book/detail/2
/book/detail/3
127.0.0.1 - - [17/Apr/2020 17:41:27] "GET /book/ HTTP/1.1" 200 -
12345
```

显然，此时得到了视图函数对应的路由。

## 五、Flask实现子域名

**子域名**（subdomain）在域名系统等级中，属于更高一层域的域，比如，`mail.example.com`和`calendar.example.com`是`example.com`的两个子域名，而`example.com`则是顶级域`.com`的子域名。
子域名在很多网站中都会用到，比如对于一个网站xxx.com，我们可以定义一个子域名cms.xxx.com来作为该网站内容管理系统的网址。
在Flask中，子域名一般也是通过蓝图来实现的，在之前创建蓝图的时候添加了一个参数url_prefix来指定url前缀，比如`url_prefix='/user'`，我们可以通过`/user/`来访问user下的地址。但使用子域名则不同，需要在主程序中**配置SERVER_NAME**，除此之外，还需要在电脑的**系统文件hosts中进行配置**。
示例如下：
在blueprints目录下创建cms.py文件如下：

```python
from flask import Blueprint

cms_bp = Blueprint('cms', __name__, subdomain='cms')


@app.route('/')
def cms():
    return 'CMS页面'
12345678
```

在主程序中注册和配置服务器名称如下：

```python
from flask import Flask, url_for
from blueprints.news import news_bp
from blueprints.book import book_bp
from blueprints.cms import cms_bp

app = Flask(__name__)
app.register_blueprint(news_bp)
app.register_blueprint(book_bp)
app.register_blueprint(cms_bp)
# 配置服务器名
app.config["SERVER_NAME"] = 'corley.com:5000'

@app.route('/')
def index():
    print(url_for('news.news'))
    print(url_for('book.book_detail', bid=2))
    return '这是首页'


if __name__ == '__main__':
    app.run(debug=True)
123456789101112131415161718192021
```

此时还不能直接访问子域名，因为**子域名不支持在IP前直接添加**，即[cms.127.0.0.1](http://cms.127.0.0.1/)的形式是无效的，所以需要在电脑系统文件hosts中**添加域名解析**，即将[127.0.0.1](http://127.0.0.1/)映射到一个域名，才能访问子域名，Windows下操作如下：
在C盘中依次打开目录：**C:→Windows→System32→drivers→etc**，找到**hosts文件**并编辑，在该文件后面添加域名映射如下：

```python
# localhost name resolution is handled within DNS itself.
#	127.0.0.1       localhost
#	::1             localhost


127.0.0.1 corley.com
127.0.0.1 cms.corley.com
1234567
```

最后两行即为要添加的内容，将[127.0.0.1](http://127.0.0.1/)映射到[corley.com](http://corley.com/)，此时访问[corley.com](http://corley.com/)即访问本地回环地址[127.0.0.1](http://127.0.0.1/)。

此时开启服务，可以在控制台看到：

```python
 * Serving Flask app "blue_flask" (lazy loading)
 * Environment: production
   WARNING: Do not use the development server in a production environment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 260-774-670
 * Running on http://corley.com:5000/ (Press CTRL+C to quit)
123456789
```

即此时访问站点需要访问http://corley.com:5000/，不能再访问http://127.0.0.1:5000/了（除非你其他应用开启了此服务），此时访问示例如下：
![flask blueprint subdomain](https://img-blog.csdnimg.cn/20200419160756405.gif#pic_center)
显然，可以访问到子域名http://cms.corley.com:5000/。

注意：
子域名不仅不能在[127.0.0.1](http://127.0.0.1/)上出现，也不能在[localhost](http://localhost/)上出现。

# Python全栈（七）Flask框架之6.数据库介绍和SQLAlchemy使用

## 一、数据库介绍

### 1.数据库简介

![结绳记事](https://img-blog.csdnimg.cn/20200419162912614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
![甲骨文](https://img-blog.csdnimg.cn/20200419163042675.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
![青铜器文字](https://img-blog.csdnimg.cn/20200419163257308.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
![羊皮纸](https://img-blog.csdnimg.cn/20200419163610429.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
![光盘](https://img-blog.csdnimg.cn/20200419163422533.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
![数据库](https://img-blog.csdnimg.cn/20200419163527983.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
从以上几幅图可以看到，数据存储经历了从口口相传、结绳记事、用龟片或贝壳、用竹简、青铜器、纸（莎纸、羊皮）到软盘、光盘、硬盘文件等漫长的发展，**存储的内容**越来越多，**便捷性和持久性**也越来越高。

传统数据记录和存储的缺点：

- 不便于保存，易丢失；
- 备份困难；
- 查找不便。

现代文件存储方式虽然在很大程度上克服了上面的困难，但还是存在一定缺陷：

- 文件较大时，查询、写入、修改等方面的性能会大大降低；
- 不易扩展
  不同编程语言操作文件的方式不同，如果切换语言操作文件会很不方便，扩展性较低。

数据库的特点：

- 持久化存储
  数据存储到本地，不易丢失
- 读写速度极高
- 保证数据的有效性
  保证各个字段的类型正确，值有意义。
- 对程序支持性非常好，容易扩展

我们在网站或者应用软件是看到的具有美观格式的数据其实都是通过**查询后端**数据库并使用特定格式**渲染**出来的。

对数据库简单直白的理解：
一个Excel可以理解为一个数据库，一个Sheet可以理解为一个表，一个表的字段可以理解为表头，即一列对应一个字段，一行数据表示一条记录，同时不同的表中的数据还可以产生关系，如外键，如下图：
![数据库与Excel对比](https://img-blog.csdnimg.cn/20200419164433315.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

### 2.MySQL介绍

MySQL是一个**关系型数据库**，由瑞典**MySQL AB公司**开发，后来被**Sun公司**收购，Sun公司后来又被**Oracle公司**收购，目前属于Oracle旗下产品。

#### MySQL的特点

- 使用C和C++编写，并使用了多种编译器进行测试，保证源代码的**可移植性**
- 支持**多种操作系统**，如Linux、Windows、AIX、FreeBSD、HP-UX、MacOS、NovellNetware、OpenBSD、OS/2 Wrap、Solaris等
- 为**多种编程语言**提供了API，如C、C++、Python、Java、Perl、PHP、Eiffel、Ruby等
- 支持**多线程**，充分利用CPU资源
- 优化的SQL查询算法，有效地提高查询速度
- 提供多语言支持，常见的编码如GB2312、BIG5、UTF8
- 提供TCP/IP、ODBC和JDBC等多种数据库连接途径
- 提供用于管理、检查、优化数据库操作的管理工具
- 大型的数据库
  可以处理拥有上千万条记录的大型数据库
- 支持多种存储引擎
- MySQL 软件采用了双授权政策，它分为社区版和商业版，由于其体积小、速度快、总体拥有成本低，尤其是开放源码这一特点，一般中小型网站的开发都选择MySQL作为网站数据库
- MySQL使用标准的SQL数据语言形式
- Mysql是可以定制的，采用了GPL协议，你可以修改源码来开发自己的Mysql系统
- 在线DDL更改功能
- 复制全局事务标识
- 复制无崩溃从机
- 复制多线程从机

关于MySQL数据库的更多内容可参考[Python全栈（三）数据库优化之1.数据库简介](https://blog.csdn.net/CUFEECR/article/details/103246704)。

### 3.MySQL数据库的安装

#### 服务端安装

服务端用于开启MySQL服务。

- 官网下载安装
  访问官网下载，但是可能很慢
- 集成工具安装
  通过phpStudy等安装。
  可点击https://download.csdn.net/download/CUFEECR/12340408下载phpstudy并安装。

#### 客户端安装使用

- CMD
- 可视化工具
  - sqlyog
  - navicat
    sqlyog可点击https://download.csdn.net/download/CUFEECR/12340815下载，解压后按照说明安装。

在SQLYog中创建数据库、表和插入数据的演示如下：
![flask database create](https://img-blog.csdnimg.cn/2020041919335730.gif#pic_center)

新版Flask支持的MySQL版本必须**大于等于5.5**。

关于数据库安装的更多内容可参考[Python全栈（三）数据库优化之1.数据库简介](https://blog.csdn.net/CUFEECR/article/details/103246704)。

## 二、SQLAlchemy介绍和基本使用

### 1.SQLAlchemy简介

数据库是一个网站的基础，Flask可以使用很**多种数据库**，如MySQL、MongoDB、SQLite、PostgreSQL等。这里使用MySQL。
在Flask中，如果想要操作数据库，可以使用ORM，使用ORM操作数据库使数据库交互变得非常简单方便。

Flask操作数据库，需要先安装一些所需的库；
安装时，先进入虚拟环境所在目录，如果未切换到虚拟环境需要通过命令`pipenv shell`进入虚拟环境。
需要安装的库如下：

- pymysql
  pymysql是用Python来操作mysql的库，通过命令`pip install pymysql`进行安装。
- SQLAlchemy
  SQLAlchemy是一个数据库的ORM框架，通过命令`pip install SQLAlchemy`安装。
- mysql-connector
  用于通过Python连接数据库，安装命令为`pip install mysql-connector`和`pip install mysql-connector-python`。

### 2.SQLAlchemy的基本使用

使用sqlalchemy执行原生SQL语句示例如下：

```python
from sqlalchemy import create_engine


#  主机地址
HOSTNAME = '127.0.0.1'
# 数据库
DATABASE = 'flask_demo'
# 端口号，一般固定，默认为3306
PORT = 3306
# 用户名和密码
USERNMAE = 'root'
PASSWORD = 'root'

# 创建数据库引擎
DB_URL = 'mysql+pymysql://{}:{}@{}:{}/{}'.format(USERNMAE, PASSWORD, HOSTNAME, PORT, DATABASE)
engine = create_engine(DB_URL)

# 创建连接
with engine.connect() as conn:
    # 执行原生SQL语句
    result = conn.execute("select * from users")
    print(result.fetchone())
12345678910111213141516171819202122
```

首先用sqlalchemy导入的函数`create_engine()`创建引擎实例，其中传入该函数的参数形式为：

```python
dialect+driver://username:password@host:port/database?charset=utf8
1
```

其中，dialect是数据库的类型，比如MySQL、PostgreSQL、SQLite等，并且转换成小写；
driver是Python中操作对应数据库的驱动，如果不指定，会选择默认的驱动，比如MySQL的默认驱动是MySQLdb；
username是连接数据库的用户名；
password是该用户的密码；
host是连接数据库的域名；
port是数据库监听的端口号；
database是需要连接的数据库的名字。

再调用该实例的`connect()`方法连接数据库，使用with语句连接数据库，如果发生异常会被捕获。

运行结果如下：

```python
XXX\Python\Python37\lib\site-packages\pymysql\cursors.py:170: Warning: (1366, "Incorrect string value: '\\xD6\\xD0\\xB9\\xFA\\xB1\\xEA...' for column 'VARIABLE_VALUE' at row 485")
  result = self._query(query)
(1, 'cl', 'corley', '123')
123
```

查询到了第一条数据，但是同时有一条警告消息，这是创建数据库引擎时使用的DB_URL产生的问题，猜测可能是MySQL驱动的问题，可以将pymysql连接数据库换成官方的连接引擎，即使用之前安装的**mysql-connector**来解决这个问题，如下：

```python
from sqlalchemy import create_engine


#  主机地址
HOSTNAME = '127.0.0.1'
# 数据库
DATABASE = 'flask_demo'
# 端口号，一般固定，默认为3306
PORT = 3306
# 用户名和密码
USERNMAE = 'root'
PASSWORD = 'root'

# 创建数据库引擎
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}'.format(USERNMAE, PASSWORD, HOSTNAME, PORT, DATABASE)
engine = create_engine(DB_URL)

# 创建连接
with engine.connect() as conn:
    # 执行原生SQL语句
    result = conn.execute("select * from users")
    for r in result:
        print(r)
1234567891011121314151617181920212223
```

打印：

```python
(1, 'cl', 'corley', '123')
(2, 'tn', 'tony', '12345')
12
```

显然，此时未出现警告信息。

执行更多原生SQL语句测试如下：
先在Python文件同级目录下创建文件constants.py，将之前文件中数据库配置选项的常量都放入该文件中，如下：

```python
#  主机地址
HOSTNAME = '127.0.0.1'
# 数据库
DATABASE = 'flask_demo'
# 端口号，一般固定，默认为3306
PORT = 3306
# 用户名和密码
USERNMAE = 'root'
PASSWORD = 'root'

DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}'.format(USERNMAE, PASSWORD, HOSTNAME, PORT, DATABASE)
1234567891011
```

同时编辑执行原生SQL语句的Python文件如下：

```python
from sqlalchemy import create_engine
from constants import DB_URL

# 创建数据库引擎
engine = create_engine(DB_URL)

# 创建连接
with engine.connect() as conn:
    # 判断表存在则删除
    conn.execute("drop table if exists staffs")
    # 创建表
    conn.execute("create table staffs(id int primary key auto_increment, name varchar(30), salary int)")
    # 插入数据
    conn.execute("insert into staffs values(default,'Corley', 10000),(default, 'Tony', 8000)")
    # 查询数据
    results = conn.execute("select * from staffs")
    # 显示数据
    for result in results:
        print(result)

1234567891011121314151617181920
```

运行之后，打印：

```python
(1, 'Corley', 10000)
(2, 'Tony', 8000)
12
```

## 三、ORM介绍和SQLAlchemy的使用

### 1.ORM介绍

随着项目规模的增大，采用原生SQL的方式会导致代码中大量的SQL语句，并对项目的进展产生不利影响：

- 代码**复用率较低**
  SQL语句较多，重复率较高，复用性较低，越复杂的SQL语句条件越多，代码会越来越多。
- 修改**不方便**
  很多SQL语句是在业务逻辑中拼出来的，如果数据库需要更改，就要去修改这些代码逻辑，很容易漏掉或增加某些SQL语句的修改
- **安全性**受影响
  原生的SQL语句SQL注入的风险较大。

此时我们可以选择ORM进行优化。
ORM（Object Relationship Mapping），即**对象关系映射**，通过ORM，我们可以使用**类和对象**的方式去操作数据库，而不用写原生的SQL语句。
通过把表映射成类，把字段作为属性，把行作为实例，ORM在执行对象操作时最终把对应的操作转换为数据库的原生语句。

ORM的优点如下：

- 易用性
  使用ORM进行数据库开发可以有效减少SQL语句，写出来的模型也更加直观。
- 性能损耗小
- 设计灵活
  可以更轻松地写出复杂的查询语句。
- 可移植性
  SQLAlchemy封装了底层的数据库实现，支持多个关系型数据库，包括MySQL、SQLite等。

模型通过ORM再转到数据库中操作，执行过程如下：
![ORM到数据库](https://img-blog.csdnimg.cn/20200419193505402.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

### 2.SQLAlchemy的简单使用

要使用ORM来操作数据库，首先需要创建一个类来与对应的表进行映射。

使用ORM创建表示例如下：

```python
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base

#  主机地址
HOSTNAME = '127.0.0.1'
# 数据库
DATABASE = 'flask_demo'
# 端口号，一般固定，默认为3306
PORT = 3306
# 用户名和密码
USERNMAE = 'root'
PASSWORD = 'root'

# 创建数据库引擎
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}'.format(USERNMAE, PASSWORD, HOSTNAME, PORT, DATABASE)
engine = create_engine(DB_URL)

# 继承declarative_base函数生成的基类
Base = declarative_base(engine)

# ORM操作数据库
class Students(Base):
    # 指定数据库中的表
    __tablename__ = 'students'
    # 给定字段
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    gender = Column(Integer, default=1,comment='1为男，2为女')

	# 格式化打印，打印当前对象时会调用
    def __repr__(self):
        return "<User(id='%s', name='%s', fullname='%s', password='%s')>" % (self.id, self.name, self.fullname, self.password)

# 模型映射到数据库中
Base.metadata.create_all()
1234567891011121314151617181920212223242526272829303132333435
```

创建类时，要使所有的类都继承自`declarative_base()`函数生成的基类；
SQLAlchemy会自动为第一个类型为Integer、标记为主键并且没有被标记为外键的字段添加自增长的属性，因此上例中定义id字段时使用`id = Column(Integer, primary_key=True)`语句即不设置**autoincrement属性**也会自动让id自增；
创建完和表映射的类后，还没有真正映射到数据库当中，需要执行代码`Base.metadata.create_all()`才能将类映射到数据库中。
运行后查看sqlyog，刷新后可以看到：
![ORM 创建数据库表](https://img-blog.csdnimg.cn/20200419193530343.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，students表已经创建成功。

在类中先定义表名，再定义字段，定义字段要用Column实例化后的对象实例；
`Column()`实例化时的参数中，第一个是数据类型，剩下的是约束；
如果类型是String时，必须指定字符串最大长度。

# Python全栈（七）Flask框架之7.ORM增删改查、数据类型和参数

## 一、Flask-ORM添加数据

先创建表：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE= 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)

class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)

Base.metadata.create_all()
123456789101112131415161718192021
```

运行之后，会看到数据库中多了一个article表。
此时实例化一条数据并访问属性如下：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE= 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)

class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)

# Base.metadata.create_all()
article = Article(name='Corley')
print(article.id)
print(article.name)
123456789101112131415161718192021222324
```

打印：

```python
None
Corley
12
```

显然，此时name可以正常打印，但是id为None，这是因为id是一个自增的主键，还未插入到数据库表中，id不存在，所以为空。
将数据提交到数据库，需要使用**Session对象**。
提交记录到数据库中：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE= 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)

class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)

# Base.metadata.create_all()
article = Article(name='Corley')

# 保存数据到数据库中
Session = sessionmaker(bind=engine)
session = Session() # 含有__call__魔术方法，可以将Session类变成方法调用
# 添加数据
session.add(article)
# 提交数据
session.commit()
print(article.id)
print(article.name)
123456789101112131415161718192021222324252627282930313233
```

打印：

```python
1
Corley
12
```

显然，此时得到了对应的id，再查看数据库article表，发现数据成功插入。
可以看出，把数据添加到session中之后，还要commit才能真正将数据保存到数据库，因为在SQLAlchemy的ORM实现中，在做commit操作之前，所有的操作都是在事务中进行的，因此如果要将事务中的操作真正映射到数据库中，还需要做**commit操作**。
说明：
sessionmaker是一个类，实例化生成Session对象，因为sessionmaker类中有魔法方法`__call__()`，所以可以**直接将对象调用**，Session成了一个可调用对象，即把实例对象用类似函数的形式调用，模糊了函数和对象之间的概念。
添加多条数据：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE= 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)

class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)

article1 = Article(name='Tony')
article2 = Article(name='Jack')

# 保存数据到数据库中
Session = sessionmaker(bind=engine)
session = Session()
# 添加数据
session.add_all([article1, article2])
# 提交数据
session.commit()
12345678910111213141516171819202122232425262728293031
```

运行之后，查看数据库会发现又多了2条数据。

## 二、Flask-ORM数据的增删改查

先创建表：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE= 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)

class Blog(Base):
    __tablename__ = 'blog'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    content = Column(String(500))
    author = Column(String(30))

Base.metadata.create_all()
123456789101112131415161718192021222324
```

运行后即创建表blog。
插入数据：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Blog(Base):
    __tablename__ = 'blog'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    content = Column(String(500))
    author = Column(String(30))


# Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()


def add_data():
    blog = Blog(title='Python', content='人生苦短，我用Python', author='Jack')
    session.add(blog)
    session.commit()


if __name__ == '__main__':
    add_data()
1234567891011121314151617181920212223242526272829303132333435363738
```

运行即插入数据成功。
注意：
此时在实例化Blog对象时要显式传入参数，否则会报错，即`Blog('Python', '人生苦短，我用Python', 'Jack')`是会报错的。
查找操作是通过`session.query()`方法实现的，这个方法会返回一个Query对象，Query对象相当于一个数组，装载了查找出来的数据，可以进行迭代。
查询所有数据用`all()`方法，测试如下：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Blog(Base):
    __tablename__ = 'blog'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    content = Column(String(500))
    author = Column(String(30))


# Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()


def add_data():
    blog = Blog(title='C', content='带你学C带你飞', author='Tom')
    session.add(blog)
    session.commit()


def search_data():
    data = session.query(Blog).all()
    for item in data:
        print(item.id, item.title, item.content, item.author)


def update_data():
    pass


if __name__ == '__main__':
    search_data()
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748
```

打印：

```python
1 Python 人生苦短，我用Python Jack
2 C 带你学C带你飞 Tom
12
```

显然，查询到所有数据。
还可以在定义模型时定义`__str__()`方法，从而可以自定义打印对象：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Blog(Base):
    __tablename__ = 'blog'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    content = Column(String(500))
    author = Column(String(30))

    def __str__(self):
        return 'Blog(id:{}, title:{}, content:{}, author:{})'.format(self.id, self.title, self.content, self.author)


# Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()


def add_data():
    blog = Blog(title='C', content='带你学C带你飞', author='Tom')
    session.add(blog)
    session.commit()


def search_data():
    data = session.query(Blog).all()
    for item in data:
        print(item)


def update_data():
    pass


if __name__ == '__main__':
    search_data()
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051
```

打印：

```python
Blog(id:1, title:Python, content:人生苦短，我用Python, author:Jack)
Blog(id:2, title:C, content:带你学C带你飞, author:Tom)
12
```

还可以指定属性查询：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Blog(Base):
    __tablename__ = 'blog'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    content = Column(String(500))
    author = Column(String(30))

    def __str__(self):
        return 'Blog(id:{}, title:{}, content:{}, author:{})'.format(self.id, self.title, self.content, self.author)


# Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()


def add_data():
    blog = Blog(title='C', content='带你学C带你飞', author='Tom')
    session.add(blog)
    session.commit()


def search_data():
    data = session.query(Blog.content, Blog.author).order_by(Blog.author)
    for item in data:
        print(item)


def update_data():
    pass


if __name__ == '__main__':
    search_data()
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051
```

打印：

```python
('人生苦短，我用Python', 'Jack')
('带你学C带你飞', 'Tom')
12
```

显然，此时返回的是元组；
如果传递了**两个或两个以上的对象**，或者传递的是**ORM类的属性**，查找得到的结果就是元组。

可以对查询的结果**切片**，如下：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Blog(Base):
    __tablename__ = 'blog'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    content = Column(String(500))
    author = Column(String(30))

    def __str__(self):
        return 'Blog(id:{}, title:{}, content:{}, author:{})'.format(self.id, self.title, self.content, self.author)


# Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()


def add_data():
    blog = Blog(title='C', content='带你学C带你飞', author='Tom')
    session.add(blog)
    session.commit()


def search_data():
    data = session.query(Blog.content)[1:]
    for item in data:
        print(item)


def update_data():
    pass


if __name__ == '__main__':
    search_data()
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051
```

打印：

```python
('带你学C带你飞',)
1
```

如果要对结果进行过滤，可以使用`filter()`和`filter_by()`两个方法。
这两个方法都是用来过滤的，区别在于，`filter_by()`传入的参数是关键字，`filter()`传入的参数是条件判断，能够传入的条件更多、更灵活。
用`filter()`方法测试如下：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Blog(Base):
    __tablename__ = 'blog'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    content = Column(String(500))
    author = Column(String(30))

    def __str__(self):
        return 'Blog(id:{}, title:{}, content:{}, author:{})'.format(self.id, self.title, self.content, self.author)


# Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()


def add_data():
    blog = Blog(title='C', content='带你学C带你飞', author='Tom')
    session.add(blog)
    session.commit()


def search_data():
    data = session.query(Blog).filter(Blog.title=='Python').all()
    for item in data:
        print(item)


def update_data():
    pass


if __name__ == '__main__':
    search_data()
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051
```

打印：

```
Blog(id:1, title:Python, content:人生苦短，我用Python, author:Jack)
1
```

用`filter_by()`方法使用示例如下：

```
def search_data():
    data = session.query(Blog).filter_by(title='C').all()
    for item in data:
        print(item)
1234
```

这与前者的方法是等效的。

只查询第一条数据用`first()`方法，测试如下：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Blog(Base):
    __tablename__ = 'blog'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    content = Column(String(500))
    author = Column(String(30))

    def __str__(self):
        return 'Blog(id:{}, title:{}, content:{}, author:{})'.format(self.id, self.title, self.content, self.author)


# Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()


def add_data():
    blog = Blog(title='C', content='带你学C带你飞', author='Tom')
    session.add(blog)
    session.commit()


def search_data():
    data = session.query(Blog).first()
    print(data)


def update_data():
    pass


if __name__ == '__main__':
    search_data()
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
```

打印：

```python
Blog(id:1, title:Python, content:人生苦短，我用Python, author:Jack)
1
```

根据id指定查询数据用`get()`方法，传入的参数即为所需查询的数据对应的id，测试如下：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Blog(Base):
    __tablename__ = 'blog'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    content = Column(String(500))
    author = Column(String(30))

    def __str__(self):
        return 'Blog(id:{}, title:{}, content:{}, author:{})'.format(self.id, self.title, self.content, self.author)


# Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()


def add_data():
    blog = Blog(title='C', content='带你学C带你飞', author='Tom')
    session.add(blog)
    session.commit()


def search_data():
    data = session.query(Blog).get(2)
    print(data)


def update_data():
    pass


if __name__ == '__main__':
    search_data()
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
```

打印：

```python
Blog(id:2, title:C, content:带你学C带你飞, author:Tom)
1
```

显然，查询到的即为第2条数据。
如果`get()`方法传的参数不存在，会返回None。

修改数据时，直接**调用对象属性并重新赋值**即可，测试如下：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Blog(Base):
    __tablename__ = 'blog'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    content = Column(String(500))
    author = Column(String(30))

    def __str__(self):
        return 'Blog(id:{}, title:{}, content:{}, author:{})'.format(self.id, self.title, self.content, self.author)


# Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()


def add_data():
    blog = Blog(title='C', content='带你学C带你飞', author='Tom')
    session.add(blog)
    session.commit()


def search_data():
    data = session.query(Blog).get(2)
    print(data)


def update_data():
    # 先查询数据
    blog = session.query(Blog).first()
    # 再修改数据
    blog.author = 'Foster'
    session.commit()
    print(blog)


if __name__ == '__main__':
    update_data()
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455
```

打印：

```python
Blog(id:1, title:Python, content:人生苦短，我用Python, author:Foster)
1
```

此时再查看数据库，数据也改变了。

删除数据用`delete()`方法：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Blog(Base):
    __tablename__ = 'blog'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    content = Column(String(500))
    author = Column(String(30))

    def __str__(self):
        return 'Blog(id:{}, title:{}, content:{}, author:{})'.format(self.id, self.title, self.content, self.author)


# Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()


def add_data():
    blog = Blog(title='C', content='带你学C带你飞', author='Tom')
    session.add(blog)
    session.commit()


def search_data():
    data = session.query(Blog).all()
    for item in data:
        print(item)


def update_data():
    # 先查询数据
    blog = session.query(Blog).first()
    # 再修改数据
    blog.author = 'Foster'
    session.commit()
    print(blog)


def delete_data():
    blog = session.query(Blog).first()
    session.delete(blog)
    session.commit()
    search_data()


if __name__ == '__main__':
    delete_data()

12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364
```

打印：

```python
Blog(id:5, title:C, content:带你学C带你飞, author:Tom)
1
```

此时查看数据库表，发现一条数据已经删除，是永久地删除了这些数据。
但是在实际开发中一般**不能直接删除数据，而是增加一个字段**，比如`is_delete`字段来标记是否删除，值1表示已经删除，值0表示未删除，在查询的时候只查询`is_delete`字段值为0的数据即可。

如果需要恢复到修改之前的数据，需要用到回滚`rollback()`，测试如下：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Blog(Base):
    __tablename__ = 'blog'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    content = Column(String(500))
    author = Column(String(30))

    def __str__(self):
        return 'Blog(id:{}, title:{}, content:{}, author:{})'.format(self.id, self.title, self.content, self.author)


# Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()


def add_data():
    blog = Blog(title='C', content='带你学C带你飞', author='Tom')
    session.add(blog)
    session.commit()


def search_data():
    data = session.query(Blog).all()
    for item in data:
        print(item)


def update_data():
    # 先查询数据
    blog = session.query(Blog).first()
    print(blog)
    # 再修改数据
    blog.author = 'Alan'
    print(blog)
    session.rollback()
    print(blog)
    session.commit()
    print(blog)


def delete_data():
    blog = session.query(Blog).get(3)
    session.delete(blog)
    session.commit()
    search_data()


if __name__ == '__main__':
    update_data()

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768
```

打印：

```python
Blog(id:3, title:Python, content:人生苦短，我用Python, author:Foster)
Blog(id:3, title:Python, content:人生苦短，我用Python, author:Alan)
Blog(id:3, title:Python, content:人生苦短，我用Python, author:Foster)
Blog(id:3, title:Python, content:人生苦短，我用Python, author:Foster)
1234
```

显然，在回滚之后，数据又恢复到初始的状态，`commit()`之后数据也未发生变化，查看数据库中数据也未发生改变。

## 三、SQLAlchemy常用数据类型

sqlalchemy常用数据类型如下：

- Integer
  整型。
- Float
  浮点类型。
- Boolean
  传递True或者False。
- DECIMAL
  定点类型，可以指定小数的位数和精度。
- Enum
  枚举类型。
- Date
  传递`datetime.date()`。
- Time
  传递`datetime.time()`。
- DateTime
  传递`datetime.datetime()`。
- String
  字符类型，使用时需要指定长度，区别于Text类型。
- Text
  文本类型。
- LONGTEXT
  长文本类型。

Float类型测试：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(Float)



Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()

book1 = Book(name='Python', price=12.34)
book2 = Book(name='Java', price=12.3456789)
session.add_all([book1, book2])
session.commit()
123456789101112131415161718192021222324252627282930313233
```

运行之后，查询数据库：

```sql
select * from book;
1
```

打印：

```sql
+----+--------+---------+
| id | name   | price   |
+----+--------+---------+
|  1 | Python |   12.34 |
|  2 | Java   | 12.3457 |
+----+--------+---------+
2 rows in set (0.00 sec)
1234567
```

显然，当小数位数较多时，只保留4位小数，损失了一部分精度。

DECIMAL类型测试如下：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String, DECIMAL
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(DECIMAL(20, 8))      # DECIMAL()的第一个参数为总位数，第二个参数为小数位数


# 删除表book以便重新创建
Base.metadata.drop_all()
Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()

book1 = Book(name='Python', price=12.34)
book2 = Book(name='Java', price=12.3456789)
session.add_all([book1, book2])
session.commit()

1234567891011121314151617181920212223242526272829303132333435
```

运行后查询数据库：

```sql
+----+--------+-------------+
| id | name   | price       |
+----+--------+-------------+
|  1 | Python | 12.34000000 |
|  2 | Java   | 12.34567890 |
+----+--------+-------------+
2 rows in set (0.01 sec)
1234567
```

显然，此时数据精度更高，在对精度要求较高的情况下可以使用这种数据类型。

Boolean类型测试：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String, DECIMAL, Boolean
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(DECIMAL(20, 8))
    is_delete = Column(Boolean)


# 删除表book以便重新创建
Base.metadata.drop_all()
Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()

book1 = Book(name='Python', price=12.34, is_delete=True)
book2 = Book(name='Java', price=12.3456789, is_delete=False)
session.add_all([book1, book2])
session.commit()

123456789101112131415161718192021222324252627282930313233343536
```

运行后查询数据库：

```sql
+----+--------+-------------+-----------+
| id | name   | price       | is_delete |
+----+--------+-------------+-----------+
|  1 | Python | 12.34000000 |         1 |
|  2 | Java   | 12.34567890 |         0 |
+----+--------+-------------+-----------+
2 rows in set (0.00 sec)
1234567
```

Enum类型测试：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String, DECIMAL, Boolean, Enum
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(DECIMAL(20, 8))
    is_delete = Column(Boolean)
    btype = Column(Enum('Science', 'Computer'))


# 删除表book以便重新创建
Base.metadata.drop_all()
Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()

book1 = Book(name='Python', price=12.34, is_delete=True, btype='Science')
book2 = Book(name='Java', price=12.3456789, is_delete=False, btype='Computer')
session.add_all([book1, book2])
session.commit()

12345678910111213141516171819202122232425262728293031323334353637
```

运行之后查询数据库：

```sql
+----+--------+-------------+-----------+----------+
| id | name   | price       | is_delete | btype    |
+----+--------+-------------+-----------+----------+
|  1 | Python | 12.34000000 |         1 | Science  |
|  2 | Java   | 12.34567890 |         0 | Computer |
+----+--------+-------------+-----------+----------+
2 rows in set (0.01 sec)
1234567
```

Datetime类型测试：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String, DECIMAL, Boolean, Enum, DateTime
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base
from datetime import datetime, timedelta

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(DECIMAL(20, 8))
    is_delete = Column(Boolean)
    btype = Column(Enum('Science', 'Computer'))
    release_time = Column(DateTime)


# 删除表book以便重新创建
Base.metadata.drop_all()
Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()

book1 = Book(name='Python', price=12.34, is_delete=True, btype='Science', release_time=datetime.now())
book2 = Book(name='Java', price=12.3456789, is_delete=False, btype='Computer', release_time=datetime.now() - timedelta(days=50))
session.add_all([book1, book2])
session.commit()

123456789101112131415161718192021222324252627282930313233343536373839
```

运行后查询数据库：

```sql
+----+--------+-------------+-----------+----------+---------------------+
| id | name   | price       | is_delete | btype    | release_time        |
+----+--------+-------------+-----------+----------+---------------------+
|  1 | Python | 12.34000000 |         1 | Science  | 2020-04-22 12:36:04 |
|  2 | Java   | 12.34567890 |         0 | Computer | 2020-03-03 12:36:04 |
+----+--------+-------------+-----------+----------+---------------------+
2 rows in set (0.01 sec)
1234567
```

Date类型和Time类型与DateTime类型用法类似。

LONGTEXT测试：
创建constants.py如下：

```python
longtext_python = '''
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
'''

longtext_java = '''
Java is a technology for developing applications that makes the web more interesting and useful. Java is not the same as JavaScript, which is a simple technology for creating web pages and can only run in a browser.
With Java, you can play games, upload photos, chat online, participate in virtual experience, and use online training, online banking, interactive map and other services. If Java is not installed, many applications and websites will not work.
By default, Java will automatically notify you of new updates to install. To ensure the latest software and computer security, you must accept and install the update. If you have been notified to update Java on a Windows computer but you remember never downloading or installing it, it is possible that Java has been preloaded with your new computer.
'''
1234567891011121314151617181920212223242526272829
```

测试LONGTEXT代码如下：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String, DECIMAL, Boolean, Enum, DateTime
from sqlalchemy.dialects.mysql import LONGTEXT
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base
from datetime import datetime, timedelta
from constants import *

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(DECIMAL(20, 8))
    is_delete = Column(Boolean)
    btype = Column(Enum('Science', 'Computer'))
    release_time = Column(DateTime)
    content = Column(LONGTEXT)


# 删除表book以便重新创建
Base.metadata.drop_all()
Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()

book1 = Book(name='Python', price=12.34, is_delete=True, btype='Science', release_time=datetime.now(), content=longtext_python)
book2 = Book(name='Java', price=12.3456789, is_delete=False, btype='Computer', release_time=datetime.now() - timedelta(days=50), content=longtext_java)
session.add_all([book1, book2])
session.commit()

123456789101112131415161718192021222324252627282930313233343536373839404142
```

运行后，查询数据库：

```sql
+----+--------+-------------+-----------+----------+---------------------+----------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------+                               
| id | name   | price       | is_delete | btype    | release_time        | content                                                        
                                                                                                                                          
                                                                                                                                          
                                                                                                                                          
                                                                                                                                          
                                                                                                                                          
                                                                                                          |                               
+----+--------+-------------+-----------+----------+---------------------+----------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------+                               
|  1 | Python | 12.34000000 |         1 | Science  | 2020-04-22 17:44:40 |                                                                
The Zen of Python, by Tim Peters                                                                                                          
                                                                                                                                          
Beautiful is better than ugly.                                                                                                            
Explicit is better than implicit.                                                                                                         
Simple is better than complex.                                                                                                            
Complex is better than complicated.                                                                                                       
Flat is better than nested.                                                                                                               
Sparse is better than dense.                                                                                                              
Readability counts.                                                                                                                       
Special cases aren't special enough to break the rules.                                                                                   
Although practicality beats purity.                                                                                                       
Errors should never pass silently.                                                                                                        
Unless explicitly silenced.                                                                                                               
In the face of ambiguity, refuse the temptation to guess.                                                                                 
There should be one-- and preferably only one --obvious way to do it.                                                                     
Although that way may not be obvious at first unless you're Dutch.                                                                        
Now is better than never.                                                                                                                 
Although never is often better than *right* now.                                                                                          
If the implementation is hard to explain, it's a bad idea.                                                                                
If the implementation is easy to explain, it may be a good idea.                                                                          
Namespaces are one honking great idea -- let's do more of those!                                                                          
 |                                                                                                                                        
|  2 | Java   | 12.34567890 |         0 | Computer | 2020-03-03 17:44:40 |                                                                
Java is a technology for developing applications that makes the web more interesting and useful. Java is not the same as JavaScript, which
 is a simple technology for creating web pages and can only run in a browser.                                                             
With Java, you can play games, upload photos, chat online, participate in virtual experience, and use online training, online banking, int
eractive map and other services. If Java is not installed, many applications and websites will not work.                                  
By default, Java will automatically notify you of new updates to install. To ensure the latest software and computer security, you must ac
cept and install the update. If you have been notified to update Java on a Windows computer but you remember never downloading or installi
ng it, it is possible that Java has been preloaded with your new computer.                                                                
                                                |                                                                                         
+----+--------+-------------+-----------+----------+---------------------+----------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------+                               
2 rows in set (0.00 sec)                                                                                                                  
                                                                                                                                          
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162
```

## 四、Column常用参数和query可用参数

### 1.Column常用参数

常用参数如下：

- default
  默认值。
- nullable
  是否可空。
- primary_key
  是否为主键。
- unique
  是否唯一。
- autoincrement
  是否自动增长。
- onupdate
  指定在**更新数据**的时候执行的函数。
- name
  该属性在数据库中的字段映射。

onupdate测试如下：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String, DECIMAL, Boolean, Enum, DateTime
from sqlalchemy.dialects.mysql import LONGTEXT
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base
from datetime import datetime, timedelta

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(DECIMAL(20, 8))
    is_delete = Column(Boolean)
    btype = Column(Enum('Science', 'Computer'))
    release_time = Column(DateTime)
    update_time = Column(DateTime, onupdate=datetime.now())


# 删除表book以便重新创建
Base.metadata.drop_all()
Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()

book1 = Book(name='Python', price=12.34, is_delete=True, btype='Science', release_time=datetime.now(), content=longtext_python)
book2 = Book(name='Java', price=12.3456789, is_delete=False, btype='Computer', release_time=datetime.now() - timedelta(days=50), content=longtext_java)
session.add_all([book1, book2])
session.commit()

1234567891011121314151617181920212223242526272829303132333435363738394041
```

运行后查询数据库：

```sql
+----+--------+-------------+-----------+----------+---------------------+-------------+ 
| id | name   | price       | is_delete | btype    | release_time        | update_time | 
+----+--------+-------------+-----------+----------+---------------------+-------------+ 
|  1 | Python | 12.34000000 |         1 | Science  | 2020-04-22 18:01:12 | NULL        | 
|  2 | Java   | 12.34567890 |         0 | Computer | 2020-03-03 18:01:12 | NULL        | 
+----+--------+-------------+-----------+----------+---------------------+-------------+ 
2 rows in set (0.00 sec)                                                                 
1234567
```

显然，此时update_time字段的值为空，改变数据：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String, DECIMAL, Boolean, Enum, DateTime
from sqlalchemy.dialects.mysql import LONGTEXT
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base
from datetime import datetime, timedelta

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(DECIMAL(20, 8))
    is_delete = Column(Boolean)
    btype = Column(Enum('Science', 'Computer'))
    release_time = Column(DateTime)
    update_time = Column(DateTime, onupdate=datetime.now())


Session = sessionmaker(bind=engine)
session = Session()

book = session.query(Book).first()
print(book.name)
book.name = 'PHP'
print(book.name)
session.commit()
1234567891011121314151617181920212223242526272829303132333435363738
```

此时修改了数据，打印：

```python
Python
PHP
12
```

再查询数据库：

```sql
+----+------+-------------+-----------+----------+---------------------+---------------------+
| id | name | price       | is_delete | btype    | release_time        | update_time         |
+----+------+-------------+-----------+----------+---------------------+---------------------+
|  1 | PHP  | 12.34000000 |         1 | Science  | 2020-04-22 18:01:12 | 2020-04-22 18:05:13 |
|  2 | Java | 12.34567890 |         0 | Computer | 2020-03-03 18:01:12 | NULL                |
+----+------+-------------+-----------+----------+---------------------+---------------------+
2 rows in set (0.01 sec)
1234567
```

显然，此时第一条数据发生改变，同时update_time字段的值也为时间，不再为空。
除了上述用法，还可以直接给onupdate传递一个函数，在修改记录时会调用这个函数。

在定义模型类时，默认情况下定义的属性名称即为数据库表中的字段名，也可以通过**name参数**来指定在数据库表中对应的字段名，如下：

```python
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String, DECIMAL, Boolean, Enum, DateTime
from sqlalchemy.dialects.mysql import LONGTEXT
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base
from datetime import datetime, timedelta

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(DECIMAL(20, 8))
    is_delete = Column(Boolean)
    btype = Column(Enum('Science', 'Computer'))
    release_time = Column(DateTime)
    update_time = Column('Last_Updated', DateTime, onupdate=datetime.now())


# 删除表book以便重新创建
Base.metadata.drop_all()
Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()

book1 = Book(name='Python', price=12.34, is_delete=True, btype='Science', release_time=datetime.now())
book2 = Book(name='Java', price=12.3456789, is_delete=False, btype='Computer', release_time=datetime.now() - timedelta(days=50))
session.add_all([book1, book2])
session.commit()
12345678910111213141516171819202122232425262728293031323334353637383940
```

运行后查询数据库：

```sql
+----+--------+-------------+-----------+----------+---------------------+--------------+
| id | name   | price       | is_delete | btype    | release_time        | Last_Updated |
+----+--------+-------------+-----------+----------+---------------------+--------------+
|  1 | Python | 12.34000000 |         1 | Science  | 2020-04-22 18:13:12 | NULL         |
|  2 | Java   | 12.34567890 |         0 | Computer | 2020-03-03 18:13:12 | NULL         |
+----+--------+-------------+-----------+----------+---------------------+--------------+
2 rows in set (0.01 sec)
1234567
```

可以看到，update_time字段改成了Last_Updated字段。

### 2.query可用参数

#### 模型对象

指定查找这个模型中所有的对象。
测试如下：
创建表并插入数据：

```python
from datetime import datetime, timedelta
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from random import randint

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(Float)

    def __str__(self):
        return 'Book(id:{}, name:{}， price：{})'.format(self.id, self.name, self.price)


# 删除表book以便重新创建
Base.metadata.drop_all()
Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()

for i in range(10):
    book = Book(name='Name %d' % (i + 1), price=randint(1, 50))
    session.add(book)

session.commit()

1234567891011121314151617181920212223242526272829303132333435363738394041
```

运行后查询数据库得：

```sql
+----+---------+-------+ 
| id | name    | price | 
+----+---------+-------+ 
|  1 | Name 1  |    45 | 
|  2 | Name 2  |    49 | 
|  3 | Name 3  |    14 | 
|  4 | Name 4  |    16 | 
|  5 | Name 5  |    17 | 
|  6 | Name 6  |     3 | 
|  7 | Name 7  |    25 | 
|  8 | Name 8  |    16 | 
|  9 | Name 9  |    32 | 
| 10 | Name 10 |    22 | 
+----+---------+-------+ 
10 rows in set (0.01 sec)
123456789101112131415
```

打印模型对象需要在模型中实现`__str__()`方法，测试如下：

```python
from datetime import datetime, timedelta
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from random import randint

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(Float)

    def __str__(self):
        return 'Book(id:{}, name:{}， price：{})'.format(self.id, self.name, self.price)


# 删除表book以便重新创建
Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()

books = session.query(Book).all()
for book in books:
    print(book)

session.commit()

12345678910111213141516171819202122232425262728293031323334353637383940
```

打印：

```python
Book(id:1, name:Name 1， price：45.0)
Book(id:2, name:Name 2， price：49.0)
Book(id:3, name:Name 3， price：14.0)
Book(id:4, name:Name 4， price：16.0)
Book(id:5, name:Name 5， price：17.0)
Book(id:6, name:Name 6， price：3.0)
Book(id:7, name:Name 7， price：25.0)
Book(id:8, name:Name 8， price：16.0)
Book(id:9, name:Name 9， price：32.0)
Book(id:10, name:Name 10， price：22.0)
12345678910
```

#### 模型属性

可以指定只查找模型中的某几个属性。
打印对象属性测试如下：

```python
from datetime import datetime, timedelta
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from random import randint

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(Float)

    def __str__(self):
        return 'Book(id:{}, name:{}， price：{})'.format(self.id, self.name, self.price)


# 删除表book以便重新创建
# Base.metadata.drop_all()
Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()

# for i in range(10):
#     book = Book(name='Name %d' % (i + 1), price=randint(1, 50))
#     session.add(book)

books = session.query(Book).all()
for book in books:
    print(book.id, book.name, book.price)

session.commit()

123456789101112131415161718192021222324252627282930313233343536373839404142434445
```

打印：

```python
1 Name 1 45.0
2 Name 2 49.0
3 Name 3 14.0
4 Name 4 16.0
5 Name 5 17.0
6 Name 6 3.0
7 Name 7 25.0
8 Name 8 16.0
9 Name 9 32.0
10 Name 10 22.0
12345678910
```

#### 聚合函数

- func.count
  统计记录的数量。
- func.avg
  求平均值。
- func.max
  求最大值。
- func.min
  求最小值。
- func.sum
  求和。

count计数测试：

```python
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy import create_engine, func
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(Float)

    def __str__(self):
        return 'Book(id:{}, name:{}， price：{})'.format(self.id, self.name, self.price)


# 删除表book以便重新创建
Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()

result = session.query(func.count(Book.id)).first()
print(result)
12345678910111213141516171819202122232425262728293031323334
```

打印：

```python
(10,)
1
```

得到了记录条数。

avg平均值测试：

```python
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy import create_engine, func
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(Float)

    def __str__(self):
        return 'Book(id:{}, name:{}， price：{})'.format(self.id, self.name, self.price)


# 删除表book以便重新创建
Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()

result = session.query(func.avg(Book.price)).first()
print(result)
12345678910111213141516171819202122232425262728293031323334
```

打印：

```python
(23.9,)
1
```

max最大值和min最小值测试：

```python
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy import create_engine, func
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(Float)

    def __str__(self):
        return 'Book(id:{}, name:{}， price：{})'.format(self.id, self.name, self.price)


# 删除表book以便重新创建
Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()

result1 = session.query(func.max(Book.price)).first()
result2 = session.query(func.min(Book.price)).first()
print(result1, result2)
1234567891011121314151617181920212223242526272829303132333435
```

打印：

```python
(49.0,) (3.0,)
1
```

sum求和测试：

```python
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy import create_engine, func
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(Float)

    def __str__(self):
        return 'Book(id:{}, name:{}， price：{})'.format(self.id, self.name, self.price)


# 删除表book以便重新创建
Base.metadata.create_all()
Session = sessionmaker(bind=engine)
session = Session()

result = session.query(func.sum(Book.price)).first()
print(result)
12345678910111213141516171819202122232425262728293031323334
```

打印：

```python
(239.0,)
```

# Python全栈（七）Flask框架之8.ORM过滤条件、外键约束和表关系

## 一、Flask数据库过滤条件

过滤条件一般是通过`filter()`方法实现的，常见的过滤条件如下：

- equals

  ```python
  filter(Book.name == 'Name 3')
  1
  ```

- not equals

  ```python
  filter(Book.name != 'Name 3')
  1
  ```

- like

  ```python
  filter(Book.name.like("%Name%"))
  1
  ```

- in

  ```python
  filter(Book.name.in_(['Name 1', 'Name 2']))
  # in_的参数也可以是query的查询结果
  filter(Book.name.in_(session.query(Book.name).filter(Book.name.like('%Name%'))))
  123
  ```

- not in

  ```python
  filter(~Book.name.in_(['Name 1', 'Name 2']))
  1
  ```

- is null

  ```python
  filter(Book.name == None)
  # 或者是
  filter(Book.name.is_(None))
  123
  ```

- is not null

  ```python
  filter(User.name != None)
  # 或者是
  filter(User.name.isnot(None))
  123
  ```

- and

  ```python
  filter(Book.name == 'Name 1', Book.price <= 50).all()
  filter(and_(Book.name == 'Name 1', Book.price <= 50)).all()
  filter(Book.name == 'Name 1').filter(Book.price <= 50).all()
  123
  ```

- or

  ```python
  filter(or_(Book.name == 'Name 1', Book.price >= 50)).all()
  1
  ```

equals测试如下：

```python
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy import create_engine, func
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(Float)

    def __str__(self):
        return 'Book(id:{}, name:{}, price：{})'.format(self.id, self.name, self.price)


Session = sessionmaker(bind=engine)
session = Session()

results = session.query(Book).filter(Book.name == 'Name 3').all()
for result in results:
    print(result)

12345678910111213141516171819202122232425262728293031323334
```

打印：

```python
Book(id:3, name:Name 3, price：14.0)
1
```

not equals测试：

```python
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy import create_engine, func
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(Float)

    def __str__(self):
        return 'Book(id:{}, name:{}, price：{})'.format(self.id, self.name, self.price)


Session = sessionmaker(bind=engine)
session = Session()

results = session.query(Book).filter(Book.name != 'Name 3').all()
for result in results:
    print(result)

12345678910111213141516171819202122232425262728293031323334
```

打印：

```python
Book(id:1, name:Name 1, price：45.0)
Book(id:2, name:Name 2, price：49.0)
Book(id:4, name:Name 4, price：16.0)
Book(id:5, name:Name 5, price：17.0)
Book(id:6, name:Name 6, price：3.0)
Book(id:7, name:Name 7, price：25.0)
Book(id:8, name:Name 8, price：16.0)
Book(id:9, name:Name 9, price：32.0)
Book(id:10, name:Name 10, price：22.0)

12345678910
```

like一般用于**模糊查询**，测试如下：

```python
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy import create_engine, func
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(Float)

    def __str__(self):
        return 'Book(id:{}, name:{}, price：{})'.format(self.id, self.name, self.price)


Session = sessionmaker(bind=engine)
session = Session()

results = session.query(Book).filter(Book.name.like('%me%')).all()
for result in results:
    print(result)

12345678910111213141516171819202122232425262728293031323334
```

打印：

```python
Book(id:1, name:Name 1, price：45.0)
Book(id:2, name:Name 2, price：49.0)
Book(id:3, name:Name 3, price：14.0)
Book(id:4, name:Name 4, price：16.0)
Book(id:5, name:Name 5, price：17.0)
Book(id:6, name:Name 6, price：3.0)
Book(id:7, name:Name 7, price：25.0)
Book(id:8, name:Name 8, price：16.0)
Book(id:9, name:Name 9, price：32.0)
Book(id:10, name:Name 10, price：22.0)

1234567891011
```

其中，%表示匹配表示**零个或者多个任意字符**，只要name字段中含有me子字符串，就会被查询到；
还可以使用下划线_，表示**一个任意字符**（必须有一个字符）。

in测试：

```python
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy import create_engine, func
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(Float)

    def __str__(self):
        return 'Book(id:{}, name:{}, price：{})'.format(self.id, self.name, self.price)


Session = sessionmaker(bind=engine)
session = Session()

results = session.query(Book).filter(Book.name.in_(['Name 2', 'Name 5'])).all()
for result in results:
    print(result)

12345678910111213141516171819202122232425262728293031323334
```

打印：

```python
Book(id:2, name:Name 2, price：49.0)
Book(id:5, name:Name 5, price：17.0)
12
```

为了避免和Python中的关键字in冲突，ORM将in后面添加一个下划线；
除了列表，`in_()`中也可以传入元组，但是建议使用列表。

not in两种方式测试：

```python
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy import create_engine, func
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    price = Column(Float)

    def __str__(self):
        return 'Book(id:{}, name:{}, price：{})'.format(self.id, self.name, self.price)


Session = sessionmaker(bind=engine)
session = Session()

results1 = session.query(Book).filter(~Book.name.in_(['Name 2', 'Name 5'])).all()
print('~ test:')
for result in results1:
    print(result)
results2 = session.query(Book).filter(Book.name.notin_(['Name 2', 'Name 5'])).all()
print('Notin test:')
for result in results2:
    print(result)

123456789101112131415161718192021222324252627282930313233343536373839
```

打印：

```python
~ test:
Book(id:1, name:Name 1, price：45.0)
Book(id:3, name:Name 3, price：14.0)
Book(id:4, name:Name 4, price：16.0)
Book(id:6, name:Name 6, price：3.0)
Book(id:7, name:Name 7, price：25.0)
Book(id:8, name:Name 8, price：16.0)
Book(id:9, name:Name 9, price：32.0)
Book(id:10, name:Name 10, price：22.0)
Notin test:
Book(id:1, name:Name 1, price：45.0)
Book(id:3, name:Name 3, price：14.0)
Book(id:4, name:Name 4, price：16.0)
Book(id:6, name:Name 6, price：3.0)
Book(id:7, name:Name 7, price：25.0)
Book(id:8, name:Name 8, price：16.0)
Book(id:9, name:Name 9, price：32.0)
Book(id:10, name:Name 10, price：22.0)

12345678910111213141516171819
```

显然，not in两种实现方式（ **~** 取反和`notin_()`方法）的查询结果是一样的，但是使用`notin_()`方法的可读性更高。

null和not null测试：
修改book表name字段允许为null之后手动插入一条name字段为空的数据，再测试：

```python
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy import create_engine, func
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50))
    price = Column(Float)

    def __str__(self):
        return 'Book(id:{}, name:{}, price：{})'.format(self.id, self.name, self.price)


Session = sessionmaker(bind=engine)
session = Session()

results1 = session.query(Book).filter(Book.name == None).all()
print('Null Test: ')
for result in results1:
    print(result)
results2 = session.query(Book).filter(Book.name != None).all()
print('Not null Test: ')
for result in results2:
    print(result)

123456789101112131415161718192021222324252627282930313233343536373839
```

打印：

```python
Null Test: 
Book(id:11, name:None， price：33.0)
Not null Test: 
Book(id:1, name:Name 1, price：45.0)
Book(id:2, name:Name 2, price：49.0)
Book(id:3, name:Name 3, price：14.0)
Book(id:4, name:Name 4, price：16.0)
Book(id:5, name:Name 5, price：17.0)
Book(id:6, name:Name 6, price：3.0)
Book(id:7, name:Name 7, price：25.0)
Book(id:8, name:Name 8, price：16.0)
Book(id:9, name:Name 9, price：32.0)
Book(id:10, name:Name 10, price：22.0)
12345678910111213
```

还可以通过`is_()`和`isnot()`方法实现：

```python
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy import create_engine, func
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50))
    price = Column(Float)

    def __str__(self):
        return 'Book(id:{}, name:{}, price：{})'.format(self.id, self.name, self.price)


Session = sessionmaker(bind=engine)
session = Session()

results1 = session.query(Book).filter(Book.name.is_(None)).all()
print('Null Test: ')
for result in results1:
    print(result)
results2 = session.query(Book).filter(Book.name.isnot(None)).all()
print('NNot null Test: ')
for result in results2:
    print(result)

123456789101112131415161718192021222324252627282930313233343536373839
```

and测试：

```python
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy import create_engine, and_
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50))
    price = Column(Float)

    def __str__(self):
        return 'Book(id:{}, name:{}, price：{})'.format(self.id, self.name, self.price)


Session = sessionmaker(bind=engine)
session = Session()

results1 = session.query(Book).filter(Book.name == 'Name 1', Book.price <= 50).all()
print('And Test 1: ')
for result in results1:
    print(result)
results2 = session.query(Book).filter(and_(Book.name == 'Name 1', Book.price <= 50)).all()
print('And Test 2: ')
for result in results2:
    print(result)
results3 = session.query(Book).filter(Book.name == 'Name 1').filter(Book.price <= 50).all()
print('And Test 3: ')
for result in results3:
    print(result)

12345678910111213141516171819202122232425262728293031323334353637383940414243
```

打印：

```python
And Test 1: 
Book(id:1, name:Name 1, price：45.0)
And Test 2: 
Book(id:1, name:Name 1, price：45.0)
And Test 3: 
Book(id:1, name:Name 1, price：45.0)

1234567
```

and可以通过3种方式实现，显然，第1种方法更简单易用。

or测试：

```python
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy import create_engine, or_
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_demo'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Book(Base):
    __tablename__ = 'book'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50))
    price = Column(Float)

    def __str__(self):
        return 'Book(id:{}, name:{}, price：{})'.format(self.id, self.name, self.price)


Session = sessionmaker(bind=engine)
session = Session()

results = session.query(Book).filter(or_(Book.name == 'Name 1', Book.price >= 30)).all()
for result in results:
    print(result)

12345678910111213141516171819202122232425262728293031323334
```

打印：

```python
Book(id:1, name:Name 1, price：45.0)
Book(id:2, name:Name 2, price：49.0)
Book(id:9, name:Name 9, price：32.0)
Book(id:11, name:None, price：33.0)

12345
```

`or_()`方法中除了放多个过滤条件，还可以只放一个过滤条件。

## 二、ORM模型的外键约束

### 1.ORM建立外键关系

在MySQL中，外键可以让表之间的关系更加紧密，SQLAlchemy也支持外键，通过**ForeignKey类**来实现，可以指定表的外键约束。

创建两个存在外键关系的表示例如下：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(30))


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50))
    content = Column(Text, nullable=False)
    uid = Column(Integer, ForeignKey('user.id'))


Base.metadata.drop_all()
Base.metadata.create_all()

12345678910111213141516171819202122232425262728293031323334
```

运行成功即创建表。

如果查看数据库表，未发现外键，可能是因为MySQL默认存储引擎为MyISAM，不支持外键，需要改成InnoDB，在MySQL配置文件种修改示例如下：
![flask orm MySQL change engine](https://img-blog.csdnimg.cn/20200425111539129.gif#pic_center)

要注意，修改之后，需要**重启MySQL服务**。

再添加数据：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(30))


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50))
    content = Column(Text, nullable=False)
    uid = Column(Integer, ForeignKey('user.id'))


# Base.metadata.create_all()

session = sessionmaker(bind=engine)()

user = User(username='Corley')
session.add(user)
article = Article(title='Python', content='人生苦短，我用Python', uid=1)
session.add(article)
session.commit()
12345678910111213141516171819202122232425262728293031323334353637383940
```

查看数据库中，可以看到user和article表中都已经有插入的数据了。
因为添加了外键，此时Article的uid参数必须是User中已经有的id，如果插入Article的数据的uid在User中不存在对应的id，就会报错、不能插入。

### 2.ORM数据库外键约束

外键约束有4种：

- RESTRICT
  严格模式，若子表中有父表对应的关联数据，删除父表对应数据，会阻止删除，也是**默认项**。
- NO ACTION
  在MySQL中，同RESTRICT。
- CASCADE
  级联模式，父表操作后，对应子表关联的数据也进行操作。
- SET NULL
  置空模式，父表被操作之后，子表对应的外键字段被置空。

restrict测试：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(30))


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50))
    content = Column(Text, nullable=False)
    uid = Column(Integer, ForeignKey('user.id', ondelete='RESTRICT'))


Base.metadata.drop_all()
Base.metadata.create_all()

session = sessionmaker(bind=engine)()

user = User(username='Corley')
session.add(user)
article = Article(title='Python', content='人生苦短，我用Python', uid=1)
session.add(article)
session.commit()
1234567891011121314151617181920212223242526272829303132333435363738394041
```

运行后，删除user表数据：

```sql
delete from user where id = 1;
1
```

打印：

```sql
ERROR 1451 (23000): Cannot delete or update a parent row: a foreign key constraint fails (`flask_orm`.`article`, CONSTRAINT `article_ibfk_1` FOREIGN KEY (`uid`) REFERENCES `user` (`id`))
1
```

显然，提示有外键约束，不能删除数据。

通过ORM删除数据库表测试：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(30))


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50))
    content = Column(Text, nullable=False)
    uid = Column(Integer, ForeignKey('user.id', ondelete='RESTRICT'))


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

user = User(username='Corley')
session.add(user)
session.commit()
article = Article(title='Python', content='人生苦短，我用Python', uid=1)
session.add(article)
session.commit()

user = session.query(User).filter(User.username == 'Corley').all()[0]
session.delete(user)
session.commit()
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647
```

打印：

```python
Traceback (most recent call last):
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\mysql\connector\connection_cext.py", line 489, in cmd_query
    raw_as_string=raw_as_string)
_mysql_connector.MySQLInterfaceError: Cannot delete or update a parent row: a foreign key constraint fails (`flask_orm`.`article`, CONSTRAINT `article_ibfk_1` FOREIGN KEY (`uid`) REFERENCES `user` (`id`))

During handling of the above exception, another exception occurred:


......


    raw_as_string=self._raw_as_string)
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\mysql\connector\connection_cext.py", line 492, in cmd_query
    sqlstate=exc.sqlstate)
sqlalchemy.exc.IntegrityError: (mysql.connector.errors.IntegrityError) 1451 (23000): Cannot delete or update a parent row: a foreign key constraint fails (`flask_orm`.`article`, CONSTRAINT `article_ibfk_1` FOREIGN KEY (`uid`) REFERENCES `user` (`id`))
[SQL: DELETE FROM user WHERE user.id = %(id)s]
[parameters: {'id': 1}]
(Background on this error at: http://sqlalche.me/e/gkpj)

12345678910111213141516171819
```

显然，此时报错，不允许删除有外键约束的数据。

cascade测试：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(30))


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50))
    content = Column(Text, nullable=False)
    uid = Column(Integer, ForeignKey('user.id', ondelete='CASCADE'))


Base.metadata.drop_all()
Base.metadata.create_all()

session = sessionmaker(bind=engine)()

user = User(username='Corley')
session.add(user)
article = Article(title='Python', content='人生苦短，我用Python', uid=1)
session.add(article)
session.commit()
1234567891011121314151617181920212223242526272829303132333435363738394041
```

运行后，再删除user表数据：

```sql
delete from user where id = 1;
1
```

打印：

```sql
Query OK, 1 row affected (0.01 sec)
1
```

显然，此时删除成功，再查询user表：

```sql
select * from user;
1
```

打印：

```sql
Empty set (0.00 sec)
1
```

再查询article表：

```sql
select * from article;
1
```

打印：

```sql
Empty set (0.00 sec)
1
```

显然，此时删除user表中数据，article表的数据也被同步删除。

用ORM删除数据测试：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(30))


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50))
    content = Column(Text, nullable=False)
    uid = Column(Integer, ForeignKey('user.id', ondelete='CASCADE'))


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

user = User(username='Corley')
session.add(user)
session.commit()
article = Article(title='Python', content='人生苦短，我用Python', uid=1)
session.add(article)
session.commit()

user = session.query(User).filter(User.username == 'Corley').all()[0]
session.delete(user)
session.commit()
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647
```

运行后查询数据库与之前结果一致。

set null测试：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(30))


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50))
    content = Column(Text, nullable=False)
    uid = Column(Integer, ForeignKey('user.id', ondelete='SET NULL'))


Base.metadata.drop_all()
Base.metadata.create_all()

session = sessionmaker(bind=engine)()

user = User(username='Corley')
session.add(user)
article = Article(title='Python', content='人生苦短，我用Python', uid=1)
session.add(article)
session.commit()
1234567891011121314151617181920212223242526272829303132333435363738394041
```

运行后，删除user表数据：

```sql
delete from user where id = 1;
1
```

打印：

```sql
Query OK, 1 row affected (0.01 sec)
1
```

显示删除成功，此时查询user表：

```sql
select * from user;
1
```

打印：

```sql
Empty set (0.00 sec)
1
```

再查询article表：

```sql
select * from article;
1
```

打印：

```sql
+----+--------+-----------------------------+------+
| id | title  | content                     | uid  |
+----+--------+-----------------------------+------+
|  1 | Python | 人生苦短，我用Python         | NULL |
+----+--------+-----------------------------+------+
1 row in set (0.00 sec)

1234567
```

显然，此时当删除user表中的数据，article表中与其关联的数据被设为null。

用ORM删除数据测试：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(30))


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50))
    content = Column(Text, nullable=False)
    uid = Column(Integer, ForeignKey('user.id', ondelete='SET NULL'))


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

user = User(username='Corley')
session.add(user)
session.commit()
article = Article(title='Python', content='人生苦短，我用Python', uid=1)
session.add(article)
session.commit()

user = session.query(User).filter(User.username == 'Corley').all()[0]
session.delete(user)
session.commit()
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647
```

运行后查询数据库与之前一致。

### 3.存在外键时数据的查询

查询数据示例如下：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(30))

    def __str__(self):
        return 'User(id: {}, username: {})'.format(self.id, self.username)


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50))
    content = Column(Text, nullable=False)
    uid = Column(Integer, ForeignKey('user.id', ondelete='SET NULL'))


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

user = User(username='Corley')
session.add(user)
session.commit()
article = Article(title='Python', content='人生苦短，我用Python', uid=1)
session.add(article)
session.commit()

article = session.query(Article).first()
uid = article.uid
user = session.query(User).get(uid)
print(user)
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051
```

打印：

```python
User(id: 1, username: Corley)

12
```

显然，这种方式先通过Article找到用户的uid，再通过uid查找用户，显得很麻烦，可以简化：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(30))

    def __str__(self):
        return 'User(id: {}, username: {})'.format(self.id, self.username)


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50))
    content = Column(Text, nullable=False)
    uid = Column(Integer, ForeignKey('user.id', ondelete='SET NULL'))


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

user = User(username='Corley')
session.add(user)
session.commit()
article = Article(title='Python', content='人生苦短，我用Python', uid=1)
session.add(article)
session.commit()

user = session.query(User).filter(User.id == Article.uid).first()
print(user)

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
```

查询结果是一样的。

## 三、Flask数据库表关系

表之间的关系有三种：

- 一对一
- 一对多
- 多对多

SQLAlchemy中的ORM可以实现这三种关系。

### 1.一对多关系

例如：
一个人可以有多辆车，但是一辆车只能对应一个人。

测试如下：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(30), nullable=False)

    articles = relationship("Article")

    def __str__(self):
        return 'User(id: {}, username: {})'.format(self.id, self.username)


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50))
    content = Column(Text, nullable=False)
    uid = Column(Integer, ForeignKey('user.id', ondelete='SET NULL'))

    author = relationship('User')

    def __str__(self):
        return 'User(id: {}, title: {}, content: {}, uid: {})'.format(self.id, self.title, self.content, self.uid)


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

user = User(username='Corley')
session.add(user)
session.commit()
article = Article(title='Python', content='人生苦短，我用Python', uid=1)
session.add(article)
session.commit()

article = session.query(Article).first()
print(article)
print(article.author)

user = session.query(User).first()
print(user)
for article in user.articles:
    print(article)

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263
```

打印：

```python
User(id: 1, title: Python, content: 人生苦短，我用Python, uid: 1)
User(id: 1, username: Corley)
User(id: 1, username: Corley)
User(id: 1, title: Python, content: 人生苦短，我用Python, uid: 1)

12345
```

显然，此时根据文章查询作者和根据作者查询文章都能正常查询，因为作者和文章时一对多的关系，所以通过作者查询文章得到的是一个列表，可以遍历列表来获取每一篇文章；
同时，表之间的关系是建立在两个表之间**存在外键约束**的前提下的，如果未建立外键约束，但在两个表之间建立一对多的关系，会出错。

联合表添加单条数据测试：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(30), nullable=False)

    articles = relationship("Article")

    def __str__(self):
        return 'User(id: {}, username: {})'.format(self.id, self.username)


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50))
    content = Column(Text, nullable=False)
    uid = Column(Integer, ForeignKey('user.id', ondelete='SET NULL'))

    author = relationship('User')

    def __str__(self):
        return 'User(id: {}, title: {}, content: {}, uid: {})'.format(self.id, self.title, self.content, self.uid)


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

user = User(username='Corley')
session.add(user)
session.commit()
article = Article(title='Python', content='人生苦短，我用Python', uid=1)
session.add(article)
session.commit()

user = User(username='Lord')
article = Article(title='Java', content='Spring')

article.author = user
session.add(article)
session.commit()
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960
```

运行后，查询user表：

```sql
select * from user;
1
```

打印：

```sql
+----+----------+       
| id | username |       
+----+----------+       
|  1 | Corley   |       
|  2 | Lord     |       
+----+----------+       
2 rows in set (0.01 sec)
                        
12345678
```

再查询article表：

```sql
select * from article;
1
```

打印：

```sql
+----+--------+----------------------+------+
| id | title  | content              | uid  |
+----+--------+----------------------+------+
|  1 | Python | 人生苦短，我用Python  |    1 |
|  2 | Java   | Spring               |    2 |
+----+--------+----------------------+------+
2 rows in set (0.00 sec)
1234567
```

显然，两条数据都插入成功，并形成映射关系。

添加多条数据测试：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(30), nullable=False)

    articles = relationship("Article")

    def __str__(self):
        return 'User(id: {}, username: {})'.format(self.id, self.username)


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50))
    content = Column(Text, nullable=False)
    uid = Column(Integer, ForeignKey('user.id', ondelete='SET NULL'))

    author = relationship('User')

    def __str__(self):
        return 'User(id: {}, title: {}, content: {}, uid: {})'.format(self.id, self.title, self.content, self.uid)


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

user = User(username='Corley')
session.add(user)
session.commit()
article = Article(title='Python', content='人生苦短，我用Python', uid=1)
session.add(article)
session.commit()

user = User(username='Jessica')
article1 = Article(title='Java', content='Spring')
article2 = Article(title='C', content='Clang')

article1.author = user
article2.author = user
session.add_all([article1, article2])
session.commit()

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263
```

运行后，查询user表：

```sql
select * from user;
1
```

打印：

```sql
+----+----------+
| id | username |
+----+----------+
|  1 | Corley   |
|  2 | Jessica  |
+----+----------+
2 rows in set (0.00 sec)
                        
12345678
```

再查询article表：

```sql
select * from article;
1
```

打印：

```sql
+----+--------+----------------------+------+
| id | title  | content              | uid  |
+----+--------+----------------------+------+
|  1 | Python | 人生苦短，我用Python  |    1 |
|  2 | Java   | Spring               |    2 |
|  3 | C      | Clang                |    2 |
+----+--------+----------------------+------+
3 rows in set (0.00 sec)

123456789
```

显然，此时也通过一对多的映射将一个user和对应的两个article插入数据库。

之前都是通过在User和Article模型中都定义关系映射来实现一对多的关系，但是也可以只在一个模型中定义relationship来实现一对多的关系，此时需要定义**反向访问属性backref**，示例如下：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(30), nullable=False)

    articles = relationship("Article", backref='author')        # 定义反向访问属性

    def __str__(self):
        return 'User(id: {}, username: {})'.format(self.id, self.username)


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50))
    content = Column(Text, nullable=False)
    uid = Column(Integer, ForeignKey('user.id', ondelete='SET NULL'))

    # author = relationship('User')

    def __str__(self):
        return 'User(id: {}, title: {}, content: {}, uid: {})'.format(self.id, self.title, self.content, self.uid)


# Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

article = session.query(Article).first()
print(article)
print(article.author)

user = session.query(User).first()
print(user)
for article in user.articles:
    print(article)

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556
```

打印：

```python
User(id: 1, title: Python, content: 人生苦短，我用Python, uid: 1)
User(id: 1, username: Corley)
User(id: 1, username: Corley)
User(id: 1, title: Python, content: 人生苦短，我用Python, uid: 1)
User(id: 4, title: Python, content: 人生苦短，我用Python, uid: 1)

123456
```

显然，此时可以互相查询；
在Article模型中未定义关系，只在User模型中定义了关系，同时在定义关系时定义了backref反向访问属性，来保证不仅能通过User来查询Article，还要能通过Article来查询User。

### 2.一对一关系

在sqlalchemy中，如果想要将两个模型映射成一对一的关系，那么应该在父模型中指定引用的时候，传递一个`uselist=False`参数，告诉父模型，以后引用这个子模型的时候，不再是一个列表，而是一个**对象**了。

当不指定uselist参数时，测试如下：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship, backref

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(50), nullable=False)
    extend = relationship("UserExtend")


class UserExtend(Base):
    __tablename__ = 'user_extend'
    id = Column(Integer, primary_key=True, autoincrement=True)
    school = Column(String(50))
    uid = Column(Integer, ForeignKey("user.id"))
    user = relationship("User")


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

user1 = User(username="Corley")
extend1 = UserExtend(school="CUFE")
print(type(user1.extend))

123456789101112131415161718192021222324252627282930313233343536373839404142
```

打印：

```python
<class 'sqlalchemy.orm.collections.InstrumentedList'>
1
```

查看源码可以知道，**InstrumentedList**是继承自list的，此时如果要指定User对象和UserExtend对象的映射关系，需要使用列表，如下：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship, backref

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(50), nullable=False)
    extend = relationship("UserExtend")


class UserExtend(Base):
    __tablename__ = 'user_extend'
    id = Column(Integer, primary_key=True, autoincrement=True)
    school = Column(String(50))
    uid = Column(Integer, ForeignKey("user.id"))
    user = relationship("User")


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

user1 = User(username="Corley")
extend1 = UserExtend(school="CUFE")
extend2 = UserExtend(school="RUC")
user1.extend = [extend1]
session.add(user1)
session.commit()

123456789101112131415161718192021222324252627282930313233343536373839404142434445
```

运行后，查询数据库，可以看到数据已经插入，因为是列表，显然此时可以插入多个数据，即一个User可以通过在列表中放多个UserExtend对象来指定多个UserExtend，没有达到一对一的目的，此时需要用到**uselist属性**，如下：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship, backref

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(50), nullable=False)
    extend = relationship("UserExtend", uselist=False)


class UserExtend(Base):
    __tablename__ = 'user_extend'
    id = Column(Integer, primary_key=True, autoincrement=True)
    school = Column(String(50))
    uid = Column(Integer, ForeignKey("user.id"))
    user = relationship("User")


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

user1 = User(username="Corley")
extend1 = UserExtend(school="CUFE")
user1.extend = extend1
session.add(user1)


user2 = User(username="Tony")
extend2 = UserExtend(school="RUC")
extend2.user = user2
session.add(extend2)
session.commit()

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
```

运行之后，查询数据库user表：

```sql
select * from user;
1
```

打印：

```sql
+----+----------+        
| id | username |        
+----+----------+        
|  1 | Corley   |        
|  2 | Tony     |        
+----+----------+        
2 rows in set (0.01 sec) 
                         
12345678
```

查询user_extend表：

```sql
select * from user_extend;
1
```

打印：

```sql
+----+--------+------+
| id | school | uid  |
+----+--------+------+
|  1 | CUFE   |    1 |
|  2 | RUC    |    2 |
+----+--------+------+
2 rows in set (0.00 sec)

12345678
```

显然，此时两个表的两条数据分别一一对应。
除了上面的方式，还可以如下：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship, backref

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(50), nullable=False)


class UserExtend(Base):
    __tablename__ = 'user_extend'
    id = Column(Integer, primary_key=True, autoincrement=True)
    school = Column(String(50))
    uid = Column(Integer, ForeignKey("user.id"))
    user = relationship("User", backref=backref("extend", uselist=False))


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

user1 = User(username="Corley")
extend1 = UserExtend(school="CUFE")
user1.extend = extend1
session.add(user1)


user2 = User(username="Tony")
extend2 = UserExtend(school="RUC")
extend2.user = user2
session.add(extend2)
session.commit()

12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849
```

效果是一样的。

### 3.多对多关系

MySQL中，多对多关系需要一个中间表来作为连接，同理在sqlalchemy中的orm也需要一个中间表。
例如，现在有一个Teacher表和一个Classes表，即老师和班级，一个老师可以教多个班级，一个班级有多个老师，是一种典型的多对多的关系。

创建teacher、classes表和中间表测试：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey, Table
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship, backref

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)

teacher_classes = Table(
    'table_classes',
    Base.metadata,
    Column('teacher_id', Integer, ForeignKey('teacher.id')),
    Column('classes_id', Integer, ForeignKey('classes.id')),
)


class Teacher(Base):
    __tablename__ = 'teacher'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    classes = relationship('Classes', backref='teachers', secondary=teacher_classes)


class Classes(Base):
    __tablename__ = 'classes'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

12345678910111213141516171819202122232425262728293031323334353637383940414243
```

运行后查看数据库可以看到，teacher、classes、teacher_classes表已经创建成功。
通过**Table类**来创建中间表，可以看到，初始化时的第1个参数teacher_classes代表的是中间表的表名，第2个参数是Base的元数据，第3个和第4个参数就是要连接的两个表，其中Column第1个参数是连接表的外键名，第2个参数是这个外键的类型，第3个参数是要连接的两个表的表名和字段。

插入数据测试：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey, Table
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship, backref

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)

teacher_classes = Table(
    'teacher_classes',
    Base.metadata,
    Column('teacher_id', Integer, ForeignKey('teacher.id')),
    Column('classes_id', Integer, ForeignKey('classes.id')),
)


class Teacher(Base):
    __tablename__ = 'teacher'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    classes = relationship('Classes', backref='teachers', secondary=teacher_classes)


class Classes(Base):
    __tablename__ = 'classes'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

teacher1 = Teacher(name='Corley')
teacher2 = Teacher(name='Tony')

class1 = Classes(name='Class 1')
class2 = Classes(name='Class 2')

teacher1.classes.append(class1)
teacher1.classes.append(class2)
teacher2.classes.append(class1)
teacher2.classes.append(class2)

session.add_all([teacher1, teacher2])
session.commit()
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556
```

运行后，查询表classes：

```sql
select * from classes;
1
```

打印：

```sql
+----+---------+           
| id | name    |           
+----+---------+           
|  1 | Class 1 |           
|  2 | Class 2 |           
+----+---------+           
2 rows in set (0.01 sec)   
                           
12345678
```

查询teacher表：

```sql
select * from teacher;
1
```

打印：

```sql
+----+--------+
| id | name   |
+----+--------+
|  1 | Corley |
|  2 | Tony   |
+----+--------+
2 rows in set (0.00 sec)
                           
12345678
```

查询teacher_classes表：

```sql
select * from teacher_classes;
1
```

打印：

```sql
+------------+------------+
| teacher_id | classes_id |
+------------+------------+
|          1 |          1 |
|          2 |          1 |
|          1 |          2 |
|          2 |          2 |
+------------+------------+
4 rows in set (0.00 sec)
                           
12345678910
```

显然，数据都已插入成功。

用ORM查询数据如下：

```python
from sqlalchemy import Column, Integer, String, Text, ForeignKey, Table
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, relationship, backref

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)

teacher_classes = Table(
    'teacher_classes',
    Base.metadata,
    Column('teacher_id', Integer, ForeignKey('teacher.id')),
    Column('classes_id', Integer, ForeignKey('classes.id')),
)


class Teacher(Base):
    __tablename__ = 'teacher'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    classes = relationship('Classes', backref='teachers', secondary=teacher_classes)

    def __str__(self):
        return 'User(id: {}, name: {}, classes: {})'.format(self.id, self.name, [str(c) for c in self.classes])


class Classes(Base):
    __tablename__ = 'classes'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    def __str__(self):
        return 'Class(id: {}, name: {})'.format(self.id, self.name)


# Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

print('Query Teacher:')
teacher = session.query(Teacher).first()
print(teacher)
for c in teacher.classes:
    print(c)
    
print('Query Class:')
classes = session.query(Classes).get(2)
print(classes)
for teacher in classes.teachers:
    print(teacher)
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859
```

打印：

```python
Query Teacher:
User(id: 1, name: Corley, classes: ['Class(id: 1, name: Class 1)', 'Class(id: 2, name: Class 2)'])
Class(id: 1, name: Class 1)
Class(id: 2, name: Class 2)
Query Class:
Class(id: 2, name: Class 2)
User(id: 1, name: Corley, classes: ['Class(id: 1, name: Class 1)', 'Class(id: 2, name: Class 2)'])
User(id: 2, name: Tony, classes: ['Class(id: 1, name: Class 1)', 'Class(id: 2, name: Class 2)'])
```

# Python全栈（七）Flask框架之9.ORM排序、分页、高级查询和子查询

## 一、数据库排序

在ORM中排序的方式有2种：

- `order_by()`
  在查询结果时，使用`order_by()`方法，可以指定根据表中的某个字段进行排序。
- 模型定义时声明
  有时候，不想每次在查询的时候都指定排序的方式，可以在**定义模型的时候就指定默认排序**的方式。

**默认情况下是升序**，还可以指定按降序方式输出结果。

生成数据：

```python
from sqlalchemy import Column, Integer, String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from random import randint

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    length = Column(Integer)

    def __str__(self):
        return 'User(id: {}, name: {}, length: {})'.format(self.id, self.title, self.length)


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

for i in range(10):
    article = Article(title='Title %d' % i, length=randint(100, 10000))
    session.add(article)

session.commit()
123456789101112131415161718192021222324252627282930313233343536373839
```

运行后即插入数据。

此时用`order_by()`方法对查询结果排序，测试如下：

```python
from sqlalchemy import Column, Integer, String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from random import randint

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    length = Column(Integer)

    def __str__(self):
        return 'User(id: {}, name: {}, length: {})'.format(self.id, self.title, self.length)


# Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

articles = session.query(Article).order_by(Article.length).all()
for article in articles:
    print(article)
12345678910111213141516171819202122232425262728293031323334353637
```

打印：

```python
User(id: 4, name: Title 3, length: 143)
User(id: 6, name: Title 5, length: 1079)
User(id: 3, name: Title 2, length: 1323)
User(id: 7, name: Title 6, length: 1589)
User(id: 1, name: Title 0, length: 1604)
User(id: 2, name: Title 1, length: 2187)
User(id: 8, name: Title 7, length: 2933)
User(id: 10, name: Title 9, length: 3556)
User(id: 5, name: Title 4, length: 3829)
User(id: 9, name: Title 8, length: 8520)

1234567891011
```

显然，是按length属性从小到大排列的，也可以得到`order_by()`方法默认是升序排序。

如果想要**降序排列**，可以采用如下两种方式：

- 使用`desc()`方法

```python
order_by(Article.length.desc()).all()
1
```

- 字段前面加符号-

```python
order_by(-Article.length).all()
1
```

其一测试如下：

```python
from sqlalchemy import Column, Integer, String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from random import randint

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    length = Column(Integer)

    def __str__(self):
        return 'User(id: {}, name: {}, length: {})'.format(self.id, self.title, self.length)


# Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

articles = session.query(Article).order_by(Article.length.desc()).all()
for article in articles:
    print(article)
12345678910111213141516171819202122232425262728293031323334353637
```

打印：

```python
User(id: 9, name: Title 8, length: 8520)
User(id: 5, name: Title 4, length: 3829)
User(id: 10, name: Title 9, length: 3556)
User(id: 8, name: Title 7, length: 2933)
User(id: 2, name: Title 1, length: 2187)
User(id: 1, name: Title 0, length: 1604)
User(id: 7, name: Title 6, length: 1589)
User(id: 3, name: Title 2, length: 1323)
User(id: 6, name: Title 5, length: 1079)
User(id: 4, name: Title 3, length: 143)
12345678910
```

这是通过在查询结果时使用`order_by()`方法来实现的，如果要想在查询时就默认以某一种方式排序，可以在建立模型时定义，如下：

```python
from sqlalchemy import Column, Integer, String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from random import randint

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    length = Column(Integer)

    def __str__(self):
        return 'User(id: {}, name: {}, length: {})'.format(self.id, self.title, self.length)

    __mapper_args__ = {
        'order_by':length
    }


# Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

articles = session.query(Article).all()
for article in articles:
    print(article)
1234567891011121314151617181920212223242526272829303132333435363738394041
```

输出与之前默认升序一样的结果，显然此时不再需要在查询时使用`order_by()`方法，还可以降序，如下：

```python
__mapper_args__ = {
	'order_by':length.desc()
}
123
```

或

```python
__mapper_args__ = {
	'order_by':-length
}
123
```

显然，在定义模型时就定义默认排序更好，因为如果在查询时再定义排序，会降低查询效率。

## 二、limit、offset和切片

- limit
  指定返回的最大记录数目。
- offset
  指定第一个返回记录行的偏移量。
- 切片
  对Query对象使用切片操作，来获取想要的数据。

limit测试：

```python
from sqlalchemy import Column, Integer, String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from random import randint

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    length = Column(Integer)

    def __str__(self):
        return 'User(id: {}, name: {}, length: {})'.format(self.id, self.title, self.length)

    __mapper_args__ = {
        'order_by':length
    }


# Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()


articles = session.query(Article).limit(3).all()
for article in articles:
    print(article)
123456789101112131415161718192021222324252627282930313233343536373839404142
```

打印：

```python
User(id: 9, name: Title 8, length: 1895)
User(id: 1, name: Title 0, length: 2038)
User(id: 7, name: Title 6, length: 2456)

1234
```

显然，此时只查询了前3条数据。

offset测试：

```python
from sqlalchemy import Column, Integer, String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from random import randint

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    length = Column(Integer)

    def __str__(self):
        return 'User(id: {}, name: {}, length: {})'.format(self.id, self.title, self.length)

    __mapper_args__ = {
        'order_by':length
    }


# Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()


articles = session.query(Article).offset(3).limit(3).all()
for article in articles:
    print(article)
123456789101112131415161718192021222324252627282930313233343536373839404142
```

打印：

```python
User(id: 3, name: Title 2, length: 4884)
User(id: 6, name: Title 5, length: 5197)
User(id: 8, name: Title 7, length: 8874)

1234
```

上面查询了从第4条开始的连续3条数据；
显然，offset的参数是**从0开始计数**的。

还可以多个查询条件同时使用：

```python
from sqlalchemy import Column, Integer, String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from random import randint

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    length = Column(Integer)

    def __str__(self):
        return 'User(id: {}, name: {}, length: {})'.format(self.id, self.title, self.length)


# Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()


articles = session.query(Article).order_by(Article.length.desc()).offset(3).limit(3).all()
for article in articles:
    print(article)
1234567891011121314151617181920212223242526272829303132333435363738
```

打印：

```python
User(id: 5, name: Title 4, length: 8897)
User(id: 8, name: Title 7, length: 8874)
User(id: 6, name: Title 5, length: 5197)

1234
```

只能先用`order_by()`排序，再用`limit()`和`offset()`取数据，而不能颠倒两者的顺序。

切片测试如下：

```python
from sqlalchemy import Column, Integer, String
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from random import randint

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True, autoincrement=True)
    title = Column(String(50), nullable=False)
    length = Column(Integer)

    def __str__(self):
        return 'User(id: {}, name: {}, length: {})'.format(self.id, self.title, self.length)


# Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()


articles = session.query(Article).all()[2:5]
for article in articles:
    print(article)
1234567891011121314151617181920212223242526272829303132333435363738
```

打印：

```python
User(id: 3, name: Title 2, length: 4884)
User(id: 4, name: Title 3, length: 9693)
User(id: 5, name: Title 4, length: 8897)

1234
```

用`limit()`和切片都可以截取部分数据，其区别在于：
用`limit()`是在查询时就限定了查询的结果，即查询的结果并不包括所有结果，而是指定的部分；
而切片是现将所有符合的结果查询出来，得到的是一个列表，再对列表切片；
显然，用`limit()`方法的**效率更高**。

## 三、高级查询和子查询

### 1.group_by

`group_by()`指定根据某个字段分组。

创建数据库表并添加数据：

```python
from sqlalchemy import Column, Integer, String, Enum
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from random import *

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


def generate_random_str(randomlength=6):
    '''生成指定长度的随机字符串'''
    random_str = ''
    base_str = 'ABCDEFGHIGKLMNOPQRSTUVWXYZabcdefghigklmnopqrstuvwxyz0123456789'
    length = len(base_str) - 1
    for i in range(randomlength):
        random_str += base_str[randint(0, length)]
    return random_str


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(50), nullable=False)
    gender = Column(Enum('男', '女'))
    age = Column(Integer)

    def __str__(self):
        return 'User(id: {}, username: {}, gender: {}, age: {})'.format(self.id, self.username, self.gender, self.age)


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()


for _ in range(20):
    user = User(username=generate_random_str(), gender=choice(['男', '女']), age=randint(5, 40))
    session.add(user)

session.commit()
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051
```

用`group_by()`进行查询：

```python
from sqlalchemy import Column, Integer, String, Enum
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from random import *

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(50), nullable=False)
    gender = Column(Enum('男', '女'))
    age = Column(Integer)

    def __str__(self):
        return 'User(id: {}, username: {}, gender: {}, age: {})'.format(self.id, self.username, self.gender, self.age)


# Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

results = session.query(User.gender).group_by(User.gender).all()
for result in results:
    print(result)
1234567891011121314151617181920212223242526272829303132333435363738
```

打印：

```python
('男',)
('女',)

123
```

很显然，`query()`和`group_by()`的参数所包含的**字段要保持一致**。

可以对每个类别进行计数，如下：

```python
from sqlalchemy import Column, Integer, String, Enum
from sqlalchemy import create_engine, func
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(50), nullable=False)
    gender = Column(Enum('男', '女'))
    age = Column(Integer)

    def __str__(self):
        return 'User(id: {}, username: {}, gender: {}, age: {})'.format(self.id, self.username, self.gender, self.age)


# Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

results = session.query(User.gender, func.count(User.id)).group_by(User.gender).all()
for result in results:
    print(result)
12345678910111213141516171819202122232425262728293031323334353637
```

打印：

```python
('男', 12)
('女', 8)

123
```

显然，查到了性别为男、女的人数。

### 2.having

`having()`对通过`group_by()`分类后的结果进一步过滤。
显然，`having()`中的字段也应该与`group_by()`中的**字段保持一致**。

根据年龄分组后再根据年龄条件过滤：

```python
from sqlalchemy import Column, Integer, String, Enum
from sqlalchemy import create_engine, func
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(50), nullable=False)
    gender = Column(Enum('男', '女'))
    age = Column(Integer)

    def __str__(self):
        return 'User(id: {}, username: {}, gender: {}, age: {})'.format(self.id, self.username, self.gender, self.age)


# Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

results = session.query(User.age, func.count(User.id)).group_by(User.age).having(User.age > 18).all()
for result in results:
    print(result)
12345678910111213141516171819202122232425262728293031323334353637
```

打印：

```python
(20, 1)
(21, 2)
(22, 1)
(32, 1)
(34, 1)
(36, 1)
(39, 3)

12345678
```

显然，得到了大于18的各个年龄阶段的人数。

### 3.子查询

MySQL中有子查询，SQLAlchemy也支持子查询，一般过程是先构造子查询，再将子查询放到父查询中。

构造数据：

```python
from random import *
from sqlalchemy import Column, Integer, String, Enum
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


def generate_random_str(randomlength=6):
    '''生成指定长度的随机字符串'''
    random_str = ''
    base_str = 'ABCDEFGHIGKLMNOPQRSTUVWXYZabcdefghigklmnopqrstuvwxyz0123456789'
    length = len(base_str) - 1
    for i in range(randomlength):
        random_str += base_str[randint(0, length)]
    return random_str


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(50), nullable=False)
    gender = Column(Enum('男', '女'))
    age = Column(Integer)
    city = Column(Enum('Beijing', 'New York', 'London', 'Paris', 'Tokyo'))

    def __str__(self):
        return 'User(id: {}, username: {}, gender: {}, age: {}, city: {})'.format(self.id, self.username, self.gender, self.age, self.city)


Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

for _ in range(50):
    user = User(username=generate_random_str(), gender=choice(['男', '女']), age=randint(5, 40), city=choice(['Beijing', 'New York', 'London', 'Paris', 'Tokyo']))
    session.add(user)

session.commit()
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051
```

运行即创建数据成功。

现在有一个需求，查询和id为3的user在同一个城市的user，可以先用一条语句查询出id为3的用户所在的城市，然后再根据城市进行第二次查询：

```python
from random import *
from sqlalchemy import Column, Integer, String, Enum
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(50), nullable=False)
    gender = Column(Enum('男', '女'))
    age = Column(Integer)
    city = Column(Enum('Beijing', 'New York', 'London', 'Paris', 'Tokyo'))

    def __str__(self):
        return 'User(id: {}, username: {}, gender: {}, age: {}, city: {})'.format(self.id, self.username, self.gender, self.age, self.city)


# Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

user = session.query(User).filter(User.id == 3).first()
results = session.query(User).filter(User.city == user.city).all()
for result in results:
    print(result)
12345678910111213141516171819202122232425262728293031323334353637383940
```

打印：

```python
User(id: 3, username: gTq26G, gender: 女, age: 8, city: Paris)
User(id: 10, username: h7hs1d, gender: 男, age: 35, city: Paris)
User(id: 14, username: xmbm6H, gender: 男, age: 31, city: Paris)
User(id: 20, username: Gq0AlG, gender: 男, age: 39, city: Paris)
User(id: 25, username: ksGOnY, gender: 女, age: 32, city: Paris)
User(id: 27, username: H8Amxe, gender: 女, age: 34, city: Paris)
User(id: 28, username: 9Vmt4P, gender: 女, age: 9, city: Paris)
User(id: 31, username: rM3Qoe, gender: 男, age: 20, city: Paris)
User(id: 32, username: 1XEtX3, gender: 男, age: 35, city: Paris)
User(id: 33, username: 3ZzS4b, gender: 男, age: 13, city: Paris)
User(id: 34, username: naCdU4, gender: 女, age: 35, city: Paris)
User(id: 35, username: iG7Yfp, gender: 女, age: 27, city: Paris)
User(id: 36, username: 12kwgW, gender: 男, age: 34, city: Paris)
User(id: 37, username: Dwl6Ba, gender: 女, age: 18, city: Paris)
User(id: 47, username: ehe80s, gender: 女, age: 7, city: Paris)
User(id: 50, username: MdxxLe, gender: 女, age: 33, city: Paris)

1234567891011121314151617
```

显然，打印出了所有符合条件的结果，但是这是多个语句查询出的，显得很麻烦，可以用子查询，如下：

```python
from sqlalchemy import Column, Integer, String, Enum
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_orm'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

engine = create_engine(DB_URL)
Base = declarative_base(engine)


class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String(50), nullable=False)
    gender = Column(Enum('男', '女'))
    age = Column(Integer)
    city = Column(Enum('Beijing', 'New York', 'London', 'Paris', 'Tokyo'))

    def __str__(self):
        return 'User(id: {}, username: {}, gender: {}, age: {}, city: {})'.format(self.id, self.username, self.gender, self.age, self.city)


# Base.metadata.drop_all()
Base.metadata.create_all()

Session = sessionmaker(bind=engine)
session = Session()

sub = session.query(User.city.label('city')).filter(User.id == 3).subquery()
results = session.query(User).filter(User.city == sub.c.city).all()
for result in results:
    print(result)
123456789101112131415161718192021222324252627282930313233343536373839
```

可以看到，一个查询如果想要变为子查询，需要通过`subquery()`方法实现，变成子查询后，通过`子查询.c属性`来访问查询出来的列。

# Python全栈（七）Flask框架之10.ORM插件、Script和Migrate

## 一、Flask-SQLAlchemy插件

之前使用SQLAlchemy都是独立于Flask的，如果需要在Flask中使用SQLAlchemy，可以使用flask-sqlalchemy插件，它是对SQLAlchemy的一个简单**封装**，以便在flask中更简单方便地使用sqlalchemy。
切换到虚拟环境使用命令`pip install flask-sqlalchemy`安装即可。

使用flask-sqlalchemy创建数据库模型（创建flask_sqlalchemy_test.py文件）如下：

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_sqlalchemy'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = DB_URL
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

# 数据库初始化
db = SQLAlchemy(app)


# ORM类定义
class User(db.Model):
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(30))


class Article(db.Model):
    __tablename__ = 'article'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    title = db.Column(db.String(50))

    uid = db.Column(db.Integer, db.ForeignKey('users.id'))

    author = db.relationship('User', backref='articles')


# 映射模型到数据库表
db.create_all()


@app.route('/')
def index():
    return '首页'


if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748
```

运行后，查询数据库如下：

```sql
show tables;
1
```

打印：

```sql
+----------------------------+     
| Tables_in_flask_sqlalchemy |     
+----------------------------+     
| article                    |     
| users                      |     
+----------------------------+     
2 rows in set (0.00 sec)           
                                   
12345678
```

显然，表已经创建成功。
使用Flask-SQLAlchemy时，所有的类都继承自db.Model，并且所有的Column和数据类型也都成为db的一个属性；
在定义模型时也可以不指定 `__tablename__`属性，不指定时会默认以模型类名的小写形式作为表名。

插入并查询数据测试如下：

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_sqlalchemy'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = DB_URL
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)


class User(db.Model):
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(30))

    def __repr__(self):
        return 'User(id: {}, name: {})'.format(self.id, self.name)


class Article(db.Model):
    __tablename__ = 'article'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    title = db.Column(db.String(50))

    uid = db.Column(db.Integer, db.ForeignKey('users.id'))

    author = db.relationship('User', backref='articles')

    def __repr__(self):
        return 'User(id: {}, title: {})'.format(self.id, self.title)


db.drop_all()
db.create_all()

# 添加数据
user = User(name='Corley')
article = Article(title='Python')
article.author = user
db.session.add(article)
db.session.commit()

# 查询数据
user = User.query.all()
print(user)

@app.route('/')
def index():
    return '首页'


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162
```

打印：

```python
[User(id: 1, name: Corley)]
 * Serving Flask app "flask_sqlalchemy_test" (lazy loading)
 * Environment: production
123
```

显然，已经成功插入数据并查询到。
添加数据时还是使用session，同时session成了db的属性；
查询数据是通过`Model.query`的方式进行的。

插入多条数据并排序查询测试：

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_sqlalchemy'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = DB_URL
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)


class User(db.Model):
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(30))

    def __repr__(self):
        return 'User(id: {}, name: {})'.format(self.id, self.name)


class Article(db.Model):
    __tablename__ = 'article'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    title = db.Column(db.String(50))

    uid = db.Column(db.Integer, db.ForeignKey('users.id'))

    author = db.relationship('User', backref='articles')

    def __repr__(self):
        return 'User(id: {}, title: {})'.format(self.id, self.title)


@app.route('/')
def index():
    return '首页'


if __name__ == '__main__':
    # app.run(debug=True)

    db.drop_all()
    db.create_all()

    # 添加数据
    user1 = User(name='Corley')
    user2 = User(name='John')
    article1 = Article(title='Python')
    article2 = Article(title='Flask')
    article1.author = user1
    article2.author = user2

    db.session.add(article1)
    db.session.add(article2)
    db.session.commit()

    # 查询数据
    user = User.query.order_by(User.name.desc()).all()
    print(user)

12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667
```

打印：

```python
[User(id: 2, name: John), User(id: 1, name: Corley)]

12
```

SQLAlchemy的**原生查询方式**也可以使用，如下：

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_sqlalchemy'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = DB_URL
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)


class User(db.Model):
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(30))

    def __repr__(self):
        return 'User(id: {}, name: {})'.format(self.id, self.name)


class Article(db.Model):
    __tablename__ = 'article'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    title = db.Column(db.String(50))

    uid = db.Column(db.Integer, db.ForeignKey('users.id'))

    author = db.relationship('User', backref='articles')

    def __repr__(self):
        return 'User(id: {}, title: {})'.format(self.id, self.title)


@app.route('/')
def index():
    return '首页'


if __name__ == '__main__':
    # app.run(debug=True)

    db.drop_all()
    db.create_all()

    # 添加数据
    user1 = User(name='Corley')
    user2 = User(name='John')
    article1 = Article(title='Python')
    article2 = Article(title='Flask')
    article1.author = user1
    article2.author = user2

    db.session.add(article1)
    db.session.add(article2)
    db.session.commit()

    # 查询数据
    # users = User.query.order_by(User.name.desc()).all()
    users = db.session.query(User).all()
    print(users)

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768
```

打印：

```python
[User(id: 1, name: Corley), User(id: 2, name: John)]

12
```

显然，也是能查询到数据的，但是第一种方式更简单易用。

删除数据测试：

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_sqlalchemy'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = DB_URL
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)


class User(db.Model):
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(30))

    def __repr__(self):
        return 'User(id: {}, name: {})'.format(self.id, self.name)


class Article(db.Model):
    __tablename__ = 'article'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    title = db.Column(db.String(50))

    uid = db.Column(db.Integer, db.ForeignKey('users.id'))

    author = db.relationship('User', backref='articles')

    def __repr__(self):
        return 'User(id: {}, title: {})'.format(self.id, self.title)


@app.route('/')
def index():
    return '首页'


if __name__ == '__main__':
    # app.run(debug=True)

    db.drop_all()
    db.create_all()

    # 添加数据
    user1 = User(name='Corley')
    user2 = User(name='John')
    article1 = Article(title='Python')
    article2 = Article(title='Flask')
    article1.author = user1
    article2.author = user2

    db.session.add(article1)
    db.session.add(article2)
    db.session.commit()

    # 删除数据
    user = User.query.filter(User.id == 2).first()
    db.session.delete(user)
    db.session.commit()

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768
```

运行后查询数据库，数据已经被删除。

## 二、Flask-Script

### 1.flask-script的基本使用

Flask-Script的作用是可以通过命令行的形式来操作Flask，如通过命令运行开发版本的服务器、设置数据库、定时任务等。
在虚拟环境中安装flask-script的命令是`pip install flask-script`。

使用装饰器可以定义命令，创建manage.py如下：

```python
from flask_script import Manager
from flask_sqlalchemy_test import app

manage = Manager(app)


@manage.command
def index():
    print('Hello Flask')


if __name__ == '__main__':
    manage.run()
12345678910111213
```

此时在命令行中切换到当前目录，执行命令`python manage.py index`，打印：

```python
Hello Flask

12
```

显然，通过命令行执行了manage.py中的`index()`函数；
通常，把脚本命令代码放在manage.py文件中。

除了使用装饰器，还可以继承自Command类，如下：

```python
from flask_script import Manager, Command
from flask_sqlalchemy_test import app

manage = Manager(app)


class Hello(Command):
    def run(self):
        print("Hello Flask")


manage.add_command('hello', Hello())

if __name__ == '__main__':
    manage.run()
123456789101112131415
```

执行命令`python manage.py hello`，打印：

```python
Hello Flask

12
```

使用这种方式需要注意：

- 自定义类必须继承自Command基类；
- 必须实现`run()`方法；
- 必须通过`add_command()`方法添加命令。

### 2.flask-script传入参数

在前面定义命令的方法中，可以传入参数，有3种方式。

#### 直接使用@option装饰器

在命令行中接收参数测试如下：

```python
from flask_script import Manager
from flask_sqlalchemy_test import app

manage = Manager(app)


@manage.command
def index():
    print('Hello Flask')


@manage.option('-n', '--name', dest='name')
@manage.option('-u', '--url', dest='url')
def hello(name, url):
    print('Hello,', name, url)


if __name__ == '__main__':
    manage.run()
12345678910111213141516171819
```

执行`python manage.py hello -n Corley -u www.corley.com`，打印如下：

```python
Hello, Corley www.corley.com

12
```

显然，接收到了指定的参数值，在定义接收参数时，除了定义`-n`、`-u`，还指定了`--name`、`--url`，一种是简写，一种是全写，即还可以写成`python manage.py hello --name Corley --url www.corley.com`。
传参数时，还可以传入默认值，在命令行中没有给定某参数的值时，就可以使用给定的默认值，如下：

```python
@manage.option('-n', '--name', dest='name', default='Corley')
@manage.option('-u', '--url', dest='url', default=None) 
12
```

#### command装饰器中传参

如下：

```python
from flask_script import Manager
from flask_sqlalchemy_test import app

manage = Manager(app)


@manage.command
def hello(name="Corley"):
    print("hello", name)


if __name__ == '__main__':
    manage.run()

1234567891011121314
```

在命令行中执行`python manage.py hello -n Jack`，打印：

```python
Hello Jack
1
```

显然这种方式不够灵活。

#### 继承Command类传参

```python
from flask_script import Manager, Command, Option
from flask_sqlalchemy_test import app

manage = Manager(app)


class Hello(Command):
    option_list = (
        Option('-n', '--name', dest='name'),
    )

    def run(self, name):
        print("Hello %s" % name)


manage.add_command('hello', Hello())

if __name__ == '__main__':
    manage.run()

1234567891011121314151617181920
```

在命令行中执行`python manage.py hello -n Corley`，打印：

```python
Hello Corley
1
```

如果要在指定参数的时候动态执行一些逻辑，可以使用`get_options()`方法，如下：

```python
from flask_script import Manager, Command, Option
from flask_sqlalchemy_test import app

manage = Manager(app)


class Hello(Command):
    def __init__(self, default_name='Corley'):
        super().__init__()
        self.default_name = default_name

    def get_options(self):
        return [
            Option('-n', '--name', dest='name', default=self.default_name),
        ]

    def run(self, name):
        print("Hello %s" % name)


manage.add_command('hello', Hello())

if __name__ == '__main__':
    manage.run()

12345678910111213141516171819202122232425
```

### 3.flask-script练习

通过命令行添加管理员测试如下：
先创建一个目录flask_add_admin，里面先创建config.py：

```python
# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_sqlalchemy'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

SQLALCHEMY_DATABASE_URI = DB_URL
SQLALCHEMY_TRACK_MODIFICATIONS = False

1234567891011
```

再创建script.py：

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
import config

app = Flask(__name__)
app.config.from_object(config)

db = SQLAlchemy(app)

class AdminUser(db.Model):
    __tablename__ = 'admin_users'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(30))
    email = db.Column(db.String(50))


db.create_all()

if __name__ == '__main__':
    app.run(debug=True)
1234567891011121314151617181920
```

运行创建数据表，再创建manage.py：

```python
from flask_script import Manager
from script import app, AdminUser, db

manage = Manager(app)


@manage.option('-u', '--name', dest='name')
@manage.option('-e', '--email', dest='email')
def add_user(name, email):
    user = AdminUser(name=name, email=email)
    db.session.add(user)
    db.session.commit()


if __name__ == '__main__':
    manage.run()

1234567891011121314151617
```

命令行切换到当前目录，执行`python manage.py add_user -u Corley -e 123@163.com`，此时即将管理员插入到数据库，再查询数据表：

```sql
select * from admin_users;
1
```

打印：

```
+----+--------+-------------+           
| id | name   | email       |           
+----+--------+-------------+           
|  1 | Corley | 123@163.com |           
+----+--------+-------------+           
1 row in set (0.00 sec)                 
                                        
1234567
```

显然，此时插入数据成功。

## 三、Flask-Migrate

在实际开发环境中，经常需要修改数据库，一般修改数据库时不会直接手动修改，而是去修改对应的ORM模型，然后再把模型映射到数据库中。
flask-migrate就是专门实现这个功能的，它是对Alembic的**进一步封装**，并集成到Flask，所有的迁移操作其实都是Alembic做的，能跟踪模型的变化，并将变化映射到数据库中。
在虚拟环境中安装flask-migrate使用`pip install flask-migrate`命令。

先创建一个新文件夹，如flask_migrate_test，其中创建文件config.py用于保存配置信息：

```python
# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_migrate'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

SQLALCHEMY_DATABASE_URI = DB_URL
SQLALCHEMY_TRACK_MODIFICATIONS = False

1234567891011
```

数据库需要提前创建好。

再创建程序主入口flask_app.py：

```python
import config
from flask import Flask
from exts import db

app = Flask(__name__)

app.config.from_object(config)

db.init_app(app)


@app.route('/')
def index():
    return '首页'

if __name__ == '__main__':
    app.run(debug=True)
1234567891011121314151617
```

再创建模板文件models.py：

```python
from exts import db

class User(db.Model):
    __tablename__ = 'user'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(30))
    email = db.Column(db.String(50))
    password = db.Column(db.String(30))
12345678
```

再创建中间文件exts.py：

```python
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()
123
```

该文件用于解决互相引用的冲突。

再创建manage.py：

```python
from flask_script import Manager
from flask_migrate import Migrate, MigrateCommand
from exts import db
from flask_app import app
from models import User

manage = Manager(app)

Migrate(app, db)

manage.add_command('db', MigrateCommand)


if __name__ == '__main__':
    manage.run()
123456789101112131415
```

此时运行flask_app.py，可以正常运行；
要让Flask-Migrate管理app中的数据库，需要使用`Migrate(app, db)`来绑定app和数据库；
MigrateCommand是flask-migrate集成的命令，使用`manage.add_command('db', MigrateCommand)`将其添加到脚本命令，以后运行`python manage.py db xxx`的命令，其实就是执行MigrateCommand。

在命令行中切换到当前目录，先执行命令`python manage.py db init`初始化迁移文件夹，看到

```shell
Creating directory XXX\flask_migrate_test\migrations ...  done
Creating directory XXX\flask_migrate_test\migrations\versions ...  done
Generating XXX\flask_migrate_test\migrations\alembic.ini ...  done
Generating XXX\flask_migrate_test\migrations\env.py ...  done
Generating XXX\flask_migrate_test\migrations\README ...  done
Generating XXX\flask_migrate_test\migrations\script.py.mako ...  done

1234567
```

即初始化成功，此时看当前目录可以看到migrates文件夹，下面有打印输出中显示的文件（夹）。

再运行命令`python manage.py db migrate`把当前的模型添加到迁移文件中，打印：

```shell
INFO  [alembic.runtime.migration] Context impl MySQLImpl.
INFO  [alembic.runtime.migration] Will assume non-transactional DDL.
INFO  [alembic.autogenerate.compare] Detected added table 'user'
Generating XXX\flask_migrate_test\migrations\versions\2b779ddbb317_.py ...  done
1234
```

即迁移成功，此时查看数据库：

```sql
show tables;
1
```

打印：

```sql
+-------------------------+         
| Tables_in_flask_migrate |         
+-------------------------+         
| alembic_version         |         
| user                    |         
+-------------------------+         
2 rows in set (0.00 sec)            
                                    
12345678
```

出现数据库表alembic_version和user则表明创建成功。

此时再运行命令`python manage.py db upgrade`把迁移文件中的数据库操作映射到数据库中，可以看到：

```shell
INFO  [alembic.runtime.migration] Context impl MySQLImpl.
INFO  [alembic.runtime.migration] Will assume non-transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 2b779ddbb317, empty message
123
```

打印出相应的版本号，查询表：

```sql
select * from alembic_version;
1
```

打印：

```sql
+--------------+          
| version_num  |          
+--------------+          
| 2b779ddbb317 |          
+--------------+          
1 row in set (0.01 sec)   
                          
1234567
```

显然，版本号已经插入数据表。

如果需要修改表，即修改模型，如models.py修改如下：

```python
from exts import db

class User(db.Model):
    __tablename__ = 'user'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(30))
    email = db.Column(db.String(50))
    password = db.Column(db.String(30))
    age = db.Column(db.Integer)
123456789
```

即增加了一个字段age，此时只需要再执行`python manage.py db migrate`和`python manage.py db upgrade`即可将变化映射到数据库，migrate打印如下：

```shell
INFO  [alembic.runtime.migration] Context impl MySQLImpl.
INFO  [alembic.runtime.migration] Will assume non-transactional DDL.
INFO  [alembic.autogenerate.compare] Detected added column 'user.age'
Generating XXX\flask_migrate_test\migrations\versions\b95ca5177a74_.py ...  done

12345
```

upgrade打印如下：

```shell
INFO  [alembic.runtime.migration] Context impl MySQLImpl.
INFO  [alembic.runtime.migration] Will assume non-transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade 2b779ddbb317 -> b95ca5177a74, empty message
```

# Python全栈（七）Flask框架之11.WTForms及其常见应用

flask-wtf是一个简化了WTFForms操作的第三方库，WTForms表单的两个主要功能是 **验证用户提交数据的合法性**和 **渲染模板**，还包括一些其他的功能，如CSRF保护、文件上传等。
安装flask-wtf使用命令 `pip install flask-wtf`。



## 一、WTForms表单验证

### 1.表单验证的基本使用

不使用WTF进行表单验证测试：
创建程序主入口wtf_demo.py：

```python
from flask import Flask, request, render_template

app = Flask(__name__)


@app.route('/')
def index():
    return '首页'


@app.route('/register/', methods=['GET', 'POST'])
def register():
    if request.method == 'GET':
        return render_template('register.html')
    else:
        username = request.form.get('username')
        password = request.form.get('password')
        pwd_confirm = request.form.get('pwd_confirm')
        if len(username) < 3 or len(username) > 15:
            return '用户名长度不正确'
        if password != pwd_confirm:
            return '两次密码输入不一致'
        if len(username) < 6 or len(username) > 20:
            return '密码长度不正确'


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526272829
```

模板目录templates下创建register.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册</title>
</head>
<body>
<form action="" method="post">
    用户名：<input type="text" name="username">
    <br>
    密码：<input type="text" name="password">
    <br>
    确认密码：<input type="text" name="pwd_confirm">
    <br>
    <input type="submit" value="注册">
</form>

</body>
</html>
12345678910111213141516171819
```

显示：
![flask form normal verification](https://img-blog.csdnimg.cn/20200503155528531.gif#pic_center)

显然，可以达到验证表单的效果，但是在`register()`视图函数中一般只是得到验证是否成功的结果即可，可以将验证单独抽离出来，使用flask-wtf进行专门验证，新建forms.py如下：

```python
from wtforms import Form, StringField
from wtforms.validators import Length, EqualTo

class RegisterForm(Form):
    username = StringField(validators=[Length(min=3, max=15, message='用户名长度不正确')])
    password = StringField(validators=[Length(min=6, max=20, message='密码长度不正确')])
    pwd_confirm = StringField(validators=[Length(min=6, max=20), EqualTo('password', message='两次密码不一致')])

12345678
```

修改wtf_demo.py如下：

```python
from flask import Flask, request, render_template
from forms import RegisterForm

app = Flask(__name__)


@app.route('/')
def index():
    return '首页'


@app.route('/register/', methods=['GET', 'POST'])
def register():
    if request.method == 'GET':
        return render_template('register.html')
    else:
        form = RegisterForm(request.form)
        if form.validate():
            return '验证成功'
        else:
            return '验证失败：\n' + str(form.errors)


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526
```

RegisterForm初始化时传入request.form，并且根据`form.validate()`的值来判断用户提交的数据是否满足表单的验证。

显示：
![flask form wtf verification](https://img-blog.csdnimg.cn/20200503172425886.gif#pic_center)
在forms.py中指定了需要上传的参数，并且指定了验证器，比如username的长度应该在3-15之间，password长度必须在6-20之间，并且应该和pwd_confirm相等才能通过验证。

### 2.Flask-WTF常用的验证器

#### Email

可以直接使用Email类进行右键地址的验证。
在forms.py中新增LoginForm如下：

```python
from wtforms import Form, StringField
from wtforms.validators import Length, EqualTo, Email

class RegisterForm(Form):
    username = StringField(validators=[Length(min=3, max=15, message='用户名长度不正确')])
    password = StringField(validators=[Length(min=6, max=20, message='密码长度不正确')])
    pwd_confirm = StringField(validators=[Length(min=6, max=20), EqualTo('password', message='两次密码不一致')])


class LoginForm(Form):
    email = StringField(validators=[Email(message='邮箱格式不正确')])
1234567891011
```

主程序中增加视图函数：

```python
from flask import Flask, request, render_template
from forms import RegisterForm, LoginForm

app = Flask(__name__)

app.config['TEMPLATE_AUTO_RELOAD'] = True


@app.route('/')
def index():
    return '首页'


@app.route('/register/', methods=['GET', 'POST'])
def register():
    if request.method == 'GET':
        return render_template('register.html')
    else:
        form = RegisterForm(request.form)
        if form.validate():
            return '验证成功'
        else:
            return '验证失败：\n' + str(form.errors)


@app.route('/login/', methods=['GET', 'POST'])
def login():
    if request.method == 'GET':
        return render_template('login.html')
    else:
        form = LoginForm(request.form)
        if form.validate():
            return '验证成功'
        else:
            return '验证失败：\n' + str(form.errors)

if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021222324252627282930313233343536373839
```

新建login.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
</head>
<body>
    <form action="" method="post">
        <table>
            <tr>
                <td>邮箱：</td>
                <td><input type="text" name="email"></td>
            </tr>
            <tr>
                <td><input type="submit" value="提交"></td>
            </tr>
        </table>
    </form>
</body>
</html>
1234567891011121314151617181920
```

运行主程序，显示：
![flask wtf email](https://img-blog.csdnimg.cn/20200509205248368.gif#pic_center)

#### Number

可以通过NumberRange类对数的范围进行验证。
修改forms.py如下;

```python
from wtforms import Form, StringField, IntegerField
from wtforms.validators import Length, EqualTo, Email, NumberRange


class RegisterForm(Form):
    username = StringField(validators=[Length(min=3, max=15, message='用户名长度不正确')])
    password = StringField(validators=[Length(min=6, max=20, message='密码长度不正确')])
    pwd_confirm = StringField(validators=[Length(min=6, max=20), EqualTo('password', message='两次密码不一致')])


class LoginForm(Form):
    age = IntegerField(validators=[NumberRange(1, 120, message='年龄范围有误！！！')])
123456789101112
```

修改login.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
</head>
<body>
    <form action="" method="post">
        <table>
            <tr>
                <td>年龄：</td>
                <td><input type="text" name="age"></td>
            </tr>
            <tr>
                <td><input type="submit" value="提交"></td>
            </tr>
        </table>
    </form>
</body>
</html>
1234567891011121314151617181920
```

显示：
![flask wtf number](https://img-blog.csdnimg.cn/20200509205307981.gif#pic_center)

#### 必填

可以通过InputRequired类限制某些字段必填。
forms.py修改如下：

```python
from wtforms import Form, StringField
from wtforms.validators import Length, EqualTo, InputRequired


class RegisterForm(Form):
    username = StringField(validators=[Length(min=3, max=15, message='用户名长度不正确')])
    password = StringField(validators=[Length(min=6, max=20, message='密码长度不正确')])
    pwd_confirm = StringField(validators=[Length(min=6, max=20), EqualTo('password', message='两次密码不一致')])


class LoginForm(Form):
    username = StringField(validators=[InputRequired(message='用户名必填')])
123456789101112
```

login.html修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
</head>
<body>
    <form action="" method="post">
        <table>
            <tr>
                <td>用户名：</td>
                <td><input type="text" name="username"></td>
            </tr>
            <tr>
                <td><input type="submit" value="提交"></td>
            </tr>
        </table>
    </form>
</body>
</html>
1234567891011121314151617181920
```

显示：
![flask wtf inputrequired](https://img-blog.csdnimg.cn/20200509205341759.gif#pic_center)

#### 正则表达式

可以通过Regexp类自定义正则表达式进行验证。
forms.py修改如下：

```python
from wtforms import Form, StringField
from wtforms.validators import Length, EqualTo, Regexp


class RegisterForm(Form):
    username = StringField(validators=[Length(min=3, max=15, message='用户名长度不正确')])
    password = StringField(validators=[Length(min=6, max=20, message='密码长度不正确')])
    pwd_confirm = StringField(validators=[Length(min=6, max=20), EqualTo('password', message='两次密码不一致')])


class LoginForm(Form):
    phone_number = StringField(validators=[Regexp(r'1[35789]\d{9}', message='手机号码不正确')])
123456789101112
```

login.py修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
</head>
<body>
    <form action="" method="post">
        <table>
            <tr>
                <td>手机号：</td>
                <td><input type="text" name="phone_number"></td>
            </tr>
            <tr>
                <td><input type="submit" value="提交"></td>
            </tr>
        </table>
    </form>
</body>
</html>
1234567891011121314151617181920
```

显示：
![flask wtf regexp](https://img-blog.csdnimg.cn/20200509205429429.gif#pic_center)

#### URL

可以使用URL类验证某个字符串是否是标准的URL格式。
forms.py修改如下：

```python
from wtforms import Form, StringField
from wtforms.validators import Length, EqualTo, URL


class RegisterForm(Form):
    username = StringField(validators=[Length(min=3, max=15, message='用户名长度不正确')])
    password = StringField(validators=[Length(min=6, max=20, message='密码长度不正确')])
    pwd_confirm = StringField(validators=[Length(min=6, max=20), EqualTo('password', message='两次密码不一致')])


class LoginForm(Form):
    info_page = StringField(validators=[URL(message='地址格式不正确')])
123456789101112
```

login.html修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
</head>
<body>
    <form action="" method="post">
        <table>
            <tr>
                <td>个人主页：</td>
                <td><input type="text" name="info_page"></td>
            </tr>
            <tr>
                <td><input type="submit" value="提交"></td>
            </tr>
        </table>
    </form>
</body>
</html>
1234567891011121314151617181920
```

显示：
![flask wtf url](https://img-blog.csdnimg.cn/20200509205446257.gif#pic_center)

#### 扩展-验证码的验证

要验证验证码长度和有效性，长度用Length验证，有效性可以在表单类中自定义方法即可。

forms.py修改如下：

```python
from wtforms import Form, StringField
from wtforms.validators import Length, EqualTo, ValidationError


class RegisterForm(Form):
    username = StringField(validators=[Length(min=3, max=15, message='用户名长度不正确')])
    password = StringField(validators=[Length(min=6, max=20, message='密码长度不正确')])
    pwd_confirm = StringField(validators=[Length(min=6, max=20), EqualTo('password', message='两次密码不一致')])


class LoginForm(Form):
    captcha = StringField(validators=[Length(min=4, max=4, message='验证码不正确')])

    def validate_captcha(self, field):
        '''自定义验证，方法名为固定格式，即validate_加要验证的变量名'''
        if field.data != '1234':
            raise ValidationError("验证码错误")
1234567891011121314151617
```

login.html修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
</head>
<body>
    <form action="" method="post">
        <table>
            <tr>
                <td>验证码：</td>
                <td><input type="text" name="captcha"></td>
            </tr>
            <tr>
                <td><input type="submit" value="提交"></td>
            </tr>
        </table>
    </form>
</body>
</html>
1234567891011121314151617181920
```

显示：
![flask wtf captcha](https://img-blog.csdnimg.cn/20200509205507393.gif#pic_center)
可以得到，自定义验证需要在表单类中定义方法，方法名为固定格式，即validate_加要验证的变量名。

## 二、WTF渲染模板

wtforms可以渲染模板，省去一部分代码。
测试：
wtf_demo.py如下：

```python
from flask import Flask, request, render_template
from forms import RegisterForm

app = Flask(__name__)

app.config['TEMPLATE_AUTO_RELOAD'] = True


@app.route('/')
def index():
    return '首页'


@app.route('/register/', methods=['GET', 'POST'])
def register():
    form = RegisterForm(request.form)
    if request.method == 'GET':
        return render_template('register.html', form=form)
    else:
        username = form.username.data
        password = form.password.data
        pwd_confirm = form.pwd_confirm.data
        if form.validate():

            return '验证成功--User: ' + username + ' Password: ' + password
        else:
            return '验证失败：\n' + str(form.errors)


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526272829303132
```

在渲染模板的时候传入了form表单参数，这样在模板中就可以使用表单form变量了。
forms.py如下：

```python
from wtforms import Form, StringField
from wtforms.validators import Length, EqualTo


class RegisterForm(Form):
    username = StringField('用户名', validators=[Length(min=3, max=15, message='用户名长度不正确')])
    password = StringField('密码', validators=[Length(min=6, max=20, message='密码长度不正确')])
    pwd_confirm = StringField('密码确认', validators=[Length(min=6, max=20), EqualTo('password', message='两次密码不一致')])
12345678
```

每个变量都增加了一个位置参数，用于在html文件中做标签提示。
register.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册</title>
</head>
<body>
<form action="" method="post">
    <table>
        <tr>
            <td>{{ form.username.label }}</td>
            <td>{{ form.username() }}</td>
        </tr>
        <tr>
            <td>{{ form.password.label }}</td>
            <td>{{ form.password() }}</td>
        </tr>
        <tr>
            <td>{{ form.pwd_confirm.label }}</td>
            <td>{{ form.pwd_confirm() }}</td>
        </tr>
        <tr>
            <td></td>
            <td><input type="submit" value="提交"></td>
        </tr>
    </table>
</form>

</body>
</html>
123456789101112131415161718192021222324252627282930
```

显示：
![flask wtf render template](https://img-blog.csdnimg.cn/20200509205528644.gif#pic_center)

## 三、WTF文件上传

### 1.文件上传的基本使用

先创建项目目录flask_upload，创建主文件flask_app.py如下：

```python
from flask import Flask, request,render_template
from werkzeug.utils import secure_filename

app = Flask(__name__)
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    return '首页'


@app.route('/upload/', methods=['GET', 'POST'])
def upload():
    if request.method == 'GET':
        return render_template('upload.html')
    else:
        desc = request.form.get('desc')
        image_file = request.files.get('image_file')
        file_name = secure_filename(image_file.filename)
        image_file.save('files/images/' + file_name)
        return '文件上传成功--' + desc


if __name__ == '__main__':
    app.run(debug=True)
12345678910111213141516171819202122232425
```

创建templates目录，下面创建upload.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>文件上传</title>
</head>
<body>
{# 文件上传，form表单必须要添加enctype属性 #}
<form action="" method="post" enctype="multipart/form-data">
    <table>
        <tr>
            <td>头像</td>
            <td><input type="file" name="image_file"></td>
        </tr>
        <tr>
            <td>描述</td>
            <td><input type="text" name="desc"></td>
        </tr>
        <tr>
            <td><input type="submit" value="上传"></td>
        </tr>
    </table>
</form>
</body>
</html>
12345678910111213141516171819202122232425
```

在项目目录下创建files文件夹用于保存上传的文件，下面创建images子目录，运行主程序之后，显示：
![flask wtf upload](https://img-blog.csdnimg.cn/20200509205554996.gif#pic_center)
显然，文件上传成功。
我们可以得到：
在模版中的form标签中，需要指定`enctype='multipart/form-data'`才能上传文件；
在后台如果想要获取上传的文件，应该使用`request.files.get(file_name)`来获取；
保存文件之前，先要使用**werkzeug.utils**类中的`secure_filename()`方法来对上传上来的文件名进行过滤，以确保安全性；
获取到上传上来的文件后，使用`filename.save(路径)`方法来保存文件。

可以看到，在上传的文件中，纯英文文件名保留不变，有中文的将中文去掉，这是由于werkzeug.utils模块中的secure_filename方法只支持ASCII编码的字符，这个问题是可以解决的，可参考https://blog.csdn.net/qq_36390239/article/details/98847888解决。

### 2.文件上传的表单验证

上面虽然上传文件成功，但是并未对文件进行验证，存在很大的风险。

进行文件验证测试：
新建forms.py如下：

```python
from wtforms import Form, FileField, StringField
from wtforms.validators import InputRequired
from flask_wtf.file import FileAllowed, FileRequired

class UploadForm(Form):
    image_file = FileField(validators=[FileRequired(message='文件必须上传'), FileAllowed(['jpg', 'png', 'gif'], message='上传文件格式有误')])
    desc = StringField(validators=[InputRequired('必须填写描述')])
1234567
```

修改flask_app.py如下：

```python
from flask import Flask, request,render_template
from werkzeug.utils import secure_filename
from werkzeug.datastructures import CombinedMultiDict
from forms import UploadForm

app = Flask(__name__)
app.config['TEMPLATE_AUTO_RELOAD'] = True

@app.route('/')
def index():
    return '首页'


@app.route('/upload/', methods=['GET', 'POST'])
def upload():
    if request.method == 'GET':
        return render_template('upload.html')
    else:
        form = UploadForm(CombinedMultiDict([request.form, request.files]))
        if form.validate():
            desc = request.form.get('desc')     # 等价于desc = form.desc.data
            image_file = request.files.get('image_file')    # 等价于image_file = form.image_file.data
            file_name = secure_filename(image_file.filename)
            image_file.save('files/images/' + file_name)
            return '文件上传成功--' + desc
        else:
            return '文件上传失败--' + str(form.errors)


if __name__ == '__main__':
    app.run(debug=True)
12345678910111213141516171819202122232425262728293031
```

显示：
![flask wtf upload validate](https://img-blog.csdnimg.cn/20200509205618274.gif#pic_center)
显然，已经实现了文件上传时的验证，其中：

```python
desc = request.form.get('desc')
image_file = request.files.get('image_file')
12
```

等价于

```python
desc = form.desc.data
image_file = form.image_file.data
12
```

可以得到：
定义表单的时候，对文件的字段需要使用**FileField**；
验证器从flask_wtf.file中导入，FileRequired是用来验证文件上传是否为空，FileAllowed用来验证上传的文件的后缀名；
在视图文件中，使用**werkzeug.datastructures**类中的`CombinedMultiDict()`方法将request.form与request.files合并，再传给表单进行验证。

### 3.访问上传资源

想要读取上传的文件时，需要定义一个视图函数，来获取指定的文件。
在这个视图函数中，使用`send_from_directory(文件的目录, 文件名)`来获取。

flask_app.py如下：

```python
from flask import Flask, request, render_template, send_from_directory
from werkzeug.utils import secure_filename
from werkzeug.datastructures import CombinedMultiDict
from forms import UploadForm

app = Flask(__name__)
app.config['TEMPLATE_AUTO_RELOAD'] = True


@app.route('/')
def index():
    return '首页'


@app.route('/upload/', methods=['GET', 'POST'])
def upload():
    if request.method == 'GET':
        return render_template('upload.html')
    else:
        form = UploadForm(CombinedMultiDict([request.form, request.files]))
        if form.validate():
            desc = request.form.get('desc')  # 等价于desc = form.desc.data
            image_file = request.files.get('image_file')  # 等价于image_file = form.image_file.data
            file_name = secure_filename(image_file.filename)
            image_file.save('files/images/' + file_name)
            return '文件上传成功--' + desc
        else:
            return '文件上传失败--' + str(form.errors)


@app.route('/images/<filename>')
def get_image(filename):
    return send_from_directory('files/images/', filename)


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526272829303132333435363738
```

显示：
![flask wtf upload access resources](https://img-blog.csdnimg.cn/20200509205638166.gif#pic_center)
很明显，成功访问到了资源。

# Python全栈（七）Flask框架之12.Cookie、Session、上下文和钩子函数

## 一、Cookie的使用

### 1.Cookie的基本概念

在网站中，http请求是无状态的，也就是说即使第一次和服务器连接后并且登录成功后，第二次请求服务器依然不能知道当前请求是哪个用户。
cookie的出现就是为了解决这个问题，类型为**小型文本文件**，是某些网站为了辨别用户身份，进行Session跟踪而储存在用户本地终端上的数据（通常经过加密），由用户客户端计算机暂时或永久保存的信息。
第一次登录后服务器返回一些数据（cookie）给浏览器，然后浏览器保存在本地，当该用户发送第二次请求的时候，就会自动将上次请求存储的cookie数据自动传送给服务器，服务器通过浏览器携带的数据来判断用户。
cookie存储的数据量有限，不同的浏览器有不同的存储大小，但一般不超过4KB，因此使用cookie只能存储一些小量的数据。

### 2.Flask中使用Cookie

在Flask中操作cookie，是通过**Response对象**来操作，可以在response返回之前，通过`response.set_cookie()`方法来设置，这个方法有以下几个参数：

- key
  cookie的键
- value
  cookie的键对应的值。
- max_age
  cookie的过期时间，如果不设置，则浏览器关闭后就会自动过期。
- expires
  过期时间，时间戳的形式（1970到现在的时间）。
- domain
  该cookie在哪个域名中有效，一般设置子域名，比如cms.example.com。
- path
  该cookie在哪个路径下有效，即当前主域名。

cookie简单测试：
flask_app.py如下：

```python
from flask import Flask, Response

app = Flask(__name__)
app.config['TEMPLATE_AUTO_RELOAD'] = True


@app.route('/')
def index():
    return '首页'


@app.route('/cookietest/')
def cookietest():
    res = Response('Hello World!!')
    res.set_cookie('username', value='corley', max_age=3)
    return res


if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021
```

显示：

![flask cookie simple use](https://img-blog.csdnimg.cn/20200509205659537.gif#pic_center)

## 二、Flask-session的介绍和使用

### 1.Session的基本概念

session和cookie的作用有点类似，都是为了存储用户相关的信息；
session可以类比字典，每个键对应着指定的值。
不同的是，cookie存储在**本地浏览器**，session是一个思路、一个概念、一个服务器存储授权信息的解决方案，不同的服务器、不同的框架、不同的语言有不同的实现。虽然实现不一样，但是目的都是为了**在服务器**更方便地存储数据。
session是为了解决cookie存储数据不安全的问题。

在现实中，cookie和session是可以结合使用的，有两种方式：

- 存储在服务端
  通过cookie存储一个session_id，具体的数据则是保存在session中。如果用户已经登录，则服务器会在cookie中保存一个session_id，下次再次请求的时候，会携带该session_id，服务器根据session_id在session库中获取用户的session数据，就能判断该用户到底是谁，以及之前保存的一些状态信息，这称为**server side session**。
  数据存储在服务器会更加安全，不容易被窃取，包括Django在内的很多框架都是采用的这种形式。
  但存储在服务器也有一定的弊端，就是会**占用一定的服务器资源**，但现在服务器存储量一般较大，存储session信息是不成问题的。
- 存储在客户端
  将session数据加密，然后存储在cookie中，这种称为**client side session**。
  flask采用的就是这种方式，但是也可以替换成其他形式。

### 2.Flask中使用Session

Flask中中使用session需要先通过`from flask import session`语句导入，再添加key和value。
Flask中的session机制是将session信息加密，然后存储在cookie中，因此需要通过配置信息`app.config['SECRET_KEY']`设置加密密钥。

session的基本使用测试如下：

```python
from flask import Flask, session
import os

app = Flask(__name__)
app.config['SECRET_KEY'] = os.urandom(24)   # os.urandom()生成指定字节数的随机字符串


@app.route('/')
def index():
    session['username'] = 'Corley'
    session['user_id'] = 1
    return '这是首页'


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617
```

显示：
![flask session use](https://img-blog.csdnimg.cn/20200513182234330.gif#pic_center)
显然，上传了session的一些信息；
其中，`os.urandom()`用于生成指定字节数的随机字符串，用于表示密钥；
同时可以看到，到期时间是浏览器会话结束时。

可以指定会话到期时间，如下：

```python
from flask import Flask, session
from datetime import timedelta
import os

app = Flask(__name__)
app.config['SECRET_KEY'] = os.urandom(24)
# 自定义过期时间
app.config['PERMANENT_SESSION_LIFETIME'] = timedelta(hours=2)


@app.route('/')
def index():
    session['username'] = 'Corley'
    session['user_id'] = 1
    # 设置持久化
    session.permanent = True
    return '这是首页'


if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819202122
```

显示：
![flask session use set time](https://img-blog.csdnimg.cn/20200513182255153.gif#pic_center)
显然，此时session的过期时间为2个小时。
如果想自定义过期时间，需要进行两方面配置：
（1）设置持久化

```python
session.permanent = True
1
```

设置了持久化后，默认的过期时间为1个月，如果想要自定义过期时间，还需要进行第二步配置。
（2）自定义过期时间

```python
app.config['PERMANENT_SESSION_LIFETIME'] = timedelta(hours=2)
1
```

还可以获取session值，测试如下：

```python
from flask import Flask, session
from datetime import timedelta
import os

app = Flask(__name__)
app.config['SECRET_KEY'] = os.urandom(24)
app.config['PERMANENT_SESSION_LIFETIME'] = timedelta(hours=2)


@app.route('/')
def index():
    username = session.get('username')
    return '这是首页' + str(username)


@app.route('/login/')
def login():
    session['username'] = 'Corley'
    session['user_id'] = 1
    session.permanent = True
    return '登录页面'


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526
```

显示：
![flask session use get value](https://img-blog.csdnimg.cn/20200513182407193.gif#pic_center)
显然，获取到了session的信息。

可以删除session，如下：

```python
from flask import Flask, session
from datetime import timedelta
import os

app = Flask(__name__)
app.config['SECRET_KEY'] = os.urandom(24)
app.config['PERMANENT_SESSION_LIFETIME'] = timedelta(hours=2)


@app.route('/')
def index():
    username = session.get('username')
    return '这是首页' + str(username)


@app.route('/login/')
def login():
    session['username'] = 'Corley'
    session['user_id'] = 1
    session.permanent = True
    return '登录页面'

@app.route('/logout/')
def logout():
    # 删除session
    session.clear()
    return '退出登录'+ str(session.get('username'))

if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819202122232425262728293031
```

显示：
![flask session use delete](https://img-blog.csdnimg.cn/20200513182430341.gif#pic_center)
显然，在访问http://127.0.0.1:5000/logout/后，session已经清空；
除了`session.clear()`，还可以指定key删除session，如删除username可以用`session.pop('username')`。

## 三、Flask上下文

Flask项目中上下文包括两类：

- 应用上下文
  用于记录和应用相关的数据，包括current_app和g。
- 请求上下文
  用于记录和请求相关的数据，包括request和session。

请求上下文request和应用上下文current_app都是全局变量，所有请求都是**共享**的。
Flask中使用特殊的机制来保证每次请求的数据都是**隔离**的，即用户A请求所产生的数据不会影响到用户B的请求，所以直接导入request对象也不会被一些脏数据影响，并且不需要像Django一样在每个函数中使用request的时候传入request参数。
4类上下文对象如下：

- request
  请求上下文对象，一般用来保存一些请求的变量，如method、args、form等。
- session
  请求上下文对象，一般用来保存一些会话信息。
- current_app
  应用上下文对象，返回当前的app。
- g
  应用上下文对象，处理请求时用于临时存储的对象。

**current_app**使用示意如下：

```python
from flask import Flask, current_app

app = Flask(__name__)


@app.route('/')
def index():
    return '这是首页'


@app.route('/login/')
def login():
    ca = current_app.name
    cfg = current_app.config['DEBUG']
    return '登录页面：' + ca + '-' + str(cfg)


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920
```

显示：
![flask context current_app use](https://img-blog.csdnimg.cn/20200513182506385.gif#pic_center)
显然，得到了当前所处的app，即初始化Flask对象时传入的参数`__name__`，也就是当前Python文件名；
除了获取当前app名称，还可以获取绑定到app中的配置参数。

注意：current_app**只能在视图函数中使用**，在视图函数之外使用会报错。

一般情况测试：
新建utils.py如下：

```python
'''
工具文件
'''


def log_a(username):
    print("Log A %s" % username)


def log_b(username):
    print("Log B %s" % username)

123456789101112
```

主程序flask_context.py如下：

```python
from flask import Flask, session
from utils import log_a, log_b
import os

app = Flask(__name__)
app.config['SECRET_KEY'] = os.urandom(24)


@app.route('/')
def index():
    username = session.get('username')
    log_a(username)
    log_b(username)
    return '这是首页'


@app.route('/login/')
def login():
    session['username'] = 'Corley'
    session['user_id'] = 1
    return '登录页面'


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526
```

运行后，先访问http://127.0.0.1:5000/，再访问http://127.0.0.1:5000/login/，再访问http://127.0.0.1:5000/，控制台打印如下：

```python
127.0.0.1 - - [13/May/2020 11:57:59] "GET / HTTP/1.1" 200 -
Log A None
Log B None
127.0.0.1 - - [13/May/2020 11:58:14] "GET /login/ HTTP/1.1" 200 -
127.0.0.1 - - [13/May/2020 11:58:23] "GET / HTTP/1.1" 200 -
Log A Corley
Log B Corley

12345678
```

显然，在访问登录页面之后，控制台打印出了username信息。

此时使用g对象进行改进：
主程序文件修改如下：

```python
from flask import Flask, current_app, g, session
from utils import log_a, log_b
import os

app = Flask(__name__)
app.config['SECRET_KEY'] = os.urandom(24)


@app.route('/')
def index():
    g.username = session.get('username')
    log_a()
    log_b()
    return '这是首页'


@app.route('/login/')
def login():
    session['username'] = 'Corley'
    session['user_id'] = 1
    return '登录页面'


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526
```

utils.py修改如下：

```python
'''
工具文件
'''
from flask import g

def log_a():
    print("Log A %s" % g.username)


def log_b():
    print("Log B %s" % g.username)

123456789101112
```

运行之后，按之前的顺序访问，控制台打印：

```python
127.0.0.1 - - [13/May/2020 12:04:34] "GET / HTTP/1.1" 200 -
Log A None
Log B None
127.0.0.1 - - [13/May/2020 12:04:42] "GET /login/ HTTP/1.1" 200 -
127.0.0.1 - - [13/May/2020 12:04:52] "GET / HTTP/1.1" 200 -
Log A Corley
Log B Corley

12345678
```

显然，达到了同样的效果，并且此时不需要再在`log_a()`和`log_b()`函数中传递参数；
每次刷新页面后，g对象都会被重置，是临时存储的对象；
g对象一般用于解决频繁传参的问题。

可以在一个视图函数中实现调用另一个函数，如下：

```python
from flask import Flask, current_app, g, session
from utils import log_a, log_b
import os

app = Flask(__name__)
app.config['SECRET_KEY'] = os.urandom(24)


@app.route('/')
def index():
    g.username = session.get('username')
    log_a()
    log_b()
    hello()
    return '这是首页'


def hello():
    print('Hello %s' % g.username)


@app.route('/login/')
def login():
    session['username'] = 'Corley'
    session['user_id'] = 1
    return '登录页面'


if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819202122232425262728293031
```

按之前的顺序访问，控制台打印如下：

```python
127.0.0.1 - - [13/May/2020 12:13:41] "GET / HTTP/1.1" 200 -
Log A None
Log B None
Hello None
127.0.0.1 - - [13/May/2020 12:13:48] "GET /login/ HTTP/1.1" 200 -
127.0.0.1 - - [13/May/2020 12:13:52] "GET / HTTP/1.1" 200 -
Log A Corley
Log B Corley
Hello Corley

12345678910
```

## 四、常用的钩子函数

钩子函数是指在执行函数和目标函数之间挂载的函数，框架开发者给调用方提供一个point即挂载点，至于挂载什么函数由调用方决定， 从而大大提高了**灵活性**。

### 1.before_first_request

第一次请求之前执行。
练习如下：

```python
from flask import Flask

app = Flask(__name__)


@app.route('/')
def index():
    print('这是首页')
    return '这是首页'


@app.before_first_request
def handle_first_request():
    print('在第一次请求之前执行')


if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819
```

运行后，访问http://127.0.0.1:5000/，控制台打印：

```python
127.0.0.1 - - [13/May/2020 12:39:01] "GET / HTTP/1.1" 200 -
在第一次请求之前执行
这是首页
123
```

### 2.before_request

在每次请求之前执行，通常可以用这个装饰器来给视图函数增加一些变量。

练习如下：

```python
from flask import Flask

app = Flask(__name__)


@app.route('/')
def index():
    print('这是首页')
    return '这是首页'


@app.before_first_request
def handle_first_request():
    print('在第一次请求之前执行')


@app.before_request
def handle_before():
    print("在每一次请求之前执行")


if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021222324
```

运行后，多次访问http://127.0.0.1:5000/，控制台打印：

```python
在第一次请求之前执行
在每一次请求之前执行
这是首页
127.0.0.1 - - [13/May/2020 12:41:36] "GET / HTTP/1.1" 200 -
在每一次请求之前执行
这是首页
127.0.0.1 - - [13/May/2020 12:41:38] "GET / HTTP/1.1" 200 -
在每一次请求之前执行
这是首页
127.0.0.1 - - [13/May/2020 12:41:39] "GET / HTTP/1.1" 200 -
在每一次请求之前执行
这是首页
127.0.0.1 - - [13/May/2020 12:41:40] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [13/May/2020 12:41:41] "GET / HTTP/1.1" 200 -
在每一次请求之前执行
这是首页

1234567891011121314151617
```

### 3.after_request

在每次请求之后执行。

练习如下：

```python
from flask import Flask

app = Flask(__name__)


@app.route('/')
def index():
    print('这是首页')
    return '这是首页'


@app.before_first_request
def handle_first_request():
    print('在第一次请求之前执行')


@app.before_request
def handle_before():
    print("在每一次请求之前执行")


@app.after_request
def handle_after(response):
    print("在每一次请求之后执行")
    return response


if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021222324252627282930
```

运行后，多次访问http://127.0.0.1:5000/，控制台打印：

```python
127.0.0.1 - - [13/May/2020 12:51:05] "GET / HTTP/1.1" 200 -
在第一次请求之前执行
在每一次请求之前执行
这是首页
在每一次请求之后执行
在每一次请求之前执行
这是首页
在每一次请求之后执行
127.0.0.1 - - [13/May/2020 12:51:06] "GET / HTTP/1.1" 200 -
在每一次请求之前执行
这是首页
在每一次请求之后执行
127.0.0.1 - - [13/May/2020 12:51:07] "GET / HTTP/1.1" 200 -
在每一次请求之前执行
这是首页
在每一次请求之后执行
127.0.0.1 - - [13/May/2020 12:51:08] "GET / HTTP/1.1" 200 -
在每一次请求之前执行
这是首页
在每一次请求之后执行
127.0.0.1 - - [13/May/2020 12:51:09] "GET / HTTP/1.1" 200 -

12345678910111213141516171819202122
```

### 4.teardown_appcontext

不管是否有异常，注册的函数都会在每次请求之后执行。

练习如下：

```python
from flask import Flask

app = Flask(__name__)


@app.route('/')
def index():
    1/0
    print('这是首页')
    return '这是首页'


@app.before_first_request
def handle_first_request():
    print('在第一次请求之前执行')


@app.before_request
def handle_before():
    print("在每一次请求之前执行")


@app.after_request
def handle_after(response):
    print("在每一次请求之后执行")
    return response


@app.teardown_appcontext
def handle_teardown(response):
    print('teardown被执行')
    return response


if __name__ == '__main__':
    app.run()

12345678910111213141516171819202122232425262728293031323334353637
```

运行后，访问http://127.0.0.1:5000/，显示：
![flask hook teardown_appcontext](https://img-blog.csdnimg.cn/20200513182634216.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
控制台打印：

```python
在第一次请求之前执行
在每一次请求之前执行
在每一次请求之后执行
teardown被执行
[2020-05-13 12:58:48,206] ERROR in app: Exception on / [GET]
Traceback (most recent call last):
  File "XXX\Test-gftU5mTd\lib\site-packages\flask\app.py", line 2447, in wsgi_app
    response = self.full_dispatch_request()
  File "XXX\Test-gftU5mTd\lib\site-packages\flask\app.py", line 1952, in full_dispatch_request
    rv = self.handle_user_exception(e)
  File "XXX\Test-gftU5mTd\lib\site-packages\flask\app.py", line 1821, in handle_user_exception
    reraise(exc_type, exc_value, tb)
  File "XXX\Test-gftU5mTd\lib\site-packages\flask\_compat.py", line 39, in reraise
    raise value
  File "XXX\Test-gftU5mTd\lib\site-packages\flask\app.py", line 1950, in full_dispatch_request
    rv = self.dispatch_request()
  File "XXX\Test-gftU5mTd\lib\site-packages\flask\app.py", line 1936, in dispatch_request
    return self.view_functions[rule.endpoint](**req.view_args)
  File "XXXX\flask_hook.py", line 8, in index
    1/0
ZeroDivisionError: division by zero
127.0.0.1 - - [13/May/2020 12:58:48] "GET / HTTP/1.1" 500 -

1234567891011121314151617181920212223
```

显然， 在出现异常的情况下也执行了`handle_teardown()`函数；
虚造关闭Debug模式才能有明显的现象，在Debug模式下是不能执行`handle_teardown()`函数的。

### 5.context_processor

上下文处理器，返回的字典中的键可以在模板上下文中使用。

假如在多个视图函数中渲染模板时都需要传入同样的参数，flask_hook.py如下：

```python
from flask import Flask, render_template

app = Flask(__name__)


@app.route('/')
def index():
    return render_template('index.html', username='Corley')


@app.route('/list/')
def list():
    return render_template('list.html', username='Corley')


if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718
```

在模板目录templates中新建index.html和list.html，index.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
    <p>这是首页</p>
    <p>{{ username }}</p>
</body>
</html>
1234567891011
```

list.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>列表</title>
</head>
<body>
    <p>这是列表</p>
    <p>{{ username }}</p>
</body>
</html>
1234567891011
```

显示：
![flask hook nocontext_processor](https://img-blog.csdnimg.cn/20200513182710804.gif#pic_center)
显然，达到了效果，但是在每个视图函数中都要传入参数，很麻烦冗余，可以用context_processor装饰器定义钩子函数传递公共参数：

```python
from flask import Flask, render_template

app = Flask(__name__)


@app.route('/')
def index():
    return render_template('index.html')


@app.route('/list/')
def list():
    return render_template('list.html')


@app.context_processor
def context_process():
    return {'username': 'Corley'}


if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223
```

效果与之前一样；
此时在每个视图函数中不需要再传递参数，而是在context_processor装饰器定义的钩子函数中返回参数。

### 6.errorhandler

errorhandler接收状态码作为参数，可以自定义处理返回这个状态码的响应的方法。

测试如下：

```python
from flask import Flask, render_template

app = Flask(__name__)


@app.route('/')
def index():
    1/0
    return render_template('index.html')


@app.route('/list/')
def list():
    return render_template('list.html')


@app.context_processor
def context_process():
    return {'username': 'Corley'}


@app.errorhandler(404)
def page_not_found(error):
    return render_template('404.html'), 404


@app.errorhandler(500)
def server_error(error):
    return '<p>服务器内部错误...</p>', 500


if __name__ == '__main__':
    app.run()

12345678910111213141516171819202122232425262728293031323334
```

新建404.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>页面404</title>
</head>
<body>
    <p>页面跑到火星去了...</p>
</body>
</html>
12345678910
```

显示：
![flask hook errorhandler](https://img-blog.csdnimg.cn/20200513183033677.gif#pic_center)
显然，捕获到了404和500错误。

还可以在视图函数中用`abort()`方法主动抛出异常，如下：

```python
from flask import Flask, render_template, abort

app = Flask(__name__)


@app.route('/')
def index():    
    return render_template('index.html')


@app.route('/list/')
def list():
    abort(404)
    return render_template('list.html')


@app.context_processor
def context_process():
    return {'username': 'Corley'}


@app.errorhandler(404)
def page_not_found(error):
    return render_template('404.html'), 404


@app.errorhandler(500)
def server_error(error):
    return '<p>服务器内部错误...</p>', 500


if __name__ == '__main__':
    app.run()

12345678910111213141516171819202122232425262728293031323334
```

显示：
![flask hook errorhandler abort](https://img-blog.csdnimg.cn/202005131830507.gif#pic_center)
显然，此时再访问http://127.0.0.1:5000/list/，就会报错；
因为在`list()`视图函数中使用`sort()`方法抛出404错误，被钩子函数`page_not_found()`捕获到，因此访问会出现404错误。

# Python全栈（七）Flask框架之13.Flask-Restful的概念和使用

## 一、Restful API规范

**Restful API**是用于在前端与后台进行通信的一套规范，使用这个规范可以让前后端开发变得更加简单。

### 1.协议

采用http或者https协议。

### 2.数据传输格式

数据之间传输的格式应该使用**json**形式，而不是xml。

### 3.url链接

url链接中不能有动词，只能有名词；
对于一些名词，如果出现复数，应该在后面加s。

### 4.HTTP请求方法

| 方法   | 含义                                             | 举例                                                        |
| ------ | ------------------------------------------------ | ----------------------------------------------------------- |
| GET    | 从服务器上获取资源                               | `/users/`获取所有用户                                       |
| …      | …                                                | `/user/id/`根据id获取一个用户                               |
| POST   | 在服务器上新创建一个资源                         | `/user/`新建一个用户                                        |
| PUT    | 在服务器上更新资源（客户端提供所有改变后的数据） | `/user/id/`更新某个id的用户的信息（需要提供用户的所有信息） |
| PATCH  | 在服务器上更新资源（客户端只提供需要改变的属性） | `/user/id/`更新某个id的用户信息（只需要提供需要改变的信息） |
| DELETE | 从服务器上删除资源                               | `/user/id/`删除一个用户                                     |

#### 状态码

| 状态码 | 描述                  | 含义                                                         |
| ------ | --------------------- | ------------------------------------------------------------ |
| 200    | OK                    | 服务器成功响应客户端的请求                                   |
| 400    | INVALID REQUEST       | 用户发出的请求有错误，服务器没有进行新建或修改数据的操作     |
| 401    | Unauthorized          | 用户没有权限访问这个请求                                     |
| 403    | Forbidden             | 因为某些原因禁止访问这个请求                                 |
| 404    | NOT FOUND             | 用户发送的请求的url不存在                                    |
| 406    | NOT Acceptable        | 用户请求不被服务器接收（比如服务器期望客户端发送某个字段，但是没有发送） |
| 500    | Internal server error | 服务器内部错误，比如出现了bug                                |

## 二、Flask-Restful插件的基本使用

### 1.基本概念

Flask-Restful是Flask中专门用来实现Restful API的插件，使用它可以**快速集成**Restful API功能。
在app的后台以及纯API的后台中，这个插件可以帮助我们节省很多时间；
在普通的网站中，这个插件显得有些鸡肋，因为在普通的网页开发中，是需要去渲染HTML代码的，而Flask-Restful在每个请求中都返回json格式的数据。

### 2.安装

Flask-Restful的环境要求：

- Flask版本>=0.8
- Python版本为2.6、2.7，或3.3及以上。

在虚拟环境中使用`pip install flask-restful`命令安装。

### 3.基本使用

使用Flask-Restful定义视图类时，要继承自`flask_restful.Resource`类，再根据具体的请求的方法来定义相应的方法。
例如，如果期望用户在客户端中使用get方法发送请求，那么就定义一个get方法，类似于MethodView。
简单使用测试如下：

```python
from flask import Flask
from flask_restful import Api, Resource

app = Flask(__name__)
api = Api(app)


class IndexView(Resource):
    def get(self):
        return {'username': 'Corley'}

    def post(self):
        return {'info': 'Login Successfully!!'}


api.add_resource(IndexView, '/', endpoint='index')

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920
```

运行之后，使用Postman模拟请求如下：
![flask restful simple use](https://img-blog.csdnimg.cn/20200513183107271.gif#pic_center)
显然，用get方法和post方法请求得到的结果是不一样的。
说明：

- endpoint用来指定使用`url_for()`方法反转url时传入的参数；如果不设置endpoint参数，会使用视图的名字的小写作为endpoint的值。
- `add_resource()`方法的第二个参数是视图函数的路由地址，这个地址与视图函数的route一样，可以传递参数；有一点不同的是，这个方法可以传递多个url来指定视图函数。

## 三、Restful参数解析

Flask-Restful插件提供了类似WTForms来验证提交的数据是否合法的包，即`reqparse`。
RequestParser对象的`add_argument()`方法可以指定字段的名字、数据类型等，常见的参数有：

- default
  默认值，如果参数没有值，那么将使用这个参数指定的值。
- required
  是否必须。
  默认为False；如果设置为True，则必须提交这个参数。
- type
  参数的数据类型，如果指定，那么将使用指定的数据类型来强制转换提交得到的值。
- choices
  选项，提交上来的值只有满足选项中的值才符合验证通过，否则验证不通过。
- help
  错误信息，如果验证失败后，将会使用help参数指定的值作为错误信息。
- trim
  是否要去掉前后的空格。

练习如下：

```python
from flask import Flask
from flask_restful import Api, Resource, reqparse, inputs

app = Flask(__name__)
api = Api(app)


class IndexView(Resource):
    def get(self):
        return {'username': 'Corley'}

    def post(self):
        parse = reqparse.RequestParser()
        parse.add_argument('username', type=str, help='用户名验证错误', required=True)
        parse.add_argument('password', type=str, help='密码验证错误', trim=True)
        parse.add_argument('age', type=int, help='年龄错误')
        parse.add_argument('gender', type=str, help='性别错误', choices=['male', 'female', 'secret'], default='secret')
        parse.add_argument('blog', type=inputs.url, help='博客地址错误')
        parse.add_argument('phone', type=inputs.regex(r'1[35789]\d{9}'), help='电话号码错误')

        args = parse.parse_args()
        return {'info': 'Register Successfully!!', 'args': args}


api.add_resource(IndexView, '/', endpoint='index')

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526272829
```

显示：
![flask restful form validate](https://img-blog.csdnimg.cn/20200518174514973.gif#pic_center)

## 四、Restful高级使用

### 1.输出字段

对于一个视图类，可以预设好一些字段用于返回，以后使用ORM模型或者自定义模型的时候，会**自动获取**模型中相应的字段，生成json数据，再返回。
这需要导入`flask_restful.marshal_with`装饰器，在视图类中定义一个字典来指定需要返回的字段，以及该字段的数据类型。

测试如下：

```python
from flask import Flask
from flask_restful import Api, Resource, reqparse, inputs, fields, marshal_with

app = Flask(__name__)
api = Api(app)


class IndexView(Resource):
    def get(self):
        return {'username': 'Corley'}

    def post(self):
        parse = reqparse.RequestParser()
        parse.add_argument('username', type=str, help='用户名验证错误', required=True)
        parse.add_argument('password', type=str, help='密码验证错误', trim=True)
        parse.add_argument('age', type=int, help='年龄错误')
        parse.add_argument('gender', type=str, help='性别错误', choices=['male', 'female', 'secret'], default='secret')
        parse.add_argument('blog', type=inputs.url, help='博客地址错误')
        parse.add_argument('phone', type=inputs.regex(r'1[35789]\d{9}'), help='电话号码错误')

        args = parse.parse_args()
        return {'info': 'Register Successfully!!', 'args': args}


class ArticleView(Resource):
    resource_fields = {
        'title': fields.String,
        'content': fields.String
    }

    @marshal_with(resource_fields)
    def get(self):
        return {'title': 'Python Flask'}


api.add_resource(IndexView, '/', endpoint='index')
api.add_resource(ArticleView, '/article/', endpoint='article')

if __name__ == '__main__':
    app.run(debug=True)

1234567891011121314151617181920212223242526272829303132333435363738394041
```

显示：
![flask restful output field](https://img-blog.csdnimg.cn/20200518174540398.gif#pic_center)

显然，get方法使用了`marshal_with`装饰器后，即使返回时未定义content字段，在网页中也会渲染该字段。

还可以在get方法中直接返回数据库的查询结果，示例如下：

```python
class ArticleView(Resource):
    resource_fields = {
        'title': fields.String,
        'content': fields.String,
        'tags': fields.String
    }

    @marshal_with(resource_fields)
    def get(self, article_id):
    	article = Article.query.get(article_id)
        return article
1234567891011
```

在get方法中返回article的时候，flask_restful会自动读取Article模型上的title、content和tags属性，组装成json格式的字符串返回给客户端。

### 2.复杂结构与格式化输出

有时候想要在返回的数据格式中，形成复杂的结构、美观的格式，此时可以使用一些特殊的字段来实现。
要在一个字段中放置一个**列表**，可以使用`fields.List`；
在一个字段下面又是一个**字典**，可以使用`fields.Nested`。

练习如下：
在当前项目目录下创建主程序文件flask_restful_test.py如下：

```python
from flask import Flask
from flask_restful import Api, Resource, fields, marshal_with
import config
from exts import db
from models import Article

app = Flask(__name__)
api = Api(app)
app.config.from_object(config)
db.init_app(app)


@app.route('/')
def index():
    return '这是首页'


class ArticleView(Resource):
    resource_fields = {
        'title': fields.String,
        'content': fields.String,
        'author': fields.Nested({
            'username': fields.String,
            'email': fields.String
        }),
        'tags': fields.List({
            'id': fields.Integer,
            'name': fields.String
        })
    }

    @marshal_with(resource_fields)
    def get(self, article_id):
        article = Article.query.get(article_id=article_id)
        return article


api.add_resource(ArticleView, '/article/<article_id>', endpoint='article')

if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021222324252627282930313233343536373839404142
```

创建配置文件config.py如下：

```python
# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_restful'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

SQLALCHEMY_DATABASE_URI = DB_URL
SQLALCHEMY_TRACK_MODIFICATIONS = False

1234567891011
```

创建模型文件models.py如下：

```python
from exts import db


class User(db.Model):
    __tablename__ = 'user'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    username = db.Column(db.String(30))
    email = db.Column(db.String(50))


article_tag_table = db.Table(
    'article_tag',
    db.Column('article_id', db.Integer, db.ForeignKey('article.id')),
    db.Column('tag_id', db.Integer, db.ForeignKey('tag.id'))
)


class Article(db.Model):
    __tablename__ = 'article'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    title = db.Column(db.String(30))
    content = db.Column(db.String(500))
    author_id = db.Column(db.Integer, db.ForeignKey('user.id'))
    author = db.relationship('User', backref='articles')
    tags = db.relationship('Tag', secondary=article_tag_table, backref='tags')


class Tag(db.Model):
    __tablename__ = 'tag'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(30))

1234567891011121314151617181920212223242526272829303132
```

创建中间文件exts.py如下：

```python
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()
123
```

创建数据库迁移映射文件manage.py如下：

```python
from flask_script import Manager
from flask_restful_test import app
from flask_migrate import Migrate, MigrateCommand
from exts import db
import models

manager = Manager(app)
Migrate(app, db)

manager.add_command('db', MigrateCommand)

if __name__ == '__main__':
    manager.run()

1234567891011121314
```

在命令行当前目录下依次执行`python manage.py db init`、`python manage.py db migrate`、`python manage.py db upgrade`，执行成功后可以在flask_restful数据库中看到建立了四个表，为了方便，手动插入数据如下：

```sql
mysql> select * from user;                          
+----+----------+-------------+                     
| id | username | email       |                     
+----+----------+-------------+                     
|  1 | Corley   | 123@163.com |                     
+----+----------+-------------+                     
1 row in set (0.00 sec)                             
                                                    
mysql> select * from article;                       
+----+--------------+------------------+-----------+
| id | title        | content          | author_id |
+----+--------------+------------------+-----------+
|  1 | Python Flask | Migrate, Restful |         1 |
+----+--------------+------------------+-----------+
1 row in set (0.00 sec)                             
                                                    
mysql> select * from tag;                           
+----+-------------+                                
| id | name        |                                
+----+-------------+                                
|  1 | Python      |                                
|  2 | Programming |                                
+----+-------------+                                
2 rows in set (0.00 sec)                            
                                                    
mysql> select * from article_tag;                   
+------------+--------+                             
| article_id | tag_id |                             
+------------+--------+                             
|          1 |      1 |                             
|          1 |      2 |                             
+------------+--------+                             
2 rows in set (0.01 sec)                            
                                                    
12345678910111213141516171819202122232425262728293031323334
```

运行主程序后，访问http://127.0.0.1:5000/article/1，可以看到：
![flask restful complex json](https://img-blog.csdnimg.cn/20200518174612647.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然， 得到了美化的json格式的数据。

### 3.重命名属性

很多时候面向用户的字段名称是不同于内部的属性名，此时使用`attribute`参数可以配置这种映射。
比如想要在返回article的id时表现的形式是`article_id`，可以写成`'article_title': fields.String(attribute='title')`。

主程序文件中属性重命名示例如下：

```python
from flask import Flask
from flask_restful import Api, Resource, fields, marshal_with
import config
from exts import db
from models import Article

app = Flask(__name__)
api = Api(app)
app.config.from_object(config)
db.init_app(app)


@app.route('/')
def index():
    return '这是首页'


class ArticleView(Resource):
    resource_fields = {
        'article_title': fields.String(attribute='title'),
        'article_content': fields.String(attribute='content'),
        'article_author': fields.Nested({
            'username': fields.String,
            'email': fields.String
        }, attribute='author'),
        'article_tags': fields.List(fields.Nested({
            'id': fields.Integer,
            'name': fields.String
        }), attribute='tags')
    }

    @marshal_with(resource_fields)
    def get(self, article_id):
        article = Article.query.get(article_id)
        return article


api.add_resource(ArticleView, '/article/<article_id>', endpoint='article')

if __name__ == '__main__':
    app.run(debug=True)

123456789101112131415161718192021222324252627282930313233343536373839404142
```

再访问http://127.0.0.1:5000/article/1，看到：
![flask restful complex json rename](https://img-blog.csdnimg.cn/20200518174714190.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然， 显示的属性是重命名之后的属性。

### 4.指定默认值

在返回一些字段的时候，有时候可能没有值，此时可以在指定fields时使用default参数给定一个**默认值**。

修改主程序文件实现指定默认值示例如下：

```python
from flask import Flask
from flask_restful import Api, Resource, fields, marshal_with
import config
from exts import db
from models import Article

app = Flask(__name__)
api = Api(app)
app.config.from_object(config)
db.init_app(app)


@app.route('/')
def index():
    return '这是首页'


class ArticleView(Resource):
    resource_fields = {
        'article_title': fields.String(attribute='title'),
        'article_content': fields.String(attribute='content'),
        'article_author': fields.Nested({
            'username': fields.String,
            'email': fields.String
        }, attribute='author'),
        'article_tags': fields.List(fields.Nested({
            'id': fields.Integer,
            'name': fields.String
        }), attribute='tags'),
        'read_count': fields.Integer(default=0)
    }

    @marshal_with(resource_fields)
    def get(self, article_id):
        article = Article.query.get(article_id)
        return article


api.add_resource(ArticleView, '/article/<article_id>', endpoint='article')

if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516171819202122232425262728293031323334353637383940414243
```

再访问http://127.0.0.1:5000/article/1，看到：
![flask restful complex json default](https://img-blog.csdnimg.cn/20200518174732979.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然， 增加了文章的默认阅读量为0，返回的页面中也将其渲染出来。
在给模型中存在的字段设置默认值时，如果数据库中对应记录的该字段的值存在，则页面渲染数据库中的值；
如果数据库中该字段的值不存在即为空时，则渲染给定的默认值。