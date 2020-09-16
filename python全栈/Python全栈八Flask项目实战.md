# Python全栈（八）Flask项目实战之1.项目搭建

Flask项目实战是做一个简单的论坛平台，实现基本功能。

Github和Gitee代码**同步更新**：
https://github.com/PythonWebProject/Flask_BBS；
https://gitee.com/Python_Web_Project/Flask_BBS。

## 一、项目目录创建

在真实项目中，实现前台front和后台cms**分离实现**，以优化整个项目的代码结构。
整个项目默认使用PyCHarm进行开发。

创建用户目录**Flask_BBS**，该项目所有的文件均保存在该目录中。

先创建**程序主入口文件**bbs.py如下：

```python
'''
前台 front
后台 cms
公有 common
'''

from flask import Flask
from exts import db
from apps.cms.views import cms_bp
from apps.front.views import front_bp
import config


app = Flask(__name__)
app.config.from_object(config)
db.init_app(app)

app.register_blueprint(cms_bp)
app.register_blueprint(front_bp)


if __name__ == '__main__':
    app.run(debug=True)
1234567891011121314151617181920212223
```

再创建**静态资源文件**保存目录static和**模板**保存目录templates。

再创建**配置文件**config.py如下：

```python
# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_bbs'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

SQLALCHEMY_DATABASE_URI = DB_URL
SQLALCHEMY_TRACK_MODIFICATIONS = False

1234567891011
```

再创建**中间文件**exts.py如下：

```python
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()
123
```

再创建**数据库映射迁移管理文件**manage.py如下：

```python
from flask_script import Manager
from bbs import app
from flask_migrate import Migrate, MigrateCommand
from exts import db

manager = Manager(app)
Migrate(app, db)

manager.add_command('db', MigrateCommand)

if __name__ == '__main__':
    manager.run()

12345678910111213
```

在项目目录右键新建**Python Package**为**apps**，下面新建Python包cms、front和common用于保存**后台**、**前台**和**公有文件**。

在cms和front目录下均创建**表单文件**forms.py、**模型文件**models.py和**视图文件**views.py文件。

cms目录下的views.py如下：

```python
from flask import Blueprint

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

@cms_bp.route('/')
def index():
    return '后台管理首页'
1234567
```

front目录下的views.py如下：

```python
from flask import Blueprint

front_bp = Blueprint('front', __name__)

@front_bp.route('/')
def index():
    return '前台首页'
1234567
```

运行主程序后，显示：
![flask bbs directory build](https://img-blog.csdnimg.cn/20200518214019471.gif#pic_center)
如果出现类似的效果，则项目目录基本构建完成。

## 二、CMS模型定义和用户添加

### 1.CMS管理员用户模型定义

cms目录下的models.py如下：

```python
from exts import db
from datetime import datetime

class CMSUser(db.Model):
    '''后台管理员用户类'''
    __tablename__ = 'cms_user'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    username = db.Column(db.String(30), nullable=False)
    password = db.Column(db.String(30), nullable=False)
    email = db.Column(db.String(50), nullable=False, unique=True)
    join_time = db.Column(db.DateTime, default=datetime.now())
1234567891011
```

创建了CMSUser后台管理员用户类。

在manage.py中导入模型：

```python
from flask_script import Manager
from bbs import app
from flask_migrate import Migrate, MigrateCommand
from exts import db
from apps.cms.models import CMSUser

manager = Manager(app)
Migrate(app, db)

manager.add_command('db', MigrateCommand)


if __name__ == '__main__':
    manager.run()

123456789101112131415
```

此时在命令行的当前目录下依次执行`python manage.py db init`、`python manage.py db migrate`、`python manage.py db upgrade`，执行成功后可以在数据库flask_bbs中看到cms_user表。

### 2.添加用户

在manage.py中添加代码来实现**通过命令行添加用户**：

```python
from flask_script import Manager
from bbs import app
from flask_migrate import Migrate, MigrateCommand
from exts import db
from apps.cms.models import CMSUser

manager = Manager(app)
Migrate(app, db)

manager.add_command('db', MigrateCommand)


@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
@manager.option('-e', '--email', dest='email')
def create_cms_user(username, password, email):
    user = CMSUser(username=username, password=password, email=email)
    db.session.add(user)
    db.session.commit()
    print('CMS用户添加成功')


if __name__ == '__main__':
    manager.run()

12345678910111213141516171819202122232425
```

此时在命令行中执行`python manage.py create_cms_user -u Corley -p admin -e 123@163.com`，打印`CMS用户添加成功`，此时查询数据库表cms_user：

```sql
select * from cms_user;
1
```

打印：

```sql
+----+----------+----------+-------------+---------------------+
| id | username | password | email       | join_time           |
+----+----------+----------+-------------+---------------------+
|  1 | Corley   | admin    | 123@163.com | 2020-05-18 20:26:19 |
+----+----------+----------+-------------+---------------------+
1 row in set (0.00 sec)

1234567
```

显然，数据插入成功，但是密码是明文，存在安全隐患，可以进一步进行优化。

删除cms_user表中数据，重新定义模型models.py如下：

```python
from datetime import datetime
from werkzeug.security import generate_password_hash
from exts import db


class CMSUser(db.Model):
    '''后台管理员用户类'''
    __tablename__ = 'cms_user'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    username = db.Column(db.String(30), nullable=False)
    _password = db.Column(db.String(100), nullable=False)
    email = db.Column(db.String(50), nullable=False, unique=True)
    join_time = db.Column(db.DateTime, default=datetime.now())

    def __init__(self, username, password, email):
        self.username = username
        self.password = password
        self.email = email

    @property
    def password(self):
        return self._password

    @password.setter
    def password(self, raw_password):
        self._password = generate_password_hash(raw_password)

123456789101112131415161718192021222324252627
```

使用`generate_password_hash()`对密码进行hash加密。
此时再依次执行`python manage.py db migrate`、`python manage.py db upgrade`、`python manage.py create_cms_user -u Corley -p admin -e 123@163.com`，打印`CMS用户添加成功`，再查询数据库表cms_user：

```sql
select * from cms_user;
1
```

打印：

```sql
+----+----------+-------------+---------------------+------------------------------------------------------------------------------------------------+
| id | username | email       | join_time           | _password                                                                                      |
+----+----------+-------------+---------------------+------------------------------------------------------------------------------------------------+
|  2 | Corley   | 123@163.com | 2020-05-18 20:42:05 | pbkdf2:sha256:150000$OtgjW9d7$6e1109428317afb0b3c093e1eb87da34c74d0a232d5cb15272737e5d218fd421 |
+----+----------+-------------+---------------------+------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)                                                                                                                               
                                                                                                                                                      
1234567
```

显然，此时的密码是经过加密的数据。

## 三、CMS登录页面搭建

本项目的很多前端HTML页面和组件都是使用BootStrap中文网https://www.bootcss.com/提供的模板。

cms目录下的views.py如下：

```python
from flask import Blueprint, render_template, views

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

@cms_bp.route('/')
def index():
    return '后台管理首页'


class LoginView(views.MethodView):
    def get(self):
        return render_template('cms/cms_login.html')


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
123456789101112131415
```

主程序文件bbs.py如下：

```python
'''
前台 front
后台 cms
公有 common
'''

from flask import Flask
from exts import db
from apps.cms.views import cms_bp
from apps.front.views import front_bp
import config


app = Flask(__name__)
app.config.from_object(config)
app.config['TEMPLATE_AUTO_RELOAD'] = True
db.init_app(app)

app.register_blueprint(cms_bp)
app.register_blueprint(front_bp)


if __name__ == '__main__':
    app.run(debug=True)
123456789101112131415161718192021222324
```

templates目录下创建cms子目录，下面创建cms_login.html，使用https://v3.bootcss.com/examples/signin/源代码，并进行一定修改如下：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <meta name="description" content="">
    <meta name="author" content="">
    <link rel="icon" href="{{ url_for('static', filename='cms/images/bbs-favicon.ico') }}">

    <title>CMS用户登录</title>

    <!-- Bootstrap core CSS -->
    <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">

    <!-- Custom styles for this template -->
    <link href="{{ url_for('static', filename='cms/css/signin.css') }}" rel="stylesheet">
    <![endif]-->
</head>

<body>

<div class="container">

    <form class="form-signin" method="post">
        <h2 class="form-signin-heading">请登录</h2>
        <label for="inputEmail" class="sr-only">邮箱地址</label>
        <input type="email" id="inputEmail" class="form-control" name="email" placeholder="邮箱地址" required autofocus>
        <label for="inputPassword" class="sr-only">密码</label>
        <input type="password" id="inputPassword" class="form-control" name="password" placeholder="密码" required>
        <div class="checkbox">
            <label>
                <input type="checkbox" value="remember-me" name="remember"> 记住我
            </label>
        </div>
        <button class="btn btn-lg btn-primary btn-block" type="submit">立即登录</button>
    </form>

</div> <!-- /container -->


</body>
</html>

123456789101112131415161718192021222324252627282930313233343536373839404142434445
```

static目录下创建cms子目录，下面创建css目录和images目录，css目录下创建signin.css如下：

```css
body {
  padding-top: 40px;
  padding-bottom: 40px;
  background-color: #eee;
}

.form-signin {
  max-width: 330px;
  padding: 15px;
  margin: 0 auto;
}
.form-signin .form-signin-heading,
.form-signin .checkbox {
  margin-bottom: 10px;
}
.form-signin .checkbox {
  font-weight: normal;
}
.form-signin .form-control {
  position: relative;
  height: auto;
  -webkit-box-sizing: border-box;
     -moz-box-sizing: border-box;
          box-sizing: border-box;
  padding: 10px;
  font-size: 16px;
}
.form-signin .form-control:focus {
  z-index: 2;
}
.form-signin input[type="email"] {
  margin-bottom: -1px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.form-signin input[type="password"] {
  margin-bottom: 10px;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}
12345678910111213141516171819202122232425262728293031323334353637383940
```

images目录下保存bbs-favicon.ico。

其中，signin.css和bbs-favicon.ico可以从BootStrap模板中获取，演示如下：
![flask bbs login file get](https://img-blog.csdnimg.cn/20200518214056752.gif#pic_center)

运行主程序，访问http://127.0.0.1:5000/cms/login/，显示：
![flask bbs login effect](https://img-blog.csdnimg.cn/20200518214112107.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，登录模板已经基本实现。

# Python全栈（八）Flask项目实战之2.CMS后台功能开发

Github和Gitee代码**同步更新**：
https://github.com/PythonWebProject/Flask_BBS；
https://gitee.com/Python_Web_Project/Flask_BBS。

## 一、CMS用户登录功能

上一节已经完成了后台管理员登录界面的渲染，现在进一步实现用户登录功能。

先实现表单验证，cms目录下的forms.py如下：

```python
from wtforms import Form, StringField, IntegerField
from wtforms.validators import Email, InputRequired, Length

class LoginForm(Form):
    email = StringField(validators=[Email(message='请输入正确的邮箱地址'), InputRequired(message='请输入邮箱')])
    password = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    remember = IntegerField()
1234567
```

views.py增加post函数如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for
from apps.cms.forms import LoginForm

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

@cms_bp.route('/')
def index():
    return '后台管理首页'


class LoginView(views.MethodView):
    def get(self):
        return render_template('cms/cms_login.html')

    def post(self):
        login_form = LoginForm(request.form)
        if login_form.validate():
            return redirect(url_for('cms.index'))
        else:
            print(login_form.errors)
        return '邮箱或密码有误'


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
123456789101112131415161718192021222324
```

命令行增加管理员用户：

```powershell
python manage.py create_cms_user -u corley -p admin1 -e cl@126.com
1
```

添加之后，运行主程序，显示：
![flask bbs cms login](https://img-blog.csdnimg.cn/20200523105135348.gif#pic_center)
显然，在登录之后，跳转到后台首页。

但是现在并未对邮箱和密码进行验证，需要做进一步的**数据库验证**：
config.py如下：

```python
import os

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_bbs'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

SQLALCHEMY_DATABASE_URI = DB_URL
SQLALCHEMY_TRACK_MODIFICATIONS = False

# 设置密钥
SECRET_KEY = os.urandom(15)
123456789101112131415
```

模型修改如下：

```python
from datetime import datetime
from werkzeug.security import generate_password_hash, check_password_hash
from exts import db


class CMSUser(db.Model):
    '''后台管理员用户类'''
    __tablename__ = 'cms_user'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    username = db.Column(db.String(30), nullable=False)
    _password = db.Column(db.String(100), nullable=False)
    email = db.Column(db.String(50), nullable=False, unique=True)
    join_time = db.Column(db.DateTime, default=datetime.now())

    def __init__(self, username, password, email):
        self.username = username
        self.password = password
        self.email = email

    @property
    def password(self):
        '''获取密码'''
        return self._password

    @password.setter
    def password(self, raw_password):
        '''设置密码'''
        self._password = generate_password_hash(raw_password)

    def check_password(self, raw_password):
        '''验证密码是否正确'''
        result = check_password_hash(self.password, raw_password)
        return result
123456789101112131415161718192021222324252627282930313233
```

视图函数views.py如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session
from apps.cms.forms import LoginForm
from apps.cms.models import CMSUser

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

@cms_bp.route('/')
def index():
    return '后台管理首页'


class LoginView(views.MethodView):
    def get(self):
        return render_template('cms/cms_login.html')

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return '邮箱或密码有误'
        else:
            print(login_form.errors)
            return '登录验证错误'


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
1234567891011121314151617181920212223242526272829303132333435363738394041
```

显示：
![flask bbs cms login validate](https://img-blog.csdnimg.cn/20200523105200136.gif#pic_center)
此时实现了登录的验证。

## 二、CMS用户错误信息返回

当然，在用户名或密码验证失败时，不应该简单地返回一个字符串，而是应该渲染一个模板，并且传入登录信息作为参数。

views.py修改如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session
from apps.cms.forms import LoginForm
from apps.cms.models import CMSUser

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

@cms_bp.route('/')
def index():
    return '后台管理首页'


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return '登录验证错误'


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
1234567891011121314151617181920212223242526272829303132333435363738394041
```

cms_login.html修改如下：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <meta name="description" content="">
    <meta name="author" content="">
    <link rel="icon" href="{{ url_for('static', filename='cms/images/bbs-favicon.ico') }}">

    <title>CMS用户登录</title>

    <!-- Bootstrap core CSS -->
    <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">

    <!-- Custom styles for this template -->
    <link href="{{ url_for('static', filename='cms/css/signin.css') }}" rel="stylesheet">
    <![endif]-->
</head>

<body>

<div class="container">

    <form class="form-signin" method="post">
        <h2 class="form-signin-heading">请登录</h2>
        <label for="inputEmail" class="sr-only">邮箱地址</label>
        <input type="email" id="inputEmail" class="form-control" name="email" placeholder="邮箱地址" required autofocus>
        <label for="inputPassword" class="sr-only">密码</label>
        <input type="password" id="inputPassword" class="form-control" name="password" placeholder="密码" required>
        <div class="checkbox">
            <label>
                <input type="checkbox" value="1" name="remember"> 记住我
            </label>
        </div>
        <button class="btn btn-lg btn-primary btn-block" type="submit">立即登录</button>
    </form>
    {% if message %}
        <p style="text-align: center" class="text-danger">{{ message }}</p>
    {% endif %}


</div> <!-- /container -->


</body>
</html>

12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849
```

显示：
![flask bbs cms login validate fail](https://img-blog.csdnimg.cn/20200523105234598.gif#pic_center)

除此之外，在验证失败时，也应该渲染一个模板。
views.py如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session
from apps.cms.forms import LoginForm
from apps.cms.models import CMSUser

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')


@cms_bp.route('/')
def index():
    return '后台管理首页'


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.errors.popitem()[1][0])


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))

12345678910111213141516171819202122232425262728293031323334353637383940414243
```

显示：
![flask bbs cms login form validate fail](https://img-blog.csdnimg.cn/20200523105314130.gif#pic_center)

## 三、CMS用户登录限制和CSRF保护

### 1.登录验证

通过钩子函数实现访问后台首页前验证是否已经登录：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session
from apps.cms.forms import LoginForm
from apps.cms.models import CMSUser

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

@cms_bp.before_request
def before_request():
    # 判断当前是否是登录页面
    if not request.path.endswith(url_for('cms.login')):
        user_id = session.get('user_id')
        # 判断是否保存到session
        if not user_id:
            return redirect(url_for('cms.login'))

@cms_bp.route('/')
def index():
    return '后台管理首页'


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.errors.popitem()[1][0])


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
```

显示：
![flask bbs cms login redirect](https://img-blog.csdnimg.cn/20200523105337441.gif#pic_center)
显然，未登录时，直接跳转到登录页面。

还可以使用**装饰器**实现，新建decorators.py如下：

```python
from flask import session, redirect, url_for

def login_required(func):
    def index(*args, **kwargs):
        if 'user_id' in session:
            return func(*args, **kwargs)
        else:
            return redirect(url_for('cms.login'))
    return index
123456789
```

views.py如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session
from apps.cms.forms import LoginForm
from apps.cms.models import CMSUser
from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

# @cms_bp.before_request
# def before_request():
#     # 判断当前是否是登录页面
#     if not request.path.endswith(url_for('cms.login')):
#         user_id = session.get('user_id')
#         # 判断是否保存到session
#         if not user_id:
#             return redirect(url_for('cms.login'))

@cms_bp.route('/')
@login_required
def index():
    return '后台管理首页'


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.errors.popitem()[1][0])


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152
```

可以达到同样的效果。

注意：
装饰器函数中的子函数名必须是**index**，否则可能会报错：

```python
Traceback (most recent call last):
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\flask\app.py", line 2464, in __call__
    return self.wsgi_app(environ, start_response)
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\flask\app.py", line 2450, in wsgi_app
    response = self.handle_exception(e)
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\flask\app.py", line 1867, in handle_exception
    reraise(exc_type, exc_value, tb)
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\flask\_compat.py", line 39, in reraise
    raise value
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\flask\app.py", line 2447, in wsgi_app
    response = self.full_dispatch_request()
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\flask\app.py", line 1952, in full_dispatch_request
    rv = self.handle_user_exception(e)
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\flask\app.py", line 1821, in handle_user_exception
    reraise(exc_type, exc_value, tb)
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\flask\_compat.py", line 39, in reraise
    raise value
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\flask\app.py", line 1950, in full_dispatch_request
    rv = self.dispatch_request()
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\flask\app.py", line 1936, in dispatch_request
    return self.view_functions[rule.endpoint](**req.view_args)
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\flask\views.py", line 89, in view
    return self.dispatch_request(*args, **kwargs)
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\flask\views.py", line 163, in dispatch_request
    return meth(*args, **kwargs)
  File "XXX\Flask_BBS\apps\cms\views.py", line 44, in post
    return redirect(url_for('cms.index'))
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\flask\helpers.py", line 370, in url_for
    return appctx.app.handle_url_build_error(error, endpoint, values)
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\flask\app.py", line 2216, in handle_url_build_error
    reraise(exc_type, exc_value, tb)
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\flask\_compat.py", line 39, in reraise
    raise value
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\flask\helpers.py", line 358, in url_for
    endpoint, values, method=method, force_external=external
  File "D:\.virtualenvs\Test-gftU5mTd\lib\site-packages\werkzeug\routing.py", line 2179, in build
    raise BuildError(endpoint, values, method, self)
werkzeug.routing.BuildError: Could not build url for endpoint 'cms.index'. Did you mean 'cms.inner' instead?
1234567891011121314151617181920212223242526272829303132333435363738
```

如果遇到类似的报错，请保持装饰器中的子函数名与所要装饰的视图函数名保持一致，即可解决。

显然，钩子函数更易于定义和使用，因此建议使用**钩子函数**，并且钩子函数也可以定义在专门的文件中，新定义hooks.py如下：

```python
from flask import request, session, url_for, redirect
from .views import cms_bp


@cms_bp.before_request
def before_request():
    # 判断当前是否是登录页面
    if not request.path.endswith(url_for('cms.login')):
        user_id = session.get('user_id')
        # 判断是否保存到session
        if not user_id:
            return redirect(url_for('cms.login'))
123456789101112
```

views.py如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session
from apps.cms.forms import LoginForm
from apps.cms.models import CMSUser
# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return '后台管理首页'


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.errors.popitem()[1][0])


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
12345678910111213141516171819202122232425262728293031323334353637383940414243444546
```

### 2.CSRF保护

CSRF即跨站请求伪造，是源于WEB的隐式**身份验证机制**，
一般的WEB身份验证机制虽然可以保证一个请求是来自于某个用户的浏览器，但却无法保证该请求是用户批准发送的。
例如，用户登录受信任的网址A，在本地生成了Cookie，在Cookie没有失效的情况下去访问了危险网站B，B可能会盗用你的身份，以你的名义发送恶意请求、盗取账号、设置购买商品等，造成个人隐私泄露和财产安全。

Flask中Flask-WTF表单保护用户**免受CSRF威胁**，可以保证表单请求的真实可靠，达到保护网站的作用。
在有不包含表单的视图时，页面请求仍需要保护。

使用如下：
主程序bbs.py中设置全局CSRF保护如下：

```python
'''
前台 front
后台 cms
公有 common
'''

from flask import Flask
from flask_wtf import CSRFProtect
from exts import db
from apps.cms.views import cms_bp
from apps.front.views import front_bp
import config


app = Flask(__name__)
CSRFProtect(app)
app.config.from_object(config)
app.config['TEMPLATE_AUTO_RELOAD'] = True
db.init_app(app)

app.register_blueprint(cms_bp)
app.register_blueprint(front_bp)


if __name__ == '__main__':
    app.run(debug=True)
1234567891011121314151617181920212223242526
```

cms_login.html表单增加CSRF验证如下：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <meta name="description" content="">
    <meta name="author" content="">
    <link rel="icon" href="{{ url_for('static', filename='cms/images/bbs-favicon.ico') }}">

    <title>CMS用户登录</title>

    <!-- Bootstrap core CSS -->
    <link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">

    <!-- Custom styles for this template -->
    <link href="{{ url_for('static', filename='cms/css/signin.css') }}" rel="stylesheet">
    <![endif]-->
</head>

<body>

<div class="container">

    <form class="form-signin" method="post">
        <input type="hidden" name="csrf_token" value="{{ csrf_token() }}">
        <h2 class="form-signin-heading">请登录</h2>
        <label for="inputEmail" class="sr-only">邮箱地址</label>
        <input type="email" id="inputEmail" class="form-control" name="email" placeholder="邮箱地址" required autofocus>
        <label for="inputPassword" class="sr-only">密码</label>
        <input type="password" id="inputPassword" class="form-control" name="password" placeholder="密码" required>
        <div class="checkbox">
            <label>
                <input type="checkbox" value="1" name="remember"> 记住我
            </label>
        </div>
        <button class="btn btn-lg btn-primary btn-block" type="submit">立即登录</button>
    </form>
    {% if message %}
        <p style="text-align: center" class="text-danger">{{ message }}</p>
    {% endif %}


</div> <!-- /container -->


</body>
</html>

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
```

显示：
![flask bbs cms login csrf protect](https://img-blog.csdnimg.cn/20200523105417530.gif#pic_center)
显然，此时页面中多了**CSRF验证的随机字符串**（虽然是被隐藏的）。

## 四、CMS用户名渲染和注销功能

### 1.后台页面基本实现

在templates/cms下创建cms_index.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}{% endblock %}</title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='cms/css/cms_base.css') }}">
    <script src="{{ url_for('static', filename='cms/js/cms_base.js') }}"></script>
    {% block head %}
    {% endblock %}
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
    <div class="container-fluid">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">熊熊乐园论坛CMS管理系统</a>
        </div>
        <div id="navbar" class="navbar-collapse collapse">
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">小柯<span>[超级管理员]</span></a></li>
                <li><a href="#">注销</a></li>
            </ul>
            <form class="navbar-form navbar-right">
                <input type="text" class="form-control" placeholder="查找...">
            </form>
        </div>
    </div>
</nav>

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3 col-md-2 sidebar">
            <ul class="nav-sidebar">
                <li class="unfold"><a href="#">首页</a></li>
                <li class="profile-li">
                    <a href="#">个人中心<span></span></a>
                    <ul class="subnav">
                        <li><a href="#">个人信息</a></li>
                        <li><a href="#">修改密码</a></li>
                        <li><a href="#">修改邮箱</a></li>
                    </ul>
                </li>

                <li class="nav-group post-manage"><a href="#">帖子管理</a></li>
                <li class="comments-manage"><a href="#">评论管理</a></li>
                <li class="board-manage"><a href="#">板块管理</a></li>

                <li class="nav-group user-manage"><a href="#">用户管理</a></li>
                <li class="role-manage"><a href="#">组管理</a></li>

                <li class="nav-group cmsuser-manage"><a href="#">CMS用户管理</a></li>
                <li class="cmsrole-manage"><a href="#">CMS组管理</a></li>
            </ul>
        </div>
        <div class="col-sm-9 col-sm-offset-3 col-md-10 col-md-offset-2 main">
            <h1>{% block page_title %} {% endblock %}</h1>
            <div class="main_content">
                {% block content %} {% endblock %}
            </div>
        </div>
    </div>
</div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273
```

在static/cms/css下创建cms_base.css如下：

```css
/*
 * Base structure
 */

/* Move down content because we have a fixed navbar that is 50px tall */
body {
    padding-top: 50px;
    overflow: hidden;
}

/*
 * Global add-ons
 */

.sub-header {
    padding-bottom: 10px;
    border-bottom: 1px solid #eee;
}

/*
 * Top navigation
 * Hide default border to remove 1px line.
 */
.navbar-fixed-top {
    border: 0;
}

/*
 * Sidebar
 */

/* Hide for mobile, show later */
.sidebar {
    display: none;
}

@media (min-width: 768px) {
    .sidebar {
        position: fixed;
        top: 51px;
        bottom: 0;
        left: 0;
        z-index: 1000;
        display: block;
        padding: 20px;
        overflow-x: hidden;
        overflow-y: auto; /* Scrollable contents if viewport is shorter than content. */
        background-color: #363a47;
        border-right: 1px solid #eee;
        margin-top: -1px;
    }
}

.nav-sidebar {
    padding: 5px 0;
    margin-left: -20px;
    margin-right: -20px;
}

.nav-sidebar > li {
    background: #494f60;
    border-bottom: 1px solid #363a47;
    border-top: 1px solid #666;
    line-height: 35px;
}

.nav-sidebar > li > a {
    background: #494f60;
    color: #9b9fb1;
    margin-left: 25px;
    display: block;
}

.nav-sidebar > li a span {
    float: right;
    width: 10px;
    height: 10px;
    border-style: solid;
    border-color: #9b9fb1 #9b9fb1 transparent transparent;
    border-width: 1px;
    transform: rotate(45deg);
    position: relative;
    top: 10px;
    margin-right: 10px;
}

.nav-sidebar > li > a:hover {
    color: #fff;
    background: #494f60;
    text-decoration: none;
}

.nav-sidebar > li > .subnav {
    display: none;
}

.nav-sidebar > li.unfold {
    background: #494f60;
}

.nav-sidebar > li.unfold > .subnav {
    display: block;
}

.nav-sidebar > li.unfold > a {
    color: #db4055;
}

.nav-sidebar > li.unfold > a span {
    transform: rotate(135deg);
    top: 5px;
    border-color: #db4055 #db4055 transparent transparent;
}

.subnav {
    padding-left: 10px;
    padding-right: 10px;
    background: #363a47;
    overflow: hidden;
}

.subnav li {
    overflow: hidden;
    margin-top: 10px;
    line-height: 25px;
    height: 25px;
}

.subnav li.active {
    background: #db4055;
}

.subnav li a {
    /*display: block;*/
    color: #9b9fb1;
    padding-left: 30px;
    height: 25px;
    line-height: 25px;
}

.subnav li a:hover {
    color: #fff;
}

.nav-group {
    margin-top: 10px;
}


.main {
    padding: 20px;
}

@media (min-width: 768px) {
    .main {
        padding-right: 40px;
        padding-left: 40px;
    }
}

.main .page-header {
    margin-top: 0;
}


/*
 * Placeholder dashboard ideas
 */

.placeholders {
    margin-bottom: 30px;
    text-align: center;
}

.placeholders h4 {
    margin-bottom: 0;
}

.placeholder {
    margin-bottom: 20px;
}

.placeholder img {
    display: inline-block;
    border-radius: 50%;
}

.main_content {
    margin-top: 20px;
}


.top-group {
    padding: 5px 10px;
    border-radius: 2px;
    background: #ecedf0;
    overflow: hidden;
}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198
```

在static/cms下创建js目录，下面创建cms_base.js和resetpwd.js，cms_base.js如下：

```js
$(function () {
    $('.nav-sidebar>li>a').click(function (event) {
        var that = $(this);
        if (that.children('a').attr('href') == '#') {
            event.preventDefault();
        }
        if (that.parent().hasClass('unfold')) {
            that.parent().removeClass('unfold');
        } else {
            that.parent().addClass('unfold').siblings().removeClass('unfold');
        }
        console.log('coming....');
    });

    $('.nav-sidebar a').mouseleave(function () {
        $(this).css('text-decoration', 'none');
    });
});


$(function () {
    var url = window.location.href;
    if (url.indexOf('profile') >= 0) {
        var profileLi = $('.profile-li');
        profileLi.addClass('unfold').siblings().removeClass('unfold');
        profileLi.children('.subnav').children().eq(0).addClass('active').siblings().removeClass('active');
    } else if (url.indexOf('resetpwd') >= 0) {
        var profileLi = $('.profile-li');
        profileLi.addClass('unfold').siblings().removeClass('unfold');
        profileLi.children('.subnav').children().eq(1).addClass('active').siblings().removeClass('active');
    } else if (url.indexOf('resetemail') >= 0) {
        var profileLi = $('.profile-li');
        profileLi.addClass('unfold').siblings().removeClass('unfold');
        profileLi.children('.subnav').children().eq(2).addClass('active').siblings().removeClass('active');
    } else if (url.indexOf('posts') >= 0) {
        var postManageLi = $('.post-manage');
        postManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('boards') >= 0) {
        var boardManageLi = $('.board-manage');
        boardManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('permissions') >= 0) {
        var permissionManageLi = $('.permission-manage');
        permissionManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('roles') >= 0) {
        var roleManageLi = $('.role-manage');
        roleManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('users') >= 0) {
        var userManageLi = $('.user-manage');
        userManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('cmsuser_manage') >= 0) {
        var cmsuserManageLi = $('.cmsuser-manage');
        cmsuserManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('cmsrole_manage') >= 0) {
        var cmsroleManageLi = $('.cmsrole-manage');
        cmsroleManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('comments') >= 0) {
        var commentsManageLi = $('.comments-manage');
        commentsManageLi.addClass('unfold').siblings().removeClass('unfold');
    }
});
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960
```

resetpwd.js如下：

```js
$(function () {
    $("#submit").click(function (event) {
        // event.preventDefault
        // 是阻止按钮默认的提交表单的事件
        event.preventDefault();

        var oldpwdE = $("input[name=oldpwd]");
        var newpwdE = $("input[name=newpwd]");
        var newpwd2E = $("input[name=newpwd2]");

        var oldpwd = oldpwdE.val();
        var newpwd = newpwdE.val();
        var newpwd2 = newpwd2E.val();

        // 1. 要在模版的meta标签中渲染一个csrf-token
        // 2. 在ajax请求的头部中设置X-CSRFtoken
        var lgajax = {
            'get': function (args) {
                args['method'] = 'get';
                this.ajax(args);
            },
            'post': function (args) {
                args['method'] = 'post';
                this.ajax(args);
            },
            'ajax': function (args) {
                // 设置csrftoken
                this._ajaxSetup();
                $.ajax(args);
            },
            '_ajaxSetup': function () {
                $.ajaxSetup({
                    'beforeSend': function (xhr, settings) {
                        if (!/^(GET|HEAD|OPTIONS|TRACE)$/i.test(settings.type) && !this.crossDomain) {
//                            var csrftoken = $('meta[name=csrf-token]').attr('content');
                            var csrftoken = $('input[name=csrf-token]').attr('value');
                            xhr.setRequestHeader("X-CSRFToken", csrftoken)
                        }
                    }
                });
            }
        };

        lgajax.post({
            'url': '/cms/resetpwd/',
            'data': {
                'oldpwd': oldpwd,
                'newpwd': newpwd,
                'newpwd2': newpwd2
            },
            'success': function (data) {
                console.log(data);
            },
            'fail': function (error) {
                console.log(error);
            }
        });
    });
});
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859
```

views.py修改如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session
from apps.cms.forms import LoginForm
from apps.cms.models import CMSUser
# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html')


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.errors.popitem()[1][0])


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
12345678910111213141516171819202122232425262728293031323334353637383940414243444546
```

显示：
![flask bbs cms index overview](https://img-blog.csdnimg.cn/20200523105501253.gif#pic_center)
显然，已经实现后台管理的基本效果。

### 2.用户名渲染

现进行用户名渲染，hooks.py如下：

```python
from flask import request, session, url_for, redirect, g
from .models import CMSUser
from .views import cms_bp


@cms_bp.before_request
def before_request():
    # 判断当前是否是登录页面
    if not request.path.endswith(url_for('cms.login')):
        user_id = session.get('user_id')
        # 判断是否保存到session
        if not user_id:
            return redirect(url_for('cms.login'))

    if 'user_id' in session:
        user_id = session.get('user_id')
        user = CMSUser.query.get(user_id)
        # 使用g对象保存用户
        if user:
            g.cms_user = user
1234567891011121314151617181920
```

cms_index.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}{% endblock %}</title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='cms/css/cms_base.css') }}">
    <script src="{{ url_for('static', filename='cms/js/cms_base.js') }}"></script>
    {% block head %}
    {% endblock %}
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
    <div class="container-fluid">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">熊熊乐园论坛CMS管理系统</a>
        </div>
        <div id="navbar" class="navbar-collapse collapse">
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">{{ g.cms_user.username }}<span>[超级管理员]</span></a></li>
                <li><a href="#">注销</a></li>
            </ul>
            <form class="navbar-form navbar-right">
                <input type="text" class="form-control" placeholder="查找...">
            </form>
        </div>
    </div>
</nav>

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3 col-md-2 sidebar">
            <ul class="nav-sidebar">
                <li class="unfold"><a href="#">首页</a></li>
                <li class="profile-li">
                    <a href="#">个人中心<span></span></a>
                    <ul class="subnav">
                        <li><a href="#">个人信息</a></li>
                        <li><a href="#">修改密码</a></li>
                        <li><a href="#">修改邮箱</a></li>
                    </ul>
                </li>

                <li class="nav-group post-manage"><a href="#">帖子管理</a></li>
                <li class="comments-manage"><a href="#">评论管理</a></li>
                <li class="board-manage"><a href="#">板块管理</a></li>

                <li class="nav-group user-manage"><a href="#">用户管理</a></li>
                <li class="role-manage"><a href="#">组管理</a></li>

                <li class="nav-group cmsuser-manage"><a href="#">CMS用户管理</a></li>
                <li class="cmsrole-manage"><a href="#">CMS组管理</a></li>
            </ul>
        </div>
        <div class="col-sm-9 col-sm-offset-3 col-md-10 col-md-offset-2 main">
            <h1>{% block page_title %} {% endblock %}</h1>
            <div class="main_content">
                {% block content %} {% endblock %}
            </div>
        </div>
    </div>
</div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273
```

显示：
![flask bbs cms index username render](https://img-blog.csdnimg.cn/20200523105522354.gif#pic_center)
显然，已经根据登录信息渲染出用户名。

### 3.注销功能实现

注销主要包括两方面：

- **清空或删除session**
- **重定向**到登录页面

现实现注销功能，views.py如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session
from apps.cms.forms import LoginForm
from apps.cms.models import CMSUser
# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html')


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.errors.popitem()[1][0])


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354
```

cms_index.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}{% endblock %}</title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='cms/css/cms_base.css') }}">
    <script src="{{ url_for('static', filename='cms/js/cms_base.js') }}"></script>
    {% block head %}
    {% endblock %}
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
    <div class="container-fluid">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">熊熊乐园论坛CMS管理系统</a>
        </div>
        <div id="navbar" class="navbar-collapse collapse">
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">{{ g.cms_user.username }}<span>[超级管理员]</span></a></li>
                <li><a href="{{ url_for('cms.logout') }}">注销</a></li>
            </ul>
            <form class="navbar-form navbar-right">
                <input type="text" class="form-control" placeholder="查找...">
            </form>
        </div>
    </div>
</nav>

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3 col-md-2 sidebar">
            <ul class="nav-sidebar">
                <li class="unfold"><a href="#">首页</a></li>
                <li class="profile-li">
                    <a href="#">个人中心<span></span></a>
                    <ul class="subnav">
                        <li><a href="#">个人信息</a></li>
                        <li><a href="#">修改密码</a></li>
                        <li><a href="#">修改邮箱</a></li>
                    </ul>
                </li>

                <li class="nav-group post-manage"><a href="#">帖子管理</a></li>
                <li class="comments-manage"><a href="#">评论管理</a></li>
                <li class="board-manage"><a href="#">板块管理</a></li>

                <li class="nav-group user-manage"><a href="#">用户管理</a></li>
                <li class="role-manage"><a href="#">组管理</a></li>

                <li class="nav-group cmsuser-manage"><a href="#">CMS用户管理</a></li>
                <li class="cmsrole-manage"><a href="#">CMS组管理</a></li>
            </ul>
        </div>
        <div class="col-sm-9 col-sm-offset-3 col-md-10 col-md-offset-2 main">
            <h1>{% block page_title %} {% endblock %}</h1>
            <div class="main_content">
                {% block content %} {% endblock %}
            </div>
        </div>
    </div>
</div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273
```

显示：
![flask bbs cms index logout](https://img-blog.csdnimg.cn/20200523105555807.gif#pic_center)
显然，实现了注销功能，注销后直接重定向到登录页面，再访问后台管理首页也会重定向到登录页。

## 五、CMS个人页面和模板抽离

### 1.个人信息页面实现

实现后台个人信息页面，views.py如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session
from apps.cms.forms import LoginForm
from apps.cms.models import CMSUser
# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html')


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html")


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.errors.popitem()[1][0])


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859
```

cms_index.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}{% endblock %}</title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='cms/css/cms_base.css') }}">
    <script src="{{ url_for('static', filename='cms/js/cms_base.js') }}"></script>
    {% block head %}
    {% endblock %}
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
    <div class="container-fluid">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">熊熊乐园论坛CMS管理系统</a>
        </div>
        <div id="navbar" class="navbar-collapse collapse">
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">{{ g.cms_user.username }}<span>[超级管理员]</span></a></li>
                <li><a href="{{ url_for('cms.logout') }}">注销</a></li>
            </ul>
            <form class="navbar-form navbar-right">
                <input type="text" class="form-control" placeholder="查找...">
            </form>
        </div>
    </div>
</nav>

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3 col-md-2 sidebar">
            <ul class="nav-sidebar">
                <li class="unfold"><a href="#">首页</a></li>
                <li class="profile-li">
                    <a href="#">个人中心<span></span></a>
                    <ul class="subnav">
                        <li><a href="{{ url_for('cms.profile') }}">个人信息</a></li>
                        <li><a href="#">修改密码</a></li>
                        <li><a href="#">修改邮箱</a></li>
                    </ul>
                </li>

                <li class="nav-group post-manage"><a href="#">帖子管理</a></li>
                <li class="comments-manage"><a href="#">评论管理</a></li>
                <li class="board-manage"><a href="#">板块管理</a></li>

                <li class="nav-group user-manage"><a href="#">用户管理</a></li>
                <li class="role-manage"><a href="#">组管理</a></li>

                <li class="nav-group cmsuser-manage"><a href="#">CMS用户管理</a></li>
                <li class="cmsrole-manage"><a href="#">CMS组管理</a></li>
            </ul>
        </div>
        <div class="col-sm-9 col-sm-offset-3 col-md-10 col-md-offset-2 main">
            <h1>熊熊论坛</h1>
            <div class="main_content">
                <h3>欢迎来到熊熊论坛</h3>
            </div>
        </div>
    </div>
</div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273
```

新建cms_profile.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>cms-个人中心</title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='cms/css/cms_base.css') }}">
    <script src="{{ url_for('static', filename='cms/js/cms_base.js') }}"></script>
    {% block head %}
    {% endblock %}
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
    <div class="container-fluid">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">熊熊乐园论坛CMS管理系统</a>
        </div>
        <div id="navbar" class="navbar-collapse collapse">
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">{{ g.cms_user.username }}<span>[超级管理员]</span></a></li>
                <li><a href="{{ url_for('cms.logout') }}">注销</a></li>
            </ul>
            <form class="navbar-form navbar-right">
                <input type="text" class="form-control" placeholder="查找...">
            </form>
        </div>
    </div>
</nav>

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3 col-md-2 sidebar">
            <ul class="nav-sidebar">
                <li class="unfold"><a href="#">首页</a></li>
                <li class="profile-li">
                    <a href="#">个人中心<span></span></a>
                    <ul class="subnav">
                        <li><a href="{{ url_for('cms.profile') }}">个人信息</a></li>
                        <li><a href="#">修改密码</a></li>
                        <li><a href="#">修改邮箱</a></li>
                    </ul>
                </li>

                <li class="nav-group post-manage"><a href="#">帖子管理</a></li>
                <li class="comments-manage"><a href="#">评论管理</a></li>
                <li class="board-manage"><a href="#">板块管理</a></li>

                <li class="nav-group user-manage"><a href="#">用户管理</a></li>
                <li class="role-manage"><a href="#">组管理</a></li>

                <li class="nav-group cmsuser-manage"><a href="#">CMS用户管理</a></li>
                <li class="cmsrole-manage"><a href="#">CMS组管理</a></li>
            </ul>
        </div>
        <div class="col-sm-9 col-sm-offset-3 col-md-10 col-md-offset-2 main">
            <h1>个人中心</h1>
            <div class="main_content">
                <table class="table table-bordered">
                    <tr>
                        <td>用户名：</td>
                        <td>Corley</td>
                    </tr>
                    <tr>
                        <td>邮箱：</td>
                        <td>cl@126.com</td>
                    </tr>
                    <tr>
                        <td>角色：</td>
                        <td>暂无</td>
                    </tr>
                    <tr>
                        <td>权限：</td>
                        <td>暂无</td>
                    </tr>
                    <tr>
                        <td>加入时间：</td>
                        <td>2020-05-21</td>
                    </tr>
                </table>
            </div>
        </div>
    </div>
</div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879808182838485868788899091929394
```

显示：
![flask bbs cms index profile](https://img-blog.csdnimg.cn/20200523105725861.gif#pic_center)
显然，可以显示管理员用户的基本信息。

### 2.模板抽离

很明显，cms_index.html和cms_profile.html有大段重复的代码，因此可以使用**模板继承**简化代码。

新建cms_base.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>
        {% block title %}

        {% endblock %}
    </title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='cms/css/cms_base.css') }}">
    <script src="{{ url_for('static', filename='cms/js/cms_base.js') }}"></script>
    {% block head %}
    {% endblock %}
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
    <div class="container-fluid">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">熊熊乐园论坛CMS管理系统</a>
        </div>
        <div id="navbar" class="navbar-collapse collapse">
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">{{ g.cms_user.username }}<span>[超级管理员]</span></a></li>
                <li><a href="{{ url_for('cms.logout') }}">注销</a></li>
            </ul>
            <form class="navbar-form navbar-right">
                <input type="text" class="form-control" placeholder="查找...">
            </form>
        </div>
    </div>
</nav>

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3 col-md-2 sidebar">
            <ul class="nav-sidebar">
                <li class="unfold"><a href="#">首页</a></li>
                <li class="profile-li">
                    <a href="#">个人中心<span></span></a>
                    <ul class="subnav">
                        <li><a href="{{ url_for('cms.profile') }}">个人信息</a></li>
                        <li><a href="#">修改密码</a></li>
                        <li><a href="#">修改邮箱</a></li>
                    </ul>
                </li>

                <li class="nav-group post-manage"><a href="#">帖子管理</a></li>
                <li class="comments-manage"><a href="#">评论管理</a></li>
                <li class="board-manage"><a href="#">板块管理</a></li>

                <li class="nav-group user-manage"><a href="#">用户管理</a></li>
                <li class="role-manage"><a href="#">组管理</a></li>

                <li class="nav-group cmsuser-manage"><a href="#">CMS用户管理</a></li>
                <li class="cmsrole-manage"><a href="#">CMS组管理</a></li>
            </ul>
        </div>
        <div class="col-sm-9 col-sm-offset-3 col-md-10 col-md-offset-2 main">
            <h1>{% block page_title %} {% endblock %}</h1>
            <div class="main_content">
                {% block content %} {% endblock %}
            </div>
        </div>
    </div>
</div>
</body>
</html>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677
```

cms_index.html修改如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台
{% endblock %}

{% block page_title %}
    熊熊论坛
{% endblock %}

{% block content %}
    欢迎来到{{ self.page_title() }}......
{% endblock %}
12345678910111213
```

cms_profile.html修改如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台个人信息
{% endblock %}

{% block page_title %}
    熊熊个人中心
{% endblock %}

{% block content %}
    {% set user = g.cms_user %}
    <table class="table table-bordered">
        <tr>
            <td>用户名：</td>
            <td>{{ user.username }}</td>
        </tr>
        <tr>
            <td>邮箱：</td>
            <td>{{ user.email }}</td>
        </tr>
        <tr>
            <td>角色：</td>
            <td>暂无</td>
        </tr>
        <tr>
            <td>权限：</td>
            <td>暂无</td>
        </tr>
        <tr>
            <td>加入时间：</td>
            <td>{{ user.join_time }}</td>
        </tr>
    </table>
{% endblock %}
1234567891011121314151617181920212223242526272829303132333435
```

显示：
![flask bbs cms index template extend](https://img-blog.csdnimg.cn/20200523105741915.gif#pic_center)
显然，模板抽离之后，也实现了基本功能，并且代码更简洁。

# Python全栈（八）Flask项目实战之3.CMS后台修改密码

Github和Gitee代码**同步更新**：
https://github.com/PythonWebProject/Flask_BBS；
https://gitee.com/Python_Web_Project/Flask_BBS。

## 一、CMS后台修改密码界面布局

先创建修改密码的模板文件cms_resetpwd.html：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    修改密码
{% endblock %}

{% block page_title %}
    后台管理员{{ self.title() }}
{% endblock %}

{% block head %}
    <style>
        .form-container {
            width: 300px;
        }
    </style>
    <script src="{{ url_for('static', filename='cms/js/resetpwd.js') }}"></script>
{% endblock %}

{% block content %}
    <form action="" method="post">
        <!--<input type="hidden" name="csrf_token" value="{{ csrf_token() }}">-->
        <div class="form-container">
            <div class="form-group">
                <div class="input-group">
                    <span class="input-group-addon">旧密码</span>
                    <input type="password" class="form-control" name="oldpwd" placeholder="请输入旧密码">
                </div>
            </div>
            <div class="form-group">
                <div class="input-group">
                    <span class="input-group-addon">新密码</span>
                    <input type="password" class="form-control" name="newpwd" placeholder="请输入新密码">
                </div>
            </div>
            <div class="form-group">
                <div class="input-group">
                    <span class="input-group-addon">确认新密码</span>
                    <input type="password" class="form-control" name="newpwd2" placeholder="请输入确认密码">
                </div>
            </div>
            <div class="form-group">
                <button class="btn btn-primary" id="submit">立即保存</button>
            </div>
        </div>
    </form>
{% endblock %}
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647
```

cms_base.html进行修改：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>
        {% block title %}

        {% endblock %}
    </title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='cms/css/cms_base.css') }}">
    <script src="{{ url_for('static', filename='cms/js/cms_base.js') }}"></script>
    {% block head %}
    {% endblock %}
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
    <div class="container-fluid">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">熊熊乐园论坛CMS管理系统</a>
        </div>
        <div id="navbar" class="navbar-collapse collapse">
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">{{ g.cms_user.username }}<span>[超级管理员]</span></a></li>
                <li><a href="{{ url_for('cms.logout') }}">注销</a></li>
            </ul>
            <form class="navbar-form navbar-right">
                <input type="text" class="form-control" placeholder="查找...">
            </form>
        </div>
    </div>
</nav>

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3 col-md-2 sidebar">
            <ul class="nav-sidebar">
                <li class="unfold"><a href="#">首页</a></li>
                <li class="profile-li">
                    <a href="#">个人中心<span></span></a>
                    <ul class="subnav">
                        <li><a href="{{ url_for('cms.profile') }}">个人信息</a></li>
                        <li><a href="{{ url_for('cms.resetpwd') }}">修改密码</a></li>
                        <li><a href="#">修改邮箱</a></li>
                    </ul>
                </li>

                <li class="nav-group post-manage"><a href="#">帖子管理</a></li>
                <li class="comments-manage"><a href="#">评论管理</a></li>
                <li class="board-manage"><a href="#">板块管理</a></li>

                <li class="nav-group user-manage"><a href="#">用户管理</a></li>
                <li class="role-manage"><a href="#">组管理</a></li>

                <li class="nav-group cmsuser-manage"><a href="#">CMS用户管理</a></li>
                <li class="cmsrole-manage"><a href="#">CMS组管理</a></li>
            </ul>
        </div>
        <div class="col-sm-9 col-sm-offset-3 col-md-10 col-md-offset-2 main">
            <h1>{% block page_title %} {% endblock %}</h1>
            <div class="main_content">
                {% block content %} {% endblock %}
            </div>
        </div>
    </div>
</div>
</body>
</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778
```

注意：
此时将csrf验证放入head元素的**meta元素**中，在使得所有继承自cms_base.html的模板可以发送csrf验证的同时，也可以使resetpwd.js中Ajax可以接收到csrf验证从而提交表单。

再在视图函数文件views.py中增加视图类：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session
from apps.cms.forms import LoginForm
from apps.cms.models import CMSUser
# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html')


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html")


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.errors.popitem()[1][0])


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html')

    def post(self):
        pass


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768
```

运行主程序后，显示：
![flask bbs cms reset pwd get](https://img-blog.csdnimg.cn/20200529102916365.gif#pic_center)
显然，此时已经渲染出修改密码的模板，但是不能提交，需要在视图类中实现post方法。

之前已经在static/cms/js下保存了resetpwd.js，这是通过ajax实现局部更新，代码如下：

```js
$(function () {
    $("#submit").click(function (event) {
        // event.preventDefault
        // 是阻止按钮默认的提交表单的事件
        event.preventDefault();

        var oldpwdE = $("input[name=oldpwd]");
        var newpwdE = $("input[name=newpwd]");
        var newpwd2E = $("input[name=newpwd2]");

        var oldpwd = oldpwdE.val();
        var newpwd = newpwdE.val();
        var newpwd2 = newpwd2E.val();

        // 1. 要在模版的meta标签中渲染一个csrf-token
        // 2. 在ajax请求的头部中设置X-CSRFtoken
        var clajax = {
            'get': function (args) {
                args['method'] = 'get';
                this.ajax(args);
            },
            'post': function (args) {
                args['method'] = 'post';
                this.ajax(args);
            },
            'ajax': function (args) {
                // 设置csrftoken
                this._ajaxSetup();
                $.ajax(args);
            },
            '_ajaxSetup': function () {
                $.ajaxSetup({
                    'beforeSend': function (xhr, settings) {
                        if (!/^(GET|HEAD|OPTIONS|TRACE)$/i.test(settings.type) && !this.crossDomain) {
                            var csrftoken = $('meta[name=csrf-token]').attr('content');
                            // var csrftoken = $('input[name=csrf-token]').attr('value');
                            xhr.setRequestHeader("X-CSRFToken", csrftoken)
                        }
                    }
                });
            }
        };

        // 表单提交方式是post方法
        clajax.post({
            'url': '/cms/resetpwd/',
            'data': {
                'oldpwd': oldpwd,
                'newpwd': newpwd,
                'newpwd2': newpwd2
            },
            'success': function (data) {
                console.log(data);
            },
            'fail': function (error) {
                console.log(error);
            }
        });
    });
});
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960
```

## 二、CMS后台通过Ajax修改密码

先在forms.py中实现修改密码提交数据的表单验证：

```python
from wtforms import Form, StringField, IntegerField
from wtforms.validators import Email, InputRequired, Length, EqualTo

class LoginForm(Form):
    email = StringField(validators=[Email(message='请输入正确的邮箱地址'), InputRequired(message='请输入邮箱')])
    password = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    remember = IntegerField()


class ResetPwdForm(Form):
    oldpwd = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    newpwd = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    newpwd2 = StringField(validators=[EqualTo("newpwd", message='两次输入密码不一致')])
12345678910111213
```

再在views.py中完善post方法：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, jsonify, g
from apps.cms.forms import LoginForm, ResetPwdForm
from apps.cms.models import CMSUser
from exts import db

# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html')


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html")


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.errors.popitem()[1][0])


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html')

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return jsonify({'code': 200, 'message': ''})
            else:
                return jsonify({'code': 400, 'message': '旧密码错误'})
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return jsonify({'code': 400, 'message': form.errors.popitem()[1][0]})


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283848586
```

显示：
![flask bbs cms reset pwd get](https://img-blog.csdnimg.cn/20200529102944456.gif#pic_center)
此时可以达到修改密码的目的，但是必须要保证输入有效。

## 三、优化Json数据返回

之前都是返回单一的json格式的数据，需要进一步优化json返回值。

在项目目录下新建utils工具目录，下创建restful.py如下：

```python
from flask import jsonify


class HttpCode(object):
    ok = 200
    unauth = 401
    paramerror = 400
    server = 500


def restful_result(code, message, data):
    return jsonify({'code': code, 'message': message, 'data': data})


def success(message='', data=None):
    return restful_result(code=HttpCode.ok, message=message, data=data)


def unauth_error(message=""):
    return restful_result(code=HttpCode.unauth, message=message, data=None)


def params_error(message=""):
    return restful_result(code=HttpCode.paramerror, message=message, data=None)


def server_error(message=""):
    return restful_result(code=HttpCode.paramerror, message=message or '服务器内部错误', data=None)
12345678910111213141516171819202122232425262728
```

修改视图函数views.py如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from apps.cms.forms import LoginForm, ResetPwdForm
from apps.cms.models import CMSUser
from exts import db
from utils import restful

# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html')


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html")


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            message = login_form.errors.popitem()[1][0]
            return self.get(message=message)


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html')

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            message = form.errors.popitem()[1][0]
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=message)


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283848586878889
```

显示：
![flask bbs cms reset pwd restful](https://img-blog.csdnimg.cn/20200529103156187.gif#pic_center)
显然，也可以达到之前的效果，并且代码结构更严整。

除此之外，在两个视图类中都有`message = login_form.errors.popitem()[1][0]`，显得重复，可以将其抽离出来，即在forms.py中为两个表单类定义父类，如下：

```python
from wtforms import Form, StringField, IntegerField
from wtforms.validators import Email, InputRequired, Length, EqualTo


class BaseForm(Form):
    def get_error(self):
        message = self.errors.popitem()[1][0]
        return message


class LoginForm(BaseForm):
    email = StringField(validators=[Email(message='请输入正确的邮箱地址'), InputRequired(message='请输入邮箱')])
    password = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    remember = IntegerField()


class ResetPwdForm(BaseForm):
    oldpwd = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    newpwd = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    newpwd2 = StringField(validators=[EqualTo("newpwd", message='两次输入密码不一致')])
1234567891011121314151617181920
```

views.py修改如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from apps.cms.forms import LoginForm, ResetPwdForm
from apps.cms.models import CMSUser
from exts import db
from utils import restful

# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html')


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html")


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html')

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687
```

可以达到与之前相同的效果，显然，大大降低了耦合性、提高了内聚性。

## 四、sweetalert修改提示框

在static目录下创建common目录，下创建sweetalert目录，下创建clalert.js如下：

```js
var clalert = {
    /*
        功能：提示错误
        参数：
            - msg：提示的内容（可选）
    */
    'alertError': function (msg) {
        swal('提示', msg, 'error');
    },
    /*
        功能：信息提示
        参数：
            - msg：提示的内容（可选）
    */
    'alertInfo': function (msg) {
        swal('提示', msg, 'warning');
    },
    /*
        功能：可以自定义标题的信息提示
        参数：
            - msg：提示的内容（可选）
    */
    'alertInfoWithTitle': function (title, msg) {
        swal(title, msg);
    },
    /*
        功能：成功的提示
        参数：
            - msg：提示的内容（必须）
            - confirmCallback：确认按钮的执行事件（可选）
    */
    'alertSuccess': function (msg, confirmCallback) {
        args = {
            'title': '提示',
            'text': msg,
            'type': 'success',
        }
        swal(args, confirmCallback);
    },
    /*
        功能：带有标题的成功提示
        参数：
            - title：提示框的标题（必须）
            - msg：提示的内容（必须）
    */
    'alertSuccessWithTitle': function (title, msg) {
        swal(title, msg, 'success');
    },
    /*
        功能：确认提示
        参数：字典的形式
            - title：提示框标题（可选）
            - type：提示框的类型（可选）
            - confirmText：确认按钮文本（可选）
            - cancelText：取消按钮文本（可选）
            - msg：提示框内容（必须）
            - confirmCallback：确认按钮点击回调（可选）
            - cancelCallback：取消按钮点击回调（可选）
    */
    'alertConfirm': function (params) {
        swal({
            'title': params['title'] ? params['title'] : '提示',
            'showCancelButton': true,
            'showConfirmButton': true,
            'type': params['type'] ? params['type'] : '',
            'confirmButtonText': params['confirmText'] ? params['confirmText'] : '确定',
            'cancelButtonText': params['cancelText'] ? params['cancelText'] : '取消',
            'text': params['msg'] ? params['msg'] : ''
        }, function (isConfirm) {
            if (isConfirm) {
                if (params['confirmCallback']) {
                    params['confirmCallback']();
                }
            } else {
                if (params['cancelCallback']) {
                    params['cancelCallback']();
                }
            }
        });
    },
    /*
        功能：带有一个输入框的提示
        参数：字典的形式
            - title：提示框的标题（可选）
            - text：提示框的内容（可选）
            - placeholder：输入框的占位文字（可选）
            - confirmText：确认按钮文字（可选）
            - cancelText：取消按钮文字（可选）
            - confirmCallback：确认后的执行事件
    */
    'alertOneInput': function (params) {
        swal({
            'title': params['title'] ? params['title'] : '请输入',
            'text': params['text'] ? params['text'] : '',
            'type': 'input',
            'showCancelButton': true,
            'animation': 'slide-from-top',
            'closeOnConfirm': false,
            'showLoaderOnConfirm': true,
            'inputPlaceholder': params['placeholder'] ? params['placeholder'] : '',
            'confirmButtonText': params['confirmText'] ? params['confirmText'] : '确定',
            'cancelButtonText': params['cancelText'] ? params['cancelText'] : '取消',
        }, function (inputValue) {
            if (inputValue === false) return false;
            if (inputValue === '') {
                swal.showInputError('输入框不能为空！');
                return false;
            }
            if (params['confirmCallback']) {
                params['confirmCallback'](inputValue);
            }
        });
    },
    /*
        功能：网络错误提示
        参数：无
    */
    'alertNetworkError': function () {
        this.alertErrorToast('网络错误');
    },
    /*
        功能：信息toast提示（1s后消失）
        参数：
            - msg：提示消息
    */
    'alertInfoToast': function (msg) {
        this.alertToast(msg, 'info');
    },
    /*
        功能：错误toast提示（1s后消失）
        参数：
            - msg：提示消息
    */
    'alertErrorToast': function (msg) {
        this.alertToast(msg, 'error');
    },
    /*
        功能：成功toast提示（1s后消失）
        参数：
            - msg：提示消息
    */
    'alertSuccessToast': function (msg) {
        if (!msg) {
            msg = '成功！';
        }
        this.alertToast(msg, 'success');
    },
    /*
        功能：弹出toast（1s后消失）
        参数：
            - msg：提示消息
            - type：toast的类型
    */
    'alertToast': function (msg, type) {
        swal({
            'title': msg,
            'text': '',
            'type': type,
            'showCancelButton': false,
            'showConfirmButton': false,
            'timer': 1000,
        });
    },
    // 关闭当前对话框
    'close': function () {
        swal.close();
    }
};
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168
```

再创建sweetalert.css如下：

```css
body.stop-scrolling {
    height: 100%;
    overflow: hidden;
}

.sweet-overlay {
    background-color: black;
    /* IE8 */
    -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=40)";
    /* IE8 */
    background-color: rgba(0, 0, 0, 0.4);
    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    display: none;
    z-index: 10000;
}

.sweet-alert {
    background-color: white;
    font-family: 'Open Sans', 'Helvetica Neue', Helvetica, Arial, sans-serif;
    width: 478px;
    padding: 17px;
    border-radius: 5px;
    text-align: center;
    position: fixed;
    left: 50%;
    top: 50%;
    margin-left: -256px;
    margin-top: -200px;
    overflow: hidden;
    display: none;
    z-index: 99999;
}

@media all and (max-width: 540px) {
    .sweet-alert {
        width: auto;
        margin-left: 0;
        margin-right: 0;
        left: 15px;
        right: 15px;
    }
}

.sweet-alert h2 {
    color: #575757;
    font-size: 30px;
    text-align: center;
    font-weight: 600;
    text-transform: none;
    position: relative;
    margin: 25px 0;
    padding: 0;
    line-height: 40px;
    display: block;
}

.sweet-alert p {
    color: #797979;
    font-size: 16px;
    text-align: center;
    font-weight: 300;
    position: relative;
    text-align: inherit;
    float: none;
    margin: 0;
    padding: 0;
    line-height: normal;
}

.sweet-alert fieldset {
    border: none;
    position: relative;
}

.sweet-alert .sa-error-container {
    background-color: #f1f1f1;
    margin-left: -17px;
    margin-right: -17px;
    overflow: hidden;
    padding: 0 10px;
    max-height: 0;
    webkit-transition: padding 0.15s, max-height 0.15s;
    transition: padding 0.15s, max-height 0.15s;
}

.sweet-alert .sa-error-container.show {
    padding: 10px 0;
    max-height: 100px;
    webkit-transition: padding 0.2s, max-height 0.2s;
    transition: padding 0.25s, max-height 0.25s;
}

.sweet-alert .sa-error-container .icon {
    display: inline-block;
    width: 24px;
    height: 24px;
    border-radius: 50%;
    background-color: #ea7d7d;
    color: white;
    line-height: 24px;
    text-align: center;
    margin-right: 3px;
}

.sweet-alert .sa-error-container p {
    display: inline-block;
}

.sweet-alert .sa-input-error {
    position: absolute;
    top: 29px;
    right: 26px;
    width: 20px;
    height: 20px;
    opacity: 0;
    -webkit-transform: scale(0.5);
    transform: scale(0.5);
    -webkit-transform-origin: 50% 50%;
    transform-origin: 50% 50%;
    -webkit-transition: all 0.1s;
    transition: all 0.1s;
}

.sweet-alert .sa-input-error::before,
.sweet-alert .sa-input-error::after {
    content: "";
    width: 20px;
    height: 6px;
    background-color: #f06e57;
    border-radius: 3px;
    position: absolute;
    top: 50%;
    margin-top: -4px;
    left: 50%;
    margin-left: -9px;
}

.sweet-alert .sa-input-error::before {
    -webkit-transform: rotate(-45deg);
    transform: rotate(-45deg);
}

.sweet-alert .sa-input-error::after {
    -webkit-transform: rotate(45deg);
    transform: rotate(45deg);
}

.sweet-alert .sa-input-error.show {
    opacity: 1;
    -webkit-transform: scale(1);
    transform: scale(1);
}

.sweet-alert input {
    width: 100%;
    box-sizing: border-box;
    border-radius: 3px;
    border: 1px solid #d7d7d7;
    height: 43px;
    margin-top: 10px;
    margin-bottom: 17px;
    font-size: 18px;
    box-shadow: inset 0px 1px 1px rgba(0, 0, 0, 0.06);
    padding: 0 12px;
    display: none;
    -webkit-transition: all 0.3s;
    transition: all 0.3s;
}

.sweet-alert input:focus {
    outline: none;
    box-shadow: 0px 0px 3px #c4e6f5;
    border: 1px solid #b4dbed;
}

.sweet-alert input:focus::-moz-placeholder {
    transition: opacity 0.3s 0.03s ease;
    opacity: 0.5;
}

.sweet-alert input:focus:-ms-input-placeholder {
    transition: opacity 0.3s 0.03s ease;
    opacity: 0.5;
}

.sweet-alert input:focus::-webkit-input-placeholder {
    transition: opacity 0.3s 0.03s ease;
    opacity: 0.5;
}

.sweet-alert input::-moz-placeholder {
    color: #bdbdbd;
}

.sweet-alert input:-ms-input-placeholder {
    color: #bdbdbd;
}

.sweet-alert input::-webkit-input-placeholder {
    color: #bdbdbd;
}

.sweet-alert.show-input input {
    display: block;
}

.sweet-alert .sa-confirm-button-container {
    display: inline-block;
    position: relative;
}

.sweet-alert .la-ball-fall {
    position: absolute;
    left: 50%;
    top: 50%;
    margin-left: -27px;
    margin-top: 4px;
    opacity: 0;
    visibility: hidden;
}

.sweet-alert button {
    background-color: #8CD4F5;
    color: white;
    border: none;
    box-shadow: none;
    font-size: 17px;
    font-weight: 500;
    -webkit-border-radius: 4px;
    border-radius: 5px;
    padding: 10px 32px;
    margin: 26px 5px 0 5px;
    cursor: pointer;
}

.sweet-alert button:focus {
    outline: none;
    box-shadow: 0 0 2px rgba(128, 179, 235, 0.5), inset 0 0 0 1px rgba(0, 0, 0, 0.05);
}

.sweet-alert button:hover {
    background-color: #7ecff4;
}

.sweet-alert button:active {
    background-color: #5dc2f1;
}

.sweet-alert button.cancel {
    background-color: #C1C1C1;
}

.sweet-alert button.cancel:hover {
    background-color: #b9b9b9;
}

.sweet-alert button.cancel:active {
    background-color: #a8a8a8;
}

.sweet-alert button.cancel:focus {
    box-shadow: rgba(197, 205, 211, 0.8) 0px 0px 2px, rgba(0, 0, 0, 0.0470588) 0px 0px 0px 1px inset !important;
}

.sweet-alert button[disabled] {
    opacity: .6;
    cursor: default;
}

.sweet-alert button.confirm[disabled] {
    color: transparent;
}

.sweet-alert button.confirm[disabled] ~ .la-ball-fall {
    opacity: 1;
    visibility: visible;
    transition-delay: 0s;
}

.sweet-alert button::-moz-focus-inner {
    border: 0;
}

.sweet-alert[data-has-cancel-button=false] button {
    box-shadow: none !important;
}

.sweet-alert[data-has-confirm-button=false][data-has-cancel-button=false] {
    padding-bottom: 40px;
}

.sweet-alert .sa-icon {
    width: 80px;
    height: 80px;
    border: 4px solid gray;
    -webkit-border-radius: 40px;
    border-radius: 40px;
    border-radius: 50%;
    margin: 20px auto;
    padding: 0;
    position: relative;
    box-sizing: content-box;
}

.sweet-alert .sa-icon.sa-error {
    border-color: #F27474;
}

.sweet-alert .sa-icon.sa-error .sa-x-mark {
    position: relative;
    display: block;
}

.sweet-alert .sa-icon.sa-error .sa-line {
    position: absolute;
    height: 5px;
    width: 47px;
    background-color: #F27474;
    display: block;
    top: 37px;
    border-radius: 2px;
}

.sweet-alert .sa-icon.sa-error .sa-line.sa-left {
    -webkit-transform: rotate(45deg);
    transform: rotate(45deg);
    left: 17px;
}

.sweet-alert .sa-icon.sa-error .sa-line.sa-right {
    -webkit-transform: rotate(-45deg);
    transform: rotate(-45deg);
    right: 16px;
}

.sweet-alert .sa-icon.sa-warning {
    border-color: #F8BB86;
}

.sweet-alert .sa-icon.sa-warning .sa-body {
    position: absolute;
    width: 5px;
    height: 47px;
    left: 50%;
    top: 10px;
    -webkit-border-radius: 2px;
    border-radius: 2px;
    margin-left: -2px;
    background-color: #F8BB86;
}

.sweet-alert .sa-icon.sa-warning .sa-dot {
    position: absolute;
    width: 7px;
    height: 7px;
    -webkit-border-radius: 50%;
    border-radius: 50%;
    margin-left: -3px;
    left: 50%;
    bottom: 10px;
    background-color: #F8BB86;
}

.sweet-alert .sa-icon.sa-info {
    border-color: #C9DAE1;
}

.sweet-alert .sa-icon.sa-info::before {
    content: "";
    position: absolute;
    width: 5px;
    height: 29px;
    left: 50%;
    bottom: 17px;
    border-radius: 2px;
    margin-left: -2px;
    background-color: #C9DAE1;
}

.sweet-alert .sa-icon.sa-info::after {
    content: "";
    position: absolute;
    width: 7px;
    height: 7px;
    border-radius: 50%;
    margin-left: -3px;
    top: 19px;
    background-color: #C9DAE1;
    left: 50%;
}

.sweet-alert .sa-icon.sa-success {
    border-color: #A5DC86;
}

.sweet-alert .sa-icon.sa-success::before,
.sweet-alert .sa-icon.sa-success::after {
    content: '';
    -webkit-border-radius: 40px;
    border-radius: 40px;
    border-radius: 50%;
    position: absolute;
    width: 60px;
    height: 120px;
    background: white;
    -webkit-transform: rotate(45deg);
    transform: rotate(45deg);
}

.sweet-alert .sa-icon.sa-success::before {
    -webkit-border-radius: 120px 0 0 120px;
    border-radius: 120px 0 0 120px;
    top: -7px;
    left: -33px;
    -webkit-transform: rotate(-45deg);
    transform: rotate(-45deg);
    -webkit-transform-origin: 60px 60px;
    transform-origin: 60px 60px;
}

.sweet-alert .sa-icon.sa-success::after {
    -webkit-border-radius: 0 120px 120px 0;
    border-radius: 0 120px 120px 0;
    top: -11px;
    left: 30px;
    -webkit-transform: rotate(-45deg);
    transform: rotate(-45deg);
    -webkit-transform-origin: 0px 60px;
    transform-origin: 0px 60px;
}

.sweet-alert .sa-icon.sa-success .sa-placeholder {
    width: 80px;
    height: 80px;
    border: 4px solid rgba(165, 220, 134, 0.2);
    -webkit-border-radius: 40px;
    border-radius: 40px;
    border-radius: 50%;
    box-sizing: content-box;
    position: absolute;
    left: -4px;
    top: -4px;
    z-index: 2;
}

.sweet-alert .sa-icon.sa-success .sa-fix {
    width: 5px;
    height: 90px;
    background-color: white;
    position: absolute;
    left: 28px;
    top: 8px;
    z-index: 1;
    -webkit-transform: rotate(-45deg);
    transform: rotate(-45deg);
}

.sweet-alert .sa-icon.sa-success .sa-line {
    height: 5px;
    background-color: #A5DC86;
    display: block;
    border-radius: 2px;
    position: absolute;
    z-index: 2;
}

.sweet-alert .sa-icon.sa-success .sa-line.sa-tip {
    width: 25px;
    left: 14px;
    top: 46px;
    -webkit-transform: rotate(45deg);
    transform: rotate(45deg);
}

.sweet-alert .sa-icon.sa-success .sa-line.sa-long {
    width: 47px;
    right: 8px;
    top: 38px;
    -webkit-transform: rotate(-45deg);
    transform: rotate(-45deg);
}

.sweet-alert .sa-icon.sa-custom {
    background-size: contain;
    border-radius: 0;
    border: none;
    background-position: center center;
    background-repeat: no-repeat;
}

/*
 * Animations
 */
@-webkit-keyframes showSweetAlert {
    0% {
        transform: scale(0.7);
        -webkit-transform: scale(0.7);
    }

    45% {
        transform: scale(1.05);
        -webkit-transform: scale(1.05);
    }

    80% {
        transform: scale(0.95);
        -webkit-transform: scale(0.95);
    }

    100% {
        transform: scale(1);
        -webkit-transform: scale(1);
    }
}

@keyframes showSweetAlert {
    0% {
        transform: scale(0.7);
        -webkit-transform: scale(0.7);
    }

    45% {
        transform: scale(1.05);
        -webkit-transform: scale(1.05);
    }

    80% {
        transform: scale(0.95);
        -webkit-transform: scale(0.95);
    }

    100% {
        transform: scale(1);
        -webkit-transform: scale(1);
    }
}

@-webkit-keyframes hideSweetAlert {
    0% {
        transform: scale(1);
        -webkit-transform: scale(1);
    }

    100% {
        transform: scale(0.5);
        -webkit-transform: scale(0.5);
    }
}

@keyframes hideSweetAlert {
    0% {
        transform: scale(1);
        -webkit-transform: scale(1);
    }

    100% {
        transform: scale(0.5);
        -webkit-transform: scale(0.5);
    }
}

@-webkit-keyframes slideFromTop {
    0% {
        top: 0%;
    }

    100% {
        top: 50%;
    }
}

@keyframes slideFromTop {
    0% {
        top: 0%;
    }

    100% {
        top: 50%;
    }
}

@-webkit-keyframes slideToTop {
    0% {
        top: 50%;
    }

    100% {
        top: 0%;
    }
}

@keyframes slideToTop {
    0% {
        top: 50%;
    }

    100% {
        top: 0%;
    }
}

@-webkit-keyframes slideFromBottom {
    0% {
        top: 70%;
    }

    100% {
        top: 50%;
    }
}

@keyframes slideFromBottom {
    0% {
        top: 70%;
    }

    100% {
        top: 50%;
    }
}

@-webkit-keyframes slideToBottom {
    0% {
        top: 50%;
    }

    100% {
        top: 70%;
    }
}

@keyframes slideToBottom {
    0% {
        top: 50%;
    }

    100% {
        top: 70%;
    }
}

.showSweetAlert[data-animation=pop] {
    -webkit-animation: showSweetAlert 0.3s;
    animation: showSweetAlert 0.3s;
}

.showSweetAlert[data-animation=none] {
    -webkit-animation: none;
    animation: none;
}

.showSweetAlert[data-animation=slide-from-top] {
    -webkit-animation: slideFromTop 0.3s;
    animation: slideFromTop 0.3s;
}

.showSweetAlert[data-animation=slide-from-bottom] {
    -webkit-animation: slideFromBottom 0.3s;
    animation: slideFromBottom 0.3s;
}

.hideSweetAlert[data-animation=pop] {
    -webkit-animation: hideSweetAlert 0.2s;
    animation: hideSweetAlert 0.2s;
}

.hideSweetAlert[data-animation=none] {
    -webkit-animation: none;
    animation: none;
}

.hideSweetAlert[data-animation=slide-from-top] {
    -webkit-animation: slideToTop 0.4s;
    animation: slideToTop 0.4s;
}

.hideSweetAlert[data-animation=slide-from-bottom] {
    -webkit-animation: slideToBottom 0.3s;
    animation: slideToBottom 0.3s;
}

@-webkit-keyframes animateSuccessTip {
    0% {
        width: 0;
        left: 1px;
        top: 19px;
    }

    54% {
        width: 0;
        left: 1px;
        top: 19px;
    }

    70% {
        width: 50px;
        left: -8px;
        top: 37px;
    }

    84% {
        width: 17px;
        left: 21px;
        top: 48px;
    }

    100% {
        width: 25px;
        left: 14px;
        top: 45px;
    }
}

@keyframes animateSuccessTip {
    0% {
        width: 0;
        left: 1px;
        top: 19px;
    }

    54% {
        width: 0;
        left: 1px;
        top: 19px;
    }

    70% {
        width: 50px;
        left: -8px;
        top: 37px;
    }

    84% {
        width: 17px;
        left: 21px;
        top: 48px;
    }

    100% {
        width: 25px;
        left: 14px;
        top: 45px;
    }
}

@-webkit-keyframes animateSuccessLong {
    0% {
        width: 0;
        right: 46px;
        top: 54px;
    }

    65% {
        width: 0;
        right: 46px;
        top: 54px;
    }

    84% {
        width: 55px;
        right: 0px;
        top: 35px;
    }

    100% {
        width: 47px;
        right: 8px;
        top: 38px;
    }
}

@keyframes animateSuccessLong {
    0% {
        width: 0;
        right: 46px;
        top: 54px;
    }

    65% {
        width: 0;
        right: 46px;
        top: 54px;
    }

    84% {
        width: 55px;
        right: 0px;
        top: 35px;
    }

    100% {
        width: 47px;
        right: 8px;
        top: 38px;
    }
}

@-webkit-keyframes rotatePlaceholder {
    0% {
        transform: rotate(-45deg);
        -webkit-transform: rotate(-45deg);
    }

    5% {
        transform: rotate(-45deg);
        -webkit-transform: rotate(-45deg);
    }

    12% {
        transform: rotate(-405deg);
        -webkit-transform: rotate(-405deg);
    }

    100% {
        transform: rotate(-405deg);
        -webkit-transform: rotate(-405deg);
    }
}

@keyframes rotatePlaceholder {
    0% {
        transform: rotate(-45deg);
        -webkit-transform: rotate(-45deg);
    }

    5% {
        transform: rotate(-45deg);
        -webkit-transform: rotate(-45deg);
    }

    12% {
        transform: rotate(-405deg);
        -webkit-transform: rotate(-405deg);
    }

    100% {
        transform: rotate(-405deg);
        -webkit-transform: rotate(-405deg);
    }
}

.animateSuccessTip {
    -webkit-animation: animateSuccessTip 0.75s;
    animation: animateSuccessTip 0.75s;
}

.animateSuccessLong {
    -webkit-animation: animateSuccessLong 0.75s;
    animation: animateSuccessLong 0.75s;
}

.sa-icon.sa-success.animate::after {
    -webkit-animation: rotatePlaceholder 4.25s ease-in;
    animation: rotatePlaceholder 4.25s ease-in;
}

@-webkit-keyframes animateErrorIcon {
    0% {
        transform: rotateX(100deg);
        -webkit-transform: rotateX(100deg);
        opacity: 0;
    }

    100% {
        transform: rotateX(0deg);
        -webkit-transform: rotateX(0deg);
        opacity: 1;
    }
}

@keyframes animateErrorIcon {
    0% {
        transform: rotateX(100deg);
        -webkit-transform: rotateX(100deg);
        opacity: 0;
    }

    100% {
        transform: rotateX(0deg);
        -webkit-transform: rotateX(0deg);
        opacity: 1;
    }
}

.animateErrorIcon {
    -webkit-animation: animateErrorIcon 0.5s;
    animation: animateErrorIcon 0.5s;
}

@-webkit-keyframes animateXMark {
    0% {
        transform: scale(0.4);
        -webkit-transform: scale(0.4);
        margin-top: 26px;
        opacity: 0;
    }

    50% {
        transform: scale(0.4);
        -webkit-transform: scale(0.4);
        margin-top: 26px;
        opacity: 0;
    }

    80% {
        transform: scale(1.15);
        -webkit-transform: scale(1.15);
        margin-top: -6px;
    }

    100% {
        transform: scale(1);
        -webkit-transform: scale(1);
        margin-top: 0;
        opacity: 1;
    }
}

@keyframes animateXMark {
    0% {
        transform: scale(0.4);
        -webkit-transform: scale(0.4);
        margin-top: 26px;
        opacity: 0;
    }

    50% {
        transform: scale(0.4);
        -webkit-transform: scale(0.4);
        margin-top: 26px;
        opacity: 0;
    }

    80% {
        transform: scale(1.15);
        -webkit-transform: scale(1.15);
        margin-top: -6px;
    }

    100% {
        transform: scale(1);
        -webkit-transform: scale(1);
        margin-top: 0;
        opacity: 1;
    }
}

.animateXMark {
    -webkit-animation: animateXMark 0.5s;
    animation: animateXMark 0.5s;
}

@-webkit-keyframes pulseWarning {
    0% {
        border-color: #F8D486;
    }

    100% {
        border-color: #F8BB86;
    }
}

@keyframes pulseWarning {
    0% {
        border-color: #F8D486;
    }

    100% {
        border-color: #F8BB86;
    }
}

.pulseWarning {
    -webkit-animation: pulseWarning 0.75s infinite alternate;
    animation: pulseWarning 0.75s infinite alternate;
}

@-webkit-keyframes pulseWarningIns {
    0% {
        background-color: #F8D486;
    }

    100% {
        background-color: #F8BB86;
    }
}

@keyframes pulseWarningIns {
    0% {
        background-color: #F8D486;
    }

    100% {
        background-color: #F8BB86;
    }
}

.pulseWarningIns {
    -webkit-animation: pulseWarningIns 0.75s infinite alternate;
    animation: pulseWarningIns 0.75s infinite alternate;
}

@-webkit-keyframes rotate-loading {
    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }
}

@keyframes rotate-loading {
    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }
}

/* Internet Explorer 9 has some special quirks that are fixed here */
/* The icons are not animated. */
/* This file is automatically merged into sweet-alert.min.js through Gulp */
/* Error icon */
.sweet-alert .sa-icon.sa-error .sa-line.sa-left {
    -ms-transform: rotate(45deg) \9;
}

.sweet-alert .sa-icon.sa-error .sa-line.sa-right {
    -ms-transform: rotate(-45deg) \9;
}

/* Success icon */
.sweet-alert .sa-icon.sa-success {
    border-color: transparent \9;
}

.sweet-alert .sa-icon.sa-success .sa-line.sa-tip {
    -ms-transform: rotate(45deg) \9;
}

.sweet-alert .sa-icon.sa-success .sa-line.sa-long {
    -ms-transform: rotate(-45deg) \9;
}

/*!
 * Load Awesome v1.1.0 (http://github.danielcardoso.net/load-awesome/)
 * Copyright 2015 Daniel Cardoso <@DanielCardoso>
 * Licensed under MIT
 */
.la-ball-fall,
.la-ball-fall > div {
    position: relative;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
}

.la-ball-fall {
    display: block;
    font-size: 0;
    color: #fff;
}

.la-ball-fall.la-dark {
    color: #333;
}

.la-ball-fall > div {
    display: inline-block;
    float: none;
    background-color: currentColor;
    border: 0 solid currentColor;
}

.la-ball-fall {
    width: 54px;
    height: 18px;
}

.la-ball-fall > div {
    width: 10px;
    height: 10px;
    margin: 4px;
    border-radius: 100%;
    opacity: 0;
    -webkit-animation: ball-fall 1s ease-in-out infinite;
    -moz-animation: ball-fall 1s ease-in-out infinite;
    -o-animation: ball-fall 1s ease-in-out infinite;
    animation: ball-fall 1s ease-in-out infinite;
}

.la-ball-fall > div:nth-child(1) {
    -webkit-animation-delay: -200ms;
    -moz-animation-delay: -200ms;
    -o-animation-delay: -200ms;
    animation-delay: -200ms;
}

.la-ball-fall > div:nth-child(2) {
    -webkit-animation-delay: -100ms;
    -moz-animation-delay: -100ms;
    -o-animation-delay: -100ms;
    animation-delay: -100ms;
}

.la-ball-fall > div:nth-child(3) {
    -webkit-animation-delay: 0ms;
    -moz-animation-delay: 0ms;
    -o-animation-delay: 0ms;
    animation-delay: 0ms;
}

.la-ball-fall.la-sm {
    width: 26px;
    height: 8px;
}

.la-ball-fall.la-sm > div {
    width: 4px;
    height: 4px;
    margin: 2px;
}

.la-ball-fall.la-2x {
    width: 108px;
    height: 36px;
}

.la-ball-fall.la-2x > div {
    width: 20px;
    height: 20px;
    margin: 8px;
}

.la-ball-fall.la-3x {
    width: 162px;
    height: 54px;
}

.la-ball-fall.la-3x > div {
    width: 30px;
    height: 30px;
    margin: 12px;
}

/*
 * Animation
 */
@-webkit-keyframes ball-fall {
    0% {
        opacity: 0;
        -webkit-transform: translateY(-145%);
        transform: translateY(-145%);
    }

    10% {
        opacity: .5;
    }

    20% {
        opacity: 1;
        -webkit-transform: translateY(0);
        transform: translateY(0);
    }

    80% {
        opacity: 1;
        -webkit-transform: translateY(0);
        transform: translateY(0);
    }

    90% {
        opacity: .5;
    }

    100% {
        opacity: 0;
        -webkit-transform: translateY(145%);
        transform: translateY(145%);
    }
}

@-moz-keyframes ball-fall {
    0% {
        opacity: 0;
        -moz-transform: translateY(-145%);
        transform: translateY(-145%);
    }

    10% {
        opacity: .5;
    }

    20% {
        opacity: 1;
        -moz-transform: translateY(0);
        transform: translateY(0);
    }

    80% {
        opacity: 1;
        -moz-transform: translateY(0);
        transform: translateY(0);
    }

    90% {
        opacity: .5;
    }

    100% {
        opacity: 0;
        -moz-transform: translateY(145%);
        transform: translateY(145%);
    }
}

@-o-keyframes ball-fall {
    0% {
        opacity: 0;
        -o-transform: translateY(-145%);
        transform: translateY(-145%);
    }

    10% {
        opacity: .5;
    }

    20% {
        opacity: 1;
        -o-transform: translateY(0);
        transform: translateY(0);
    }

    80% {
        opacity: 1;
        -o-transform: translateY(0);
        transform: translateY(0);
    }

    90% {
        opacity: .5;
    }

    100% {
        opacity: 0;
        -o-transform: translateY(145%);
        transform: translateY(145%);
    }
}

@keyframes ball-fall {
    0% {
        opacity: 0;
        -webkit-transform: translateY(-145%);
        -moz-transform: translateY(-145%);
        -o-transform: translateY(-145%);
        transform: translateY(-145%);
    }

    10% {
        opacity: .5;
    }

    20% {
        opacity: 1;
        -webkit-transform: translateY(0);
        -moz-transform: translateY(0);
        -o-transform: translateY(0);
        transform: translateY(0);
    }

    80% {
        opacity: 1;
        -webkit-transform: translateY(0);
        -moz-transform: translateY(0);
        -o-transform: translateY(0);
        transform: translateY(0);
    }

    90% {
        opacity: .5;
    }

    100% {
        opacity: 0;
        -webkit-transform: translateY(145%);
        -moz-transform: translateY(145%);
        -o-transform: translateY(145%);
        transform: translateY(145%);
    }
}
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879808182838485868788899091929394959697989910010110210310410510610710810911011111211311411511611711811912012112212312412512612712812913013113213313413513613713813914014114214314414514614714814915015115215315415515615715815916016116216316416516616716816917017117217317417517617717817918018118218318418518618718818919019119219319419519619719819920020120220320420520620720820921021121221321421521621721821922022122222322422522622722822923023123223323423523623723823924024124224324424524624724824925025125225325425525625725825926026126226326426526626726826927027127227327427527627727827928028128228328428528628728828929029129229329429529629729829930030130230330430530630730830931031131231331431531631731831932032132232332432532632732832933033133233333433533633733833934034134234334434534634734834935035135235335435535635735835936036136236336436536636736836937037137237337437537637737837938038138238338438538638738838939039139239339439539639739839940040140240340440540640740840941041141241341441541641741841942042142242342442542642742842943043143243343443543643743843944044144244344444544644744844945045145245345445545645745845946046146246346446546646746846947047147247347447547647747847948048148248348448548648748848949049149249349449549649749849950050150250350450550650750850951051151251351451551651751851952052152252352452552652752852953053153253353453553653753853954054154254354454554654754854955055155255355455555655755855956056156256356456556656756856957057157257357457557657757857958058158258358458558658758858959059159259359459559659759859960060160260360460560660760860961061161261361461561661761861962062162262362462562662762862963063163263363463563663763863964064164264364464564664764864965065165265365465565665765865966066166266366466566666766866967067167267367467567667767867968068168268368468568668768868969069169269369469569669769869970070170270370470570670770870971071171271371471571671771871972072172272372472572672772872973073173273373473573673773873974074174274374474574674774874975075175275375475575675775875976076176276376476576676776876977077177277377477577677777877978078178278378478578678778878979079179279379479579679779879980080180280380480580680780880981081181281381481581681781881982082182282382482582682782882983083183283383483583683783883984084184284384484584684784884985085185285385485585685785885986086186286386486586686786886987087187287387487587687787887988088188288388488588688788888989089189289389489589689789889990090190290390490590690790890991091191291391491591691791891992092192292392492592692792892993093193293393493593693793893994094194294394494594694794894995095195295395495595695795895996096196296396496596696796896997097197297397497597697797897998098198298398498598698798898999099199299399499599699799899910001001100210031004100510061007100810091010101110121013101410151016101710181019102010211022102310241025102610271028102910301031103210331034103510361037103810391040104110421043104410451046104710481049105010511052105310541055105610571058105910601061106210631064106510661067106810691070107110721073107410751076107710781079108010811082108310841085108610871088108910901091109210931094109510961097109810991100110111021103110411051106110711081109111011111112111311141115111611171118111911201121112211231124112511261127112811291130113111321133113411351136113711381139114011411142114311441145114611471148114911501151115211531154115511561157115811591160116111621163116411651166116711681169117011711172117311741175117611771178117911801181118211831184118511861187118811891190119111921193119411951196119711981199120012011202120312041205120612071208120912101211121212131214121512161217121812191220122112221223122412251226122712281229123012311232123312341235123612371238123912401241124212431244124512461247124812491250125112521253125412551256125712581259126012611262126312641265126612671268126912701271127212731274127512761277127812791280128112821283128412851286128712881289129012911292129312941295129612971298
```

再创建sweetalert.min.js如下：

```js
!function (e, t, n) {
    "use strict";
    !function o(e, t, n) {
        function a(s, l) {
            if (!t[s]) {
                if (!e[s]) {
                    var i = "function" == typeof require && require;
                    if (!l && i) return i(s, !0);
                    if (r) return r(s, !0);
                    var u = new Error("Cannot find module '" + s + "'");
                    throw u.code = "MODULE_NOT_FOUND", u
                }
                var c = t[s] = {exports: {}};
                e[s][0].call(c.exports, function (t) {
                    var n = e[s][1][t];
                    return a(n ? n : t)
                }, c, c.exports, o, e, t, n)
            }
            return t[s].exports
        }

        for (var r = "function" == typeof require && require, s = 0; s < n.length; s++) a(n[s]);
        return a
    }({
        1: [function (o, a, r) {
            function s(e) {
                return e && e.__esModule ? e : {"default": e}
            }

            Object.defineProperty(r, "__esModule", {value: !0});
            var l, i, u, c, d = o("./modules/handle-dom"), f = o("./modules/utils"), p = o("./modules/handle-swal-dom"),
                m = o("./modules/handle-click"), v = o("./modules/handle-key"), y = s(v),
                b = o("./modules/default-params"), h = s(b), g = o("./modules/set-params"), w = s(g);
            r["default"] = u = c = function () {
                function o(e) {
                    var t = a;
                    return t[e] === n ? h["default"][e] : t[e]
                }

                var a = arguments[0];
                if ((0, d.addClass)(t.body, "stop-scrolling"), (0, p.resetInput)(), a === n) return (0, f.logStr)("SweetAlert expects at least 1 attribute!"), !1;
                var r = (0, f.extend)({}, h["default"]);
                switch (typeof a) {
                    case"string":
                        r.title = a, r.text = arguments[1] || "", r.type = arguments[2] || "";
                        break;
                    case"object":
                        if (a.title === n) return (0, f.logStr)('Missing "title" argument!'), !1;
                        r.title = a.title;
                        for (var s in h["default"]) r[s] = o(s);
                        r.confirmButtonText = r.showCancelButton ? "Confirm" : h["default"].confirmButtonText, r.confirmButtonText = o("confirmButtonText"), r.doneFunction = arguments[1] || null;
                        break;
                    default:
                        return (0, f.logStr)('Unexpected type of argument! Expected "string" or "object", got ' + typeof a), !1
                }
                (0, w["default"])(r), (0, p.fixVerticalPosition)(), (0, p.openModal)(arguments[1]);
                for (var u = (0, p.getModal)(), v = u.querySelectorAll("button"), b = ["onclick", "onmouseover", "onmouseout", "onmousedown", "onmouseup", "onfocus"], g = function (e) {
                    return (0, m.handleButton)(e, r, u)
                }, C = 0; C < v.length; C++) for (var S = 0; S < b.length; S++) {
                    var x = b[S];
                    v[C][x] = g
                }
                (0, p.getOverlay)().onclick = g, l = e.onkeydown;
                var k = function (e) {
                    return (0, y["default"])(e, r, u)
                };
                e.onkeydown = k, e.onfocus = function () {
                    setTimeout(function () {
                        i !== n && (i.focus(), i = n)
                    }, 0)
                }, c.enableButtons()
            }, u.setDefaults = c.setDefaults = function (e) {
                if (!e) throw new Error("userParams is required");
                if ("object" != typeof e) throw new Error("userParams has to be a object");
                (0, f.extend)(h["default"], e)
            }, u.close = c.close = function () {
                var o = (0, p.getModal)();
                (0, d.fadeOut)((0, p.getOverlay)(), 5), (0, d.fadeOut)(o, 5), (0, d.removeClass)(o, "showSweetAlert"), (0, d.addClass)(o, "hideSweetAlert"), (0, d.removeClass)(o, "visible");
                var a = o.querySelector(".sa-icon.sa-success");
                (0, d.removeClass)(a, "animate"), (0, d.removeClass)(a.querySelector(".sa-tip"), "animateSuccessTip"), (0, d.removeClass)(a.querySelector(".sa-long"), "animateSuccessLong");
                var r = o.querySelector(".sa-icon.sa-error");
                (0, d.removeClass)(r, "animateErrorIcon"), (0, d.removeClass)(r.querySelector(".sa-x-mark"), "animateXMark");
                var s = o.querySelector(".sa-icon.sa-warning");
                return (0, d.removeClass)(s, "pulseWarning"), (0, d.removeClass)(s.querySelector(".sa-body"), "pulseWarningIns"), (0, d.removeClass)(s.querySelector(".sa-dot"), "pulseWarningIns"), setTimeout(function () {
                    var e = o.getAttribute("data-custom-class");
                    (0, d.removeClass)(o, e)
                }, 300), (0, d.removeClass)(t.body, "stop-scrolling"), e.onkeydown = l, e.previousActiveElement && e.previousActiveElement.focus(), i = n, clearTimeout(o.timeout), !0
            }, u.showInputError = c.showInputError = function (e) {
                var t = (0, p.getModal)(), n = t.querySelector(".sa-input-error");
                (0, d.addClass)(n, "show");
                var o = t.querySelector(".sa-error-container");
                (0, d.addClass)(o, "show"), o.querySelector("p").innerHTML = e, setTimeout(function () {
                    u.enableButtons()
                }, 1), t.querySelector("input").focus()
            }, u.resetInputError = c.resetInputError = function (e) {
                if (e && 13 === e.keyCode) return !1;
                var t = (0, p.getModal)(), n = t.querySelector(".sa-input-error");
                (0, d.removeClass)(n, "show");
                var o = t.querySelector(".sa-error-container");
                (0, d.removeClass)(o, "show")
            }, u.disableButtons = c.disableButtons = function (e) {
                var t = (0, p.getModal)(), n = t.querySelector("button.confirm"), o = t.querySelector("button.cancel");
                n.disabled = !0, o.disabled = !0
            }, u.enableButtons = c.enableButtons = function (e) {
                var t = (0, p.getModal)(), n = t.querySelector("button.confirm"), o = t.querySelector("button.cancel");
                n.disabled = !1, o.disabled = !1
            }, "undefined" != typeof e ? e.sweetAlert = e.swal = u : (0, f.logStr)("SweetAlert is a frontend module!"), a.exports = r["default"]
        }, {
            "./modules/default-params": 2,
            "./modules/handle-click": 3,
            "./modules/handle-dom": 4,
            "./modules/handle-key": 5,
            "./modules/handle-swal-dom": 6,
            "./modules/set-params": 8,
            "./modules/utils": 9
        }], 2: [function (e, t, n) {
            Object.defineProperty(n, "__esModule", {value: !0});
            var o = {
                title: "",
                text: "",
                type: null,
                allowOutsideClick: !1,
                showConfirmButton: !0,
                showCancelButton: !1,
                closeOnConfirm: !0,
                closeOnCancel: !0,
                confirmButtonText: "OK",
                confirmButtonColor: "#8CD4F5",
                cancelButtonText: "Cancel",
                imageUrl: null,
                imageSize: null,
                timer: null,
                customClass: "",
                html: !1,
                animation: !0,
                allowEscapeKey: !0,
                inputType: "text",
                inputPlaceholder: "",
                inputValue: "",
                showLoaderOnConfirm: !1
            };
            n["default"] = o, t.exports = n["default"]
        }, {}], 3: [function (t, n, o) {
            Object.defineProperty(o, "__esModule", {value: !0});
            var a = t("./utils"), r = (t("./handle-swal-dom"), t("./handle-dom")), s = function (t, n, o) {
                function s(e) {
                    m && n.confirmButtonColor && (p.style.backgroundColor = e)
                }

                var u, c, d, f = t || e.event, p = f.target || f.srcElement, m = -1 !== p.className.indexOf("confirm"),
                    v = -1 !== p.className.indexOf("sweet-overlay"), y = (0, r.hasClass)(o, "visible"),
                    b = n.doneFunction && "true" === o.getAttribute("data-has-done-function");
                switch (m && n.confirmButtonColor && (u = n.confirmButtonColor, c = (0, a.colorLuminance)(u, -.04), d = (0, a.colorLuminance)(u, -.14)), f.type) {
                    case"mouseover":
                        s(c);
                        break;
                    case"mouseout":
                        s(u);
                        break;
                    case"mousedown":
                        s(d);
                        break;
                    case"mouseup":
                        s(c);
                        break;
                    case"focus":
                        var h = o.querySelector("button.confirm"), g = o.querySelector("button.cancel");
                        m ? g.style.boxShadow = "none" : h.style.boxShadow = "none";
                        break;
                    case"click":
                        var w = o === p, C = (0, r.isDescendant)(o, p);
                        if (!w && !C && y && !n.allowOutsideClick) break;
                        m && b && y ? l(o, n) : b && y || v ? i(o, n) : (0, r.isDescendant)(o, p) && "BUTTON" === p.tagName && sweetAlert.close()
                }
            }, l = function (e, t) {
                var n = !0;
                (0, r.hasClass)(e, "show-input") && (n = e.querySelector("input").value, n || (n = "")), t.doneFunction(n), t.closeOnConfirm && sweetAlert.close(), t.showLoaderOnConfirm && sweetAlert.disableButtons()
            }, i = function (e, t) {
                var n = String(t.doneFunction).replace(/\s/g, ""),
                    o = "function(" === n.substring(0, 9) && ")" !== n.substring(9, 10);
                o && t.doneFunction(!1), t.closeOnCancel && sweetAlert.close()
            };
            o["default"] = {handleButton: s, handleConfirm: l, handleCancel: i}, n.exports = o["default"]
        }, {"./handle-dom": 4, "./handle-swal-dom": 6, "./utils": 9}], 4: [function (n, o, a) {
            Object.defineProperty(a, "__esModule", {value: !0});
            var r = function (e, t) {
                return new RegExp(" " + t + " ").test(" " + e.className + " ")
            }, s = function (e, t) {
                r(e, t) || (e.className += " " + t)
            }, l = function (e, t) {
                var n = " " + e.className.replace(/[\t\r\n]/g, " ") + " ";
                if (r(e, t)) {
                    for (; n.indexOf(" " + t + " ") >= 0;) n = n.replace(" " + t + " ", " ");
                    e.className = n.replace(/^\s+|\s+$/g, "")
                }
            }, i = function (e) {
                var n = t.createElement("div");
                return n.appendChild(t.createTextNode(e)), n.innerHTML
            }, u = function (e) {
                e.style.opacity = "", e.style.display = "block"
            }, c = function (e) {
                if (e && !e.length) return u(e);
                for (var t = 0; t < e.length; ++t) u(e[t])
            }, d = function (e) {
                e.style.opacity = "", e.style.display = "none"
            }, f = function (e) {
                if (e && !e.length) return d(e);
                for (var t = 0; t < e.length; ++t) d(e[t])
            }, p = function (e, t) {
                for (var n = t.parentNode; null !== n;) {
                    if (n === e) return !0;
                    n = n.parentNode
                }
                return !1
            }, m = function (e) {
                e.style.left = "-9999px", e.style.display = "block";
                var t, n = e.clientHeight;
                return t = "undefined" != typeof getComputedStyle ? parseInt(getComputedStyle(e).getPropertyValue("padding-top"), 10) : parseInt(e.currentStyle.padding), e.style.left = "", e.style.display = "none", "-" + parseInt((n + t) / 2) + "px"
            }, v = function (e, t) {
                if (+e.style.opacity < 1) {
                    t = t || 16, e.style.opacity = 0, e.style.display = "block";
                    var n = +new Date, o = function a() {
                        e.style.opacity = +e.style.opacity + (new Date - n) / 100, n = +new Date, +e.style.opacity < 1 && setTimeout(a, t)
                    };
                    o()
                }
                e.style.display = "block"
            }, y = function (e, t) {
                t = t || 16, e.style.opacity = 1;
                var n = +new Date, o = function a() {
                    e.style.opacity = +e.style.opacity - (new Date - n) / 100, n = +new Date, +e.style.opacity > 0 ? setTimeout(a, t) : e.style.display = "none"
                };
                o()
            }, b = function (n) {
                if ("function" == typeof MouseEvent) {
                    var o = new MouseEvent("click", {view: e, bubbles: !1, cancelable: !0});
                    n.dispatchEvent(o)
                } else if (t.createEvent) {
                    var a = t.createEvent("MouseEvents");
                    a.initEvent("click", !1, !1), n.dispatchEvent(a)
                } else t.createEventObject ? n.fireEvent("onclick") : "function" == typeof n.onclick && n.onclick()
            }, h = function (t) {
                "function" == typeof t.stopPropagation ? (t.stopPropagation(), t.preventDefault()) : e.event && e.event.hasOwnProperty("cancelBubble") && (e.event.cancelBubble = !0)
            };
            a.hasClass = r, a.addClass = s, a.removeClass = l, a.escapeHtml = i, a._show = u, a.show = c, a._hide = d, a.hide = f, a.isDescendant = p, a.getTopMargin = m, a.fadeIn = v, a.fadeOut = y, a.fireClick = b, a.stopEventPropagation = h
        }, {}], 5: [function (t, o, a) {
            Object.defineProperty(a, "__esModule", {value: !0});
            var r = t("./handle-dom"), s = t("./handle-swal-dom"), l = function (t, o, a) {
                var l = t || e.event, i = l.keyCode || l.which, u = a.querySelector("button.confirm"),
                    c = a.querySelector("button.cancel"), d = a.querySelectorAll("button[tabindex]");
                if (-1 !== [9, 13, 32, 27].indexOf(i)) {
                    for (var f = l.target || l.srcElement, p = -1, m = 0; m < d.length; m++) if (f === d[m]) {
                        p = m;
                        break
                    }
                    9 === i ? (f = -1 === p ? u : p === d.length - 1 ? d[0] : d[p + 1], (0, r.stopEventPropagation)(l), f.focus(), o.confirmButtonColor && (0, s.setFocusStyle)(f, o.confirmButtonColor)) : 13 === i ? ("INPUT" === f.tagName && (f = u, u.focus()), f = -1 === p ? u : n) : 27 === i && o.allowEscapeKey === !0 ? (f = c, (0, r.fireClick)(f, l)) : f = n
                }
            };
            a["default"] = l, o.exports = a["default"]
        }, {"./handle-dom": 4, "./handle-swal-dom": 6}], 6: [function (n, o, a) {
            function r(e) {
                return e && e.__esModule ? e : {"default": e}
            }

            Object.defineProperty(a, "__esModule", {value: !0});
            var s = n("./utils"), l = n("./handle-dom"), i = n("./default-params"), u = r(i), c = n("./injected-html"),
                d = r(c), f = ".sweet-alert", p = ".sweet-overlay", m = function () {
                    var e = t.createElement("div");
                    for (e.innerHTML = d["default"]; e.firstChild;) t.body.appendChild(e.firstChild)
                }, v = function x() {
                    var e = t.querySelector(f);
                    return e || (m(), e = x()), e
                }, y = function () {
                    var e = v();
                    return e ? e.querySelector("input") : void 0
                }, b = function () {
                    return t.querySelector(p)
                }, h = function (e, t) {
                    var n = (0, s.hexToRgb)(t);
                    e.style.boxShadow = "0 0 2px rgba(" + n + ", 0.8), inset 0 0 0 1px rgba(0, 0, 0, 0.05)"
                }, g = function (n) {
                    var o = v();
                    (0, l.fadeIn)(b(), 10), (0, l.show)(o), (0, l.addClass)(o, "showSweetAlert"), (0, l.removeClass)(o, "hideSweetAlert"), e.previousActiveElement = t.activeElement;
                    var a = o.querySelector("button.confirm");
                    a.focus(), setTimeout(function () {
                        (0, l.addClass)(o, "visible")
                    }, 500);
                    var r = o.getAttribute("data-timer");
                    if ("null" !== r && "" !== r) {
                        var s = n;
                        o.timeout = setTimeout(function () {
                            var e = (s || null) && "true" === o.getAttribute("data-has-done-function");
                            e ? s(null) : sweetAlert.close()
                        }, r)
                    }
                }, w = function () {
                    var e = v(), t = y();
                    (0, l.removeClass)(e, "show-input"), t.value = u["default"].inputValue, t.setAttribute("type", u["default"].inputType), t.setAttribute("placeholder", u["default"].inputPlaceholder), C()
                }, C = function (e) {
                    if (e && 13 === e.keyCode) return !1;
                    var t = v(), n = t.querySelector(".sa-input-error");
                    (0, l.removeClass)(n, "show");
                    var o = t.querySelector(".sa-error-container");
                    (0, l.removeClass)(o, "show")
                }, S = function () {
                    var e = v();
                    e.style.marginTop = (0, l.getTopMargin)(v())
                };
            a.sweetAlertInitialize = m, a.getModal = v, a.getOverlay = b, a.getInput = y, a.setFocusStyle = h, a.openModal = g, a.resetInput = w, a.resetInputError = C, a.fixVerticalPosition = S
        }, {"./default-params": 2, "./handle-dom": 4, "./injected-html": 7, "./utils": 9}], 7: [function (e, t, n) {
            Object.defineProperty(n, "__esModule", {value: !0});
            var o = '<div class="sweet-overlay" tabIndex="-1"></div><div class="sweet-alert"><div class="sa-icon sa-error">\n      <span class="sa-x-mark">\n        <span class="sa-line sa-left"></span>\n        <span class="sa-line sa-right"></span>\n      </span>\n    </div><div class="sa-icon sa-warning">\n      <span class="sa-body"></span>\n      <span class="sa-dot"></span>\n    </div><div class="sa-icon sa-info"></div><div class="sa-icon sa-success">\n      <span class="sa-line sa-tip"></span>\n      <span class="sa-line sa-long"></span>\n\n      <div class="sa-placeholder"></div>\n      <div class="sa-fix"></div>\n    </div><div class="sa-icon sa-custom"></div><h2>Title</h2>\n    <p>Text</p>\n    <fieldset>\n      <input type="text" tabIndex="3" />\n      <div class="sa-input-error"></div>\n    </fieldset><div class="sa-error-container">\n      <div class="icon">!</div>\n      <p>Not valid!</p>\n    </div><div class="sa-button-container">\n      <button class="cancel" tabIndex="2">Cancel</button>\n      <div class="sa-confirm-button-container">\n        <button class="confirm" tabIndex="1">OK</button><div class="la-ball-fall">\n          <div></div>\n          <div></div>\n          <div></div>\n        </div>\n      </div>\n    </div></div>';
            n["default"] = o, t.exports = n["default"]
        }, {}], 8: [function (e, t, o) {
            Object.defineProperty(o, "__esModule", {value: !0});
            var a = e("./utils"), r = e("./handle-swal-dom"), s = e("./handle-dom"),
                l = ["error", "warning", "info", "success", "input", "prompt"], i = function (e) {
                    var t = (0, r.getModal)(), o = t.querySelector("h2"), i = t.querySelector("p"),
                        u = t.querySelector("button.cancel"), c = t.querySelector("button.confirm");
                    if (o.innerHTML = e.html ? e.title : (0, s.escapeHtml)(e.title).split("\n").join("<br>"), i.innerHTML = e.html ? e.text : (0, s.escapeHtml)(e.text || "").split("\n").join("<br>"), e.text && (0, s.show)(i), e.customClass) (0, s.addClass)(t, e.customClass), t.setAttribute("data-custom-class", e.customClass); else {
                        var d = t.getAttribute("data-custom-class");
                        (0, s.removeClass)(t, d), t.setAttribute("data-custom-class", "")
                    }
                    if ((0, s.hide)(t.querySelectorAll(".sa-icon")), e.type && !(0, a.isIE8)()) {
                        var f = function () {
                            for (var o = !1, a = 0; a < l.length; a++) if (e.type === l[a]) {
                                o = !0;
                                break
                            }
                            if (!o) return logStr("Unknown alert type: " + e.type), {v: !1};
                            var i = ["success", "error", "warning", "info"], u = n;
                            -1 !== i.indexOf(e.type) && (u = t.querySelector(".sa-icon.sa-" + e.type), (0, s.show)(u));
                            var c = (0, r.getInput)();
                            switch (e.type) {
                                case"success":
                                    (0, s.addClass)(u, "animate"), (0, s.addClass)(u.querySelector(".sa-tip"), "animateSuccessTip"), (0, s.addClass)(u.querySelector(".sa-long"), "animateSuccessLong");
                                    break;
                                case"error":
                                    (0, s.addClass)(u, "animateErrorIcon"), (0, s.addClass)(u.querySelector(".sa-x-mark"), "animateXMark");
                                    break;
                                case"warning":
                                    (0, s.addClass)(u, "pulseWarning"), (0, s.addClass)(u.querySelector(".sa-body"), "pulseWarningIns"), (0, s.addClass)(u.querySelector(".sa-dot"), "pulseWarningIns");
                                    break;
                                case"input":
                                case"prompt":
                                    c.setAttribute("type", e.inputType), c.value = e.inputValue, c.setAttribute("placeholder", e.inputPlaceholder), (0, s.addClass)(t, "show-input"), setTimeout(function () {
                                        c.focus(), c.addEventListener("keyup", swal.resetInputError)
                                    }, 400)
                            }
                        }();
                        if ("object" == typeof f) return f.v
                    }
                    if (e.imageUrl) {
                        var p = t.querySelector(".sa-icon.sa-custom");
                        p.style.backgroundImage = "url(" + e.imageUrl + ")", (0, s.show)(p);
                        var m = 80, v = 80;
                        if (e.imageSize) {
                            var y = e.imageSize.toString().split("x"), b = y[0], h = y[1];
                            b && h ? (m = b, v = h) : logStr("Parameter imageSize expects value with format WIDTHxHEIGHT, got " + e.imageSize)
                        }
                        p.setAttribute("style", p.getAttribute("style") + "width:" + m + "px; height:" + v + "px")
                    }
                    t.setAttribute("data-has-cancel-button", e.showCancelButton), e.showCancelButton ? u.style.display = "inline-block" : (0, s.hide)(u), t.setAttribute("data-has-confirm-button", e.showConfirmButton), e.showConfirmButton ? c.style.display = "inline-block" : (0, s.hide)(c), e.cancelButtonText && (u.innerHTML = (0, s.escapeHtml)(e.cancelButtonText)), e.confirmButtonText && (c.innerHTML = (0, s.escapeHtml)(e.confirmButtonText)), e.confirmButtonColor && (c.style.backgroundColor = e.confirmButtonColor, c.style.borderLeftColor = e.confirmLoadingButtonColor, c.style.borderRightColor = e.confirmLoadingButtonColor, (0, r.setFocusStyle)(c, e.confirmButtonColor)), t.setAttribute("data-allow-outside-click", e.allowOutsideClick);
                    var g = !!e.doneFunction;
                    t.setAttribute("data-has-done-function", g), e.animation ? "string" == typeof e.animation ? t.setAttribute("data-animation", e.animation) : t.setAttribute("data-animation", "pop") : t.setAttribute("data-animation", "none"), t.setAttribute("data-timer", e.timer)
                };
            o["default"] = i, t.exports = o["default"]
        }, {"./handle-dom": 4, "./handle-swal-dom": 6, "./utils": 9}], 9: [function (t, n, o) {
            Object.defineProperty(o, "__esModule", {value: !0});
            var a = function (e, t) {
                for (var n in t) t.hasOwnProperty(n) && (e[n] = t[n]);
                return e
            }, r = function (e) {
                var t = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(e);
                return t ? parseInt(t[1], 16) + ", " + parseInt(t[2], 16) + ", " + parseInt(t[3], 16) : null
            }, s = function () {
                return e.attachEvent && !e.addEventListener
            }, l = function (t) {
                "undefined" != typeof e && e.console && e.console.log("SweetAlert: " + t)
            }, i = function (e, t) {
                e = String(e).replace(/[^0-9a-f]/gi, ""), e.length < 6 && (e = e[0] + e[0] + e[1] + e[1] + e[2] + e[2]), t = t || 0;
                var n, o, a = "#";
                for (o = 0; 3 > o; o++) n = parseInt(e.substr(2 * o, 2), 16), n = Math.round(Math.min(Math.max(0, n + n * t), 255)).toString(16), a += ("00" + n).substr(n.length);
                return a
            };
            o.extend = a, o.hexToRgb = r, o.isIE8 = s, o.logStr = l, o.colorLuminance = i
        }, {}]
    }, {}, [1]), "function" == typeof define && define.amd ? define(function () {
        return sweetAlert
    }) : "undefined" != typeof module && module.exports && (module.exports = sweetAlert)
}(window, document);
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206207208209210211212213214215216217218219220221222223224225226227228229230231232233234235236237238239240241242243244245246247248249250251252253254255256257258259260261262263264265266267268269270271272273274275276277278279280281282283284285286287288289290291292293294295296297298299300301302303304305306307308309310311312313314315316317318319320321322323324325326327328329330331332333334335336337338339340341342343344345346347348349350351352353354355356357358359360361362363364365366367368369370371372373374375376377378379380381382383384385386387388389390391
```

现在templates/cms目录下创建sweetalert测试文件sa_test.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SweetAlert测试</title>
    <script src="{{ url_for('static', filename='common/sweetalert/sweetalert.min.js') }}"></script>
    <script src="{{ url_for('static', filename='common/sweetalert/clalert.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='common/sweetalert/sweetalert.css') }}">
</head>
<body>
<button id="btn1">错误提示按钮</button>
<button id="btn2">信息提示按钮</button>
<button id="btn3">成功提示按钮</button>
<button id="btn4">多选提示按钮</button>
<script>
    var btn1 = document.getElementById("btn1")
    btn1.onclick = function () {
        clalert.alertError('错误提示')
    }
    var btn2 = document.getElementById("btn2")
    btn2.onclick = function () {
        clalert.alertInfo('信息提示')
    }
    var btn3 = document.getElementById("btn3")
    btn3.onclick = function () {
        clalert.alertSuccess('成功提示')
    }
    var btn3 = document.getElementById("btn4")
    btn3.onclick = function () {
        clalert.alertConfirm({
            'confirmText': '再发一篇文章',
            'cancelText': '回到首页',
            'msg': '发布成功',
            'confirmCallback': function () {
                alert('点击确认')
            }
        })
    }
</script>
</body>
</html>
123456789101112131415161718192021222324252627282930313233343536373839404142
```

在views.py增加视图函数sa_test()用户测试弹框效果如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from apps.cms.forms import LoginForm, ResetPwdForm
from apps.cms.models import CMSUser
from exts import db
from utils import restful

# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html')


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html")


@cms_bp.route('/satest/')
def sa_test():
    return render_template("cms/sa_test.html")


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html')

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283848586878889909192
```

显示：
![flask bbs cms reset pwd sweetalert test](https://img-blog.csdnimg.cn/2020052910332166.gif#pic_center)
显然，各种弹框样式不同，并且非常美观，可用于修改密码时的提示。

将提示框sweetalert的资源文件引入cms_base.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>
        {% block title %}

        {% endblock %}
    </title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='cms/css/cms_base.css') }}">
    <script src="{{ url_for('static', filename='cms/js/cms_base.js') }}"></script>
    {# 提示框资源文件 #}
    <script src="{{ url_for('static', filename='common/sweetalert/sweetalert.min.js') }}"></script>
    <script src="{{ url_for('static', filename='common/sweetalert/clalert.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='common/sweetalert/sweetalert.css') }}">
    {% block head %}
    {% endblock %}
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
    <div class="container-fluid">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">熊熊乐园论坛CMS管理系统</a>
        </div>
        <div id="navbar" class="navbar-collapse collapse">
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">{{ g.cms_user.username }}<span>[超级管理员]</span></a></li>
                <li><a href="{{ url_for('cms.logout') }}">注销</a></li>
            </ul>
            <form class="navbar-form navbar-right">
                <input type="text" class="form-control" placeholder="查找...">
            </form>
        </div>
    </div>
</nav>

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3 col-md-2 sidebar">
            <ul class="nav-sidebar">
                <li class="unfold"><a href="#">首页</a></li>
                <li class="profile-li">
                    <a href="#">个人中心<span></span></a>
                    <ul class="subnav">
                        <li><a href="{{ url_for('cms.profile') }}">个人信息</a></li>
                        <li><a href="{{ url_for('cms.resetpwd') }}">修改密码</a></li>
                        <li><a href="#">修改邮箱</a></li>
                    </ul>
                </li>

                <li class="nav-group post-manage"><a href="#">帖子管理</a></li>
                <li class="comments-manage"><a href="#">评论管理</a></li>
                <li class="board-manage"><a href="#">板块管理</a></li>

                <li class="nav-group user-manage"><a href="#">用户管理</a></li>
                <li class="role-manage"><a href="#">组管理</a></li>

                <li class="nav-group cmsuser-manage"><a href="#">CMS用户管理</a></li>
                <li class="cmsrole-manage"><a href="#">CMS组管理</a></li>
            </ul>
        </div>
        <div class="col-sm-9 col-sm-offset-3 col-md-10 col-md-offset-2 main">
            <h1>{% block page_title %} {% endblock %}</h1>
            <div class="main_content">
                {% block content %} {% endblock %}
            </div>
        </div>
    </div>
</div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879808182
```

resetpwd.js修改如下：

```js
$(function () {
    $("#submit").click(function (event) {
        // event.preventDefault
        // 是阻止按钮默认的提交表单的事件
        event.preventDefault();

        var oldpwdE = $("input[name=oldpwd]");
        var newpwdE = $("input[name=newpwd]");
        var newpwd2E = $("input[name=newpwd2]");

        var oldpwd = oldpwdE.val();
        var newpwd = newpwdE.val();
        var newpwd2 = newpwd2E.val();

        // 1. 要在模版的meta标签中渲染一个csrf-token
        // 2. 在ajax请求的头部中设置X-CSRFtoken
        var clajax = {
            'get': function (args) {
                args['method'] = 'get';
                this.ajax(args);
            },
            'post': function (args) {
                args['method'] = 'post';
                this.ajax(args);
            },
            'ajax': function (args) {
                // 设置csrftoken
                this._ajaxSetup();
                $.ajax(args);
            },
            '_ajaxSetup': function () {
                $.ajaxSetup({
                    'beforeSend': function (xhr, settings) {
                        if (!/^(GET|HEAD|OPTIONS|TRACE)$/i.test(settings.type) && !this.crossDomain) {
                            var csrftoken = $('meta[name=csrf-token]').attr('content');
                            // var csrftoken = $('input[name=csrf-token]').attr('value');
                            xhr.setRequestHeader("X-CSRFToken", csrftoken)
                        }
                    }
                });
            }
        };

        // 表单提交方式是post方法
        clajax.post({
            'url': '/cms/resetpwd/',
            'data': {
                'oldpwd': oldpwd,
                'newpwd': newpwd,
                'newpwd2': newpwd2
            },
            'success': function (data) {
                // console.log(data);
                if (data['code'] === 200) {
                    clalert.alertSuccess('密码修改成功');
                    oldpwd.val("");
                    newpwd.val("");
                    newpwd2.val("");
                } else {
                    var message = data['message'];
                    clalert.alertInfo(message)
                }
            },
            'fail': function (error) {
                // console.log(error);
                clalert.alertNetworkError();
            }
        });
    });
});
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970
```

显示：
![flask bbs cms reset pwd sweetalert alert](https://img-blog.csdnimg.cn/20200529103347676.gif#pic_center)
此时修改密码的相关消息都是通过弹框显示出来的，更加人性化。

## 五、修改邮箱界面搭建

在templates/cms目录下创建cms_resetemail.html如下:

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    修改邮箱
{% endblock %}

{% block page_title %}
    后台管理员：{{ self.title() }}
{% endblock %}

{% block head %}
    <style>
        .form-container {
            width: 300px;
        }
    </style>
{% endblock %}

{% block content %}
    <form action="" method="post">
        <div class="form-container">
            <div class="form-group">
                <div class="input-group">
                    <input type="email" class="form-control" name="email" placeholder="新邮箱">
                    <span class="input-group-addon" id="captcha-btn" style="cursor:pointer;">获取验证码</span>
                </div>
            </div>
            <div class="form-group">
                <input type="text" class="form-control" name="captcha" placeholder="邮箱验证码">
            </div>
            <div class="form-group">
                <button class="btn btn-primary" id="submit">立即修改</button>
            </div>
        </div>
    </form>
{% endblock %}
123456789101112131415161718192021222324252627282930313233343536
```

在static/cms/js目录下创建resetemail.js如下：

```js
var lgajax = {
    'get': function (args) {
        args['method'] = 'get';
        this.ajax(args);
    },
    'post': function (args) {
        args['method'] = 'post';
        this.ajax(args);
    },
    'ajax': function (args) {
        // 设置csrftoken
        this._ajaxSetup();
        $.ajax(args);
    },
    '_ajaxSetup': function () {
        $.ajaxSetup({
            'beforeSend': function (xhr, settings) {
                if (!/^(GET|HEAD|OPTIONS|TRACE)$/i.test(settings.type) && !this.crossDomain) {
                    var csrftoken = $('meta[name=csrf-token]').attr('content');
                    xhr.setRequestHeader("X-CSRFToken", csrftoken)
                }
            }
        });
    }
};
$(function () {
    $("#captcha-btn").click(function (event) {
        event.preventDefault();
        var email = $("input[name='email']").val();
        if (!email) {
            lgalert.alertInfoToast('请输入邮箱');
            return;
        }
        var lgajax = {
            'get': function (args) {
                args['method'] = 'get';
                this.ajax(args);
            },
            'post': function (args) {
                args['method'] = 'post';
                this.ajax(args);
            },
            'ajax': function (args) {
                // 设置csrftoken
                this._ajaxSetup();
                $.ajax(args);
            },
            '_ajaxSetup': function () {
                $.ajaxSetup({
                    'beforeSend': function (xhr, settings) {
                        if (!/^(GET|HEAD|OPTIONS|TRACE)$/i.test(settings.type) && !this.crossDomain) {
                            var csrftoken = $('meta[name=csrf-token]').attr('content');
                            xhr.setRequestHeader("X-CSRFToken", csrftoken)
                        }
                    }
                });
            }
        };
        lgajax.get({
            'url': '/cms/email_captcha/',
            'data': {
                'email': email
            },
            'success': function (data) {
                if (data['code'] == 200) {
                    lgalert.alertSuccessToast('邮件发送成功！请注意查收！');
                } else {
                    lgalert.alertInfo(data['message']);
                }
            },
            'fail': function (error) {
                lgalert.alertNetworkError();
            }
        });
    });
});

$(function () {
    $("#submit").click(function (event) {
        event.preventDefault();
        var emailE = $("input[name='email']");
        var captchaE = $("input[name='captcha']");

        var email = emailE.val();
        var captcha = captchaE.val();

        lgajax.post({
            'url': '/cms/resetemail/',
            'data': {
                'email': email,
                'captcha': captcha
            },
            'success': function (data) {
                if (data['code'] == 200) {
                    emailE.val("");
                    captchaE.val("");
                    lgalert.alertSuccessToast('恭喜！邮箱修改成功！');
                } else {
                    lgalert.alertInfo(data['message']);
                }
            },
            'fail': function (error) {
                lgalert.alertNetworkError();
            }
        });
    });
});
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107
```

views.py中增加视图类：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from apps.cms.forms import LoginForm, ResetPwdForm
from apps.cms.models import CMSUser
from exts import db
from utils import restful

# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html')


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html")


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html')

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html')

    def post(self):
        pass


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596
```

cms_base.html修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>
        {% block title %}

        {% endblock %}
    </title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='cms/css/cms_base.css') }}">
    <script src="{{ url_for('static', filename='cms/js/cms_base.js') }}"></script>
    {# 提示框资源文件 #}
    <script src="{{ url_for('static', filename='common/sweetalert/sweetalert.min.js') }}"></script>
    <script src="{{ url_for('static', filename='common/sweetalert/clalert.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='common/sweetalert/sweetalert.css') }}">
    {% block head %}
    {% endblock %}
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
    <div class="container-fluid">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">熊熊乐园论坛CMS管理系统</a>
        </div>
        <div id="navbar" class="navbar-collapse collapse">
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">{{ g.cms_user.username }}<span>[超级管理员]</span></a></li>
                <li><a href="{{ url_for('cms.logout') }}">注销</a></li>
            </ul>
            <form class="navbar-form navbar-right">
                <input type="text" class="form-control" placeholder="查找...">
            </form>
        </div>
    </div>
</nav>

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3 col-md-2 sidebar">
            <ul class="nav-sidebar">
                <li class="unfold"><a href="#">首页</a></li>
                <li class="profile-li">
                    <a href="#">个人中心<span></span></a>
                    <ul class="subnav">
                        <li><a href="{{ url_for('cms.profile') }}">个人信息</a></li>
                        <li><a href="{{ url_for('cms.resetpwd') }}">修改密码</a></li>
                        <li><a href="{{ url_for('cms.resetemail') }}">修改邮箱</a></li>
                    </ul>
                </li>

                <li class="nav-group post-manage"><a href="#">帖子管理</a></li>
                <li class="comments-manage"><a href="#">评论管理</a></li>
                <li class="board-manage"><a href="#">板块管理</a></li>

                <li class="nav-group user-manage"><a href="#">用户管理</a></li>
                <li class="role-manage"><a href="#">组管理</a></li>

                <li class="nav-group cmsuser-manage"><a href="#">CMS用户管理</a></li>
                <li class="cmsrole-manage"><a href="#">CMS组管理</a></li>
            </ul>
        </div>
        <div class="col-sm-9 col-sm-offset-3 col-md-10 col-md-offset-2 main">
            <h1>{% block page_title %} {% endblock %}</h1>
            <div class="main_content">
                {% block content %} {% endblock %}
            </div>
        </div>
    </div>
</div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879808182
```

显示：
![flask bbs cms reset email get](https://img-blog.csdnimg.cn/20200529103412107.gif#pic_center)
显然，此时要修改邮箱，需要实现发送邮件的功能，Flask中实现了这个功能，需要使用到flask-mail模块，可在虚拟环境中通过`pip install flask-mail`命令安装。

# Python全栈（八）Flask项目实战之4.CMS后台修改邮箱和权限介绍

Github和Gitee代码**同步更新**：
https://github.com/PythonWebProject/Flask_BBS；
https://gitee.com/Python_Web_Project/Flask_BBS。

## 一、Flask-Mail发送邮件

因为在cms_resetpwd.html和cms_resetemail.html中的style代码块都含有定义央视的相同代码，因此可以抽离出来，将其放入static/cms/css/cms_base.css中如下：

```css
/*
 * Base structure
 */

/* Move down content because we have a fixed navbar that is 50px tall */
body {
    padding-top: 50px;
    overflow: hidden;
}

/*
 * Global add-ons
 */

.sub-header {
    padding-bottom: 10px;
    border-bottom: 1px solid #eee;
}

/*
 * Top navigation
 * Hide default border to remove 1px line.
 */
.navbar-fixed-top {
    border: 0;
}

/*
 * Sidebar
 */

/* Hide for mobile, show later */
.sidebar {
    display: none;
}

@media (min-width: 768px) {
    .sidebar {
        position: fixed;
        top: 51px;
        bottom: 0;
        left: 0;
        z-index: 1000;
        display: block;
        padding: 20px;
        overflow-x: hidden;
        overflow-y: auto; /* Scrollable contents if viewport is shorter than content. */
        background-color: #363a47;
        border-right: 1px solid #eee;
        margin-top: -1px;
    }
}

.nav-sidebar {
    padding: 5px 0;
    margin-left: -20px;
    margin-right: -20px;
}

.nav-sidebar > li {
    background: #494f60;
    border-bottom: 1px solid #363a47;
    border-top: 1px solid #666;
    line-height: 35px;
}

.nav-sidebar > li > a {
    background: #494f60;
    color: #9b9fb1;
    margin-left: 25px;
    display: block;
}

.nav-sidebar > li a span {
    float: right;
    width: 10px;
    height: 10px;
    border-style: solid;
    border-color: #9b9fb1 #9b9fb1 transparent transparent;
    border-width: 1px;
    transform: rotate(45deg);
    position: relative;
    top: 10px;
    margin-right: 10px;
}

.nav-sidebar > li > a:hover {
    color: #fff;
    background: #494f60;
    text-decoration: none;
}

.nav-sidebar > li > .subnav {
    display: none;
}

.nav-sidebar > li.unfold {
    background: #494f60;
}

.nav-sidebar > li.unfold > .subnav {
    display: block;
}

.nav-sidebar > li.unfold > a {
    color: #db4055;
}

.nav-sidebar > li.unfold > a span {
    transform: rotate(135deg);
    top: 5px;
    border-color: #db4055 #db4055 transparent transparent;
}

.subnav {
    padding-left: 10px;
    padding-right: 10px;
    background: #363a47;
    overflow: hidden;
}

.subnav li {
    overflow: hidden;
    margin-top: 10px;
    line-height: 25px;
    height: 25px;
}

.subnav li.active {
    background: #db4055;
}

.subnav li a {
    /*display: block;*/
    color: #9b9fb1;
    padding-left: 30px;
    height: 25px;
    line-height: 25px;
}

.subnav li a:hover {
    color: #fff;
}

.nav-group {
    margin-top: 10px;
}


.main {
    padding: 20px;
}

@media (min-width: 768px) {
    .main {
        padding-right: 40px;
        padding-left: 40px;
    }
}

.main .page-header {
    margin-top: 0;
}


/*
 * Placeholder dashboard ideas
 */

.placeholders {
    margin-bottom: 30px;
    text-align: center;
}

.placeholders h4 {
    margin-bottom: 0;
}

.placeholder {
    margin-bottom: 20px;
}

.placeholder img {
    display: inline-block;
    border-radius: 50%;
}

.main_content {
    margin-top: 20px;
}


.top-group {
    padding: 5px 10px;
    border-radius: 2px;
    background: #ecedf0;
    overflow: hidden;
}

.form-container {
    width: 300px;
}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202
```

而在原模板中删除样式定义，cms_resetemail.html如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    修改邮箱
{% endblock %}

{% block page_title %}
    后台管理员：{{ self.title() }}
{% endblock %}

{% block head %}
{% endblock %}

{% block content %}
    <form action="" method="post">
        <div class="form-container">
            <div class="form-group">
                <div class="input-group">
                    <input type="email" class="form-control" name="email" placeholder="新邮箱">
                    <span class="input-group-addon" id="captcha-btn" style="cursor:pointer;">获取验证码</span>
                </div>
            </div>
            <div class="form-group">
                <input type="text" class="form-control" name="captcha" placeholder="邮箱验证码">
            </div>
            <div class="form-group">
                <button class="btn btn-primary" id="submit">立即修改</button>
            </div>
        </div>
    </form>
{% endblock %}
12345678910111213141516171819202122232425262728293031
```

cms_resetpwd.html如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    修改密码
{% endblock %}

{% block page_title %}
    后台管理员：{{ self.title() }}
{% endblock %}

{% block head %}
    <script src="{{ url_for('static', filename='cms/js/resetpwd.js') }}"></script>
{% endblock %}

{% block content %}
    <form action="" method="post">
{#        <input type="hidden" name="csrf_token" value="{{ csrf_token() }}">#}
        <div class="form-container">
            <div class="form-group">
                <div class="input-group">
                    <span class="input-group-addon">旧密码</span>
                    <input type="password" class="form-control" name="oldpwd" placeholder="请输入旧密码">
                </div>
            </div>
            <div class="form-group">
                <div class="input-group">
                    <span class="input-group-addon">新密码</span>
                    <input type="password" class="form-control" name="newpwd" placeholder="请输入新密码">
                </div>
            </div>
            <div class="form-group">
                <div class="input-group">
                    <span class="input-group-addon">确认新密码</span>
                    <input type="password" class="form-control" name="newpwd2" placeholder="请输入确认密码">
                </div>
            </div>
            <div class="form-group">
                <button class="btn btn-primary" id="submit">立即保存</button>
            </div>
        </div>
    </form>
{% endblock %}
123456789101112131415161718192021222324252627282930313233343536373839404142
```

给发送邮件定义配置项，config.py如下：

```python
import os

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_bbs'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

TEMPLATE_AUTO_RELOAD = True
SQLALCHEMY_DATABASE_URI = DB_URL
SQLALCHEMY_TRACK_MODIFICATIONS = False
SQLALCHEMY_POOL_RECYCLE = 280
SQLALCHEMY_POOL_SIZE = 20

# 设置密钥
SECRET_KEY = os.urandom(15)

# 发送邮箱服务地址
MAIL_SERVER = 'smtp.qq.com'
# 邮箱端口，为587或465，为587时TLS设置为True，为465时SSL设置为True
MAIL_PORT = 587
MAIL_USE_TLS = True
# MAIL_USE_SSL = True # MAIL_PORT为465时设置此项
# 用户名可以为你的邮箱，需要自行添加
MAIL_USERNAME = '123456789@qq.com'
# 邮箱密码，不是邮箱账号密码，而是第三方客户端登录使用的授权码，需要自行获取
MAIL_PASSWORD = 'xxxxxxxxxxxxxxxx'
# 发送者即你的邮箱，需要自行添加
MAIL_DEFAULT_SENDER = '123456789@qq.com'
12345678910111213141516171819202122232425262728293031
```

其中，需要为邮箱开启POP3/SMTP和IMAP/SMTP服务，并获取随机授权码用于第三方登录，以QQ邮箱为例开启服务和获取授权码示例如下：
![flask bbs cms reset email set authcode](https://img-blog.csdnimg.cn/20200605210403121.gif#pic_center)
也可根据需要重新获取授权码，也是通过给指定号码发送短信，与前面方式类似。

现导入并初始化Mail模块，为了防止相互引用，还是需要使用中间文件exts.py，如下：

```python
from flask_sqlalchemy import SQLAlchemy
from flask_mail import Mail

db = SQLAlchemy()

mail = Mail()
123456
```

bbs.py如下：

```python
'''
前台 front
后台 cms
公有 common
'''

from flask import Flask
from flask_wtf import CSRFProtect
from exts import db, mail
from apps.cms.views import cms_bp
from apps.front.views import front_bp
import config


app = Flask(__name__)
CSRFProtect(app)
app.config.from_object(config)
db.init_app(app)
mail.init_app(app)

app.register_blueprint(cms_bp)
app.register_blueprint(front_bp)


if __name__ == '__main__':
    app.run(debug=True)
1234567891011121314151617181920212223242526
```

在views.py中实现邮件发送测试：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message
from apps.cms.forms import LoginForm, ResetPwdForm
from apps.cms.models import CMSUser
from exts import db, mail
from utils import restful

# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html')


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route('/send_mail/')
def send_message():
    try:
        message = Message('邮件发送测试', recipients=['987654321@qq.com'], body='这是邮件发送测试，收到请忽略！！！')
        mail.send(message)
        return '邮件发送成功'
    except:
        return '邮件发送失败'


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html")


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html')

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html')

    def post(self):
        pass


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107
```

示例如下：
![flask bbs cms reset email send test](https://img-blog.csdnimg.cn/2020060521042673.gif#pic_center)
此时查看收件邮箱，已经收到了发送的邮件。

## 二、发送邮件验证码

修改邮箱也需要通过Ajax实现，resetemail.js修改如下：

```js
var clajax = {
    'get': function (args) {
        args['method'] = 'get';
        this.ajax(args);
    },
    'post': function (args) {
        args['method'] = 'post';
        this.ajax(args);
    },
    'ajax': function (args) {
        // 设置csrftoken
        this._ajaxSetup();
        $.ajax(args);
    },
    '_ajaxSetup': function () {
        $.ajaxSetup({
            'beforeSend': function (xhr, settings) {
                if (!/^(GET|HEAD|OPTIONS|TRACE)$/i.test(settings.type) && !this.crossDomain) {
                    var csrftoken = $('meta[name=csrf-token]').attr('content');
                    xhr.setRequestHeader("X-CSRFToken", csrftoken)
                }
            }
        });
    }
};
$(function () {
    $("#captcha-btn").click(function (event) {
        event.preventDefault();
        var email = $("input[name='email']").val();
        if (!email) {
            clalert.alertInfoToast('请输入邮箱');
            return;
        }
        var clajax = {
            'get': function (args) {
                args['method'] = 'get';
                this.ajax(args);
            },
            'post': function (args) {
                args['method'] = 'post';
                this.ajax(args);
            },
            'ajax': function (args) {
                // 设置csrftoken
                this._ajaxSetup();
                $.ajax(args);
            },
            '_ajaxSetup': function () {
                $.ajaxSetup({
                    'beforeSend': function (xhr, settings) {
                        if (!/^(GET|HEAD|OPTIONS|TRACE)$/i.test(settings.type) && !this.crossDomain) {
                            var csrftoken = $('meta[name=csrf-token]').attr('content');
                            xhr.setRequestHeader("X-CSRFToken", csrftoken)
                        }
                    }
                });
            }
        };
        clajax.get({
            'url': '/cms/email_captcha/',
            'data': {
                'email': email
            },
            'success': function (data) {
                if (data['code'] === 200) {
                    clalert.alertSuccessToast('邮件发送成功！请注意查收！');
                } else {
                    clalert.alertInfo(data['message']);
                }
            },
            'fail': function (error) {
                clalert.alertNetworkError();
            }
        });
    });
});

$(function () {
    $("#submit").click(function (event) {
        event.preventDefault();
        var emailE = $("input[name='email']");
        var captchaE = $("input[name='captcha']");

        var email = emailE.val();
        var captcha = captchaE.val();

        clajax.post({
            'url': '/cms/resetemail/',
            'data': {
                'email': email,
                'captcha': captcha
            },
            'success': function (data) {
                if (data['code'] === 200) {
                    emailE.val("");
                    captchaE.val("");
                    clalert.alertSuccessToast('恭喜！邮箱修改成功！');
                } else {
                    clalert.alertInfo(data['message']);
                }
            },
            'fail': function (error) {
                clalert.alertNetworkError();
            }
        });
    });
});
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107
```

cms_resetemail.html中导入js文件如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    修改邮箱
{% endblock %}

{% block page_title %}
    后台管理员：{{ self.title() }}
{% endblock %}

{% block head %}
    <script src="{{ url_for('static', filename='cms/js/resetemail.js') }}"></script>
{% endblock %}

{% block content %}
    <form action="" method="post">
        <div class="form-container">
            <div class="form-group">
                <div class="input-group">
                    <input type="email" class="form-control" name="email" placeholder="新邮箱">
                    <span class="input-group-addon" id="captcha-btn" style="cursor:pointer;">获取验证码</span>
                </div>
            </div>
            <div class="form-group">
                <input type="text" class="form-control" name="captcha" placeholder="邮箱验证码">
            </div>
            <div class="form-group">
                <button class="btn btn-primary" id="submit">立即修改</button>
            </div>
        </div>
    </form>
{% endblock %}
1234567891011121314151617181920212223242526272829303132
```

在工具目录utils下创建random_captcha.py如下：

```python
import random


def get_random_captcha(num):
    '''生成随机验证码'''
    code = ''
    for i in range(num):
        num = str(random.randint(0, 9))
        upper = chr(random.randint(65, 90))
        lower = chr(random.randint(97, 122))
        lst = [num, upper, lower]
        ret = random.choice(lst)
        code = ''.join([code, ret])
    return code
1234567891011121314
```

views.py修改如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message
from apps.cms.forms import LoginForm, ResetPwdForm
from apps.cms.models import CMSUser
from exts import db, mail
from utils import restful, random_captcha

# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html')


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html")


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html')

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html')

    def post(self):
        pass


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，请及时输入、注意保密。' % captcha)
            mail.send(message)
            return restful.success(message='邮件发送成功，请注意接收验证码')
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114
```

显示：
![flask bbs cms reset email send captcha](https://img-blog.csdnimg.cn/20200605210452157.gif#pic_center)
显然，此时已经可以发送验证码，现在就是保存验证码和验证的问题了。

我们需要使用Redis，一是因为验证码需要频繁读取，要是频繁读写MySQL，会影响网站的整体速度，二是因为验证码可以设置有效期，Redis更方便设置。

在操作前需要先开启Redis服务端，可参考https://blog.csdn.net/CUFEECR/article/details/105010792。

在utils目录下创建clcache.py如下：

```python
import redis

# 连接Redis数据库
r = redis.StrictRedis(host='localhost', port=6379, db=0, decode_responses=True)


def save_captcha(key, value, timeout=300):
    '''把验证码存到Redis'''
    return r.set(key, value, timeout)


def get_captcha(key):
    '''从Redis中取验证码'''
    return r.get(key)


def delete_captcha(key):
    return r.delete(key)
123456789101112131415161718
```

在初始化Redis对象时，设置decode_responses参数为True是为了使查询数据时不返回二进制数据（即字节型数据），而是返回普通字符串。

views.py修改如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message
from apps.cms.forms import LoginForm, ResetPwdForm
from apps.cms.models import CMSUser
from exts import db, mail
from utils import restful, random_captcha, clcache

# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html')


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html")


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html')

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html')

    def post(self):
        pass


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115
```

显示：
![flask bbs cms reset email send save captcha](https://img-blog.csdnimg.cn/20200605210753139.gif#pic_center)
显然，此时在发送验证码后，存到了Redis中。

## 三、修改邮箱功能实现

在forms.py中增加验证修改邮箱的表单如下：

```python
from wtforms import Form, StringField, IntegerField, ValidationError
from wtforms.validators import Email, InputRequired, Length, EqualTo
from utils import clcache


class BaseForm(Form):
    def get_error(self):
        message = self.errors.popitem()[1][0]
        return message


class LoginForm(BaseForm):
    email = StringField(validators=[Email(message='请输入正确的邮箱地址'), InputRequired(message='请输入邮箱')])
    password = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    remember = IntegerField()


class ResetPwdForm(BaseForm):
    oldpwd = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    newpwd = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    newpwd2 = StringField(validators=[EqualTo("newpwd", message='两次输入密码不一致')])


class ResetEmailForm(BaseForm):
    email = StringField(validators=[Email(message='您的邮箱格式有误，请重新输入')])
    captcha = StringField(validators=[Length(4, 4, message='请输入4位验证码')])

    def validate_captcha(self, form):
        captcha = self.captcha.data
        email = self.email.data
        redis_captcha = clcache.get_captcha(email)
        if not redis_captcha or captcha.lower() != redis_captcha.lower():
            raise ValidationError('邮箱验证码错误')
123456789101112131415161718192021222324252627282930313233
```

views.py修改如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message
from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm
from apps.cms.models import CMSUser
from exts import db, mail
from utils import restful, random_captcha, clcache

# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html')


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html")


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html')

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html')

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            return '验证成功'
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119
```

显示：
![flask bbs cms reset email form validate](https://img-blog.csdnimg.cn/20200605210819689.gif#pic_center)
显然，此时已经对邮箱和验证码进行验证，可以正式实现修改邮箱了，views.py如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message
from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm
from apps.cms.models import CMSUser
from exts import db, mail
from utils import restful, random_captcha, clcache

# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html')


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html")


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html')

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html')

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            email = form.email.data
            # 修改用户邮箱
            g.cms_user.email = email
            return restful.success('修改邮箱成功！！')
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122
```

显示：
![flask bbs cms reset email](https://img-blog.csdnimg.cn/20200605210843932.gif#pic_center)
显然，修改密码之后注销重新登录就不能再使用之前的邮箱了，而要使用修改过后的邮箱。

## 四、权限管理介绍

在这里，我们使用**二进制**（位运算，包括与运算和或运算等）来实现**权限管理**。

比如，超级管理员的权限为11111111（各位代表不同的权限，8位代表有8种不同的权限），一般管理员的权限为00000001，则11111111&00000001=00000001，为一般管理员的权限本身。

本平台属于多用户平台，即有多个不同的用户，每个用户对应着角色，用户与角色之间的关系为**多对多**；
角色又对应着权限，角色与权限之间的关系也为**多对多**。

权限有访问、管理帖子、管理评论、管理板块、管理前台用户、管理后台用户、管理后台管理员等7类。
角色及其对应的权限如下：
访问者：

- 访问

运营者角色：

- 访问
- 管理帖子
- 管理评论
- 管理后台用户
- 管理前台用户
- 管理板块

开发者：
所有权限，即超级管理员。

## 五、权限和角色模型定义

现根据前面的分析建立相关模型。

在models.py中新建模型如下：

```python
from datetime import datetime
from werkzeug.security import generate_password_hash, check_password_hash
from exts import db


class CMSUser(db.Model):
    '''后台管理员用户类'''
    __tablename__ = 'cms_user'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    username = db.Column(db.String(30), nullable=False)
    _password = db.Column(db.String(100), nullable=False)
    email = db.Column(db.String(50), nullable=False, unique=True)
    join_time = db.Column(db.DateTime, default=datetime.now())

    def __init__(self, username, password, email):
        self.username = username
        self.password = password
        self.email = email

    @property
    def password(self):
        '''获取密码'''
        return self._password

    @password.setter
    def password(self, raw_password):
        '''设置密码'''
        self._password = generate_password_hash(raw_password)

    def check_password(self, raw_password):
        '''验证密码是否正确'''
        result = check_password_hash(self.password, raw_password)
        return result


class CMSPermission(object):
    # 255二进制表示所有权限
    ALL_PERMISSION = 0b11111111

    # 访问权限
    VISITOR = 0b00000001

    # 管理帖子权限
    POSTER = 0b00000010

    # 管理评论权限
    COMMENTER = 0b00000100

    # 管理板块
    BOARDER = 0b00001000

    # 管理前台用户
    FRONTUSER = 0b00010000

    # 管理前台用户
    CMSUSER = 0b00100000

    # 管理前台用户
    ADMINER = 0b01000000


cms_role_user = db.Table(
    'cms_role_user',
    db.Column('cms_role_id', db.Integer, db.ForeignKey('cms_role.id'), primary_key=True),
    db.Column('cms_user_id', db.Integer, db.ForeignKey('cms_user.id'), primary_key=True)
)


class CMSRole(db.Model):
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(50), nullable=False)
    desc = db.Column(db.String(200), nullable=False)
    create_time = db.Column(db.DateTime, default=datetime.now())
    permissions = db.Column(db.Integer, default=CMSPermission.VISITOR)

    users = db.relationship('CMSUser', secondary=cms_role_user, backref='roles')
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576
```

manage.py修改如下：

```python
from flask_script import Manager
from bbs import app
from flask_migrate import Migrate, MigrateCommand
from exts import db
from apps.cms.models import CMSUser, CMSRole

manager = Manager(app)
Migrate(app, db)

manager.add_command('db', MigrateCommand)


@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
@manager.option('-e', '--email', dest='email')
def create_cms_user(username, password, email):
    user = CMSUser(username=username, password=password, email=email)
    db.session.add(user)
    db.session.commit()
    print('CMS用户添加成功')


if __name__ == '__main__':
    manager.run()

12345678910111213141516171819202122232425
```

在项目主目录下执行`python manage.py db migrate`和`python manage.py db update`命令，执行成功后查询数据库：

```sql
show tables;
1
```

打印：

```sql
+---------------------+
| Tables_in_flask_bbs |
+---------------------+
| alembic_version     |
| cms_role            |
| cms_role_user       |
| cms_user            |
+---------------------+
4 rows in set (0.01 sec)

12345678910
```

显然，已经成功创建所需的表。

# Python全栈（八）Flask项目实战之5.CMS后台权限验证

Github和Gitee代码**同步更新**：
https://github.com/PythonWebProject/Flask_BBS；
https://gitee.com/Python_Web_Project/Flask_BBS。

## 一、添加用户角色

在manage.py中实现添加用户角色的功能如下：

```python
from flask_script import Manager
from bbs import app
from flask_migrate import Migrate, MigrateCommand
from exts import db
from apps.cms.models import CMSUser, CMSRole, CMSPermission

manager = Manager(app)
Migrate(app, db)

manager.add_command('db', MigrateCommand)


@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
@manager.option('-e', '--email', dest='email')
def create_cms_user(username, password, email):
    user = CMSUser(username=username, password=password, email=email)
    db.session.add(user)
    db.session.commit()
    print('CMS用户添加成功')


@manager.command
def create_role():
    # 访问者
    visitor = CMSRole(name='访问者', desc='只能查看数据，不能修改数据')
    visitor.permissions = CMSPermission.VISITOR  # 也可以省去，因为默认权限就是VISITOR

    # 运营者
    operator = CMSRole(name='运营', desc='管理帖子，管理评论，管理前台和后台用户')
    # 有多个权限时，使用或运算表示
    operator.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER
    # 管理员
    admin = CMSRole(name='管理员', desc='拥有本系统大部分权限')
    admin.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER | CMSPermission.BOARDER

    # 开发人员
    developer = CMSRole(name='开发者', desc='拥有所有权限')
    developer.permissions = CMSPermission.ALL_PERMISSION

    db.session.add_all([visitor, operator, admin, developer])
    db.session.commit()
    print('角色添加成功')


if __name__ == '__main__':
    manager.run()

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748
```

此时在命令行中项目主目录下执行`python manage.py create_role`命令，打印`角色添加成功`，再查询数据库`select * from cms_role;`，打印：

```sql
+----+-----------+-----------------------------------------------------------+---------------------+-------------+                           
| id | name      | desc                                                      | create_time         | permissions |                           
+----+-----------+-----------------------------------------------------------+---------------------+-------------+                           
|  1 | 访问者    | 只能查看数据，不能修改数据                                | 2020-06-18 19:35:52 |           1 |                                           
|  2 | 运营      | 管理帖子，管理评论，管理前台和后台用户                    | 2020-06-18 19:35:52 |          55 |                                                
|  3 | 管理员    | 拥有本系统大部分权限                                      | 2020-06-18 19:35:52 |          63 |                                        
|  4 | 开发者    | 拥有所有权限                                              | 2020-06-18 19:35:52 |         255 |                                    
+----+-----------+-----------------------------------------------------------+---------------------+-------------+                           
4 rows in set (0.00 sec)                                                                                                                     
                                                                                                                                             
12345678910
```

显然，数据创建成功。

## 二、封装权限判断功能

现在用户和角色之间还没有关联，需要通过中间表cms_role_user进行关联。

先完善模型models.py，封装权限判断功能如下：

```python
from datetime import datetime
from werkzeug.security import generate_password_hash, check_password_hash
from exts import db


class CMSUser(db.Model):
    '''后台管理员用户类'''
    __tablename__ = 'cms_user'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    username = db.Column(db.String(30), nullable=False)
    _password = db.Column(db.String(100), nullable=False)
    email = db.Column(db.String(50), nullable=False, unique=True)
    join_time = db.Column(db.DateTime, default=datetime.now())

    def __init__(self, username, password, email):
        self.username = username
        self.password = password
        self.email = email

    @property
    def password(self):
        '''获取密码'''
        return self._password

    @password.setter
    def password(self, raw_password):
        '''设置密码'''
        self._password = generate_password_hash(raw_password)

    def check_password(self, raw_password):
        '''验证密码是否正确'''
        result = check_password_hash(self.password, raw_password)
        return result

    @property
    def permissions(self):
        '''判断用户权限'''
        if not self.roles:
            return 0

        all_permissions = 0
        for role in self.roles:
            permissions = role.permissions
            all_permissions |= permissions
        return all_permissions

    def has_permission(self, permission):
        '''判断当前用户是否有某个权限'''
        all_permissions = self.permissions
        result = all_permissions & permission == permission  # 如果当前用户有某个权限，则他的全部权限与该权限按位与的结果等于该权限本身
        return result

    @property
    def is_developer(self):
        '''判断当前用户是否是开发人员'''
        return self.has_permission(CMSPermission.ALL_PERMISSION)


class CMSPermission(object):
    # 255二进制表示所有权限
    ALL_PERMISSION = 0b11111111

    # 访问权限
    VISITOR = 0b00000001

    # 管理帖子权限
    POSTER = 0b00000010

    # 管理评论权限
    COMMENTER = 0b00000100

    # 管理板块
    BOARDER = 0b00001000

    # 管理前台用户
    FRONTUSER = 0b00010000

    # 管理前台用户
    CMSUSER = 0b00100000

    # 管理前台用户
    ADMINER = 0b01000000


cms_role_user = db.Table(
    'cms_role_user',
    db.Column('cms_role_id', db.Integer, db.ForeignKey('cms_role.id'), primary_key=True),
    db.Column('cms_user_id', db.Integer, db.ForeignKey('cms_user.id'), primary_key=True)
)


class CMSRole(db.Model):
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(50), nullable=False)
    desc = db.Column(db.String(200), nullable=False)
    create_time = db.Column(db.DateTime, default=datetime.now())
    permissions = db.Column(db.Integer, default=CMSPermission.VISITOR)

    users = db.relationship('CMSUser', secondary=cms_role_user, backref='roles')

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100
```

再在manage.py中添加验证：

```python
from flask_script import Manager
from bbs import app
from flask_migrate import Migrate, MigrateCommand
from exts import db
from apps.cms.models import CMSUser, CMSRole, CMSPermission

manager = Manager(app)
Migrate(app, db)

manager.add_command('db', MigrateCommand)


@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
@manager.option('-e', '--email', dest='email')
def create_cms_user(username, password, email):
    user = CMSUser(username=username, password=password, email=email)
    db.session.add(user)
    db.session.commit()
    print('CMS用户添加成功')


@manager.command
def create_role():
    # 访问者
    visitor = CMSRole(name='访问者', desc='只能查看数据，不能修改数据')
    visitor.permissions = CMSPermission.VISITOR  # 也可以省去，因为默认权限就是VISITOR

    # 运营者
    operator = CMSRole(name='运营', desc='管理帖子，管理评论，管理前台和后台用户')
    # 有多个权限时，使用或运算表示
    operator.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER
    # 管理员
    admin = CMSRole(name='管理员', desc='拥有本系统大部分权限')
    admin.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER | CMSPermission.BOARDER

    # 开发人员
    developer = CMSRole(name='开发者', desc='拥有所有权限')
    developer.permissions = CMSPermission.ALL_PERMISSION

    db.session.add_all([visitor, operator, admin, developer])
    db.session.commit()
    print('角色添加成功')


@manager.command
def test_permission():
    user = CMSUser.query.first()
    if user.has_permission(CMSPermission.VISITOR):
        print('该用户有使用者权限')
    else:
        print('该用户没有使用者权限')


if __name__ == '__main__':
    manager.run()

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657
```

此时在命令行中执行`python manage.py test_permission`，会打印`该用户没有使用者权限`，这是因为还未将用户与角色进行关联，需要进行关联，最简单的方法是直接在cms_role_user表中插入1条数据，cms_role_id和cms_user_id均为1，如下：

```sql
insert into cms_role_user values(1,1);
1
```

查询cms_role_user表：

```sql
select * from cms_role_user;
1
```

打印：

```sql
+-------------+-------------+   
| cms_role_id | cms_user_id |   
+-------------+-------------+   
|           1 |           1 |   
+-------------+-------------+   
1 row in set (0.00 sec)         
                                
1234567
```

显然，此时将id为1的角色（访问者）赋给id为1的用户（当前登录用户）。

此时再执行`python manage.py test_permission`，会打印`该用户有使用者权限`，即当前用户拥有访问论坛的权限。

在实际中一般不会在数据库中手动添加数据，而是通过命令行的方式添加。
现在manage.py中实现在命令行将用户添加到角色的功能：

```python
from flask_script import Manager
from bbs import app
from flask_migrate import Migrate, MigrateCommand
from exts import db
from apps.cms.models import CMSUser, CMSRole, CMSPermission

manager = Manager(app)
Migrate(app, db)

manager.add_command('db', MigrateCommand)


@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
@manager.option('-e', '--email', dest='email')
def create_cms_user(username, password, email):
    user = CMSUser(username=username, password=password, email=email)
    db.session.add(user)
    db.session.commit()
    print('CMS用户添加成功')


@manager.command
def create_role():
    # 访问者
    visitor = CMSRole(name='访问者', desc='只能查看数据，不能修改数据')
    visitor.permissions = CMSPermission.VISITOR  # 也可以省去，因为默认权限就是VISITOR

    # 运营者
    operator = CMSRole(name='运营', desc='管理帖子，管理评论，管理前台和后台用户')
    # 有多个权限时，使用或运算表示
    operator.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER
    # 管理员
    admin = CMSRole(name='管理员', desc='拥有本系统大部分权限')
    admin.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER | CMSPermission.BOARDER

    # 开发人员
    developer = CMSRole(name='开发者', desc='拥有所有权限')
    developer.permissions = CMSPermission.ALL_PERMISSION

    db.session.add_all([visitor, operator, admin, developer])
    db.session.commit()
    print('角色添加成功')


@manager.command
def test_permission():
    user = CMSUser.query.first()
    if user.has_permission(CMSPermission.VISITOR):
        print('该用户有使用者权限')
    else:
        print('该用户没有使用者权限')


@manager.option('-e', '--email', dest='email')
@manager.option('-n', '--name', dest='name')
def add_user_to_role(email, name):
    user = CMSUser.query.filter_by(email=email).first()
    if user:
        role = CMSRole.query.filter_by(name=name).first()
        if role:
            role.users.append(user)
            db.session.commit()
            print('用户添加到角色添加成功')
        else:
            print('角色不存在')
    else:
        print('邮箱不存在')


if __name__ == '__main__':
    manager.run()

12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273
```

在命令行中执行命令`python manage.py add_user_to_role -e 379869029@qq.com -n 开发者`，打印`用户添加到角色添加成功`，再查询cms_role_user表：

```sql
select * from cms_role_user;
1
```

打印：

```sql
+-------------+-------------+
| cms_role_id | cms_user_id |
+-------------+-------------+
|           1 |           1 |
|           4 |           1 |
+-------------+-------------+
2 rows in set (0.00 sec)
                            
12345678
```

显然，已经将数据插入数据库中。

此时再执行`python manage.py test_permission`，打印`该用户有使用者权限`，显然因为刚刚给当前用户赋予开发者角色，所以已经有访问者（使用者）角色。

## 三、客户端权限验证

现对个人中心页面进行完善，将个人信息页角色和权限补充完整，对右上角账户信息（用户名和角色）进行完善，因为一个用户可能对应着多个角色，这里选择显示权限最大的角色，一般有两种方法：
（1）可以在钩子函数中的请求前处理器中判断，将当前用户权限最大的角色存入g对象，以便于视图函数中可以直接引用g对象将角色变量传入模板进行渲染；
（2）也可以使用钩子函数中的上下文处理器以字典的形式传入，模板渲染时也可以接收到该角色变量。

这里采用第1种方式，后面也有采用第2种方式的地方。

cms_base.html修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>
        {% block title %}

        {% endblock %}
    </title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='cms/css/cms_base.css') }}">
    <script src="{{ url_for('static', filename='cms/js/cms_base.js') }}"></script>
    {# 提示框资源文件 #}
    <script src="{{ url_for('static', filename='common/sweetalert/sweetalert.min.js') }}"></script>
    <script src="{{ url_for('static', filename='common/sweetalert/clalert.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='common/sweetalert/sweetalert.css') }}">
    {% block head %}
    {% endblock %}
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
    <div class="container-fluid">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">熊熊乐园论坛CMS管理系统</a>
        </div>
        <div id="navbar" class="navbar-collapse collapse">
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">{{ g.cms_user.username }}<span>[{% block role %}{% endblock %}]</span></a></li>
                <li><a href="{{ url_for('cms.logout') }}">注销</a></li>
            </ul>
            <form class="navbar-form navbar-right">
                <input type="text" class="form-control" placeholder="查找...">
            </form>
        </div>
    </div>
</nav>

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3 col-md-2 sidebar">
            <ul class="nav-sidebar">
                <li class="unfold"><a href="{{ url_for('cms.index') }}">首页</a></li>
                <li class="profile-li">
                    <a href="#">个人中心<span></span></a>
                    <ul class="subnav">
                        <li><a href="{{ url_for('cms.profile') }}">个人信息</a></li>
                        <li><a href="{{ url_for('cms.resetpwd') }}">修改密码</a></li>
                        <li><a href="{{ url_for('cms.resetemail') }}">修改邮箱</a></li>
                    </ul>
                </li>

                <li class="nav-group post-manage"><a href="#">帖子管理</a></li>
                <li class="comments-manage"><a href="#">评论管理</a></li>
                <li class="board-manage"><a href="#">板块管理</a></li>

                <li class="nav-group user-manage"><a href="#">用户管理</a></li>
                <li class="role-manage"><a href="#">组管理</a></li>

                <li class="nav-group cmsuser-manage"><a href="#">CMS用户管理</a></li>
                <li class="cmsrole-manage"><a href="#">CMS组管理</a></li>
            </ul>
        </div>
        <div class="col-sm-9 col-sm-offset-3 col-md-10 col-md-offset-2 main">
            <h1>{% block page_title %} {% endblock %}</h1>
            <div class="main_content">
                {% block content %} {% endblock %}
            </div>
        </div>
    </div>
</div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879808182
```

cms_index.html修改如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台
{% endblock %}

{% block page_title %}
    熊熊论坛
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    欢迎来到{{ self.page_title() }}......
{% endblock %}
1234567891011121314151617
```

cms_profile.html修改如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台个人信息
{% endblock %}

{% block page_title %}
    熊熊个人中心
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    {% set user = g.cms_user %}
    <table class="table table-bordered">
        <tr>
            <td>用户名：</td>
            <td>{{ user.username }}</td>
        </tr>
        <tr>
            <td>邮箱：</td>
            <td>{{ user.email }}</td>
        </tr>
        <tr>
            <td>角色：</td>
            <td>
                {% for role in user.roles %}
                    {{ role.name }}
                    {% if not loop.last %}
                        &nbsp;
                    {% endif %}
                {% endfor %}
            </td>
        </tr>
        <tr>
            <td>权限：</td>
            <td>
                {% for role in user.roles %}
                    {{ role.desc }}
                    {% if not loop.last %}
                        ；
                    {% endif %}
                {% endfor %}

            </td>
        </tr>
        <tr>
            <td>加入时间：</td>
            <td>{{ user.join_time }}</td>
        </tr>
    </table>
{% endblock %}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354
```

cms_resetpwd.html修改如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    修改密码
{% endblock %}

{% block page_title %}
    后台管理员：{{ self.title() }}
{% endblock %}

{% block head %}
    <script src="{{ url_for('static', filename='cms/js/resetpwd.js') }}"></script>
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    <form action="" method="post">
{#        <input type="hidden" name="csrf_token" value="{{ csrf_token() }}">#}
        <div class="form-container">
            <div class="form-group">
                <div class="input-group">
                    <span class="input-group-addon">旧密码</span>
                    <input type="password" class="form-control" name="oldpwd" placeholder="请输入旧密码">
                </div>
            </div>
            <div class="form-group">
                <div class="input-group">
                    <span class="input-group-addon">新密码</span>
                    <input type="password" class="form-control" name="newpwd" placeholder="请输入新密码">
                </div>
            </div>
            <div class="form-group">
                <div class="input-group">
                    <span class="input-group-addon">确认新密码</span>
                    <input type="password" class="form-control" name="newpwd2" placeholder="请输入确认密码">
                </div>
            </div>
            <div class="form-group">
                <button class="btn btn-primary" id="submit">立即保存</button>
            </div>
        </div>
    </form>
{% endblock %}
12345678910111213141516171819202122232425262728293031323334353637383940414243444546
```

cms_resetemail.html修改如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    修改邮箱
{% endblock %}

{% block page_title %}
    后台管理员：{{ self.title() }}
{% endblock %}

{% block head %}
    <script src="{{ url_for('static', filename='cms/js/resetemail.js') }}"></script>
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    <form action="" method="post">
        <div class="form-container">
            <div class="form-group">
                <div class="input-group">
                    <input type="email" class="form-control" name="email" placeholder="新邮箱">
                    <span class="input-group-addon" id="captcha-btn" style="cursor:pointer;">获取验证码</span>
                </div>
            </div>
            <div class="form-group">
                <input type="text" class="form-control" name="captcha" placeholder="邮箱验证码">
            </div>
            <div class="form-group">
                <button class="btn btn-primary" id="submit">立即修改</button>
            </div>
        </div>
    </form>
{% endblock %}
123456789101112131415161718192021222324252627282930313233343536
```

hooks.py修改如下：

```python
from flask import request, session, url_for, redirect, g
from .models import CMSUser, CMSRole
from .views import cms_bp


@cms_bp.before_request
def before_request():
    # 判断当前是否是登录页面
    if not request.path.endswith(url_for('cms.login')):
        user_id = session.get('user_id')
        # 判断是否保存到session
        if not user_id:
            return redirect(url_for('cms.login'))

    if 'user_id' in session:
        user_id = session.get('user_id')
        user = CMSUser.query.get(user_id)
        # 使用g对象保存用户
        if user:
            g.cms_user = user
            roles = user.roles
            max_permission = max([role.permissions for role in roles])
            max_role = CMSRole.query.filter_by(permissions=max_permission).first().name
            g.max_role = max_role
123456789101112131415161718192021222324
```

views.py修改如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message
from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm
from apps.cms.models import CMSUser, CMSRole
from exts import db, mail
from utils import restful, random_captcha, clcache

# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html', max_role=g.max_role)


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html", max_role=g.max_role)


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html', max_role=g.max_role)

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html', max_role=g.max_role)

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            email = form.email.data
            # 修改用户邮箱
            g.cms_user.email = email
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123
```

显示：
![flask bbs cms profile perfect](https://img-blog.csdnimg.cn/20200619204251796.gif#pic_center)

显然，个人中心页面已经进一步完善。

接着实现根据不同的用户权限来显示不同的管理页面的功能。

hooks.py修改如下：

```python
from flask import request, session, url_for, redirect, g
from .models import CMSUser, CMSRole, CMSPermission
from .views import cms_bp


@cms_bp.before_request
def before_request():
    # 判断当前是否是登录页面
    if not request.path.endswith(url_for('cms.login')):
        user_id = session.get('user_id')
        # 判断是否保存到session
        if not user_id:
            return redirect(url_for('cms.login'))

    if 'user_id' in session:
        user_id = session.get('user_id')
        user = CMSUser.query.get(user_id)
        # 使用g对象保存用户
        if user:
            g.cms_user = user
            roles = user.roles
            max_permission = max([role.permissions for role in roles])
            max_role = CMSRole.query.filter_by(permissions=max_permission).first().name
            g.max_role = max_role


@cms_bp.context_processor
def cms_context_processor():
    return {'CMSPermission': CMSPermission}
1234567891011121314151617181920212223242526272829
```

cms_base.html修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>
        {% block title %}

        {% endblock %}
    </title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='cms/css/cms_base.css') }}">
    <script src="{{ url_for('static', filename='cms/js/cms_base.js') }}"></script>
    {# 提示框资源文件 #}
    <script src="{{ url_for('static', filename='common/sweetalert/sweetalert.min.js') }}"></script>
    <script src="{{ url_for('static', filename='common/sweetalert/clalert.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='common/sweetalert/sweetalert.css') }}">
    {% block head %}
    {% endblock %}
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
    <div class="container-fluid">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">熊熊乐园论坛CMS管理系统</a>
        </div>
        <div id="navbar" class="navbar-collapse collapse">
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">{{ g.cms_user.username }}<span>[{% block role %}{% endblock %}]</span></a></li>
                <li><a href="{{ url_for('cms.logout') }}">注销</a></li>
            </ul>
            <form class="navbar-form navbar-right">
                <input type="text" class="form-control" placeholder="查找...">
            </form>
        </div>
    </div>
</nav>

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3 col-md-2 sidebar">
            <ul class="nav-sidebar">
                <li class="unfold"><a href="{{ url_for('cms.index') }}">首页</a></li>
                <li class="profile-li">
                    <a href="#">个人中心<span></span></a>
                    <ul class="subnav">
                        <li><a href="{{ url_for('cms.profile') }}">个人信息</a></li>
                        <li><a href="{{ url_for('cms.resetpwd') }}">修改密码</a></li>
                        <li><a href="{{ url_for('cms.resetemail') }}">修改邮箱</a></li>
                    </ul>
                </li>

                {% set user = g.cms_user %}
                {% if user.has_permission(CMSPermission.POSTER) %}
                    <li class="nav-group post-manage"><a href="#">帖子管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.COMMENTER) %}
                    <li class="comments-manage"><a href="#">评论管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.BOARDER) %}
                    <li class="board-manage"><a href="#">板块管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.FRONTUSER) %}
                    <li class="nav-group user-manage"><a href="#">前台用户管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.CMSUSER) %}
                    <li class="nav-group cmsuser-manage"><a href="#">CMS用户管理</a></li>
                {% endif %}
                {% if user.is_developer %}
                    <li class="cmsrole-manage"><a href="#">CMS组管理</a></li>
                {% endif %}


            </ul>
        </div>
        <div class="col-sm-9 col-sm-offset-3 col-md-10 col-md-offset-2 main">
            <h1>{% block page_title %} {% endblock %}</h1>
            <div class="main_content">
                {% block content %} {% endblock %}
            </div>
        </div>
    </div>
</div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879808182838485868788899091929394
```

显示：
![flask bbs cms  client auth](https://img-blog.csdnimg.cn/20200619204327107.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，不同的用户左侧管理栏，显示有所区别。

## 四、服务端权限验证

现逐步实现左侧管理栏所有功能对应的视图函数和模板。

在templates/cms目录下新建cms_posts.html如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台帖子管理
{% endblock %}

{% block page_title %}
    熊熊帖子管理
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    帖子管理部分
{% endblock %}
1234567891011121314151617
```

新建cms_boards.html如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台板块管理
{% endblock %}

{% block page_title %}
    熊熊板块管理
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    板块管理部分
{% endblock %}
1234567891011121314151617
```

新建cms_comments.html如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台评论管理
{% endblock %}

{% block page_title %}
    熊熊评论管理
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    评论管理部分
{% endblock %}
1234567891011121314151617
```

新建cms_fusers.html如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛前台用户管理
{% endblock %}

{% block page_title %}
    熊熊前台用户管理
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    前台用户管理部分
{% endblock %}
1234567891011121314151617
```

新建cms_cusers.html如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台用户管理
{% endblock %}

{% block page_title %}
    熊熊后台管理
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    后台用户管理部分
{% endblock %}
1234567891011121314151617
```

新建cms_croles.html如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台用户组管理
{% endblock %}

{% block page_title %}
    熊熊后台用户组管理
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    后台用户组管理部分
{% endblock %}
1234567891011121314151617
```

views.py修改如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message
from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm
from apps.cms.models import CMSUser, CMSRole
from exts import db, mail
from utils import restful, random_captcha, clcache

# from .decorators import login_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request


@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html', max_role=g.max_role)


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html", max_role=g.max_role)


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html', max_role=g.max_role)

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html', max_role=g.max_role)

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            email = form.email.data
            # 修改用户邮箱
            g.cms_user.email = email
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


@cms_bp.route('/posts/')
def posts():
    return render_template('cms/cms_posts.html', max_role=g.max_role)


@cms_bp.route('/comments/')
def comments():
    return render_template('cms/cms_comments.html', max_role=g.max_role)


@cms_bp.route('/boards/')
def boards():
    return render_template('cms/cms_boards.html', max_role=g.max_role)


@cms_bp.route('/fusers/')
def fusers():
    return render_template('cms/cms_fusers.html', max_role=g.max_role)


@cms_bp.route('/cusers/')
def cusers():
    return render_template('cms/cms_cusers.html', max_role=g.max_role)


@cms_bp.route('/croles/')
def croles():
    return render_template('cms/cms_croles.html', max_role=g.max_role)


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153
```

cms_base.html修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>
        {% block title %}

        {% endblock %}
    </title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='cms/css/cms_base.css') }}">
    <script src="{{ url_for('static', filename='cms/js/cms_base.js') }}"></script>
    {# 提示框资源文件 #}
    <script src="{{ url_for('static', filename='common/sweetalert/sweetalert.min.js') }}"></script>
    <script src="{{ url_for('static', filename='common/sweetalert/clalert.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='common/sweetalert/sweetalert.css') }}">
    {% block head %}
    {% endblock %}
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
    <div class="container-fluid">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">熊熊乐园论坛CMS管理系统</a>
        </div>
        <div id="navbar" class="navbar-collapse collapse">
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">{{ g.cms_user.username }}<span>[{% block role %}{% endblock %}]</span></a></li>
                <li><a href="{{ url_for('cms.logout') }}">注销</a></li>
            </ul>
            <form class="navbar-form navbar-right">
                <input type="text" class="form-control" placeholder="查找...">
            </form>
        </div>
    </div>
</nav>

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3 col-md-2 sidebar">
            <ul class="nav-sidebar">
                <li class="unfold"><a href="{{ url_for('cms.index') }}">首页</a></li>
                <li class="profile-li">
                    <a href="#">个人中心<span></span></a>
                    <ul class="subnav">
                        <li><a href="{{ url_for('cms.profile') }}">个人信息</a></li>
                        <li><a href="{{ url_for('cms.resetpwd') }}">修改密码</a></li>
                        <li><a href="{{ url_for('cms.resetemail') }}">修改邮箱</a></li>
                    </ul>
                </li>

                {% set user = g.cms_user %}
                {% if user.has_permission(CMSPermission.POSTER) %}
                    <li class="nav-group post-manage"><a href="{{ url_for('cms.posts') }}">帖子管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.COMMENTER) %}
                    <li class="comments-manage"><a href="{{ url_for('cms.comments') }}">评论管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.BOARDER) %}
                    <li class="board-manage"><a href="{{ url_for('cms.boards') }}">板块管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.FRONTUSER) %}
                    <li class="nav-group user-manage"><a href="{{ url_for('cms.fusers') }}">前台用户管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.CMSUSER) %}
                    <li class="nav-group cmsuser-manage"><a href="{{ url_for('cms.cusers') }}">CMS用户管理</a></li>
                {% endif %}
                {% if user.is_developer %}
                    <li class="cmsrole-manage"><a href="{{ url_for('cms.croles') }}">CMS组管理</a></li>
                {% endif %}


            </ul>
        </div>
        <div class="col-sm-9 col-sm-offset-3 col-md-10 col-md-offset-2 main">
            <h1>{% block page_title %} {% endblock %}</h1>
            <div class="main_content">
                {% block content %} {% endblock %}
            </div>
        </div>
    </div>
</div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879808182838485868788899091929394
```

static/cms/js/cms_base.js修改如下：

```js
$(function () {
    $('.nav-sidebar>li>a').click(function (event) {
        var that = $(this);
        if (that.children('a').attr('href') == '#') {
            event.preventDefault();
        }
        if (that.parent().hasClass('unfold')) {
            that.parent().removeClass('unfold');
        } else {
            that.parent().addClass('unfold').siblings().removeClass('unfold');
        }
        console.log('coming....');
    });

    $('.nav-sidebar a').mouseleave(function () {
        $(this).css('text-decoration', 'none');
    });
});


$(function () {
    var url = window.location.href;
    if (url.indexOf('profile') >= 0) {
        var profileLi = $('.profile-li');
        profileLi.addClass('unfold').siblings().removeClass('unfold');
        profileLi.children('.subnav').children().eq(0).addClass('active').siblings().removeClass('active');
    } else if (url.indexOf('resetpwd') >= 0) {
        var profileLi = $('.profile-li');
        profileLi.addClass('unfold').siblings().removeClass('unfold');
        profileLi.children('.subnav').children().eq(1).addClass('active').siblings().removeClass('active');
    } else if (url.indexOf('resetemail') >= 0) {
        var profileLi = $('.profile-li');
        profileLi.addClass('unfold').siblings().removeClass('unfold');
        profileLi.children('.subnav').children().eq(2).addClass('active').siblings().removeClass('active');
    } else if (url.indexOf('posts') >= 0) {
        var postManageLi = $('.post-manage');
        postManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('boards') >= 0) {
        var boardManageLi = $('.board-manage');
        boardManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('permissions') >= 0) {
        var permissionManageLi = $('.permission-manage');
        permissionManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('froles') >= 0) {
        var roleManageLi = $('.role-manage');
        roleManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('fusers') >= 0) {
        var userManageLi = $('.user-manage');
        userManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('cusers') >= 0) {
        var cmsuserManageLi = $('.cmsuser-manage');
        cmsuserManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('croles') >= 0) {
        var cmsroleManageLi = $('.cmsrole-manage');
        cmsroleManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('comments') >= 0) {
        var commentsManageLi = $('.comments-manage');
        commentsManageLi.addClass('unfold').siblings().removeClass('unfold');
    }
});
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960
```

显示：
![flask bbs cms background perfect](https://img-blog.csdnimg.cn/20200619204406964.gif#pic_center)

显然，可以通过点击管理栏跳转到不同的路由，同时还可以通过键入路由来访问，这并没有进行限制，需要在服务端进一步进行权限验证。

在apps/cms/decorators.py中创建用于验证权限的装饰器如下：

```python
from flask import session, redirect, url_for, g
from functools import wraps

def login_required(func):
    def index(*args, **kwargs):
        if 'user_id' in session:
            return func(*args, **kwargs)
        else:
            return redirect(url_for('cms.login'))
    return index


def permission_required(permission):
    '''装饰器传参，多层嵌套'''
    def outter(func):
        @wraps(func)
        def inner(*args, **kwargs):
            user = g.cms_user
            if user.has_permission(permission):
                return func(*args, **kwargs)
            else:
                return redirect(url_for('cms.index'))
        return inner
    return outter
123456789101112131415161718192021222324
```

因为装饰器中要传入具体权限参数进行验证，所以定义装饰器时要多层嵌套，并且使用`wraps`装饰器来传入中层函数的参数，以便内层函数返回。

再在views.py中对视图函数使用装饰器进行装饰，如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message

from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm
from apps.cms.models import CMSUser, CMSPermission
from exts import db, mail
from utils import restful, random_captcha, clcache
from .decorators import permission_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request

@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html', max_role=g.max_role)


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html", max_role=g.max_role)


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html', max_role=g.max_role)

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html', max_role=g.max_role)

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            email = form.email.data
            # 修改用户邮箱
            g.cms_user.email = email
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


@cms_bp.route('/posts/')
@permission_required(CMSPermission.POSTER)
def posts():
    return render_template('cms/cms_posts.html', max_role=g.max_role)


@cms_bp.route('/comments/')
@permission_required(CMSPermission.COMMENTER)
def comments():
    return render_template('cms/cms_comments.html', max_role=g.max_role)


@cms_bp.route('/boards/')
@permission_required(CMSPermission.BOARDER)
def boards():
    return render_template('cms/cms_boards.html', max_role=g.max_role)


@cms_bp.route('/fusers/')
@permission_required(CMSPermission.FRONTUSER)
def fusers():
    return render_template('cms/cms_fusers.html', max_role=g.max_role)


@cms_bp.route('/cusers/')
@permission_required(CMSPermission.CMSUSER)
def cusers():
    return render_template('cms/cms_cusers.html', max_role=g.max_role)


@cms_bp.route('/croles/')
@permission_required(CMSPermission.ADMINER)
def croles():
    return render_template('cms/cms_croles.html', max_role=g.max_role)


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158
```

先创建一个新用户为运营角色用于测试，先执行`python manage.py create_cms_user -u testuser -p test12 -e test@test.com`创建测试用户，再执行命令`>python manage.py add_user_to_role -e test@test.com -n 运营`将其关联到运营角色。

现进行测试如下：
![authflask bbs cms authority test](https://img-blog.csdnimg.cn/20200619204429584.gif#pic_center)
显然，因为测试用户的角色为运营，有管理帖子、管理评论、管理前台和后台用户的权限，但是没有板块管理和CMS组管理的权限，所以在访问cms/boards和cms/croles的时候，自动重定向到后台首页。

# Python全栈（八）Flask项目实战之6.前台注册功能开发

Github和Gitee代码**同步更新**：
https://github.com/PythonWebProject/Flask_BBS；
https://gitee.com/Python_Web_Project/Flask_BBS。

后台的整体页面和基本架构已经搭建好，具体管理功能还未实现，现在转到前台开发，在前台完善之后，再转回后台开发。

## 一、前台用户模型的定义

在apps/front/models.py中定义模型如下：

```python
import shortuuid
import enum
from werkzeug.security import generate_password_hash, check_password_hash
from datetime import datetime

from exts import db


class GenderEnum(enum.Enum):
    MALE = 1
    FAMALE = 2
    SECRET = 3
    UNKNOWN = 4


class FrontUser(db.Model):
    __tablename__ = 'front_user'
    id = db.Column(db.String(40), primary_key=True, default=shortuuid.uuid)
    telephone = db.Column(db.String(11), nullable=False, unique=True)
    username = db.Column(db.String(50), nullable=False)
    _password = db.Column(db.String(100), nullable=False)
    email = db.Column(db.String(100), unique=False)
    realname = db.Column(db.String(30))
    avatar = db.Column(db.String(80))
    signature = db.Column(db.String(200))
    gender = db.Column(db.Enum(GenderEnum), default=GenderEnum.UNKNOWN)

    join_time = db.Column(db.DateTime, default=datetime.now)

    def __init__(self, *args, **kwargs):
        if 'password' in kwargs:
            self.password = kwargs.get('password')
            kwargs.pop('password')

        super().__init__(*args, **kwargs)

    @property
    def password(self):
        '''获取密码'''
        return self._password

    @password.setter
    def password(self, raw_password):
        '''设置密码'''
        self._password = generate_password_hash(raw_password)

    def check_password(self, raw_password):
        '''验证密码是否正确'''
        result = check_password_hash(self.password, raw_password)
        return result

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051
```

在manage.py中进行映射如下：

```python
from flask_script import Manager
from bbs import app
from flask_migrate import Migrate, MigrateCommand
from exts import db
from apps.cms.models import CMSUser, CMSRole, CMSPermission
from apps.front.models import FrontUser

manager = Manager(app)
Migrate(app, db)

manager.add_command('db', MigrateCommand)


@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
@manager.option('-e', '--email', dest='email')
def create_cms_user(username, password, email):
    user = CMSUser(username=username, password=password, email=email)
    db.session.add(user)
    db.session.commit()
    print('CMS用户添加成功')


@manager.command
def create_role():
    # 访问者
    visitor = CMSRole(name='访问者', desc='只能查看数据，不能修改数据')
    visitor.permissions = CMSPermission.VISITOR  # 也可以省去，因为默认权限就是VISITOR

    # 运营者
    operator = CMSRole(name='运营', desc='管理帖子，管理评论，管理前台和后台用户')
    # 有多个权限时，使用或运算表示
    operator.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER
    # 管理员
    admin = CMSRole(name='管理员', desc='拥有本系统大部分权限')
    admin.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER | CMSPermission.BOARDER

    # 开发人员
    developer = CMSRole(name='开发者', desc='拥有所有权限')
    developer.permissions = CMSPermission.ALL_PERMISSION

    db.session.add_all([visitor, operator, admin, developer])
    db.session.commit()
    print('角色添加成功')


@manager.command
def test_permission():
    user = CMSUser.query.first()
    if user.has_permission(CMSPermission.VISITOR):
        print('该用户有使用者权限')
    else:
        print('该用户没有使用者权限')


@manager.option('-e', '--email', dest='email')
@manager.option('-n', '--name', dest='name')
def add_user_to_role(email, name):
    user = CMSUser.query.filter_by(email=email).first()
    if user:
        role = CMSRole.query.filter_by(name=name).first()
        if role:
            role.users.append(user)
            db.session.commit()
            print('用户添加到角色添加成功')
        else:
            print('角色不存在')
    else:
        print('邮箱不存在')


if __name__ == '__main__':
    manager.run()

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374
```

在命令行中执行`python manage.py db migrate`和`python manage.py db upgrade`后，再查询数据库：

```sql
show tables;
1
```

打印：

```sql
+---------------------+        
| Tables_in_flask_bbs |        
+---------------------+        
| alembic_version     |        
| cms_role            |        
| cms_role_user       |        
| cms_user            |        
| front_user          |        
+---------------------+        
5 rows in set (0.01 sec)       
                               
1234567891011
```

显然，已经建表成功。

说明：
（1）在定义模型类时，如果指定了`__tablename__`属性，则映射到数据库中的表名即为指定的值；
如果未指定`__tablename__`，则根据模型名进行命名：
如果模型名为非驼峰命名，则数据库表名为模型名的小写形式，如模型名为Frontuser时，数据库表名为frontuser；
如果模型名为驼峰命名，则数据库表名为模型名用下划线连接，如模型名为FrontUser时，数据库表名为front_user。

（2）前台用户id不采用自增的整数而采用shortuuid，是为了防止用户恶意攻击、获取网站的用户信息，可以在很大程度上提高网站的安全性。

现进行插入前台用户数据测试，manage.py如下：

```python
from flask_script import Manager
from bbs import app
from flask_migrate import Migrate, MigrateCommand
from exts import db
from apps.cms.models import CMSUser, CMSRole, CMSPermission
from apps.front.models import FrontUser

manager = Manager(app)
Migrate(app, db)

manager.add_command('db', MigrateCommand)


@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
@manager.option('-e', '--email', dest='email')
def create_cms_user(username, password, email):
    user = CMSUser(username=username, password=password, email=email)
    db.session.add(user)
    db.session.commit()
    print('CMS用户添加成功')


@manager.command
def create_role():
    # 访问者
    visitor = CMSRole(name='访问者', desc='只能查看数据，不能修改数据')
    visitor.permissions = CMSPermission.VISITOR  # 也可以省去，因为默认权限就是VISITOR

    # 运营者
    operator = CMSRole(name='运营', desc='管理帖子，管理评论，管理前台和后台用户')
    # 有多个权限时，使用或运算表示
    operator.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER
    # 管理员
    admin = CMSRole(name='管理员', desc='拥有本系统大部分权限')
    admin.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER | CMSPermission.BOARDER

    # 开发人员
    developer = CMSRole(name='开发者', desc='拥有所有权限')
    developer.permissions = CMSPermission.ALL_PERMISSION

    db.session.add_all([visitor, operator, admin, developer])
    db.session.commit()
    print('角色添加成功')


@manager.command
def test_permission():
    user = CMSUser.query.first()
    if user.has_permission(CMSPermission.VISITOR):
        print('该用户有使用者权限')
    else:
        print('该用户没有使用者权限')


@manager.option('-e', '--email', dest='email')
@manager.option('-n', '--name', dest='name')
def add_user_to_role(email, name):
    user = CMSUser.query.filter_by(email=email).first()
    if user:
        role = CMSRole.query.filter_by(name=name).first()
        if role:
            role.users.append(user)
            db.session.commit()
            print('用户添加到角色添加成功')
        else:
            print('角色不存在')
    else:
        print('邮箱不存在')


@manager.option('-t', '--telephone', dest='telephone')
@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
def create_front_user(telephone, username, password):
    user = FrontUser(telephone=telephone, username=username, password=password)
    db.session.add(user)
    db.session.commit()
    print('前台用户添加成功')


if __name__ == '__main__':
    manager.run()

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384
```

在命令行中执行命令`python manage.py create_front_user -t 123888888 88 -u fronttestuser -p testfr`，再查询数据库：

```sql
select * from front_user;
1
```

打印：

```sql
+------------------------+-------------+---------------+------------------------------------------------------------------------------------------------+-------+----------+--------+-----------+---------+---------------------+     
| id                     | telephone   | username      | _password                                                                                      | email | realname | avatar | signature | gender  | join_time           |     
+------------------------+-------------+---------------+------------------------------------------------------------------------------------------------+-------+----------+--------+-----------+---------+---------------------+     
| VfNR5hNVP264oYJifjFfUG | 12388888888 | fronttestuser | pbkdf2:sha256:150000$IcWCTIzC$91196213fc1f0aa8662ef23113d555591c0fb11111e356950124529e8d7a581c | NULL  | NULL     | NULL   | NULL      | UNKNOWN | 2020-06-23 18:30:20 |     
+------------------------+-------------+---------------+------------------------------------------------------------------------------------------------+-------+----------+--------+-----------+---------+---------------------+     
1 row in set (0.01 sec)                                                                                                                                                                                                               
                                                                                                                                                                                                                                      
1234567
```

显然，数据插入成功。

## 二、注册页面完成和图形验证码类

### 1.注册和登录页面搭建

现进行前端注册页面的完成。

在templates目录下新建front目录，下创建注册页面front_signup.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>论坛注册</title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <!-- <link href="{{ url_for('static', filename='front/css/front_signbase.css') }}" rel="stylesheet"> -->
    <link href="css/front_signbase.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</head>
<body>
<div class="outer-box">
    <div class="logo-box">
        <a href="/">
            <img src="{{ url_for('static',filename='common/images/logo.png') }}"/>
        </a>
    </div>
    <h2 class="page-title">熊熊论坛注册</h2>

    <div class="sign-box">
        <div class="form-group">
            <div class="input-group">
                <input type="text" name="telephone" class="form-control" placeholder="手机号码">
                <span class="input-group-btn">
                        <button class="btn btn-default">
                            发送验证码
                        </button>
                    </span>
            </div>
        </div>
        <div class="form-group">
            <input type="text" name="sms_captcha" class="form-control" placeholder="短信验证码">
        </div>
        <div class="form-group">
            <input type="text" name="username" class="form-control" placeholder="用户名">
        </div>
        <div class="form-group">
            <input type="password" name="password" class="form-control" placeholder="密码">
        </div>
        <div class="form-group">
            <input type="password" name="password2" class="form-control" placeholder="确认密码">
        </div>
        <div class="form-group">
            <div class="input-group">
                <input type="text" name="graph_captcha" class="form-control" placeholder="图形验证码">

                <span class="input-group-addon captcha-addon">
                        图形验证码
{#                    <img id="captcha-img" class="captcha-img" src="{{ url_for('front.graph_captcha') }}" alt="">#}
                </span>
            </div>
        </div>
        <div class="form-group">
            <button class="btn btn-warning btn-block">
                立即注册
            </button>
        </div>
    </div>

</div>
</body>
</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263
```

再创建登录页面front_signin.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>论坛登录</title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <link href="{{ url_for('static', filename='front/css/front_signbase.css') }}" rel="stylesheet">
    <!--    <link href="../../static/front/css/front_signbase.css" rel="stylesheet">-->
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <script src="{{ url_for('static',filename='common/sweetalert/sweetalert.min.js') }}"></script>
    <script src="{{ url_for('static',filename='common/sweetalert/lgalert.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static',filename='common/sweetalert/sweetalert.css') }}">
    <script src="{{ url_for('static', filename='front/js/front_signin.js') }}"></script>
    <script src="https://cdn.bootcss.com/blueimp-md5/2.10.0/js/md5.js"></script>
</head>
<body>
<div class="outer-box">
    <div class="logo-box">
        <a href="/">
            <img src="{{ url_for('static',filename='common/images/logo.png') }}"/>
        </a>
    </div>
    <h2 class="page-title">
        熊熊论坛登录
    </h2>
    <div class="sign-box">
        <div class="form-group">
            <input type="text" class="form-control" name="telephone" placeholder="手机号码">
        </div>
        <div class="form-group">
            <input type="password" class="form-control" name="password" placeholder="密码">
        </div>
        <div class="checkbox">
            <label>
                <input type="checkbox" name="remember" value="1">记住我
            </label>
        </div>
        <div class="form-group">
            <button class="btn btn-warning btn-block" id="submit-btn">立即登录</button>
        </div>
        <div class="form-group">
            <a href="{{ url_for('front.signup') }}" class="signup-link">没有账号？立即注册</a>
            <a href="#" class="resetpwd-link" style="float:right;">找回密码</a>
        </div>
    </div>
    <span style="display:none;" id="return-to-span">{{ return_to }}</span>
</div>
</body>
</html> 

12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152
```

在static目录下创建front目录，下创建css目录和js目录，css目录下创建front_signbase.css如下：

```css
body {
    background: #f3f3f3;
}

.outer-box {
    width: 854px;
    background: #fff;
    margin: 0 auto;
    overflow: hidden;
}

.logo-box {
    text-align: center;
    padding-top: 40px;
}

.logo-box img {
    width: 60px;
    height: 60px;
}

.page-title {
    text-align: center;
}

.sign-box {
    width: 300px;
    margin: 0 auto;
    padding-top: 50px;
}

.captcha-addon {
    padding: 0;
    overflow: hidden;
}

.captcha-img {
    width: 94px;
    height: 32px;
    cursor: pointer;
}

.captcha-addon {
    padding: 0;
    /*溢出隐藏*/
    overflow: hidden;
}

.captcha-img {
    width: 94px;
    height: 32px;
    cursor: pointer;
}
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253
```

js目录下创建front_signup.js如下：

```js
var param = {
    setParam: function (href, key, value) {
        // 重新加载整个页面
        var isReplaced = false;
        var urlArray = href.split('?');
        if (urlArray.length > 1) {
            var queryArray = urlArray[1].split('&');
            for (var i = 0; i < queryArray.length; i++) {
                var paramsArray = queryArray[i].split('=');
                if (paramsArray[0] == key) {
                    paramsArray[1] = value;
                    queryArray[i] = paramsArray.join('=');
                    isReplaced = true;
                    break;
                }
            }

            if (!isReplaced) {
                var params = {};
                params[key] = value;
                if (urlArray.length > 1) {
                    href = href + '&' + $.param(params);
                } else {
                    href = href + '?' + $.param(params);
                }
            } else {
                var params = queryArray.join('&');
                urlArray[1] = params;
                href = urlArray.join('?');
            }
        } else {
            var param = {};
            param[key] = value;
            if (urlArray.length > 1) {
                href = href + '&' + $.param(param);
            } else {
                href = href + '?' + $.param(param);
            }
        }
        return href;
    }
};


var clajax = {
    'get': function (args) {
        args['method'] = 'get';
        this.ajax(args);
    },
    'post': function (args) {
        args['method'] = 'post';
        this.ajax(args);
    },
    'ajax': function (args) {
        // 设置csrftoken
        this._ajaxSetup();
        $.ajax(args);
    },
    '_ajaxSetup': function () {
        $.ajaxSetup({
            'beforeSend': function (xhr, settings) {
                if (!/^(GET|HEAD|OPTIONS|TRACE)$/i.test(settings.type) && !this.crossDomain) {
                    var csrftoken = $('meta[name=csrf-token]').attr('content');
                    xhr.setRequestHeader("X-CSRFToken", csrftoken)
                }
            }
        });
    }
};

$(function () {
    $('#captcha-img').click(function (event) {
        var self = $(this);
        var src = self.attr('src');
        var newsrc = param.setParam(src, 'cp', Math.random());
        self.attr('src', newsrc);
    });
});


// $(function () {
// var __encode ='sojson.com',_a={}, _0xb483=["\x5F\x64\x65\x63\x6F\x64\x65","\x68\x74\x74\x70\x3A\x2F\x2F\x77\x77\x77\x2E\x73\x6F\x6A\x73\x6F\x6E\x2E\x63\x6F\x6D\x2F\x6A\x61\x76\x61\x73\x63\x72\x69\x70\x74\x6F\x62\x66\x75\x73\x63\x61\x74\x6F\x72\x2E\x68\x74\x6D\x6C"];(function(_0xd642x1){_0xd642x1[_0xb483[0]]= _0xb483[1]})(_a);var __Ox82dc8=["\x70\x72\x65\x76\x65\x6E\x74\x44\x65\x66\x61\x75\x6C\x74","\x76\x61\x6C","\x69\x6E\x70\x75\x74\x5B\x6E\x61\x6D\x65\x3D\x27\x74\x65\x6C\x65\x70\x68\x6F\x6E\x65\x27\x5D","\x74\x65\x73\x74","\u8BF7\u8F93\u5165\u6B63\u786E\u7684\u624B\u673A\u53F7\u7801\uFF01","\x61\x6C\x65\x72\x74\x49\x6E\x66\x6F","\x67\x65\x74\x54\x69\x6D\x65","\x71\x33\x34\x32\x33\x38\x30\x35\x67\x64\x66\x6C\x76\x62\x64\x66\x76\x68\x73\x64\x6F\x61\x60\x23\x24\x25","\x2F\x63\x2F\x73\x6D\x73\x5F\x63\x61\x70\x74\x63\x68\x61\x2F","\x63\x6F\x64\x65","\u77ED\u4FE1\u9A8C\u8BC1\u7801\u53D1\u9001\u6210\u529F\uFF01","\x61\x6C\x65\x72\x74\x53\x75\x63\x63\x65\x73\x73\x54\x6F\x61\x73\x74","\x64\x69\x73\x61\x62\x6C\x65\x64","\x61\x74\x74\x72","\x74\x65\x78\x74","\x72\x65\x6D\x6F\x76\x65\x41\x74\x74\x72","\u53D1\u9001\u9A8C\u8BC1\u7801","\x6D\x65\x73\x73\x61\x67\x65","\x61\x6C\x65\x72\x74\x49\x6E\x66\x6F\x54\x6F\x61\x73\x74","\x70\x6F\x73\x74","\x63\x6C\x69\x63\x6B","\x23\x73\x6D\x73\x2D\x63\x61\x70\x74\x63\x68\x61\x2D\x62\x74\x6E","\x75\x6E\x64\x65\x66\x69\x6E\x65\x64","\x6C\x6F\x67","\u5220\u9664","\u7248\u672C\u53F7\uFF0C\x6A\x73\u4F1A\u5B9A\u671F\u5F39\u7A97\uFF0C","\u8FD8\u8BF7\u652F\u6301\u6211\u4EEC\u7684\u5DE5\u4F5C","\x73\x6F\x6A\x73","\x6F\x6E\x2E\x63\x6F\x6D"];$(function(){$(__Ox82dc8[0x15])[__Ox82dc8[0x14]](function(_0x6781x1){_0x6781x1[__Ox82dc8[0x0]]();var _0x6781x2=$(this);var _0x6781x3=$(__Ox82dc8[0x2])[__Ox82dc8[0x1]]();if(!(/^1[345879]\d{9}$/[__Ox82dc8[0x3]](_0x6781x3))){lgalert[__Ox82dc8[0x5]](__Ox82dc8[0x4]);return};var _0x6781x4=( new Date)[__Ox82dc8[0x6]]();var _0x6781x5=md5(_0x6781x4+ _0x6781x3+ __Ox82dc8[0x7]);lgajax[__Ox82dc8[0x13]]({'\x75\x72\x6C':__Ox82dc8[0x8],'\x64\x61\x74\x61':{'\x74\x65\x6C\x65\x70\x68\x6F\x6E\x65':_0x6781x3,'\x74\x69\x6D\x65\x73\x74\x61\x6D\x70':_0x6781x4,'\x73\x69\x67\x6E':_0x6781x5},'\x73\x75\x63\x63\x65\x73\x73':function(_0x6781x6){if(_0x6781x6[__Ox82dc8[0x9]]== 200){lgalert[__Ox82dc8[0xb]](__Ox82dc8[0xa]);_0x6781x2[__Ox82dc8[0xd]](__Ox82dc8[0xc],__Ox82dc8[0xc]);var _0x6781x7=60;var _0x6781x8=setInterval(function(){_0x6781x7--;_0x6781x2[__Ox82dc8[0xe]](_0x6781x7);if(_0x6781x7<= 0){_0x6781x2[__Ox82dc8[0xf]](__Ox82dc8[0xc]);clearInterval(_0x6781x8);_0x6781x2[__Ox82dc8[0xe]](__Ox82dc8[0x10])}},1000)}else {lgalert[__Ox82dc8[0x12]](_0x6781x6[__Ox82dc8[0x11]])}}})})});;;(function(_0x6781x9,_0x6781xa,_0x6781xb,_0x6781xc,_0x6781xd,_0x6781xe){_0x6781xe= __Ox82dc8[0x16];_0x6781xc= function(_0x6781xf){if( typeof alert!== _0x6781xe){alert(_0x6781xf)};if( typeof console!== _0x6781xe){console[__Ox82dc8[0x17]](_0x6781xf)}};_0x6781xb= function(_0x6781x10,_0x6781x9){return _0x6781x10+ _0x6781x9};_0x6781xd= _0x6781xb(__Ox82dc8[0x18],_0x6781xb(__Ox82dc8[0x19],__Ox82dc8[0x1a]));try{_0x6781x9= __encode;if(!( typeof _0x6781x9!== _0x6781xe&& _0x6781x9=== _0x6781xb(__Ox82dc8[0x1b],__Ox82dc8[0x1c]))){_0x6781xc(_0x6781xd)}}catch(e){_0x6781xc(_0x6781xd)}})({})
// })

$(function () {
    $("#sms-captcha-btn").click(function (event) {
        event.preventDefault();
        var self = $(this);
        var telephone = $("input[name='telephone']").val();
        if (!(/^1[345879]\d{9}$/.test(telephone))) {

            clalert.alertInfo('请输入正确的手机号码！');
            return;
        }
        var timestamp = (new Date).getTime();

        var sign = md5(timestamp + telephone + "q3423805gdflvbdfvhsdoa`#$%");
        clajax.post({
            'url': '/c/sms_captcha/',
            'data': {
                'telephone': telephone,
                'timestamp': timestamp,
                'sign': sign
            },
            'success': function (data) {
                if (data['code'] === 200) {
                    clalert.alertSuccessToast('短信验证码发送成功！');
                    self.attr("disabled", 'disabled');
                    var timeCount = 60;
                    var timer = setInterval(function () {
                        timeCount--;
                        self.text(timeCount);
                        if (timeCount <= 0) {
                            self.removeAttr('disabled');
                            clearInterval(timer);
                            self.text('发送验证码');
                        }
                    }, 1000);
                } else {
                    clalert.alertInfoToast(data['message']);
                }
            }
        });
    });
});

$(function () {
    $("#submit-btn").click(function (event) {
        event.preventDefault();
        var telephone_input = $("input[name='telephone']");
        var sms_captcha_input = $("input[name='sms_captcha']");
        var username_input = $("input[name='username']");
        var password1_input = $("input[name='password1']");
        var password2_input = $("input[name='password2']");
        var graph_captcha_input = $("input[name='graph_captcha']");

        var telephone = telephone_input.val();
        var sms_captcha = sms_captcha_input.val();
        var username = username_input.val();
        var password1 = password1_input.val();
        var password2 = password2_input.val();
        var graph_captcha = graph_captcha_input.val();

        clajax.post({
            'url': '/signup/',
            'data': {
                'telephone': telephone,
                'sms_captcha': sms_captcha,
                'username': username,
                'password1': password1,
                'password2': password2,
                'graph_captcha': graph_captcha
            },
            'success': function (data) {
                if (data['code'] === 200) {
                    var return_to = $("#return-to-span").text();
                    if (return_to) {
                        window.location = return_to;
                    } else {
                        window.location = '/';
                    }
                } else {
                    clalert.alertInfo(data['message']);
                }
            },
            'fail': function () {
                clalert.alertNetworkError();
            }
        });
    });
});
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171
```

再创建front_signin.js如下：

```js
var clajax = {
    'get': function (args) {
        args['method'] = 'get';
        this.ajax(args);
    },
    'post': function (args) {
        args['method'] = 'post';
        this.ajax(args);
    },
    'ajax': function (args) {
        // 设置csrftoken
        this._ajaxSetup();
        $.ajax(args);
    },
    '_ajaxSetup': function () {
        $.ajaxSetup({
            'beforeSend': function (xhr, settings) {
                if (!/^(GET|HEAD|OPTIONS|TRACE)$/i.test(settings.type) && !this.crossDomain) {
                    var csrftoken = $('meta[name=csrf-token]').attr('content');
                    xhr.setRequestHeader("X-CSRFToken", csrftoken)
                }
            }
        });
    }
};

$(function () {
    $("#submit-btn").click(function (event) {
        event.preventDefault();
        var telephone_input = $("input[name='telephone']");
        var password_input = $("input[name='password']");
        var remember_input = $("input[name='remember']");

        var telephone = telephone_input.val();
        var password = password_input.val();
        var remember = remember_input.checked ? 1 : 0;


        clajax.post({
            'url': '/signin/',
            'data': {
                'telephone': telephone,
                'password': password,
                'remember': remember
            },
            'success': function (data) {
                if (data['code'] === 200) {
                    var return_to = $("#return-to-span").text();
                    if (return_to) {
                        window.location = return_to;
                    } else {
                        window.location = '/';
                    }
                } else {
                    clalert.alertInfo(data['message']);
                }
            }
        });
    });
});
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960
```

在apps/front/views中实现视图类如下：

```python
from flask import Blueprint, views, render_template

front_bp = Blueprint('front', __name__)


@front_bp.route('/')
def index():
    return '前台首页'


class SignupView(views.MethodView):
    def get(self):
        return render_template('front/front_signup.html')


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))

1234567891011121314151617
```

进行访问测试如下：
![flsk front signup raw page](https://img-blog.csdnimg.cn/20200624124808295.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

此时，已经访问到注册页面，只是还没有加载样式。

在static/common下创建目录images，下放入logo.gif，front_signup.html修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>论坛注册</title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <link href="{{ url_for('static', filename='front/css/front_signbase.css') }}" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</head>
<body>
<div class="outer-box">
    <div class="logo-box">
        <a href="/">
            <img src="{{ url_for('static',filename='common/images/logo.gif') }}"/>
        </a>
    </div>
    <h2 class="page-title">熊熊论坛注册</h2>

    <div class="sign-box">
        <div class="form-group">
            <div class="input-group">
                <input type="text" name="telephone" class="form-control" placeholder="手机号码">
                <span class="input-group-btn">
                        <button class="btn btn-default">
                            发送验证码
                        </button>
                    </span>
            </div>
        </div>
        <div class="form-group">
            <input type="text" name="sms_captcha" class="form-control" placeholder="短信验证码">
        </div>
        <div class="form-group">
            <input type="text" name="username" class="form-control" placeholder="用户名">
        </div>
        <div class="form-group">
            <input type="password" name="password" class="form-control" placeholder="密码">
        </div>
        <div class="form-group">
            <input type="password" name="password2" class="form-control" placeholder="确认密码">
        </div>
        <div class="form-group">
            <div class="input-group">
                <input type="text" name="graph_captcha" class="form-control" placeholder="图形验证码">

                <span class="input-group-addon captcha-addon">
                        图形验证码
{#                    <img id="captcha-img" class="captcha-img" src="{{ url_for('front.graph_captcha') }}" alt="">#}
                </span>
            </div>
        </div>
        <div class="form-group">
            <button class="btn btn-warning btn-block">
                立即注册
            </button>
        </div>
    </div>

</div>
</body>
</html>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162
```

再次访问，显示：
![flsk front signup css rendered page](https://img-blog.csdnimg.cn/20200624124826752.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

此时的页面使用了css，变得更加美观。

### 2.图形验证码的实现

本项目中，图形验证码主要通过pillow库实现，需要通过`pip install pillow`命令安装。

在utils目录下创建captcha目录，下创建`__init__.py`如下：

```python
import random
import string
# Image：一个画布
# ImageDraw：一个画笔
# ImageFont:画笔的字体
from PIL import Image, ImageDraw, ImageFont


# Captcha验证码
class Captcha(object):
    # 生成几位数的验证码
    number = 4
    # 验证码图片的宽度和高度
    size = (100, 30)
    # 验证码字体大小
    fontsize = 25
    # 加入干扰线的条数
    line_number = 2

    # 构建一个验证码源文本
    SOURCE = list(string.ascii_letters)
    for index in range(0, 10):
        SOURCE.append(str(index))

    # 用来绘制干扰线
    @classmethod
    def __gene_line(cls, draw, width, height):
        begin = (random.randint(0, width), random.randint(0, height))
        end = (random.randint(0, width), random.randint(0, height))
        draw.line([begin, end], fill=cls.__gene_random_color(), width=2)

    # 用来绘制干扰点
    @classmethod
    def __gene_points(cls, draw, point_chance, width, height):
        chance = min(100, max(0, int(point_chance)))  # 大小限制在[0, 100]
        for w in range(width):
            for h in range(height):
                tmp = random.randint(0, 100)
                if tmp > 100 - chance:
                    draw.point((w, h), fill=cls.__gene_random_color())

    # 生成随机的颜色
    @classmethod
    def __gene_random_color(cls, start=0, end=255):
        random.seed()
        return (random.randint(start, end), random.randint(start, end), random.randint(start, end))

    # 随机选择一个字体
    @classmethod
    def __gene_random_font(cls):
        fonts = [
            'arial.ttf',
            'framdit.ttf',
            'segoepr.ttf',
            'simkai.ttf',
            'framd.ttf',
            'Inkfree.ttf',
            'ariblk.ttf'
        ]
        font = random.choice(fonts)
        return 'utils/captcha/' + font

    # 用来随机生成一个字符串(包括英文和数字)
    @classmethod
    def gene_text(cls, number):
        # number是生成验证码的位数
        return ''.join(random.sample(cls.SOURCE, number))

    # 生成验证码
    @classmethod
    def gene_graph_captcha(cls):
        # 验证码图片的宽和高
        width, height = cls.size
        # 创建图片
        # R：Red（红色）0-255
        # G：G（绿色）0-255
        # B：B（蓝色）0-255
        # A：Alpha（透明度）
        image = Image.new('RGBA', (width, height), cls.__gene_random_color(0, 100))
        # 验证码的字体
        font = ImageFont.truetype(cls.__gene_random_font(), cls.fontsize)
        # 创建画笔
        draw = ImageDraw.Draw(image)
        # 生成字符串
        text = cls.gene_text(cls.number)
        # 获取字体的尺寸
        font_width, font_height = font.getsize(text)
        # 填充字符串
        draw.text(((width - font_width) / 2, (height - font_height) / 2), text, font=font,
                  fill=cls.__gene_random_color(150, 255))
        # 绘制干扰线
        for x in range(0, cls.line_number):
            cls.__gene_line(draw, width, height)
        # 绘制噪点
        cls.__gene_points(draw, 10, width, height)
        return (text, image)


if __name__ == '__main__':
    c = Captcha()
    print(c.gene_graph_captcha())

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102
```

该目录下还包括需要用到的一些字体，如需使用更多字体，还可以在`C:\Windows\Fonts`目录中自行选择。

## 三、点击更换图形验证码

apps/front/views.py中定义生成验证码图片并返回前端的视图函数，如下：

```python
from flask import Blueprint, views, render_template, make_response
from io import BytesIO

from utils.captcha import Captcha

front_bp = Blueprint('front', __name__)


@front_bp.route('/')
def index():
    return '前台首页'


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


class SignupView(views.MethodView):
    def get(self):
        return render_template('front/front_signup.html')


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))

123456789101112131415161718192021222324252627282930313233343536373839
```

测试如下：
![flsk front signup graph_captcha generate](https://img-blog.csdnimg.cn/20200624124854162.gif#pic_center)

显然，已经生成了随机验证码。

接下来实现将生成的验证码插入到注册页面中，front_signup.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>论坛注册</title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <link href="{{ url_for('static', filename='front/css/front_signbase.css') }}" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</head>
<body>
<div class="outer-box">
    <div class="logo-box">
        <a href="/">
            <img src="{{ url_for('static',filename='common/images/logo.gif') }}"/>
        </a>
    </div>
    <h2 class="page-title">熊熊论坛注册</h2>

    <div class="sign-box">
        <div class="form-group">
            <div class="input-group">
                <input type="text" name="telephone" class="form-control" placeholder="手机号码">
                <span class="input-group-btn">
                        <button class="btn btn-default">
                            发送验证码
                        </button>
                    </span>
            </div>
        </div>
        <div class="form-group">
            <input type="text" name="sms_captcha" class="form-control" placeholder="短信验证码">
        </div>
        <div class="form-group">
            <input type="text" name="username" class="form-control" placeholder="用户名">
        </div>
        <div class="form-group">
            <input type="password" name="password" class="form-control" placeholder="密码">
        </div>
        <div class="form-group">
            <input type="password" name="password2" class="form-control" placeholder="确认密码">
        </div>
        <div class="form-group">
            <div class="input-group">
                <input type="text" name="graph_captcha" class="form-control" placeholder="图形验证码">

                <span class="input-group-addon captcha-addon">
                    <img id="captcha-img" class="captcha-img" src="{{ url_for('front.graph_captcha') }}" alt="">
                </span>
            </div>
        </div>
        <div class="form-group">
            <button class="btn btn-warning btn-block">
                立即注册
            </button>
        </div>
    </div>

</div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061
```

显示：
![flsk front signup captcha insert](https://img-blog.csdnimg.cn/20200624124918480.gif#pic_center)

此时，实现了在注册页面插入验证码，但是并不能通过点击验证码来更换验证码，需要通过JS实现，front_signup.html修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>论坛注册</title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <link href="{{ url_for('static', filename='front/css/front_signbase.css') }}" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <script src="{{ url_for('static', filename='front/js/front_signup.js') }}"></script>
</head>
<body>
<div class="outer-box">
    <div class="logo-box">
        <a href="/">
            <img src="{{ url_for('static',filename='common/images/logo.gif') }}"/>
        </a>
    </div>
    <h2 class="page-title">熊熊论坛注册</h2>

    <div class="sign-box">
        <div class="form-group">
            <div class="input-group">
                <input type="text" name="telephone" class="form-control" placeholder="手机号码">
                <span class="input-group-btn">
                        <button class="btn btn-default">
                            发送验证码
                        </button>
                    </span>
            </div>
        </div>
        <div class="form-group">
            <input type="text" name="sms_captcha" class="form-control" placeholder="短信验证码">
        </div>
        <div class="form-group">
            <input type="text" name="username" class="form-control" placeholder="用户名">
        </div>
        <div class="form-group">
            <input type="password" name="password" class="form-control" placeholder="密码">
        </div>
        <div class="form-group">
            <input type="password" name="password2" class="form-control" placeholder="确认密码">
        </div>
        <div class="form-group">
            <div class="input-group">
                <input type="text" name="graph_captcha" class="form-control" placeholder="图形验证码">

                <span class="input-group-addon captcha-addon">
                    <img id="captcha-img" class="captcha-img" src="{{ url_for('front.graph_captcha') }}" alt="">
                </span>
            </div>
        </div>
        <div class="form-group">
            <button class="btn btn-warning btn-block">
                立即注册
            </button>
        </div>
    </div>

</div>
</body>
</html>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162
```

显示：
![flsk front signup captcha change](https://img-blog.csdnimg.cn/20200624142358792.gif#pic_center)

显然，此时可以点击验证码图片来更换验证码，从网络请求可以看到，该功能是通过改变获取验证码的请求的参数实现的。

## 四、发送短信验证码

在这里我们需要选择第三方短信发送平台来发送验证码，常见的包括阿里大于、聚合数据、云片等，这里我选择云片https://www.yunpian.com/console/#/admin进行测试。

使用前需要注册账号、实名认证并创建签名、模板，认证后会赠送10条短信，用于测试足够，如有更大的需求，可以按需进行充值。

这个过程会比较麻烦，因为新增签名时需要进行公司、网站或产品等的认证，对于个人来说，最简单的渠道就是通过公众号进行认证，新增签名示例如下：
![云片 签名报备](https://img-blog.csdnimg.cn/20200624142416127.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

新增模板示例如下：
![云片 模板报备](https://img-blog.csdnimg.cn/20200624142441543.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

完成新增并等待完成审核后，就可以获取APIKEY了，步骤如下：
![云片 获取APIKEY](https://img-blog.csdnimg.cn/20200624142456427.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

此时就可以接入API了，可以自行查看Python API文档，也可以直接按下面的步骤来：
（1）在虚拟环境中安装Python SDK：

```shell
pip install yunpian-python-sdk
1
```

（2）在utils目录下创建send_msg.py如下：

```python
from yunpian_python_sdk.model import constant as YC
from yunpian_python_sdk.ypclient import YunpianClient
from config import YP_API

def send_mobile_msg(mobile, code):
    # 初始化client,apikey作为所有请求的默认值
    clnt = YunpianClient(YP_API)
    param = {YC.MOBILE: mobile, YC.TEXT: '【Python进化讲堂】欢迎您注册熊熊论坛，验证码：{}（5分钟内有效，如非本人操作，请忽略）'.format(code)}
    r = clnt.sms().single_send(param)
    print(r.code(), r.msg(), r.data())
    return r.code()

123456789101112
```

config.py修改如下：

```python
import os

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_bbs'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

TEMPLATE_AUTO_RELOAD = True
SQLALCHEMY_DATABASE_URI = DB_URL
SQLALCHEMY_TRACK_MODIFICATIONS = False
SQLALCHEMY_POOL_RECYCLE = 280
SQLALCHEMY_POOL_SIZE = 20

# 设置密钥
SECRET_KEY = os.urandom(15)

# 发送邮箱服务地址
MAIL_SERVER = 'smtp.qq.com'
# 邮箱端口，为587或465，为587时TLS设置为True，为465时SSL设置为True
MAIL_PORT = 587
MAIL_USE_TLS = True
# MAIL_USE_SSL = True # MAIL_PORT为465时设置此项
# 用户名可以为你的邮箱，需要自行添加
MAIL_USERNAME = '379869029@qq.com'
# 邮箱密码，不是邮箱账号密码，而是第三方客户端登录使用的授权码，需要自行获取
MAIL_PASSWORD = 'vrxiqciwjnbcxxxx'
# 发送者即你的邮箱，需要自行添加
MAIL_DEFAULT_SENDER = '379869029@qq.com'

# 云片APIKEY
YP_API = 'edf71361381f31b3957beda37f20xxxx'
12345678910111213141516171819202122232425262728293031323334
```

完成这两步之后，就可以将发送短信验证码的功能集成到项目中了。

如果在后面发送短信验证码失败，原因可能是IP限制，云片网对使用云片短信服务的IP进行了限制，需要将公网IP添加到IP白名单，到https://www.yunpian.com/console/#/setting/system/ipWhiteList添加即可。

在apps/common目录下创建views.py如下：

```python
from flask import Blueprint, request
from utils import send_msg, restful
from utils.captcha import Captcha

common_bp = Blueprint('common', __name__, url_prefix='/c')


@common_bp.route('/sms_captcha/', methods=['POST'])
def sms_captcha():
    telephone = request.form.get('telephone')
    if not telephone:
        return restful.params_error(message='请填写手机号码')

    captcha = Captcha.gene_text(4)
    if send_msg.send_mobile_msg(telephone, captcha) == 0:
        return restful.success()
    else:
        return restful.params_error(message='发送失败')
123456789101112131415161718
```

将其注册到主程序文件bbs.py中如下：

```python
'''
前台 front
后台 cms
公有 common
'''

from flask import Flask
from flask_wtf import CSRFProtect
from exts import db, mail
from apps.cms.views import cms_bp
from apps.front.views import front_bp
from apps.common.views import common_bp
import config


app = Flask(__name__) # type:Flask
CSRFProtect(app)
app.config.from_object(config)
db.init_app(app)
mail.init_app(app)

app.register_blueprint(cms_bp)
app.register_blueprint(front_bp)
app.register_blueprint(common_bp)


if __name__ == '__main__':
    app.run(debug=True)
12345678910111213141516171819202122232425262728
```

front_signup.html修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>论坛注册</title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <link href="{{ url_for('static', filename='front/css/front_signbase.css') }}" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <script src="{{ url_for('static', filename='front/js/front_signup.js') }}"></script>
    {# 提示框资源文件 #}
    <script src="{{ url_for('static', filename='common/sweetalert/sweetalert.min.js') }}"></script>
    <script src="{{ url_for('static', filename='common/sweetalert/clalert.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='common/sweetalert/sweetalert.css') }}">
    {#  md5加密JS文件导入  #}
    <script src="https://cdn.bootcss.com/blueimp-md5/2.10.0/js/md5.js"></script>
</head>
<body>
<div class="outer-box">
    <div class="logo-box">
        <a href="/">
            <img src="{{ url_for('static',filename='common/images/logo.gif') }}"/>
        </a>
    </div>
    <h2 class="page-title">熊熊论坛注册</h2>

    <div class="sign-box">
        <div class="form-group">
            <div class="input-group">
                <input type="text" name="telephone" class="form-control" placeholder="手机号码">
                <span class="input-group-btn">
                        <button class="btn btn-default" id="sms-captcha-btn">
                            发送验证码
                        </button>
                    </span>
            </div>
        </div>
        <div class="form-group">
            <input type="text" name="sms_captcha" class="form-control" placeholder="短信验证码">
        </div>
        <div class="form-group">
            <input type="text" name="username" class="form-control" placeholder="用户名">
        </div>
        <div class="form-group">
            <input type="password" name="password" class="form-control" placeholder="密码">
        </div>
        <div class="form-group">
            <input type="password" name="password2" class="form-control" placeholder="确认密码">
        </div>
        <div class="form-group">
            <div class="input-group">
                <input type="text" name="graph_captcha" class="form-control" placeholder="图形验证码">

                <span class="input-group-addon captcha-addon">
                    <img id="captcha-img" class="captcha-img" src="{{ url_for('front.graph_captcha') }}" alt="">
                </span>
            </div>
        </div>
        <div class="form-group">
            <button class="btn btn-warning btn-block">
                立即注册
            </button>
        </div>
    </div>

</div>
</body>
</html>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768
```

变化主要有：
（1）给发送验证码的button标签添加id，用于增加JS事件；
（2）导入提示框资源文件，用于不同情况的弹框提示；
（3）导入md5加密JS文件，用于后续的加密。

进行测试：
![flsk front signup msg captcha test](https://img-blog.csdnimg.cn/20200624142554593.gif#pic_center)

显然，此时已经可以发送短信了，只是后续的CSRF保护等处理还没有实现，所以状态码是400。

## 五、短信验证码接口加密验证和JS代码加密

前面的状态码400（Bad Request）是因为在模板中没有加入csrf，因此请求失败，现在front_signup.html中加入csrf如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>论坛注册</title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <link href="{{ url_for('static', filename='front/css/front_signbase.css') }}" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <script src="{{ url_for('static', filename='front/js/front_signup.js') }}"></script>
    {# 提示框资源文件 #}
    <script src="{{ url_for('static', filename='common/sweetalert/sweetalert.min.js') }}"></script>
    <script src="{{ url_for('static', filename='common/sweetalert/clalert.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='common/sweetalert/sweetalert.css') }}">
    {#  md5加密JS文件导入  #}
    <script src="https://cdn.bootcss.com/blueimp-md5/2.10.0/js/md5.js"></script>
</head>
<body>
<div class="outer-box">
    <div class="logo-box">
        <a href="/">
            <img src="{{ url_for('static',filename='common/images/logo.gif') }}"/>
        </a>
    </div>
    <h2 class="page-title">熊熊论坛注册</h2>

    <div class="sign-box">
        <div class="form-group">
            <div class="input-group">
                <input type="text" name="telephone" class="form-control" placeholder="手机号码">
                <span class="input-group-btn">
                        <button class="btn btn-default" id="sms-captcha-btn">
                            发送验证码
                        </button>
                    </span>
            </div>
        </div>
        <div class="form-group">
            <input type="text" name="sms_captcha" class="form-control" placeholder="短信验证码">
        </div>
        <div class="form-group">
            <input type="text" name="username" class="form-control" placeholder="用户名">
        </div>
        <div class="form-group">
            <input type="password" name="password" class="form-control" placeholder="密码">
        </div>
        <div class="form-group">
            <input type="password" name="password2" class="form-control" placeholder="确认密码">
        </div>
        <div class="form-group">
            <div class="input-group">
                <input type="text" name="graph_captcha" class="form-control" placeholder="图形验证码">

                <span class="input-group-addon captcha-addon">
                    <img id="captcha-img" class="captcha-img" src="{{ url_for('front.graph_captcha') }}" alt="">
                </span>
            </div>
        </div>
        <div class="form-group">
            <button class="btn btn-warning btn-block">
                立即注册
            </button>
        </div>
    </div>

</div>
</body>
</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869
```

再进行测试：
![flsk front signup msg captcha success](https://img-blog.csdnimg.cn/20200624142617811.gif#pic_center)

显然，此时已经可以正常发送验证码。

但是此时可以看到明显的发送验证码的请求，如果被恶意使用者发现，很可能会利用该请求地址滥用网站的验证码发送资源、造成损失，甚至可能遭遇恶意攻击，因此需要对发送验证码的功能采取一定的验证保护措施，即进行表单验证，在apps/common下创建表单文件forms.py如下：

```python
from wtforms import Form, StringField
from wtforms.validators import InputRequired, regexp
import hashlib

class SMSCaptchaForm(Form):
    telephone = StringField(validators=[regexp(r'1[3-9]\d{9}')])
    timestamp = StringField(validators=[regexp(r'\d{13}')])
    sign = StringField(validators=[InputRequired()])

    def validate_sign(self, field):
        '''验证前端发送过来的sign与后端用同样的加密方式生成的sign是否一致'''
        telephone = self.telephone.data
        timestamp = self.timestamp.data
        # 前端传来的sign
        sign = self.sign.data
        # 后端生成的sign
        sign2 = hashlib.md5((timestamp + telephone + "q3423805gdflvbdfvhsdoa`#$%").encode('utf-8')).hexdigest()
        print('Client Sign:', sign)
        print('Server Sign:', sign2)

        if sign == sign2:
            return True
        else:
            return False
123456789101112131415161718192021222324
```

因为在前端发送验证码时，传回了ms加密信息，代码为`var sign = md5(timestamp + telephone + "q3423805gdflvbdfvhsdoa#$%");`，我们可以在后端采用同样的加密方式来加密3个参数，通过对比来判断是否为正常发送。

apps/common/views.py修改如下：

```python
from flask import Blueprint, request
from utils import send_msg, restful
from utils.captcha import Captcha
from .forms import SMSCaptchaForm

common_bp = Blueprint('common', __name__, url_prefix='/c')


@common_bp.route('/sms_captcha/', methods=['POST'])
def sms_captcha():
    form = SMSCaptchaForm(request.form)
    
    if form.validate():
        return restful.success()
    else:
        return restful.params_error(message='参数错误')
    
    '''
    telephone = request.form.get('telephone')
    if not telephone:
        return restful.params_error(message='请填写手机号码')

    captcha = Captcha.gene_text(4)
    if send_msg.send_mobile_msg(telephone, captcha) == 0:
        return restful.success()
    else:
        return restful.params_error(message='发送失败')
    '''
12345678910111213141516171819202122232425262728
```

再次访问：
![flsk front signup msg captcha MD5 validate](https://img-blog.csdnimg.cn/20200624142651392.gif#pic_center)

显然，此时可以对手机号进行验证，只有在手机号输入正确时才会发送验证码，但是也可以看到，在前端可以访问到JS文件，可以看到MD5加密的具体方式，因此需要进一步对JS文件进行加密，待加密的代码部分如下：
![JS待加密代码](https://img-blog.csdnimg.cn/20200624142800294.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
有很多网站可以对JS代码进行加密，如https://www.sojson.com/jscodeconfusion.html等，并且有多种加密方式，如JS普通加密、JS混合加密、JS代码混淆等，加密后的front_signup.html如下：

```js
var param = {
    setParam: function (href, key, value) {
        // 重新加载整个页面
        var isReplaced = false;
        var urlArray = href.split('?');
        if (urlArray.length > 1) {
            var queryArray = urlArray[1].split('&');
            for (var i = 0; i < queryArray.length; i++) {
                var paramsArray = queryArray[i].split('=');
                if (paramsArray[0] == key) {
                    paramsArray[1] = value;
                    queryArray[i] = paramsArray.join('=');
                    isReplaced = true;
                    break;
                }
            }

            if (!isReplaced) {
                var params = {};
                params[key] = value;
                if (urlArray.length > 1) {
                    href = href + '&' + $.param(params);
                } else {
                    href = href + '?' + $.param(params);
                }
            } else {
                var params = queryArray.join('&');
                urlArray[1] = params;
                href = urlArray.join('?');
            }
        } else {
            var param = {};
            param[key] = value;
            if (urlArray.length > 1) {
                href = href + '&' + $.param(param);
            } else {
                href = href + '?' + $.param(param);
            }
        }
        return href;
    }
};


var clajax = {
    'get': function (args) {
        args['method'] = 'get';
        this.ajax(args);
    },
    'post': function (args) {
        args['method'] = 'post';
        this.ajax(args);
    },
    'ajax': function (args) {
        // 设置csrftoken
        this._ajaxSetup();
        $.ajax(args);
    },
    '_ajaxSetup': function () {
        $.ajaxSetup({
            'beforeSend': function (xhr, settings) {
                if (!/^(GET|HEAD|OPTIONS|TRACE)$/i.test(settings.type) && !this.crossDomain) {
                    var csrftoken = $('meta[name=csrf-token]').attr('content');
                    xhr.setRequestHeader("X-CSRFToken", csrftoken)
                }
            }
        });
    }
};

$(function () {
    $('#captcha-img').click(function (event) {
        var self = $(this);
        var src = self.attr('src');
        var newsrc = param.setParam(src, 'cp', Math.random());
        self.attr('src', newsrc);
    });
});


$(function () {
    var __encode ='sojson.com',_a={}, _0xb483=["\x5F\x64\x65\x63\x6F\x64\x65","\x68\x74\x74\x70\x3A\x2F\x2F\x77\x77\x77\x2E\x73\x6F\x6A\x73\x6F\x6E\x2E\x63\x6F\x6D\x2F\x6A\x61\x76\x61\x73\x63\x72\x69\x70\x74\x6F\x62\x66\x75\x73\x63\x61\x74\x6F\x72\x2E\x68\x74\x6D\x6C"];(function(_0xd642x1){_0xd642x1[_0xb483[0]]= _0xb483[1]})(_a);var __Ox89ca5=["\x70\x72\x65\x76\x65\x6E\x74\x44\x65\x66\x61\x75\x6C\x74","\x76\x61\x6C","\x69\x6E\x70\x75\x74\x5B\x6E\x61\x6D\x65\x3D\x27\x74\x65\x6C\x65\x70\x68\x6F\x6E\x65\x27\x5D","\x74\x65\x73\x74","\u8BF7\u8F93\u5165\u6B63\u786E\u7684\u624B\u673A\u53F7\u7801\uFF01","\x61\x6C\x65\x72\x74\x49\x6E\x66\x6F","\x67\x65\x74\x54\x69\x6D\x65","\x71\x33\x34\x32\x33\x38\x30\x35\x67\x64\x66\x6C\x76\x62\x64\x66\x76\x68\x73\x64\x6F\x61\x60\x23\x24\x25","\x2F\x63\x2F\x73\x6D\x73\x5F\x63\x61\x70\x74\x63\x68\x61\x2F","\x63\x6F\x64\x65","\u77ED\u4FE1\u9A8C\u8BC1\u7801\u53D1\u9001\u6210\u529F\uFF01","\x61\x6C\x65\x72\x74\x53\x75\x63\x63\x65\x73\x73\x54\x6F\x61\x73\x74","\x64\x69\x73\x61\x62\x6C\x65\x64","\x61\x74\x74\x72","\x74\x65\x78\x74","\x72\x65\x6D\x6F\x76\x65\x41\x74\x74\x72","\u53D1\u9001\u9A8C\u8BC1\u7801","\x6D\x65\x73\x73\x61\x67\x65","\x61\x6C\x65\x72\x74\x49\x6E\x66\x6F\x54\x6F\x61\x73\x74","\x70\x6F\x73\x74","\x63\x6C\x69\x63\x6B","\x23\x73\x6D\x73\x2D\x63\x61\x70\x74\x63\x68\x61\x2D\x62\x74\x6E","\x75\x6E\x64\x65\x66\x69\x6E\x65\x64","\x6C\x6F\x67","\u5220\u9664","\u7248\u672C\u53F7\uFF0C\x6A\x73\u4F1A\u5B9A\u671F\u5F39\u7A97\uFF0C","\u8FD8\u8BF7\u652F\u6301\u6211\u4EEC\u7684\u5DE5\u4F5C","\x73\x6F\x6A\x73","\x6F\x6E\x2E\x63\x6F\x6D"];$(__Ox89ca5[0x15])[__Ox89ca5[0x14]](function(_0x6286x1){_0x6286x1[__Ox89ca5[0x0]]();var _0x6286x2=$(this);var _0x6286x3=$(__Ox89ca5[0x2])[__Ox89ca5[0x1]]();if(!(/^1[345879]\d{9}$/[__Ox89ca5[0x3]](_0x6286x3))){clalert[__Ox89ca5[0x5]](__Ox89ca5[0x4]);return};var _0x6286x4=( new Date)[__Ox89ca5[0x6]]();var _0x6286x5=md5(_0x6286x4+ _0x6286x3+ __Ox89ca5[0x7]);clajax[__Ox89ca5[0x13]]({'\x75\x72\x6C':__Ox89ca5[0x8],'\x64\x61\x74\x61':{'\x74\x65\x6C\x65\x70\x68\x6F\x6E\x65':_0x6286x3,'\x74\x69\x6D\x65\x73\x74\x61\x6D\x70':_0x6286x4,'\x73\x69\x67\x6E':_0x6286x5},'\x73\x75\x63\x63\x65\x73\x73':function(_0x6286x6){if(_0x6286x6[__Ox89ca5[0x9]]=== 200){clalert[__Ox89ca5[0xb]](__Ox89ca5[0xa]);_0x6286x2[__Ox89ca5[0xd]](__Ox89ca5[0xc],__Ox89ca5[0xc]);var _0x6286x7=60;var _0x6286x8=setInterval(function(){_0x6286x7--;_0x6286x2[__Ox89ca5[0xe]](_0x6286x7);if(_0x6286x7<= 0){_0x6286x2[__Ox89ca5[0xf]](__Ox89ca5[0xc]);clearInterval(_0x6286x8);_0x6286x2[__Ox89ca5[0xe]](__Ox89ca5[0x10])}},1000)}else {clalert[__Ox89ca5[0x12]](_0x6286x6[__Ox89ca5[0x11]])}}})});;;(function(_0x6286x9,_0x6286xa,_0x6286xb,_0x6286xc,_0x6286xd,_0x6286xe){_0x6286xe= __Ox89ca5[0x16];_0x6286xc= function(_0x6286xf){if( typeof alert!== _0x6286xe){alert(_0x6286xf)};if( typeof console!== _0x6286xe){console[__Ox89ca5[0x17]](_0x6286xf)}};_0x6286xb= function(_0x6286x10,_0x6286x9){return _0x6286x10+ _0x6286x9};_0x6286xd= _0x6286xb(__Ox89ca5[0x18],_0x6286xb(__Ox89ca5[0x19],__Ox89ca5[0x1a]));try{_0x6286x9= __encode;if(!( typeof _0x6286x9!== _0x6286xe&& _0x6286x9=== _0x6286xb(__Ox89ca5[0x1b],__Ox89ca5[0x1c]))){_0x6286xc(_0x6286xd)}}catch(e){_0x6286xc(_0x6286xd)}})({})
});

$(function () {
    $("#submit-btn").click(function (event) {
        event.preventDefault();
        var telephone_input = $("input[name='telephone']");
        var sms_captcha_input = $("input[name='sms_captcha']");
        var username_input = $("input[name='username']");
        var password1_input = $("input[name='password1']");
        var password2_input = $("input[name='password2']");
        var graph_captcha_input = $("input[name='graph_captcha']");

        var telephone = telephone_input.val();
        var sms_captcha = sms_captcha_input.val();
        var username = username_input.val();
        var password1 = password1_input.val();
        var password2 = password2_input.val();
        var graph_captcha = graph_captcha_input.val();

        clajax.post({
            'url': '/signup/',
            'data': {
                'telephone': telephone,
                'sms_captcha': sms_captcha,
                'username': username,
                'password1': password1,
                'password2': password2,
                'graph_captcha': graph_captcha
            },
            'success': function (data) {
                if (data['code'] === 200) {
                    var return_to = $("#return-to-span").text();
                    if (return_to) {
                        window.location = return_to;
                    } else {
                        window.location = '/';
                    }
                } else {
                    clalert.alertInfo(data['message']);
                }
            },
            'fail': function () {
                clalert.alertNetworkError();
            }
        });
    });
});
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129
```

示例如下：
![flsk front signup msg captcha JS encryption](https://img-blog.csdnimg.cn/20200624142825922.gif#pic_center)

显然，此时难以再轻易地获取到MD5加密的具体方式，安全性也提高了很多。

# Python全栈（八）Flask项目实战之7.前台注册和登录功能

Github和Gitee代码**同步更新**：
https://github.com/PythonWebProject/Flask_BBS；
https://gitee.com/Python_Web_Project/Flask_BBS。

## 一、注册功能完成

在完成注册功能时，首先需要将生成的图形验证码和短信验证码保存到Redis中，apps/common/views.py如下：

```python
from flask import Blueprint, request
from utils import send_msg, restful
from utils.captcha import Captcha
from .forms import SMSCaptchaForm
from utils import clcache

common_bp = Blueprint('common', __name__, url_prefix='/c')


@common_bp.route('/sms_captcha/', methods=['POST'])
def sms_captcha():
    form = SMSCaptchaForm(request.form)

    if form.validate():
        telephone = form.telephone.data
        captcha = Captcha.gene_text(4)
        print('发送的短信验证码：{}'.format(captcha))
        if send_msg.send_mobile_msg(telephone, captcha) == 0:
            clcache.save_captcha(telephone, captcha)
            return restful.success()
        else:
            return restful.params_error(message='发送失败')
    else:
        return restful.params_error(message='参数错误')
123456789101112131415161718192021222324
```

apps/front/views.py如下：

```python
from flask import Blueprint, views, render_template, make_response, request
from io import BytesIO

from utils.captcha import Captcha
from utils import clcache, restful
from .forms import SignupForm
from .models import FrontUser
from exts import db

front_bp = Blueprint('front', __name__)


@front_bp.route('/')
def index():
    return '前台首页'


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


class SignupView(views.MethodView):
    def get(self):
        return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960
```

此时访问注册页，并输入手机号获取验证码，再查询Redis，可以看到新插入的验证码数据，默认会在5分钟后过期。

在apps/front/forms.py中定义验证表单如下：

```python
from wtforms import Form, StringField, ValidationError
from wtforms.validators import Length, EqualTo, Regexp
from utils import clcache


class BaseForm(Form):
    def get_error(self):
        message = self.errors.popitem()[1][0]
        return message


class SignupForm(BaseForm):
    telephone = StringField(validators=[Regexp(r'1[3-9]\d{9}', message='请输入正确的手机号')])
    sms_captcha = StringField(validators=[Regexp(r'\w{4}', message='请输入4位短信验证码')])
    username = StringField(validators=[Length(min=2, max=20, message='用户名应介于2-20个字符，请重新输入')])
    password1 = StringField(validators=[Regexp(r'[0-9a-zA-Z_\.]{6,20}', message='密码应介于6-20位，并且由数字、英文字母或下划线、英文句号或短横线组成')])
    password2 = StringField(validators=[EqualTo('password1', message='两次输入密码不一致')])
    graph_captcha = StringField(validators=[Regexp(r'\w{4}', message='请输入4位图形验证码')])

    def validate_sms_captcha(self, field):
        telephone = self.telephone.data
        sms_captcha = self.sms_captcha.data

        # 从Redis中取验证码
        sms_captcha_redis = clcache.get_captcha(telephone)
        if not sms_captcha_redis or sms_captcha_redis.lower() != sms_captcha.lower():
            raise ValidationError(message='短信验证码过期或有误')

    def validate_graph_captcha(self, field):
        graph_captcha = self.graph_captcha.data
        
        # 从Redis中取图形验证码
        graph_captcha_redis = clcache.get_captcha(graph_captcha.lower())
        if not graph_captcha_redis or graph_captcha_redis.lower() != graph_captcha.lower():
            raise ValidationError(message='图形验证码过期或有误')
1234567891011121314151617181920212223242526272829303132333435
```

templates/front/front_signup.html修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>论坛注册</title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <link href="{{ url_for('static', filename='front/css/front_signbase.css') }}" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <script src="{{ url_for('static', filename='front/js/front_signup.js') }}"></script>
    {# 提示框资源文件 #}
    <script src="{{ url_for('static', filename='common/sweetalert/sweetalert.min.js') }}"></script>
    <script src="{{ url_for('static', filename='common/sweetalert/clalert.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='common/sweetalert/sweetalert.css') }}">
    {#  md5加密JS文件导入  #}
    <script src="https://cdn.bootcss.com/blueimp-md5/2.10.0/js/md5.js"></script>
</head>
<body>
<div class="outer-box">
    <div class="logo-box">
        <a href="/">
            <img src="{{ url_for('static',filename='common/images/logo.gif') }}"/>
        </a>
    </div>
    <h2 class="page-title">熊熊论坛注册</h2>

    <div class="sign-box">
        <div class="form-group">
            <div class="input-group">
                <input type="text" name="telephone" class="form-control" placeholder="手机号码">
                <span class="input-group-btn">
                        <button class="btn btn-default" id="sms-captcha-btn">
                            发送验证码
                        </button>
                    </span>
            </div>
        </div>
        <div class="form-group">
            <input type="text" name="sms_captcha" class="form-control" placeholder="短信验证码">
        </div>
        <div class="form-group">
            <input type="text" name="username" class="form-control" placeholder="用户名">
        </div>
        <div class="form-group">
            <input type="password" name="password1" class="form-control" placeholder="密码">
        </div>
        <div class="form-group">
            <input type="password" name="password2" class="form-control" placeholder="确认密码">
        </div>
        <div class="form-group">
            <div class="input-group">
                <input type="text" name="graph_captcha" class="form-control" placeholder="图形验证码">

                <span class="input-group-addon captcha-addon">
                    <img id="captcha-img" class="captcha-img" src="{{ url_for('front.graph_captcha') }}" alt="">
                </span>
            </div>
        </div>
        <div class="form-group">
            <button class="btn btn-warning btn-block" id="submit-btn">
                立即注册
            </button>
        </div>
    </div>

</div>
</body>
</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869
```

现在进行完整的注册过程的测试如下：
![flsk front signup finish](https://img-blog.csdnimg.cn/20200625144324671.gif#pic_center)

显然，只有在表单完全验证成功后，才能完成注册的操作。
此时再查询数据库：

```sql
select * from front_user;
1
```

打印：

```sql
+------------------------+-------------+---------------+------------------------------------------------------------------------------------------------+-------+----------+--------+-----------+---------+---------------------+
| id                     | telephone   | username      | _password                                                                                      | email | realname | avatar | signature | gender  | join_time           |
+------------------------+-------------+---------------+------------------------------------------------------------------------------------------------+-------+----------+--------+-----------+---------+---------------------+
| 3wN6t8S7p9qzPNGCCjgHkL | 19911111111 | corley        | pbkdf2:sha256:150000$kFkAOs85$a5cb32d2f6cba09e4121e004d2033959869b3ee663028461af44458bc68497e6 | NULL  | NULL     | NULL   | NULL      | UNKNOWN | 2020-06-24 22:31:37 |
| VfNR5hNVP264oYJifjFfUG | 12388888888 | fronttestuser | pbkdf2:sha256:150000$IcWCTIzC$91196213fc1f0aa8662ef23113d555591c0fb11111e356950124529e8d7a581c | NULL  | NULL     | NULL   | NULL      | UNKNOWN | 2020-06-23 18:30:20 |
+------------------------+-------------+---------------+------------------------------------------------------------------------------------------------+-------+----------+--------+-----------+---------+---------------------+
2 rows in set (0.01 sec)

12345678
```

显然，已经将数据插入到表中。

## 二、注册跳转回上一个页面

前面注册完直接跳转到前台首页，但在一般网页中，可能会在浏览某个界面的时候需要进行注册或登录，在注册或登录完成后又需要跳转回之前的页面。

这里需要用到一个很重要的概念referer，这表示**页面的跳转**，即从哪一个页面跳转到当前页面，这里的跳转是指站内实现的自动跳转，而不是手动输入地址的跳转。先对referer进行简单测试，front/views.py如下：

```python
from flask import Blueprint, views, render_template, make_response, request
from io import BytesIO

from utils.captcha import Captcha
from utils import clcache, restful
from .forms import SignupForm
from .models import FrontUser
from exts import db

front_bp = Blueprint('front', __name__)


@front_bp.route('/')
def index():
    return '前台首页'


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))

12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667
```

在templates/front下新建模板referer_test.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>跳转测试</title>
</head>
<body>
<h3>
    <a href="{{ url_for('front.signup') }}">跳转测试</a>
</h3>

</body>
</html>
12345678910111213
```

进行测试如下：
![flsk front signup referer test](https://img-blog.csdnimg.cn/20200625144353779.gif#pic_center)

显然，在从referer_test跳转到signup之后，在请求参数中的Referer参数即为跳转之前的页面http://127.0.0.1:5000/referer_test/。

现进行完善，front/views.py如下：

```python
from flask import Blueprint, views, render_template, make_response, request
from io import BytesIO

from utils.captcha import Captcha
from utils import clcache, restful
from .forms import SignupForm
from .models import FrontUser
from exts import db

front_bp = Blueprint('front', __name__)


@front_bp.route('/')
def index():
    return '前台首页'


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        if return_to and return_to != request.url:
            return render_template('front/front_signup.html',return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071
```

front_signup.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>论坛注册</title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <link href="{{ url_for('static', filename='front/css/front_signbase.css') }}" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <script src="{{ url_for('static', filename='front/js/front_signup.js') }}"></script>
    {# 提示框资源文件 #}
    <script src="{{ url_for('static', filename='common/sweetalert/sweetalert.min.js') }}"></script>
    <script src="{{ url_for('static', filename='common/sweetalert/clalert.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='common/sweetalert/sweetalert.css') }}">
    {#  md5加密JS文件导入  #}
    <script src="https://cdn.bootcss.com/blueimp-md5/2.10.0/js/md5.js"></script>
</head>
<body>
<div class="outer-box">
    <div class="logo-box">
        <a href="/">
            <img src="{{ url_for('static',filename='common/images/logo.gif') }}"/>
        </a>
    </div>
    <h2 class="page-title">熊熊论坛注册</h2>

    <div class="sign-box">
        <div class="form-group">
            <div class="input-group">
                <input type="text" name="telephone" class="form-control" placeholder="手机号码">
                <span class="input-group-btn">
                        <button class="btn btn-default" id="sms-captcha-btn">
                            发送验证码
                        </button>
                    </span>
            </div>
        </div>
        <div class="form-group">
            <input type="text" name="sms_captcha" class="form-control" placeholder="短信验证码">
        </div>
        <div class="form-group">
            <input type="text" name="username" class="form-control" placeholder="用户名">
        </div>
        <div class="form-group">
            <input type="password" name="password1" class="form-control" placeholder="密码">
        </div>
        <div class="form-group">
            <input type="password" name="password2" class="form-control" placeholder="确认密码">
        </div>
        <div class="form-group">
            <div class="input-group">
                <input type="text" name="graph_captcha" class="form-control" placeholder="图形验证码">

                <span class="input-group-addon captcha-addon">
                    <img id="captcha-img" class="captcha-img" src="{{ url_for('front.graph_captcha') }}" alt="">
                </span>
            </div>
        </div>
        <div class="form-group">
            <span style="display: none"id="return-to-span">{{ return_to }}</span>
            <button class="btn btn-warning btn-block" id="submit-btn">
                立即注册
            </button>
        </div>
    </div>

</div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970
```

测试：
![flsk front signup referer hop](https://img-blog.csdnimg.cn/20200625144646871.gif#pic_center)

显然，在完成注册后，又跳回到跳转之前的页面。

但是还存在一个问题，如果是站外的地址设置跳转到本业的某个地址，再跳转回去就可能存在一定的风险，需要采取一些安全措施，急需要判断请求是否属于站内的请求。

在utils目录下创建url验证文件safe_url.py如下：

```python
from urllib.parse import urlparse, urljoin
from flask import request


def is_safe_url(target):
    ref_url = urlparse(request.host_url)
    test_url = urlparse(urljoin(request.host_url, target))
    
    return test_url.scheme in ('http', 'https') and ref_url.netloc == test_url.netloc

12345678910
```

views.py完善如下：

```python
from flask import Blueprint, views, render_template, make_response, request
from io import BytesIO

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm
from .models import FrontUser
from exts import db

front_bp = Blueprint('front', __name__)


@front_bp.route('/')
def index():
    return '前台首页'


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html',return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172
```

## 三、登录功能完成

登录功能也需要**表单验证**，forms.py如下：

```python
from wtforms import Form, StringField, ValidationError
from wtforms.validators import Length, EqualTo, Regexp, InputRequired
from utils import clcache


class BaseForm(Form):
    def get_error(self):
        message = self.errors.popitem()[1][0]
        return message


class SignupForm(BaseForm):
    telephone = StringField(validators=[Regexp(r'1[3-9]\d{9}', message='请输入正确的手机号')])
    sms_captcha = StringField(validators=[Regexp(r'\w{4}', message='请输入4位短信验证码')])
    username = StringField(validators=[Length(min=2, max=20, message='用户名应介于2-20个字符，请重新输入')])
    password1 = StringField(validators=[Regexp(r'[0-9a-zA-Z_\.]{6,20}', message='密码应介于6-20位，并且由数字、英文字母或下划线、英文句号或短横线组成')])
    password2 = StringField(validators=[EqualTo('password1', message='两次输入密码不一致')])
    graph_captcha = StringField(validators=[Regexp(r'\w{4}', message='请输入4位图形验证码')])

    def validate_sms_captcha(self, field):
        telephone = self.telephone.data
        sms_captcha = self.sms_captcha.data

        # 从Redis中取验证码
        sms_captcha_redis = clcache.get_captcha(telephone)
        if not sms_captcha_redis or sms_captcha_redis.lower() != sms_captcha.lower():
            raise ValidationError(message='短信验证码过期或有误')

    def validate_graph_captcha(self, field):
        graph_captcha = self.graph_captcha.data
        
        # 从Redis中取图形验证码
        graph_captcha_redis = clcache.get_captcha(graph_captcha.lower())
        if not graph_captcha_redis or graph_captcha_redis.lower() != graph_captcha.lower():
            raise ValidationError(message='图形验证码过期或有误')


class SigninForm(BaseForm):
    telephone = StringField(validators=[Regexp(r'1[3-9]\d{9}', message='请输入正确的手机号')])
    password = StringField(validators=[Regexp(r'[0-9a-zA-Z_\.]{6,20}', message='您输入的密码有误，请重新输入')])
    remember = StringField(InputRequired())
1234567891011121314151617181920212223242526272829303132333435363738394041
```

视图文件views.py如下：

```python
from flask import Blueprint, views, render_template, make_response, request, session
from io import BytesIO

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm
from .models import FrontUser
from exts import db

front_bp = Blueprint('front', __name__)


@front_bp.route('/')
def index():
    return '前台首页'


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html',return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899
```

referer_test.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>跳转测试</title>
</head>
<body>
<h3>
    <a href="{{ url_for('front.signup') }}">注册跳转测试</a>
</h3>
<h3>
    <a href="{{ url_for('front.signin') }}">登录跳转测试</a>
</h3>

</body>
</html>
12345678910111213141516
```

front_signin.html修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>论坛登录</title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <link href="{{ url_for('static', filename='front/css/front_signbase.css') }}" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <script src="{{ url_for('static',filename='common/sweetalert/sweetalert.min.js') }}"></script>
    <script src="{{ url_for('static',filename='common/sweetalert/clalert.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static',filename='common/sweetalert/sweetalert.css') }}">
    <script src="{{ url_for('static', filename='front/js/front_signin.js') }}"></script>
    <script src="https://cdn.bootcss.com/blueimp-md5/2.10.0/js/md5.js"></script>
</head>
<body>
<div class="outer-box">
    <div class="logo-box">
        <a href="/">
            <img src="{{ url_for('static',filename='common/images/logo.gif') }}"/>
        </a>
    </div>
    <h2 class="page-title">
        熊熊论坛登录
    </h2>
    <div class="sign-box">
        <div class="form-group">
            <input type="text" class="form-control" name="telephone" placeholder="手机号码">
        </div>
        <div class="form-group">
            <input type="password" class="form-control" name="password" placeholder="密码">
        </div>
        <div class="checkbox">
            <label>
                <input type="checkbox" name="remember" value="1">记住我
            </label>
        </div>
        <div class="form-group">
            <button class="btn btn-warning btn-block" id="submit-btn">立即登录</button>
        </div>
        <div class="form-group">
            <a href="{{ url_for('front.signup') }}" class="signup-link">没有账号？立即注册</a>
            <a href="#" class="resetpwd-link" style="float:right;">找回密码</a>
        </div>
    </div>
    <span style="display:none;" id="return-to-span">{{ return_to }}</span>
</div>
</body>
</html> 

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051
```

测试如下：
![flsk front signin finish](https://img-blog.csdnimg.cn/20200625144720281.gif#pic_center)

显然，对登录进行了表单验证，并且在登录成功后跳转回之前的页面。

## 四、首页和轮播图页面搭建

现在搭建首页页面。

从https://v3.bootcss.com/components/#navbar复制**导航条**模板样式并进行修改，新建front_index.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <title>首页</title>
</head>
<body>
<nav class="navbar navbar-default">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                    data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">Python栏目</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
                <li class="active"><a href="/">首页 <span class="sr-only">(current)</span></a></li>
            </ul>
            <form class="navbar-form navbar-left">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">搜索</button>
            </form>
            <ul class="nav navbar-nav navbar-right">
                <li><a href="{{ url_for('front.signin') }}">登录</a></li>
                <li><a href="{{ url_for('front.signup') }}">注册</a></li>
{#                <li class="dropdown">#}
{#                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"#}
{#                       aria-expanded="false">Dropdown <span class="caret"></span></a>#}
{#                    <ul class="dropdown-menu">#}
{#                        <li><a href="#">Action</a></li>#}
{#                        <li><a href="#">Another action</a></li>#}
{#                        <li><a href="#">Something else here</a></li>#}
{#                        <li role="separator" class="divider"></li>#}
{#                        <li><a href="#">Separated link</a></li>#}
{#                    </ul>#}
{#                </li>#}
            </ul>
        </div><!-- /.navbar-collapse -->
    </div><!-- /.container-fluid -->
</nav>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455
```

views.py修改如下：

```python
from flask import Blueprint, views, render_template, make_response, request, session
from io import BytesIO

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm
from .models import FrontUser
from exts import db

front_bp = Blueprint('front', __name__)


@front_bp.route('/')
def index():
    return render_template('front/front_index.html')


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html',return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899
```

显示：
![flask front index navigation bar](https://img-blog.csdnimg.cn/20200625144741245.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，基本的导航栏已经实现。

现在实现**轮播图**效果。

在static/front下创建images目录，下面存放3张图片python-1.jpg、python-2.jpg、python-3.jpg用于测试轮播图效果，front_index.hmtl完善如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <title>首页</title>
</head>
<body>
<nav class="navbar navbar-default">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                    data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">Python栏目</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
                <li class="active"><a href="/">首页 <span class="sr-only">(current)</span></a></li>
            </ul>
            <form class="navbar-form navbar-left">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">搜索</button>
            </form>
            <ul class="nav navbar-nav navbar-right">
                <li><a href="{{ url_for('front.signin') }}">登录</a></li>
                <li><a href="{{ url_for('front.signup') }}">注册</a></li>
                {#                <li class="dropdown">#}
                {#                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"#}
                {#                       aria-expanded="false">Dropdown <span class="caret"></span></a>#}
                {#                    <ul class="dropdown-menu">#}
                {#                        <li><a href="#">Action</a></li>#}
                {#                        <li><a href="#">Another action</a></li>#}
                {#                        <li><a href="#">Something else here</a></li>#}
                {#                        <li role="separator" class="divider"></li>#}
                {#                        <li><a href="#">Separated link</a></li>#}
                {#                    </ul>#}
                {#                </li>#}
            </ul>
        </div><!-- /.navbar-collapse -->
    </div><!-- /.container-fluid -->
</nav>

<div class="main-container">
    <div class="cl-container">
        <div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
            <!-- Indicators 指令 -->
            <ol class="carousel-indicators">
                <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
                <li data-target="#carousel-example-generic" data-slide-to="1"></li>
                <li data-target="#carousel-example-generic" data-slide-to="2"></li>
            </ol>

            <!-- Wrapper for slides 轮播图 -->
            <div class="carousel-inner" role="listbox">
                <div class="item active">
                    <img src="{{ url_for('static', filename='front/images/python-1.jpg') }}" alt="...">
                    <div class="carousel-caption">
                        ...
                    </div>
                </div>
                <div class="item">
                    <img src="{{ url_for('static', filename='front/images/python-2.jpg') }}" alt="...">
                    <div class="carousel-caption">
                        ...
                    </div>
                </div>
                <div class="item">
                    <img src="{{ url_for('static', filename='front/images/python-3.jpg') }}" alt="...">
                    <div class="carousel-caption">
                        ...
                    </div>
                </div>
                ...
            </div>

            <!-- Controls 左右切换控制 -->
            <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
                <span class="sr-only">Previous</span>
            </a>
            <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
                <span class="sr-only">Next</span>
            </a>
        </div>
    </div>

    <div class="sm-container">

    </div>
</div>

</body>
</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106
```

显示：
![flsk front index slides raw](https://img-blog.csdnimg.cn/20200625144759329.gif#pic_center)

显然，此时已经可以达到轮播的基本效果，但是由于一些样式还未完善，因此不太美观。

# Python全栈（八）Flask项目实战之8.CMS后台轮播图管理

Github和Gitee代码**同步更新**：
https://github.com/PythonWebProject/Flask_BBS；
https://gitee.com/Python_Web_Project/Flask_BBS。

## 一、首页轮播图

在前面的基础上进一步完善轮播图的功能。

static/front/css下新建front_base.css如下：

```css
a, abbr, acronym, address, applet, article, aside, audio, b, big, blockquote, body, canvas, caption, center, cite, code, dd, del, details, dfn, div, dl, dt, em, embed, fieldset, figcaption, figure, footer, form, h1, h2, h3, h4, h5, h6, header, html, i, iframe, img, ins, kbd, label, legend, li, mark, menu, nav, object, ol, output, p, pre, q, ruby, s, samp, section, small, span, strike, strong, sub, summary, sup, table, tbody, td, tfoot, th, thead, time, tr, tt, u, ul, var, video {
    margin: 0;
    padding: 0;
    border: 0;
    vertical-align: baseline;
    list-style: none;
}

.main-container {
    width: 990px;
    margin: 0 auto;
    overflow: hidden;
}

.cl-container {
    width: 730px;
    float: left;
}

.sm-container {
    width: 250px;
    float: right;
}
1234567891011121314151617181920212223
```

新建front_index.css如下：

```css
.index-banner {
    border-radius: 10px;
    overflow: hidden;
    height: 200px;
}

.index-banner img {
    height: 200px;
}

.post-group {
    border: 1px solid #ddd;
    margin-top: 20px;
    overflow: hidden;
    border-radius: 5px;
    padding: 10px;
}

.post-group-head {
    overflow: hidden;
    list-style: none;
}

.post-group-head li {
    float: left;
    padding: 5px 10px;
}

.post-group-head li a {
    color: #333;
}

.post-group-head li.active {
    background: #ccc;
}

.post-list-group {
    margin-top: 20px;
}

.post-list-group li {
    overflow: hidden;
    padding-bottom: 20px;
}

.author-avatar-group {
    float: left;
}

.author-avatar-group img {
    width: 50px;
    height: 50px;
    border-radius: 50%;
}

.post-info-group {
    float: left;
    margin-left: 10px;
    border-bottom: 1px solid #e6e6e6;
    width: 85%;
    padding-bottom: 10px;
}

.post-info-group .post-info {
    margin-top: 10px;
    font-size: 12px;
    color: #8c8c8c;
}

.post-info span {
    margin-right: 10px;
}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172
```

templates/front/front_index.html修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='front/css/front_base.css') }}">
    <link rel="stylesheet" href="{{ url_for('static', filename='front/css/front_index.css') }}">
    <title>首页</title>
</head>
<body>
<nav class="navbar navbar-default">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                    data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">Python栏目</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
                <li class="active"><a href="/">首页 <span class="sr-only">(current)</span></a></li>
            </ul>
            <form class="navbar-form navbar-left">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">搜索</button>
            </form>
            <ul class="nav navbar-nav navbar-right">
                <li><a href="{{ url_for('front.signin') }}">登录</a></li>
                <li><a href="{{ url_for('front.signup') }}">注册</a></li>
                {#                <li class="dropdown">#}
                {#                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"#}
                {#                       aria-expanded="false">Dropdown <span class="caret"></span></a>#}
                {#                    <ul class="dropdown-menu">#}
                {#                        <li><a href="#">Action</a></li>#}
                {#                        <li><a href="#">Another action</a></li>#}
                {#                        <li><a href="#">Something else here</a></li>#}
                {#                        <li role="separator" class="divider"></li>#}
                {#                        <li><a href="#">Separated link</a></li>#}
                {#                    </ul>#}
                {#                </li>#}
            </ul>
        </div><!-- /.navbar-collapse -->
    </div><!-- /.container-fluid -->
</nav>

<div class="main-container">
    <div class="cl-container">
        <div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
            <!-- Indicators 指令 -->
            <ol class="carousel-indicators">
                <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
                <li data-target="#carousel-example-generic" data-slide-to="1"></li>
                <li data-target="#carousel-example-generic" data-slide-to="2"></li>
            </ol>

            <!-- Wrapper for slides 轮播图 -->
            <div class="carousel-inner" role="listbox">
                <div class="item active">
                    <img src="{{ url_for('static', filename='front/images/python-1.jpg') }}" alt="...">
                    <div class="carousel-caption">
                        ...
                    </div>
                </div>
                <div class="item">
                    <img src="{{ url_for('static', filename='front/images/python-2.jpg') }}" alt="...">
                    <div class="carousel-caption">
                        ...
                    </div>
                </div>
                <div class="item">
                    <img src="{{ url_for('static', filename='front/images/python-3.jpg') }}" alt="...">
                    <div class="carousel-caption">
                        ...
                    </div>
                </div>
                ...
            </div>

            <!-- Controls 左右切换控制 -->
            <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
                <span class="sr-only">Previous</span>
            </a>
            <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
                <span class="sr-only">Next</span>
            </a>
        </div>
    </div>

    <div class="sm-container">

    </div>
</div>

</body>
</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108
```

此时访问首页如下：
![flsk front index slides css rendered](https://img-blog.csdnimg.cn/20200627202346353.gif#pic_center)

显然，轮播图**居中**，格式也更美观。

这里要注意：
用于轮播的个图片必须**像素大小一致**，否则各个图片在播放时会大小不一致，影响美观。

因为还会有更多页面，所以可以进行**模板继承**。

templates/front下新建父模板front_base.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='front/css/front_base.css') }}">
    <link rel="stylesheet" href="{{ url_for('static', filename='front/css/front_index.css') }}">
    <title>{% block title %}{% endblock %}</title>
    <title>{% block head %}{% endblock %}</title>
</head>
<body>
<nav class="navbar navbar-default">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                    data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">Python栏目</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
                <li class="active"><a href="/">首页 <span class="sr-only">(current)</span></a></li>
            </ul>
            <form class="navbar-form navbar-left">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">搜索</button>
            </form>
            <ul class="nav navbar-nav navbar-right">
                <li><a href="{{ url_for('front.signin') }}">登录</a></li>
                <li><a href="{{ url_for('front.signup') }}">注册</a></li>
                {#                <li class="dropdown">#}
                {#                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"#}
                {#                       aria-expanded="false">Dropdown <span class="caret"></span></a>#}
                {#                    <ul class="dropdown-menu">#}
                {#                        <li><a href="#">Action</a></li>#}
                {#                        <li><a href="#">Another action</a></li>#}
                {#                        <li><a href="#">Something else here</a></li>#}
                {#                        <li role="separator" class="divider"></li>#}
                {#                        <li><a href="#">Separated link</a></li>#}
                {#                    </ul>#}
                {#                </li>#}
            </ul>
        </div><!-- /.navbar-collapse -->
    </div><!-- /.container-fluid -->
</nav>

{% block main_content %}

{% endblock %}


</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364
```

front_index.html修改如下：

```html
{% extends 'front/front_base.html' %}

{% block title %}首页{% endblock %}

{% block main_content %}
    <div class="main-container">
        <div class="cl-container">
            <div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
                <!-- Indicators 指令 -->
                <ol class="carousel-indicators">
                    <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
                    <li data-target="#carousel-example-generic" data-slide-to="1"></li>
                    <li data-target="#carousel-example-generic" data-slide-to="2"></li>
                </ol>

                <!-- Wrapper for slides 轮播图 -->
                <div class="carousel-inner" role="listbox">
                    <div class="item active">
                        <img src="{{ url_for('static', filename='front/images/python-1.jpg') }}" alt="...">
                        <div class="carousel-caption">
                            ...
                        </div>
                    </div>
                    <div class="item">
                        <img src="{{ url_for('static', filename='front/images/python-2.jpg') }}" alt="...">
                        <div class="carousel-caption">
                            ...
                        </div>
                    </div>
                    <div class="item">
                        <img src="{{ url_for('static', filename='front/images/python-3.jpg') }}" alt="...">
                        <div class="carousel-caption">
                            ...
                        </div>
                    </div>
                    ...
                </div>

                <!-- Controls 左右切换控制 -->
                <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                    <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
                    <span class="sr-only">Previous</span>
                </a>
                <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                    <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
                    <span class="sr-only">Next</span>
                </a>
            </div>
        </div>

        <div class="sm-container">

        </div>
    </div>
{% endblock %}
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455
```

## 二、CMS轮播图管理页面

先新增**管理轮播图权限**（在实际开发中一般是先需求分析确定有哪些权限，一次性添加），设定只有运营或者权限更高的角色才能使用该权限，要改动的地方较多（所以初始的需求分析还是很重要的，否则一旦有小的变动则牵一发而动全身啊），cms_base.html修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>
        {% block title %}

        {% endblock %}
    </title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='cms/css/cms_base.css') }}">
    <script src="{{ url_for('static', filename='cms/js/cms_base.js') }}"></script>
    {# 提示框资源文件 #}
    <script src="{{ url_for('static', filename='common/sweetalert/sweetalert.min.js') }}"></script>
    <script src="{{ url_for('static', filename='common/sweetalert/clalert.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='common/sweetalert/sweetalert.css') }}">
    {% block head %}
    {% endblock %}
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
    <div class="container-fluid">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">熊熊乐园论坛CMS管理系统</a>
        </div>
        <div id="navbar" class="navbar-collapse collapse">
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">{{ g.cms_user.username }}<span>[{% block role %}{% endblock %}]</span></a></li>
                <li><a href="{{ url_for('cms.logout') }}">注销</a></li>
            </ul>
            <form class="navbar-form navbar-right">
                <input type="text" class="form-control" placeholder="查找...">
            </form>
        </div>
    </div>
</nav>

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3 col-md-2 sidebar">
            <ul class="nav-sidebar">
                <li class="unfold"><a href="{{ url_for('cms.index') }}">首页</a></li>
                <li class="profile-li">
                    <a href="#">个人中心<span></span></a>
                    <ul class="subnav">
                        <li><a href="{{ url_for('cms.profile') }}">个人信息</a></li>
                        <li><a href="{{ url_for('cms.resetpwd') }}">修改密码</a></li>
                        <li><a href="{{ url_for('cms.resetemail') }}">修改邮箱</a></li>
                    </ul>
                </li>

                {% set user = g.cms_user %}
                {% if user.has_permission(CMSPermission.POSTER) %}
                    <li class="nav-group post-manage"><a href="{{ url_for('cms.posts') }}">帖子管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.BANNER) %}
                    <li class="banner-manage"><a href="{{ url_for('cms.banners') }}">轮播图管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.COMMENTER) %}
                    <li class="comments-manage"><a href="{{ url_for('cms.comments') }}">评论管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.BOARDER) %}
                    <li class="board-manage"><a href="{{ url_for('cms.boards') }}">板块管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.FRONTUSER) %}
                    <li class="nav-group user-manage"><a href="{{ url_for('cms.fusers') }}">前台用户管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.CMSUSER) %}
                    <li class="nav-group cmsuser-manage"><a href="{{ url_for('cms.cusers') }}">CMS用户管理</a></li>
                {% endif %}
                {% if user.is_developer %}
                    <li class="cmsrole-manage"><a href="{{ url_for('cms.croles') }}">CMS组管理</a></li>
                {% endif %}


            </ul>
        </div>
        <div class="col-sm-9 col-sm-offset-3 col-md-10 col-md-offset-2 main">
            <h1>{% block page_title %} {% endblock %}</h1>
            <div class="main_content">
                {% block content %} {% endblock %}
            </div>
        </div>
    </div>
</div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879808182838485868788899091929394959697
```

cms_base.js修改如下：

```js
$(function () {
    $('.nav-sidebar>li>a').click(function (event) {
        var that = $(this);
        if (that.children('a').attr('href') == '#') {
            event.preventDefault();
        }
        if (that.parent().hasClass('unfold')) {
            that.parent().removeClass('unfold');
        } else {
            that.parent().addClass('unfold').siblings().removeClass('unfold');
        }
        console.log('coming....');
    });

    $('.nav-sidebar a').mouseleave(function () {
        $(this).css('text-decoration', 'none');
    });
});


$(function () {
    var url = window.location.href;
    if (url.indexOf('profile') >= 0) {
        var profileLi = $('.profile-li');
        profileLi.addClass('unfold').siblings().removeClass('unfold');
        profileLi.children('.subnav').children().eq(0).addClass('active').siblings().removeClass('active');
    } else if (url.indexOf('resetpwd') >= 0) {
        var profileLi = $('.profile-li');
        profileLi.addClass('unfold').siblings().removeClass('unfold');
        profileLi.children('.subnav').children().eq(1).addClass('active').siblings().removeClass('active');
    } else if (url.indexOf('resetemail') >= 0) {
        var profileLi = $('.profile-li');
        profileLi.addClass('unfold').siblings().removeClass('unfold');
        profileLi.children('.subnav').children().eq(2).addClass('active').siblings().removeClass('active');
    } else if (url.indexOf('posts') >= 0) {
        var postManageLi = $('.post-manage');
        postManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('banners') >= 0) {
        var postManageLi = $('.banner-manage');
        postManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('boards') >= 0) {
        var boardManageLi = $('.board-manage');
        boardManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('permissions') >= 0) {
        var permissionManageLi = $('.permission-manage');
        permissionManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('froles') >= 0) {
        var roleManageLi = $('.role-manage');
        roleManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('fusers') >= 0) {
        var userManageLi = $('.user-manage');
        userManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('cusers') >= 0) {
        var cmsuserManageLi = $('.cmsuser-manage');
        cmsuserManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('croles') >= 0) {
        var cmsroleManageLi = $('.cmsrole-manage');
        cmsroleManageLi.addClass('unfold').siblings().removeClass('unfold');
    } else if (url.indexOf('comments') >= 0) {
        var commentsManageLi = $('.comments-manage');
        commentsManageLi.addClass('unfold').siblings().removeClass('unfold');
    }
});
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263
```

apps/cms/models.py修改如下：

```python
from datetime import datetime
from werkzeug.security import generate_password_hash, check_password_hash
from exts import db


class CMSUser(db.Model):
    '''后台管理员用户类'''
    __tablename__ = 'cms_user'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    username = db.Column(db.String(30), nullable=False)
    _password = db.Column(db.String(100), nullable=False)
    email = db.Column(db.String(50), nullable=False, unique=True)
    join_time = db.Column(db.DateTime, default=datetime.now())

    def __init__(self, username, password, email):
        self.username = username
        self.password = password
        self.email = email

    @property
    def password(self):
        '''获取密码'''
        return self._password

    @password.setter
    def password(self, raw_password):
        '''设置密码'''
        self._password = generate_password_hash(raw_password)

    def check_password(self, raw_password):
        '''验证密码是否正确'''
        result = check_password_hash(self.password, raw_password)
        return result

    @property
    def permissions(self):
        '''判断用户权限'''
        if not self.roles:
            return 0

        all_permissions = 0
        for role in self.roles:
            permissions = role.permissions
            all_permissions |= permissions
        return all_permissions

    def has_permission(self, permission):
        '''判断当前用户是否有某个权限'''
        all_permissions = self.permissions
        result = all_permissions & permission == permission  # 如果当前用户有某个权限，则他的全部权限与该权限按位与的结果等于该权限本身
        return result

    @property
    def is_developer(self):
        '''判断当前用户是否是开发人员'''
        return self.has_permission(CMSPermission.ALL_PERMISSION)


class CMSPermission(object):
    # 255二进制表示所有权限
    ALL_PERMISSION = 0b11111111

    # 访问权限
    VISITOR = 0b00000001

    # 管理帖子权限
    POSTER = 0b00000010

    # 管理轮播图
    BANNER = 0b00000100

    # 管理评论权限
    COMMENTER = 0b00001000

    # 管理板块
    BOARDER = 0b00010000

    # 管理前台用户
    FRONTUSER = 0b00100000

    # 管理前台用户
    CMSUSER = 0b01000000

    # 管理前台用户
    ADMINER = 0b10000000


cms_role_user = db.Table(
    'cms_role_user',
    db.Column('cms_role_id', db.Integer, db.ForeignKey('cms_role.id'), primary_key=True),
    db.Column('cms_user_id', db.Integer, db.ForeignKey('cms_user.id'), primary_key=True)
)


class CMSRole(db.Model):
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(50), nullable=False)
    desc = db.Column(db.String(200), nullable=False)
    create_time = db.Column(db.DateTime, default=datetime.now())
    permissions = db.Column(db.Integer, default=CMSPermission.VISITOR)

    users = db.relationship('CMSUser', secondary=cms_role_user, backref='roles')

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103
```

manage.py修改如下：

```python
from flask_script import Manager
from bbs import app
from flask_migrate import Migrate, MigrateCommand
from exts import db
from apps.cms.models import CMSUser, CMSRole, CMSPermission
from apps.front.models import FrontUser

manager = Manager(app)
Migrate(app, db)

manager.add_command('db', MigrateCommand)


@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
@manager.option('-e', '--email', dest='email')
def create_cms_user(username, password, email):
    user = CMSUser(username=username, password=password, email=email)
    db.session.add(user)
    db.session.commit()
    print('CMS用户添加成功')


@manager.command
def create_role():
    # 访问者
    visitor = CMSRole(name='访问者', desc='只能查看数据，不能修改数据')
    visitor.permissions = CMSPermission.VISITOR  # 也可以省去，因为默认权限就是VISITOR

    # 运营者
    operator = CMSRole(name='运营', desc='管理帖子，管理评论，管理前台和后台用户，管理轮播图')
    # 有多个权限时，使用或运算表示
    operator.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER | CMSPermission.BANNER
    # 管理员
    admin = CMSRole(name='管理员', desc='拥有本系统大部分权限')
    admin.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER | CMSPermission.BOARDER | CMSPermission.BANNER

    # 开发人员
    developer = CMSRole(name='开发者', desc='拥有所有权限')
    developer.permissions = CMSPermission.ALL_PERMISSION

    db.session.add_all([visitor, operator, admin, developer])
    db.session.commit()
    print('角色添加成功')


@manager.command
def test_permission():
    user = CMSUser.query.first()
    if user.has_permission(CMSPermission.VISITOR):
        print('该用户有使用者权限')
    else:
        print('该用户没有使用者权限')


@manager.option('-e', '--email', dest='email')
@manager.option('-n', '--name', dest='name')
def add_user_to_role(email, name):
    user = CMSUser.query.filter_by(email=email).first()
    if user:
        role = CMSRole.query.filter_by(name=name).first()
        if role:
            role.users.append(user)
            db.session.commit()
            print('用户添加到角色添加成功')
        else:
            print('角色不存在')
    else:
        print('邮箱不存在')


@manager.option('-t', '--telephone', dest='telephone')
@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
def create_front_user(telephone, username, password):
    user = FrontUser(telephone=telephone, username=username, password=password)
    db.session.add(user)
    db.session.commit()
    print('前台用户添加成功')


if __name__ == '__main__':
    manager.run()

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384
```

apps/cms/views.py修改如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message

from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm
from apps.cms.models import CMSUser, CMSPermission
from exts import db, mail
from utils import restful, random_captcha, clcache
from .decorators import permission_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request

@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html', max_role=g.max_role)


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html", max_role=g.max_role)


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html', max_role=g.max_role)

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html', max_role=g.max_role)

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            email = form.email.data
            # 修改用户邮箱
            g.cms_user.email = email
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


@cms_bp.route('/posts/')
@permission_required(CMSPermission.POSTER)
def posts():
    return render_template('cms/cms_posts.html', max_role=g.max_role)


@cms_bp.route('/banners/')
@permission_required(CMSPermission.BANNER)
def banners():
    return render_template('cms/cms_banners.html', max_role=g.max_role)


@cms_bp.route('/comments/')
@permission_required(CMSPermission.COMMENTER)
def comments():
    return render_template('cms/cms_comments.html', max_role=g.max_role)


@cms_bp.route('/boards/')
@permission_required(CMSPermission.BOARDER)
def boards():
    return render_template('cms/cms_boards.html', max_role=g.max_role)


@cms_bp.route('/fusers/')
@permission_required(CMSPermission.FRONTUSER)
def fusers():
    return render_template('cms/cms_fusers.html', max_role=g.max_role)


@cms_bp.route('/cusers/')
@permission_required(CMSPermission.CMSUSER)
def cusers():
    return render_template('cms/cms_cusers.html', max_role=g.max_role)


@cms_bp.route('/croles/')
@permission_required(CMSPermission.ADMINER)
def croles():
    return render_template('cms/cms_croles.html', max_role=g.max_role)


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164
```

templates/cms下新建cms_banners.html如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台轮播图管理
{% endblock %}

{% block page_title %}
    熊熊轮播图管理
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    轮播图管理部分
{% endblock %}
1234567891011121314151617
```

此时需要重新进行**数据映射**，需要先在manage.py中注释掉cms_role_user中间表定义的相关部分，映射后再取消注释再次映射，重新构造数据用于测试，如果中间表映射失败可能是两张表（cms_user和cms_role）的**存储引擎**不对，修改为**InnoDB**重启数据库后再进行映射。

效果如下：
![flsk cms add banner function](https://img-blog.csdnimg.cn/20200627202450977.gif#pic_center)

显然，已将其添加到内容管理模块。

现进一步完善轮播图管理页面，static/cms/js下新建banners.js如下：

```js
$(function () {
    $("#save-banner-btn").click(function (event) {
        event.preventDefault();
        var self = $(this);
        var dialog = $("#banner-dialog");
        var nameInput = $("input[name='name']");
        var imageInput = $("input[name='image_url']");
        var linkInput = $("input[name='link_url']");
        var priorityInput = $("input[name='priority']");


        var name = nameInput.val();
        var image_url = imageInput.val();
        var link_url = linkInput.val();
        var priority = priorityInput.val();
        var submitType = self.attr('data-type');
        var bannerId = self.attr("data-id");

        if (!name || !image_url || !link_url || !priority) {
            clalert.alertInfoToast('请输入完整的轮播图数据！');
            return;
        }

        var url = '';
        if (submitType === 'update') {
            url = '/cms/ubanner/';
        } else {
            url = '/cms/abanner/';
        }

        clajax.post({
            "url": url,
            'data': {
                'name': name,
                'image_url': image_url,
                'link_url': link_url,
                'priority': priority,
                'banner_id': bannerId
            },
            'success': function (data) {
                dialog.modal("hide");
                if (data['code'] === 200) {
                    // 重新加载这个页面
                    window.location.reload();
                } else {
                    clalert.alertInfo(data['message']);
                }
            },
            'fail': function () {
                clalert.alertNetworkError();
            }
        });
    });
});

$(function () {
    $(".edit-banner-btn").click(function (event) {
        var self = $(this);
        var dialog = $("#banner-dialog");
        dialog.modal("show");

        var tr = self.parent().parent();
        var name = tr.attr("data-name");
        var image_url = tr.attr("data-image");
        var link_url = tr.attr("data-link");
        var priority = tr.attr("data-priority");

        var nameInput = dialog.find("input[name='name']");
        var imageInput = dialog.find("input[name='image_url']");
        var linkInput = dialog.find("input[name='link_url']");
        var priorityInput = dialog.find("input[name='priority']");
        var saveBtn = dialog.find("#save-banner-btn");

        nameInput.val(name);
        imageInput.val(image_url);
        linkInput.val(link_url);
        priorityInput.val(priority);
        saveBtn.attr("data-type", 'update');
        saveBtn.attr('data-id', tr.attr('data-id'));
    });
});

$(function () {
    $(".delete-banner-btn").click(function (event) {
        var self = $(this);
        var tr = self.parent().parent();
        var banner_id = tr.attr('data-id');
        clalert.alertConfirm({
            "msg": "您确定要删除这个轮播图吗？",
            'confirmCallback': function () {
                clajax.post({
                    'url': '/cms/dbanner/',
                    'data': {
                        'banner_id': banner_id
                    },
                    'success': function (data) {
                        if (data['code'] === 200) {
                            window.location.reload();
                        } else {
                            clalert.alertInfo(data['message']);
                        }
                    }
                })
            }
        });
    });
});

$(function () {
    clqiniu.setUp({
        'domain': 'http://7xqenu.com1.z0.glb.clouddn.com/',
        'browse_btn': 'upload-btn',
        'uptoken_url': '/c/uptoken/',
        'success': function (up, file, info) {
            var imageInput = $("input[name='image_url']");
            imageInput.val(file.name);
        }
    });
});
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119
```

static/common下新建clqiniu.js如下：

```js
//'use strict';

var clqiniu = {
    'setUp': function (args) {
        var domain = args['domain'];
        var params = {
            browse_button: args['browse_btn'],
            runtimes: 'html5,flash,html4', //上传模式，依次退化
            max_file_size: '500mb', //文件最大允许的尺寸
            dragdrop: false, //是否开启拖拽上传
            chunk_size: '4mb', //分块上传时，每片的大小
            uptoken_url: args['uptoken_url'], //ajax请求token的url
            domain: domain, //图片下载时候的域名
            get_new_uptoken: false, //是否每次上传文件都要从业务服务器获取token
            auto_start: true, //如果设置了true,只要选择了图片,就会自动上传
            unique_names: true,
            multi_selection: false,
            filters: {
                mime_types: [
                    {title: 'Image files', extensions: 'jpg,gif,png'},
                    {title: 'Video files', extensions: 'flv,mpg,mpeg,avi,wmv,mov,asf,rm,rmvb,mkv,m4v,mp4'}
                ]
            },
            log_level: 5, //log级别
            init: {
                'FileUploaded': function (up, file, info) {
                    if (args['success']) {
                        var success = args['success'];
                        file.name = domain + file.target_name;
                        success(up, file, info);
                    }
                },
                'Error': function (up, err, errTip) {
                    if (args['error']) {
                        var error = args['error'];
                        error(up, err, errTip);
                    }
                },
                'UploadProgress': function (up, file) {
                    if (args['progress']) {
                        args['progress'](up, file);
                    }
                },
                'FilesAdded': function (up, files) {
                    if (args['fileadded']) {
                        args['fileadded'](up, files);
                    }
                },
                'UploadComplete': function () {
                    if (args['complete']) {
                        args['complete']();
                    }
                }
            }
        };

        // 把args中的参数放到params中去
        for (var key in args) {
            params[key] = args[key];
        }
        var uploader = Qiniu.uploader(params);
        return uploader;
    }
};

1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465
```

cms_banners.html完善如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台轮播图管理
{% endblock %}

{% block head %}
    <script src="https://cdn.staticfile.org/Plupload/2.1.1/moxie.js"></script>
    <script src="https://cdn.staticfile.org/Plupload/2.1.1/plupload.dev.js"></script>
    <script src="https://cdn.staticfile.org/qiniu-js-sdk/1.0.14-beta/qiniu.js"></script>
    <script src="{{ url_for('static', filename='common/clqiniu.js') }}"></script>
    <script src="{{ url_for('static',filename='cms/js/banners.js') }}"></script>
    <style>
        .top-box button {
            float: right;
        }
    </style>
{% endblock %}

{% block page_title %}
    熊熊轮播图管理
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    <div class="top-box">
        <button class="btn btn-warning" data-toggle="modal" data-target="#banner-dialog">添加轮播图</button>
    </div>
    <table class="table table-bordered">
        <thead>
        <tr>
            <th>名称</th>
            <th>图片链接</th>
            <th>跳转链接</th>
            <th>优先级</th>
            <th>创建时间</th>
            <th>操作</th>
        </tr>
        </thead>
        <tbody>
        <tr data-name="" data-image="" data-link=""
            data-priority="" data-id="">
            <td>图片名字</td>
            <td><a href="" target="_blank">www.baidu.com</a></td>
            <td><a href="" target="_blank">www.baidu.com</a></td>
            <td>1</td>
            <td>2020-05-20</td>
            <td>
                <button class="btn btn-default btn-xs edit-banner-btn">编辑</button>
                <button class="btn btn-danger btn-xs delete-banner-btn">删除</button>
            </td>
        </tr>

        </tbody>
    </table>

    <!-- Modal -->
    <div class="modal fade" id="banner-dialog" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span
                            aria-hidden="true">&times;</span>
                    </button>
                    <h4 class="modal-title" id="myModalLabel">轮播图</h4>
                </div>
                <div class="modal-body">
                    <form action="" class="form-horizontal">
                        <div class="form-group">
                            <label class="col-sm-2 control-label">名称：</label>
                            <div class="col-sm-10">
                                <input type="text" class="form-control" name="name" placeholder="轮播图名称">
                            </div>
                        </div>
                        <div class="form-group">
                            <label class="col-sm-2 control-label">图片：</label>
                            <div class="col-sm-7">
                                <input type="text" class="form-control" name="image_url"
                                       placeholder="轮播图图片">
                            </div>
                            <button class="btn btn-info col-sm-2" id="upload-btn">添加图片</button>
                        </div>
                        <div class="form-group">
                            <label class="col-sm-2 control-label">跳转：</label>
                            <div class="col-sm-10">
                                <input type="text" class="form-control" name="link_url" placeholder="跳转链接">
                            </div>
                        </div>
                        <div class="form-group">
                            <label class="col-sm-2 control-label">权重：</label>
                            <div class="col-sm-10">
                                <input type="number" class="form-control" name="priority" placeholder="优先级">
                            </div>
                        </div>
                    </form>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default" data-dismiss="modal">关闭</button>
                    <button type="button" class="btn btn-primary" id="save-banner-btn">保存</button>
                </div>
            </div>
        </div>
    </div>
{% endblock %}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107
```

进行测试：
![flsk cms add banner perfect](https://img-blog.csdnimg.cn/20200627202541390.gif#pic_center)

显然，此时已经具有轮播图管理的基本雏形。

## 三、CMS添加轮播图

在添加轮播图时，需要上传图片，有两种方式：

- 直接输入图片链接
- 上传本地图片
  上传本地图片不是直接上传到服务器，而是上传到第三方平台，如七牛云，主要是从三方面考虑：**减少服务器资源占用**，降低成本；利于**CDN加速**，更快访问；**安全性**的考虑，减少文件上传带来的威胁。

新建轮播图也需要保存数据，即需要**创建模型**，apps/cms/models.py修改如下：

```python
from datetime import datetime
from werkzeug.security import generate_password_hash, check_password_hash
from exts import db


class CMSUser(db.Model):
    '''后台管理员用户类'''
    __tablename__ = 'cms_user'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    username = db.Column(db.String(30), nullable=False)
    _password = db.Column(db.String(100), nullable=False)
    email = db.Column(db.String(50), nullable=False, unique=True)
    join_time = db.Column(db.DateTime, default=datetime.now())

    def __init__(self, username, password, email):
        self.username = username
        self.password = password
        self.email = email

    @property
    def password(self):
        '''获取密码'''
        return self._password

    @password.setter
    def password(self, raw_password):
        '''设置密码'''
        self._password = generate_password_hash(raw_password)

    def check_password(self, raw_password):
        '''验证密码是否正确'''
        result = check_password_hash(self.password, raw_password)
        return result

    @property
    def permissions(self):
        '''判断用户权限'''
        if not self.roles:
            return 0

        all_permissions = 0
        for role in self.roles:
            permissions = role.permissions
            all_permissions |= permissions
        return all_permissions

    def has_permission(self, permission):
        '''判断当前用户是否有某个权限'''
        all_permissions = self.permissions
        result = all_permissions & permission == permission  # 如果当前用户有某个权限，则他的全部权限与该权限按位与的结果等于该权限本身
        return result

    @property
    def is_developer(self):
        '''判断当前用户是否是开发人员'''
        return self.has_permission(CMSPermission.ALL_PERMISSION)


class CMSPermission(object):
    # 255二进制表示所有权限
    ALL_PERMISSION = 0b11111111

    # 访问权限
    VISITOR = 0b00000001

    # 管理帖子权限
    POSTER = 0b00000010

    # 管理轮播图
    BANNER = 0b00000100

    # 管理评论权限
    COMMENTER = 0b00001000

    # 管理板块
    BOARDER = 0b00010000

    # 管理前台用户
    FRONTUSER = 0b00100000

    # 管理前台用户
    CMSUSER = 0b01000000

    # 管理前台用户
    ADMINER = 0b10000000


cms_role_user = db.Table(
    'cms_role_user',
    db.Column('cms_role_id', db.Integer, db.ForeignKey('cms_role.id'), primary_key=True),
    db.Column('cms_user_id', db.Integer, db.ForeignKey('cms_user.id'), primary_key=True)
)


class CMSRole(db.Model):
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(50), nullable=False)
    desc = db.Column(db.String(200), nullable=False)
    create_time = db.Column(db.DateTime, default=datetime.now())
    permissions = db.Column(db.Integer, default=CMSPermission.VISITOR)

    users = db.relationship('CMSUser', secondary=cms_role_user, backref='roles')


class BannerModel(db.Model):
    __tablename__ = 'banner'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(30), nullable=False)
    # 图片链接
    image_url = db.Column(db.String(100), nullable=False)
    # 跳转链接
    link_url = db.Column(db.String(100), nullable=False)
    # 优先级
    priority = db.Column(db.Integer, default=0)
    create_time = db.Column(db.DateTime, default=datetime.now)
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115
```

manage.py如下：

```python
from flask_script import Manager
from bbs import app
from flask_migrate import Migrate, MigrateCommand
from exts import db
from apps.cms.models import CMSUser, CMSRole, CMSPermission, BannerModel
from apps.front.models import FrontUser

manager = Manager(app)
Migrate(app, db)

manager.add_command('db', MigrateCommand)


@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
@manager.option('-e', '--email', dest='email')
def create_cms_user(username, password, email):
    user = CMSUser(username=username, password=password, email=email)
    db.session.add(user)
    db.session.commit()
    print('CMS用户添加成功')


@manager.command
def create_role():
    # 访问者
    visitor = CMSRole(name='访问者', desc='只能查看数据，不能修改数据')
    visitor.permissions = CMSPermission.VISITOR  # 也可以省去，因为默认权限就是VISITOR

    # 运营者
    operator = CMSRole(name='运营', desc='管理帖子，管理评论，管理前台和后台用户，管理轮播图')
    # 有多个权限时，使用或运算表示
    operator.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER | CMSPermission.BANNER
    # 管理员
    admin = CMSRole(name='管理员', desc='拥有本系统大部分权限')
    admin.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER | CMSPermission.BOARDER | CMSPermission.BANNER

    # 开发人员
    developer = CMSRole(name='开发者', desc='拥有所有权限')
    developer.permissions = CMSPermission.ALL_PERMISSION

    db.session.add_all([visitor, operator, admin, developer])
    db.session.commit()
    print('角色添加成功')


@manager.command
def test_permission():
    user = CMSUser.query.first()
    if user.has_permission(CMSPermission.VISITOR):
        print('该用户有使用者权限')
    else:
        print('该用户没有使用者权限')


@manager.option('-e', '--email', dest='email')
@manager.option('-n', '--name', dest='name')
def add_user_to_role(email, name):
    user = CMSUser.query.filter_by(email=email).first()
    if user:
        role = CMSRole.query.filter_by(name=name).first()
        if role:
            role.users.append(user)
            db.session.commit()
            print('用户添加到角色添加成功')
        else:
            print('角色不存在')
    else:
        print('邮箱不存在')


@manager.option('-t', '--telephone', dest='telephone')
@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
def create_front_user(telephone, username, password):
    user = FrontUser(telephone=telephone, username=username, password=password)
    db.session.add(user)
    db.session.commit()
    print('前台用户添加成功')


if __name__ == '__main__':
    manager.run()

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384
```

然后执行`python manage.py db migrate`和`python manage.py db upgrade`命令将表映射到数据库中。

apps/cms/forms.py下新增表单验证类如下：

```python
from wtforms import Form, StringField, IntegerField, ValidationError
from wtforms.validators import Email, InputRequired, Length, EqualTo, URL
from utils import clcache


class BaseForm(Form):
    def get_error(self):
        message = self.errors.popitem()[1][0]
        return message


class LoginForm(BaseForm):
    email = StringField(validators=[Email(message='请输入正确的邮箱地址'), InputRequired(message='请输入邮箱')])
    password = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    remember = IntegerField()


class ResetPwdForm(BaseForm):
    oldpwd = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    newpwd = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    newpwd2 = StringField(validators=[EqualTo("newpwd", message='两次输入密码不一致')])


class ResetEmailForm(BaseForm):
    email = StringField(validators=[Email(message='您的邮箱格式有误，请重新输入')])
    captcha = StringField(validators=[Length(4, 4, message='请输入4位验证码')])

    def validate_captcha(self, form):
        captcha = self.captcha.data
        email = self.email.data
        redis_captcha = clcache.get_captcha(email)
        if not redis_captcha or captcha.lower() != redis_captcha.lower():
            raise ValidationError('邮箱验证码错误')


class AddBannerForm(BaseForm):
    name = StringField(validators=[InputRequired(message='请输入轮播图名称')])
    image_url = StringField(validators=[InputRequired(message='请输入图片链接'), URL(message='请注意图片链接格式')])
    link_url = StringField(validators=[InputRequired(message='请输入跳转链接'), URL(message='请注意跳转链接格式')])
    priority = IntegerField(validators=[InputRequired(message='请输入轮播图优先级')])
12345678910111213141516171819202122232425262728293031323334353637383940
```

apps/cms/views.py修改如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message

from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm, AddBannerForm
from apps.cms.models import CMSUser, CMSPermission, BannerModel
from exts import db, mail
from utils import restful, random_captcha, clcache
from .decorators import permission_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request

@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html', max_role=g.max_role)


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html", max_role=g.max_role)


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html', max_role=g.max_role)

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html', max_role=g.max_role)

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            email = form.email.data
            # 修改用户邮箱
            g.cms_user.email = email
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


@cms_bp.route('/posts/')
@permission_required(CMSPermission.POSTER)
def posts():
    return render_template('cms/cms_posts.html', max_role=g.max_role)


@cms_bp.route('/banners/')
@permission_required(CMSPermission.BANNER)
def banners():
    return render_template('cms/cms_banners.html', max_role=g.max_role)


@cms_bp.route('/abanner/', methods=['POST'])
def abanner():
    form = AddBannerForm(request.form)
    if form.validate():
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel(name=name, image_url=image_url, link_url=link_url, priority=priority)
        db.session.add(banner)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message=form.get_error())


@cms_bp.route('/comments/')
@permission_required(CMSPermission.COMMENTER)
def comments():
    return render_template('cms/cms_comments.html', max_role=g.max_role)


@cms_bp.route('/boards/')
@permission_required(CMSPermission.BOARDER)
def boards():
    return render_template('cms/cms_boards.html', max_role=g.max_role)


@cms_bp.route('/fusers/')
@permission_required(CMSPermission.FRONTUSER)
def fusers():
    return render_template('cms/cms_fusers.html', max_role=g.max_role)


@cms_bp.route('/cusers/')
@permission_required(CMSPermission.CMSUSER)
def cusers():
    return render_template('cms/cms_cusers.html', max_role=g.max_role)


@cms_bp.route('/croles/')
@permission_required(CMSPermission.ADMINER)
def croles():
    return render_template('cms/cms_croles.html', max_role=g.max_role)


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180
```

banners.js修改如下：

```js
var clajax = {
    'get': function (args) {
        args['method'] = 'get';
        this.ajax(args);
    },
    'post': function (args) {
        args['method'] = 'post';
        this.ajax(args);
    },
    'ajax': function (args) {
        // 设置csrftoken
        this._ajaxSetup();
        $.ajax(args);
    },
    '_ajaxSetup': function () {
        $.ajaxSetup({
            'beforeSend': function (xhr, settings) {
                if (!/^(GET|HEAD|OPTIONS|TRACE)$/i.test(settings.type) && !this.crossDomain) {
                    var csrftoken = $('meta[name=csrf-token]').attr('content');
                    xhr.setRequestHeader("X-CSRFToken", csrftoken)
                }
            }
        });
    }
};

$(function () {
    $("#save-banner-btn").click(function (event) {
        event.preventDefault();
        var self = $(this);
        var dialog = $("#banner-dialog");
        var nameInput = $("input[name='name']");
        var imageInput = $("input[name='image_url']");
        var linkInput = $("input[name='link_url']");
        var priorityInput = $("input[name='priority']");


        var name = nameInput.val();
        var image_url = imageInput.val();
        var link_url = linkInput.val();
        var priority = priorityInput.val();
        var submitType = self.attr('data-type');
        var bannerId = self.attr("data-id");

        if (!name || !image_url || !link_url || !priority) {
            clalert.alertInfoToast('请输入完整的轮播图数据！');
            return;
        }

        var url = '';
        if (submitType === 'update') {
            url = '/cms/ubanner/';
        } else {
            url = '/cms/abanner/';
        }

        clajax.post({
            "url": url,
            'data': {
                'name': name,
                'image_url': image_url,
                'link_url': link_url,
                'priority': priority,
                'banner_id': bannerId
            },
            'success': function (data) {
                dialog.modal("hide");
                if (data['code'] === 200) {
                    // 重新加载这个页面
                    window.location.reload();
                } else {
                    clalert.alertInfo(data['message']);
                }
            },
            'fail': function () {
                clalert.alertNetworkError();
            }
        });
    });
});

$(function () {
    $(".edit-banner-btn").click(function (event) {
        var self = $(this);
        var dialog = $("#banner-dialog");
        dialog.modal("show");

        var tr = self.parent().parent();
        var name = tr.attr("data-name");
        var image_url = tr.attr("data-image");
        var link_url = tr.attr("data-link");
        var priority = tr.attr("data-priority");

        var nameInput = dialog.find("input[name='name']");
        var imageInput = dialog.find("input[name='image_url']");
        var linkInput = dialog.find("input[name='link_url']");
        var priorityInput = dialog.find("input[name='priority']");
        var saveBtn = dialog.find("#save-banner-btn");

        nameInput.val(name);
        imageInput.val(image_url);
        linkInput.val(link_url);
        priorityInput.val(priority);
        saveBtn.attr("data-type", 'update');
        saveBtn.attr('data-id', tr.attr('data-id'));
    });
});

$(function () {
    $(".delete-banner-btn").click(function (event) {
        var self = $(this);
        var tr = self.parent().parent();
        var banner_id = tr.attr('data-id');
        clalert.alertConfirm({
            "msg": "您确定要删除这个轮播图吗？",
            'confirmCallback': function () {
                clajax.post({
                    'url': '/cms/dbanner/',
                    'data': {
                        'banner_id': banner_id
                    },
                    'success': function (data) {
                        if (data['code'] === 200) {
                            window.location.reload();
                        } else {
                            clalert.alertInfo(data['message']);
                        }
                    }
                })
            }
        });
    });
});

$(function () {
    clqiniu.setUp({
        'domain': 'http://7xqenu.com1.z0.glb.clouddn.com/',
        'browse_btn': 'upload-btn',
        'uptoken_url': '/c/uptoken/',
        'success': function (up, file, info) {
            var imageInput = $("input[name='image_url']");
            imageInput.val(file.name);
        }
    });
});
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145
```

现进行增加轮播图测试：
![flsk cms add banner add test](https://img-blog.csdnimg.cn/20200627202634804.gif#pic_center)

再查询数据库：

```sql
select * from banner;
1
```

打印：

```sql
+----+-----------------------------+------------------------------------------------------------------------------------------+-------------------------------+----------+---------------------+
| id | name                        | image_url                                                                                | link_url                      | priority | create_time         |
+----+-----------------------------+------------------------------------------------------------------------------------------+-------------------------------+----------+---------------------+
|  1 | 人生苦短，我用Python        | http://dingyue.ws.126.net/M94eySrao1InTPhmXU5PBukIvRUkdjF0r9mTwJYPw06Uw1549881599393.jpg | https://blog.csdn.net/CUFEECR |        1 | 2020-06-27 13:57:34 |
+----+-----------------------------+------------------------------------------------------------------------------------------+-------------------------------+----------+---------------------+
1 row in set (0.01 sec)
                       
1234567
```

显然，已经将轮播图数据插入表中，但是页面上还未同步显示，需要进行修改。

views.py修改如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message

from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm, AddBannerForm
from apps.cms.models import CMSUser, CMSPermission, BannerModel
from exts import db, mail
from utils import restful, random_captcha, clcache
from .decorators import permission_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request

@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html', max_role=g.max_role)


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html", max_role=g.max_role)


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html', max_role=g.max_role)

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html', max_role=g.max_role)

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            email = form.email.data
            # 修改用户邮箱
            g.cms_user.email = email
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


@cms_bp.route('/posts/')
@permission_required(CMSPermission.POSTER)
def posts():
    return render_template('cms/cms_posts.html', max_role=g.max_role)


@cms_bp.route('/banners/')
@permission_required(CMSPermission.BANNER)
def banners():
    banners = BannerModel.query.order_by(BannerModel.priority.desc()).all()
    return render_template('cms/cms_banners.html', max_role=g.max_role, banners=banners)


@cms_bp.route('/abanner/', methods=['POST'])
def abanner():
    form = AddBannerForm(request.form)
    if form.validate():
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel(name=name, image_url=image_url, link_url=link_url, priority=priority)
        db.session.add(banner)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message=form.get_error())


@cms_bp.route('/comments/')
@permission_required(CMSPermission.COMMENTER)
def comments():
    return render_template('cms/cms_comments.html', max_role=g.max_role)


@cms_bp.route('/boards/')
@permission_required(CMSPermission.BOARDER)
def boards():
    return render_template('cms/cms_boards.html', max_role=g.max_role)


@cms_bp.route('/fusers/')
@permission_required(CMSPermission.FRONTUSER)
def fusers():
    return render_template('cms/cms_fusers.html', max_role=g.max_role)


@cms_bp.route('/cusers/')
@permission_required(CMSPermission.CMSUSER)
def cusers():
    return render_template('cms/cms_cusers.html', max_role=g.max_role)


@cms_bp.route('/croles/')
@permission_required(CMSPermission.ADMINER)
def croles():
    return render_template('cms/cms_croles.html', max_role=g.max_role)


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181
```

cms_banners.html如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台轮播图管理
{% endblock %}

{% block head %}
    <script src="https://cdn.staticfile.org/Plupload/2.1.1/moxie.js"></script>
    <script src="https://cdn.staticfile.org/Plupload/2.1.1/plupload.dev.js"></script>
    <script src="https://cdn.staticfile.org/qiniu-js-sdk/1.0.14-beta/qiniu.js"></script>
    <script src="{{ url_for('static', filename='common/clqiniu.js') }}"></script>
    <script src="{{ url_for('static',filename='cms/js/banners.js') }}"></script>
    <style>
        .top-box button {
            float: right;
        }
    </style>
{% endblock %}

{% block page_title %}
    熊熊轮播图管理
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    <div class="top-box">
        <button class="btn btn-warning" data-toggle="modal" data-target="#banner-dialog">添加轮播图</button>
    </div>
    <table class="table table-bordered">
        <thead>
        <tr>
            <th>名称</th>
            <th>图片链接</th>
            <th>跳转链接</th>
            <th>优先级</th>
            <th>创建时间</th>
            <th>操作</th>
        </tr>
        </thead>
        <tbody>
        {% for banner in banners %}
            <tr data-name="{{ banner.name }}" data-image="{{ banner.image_url }}" data-link="{{ banner.link_url }}"
            data-priority="{{ banner.priority }}" data-id="{{ banner.id }}">
            <td>{{ banner.name}}</td>
            <td><a href="{{ banner.image_url }}" target="_blank">{{ banner.image_url | truncate(45) }}</a></td>
            <td><a href="{{ banner.link_url }}" target="_blank">{{ banner.link_url | truncate(35) }}</a></td>
            <td>{{ banner.priority }}</td>
            <td>{{ banner.create_time }}</td>
            <td>
                <button class="btn btn-default btn-xs edit-banner-btn">编辑</button>
                <button class="btn btn-danger btn-xs delete-banner-btn">删除</button>
            </td>
        </tr>
        {% endfor %}



        </tbody>
    </table>

    <!-- Modal -->
    <div class="modal fade" id="banner-dialog" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span
                            aria-hidden="true">&times;</span>
                    </button>
                    <h4 class="modal-title" id="myModalLabel">轮播图</h4>
                </div>
                <div class="modal-body">
                    <form action="" class="form-horizontal">
                        <div class="form-group">
                            <label class="col-sm-2 control-label">名称：</label>
                            <div class="col-sm-10">
                                <input type="text" class="form-control" name="name" placeholder="轮播图名称">
                            </div>
                        </div>
                        <div class="form-group">
                            <label class="col-sm-2 control-label">图片：</label>
                            <div class="col-sm-7">
                                <input type="text" class="form-control" name="image_url"
                                       placeholder="轮播图图片">
                            </div>
                            <button class="btn btn-info col-sm-2" id="upload-btn">添加图片</button>
                        </div>
                        <div class="form-group">
                            <label class="col-sm-2 control-label">跳转：</label>
                            <div class="col-sm-10">
                                <input type="text" class="form-control" name="link_url" placeholder="跳转链接">
                            </div>
                        </div>
                        <div class="form-group">
                            <label class="col-sm-2 control-label">权重：</label>
                            <div class="col-sm-10">
                                <input type="number" class="form-control" name="priority" placeholder="优先级">
                            </div>
                        </div>
                    </form>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default" data-dismiss="modal">关闭</button>
                    <button type="button" class="btn btn-primary" id="save-banner-btn">保存</button>
                </div>
            </div>
        </div>
    </div>
{% endblock %}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111
```

显示：
![flsk cms banner data loaded](https://img-blog.csdnimg.cn/20200627202704625.gif#pic_center)

显然，此时已经显示出轮播图数据，并且在新增轮播图后会自动刷新出新的数据。

## 四、CMS轮播图编辑和删除

添加轮播图和编辑轮播图都是通过**JS**实现的，它们是通过button标签的data-type属性来区分的，以此来达到编辑和添加时用不同的url来发送数据的目的。

forms.py中增加对修改轮播图的验证如下：

```python
from wtforms import Form, StringField, IntegerField, ValidationError
from wtforms.validators import Email, InputRequired, Length, EqualTo, URL
from utils import clcache


class BaseForm(Form):
    def get_error(self):
        message = self.errors.popitem()[1][0]
        return message


class LoginForm(BaseForm):
    email = StringField(validators=[Email(message='请输入正确的邮箱地址'), InputRequired(message='请输入邮箱')])
    password = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    remember = IntegerField()


class ResetPwdForm(BaseForm):
    oldpwd = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    newpwd = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    newpwd2 = StringField(validators=[EqualTo("newpwd", message='两次输入密码不一致')])


class ResetEmailForm(BaseForm):
    email = StringField(validators=[Email(message='您的邮箱格式有误，请重新输入')])
    captcha = StringField(validators=[Length(4, 4, message='请输入4位验证码')])

    def validate_captcha(self, form):
        captcha = self.captcha.data
        email = self.email.data
        redis_captcha = clcache.get_captcha(email)
        if not redis_captcha or captcha.lower() != redis_captcha.lower():
            raise ValidationError('邮箱验证码错误')


class AddBannerForm(BaseForm):
    name = StringField(validators=[InputRequired(message='请输入轮播图名称')])
    image_url = StringField(validators=[InputRequired(message='请输入图片链接'), URL(message='请注意图片链接格式')])
    link_url = StringField(validators=[InputRequired(message='请输入跳转链接'), URL(message='请注意跳转链接格式')])
    priority = IntegerField(validators=[InputRequired(message='请输入轮播图优先级')])


class UpdateBannerForm(AddBannerForm):
    banner_id = IntegerField(validators=[InputRequired(message='轮播图不存在')])
1234567891011121314151617181920212223242526272829303132333435363738394041424344
```

views.py修改如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message

from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm, AddBannerForm, UpdateBannerForm
from apps.cms.models import CMSUser, CMSPermission, BannerModel
from exts import db, mail
from utils import restful, random_captcha, clcache
from .decorators import permission_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request

@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html', max_role=g.max_role)


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html", max_role=g.max_role)


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html', max_role=g.max_role)

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html', max_role=g.max_role)

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            email = form.email.data
            # 修改用户邮箱
            g.cms_user.email = email
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


@cms_bp.route('/posts/')
@permission_required(CMSPermission.POSTER)
def posts():
    return render_template('cms/cms_posts.html', max_role=g.max_role)


@cms_bp.route('/banners/')
@permission_required(CMSPermission.BANNER)
def banners():
    banners = BannerModel.query.order_by(BannerModel.priority.desc()).all()
    return render_template('cms/cms_banners.html', max_role=g.max_role, banners=banners)


@cms_bp.route('/abanner/', methods=['POST'])
def abanner():
    form = AddBannerForm(request.form)
    if form.validate():
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel(name=name, image_url=image_url, link_url=link_url, priority=priority)
        db.session.add(banner)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message=form.get_error())


@cms_bp.route('/ubanner/', methods=['POST'])
def ubanner():
    # 根据id修改数据
    form = UpdateBannerForm(request.form)
    if form.validate():
        banner_id = form.banner_id.data
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel.query.get(banner_id)
        if banner:
            banner.name = name
            banner.image_url = image_url
            banner.link_url = link_url
            banner.priority = priority
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='轮播图不存在')
    else:
        return restful.params_error(form.get_error())


@cms_bp.route('/comments/')
@permission_required(CMSPermission.COMMENTER)
def comments():
    return render_template('cms/cms_comments.html', max_role=g.max_role)


@cms_bp.route('/boards/')
@permission_required(CMSPermission.BOARDER)
def boards():
    return render_template('cms/cms_boards.html', max_role=g.max_role)


@cms_bp.route('/fusers/')
@permission_required(CMSPermission.FRONTUSER)
def fusers():
    return render_template('cms/cms_fusers.html', max_role=g.max_role)


@cms_bp.route('/cusers/')
@permission_required(CMSPermission.CMSUSER)
def cusers():
    return render_template('cms/cms_cusers.html', max_role=g.max_role)


@cms_bp.route('/croles/')
@permission_required(CMSPermission.ADMINER)
def croles():
    return render_template('cms/cms_croles.html', max_role=g.max_role)


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205
```

显示：
![flsk cms banner update](https://img-blog.csdnimg.cn/20200627202752224.gif#pic_center)

显然，修改数据并提交后，重新加载数据，显示的也是修改之后的数据。

删除banner也是通过JS实现的，通过发送banner_id到后端进行**业务逻辑处理**来实现。

在views.py中实现删除的业务逻辑如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message

from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm, AddBannerForm, UpdateBannerForm
from apps.cms.models import CMSUser, CMSPermission, BannerModel
from exts import db, mail
from utils import restful, random_captcha, clcache
from .decorators import permission_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request

@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html', max_role=g.max_role)


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html", max_role=g.max_role)


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html', max_role=g.max_role)

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html', max_role=g.max_role)

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            email = form.email.data
            # 修改用户邮箱
            g.cms_user.email = email
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


@cms_bp.route('/posts/')
@permission_required(CMSPermission.POSTER)
def posts():
    return render_template('cms/cms_posts.html', max_role=g.max_role)


@cms_bp.route('/banners/')
@permission_required(CMSPermission.BANNER)
def banners():
    banners = BannerModel.query.order_by(BannerModel.priority.desc()).all()
    return render_template('cms/cms_banners.html', max_role=g.max_role, banners=banners)


@cms_bp.route('/abanner/', methods=['POST'])
def abanner():
    form = AddBannerForm(request.form)
    if form.validate():
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel(name=name, image_url=image_url, link_url=link_url, priority=priority)
        db.session.add(banner)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message=form.get_error())


@cms_bp.route('/ubanner/', methods=['POST'])
def ubanner():
    # 根据id修改数据
    form = UpdateBannerForm(request.form)
    if form.validate():
        banner_id = form.banner_id.data
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel.query.get(banner_id)
        if banner:
            banner.name = name
            banner.image_url = image_url
            banner.link_url = link_url
            banner.priority = priority
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='轮播图不存在')
    else:
        return restful.params_error(form.get_error())


@cms_bp.route('/dbanner/', methods=['POST'])
def dbanner():
    banner_id = request.form.get('banner_id')
    if not banner_id:
        return restful.params_error(message='数据请求有误')
    banner = BannerModel.query.get(banner_id)
    if banner:
        db.session.delete(banner)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message='轮播图不存在')


@cms_bp.route('/comments/')
@permission_required(CMSPermission.COMMENTER)
def comments():
    return render_template('cms/cms_comments.html', max_role=g.max_role)


@cms_bp.route('/boards/')
@permission_required(CMSPermission.BOARDER)
def boards():
    return render_template('cms/cms_boards.html', max_role=g.max_role)


@cms_bp.route('/fusers/')
@permission_required(CMSPermission.FRONTUSER)
def fusers():
    return render_template('cms/cms_fusers.html', max_role=g.max_role)


@cms_bp.route('/cusers/')
@permission_required(CMSPermission.CMSUSER)
def cusers():
    return render_template('cms/cms_cusers.html', max_role=g.max_role)


@cms_bp.route('/croles/')
@permission_required(CMSPermission.ADMINER)
def croles():
    return render_template('cms/cms_croles.html', max_role=g.max_role)


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206207208209210211212213214215216217218219
```

演示如下：
![flsk cms banner delete](https://img-blog.csdnimg.cn/20200627202819954.gif#pic_center)

显然，在点击删除并提交数据后，数据被真的删除，显然在实际开发中是**有很大风险**的，可以增加一个字段来表示是否被删除。

models.py修改如下：

```python
from datetime import datetime
from werkzeug.security import generate_password_hash, check_password_hash
from exts import db


class CMSUser(db.Model):
    '''后台管理员用户类'''
    __tablename__ = 'cms_user'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    username = db.Column(db.String(30), nullable=False)
    _password = db.Column(db.String(100), nullable=False)
    email = db.Column(db.String(50), nullable=False, unique=True)
    join_time = db.Column(db.DateTime, default=datetime.now())

    def __init__(self, username, password, email):
        self.username = username
        self.password = password
        self.email = email

    @property
    def password(self):
        '''获取密码'''
        return self._password

    @password.setter
    def password(self, raw_password):
        '''设置密码'''
        self._password = generate_password_hash(raw_password)

    def check_password(self, raw_password):
        '''验证密码是否正确'''
        result = check_password_hash(self.password, raw_password)
        return result

    @property
    def permissions(self):
        '''判断用户权限'''
        if not self.roles:
            return 0

        all_permissions = 0
        for role in self.roles:
            permissions = role.permissions
            all_permissions |= permissions
        return all_permissions

    def has_permission(self, permission):
        '''判断当前用户是否有某个权限'''
        all_permissions = self.permissions
        result = all_permissions & permission == permission  # 如果当前用户有某个权限，则他的全部权限与该权限按位与的结果等于该权限本身
        return result

    @property
    def is_developer(self):
        '''判断当前用户是否是开发人员'''
        return self.has_permission(CMSPermission.ALL_PERMISSION)


class CMSPermission(object):
    # 255二进制表示所有权限
    ALL_PERMISSION = 0b11111111

    # 访问权限
    VISITOR = 0b00000001

    # 管理帖子权限
    POSTER = 0b00000010

    # 管理轮播图
    BANNER = 0b00000100

    # 管理评论权限
    COMMENTER = 0b00001000

    # 管理板块
    BOARDER = 0b00010000

    # 管理前台用户
    FRONTUSER = 0b00100000

    # 管理前台用户
    CMSUSER = 0b01000000

    # 管理前台用户
    ADMINER = 0b10000000


cms_role_user = db.Table(
    'cms_role_user',
    db.Column('cms_role_id', db.Integer, db.ForeignKey('cms_role.id'), primary_key=True),
    db.Column('cms_user_id', db.Integer, db.ForeignKey('cms_user.id'), primary_key=True)
)


class CMSRole(db.Model):
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(50), nullable=False)
    desc = db.Column(db.String(200), nullable=False)
    create_time = db.Column(db.DateTime, default=datetime.now())
    permissions = db.Column(db.Integer, default=CMSPermission.VISITOR)

    users = db.relationship('CMSUser', secondary=cms_role_user, backref='roles')


class BannerModel(db.Model):
    __tablename__ = 'banner'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(30), nullable=False)
    # 图片链接
    image_url = db.Column(db.String(255), nullable=False)
    # 跳转链接
    link_url = db.Column(db.String(255), nullable=False)
    # 优先级
    priority = db.Column(db.Integer, default=0)
    create_time = db.Column(db.DateTime, default=datetime.now)
    # 1表示被删除，0表示未删除，默认为0
    is_delete = db.Column(db.Integer, default=0)
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117
```

再通过命令进行映射，查询数据库，如下：

```sql
+----+-----------------------------+------------------------------------------------------------------------------------------------+-----------------------------------------------------+----------+---------------------+-----------+ 
| id | name                        | image_url                                                                                      | link_url                                            | priority | create_time         | is_delete | 
+----+-----------------------------+------------------------------------------------------------------------------------------------+-----------------------------------------------------+----------+---------------------+-----------+ 
|  3 | 人生苦短，我用Python        | http://dingyue.ws.126.net/ac51oREs50P=k9WskL7orDB3=qaRFiEDSwvLaGJIpz7it1549881599391.jpg       | https://blog.csdn.net/CUFEECR                       |        2 | 2020-06-27 18:05:36 |      NULL |        
|  4 | Python之禅                  | https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=2726542629,2791595101&fm=15&gp=0.jpg | https://blog.csdn.net/cufeecr/category_9441490.html |        3 | 2020-06-27 18:55:46 |         0 |   
+----+-----------------------------+------------------------------------------------------------------------------------------------+-----------------------------------------------------+----------+---------------------+-----------+ 
2 rows in set (0.01 sec)                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                                                          
12345678
```

显然，增加了is_delete字段，并且新插入的数据的is_delete值默认为0.

现在再次演示删除如下：
![flsk cms banner delete fake](https://img-blog.csdnimg.cn/20200627202841808.gif#pic_center)

显然，此时进行删除操作后，被删除的数据也不在页面中显示，此时再查询数据表如下：

```sql
+----+-----------------------------+------------------------------------------------------------------------------------------------+-----------------------------------------------------+----------+---------------------+-----------+
| id | name                        | image_url                                                                                      | link_url                                            | priority | create_time         | is_delete |
+----+-----------------------------+------------------------------------------------------------------------------------------------+-----------------------------------------------------+----------+---------------------+-----------+
|  3 | 人生苦短，我用Python        | http://dingyue.ws.126.net/ac51oREs50P=k9WskL7orDB3=qaRFiEDSwvLaGJIpz7it1549881599391.jpg       | https://blog.csdn.net/CUFEECR                       |        2 | 2020-06-27 18:05:36 |      NULL |
|  4 | Python之禅                  | https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=2726542629,2791595101&fm=15&gp=0.jpg | https://blog.csdn.net/cufeecr/category_9441490.html |        3 | 2020-06-27 18:55:46 |         1 |
+----+-----------------------------+------------------------------------------------------------------------------------------------+-----------------------------------------------------+----------+---------------------+-----------+
2 rows in set (0.01 sec)

12345678
```

显然，此时is_delete字段的值已经变为1，表示被删除。

## 五、七牛云介绍

之前上传的是图片链接，也可以上传本地图片，前面已经是货到过，我们上传本地图片不是到服务器，而是上传到**第三方平台**，这里选择**七牛云**https://www.qiniu.com/。

在创建完七牛云账号并认证成功后，有两步需要做：
（1）创建对象存储空间
示例如下：
![flsk cms banner qiniu create space](https://img-blog.csdnimg.cn/20200627202910401.gif#pic_center)

生产环境下，应该绑定公司自己的域名。

（2）获取**SDK文档**，以便于在项目中上传图片
示例如下：
![flsk cms banner qiniu sdk get](https://img-blog.csdnimg.cn/20200627202927691.gif#pic_center)

显然，我们首先需要通过命令`pip install qiniu`安装所需要的依赖库，在utils目录下创建upload_qiniu.py如下：

```python
from qiniu import Auth, put_file, etag

from config import QN_AK, QN_SK

# 构建鉴权对象
q = Auth(QN_AK, QN_SK)
# 要上传的空间
bucket_name = 'corley-images'
# 上传后保存的文件名
key = 'logo.png'
# 生成上传 Token，可以指定过期时间等
token = q.upload_token(bucket_name, key, 3600)
# 要上传文件的本地路径
localfile = 'E:\Test\logo.gif'
ret, info = put_file(token, key, localfile)
print('ret :', ret)
print('info:', info)
assert ret['key'] == key
assert ret['hash'] == etag(localfile)

1234567891011121314151617181920
```

config.py如下：

```python
import os

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_bbs'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

TEMPLATE_AUTO_RELOAD = True
SQLALCHEMY_DATABASE_URI = DB_URL
SQLALCHEMY_TRACK_MODIFICATIONS = False
SQLALCHEMY_POOL_RECYCLE = 280
SQLALCHEMY_POOL_SIZE = 20

# 设置密钥
SECRET_KEY = os.urandom(15)

# 发送邮箱服务地址
MAIL_SERVER = 'smtp.qq.com'
# 邮箱端口，为587或465，为587时TLS设置为True，为465时SSL设置为True
MAIL_PORT = 587
MAIL_USE_TLS = True
# MAIL_USE_SSL = True # MAIL_PORT为465时设置此项
# 用户名可以为你的邮箱，需要自行添加
MAIL_USERNAME = '379869029@qq.com'
# 邮箱密码，不是邮箱账号密码，而是第三方客户端登录使用的授权码，需要自行获取
MAIL_PASSWORD = 'vrxiqciwjnbcxxxx'
# 发送者即你的邮箱，需要自行添加
MAIL_DEFAULT_SENDER = '379869029@qq.com'

# 云片APIKEY
YP_API = 'edf71361381f31b3957beda37f20xxxx'

# 七牛云密钥
QN_AK = '_PL7p4sTSlfAKl71hIkuG3F6y18681ZKkNJ3xxxx'
QN_SK = '4P09jodUqhhEqIOcaqT9daVFmKjCJI3l6gsAxxxx'
1234567891011121314151617181920212223242526272829303132333435363738
```

运行结果如下：

```python
ret : {'hash': 'Fmx0VjUbKEYNFHSueiY7SnLxWY8j', 'key': 'logo.png'}
info: _ResponseInfo__response:<Response [200]>, exception:None, status_code:200, text_body:{"hash":"Fmx0VjUbKEYNFHSueiY7SnLxWY8j","key":"logo.png"}, req_id:HswAAADWYOplZBwW, x_log:X-Log

123
```

再查看七牛云官网**对象存储空间管理**如下：
![七牛云Python上传结果](https://img-blog.csdnimg.cn/20200627202959915.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，图片已经上传成功。

其中，七牛云的**Access Key**和**Secret Key**通过以下方式获取：
![七牛云 获取密钥](https://img-blog.csdnimg.cn/20200627203020536.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

# Python全栈（八）Flask项目实战之9.CMS七牛云上传和板块管理

Github和Gitee代码**同步更新**：
https://github.com/PythonWebProject/Flask_BBS；
https://gitee.com/Python_Web_Project/Flask_BBS。

## 一、前台轮播图展示

之前已经实现了在后台添加、编辑、删除轮播图，现在新增几组数据的基础上将其显示到前台，插入数据如下：
![flask cms banner data added](https://img-blog.csdnimg.cn/20200629214950369.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

现在apps/front/views.py修改如下：

```python
from flask import Blueprint, views, render_template, make_response, request, session
from io import BytesIO

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm
from .models import FrontUser
from apps.cms.models import BannerModel
from exts import db

front_bp = Blueprint('front', __name__)


@front_bp.route('/')
def index():
    banners = BannerModel.query.order_by(BannerModel.priority.desc()).limit(4)
    return render_template('front/front_index.html', banners=banners)


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html',return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101
```

templates/front/front_index.html修改如下：

```html
{% extends 'front/front_base.html' %}

{% block title %}首页{% endblock %}

{% block main_content %}
    <div class="main-container">
        <div class="cl-container">
            <div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
                <!-- Indicators 指令 -->
                <ol class="carousel-indicators">
                    {% for banner in banners %}
                        {% if loop.first %}
                            <li data-target="#carousel-example-generic" data-slide-to="{{ loop.index0 }}"
                                class="active"></li>
                        {% else %}
                            <li data-target="#carousel-example-generic" data-slide-to="{{ loop.index0 }}"></li>
                        {% endif %}
                    {% endfor %}
                </ol>

                <!-- Wrapper for slides 轮播图 -->
                <div class="carousel-inner" role="listbox">
                    {% for banner in banners %}
                        {% if loop.first %}
                            <div class="item active">
                                <img src="{{ banner.image_url }}" alt="{{ banner.name }}" width="400px" height="240px">
                                <div class="carousel-caption">
                                    {{ banner.name }}
                                </div>
                            </div>
                        {% else %}
                            <div class="item">
                                <img src="{{ banner.image_url }}" alt="{{ banner.name }}" width="400px" height="240px">
                                <div class="carousel-caption">
                                    {{ banner.name }}
                                </div>
                            </div>
                        {% endif %}


                    {% endfor %}
                    ...
                </div>

                <!-- Controls 左右切换控制 -->
                <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                    <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
                    <span class="sr-only">Previous</span>
                </a>
                <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                    <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
                    <span class="sr-only">Next</span>
                </a>
            </div>
        </div>

        <div class="sm-container">

        </div>
    </div>
{% endblock %}
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061
```

显示：
![flask front banner show](https://img-blog.csdnimg.cn/20200629215016550.gif#pic_center)

显然，已经将后台的轮播图数据展示到前台中。

## 二、文件上传七牛云

apps/common/views.py修改如下：

```python
from flask import Blueprint, request, jsonify
from qiniu import Auth, put_file, etag

from utils import send_msg, restful
from utils.captcha import Captcha
from .forms import SMSCaptchaForm
from utils import clcache
from config import QN_AK, QN_SK

common_bp = Blueprint('common', __name__, url_prefix='/c')


@common_bp.route('/sms_captcha/', methods=['POST'])
def sms_captcha():
    form = SMSCaptchaForm(request.form)

    if form.validate():
        telephone = form.telephone.data
        captcha = Captcha.gene_text(4)
        print('发送的短信验证码：{}'.format(captcha))
        if send_msg.send_mobile_msg(telephone, captcha) == 0:
            clcache.save_captcha(telephone, captcha)
            return restful.success()
        else:
            return restful.params_error(message='发送失败')
    else:
        return restful.params_error(message='参数错误')


@common_bp.route('/uptoken/')
def uptoken():
    # 构建鉴权对象
    q = Auth(QN_AK, QN_SK)
    # 要上传的空间
    bucket_name = 'bbs-images'
    # 生成上传 Token，可以指定过期时间等
    token = q.upload_token(bucket_name)

    return jsonify({'uptoken':token})
123456789101112131415161718192021222324252627282930313233343536373839
```

获取创建的存储空间对应的域名示意如下：
![flask common banner upload qiniu domail find](https://img-blog.csdnimg.cn/20200630233922865.gif#pic_center)

获取到域名后，到static/cms/js/banner.js中替换初始化七牛云部分的原始域名如下：

```js
var clajax = {
    'get': function (args) {
        args['method'] = 'get';
        this.ajax(args);
    },
    'post': function (args) {
        args['method'] = 'post';
        this.ajax(args);
    },
    'ajax': function (args) {
        // 设置csrftoken
        this._ajaxSetup();
        $.ajax(args);
    },
    '_ajaxSetup': function () {
        $.ajaxSetup({
            'beforeSend': function (xhr, settings) {
                if (!/^(GET|HEAD|OPTIONS|TRACE)$/i.test(settings.type) && !this.crossDomain) {
                    var csrftoken = $('meta[name=csrf-token]').attr('content');
                    xhr.setRequestHeader("X-CSRFToken", csrftoken)
                }
            }
        });
    }
};

$(function () {
    $("#save-banner-btn").click(function (event) {
        event.preventDefault();
        var self = $(this);
        var dialog = $("#banner-dialog");
        var nameInput = $("input[name='name']");
        var imageInput = $("input[name='image_url']");
        var linkInput = $("input[name='link_url']");
        var priorityInput = $("input[name='priority']");


        var name = nameInput.val();
        var image_url = imageInput.val();
        var link_url = linkInput.val();
        var priority = priorityInput.val();
        var submitType = self.attr('data-type');
        var bannerId = self.attr("data-id");

        if (!name || !image_url || !link_url || !priority) {
            clalert.alertInfoToast('请输入完整的轮播图数据！');
            return;
        }

        var url = '';
        if (submitType === 'update') {
            url = '/cms/ubanner/';
        } else {
            url = '/cms/abanner/';
        }

        clajax.post({
            "url": url,
            'data': {
                'name': name,
                'image_url': image_url,
                'link_url': link_url,
                'priority': priority,
                'banner_id': bannerId
            },
            'success': function (data) {
                dialog.modal("hide");
                if (data['code'] === 200) {
                    // 重新加载这个页面
                    window.location.reload();
                } else {
                    clalert.alertInfo(data['message']);
                }
            },
            'fail': function () {
                clalert.alertNetworkError();
            }
        });
    });
});

$(function () {
    $(".edit-banner-btn").click(function (event) {
        var self = $(this);
        var dialog = $("#banner-dialog");
        dialog.modal("show");

        var tr = self.parent().parent();
        var name = tr.attr("data-name");
        var image_url = tr.attr("data-image");
        var link_url = tr.attr("data-link");
        var priority = tr.attr("data-priority");

        var nameInput = dialog.find("input[name='name']");
        var imageInput = dialog.find("input[name='image_url']");
        var linkInput = dialog.find("input[name='link_url']");
        var priorityInput = dialog.find("input[name='priority']");
        var saveBtn = dialog.find("#save-banner-btn");

        nameInput.val(name);
        imageInput.val(image_url);
        linkInput.val(link_url);
        priorityInput.val(priority);
        saveBtn.attr("data-type", 'update');
        saveBtn.attr('data-id', tr.attr('data-id'));
    });
});

$(function () {
    $(".delete-banner-btn").click(function (event) {
        var self = $(this);
        var tr = self.parent().parent();
        var banner_id = tr.attr('data-id');
        clalert.alertConfirm({
            "msg": "您确定要删除这个轮播图吗？",
            'confirmCallback': function () {
                clajax.post({
                    'url': '/cms/dbanner/',
                    'data': {
                        'banner_id': banner_id
                    },
                    'success': function (data) {
                        if (data['code'] === 200) {
                            window.location.reload();
                        } else {
                            clalert.alertInfo(data['message']);
                        }
                    }
                })
            }
        });
    });
});

$(function () {
    clqiniu.setUp({
        'domain': 'http://qcquu7fd2.bkt.clouddn.com/',
        'browse_btn': 'upload-btn',
        'uptoken_url': '/c/uptoken/',
        'success': function (up, file, info) {
            var imageInput = $("input[name='image_url']");
            imageInput.val(file.name);
        }
    });
});
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145
```

进行测试如下：
![flask common banner upload local images](https://img-blog.csdnimg.cn/20200630232810511.gif#pic_center)
可以看到，在上传本地图片时，先上传到了七牛云，再将七牛云的图片链接作为轮播图的图片链接保存到数据库中；为了验证，可以到七牛云控制台查看，已经有了刚上传的图片。

**注意：**
这里使用七牛云上传图片是可能会遇到一个bug，如果你在创建存储空间时选的是华东以外的其他区，可能就会出现问题，如提示域名错误等，不知道具体是什么原因，可能是七牛云设计上的一个bug，所以得重新创建一个区，区域选择**华东**，我在这一步也费了很多时间死活没找出原因，后来看到别人的留言说可能是发呢取得问题，才新建了一个华东区的存储空间bbs-images，并修改为对应的域名，问题圆满解决。

### 七牛云JS上传流程分析

banners.js中处理七牛云上传图片时**初始化**的代码如下：

```js
$(function () {
    clqiniu.setUp({
        'domain': 'http://qcquu7fd2.bkt.clouddn.com/',
        'browse_btn': 'upload-btn',
        'uptoken_url': '/c/uptoken/',
        'success': function (up, file, info) {
            var imageInput = $("input[name='image_url']");
            imageInput.val(file.name);
        }
    });
});
1234567891011
```

`setUp()`是clqiniu.js中定义的一个方法，传入参数进行初始化：
domain表示上传到七牛云的域名；
browse_btn表示后台上传点击的添加图片按钮的id；
success是上传成功后的操作；
uptoken_url是上传需要传的token对应的地址。

## 三、板块页面渲染

CMS后台管理的一个功能是**板块管理**，即一篇文章属于哪一个板块，例如发布一篇与Numpy有关的文章可以归到Python数据分析板块，类似于掘金发布文章时需要选择的**分类**。

首先完善templates/cms/cms_boards.html如下：

```python
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台板块管理
{% endblock %}

{% block page_title %}
    熊熊板块管理
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    <div class="top-box">
        <button class="btn btn-warning" style="float:right;" id="add-board-btn">添加新板块</button>
    </div>
    <table class="table table-bordered">
        <thead>
        <tr>
            <th>板块名称</th>
            <th>帖子数量</th>
            <th>创建时间</th>
            <th>操作</th>
        </tr>
        </thead>
        <tbody>
        <tr data-name="" data-id="">
            <td>Python基础</td>
            <td>0</td>
            <td>2020-06-29</td>
            <td>
                <button class="btn btn-default btn-xs edit-board-btn">编辑</button>
                <button class="btn btn-danger btn-xs delete-board-btn">删除</button>
            </td>
        </tr>
        </tbody>
    </table>
{% endblock %}
12345678910111213141516171819202122232425262728293031323334353637383940
```

显示：
![flask cms boards preview](https://img-blog.csdnimg.cn/20200629215132987.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

此时，已经显示出了基本的页面效果。

static/cms/js目录下新建boards.js如下：

```js
var clajax = {
    'get': function (args) {
        args['method'] = 'get';
        this.ajax(args);
    },
    'post': function (args) {
        args['method'] = 'post';
        this.ajax(args);
    },
    'ajax': function (args) {
        // 设置csrftoken
        this._ajaxSetup();
        $.ajax(args);
    },
    '_ajaxSetup': function () {
        $.ajaxSetup({
            'beforeSend': function (xhr, settings) {
                if (!/^(GET|HEAD|OPTIONS|TRACE)$/i.test(settings.type) && !this.crossDomain) {
                    var csrftoken = $('meta[name=csrf-token]').attr('content');
                    xhr.setRequestHeader("X-CSRFToken", csrftoken)
                }
            }
        });
    }
};

$(function () {
    $("#add-board-btn").click(function (event) {
        event.preventDefault();
        clalert.alertOneInput({
            'text': '请输入板块名称！',
            'placeholder': '板块名称',
            'confirmCallback': function (inputValue) {
                clajax.post({
                    'url': '/cms/aboard/',
                    'data': {
                        'name': inputValue
                    },
                    'success': function (data) {
                        if (data['code'] === 200) {
                            window.location.reload();
                        } else {
                            clalert.alertInfo(data['message']);
                        }
                    }
                });
            }
        });
    });
});

$(function () {
    $(".edit-board-btn").click(function () {
        var self = $(this);
        var tr = self.parent().parent();
        var name = tr.attr('data-name');
        var board_id = tr.attr("data-id");

        clalert.alertOneInput({
            'text': '请输入新的板块名称！',
            'placeholder': name,
            'confirmCallback': function (inputValue) {
                clajax.post({
                    'url': '/cms/uboard/',
                    'data': {
                        'board_id': board_id,
                        'name': inputValue
                    },
                    'success': function (data) {
                        if (data['code'] === 200) {
                            window.location.reload();
                        } else {
                            clalert.alertInfo(data['message']);
                        }
                    }
                });
            }
        });
    });
});


$(function () {
    $(".delete-board-btn").click(function (event) {
        var self = $(this);
        var tr = self.parent().parent();
        var board_id = tr.attr('data-id');
        clalert.alertConfirm({
            "msg": "您确定要删除这个板块吗？",
            'confirmCallback': function () {
                clajax.post({
                    'url': '/cms/dboard/',
                    'data': {
                        // form  input name value
                        'board_id': board_id
                    },
                    'success': function (data) {
                        if (data['code'] === 200) {
                            window.location.reload();
                        } else {
                            clalert.alertInfo(data['message']);
                        }
                    }
                })
            }
        });
    });
});
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108
```

cms_boards.html完善如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台板块管理
{% endblock %}

{% block head %}
    <script src="{{ url_for('static', filename='cms/js/boards.js') }}"></script>
{% endblock %}

{% block page_title %}
    熊熊板块管理
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    <div class="top-box">
        <button class="btn btn-warning" style="float:right;" id="add-board-btn">添加新板块</button>
    </div>
    <table class="table table-bordered">
        <thead>
        <tr>
            <th>板块名称</th>
            <th>帖子数量</th>
            <th>创建时间</th>
            <th>操作</th>
        </tr>
        </thead>
        <tbody>
        <tr data-name="" data-id="">
            <td>Python基础</td>
            <td>0</td>
            <td>2020-06-29</td>
            <td>
                <button class="btn btn-default btn-xs edit-board-btn">编辑</button>
                <button class="btn btn-danger btn-xs delete-board-btn">删除</button>
            </td>
        </tr>
        </tbody>
    </table>
{% endblock %}
1234567891011121314151617181920212223242526272829303132333435363738394041424344
```

apps/cms/models.py创建**板块模型**如下：

```python
from datetime import datetime
from werkzeug.security import generate_password_hash, check_password_hash
from exts import db


class CMSUser(db.Model):
    '''后台管理员用户类'''
    __tablename__ = 'cms_user'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    username = db.Column(db.String(30), nullable=False)
    _password = db.Column(db.String(100), nullable=False)
    email = db.Column(db.String(50), nullable=False, unique=True)
    join_time = db.Column(db.DateTime, default=datetime.now())

    def __init__(self, username, password, email):
        self.username = username
        self.password = password
        self.email = email

    @property
    def password(self):
        '''获取密码'''
        return self._password

    @password.setter
    def password(self, raw_password):
        '''设置密码'''
        self._password = generate_password_hash(raw_password)

    def check_password(self, raw_password):
        '''验证密码是否正确'''
        result = check_password_hash(self.password, raw_password)
        return result

    @property
    def permissions(self):
        '''判断用户权限'''
        if not self.roles:
            return 0

        all_permissions = 0
        for role in self.roles:
            permissions = role.permissions
            all_permissions |= permissions
        return all_permissions

    def has_permission(self, permission):
        '''判断当前用户是否有某个权限'''
        all_permissions = self.permissions
        result = all_permissions & permission == permission  # 如果当前用户有某个权限，则他的全部权限与该权限按位与的结果等于该权限本身
        return result

    @property
    def is_developer(self):
        '''判断当前用户是否是开发人员'''
        return self.has_permission(CMSPermission.ALL_PERMISSION)


class CMSPermission(object):
    # 255二进制表示所有权限
    ALL_PERMISSION = 0b11111111

    # 访问权限
    VISITOR = 0b00000001

    # 管理帖子权限
    POSTER = 0b00000010

    # 管理轮播图
    BANNER = 0b00000100

    # 管理评论权限
    COMMENTER = 0b00001000

    # 管理板块
    BOARDER = 0b00010000

    # 管理前台用户
    FRONTUSER = 0b00100000

    # 管理前台用户
    CMSUSER = 0b01000000

    # 管理前台用户
    ADMINER = 0b10000000


cms_role_user = db.Table(
    'cms_role_user',
    db.Column('cms_role_id', db.Integer, db.ForeignKey('cms_role.id'), primary_key=True),
    db.Column('cms_user_id', db.Integer, db.ForeignKey('cms_user.id'), primary_key=True)
)


class CMSRole(db.Model):
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(50), nullable=False)
    desc = db.Column(db.String(200), nullable=False)
    create_time = db.Column(db.DateTime, default=datetime.now())
    permissions = db.Column(db.Integer, default=CMSPermission.VISITOR)

    users = db.relationship('CMSUser', secondary=cms_role_user, backref='roles')


class BannerModel(db.Model):
    __tablename__ = 'banner'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(30), nullable=False)
    # 图片链接
    image_url = db.Column(db.String(255), nullable=False)
    # 跳转链接
    link_url = db.Column(db.String(255), nullable=False)
    # 优先级
    priority = db.Column(db.Integer, default=0)
    create_time = db.Column(db.DateTime, default=datetime.now)
    # 1表示被删除，0表示未删除，默认为0
    is_delete = db.Column(db.Integer, default=0)


class BoardModel(db.Model):
    __tablename__ = 'cms_board'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(50), nullable=False)
    create_time = db.Column(db.DateTime, default=datetime.now)
    # 1表示被删除，0表示未删除，默认为0
    is_delete = db.Column(db.Integer, default=0)
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126
```

manage.py导入模型如下：

```python
from flask_script import Manager
from bbs import app
from flask_migrate import Migrate, MigrateCommand
from exts import db
from apps.cms.models import CMSUser, CMSRole, CMSPermission, BannerModel, BoardModel
from apps.front.models import FrontUser

manager = Manager(app)
Migrate(app, db)

manager.add_command('db', MigrateCommand)


@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
@manager.option('-e', '--email', dest='email')
def create_cms_user(username, password, email):
    user = CMSUser(username=username, password=password, email=email)
    db.session.add(user)
    db.session.commit()
    print('CMS用户添加成功')


@manager.command
def create_role():
    # 访问者
    visitor = CMSRole(name='访问者', desc='只能查看数据，不能修改数据')
    visitor.permissions = CMSPermission.VISITOR  # 也可以省去，因为默认权限就是VISITOR

    # 运营者
    operator = CMSRole(name='运营', desc='管理帖子，管理评论，管理前台和后台用户，管理轮播图')
    # 有多个权限时，使用或运算表示
    operator.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER | CMSPermission.BANNER
    # 管理员
    admin = CMSRole(name='管理员', desc='拥有本系统大部分权限')
    admin.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER | CMSPermission.BOARDER | CMSPermission.BANNER

    # 开发人员
    developer = CMSRole(name='开发者', desc='拥有所有权限')
    developer.permissions = CMSPermission.ALL_PERMISSION

    db.session.add_all([visitor, operator, admin, developer])
    db.session.commit()
    print('角色添加成功')


@manager.command
def test_permission():
    user = CMSUser.query.first()
    if user.has_permission(CMSPermission.VISITOR):
        print('该用户有使用者权限')
    else:
        print('该用户没有使用者权限')


@manager.option('-e', '--email', dest='email')
@manager.option('-n', '--name', dest='name')
def add_user_to_role(email, name):
    user = CMSUser.query.filter_by(email=email).first()
    if user:
        role = CMSRole.query.filter_by(name=name).first()
        if role:
            role.users.append(user)
            db.session.commit()
            print('用户添加到角色添加成功')
        else:
            print('角色不存在')
    else:
        print('邮箱不存在')


@manager.option('-t', '--telephone', dest='telephone')
@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
def create_front_user(telephone, username, password):
    user = FrontUser(telephone=telephone, username=username, password=password)
    db.session.add(user)
    db.session.commit()
    print('前台用户添加成功')


if __name__ == '__main__':
    manager.run()

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384
```

然后在命令行中进行**数据库映射**。

## 四、CMS板块的增删改查

先实现展示和增加板块的功能。

apps/cms/forms.py完善增加板块的表单验证如下：

```python
from wtforms import Form, StringField, IntegerField, ValidationError
from wtforms.validators import Email, InputRequired, Length, EqualTo, URL
from utils import clcache


class BaseForm(Form):
    def get_error(self):
        message = self.errors.popitem()[1][0]
        return message


class LoginForm(BaseForm):
    email = StringField(validators=[Email(message='请输入正确的邮箱地址'), InputRequired(message='请输入邮箱')])
    password = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    remember = IntegerField()


class ResetPwdForm(BaseForm):
    oldpwd = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    newpwd = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    newpwd2 = StringField(validators=[EqualTo("newpwd", message='两次输入密码不一致')])


class ResetEmailForm(BaseForm):
    email = StringField(validators=[Email(message='您的邮箱格式有误，请重新输入')])
    captcha = StringField(validators=[Length(4, 4, message='请输入4位验证码')])

    def validate_captcha(self, form):
        captcha = self.captcha.data
        email = self.email.data
        redis_captcha = clcache.get_captcha(email)
        if not redis_captcha or captcha.lower() != redis_captcha.lower():
            raise ValidationError('邮箱验证码错误')


class AddBannerForm(BaseForm):
    name = StringField(validators=[InputRequired(message='请输入轮播图名称')])
    image_url = StringField(validators=[InputRequired(message='请输入图片链接'), URL(message='请注意图片链接格式')])
    link_url = StringField(validators=[InputRequired(message='请输入跳转链接'), URL(message='请注意跳转链接格式')])
    priority = IntegerField(validators=[InputRequired(message='请输入轮播图优先级')])


class UpdateBannerForm(AddBannerForm):
    banner_id = IntegerField(validators=[InputRequired(message='轮播图不存在')])


class AddBoardForm(BaseForm):
    name = StringField(validators=[InputRequired(message='请输入板块名称')])
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748
```

apps/cms/views.py中完善增加板块的业务逻辑如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message
from sqlalchemy import or_

from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm, AddBannerForm, UpdateBannerForm, AddBoardForm
from apps.cms.models import CMSUser, CMSPermission, BannerModel, BoardModel
from exts import db, mail
from utils import restful, random_captcha, clcache
from .decorators import permission_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request

@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html', max_role=g.max_role)


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html", max_role=g.max_role)


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html', max_role=g.max_role)

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html', max_role=g.max_role)

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            email = form.email.data
            # 修改用户邮箱
            g.cms_user.email = email
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


@cms_bp.route('/posts/')
@permission_required(CMSPermission.POSTER)
def posts():
    return render_template('cms/cms_posts.html', max_role=g.max_role)


@cms_bp.route('/banners/')
@permission_required(CMSPermission.BANNER)
def banners():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(BannerModel.priority.desc()).all()
    return render_template('cms/cms_banners.html', max_role=g.max_role, banners=banners)


@cms_bp.route('/abanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def abanner():
    form = AddBannerForm(request.form)
    if form.validate():
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel(name=name, image_url=image_url, link_url=link_url, priority=priority)
        db.session.add(banner)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message=form.get_error())


@cms_bp.route('/ubanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def ubanner():
    # 根据id修改数据
    form = UpdateBannerForm(request.form)
    if form.validate():
        banner_id = form.banner_id.data
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel.query.get(banner_id)
        if banner and banner.is_delete != 1:
            banner.name = name
            banner.image_url = image_url
            banner.link_url = link_url
            banner.priority = priority
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='轮播图不存在')
    else:
        return restful.params_error(form.get_error())


@cms_bp.route('/dbanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def dbanner():
    banner_id = request.form.get('banner_id')
    if not banner_id:
        return restful.params_error(message='数据请求有误')
    banner = BannerModel.query.get(banner_id)
    if banner and banner.is_delete != 1:
        banner.is_delete = 1
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message='轮播图不存在')


@cms_bp.route('/comments/')
@permission_required(CMSPermission.COMMENTER)
def comments():
    return render_template('cms/cms_comments.html', max_role=g.max_role)


@cms_bp.route('/boards/')
@permission_required(CMSPermission.BOARDER)
def boards():
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    return render_template('cms/cms_boards.html', max_role=g.max_role, boards=boards)


@cms_bp.route('/aboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def aboard():
    form = AddBoardForm(request.form)
    if form.validate():
        name = form.name.data
        board = BoardModel(name=name)
        db.session.add(board)
        db.session.commit()
        return restful.success()
    else:
        restful.params_error(message=form.get_error())


@cms_bp.route('/fusers/')
@permission_required(CMSPermission.FRONTUSER)
def fusers():
    return render_template('cms/cms_fusers.html', max_role=g.max_role)


@cms_bp.route('/cusers/')
@permission_required(CMSPermission.CMSUSER)
def cusers():
    return render_template('cms/cms_cusers.html', max_role=g.max_role)


@cms_bp.route('/croles/')
@permission_required(CMSPermission.ADMINER)
def croles():
    return render_template('cms/cms_croles.html', max_role=g.max_role)


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206207208209210211212213214215216217218219220221222223224225226227228229230231232233234235236237238
```

cms_boards.html完善如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台板块管理
{% endblock %}

{% block head %}
    <script src="{{ url_for('static', filename='cms/js/boards.js') }}"></script>
{% endblock %}

{% block page_title %}
    熊熊板块管理
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    <div class="top-box">
        <button class="btn btn-warning" style="float:right;" id="add-board-btn">添加新板块</button>
    </div>
    <table class="table table-bordered">
        <thead>
        <tr>
            <th>板块名称</th>
            <th>帖子数量</th>
            <th>创建时间</th>
            <th>操作</th>
        </tr>
        </thead>
        <tbody>
        {% for board in boards %}
        <tr data-name="{{ board.name }}" data-id="{{ board.id }}">
            <td>{{ board.name}}</td>
            <td>0</td>
            <td>{{ board.create_time }}</td>
            <td>
                <button class="btn btn-default btn-xs edit-board-btn">编辑</button>
                <button class="btn btn-danger btn-xs delete-board-btn">删除</button>
            </td>
        </tr>
        {% endfor %}


        </tbody>
    </table>
{% endblock %}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748
```

演示如下：
![flask cms boards add board](https://img-blog.csdnimg.cn/2020062921523147.gif#pic_center)

显然，成功添加板块数据。

现实现板块修改功能。

forms.py中完善如下：

```python
from wtforms import Form, StringField, IntegerField, ValidationError
from wtforms.validators import Email, InputRequired, Length, EqualTo, URL
from utils import clcache


class BaseForm(Form):
    def get_error(self):
        message = self.errors.popitem()[1][0]
        return message


class LoginForm(BaseForm):
    email = StringField(validators=[Email(message='请输入正确的邮箱地址'), InputRequired(message='请输入邮箱')])
    password = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    remember = IntegerField()


class ResetPwdForm(BaseForm):
    oldpwd = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    newpwd = StringField(validators=[Length(6, 20, message='密码长度应介于6-20')])
    newpwd2 = StringField(validators=[EqualTo("newpwd", message='两次输入密码不一致')])


class ResetEmailForm(BaseForm):
    email = StringField(validators=[Email(message='您的邮箱格式有误，请重新输入')])
    captcha = StringField(validators=[Length(4, 4, message='请输入4位验证码')])

    def validate_captcha(self, form):
        captcha = self.captcha.data
        email = self.email.data
        redis_captcha = clcache.get_captcha(email)
        if not redis_captcha or captcha.lower() != redis_captcha.lower():
            raise ValidationError('邮箱验证码错误')


class AddBannerForm(BaseForm):
    name = StringField(validators=[InputRequired(message='请输入轮播图名称')])
    image_url = StringField(validators=[InputRequired(message='请输入图片链接'), URL(message='请注意图片链接格式')])
    link_url = StringField(validators=[InputRequired(message='请输入跳转链接'), URL(message='请注意跳转链接格式')])
    priority = IntegerField(validators=[InputRequired(message='请输入轮播图优先级')])


class UpdateBannerForm(AddBannerForm):
    banner_id = IntegerField(validators=[InputRequired(message='轮播图不存在')])


class AddBoardForm(BaseForm):
    name = StringField(validators=[InputRequired(message='请输入板块名称')])


class UpdateBoardForm(AddBoardForm):
    board_id = IntegerField(validators=[InputRequired(message='未获取到板块id')])
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152
```

views.py中实现业务逻辑如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message
from sqlalchemy import or_

from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm, AddBannerForm, UpdateBannerForm, AddBoardForm, UpdateBoardForm
from apps.cms.models import CMSUser, CMSPermission, BannerModel, BoardModel
from exts import db, mail
from utils import restful, random_captcha, clcache
from .decorators import permission_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request

@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html', max_role=g.max_role)


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html", max_role=g.max_role)


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html', max_role=g.max_role)

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html', max_role=g.max_role)

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            email = form.email.data
            # 修改用户邮箱
            g.cms_user.email = email
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


@cms_bp.route('/posts/')
@permission_required(CMSPermission.POSTER)
def posts():
    return render_template('cms/cms_posts.html', max_role=g.max_role)


@cms_bp.route('/banners/')
@permission_required(CMSPermission.BANNER)
def banners():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(BannerModel.priority.desc()).all()
    return render_template('cms/cms_banners.html', max_role=g.max_role, banners=banners)


@cms_bp.route('/abanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def abanner():
    form = AddBannerForm(request.form)
    if form.validate():
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel(name=name, image_url=image_url, link_url=link_url, priority=priority)
        db.session.add(banner)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message=form.get_error())


@cms_bp.route('/ubanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def ubanner():
    # 根据id修改数据
    form = UpdateBannerForm(request.form)
    if form.validate():
        banner_id = form.banner_id.data
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel.query.get(banner_id)
        if banner and banner.is_delete != 1:
            banner.name = name
            banner.image_url = image_url
            banner.link_url = link_url
            banner.priority = priority
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='轮播图不存在')
    else:
        return restful.params_error(form.get_error())


@cms_bp.route('/dbanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def dbanner():
    banner_id = request.form.get('banner_id')
    if not banner_id:
        return restful.params_error(message='数据请求有误')
    banner = BannerModel.query.get(banner_id)
    if banner and banner.is_delete != 1:
        banner.is_delete = 1
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message='轮播图不存在')


@cms_bp.route('/comments/')
@permission_required(CMSPermission.COMMENTER)
def comments():
    return render_template('cms/cms_comments.html', max_role=g.max_role)


@cms_bp.route('/boards/')
@permission_required(CMSPermission.BOARDER)
def boards():
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    return render_template('cms/cms_boards.html', max_role=g.max_role, boards=boards)


@cms_bp.route('/aboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def aboard():
    form = AddBoardForm(request.form)
    if form.validate():
        name = form.name.data
        board = BoardModel(name=name)
        db.session.add(board)
        db.session.commit()
        return restful.success()
    else:
        restful.params_error(message=form.get_error())


@cms_bp.route('/uboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def uboard():
    form = UpdateBoardForm(request.form)
    if form.validate():
        board_id = form.board_id.data
        name = form.name.data
        board = BoardModel.query.get(board_id)
        if board and board.is_delete != 1:
            board.name = name
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='没有该板块')
    else:
        return restful.params_error(form.get_error())


@cms_bp.route('/fusers/')
@permission_required(CMSPermission.FRONTUSER)
def fusers():
    return render_template('cms/cms_fusers.html', max_role=g.max_role)


@cms_bp.route('/cusers/')
@permission_required(CMSPermission.CMSUSER)
def cusers():
    return render_template('cms/cms_cusers.html', max_role=g.max_role)


@cms_bp.route('/croles/')
@permission_required(CMSPermission.ADMINER)
def croles():
    return render_template('cms/cms_croles.html', max_role=g.max_role)


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206207208209210211212213214215216217218219220221222223224225226227228229230231232233234235236237238239240241242243244245246247248249250251252253254255256
```

显示：
![flask cms boards update board](https://img-blog.csdnimg.cn/20200629215258595.gif#pic_center)

显然，此时可以正常修改板块信息。

现在views.py中实现删除板块的逻辑：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message
from sqlalchemy import or_

from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm, AddBannerForm, UpdateBannerForm, AddBoardForm, UpdateBoardForm
from apps.cms.models import CMSUser, CMSPermission, BannerModel, BoardModel
from exts import db, mail
from utils import restful, random_captcha, clcache
from .decorators import permission_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request

@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html', max_role=g.max_role)


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html", max_role=g.max_role)


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html', max_role=g.max_role)

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html', max_role=g.max_role)

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            email = form.email.data
            # 修改用户邮箱
            g.cms_user.email = email
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


@cms_bp.route('/posts/')
@permission_required(CMSPermission.POSTER)
def posts():
    return render_template('cms/cms_posts.html', max_role=g.max_role)


@cms_bp.route('/banners/')
@permission_required(CMSPermission.BANNER)
def banners():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(BannerModel.priority.desc()).all()
    return render_template('cms/cms_banners.html', max_role=g.max_role, banners=banners)


@cms_bp.route('/abanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def abanner():
    form = AddBannerForm(request.form)
    if form.validate():
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel(name=name, image_url=image_url, link_url=link_url, priority=priority)
        db.session.add(banner)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message=form.get_error())


@cms_bp.route('/ubanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def ubanner():
    # 根据id修改数据
    form = UpdateBannerForm(request.form)
    if form.validate():
        banner_id = form.banner_id.data
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel.query.get(banner_id)
        if banner and banner.is_delete != 1:
            banner.name = name
            banner.image_url = image_url
            banner.link_url = link_url
            banner.priority = priority
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='轮播图不存在')
    else:
        return restful.params_error(form.get_error())


@cms_bp.route('/dbanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def dbanner():
    banner_id = request.form.get('banner_id')
    if not banner_id:
        return restful.params_error(message='数据请求有误')
    banner = BannerModel.query.get(banner_id)
    if banner and banner.is_delete != 1:
        banner.is_delete = 1
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message='轮播图不存在')


@cms_bp.route('/comments/')
@permission_required(CMSPermission.COMMENTER)
def comments():
    return render_template('cms/cms_comments.html', max_role=g.max_role)


@cms_bp.route('/boards/')
@permission_required(CMSPermission.BOARDER)
def boards():
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    return render_template('cms/cms_boards.html', max_role=g.max_role, boards=boards)


@cms_bp.route('/aboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def aboard():
    form = AddBoardForm(request.form)
    if form.validate():
        name = form.name.data
        board = BoardModel(name=name)
        db.session.add(board)
        db.session.commit()
        return restful.success()
    else:
        restful.params_error(message=form.get_error())


@cms_bp.route('/uboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def uboard():
    form = UpdateBoardForm(request.form)
    if form.validate():
        board_id = form.board_id.data
        name = form.name.data
        board = BoardModel.query.get(board_id)
        if board and board.is_delete != 1:
            board.name = name
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='没有该板块')
    else:
        return restful.params_error(form.get_error())


@cms_bp.route('/dboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def dboard():
    board_id = request.form.get('board_id')
    if not board_id:
        return restful.params_error(message='板块不存在')
    board = BoardModel.query.get(board_id)
    if board and board.is_delete != 1:
        board.is_delete = 1
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message='板块不存在')


@cms_bp.route('/fusers/')
@permission_required(CMSPermission.FRONTUSER)
def fusers():
    return render_template('cms/cms_fusers.html', max_role=g.max_role)


@cms_bp.route('/cusers/')
@permission_required(CMSPermission.CMSUSER)
def cusers():
    return render_template('cms/cms_cusers.html', max_role=g.max_role)


@cms_bp.route('/croles/')
@permission_required(CMSPermission.ADMINER)
def croles():
    return render_template('cms/cms_croles.html', max_role=g.max_role)


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206207208209210211212213214215216217218219220221222223224225226227228229230231232233234235236237238239240241242243244245246247248249250251252253254255256257258259260261262263264265266267268269270271
```

显示：
![flask cms boards delete board](https://img-blog.csdnimg.cn/2020062921532152.gif#pic_center)

显然，此时被删除的数据不在页面中显示。

此时再查询数据库，如下：

```sql
select * from cms_board;
1
```

打印：

```sql
+----+------------------------+---------------------+-----------+
| id | name                   | create_time         | is_delete |
+----+------------------------+---------------------+-----------+
|  1 | Python入门             | 2020-06-29 20:01:24 |         0 |
|  2 | Python数据库           | 2020-06-29 20:01:37 |         1 |
|  3 | Python数据分析         | 2020-06-29 20:01:48 |         0 |
|  4 | Python Flask Web框架   | 2020-06-29 20:02:02 |         0 |
|  5 | Python Django框架      | 2020-06-29 20:02:17 |         1 |
+----+------------------------+---------------------+-----------+
5 rows in set (0.00 sec)

1234567891011
```

显然，数据并没有被真正地删除，只是is_delete字段值变为1。

## 五、博客编辑器的选择

前台发布博客和帖子需要用到**富文本编辑器**，网上可以搜索到很多，但是我们选择需要有一定的标准：

1. 对于程序员来说，一般要**支持Markdown**，便于书写，并且实现编辑和预览同步；
2. 界面要相对简洁，但是又不能有失美观。

这里我选择**Editor.md**，如下：
![Editor.md preview](https://img-blog.csdnimg.cn/2020062921542553.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
它功能非常丰富，支持Markdown，左端编辑、右端预览，非常方便，并且免费，其地址为https://pandao.github.io/editor.md/index.html。

需要进行下载，可以在Github上下载https://github.com/pandao/editor.md/archive/master.zip，也可以点击https://download.csdn.net/download/CUFEECR/12562040下载，后面帖子的编辑会基于**Editor.md**进行开发。

# Python全栈（八）Flask项目实战之10.前台发布帖子和后台帖子管理页面搭建

Github和Gitee代码**同步更新**：
https://github.com/PythonWebProject/Flask_BBS；
https://gitee.com/Python_Web_Project/Flask_BBS。

## 一、前台板块页面搭建

前面已经实现了在后台管理板块，现在在前台显示所有**板块（分类）**。

front_index.html完善板块显示和博客显示如下：

```html
{% extends 'front/front_base.html' %}

{% block title %}首页{% endblock %}

{% block main_content %}
    <div class="main-container">
        <div class="cl-container">
            <div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
                <!-- Indicators 指令 -->
                <ol class="carousel-indicators">
                    {% for banner in banners %}
                        {% if loop.first %}
                            <li data-target="#carousel-example-generic" data-slide-to="{{ loop.index0 }}"
                                class="active"></li>
                        {% else %}
                            <li data-target="#carousel-example-generic" data-slide-to="{{ loop.index0 }}"></li>
                        {% endif %}
                    {% endfor %}
                </ol>

                <!-- Wrapper for slides 轮播图 -->
                <div class="carousel-inner" role="listbox">
                    {% for banner in banners %}
                        {% if loop.first %}
                            <div class="item active">
                                <img src="{{ banner.image_url }}" alt="{{ banner.name }}" style="width: 400px; height: 240px">
                                <div class="carousel-caption">
                                    {{ banner.name }}
                                </div>
                            </div>
                        {% else %}
                            <div class="item">
                                <img src="{{ banner.image_url }}" alt="{{ banner.name }}" style="width: 400px; height: 240px">
                                <div class="carousel-caption">
                                    {{ banner.name }}
                                </div>
                            </div>
                        {% endif %}


                    {% endfor %}
                    ...
                </div>

                <!-- Controls 左右切换控制 -->
                <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                    <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
                    <span class="sr-only">Previous</span>
                </a>
                <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                    <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
                    <span class="sr-only">Next</span>
                </a>
            </div>

            <div class="post-group">
                <ul class="post-group-head">
                    <li class=""><a href="#">最新</a></li>
                    <li class=""><a href="#">精华帖子</a></li>
                    <li class=""><a href="#">点赞最多</a></li>
                    <li class=""><a href="#">评论最多</a></li>
                </ul>
                <ul class="post-list-group">
                    <li>
                        <div class="author-avatar-group">
                            <img src="#" alt="">
                        </div>
                        <div class="post-info-group">
                            <p class="post-title">
                                <a href="#">Python文章</a>
                                <span class="label label-danger">精华帖</span>
                            </p>
                            <p class="post-info">
                                <span>作者:Corley</span>
                                <span>发表时间:2020-07-01</span>
                                <span>评论:0</span>
                                <span>阅读:0</span>
                            </p>
                        </div>
                    </li>
                </ul>
                <div style="text-align:center;">

                </div>
            </div>
        </div>

        <div class="sm-container">
            <div style="padding-bottom:10px;">
                <a href="#" class="btn btn-warning btn-block">发布帖子</a>
            </div>
            <div class="list-group">
                <a href="/" class="list-group-item active">所有板块</a>
                {% for board in boards %}
                    {% if current_board == board.id %}
                        <a href="{{ url_for('front.index', board_id=board.id) }}"
                           class="list-group-item active">{{ board.name }}</a>
                    {% else %}
                        <a href="{{ url_for('front.index', board_id=board.id) }}"
                           class="list-group-item">{{ board.name }}</a>

                    {% endif %}
                {% endfor %}
            </div>
        </div>
    </div>
{% endblock %}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107
```

apps/front/views.py完善如下：

```python
from flask import Blueprint, views, render_template, make_response, request, session
from io import BytesIO
from sqlalchemy import or_

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm
from .models import FrontUser
from apps.cms.models import BannerModel, BoardModel
from exts import db

front_bp = Blueprint('front', __name__)


@front_bp.route('/')
def index():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(BannerModel.priority.desc()).limit(4)
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    board_id = request.args.get('board_id', type=int, default=None)
    context = {
        'banners':banners,
        'boards':boards,
        'current_board':board_id
    }
    return render_template('front/front_index.html', **context)


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html',return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109
```

显示：
![flask front board post show](https://img-blog.csdnimg.cn/20200702195022816.gif#pic_center)

显然，此时轮播图正常显示，板块也显示出来，并且点击该板块还会有颜色**突出显示**，地址中的参数值也会变化，页面下部也显示出博客的基本雏形。

## 二、发布帖子页面搭建

因为编辑博客需要富文本编辑器，之前已经选择了**Editor.md**，将下载解压后的主目录重命名为editormd，并复制到static目录下。
因为需要使用**JQuery**，所以在static/front/js下创建jquery.min.js，代码为https://code.jquery.com/jquery-1.11.1.min.js中的文本，全选复制粘贴即可，或者直接点击https://download.csdn.net/download/CUFEECR/12570955下载并复制到static/front/js目录下。

templates/front下新建front_apost.html如下：

```html
{% extends 'front/front_base.html' %}

{% block title %}
    帖子发布
{% endblock %}

{% block head %}
    <link rel="stylesheet" href="{{ url_for('static', filename='editormd/css/editormd.css') }}"/>
    <script src="{{ url_for('static', filename='front/js/jquery.min.js') }}"></script>
    <script src="{{ url_for('static', filename='editormd/editormd.js') }}"></script>
{% endblock %}

{% block main_content %}
    <div class="main-container">
        <form action="" method="post">
            <input type="hidden" name="csrf_token" value="{{ csrf_token() }}">
            <div class="form-group">
                <div class="input-group">
                    <span class="input-group-addon">标题</span>
                    <input type="text" class="form-control" name="title">
                </div>
            </div>
            <div class="form-group">
                <div class="input-group">
                    <span class="input-group-addon">板块</span>
                    <select name="board_id" class="form-control">
                        <option value="">Python入门</option>
                        <option value="">Python数据分析</option>
                    </select>
                </div>
            </div>
            <div id="editor" class="form-group">
                <textarea name="content" id="TextContent"></textarea>
            </div>
            <div class="form-group">
                <button class="btn btn-danger" id="submit-btn">发布帖子</button>
            </div>
        </form>

        <script type="text/javascript">
            var testEditor;

            $(function () {
                testEditor = editormd("editor", {
                    width: "100%",
                    height: 640,
                    syncScrolling: "single",
                    path: "{{ url_for('static',filename='editormd/lib/') }}",
                    // 上传图片
                    imageUpload: true,
                    imageFormats: ["jpg", "jpeg", "gif", "png", "bmp", "webp"],
                    // 上传图片时指定调用后台的视图函数
                    imageUploadURL: "",

                });
            });
        </script>
    </div>
{% endblock %}
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859
```

apps/front/views.py修改如下：

```python
from flask import Blueprint, views, render_template, make_response, request, session
from io import BytesIO
from sqlalchemy import or_

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm
from .models import FrontUser
from apps.cms.models import BannerModel, BoardModel
from exts import db

front_bp = Blueprint('front', __name__)


@front_bp.route('/')
def index():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(BannerModel.priority.desc()).limit(4)
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    board_id = request.args.get('board_id', type=int, default=None)
    context = {
        'banners':banners,
        'boards':boards,
        'current_board':board_id
    }
    return render_template('front/front_index.html', **context)


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


class PostView(views.MethodView):
    def get(self):
        return render_template('front/front_apost.html')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html',return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))
front_bp.add_url_rule('/apost/', view_func=PostView.as_view('apost'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115
```

访问示例如下：
![flask front apost preview](https://img-blog.csdnimg.cn/20200702195525887.gif#pic_center)
显然，此时已经可以使用markdown编辑博客，但是还不能发布。

前面没有对发帖前进行**登录验证**，但是显然在发帖子之前必须验证是否登录，如果没登录还需要跳转到登录页登录之后才能发帖，此时需要用**装饰器**进行验证，apps/front下新建decorators.py如下：

```python
from flask import session, redirect, url_for, g

def login_required(func):
    def inner(*args, **kwargs):
        if 'user_id' in session:
            return func(*args, **kwargs)
        else:
            return redirect(url_for('front.signin'))
    return inner
123456789
```

views.py完善如下：

```python
from flask import Blueprint, views, render_template, make_response, request, session
from io import BytesIO
from sqlalchemy import or_

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm
from .models import FrontUser
from .decorators import login_required
from apps.cms.models import BannerModel, BoardModel
from exts import db

front_bp = Blueprint('front', __name__)


@front_bp.route('/')
def index():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(BannerModel.priority.desc()).limit(4)
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    board_id = request.args.get('board_id', type=int, default=None)
    context = {
        'banners':banners,
        'boards':boards,
        'current_board':board_id
    }
    return render_template('front/front_index.html', **context)


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


class PostView(views.MethodView):
    decorators = [login_required]
    def get(self):
        return render_template('front/front_apost.html')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html',return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))
front_bp.add_url_rule('/apost/', view_func=PostView.as_view('apost'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117
```

front_index.html修改如下：

```html
{% extends 'front/front_base.html' %}

{% block title %}首页{% endblock %}

{% block main_content %}
    <div class="main-container">
        <div class="cl-container">
            <div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
                <!-- Indicators 指令 -->
                <ol class="carousel-indicators">
                    {% for banner in banners %}
                        {% if loop.first %}
                            <li data-target="#carousel-example-generic" data-slide-to="{{ loop.index0 }}"
                                class="active"></li>
                        {% else %}
                            <li data-target="#carousel-example-generic" data-slide-to="{{ loop.index0 }}"></li>
                        {% endif %}
                    {% endfor %}
                </ol>

                <!-- Wrapper for slides 轮播图 -->
                <div class="carousel-inner" role="listbox">
                    {% for banner in banners %}
                        {% if loop.first %}
                            <div class="item active">
                                <img src="{{ banner.image_url }}" alt="{{ banner.name }}" style="width: 400px; height: 240px">
                                <div class="carousel-caption">
                                    {{ banner.name }}
                                </div>
                            </div>
                        {% else %}
                            <div class="item">
                                <img src="{{ banner.image_url }}" alt="{{ banner.name }}" style="width: 400px; height: 240px">
                                <div class="carousel-caption">
                                    {{ banner.name }}
                                </div>
                            </div>
                        {% endif %}


                    {% endfor %}
                    ...
                </div>

                <!-- Controls 左右切换控制 -->
                <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                    <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
                    <span class="sr-only">Previous</span>
                </a>
                <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                    <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
                    <span class="sr-only">Next</span>
                </a>
            </div>

            <div class="post-group">
                <ul class="post-group-head">
                    <li class=""><a href="#">最新</a></li>
                    <li class=""><a href="#">精华帖子</a></li>
                    <li class=""><a href="#">点赞最多</a></li>
                    <li class=""><a href="#">评论最多</a></li>
                </ul>
                <ul class="post-list-group">
                    <li>
                        <div class="author-avatar-group">
                            <img src="#" alt="">
                        </div>
                        <div class="post-info-group">
                            <p class="post-title">
                                <a href="#">Python文章</a>
                                <span class="label label-danger">精华帖</span>
                            </p>
                            <p class="post-info">
                                <span>作者:Corley</span>
                                <span>发表时间:2020-07-01</span>
                                <span>评论:0</span>
                                <span>阅读:0</span>
                            </p>
                        </div>
                    </li>
                </ul>
                <div style="text-align:center;">

                </div>
            </div>
        </div>

        <div class="sm-container">
            <div style="padding-bottom:10px;">
                <a href="{{ url_for('front.apost') }}" class="btn btn-warning btn-block">发布帖子</a>
            </div>
            <div class="list-group">
                <a href="/" class="list-group-item active">所有板块</a>
                {% for board in boards %}
                    {% if current_board == board.id %}
                        <a href="{{ url_for('front.index', board_id=board.id) }}"
                           class="list-group-item active">{{ board.name }}</a>
                    {% else %}
                        <a href="{{ url_for('front.index', board_id=board.id) }}"
                           class="list-group-item">{{ board.name }}</a>

                    {% endif %}
                {% endfor %}
            </div>
        </div>
    </div>
{% endblock %}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107
```

测试如下：
![flask front apost login required](https://img-blog.csdnimg.cn/20200702195613446.gif#pic_center)

显然，此时如果未登录，会跳转到登录页，登录后才能访问发帖等页面。

**说明**：
（1）不能像后端那样用钩子函数进行限制，因为进入后台管理页本身就要先登录才能进入，而前台本身需要作为游客也可以访问，因此要想限制前台用户登录后才能进行更多操作，因此使用装饰器更合适；
（2）因为前后台都需要对登录进行验证，如果登录成功，则需要将user id存入**session**，之前后台保存的时候使用的key是user_id，为防止前后端同时登录造成user id覆盖和冲突，因此后台存入的key统一都改为cms_user_id（主要在views.py、decorators.py和hooks.py中），而前台使用user_id作为保存的key。

现在需要对右上角的显示进行调整优化，如果未登录则显示登录、注册按钮，否则显示用户信息即可。

apps/front下新建hooks.py用于在**上下文**保存前台用户如下：

```python
from flask import session, g
from .models import FrontUser
from .views import front_bp


@front_bp.before_request
def before_request():
    if 'user_id' in session:
        user_id = session.get('user_id')
        user = FrontUser.query.get(user_id)
        # 使用g对象保存用户
        if user:
            g.front_user = user
12345678910111213
```

views.py完善如下：

```python
from flask import Blueprint, views, render_template, make_response, request, session
from io import BytesIO
from sqlalchemy import or_

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm
from .models import FrontUser
from .decorators import login_required
from apps.cms.models import BannerModel, BoardModel
from exts import db

front_bp = Blueprint('front', __name__)

from .hooks import before_request

@front_bp.route('/')
def index():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(BannerModel.priority.desc()).limit(4)
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    board_id = request.args.get('board_id', type=int, default=None)
    context = {
        'banners':banners,
        'boards':boards,
        'current_board':board_id
    }
    return render_template('front/front_index.html', **context)


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


class PostView(views.MethodView):
    decorators = [login_required]
    def get(self):
        return render_template('front/front_apost.html')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html',return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))
front_bp.add_url_rule('/apost/', view_func=PostView.as_view('apost'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118
```

front_base.html修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='front/css/front_base.css') }}">
    <link rel="stylesheet" href="{{ url_for('static', filename='front/css/front_index.css') }}">
    <title>{% block title %}{% endblock %}</title>
    {% block head %}{% endblock %}
</head>
<body>
<nav class="navbar navbar-default">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                    data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">Python栏目</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
                <li class="active"><a href="/">首页 <span class="sr-only">(current)</span></a></li>
            </ul>
            <form class="navbar-form navbar-left">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">搜索</button>
            </form>
            <ul class="nav navbar-nav navbar-right">
                {% if g.front_user %}
                    <li class="dropdown">
                        <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
                           aria-expanded="false">{{ g.front_user.username}}<span class="caret"></span></a>
                        <ul class="dropdown-menu">
                            <li><a href="#">个人中心</a></li>
                            <li><a href="#">设置</a></li>
                            <li><a href="#">退出登录</a></li>
                        </ul>
                    </li>
                {% else %}
                    <li><a href="{{ url_for('front.signin') }}">登录</a></li>
                    <li><a href="{{ url_for('front.signup') }}">注册</a></li>
                {% endif %}


            </ul>
        </div><!-- /.navbar-collapse -->
    </div><!-- /.container-fluid -->
</nav>

{% block main_content %}

{% endblock %}


</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667
```

显示：
![flask front user info](https://img-blog.csdnimg.cn/20200702195752137.gif#pic_center)

此时，右上角显示已经完善。

## 三、前台帖子模型创建

对于用markdown编辑的博客，我们不应该保存markdown格式的原始内容，而是应该保存渲染后的html内容，这样渲染到前端才会有特定的格式，需要**建立模型**来保存博客数据，apps/front/models.py如下：

```python
import shortuuid
import enum
from werkzeug.security import generate_password_hash, check_password_hash
from datetime import datetime
from markdown import markdown
import bleach

from exts import db


class GenderEnum(enum.Enum):
    MALE = 1
    FAMALE = 2
    SECRET = 3
    UNKNOWN = 4


class FrontUser(db.Model):
    __tablename__ = 'front_user'
    id = db.Column(db.String(40), primary_key=True, default=shortuuid.uuid)
    telephone = db.Column(db.String(11), nullable=False, unique=True)
    username = db.Column(db.String(50), nullable=False)
    _password = db.Column(db.String(100), nullable=False)
    email = db.Column(db.String(100), unique=False)
    realname = db.Column(db.String(30))
    avatar = db.Column(db.String(80))
    signature = db.Column(db.String(200))
    gender = db.Column(db.Enum(GenderEnum), default=GenderEnum.UNKNOWN)

    join_time = db.Column(db.DateTime, default=datetime.now)

    def __init__(self, *args, **kwargs):
        if 'password' in kwargs:
            self.password = kwargs.get('password')
            kwargs.pop('password')

        super().__init__(*args, **kwargs)

    @property
    def password(self):
        '''获取密码'''
        return self._password

    @password.setter
    def password(self, raw_password):
        '''设置密码'''
        self._password = generate_password_hash(raw_password)

    def check_password(self, raw_password):
        '''验证密码是否正确'''
        result = check_password_hash(self.password, raw_password)
        return result


class PostModel(db.Model):
    __tablename__ = 'post'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    title = db.Column(db.String(100), nullable=False)
    content = db.Column(db.Text, nullable=False)
    content_html = db.Column(db.Text)
    create_time = db.Column(db.DateTime, default=datetime.now)

    board_id = db.Column(db.Integer, db.ForeignKey("cms_board.id"))
    author_id = db.Column(db.String(40), db.ForeignKey("front_user.id"))
    # 1表示被删除，0表示未删除，默认为0
    is_delete = db.Column(db.Integer, default=0)

    board = db.relationship("BoardModel", backref='posts')
    author = db.relationship("FrontUser", backref='posts')

    def on_changed_content(target, value, oldvalue, initiator):
        '''实现markdown转HTML并进行安全验证'''
        # 允许上传的标签
        allowed_tags = ['a', 'abbr', 'acronym', 'b', 'blockquote', 'code', 'em', 'i', 'li', 'ol', 'pre', 'strong', 'ul',
                        'h1', 'h2', 'h3', 'p', 'img', 'video', 'div', 'iframe', 'p', 'br', 'span', 'hr', 'src', 'class']

        # 允许上传的属性
        allowed_attrs = {'*': ['class'],
                         'a': ['href', 'rel'],
                         'img': ['src', 'alt']}

        # 目标设置
        target.content_html = bleach.linkify(
            bleach.clean(markdown(value, output_format='html'), tags=allowed_tags, strip=True,
                         attributes=allowed_attrs))


# 数据库事件监听
db.event.listen(PostModel.content, 'set', PostModel.on_changed_content)
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283848586878889
```

manage.py修改如下：

```python
from flask_script import Manager
from bbs import app
from flask_migrate import Migrate, MigrateCommand
from exts import db
from apps.cms.models import CMSUser, CMSRole, CMSPermission, BannerModel, BoardModel
from apps.front.models import FrontUser, PostModel

manager = Manager(app)
Migrate(app, db)

manager.add_command('db', MigrateCommand)


@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
@manager.option('-e', '--email', dest='email')
def create_cms_user(username, password, email):
    user = CMSUser(username=username, password=password, email=email)
    db.session.add(user)
    db.session.commit()
    print('CMS用户添加成功')


@manager.command
def create_role():
    # 访问者
    visitor = CMSRole(name='访问者', desc='只能查看数据，不能修改数据')
    visitor.permissions = CMSPermission.VISITOR  # 也可以省去，因为默认权限就是VISITOR

    # 运营者
    operator = CMSRole(name='运营', desc='管理帖子，管理评论，管理前台和后台用户，管理轮播图')
    # 有多个权限时，使用或运算表示
    operator.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER | CMSPermission.BANNER
    # 管理员
    admin = CMSRole(name='管理员', desc='拥有本系统大部分权限')
    admin.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER | CMSPermission.BOARDER | CMSPermission.BANNER

    # 开发人员
    developer = CMSRole(name='开发者', desc='拥有所有权限')
    developer.permissions = CMSPermission.ALL_PERMISSION

    db.session.add_all([visitor, operator, admin, developer])
    db.session.commit()
    print('角色添加成功')


@manager.command
def test_permission():
    user = CMSUser.query.first()
    if user.has_permission(CMSPermission.VISITOR):
        print('该用户有使用者权限')
    else:
        print('该用户没有使用者权限')


@manager.option('-e', '--email', dest='email')
@manager.option('-n', '--name', dest='name')
def add_user_to_role(email, name):
    user = CMSUser.query.filter_by(email=email).first()
    if user:
        role = CMSRole.query.filter_by(name=name).first()
        if role:
            role.users.append(user)
            db.session.commit()
            print('用户添加到角色添加成功')
        else:
            print('角色不存在')
    else:
        print('邮箱不存在')


@manager.option('-t', '--telephone', dest='telephone')
@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
def create_front_user(telephone, username, password):
    user = FrontUser(telephone=telephone, username=username, password=password)
    db.session.add(user)
    db.session.commit()
    print('前台用户添加成功')


if __name__ == '__main__':
    manager.run()

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384
```

此时在命令行中项目主目录下执行命令进行数据库映射，再查询数据库：

```sql
show tables;
1
```

打印：

```sql
+---------------------+
| Tables_in_flask_bbs |
+---------------------+
| alembic_version     |
| banner              |
| cms_board           |
| cms_role            |
| cms_role_user       |
| cms_user            |
| front_user          |
| post                |
+---------------------+
8 rows in set (0.00 sec)

1234567891011121314
```

显然，已经创建post表成功。

**说明**：
因为在前台需要将markdown格式的原始文本渲染成有特定格式的博客、美化HTML格式并且防止用户的XSS注入，需要通过命令`pip install markdown`和`pip install bleach`在虚拟环境中安装Python库markdown和bleach。

## 四、文章的发布

### 1.基本实现

现在实现发布博客的**业务逻辑**。

需要在apps/front/forms.py中进行表单验证：

```python
from wtforms import Form, StringField, ValidationError, IntegerField
from wtforms.validators import Length, EqualTo, Regexp, InputRequired
from utils import clcache


class BaseForm(Form):
    def get_error(self):
        message = self.errors.popitem()[1][0]
        return message


class SignupForm(BaseForm):
    telephone = StringField(validators=[Regexp(r'1[3-9]\d{9}', message='请输入正确的手机号')])
    sms_captcha = StringField(validators=[Regexp(r'\w{4}', message='请输入4位短信验证码')])
    username = StringField(validators=[Length(min=2, max=20, message='用户名应介于2-20个字符，请重新输入')])
    password1 = StringField(validators=[Regexp(r'[0-9a-zA-Z_\.]{6,20}', message='密码应介于6-20位，并且由数字、英文字母或下划线、英文句号或短横线组成')])
    password2 = StringField(validators=[EqualTo('password1', message='两次输入密码不一致')])
    graph_captcha = StringField(validators=[Regexp(r'\w{4}', message='请输入4位图形验证码')])

    def validate_sms_captcha(self, field):
        telephone = self.telephone.data
        sms_captcha = self.sms_captcha.data

        # 从Redis中取验证码
        sms_captcha_redis = clcache.get_captcha(telephone)
        if not sms_captcha_redis or sms_captcha_redis.lower() != sms_captcha.lower():
            raise ValidationError(message='短信验证码过期或有误')

    def validate_graph_captcha(self, field):
        graph_captcha = self.graph_captcha.data
        
        # 从Redis中取图形验证码
        graph_captcha_redis = clcache.get_captcha(graph_captcha.lower())
        if not graph_captcha_redis or graph_captcha_redis.lower() != graph_captcha.lower():
            raise ValidationError(message='图形验证码过期或有误')


class SigninForm(BaseForm):
    telephone = StringField(validators=[Regexp(r'1[3-9]\d{9}', message='请输入正确的手机号')])
    password = StringField(validators=[Regexp(r'[0-9a-zA-Z_\.]{6,20}', message='您输入的密码有误，请重新输入')])
    remember = StringField(InputRequired())


class AddPostForm(BaseForm):
    title = StringField(validators=[InputRequired(message='请输入博客标题')])
    board_id = IntegerField(validators=[InputRequired(message='数据提交有误，请重试')])
    content = StringField(validators=[InputRequired(message='请输入博客内容')])
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647
```

views.py中实现业务逻辑如下：

```python
from flask import Blueprint, views, render_template, make_response, request, session, g, redirect, url_for
from io import BytesIO
from sqlalchemy import or_

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm, AddPostForm
from .models import FrontUser, PostModel
from .decorators import login_required
from apps.cms.models import BannerModel, BoardModel
from exts import db

front_bp = Blueprint('front', __name__)

from .hooks import before_request

@front_bp.route('/')
def index():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(BannerModel.priority.desc()).limit(4)
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    board_id = request.args.get('board_id', type=int, default=None)
    context = {
        'banners':banners,
        'boards':boards,
        'current_board':board_id
    }
    return render_template('front/front_index.html', **context)


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


class PostView(views.MethodView):
    decorators = [login_required]

    def get(self):
        boards = BoardModel.query.all()
        return render_template('front/front_apost.html', boards=boards)

    def post(self):
        form = AddPostForm(request.form)
        if form.validate():
            title = form.title.data
            content = form.content.data
            board_id = form.board_id.data
            board = BoardModel.query.get(board_id)
            if board and board.is_delete != 1:
                post = PostModel(title=title, content=content)
                post.board = board
                post.author = g.front_user
                db.session.add(post)
                db.session.commit()
                return redirect(url_for('front.index'))
            else:
                return restful.params_error(message='没有该板块，请重新选择')
        else:
            return restful.params_error(form.get_error())


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html',return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))
front_bp.add_url_rule('/apost/', view_func=PostView.as_view('apost'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139
```

进行发布博客测试如下：
![flask front post test](https://img-blog.csdnimg.cn/2020070219584434.gif#pic_center)

显然，此时已经可以正常发帖，并且在发布之后跳转回首页，再查询数据据库：

```sql
select * from post;
1
```

打印：

```sql
+----+--------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------+----------+------------------------+-----------+
| id | title              | content



                                                                                                     | content_html



                                                                                                                                                                                | create_time         | board_id | author_id              | is_delete |
+----+--------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------+----------+------------------------+-----------+
|  1 | Python面向对象     | # 一、面向对象的简介
面向对象**OOP**
什么是对象：
对象就是**内存**中存储指定数据的一块区域。
实际上对象就是一个容器，专门用来存数据。
程序运行的通俗解释：
代码存在硬盘，CPU处理代码，CPU和硬盘之间有内存，解释器将代码交给内存，CPU再从内存读取。

# 二、面向对象的结构
## 1.id（标识）
用来标识对象的唯一性，每个对象都有唯一的id，每个id指向一个内存地址值.
id由解释器生成，其实就是对象的内存地址。
## 2.type（类型）
类型决定了对象有哪些功能。
可以通过`type()`函数来查看对象的类型。
## 3.value（值）
值就是对象中存储的具体数据，分为
☆可变对象：**列表list、集合set、字典dict**
☆不可变对象：**数值类型int、float，字符串str，元组tuple，布尔型bool**

# 三、面向对象的举例
所谓**面向对象**，简单理解就是语言中所有的操作都是通过对象来进行的。
**面向过程 --> 面向对象**
面向过程的典型代表是C语言，是早期语言的特点。
※面向对象是一种思考问题的方式，是一种思想；
※面向对象将事物变得简单化。
对**面向对象**的理解：
（1）可以让复杂事务变得简单化，人由执行者过渡到指挥者；
（2）具备封装、继承和多态；
（3）举例！！
举例：孩子吃瓜
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
优点：
比较符合人的思维，编写起来比较容易；
缺点：
这种编程方式只适用于一个功能，我们要实现别的功能的时候，往往需要编写新的代码，复用性较低。
面向对象就是**让孩子吃瓜**，而不管具体怎么实现，细节在对象内部。
面向对象：
优点：容易维护、方便复用；
缺点：不符合人的思维，编写时比较麻烦。 | <h1>一、面向对象的简介</h1>
<p>面向对象<strong>OOP</strong>
什么是对象：
对象就是<strong>内存</strong>中存储指定数据的一块区域。
实际上对象就是一个容器，专门用来存数据。
程序运行的通俗解释：
代码存在硬盘，CPU处理代码，CPU和硬盘之间有内存，解释器将代码交给内存，CPU再从内存读取。</p>
<h1>二、面向对象的结构</h1>
<h2><a href="http://1.id" rel="nofollow">1.id</a>（标识）</h2>
<p>用来标识对象的唯一性，每个对象都有唯一的id，每个id指向一个内存地址值.
id由解释器生成，其实就是对象的内存地址。</p>
<h2>2.type（类型）</h2>
<p>类型决定了对象有哪些功能。
可以通过<code>type()</code>函数来查看对象的类型。</p>
<h2>3.value（值）</h2>
<p>值就是对象中存储的具体数据，分为
☆可变对象：<strong>列表list、集合set、字典dict</strong>
☆不可变对象：<strong>数值类型int、float，字符串str，元组tuple，布尔型bool</strong></p>
<h1>三、面向对象的举例</h1>
<p>所谓<strong>面向对象</strong>，简单理解就是语言中所有的操作都是通过对象来进行的。
<strong>面向过程 --&gt; 面向对象</strong>
面向过程的典型代表是C语言，是早期语言的特点。
※面向对象是一种思考问题的方式，是一种思想；
※面向对象将事物变得简单化。
对<strong>面向对象</strong>的理解：
（1）可以让复杂事务变得简单化，人由执行者过渡到指挥者；
（2）具备封装、继承和多态；
（3）举例！！
举例：孩子吃瓜
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
优点：
比较符合人的思维，编写起来比较容易；
缺点：
这种编程方式只适用于一个功能，我们要实现别的功能的时候，往往需要编写新的代码，复用性较低。
面向对象就是<strong>让孩子吃瓜</strong>，而不管具体怎么实现，细节在对象内部。
面向对象：
优点：容易维护、方便复用；
缺点：不符合人的思维，编写时比较麻烦。</p> | 2020-07-01 22:00:26 |        1 | 3wN6t8S7p9qzPNGCCjgHkL |         0 |
+----+--------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------+----------+------------------------+-----------+
1 row in set (0.00 sec)

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109
```

虽然数据有点多，但是还是可以看出，content字段存的是原始markdown文本，而content_html存的就是转化成HTML格式的文本了。

之前是完成编辑并发布后就**重定向**到首页，还可以使用之前的方式，即使用**Ajax**来处理提交逻辑，front_apost.html修改如下：

```html
{% extends 'front/front_base.html' %}

{% block title %}
    帖子发布
{% endblock %}

{% block head %}
    <link rel="stylesheet" href="{{ url_for('static', filename='editormd/css/editormd.css') }}"/>
    <script src="{{ url_for('static', filename='front/js/jquery.min.js') }}"></script>
    <script src="{{ url_for('static', filename='editormd/editormd.js') }}"></script>
    <script src="{{ url_for('static', filename='front/js/front_apost.js') }}"></script>
{% endblock %}

{% block main_content %}
    <div class="main-container">
        <form action="" method="post">
            <input type="hidden" name="csrf_token" value="{{ csrf_token() }}">
            <div class="form-group">
                <div class="input-group">
                    <span class="input-group-addon">标题</span>
                    <input type="text" class="form-control" name="title">
                </div>
            </div>
            <div class="form-group">
                <div class="input-group">
                    <span class="input-group-addon">板块</span>
                    <select name="board_id" class="form-control">
                        {% for board in boards %}
                            <option value="{{ board.id }}">{{ board.name }}</option>
                        {% endfor %}


                        <option value="">Python数据分析</option>
                    </select>
                </div>
            </div>
            <div id="editor" class="form-group">
                <textarea name="content" id="TextContent"></textarea>
            </div>
            <div class="form-group">
                <button class="btn btn-danger" id="submit-btn">发布帖子</button>
            </div>
        </form>

        <script type="text/javascript">
            var testEditor;

            $(function () {
                testEditor = editormd("editor", {
                    width: "100%",
                    height: 640,
                    syncScrolling: "single",
                    path: "{{ url_for('static',filename='editormd/lib/') }}",
                    // 上传图片
                    imageUpload: true,
                    imageFormats: ["jpg", "jpeg", "gif", "png", "bmp", "webp"],
                    // 上传图片时指定调用后台的视图函数
                    imageUploadURL: "",

                });
            });
        </script>
    </div>
{% endblock %}
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364
```

front_base.html修改如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='front/css/front_base.css') }}">
    <link rel="stylesheet" href="{{ url_for('static', filename='front/css/front_index.css') }}">
    <script src="{{ url_for('static', filename='common/sweetalert/sweetalert.min.js') }}"></script>
    <script src="{{ url_for('static', filename='common/sweetalert/clalert.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='common/sweetalert/sweetalert.css') }}">
    <title>{% block title %}{% endblock %}</title>
    {% block head %}{% endblock %}
</head>
<body>
<nav class="navbar navbar-default">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                    data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">Python栏目</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
                <li class="active"><a href="/">首页 <span class="sr-only">(current)</span></a></li>
            </ul>
            <form class="navbar-form navbar-left">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">搜索</button>
            </form>
            <ul class="nav navbar-nav navbar-right">
                {% if g.front_user %}
                    <li class="dropdown">
                        <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
                           aria-expanded="false">{{ g.front_user.username }}<span class="caret"></span></a>
                        <ul class="dropdown-menu">
                            <li><a href="#">个人中心</a></li>
                            <li><a href="#">设置</a></li>
                            <li><a href="#">退出登录</a></li>
                        </ul>
                    </li>
                {% else %}
                    <li><a href="{{ url_for('front.signin') }}">登录</a></li>
                    <li><a href="{{ url_for('front.signup') }}">注册</a></li>
                {% endif %}


            </ul>
        </div><!-- /.navbar-collapse -->
    </div><!-- /.container-fluid -->
</nav>

{% block main_content %}

{% endblock %}


</body>
</html>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071
```

在static/front/js下新建front_apost.js如下：

```js
var clajax = {
    'get': function (args) {
        args['method'] = 'get';
        this.ajax(args);
    },
    'post': function (args) {
        args['method'] = 'post';
        this.ajax(args);
    },
    'ajax': function (args) {
        // 设置csrftoken
        this._ajaxSetup();
        $.ajax(args);
    },
    '_ajaxSetup': function () {
        $.ajaxSetup({
            'beforeSend': function (xhr, settings) {
                if (!/^(GET|HEAD|OPTIONS|TRACE)$/i.test(settings.type) && !this.crossDomain) {
                    var csrftoken = $('meta[name=csrf-token]').attr('content');
                    xhr.setRequestHeader("X-CSRFToken", csrftoken)
                }
            }
        });
    }
};

$(function () {


    $("#submit-btn").click(function (event) {
        event.preventDefault();
        var titleInput = $('input[name="title"]');
        var boardSelect = $("select[name='board_id']");
        var contentText = $("textarea[name='content']");

        var title = titleInput.val();
        var board_id = boardSelect.val();
        var content = contentText.val();

        clajax.post({
            'url': '/apost/',
            'data': {
                'title': title,
                'content': content,
                'board_id': board_id
            },
            'success': function (data) {
                if (data['code'] === 200) {
                    clalert.alertConfirm({
                        'msg': '恭喜！帖子发表成功！',
                        'cancelText': '回到首页',
                        'confirmText': '再发一篇',
                        'cancelCallback': function () {
                            window.location = '/';
                        },
                        'confirmCallback': function () {
                            window.location = '/apost/';
                        }
                    });
                } else {
                    clalert.alertInfo(data['message']);
                }
            }
        });
    });
});
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566
```

views.py修改如下：

```python
from flask import Blueprint, views, render_template, make_response, request, session, g, redirect, url_for
from io import BytesIO
from sqlalchemy import or_

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm, AddPostForm
from .models import FrontUser, PostModel
from .decorators import login_required
from apps.cms.models import BannerModel, BoardModel
from exts import db

front_bp = Blueprint('front', __name__)

from .hooks import before_request

@front_bp.route('/')
def index():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(BannerModel.priority.desc()).limit(4)
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    board_id = request.args.get('board_id', type=int, default=None)
    context = {
        'banners':banners,
        'boards':boards,
        'current_board':board_id
    }
    return render_template('front/front_index.html', **context)


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


class PostView(views.MethodView):
    decorators = [login_required]

    def get(self):
        boards = BoardModel.query.all()
        return render_template('front/front_apost.html', boards=boards)

    def post(self):
        form = AddPostForm(request.form)
        if form.validate():
            title = form.title.data
            content = form.content.data
            board_id = form.board_id.data
            board = BoardModel.query.get(board_id)
            if board and board.is_delete != 1:
                post = PostModel(title=title, content=content)
                post.board = board
                post.author = g.front_user
                db.session.add(post)
                db.session.commit()
                # return redirect(url_for('front.index'))
                return restful.success()
            else:
                return restful.params_error(message='没有该板块，请重新选择')
        else:
            return restful.params_error(form.get_error())


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html',return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))
front_bp.add_url_rule('/apost/', view_func=PostView.as_view('apost'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140
```

演示：
![flask front post rewrite redirect](https://img-blog.csdnimg.cn/20200702195938546.gif#pic_center)

显然，此时在编辑完后既可以重新编辑、再次发布，也可以直接跳到首页。

### 2.项目优化

#### （1）功能优化–Markdown编辑上传上传本地图片

要是你足够细心，可以看到在编辑Markdown时，如果需要上传图片，只能粘贴图片链接、而不能上传本地图片，要想上传本地图片，需要有较大的改动。

apps/common/views.py中增加对Markdown中上传图片到七牛云的处理的视图函数如下：

```python
from flask import Blueprint, request, jsonify
from qiniu import Auth
from datetime import datetime
import os

from utils import send_msg, restful
from utils.captcha import Captcha
from .forms import SMSCaptchaForm
from utils import clcache
from utils.random_captcha import get_random_captcha
from exts import qiniu_store, csrf
from config import QINIU_ACCESS_KEY, QINIU_SECRET_KEY

common_bp = Blueprint('common', __name__, url_prefix='/c')


@common_bp.route('/sms_captcha/', methods=['POST'])
def sms_captcha():
    form = SMSCaptchaForm(request.form)

    if form.validate():
        telephone = form.telephone.data
        captcha = Captcha.gene_text(4)
        print('发送的短信验证码：{}'.format(captcha))
        if send_msg.send_mobile_msg(telephone, captcha) == 0:
            clcache.save_captcha(telephone, captcha)
            return restful.success()
        else:
            return restful.params_error(message='发送失败')
    else:
        return restful.params_error(message='参数错误')


@common_bp.route('/uptoken/')
def uptoken():
    # 构建鉴权对象
    q = Auth(QINIU_ACCESS_KEY, QINIU_SECRET_KEY)
    # 要上传的空间
    bucket_name = 'bbs-images'
    # 生成上传 Token，可以指定过期时间等
    token = q.upload_token(bucket_name)

    return jsonify({'uptoken':token})


@common_bp.route('/edittoken/', methods=['POST'])
@csrf.exempt
def edittoken():
    data = request.files['editormd-image-file']
    if not data:
        res = {
            'success': 0,
            'message': 'upload failed'
        }
    else:
        ex = os.path.splitext(data.filename)[1]
        filename = datetime.now().strftime('%Y%m%d%H%M%S') + '-' + get_random_captcha(4) + ex
        file = data.stream.read()
        qiniu_store.save(file, filename)
        print(qiniu_store._base_url)
        print(filename)
        print(qiniu_store.url(filename))
        res = {
            'success': 1,
            'message': 'upload success',
            'url': qiniu_store.url(filename)
        }

    return jsonify(res)
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869
```

bbs.py修改如下：

```python
'''
前台 front
后台 cms
公有 common
'''

from flask import Flask

from exts import db, mail, qiniu_store, csrf
from apps.cms.views import cms_bp
from apps.front.views import front_bp
from apps.common.views import common_bp
import config


app = Flask(__name__) # type:Flask


app.config.from_object(config)
csrf.init_app(app)
db.init_app(app)
mail.init_app(app)
qiniu_store.init_app(app)

app.register_blueprint(cms_bp)
app.register_blueprint(front_bp)
app.register_blueprint(common_bp)


if __name__ == '__main__':
    app.run(debug=True)
12345678910111213141516171819202122232425262728293031
```

config.py完善如下：

```python
import os

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_bbs'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

TEMPLATE_AUTO_RELOAD = True
SQLALCHEMY_DATABASE_URI = DB_URL
SQLALCHEMY_TRACK_MODIFICATIONS = False
SQLALCHEMY_POOL_RECYCLE = 280
SQLALCHEMY_POOL_SIZE = 20

# 设置密钥
SECRET_KEY = os.urandom(15)

# 发送邮箱服务地址
MAIL_SERVER = 'smtp.qq.com'
# 邮箱端口，为587或465，为587时TLS设置为True，为465时SSL设置为True
MAIL_PORT = 587
MAIL_USE_TLS = True
# MAIL_USE_SSL = True # MAIL_PORT为465时设置此项
# 用户名可以为你的邮箱，需要自行添加
MAIL_USERNAME = '379869029@qq.com'
# 邮箱密码，不是邮箱账号密码，而是第三方客户端登录使用的授权码，需要自行获取
MAIL_PASSWORD = 'vrxiqciwjnbcxxxx'
# 发送者即你的邮箱，需要自行添加
MAIL_DEFAULT_SENDER = '379869029@qq.com'

# 云片APIKEY
YP_API = 'edf71361381f31b3957beda37f20xxxx'

# 七牛云存储配置
QINIU_ACCESS_KEY = '_PL7p4sTSlfAKl71hIkuG3F6y18681ZKkNJ3xxxx'
QINIU_SECRET_KEY = '4P09jodUqhhEqIOcaqT9daVFmKjCJI3l6gsAxxxx'
QINIU_BUCKET_NAME = 'bbs-images'
QINIU_BUCKET_DOMAIN = 'qcquu7fd2.bkt.clouddn.com'
12345678910111213141516171819202122232425262728293031323334353637383940
```

front_apost.html修改如下：

```html
{% extends 'front/front_base.html' %}

{% block title %}
    帖子发布
{% endblock %}

{% block head %}
    <script type="text/javascript">
        $(function () {
            editormd("editor", {
                height: 640,
                syncScrolling: "single",
                path: "{{ url_for('static',filename='editormd/lib/') }}",
                saveHTMLToTextarea : true,
                imageUpload: true,
                imageFormats: ["jpg", "jpeg", "gif", "png", "bmp", "webp"],
                imageUploadURL: "{{ url_for('common.edittoken') }}"
            });
        });
    </script>
    <link rel="stylesheet" href="{{ url_for('static', filename='editormd/css/editormd.css') }}"/>
    <script src="{{ url_for('static', filename='front/js/jquery.min.js') }}"></script>
    <script src="{{ url_for('static', filename='editormd/editormd.js') }}"></script>
    <script src="{{ url_for('static', filename='front/js/front_apost.js') }}"></script>
{% endblock %}

{% block main_content %}
    <div class="main-container">
        <form action="" method="post">
            <div class="form-group">
                <div class="input-group">
                    <span class="input-group-addon">标题</span>
                    <input type="text" class="form-control" name="title">
                </div>
            </div>
            <div class="form-group">
                <div class="input-group">
                    <span class="input-group-addon">板块</span>
                    <select name="board_id" class="form-control">
                        {% for board in boards %}
                            <option value="{{ board.id }}">{{ board.name }}</option>
                        {% endfor %}


                        <option value="">Python数据分析</option>
                    </select>
                </div>
            </div>
            <div id="editor" class="form-group">
                <textarea name="content" id="TextContent"></textarea>
            </div>
            <div class="form-group">
                <button class="btn btn-danger" id="submit-btn">发布帖子</button>
            </div>
        </form>

        <script type="text/javascript">
            var testEditor;

            $(function () {
                testEditor = editormd("editor", {
                    width: "100%",
                    height: 640,
                    syncScrolling: "single",
                    path: "{{ url_for('static',filename='editormd/lib/') }}",
                    // 上传图片
                    imageUpload: true,
                    imageFormats: ["jpg", "jpeg", "gif", "png", "bmp", "webp"],
                    // 上传图片时指定调用后台的视图函数
                    imageUploadURL: "",

                });
            });
        </script>
    </div>
{% endblock %}
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576
```

除此之外，还需要对editor.md中的文件进行修改，主要是为了**加入CSRF**，这样才能正常提交数据，static\editormd\plugins\image-dialog\image-dialog.js修改如下：

```js
/*!
 * Image (upload) dialog plugin for Editor.md
 *
 * @file        image-dialog.js
 * @author      pandao
 * @version     1.3.4
 * @updateTime  2015-06-09
 * {@link       https://github.com/pandao/editor.md}
 * @license     MIT
 */

(function () {

    var factory = function (exports) {

        var pluginName = "image-dialog";

        exports.fn.imageDialog = function () {

            var _this = this;
            var cm = this.cm;
            var lang = this.lang;
            var editor = this.editor;
            var settings = this.settings;
            var cursor = cm.getCursor();
            var selection = cm.getSelection();
            var imageLang = lang.dialog.image;
            var classPrefix = this.classPrefix;
            var iframeName = classPrefix + "image-iframe";
            var dialogName = classPrefix + pluginName, dialog;

            cm.focus();

            var loading = function (show) {
                var _loading = dialog.find("." + classPrefix + "dialog-mask");
                _loading[(show) ? "show" : "hide"]();
            };

            if (editor.find("." + dialogName).length < 1) {
                var guid = (new Date).getTime();
                var action = settings.imageUploadURL + (settings.imageUploadURL.indexOf("?") >= 0 ? "&" : "?") + "guid=" + guid;


                if (settings.crossDomainUpload) {
                    action += "&callback=" + settings.uploadCallbackURL + "&dialog_id=editormd-image-dialog-" + guid;
                }

                var csrfToken = $('meta[name="csrf-token"]').attr('content');
                var csrfField = "";
                if (csrfToken) {
                    csrfField = "<input type='hidden' name='csrf_token' value='" + csrfToken + "' />";
                }

                var dialogContent = ((settings.imageUpload) ? "<form action=\"" + action + "\" target=\"" + iframeName + "\" method=\"post\" enctype=\"multipart/form-data\" class=\"" + classPrefix + "form\">" : "<div class=\"" + classPrefix + "form\">") +
                    ((settings.imageUpload) ? "<iframe name=\"" + iframeName + "\" id=\"" + iframeName + "\" guid=\"" + guid + "\"></iframe>" : "") +
                    "<label>" + imageLang.url + "</label>" +
                    "<input type=\"text\" data-url />" + (function () {
                        return (settings.imageUpload) ? "<div class=\"" + classPrefix + "file-input\">" +
                            "<input type=\"file\" name=\"" + classPrefix + "image-file\" accept=\"image/*\" />" +
                            csrfField +
                            "<input type=\"submit\" value=\"" + imageLang.uploadButton + "\" />" +
                            "</div>" : "";
                    })() +
                    "<br/>" +
                    "<label>" + imageLang.alt + "</label>" +
                    "<input type=\"text\" value=\"" + selection + "\" data-alt />" +
                    "<br/>" +
                    "<label>" + imageLang.link + "</label>" +
                    "<input type=\"text\" value=\"http://\" data-link />" +
                    "<br/>" + csrfField +
                    ((settings.imageUpload) ? "</form>" : "</div>");

                //var imageFooterHTML = "<button class=\"" + classPrefix + "btn " + classPrefix + "image-manager-btn\" style=\"float:left;\">" + imageLang.managerButton + "</button>";

                dialog = this.createDialog({
                    title: imageLang.title,
                    width: (settings.imageUpload) ? 465 : 380,
                    height: 254,
                    name: dialogName,
                    content: dialogContent,
                    mask: settings.dialogShowMask,
                    drag: settings.dialogDraggable,
                    lockScreen: settings.dialogLockScreen,
                    maskStyle: {
                        opacity: settings.dialogMaskOpacity,
                        backgroundColor: settings.dialogMaskBgColor
                    },
                    buttons: {
                        enter: [lang.buttons.enter, function () {
                            var url = this.find("[data-url]").val();
                            var alt = this.find("[data-alt]").val();
                            var link = this.find("[data-link]").val();

                            if (url === "") {
                                alert(imageLang.imageURLEmpty);
                                return false;
                            }

                            var altAttr = (alt !== "") ? " \"" + alt + "\"" : "";

                            if (link === "" || link === "http://") {
                                cm.replaceSelection("![" + alt + "](" + url + altAttr + ")");
                            } else {
                                cm.replaceSelection("[![" + alt + "](" + url + altAttr + ")](" + link + altAttr + ")");
                            }

                            if (alt === "") {
                                cm.setCursor(cursor.line, cursor.ch + 2);
                            }

                            this.hide().lockScreen(false).hideMask();

                            //删除对话框
                            this.remove();

                            return false;
                        }],

                        cancel: [lang.buttons.cancel, function () {
                            this.hide().lockScreen(false).hideMask();

                            //删除对话框
                            this.remove();

                            return false;
                        }]
                    }
                });

                dialog.attr("id", classPrefix + "image-dialog-" + guid);

                if (!settings.imageUpload) {
                    return;
                }

                var fileInput = dialog.find("[name=\"" + classPrefix + "image-file\"]");

                fileInput.bind("change", function () {
                    var fileName = fileInput.val();
                    var isImage = new RegExp("(\\.(" + settings.imageFormats.join("|") + "))$", "i"); // /(\.(webp|jpg|jpeg|gif|bmp|png))$/

                    if (fileName === "") {
                        alert(imageLang.uploadFileEmpty);

                        return false;
                    }


                    if (!isImage.test(fileName)) {
                        alert(imageLang.formatNotAllowed + settings.imageFormats.join(", "));

                        return false;
                    }

                    loading(true);

                    var submitHandler = function () {

                        var uploadIframe = document.getElementById(iframeName);

                        uploadIframe.onload = function () {

                            loading(false);

                            var body = (uploadIframe.contentWindow ? uploadIframe.contentWindow : uploadIframe.contentDocument).document.body;
                            var json = (body.innerText) ? body.innerText : ((body.textContent) ? body.textContent : null);

                            json = (typeof JSON.parse !== "undefined") ? JSON.parse(json) : eval("(" + json + ")");

                            if (!settings.crossDomainUpload) {
                                if (json.success === 1) {
                                    dialog.find("[data-url]").val(json.url);
                                } else {
                                    alert(json.message);
                                }
                            }

                            return false;
                        };
                    };

                    dialog.find("[type=\"submit\"]").bind("click", submitHandler).trigger("click");
                });
            }

            dialog = editor.find("." + dialogName);
            dialog.find("[type=\"text\"]").val("");
            dialog.find("[type=\"file\"]").val("");
            dialog.find("[data-link]").val("http://");

            this.dialogShowMask(dialog);
            this.dialogLockScreen();
            dialog.show();

        };

    };

    // CommonJS/Node.js
    if (typeof require === "function" && typeof exports === "object" && typeof module === "object") {
        module.exports = factory;
    } else if (typeof define === "function")  // AMD/CMD/Sea.js
    {
        if (define.amd) { // for Require.js

            define(["editormd"], function (editormd) {
                factory(editormd);
            });

        } else { // for Sea.js
            define(function (require) {
                var editormd = require("./../../editormd");
                factory(editormd);
            });
        }
    } else {
        factory(window.editormd);
    }


})();

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206207208209210211212213214215216217218219220221222
```

进行测试如下：
![flask front post upload local image](https://img-blog.csdnimg.cn/20200702200029472.gif#pic_center)

显然，此时才能正常上传图片。

说明：
因为七牛云提供了在Flask中使用的专用SDK，即**Flask-QiniuStorage**，可以更方便地在Flask中实现上传文件到七牛云，因此在实现Editor.md上传图片到七牛云时也用的是该SDK，需要在**虚拟环境**中通过命令`pip install Flask-QiniuStorage`进行安装。

#### （2）代码优化–抽离Ajax代码

之前在实现前后台功能的时候，用到了Ajax，在很多JS文件中都有重复的代码，包括前后台，可以抽离出来，放到一个单独的文件中，在static/common中新建clajax.js如下：

```js
var clajax = {
    'get': function (args) {
        args['method'] = 'get';
        this.ajax(args);
    },
    'post': function (args) {
        args['method'] = 'post';
        this.ajax(args);
    },
    'ajax': function (args) {
        // 设置csrftoken
        this._ajaxSetup();
        $.ajax(args);
    },
    '_ajaxSetup': function () {
        $.ajaxSetup({
            'beforeSend': function (xhr, settings) {
                if (!/^(GET|HEAD|OPTIONS|TRACE)$/i.test(settings.type) && !this.crossDomain) {
                    var csrftoken = $('meta[name=csrf-token]').attr('content');
                    xhr.setRequestHeader("X-CSRFToken", csrftoken)
                }
            }
        });
    }
};
12345678910111213141516171819202122232425
```

其他JS文件，包括front_apost.js、front_signin.js、front_signup.js、banner.js、boards.js、posts.js和resetemail.js都去掉重复的代码部分，并且在前后台父模板中都导入clajax.js，cms_base.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <title>
        {% block title %}

        {% endblock %}
    </title>
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='cms/css/cms_base.css') }}">
    <script src="{{ url_for('static', filename='cms/js/cms_base.js') }}"></script>
    <script src="{{ url_for('static', filename='common/clajax.js') }}"></script>
    {# 提示框资源文件 #}
    <script src="{{ url_for('static', filename='common/sweetalert/sweetalert.min.js') }}"></script>
    <script src="{{ url_for('static', filename='common/sweetalert/clalert.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='common/sweetalert/sweetalert.css') }}">
    {% block head %}
    {% endblock %}
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top" role="navigation">
    <div class="container-fluid">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">熊熊乐园论坛CMS管理系统</a>
        </div>
        <div id="navbar" class="navbar-collapse collapse">
            <ul class="nav navbar-nav navbar-right">
                <li><a href="#">{{ g.cms_user.username }}<span>[{% block role %}{% endblock %}]</span></a></li>
                <li><a href="{{ url_for('cms.logout') }}">注销</a></li>
            </ul>
            <form class="navbar-form navbar-right">
                <input type="text" class="form-control" placeholder="查找...">
            </form>
        </div>
    </div>
</nav>

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3 col-md-2 sidebar">
            <ul class="nav-sidebar">
                <li class="unfold"><a href="{{ url_for('cms.index') }}">首页</a></li>
                <li class="profile-li">
                    <a href="#">个人中心<span></span></a>
                    <ul class="subnav">
                        <li><a href="{{ url_for('cms.profile') }}">个人信息</a></li>
                        <li><a href="{{ url_for('cms.resetpwd') }}">修改密码</a></li>
                        <li><a href="{{ url_for('cms.resetemail') }}">修改邮箱</a></li>
                    </ul>
                </li>

                {% set user = g.cms_user %}
                {% if user.has_permission(CMSPermission.POSTER) %}
                    <li class="nav-group post-manage"><a href="{{ url_for('cms.posts') }}">帖子管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.BANNER) %}
                    <li class="banner-manage"><a href="{{ url_for('cms.banners') }}">轮播图管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.COMMENTER) %}
                    <li class="comments-manage"><a href="{{ url_for('cms.comments') }}">评论管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.BOARDER) %}
                    <li class="board-manage"><a href="{{ url_for('cms.boards') }}">板块管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.FRONTUSER) %}
                    <li class="nav-group user-manage"><a href="{{ url_for('cms.fusers') }}">前台用户管理</a></li>
                {% endif %}
                {% if user.has_permission(CMSPermission.CMSUSER) %}
                    <li class="nav-group cmsuser-manage"><a href="{{ url_for('cms.cusers') }}">CMS用户管理</a></li>
                {% endif %}
                {% if user.is_developer %}
                    <li class="cmsrole-manage"><a href="{{ url_for('cms.croles') }}">CMS组管理</a></li>
                {% endif %}


            </ul>
        </div>
        <div class="col-sm-9 col-sm-offset-3 col-md-10 col-md-offset-2 main">
            <h1>{% block page_title %} {% endblock %}</h1>
            <div class="main_content">
                {% block content %} {% endblock %}
            </div>
        </div>
    </div>
</div>
</body>
</html>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283848586878889909192939495969798
```

front_base.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <script src="http://cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <link href="http://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script src="http://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='front/css/front_base.css') }}">
    <link rel="stylesheet" href="{{ url_for('static', filename='front/css/front_index.css') }}">
    <script src="{{ url_for('static', filename='common/sweetalert/sweetalert.min.js') }}"></script>
    <script src="{{ url_for('static', filename='common/sweetalert/clalert.js') }}"></script>
    <script src="{{ url_for('static', filename='common/clajax.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='common/sweetalert/sweetalert.css') }}">
    <title>{% block title %}{% endblock %}</title>
    {% block head %}{% endblock %}
</head>
<body>
<nav class="navbar navbar-default">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                    data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">Python栏目</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
                <li class="active"><a href="/">首页 <span class="sr-only">(current)</span></a></li>
            </ul>
            <form class="navbar-form navbar-left">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">搜索</button>
            </form>
            <ul class="nav navbar-nav navbar-right">
                {% if g.front_user %}
                    <li class="dropdown">
                        <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
                           aria-expanded="false">{{ g.front_user.username }}<span class="caret"></span></a>
                        <ul class="dropdown-menu">
                            <li><a href="#">个人中心</a></li>
                            <li><a href="#">设置</a></li>
                            <li><a href="#">退出登录</a></li>
                        </ul>
                    </li>
                {% else %}
                    <li><a href="{{ url_for('front.signin') }}">登录</a></li>
                    <li><a href="{{ url_for('front.signup') }}">注册</a></li>
                {% endif %}


            </ul>
        </div><!-- /.navbar-collapse -->
    </div><!-- /.container-fluid -->
</nav>

{% block main_content %}

{% endblock %}


</body>
</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172
```

修改完成后进行测试，功能正常。

## 五、CMS后台帖子管理页面搭建

文章发布之后还需要在前台显示出来，在后台进行管理。

front_index.html修改如下：

```html
{% extends 'front/front_base.html' %}

{% block title %}首页{% endblock %}

{% block main_content %}
    <div class="main-container">
        <div class="cl-container">
            <div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
                <!-- Indicators 指令 -->
                <ol class="carousel-indicators">
                    {% for banner in banners %}
                        {% if loop.first %}
                            <li data-target="#carousel-example-generic" data-slide-to="{{ loop.index0 }}"
                                class="active"></li>
                        {% else %}
                            <li data-target="#carousel-example-generic" data-slide-to="{{ loop.index0 }}"></li>
                        {% endif %}
                    {% endfor %}
                </ol>

                <!-- Wrapper for slides 轮播图 -->
                <div class="carousel-inner" role="listbox">
                    {% for banner in banners %}
                        {% if loop.first %}
                            <div class="item active">
                                <img src="{{ banner.image_url }}" alt="{{ banner.name }}" style="width: 400px; height: 240px">
                                <div class="carousel-caption">
                                    {{ banner.name }}
                                </div>
                            </div>
                        {% else %}
                            <div class="item">
                                <img src="{{ banner.image_url }}" alt="{{ banner.name }}" style="width: 400px; height: 240px">
                                <div class="carousel-caption">
                                    {{ banner.name }}
                                </div>
                            </div>
                        {% endif %}


                    {% endfor %}
                    ...
                </div>

                <!-- Controls 左右切换控制 -->
                <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                    <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
                    <span class="sr-only">Previous</span>
                </a>
                <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                    <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
                    <span class="sr-only">Next</span>
                </a>
            </div>

            <div class="post-group">
                <ul class="post-group-head">
                    <li class=""><a href="#">最新</a></li>
                    <li class=""><a href="#">精华帖子</a></li>
                    <li class=""><a href="#">点赞最多</a></li>
                    <li class=""><a href="#">评论最多</a></li>
                </ul>
                <ul class="post-list-group">
                    {% for post in posts %}
                    <li>
                        <div class="author-avatar-group">
                            <img src="#" alt="">
                        </div>
                        <div class="post-info-group">
                            <p class="post-title">
                                <a href="#">{{ post.title | truncate(20) }}</a>
                                <span class="label label-danger">精华帖</span>
                            </p>
                            <p class="post-info">
                                <span>作者: {{ post.author.username | truncate(6) }}</span>
                                <span>发表时间: {{ post.create_time }}</span>
                                <span>评论: 0</span>
                                <span>阅读: 0</span>
                            </p>
                        </div>
                    </li>
                    {% endfor %}

                </ul>
                <div style="text-align:center;">

                </div>
            </div>
        </div>

        <div class="sm-container">
            <div style="padding-bottom:10px;">
                <a href="{{ url_for('front.apost') }}" class="btn btn-warning btn-block">发布帖子</a>
            </div>
            <div class="list-group">
                <a href="/" class="list-group-item active">所有板块</a>
                {% for board in boards %}
                    {% if current_board == board.id %}
                        <a href="{{ url_for('front.index', board_id=board.id) }}"
                           class="list-group-item active">{{ board.name }}</a>
                    {% else %}
                        <a href="{{ url_for('front.index', board_id=board.id) }}"
                           class="list-group-item">{{ board.name }}</a>

                    {% endif %}
                {% endfor %}
            </div>
        </div>
    </div>
{% endblock %}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110
```

views.py完善如下：

```python
from flask import Blueprint, views, render_template, make_response, request, session, g, redirect, url_for
from io import BytesIO
from sqlalchemy import or_

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm, AddPostForm
from .models import FrontUser, PostModel
from .decorators import login_required
from apps.cms.models import BannerModel, BoardModel
from exts import db

front_bp = Blueprint('front', __name__)

from .hooks import before_request


@front_bp.route('/')
def index():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(
        BannerModel.priority.desc()).limit(4)
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    board_id = request.args.get('board_id', type=int, default=None)
    posts = PostModel.query.filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None)).all()
    context = {
        'banners': banners,
        'boards': boards,
        'current_board': board_id,
        'posts': posts
    }
    return render_template('front/front_index.html', **context)


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


class PostView(views.MethodView):
    decorators = [login_required]

    def get(self):
        boards = BoardModel.query.all()
        return render_template('front/front_apost.html', boards=boards)

    def post(self):
        form = AddPostForm(request.form)
        if form.validate():
            title = form.title.data
            content = form.content.data
            board_id = form.board_id.data
            board = BoardModel.query.get(board_id)
            if board and board.is_delete != 1:
                post = PostModel(title=title, content=content)
                post.board = board
                post.author = g.front_user
                db.session.add(post)
                db.session.commit()
                # return redirect(url_for('front.index'))
                return restful.success()
            else:
                return restful.params_error(message='没有该板块，请重新选择')
        else:
            return restful.params_error(form.get_error())


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html', return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))
front_bp.add_url_rule('/apost/', view_func=PostView.as_view('apost'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144
```

显示：
![flask front posts preview](https://img-blog.csdnimg.cn/20200702200120589.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

可以看到之前发布的贴子已经显示出来。

现进行后台帖子管理页面的搭建。

apps/cms/cms_posts.html修改如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台帖子管理
{% endblock %}

{% block head %}
    <script src="{{ url_for('static', filename='cms/js/posts.js') }}"></script>
{% endblock %}

{% block page_title %}
    熊熊帖子管理
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    <table class="table table-bordered">
        <thead>
        <tr>
            <th>标题</th>
            <th>发布时间</th>
            <th>板块</th>
            <th>作者</th>
            <th>操作</th>
        </tr>
        </thead>
        <tbody>
        {% for post in posts %}
            <tr data-id="1" data-highlight="">
                <td><a target="_blank" href="#">{{ post.title }}</a></td>
                <td>{{ post.create_time }}</td>
                <td>{{ post.board.name }}</td>
                <td>{{ post.author.username }}</td>
                <td>
                    <!-- <button class="btn btn-default btn-xs highlight-btn">取消加精</button> -->
                    <button class="btn btn-default btn-xs highlight-btn">加精</button>
                    <button class="btn btn-danger btn-xs">移除</button>
                </td>
            </tr>
        {% endfor %}
        </tbody>
    </table>
{% endblock %}
12345678910111213141516171819202122232425262728293031323334353637383940414243444546
```

在static/cms/js下创建posts.js如下：

```js
$(function () {
    $(".highlight-btn").click(function () {
        var self = $(this);
        var tr = self.parent().parent();
        var post_id = tr.attr("data-id");
        var highlight = parseInt(tr.attr("data-highlight"));
        var url = "";
        if (highlight) {
            url = "/cms/uhpost/";
        } else {
            url = "/cms/hpost/";
        }
        clajax.post({
            'url': url,
            'data': {
                'post_id': post_id
            },
            'success': function (data) {
                if (data['code'] === 200) {
                    clalert.alertSuccessToast('操作成功！');
                    setTimeout(function () {
                        window.location.reload();
                    }, 500);
                } else {
                    clalert.alertInfo(data['message']);
                }
            }
        });
    });
});


$(function () {
    $(".btn-xs").click(function () {
        var self = $(this);
        var tr = self.parent().parent();
        var post_id = tr.attr("data-id");
        clalert.alertConfirm({
            "msg": "您确定要删除这篇帖子吗？",
            'confirmCallback': function () {
                clajax.post({
                    'url': '/cms/dpost/',
                    'data': {
                        'post_id': post_id
                    },
                    'success': function (data) {
                        if (data['code'] === 200) {
                            window.location.reload();
                        } else {
                            clalert.alertInfo(data['message']);
                        }
                    }
                })
            }
        });


    });
});
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859
```

apps/cms/views.py修改如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message
from sqlalchemy import or_

from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm, AddBannerForm, UpdateBannerForm, AddBoardForm, UpdateBoardForm
from apps.cms.models import CMSUser, CMSPermission, BannerModel, BoardModel
from apps.front.models import PostModel
from exts import db, mail
from utils import restful, random_captcha, clcache
from .decorators import permission_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request

@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html', max_role=g.max_role)


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['cms_user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html", max_role=g.max_role)


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['cms_user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html', max_role=g.max_role)

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html', max_role=g.max_role)

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            email = form.email.data
            # 修改用户邮箱
            g.cms_user.email = email
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


@cms_bp.route('/posts/')
@permission_required(CMSPermission.POSTER)
def posts():
    posts = PostModel.query.filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None)).all()
    return render_template('cms/cms_posts.html', max_role=g.max_role, posts=posts)


@cms_bp.route('/banners/')
@permission_required(CMSPermission.BANNER)
def banners():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(BannerModel.priority.desc()).all()
    return render_template('cms/cms_banners.html', max_role=g.max_role, banners=banners)


@cms_bp.route('/abanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def abanner():
    form = AddBannerForm(request.form)
    if form.validate():
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel(name=name, image_url=image_url, link_url=link_url, priority=priority)
        db.session.add(banner)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message=form.get_error())


@cms_bp.route('/ubanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def ubanner():
    # 根据id修改数据
    form = UpdateBannerForm(request.form)
    if form.validate():
        banner_id = form.banner_id.data
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel.query.get(banner_id)
        if banner and banner.is_delete != 1:
            banner.name = name
            banner.image_url = image_url
            banner.link_url = link_url
            banner.priority = priority
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='轮播图不存在')
    else:
        return restful.params_error(form.get_error())


@cms_bp.route('/dbanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def dbanner():
    banner_id = request.form.get('banner_id')
    if not banner_id:
        return restful.params_error(message='数据请求有误')
    banner = BannerModel.query.get(banner_id)
    if banner and banner.is_delete != 1:
        banner.is_delete = 1
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message='轮播图不存在')


@cms_bp.route('/comments/')
@permission_required(CMSPermission.COMMENTER)
def comments():
    return render_template('cms/cms_comments.html', max_role=g.max_role)


@cms_bp.route('/boards/')
@permission_required(CMSPermission.BOARDER)
def boards():
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    return render_template('cms/cms_boards.html', max_role=g.max_role, boards=boards)


@cms_bp.route('/aboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def aboard():
    form = AddBoardForm(request.form)
    if form.validate():
        name = form.name.data
        board = BoardModel(name=name)
        db.session.add(board)
        db.session.commit()
        return restful.success()
    else:
        restful.params_error(message=form.get_error())


@cms_bp.route('/uboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def uboard():
    form = UpdateBoardForm(request.form)
    if form.validate():
        board_id = form.board_id.data
        name = form.name.data
        board = BoardModel.query.get(board_id)
        if board and board.is_delete != 1:
            board.name = name
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='没有该板块')
    else:
        return restful.params_error(form.get_error())


@cms_bp.route('/dboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def dboard():
    board_id = request.form.get('board_id')
    if not board_id:
        return restful.params_error(message='板块不存在')
    board = BoardModel.query.get(board_id)
    if board and board.is_delete != 1:
        board.is_delete = 1
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message='板块不存在')


@cms_bp.route('/fusers/')
@permission_required(CMSPermission.FRONTUSER)
def fusers():
    return render_template('cms/cms_fusers.html', max_role=g.max_role)


@cms_bp.route('/cusers/')
@permission_required(CMSPermission.CMSUSER)
def cusers():
    return render_template('cms/cms_cusers.html', max_role=g.max_role)


@cms_bp.route('/croles/')
@permission_required(CMSPermission.ADMINER)
def croles():
    return render_template('cms/cms_croles.html', max_role=g.max_role)


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206207208209210211212213214215216217218219220221222223224225226227228229230231232233234235236237238239240241242243244245246247248249250251252253254255256257258259260261262263264265266267268269270271272273
```

显示：
![flask cms post page](https://img-blog.csdnimg.cn/20200702200142114.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

此时后台已经搭建好管理帖子的页面。

# Python全栈（八）Flask项目实战之11.帖子管理和帖子分页

Github和Gitee代码**同步更新**：
https://github.com/PythonWebProject/Flask_BBS；
https://gitee.com/Python_Web_Project/Flask_BBS。

## 一、帖子详情页面

先进行帖子详情页面的设计和完善，这里评论等编辑使用百度的**富文本编辑器UEditor**，因为可以定制，实现比较简洁的界面，并且可以保留复制的内容的原始样式，运用到本项目的定制UEditor可直接点击https://download.csdn.net/download/CUFEECR/12572088下载后解压复制到static目录下即可。

templates/front下新建front_pdetail.html如下：

```html
{% extends 'front/front_base.html' %}

{% block title %}
    帖子详情页
{% endblock %}

{% block head %}
    <script src="{{ url_for('static', filename='ueditor/ueditor.config.js') }}"></script>
    <script src="{{ url_for('static', filename='ueditor/ueditor.all.min.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='front/css/front_pdetail.css') }}">
    <script src="{{ url_for('static', filename='front/js/front_pdetail.js') }}"></script>
{% endblock %}

{% block main_content %}
    <div class="main-container">
        <div class="cl-container">
            <div class="post-container">
                <h2>{{ post.title }}</h2>
                <p class="post-info-group">
                    <span>发表时间：{{ post.create_time }}</span>
                    <span>作者：{{ post.author.username }}</span>
                    <span>所属板块：{{ post.board.name }}</span>
                    <span>阅读数：0</span>
                    <span>评论数：0</span>
                </p>
                <article class="post-content" id="post-content" data-id="">
                    {{ post.content_html | safe }}
                </article>
            </div>
            <div class="comment-group">
                <h3>评论列表</h3>
                <ul class="comment-list-group">

                    <li>
                        <div class="avatar-group">
                            <img src="{{ url_for('static', filename='common/images/logo.png') }}"
                                 alt="">
                        </div>
                        <div class="comment-content">
                            <p class="author-info">
                                <span>Corley</span>
                                <span>2020-07-03</span>
                            </p>
                            <p class="comment-txt">
                                评论内容
                            </p>
                        </div>
                    </li>
                </ul>
            </div>
            <div class="add-comment-group">
                <h3>发表评论</h3>
                <script id="editor" type="text/plain" style="height:100px;"></script>
                <!--        <textarea name="content" id="comment" cols="99" rows="10"></textarea>-->
                <div class="comment-btn-group">
                    <button class="btn btn-primary" id="comment-btn">发表评论</button>
                </div>
            </div>
        </div>

        <div class="sm-container"></div>
    </div>
{% endblock %}



123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566
```

static/front/css下新建样式文件front_pdetail.css如下：

```css
.post-container {
    border: 1px solid #e6e6e6;
    padding: 10px;
}

.post-info-group {
    width: 100%;
    font-size: 12px;
    color: #8c8c8c;
    border-bottom: 1px solid #e6e6e6;
    margin-top: 20px;
    padding-bottom: 10px;
}

.post-info-group span {
    margin-right: 20px;
}

.post-content {
    margin-top: 20px;
}

.post-content img {
    max-width: 100%;
}

.comment-group {
    margin-top: 20px;
    border: 1px solid #e8e8e8;
    padding: 10px;
}

.add-comment-group {
    margin-top: 20px;
    padding: 10px;
    border: 1px solid #e8e8e8;
}

.add-comment-group h3 {
    margin-bottom: 10px;
}

.comment-btn-group {
    margin-top: 10px;
    text-align: right;
}

.comment-list-group li {
    overflow: hidden;
    padding: 10px 0;
    border-bottom: 1px solid #e8e8e8;
}

.avatar-group {
    float: left;
}

.avatar-group img {
    width: 50px;
    height: 50px;
    border-radius: 50%;
}

.comment-content {
    float: left;
    margin-left: 10px;
}

.comment-content .author-info {
    font-size: 12px;
    color: #8c8c8c;
}

.author-info span {
    margin-right: 10px;
}

.comment-content .comment-txt {
    margin-top: 10px;
}
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980
```

static/front/js下新建front_pdetail.js如下：

```js
$(function () {
    $("#comment-btn").click(function (event) {
        event.preventDefault();

        var content = $("#comment").val();
        var post_id = $("#post-content").attr("data-id");
        clajax.post({
            'url': '/acomment/',
            'data': {
                'content': content,
                'post_id': post_id
            },
            'success': function (data) {
                if (data['code'] === 200) {
                    window.location.reload();
                } else {
                    clalert.alertInfo(data['message']);
                }
            }
        });
    });
});

//初始化UEditor
$(function () {
    var ue = UE.getEditor('editor', {
        'serverUrl': '/ueditor/upload',
        'toolbars': [
            ['fullscreen', 'source', 'undo', 'redo'],
            ['bold', 'italic', 'underline', 'fontborder', 'strikethrough', 'superscript', 'subscript', 'removeformat', 'formatmatch', 'autotypeset', 'blockquote', 'pasteplain', '|', 'forecolor', 'backcolor', 'insertorderedlist', 'insertunorderedlist', 'selectall', 'cleardoc']
        ]
    });
    window.ue = ue;
})
12345678910111213141516171819202122232425262728293031323334
```

apps/front/views.py完善如下：

```python
from flask import Blueprint, views, render_template, make_response, request, session, g, redirect, url_for
from io import BytesIO
from sqlalchemy import or_

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm, AddPostForm
from .models import FrontUser, PostModel
from .decorators import login_required
from apps.cms.models import BannerModel, BoardModel
from exts import db

front_bp = Blueprint('front', __name__)

from .hooks import before_request


@front_bp.route('/')
def index():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(
        BannerModel.priority.desc()).limit(4)
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    board_id = request.args.get('board_id', type=int, default=None)
    posts = PostModel.query.filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None)).all()
    context = {
        'banners': banners,
        'boards': boards,
        'current_board': board_id,
        'posts': posts
    }
    return render_template('front/front_index.html', **context)


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


class PostView(views.MethodView):
    decorators = [login_required]

    def get(self):
        boards = BoardModel.query.all()
        return render_template('front/front_apost.html', boards=boards)

    def post(self):
        form = AddPostForm(request.form)
        if form.validate():
            title = form.title.data
            content = form.content.data
            board_id = form.board_id.data
            board = BoardModel.query.get(board_id)
            if board and board.is_delete != 1:
                post = PostModel(title=title, content=content)
                post.board = board
                post.author = g.front_user
                db.session.add(post)
                db.session.commit()
                # return redirect(url_for('front.index'))
                return restful.success()
            else:
                return restful.params_error(message='没有该板块，请重新选择')
        else:
            return restful.params_error(form.get_error())


@front_bp.route('/p/<post_id>/')
def post_detail(post_id):
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        return render_template('front/front_pdetail.html', post=post)
    else:
        return restful.params_error(message='文章不存在')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html', return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))
front_bp.add_url_rule('/apost/', view_func=PostView.as_view('apost'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153
```

测试如下：
![flask front post detail preview](https://img-blog.csdnimg.cn/20200704091532670.gif#pic_center)

显然，文章详情基本页面已经搭建好，但是这是通过手动输入链接的方式访问文章详情的，这在实际中是很不方便的，我们可以直接通过点击文章链接就自动跳转到文章详情页，front_index.html修改如下：

```html
{% extends 'front/front_base.html' %}

{% block title %}首页{% endblock %}

{% block main_content %}
    <div class="main-container">
        <div class="cl-container">
            <div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
                <!-- Indicators 指令 -->
                <ol class="carousel-indicators">
                    {% for banner in banners %}
                        {% if loop.first %}
                            <li data-target="#carousel-example-generic" data-slide-to="{{ loop.index0 }}"
                                class="active"></li>
                        {% else %}
                            <li data-target="#carousel-example-generic" data-slide-to="{{ loop.index0 }}"></li>
                        {% endif %}
                    {% endfor %}
                </ol>

                <!-- Wrapper for slides 轮播图 -->
                <div class="carousel-inner" role="listbox">
                    {% for banner in banners %}
                        {% if loop.first %}
                            <div class="item active">
                                <img src="{{ banner.image_url }}" alt="{{ banner.name }}" style="width: 400px; height: 240px">
                                <div class="carousel-caption">
                                    {{ banner.name }}
                                </div>
                            </div>
                        {% else %}
                            <div class="item">
                                <img src="{{ banner.image_url }}" alt="{{ banner.name }}" style="width: 400px; height: 240px">
                                <div class="carousel-caption">
                                    {{ banner.name }}
                                </div>
                            </div>
                        {% endif %}


                    {% endfor %}
                    ...
                </div>

                <!-- Controls 左右切换控制 -->
                <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                    <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
                    <span class="sr-only">Previous</span>
                </a>
                <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                    <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
                    <span class="sr-only">Next</span>
                </a>
            </div>

            <div class="post-group">
                <ul class="post-group-head">
                    <li class=""><a href="#">最新</a></li>
                    <li class=""><a href="#">精华帖子</a></li>
                    <li class=""><a href="#">点赞最多</a></li>
                    <li class=""><a href="#">评论最多</a></li>
                </ul>
                <ul class="post-list-group">
                    {% for post in posts %}
                    <li>
                        <div class="author-avatar-group">
                            <img src="#" alt="">
                        </div>
                        <div class="post-info-group">
                            <p class="post-title">
                                <a href="{{ url_for('front.post_detail', post_id=post.id) }}" target="_blank">{{ post.title | truncate(20) }}</a>
                                <span class="label label-danger">精华帖</span>
                            </p>
                            <p class="post-info">
                                <span>作者：{{ post.author.username | truncate(6) }}</span>
                                <span>发表时间：{{ post.create_time }}</span>
                                <span>评论：0</span>
                                <span>阅读：0</span>
                            </p>
                        </div>
                    </li>
                    {% endfor %}

                </ul>
                <div style="text-align:center;">

                </div>
            </div>
        </div>

        <div class="sm-container">
            <div style="padding-bottom:10px;">
                <a href="{{ url_for('front.apost') }}" class="btn btn-warning btn-block">发布帖子</a>
            </div>
            <div class="list-group">
                <a href="/" class="list-group-item active">所有板块</a>
                {% for board in boards %}
                    {% if current_board == board.id %}
                        <a href="{{ url_for('front.index', board_id=board.id) }}"
                           class="list-group-item active">{{ board.name }}</a>
                    {% else %}
                        <a href="{{ url_for('front.index', board_id=board.id) }}"
                           class="list-group-item">{{ board.name }}</a>

                    {% endif %}
                {% endfor %}
            </div>
        </div>
    </div>
{% endblock %}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110
```

再进行测试：
![flask front post detail link click](https://img-blog.csdnimg.cn/20200704091555603.gif#pic_center)

显然，此时可以进行正常的点击链接访问。

## 二、CMS后台帖子管理

现进行后台帖子管理功能的实现，主要有两个功能：**加精和移除**。

加精可以在post表中增加一个字段，也可以直接新建一个表用于专门保存精华帖，另外前台中也需要单独显示精华帖，因此建议新建一个表，即创建一个精华帖的模型。

apps/cms/models.py如下：

```python
from datetime import datetime
from werkzeug.security import generate_password_hash, check_password_hash
from exts import db


class CMSUser(db.Model):
    '''后台管理员用户类'''
    __tablename__ = 'cms_user'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    username = db.Column(db.String(30), nullable=False)
    _password = db.Column(db.String(100), nullable=False)
    email = db.Column(db.String(50), nullable=False, unique=True)
    join_time = db.Column(db.DateTime, default=datetime.now())

    def __init__(self, username, password, email):
        self.username = username
        self.password = password
        self.email = email

    @property
    def password(self):
        '''获取密码'''
        return self._password

    @password.setter
    def password(self, raw_password):
        '''设置密码'''
        self._password = generate_password_hash(raw_password)

    def check_password(self, raw_password):
        '''验证密码是否正确'''
        result = check_password_hash(self.password, raw_password)
        return result

    @property
    def permissions(self):
        '''判断用户权限'''
        if not self.roles:
            return 0

        all_permissions = 0
        for role in self.roles:
            permissions = role.permissions
            all_permissions |= permissions
        return all_permissions

    def has_permission(self, permission):
        '''判断当前用户是否有某个权限'''
        all_permissions = self.permissions
        result = all_permissions & permission == permission  # 如果当前用户有某个权限，则他的全部权限与该权限按位与的结果等于该权限本身
        return result

    @property
    def is_developer(self):
        '''判断当前用户是否是开发人员'''
        return self.has_permission(CMSPermission.ALL_PERMISSION)


class CMSPermission(object):
    # 255二进制表示所有权限
    ALL_PERMISSION = 0b11111111

    # 访问权限
    VISITOR = 0b00000001

    # 管理帖子权限
    POSTER = 0b00000010

    # 管理轮播图
    BANNER = 0b00000100

    # 管理评论权限
    COMMENTER = 0b00001000

    # 管理板块
    BOARDER = 0b00010000

    # 管理前台用户
    FRONTUSER = 0b00100000

    # 管理前台用户
    CMSUSER = 0b01000000

    # 管理前台用户
    ADMINER = 0b10000000


cms_role_user = db.Table(
    'cms_role_user',
    db.Column('cms_role_id', db.Integer, db.ForeignKey('cms_role.id'), primary_key=True),
    db.Column('cms_user_id', db.Integer, db.ForeignKey('cms_user.id'), primary_key=True)
)


class CMSRole(db.Model):
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(50), nullable=False)
    desc = db.Column(db.String(200), nullable=False)
    create_time = db.Column(db.DateTime, default=datetime.now())
    permissions = db.Column(db.Integer, default=CMSPermission.VISITOR)

    users = db.relationship('CMSUser', secondary=cms_role_user, backref='roles')


class BannerModel(db.Model):
    __tablename__ = 'banner'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(30), nullable=False)
    # 图片链接
    image_url = db.Column(db.String(255), nullable=False)
    # 跳转链接
    link_url = db.Column(db.String(255), nullable=False)
    # 优先级
    priority = db.Column(db.Integer, default=0)
    create_time = db.Column(db.DateTime, default=datetime.now)
    # 1表示被删除，0表示未删除，默认为0
    is_delete = db.Column(db.Integer, default=0)


class BoardModel(db.Model):
    __tablename__ = 'cms_board'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    name = db.Column(db.String(50), nullable=False)
    create_time = db.Column(db.DateTime, default=datetime.now)
    # 1表示被删除，0表示未删除，默认为0
    is_delete = db.Column(db.Integer, default=0)


class HighllightPostModel(db.Model):
    __tablename__ = 'highlight_post'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    post_id = db.Column(db.Integer, db.ForeignKey('post.id'))
    create_time = db.Column(db.DateTime, default=datetime.now)
    
    post = db.relationship('PostModel', backref='highlight')
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135
```

manage.py修改如下：

```python
from flask_script import Manager
from bbs import app
from flask_migrate import Migrate, MigrateCommand
from exts import db
from apps.cms.models import CMSUser, CMSRole, CMSPermission, BannerModel, BoardModel, HighlightPostModel
from apps.front.models import FrontUser, PostModel

manager = Manager(app)
Migrate(app, db)

manager.add_command('db', MigrateCommand)


@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
@manager.option('-e', '--email', dest='email')
def create_cms_user(username, password, email):
    user = CMSUser(username=username, password=password, email=email)
    db.session.add(user)
    db.session.commit()
    print('CMS用户添加成功')


@manager.command
def create_role():
    # 访问者
    visitor = CMSRole(name='访问者', desc='只能查看数据，不能修改数据')
    visitor.permissions = CMSPermission.VISITOR  # 也可以省去，因为默认权限就是VISITOR

    # 运营者
    operator = CMSRole(name='运营', desc='管理帖子，管理评论，管理前台和后台用户，管理轮播图')
    # 有多个权限时，使用或运算表示
    operator.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER | CMSPermission.BANNER
    # 管理员
    admin = CMSRole(name='管理员', desc='拥有本系统大部分权限')
    admin.permissions = CMSPermission.VISITOR | CMSPermission.POSTER | CMSPermission.CMSUSER | CMSPermission.COMMENTER | CMSPermission.FRONTUSER | CMSPermission.BOARDER | CMSPermission.BANNER

    # 开发人员
    developer = CMSRole(name='开发者', desc='拥有所有权限')
    developer.permissions = CMSPermission.ALL_PERMISSION

    db.session.add_all([visitor, operator, admin, developer])
    db.session.commit()
    print('角色添加成功')


@manager.command
def test_permission():
    user = CMSUser.query.first()
    if user.has_permission(CMSPermission.VISITOR):
        print('该用户有使用者权限')
    else:
        print('该用户没有使用者权限')


@manager.option('-e', '--email', dest='email')
@manager.option('-n', '--name', dest='name')
def add_user_to_role(email, name):
    user = CMSUser.query.filter_by(email=email).first()
    if user:
        role = CMSRole.query.filter_by(name=name).first()
        if role:
            role.users.append(user)
            db.session.commit()
            print('用户添加到角色添加成功')
        else:
            print('角色不存在')
    else:
        print('邮箱不存在')


@manager.option('-t', '--telephone', dest='telephone')
@manager.option('-u', '--username', dest='username')
@manager.option('-p', '--password', dest='password')
def create_front_user(telephone, username, password):
    user = FrontUser(telephone=telephone, username=username, password=password)
    db.session.add(user)
    db.session.commit()
    print('前台用户添加成功')


if __name__ == '__main__':
    manager.run()

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384
```

在命令行中进行**数据库映射**即建表成功，在数据库中查询可以看到新建的表highlight_post。

完善apps/cms/views.py如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message
from sqlalchemy import or_

from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm, AddBannerForm, UpdateBannerForm, AddBoardForm, UpdateBoardForm
from apps.cms.models import CMSUser, CMSPermission, BannerModel, BoardModel, HighlightPostModel
from apps.front.models import PostModel
from exts import db, mail
from utils import restful, random_captcha, clcache
from .decorators import permission_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request

@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html', max_role=g.max_role)


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['cms_user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html", max_role=g.max_role)


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['cms_user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html', max_role=g.max_role)

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html', max_role=g.max_role)

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            email = form.email.data
            # 修改用户邮箱
            g.cms_user.email = email
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


@cms_bp.route('/posts/')
@permission_required(CMSPermission.POSTER)
def posts():
    posts = PostModel.query.filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None)).all()
    return render_template('cms/cms_posts.html', max_role=g.max_role, posts=posts)


@cms_bp.route('/hpost/', methods=['POST'])
@permission_required(CMSPermission.POSTER)
def hpost():
    post_id = request.form.get('post_id')
    if not post_id:
        return restful.params_error(message='数据提交有误')
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        highlight = HighlightPostModel()
        highlight.post = post
        db.session.add(highlight)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error('该文章去火星啦😀')


@cms_bp.route('/uhpost/', methods=['POST'])
@permission_required(CMSPermission.POSTER)
def uhpost():
    post_id = request.form.get('post_id')
    if not post_id:
        return restful.params_error(message='数据提交有误')
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        highlight = HighlightPostModel.query.filter_by(post_id=post_id).first()
        db.session.delete(highlight)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error('该文章去火星啦😀')


@cms_bp.route('/banners/')
@permission_required(CMSPermission.BANNER)
def banners():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(BannerModel.priority.desc()).all()
    return render_template('cms/cms_banners.html', max_role=g.max_role, banners=banners)


@cms_bp.route('/abanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def abanner():
    form = AddBannerForm(request.form)
    if form.validate():
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel(name=name, image_url=image_url, link_url=link_url, priority=priority)
        db.session.add(banner)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message=form.get_error())


@cms_bp.route('/ubanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def ubanner():
    # 根据id修改数据
    form = UpdateBannerForm(request.form)
    if form.validate():
        banner_id = form.banner_id.data
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel.query.get(banner_id)
        if banner and banner.is_delete != 1:
            banner.name = name
            banner.image_url = image_url
            banner.link_url = link_url
            banner.priority = priority
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='轮播图不存在')
    else:
        return restful.params_error(form.get_error())


@cms_bp.route('/dbanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def dbanner():
    banner_id = request.form.get('banner_id')
    if not banner_id:
        return restful.params_error(message='数据请求有误')
    banner = BannerModel.query.get(banner_id)
    if banner and banner.is_delete != 1:
        banner.is_delete = 1
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message='轮播图不存在')


@cms_bp.route('/comments/')
@permission_required(CMSPermission.COMMENTER)
def comments():
    return render_template('cms/cms_comments.html', max_role=g.max_role)


@cms_bp.route('/boards/')
@permission_required(CMSPermission.BOARDER)
def boards():
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    return render_template('cms/cms_boards.html', max_role=g.max_role, boards=boards)


@cms_bp.route('/aboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def aboard():
    form = AddBoardForm(request.form)
    if form.validate():
        name = form.name.data
        board = BoardModel(name=name)
        db.session.add(board)
        db.session.commit()
        return restful.success()
    else:
        restful.params_error(message=form.get_error())


@cms_bp.route('/uboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def uboard():
    form = UpdateBoardForm(request.form)
    if form.validate():
        board_id = form.board_id.data
        name = form.name.data
        board = BoardModel.query.get(board_id)
        if board and board.is_delete != 1:
            board.name = name
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='没有该板块')
    else:
        return restful.params_error(form.get_error())


@cms_bp.route('/dboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def dboard():
    board_id = request.form.get('board_id')
    if not board_id:
        return restful.params_error(message='板块不存在')
    board = BoardModel.query.get(board_id)
    if board and board.is_delete != 1:
        board.is_delete = 1
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message='板块不存在')


@cms_bp.route('/fusers/')
@permission_required(CMSPermission.FRONTUSER)
def fusers():
    return render_template('cms/cms_fusers.html', max_role=g.max_role)


@cms_bp.route('/cusers/')
@permission_required(CMSPermission.CMSUSER)
def cusers():
    return render_template('cms/cms_cusers.html', max_role=g.max_role)


@cms_bp.route('/croles/')
@permission_required(CMSPermission.ADMINER)
def croles():
    return render_template('cms/cms_croles.html', max_role=g.max_role)


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206207208209210211212213214215216217218219220221222223224225226227228229230231232233234235236237238239240241242243244245246247248249250251252253254255256257258259260261262263264265266267268269270271272273274275276277278279280281282283284285286287288289290291292293294295296297298299300301302303304305306
```

cms_posts.html修改如下：

```html
{% extends 'cms/cms_base.html' %}

{% block title %}
    熊熊论坛后台帖子管理
{% endblock %}

{% block head %}
    <script src="{{ url_for('static', filename='cms/js/posts.js') }}"></script>
{% endblock %}

{% block page_title %}
    熊熊帖子管理
{% endblock %}

{% block role %}
    {{ max_role }}
{% endblock %}

{% block content %}
    <table class="table table-bordered">
        <thead>
        <tr>
            <th>标题</th>
            <th>发布时间</th>
            <th>板块</th>
            <th>作者</th>
            <th>操作</th>
        </tr>
        </thead>
        <tbody>
        {% for post in posts %}
            <tr data-id="{{ post.id }}" data-highlight="{{ 1 if post.highlight else 0 }}">
                <td><a target="_blank" href="{{ url_for('front.post_detail', post_id=post.id) }}">{{ post.title }}</a></td>
                <td>{{ post.create_time }}</td>
                <td>{{ post.board.name }}</td>
                <td>{{ post.author.username }}</td>
                <td>
                    {% if post.highlight %}
                        <button class="btn btn-default btn-xs highlight-btn" style="width: 65px">取消加精</button>
                    {% else %}
                        <button class="btn btn-default btn-xs highlight-btn" style="width: 65px">加精</button>
                    {% endif %}

                    <button class="btn btn-danger btn-xs">移除</button>
                </td>
            </tr>
        {% endfor %}
        </tbody>
    </table>
{% endblock %}
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
```

进行测试如下：
![flask cms post highlight](https://img-blog.csdnimg.cn/20200704091741783.gif#pic_center)

显然，可以在加精与取消加精之间正常切换。

接下来实现移除（即删除）帖子的功能，这里并非真正删除，而是将is_delete字段设为1。

views.py修改如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message
from sqlalchemy import or_

from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm, AddBannerForm, UpdateBannerForm, AddBoardForm, UpdateBoardForm
from apps.cms.models import CMSUser, CMSPermission, BannerModel, BoardModel, HighlightPostModel
from apps.front.models import PostModel
from exts import db, mail
from utils import restful, random_captcha, clcache
from .decorators import permission_required  # .代表当前路径

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request

@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html', max_role=g.max_role)


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['cms_user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html", max_role=g.max_role)


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['cms_user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html', max_role=g.max_role)

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html', max_role=g.max_role)

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            email = form.email.data
            # 修改用户邮箱
            g.cms_user.email = email
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            message = Message('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
            mail.send(message)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


@cms_bp.route('/posts/')
@permission_required(CMSPermission.POSTER)
def posts():
    posts = PostModel.query.filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None)).all()
    return render_template('cms/cms_posts.html', max_role=g.max_role, posts=posts)


@cms_bp.route('/hpost/', methods=['POST'])
@permission_required(CMSPermission.POSTER)
def hpost():
    post_id = request.form.get('post_id')
    if not post_id:
        return restful.params_error(message='数据提交有误')
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        highlight = HighlightPostModel()
        highlight.post = post
        db.session.add(highlight)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error('该文章去火星啦😀')


@cms_bp.route('/uhpost/', methods=['POST'])
@permission_required(CMSPermission.POSTER)
def uhpost():
    post_id = request.form.get('post_id')
    if not post_id:
        return restful.params_error(message='数据提交有误')
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        highlight = HighlightPostModel.query.filter_by(post_id=post_id).first()
        db.session.delete(highlight)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error('该文章去火星啦😀')


@cms_bp.route('/dpost/', methods=['POST'])
@permission_required(CMSPermission.POSTER)
def dpost():
    post_id = request.form.get('post_id')
    if not post_id:
        return restful.params_error(message='数据提交有误')
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        post.is_delete = 1
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error('该文章去火星啦😀')


@cms_bp.route('/banners/')
@permission_required(CMSPermission.BANNER)
def banners():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(BannerModel.priority.desc()).all()
    return render_template('cms/cms_banners.html', max_role=g.max_role, banners=banners)


@cms_bp.route('/abanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def abanner():
    form = AddBannerForm(request.form)
    if form.validate():
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel(name=name, image_url=image_url, link_url=link_url, priority=priority)
        db.session.add(banner)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message=form.get_error())


@cms_bp.route('/ubanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def ubanner():
    # 根据id修改数据
    form = UpdateBannerForm(request.form)
    if form.validate():
        banner_id = form.banner_id.data
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel.query.get(banner_id)
        if banner and banner.is_delete != 1:
            banner.name = name
            banner.image_url = image_url
            banner.link_url = link_url
            banner.priority = priority
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='轮播图不存在')
    else:
        return restful.params_error(form.get_error())


@cms_bp.route('/dbanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def dbanner():
    banner_id = request.form.get('banner_id')
    if not banner_id:
        return restful.params_error(message='数据请求有误')
    banner = BannerModel.query.get(banner_id)
    if banner and banner.is_delete != 1:
        banner.is_delete = 1
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message='轮播图不存在')


@cms_bp.route('/comments/')
@permission_required(CMSPermission.COMMENTER)
def comments():
    return render_template('cms/cms_comments.html', max_role=g.max_role)


@cms_bp.route('/boards/')
@permission_required(CMSPermission.BOARDER)
def boards():
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    return render_template('cms/cms_boards.html', max_role=g.max_role, boards=boards)


@cms_bp.route('/aboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def aboard():
    form = AddBoardForm(request.form)
    if form.validate():
        name = form.name.data
        board = BoardModel(name=name)
        db.session.add(board)
        db.session.commit()
        return restful.success()
    else:
        restful.params_error(message=form.get_error())


@cms_bp.route('/uboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def uboard():
    form = UpdateBoardForm(request.form)
    if form.validate():
        board_id = form.board_id.data
        name = form.name.data
        board = BoardModel.query.get(board_id)
        if board and board.is_delete != 1:
            board.name = name
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='没有该板块')
    else:
        return restful.params_error(form.get_error())


@cms_bp.route('/dboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def dboard():
    board_id = request.form.get('board_id')
    if not board_id:
        return restful.params_error(message='板块不存在')
    board = BoardModel.query.get(board_id)
    if board and board.is_delete != 1:
        board.is_delete = 1
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message='板块不存在')


@cms_bp.route('/fusers/')
@permission_required(CMSPermission.FRONTUSER)
def fusers():
    return render_template('cms/cms_fusers.html', max_role=g.max_role)


@cms_bp.route('/cusers/')
@permission_required(CMSPermission.CMSUSER)
def cusers():
    return render_template('cms/cms_cusers.html', max_role=g.max_role)


@cms_bp.route('/croles/')
@permission_required(CMSPermission.ADMINER)
def croles():
    return render_template('cms/cms_croles.html', max_role=g.max_role)


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206207208209210211212213214215216217218219220221222223224225226227228229230231232233234235236237238239240241242243244245246247248249250251252253254255256257258259260261262263264265266267268269270271272273274275276277278279280281282283284285286287288289290291292293294295296297298299300301302303304305306307308309310311312313314315316317318319320321
```

进行测试：
![flask cms post delete](https://img-blog.csdnimg.cn/20200704091813954.gif#pic_center)

显然，已经实现删除帖子的功能。

## 三、前台帖子发布评论

先创建评论的模型，apps/front/models.py修改如下：

```python
import shortuuid
import enum
from werkzeug.security import generate_password_hash, check_password_hash
from datetime import datetime
from markdown import markdown
import bleach

from exts import db


class GenderEnum(enum.Enum):
    MALE = 1
    FAMALE = 2
    SECRET = 3
    UNKNOWN = 4


class FrontUser(db.Model):
    __tablename__ = 'front_user'
    id = db.Column(db.String(40), primary_key=True, default=shortuuid.uuid)
    telephone = db.Column(db.String(11), nullable=False, unique=True)
    username = db.Column(db.String(50), nullable=False)
    _password = db.Column(db.String(100), nullable=False)
    email = db.Column(db.String(100), unique=False)
    realname = db.Column(db.String(30))
    avatar = db.Column(db.String(80))
    signature = db.Column(db.String(200))
    gender = db.Column(db.Enum(GenderEnum), default=GenderEnum.UNKNOWN)

    join_time = db.Column(db.DateTime, default=datetime.now)

    def __init__(self, *args, **kwargs):
        if 'password' in kwargs:
            self.password = kwargs.get('password')
            kwargs.pop('password')

        super().__init__(*args, **kwargs)

    @property
    def password(self):
        '''获取密码'''
        return self._password

    @password.setter
    def password(self, raw_password):
        '''设置密码'''
        self._password = generate_password_hash(raw_password)

    def check_password(self, raw_password):
        '''验证密码是否正确'''
        result = check_password_hash(self.password, raw_password)
        return result


class PostModel(db.Model):
    __tablename__ = 'post'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    title = db.Column(db.String(100), nullable=False)
    content = db.Column(db.Text, nullable=False)
    content_html = db.Column(db.Text)
    create_time = db.Column(db.DateTime, default=datetime.now)

    board_id = db.Column(db.Integer, db.ForeignKey("cms_board.id"))
    author_id = db.Column(db.String(40), db.ForeignKey("front_user.id"))
    # 1表示被删除，0表示未删除，默认为0
    is_delete = db.Column(db.Integer, default=0)

    board = db.relationship("BoardModel", backref='posts')
    author = db.relationship("FrontUser", backref='posts')

    def on_changed_content(target, value, oldvalue, initiator):
        '''实现markdown转HTML并进行安全验证'''
        # 允许上传的标签
        allowed_tags = ['a', 'abbr', 'acronym', 'b', 'blockquote', 'code', 'em', 'i', 'li', 'ol', 'pre', 'strong', 'ul',
                        'h1', 'h2', 'h3', 'p', 'img', 'video', 'div', 'iframe', 'p', 'br', 'span', 'hr', 'src', 'class']

        # 允许上传的属性
        allowed_attrs = {'*': ['class'],
                         'a': ['href', 'rel'],
                         'img': ['src', 'alt']}

        # 目标设置
        target.content_html = bleach.linkify(
            bleach.clean(markdown(value, output_format='html'), tags=allowed_tags, strip=True,
                         attributes=allowed_attrs))


# 数据库事件监听
db.event.listen(PostModel.content, 'set', PostModel.on_changed_content)


class CommentModel(db.Model):
    __tablename__ = 'comment'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    content = db.Column(db.Text, nullable=False)
    create_time = db.Column(db.DateTime, default=datetime.now)
    
    commenter_id = db.Column(db.String(40), db.ForeignKey('front_user.id'))
    post_id = db.Column(db.Integer, db.ForeignKey('post.id'))
    # 1表示被删除，0表示未删除，默认为0
    is_delete = db.Column(db.Integer, default=0)

    post = db.relationship('PostModel', backref='comments')
    commenter = db.relationship('FrontUser', backref='comments')
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104
```

在manage.py中导入并映射到数据库中。

对于评论，需要进行**表单验证**，apps/front/forms.py如下：

```python
from wtforms import Form, StringField, ValidationError, IntegerField
from wtforms.validators import Length, EqualTo, Regexp, InputRequired
from utils import clcache


class BaseForm(Form):
    def get_error(self):
        message = self.errors.popitem()[1][0]
        return message


class SignupForm(BaseForm):
    telephone = StringField(validators=[Regexp(r'1[3-9]\d{9}', message='请输入正确的手机号')])
    sms_captcha = StringField(validators=[Regexp(r'\w{4}', message='请输入4位短信验证码')])
    username = StringField(validators=[Length(min=2, max=20, message='用户名应介于2-20个字符，请重新输入')])
    password1 = StringField(validators=[Regexp(r'[0-9a-zA-Z_\.]{6,20}', message='密码应介于6-20位，并且由数字、英文字母或下划线、英文句号或短横线组成')])
    password2 = StringField(validators=[EqualTo('password1', message='两次输入密码不一致')])
    graph_captcha = StringField(validators=[Regexp(r'\w{4}', message='请输入4位图形验证码')])

    def validate_sms_captcha(self, field):
        telephone = self.telephone.data
        sms_captcha = self.sms_captcha.data

        # 从Redis中取验证码
        sms_captcha_redis = clcache.get_captcha(telephone)
        if not sms_captcha_redis or sms_captcha_redis.lower() != sms_captcha.lower():
            raise ValidationError(message='短信验证码过期或有误')

    def validate_graph_captcha(self, field):
        graph_captcha = self.graph_captcha.data
        
        # 从Redis中取图形验证码
        graph_captcha_redis = clcache.get_captcha(graph_captcha.lower())
        if not graph_captcha_redis or graph_captcha_redis.lower() != graph_captcha.lower():
            raise ValidationError(message='图形验证码过期或有误')


class SigninForm(BaseForm):
    telephone = StringField(validators=[Regexp(r'1[3-9]\d{9}', message='请输入正确的手机号')])
    password = StringField(validators=[Regexp(r'[0-9a-zA-Z_\.]{6,20}', message='您输入的密码有误，请重新输入')])
    remember = StringField(InputRequired())


class AddPostForm(BaseForm):
    title = StringField(validators=[InputRequired(message='请输入博客标题')])
    board_id = IntegerField(validators=[InputRequired(message='数据提交有误，请重试')])
    content = StringField(validators=[InputRequired(message='请输入博客内容')])


class AddCommentForm(BaseForm):
    content = StringField(validators=[InputRequired(message='请输入评论内容')])
    post_id = IntegerField(validators=[InputRequired(message='数据提交有误，请重试')])
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152
```

front_pdetail.js修改如下：

```js
$(function () {
    $("#comment-btn").click(function (event) {
        event.preventDefault();

        var content = window.ue.getContent();
        var post_id = $("#post-content").attr("data-id");
        clajax.post({
            'url': '/acomment/',
            'data': {
                'content': content,
                'post_id': post_id
            },
            'success': function (data) {
                if (data['code'] === 200) {
                    window.location.reload();
                } else {
                    // clalert.alertInfo(data['message']);
                    clalert.alertConfirm({
                        'msg': '您还未登录，请登录后再评论!!',
                        'cancelText': '继续浏览',
                        'confirmText': '先登录再评论',
                        'cancelCallback': function () {
                            window.location.reload();
                        },
                        'confirmCallback': function () {
                            window.location = '/signin/';
                        }
                    });
                }
            }
        });
    });
});

//初始化UEditor
$(function () {
    var ue = UE.getEditor('editor', {
        'serverUrl': '/ueditor/upload',
        'toolbars': [
            ['fullscreen', 'source', 'undo', 'redo'],
            ['bold', 'italic', 'underline', 'fontborder', 'strikethrough', 'superscript', 'subscript', 'removeformat', 'formatmatch', 'autotypeset', 'blockquote', 'pasteplain', '|', 'forecolor', 'backcolor', 'insertorderedlist', 'insertunorderedlist', 'selectall', 'cleardoc']
        ]
    });
    window.ue = ue;
})
123456789101112131415161718192021222324252627282930313233343536373839404142434445
```

front_pdetail.html如下：

```html
{% extends 'front/front_base.html' %}

{% block title %}
    {{ post.title }}
{% endblock %}

{% block head %}
    <script src="{{ url_for('static', filename='ueditor/ueditor.config.js') }}"></script>
    <script src="{{ url_for('static', filename='ueditor/ueditor.all.min.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='front/css/front_pdetail.css') }}">
    <script src="{{ url_for('static', filename='front/js/front_pdetail.js') }}"></script>
{% endblock %}

{% block main_content %}
    <div class="main-container">
        <div class="cl-container">
            <div class="post-container">
                <h2>{{ post.title }}</h2>
                <p class="post-info-group">
                    <span>发表时间：{{ post.create_time }}</span>
                    <span>作者：{{ post.author.username }}</span>
                    <span>所属板块：{{ post.board.name }}</span>
                    <span>阅读数：0</span>
                    <span>评论数：0</span>
                </p>
                <article class="post-content" id="post-content" data-id="{{ post.id }}">
                    {{ post.content_html | safe }}
                </article>
            </div>
            <div class="comment-group">
                <h3>评论列表</h3>
                <ul class="comment-list-group">
                    {% for commont in post.comments %}
                        <li>
                            <div class="avatar-group">
                                <img src="{{ url_for('static', filename='common/images/logo.png') }}"
                                     alt="">
                            </div>
                            <div class="comment-content">
                                <p class="author-info">
                                    <span>{{ commont.commenter.username }}</span>
                                    <span>{{ commont.create_time }}</span>
                                </p>
                                <p class="comment-txt">
                                    {{ commont.content | safe }}
                                </p>
                            </div>
                        </li>
                    {% endfor %}
                </ul>
            </div>
            <div class="add-comment-group">
                <h3>发表评论</h3>
                <script id="editor" type="text/plain" style="height:100px;"></script>
                <!--        <textarea name="content" id="comment" cols="99" rows="10"></textarea>-->
                <div class="comment-btn-group">
                    <button class="btn btn-primary" id="comment-btn">发表评论</button>
                </div>
            </div>
        </div>

        <div class="sm-container"></div>
    </div>
{% endblock %}



12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667
```

apps/front/views.py如下:

```python
from flask import Blueprint, views, render_template, make_response, request, session, g
from io import BytesIO
from sqlalchemy import or_

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm, AddPostForm, AddCommentForm
from .models import FrontUser, PostModel, CommentModel
from .decorators import login_required
from apps.cms.models import BannerModel, BoardModel
from exts import db

front_bp = Blueprint('front', __name__)

from .hooks import before_request


@front_bp.route('/')
def index():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(
        BannerModel.priority.desc()).limit(4)
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    board_id = request.args.get('board_id', type=int, default=None)
    posts = PostModel.query.filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None)).all()
    context = {
        'banners': banners,
        'boards': boards,
        'current_board': board_id,
        'posts': posts,
        'g': g
    }
    return render_template('front/front_index.html', **context)


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


@front_bp.route('/acomment/', methods=['POST'])
@login_required
def add_comment():
    form = AddCommentForm(request.form)
    if form.validate():
        content = form.content.data
        post_id = form.post_id.data
        post = PostModel.query.get(post_id)
        if post and post.id != 1:
            comment = CommentModel(content=content)
            comment.post = post
            comment.commenter = g.front_user
            db.session.add(comment)
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='该文章去火星啦😀')
    else:
        return restful.params_error(message=form.get_error())


class PostView(views.MethodView):
    decorators = [login_required]

    def get(self):
        boards = BoardModel.query.all()
        return render_template('front/front_apost.html', boards=boards)

    def post(self):
        form = AddPostForm(request.form)
        if form.validate():
            title = form.title.data
            content = form.content.data
            board_id = form.board_id.data
            board = BoardModel.query.get(board_id)
            if board and board.is_delete != 1:
                post = PostModel(title=title, content=content)
                post.board = board
                post.author = g.front_user
                db.session.add(post)
                db.session.commit()
                # return redirect(url_for('front.index'))
                return restful.success()
            else:
                return restful.params_error(message='没有该板块，请重新选择')
        else:
            return restful.params_error(form.get_error())


@front_bp.route('/p/<post_id>/')
def post_detail(post_id):
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        return render_template('front/front_pdetail.html', post=post)
    else:
        return restful.params_error(message='文章不存在')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html', return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))
front_bp.add_url_rule('/apost/', view_func=PostView.as_view('apost'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175
```

测试如下：
![flask front comment add](https://img-blog.csdnimg.cn/2020070409184987.gif#pic_center)

显然，此时评论必须先进行登录，并且评论后的内容实时显示到评论列表。

现在实现根据主页右侧的板块的选择而显示不同板块的文章，front_index.html修改如下：

```html
{% extends 'front/front_base.html' %}

{% block title %}首页{% endblock %}

{% block main_content %}
    <div class="main-container">
        <div class="cl-container">
            <div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
                <!-- Indicators 指令 -->
                <ol class="carousel-indicators">
                    {% for banner in banners %}
                        {% if loop.first %}
                            <li data-target="#carousel-example-generic" data-slide-to="{{ loop.index0 }}"
                                class="active"></li>
                        {% else %}
                            <li data-target="#carousel-example-generic" data-slide-to="{{ loop.index0 }}"></li>
                        {% endif %}
                    {% endfor %}
                </ol>

                <!-- Wrapper for slides 轮播图 -->
                <div class="carousel-inner" role="listbox">
                    {% for banner in banners %}
                        {% if loop.first %}
                            <div class="item active">
                                <img src="{{ banner.image_url }}" alt="{{ banner.name }}"
                                     style="width: 400px; height: 240px">
                                <div class="carousel-caption">
                                    {{ banner.name }}
                                </div>
                            </div>
                        {% else %}
                            <div class="item">
                                <img src="{{ banner.image_url }}" alt="{{ banner.name }}"
                                     style="width: 400px; height: 240px">
                                <div class="carousel-caption">
                                    {{ banner.name }}
                                </div>
                            </div>
                        {% endif %}


                    {% endfor %}
                    ...
                </div>

                <!-- Controls 左右切换控制 -->
                <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                    <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
                    <span class="sr-only">Previous</span>
                </a>
                <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                    <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
                    <span class="sr-only">Next</span>
                </a>
            </div>

            <div class="post-group">
                <ul class="post-group-head">
                    <li class=""><a href="#">最新</a></li>
                    <li class=""><a href="#">精华帖子</a></li>
                    <li class=""><a href="#">点赞最多</a></li>
                    <li class=""><a href="#">评论最多</a></li>
                </ul>
                <ul class="post-list-group">
                    {% for post in posts %}
                        <li>
                            <div class="author-avatar-group">
                                <img src="#" alt="">
                            </div>
                            <div class="post-info-group">
                                <p class="post-title">
                                    <a href="{{ url_for('front.post_detail', post_id=post.id) }}"
                                       target="_blank">{{ post.title | truncate(20) }}</a>
                                    {% if post.highlight %}
                                        <span class="label label-danger">精华帖</span>
                                    {% endif %}
                                </p>
                                <p class="post-info">
                                    <span>作者：{{ post.author.username | truncate(6) }}</span>
                                    <span>发表时间：{{ post.create_time }}</span>
                                    <span>评论：0</span>
                                    <span>阅读：0</span>
                                </p>
                            </div>
                        </li>
                    {% endfor %}

                </ul>
                <div style="text-align:center;">

                </div>
            </div>
        </div>

        <div class="sm-container">
            <div style="padding-bottom:10px;">
                <a href="{{ url_for('front.apost') }}" class="btn btn-warning btn-block">发布帖子</a>
            </div>
            <div class="list-group">
                {% if show_all %}
                    <a href="/" class="list-group-item active">所有板块</a>
                {% else %}
                    <a href="/" class="list-group-item">所有板块</a>
                {% endif %}

                {% for board in boards %}
                    {% if current_board == board.id %}
                        <a href="{{ url_for('front.index', board_id=board.id) }}"
                           class="list-group-item active">{{ board.name }}</a>
                    {% else %}
                        <a href="{{ url_for('front.index', board_id=board.id) }}"
                           class="list-group-item">{{ board.name }}</a>

                    {% endif %}
                {% endfor %}
            </div>
        </div>
    </div>
{% endblock %}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120
```

views.py修改如下：

```python
from flask import Blueprint, views, render_template, make_response, request, session, g
from io import BytesIO
from sqlalchemy import or_

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm, AddPostForm, AddCommentForm
from .models import FrontUser, PostModel, CommentModel
from .decorators import login_required
from apps.cms.models import BannerModel, BoardModel
from exts import db

front_bp = Blueprint('front', __name__)

from .hooks import before_request


@front_bp.route('/')
def index():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(
        BannerModel.priority.desc()).limit(4)
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    board_id = request.args.get('board_id', type=int, default=None)
    if board_id:
        posts = PostModel.query.filter_by(board_id=board_id).filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None)).all()
    else:
        posts = PostModel.query.filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None)).all()
    request_arg = request.args.get('board_id')
    if request_arg:
        show_all = False
    else:
        show_all = True
    context = {
        'banners': banners,
        'boards': boards,
        'current_board': board_id,
        'posts': posts,
        'show_all': show_all
    }
    return render_template('front/front_index.html', **context)


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


@front_bp.route('/acomment/', methods=['POST'])
@login_required
def add_comment():
    form = AddCommentForm(request.form)
    if form.validate():
        content = form.content.data
        post_id = form.post_id.data
        post = PostModel.query.get(post_id)
        if post and post.id != 1:
            comment = CommentModel(content=content)
            comment.post = post
            comment.commenter = g.front_user
            db.session.add(comment)
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='该文章去火星啦😀')
    else:
        return restful.params_error(message=form.get_error())


class PostView(views.MethodView):
    decorators = [login_required]

    def get(self):
        boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
        return render_template('front/front_apost.html', boards=boards)

    def post(self):
        form = AddPostForm(request.form)
        if form.validate():
            title = form.title.data
            content = form.content.data
            board_id = form.board_id.data
            board = BoardModel.query.get(board_id)
            if board and board.is_delete != 1:
                post = PostModel(title=title, content=content)
                post.board = board
                post.author = g.front_user
                db.session.add(post)
                db.session.commit()
                # return redirect(url_for('front.index'))
                return restful.success()
            else:
                return restful.params_error(message='没有该板块，请重新选择')
        else:
            return restful.params_error(form.get_error())


@front_bp.route('/p/<post_id>/')
def post_detail(post_id):
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        return render_template('front/front_pdetail.html', post=post)
    else:
        return restful.params_error(message='文章不存在')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html', return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))
front_bp.add_url_rule('/apost/', view_func=PostView.as_view('apost'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183
```

显示：
![flask front index board show](https://img-blog.csdnimg.cn/20200704091923568.gif#pic_center)

显然，已经达到基本效果。

## 四、前台分页

现在实现文章分页，需要用到插件，用命令`pip install flask-paginate`安装。

配置文件config.py修改如下：

```python
import os

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_bbs'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

TEMPLATE_AUTO_RELOAD = True
SQLALCHEMY_DATABASE_URI = DB_URL
SQLALCHEMY_TRACK_MODIFICATIONS = False
SQLALCHEMY_POOL_RECYCLE = 280
SQLALCHEMY_POOL_SIZE = 20

# 设置密钥
SECRET_KEY = os.urandom(15)

# 发送邮箱服务地址
MAIL_SERVER = 'smtp.qq.com'
# 邮箱端口，为587或465，为587时TLS设置为True，为465时SSL设置为True
MAIL_PORT = 587
MAIL_USE_TLS = True
# MAIL_USE_SSL = True # MAIL_PORT为465时设置此项
# 用户名可以为你的邮箱，需要自行添加
MAIL_USERNAME = '379869029@qq.com'
# 邮箱密码，不是邮箱账号密码，而是第三方客户端登录使用的授权码，需要自行获取
MAIL_PASSWORD = 'vrxiqciwjnbcxxxx'
# 发送者即你的邮箱，需要自行添加
MAIL_DEFAULT_SENDER = '379869029@qq.com'

# 云片APIKEY
YP_API = 'edf71361381f31b3957beda37f20xxxx'

# 七牛云存储配置
QINIU_ACCESS_KEY = '_PL7p4sTSlfAKl71hIkuG3F6y18681ZKkNJ3xxxx'
QINIU_SECRET_KEY = '4P09jodUqhhEqIOcaqT9daVFmKjCJI3l6gsAxxxx'
QINIU_BUCKET_NAME = 'bbs-images'
QINIU_BUCKET_DOMAIN = 'qcquu7fd2.bkt.clouddn.com'

# 分页配置
PER_PAGE = 10
12345678910111213141516171819202122232425262728293031323334353637383940414243
```

front_index.html修改如下：

```html
{% extends 'front/front_base.html' %}

{% block title %}首页{% endblock %}

{% block main_content %}
    <div class="main-container">
        <div class="cl-container">
            <div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
                <!-- Indicators 指令 -->
                <ol class="carousel-indicators">
                    {% for banner in banners %}
                        {% if loop.first %}
                            <li data-target="#carousel-example-generic" data-slide-to="{{ loop.index0 }}"
                                class="active"></li>
                        {% else %}
                            <li data-target="#carousel-example-generic" data-slide-to="{{ loop.index0 }}"></li>
                        {% endif %}
                    {% endfor %}
                </ol>

                <!-- Wrapper for slides 轮播图 -->
                <div class="carousel-inner" role="listbox">
                    {% for banner in banners %}
                        {% if loop.first %}
                            <div class="item active">
                                <img src="{{ banner.image_url }}" alt="{{ banner.name }}"
                                     style="width: 400px; height: 240px">
                                <div class="carousel-caption">
                                    {{ banner.name }}
                                </div>
                            </div>
                        {% else %}
                            <div class="item">
                                <img src="{{ banner.image_url }}" alt="{{ banner.name }}"
                                     style="width: 400px; height: 240px">
                                <div class="carousel-caption">
                                    {{ banner.name }}
                                </div>
                            </div>
                        {% endif %}


                    {% endfor %}
                    ...
                </div>

                <!-- Controls 左右切换控制 -->
                <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                    <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
                    <span class="sr-only">Previous</span>
                </a>
                <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                    <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
                    <span class="sr-only">Next</span>
                </a>
            </div>

            <div class="post-group">
                <ul class="post-group-head">
                    <li class=""><a href="#">最新</a></li>
                    <li class=""><a href="#">精华帖子</a></li>
                    <li class=""><a href="#">点赞最多</a></li>
                    <li class=""><a href="#">评论最多</a></li>
                </ul>
                <ul class="post-list-group">
                    {% for post in posts %}
                        <li>
                            <div class="author-avatar-group">
                                <img src="#" alt="">
                            </div>
                            <div class="post-info-group">
                                <p class="post-title">
                                    <a href="{{ url_for('front.post_detail', post_id=post.id) }}"
                                       target="_blank">{{ post.title | truncate(20) }}</a>
                                    {% if post.highlight %}
                                        <span class="label label-danger">精华帖</span>
                                    {% endif %}
                                </p>
                                <p class="post-info">
                                    <span>作者：{{ post.author.username | truncate(6) }}</span>
                                    <span>发表时间：{{ post.create_time }}</span>
                                    <span>评论：0</span>
                                    <span>阅读：0</span>
                                </p>
                            </div>
                        </li>
                    {% endfor %}

                </ul>
                <div style="text-align:center;">
                    {{ pagination.links }}
                </div>
            </div>
        </div>

        <div class="sm-container">
            <div style="padding-bottom:10px;">
                <a href="{{ url_for('front.apost') }}" class="btn btn-warning btn-block">发布帖子</a>
            </div>
            <div class="list-group">
                {% if show_all %}
                    <a href="/" class="list-group-item active">所有板块</a>
                {% else %}
                    <a href="/" class="list-group-item">所有板块</a>
                {% endif %}

                {% for board in boards %}
                    {% if current_board == board.id %}
                        <a href="{{ url_for('front.index', board_id=board.id) }}"
                           class="list-group-item active">{{ board.name }}</a>
                    {% else %}
                        <a href="{{ url_for('front.index', board_id=board.id) }}"
                           class="list-group-item">{{ board.name }}</a>

                    {% endif %}
                {% endfor %}
            </div>
        </div>
    </div>
{% endblock %}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120
```

views.py修改如下：

```python
from flask import Blueprint, views, render_template, make_response, request, session, g
from flask_paginate import Pagination, get_page_parameter
from io import BytesIO
from sqlalchemy import or_

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm, AddPostForm, AddCommentForm
from .models import FrontUser, PostModel, CommentModel
from .decorators import login_required
from apps.cms.models import BannerModel, BoardModel
from exts import db
import config

front_bp = Blueprint('front', __name__)

from .hooks import before_request


@front_bp.route('/')
def index():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(
        BannerModel.priority.desc()).limit(4)
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    board_id = request.args.get('board_id', type=int, default=None)
    # 当前页
    page = request.args.get(get_page_parameter(), type=int, default=1)
    start = (page - 1) * config.PER_PAGE
    end = start + config.PER_PAGE
    if board_id:
        posts = PostModel.query.filter_by(board_id=board_id).filter(
            or_(PostModel.is_delete == 0, PostModel.is_delete == None)).slice(start, end)
        total = PostModel.query.filter_by(board_id=board_id).filter(
            or_(PostModel.is_delete == 0, PostModel.is_delete == None)).count()
    else:
        posts = PostModel.query.filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None)).slice(start, end)
        total = PostModel.query.filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None)).count()
    request_arg = request.args.get('board_id')
    if request_arg:
        show_all = False
    else:
        show_all = True

    pagination = Pagination(page=page, total=total, bs_version=3, per_page=config.PER_PAGE)
    context = {
        'banners': banners,
        'boards': boards,
        'current_board': board_id,
        'posts': posts,
        'show_all': show_all,
        'pagination': pagination
    }
    return render_template('front/front_index.html', **context)


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


@front_bp.route('/acomment/', methods=['POST'])
@login_required
def add_comment():
    form = AddCommentForm(request.form)
    if form.validate():
        content = form.content.data
        post_id = form.post_id.data
        post = PostModel.query.get(post_id)
        if post and post.id != 1:
            comment = CommentModel(content=content)
            comment.post = post
            comment.commenter = g.front_user
            db.session.add(comment)
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='该文章去火星啦😀')
    else:
        return restful.params_error(message=form.get_error())


class PostView(views.MethodView):
    decorators = [login_required]

    def get(self):
        boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
        return render_template('front/front_apost.html', boards=boards)

    def post(self):
        form = AddPostForm(request.form)
        if form.validate():
            title = form.title.data
            content = form.content.data
            board_id = form.board_id.data
            board = BoardModel.query.get(board_id)
            if board and board.is_delete != 1:
                post = PostModel(title=title, content=content)
                post.board = board
                post.author = g.front_user
                db.session.add(post)
                db.session.commit()
                # return redirect(url_for('front.index'))
                return restful.success()
            else:
                return restful.params_error(message='没有该板块，请重新选择')
        else:
            return restful.params_error(form.get_error())


@front_bp.route('/p/<post_id>/')
def post_detail(post_id):
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        return render_template('front/front_pdetail.html', post=post)
    else:
        return restful.params_error(message='文章不存在')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html', return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))
front_bp.add_url_rule('/apost/', view_func=PostView.as_view('apost'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196
```

因为数据过少，翻页的效果不是太明显，因此需要创建一些测试数据，manage.py修改如下：

```python
from flask import Blueprint, views, render_template, make_response, request, session, g
from flask_paginate import Pagination, get_page_parameter
from io import BytesIO
from sqlalchemy import or_

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm, AddPostForm, AddCommentForm
from .models import FrontUser, PostModel, CommentModel
from .decorators import login_required
from apps.cms.models import BannerModel, BoardModel
from exts import db
import config

front_bp = Blueprint('front', __name__)

from .hooks import before_request


@front_bp.route('/')
def index():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(
        BannerModel.priority.desc()).limit(4)
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    board_id = request.args.get('board_id', type=int, default=None)
    # 当前页
    page = request.args.get(get_page_parameter(), type=int, default=1)
    start = (page - 1) * config.PER_PAGE
    end = start + config.PER_PAGE
    if board_id:
        posts = PostModel.query.filter_by(board_id=board_id).filter(
            or_(PostModel.is_delete == 0, PostModel.is_delete == None)).slice(start, end)
        total = PostModel.query.filter_by(board_id=board_id).filter(
            or_(PostModel.is_delete == 0, PostModel.is_delete == None)).count()
    else:
        posts = PostModel.query.filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None)).slice(start, end)
        total = PostModel.query.filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None)).count()
    request_arg = request.args.get('board_id')
    if request_arg:
        show_all = False
    else:
        show_all = True

    pagination = Pagination(page=page, total=total, bs_version=3, per_page=config.PER_PAGE)
    context = {
        'banners': banners,
        'boards': boards,
        'current_board': board_id,
        'posts': posts,
        'show_all': show_all,
        'pagination': pagination
    }
    return render_template('front/front_index.html', **context)


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


@front_bp.route('/acomment/', methods=['POST'])
@login_required
def add_comment():
    form = AddCommentForm(request.form)
    if form.validate():
        content = form.content.data
        post_id = form.post_id.data
        post = PostModel.query.get(post_id)
        if post and post.id != 1:
            comment = CommentModel(content=content)
            comment.post = post
            comment.commenter = g.front_user
            db.session.add(comment)
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='该文章去火星啦😀')
    else:
        return restful.params_error(message=form.get_error())


class PostView(views.MethodView):
    decorators = [login_required]

    def get(self):
        boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
        return render_template('front/front_apost.html', boards=boards)

    def post(self):
        form = AddPostForm(request.form)
        if form.validate():
            title = form.title.data
            content = form.content.data
            board_id = form.board_id.data
            board = BoardModel.query.get(board_id)
            if board and board.is_delete != 1:
                post = PostModel(title=title, content=content)
                post.board = board
                post.author = g.front_user
                db.session.add(post)
                db.session.commit()
                # return redirect(url_for('front.index'))
                return restful.success()
            else:
                return restful.params_error(message='没有该板块，请重新选择')
        else:
            return restful.params_error(form.get_error())


@front_bp.route('/p/<post_id>/')
def post_detail(post_id):
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        return render_template('front/front_pdetail.html', post=post)
    else:
        return restful.params_error(message='文章不存在')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html', return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))
front_bp.add_url_rule('/apost/', view_func=PostView.as_view('apost'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196
```

命令行中执行`python manage.py test_post_page`，即创建数据成功。

进行测试如下：
![flask front index paginate](https://img-blog.csdnimg.cn/20200704091954400.gif#pic_center)

显然，已经实现翻页的基本功能。

说明：
（1）在初始化Pagination对象时还可以传入inner_window、outer_window、prev_label、next_label和per_page_parameter等参数，可以根据需要定制；
（2）在**数据量较多**时，分页会正常显示，但是**数据量较少**时，如果在初始化Pagination时未指定per_page参数，则不会显示出分页，需要指定**per_page**为一个较小的数才能显示出分页，并且如果只有1条数据时，默认会不显示数据，需要指定**show_single_page**参数为True。

# Python全栈（八）Flask项目实战之12.前台页面完善

Github和Gitee代码**同步更新**：
https://github.com/PythonWebProject/Flask_BBS；
https://gitee.com/Python_Web_Project/Flask_BBS。

## 一、首页的排序和状态选中

先实现首页按照不同板块和不同浏览方式继续组合，front_index.html如下：

```html
{% extends 'front/front_base.html' %}

{% block title %}首页{% endblock %}

{% block main_content %}
    <div class="main-container">
        <div class="cl-container">
            <div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
                <!-- Indicators 指令 -->
                <ol class="carousel-indicators">
                    {% for banner in banners %}
                        {% if loop.first %}
                            <li data-target="#carousel-example-generic" data-slide-to="{{ loop.index0 }}"
                                class="active"></li>
                        {% else %}
                            <li data-target="#carousel-example-generic" data-slide-to="{{ loop.index0 }}"></li>
                        {% endif %}
                    {% endfor %}
                </ol>

                <!-- Wrapper for slides 轮播图 -->
                <div class="carousel-inner" role="listbox">
                    {% for banner in banners %}
                        {% if loop.first %}
                            <div class="item active">
                                <img src="{{ banner.image_url }}" alt="{{ banner.name }}"
                                     style="width: 400px; height: 240px">
                                <div class="carousel-caption">
                                    {{ banner.name }}
                                </div>
                            </div>
                        {% else %}
                            <div class="item">
                                <img src="{{ banner.image_url }}" alt="{{ banner.name }}"
                                     style="width: 400px; height: 240px">
                                <div class="carousel-caption">
                                    {{ banner.name }}
                                </div>
                            </div>
                        {% endif %}


                    {% endfor %}
                    ...
                </div>

                <!-- Controls 左右切换控制 -->
                <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
                    <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
                    <span class="sr-only">Previous</span>
                </a>
                <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                    <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
                    <span class="sr-only">Next</span>
                </a>
            </div>

            <div class="post-group">
                <ul class="post-group-head">
                    {% if current_sort == 1 %}
                        <li class="active"><a href="{{ url_for('front.index', st=1, board_id=current_board) }}">最新</a></li>
                    {% else %}
                        <li class=""><a href="{{ url_for('front.index', st=1, board_id=current_board) }}">最新</a></li>
                    {% endif %}

                    {% if current_sort == 2 %}
                        <li class="active"><a href="{{ url_for('front.index', st=2, board_id=current_board) }}">精华帖子</a></li>
                    {% else %}
                        <li class=""><a href="{{ url_for('front.index', st=2, board_id=current_board) }}">精华帖子</a></li>
                    {% endif %}
                    {% if current_sort == 3 %}
                        <li class="active"><a href="{{ url_for('front.index', st=3, board_id=current_board) }}">点赞最多</a></li>
                    {% else %}
                        <li class=""><a href="{{ url_for('front.index', st=3, board_id=current_board) }}">点赞最多</a></li>
                    {% endif %}
                    {% if current_sort == 4 %}
                        <li class="active"><a href="{{ url_for('front.index', st=4, board_id=current_board) }}">评论最多</a></li>
                    {% else %}
                        <li class=""><a href="{{ url_for('front.index', st=4, board_id=current_board) }}">评论最多</a></li>
                    {% endif %}
                </ul>
                <ul class="post-list-group">
                    {% for post in posts %}
                        <li>
                            <div class="author-avatar-group">
                                <img src="#" alt="">
                            </div>
                            <div class="post-info-group">
                                <p class="post-title">
                                    <a href="{{ url_for('front.post_detail', post_id=post.id) }}"
                                       target="_blank">{{ post.title | truncate(20) }}</a>
                                    {% if post.highlight %}
                                        <span class="label label-danger">精华帖</span>
                                    {% endif %}
                                </p>
                                <p class="post-info">
                                    <span>作者：{{ post.author.username | truncate(6) }}</span>
                                    <span>发表时间：{{ post.create_time }}</span>
                                    <span>评论：0</span>
                                    <span>阅读：0</span>
                                </p>
                            </div>
                        </li>
                    {% endfor %}

                </ul>
                <div style="text-align:center;">
                    {{ pagination.links }}
                </div>
            </div>
        </div>

        <div class="sm-container">
            <div style="padding-bottom:10px;">
                <a href="{{ url_for('front.apost') }}" class="btn btn-warning btn-block">发布帖子</a>
            </div>
            <div class="list-group">
                {% if show_all %}
                    <a href="{{ url_for('front.index', st=current_sort) }}" class="list-group-item active">所有板块</a>
                {% else %}
                    <a href="{{ url_for('front.index', st=current_sort) }}" class="list-group-item">所有板块</a>
                {% endif %}

                {% for board in boards %}
                    {% if current_board == board.id %}
                        <a href="{{ url_for('front.index', st=current_sort, board_id=board.id) }}"
                           class="list-group-item active">{{ board.name }}</a>
                    {% else %}
                        <a href="{{ url_for('front.index', st=current_sort, board_id=board.id) }}"
                           class="list-group-item">{{ board.name }}</a>

                    {% endif %}
                {% endfor %}
            </div>
        </div>
    </div>
{% endblock %}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137
```

apps/front/views.py如下：

```python
from flask import Blueprint, views, render_template, make_response, request, session, g
from flask_paginate import Pagination, get_page_parameter
from io import BytesIO
from sqlalchemy import or_

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm, AddPostForm, AddCommentForm
from .models import FrontUser, PostModel, CommentModel
from .decorators import login_required
from apps.cms.models import BannerModel, BoardModel
from exts import db
import config

front_bp = Blueprint('front', __name__)

from .hooks import before_request


@front_bp.route('/')
def index():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(
        BannerModel.priority.desc()).limit(4)
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    board_id = request.args.get('board_id', type=int, default=None)
    # 当前页
    page = request.args.get(get_page_parameter(), type=int, default=1)
    start = (page - 1) * config.PER_PAGE
    end = start + config.PER_PAGE
    sort = request.args.get('st', type=int, default=1)
    if board_id:
        posts = PostModel.query.filter_by(board_id=board_id).filter(
            or_(PostModel.is_delete == 0, PostModel.is_delete == None)).slice(start, end)
        total = PostModel.query.filter_by(board_id=board_id).filter(
            or_(PostModel.is_delete == 0, PostModel.is_delete == None)).count()
    else:
        posts = PostModel.query.filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None)).slice(start, end)
        total = PostModel.query.filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None)).count()
    request_arg = request.args.get('board_id')
    if request_arg:
        show_all = False
    else:
        show_all = True

    pagination = Pagination(page=page, total=total, bs_version=3, per_page=config.PER_PAGE)
    context = {
        'banners': banners,
        'boards': boards,
        'current_board': board_id,
        'posts': posts,
        'show_all': show_all,
        'pagination': pagination,
        'current_sort': sort
    }
    return render_template('front/front_index.html', **context)


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


@front_bp.route('/acomment/', methods=['POST'])
@login_required
def add_comment():
    form = AddCommentForm(request.form)
    if form.validate():
        content = form.content.data
        post_id = form.post_id.data
        post = PostModel.query.get(post_id)
        if post and post.id != 1:
            comment = CommentModel(content=content)
            comment.post = post
            comment.commenter = g.front_user
            db.session.add(comment)
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='该文章去火星啦😀')
    else:
        return restful.params_error(message=form.get_error())


class PostView(views.MethodView):
    decorators = [login_required]

    def get(self):
        boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
        return render_template('front/front_apost.html', boards=boards)

    def post(self):
        form = AddPostForm(request.form)
        if form.validate():
            title = form.title.data
            content = form.content.data
            board_id = form.board_id.data
            board = BoardModel.query.get(board_id)
            if board and board.is_delete != 1:
                post = PostModel(title=title, content=content)
                post.board = board
                post.author = g.front_user
                db.session.add(post)
                db.session.commit()
                # return redirect(url_for('front.index'))
                return restful.success()
            else:
                return restful.params_error(message='没有该板块，请重新选择')
        else:
            return restful.params_error(form.get_error())


@front_bp.route('/p/<post_id>/')
def post_detail(post_id):
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        return render_template('front/front_pdetail.html', post=post)
    else:
        return restful.params_error(message='文章不存在')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html', return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))
front_bp.add_url_rule('/apost/', view_func=PostView.as_view('apost'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198
```

显示：
![flask front index board sort preview](https://img-blog.csdnimg.cn/20200705202615402.gif#pic_center)

此时点击不同的板块和排序方式，链接都会随之变化，但是文章内容排序还是未发生变化，因此需要再进行改进。

views.py完善如下：

```python
from flask import Blueprint, views, render_template, make_response, request, session, g
from flask_paginate import Pagination, get_page_parameter
from io import BytesIO
from sqlalchemy import or_
from sqlalchemy.sql import func

from utils.captcha import Captcha
from utils import clcache, restful, safe_url
from .forms import SignupForm, SigninForm, AddPostForm, AddCommentForm
from .models import FrontUser, PostModel, CommentModel
from .decorators import login_required
from apps.cms.models import BannerModel, BoardModel, HighlightPostModel
from exts import db
import config

front_bp = Blueprint('front', __name__)

from .hooks import before_request


@front_bp.route('/')
def index():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(
        BannerModel.priority.desc()).limit(4)
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    board_id = request.args.get('board_id', type=int, default=None)
    # 当前页
    page = request.args.get(get_page_parameter(), type=int, default=1)
    start = (page - 1) * config.PER_PAGE
    end = start + config.PER_PAGE
    sort = request.args.get('st', type=int, default=1)
    query_obj = PostModel.query.filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None))
    # 最新文章
    if sort == 1:
        query_obj = query_obj.order_by(PostModel.create_time.desc())
    # 精华帖
    elif sort == 2:
        query_obj = db.session.query(PostModel).join(HighlightPostModel).order_by(HighlightPostModel.create_time.desc()).filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None))
    # 点赞最多
    elif sort == 3:
        query_obj = query_obj.order_by(PostModel.create_time.desc())
    # 评论最多
    elif sort == 4:
        query_obj = db.session.query(PostModel).join(CommentModel).filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None)).group_by(PostModel.id).order_by(func.count(CommentModel.id).desc())

    if board_id:
        query_obj = query_obj.filter(PostModel.board_id==board_id)
        posts = query_obj.slice(start, end)
        total = query_obj.count()
    else:
        posts = query_obj.slice(start, end)
        total = query_obj.count()
    request_arg = request.args.get('board_id')
    if request_arg:
        show_all = False
    else:
        show_all = True

    pagination = Pagination(page=page, total=total, bs_version=3, per_page=config.PER_PAGE)
    context = {
        'banners': banners,
        'boards': boards,
        'current_board': board_id,
        'posts': posts,
        'show_all': show_all,
        'pagination': pagination,
        'current_sort': sort
    }
    return render_template('front/front_index.html', **context)


@front_bp.route('/captcha/')
def graph_captcha():
    try:
        text, image = Captcha.gene_graph_captcha()
        print('发送的图形验证码：{}'.format(text))
        clcache.save_captcha(text.lower(), text)
        # 处理图片二进制流的传输
        out = BytesIO()
        # 把图片保存到字节流中，并指定格式为png
        image.save(out, 'png')
        # 指定文件流指针,从文件最开始开始读
        out.seek(0)
        # 将字节流包装到Response对象中,返回前端
        resp = make_response(out.read())
        resp.content_type = 'image/png'
    except:
        return graph_captcha()

    return resp


@front_bp.route('/referer_test/')
def referer_test():
    return render_template('front/referer_test.html')


@front_bp.route('/acomment/', methods=['POST'])
@login_required
def add_comment():
    form = AddCommentForm(request.form)
    if form.validate():
        content = form.content.data
        post_id = form.post_id.data
        post = PostModel.query.get(post_id)
        if post and post.id != 1:
            comment = CommentModel(content=content)
            comment.post = post
            comment.commenter = g.front_user
            db.session.add(comment)
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='该文章去火星啦😀')
    else:
        return restful.params_error(message=form.get_error())


class PostView(views.MethodView):
    decorators = [login_required]

    def get(self):
        boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
        return render_template('front/front_apost.html', boards=boards)

    def post(self):
        form = AddPostForm(request.form)
        if form.validate():
            title = form.title.data
            content = form.content.data
            board_id = form.board_id.data
            board = BoardModel.query.get(board_id)
            if board and board.is_delete != 1:
                post = PostModel(title=title, content=content)
                post.board = board
                post.author = g.front_user
                db.session.add(post)
                db.session.commit()
                # return redirect(url_for('front.index'))
                return restful.success()
            else:
                return restful.params_error(message='没有该板块，请重新选择')
        else:
            return restful.params_error(form.get_error())


@front_bp.route('/p/<post_id>/')
def post_detail(post_id):
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        post.read_count += 1
        db.session.commit()
        return render_template('front/front_pdetail.html', post=post)
    else:
        return restful.params_error(message='文章不存在')


class SignupView(views.MethodView):
    def get(self):
        # referrer表示页面的跳转，即从哪个页面跳转到当前页面
        print('Referer:', request.referrer)
        return_to = request.referrer
        # is_safe_url()判断请求是否来自站内
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signup.html', return_to=return_to)
        else:
            return render_template('front/front_signup.html')

    def post(self):
        form = SignupForm(request.form)
        if form.validate():
            # 验证成功则保存数据
            telephone = form.telephone.data
            username = form.username.data
            password = form.password1.data

            user = FrontUser(telephone=telephone, username=username, password=password)
            db.session.add(user)
            db.session.commit()
            return restful.success(message='注册成功，欢迎您进入熊熊论坛')
        else:
            return restful.params_error(message=form.get_error())


class SigninView(views.MethodView):
    def get(self):
        return_to = request.referrer
        if return_to and return_to != request.url and safe_url.is_safe_url(return_to):
            return render_template('front/front_signin.html', return_to=return_to)
        else:
            return render_template('front/front_signin.html')

    def post(self):
        form = SigninForm(request.form)
        if form.validate():
            telephone = form.telephone.data
            password = form.password.data
            remember = form.remember.data
            user = FrontUser.query.filter_by(telephone=telephone).first()
            if user and user.check_password(password):
                session['user_id'] = user.id
                if remember:
                    session.permanent = True
                return restful.success(message='登录成功')
            else:
                return restful.params_error(message='手机号或密码有误')
        else:
            return restful.params_error(message=form.get_error())


front_bp.add_url_rule('/signup/', view_func=SignupView.as_view('signup'))
front_bp.add_url_rule('/signin/', view_func=SigninView.as_view('signin'))
front_bp.add_url_rule('/apost/', view_func=PostView.as_view('apost'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206207208209210211212213214
```

显示：
![flask front index board sort finish](https://img-blog.csdnimg.cn/20200705202638145.gif#pic_center)

此时，已经实现了板块和排序同时筛选的效果（点赞功能暂未实现，后期会进一步实现）。

## 二、celery的简单使用

现在大概介绍celery的使用，是用来异步执行任务的工具，比如在发送邮件和短信验证码时，一般是一个发了，才能发送下一个，显然这效率很低，可以使用celery实现多任务处理、提高发送效率。

celery将所有任务task添加到队列（即中间件，一般由Redis、Kafka等实现），队列再将task发给worker，再将其保存到backend后台存储起来。

在Python中使用celery需要安装，在虚拟环境中执行`pip install celery`命令即可。

为了了解celery的简单使用，可以新建一个独立于项目的Python文件demo.py如下：

```python
from celery import Celery
import time

celery = Celery('task', broker='redis://127.0.0.1:6379/0', backend='redis://127.0.0.1:6379/0')

@celery.task
def send_mail():
    print('邮件发送开始')
    time.sleep(2)
    print('邮件发送完成')
12345678910
```

再在同级目录下再新建一个Python文件main.py如下：

```python
from demo import send_mail

if __name__ == '__main__':
    send_mail.delay()
1234
```

此时再在两个Python文件同级目录下执行`celery -A demo.celery worker --loglevel=info`来开启监控，打印如下：

```shell
 -------------- celery@DESKTOP-L8PM0E9 v4.4.6 (cliffs)
--- ***** -----
-- ******* ---- Windows-10-10.0.18362-SP0 2020-07-04 20:55:27
- *** --- * ---
- ** ---------- [config]
- ** ---------- .> app:         task:0x37dc4c0
- ** ---------- .> transport:   redis://127.0.0.1:6379/0
- ** ---------- .> results:     redis://127.0.0.1:6379/0
- *** --- * --- .> concurrency: 8 (prefork)
-- ******* ---- .> task events: OFF (enable -E to monitor tasks in this worker)
--- ***** -----
 -------------- [queues]
                .> celery           exchange=celery(direct) key=celery


[tasks]
  . demo.send_mail

[2020-07-04 20:55:27,370: INFO/MainProcess] Connected to redis://127.0.0.1:6379/0
[2020-07-04 20:55:27,436: INFO/MainProcess] mingle: searching for neighbors
[2020-07-04 20:55:27,935: INFO/SpawnPoolWorker-3] child process 24484 calling self.run()
[2020-07-04 20:55:27,986: INFO/SpawnPoolWorker-5] child process 6072 calling self.run()
[2020-07-04 20:55:27,990: INFO/SpawnPoolWorker-2] child process 18732 calling self.run()
[2020-07-04 20:55:27,965: INFO/SpawnPoolWorker-4] child process 23788 calling self.run()
[2020-07-04 20:55:27,985: INFO/SpawnPoolWorker-1] child process 17512 calling self.run()
[2020-07-04 20:55:28,130: INFO/SpawnPoolWorker-7] child process 22556 calling self.run()
[2020-07-04 20:55:28,094: INFO/SpawnPoolWorker-6] child process 26140 calling self.run()
[2020-07-04 20:55:28,360: INFO/SpawnPoolWorker-8] child process 24440 calling self.run()
[2020-07-04 20:55:28,519: INFO/MainProcess] mingle: all alone
[2020-07-04 20:55:28,578: INFO/MainProcess] celery@DESKTOP-L8PM0E9 ready.
12345678910111213141516171819202122232425262728293031
```

此时已经开启监控。

此时多次执行之前创建的main.py，打印如下：

```shell
[2020-07-04 21:05:53,979: INFO/MainProcess] Received task: demo.send_mail[724b46b2-77d5-4277-95af-3ec44448f7aa]
[2020-07-04 21:05:53,980: WARNING/MainProcess] 邮件发送开始
[2020-07-04 21:05:55,981: WARNING/MainProcess] 邮件发送完成
[2020-07-04 21:05:55,986: INFO/MainProcess] Task demo.send_mail[724b46b2-77d5-4277-95af-3ec44448f7aa] succeeded in 2.015999999974156s: None
[2020-07-04 21:06:03,541: INFO/MainProcess] Received task: demo.send_mail[374b8826-956f-436a-8a3a-0ca4fe6e23ad]
[2020-07-04 21:06:03,543: WARNING/MainProcess] 邮件发送开始
[2020-07-04 21:06:05,546: WARNING/MainProcess] 邮件发送完成
[2020-07-04 21:06:05,548: INFO/MainProcess] Task demo.send_mail[374b8826-956f-436a-8a3a-0ca4fe6e23ad] succeeded in 2.014999999984866s: None
[2020-07-04 21:06:05,551: INFO/MainProcess] Received task: demo.send_mail[96b628b3-6da0-4f6b-b6cf-5f1496c1588a]
[2020-07-04 21:06:05,551: WARNING/MainProcess] 邮件发送开始
[2020-07-04 21:06:07,552: WARNING/MainProcess] 邮件发送完成
[2020-07-04 21:06:07,554: INFO/MainProcess] Task demo.send_mail[96b628b3-6da0-4f6b-b6cf-5f1496c1588a] succeeded in 2.0s: None
[2020-07-04 21:06:49,367: INFO/MainProcess] Received task: demo.send_mail[ff0f6e5d-8a60-490e-9378-98ea6cf78519]
[2020-07-04 21:06:49,368: WARNING/MainProcess] 邮件发送开始
[2020-07-04 21:06:51,370: WARNING/MainProcess] 邮件发送完成
[2020-07-04 21:06:51,373: INFO/MainProcess] Task demo.send_mail[ff0f6e5d-8a60-490e-9378-98ea6cf78519] succeeded in 2.0s: None
[2020-07-04 21:06:53,373: INFO/MainProcess] Received task: demo.send_mail[90a3e52f-4e02-44f1-831e-4b0e4c0dfb8f]
[2020-07-04 21:06:53,375: WARNING/MainProcess] 邮件发送开始
[2020-07-04 21:06:55,375: WARNING/MainProcess] 邮件发送完成
[2020-07-04 21:06:55,376: INFO/MainProcess] Task demo.send_mail[90a3e52f-4e02-44f1-831e-4b0e4c0dfb8f] succeeded in 2.0s: None

123456789101112131415161718192021
```

显然，多次任务都执行成功。

说明：
（1）`send_mail.delay()`函数必须放在`if __name__ == '__main__'`之下，否则会出现异常；
（2）如果在开启监控时报错，可以增加一个参数pool，即`celery -A demo.celery worker --loglevel=info --pool=solo`。

此时连接Redis查询`keys *`，可以看到：

```shell
 1) "_kombu.binding.celery"                                
 2) "_kombu.binding.celeryev"                              
 3) "celery-task-meta-374b8826-956f-436a-8a3a-0ca4fe6e23ad"
 4) "celery-task-meta-9c395b78-6821-4129-bc10-b16cf472403a"
 5) "celery-task-meta-90a3e52f-4e02-44f1-831e-4b0e4c0dfb8f"
 6) "celery-task-meta-96b628b3-6da0-4f6b-b6cf-5f1496c1588a"
 7) "celery-task-meta-8f300c84-eaba-4c53-adaa-08066dd85265"
 8) "celery-task-meta-ff0f6e5d-8a60-490e-9378-98ea6cf78519"
 9) "celery-task-meta-724b46b2-77d5-4277-95af-3ec44448f7aa"
10) "celery-task-meta-e30213ef-955e-4a4d-a3cd-4a5d32488aca"
11) "_kombu.binding.celery.pidbox"                         
1234567891011
```

此时即实施监控任务执行情况，将之前执行的任务添加到Redis中。

## 三、flask-celery异步发送邮件

现在将celery整合到项目中。

在项目主目录下新建task.py如下：

```python
from flask import  Flask
from celery import Celery
from flask_mail import Message

from exts import mail
import config

app = Flask(__name__)

mail.init_app(app)


def make_celery(app):
    celery = Celery(
        app.import_name,
        backend=config.CELERY_RESULT_BACKEND,
        broker=config.CELERY_BROKER_URL
    )
    # celery.conf.update(app.config)

    class ContextTask(celery.Task):
        def __call__(self, *args, **kwargs):
            with app.app_context():
                return self.run(*args, **kwargs)

    celery.Task = ContextTask
    return celery


celery = make_celery(app)


@celery.task
def send_mail_celery(subject, recipients, body):
    message = Message(subject=subject, recipients=recipients, body=body)
    mail.send(message)

12345678910111213141516171819202122232425262728293031323334353637
```

config.py修改如下：

```python
import os

# 数据库连接配置
HOSTNAME = '127.0.0.1'
PORT = 3306
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'flask_bbs'
DB_URL = 'mysql+mysqlconnector://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOSTNAME, PORT, DATABASE)

TEMPLATE_AUTO_RELOAD = True
SQLALCHEMY_DATABASE_URI = DB_URL
SQLALCHEMY_TRACK_MODIFICATIONS = False
SQLALCHEMY_POOL_RECYCLE = 280
SQLALCHEMY_POOL_SIZE = 20

# 设置密钥
SECRET_KEY = os.urandom(15)

# 发送邮箱服务地址
MAIL_SERVER = 'smtp.qq.com'
# 邮箱端口，为587或465，为587时TLS设置为True，为465时SSL设置为True
MAIL_PORT = 587
MAIL_USE_TLS = True
# MAIL_USE_SSL = True # MAIL_PORT为465时设置此项
# 用户名可以为你的邮箱，需要自行添加
MAIL_USERNAME = '379869029@qq.com'
# 邮箱密码，不是邮箱账号密码，而是第三方客户端登录使用的授权码，需要自行获取
MAIL_PASSWORD = 'vrxiqciwjnbcxxxx'
# 发送者即你的邮箱，需要自行添加
MAIL_DEFAULT_SENDER = '379869029@qq.com'

# 云片APIKEY
YP_API = 'edf71361381f31b3957beda37f20xxxx'

# 七牛云存储配置
QINIU_ACCESS_KEY = '_PL7p4sTSlfAKl71hIkuG3F6y18681ZKkNJ3xxxx'
QINIU_SECRET_KEY = '4P09jodUqhhEqIOcaqT9daVFmKjCJI3l6gsAxxxx'
QINIU_BUCKET_NAME = 'bbs-images'
QINIU_BUCKET_DOMAIN = 'qcquu7fd2.bkt.clouddn.com'

# 分页配置
PER_PAGE = 10
CMS_PER_PAGE = 15

# celery配置项
CELERY_RESULT_BACKEND = 'redis://127.0.0.1:6379/0'
CELERY_BROKER_URL = 'redis://127.0.0.1:6379/0'
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748
```

views.py修改如下：

```python
from flask import Blueprint, render_template, views, request, redirect, url_for, session, g
from flask_mail import Message
from flask_paginate import Pagination, get_page_parameter
from sqlalchemy import or_

from apps.cms.forms import LoginForm, ResetPwdForm, ResetEmailForm, AddBannerForm, UpdateBannerForm, AddBoardForm, UpdateBoardForm
from apps.cms.models import CMSUser, CMSPermission, BannerModel, BoardModel, HighlightPostModel
from apps.front.models import PostModel
from exts import db, mail
from utils import restful, random_captcha, clcache
from .decorators import permission_required  # .代表当前路径
from task import send_mail_celery
import config

cms_bp = Blueprint('cms', __name__, url_prefix='/cms')

from .hooks import before_request

@cms_bp.route('/')
# @login_required
def index():
    return render_template('cms/cms_index.html', max_role=g.max_role)


@cms_bp.route('/logout/')
def logout():
    # 清空session
    del session['cms_user_id']
    # 重定向到登录页面
    return redirect(url_for('cms.login'))


@cms_bp.route("/profile/")
def profile():
    return render_template("cms/cms_profile.html", max_role=g.max_role)


class LoginView(views.MethodView):
    def get(self, message=None):
        return render_template('cms/cms_login.html', message=message)

    def post(self):
        login_form = LoginForm(request.form)
        # 判断表单是否验证
        if login_form.validate():
            # 获取数据
            email = login_form.email.data
            password = login_form.password.data
            remember = login_form.remember.data
            user = CMSUser.query.filter_by(email=email).first()
            # 根据用户验证密码是否正确
            if user and user.check_password(password):
                # 将用户id记录入session
                session['cms_user_id'] = user.id
                # 如果记住密码，需要持久化
                if remember:
                    session.permanent = True
                # 登录成功，则跳转到后台首页
                return redirect(url_for('cms.index'))
            else:
                return self.get(message='邮箱或密码有误')
        else:
            print(login_form.errors)
            return self.get(message=login_form.get_error())


class ResetPwdView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetpwd.html', max_role=g.max_role)

    def post(self):
        form = ResetPwdForm(request.form)
        if form.validate():
            oldpwd = form.oldpwd.data
            newpwd = form.newpwd.data
            # 获取当前用户
            user = g.cms_user
            if user.check_password(oldpwd):
                # 密码验证通过，则修改密码
                user.password = newpwd
                db.session.commit()
                return restful.success()
            else:
                return restful.params_error(message='旧密码错误')
        else:
            # 当给Ajax返回数据时，要返回json格式的数据
            return restful.params_error(message=form.get_error())


class ResetEmailView(views.MethodView):
    def get(self):
        return render_template('cms/cms_resetemail.html', max_role=g.max_role)

    def post(self):
        form = ResetEmailForm(request.form)
        if form.validate():
            email = form.email.data
            # 修改用户邮箱
            g.cms_user.email = email
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(form.get_error())


class EmailCaptchaView(views.MethodView):
    def get(self):
        email = request.args.get('email')
        if not email:
            return restful.params_error('请传递邮箱参数')
        # 发送邮件验证码，可以是4位或6位的数字与英文组合
        captcha = random_captcha.get_random_captcha(4)
        try:
            send_mail_celery.delay('熊熊论坛验证码', recipients=[email], body='您正在进行更改邮箱验证，验证码是%s，5分钟内有效，请及时输入、注意保密。' % captcha)
        except Exception as e:
            print(e.args[0])
            return restful.server_error('邮件发送异常，请检查重试')
        clcache.save_captcha(email, captcha)
        return restful.success(message='邮件发送成功，请注意接收验证码')


@cms_bp.route('/posts/')
@permission_required(CMSPermission.POSTER)
def posts():
    query_obj = PostModel.query.filter(or_(PostModel.is_delete == 0, PostModel.is_delete == None))
    # posts = query_obj.all()
    page = request.args.get(get_page_parameter(), type=int, default=1)
    start = (page - 1) * config.CMS_PER_PAGE
    end = start + config.CMS_PER_PAGE
    posts = query_obj.slice(start, end)
    total = query_obj.count()
    pagination = Pagination(page=page, total=total, bs_version=4, per_page=config.CMS_PER_PAGE)
    return render_template('cms/cms_posts.html', max_role=g.max_role, posts=posts, pagination=pagination)


@cms_bp.route('/hpost/', methods=['POST'])
@permission_required(CMSPermission.POSTER)
def hpost():
    post_id = request.form.get('post_id')
    if not post_id:
        return restful.params_error(message='数据提交有误')
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        highlight = HighlightPostModel()
        highlight.post = post
        db.session.add(highlight)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error('该文章去火星啦😀')


@cms_bp.route('/uhpost/', methods=['POST'])
@permission_required(CMSPermission.POSTER)
def uhpost():
    post_id = request.form.get('post_id')
    if not post_id:
        return restful.params_error(message='数据提交有误')
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        highlight = HighlightPostModel.query.filter_by(post_id=post_id).first()
        db.session.delete(highlight)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error('该文章去火星啦😀')


@cms_bp.route('/dpost/', methods=['POST'])
@permission_required(CMSPermission.POSTER)
def dpost():
    post_id = request.form.get('post_id')
    if not post_id:
        return restful.params_error(message='数据提交有误')
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        post.is_delete = 1
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error('该文章去火星啦😀')


@cms_bp.route('/banners/')
@permission_required(CMSPermission.BANNER)
def banners():
    banners = BannerModel.query.filter(or_(BannerModel.is_delete == 0, BannerModel.is_delete == None)).order_by(BannerModel.priority.desc()).all()
    return render_template('cms/cms_banners.html', max_role=g.max_role, banners=banners)


@cms_bp.route('/abanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def abanner():
    form = AddBannerForm(request.form)
    if form.validate():
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel(name=name, image_url=image_url, link_url=link_url, priority=priority)
        db.session.add(banner)
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message=form.get_error())


@cms_bp.route('/ubanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def ubanner():
    # 根据id修改数据
    form = UpdateBannerForm(request.form)
    if form.validate():
        banner_id = form.banner_id.data
        name = form.name.data
        image_url = form.image_url.data
        link_url = form.link_url.data
        priority = form.priority.data
        banner = BannerModel.query.get(banner_id)
        if banner and banner.is_delete != 1:
            banner.name = name
            banner.image_url = image_url
            banner.link_url = link_url
            banner.priority = priority
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='轮播图不存在')
    else:
        return restful.params_error(form.get_error())


@cms_bp.route('/dbanner/', methods=['POST'])
@permission_required(CMSPermission.BANNER)
def dbanner():
    banner_id = request.form.get('banner_id')
    if not banner_id:
        return restful.params_error(message='数据请求有误')
    banner = BannerModel.query.get(banner_id)
    if banner and banner.is_delete != 1:
        banner.is_delete = 1
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message='轮播图不存在')


@cms_bp.route('/comments/')
@permission_required(CMSPermission.COMMENTER)
def comments():
    return render_template('cms/cms_comments.html', max_role=g.max_role)


@cms_bp.route('/boards/')
@permission_required(CMSPermission.BOARDER)
def boards():
    boards = BoardModel.query.filter(or_(BoardModel.is_delete == 0, BoardModel.is_delete == None)).all()
    return render_template('cms/cms_boards.html', max_role=g.max_role, boards=boards)


@cms_bp.route('/aboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def aboard():
    form = AddBoardForm(request.form)
    if form.validate():
        name = form.name.data
        board = BoardModel(name=name)
        db.session.add(board)
        db.session.commit()
        return restful.success()
    else:
        restful.params_error(message=form.get_error())


@cms_bp.route('/uboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def uboard():
    form = UpdateBoardForm(request.form)
    if form.validate():
        board_id = form.board_id.data
        name = form.name.data
        board = BoardModel.query.get(board_id)
        if board and board.is_delete != 1:
            board.name = name
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='没有该板块')
    else:
        return restful.params_error(form.get_error())


@cms_bp.route('/dboard/', methods=['POST'])
@permission_required(CMSPermission.BOARDER)
def dboard():
    board_id = request.form.get('board_id')
    if not board_id:
        return restful.params_error(message='板块不存在')
    board = BoardModel.query.get(board_id)
    if board and board.is_delete != 1:
        board.is_delete = 1
        db.session.commit()
        return restful.success()
    else:
        return restful.params_error(message='板块不存在')


@cms_bp.route('/fusers/')
@permission_required(CMSPermission.FRONTUSER)
def fusers():
    return render_template('cms/cms_fusers.html', max_role=g.max_role)


@cms_bp.route('/cusers/')
@permission_required(CMSPermission.CMSUSER)
def cusers():
    return render_template('cms/cms_cusers.html', max_role=g.max_role)


@cms_bp.route('/croles/')
@permission_required(CMSPermission.ADMINER)
def croles():
    return render_template('cms/cms_croles.html', max_role=g.max_role)


cms_bp.add_url_rule('/login/', view_func=LoginView.as_view('login'))
cms_bp.add_url_rule('/resetpwd/', view_func=ResetPwdView.as_view('resetpwd'))
cms_bp.add_url_rule('/resetemail/', view_func=ResetEmailView.as_view('resetemail'))
cms_bp.add_url_rule('/email_captcha/', view_func=EmailCaptchaView.as_view('email_captcha'))

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206207208209210211212213214215216217218219220221222223224225226227228229230231232233234235236237238239240241242243244245246247248249250251252253254255256257258259260261262263264265266267268269270271272273274275276277278279280281282283284285286287288289290291292293294295296297298299300301302303304305306307308309310311312313314315316317318319320321322323324325326327328329330
```

进行测试如下：
![flask cms send mail celery](https://img-blog.csdnimg.cn/20200705202744696.gif#pic_center)

显然，此时也可以正常发送邮件。

说明：
在task.py中使用bbs.py中的Flask对象app会产生循环引用的问题，为了避免这个问题，因此在task.py中重新初始化一个Flask对象，来舒适化Celery对象。

## 四、帖子的阅读和评论

### 1.阅读数的实现

在不考虑IP和频繁访问的前提下，阅读数可以通过直接在模型中增加一个字段read_count来实现，只要请求了一次视图函数对应的路由，该字段即加1。

apps/front/models.py中的PostModel修改如下：

```python
class PostModel(db.Model):
    __tablename__ = 'post'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    title = db.Column(db.String(100), nullable=False)
    content = db.Column(db.Text, nullable=False)
    content_html = db.Column(db.Text)
    create_time = db.Column(db.DateTime, default=datetime.now)

    board_id = db.Column(db.Integer, db.ForeignKey("cms_board.id"))
    author_id = db.Column(db.String(40), db.ForeignKey("front_user.id"))
    # 1表示被删除，0表示未删除，默认为0
    is_delete = db.Column(db.Integer, default=0)
    read_count = db.Column(db.Integer, default=0)

    board = db.relationship("BoardModel", backref='posts')
    author = db.relationship("FrontUser", backref='posts')

    def on_changed_content(target, value, oldvalue, initiator):
        '''实现markdown转HTML并进行安全验证'''
        # 允许上传的标签
        allowed_tags = ['a', 'abbr', 'acronym', 'b', 'blockquote', 'code', 'em', 'i', 'li', 'ol', 'pre', 'strong', 'ul',
                        'h1', 'h2', 'h3', 'p', 'img', 'video', 'div', 'iframe', 'p', 'br', 'span', 'hr', 'src', 'class']

        # 允许上传的属性
        allowed_attrs = {'*': ['class'],
                         'a': ['href', 'rel'],
                         'img': ['src', 'alt']}

        # 目标设置
        target.content_html = bleach.linkify(
            bleach.clean(markdown(value, output_format='html'), tags=allowed_tags, strip=True,
                         attributes=allowed_attrs))
1234567891011121314151617181920212223242526272829303132
```

apps/front/views中的视图函数修改如下：

```python
@front_bp.route('/p/<post_id>/')
def post_detail(post_id):
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        # 阅读数增加
        post.read_count += 1
        db.session.commit()
        return render_template('front/front_pdetail.html', post=post)
    else:
        return restful.params_error(message='文章不存在')
12345678910
```

模板front_index.html修改如下：

```html
<p class="post-info">
    <span>作者：{{ post.author.username | truncate(6) }}</span>
    <span>发表时间：{{ post.create_time }}</span>
    <span>阅读：{{ post.read_count }}</span>
</p>
12345
```

front_pdetail.html修改如下：

```html
<p class="post-info-group">
    <span>发表时间：{{ post.create_time }}</span>
    <span>作者：{{ post.author.username }}</span>
    <span>所属板块：{{ post.board.name }}</span>
    <span>阅读数：{{ post.read_count }}</span>
</p>
123456
```

演示如下：
![flask front read count add](https://img-blog.csdnimg.cn/20200705202809665.gif#pic_center)

此时，显然可以看到，每刷新一次阅读数都会加1.

### 2.评论数的实现

现在实现评论数，有两种方式实现：
一是增加一个字段用于保存评论数，每增加一条评论，该字段就加1，删除评论则减1；
二是直接使用联合查询，对某一帖子进行查询并计数。

这里采用第1种方式，即新增一个字段comment_count，如下：

```python
class PostModel(db.Model):
    __tablename__ = 'post'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    title = db.Column(db.String(100), nullable=False)
    content = db.Column(db.Text, nullable=False)
    content_html = db.Column(db.Text)
    create_time = db.Column(db.DateTime, default=datetime.now)

    board_id = db.Column(db.Integer, db.ForeignKey("cms_board.id"))
    author_id = db.Column(db.String(40), db.ForeignKey("front_user.id"))
    # 1表示被删除，0表示未删除，默认为0
    is_delete = db.Column(db.Integer, default=0)
    read_count = db.Column(db.Integer, default=0)
    like_count = db.Column(db.Integer, default=0)
    comment_count = db.Column(db.Integer, default=0)

    board = db.relationship("BoardModel", backref='posts')
    author = db.relationship("FrontUser", backref='posts')
123456789101112131415161718
```

front_pdetail.html如下：

```html
<p class="post-info-group">
    <span>发表时间：{{ post.create_time }}</span>
    <span>作者：{{ post.author.username }}</span>
    <span>所属板块：{{ post.board.name }}</span>
    <span>阅读数：{{ post.read_count }}</span>
    <span>评论数：{{ post.comment_count }}</span>
</p>
1234567
```

front_index.html如下：

```html
<p class="post-info">
    <span>作者：{{ post.author.username | truncate(6) }}</span>
    <span>发表时间：{{ post.create_time }}</span>
    <span>阅读：{{ post.read_count }}</span>
    <span>点赞：{{ post.like_count }}</span>
    <span>评论：{{ post.comment_count }}</span>
</p>
1234567
```

视图函数修改如下：

```python
@front_bp.route('/acomment/', methods=['POST'])
@login_required
def add_comment():
    form = AddCommentForm(request.form)
    if form.validate():
        content = form.content.data
        post_id = form.post_id.data
        post = PostModel.query.get(post_id)
        if post and post.id != 1:
            comment = CommentModel(content=content)
            comment.post = post
            comment.commenter = g.front_user
            db.session.add(comment)
            post.comment_count += 1
            db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='该文章去火星啦😀')
    else:
        return restful.params_error(message=form.get_error())
1234567891011121314151617181920
```

进行测试如下：
![flask front comment count add](https://img-blog.csdnimg.cn/20200705202829356.gif#pic_center)

显然，在评论之后会自动增加评论数。

## 五、点赞功能的实现

点赞采用增加字段并且新建模型专门保存的方式。

模型如下：

```
class PostModel(db.Model):
    __tablename__ = 'post'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    title = db.Column(db.String(100), nullable=False)
    content = db.Column(db.Text, nullable=False)
    content_html = db.Column(db.Text)
    create_time = db.Column(db.DateTime, default=datetime.now)

    board_id = db.Column(db.Integer, db.ForeignKey("cms_board.id"))
    author_id = db.Column(db.String(40), db.ForeignKey("front_user.id"))
    # 1表示被删除，0表示未删除，默认为0
    is_delete = db.Column(db.Integer, default=0)
    read_count = db.Column(db.Integer, default=0)
    like_count = db.Column(db.Integer, default=0)
    comment_count = db.Column(db.Integer, default=0)

    board = db.relationship("BoardModel", backref='posts')
    author = db.relationship("FrontUser", backref='posts')


class PraiseModel(db.Model):
    __tablename__ = 'praise'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)

    post_id = db.Column(db.Integer, db.ForeignKey('post.id'))
    praiser_id = db.Column(db.String(40), db.ForeignKey('front_user.id'))

    post = db.relationship('PostModel', backref='praises')
    praiser = db.relationship('FrontUser', backref='praises')
1234567891011121314151617181920212223242526272829
```

模板修改如下：

```
{% extends 'front/front_base.html' %}

{% block title %}
    {{ post.title }}
{% endblock %}

{% block head %}
    <script src="{{ url_for('static', filename='ueditor/ueditor.config.js') }}"></script>
    <script src="{{ url_for('static', filename='ueditor/ueditor.all.min.js') }}"></script>
    <link rel="stylesheet" href="{{ url_for('static', filename='front/css/front_pdetail.css') }}">
    <link rel="stylesheet" href="{{ url_for('static', filename='praise/css/praise.css') }}">
    <script src="{{ url_for('static', filename='front/js/front_pdetail.js') }}"></script>
    <script>
        $(function () {
            $("#praise").click(function () {
                var praise_img = $("#praise-img");
                var text_box = $("#add-num");
                var praise_txt = $("#praise-txt");
                var num = parseInt(praise_txt.text());
                var post_id = $("#post-mh").attr("data-id");
                if (praise_img.attr("src") === ("/static/praise/images/yizan.png")) {
                    $(this).html("<img src='{{ url_for('static', filename='praise/images/zan.png') }}' id='praise-img' class='animation' />");
                    praise_txt.removeClass("hover");
                    text_box.show().html("<em class='add-animation'>-1</em>");
                    $(".add-animation").removeClass("hover");
                    num -= 1;
                    praise_txt.text(num);
                    clajax.post({
                        'url': '/praise/',
                        'data': {
                            'is_praised': 1,
                            'post_id': post_id
                        },
                        'success': function (data) {
                            if (data['code'] === 200) {
                            } else {
                                clalert.alertConfirm({
                                    'msg': '您还未登录，请登录后再点赞!!',
                                    'cancelText': '继续浏览',
                                    'confirmText': '先登录再点赞',
                                    'cancelCallback': function () {
                                    },
                                    'confirmCallback': function () {
                                        window.location = '/signin/';
                                    }
                                });
                            }
                        }
                    })
                } else {
                    $(this).html("<img src='{{ url_for('static', filename='praise/images/yizan.png') }}' id='praise-img' class='animation' />");
                    praise_txt.addClass("hover");
                    text_box.show().html("<em class='add-animation'>+1</em>");
                    $(".add-animation").addClass("hover");
                    num += 1;
                    praise_txt.text(num);
                    clajax.post({
                        'url': '/praise/',
                        'data': {
                            'is_praised': 0,
                            'post_id': post_id
                        },
                        'success': function (data) {
                            if (data['code'] === 200) {
                            } else {
                                clalert.alertConfirm({
                                    'msg': '您还未登录，请登录后再点赞!!',
                                    'cancelText': '继续浏览',
                                    'confirmText': '先登录再点赞',
                                    'cancelCallback': function () {
                                    },
                                    'confirmCallback': function () {
                                        window.location = '/signin/';
                                    }
                                });
                            }
                        }
                    })
                }
            });
        })
    </script>
{% endblock %}

{% block main_content %}
    <div class="main-container">
        <div class="cl-container">
            <div class="post-container">
                <h2 id="post-mh" data-id="{{ post.id }}">{{ post.title }}</h2>
                <p class="post-info-group">
                    <span>发表时间：{{ post.create_time }}</span>
                    <span>作者：{{ post.author.username }}</span>
                    <span>所属板块：{{ post.board.name }}</span>
                    <span>阅读数：{{ post.read_count }}</span>
                    <span>评论数：{{ post.comment_count }}</span>
                </p>
                <article class="post-content" id="post-content" data-id="{{ post.id }}">
                    {{ post.content_html | safe }}
                </article>
            </div>

            <div class="praise">
                {% if praise %}
                    <span id="praise"><img src="{{ url_for('static', filename='praise/images/yizan.png') }}"
                                           id="praise-img"/></span>
                {% else %}
                    <span id="praise"><img src="{{ url_for('static', filename='praise/images/zan.png') }}"
                                           id="praise-img"/></span>
                {% endif %}

                <span id="praise-txt">{{ post.like_count }}</span>
                <span id="add-num"><em>+1</em></span>
            </div>

            <div class="comment-group">
                <h3>评论列表</h3>
                <ul class="comment-list-group">
                    {% for commont in post.comments %}
                        <li>
                            <div class="avatar-group">
                                <img src="{{ url_for('static', filename='common/images/logo.png') }}"
                                     alt="">
                            </div>
                            <div class="comment-content">
                                <p class="author-info">
                                    <span>{{ commont.commenter.username }}</span>
                                    <span>{{ commont.create_time }}</span>
                                </p>
                                <p class="comment-txt">
                                    {{ commont.content | safe }}
                                </p>
                            </div>
                        </li>
                    {% endfor %}
                </ul>
            </div>
            <div class="add-comment-group">
                <h3>发表评论</h3>
                <script id="editor" type="text/plain" style="height:100px;"></script>
                <!--        <textarea name="content" id="comment" cols="99" rows="10"></textarea>-->
                <div class="comment-btn-group">
                    <button class="btn btn-primary" id="comment-btn">发表评论</button>
                </div>
            </div>
        </div>

        <div class="sm-container"></div>
    </div>
{% endblock %}

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150
```

视图函数修改如下：

```
@front_bp.route('/p/<post_id>/')
def post_detail(post_id):
    post = PostModel.query.get(post_id)
    if post and post.is_delete != 1:
        # 阅读数增加
        post.read_count += 1
        db.session.commit()
        if hasattr(g, 'front_user'):
            praise = PraiseModel.query.filter_by(post_id=post_id).filter_by(praiser_id=g.front_user.id).first()
            if praise:
                return render_template('front/front_pdetail.html', post=post, praise=1)
        return render_template('front/front_pdetail.html', post=post, praise=0)
    else:
        return restful.params_error(message='文章不存在')


@front_bp.route('/praise/', methods=['POST'])
def praise():
    if hasattr(g, 'front_user'):
        is_praise = request.form.get('is_praised', type=int, default=0)
        post_id = request.form.get('post_id', type=int, default=None)
        post = PostModel.query.get(post_id)
        if post and post.is_delete != 1:
            print('isd: ', is_praise)
            if is_praise:
                post.like_count -= 1
                praise = PraiseModel.query.filter_by(post_id=post_id).filter_by(praiser_id=g.front_user.id).first()
                db.session.delete(praise)
                db.session.commit()
            else:
                post.like_count += 1
                praise = PraiseModel(post_id=post_id, praiser_id=g.front_user.id)
                db.session.add(praise)
                db.session.commit()
            return restful.success()
        else:
            return restful.params_error(message='文章不存在')
    else:
        return redirect(url_for('front.signin'))
123456789101112131415161718192021222324252627282930313233343536373839
```

进行测试如下：
![flask front praise post](https://img-blog.csdnimg.cn/20200706112324551.gif#pic_center)

显然，此时可以对文章进行点赞，并且数据库会实施保存。

# Python全栈（八）Flask项目实战之13.项目部署

Flask项目（论坛BBS）已经基本结束，后期会不断完善，Github和Gitee代码**同步更新**：
https://github.com/PythonWebProject/Flask_BBS；
https://gitee.com/Python_Web_Project/Flask_BBS。

## 一、Ajax初印象

之前在项目中处理事件数据传输等时用到了Ajax，现在对其做一个进一步的认识。

Ajax是Async Javascript and XML的缩写，即**异步JavaScript和XML**，是指一种创建交互式、快速动态网页应用的网页开发技术，无需重新加载整个网页的情况下、能够**更新部分网页**的技术。
XML是一种数据传输格式，类似于HTML元素是成对出现的；
现在用的较多的数据传输格式时Json。

新建一个文件夹，在其中创建flask_ajax.html如下：

```python
from flask import Flask, render_template, request, jsonify

app = Flask(__name__)


@app.route('/index/', methods=['GET', 'POST'])
def index():
    if request.method == 'GET':
        return render_template('index.html')
    else:
        return jsonify({'code': 200, 'data': 'success'})


if __name__ == '__main__':
    app.run(debug=True)

12345678910111213141516
```

在该目录下创建templates目录，下创建index.html如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    {# 导入Jquery #}
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.js"></script>
    <title>Ajax测试</title>
</head>
<body>
<button id="btn">点击</button>
</body>
</html>


<script>
    {# Jquery原生Ajax #}
    $('#btn').click(function () {
        $.ajax({
            type: 'post',
            url: '/index/',
            dataType: 'json',
            data: {
                'name': 'Corley',
                'age': 18
            },
            success: function (data) {
                alert(data['data'])
            },
            error: function (data) {
                alert(data)
            }
        })
    })
</script>
12345678910111213141516171819202122232425262728293031323334
```

进行测试如下：
![Ajax test](https://img-blog.csdnimg.cn/20200706182131827.gif#pic_center)

当前台使用Ajax传递数据到`/index/`路由后，后台视图函数接收到，返回json格式的数据，前台进行处理，这里是弹出。

使用Ajax的最大优点是它可以在不更新整个页面的情况下维护数据，这使得Web应用程序能够更快地响应用户的操作，避免在网络上发送未更改的信息。

## 二、选购服务器和安装MySQL、Redis

### 1.使用前的准备

购买选择：
对于个人学习和项目测试来说，服务器配置要求不需要太高，1核1G就可以使用，现在市面上很多服务器提供商均可选择，在系统上一般选择**CentOS**，在选择之后需要设置密码，即之后的登录密码，购买成功后实例可能会默认开启，如果没开启需要开启。除此之外，还需要进行安全组设置，在实例的设置中即可找到，通过**安全组设置**开放常用的端口，常见的需要开放的端口如下：

- 80/80
- 3306/3306
- 6379/6379
- 23/23
- 443/433
- 22/22
- 3389/3389

在进行下一步操作之前，一般需要**拍一个快照**，即保存服务器的**初始状态**，如果在遇到难以解决的问题时，可以返回初始状态重新开始。

服务器连接选择**XShell**，新建会话，输入主机IP和密码即可连接进入命令行。

### 2.Python、MySQL和Redis的安装和配置

以下都是在XShell连接到服务器后，在命令行中操作的。

#### Python的安装

Python的安装不是必需的，因为CentOS一般自带Python2和Python3，也可以根据自己的需要选择安装，以安装Python3.7为例进行说明：
（1）安装依赖包

```shell
yum install opensll-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel gcc
gcc-c++ opensll-devel libffi-devel python-devel mariadb-devel
12
```

（2）下载Python源码并解压

```shell
wget https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tgz
tar -xzvf Python-3.7.3.tgz -C /tmp
cd /tmp/Python-3.7.3
123
```

（3）把Python3.7安装到 /usr/local目录

```shell
./configure --prefix=/usr/local
make
make altinstall
123
```

（4）更改/usr/bin/python链接

```shell
ln -s /usr/local/bin/python3.7 /usr/bin/python3
ln -s /usr/local/bin/pip3.7 /usr/bin/pip3
12
```

#### MySQL的安装和配置

（1）下载 MySQL yum包

```shell
wget http://repo.mysql.com/mysql57-community-release-el7-10.noarch.rpm
1
```

（2）安装MySQL源

```shell
rpm -Uvh mysql57-community-release-el7-10.noarch.rpm
1
```

（3）安装MySQL服务端

```shell
yum install -y mysql-community-server
1
```

（4）启动MySQL并检查是否启动成功

```shell
systemctl start mysqld.service
systemctl status mysqld.service
12
```

（5）获取临时密码并连接修改密码
MySQL5.7为root用户随机生成了一个密码，需要通过命令获取密码并连接，并修改为自己的密码。因为MySQL的密码规则需要很复杂，我们个人使用时一般不需要这样设置，所以需要进行全局修改。

```shell
# 获取临时密码
grep 'temporary password' /var/log/mysqld.log

# 通过临时密码连接
mysql -uroot -p

# 全局设置并修改密码
set global validate_password_policy=0;
set global validate_password_length=1;
ALTER USER 'root'@'localhost' IDENTIFIED BY 'yourpassword';
12345678910
```

（6）授权其他机器远程登录

```shell
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'yourpassword' WITH GRANT OPTION;
FLUSH PRIVILEGES;
12
```

（7）设置MySQL的字符集并重启

```shell
vim /etc/my.cnf

# 切换为输入模式并添加内容
[mysql]
default-character-set=utf8

systemctl restart mysqld.service
1234567
```

（8）查看MySQL运行状态

```shell
ps -aux|grep mysql
1
```

（9）开始使用

```shell
create database flask_bbs charset=utf8;
use flask_bbs;
...
123
```

#### 安装Redis

（1）安装Redis

```shell
yum install redis
systemctl start redis
12
```

（2）连接Redis

```shell
redis-cli
1
```

## 三、上传代码到服务器

### 1.安装虚拟环境

可选，如果在当前服务器只有一个Python项目，可以不用安装修环境；
如果有多个项目，因为不同项目对依赖库的版本等要求不一定相同，因此**建议安装**虚拟环境。

命令为：

```shell
pip3 install pipenv
mkdir project
cd project
pipenv shell
1234
```

退出依赖环境直接用`deactivate`命令即可。

### 2.上传项目

有两种方式上传代码：
一是使用工具**FileZilla**进行文件上传；
二是使用Git将代码上传带Github、Gitee等托管平台，再在服务器上pull代码。

使用FileZilla时先连接服务器，再在本地选择文件上传到服务器即可。

代码上传之后，需要在服务器上安装所需要的库：
先在本地的项目中将环境导出，以便在服务器上安装：

```shell
pip freeze > requirements.txt
1
```

将文件上传到服务器上后，在虚拟环境中安装环境：

```shell
pip install -r requirements.txt
1
```

### 3.启动项目

在启动项目前需要映射数据库，尽量删除migrations文件夹并**重新映射**，命令如下：

```shell
# 删除原有的映射文件
rm -rf migrations/ 
python manage.py db init
python manage.py db migrate
python manage.py db upgrade
12345
```

还可以将项目测试时产生的数据上传到服务器（生产环境中不需要），需要先将MySQL数据导出到本地，示意如下：
![flask data localize](https://img-blog.csdnimg.cn/20200706182232451.gif#pic_center)

然后将其上传到服务器（root目录下即可），并在服务器中的MySQL命令行中执行命令`source /root/bbs.sql`即将数据复制到服务器中。

在运行之前，需要修改主程序文件app运行的host，即为`app.run(host='0.0.0.0')`，才能让用户访问。在项目运行之后，用户直接输入服务器外网IP和端口号即可访问论坛首页。

但是存在一个问题，直接这么运行，因为Flask对并发的支持不太好，因此如果访问量增大时，服务器很容易出现崩溃的情况，这个项目会显得很脆弱。

## 四、Nginx和uwsgi的使用

实际生产中部署项目会用到Nginx和uwsgi，来提高项目的并发性能。

### 1.uwsgi的安装和使用

uwsgi是一个使用Python编写的应用服务器，可直接通过`pip3 install uwsgi`安装。**非静态文件**的网络请求必须通过它完成，它也可以充当静态文件服务器（虽然不是它的强项）。注意，uwsgi必须安装在**系统级别**的Python环境中，不要安装到虚拟环境中。

在项目目录下创建uwsgi.ini如下：

```bash
[uwsgi]
# 必须全部为绝对路径
# 项目的路径
chdir = /root/project/bbs/
# flask的wsgi文件
wsgi-file = /project/bbs/bbs.py
# 回调的app对象
callable = app
# Python虚拟环境的路径 pipenv --venv命令进入到虚拟环境,目录里面执行
home = /root/.local/share/virtualenvs/project--xxxxxxx
# 进程相关的设置
# 主进程
master = true
# 最大数量的工作进程
processes = 10
socket = :5000
# 设置socket的权限
chmod-socket = 666
# 退出的时候是否清理环境
vacuum = true
1234567891011121314151617181920
```

uwsgi需要**gcc编译环境**，可通过命令`yum install -y gcc* pcre-devel openssl-devel`进行安装。

然后执行`uwsgi --ini uwsgi.ini`运行项目，如果没有报错，再访问`http://服务器IP:5000`，如果可以访问到页面就说明uwsgi配置成功。

### 2.Nginx的配置和使用

虽然uwsgi可以可以达到部署项目的目的，但还是需要采用nginx来作为Web服务器。

使用nginx来作为web服务器有以下好处：

- uwsgi对静态文件资源处理并不好，包括**响应速度、缓存**等；
- nginx作为专业的web服务器，暴露在公网上会比uwsgi更安全；
- 运维起来更加方便，比如要将某些IP写入黑名单，nginx可以很方便地实现，而uwsgi可能还要写很多代码才能实现。

安装：
通过`yum install nginx`命令安装。

简单的操作命令如下：

- 启动

```shell
systemctl start nginx
1
```

- 关闭

```shell
systemctl stop nginx
1
```

- 重启

```shell
systemctl restart nginx
1
```

切换到`/etc/nginx/conf.d`目录下，新建bbs.conf如下：

```bash
upstream bbs{
    server 127.0.0.1:5000;
}

# 配置服务器
server {
    # 监听的端口号
    listen 80;
    # 域名
    server_name your_server_domain;
    charset utf-8;
    # 最大的文件上传尺寸
    client_max_body_size 75M;

    # 静态文件访问的url
    location /static {
        # 静态文件地址
        alias /root/project/bbs/static;
    }

    # 最后，发送所有非静态文件请求到flask服务器
    location / {
        uwsgi_pass 127.0.0.1:5000;
        # uwsgi_params文件地址
        include /etc/nginx/uwsgi_params;
    }
}
123456789101112131415161718192021222324252627
```

写文配置文件之后，可以检验是否设置成功，使用命令`nginx -t`或`service nginx configtest`进行测试，如果不报错，说明成功。每次修改完了配置文件，都要记得用命令`systemctl restart nginx`重启Nginx。

此时访问项目只需要IP、而不再需要端口号了；
如果没有加载出样式，可能是nginx的配置文件设置有问题，`cd ..`，并`vim nginx.conf`，将`use nginx`改为`user root`，保存退出并**重启nginx服务**即可。

项目部署大致原理如下：
![Flask项目部署](https://img-blog.csdnimg.cn/20200706182325940.jpg#pic_center)

有了Nginx和uwsgi后，客户端访问项目是不再直接访问项目本身，而是通过Nginx转发端口，处理静态资源文件的请求，并且**提高并发能力**；而uwsgi负责协议转换和通信，提高性能。从而整体上提高了项目抗负载的能力，提高了对高访问量的应对能力。