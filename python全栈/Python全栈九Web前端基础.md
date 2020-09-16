# Python全栈（九）Web前端基础之1.Web前端介绍和HTML基本标签

## 一、前端和HTML介绍

### 1.前端介绍

在web1.0时代的网页多是**静态网页**，还没有前端的概念，内容比较单一，只能看到基本的文字和图片功能，但是缺乏**交互性**。
在2005年左右开始，进入web2.0时代，出现了动态页面与静态页面混合的场景，可以进行登录、评论等多种类型的操作。

前端最基础的范围包括**HTML结构**、**CSS样式**和**JavaScript**行为三类。

### 2.HTML介绍

HTML即**超文本标记语言**(Hyper Text Markup Language)，是W3C组织推荐使用的一个国际标准，是一种用来制作超文本文档的简单标记语言。

不同的格式用不同的标签来标记，具有不同的语义。

HTML的发展如下：
![HTML发展](https://img-blog.csdnimg.cn/20200810122501706.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

HTML的特点如下：

- 只是标记语言而不是编程语言，所以直接由浏览器执行即可。
- 超文本文件：
  超文本是指页面内可以包含图片、链接，甚至音乐、程序等非文字元素。
- 大小写不敏感。
- html或htm为后缀。

### 3.编辑器与插件的安装

前端编辑器可以选择HBuilder、Sublime、WebStorm、VSCode等。

以Sublime为例，可点击https://download.csdn.net/download/CUFEECR/12699886或http://www.sublimetext.com/3下载Sublime Text 3的安装包，安装完成后，为了更高效地开发，需要安装一些插件。

在打开的sublime界面，未安装程序包控制时如下：
![sublime 未安装程序包控制](https://img-blog.csdnimg.cn/20200810122516176.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

在Tools栏直接点击最后一个即可自动安装，安装完成会提示安装成功，再查看如下：
![sublime 已安装程序包控制](https://img-blog.csdnimg.cn/20200810122532570.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
显然，此时Preferences栏多了Package Settings和Package Control。

此时再点击快捷键`Ctrl+Shift+P`，在弹框中输入Install Package，然后等待安装，直到弹出输入框，此时直接输入插件名如Emmet、JsFormat、SideBarEnhancements、CssComb、Autoprefixer等即可完成插件安装。

## 二、HTML结构

一个最基本的HTML代码结构如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!-- 不显示在网页中 -->
</head>
<body>
    <!-- 显示在网页中 -->
</body>
</html>
123456789101112
```

其中，`<!DOCTYPE html>`用来声明这是一个HTML文档，而不是标签；
`<html lang="en">`用来标明HTML文件；
`<head>`用来标明头部；
`<body>`用来标明主体部分。

标签是由尖括号包围关键字组成的，
可以看到，标签通常是**成对出现**的，包括开始标签和结束标签，形式为`<开始标签></结束标签>`，称为**双标签**；
也有单标签，如`<hr/>`、`<br>`、`<meta>`等，也称为**自闭合标签**；
标签与包裹在其中的内容共同构成元素，标签还可以定义很多属性。

## 三、基础标签

### 1.标题标签

标题标签为h1-h6，测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML基础</title>
</head>

<body>
    <h1>我是h1</h1>
    <h2>我是h2</h2>
    <h3>我是h3</h3>
    <h4>我是h4</h4>
    <h5>我是h5</h5>
    <h6>我是h6</h6>
</body>

</html>
12345678910111213141516171819
```

显示如下：
![标题标签](https://img-blog.csdnimg.cn/20200810122559848.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

显然，不同级别的标题标签有不同的样式。

### 2.段落标签

一个p标签代表一个段落，语法如下：

```html
<p align=''></p>
1
```

align属性表示文字对齐方式，常见的有left（左对齐）、right（右对齐）、center（居中对齐）和justify（两端对齐）。

进行测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML基础</title>
</head>

<body>
    <h1>我是标题</h1>
    <p align='center'>我是小妖怪，逍遥又自在。</p>
    <p align='right'>杀人不眨眼，吃人不放盐。</p>
    <p align='left'>一口七八个，肚子要撑破。</p>
    <p align='justify'>茅房去拉屎，想起忘带纸。</p>
</body>

</html>
123456789101112131415161718
```

效果如下：
![p标签](https://img-blog.csdnimg.cn/20200810122615524.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

需要注意，在p标签内加入`<br>`标签，只是换行，并没有将原来的一个段落变成两个段落；
空格在p标签中不起作用，` `代表空格才能有效果。

要想在网页中保留在html代码中的保留**空格和换行符**等文字格式，可以使用pre标签，如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML基础</title>
</head>

<body>
    <h1>我是标题</h1>
    <p align='center'>我是小妖怪，逍遥又自在。</p>
    <p align='right'>杀人不眨眼，吃人不放盐。</p>
    <p align='left'>一口七八个，肚子要撑破。</p>
    <p align='justify'>茅房去拉屎，想起忘带纸。</p>
    <pre>我是小妖怪，
        逍遥又自在。茅房去拉屎，
        想起忘带纸。
杀人不眨眼，吃人不放盐。
    </pre>
</body>

</html>
1234567891011121314151617181920212223
```

显示：
![pre标签](https://img-blog.csdnimg.cn/20200810122638579.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 3.水平线标签

`<hr/>`，单标签。

### 4.修饰标签

文字斜体`<i></i>`、`<em></em>`；
加粗`<b></b>`、`<strong></strong>`；
下标`<sub>`；
上标`<sup>`；
下划线`<ins>`；
删除线`<del>`。

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML基础</title>
</head>

<body>
    <h1>我是标题</h1>
    <p>我是小妖怪，逍遥又自在。</p>
    <hr>
    <p><i>杀人不眨眼，</i>吃人不放盐。</p>
    <p><strong>一口七八个，</strong>肚子要撑破。</p>
    <p>茅房去拉屎，想起忘带纸。</p>
    <p>2H<sub>2</sub> + O<sub>2</sub> = 2H<sub>2</sub>O</p>
    <p>1<sup>2</sup> + 2<sup>2</sup> = 5</p>
    <p><ins>我是小妖怪，</ins>逍遥又自在。</p>
    <p><del>杀人不眨眼，</del>吃人不放盐。</p>
</body>

</html>
1234567891011121314151617181920212223
```

效果如图：
![水平线标签和修饰标签](https://img-blog.csdnimg.cn/20200810122724882.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 5.常用特殊符号

| HTML符号 | 意义   |
| -------- | ------ |
| &lt;     | <      |
| &gt;     | >      |
| &nbsp;   | 空格   |
| &reg;    | 已注册 |
| &copy;   | 版权   |
| &trade;  | 商标   |

标签字符是不会显示在网页中的，要想其显示在网页中，要使用大于、小于等特殊符号。

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML基础</title>
</head>

<body>
    <h1>我是标题</h1>
    <p>我是小妖怪，逍遥又自在。</p>
    <hr>
    <p><i>杀人不眨眼，</i>吃人不放盐。</p>
    <p>&lt;strong&gt;一口七八个，&lt;/strong&gt;肚子要撑破。</p>
    <p>茅房去拉屎，想起忘带纸。</p>
    <p>已注册&reg;</p>
</body>

</html>
1234567891011121314151617181920
```

效果如下：
![特殊符号](https://img-blog.csdnimg.cn/20200810122807706.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 6.列表标签

列表标签分为有序列表、无序列表和定义列表。
（1）无序列表
用ul（Unordered list）标签定义，格式如下：

```html
<ul type=''>
    <li></li>
    <li></li>
</ul>
1234
```

其中type属性表示列表项样式类型，disc为原点（默认），square为正方形，circle为空心圆。

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML基础</title>
</head>

<body>
    <h1>我是标题</h1>
    <hr>
    <ul type='disc'>
        <li>disc样式1</li>
        <li>disc样式2</li>
    </ul>
    <ul type='square'>
        <li>square样式1</li>
        <li>square样式2</li>
    </ul>
    <ul type='circle'>
        <li>circle样式1</li>
        <li>circle样式2</li>
    </ul>
</body>

</html>
123456789101112131415161718192021222324252627
```

显示：
![无序列表标签](https://img-blog.csdnimg.cn/20200810122822702.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

（2）有序列表
用ol（Ordered list）标签定义，格式如下：

```html
<ul type=''>
    <li></li>
    <li></li>
</ul>
1234
```

其中type属性表示编号样式类型，属性和取值意义如下：

| type属性 | 取值意义     |
| -------- | ------------ |
| 1        | 数字（默认） |
| a        | 小写字母     |
| A        | 大写字母     |
| i        | 小写罗马字母 |
| I        | 大写罗马字母 |

进行测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML基础</title>
</head>

<body>
    <h1>我是标题</h1>
    <hr>
    <ol type='1'>
        <li>数字样式1</li>
        <li>数字样式2</li>
    </ol>
    <ol type='a'>
        <li>小写字母样式1</li>
        <li>小写字母样式2</li>
    </ol>
    <ol type='I'>
        <li>大写罗马字母样式1</li>
        <li>大写罗马字母样式2</li>
    </ol>
</body>

</html>
123456789101112131415161718192021222324252627
```

显示如下：
![有序列表](https://img-blog.csdnimg.cn/20200810122840868.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

（3）定义列表
用dl（Definition list）标签定义，常用在表示嵌套包含的列表中，其格式如下：

```html
<dl>
    <dt>定义列表项</dt>
    <dd>列表描述项</dd>
    <dd>列表描述项</dd>
    <dt>定义列表项</dt>
    <dd>列表描述项</dd>
</dl>
1234567
```

测试如下：

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML基础</title>
</head>

<body>
    <h1>我是标题</h1>
    <hr>
    <dl>
        <dt>历史</dt>
        <dd>秦汉</dd>
        <dd>隋唐</dd>
        <dd>明清</dd>
        <dt>地理</dt>
        <dd>中国</dd>
        <dd>世界</dd>
    </dl>
</body>

</html>
123456789101112131415161718192021222324
```

显示如下：
![定义列表](https://img-blog.csdnimg.cn/20200810122858167.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 7.图片标签

语法为：

```html
<img src="" alt="">
1
```

常见的属性和意义如下：

| 属性   | 意义                     |
| ------ | ------------------------ |
| src    | 显示图像地址             |
| alt    | 图像代替文本             |
| width  | 图像宽度（数值或百分比） |
| height | 图像宽度（数值或百分比） |

其中，百分比是相对于**父元素**（如果有父元素）或整个浏览器界面（无父元素）而言的；
对于src属性，可以使用图片文件的**相对路径**（相对于当前HTML文件的地址），也可以使用**绝对路径**（从盘符到图片文件的完整路径）；
如果图片在HTML文件的同级目录下，可以直接使用图片文件名，这是相对路径；
为了项目的可移植性等考虑，**尽量使用相对路径**；
除了本地图片文件，还可以使用图片链接。

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML基础</title>
</head>

<body>
    <h1>我是标题</h1>
    <hr>
    <img src="小姐姐.jpg" alt="小姐姐" height="50%">
    <br>
    <img src="https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png" alt="百度logo" width="300px">
</body>

</html>
123456789101112131415161718
```

需要在HTML文件的同级目录下保存一个文件图片`小姐姐.jpg`，显示如下：
![图片标签](https://img-blog.csdnimg.cn/2020081012321977.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 8.超链接标签

语法为：

```html
<a href=""></a>
1
```

常见的属性和意义如下：

| 属性   | 意义                            |
| ------ | ------------------------------- |
| href   | 链接地址                        |
| target | 目标窗口，取值有_self、_blank等 |
| title  | 链接提示文字                    |
| name   | 链接命名                        |

其中，href既可以是**站内链接**，也可以是**站外链接**；
name属性可以用来**定义锚**，定义同页面的锚语法如下：

```html
<a href="#锚名1">目录1</a>
<a href="#锚名2">目录2</a>

<a href="..." name="锚名1">内容</a>
<a href="..." name="锚名2">内容</a>
12345
```

定义不同页面的锚的语法如下：

```html
网页1: 
<a href="网页2名称#锚名">...</a>
网页2: 
<a name="锚名">...</a>
1234
```

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML基础</title>
</head>

<body>
    <h1>我是标题</h1>
    <!-- name属性定义锚实现同页面跳转 -->
    <a href="#Part1">第一部分</a>
    <a href="#Part2">第二部分</a>
    <!-- name属性定义锚实现不同页面跳转 -->
    <a href="image.html#girl">跳转到其他页</a>
    <hr>
    <!-- 站内跳转，当前窗口跳转 -->
    <a href="list.html" title="站内跳转">点我一下-站内跳转</a>
    <br>
    <!-- 站外跳转，新窗口跳转 -->
    <a href="https://www.baidu.com" target="_blank" title="站外跳转">点我一下-站外跳转</a>
    <a href="" name='Part1'><img src="小姐姐.jpg" alt="" height="2000px"></a>
    <br>
    <a href="" name='Part2'><img src="https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png" alt="" height="500px"></a>
</body>

</html>
12345678910111213141516171819202122232425262728
```

还需要在同级目录下创建list.html如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML基础</title>
</head>

<body>
    <h1>我是标题</h1>
    <hr>
    <dl>
        <dt>历史</dt>
        <dd>秦汉</dd>
        <dd>隋唐</dd>
        <dd>明清</dd>
        <dt>地理</dt>
        <dd>中国</dd>
        <dd>世界</dd>
    </dl>
</body>

</html>
123456789101112131415161718192021222324
```

新建image.html如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML基础</title>
</head>

<body>
    <h1>我是标题</h1>
    <hr>
    <a href="" name='girl'><img src="小姐姐.jpg" alt="" height="2000px"></a>
    <br>
    <a href="" name='baidu'><img src="https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png" alt="" height="500px"></a>
</body>

</html>
123456789101112131415161718
```

显示：
![tag a](https://img-blog.csdnimg.cn/20200810123128692.gif)

除此之外，还可以使用a标签**发送邮件**和**下载文件**，语法如下：

```html
电子邮件链接:
<a href="mailto:邮件地址">..</a>
文件下载:
<a href-"下载文件的地址">..</a>
1234
```

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML基础</title>
</head>

<body>
    <h1>我是标题</h1>
    <!-- name属性定义锚实现同页面跳转 -->
    <a href="mailto:123@123.com">点击发送邮件</a>
    <br>
    <a href="小姐姐.zip">点我下载</a>
</body>

</html>
123456789101112131415161718
```

显示：
![tag a mail download](https://img-blog.csdnimg.cn/20200810123144255.gif)

# Python全栈（九）Web前端基础之2.HTML高级标签和CSS介绍

## 一、HTML高级标签

### 1.div与span标签

div标签语法为：

```html
<div>内容</div>
1
```

与其内的内容组成块元素、表示一块内容，**没有具体的语义**，唯一的格式就是换行，**常结合CSS用于页面布局**。

如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div>你好</div>
    <div>你好</div>
</body>
</html>
123456789101112
```

显示：
![html div](https://img-blog.csdnimg.cn/20200815095740959.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

可以看到，每一个div元素都**占满一行**。

与p标签对比如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div>金樽清酒斗十千，玉盘珍羞直万钱。停杯投箸不能食，拔剑四顾心茫然。欲渡黄河冰塞川，将登太行雪满山。闲来垂钓碧溪上，忽复乘舟梦日边。行路难，行路难，多歧路，今安在？长风破浪会有时，直挂云帆济沧海。</div>
    <div>金樽清酒斗十千，玉盘珍羞直万钱。停杯投箸不能食，拔剑四顾心茫然。欲渡黄河冰塞川，将登太行雪满山。闲来垂钓碧溪上，忽复乘舟梦日边。行路难，行路难，多歧路，今安在？长风破浪会有时，直挂云帆济沧海。</div>
    <div>金樽清酒斗十千，玉盘珍羞直万钱。停杯投箸不能食，拔剑四顾心茫然。欲渡黄河冰塞川，将登太行雪满山。闲来垂钓碧溪上，忽复乘舟梦日边。行路难，行路难，多歧路，今安在？长风破浪会有时，直挂云帆济沧海。</div>
    <p>金樽清酒斗十千，玉盘珍羞直万钱。停杯投箸不能食，拔剑四顾心茫然。欲渡黄河冰塞川，将登太行雪满山。闲来垂钓碧溪上，忽复乘舟梦日边。行路难，行路难，多歧路，今安在？长风破浪会有时，直挂云帆济沧海。</p>
    <p>金樽清酒斗十千，玉盘珍羞直万钱。停杯投箸不能食，拔剑四顾心茫然。欲渡黄河冰塞川，将登太行雪满山。闲来垂钓碧溪上，忽复乘舟梦日边。行路难，行路难，多歧路，今安在？长风破浪会有时，直挂云帆济沧海。</p>
    <p>金樽清酒斗十千，玉盘珍羞直万钱。停杯投箸不能食，拔剑四顾心茫然。欲渡黄河冰塞川，将登太行雪满山。闲来垂钓碧溪上，忽复乘舟梦日边。行路难，行路难，多歧路，今安在？长风破浪会有时，直挂云帆济沧海。</p>
</body>
</html>
12345678910111213141516
```

显示：
![html div p](https://img-blog.csdnimg.cn/20200815095755857.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，因为div元素除了占行没有任何格式，而p标签有段落的格式，因此在页面中表现出来的也不一样，p标签形成的段落之间有空格。

span标签语法为：

```html
<span>内容</span>
1
```

与其内的内容形成行内元素、内容有多宽就占用多宽的空间距离，**没有具体的语义**，常用于**修改段落中的局部样式**。

测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div>金樽清酒斗十千，玉盘珍羞直万钱。停杯投箸不能食，拔剑四顾心茫然。欲渡黄河冰塞川，将登太行雪满山。闲来垂钓碧溪上，忽复乘舟梦日边。行路难，行路难，多歧路，今安在？长风破浪会有时，直挂云帆济沧海。</div>
    <div>金樽清酒斗十千，玉盘珍羞直万钱。停杯投箸不能食，拔剑四顾心茫然。欲渡黄河冰塞川，将登太行雪满山。闲来垂钓碧溪上，忽复乘舟梦日边。行路难，行路难，多歧路，今安在？长风破浪会有时，直挂云帆济沧海。</div>
    <div>
        <h1>span标签</h1>
        <p>金樽清酒斗十千，<span style="color: tomato;">玉盘珍羞直万钱</span>。停杯投箸不能食，拔剑四顾心茫然。欲渡黄河冰塞川，将登太行雪满山。闲来垂钓碧溪上，忽复乘舟梦日边。行路难，行路难，多歧路，今安在？长风破浪会有时，直挂云帆济沧海。</p>
    </div>
    
</body>
</html>
1234567891011121314151617
```

显示：
![html span](https://img-blog.csdnimg.cn/20200815095820340.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

可以看到，sapn标签可以定义局部样式。

### 2.表格标签

语法为：

```html
<table>
    <tr>
        <td>第一行第一列</td>
        <td>第一行第二列</td>
    </tr>
    <tr>
        <td>第二行第一列</td>
        <td>第二行第二列</td>
    </tr>
</table>
12345678910
```

其中的标签和意义如下：

| 标签   | 意义         |
| ------ | ------------ |
| table  | 表格         |
| tr     | 一行         |
| td(th) | 一列（表头） |

测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <table>
        <tr>
            <td>第一行第一列</td>
            <td>第一行</td>
        </tr>
        <tr>
            <td>第一列</td>
            <td>第二行第二列</td>
        </tr>
    </table>
    
</body>
</html>
123456789101112131415161718192021
```

显示：
![html table](https://img-blog.csdnimg.cn/20200815095849624.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

此时是没有表格边框的，要想显示边框，需要定义**border属性**。

table标签的常用属性及意义如下：

| table属性 | 意义                                                         |
| --------- | ------------------------------------------------------------ |
| border    | 表格边框（像素）                                             |
| width     | 表格宽度（像素）                                             |
| height    | 表格高度（像素）                                             |
| align     | 表格相对于父元素的对齐方式，取值分别为left（默认）、right、center |

测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <table border="1" width='200' height='80' align="center">
        <tr>
            <td>第一行第一列</td>
            <td>第一行</td>
        </tr>
        <tr>
            <td>第一列</td>
            <td>第二行第二列</td>
        </tr>
    </table>
    <table border="1" width='200' height='80'>
        <tr>
            <th>姓名</th>
            <th>性别</th>
        </tr>
        <tr>
            <td>张三</td>
            <td>男</td>
        </tr>
        <tr>
            <td>李四</td>
            <td>女</td>
        </tr>
    </table>
    
</body>
</html>
1234567891011121314151617181920212223242526272829303132333435
```

显示：
![html table border](https://img-blog.csdnimg.cn/2020081509593341.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，此时有更多的格式，包括边框、表格宽度、表格高度、表头格式、对齐方式等。

同时，单元格即td（th）也有一些属性：

| td（th）属性 | 意义                                |
| ------------ | ----------------------------------- |
| align        | 水平对齐，取值为left、right和center |
| valign       | 垂直对齐，取值为top、bottom和middle |
| colspan      | 水平合并                            |
| rowspan      | 垂直合并                            |

水平和垂直对齐测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <table border="1" width='500' height='200' align="center">
        <tr>
            <td>第一行第一列</td>
            <td>第一行</td>
        </tr>
        <tr>
            <td>第一列</td>
            <td>第二行第二列</td>
        </tr>
    </table>
    <table border="1" width='500' height='200'>
        <tr>
            <th align="center">姓名</th>
            <th  align="center" valign='top'>性别</th>
        </tr>
        <tr>
            <td align="center" valign='bottom'>张三</td>
            <td align="center">男</td>
        </tr>
        <tr>
            <td>李四</td>
            <td>女</td>
        </tr>
    </table>
    
</body>
</html>
1234567891011121314151617181920212223242526272829303132333435
```

显示：
![html table border td](https://img-blog.csdnimg.cn/20200815095945507.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

表格还可以跨行和跨列合并单元格，这是需要用到td（th）的**colspan和rowspan属性**，测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <table border="1">
        <tr>
            <td rowspan="2" width='300' height='50'>姓名</td>
            <td colspan="2" width='300' height='50'>性别</td>
            <!-- <td></td> -->
        </tr>
        <tr>
            <!-- <td></td> -->
            <td align='center'>男</td>
            <td align='center'>女</td>
        </tr>
        <tr>
            <td align='center'>Corley</td>
            <td align='center'>是</td>
            <td align='center'></td>
        </tr>
        <tr>
            <td align='center'>Jack</td>
            <td align='center'></td>
            <td align='center'>是</td>
        </tr>
    </table>
    
</body>
</html>
123456789101112131415161718192021222324252627282930313233
```

显示：
![html table span](https://img-blog.csdnimg.cn/20200815100033736.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，已经实现了表格的横向和纵向跨行。

### 3.表单标签

表单标签form标签一般用来与用户交互并提交数据，语法如下：

```html
<form>
    <label>用户名: </label>
    <input type="text">
    ...
</form>
12345
```

form标签有两个属性：
**action属性**定义表单数据提交地址(空的默认为提交到当前页)；
**method属性**定义表单提交方式(get/post)。

form标签可以定义许多子标签，包括label标签、input标签、textarea标签和select标签等。

label标签为表单定义文字注释，便于用户直观了解标签的作用；
for属性与label标签内部的表单标签的id名绑定，点击label区域激活该id标签。

input标签包含很多属性，type属性定义了input标签的类型，如下：

| input标签type属性 | 意义                                                         |
| ----------------- | ------------------------------------------------------------ |
| text              | 文本（明文）输入框                                           |
| password          | 密文输入框                                                   |
| radio             | 单选框，一般需要通过name属性来将指定的单选框捆绑到一组，如果要提交需要指定value属性 |
| checkbox          | 多选框，一般需要通过name属性来将指定的多选框捆绑到一组，如果要提交需要指定value属性 |
| submit            | 表单提交，可以通过value属性指定按钮显示值                    |
| reset             | 重置，可以通过value属性指定按钮显示值                        |
| file              | 文件上传                                                     |

textarea标签为多行文本输入框，语法如下：

```html
<textarea name="" id="" cols="30" rows="10"></textarea>
1
```

通常需要设置**cols和rows属性**，当不设置cols属性与rows属性时，文本框可以自动收缩，会影响布局；
提交时要设置name属性。

select标签为下拉框，语法为：

```html
<select name="site">
    <option value="0">1</option>
    <option value="1">2</option>
    <option value="2">3</option>
    <option value="3">4</option>
</select> 
123456
```

如果要提交数据，需要给select指定name属性、option指定value属性。

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <form action="" method="GET">
        <div>
            <label for="">用户名：</label>
            <input type="text" name="username">
        </div>
        <div>
            <label for="">密&nbsp;&nbsp;&nbsp;&nbsp;码：</label>
            <input type="password" name="password">
        </div>
        <div>
            <label for="">性&nbsp;&nbsp;&nbsp;&nbsp;别：</label>
            <input type="radio" name="gender">男
            <input type="radio" name="gender">女
        </div>
        <div>
            <label for="">民&nbsp;&nbsp;&nbsp;&nbsp;族：</label>
            <label for="major"><input type="radio" name="major" id="major">汉族</label>
            <label for="minor"><input type="radio" name="minor" id="minor">少数民族</label>
        </div>
        <div>
            <label for="">爱&nbsp;&nbsp;&nbsp;&nbsp;好：</label>
            <input type="checkbox" name="hobby">音乐
            <input type="checkbox" name="hobby">舞蹈
            <input type="checkbox" name="hobby">游戏
            <input type="checkbox" name="hobby">读书
            <input type="checkbox" name="hobby">爬山
        </div>
        <div>
            <input type="submit" value="点我一下">
        </div>
    </form>
    
</body>

</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445
```

演示如下：
![html form get](https://img-blog.csdnimg.cn/2020081510011784.gif#pic_center)

显然，在输入用户名和密码后，点击提交时，输入的值都作为参数传递到url中，但是密码等敏感信息也传递到url中显示出来会显得很不安全，所以可以使用POST方法。

再次测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <form action="" method="POST">
        <div>
            <label for="">用户名：</label>
            <input type="text" name="username">
        </div>
        <div>
            <label for="">密&nbsp;&nbsp;&nbsp;&nbsp;码：</label>
            <input type="password" name="password">
        </div>
        <div>
            <label for="">性&nbsp;&nbsp;&nbsp;&nbsp;别：</label>
            <input type="radio" name="gender" value="0">男
            <input type="radio" name="gender" value="1">女
        </div>
        <div>
            <label for="">民&nbsp;&nbsp;&nbsp;&nbsp;族：</label>
            <label for="major"><input type="radio" name="major" id="major" value="0">汉族</label>
            <label for="minor"><input type="radio" name="minor" id="minor" value="1">少数民族</label>
        </div>
        <div>
            <label for="">爱&nbsp;&nbsp;&nbsp;&nbsp;好：</label>
            <input type="checkbox" name="hobby" value="0">音乐
            <input type="checkbox" name="hobby" value="1">舞蹈
            <input type="checkbox" name="hobby" value="2">游戏
            <input type="checkbox" name="hobby" value="3">读书
            <input type="checkbox" name="hobby" value="4">爬山
        </div>
        <div>
            <label for="">简&nbsp;&nbsp;&nbsp;&nbsp;介：</label>
            <textarea name="" id="" cols="30" rows="10"></textarea>
        </div>
        <div>
            <label for="">城&nbsp;&nbsp;&nbsp;&nbsp;市：</label>
            <select name="site">
                <option value="0">北京</option>
                <option value="1">上海</option>
                <option value="2">广州</option>
                <option value="3">成都</option>
            </select>                
        </div>
        <div>
            <input type="submit" value="点我一下">
            <input type="reset" value="重置">
        </div>
        
    </form>
    
</body>

</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960
```

演示如下：
![html form post](https://img-blog.csdnimg.cn/20200815100130809.gif#pic_center)

显然，此时不再将数据显示到url中。

## 二、CSS基础

#### 1.CSS介绍

CSS全称**Cascading Style Sheets**，即**层叠样式表**，样式定义了HTML元素如何显示，样式通常又会存在于样式表中。
通常把HTML元素的样式都统一收集起来写在一个地方或一个CSS文件里。

CSS的作用如下：

- 美化网页
- 将HTML页面的内容与样式分离
- 提高Web开发的工作效率

#### 2.引入样式方式

#### 行内样式

给body中需要添加样式的标签添加style属性。

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>css引入</title>
    <!-- <link rel="stylesheet" href="css/main.css"> -->
    <style>
        h1 {
            color: tomato;
        }
    </style>
</head>

<body>
    <h1>CSS测试</h1>
    <div>
        <body>
            <a href="http://www.baidu.com" style="font-size:20px;color:chartreuse;">百度一下</a>
        </body>
    </div>
</body>

</html>
12345678910111213141516171819202122232425
```

显示：
![html css link](https://img-blog.csdnimg.cn/20200815100154793.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

#### 嵌入式

方式为在标签中添加style标签：

```html
<head>
    <meta charset="UTF-8">
    <title>html之css</title>
    <style>
        h1{
            color: tomato;
        }
    </style>
</head>
123456789
```

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>css引入</title>
    <!-- <link rel="stylesheet" href="css/main.css"> -->
    <style>
        h1{
            color: tomato;
        }
    </style>
</head>

<body>
    <h1>CSS测试</h1>
    <div>哪有什么岁月静好，不过是有人替你负重前行</div>
</body>

</html>
123456789101112131415161718192021
```

显示：
![html css insert](https://img-blog.csdnimg.cn/20200815100208242.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

#### 外联式

步骤为：
新建css为后缀的文件，并在文件中写css样式；
在标签中导入css文件如下：

```html
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="css文件相对路径">
    <title>css引入</title>
</head>
12345
```

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>css引入</title>
    <link rel="stylesheet" href="css/main.css">
</head>

<body>
    <h1>CSS测试</h1>
    <div>哪有什么岁月静好，不过是有人替你负重前行</div>
</body>

</html>
12345678910111213141516
```

在同级目录下创建css目录，下创建main.css如下：

```css
div{
    color: blueviolet;
}
123
```

此时显示：
![html css inline](https://img-blog.csdnimg.cn/20200815100219776.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，已经显示出样式。

在实际开发中，外联式最常使用，其次是嵌入式，最不建议使用的是行内样式。

# Python全栈（九）Web前端基础之3.CSS常见样式和选择器

## 一、CSS常见样式

### 1.常见文本样式

常用文本样式如下：

| 样式            | 意义和取值                                                   |
| --------------- | ------------------------------------------------------------ |
| color           | 文字颜色                                                     |
| font-size       | 文字大小                                                     |
| font-family     | 字体                                                         |
| font-style      | 是否倾斜，italic-倾斜、normal-正常(通常用来去倾斜)           |
| font-weight     | 是否加粗，bold-加粗、normal-正常                             |
| line-height     | 行高，即第二行底部到第一行底部的高度                         |
| text-decoration | 文本修饰，underline-下划线、overline-上划线、line-through文本穿过的线、none-默认无线 |
| text-indent     | 首行缩进 每个字体大小*缩进个数(仅对第一行有作用,可以取负值)  |

其中，font-style一般用于给i标签或em标签**去倾斜**；
font-weight一般用于给b标签或strong标签或标题标签**去加粗**；

颜色表示法一般有4种：

- 颜色名表示
  例如red。
- rgb表示
  例如`rgb(255,0,0)`表示红色。
- rgba表示
  相对于rgb来说加了透明度，如`rbga(255,0,0,0.6)`。
- 16进制数值表示
  例如`#ff0000`表示红色。

color、font-size、font-family、font-style、font-weight测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>css引入</title>
    <!-- <link rel="stylesheet" href="css/main.css"> -->
    <style>
        div {
            font-size: 20px;
            color: chocolate;
            font-family: 'Microsoft Yahei';
            line-height: 80px;
        }
        em{
            font-style: normal;
        }
        h6{
            font-weight: normal;
        }
    </style>
</head>

<body>
    <h1>CSS测试</h1>
    <div>
        你是唯一一个我奔赴千里，<em>跨越山河都想见的人。</em> 如果说林忆莲是李宗盛的灵魂，那你便是我的灵魂，我的信仰，我的追求。在我的笔下，总会不经意的提起你，那天你对我说：“前世的五百次回眸才换来今生擦肩而过，认识小天一定是前世的几千次回眸，所以感谢缘分。”或许这句话你不仅对我说过，也会有其他人，缘分是一种很奇妙的东西，但我仍然珍惜这份缘，与你相遇，此生之幸。
    </div>
</body>

</html>
1234567891011121314151617181920212223242526272829303132
```

显示：
![html css common style](https://img-blog.csdnimg.cn/2020081510023247.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

text-decoration和text-indent测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        p{
            text-decoration: underline;
            text-indent: 32px;
        }
        div{
            text-decoration: overline;
            text-indent: -32px;
        }
        pre{
            text-decoration: line-through;
        }
    </style>
</head>

<body>
    <p>那年初三，最后一节课，我们依依不舍的，静静的等班主任上完这一课，然后各奔东西！
        班主任红着双眼哭着对我们说，你们是我教的最好的一届，谢谢你们！本来我想再多教几年，就可以退休了，现在肯定不行了，自从教了你们这一届，我才知人真的可以活活被气死……
        我们一下子蒙了，我们都是乖宝宝啊，学习成绩也都好啊！
        这时，老师又发飙了，本来好好的带你们一个差生班，我也乐得清闲！现在一下子成绩超过了尖子班，全级第一！校里还决定，往后的几个尖子班全让我带，工作量压得我喘不过气来，你们说气人不？……</p>
    <div>那年初三，最后一节课，我们依依不舍的，静静的等班主任上完这一课，然后各奔东西！
        班主任红着双眼哭着对我们说，你们是我教的最好的一届，谢谢你们！本来我想再多教几年，就可以退休了，现在肯定不行了，自从教了你们这一届，我才知人真的可以活活被气死……
        我们一下子蒙了，我们都是乖宝宝啊，学习成绩也都好啊！
        这时，老师又发飙了，本来好好的带你们一个差生班，我也乐得清闲！现在一下子成绩超过了尖子班，全级第一！校里还决定，往后的几个尖子班全让我带，工作量压得我喘不过气来，你们说气人不？……</div>
    <pre>那年初三，最后一节课，我们依依不舍的，静静的等班主任上完这一课，然后各奔东西！
        班主任红着双眼哭着对我们说，你们是我教的最好的一届，谢谢你们！本来我想再多教几年，就可以退休了，现在肯定不行了，自从教了你们这一届，我才知人真的可以活活被气死……
        我们一下子蒙了，我们都是乖宝宝啊，学习成绩也都好啊！
        这时，老师又发飙了，本来好好的带你们一个差生班，我也乐得清闲！现在一下子成绩超过了尖子班，全级第一！校里还决定，往后的几个尖子班全让我带，工作量压得我喘不过气来，你们说气人不？……</pre>
</body>

</html>
1234567891011121314151617181920212223242526272829303132333435363738
```

显示：
![html css text](https://img-blog.csdnimg.cn/20200816214502362.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

### 2.列表常见样式

列表常见样式和意义如下：

| 样式                | 意义和值                                                     |
| ------------------- | ------------------------------------------------------------ |
| list-style-type     | 列表项样式，disc-实心圆、square-正方形、circle-空心圆、none-无 |
| list-style-image    | 图片列表项，url(相对路径)                                    |
| list-style-position | 列表项位置，outside(外边)、inside(里边)                      |
| list-style          | none(最常用)                                                 |

练习如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        *{
            margin: 0px;
            padding: 0px;
        }
        ul li{
            list-style-type: circle;
        }
        ol li{
            list-style-type: none;
            list-style-image: url(image.png);
            border: 1px brown solid;
            list-style-position: inside;
        }
    </style>
</head>

<body>
    <ul>
        <li>床前明月光</li>
        <li>疑是地上霜</li>
        <li>举头望明月</li>
    </ul>
    <ol>
        <li>床前明月光</li>
        <li>疑是地上霜</li>
        <li>举头望明月</li>
    </ol>
</body>

</html>
1234567891011121314151617181920212223242526272829303132333435363738
```

显示：
![html css list style](https://img-blog.csdnimg.cn/20200816214517592.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

### 3.背景样式

背景图的相关样式和意义如下：

| 样式                | 意义和取值                                                   |
| ------------------- | ------------------------------------------------------------ |
| background-color    | 背景颜色值                                                   |
| background-image    | 背景图片url(相对路径)                                        |
| background-repeat   | 背景重复方式，no-repeat不平铺、repeat平铺、repeat-x横向平铺、repeat-y纵向平铺 |
| background-position | 背景位置，水平(left/center/right)、垂直(top/center/bottom)   |
| background          | 背景色，将前四个属性缩写为一个(常用)：`url(背景图相对路径) no-repeat center top` |

对于background-image：
当容器尺寸=图片尺寸→刚好；
当容器尺寸>图片尺寸→图片平铺；
当容器尺寸<图片尺寸→仅显示容器以内的背景图。

进行测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box1{
            width: 1000px;
            height: 1000px;
            border: 8px solid;
            background-color: cadetblue;
        }
        .box2{
            width: 1000px;
            height: 1000px;
            border: 8px solid;
            background-color: cadetblue;
            background-image: url(wallpaper.jpg);
        }
        .box3{
            width: 1000px;
            height: 1000px;
            border: 8px solid;
            background-color: cadetblue;
            background-image: url(wallpaper.jpg);
            background-repeat: repeat-x;
        }
        .box4{
            width: 1000px;
            height: 1000px;
            border: 8px solid;
            background-color: cadetblue;
            background-image: url(wallpaper.jpg);
            background-repeat: no-repeat;
            background-position: center center;
        }
        .box5{
            width: 1000px;
            height: 1000px;
            border: 8px solid;
            background: red url(wallpaper.jpg) no-repeat left bottom;
        }
    </style>
</head>

<body>
    <div class="box1">盒子1</div>
    <br>
    <div class="box2">盒子2</div>
    <br>
    <div class="box3">盒子3</div>
    <br>
    <div class="box4">盒子4</div>
    <br>
    <div class="box5">盒子5</div>
</body>

</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960
```

显示：
![html css background](https://img-blog.csdnimg.cn/20200816214849315.gif#pic_center)

### 4.浮动样式

浮动主要为了**脱离文档流**(垂直排列)，语法为`选择符{float:left/right}`。

特点：

- div块元素失去**块状换行显示**特征，变为**行内元素**；
- 紧贴上一个浮动元素(同方向)或父级元素的边框，如宽度不够将**换行显示**；
- 占据行内元素(文字段落)的空间，导致行内元素**围绕显示**。

先进行测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box1{
            width: 100px;
            height: 100px;
            border: 3px solid blue;
            background-color: cadetblue;
            margin-bottom: 30px;
            float: left;
        }
        .box2{
            width: 200px;
            height: 200px;
            border: 3px solid blue;
            background-color: palevioletred;
            margin-bottom: 30px;
        }
        .box3{
            width: 300px;
            height: 300px;
            border: 3px solid blue;
            background-color: wheat;
            margin-bottom: 30px;
        }
    </style>
</head>

<body>
    <div class="box1">盒子1</div>
    <br>
    <div class="box2">盒子2</div>
    <br>
    <div class="box3">盒子3</div>
    <br>
</body>

</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243
```

显示：
![html css float left](https://img-blog.csdnimg.cn/20200816214908931.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

可以看到，因为盒子1向左浮动，不在占据位置，因此盒子2向上移动，占据了盒子1的位置，盒子3依次向上移动。

再测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box1{
            width: 100px;
            height: 100px;
            border: 3px solid blue;
            background-color: cadetblue;
            margin-bottom: 30px;
            /* float: left; */
        }
        .box2{
            width: 200px;
            height: 200px;
            border: 3px solid blue;
            background-color: palevioletred;
            margin-bottom: 30px;
            float: right;
        }
        .box3{
            width: 300px;
            height: 300px;
            border: 3px solid blue;
            background-color: wheat;
            margin-bottom: 30px;
        }
    </style>
</head>

<body>
    <div class="box1">盒子1</div>
    <br>
    <div class="box2">盒子2</div>
    <br>
    <div class="box3">盒子3</div>
    <br>
</body>

</html>
1234567891011121314151617181920212223242526272829303132333435363738394041424344
```

显示：
![html css float right](https://img-blog.csdnimg.cn/20200816214924299.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

此时因为盒子2向右浮动，失去了块状元素的特性，因此盒子3向上移动，盒子1保持不动。

还可以使3个盒子并排展示，测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box1{
            width: 100px;
            height: 100px;
            border: 3px solid blue;
            background-color: cadetblue;
            margin-bottom: 30px;
            float: left;
        }
        .box2{
            width: 200px;
            height: 200px;
            border: 3px solid blue;
            background-color: palevioletred;
            margin-bottom: 30px;
            float: left;
        }
        .box3{
            width: 300px;
            height: 300px;
            border: 3px solid blue;
            background-color: wheat;
            margin-bottom: 30px;
            float: left;
        }
    </style>
</head>

<body>
    <div class="box1">盒子1</div>
    <div class="box2">盒子2</div>
    <div class="box3">盒子3</div>
    <br>
</body>

</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243
```

显示：
![html css float left 3](https://img-blog.csdnimg.cn/20200816214936135.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

再进行行内元素围绕显示测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box {
            width: 100px;
            height: 100px;
            border: 3px solid blue;
            background-color: rgba(194, 177, 26, 0.4);
            margin-bottom: 15px;
            float: left;
        }

        p {
            font-size: 30px;
        }

    </style>
</head>

<body>
    <div class="box">盒子</div>
    <p>挨饿这事，干得好就叫减肥；偷懒这事，干得好就叫享受；死皮赖脸这事，干得好就叫执着；装傻这事，如果干得好，那叫大智若愚。女人的一生，小时候淘气，长大了淘宝，工作了淘金，结婚了淘米，老了，淘汰。人最大的痛苦莫过于经历了大风大浪，不但没看见彩虹，结果还得了风湿。吃货的日常状态：嘴里很享受，心里很想瘦。胖子只有两条出路，要不就让身材变好，要不就让心态变好。</p>
</body>

</html>
123456789101112131415161718192021222324252627282930
```

显示：
![html css float inline text](https://img-blog.csdnimg.cn/2020081621495068.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，文字围绕着盒子显示。

但是浮动样式也会存在一些问题：
父元素不设置高度，让子元素撑起来。子元素浮动之后，父元素没有高度了,则会导致**高度塌陷**。

浮动塌陷测试，先不设置浮动样式如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            width: 800px;
            border: 3px solid chartreuse;
        }
        .box1{
            width: 200px;
            height: 200px;
            border: 3px solid blue;
            background-color: cadetblue;
        }
        .box2{
            width: 200px;
            height: 200px;
            border: 3px solid blue;
            background-color: palevioletred;
        }
    </style>
</head>

<body>
    <div class="box">
        <div class="box1">盒子1</div>
        <div class="box2">盒子2</div>
    </div>
</body>

</html>
1234567891011121314151617181920212223242526272829303132333435
```

显示：
![html css float no](https://img-blog.csdnimg.cn/20200816215006184.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

很明显，两个盒子的高度撑起了父元素的高度。

再设置浮动样式如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            width: 800px;
            border: 3px solid chartreuse;
        }
        .box1{
            width: 200px;
            height: 200px;
            border: 3px solid blue;
            background-color: cadetblue;
            float: left;
        }
        .box2{
            width: 200px;
            height: 200px;
            border: 3px solid blue;
            background-color: palevioletred;
            float: left;
        }
    </style>
</head>

<body>
    <div class="box">
        <div class="box1">盒子1</div>
        <div class="box2">盒子2</div>
    </div>
</body>

</html>
12345678910111213141516171819202122232425262728293031323334353637
```

显示：
![html css float collapse](https://img-blog.csdnimg.cn/20200816215019958.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

此时可以看到，两个盒子并排显示，但是父元素的上下边框重合，高度消失，即发生了高度塌陷，则父元素后的其他元素也会发生变化。

有3种解决办法：
（1）父元素设置高度；
（2）给父元素添加`overflow:hidden`；
（3）在浮动元素下添加空div，并添加声明`clear:both;height:0;overflow:hidden`(或`font-size:0;`)。

第一种方法解决如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            width: 800px;
            height: 200px;
            border: 3px solid chartreuse;
        }
        .box1{
            width: 200px;
            height: 200px;
            border: 3px solid blue;
            background-color: cadetblue;
            float: left;
        }
        .box2{
            width: 200px;
            height: 200px;
            border: 3px solid blue;
            background-color: palevioletred;
            float: left;
        }
    </style>
</head>

<body>
    <div class="box">
        <div class="box1">盒子1</div>
        <div class="box2">盒子2</div>
    </div>
</body>

</html>
1234567891011121314151617181920212223242526272829303132333435363738
```

显示：
![html css float collapse solution1](https://img-blog.csdnimg.cn/20200816215041493.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

此时显示正常，但是给父元素指定高度，显得很不灵活，不能根据子元素的变化而变化。

第二种方法测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            width: 800px;
            /* height: 200px; */
            overflow: hidden;
            border: 3px solid chartreuse;
        }
        .box1{
            width: 200px;
            height: 200px;
            border: 3px solid blue;
            background-color: cadetblue;
            float: left;
        }
        .box2{
            width: 200px;
            height: 200px;
            border: 3px solid blue;
            background-color: palevioletred;
            float: left;
        }
    </style>
</head>

<body>
    <div class="box">
        <div class="box1">盒子1</div>
        <div class="box2">盒子2</div>
    </div>
</body>

</html>
123456789101112131415161718192021222324252627282930313233343536373839
```

效果与之前相同，显然更加灵活，但是具有一定的危害。

第三种方法解决如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            width: 800px;
            /* height: 200px; */
            overflow: hidden;
            border: 3px solid chartreuse;
        }
        .box1{
            width: 200px;
            height: 200px;
            border: 3px solid blue;
            background-color: cadetblue;
            float: left;
        }
        .box2{
            width: 200px;
            height: 200px;
            border: 3px solid blue;
            background-color: palevioletred;
            float: left;
        }
        .box3{
            clear: both;
            font-size: 0;
        }
    </style>
</head>

<body>
    <div class="box">
        <div class="box1">盒子1</div>
        <div class="box2">盒子2</div>
        <div class="box3">盒子3</div>
    </div>
</body>

</html>
1234567891011121314151617181920212223242526272829303132333435363738394041424344
```

效果与之前相同，此时更加灵活，盒子3不占据任何位置，就像透明的一样，但是只要它存在，就会撑起父元素的高度，从而不会产生高度塌陷。

如果子元素的宽度之和超过父元素的高度时，就会排到下一行，测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            width: 820px;
            /* height: 200px; */
            overflow: hidden;
            border: 3px solid chartreuse;
        }
        .box1{
            width: 400px;
            height: 200px;
            border: 3px solid blue;
            background-color: cadetblue;
            float: left;
        }
        .box2{
            width: 400px;
            height: 200px;
            border: 3px solid blue;
            background-color: palevioletred;
            float: left;
        }
        .box3{
            width: 400px;
            height: 200px;
            border: 3px solid blue;
            background-color: gold;
            float: left;
        }
        .box4{
            clear: both;
            font-size: 0;
        }
    </style>
</head>

<body>
    <div class="box">
        <div class="box1">盒子1</div>
        <div class="box2">盒子2</div>
        <div class="box3">盒子3</div>
        <div class="box4">盒子4</div>
    </div>
</body>

</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152
```

显示：
![html css float multi](https://img-blog.csdnimg.cn/20200816215106527.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

因为盒子1和盒子2的宽度之和已经达到父元素的宽度，再加上盒子3的宽度就会超过父元素宽度，因此盒子3自动移到下一行。

## 二、CSS常用选择器

### 1.CSS元素选择器介绍

CSS语法为：

```css
选择符{
    属性:属性值;
    属性:属性值;
    ...
}
12345
```

有以下特点：
当一个属性有多个属性值时,属性值与属性值**不分先后顺序**；
空格、换行等不影响属性显示。

验证这些特点如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            width: 100px;
            height: 100px;
            background-color: hotpink;
            border: 5px aqua solid;
        }
        p{
            color: burlywood;
        }
    </style>
</head>

<body>
    <div>内涵段子</div>
    <p>人们都说陪伴是最长情的告白，其实，长得好看是陪伴，长得丑就是纠缠。&nbsp;我算是发现了，有的吃货之所以要找个人谈恋爱，纯粹是因为有些地方的饭不适合一个人去吃。<br>自己在家玩手机真的很无聊，应该多找几个朋友出去走走，找个有免费wifi的地方一块玩手机。</p>
</body>

</html>
1234567891011121314151617181920212223242526
```

显示：
![html css style noorder](https://img-blog.csdnimg.cn/20200816215204543.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，在定义div的border时为`5px aqua solid`，与之前的`3px solid blue`顺序不一样，但是不影响显示；
同时在p标签中有空格和外行，也不影响样式的渲染。

选择符表示要定义样式的对象，可以是元素本身，也可以是一类元素或者指定名称的元素，主要分为:

- 元素选择器
- id选择器
- 类选择器
- 通配符选择器
- 群组选择器
- 包含选择器
- 伪类选择器

### 2.元素选择器

元素选择器也就是已经存在的标签的元素名称，之前的例子都属于元素选择器。
比如已存在div标签时，可以定义样式语法为：

```css
div{
    属性:属性值;
}
123
```

举例如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        h1{
            font-style: italic;
        }
        p{
            font-weight: bold;
        }
    </style>
</head>

<body>
    <h1>CSS选择器</h1>
    <h1>CSS选择器</h1>
    <h1>CSS选择器</h1>
    <h1>CSS选择器</h1>
    <p>元素选择器</p>
    <p>元素选择器</p>
    <p>元素选择器</p>
    <p>元素选择器</p>
</body>

</html>
1234567891011121314151617181920212223242526272829
```

显示：
![html css selector element](https://img-blog.csdnimg.cn/2020081621522120.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，可以对一类指定的元素定义样式。

### 3.id选择器

之前是对一类指定的元素定义样式，但是要想对指定的某一个元素定义样式，就不能通过这种方式了，可以通过给某一个元素指定id属性、**唯一**标识某个元素，并定义样式，这就是id选择器。

当结构要体现**单一样式**时，就使用id选择符，语法如下：

```css
#id名{
    属性:属性值;
}
123
```

在整个html文件中id是唯一的，主要是为了后续使用js时不报错。

练习如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        h1{
            font-style: italic;
        }
        p{
            font-weight: bold;
        }
        #title3{
            color: chartreuse;
        }
        #p3{
            color: darkcyan;
        }
    </style>
</head>

<body>
    <h1>CSS选择器</h1>
    <h1>CSS选择器</h1>
    <h1 id="title3">CSS选择器</h1>
    <h1>CSS选择器</h1>

    <p>id选择器</p>
    <p>id选择器</p>
    <p id="p3">id选择器</p>
    <p>id选择器</p>
</body>

</html>
123456789101112131415161718192021222324252627282930313233343536
```

显示：
![html css selector id](https://img-blog.csdnimg.cn/20200816215239553.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

### 4.类选择器

在html件当中如果某一类标签想要使用同一个样式时则使用类（class）选择器，语法为：

```css
.class名{
    属性:属性值;
}
123
```

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        h1{
            font-style: italic;
        }
        p{
            font-weight: bold;
        }
        #title3{
            color: chartreuse;
        }
        #p3{
            color: darkcyan;
        }
        .txt{
            color: firebrick;
        }
    </style>
</head>

<body>
    <h1>CSS选择器</h1>
    <h1>CSS选择器</h1>
    <h1 id="title3">CSS选择器</h1>
    <h1 class="txt">CSS选择器</h1>

    <p>类选择器</p>
    <p>类选择器</p>
    <p id="p3">类选择器</p>
    <p class="txt">类选择器</p>
</body>

</html>
123456789101112131415161718192021222324252627282930313233343536373839
```

显示：
![html css selector class](https://img-blog.csdnimg.cn/20200816215256639.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，id选择器和类选择器是有区别的：
id选择器是对某一唯一元素定义样式，id属性值**不允许重复**；
类选择器是对某一类元素定义样式，class属性值**可以重复**。

### 5.通配选择器

在html中如果想要**选中所有元素**，则用通配选择器。
通常使用通配选择器去掉所有边距，比如：

```css
*{
    margin:0;
    padding:0;
}
1234
```

在没有定义通配选择器时如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            width: 800px;
            border: 20px solid gray;
        }
    </style>
</head>

<body>
    <div></div>
    <h1>CSS选择器</h1>
    <p>CSS选择器之通配选择器</p>

    <ul>
        <li>通配选择器</li>
        <li>通配选择器</li>
        <li>通配选择器</li>
    </ul>
</body>

</html>
12345678910111213141516171819202122232425262728
```

显示：
![html css selector wildcard no](https://img-blog.csdnimg.cn/20200816215321421.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

可以看到，元素左边和上边均有边框宽度，这样不利于前端CSS布局计算和开发。

设置边框宽度如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 800px;
            border: 20px solid gray;
        }
    </style>
</head>

<body>
    <div></div>
    <h1>CSS选择器</h1>
    <p>CSS选择器之通配选择器</p>

    <ul>
        <li>通配选择器</li>
        <li>通配选择器</li>
        <li>通配选择器</li>
    </ul>
</body>

</html>
1234567891011121314151617181920212223242526272829303132
```

显示：
![html css selector wildcard](https://img-blog.csdnimg.cn/20200816215333563.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，此时不再有边框宽度。

### 6.群组选择器

在html中，如果多组样式中有部分相同的样式，则可以提取出来写在一个样式中，并且选择符与选择符之间用逗号隔开，就是群组选择器。
语法为：

```css
选择符1, 选择符2{
    属性:属性值;
}
123
```

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box1{
            width: 100px;
            height: 100px;
            margin-bottom: 10px;
            background-color: hotpink;
        }
        .box2{
            width: 100px;
            height: 100px;
            margin-bottom: 10px;
            background-color: indianred;
        }
        .box3{
            width: 100px;
            height: 100px;
            margin-bottom: 10px;
            background-color: khaki;
        }
    </style>
</head>

<body>
    <div class="box1"></div>
    <div class="box2"></div>
    <div class="box3"></div>
</body>

</html>
123456789101112131415161718192021222324252627282930313233343536
```

显示：
![html css selector group no](https://img-blog.csdnimg.cn/2020081621540673.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，3个盒子有部分相同样式和部分不同样式，因此可以将公共部分抽离出来、简化代码，如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box1{
            background-color: hotpink;
        }
        .box2{
            background-color: indianred;
        }
        .box3{
            background-color: khaki;
        }
        .box1, .box2, .box3{
            width: 100px;
            height: 100px;
            margin-bottom: 10px;
        }
    </style>
</head>

<body>
    <div class="box1"></div>
    <div class="box2"></div>
    <div class="box3"></div>
</body>

</html>
1234567891011121314151617181920212223242526272829303132
```

效果相同。

### 7.包含选择器

在HTML中，当需要为某标签下的子标签添加样式时，就可以使用包含选择器，注意父选择符与子选择符之间用**空格**隔开，语法如下：

```css
父选择符 子选择符{
    属性:属性值;
}
123
```

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
         p span{
             color: lightcoral;
         }       
    </style>
</head>

<body>
    <p>每当想偷懒松懈的时候，我就告诉自己，<span>比我优秀的人比我还努力，</span> 那我努力还有什么用？世界真的很小，总是在没化妆的时候见到一些想刻意见到的人，花枝招展没有局，邋里邋遢遇前任，乔装打扮无人约，素面朝天遇情敌。当有人在背后说你坏话时，往往会有很多人跟着起哄，你不用在意，<span>因为吃屎的注定跟拉屎的团结友爱。</span></p>
    <span>吃得苦中苦，心里会更堵。</span>
</body>

</html>
1234567891011121314151617181920
```

显示：
![html css selector contain](https://img-blog.csdnimg.cn/20200816215421961.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

此时，只有p标签下的span标签，才会被渲染样式。

### 8.伪类选择器

在html中，当需要向某些选择器添加特殊的效果时，就可以使用伪类选择器，通常与a标签一起使用，语法如下：

```css
a:link{color: #FF000O}  	/*未访问的链接*/
a:visited{color: #00FFOO}  	/*已访问的链接*/
a:hover{color: #FFOOFF}    	/*鼠标移动到链接上*/
a:active{color: #0000FF}   	/*选定的链接*/
1234
```

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        a:link {
            color: #FF0000
        }

        a:visited {
            color: #00FF00
        }

        a:hover {
            color: #FF00FF
        }

        a:active {
            color: #0000FF
        }

    </style>
</head>

<body>
    <a href="#">点我一下</a>
</body>

</html>
1234567891011121314151617181920212223242526272829303132
```

显示：
![html css selector fake class](https://img-blog.csdnimg.cn/20200816215448328.gif#pic_center)

显然，不同的状态有不同的颜色。

但是通常有两种颜色变化即可，如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        a {
            color: blue
        }

        a:hover {
            color: #FF00FF
        }

    </style>
</head>

<body>
    <a href="#">点我一下</a>
</body>

</html>
123456789101112131415161718192021222324
```

显示：
![html css selector fake class simple](https://img-blog.csdnimg.cn/20200816215458281.gif#pic_center)

此时效果更简洁。

### 9.选择器优先级

选择器优先级如下：

| 选择器     | 优先级权重 |
| ---------- | ---------- |
| 行内样式   | 1000       |
| id选择器   | 100        |
| 类选择器   | 10         |
| 元素选择器 | 1          |
| 通配选择器 | 0          |

优先级权重越高，优先级越大，就会优先显示。

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .cls {
            width: 100px;
            height: 100px;
            background-color: magenta;
        }

        #box {
            background-color: navajowhite;
        }
    </style>
</head>

<body>
    <div class="cls" id="box"></div>
    <div class="cls" style="background-color: orange;"></div>
</body>

</html>
1234567891011121314151617181920212223242526
```

显示：
![html css selector priority](https://img-blog.csdnimg.cn/20200816215514235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，因为id选择器的优先级高于类选择器，所以第一个div的颜色为id选择器的颜色navajowhite；
同时行内样式的优先级高于类选择器，所以第二个选择器的颜色为行内样式的颜色orange。

# Python全栈（九）Web前端基础之4.CSS盒模型和JS基础

## 一、CSS盒模型

### 1.盒模型介绍

CSS对HTML文档元素生成了一个描述该元素在HTML文档布局中所占空间的**矩形元素框**，可以形象地将其看作是一个盒子。
盒模型并不是官方提出的，而是后期针对块元素提出的。

盒模型又称框模型（Box Model），包含了**元素内容（content）**、**内边距（padding）**、**边框（border）**、**外边距（margin）** 几个要素，图示如下：
![html css boxmodel image](https://img-blog.csdnimg.cn/20200817195052339.gif#pic_center)

### 2.盒模型之border用法

border表示边框，包括边框宽度、边框风格、边框颜色，例如`ele:{border:5px solid red;}`。
也可以分别定义样式，如下：

| 样式     | 符号                                                         |
| -------- | ------------------------------------------------------------ |
| 边框宽度 | border-width                                                 |
| 边框颜色 | border-color                                                 |
| 边框样式 | border-style:solid(实线)、dashed(虚线)、dotted(点划线)、double(双线) |

4个不同方向的边框可以单独使用，即border-bottom、border-left、border-right、border-top分别定义。

先实现浮动效果如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            height: 200px;
            width: 600px;
            border: 5px solid skyblue;
        }
        .box1{
            height: 100%;
            width: 200px;
            background-color: palevioletred;
            float: left;
        }
        .box2{
            height: 100%;
            width: 400px;
            background-color: rebeccapurple;
            float: left;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="box1"></div>
        <div class="box2"></div>
    </div>
</body>
</html>
123456789101112131415161718192021222324252627282930313233
```

显示：
![html css boxmodel border no](https://img-blog.csdnimg.cn/2020081719511342.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

此时给box1设置宽度如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            height: 200px;
            width: 600px;
            border: 5px solid skyblue;
        }
        .box1{
            height: 100%;
            width: 200px;
            background-color: palevioletred;
            float: left;
            border: 2px double tan;
        }
        .box2{
            height: 100%;
            width: 400px;
            background-color: rebeccapurple;
            float: left;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="box1"></div>
        <div class="box2"></div>
    </div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334
```

显示：
![html css boxmodel border](https://img-blog.csdnimg.cn/2020081720044276.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，此时box2被挤到第二排，这是因为box1设置了border宽度为2，所以box1的整体宽度为202，与box2相加为604，超过600，所以box2只能显示到下一行。

要想box2留在第一行，可以将box1的宽度和高度设置为196，使整体宽度恢复为200即可，即如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            height: 200px;
            width: 600px;
            border: 5px solid skyblue;
        }
        .box1{
            height: 196px;
            width: 196px;
            background-color: palevioletred;
            float: left;
            border: 2px double tan;
        }
        .box2{
            height: 100%;
            width: 400px;
            background-color: rebeccapurple;
            float: left;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="box1"></div>
        <div class="box2"></div>
    </div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334
```

可以发现：
border会改变原有盒子的大小，在实际操作当中，添加了border，就要相应调整盒子的width、height属性才能保持大小不变；
增减宽高时注意如果添加了上下左右方向上的border,则增减都是**双倍**的。

### 3.盒模型之padding用法

padding是指元素内容到border之间的距离。

有4种定义padding的方式：

- 四个值
  上 右 下 左：`{padding:10px 20px 30px 40px}`。
- 三个值
  上 左右 下：`{padding:10px 20px 30px}`。
- 二个值
  上下 左右：`{padding:10px 20px}`。
- 一个值
  四个方向：`{padding:2px}`。

也可以4个方向分别定义，即padding-top、padding-right、padding-bottom、padding-left各自定义。

在没有设置padding时，如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            height: 200px;
            width: 600px;
            border: 5px solid skyblue;
        }
        .box1{
            height: 196px;
            width: 196px;
            background-color: palevioletred;
            float: left;
            border: 2px double tan;
        }
        .box2{
            height: 100%;
            width: 400px;
            background-color: rebeccapurple;
            float: left;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="box1">CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。</div>
        <div class="box2"></div>
    </div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334
```

显示：
![html css boxmodel padding no](https://img-blog.csdnimg.cn/20200817200503977.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

可以看到，在没有设置padding时，文字紧挨着盒子边框内测显示，没有空闲距离。

设置padding属性如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            height: 200px;
            width: 600px;
            border: 5px solid skyblue;
        }
        .box1{
            height: 176px;
            width: 176px;
            background-color: palevioletred;
            float: left;
            border: 2px double tan;
            padding: 10px;
        }
        .box2{
            height: 100%;
            width: 400px;
            background-color: rebeccapurple;
            float: left;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="box1">CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。</div>
        <div class="box2"></div>
    </div>
</body>
</html>
1234567891011121314151617181920212223242526272829303132333435
```

显示：
![html css boxmodel padding](https://img-blog.csdnimg.cn/20200817200518699.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

此时，文字与border之间存在一定的距离。

可以看到：
添加padding也会增大该盒子的大小，在实际操作当中，添加了padding，就要相应调整盒子的width、height属性；
增减宽高时注意我们如果添加了上下左右方向上的padding,则增减都是**双倍**的；
背景颜色、背景图片填充盒子会**覆盖**content和padding区域。

### 4.盒模型之margin用法

margin是指盒子与盒子之间的距离。

有4种定义margin的方式：

- 四个值
  上 右 下 左：`{margin:10px 20px 30px 40px}`。
- 三个值
  上、左右、下：`{margin:10px 20px 30px}`。
- 二个值
  上下、左右：`{margin:10px 20px}`。
- 一个值
  四个方向：`{margin:2px}`。

也可以四个方向单独定义，即margin-top、margin-right、margin-bottom、margin-left各自定义。

测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box1{
            height: 100px;
            width: 100px;
            background-color: palevioletred;
            margin-bottom: 50px;
        }
        .box2{
            height: 200px;
            width: 200px;
            background-color: rebeccapurple;
            margin-top: 100px;
        }
    </style>
</head>
<body>
    <div class="box1"></div>
    <div class="box2"></div>
</body>
</html>
1234567891011121314151617181920212223242526
```

显示：
![html css boxmodel margin big](https://img-blog.csdnimg.cn/20200817200559235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

可以看到，box2的margin为100，即有多个元素垂直相遇时，会选择**较大**的margin作为公共margin。

再测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box1{
            height: 100px;
            width: 100px;
            background-color: palevioletred;
        }
        .box2{
            height: 200px;
            width: 200px;
            background-color: rebeccapurple;
        }
        .box3{
            width: 500px;
            height: 500px;
            background-color: violet;
        }
        .box4{
            width: 100px;
            height: 100px;
            background-color: wheat;
            margin-top: 50px;
        }
    </style>
</head>
<body>
    <div class="box1"></div>
    <div class="box2"></div>
    <div class="box3">
        <div class="box4"></div>
    </div>
</body>
</html>
1234567891011121314151617181920212223242526272829303132333435363738
```

显示：
![html css boxmodel margin parent no](https://img-blog.csdnimg.cn/2020081720061481.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，在给子元素box4设置了margin-top之后，并没有使box4的上边界离box3的上边界为指定距离，反而使得父元素的margin-top变为指定距离。如果父元素和子元素均设置了margin-top，也会选择较大者，如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box1{
            height: 100px;
            width: 100px;
            background-color: palevioletred;
        }
        .box2{
            height: 200px;
            width: 200px;
            background-color: rebeccapurple;
        }
        .box3{
            width: 500px;
            height: 500px;
            background-color: violet;
            margin-top: 100px;
        }
        .box4{
            width: 100px;
            height: 100px;
            background-color: wheat;
            margin-top: 50px;
        }
    </style>
</head>
<body>
    <div class="box1"></div>
    <div class="box2"></div>
    <div class="box3">
        <div class="box4"></div>
    </div>
</body>
</html>
123456789101112131415161718192021222324252627282930313233343536373839
```

显示：
![html css boxmodel margin parent big](https://img-blog.csdnimg.cn/20200817200644548.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

显然，此时margin为较大者100px。

可以解决如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box1{
            height: 100px;
            width: 100px;
            background-color: palevioletred;
        }
        .box2{
            height: 200px;
            width: 200px;
            background-color: rebeccapurple;
        }
        .box3{
            width: 500px;
            height: 500px;
            background-color: violet;
            overflow: hidden;
        }
        .box4{
            width: 100px;
            height: 100px;
            background-color: wheat;
            margin-top: 50px;
        }
    </style>
</head>
<body>
    <div class="box1"></div>
    <div class="box2"></div>
    <div class="box3">
        <div class="box4"></div>
    </div>
</body>
</html>
123456789101112131415161718192021222324252627282930313233343536373839
```

显示：
![html css boxmodel margin solve](https://img-blog.csdnimg.cn/20200817200727443.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

此时，子元素的上边界离父元素的上边界为指定的距离。

可以看到：
系统会为某些元素设置默认的margin值或padding值，会对布局的计算有影响，所以一般先清除掉所有元素自带的margin和padding值，即`*{margin:0;padding:0;}`；
相邻兄弟元素在垂直方向相遇，取margin中的较大值；
当父级子级元素都设置了margin时，子级的margin值会出现融合的情况，融合后会取两个元素里较大的值作为融合后的值，解决方法是给父元素设置`{overflow:hidden}`。

### 5.文本溢出overflow

overflow样式：
定义溢出元素内容区的内容会如何处理。
语法为：`{overflow:visible/hidden/scroll/auto/inherit;}`。

其中的参数值和意义如下：

| 参数值  | 意义                                       |
| ------- | ------------------------------------------ |
| visible | 默认值，内容不会被修剪，会出现在元素框之外 |
| hidden  | 内容会被修剪,并且其余内容是不可见的        |
| scroll  | 内容会被修剪，但是浏览器显示滚动条         |
| auto    | 如果内容超出则会显示滚动条，不超出则不显示 |
| inherit | 规定应该从父元素继承overflow属性的值       |

测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            height: 300px;
            width: 600px;
            background-color: palevioletred;
            overflow: auto;
        }
        .box1{
            height: 300px;
            width: 300px;
            background-color: yellowgreen;
        }
        .box2{
            height: 300px;
            width: 300px;
            background-color: aqua;
            overflow: hidden;
        }
        .box3{
            height: 300px;
            width: 300px;
            background-color: burlywood;
            overflow: scroll;
        }
        .box4{
            height: 300px;
            width: 300px;
            background-color: coral;
            overflow: auto;
        }
        .box5{
            height: 300px;
            width: 300px;
            background-color: darkcyan;
            overflow: inherit;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="box1">
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
        </div>
        <div class="box2">
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
        </div>
        <div class="box3">
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
        </div>
        <div class="box4">
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
        </div>
        <div class="box5">
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
            CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。
        </div>
    </div>
</body>
</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384
```

显示：
![html css boxmodel overflow](https://img-blog.csdnimg.cn/2020081720075244.gif#pic_center)

显然，体现出了各种效果的overflow。

## 二、JS介绍

### 1.JS发展历史

JS发展历史如下：
![html js history](https://img-blog.csdnimg.cn/20200817200815632.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

为了统一三家，ECMA(欧洲计算机制造协会)定义了ECMA-262规范。国际标准化组织及国际电工委员会（ISO/IEC）也采纳ECMAScript作为标准（ISO/IEC-16262）。从此，Web浏览器就开始努力（虽然有着不同的程度的成功和失败）将ECMAScript作为JavaScript实现的基础，其中**EcmaScript是规范**。

### 2.JS组成

JavaScript是一种专为与网页交互而设计的客户端脚本语言，最初是为了实现表单验证，之前表单验证都是在服务器完成的。

JS组成包括3部分：
（1）核心：ECMAScript；
（2）浏览器对象模型：BOM，即Broswer object model，整合了js和浏览器；
（3）文档对象模型：DOM，即Document object model，整合了js、css和html。

## 三、JS基础

### 1.JS引入方式

有两种引入方式：

- 直接在HTML中编写

  例如：

  ```html
  <script type="text/javascript">
      alert('hello world');
  </script>
  123
  ```

- 引入外部JS

  如下：

  ```html
  <script type="text/javascript" src="js/demo.js"></script>
  1
  ```

第一种方式测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        alert('hello world');
    </script>
    
</head>
<body>
    
</body>
</html>
123456789101112131415
```

显示：
![html js import way1](https://img-blog.csdnimg.cn/20200817200838760.gif#pic_center)

显然，弹出了弹出框显示消息。

第二种方式测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript" src="js/demo.js">
    </script>
    
</head>
<body>
    
</body>
</html>
1234567891011121314
```

在同级目录下创建js目录，下创建demo.js如下：

```js
alert('JS引入方式测试')
1
```

显示：
![html js import way2](https://img-blog.csdnimg.cn/20200817200850898.gif#pic_center)

需要注意：
如果导入外部的JS文件，不允许再在该script标签中写其他JS代码；
`type="text/javascript`说明当前script标签中文本的类型；
所有JS代码写在script标签中，script放在head标签中；
可以引入多个script标签，多个script标签顺序执行；

还可以通过JS向页面中传入内容并显示：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript" src="js/demo.js">
    </script>
    <script type="text/javascript">
        document.write('hello');
        document.write('<h1>你好</h1>');
        document.write('&lt;h1&gt;你好&lt;/h1&gt;');
    </script>
    
</head>
<body>
    
</body>
</html>
12345678910111213141516171819
```

显示：
![html js import way2 document write](https://img-blog.csdnimg.cn/20200817200906726.gif#pic_center)

JS注释包括单行注释和多行注释：
单行注释符为`//`，快捷键为`Ctrl+/`；
多行注释符为`/**/`，快捷键为`Ctrl+Shift+/`。

### 2.JS基本数据类型和变量

JS的基本数据类型如下：

| 数据类型      | 举例                    |
| ------------- | ----------------------- |
| 数字number    | 100、3.14               |
| 字符串string  | ‘hello’、“hello”        |
| 布尔值boolean | true、false             |
| 特殊数据类型  | null空、undefined未声明 |

JS中的变量是值可以改变的量。
通过关键字(系统定义的有特殊功能的单词)`var`声明变量；
声明之后可以对变量赋值。

声明变量的时候，同时给变量赋值，叫**初始化**；
同时定义多个变量时，变量之间要使用逗号隔开。

示意如下:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        var number = 3.14159;
        alert(number);
        number = 2.71828;
        alert(number);
    </script>
    
</head>
<body>
    
</body>
</html>
123456789101112131415161718
```

显示：
![html js variable initialize](https://img-blog.csdnimg.cn/20200817200929873.gif#pic_center)

在声明了变量之后，JS为变量number开辟了一块**内存区域**用于保存该变量，具有一定的**内存地址**，变量的值改变之后，地址也不会发生变化。

可以用`typeof`来判断变量的类型，如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        var number = 3.14159;
        alert(typeof number);
        number = '2.71828';
        alert(typeof number);
    </script>
    
</head>
<body>
    
</body>
</html>
123456789101112131415161718
```

显示：
![html js variable typeof](https://img-blog.csdnimg.cn/20200817200943226.gif#pic_center)

从中也可以看到，JS是一门弱语言，变量类型可以改变。

和其他语言一样，JS中也有标识符：
用户自定义的所有名字叫做**标识符**。

标识符规命名则：
（1）由数字、字母、下划线、$组成，其余不可；
（2）不能以数字开头；
（3）区分大小写；
（4）标识符必须见名知意。

### 3.JS-BOM

BOM就是浏览器对象模型，浏览器可以通过调用系统对话框，向用户显示信息。
系统提供了三个函数，可以完成系统对话框的操作：
（1）`window.alert("警告框");`；
（2）`window.confirm("请选择确定和取消");`;
（3）`window.prompt("请输入一个数", "默认值");`。

其中，window均可以省略。

测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        var number = 3.14159;
        window.alert(number);
        number = '2.71828';
        var result = confirm('请选择确认yes或者no');
        alert(result);
        result = prompt('请输入', 1111);
        alert(result);
    </script>
    
</head>
<body>
    
</body>
</html>
123456789101112131415161718192021
```

显示：
![html js bom](https://img-blog.csdnimg.cn/20200817201016731.gif#pic_center)

可以看到，`confirm()`和`prompt()`均有返回值。

### 4.JS-DOM

DOM即文档对象模型包括`<html>`开始到`</html>`结束。
其中，HTML+CSS负责页面内容，JS负责页面行为操作，DOM则是**打通HTML和CSS与JS**壁垒的工具。

在JS中所有节点都是对象，DOM中节点分为3类：

- 元素节点
  如`<div></div>`。
- 属性节点
  如`title="tit1"`。
- 文本节点
  标签中的内容。

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        window.onload = function () {
            var res = document.getElementById('box');
            alert(res);
        }
    </script>

</head>

<body>
    <div id="box">
        DOM测试
    </div>
</body>

</html>
1234567891011121314151617181920212223
```

显示：
![html js dom simple](https://img-blog.csdnimg.cn/20200817201029422.gif#pic_center)

因为在渲染HTML网页时，代码是从上向下依次执行的，所以将DOM代码放入`window.onload = function () {}`来保证先渲染HTML代码再执行大括号中的代码。

获取到元素节点对象后，可以进一步通过该节点获取属性节点和文本节点：
（1）属性包括tagName、className、id、title、style等，访问属性的方法有两种：

- `元素节点.属性名`
- `元素节点[属性名]`

（2）文本节点为`innerHTML`。

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        window.onload = function () {
            var res = document.getElementById('box');
            alert(res.className);
            alert(res.style['width']);
            alert(res.style.backgroundColor);
            res.setAttribute('name', 'box');
            alert(res.getAttribute('class'));
        }
    </script>

</head>

<body>
    <div id="box" class='box' style="width: 200px; height: 200px; background-color: fuchsia;">
        DOM测试
    </div>
</body>

</html>
123456789101112131415161718192021222324252627
```

显示：
![html js dom style set](https://img-blog.csdnimg.cn/20200817201312246.gif#pic_center)

在获取style属性时，如background-color等属性由多个单词组成，直接使用原格式会获取不到属性，需要使用驼峰命名法；
给元素设置属性调用`setAttribute()`方法，获取属性调用`getAttribute()`方法；
在获取元素的class时，如果直接用`点号.`或`[]`获取，要使用`className`，如果调用`getAttribute()`方法直接使用`class`即可。

再练习如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        window.onload = function () {
            var res = document.getElementsByTagName('li');
            alert(res);
            alert(res.length);
            for(var i=0; i<res.length;i++){
                alert(res[i].innerHTML);
            }
        }
    </script>

</head>

<body>
    <ul>
        <li>111</li>
        <li>222</li>
        <li>333</li>
        <li>444</li>
    </ul>
    <ol>
        <li>555</li>
        <li>666</li>
        <li>777</li>
        <li>888</li>
    </ol>
</body>

</html>
123456789101112131415161718192021222324252627282930313233343536
```

显示：
![html js dom tagname for](https://img-blog.csdnimg.cn/20200817201537788.gif#pic_center)

这是通过`getElementsByTagName()`方法获取元素的，同时获取到了多个`li`标签元素；
有**length属性**即元素个数；
可以通过**for循环**获取到每个元素及其属性。

# Python全栈（九）Web前端基础之5.JS获取节点和常见事件

## 一、获取元素节点

### 1.通过顶层document节点获取

上一节已经提到了两种JS获取节点的方法`document.getElementById()`和`document.getElementsByTagName()`，其实是通过**顶层document节点**获取元素节点的方法共有4种：

| 获取依据      | 方法                                |
| ------------- | ----------------------------------- |
| id属性        | `document.getElementById()`         |
| tagName标签名 | `document.getElementsByTagName()`   |
| name属性      | `document.getElementsByName()`      |
| class属性     | `document.getElementsByClassName()` |

`getElementsByClassName()`练习如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        window.onload = function(){
            var lis = document.getElementsByClassName('li1');
            alert(lis.length);
            for(var i=0; i<lis.length;i++){
                alert(lis[i].innerHTML);
            }
        }
    </script>
</head>
<body>
    <ul>
        <li>111</li>
        <li class="li1">222</li>
        <li>333</li>
        <li>444</li>
    </ul>
    <ol>
        <li class="li1">555</li>
        <li>666</li>
        <li>777</li>
        <li class="li1">888</li>
    </ol>
</body>
</html>
12345678910111213141516171819202122232425262728293031
```

显示：
![html js get node getElementsByClassName](https://img-blog.csdnimg.cn/20200820214155973.gif#pic_center)

显然，可以获取到指定的元素。

前面还提到了获取节点的样式，如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        window.onload = function(){
            var result = document.getElementsByTagName('ul')[0];
            alert(result.style.backgroundColor);
        }
    </script>
</head>
<body>
    <ul style="background-color: aqua;">
        <li>111</li>
        <li class="li1">222</li>
        <li>333</li>
        <li>444</li>
    </ul>
    <ol>
        <li class="li1">555</li>
        <li>666</li>
        <li>777</li>
        <li class="li1">888</li>
    </ol>
</body>
</html>
12345678910111213141516171819202122232425262728
```

但是这种方式只能获取定义的行内样式，而不能获取嵌入式和外联式的样式，要获取这两种样式，需要用`getComputedStyle(元素节点)[获取样式类型];`。

练习如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        ul{
            background-color: bisque;
        }
    </style>
    <script type="text/javascript">
        window.onload = function(){
            var result = document.getElementsByTagName('ul')[0];
            var bgcolor = getComputedStyle(result)['background-color'];
            alert(bgcolor);
        }
    </script>
</head>
<body>
    <ul>
        <li>111</li>
        <li class="li1">222</li>
        <li>333</li>
        <li>444</li>
    </ul>
    <ol>
        <li class="li1">555</li>
        <li>666</li>
        <li>777</li>
        <li class="li1">888</li>
    </ol>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334
```

显示：
![html js get node getComputedStyle](https://img-blog.csdnimg.cn/20200820214233300.gif#pic_center)

此时，显示的颜色值是**RGB**值。

### 2.通过父节点获取

常见用法如下：

| 用法                 | 含义                                                         |
| -------------------- | ------------------------------------------------------------ |
| parentObj.childNodes | 获取作为指定对象直接后代的**HTML元素**和**TextNode对象**的集合 |
| parentObj.firstChild | 获得第一个子节点                                             |
| parentObj.lastChild  | 获得第二个子节点                                             |

childNodes练习如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        window.onload = function(){
            var ul = document.getElementsByTagName('ul')[0];
            alert(ul.childNodes);
            // 获取ul下所有li
            var ulChild = ul.childNodes;
            alert(ulChild.length);
            for(var i=0;i<ulChild.length;i++){
                alert(ulChild[i]);
            }
        }
    </script>
</head>
<body>
    <ul>
        <li>111</li>
        <li class="li1">222</li>
        <li>333</li>
        <li>444</li>
    </ul>
    <ol>
        <li class="li1">555</li>
        <li>666</li>
        <li>777</li>
        <li class="li1">888</li>
    </ol>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334
```

显示：
![html js get node childNodes](https://img-blog.csdnimg.cn/20200820214247452.gif#pic_center)

可以看到，父节点有5个子HTML元素节点和4个文本节点。

firstChild和lastChild练习如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        window.onload = function(){
            var ul = document.getElementsByTagName('ul')[0];
            alert(ul.firstChild);
            alert(ul.lastChild);
        }
    </script>
</head>
<body>
    <ul>
        <li>111</li>
        <li class="li1">222</li>
        <li>333</li>
        <li>444</li>
    </ul>
    <ol>
        <li class="li1">555</li>
        <li>666</li>
        <li>777</li>
        <li class="li1">888</li>
    </ol>
</body>
</html>
1234567891011121314151617181920212223242526272829
```

显示：
![html js get node firstnode lastnode](https://img-blog.csdnimg.cn/20200820214308545.gif#pic_center)

获取到了父节点的第一个和最后一个节点，为文本节点。

## 二、JS鼠标事件

JS事件是用来获取事件的详细信息，如鼠标位置、键盘按键等，包括**鼠标事件**、**表单事件**和**键盘事件**等。

原生JS事件名称为`on+事件名称`。

常见的鼠标事件如下：

| 事件        | 含义                                     |
| ----------- | ---------------------------------------- |
| onClick     | 鼠标点击事件                             |
| onDblClick  | 鼠标双击事件                             |
| onMouseDown | 鼠标上的按钮被按下时激发的事件           |
| onMouseUp   | 鼠标按下后，松开时激发的事件             |
| onMouseOver | 当鼠标移动到某对象范围的上方时触发的事件 |
| onMouseMove | 鼠标移动时触发的事件                     |
| onMouseOut  | 当鼠标离开某对象范围时触发的事件         |

onClick和onMouseOver测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        window.onload = function(){
            var res = document.getElementById('mevent');
            res.onclick = function(){
                console.log('onclicked');
            }
            res.onmouseover = function(){
                console.log('onmouseover');
                this.style.backgroundColor = 'rgb(101, 95, 160)';
            }
        }
    </script>
</head>
<body>
    <div id="mevent" style="background-color: cadetblue; width: 200px; height: 200px;"></div>
</body>
</html>
1234567891011121314151617181920212223
```

显示：
![html js mouseevent onclick onmouseover](https://img-blog.csdnimg.cn/2020082021433225.gif#pic_center)

可以看到，成功执行了onClick和onMouseOver事件。

mousedown和onmouseup测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        window.onload = function(){
            var res = document.getElementById('mevent');
            res.onclick = function(){
                console.log('onclicked');
            }
            res.onmouseover = function(){
                console.log('onmouseover');
                this.style.backgroundColor = 'rgb(101, 95, 160)';
            }
            res.onmousedown = function(){
                console.log('mousedown');
            }
            res.onmouseup = function(){
                console.log('onmouseup');
            }
        }
    </script>
</head>
<body>
    <div id="mevent" style="background-color: cadetblue; width: 200px; height: 200px;"></div>
</body>
</html>
1234567891011121314151617181920212223242526272829
```

显示：
![html js mouseevent onclick mousedown onmouseup](https://img-blog.csdnimg.cn/20200820214352577.gif#pic_center)

显然，onclick事件包括了mousedown和onmouseup两个事件，即mousedown和onmouseup事件共同组成onclick事件。

鼠标事件属性如下：

| 事件      | 含义                     |
| --------- | ------------------------ |
| e.clientX | 鼠标距离可视区的左边距离 |
| e.clientY | 鼠标距离可视区的上边距离 |
| e.pageX   | 鼠标距离页面的左边距离   |
| e.pageY   | 鼠标距离页面的上边距离   |
| e.offsetX | 鼠标距离事件源的左边距离 |
| e.offsetY | 鼠标距离事件源的上边距离 |

测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        window.onload = function(){
            var res = document.getElementById('test');
            res.onclick = function(e){
                console.log(e.clientX, e.clientY);
                console.log(e.pageX, e.pageY);
                console.log(e.offsetX, e.offsetY);
            }
        }
    </script>
</head>
<body>
    <div id="test" style="background-color: rebeccapurple; width: 300px; height: 3500px;"></div>
</body>
</html>
123456789101112131415161718192021
```

显示：
![html js mouseevent attribute](https://img-blog.csdnimg.cn/20200820214417208.gif#pic_center)

显然，clientX和clientY是当前的**可见区域**的位置，pageX和pageY是整个**页面的实际位置**，offsetX和offsetY是相对于**事件源即div**的位置。

再实现关于鼠标事件属性的简单应用如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            width: 600px;
            height: 400px;
            border: 2px solid darkcyan;
        }
    </style>
    <script type="text/javascript">
        window.onload = function(){
            var oDiv = document.getElementsByTagName('div')[0];
            var oSpan = oDiv.children[0];
            oDiv.onmousemove = function(e){
                var x = e.clientX;
                var y = e.clientY;
                console.log(x, y);
                oSpan.innerHTML = x + ',' + y + 'px';
            }
            oDiv.onmouseout = function(){
                oSpan.innerHTML = '';
            }
        }
    </script>
</head>
<body>
    <div>
        <span></span>
    </div>
</body>
</html>
1234567891011121314151617181920212223242526272829303132333435
```

显示：
![html js mouseevent attribute application](https://img-blog.csdnimg.cn/20200820214431860.gif#pic_center)

显然，实现了显示与位置同步的效果。

## 三、JS表单事件

常见的表单事件如下：

| 事件     | 意义                                               |
| -------- | -------------------------------------------------- |
| onBlur   | 当前元素失去焦点时触发的事件                       |
| onFocus  | 当某个元素获得焦点时触发的事件                     |
| onChange | 当前元素失去焦点并且元素的内容发生改变而触发的事件 |
| onInput  | 当前元素获取输入时触发的事件                       |
| onSubmit | 提交表单                                           |

练习如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        window.onload = function(){
            var oForm = document.getElementsByTagName('form')[0];
            var aInput = document.getElementsByTagName('input');
            oForm.onsubmit = function(){
                console.log('submitted');
                // 阻止页面跳转
                return false;
            }
            aInput[0].onfocus = function(){
                console.log('focused');
            }
            aInput[0].onblur = function(){
                console.log('blured');
            }
            aInput[0].onchange = function(){
                console.log('changed');
            }
            aInput[0].oninput = function(){
                console.log('inputed');
            }
        }
    </script>
</head>
<body>
    <div id="mevent" style="background-color: cadetblue; width: 200px; height: 200px;"></div>
    <form action="">
        <input type="text">
        <input type="submit">
    </form>
</body>
</html>
1234567891011121314151617181920212223242526272829303132333435363738
```

显示：
![html js formevent on on on on](https://img-blog.csdnimg.cn/20200820214449145.gif#pic_center)

为了阻止提交后页面跳转刷新而看不到`submitted`的效果，所以需要`return false;`。

## 四、JS键盘事件

键盘事件主要包括以下事件：

| 事件       | 意义                                       |
| ---------- | ------------------------------------------ |
| onKeyPress | 当键盘上的某个键被按下并且释放时触发的事件 |
| onKeyDown  | 当键盘上某个按键被按下时触发的事件         |
| onKeyUp    | 当键盘上某个按键被按放开时触发的事件       |

练习如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            width: 600px;
            height: 400px;
            border: 2px solid darkcyan;
        }
    </style>
    <script type="text/javascript">
        window.onload = function(){
            document.onkeypress = function(){
                console.log('key pressed');
            }
            document.onkeypress = function(){
                console.log('key down');
            }
            document.onkeyup = function(){
                console.log('key up');
            }
            document.onkeydown = function(e){
                console.log(e.altKey, e.shiftKey, e.ctrlKey);
                console.log(e.keyCode);
            }
        }
    </script>
</head>
<body>
    <div>
        <span></span>
    </div>
</body>
</html>
12345678910111213141516171819202122232425262728293031323334353637
```

显示：
![html js keyevent test](https://img-blog.csdnimg.cn/20200820214505506.gif#pic_center)

onkeypress事件是按下**字符键**（字母和数字）时触发的；
onkeydown事件是按下**任意键**均可触发。

键盘事件的简单应用如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            width: 600px;
            height: 400px;
            border: 2px solid darkcyan;
        }
    </style>
    <script type="text/javascript">
        window.onload = function(){
            var oDiv = document.getElementsByTagName('div')[0];
            var aInput = document.getElementsByTagName('input');
            aInput[1].onclick = commentTxt;
            function commentTxt(){
                oDiv.innerHTML += aInput[0].value;
            }
            aInput[0].onkeydown = function(e){
                if(e.keyCode == 13){
                    commentTxt();
                }
            }
        }
    </script>
</head>
<body>
    <div>
    </div>
    <input type="text">
    <input type="button" value="发送">
</body>
</html>
123456789101112131415161718192021222324252627282930313233343536
```

显示：
![html js keyevent application](https://img-blog.csdnimg.cn/20200820214530691.gif#pic_center)

可以看到，实现了点击Enter键进行提交的功能；
还实现了函数封装、提高了代码的复用性。

## 五、阻止浏览器默认行为和封装函数

### 1.阻止浏览器默认行为

由于JavaScript事件本身所具有的的属性，浏览器呈现出很多**默认行为**，如a标签的跳转、submit提交、右键菜单和文本框输入等。

阻止默认行为的方式有3种：
（1）`event.preventDefault();`
（2）`event.returnValue = false;`
（3）`return false;`

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        window.onload = function () {
            var link = document.getElementsByTagName('a');
            link[0].onclick = function () {
                console.log('a clicked');
            }
            link[1].onclick = function () {
                console.log('a1 clicked');
                return false;
            }
            link[2].onclick = function (e) {
                console.log('a2 clicked');
                e.preventDefault();
            }
            link[3].onclick = function (e) {
                console.log('a3 clicked');
                e.returnValue = false;
            }

            document.oncontextmenu = function(){
                console.log('right click');
                return false;
            }
        }
    </script>
</head>

<body>
    <div>
        <a href="https://www.baidu.com">点我一下</a><br>
        <a href="https://www.baidu.com">点我一下1</a><br>
        <a href="https://www.baidu.com">点我一下2</a><br>
        <a href="https://www.baidu.com">点我一下3</a>
    </div>
</body>

</html>
1234567891011121314151617181920212223242526272829303132333435363738394041424344
```

显示：
![html js default event prevent](https://img-blog.csdnimg.cn/20200820214545515.gif#pic_center)

### 2.封装函数

在编写JS时，有时候代码太长、并且重复多，这是为了简化代码、提高复用性，可以**封装函数**。
通过封装函数，可以拥有几种获取元素节点的功能，如下：

| 选择器参数 | 意义                      |
| ---------- | ------------------------- |
| #id        | 通过id获取元素节点        |
| .class     | 通过className获取元素节点 |
| tagName    | 通过tagName获取元素节点   |
| name=xxx   | 通过name获取元素节点      |

进行测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript">
        function $(vArg) {
            switch (vArg[0]) {
                case '#':
                    return document.getElementById(vArg.substring(1))
                    break;
                case '.':
                    return document.getElementsByClassName(vArg.substring(1))
                    break;
                default:
                    var str = vArg.substring(0, 5);
                    if (str == 'name=') {
                        return document.getElementsByName(vArg.substring(5));
                    }
                    else {
                        return document.getElementsByTagName(vArg);
                    }
            }
        }
        window.onload = function () {
            alert($('#div1').innerHTML);
            alert($('.box').length);
            alert($('.box')[1].innerHTML);
            alert($('div').length);
            alert($('name=hello').length);
            alert($('name=hello')[0].innerHTML);
        }
    </script>
</head>

<body>
    <div id="div1">
        <span class="box">111</span>
        <span class="box">222</span>
        <span name='hello'>333</span>
        <span class="box">444</span>
    </div>
    <div></div>
</body>

</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748
```

显示：
![html js encapsulation function](https://img-blog.csdnimg.cn/20200820214559300.gif#pic_center)

# Python全栈（九）Web前端基础之6.JQuery的基本使用

## 一、JQuery简单介绍

JQuery是一个高效、精简并且功能丰富的JavaScript工具库，一般包括**未压缩版jquery-x.x.x.js**和**压缩版jquery-x.x.x.min.js**。
要使用jQuery，有两种引入方式：
（1）下载到本地，然后导入，可以在官网https://jquery.com/download/或者https://download.csdn.net/download/CUFEECR/12734231下载最新版3.5.1，然后本地导入，如下：

```html
<head>
    <script src="jquery-3.5.1.min.js"></script>
</head>
123
```

（2）使用**CDN引入**JQuery，不需要下载，而是在线加载，如下：

```html
<head>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"><script>
</head>
123
```

简单使用如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(document).ready(function(){
            var $div = $('#div1');
            alert('Jquery '+$div);
        })
    </script>
</head>

<body>
    <div id="div1"></div>
</body>

</html>
123456789101112131415161718192021
```

显示：
![html js jquery simple](https://img-blog.csdnimg.cn/20200820214616282.gif#pic_center)

其中，

```js
$(document).ready(function(){
    var $div = $('#div1');
    alert('Jquery '+$div);
})
1234
```

比之前的

```js
window.onload = function () {
    var $div = $('#div1');
    alert('Jquery '+$div);
}
1234
```

加载更快，因为后者是等待**整个浏览器加载完毕**再执行，后者只需要**DOM元素加载完毕**即可。

前者还可以简化为：

```js
$(function(){
    var $div = $('#div1');
    alert('Jquery '+$div);
})
1234
```

这也是经常用到的形式。

## 二、选择器

### 1.JQuery基础选择器

JQuery选择器规则与CSS样式相同，如下：

| 选择器                 | 意义                       |
| ---------------------- | -------------------------- |
| $("#id")               | id                         |
| $(".class")            | 类                         |
| $(“li”)                | 所有li元素                 |
| $("#ul1 li span")      | ul下的li下的span元素       |
| $(“input[name=first]”) | name属性为first的input元素 |

测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        #div1{
            color: fuchsia;
        }
        .box{
            color: goldenrod;
        }
        .list li{
            background-color: indigo;
        }
    </style>
    <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(function(){
            // id选择器
            var $div1 = $('#div1');
            alert($div1.css('color'));

            // 类选择器
            var $box = $('.box');
            alert($box.css('font-size'));
            var $lis = $('.list li');
            $lis.css({'background-color': 'khaki'})
        })
    </script>
</head>
<body>
    <div id="div1">第一个div元素</div>
    <div class="box" style="font-size: 30px;"></div>
    <div class="box"></div>
    <ul class="list">
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
        <li>6</li>
        <li>7</li>
        <li>8</li>
    </ul>
</body>
</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748
```

显示：
![html jquery selector](https://img-blog.csdnimg.cn/20200821195315818.gif#pic_center)

可以看到：
获取到多个元素时，在获取样式时一般只获取第一个元素的样式；
JQuery比原生JS的**容错性更高**，对于样式可以同时使用链式写法和驼峰命名法。

### 2.过滤选择器

常见的过滤选择器如下：

| 选择器                       | 意义                            |
| ---------------------------- | ------------------------------- |
| $(‘div’).has(‘p’);           | 选择包含p元素的div元素          |
| $(‘div’).not(’.myclass’);    | 选择class不等于myclass的div元素 |
| $(‘div’).filter(’.myclass’); | 选择class等于myclass的div元素   |
| $(‘div’).eq(5);              | 索引从0开始、选择第6个div元素   |

测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(function(){
            $('div').css({'background-color': 'blue'});
            $('div').has('p').css({'color': 'red'});
            alert($('#box').index());
            $('div').eq(4).css({'text-indent': '32px'});
            alert($('div').filter('#box').css('text-indent'));
        })
    </script>
</head>
<body>
    <div>1</div>
    <div><p>2</p></div>
    <div>3</div>
    <div>4</div>
    <div id="box">5</div>
    <div>6</div>
    <div>7</div>
    <div>8</div>
</body>
</html>
12345678910111213141516171819202122232425262728
```

显示：
![html jquery selector filter](https://img-blog.csdnimg.cn/20200821195332932.gif#pic_center)

### 3.选择集转移

Jquery常见的选择集转移包括：

| 选择集                     | 含义                              |
| -------------------------- | --------------------------------- |
| $(‘div’).prev();           | 选择div元素前面紧挨的同辈元素     |
| $(‘div’).preAll();         | 选择div元素之前所有的同辈元素     |
| $(‘div’).next();           | 选择div元素之后紧挨的同辈元素     |
| $(‘div’).nextAll();        | 选择div元素之后所有的同辈元素     |
| $(‘div’).parent();         | 选择div的父元素                   |
| $(‘div’).children();       | 选择div所有的子元素               |
| $(‘div’).siblings();       | 选择div的同级元素                 |
| $(‘div’).find(’.myclass’); | 选择div内的class等于myclass的元素 |

测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(function(){
            $('div').eq(4).prev().css({'color': 'green'});
            $('div').find('.nine').css({'color': 'pink'});
        })
    </script>
</head>
<body>
    <div>1</div>
    <div><p>2</p></div>
    <div>3</div>
    <div>4</div>
    <div id="box">5</div>
    <div>6</div>
    <div>7</div>
    <div><span>8</span><span class="nine">9</span></div>
</body>
</html>
12345678910111213141516171819202122232425
```

显示：
![html jquery selector transform](https://img-blog.csdnimg.cn/20200821195348815.gif#pic_center)

其中，`find()`是获取当前标签下符合条件的子元素，而`filter()`是获取当前标签中符合的元素。

### 4.判断是否选择到元素

jquery有容错机制，所以即使没有找到元素，也不会报错，可以用length属性来判断是否找到了元素：

- length=0 → 没选择到
- length大于0 → 选择到

练习如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(function(){
            var $div1 = $('#div1');
            alert($div1.attr('title'));
            alert($div1.length);
        })
    </script>
</head>
<body>
    <div id="div1"></div>
</body>
</html>
12345678910111213141516171819
```

显示：
![html jquery selector length](https://img-blog.csdnimg.cn/20200821195404802.gif#pic_center)

显然，因为JQuery具有较高的容错性，因此虽然div标签虽然没有title属性，也没有报错。

## 三、属性和操作样式类名

### 1.属性

常见的JQuery属性如下：

| 属性                                      | 含义                            |
| ----------------------------------------- | ------------------------------- |
| $(“div”).css(“color”);                    | 获取div样式                     |
| $(“div”).css({“fontSize”:“color”:“red”}); | 设置div样式                     |
| $("#div").html();                         | js中innerHTML()                 |
| $("#div").val();                          | js中value()                     |
| $("#div").prop(“src”);                    | 取出某个属性的值(自有属性)      |
| $("#div").prop({src:“test.jpg”});         | 设置某个属性的值                |
| $("#div").attr();                         | js中attribute()，获取自定义属性 |

其中，`attr()`是获取自定义的属性，`prop()`用于获取自有的属性，如果是路径时获取到的是绝对路径。

测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(function(){
            var $a = $('.link');
            console.log($a.prop('class'));
            console.log($a.attr('xxx'));
            console.log($a.html());
            console.log($a.html('点我两下'));

            var $img = $('#img1');
            console.log($img.prop('src'));
            $img.attr({'aaa': 'bbb'});

        })
    </script>
</head>
<body>
    <a href="#" class="link" xxx='yyy'>点我一下</a>
    <img src="images/qrcode.png" alt="图片加载失败" id="img1">
</body>
</html>
123456789101112131415161718192021222324252627
```

显示：
![html jquery attribute](https://img-blog.csdnimg.cn/20200821195425266.gif#pic_center)

### 2.操作样式类名

对样式类名的常见操作如下：

| 操作                                          | 意义                            |
| --------------------------------------------- | ------------------------------- |
| $("#div1").addClass(“divClass2”) ;            | 为id为div1对象追加样式divClass2 |
| $("#div1").removeClass(“divClass”);           | 为id=div1移除divClass样式       |
| $("#div1").removeClass(“divClass divClass2”); | 移除多个样式                    |
| $("#div1").toggleClass(“anotherClass”) ;      | 重复切换anotherClass样式        |

练习如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            width: 100px;
            height: 100px;
            background-color: lightgreen;
        }
        .big{
            font-size: 30px;
        }
        .red{
            color: magenta;
        }
    </style>
    <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(function(){
            var $box = $('.box');
            $box.addClass('big');
            $box.removeClass('red');
        })
    </script>
</head>
<body>
    <div class="box red">div盒子</div>
</body>
</html>
1234567891011121314151617181920212223242526272829303132
```

显示：
![html jquery class operation](https://img-blog.csdnimg.cn/20200821195453364.gif#pic_center)

现实现一个点击选项卡按钮出现不同盒子的案例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .btns input{
            width: 100px;
            height: 40px;
            background-color: navy;
            border: 0;
        }
        .btns .current{
            background-color: palevioletred;
        }
        .cons div{
            width: 400px;
            height: 200px;
            background-color: orchid;
            text-align: center;
            font-size: 30px;
            line-height: 200px;
            display: none;
        }
        .cons .active{
            display: block;
        }
    </style>
    <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(function(){
            var $btn = $('.btns input');
            var $div = $('.cons div');
            $btn.click(function(){
                $(this).addClass('current');
                $(this).siblings().removeClass('current');

                $div.eq($(this).index()).addClass('active');
                $div.eq($(this).index()).siblings().removeClass('active');
            })
        })
    </script>
</head>
<body>
    <div class="btns">
        <input type="button" value="01" class="current">
        <input type="button" value="02">
        <input type="button" value="03">
    </div>
    <div class="cons">
        <div class="active"><span>选项卡1</span></div>
        <div><span>选项卡2</span></div>
        <div><span>选项卡3</span></div>
    </div>
</body>
</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657
```

显示：
![html jquery class operation application](https://img-blog.csdnimg.cn/20200821195505206.gif#pic_center)

## 四、事件

JQuery的常见事件如下：

| 事件         | 意义                                         |
| ------------ | -------------------------------------------- |
| blur()       | 元素失去焦点                                 |
| focus()      | 元素获得焦点                                 |
| click()      | 鼠标单击                                     |
| mouseover()  | 鼠标进入(进入子元素也触发)                   |
| mouseout()   | 鼠标离开(离开子元素也触发)                   |
| mouseenter() | 鼠标进入(进入子元素不触发)                   |
| mouseleave() | 鼠标离开(离开子元素不触发)                   |
| hover()      | 同时为mouseenter和mouseleave事件指定处理函数 |
| ready()      | DOM加载完成                                  |
| resize()     | 浏览器窗口的大小发生变化                     |
| scroll()     | 滚动条的位置发生变化                         |
| submit()     | 用户递交表单                                 |

获取焦点测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(function(){
            $('#input01').focus();
            $('#input02').focus();
        })
    </script>
</head>
<body>
    <form action="">
        <input type="text" id="input01">
        <input type="text" id="input02">
        <input type="submit" value="提交">
    </form>
</body>
</html>
12345678910111213141516171819202122
```

显示：
![html jquery event focus](https://img-blog.csdnimg.cn/20200821195522341.gif#pic_center)

可以看到，当有两个或多个输入框设置了焦点时，只有**最后一个焦点有效**。

失去焦点测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(function(){
            $('#input01').focus();
            $('#input02').blur(function(){
                var $val = $(this).val();
                alert($val);
            });
        })
    </script>
</head>
<body>
    <form action="">
        <input type="text" id="input01">
        <input type="text" id="input02">
        <input type="submit" value="提交">
    </form>
</body>
</html>
12345678910111213141516171819202122232425
```

显示：
![html jquery event blur](https://img-blog.csdnimg.cn/20200821195535925.gif#pic_center)

mouseover和mouseout测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .con{
            width: 200px;
            height: 200px;
            background-color: rebeccapurple;
        }
        .box{
            width: 100px;
            height: 100px;
            background-color: sienna;
        }
    </style>
    <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(function(){
            $('.con').mouseover(function(){
                alert('mouseover移入');
            })
            $('.con').mouseout(function(){
                alert('mouseout移出');
            })
        })
    </script>
</head>
<body>
    <div class="con">
        <div class="box"></div>
    </div>
</body>
</html>
123456789101112131415161718192021222324252627282930313233343536
```

显示：
![html jquery event mouseover mouseout](https://img-blog.csdnimg.cn/20200821195551580.gif#pic_center)

可以看到，在移入和移出子元素和父元素时，都会触发事件，显得很混乱，称为**冒泡事件**。

mouseenter和mouseleave测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .con{
            width: 200px;
            height: 200px;
            background-color: rebeccapurple;
        }
        .box{
            width: 100px;
            height: 100px;
            background-color: sienna;
        }
    </style>
    <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(function(){
            $('.con').mouseenter(function(){
                alert('mouseenter移入');
            })
            $('.con').mouseleave(function(){
                alert('mouseleave移出');
            })
        })
    </script>
</head>
<body>
    <div class="con">
        <div class="box"></div>
    </div>
</body>
</html>
123456789101112131415161718192021222324252627282930313233343536
```

显示：
![html jquery event mouseenter mouseleave](https://img-blog.csdnimg.cn/20200821195603771.gif#pic_center)

显然，此时清晰很多。

hover测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .con{
            width: 200px;
            height: 200px;
            background-color: rebeccapurple;
        }
        .box{
            width: 100px;
            height: 100px;
            background-color: sienna;
        }
    </style>
    <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(function(){
            $('.con').hover(function(){
                alert('hover移入');
            },function(){
                alert('hover移出');
            })
        })
    </script>
</head>
<body>
    <div class="con">
        <div class="box"></div>
    </div>
</body>
</html>
1234567891011121314151617181920212223242526272829303132333435
```

显示：
![html jquery event hover](https://img-blog.csdnimg.cn/20200821195631949.gif#pic_center)

可以看到，hover同时实现了mouseenter和mouseleave事件。

resize测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .con{
            width: 200px;
            height: 200px;
            background-color: rebeccapurple;
        }
        .box{
            width: 100px;
            height: 100px;
            background-color: sienna;
        }
    </style>
    <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(window).resize(function(){
            var $w = $(window).width();
            alert($w);
        })
    </script>
</head>
<body>
    <div class="con">
        <div class="box"></div>
    </div>
</body>
</html>
1234567891011121314151617181920212223242526272829303132
```

显示：
![html jquery event resize](https://img-blog.csdnimg.cn/20200821195649677.gif#pic_center)

## 五、特殊效果和动画

### 1.特殊效果

JQuery常见特殊效果如下：

| 效果          | 意义                     |
| ------------- | ------------------------ |
| fadeIn()      | 淡入                     |
| fadeOut()     | 淡出                     |
| fadeToggle()  | 切换淡入淡出             |
| hide()        | 隐藏元素                 |
| show()        | 显示元素                 |
| toggle()      | 切换元素的可见状态       |
| slideDown()   | 向下展开                 |
| slideUp()     | 向上卷起                 |
| slideToggle() | 依次展开或者卷起某个元素 |

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box {
            width: 300px;
            height: 300px;
            background-color: teal;
            display: none;
        }
    </style>
    <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(function () {
            $('#btn1').click(function () {
                // 淡入
                $('.box').fadeIn(1500);
            })
            $('#btn2').click(function () {
                // 淡出
                $('.box').fadeOut(2500);
            })
            $('#btn3').click(function () {
                // 淡入淡出
                $('.box').fadeToggle(500);
            })
            $('#btn4').click(function () {
                // 卷起
                $('.box').slideDown(500);
            })
            $('#btn5').click(function () {
                // 卷起卷出
                $('.box').slideToggle(1500);
            })
        })
    </script>
</head>

<body>
    <input type="button" value="淡入" id="btn1">
    <input type="button" value="淡出" id="btn2">
    <input type="button" value="淡入淡出" id="btn3">
    <input type="button" value="卷起" id="btn4">
    <input type="button" value="卷起卷出" id="btn5">
    <div class="box"></div>
</body>

</html>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152
```

显示：
![html jquery special effect](https://img-blog.csdnimg.cn/20200821195724813.gif#pic_center)

### 2.动画

动画调用`animate()`方法，用法举例如下：

```js
$('.box').animate({'height':'400px'},1000,function(){$('.box').animate({'width':'400px'});});
1
```

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box {
            width: 300px;
            height: 300px;
            background-color: teal;
        }
    </style>
    <script type="text/javascript" src="js/jquery-3.5.1.min.js"></script>
    <script type="text/javascript">
        $(function () {
            $('#btn1').click(function () {
                // 变大
                $('.box').animate({'width': '600px'}, 1500, function(){
                    $('.box').animate({'height': '700px'}, 2500)
                });
            })
            $('#btn2').click(function () {
                // 持续增长
                $('.box').animate({'width': '+=50'});
            })
        })
    </script>
</head>

<body>
    <input type="button" value="变大" id="btn1">
    <input type="button" value="持续增长" id="btn2">
    <div class="box"></div>
</body>

</html>
1234567891011121314151617181920212223242526272829303132333435363738
```

显示：
![html jquery animate](https://img-blog.csdnimg.cn/20200821195743587.gif#pic_center)