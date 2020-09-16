# Python全栈 Linux基础之1.Linux初章

## 一、操作系统简介

**操作系统**(Operation System,简称OS)
操作系统是管理和控制计算机硬件与软件资源的计算机程序，是直接运行在**裸机**上的最基本的系统软件，任何其他软件都必须在操作系统的支持下才能运行。
裸机：
没有安装操作系统的计算机被称为裸机。
裸机的必要硬件：
CPU（中央处理器）、内存条、磁盘、声卡、显卡等。
操作系统是运行在CPU上的。
正常运行的必要软件：

- 操作系统：调用硬件，使功能正常运行；
- 一般软件：操作操作系统，间接调用硬件。

比喻：裸机是毛坯房，装上操作系统后就成了装修好的房子。

### 1.个人计算机PC（Personal Computer）操作系统–客户端

- Windows系列：一般用户
- MacOS：高等办公
- Linux：程序开发

### 2.服务器操作系统

- Linux：
  占有率高，免费，功能更齐全，为了解决多用户同时使用而诞生。
- Windows：
  占有率低，收费。

### 3.嵌入式及移动移动设备操作系统

- Linux（嵌入式）
- iOS（基于Unix）
- Android（基于Linux）

## 二、操作系统发展史

几个改变世界的人：
![几个改变世界的人](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yOTkwNzMwLWE5YzJkMjIwYTlhZDNiZTgucG5n?x-oss-process=image/format,png)

### 1.简述：

> Unix的诞生：
> 肯让自己开发的游戏运行
> 肯在完善Unix的时候，创造了C语言
>
> 公司介入：商业化，收费
>
> 比尔盖茨： IBM进军个人计算机 Dos系统 CPU生产：Intel 比尔盖茨与乔布斯交流之后推出Windows
>
> Linux的出现与黑客组织有关 林纳斯读大学时，在Minux的基础上开发出Linux
> 雷蒙德：黑客，传播Linux，发起GNU，开发软件、开源

### 2.发展详述

#### Unix的诞生

**大事件：**

- 1965年 ,人类为了研发一个多用途，多用户的操作系统。有三个著名的公司联合起来进行研发。贝尔实验室、通用电器公司（General Electrics；G.E.）及麻省理工学院（Massachusetts Institute of Technology；MIT）。
- 1969年 由于项目渐渐迷失了方向(估计是 进度缓慢 然后 资金短缺) 最终 贝尔实验室（Bell Labs）退出了这个项目。
- 1969年 肯.汤姆逊 为了他的游戏(Space Travel)星际旅游能够跑起来 找到了一台被淘汰的电脑 pdp-7 花了一个月的时间,写出了一个伟大的操作系统 Unix 的原型
- 肯的另一个同事丹尼斯.里奇对Unix有着很大兴趣,他就在当时的一门高级语言 BCPL 的基础上开发了一种新的高级语言将Unix重新写了一遍.这个语言也就是后来大名鼎鼎的C语言
- 1969年8月 芬兰一个叫做林纳斯 Linus.Torvalds的婴儿来到了这个世界。

> 尽管是为了一款游戏 . 但是事实就是如此 Unix和C诞生了，C与Unix很快成为全世界的主导开启了操作系统的新历史

由于Unix是开源的，这使得Unxi的功能和特性很快的被丰富。
贝尔实验室隶属于 AT&T公司(美国电话及电报公司).公司很快看到了商机.将Unix私有化,并注册Unix商标.这时的Unix价格非常的昂贵。
此时很多公司也都分分投靠 AT&T 并开发自己的Unxi版本.其中有著名的IBM公司。

#### Windows 和 MacOS 的恩恩怨怨

毫无疑问，Unix的诞生，对与计算机的发展起到了至关重要的作用。
时光来到了1975年，IBM的一个市场总监做了一个大胆的预测，他预测大概在20世纪末世界上个人计算机的数量将会普及。
那么这个预测就促使IBM开始进军个人电脑市场!那么就是因为这个大胆的假设和IBM的决定!!成就了世界两个巨无霸企业。
由于当时Unix价格昂贵，IBM一台个人电脑造价2w美元，操作系统都要花4w。所以IBM不得不放弃Unix。
但是操作系统与CPU又有着直接的关系，不同的cpu运行的操作系统是不一样的。而当时的主流CPU都是兼容Unix操作系统的.所以IBM只能选择一家小公司来生产cpu.这家公司就是Inter!!(没错,当时他还只是一个小公司)。

当IBM拥有了CPU之后，微软的创始人**比尔盖茨**花了5W美金 买下了他同学利用一个月的时间写的操作系统.对方欣喜若狂,简直快要疯了!!
比尔盖茨得到之后,将这个操作系统 命名为DOS .并且带上操作系统.找到了他妈妈 .当时是IBM董事会的成员.然后与IBM签订了合约!! 那么他的同学这时候就真的疯了!!
由于IBM需要廉价的计算机。微软可以提供廉价的DOS系统.所以顺水推舟 .但是聪明 的比尔盖茨 不是卖操作系统，而是卖操作系统的许可。每台IBM兼容机都捆绑一个DOS。比尔盖茨的招数就是捆绑，从dos到IE都是如出一辙，当然特别奏效。IBM认为硬件才是赚钱的根本,所以才会和年轻比尔盖茨合作.结果电脑进入市场,反响非常好，比尔盖茨也就成为了千万富豪!!

在市场的另一边 另外一小伙子在一个车库成立了一家公司! 他也在卖电脑 .这个人就是**乔布斯**。
当时乔布斯的苹果公司 采用的是unix 操作系统 后期发展为**MacOS** .由于 乔布斯 对产品的卓越追求，让他不断的改进Apple的电脑,震撼了整个个人电脑市场，因此乔布斯身价过亿。
Apple蒸蒸日上，比尔盖茨不爽了! 不可否认乔布斯和比尔盖茨都很伟大，但是不同的是比尔盖茨更狡猾!! 有一天比尔盖茨找到了乔布斯，大加赞赏了他的伟大，然后卑微的祈求一份Apple的产品原型，并承诺微软的一切成果都是苹果的。不可一世的乔布斯答应了，比尔盖茨得到了产品原型机，马上组织团队研发，并在1990年5月份推出**Windows3.0**!!!从而开始了微软在操作系统上的垄断地位。
也是从这一刻开始,微软和苹果的**血海深仇**就此结下。

#### Linux的传奇

那么在 微软的Windows 和 苹果的 MacOS 大战的时候，Unix在干嘛呢?? Unix正忙着打官司，然后一直卖着它 昂贵的操作系统 也正是这些时间，错过了个人计算机 操作系统发展的黄金时间!
**Minix：**
那么时间 来到了 1991年 !! 还记得22年前在 芬兰出生的那个男孩吗?林纳斯 他现在正在读大学.学校有一门课程叫做**操作系统** ，但是由于 Unix的闭源，林纳斯的大学教授就没法讲课了. 但是那个年代的大牛就是这么牛逼，这个教授一狠心 就自己写了一个操作系统,用来上课. 这个操作系统具备了Unix的基本功能, 由于比较小巧，所以叫做 Minix。
**Linux诞生：**
但是Minix 和一开始的 Unix 一样.不具备移植性 在其他的机器上面 没法安装。
而 林纳斯 有一台自己的电脑,却不能将Minix运行在自己的电脑上,所以 林纳斯 也不得不走上了他无数前辈的道路，自己写一个操作系统!
仅仅两个月后 林纳斯 就完成了第一个版本的操作系统.并通过自己的名字命名.叫做Linux . 随后他将源码上传到网络.让大家一起来完善其功能.可是那时的Linux一直只流行在在大学的校园.
那么Linux是如何推广到全球,最终 成为 操作系统 的 王者的呢? 这里就不得不提到一个人!! ESR 埃里克·雷蒙德(Eric Raymond)
**GNU和Linux的结合：**
雷蒙德 是何许人也? 著名的黑客。有一部纪录片Revolution OS(操作系统革命) 这样描述的

> 在一次开发者大会上，雷蒙德 遇到一个微软工程师，他看到对方衣服上的标志 就问他是不是在微软工作.
>
> 那位工程师 略带一点 嘲讽和鄙视 就反问 雷蒙德 你是干什么的
>
> 雷蒙德 微微一笑 留下了一句很牛逼的话：我 是你们可怕的恶梦。。。

当然雷蒙德 是微软的恶梦吗？不 他 不仅仅是 微软的噩梦 ，他 是一切收费软件公司的恶梦。微软靠卖软件大发特发。这让雷蒙德很不满。他认为所有软件都应该自由的让人们使用。

所以 1983年，雷蒙德 发起了“GNU（GNU’s Not Unix的递归缩写）”计划。GNU激发了软件界极大的热情，世界各地的软件奇才全部加入进来，开发出很多优秀的软件 比如 C语言编译器，gcc 等等。而且 很多免费软件的水平甚至都已经超过了相应的付费版本。

可是还有一个问题没有解决。GNU编写了很多自由免费的软件，可是这些免费软件却运行在收费 的Unix上。所以雷蒙德 这个时候就很郁闷… 然后承诺大家两年内重新写一个操作系统，但是 黑客是不愿意按部就班做事的。就这样五年时间过去了,操作系统还没出来!!。当然 历史总是如此的巧合!!雷蒙德 懊恼没有操作系统。而 芬兰那边，林纳斯 只有一个操作系统内核 Linux 而没有相应的应用软件。他们在各自的领域奋斗多年之后，命运终于安排他们走到了一起 最终 Linux 加入了 GNU 计划!!
然后,整个世界都变了!!今天在全球前500台超级计算机中，有413台选用Linux。这些计算机遍布世界各地的多个行业，大到航天科技，小到IC卡芯片，全都由Linux所主宰。在移动领域，Android 就是 Linux 内核。服务器也是Linux独领风骚!Linux就像病毒一样,在一群黑客手中,席卷全球！我,是你们可怕的噩梦! Linux已然完成对微软的复仇!
它是一个标志! 是一群黑客追求自由,共享所开启的一个新的时代!!

## 三、Linux操作系统

Linux系统分为Linux内核和发行版。

### 1.Linux内核（Kernel）

操作系统内核是指大多数操作系统的核心部分。
它由操作系统中用于管理存储器、文件、外设和系统资源的那些部分组成。
操作系统内核通常运行进程，并提供进程间的通信。
Linux内核版本又分为**稳定版**和**开发版**，两种版本是相互关联、相互循环。
**稳定版**：具有工业级强度，可以广泛地应用和部署；
**开发版**：由于要试验各种解决方案，所以变化很快。
内核源码网址：[http://www.kernel.org](http://www.kernel.org/)。

### 2.Linux发行版

即我们常说的Linux操作系统，也是由Linux内核与各种常用软件的集合产品，类似Windows包含了桌面环境。
全球大约有数百款的Linux系统版本，每个系统版本都有自己的特性和目标人群。

## 四、Ubuntu简介及操作简单演示

### 1.简介

Ubuntu(乌班图)是一个以桌面应用为主的开源GNU/Linux操作系统,主要依赖Canonical有限公司的支持，同时也有很多来自Linux社区的热心人士提供协助。
作为Linux发行版之一，Canonical 的Ubuntu 胜过其他所有的 Linux 服务器发行版 ,它简单易用同时又相当稳定，而且具有庞大的社区力量，用户可以方便地从社区获得帮助。
Ubuntu在服务器领域是妥妥的赢家。

※在Windows下安装虚拟机+Ubuntu的详细过程可参考https://www.jianshu.com/p/2883f9af89db。

### 2.Ubuntu的目录结构

如图
![Ubuntu的主要目录](https://img-blog.csdnimg.cn/2020012114571488.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

#### Ubuntu的主要目录

- /：根目录，一般根目录下只存放目录，在 linux 下有且只有一个根目录，所有的东西都是从这里开始
- /bin、/usr/bin：可执行二进制文件的目录，如常用的命令 ls、tar、mv、cat 等
- /boot：放置 linux 系统启动时用到的一些文件，如 linux 的内核文件：/boot/vmlinuz，系统引导管理器：/boot/grub
- /dev：存放linux系统下的设备文件，访问该目录下某个文件，相当于访问某个设备，常用的是挂载光驱mount /dev/cdrom /mnt
- /etc：系统配置文件存放的目录，不建议在此目录下存放可执行文件，重要的配置文件有
  - /etc/inittab
  - /etc/fstab
  - /etc/init.d
  - /etc/X11
  - /etc/sysconfig
  - /etc/xinetd.d
- /home：系统默认的用户家目录，新增用户账号时，用户的家目录都存放在此目录下
  - ~ 表示当前用户的家目录
  - ~edu 表示用户 edu 的家目录
- /lib、/usr/lib、/usr/local/lib：系统使用的函数库的目录，程序在执行过程中，需要调用一些额外的参数时需要函数库的协助
- /lost+fount：系统异常产生错误时，会将一些遗失的片段放置于此目录下
- /mnt: /media：光盘默认挂载点，通常光盘挂载于 /mnt/cdrom 下，也不一定，可以选择任意位置进行挂载
- /opt：给主机额外安装软件所摆放的目录
- /proc：此目录的数据都在内存中，如系统核心，外部设备，网络状态，由于数据都存放于内存中，所以不占用磁盘空间，比较重要的文件有：/proc/cpuinfo、/proc/interrupts、/proc/dma、/proc/ioports、/proc/net/* 等
- /root：系统管理员root的家目录
- /sbin、/usr/sbin、/usr/local/sbin：放置系统管理员使用的可执行命令，如 fdisk、shutdown、mount 等。与 /bin 不同的是，这几个目录是给系统管理员 root 使用的命令，一般用户只能"查看"而不能设置和使用
- /tmp：一般用户或正在执行的程序临时存放文件的目录，任何人都可以访问，重要数据不可放置在此目录下
- /srv：服务启动之后需要访问的数据目录，如 www 服务需要访问的网页数据存放在 /srv/www 内
- /usr：应用程序存放目录
  - /usr/bin：存放应用程序
  - /usr/share：存放共享数据
  - /usr/lib：存放不能直接运行的，却是许多程序运行所必需的一些函数库文件
  - /usr/local：存放软件升级包
  - /usr/share/doc：系统说明文件存放目录
  - /usr/share/man：程序说明文件存放目录
- /var：放置系统执行过程中经常变化的文件
  - /var/log：随时更改的日志文件
  - /var/spool/mail：邮件存放的目录
  - /var/run：程序或服务启动后，其 PID 存放在该目录下

### 3.Ubuntu的常见快捷键

可以在**System Setting -> Keyboard -> Shortcuts**中查看各种快捷键。

- 终端： **Ctrl+Alt+T**
- 终端新建标签页： **Ctrl+Shift+T**
- 终端复制粘贴: **Ctrl+Shift+C**, **Ctrl+Shift+V**
- 显示常用快捷键： 按住**Super(Win)**不动
- 截活动窗口图： **Alt+Print**
- 区域截图： **Shift+Print**
- 源切换： **Super(Win)+Space**
- 安装： `sudo apt-get install`
- 卸载： `sudo apt-get remove`
- 移除没用的包: `sudo apt-get autoremove`

### 4.Ubuntu的常见设置

#### 语言设置

- 通过右上角的 设置按钮 找到**System Settings**…
- 然后选中**Language Support** 项
- Ubuntu的语言选项有多种语言，将第一语言设置为中文(因为如果中文显示不了的,会使用英文显示)
- 设置完成后.选择**Apply System-wide**(应用到整个系统)这时，输入管理员密码以确认，最后点击 Close 按钮关闭对话框，重启电脑。

![语言设置](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yOTkwNzMwLTU5OTdhNDg0YmQyMjM2ZWQucG5n?x-oss-process=image/format,png)

注意：

- 不是点击，是拖动，分为第一语言、第二语言…；
- 重启成功后,会让你选择文件夹名称显示.如果是为了学习.我建议大家保持原来的文件夹名称,这样便于后期在学习中熟悉Linux目录结构. 选择**Keep Old Names**
  ![Keep Old Names](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8yOTkwNzMwLWU3NzQ2ZDdlNzhjZDg4YTMucG5n?x-oss-process=image/format,png)

#### Launcher(菜单栏)设置

在系统设置中,找不到菜单栏的位置设置.所以只能通过**终端命令**进行设置。
命令：

```shell
gsettings set 软件包名
1
```

软件包名必须**唯一**标识，如com.baidu.guanjia。
举例：

```shell
gsettings set com.baidu.guanjia xxx
1
```

- 菜单栏靠左

```shell
$ gsettings set com.canonical.Unity.Launcher launcher-position Left
1
```

- 菜单栏靠下

```powershell
$ gsettings set com.canonical.Unity.Launcher launcher-position Bottom
1
```

注意参数首字母大写

#### Ubuntu常用软件及安装

- 设置软件源：
  默认的软件源是官方的， 服务器在国外，速度慢的令人发指，国内提供镜像源，访问更快，将主服务器内容COPY。 所以需要先设置一个速度较快的软件源， **System Settings -> Software & Updates -> Ubuntu Software -> Download from**选择Others， 然后自动选择一个网速比较快的服务器(多半是某个大学的)即可。

**apt(Advanced Packaging Tool)**：
安装／卸载软件；
**CTRL+ALT+T**：
快速打开终端。

- 安装软件：

```shell
$ sudo apt install 软件包
1
```

- 卸载软件

```shell
$ sudo apt remove 软件名
1
```

- 更新已安装的包

```shell
$ sudo apt upgrade  或者 sudo apt-get upgrade
1
```

- 升级

```shell
 sudo apt-get update
1
```

由于有些Ubuntu中没有自带**vim** 而是 **vi** 这个古老的编辑器，所以我们需要安装vim

```shell
sudo apt-get install vim
1
```

※注意：在安装过程中有可能出现下列错误：

> vim : 依赖: vim-common (= 2:7.4.826-1ubuntu1) 但是 2:7.4.1689-3ubuntu1.1
> 正要被安装 E: 无法修正错误，因为您要求某些软件包保持现状，就是它们破坏了软件包间的依赖关系。
> 解决办法：

```shell
sudo apt-get remove vim-common
sudo apt-get install vim
```

# Python全栈 Linux基础之2.Linux终端命令简介

## 一、终端命令格式及帮助文档

### 1.终端命令的格式：

```powershell
command [options] [parameter]
1
```

| command  | [-options] | [parameter]      |
| -------- | ---------- | ---------------- |
| 命令名称 | 选项       | 传递给命令的参数 |

注意：
**[ ]** 代表内容可选填；
选项前面有 **-**，参数前面没有。
常见命令：
**ls**：
文件列表。

```powershell
Desktop    Downloads         Music     Public     Videos
Documents  examples.desktop  Pictures  Templates
12
```

**pwd**：
当前路径。

```powershell
/home/xxx
1
```

**~** 表示家目录
**clear**：
清屏。

### 2.help&&man

**–help**帮助文档：
command --help
显示command命令的帮助信息。

```powershell
ls --help
1
```

打印

```powershell
用法：ls [选项]... [文件]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

必选参数对长短选项同时适用。
  -a, --all			不隐藏任何以. 开始的项目
  -A, --almost-all		列出除. 及.. 以外的任何项目
      --author			与-l 同时使用时列出每个文件的作者
  -b, --escape			以八进制溢出序列表示不可打印的字符
      --block-size=SIZE      scale sizes by SIZE before printing them; e.g.,
                               '--block-size=M' prints sizes in units of
                               1,048,576 bytes; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                               modification of file status information);
                               with -l: show ctime and sort by name;
                               otherwise: sort by ctime, newest first
  -C                         list entries by columns
      --color[=WHEN]         colorize the output; WHEN can be 'always' (default
                               if omitted), 'auto', or 'never'; more info below
  ...
  -1                         list one file per line.  Avoid '\n' with -q or -b
      --help		显示此帮助信息并退出
      --version		显示版本信息并退出

The SIZE argument is an integer and optional unit (example: 10K is 10*1024).
Units are K,M,G,T,P,E,Z,Y (powers of 1024) or KB,MB,... (powers of 1000).

使用色彩来区分文件类型的功能已被禁用，默认设置和 --color=never 同时禁用了它。
使用 --color=auto 选项，ls 只在标准输出被连至终端时才生成颜色代码。
LS_COLORS 环境变量可改变此设置，可使用 dircolors 命令来设置。

退出状态：
 0  正常
 1  一般问题 (例如：无法访问子文件夹)
 2  严重问题 (例如：无法使用命令行参数)
...
12345678910111213141516171819202122232425262728293031323334353637
```

**man**使用手册：
man command
查询command命令的使用手册（man是manual的缩写，是Linux提供的手册）。

```powershell
man ls
1
```

显示：
![man command](https://img-blog.csdnimg.cn/20200123103544986.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
手册中的操作按键

| 操作按键 | 功能                        |
| -------- | --------------------------- |
| 空格键   | 显示下一页                  |
| 回车键   | 显示下一行                  |
| b        | back，显示上一页            |
| f        | forward，向前和空格效果一样 |
| q        | quit，退出                  |

## 二、最常用的Linux命令

### 1.常用命令

| 命令             | 功能                                    |
| ---------------- | --------------------------------------- |
| ls               | list，列表显示当前目录下的内容          |
| pwd              | print wrok directory ，查看当前所在路径 |
| cd 路径名称      | change directory,跳转到指定路径         |
| touch 文件名称   | 创建这个文件                            |
| mkdir 文件夹名称 | make directory， 创建这个文件夹         |
| rm 文件名        | remove,删除指定的文件                   |
| clear            | 清屏                                    |

注意：
rm无法删除文件夹，删除文件夹要用`rm -r xxx`；
通过终端删除文件不会保存到回收站，直接彻底删除。

### 2.练习

到桌面下：

```powershell
cd Desktop
1
```

再到Documents：

```powershell
cd /home/corley/Documents
1
```

创建新文件：

```powershell
touch 123.txt
touch 124.txt
touch 125.txt
123
```

创建新文件夹：

```powershell
mkdir 123
1
```

查看123文件夹中的文件：

```powershell
cd 123
pwd ls
12
```

删除文件：

```powershell
rm 125.txt
1
```

回到桌面删除文件夹：

```powershell
rm -r 123
1
```

## 三、终端常用快捷键

**终端常用快捷键速查表**：

| 快捷键                   | 功能                                           |
| ------------------------ | ---------------------------------------------- |
| Tab                      | 自动补全                                       |
| ctrl + shift + +         | 放大字体                                       |
| ctrl + -                 | 缩小字体                                       |
| Ctrl + Alt + t           | 打开终端窗口                                   |
| Ctrl + a                 | 光标移动到开始位置                             |
| Ctrl + e                 | 光标移动到最末尾                               |
| Ctrl + k                 | 删除此处至末尾的所有内容                       |
| Ctrl + u                 | 删除此处至开始的所有内容                       |
| Ctrl + d                 | 删除当前字符                                   |
| Ctrl + h                 | 删除当前字符前一个字符                         |
| Ctrl + w                 | 删除此处到左边的单词                           |
| Ctrl + y                 | 粘贴由Ctrl+u， Ctrl+d， Ctrl+w删除的单词       |
| Ctrl + l                 | 相当于clear，即清屏                            |
| Ctrl + r                 | 查找历史命令                                   |
| Ctrl + b                 | 向回移动光标                                   |
| Ctrl + f                 | 向前移动光标                                   |
| Ctrl + t                 | 将光标位置的字符和前一个字符进行位置交换       |
| Ctrl + &                 | 恢复 ctrl+h 或者 ctrl+d 或者 ctrl+w 删除的内容 |
| Ctrl + S                 | 暂停屏幕输出                                   |
| Ctrl + Q                 | 继续屏幕输出                                   |
| Ctrl + ←                 | 光标移动到上一个单词的词首                     |
| Ctrl + →                 | 光标移动到下一个单词的词尾                     |
| Ctrl + p                 | 向上显示缓存命令                               |
| Ctrl + n                 | 向下显示缓存命令                               |
| Ctrl + d                 | 关闭终端                                       |
| Ctrl + c                 | 终止进程/命令                                  |
| Shift + 上或下           | 终端上下滚动                                   |
| Shift + PgUp/PgDn        | 终端上下翻页滚动                               |
| Ctrl + Shift + n         | 新终端                                         |
| alt + F2                 | 输入gnome-terminal打开终端                     |
| Shift + Ctrl + T         | 打开新的标签页                                 |
| Shift + Ctrl + W         | 关闭标签页                                     |
| Shift + Ctrl + C         | 复制                                           |
| Shift + Ctrl + V         | 粘贴                                           |
| Alt + 数字               | 切换至对应的标签页                             |
| Shift + Ctrl + N         | 打开新的终端窗口                               |
| Shift + Ctrl + Q         | 管壁终端窗口                                   |
| Shift + Ctrl + PgUp/PgDn | 左移右移标签页                                 |
| Ctrl + PgUp/PgDn         | 切换标签页                                     |
| F1                       | 打开帮助指南                                   |
| F10                      | 激活菜单栏                                     |
| F11                      | 全屏切换                                       |
| Alt + F                  | 打开 “文件” 菜单（file）                       |
| Alt + E                  | 打开 “编辑” 菜单（edit）                       |
| Alt + V                  | 打开 “查看” 菜单（view）                       |
| Alt + S                  | 打开 “搜索” 菜单（search）                     |
| Alt + T                  | 打开 “终端” 菜单（terminal）                   |
| Alt + H                  | 打开 “帮助” 菜单（help）                       |

快捷键不用死记硬背，要形成**肌肉记忆**。

# Python全栈 Linux基础之3.Linux常用命令

## 一、ls及其使用

ls是英文单词list的缩写，功能是列出当前目录下的文件列表，是非常常见的Linux命令之一。

### 1.Linux下目录特点

以 **.** 开头的文件是隐藏文件，使用ls查看时，需要加上 **-a(all)** 参数才能显示。
**.** 代表当前目录，**.** **.** 代表上一级目录(可以理解为隐藏的两个文件路径)。
**cd .**
跳到当前目录；
**cd . .**
跳到上一级目录。

### 2.ls常用选项

| 选项 | 功能                                                       |
| ---- | ---------------------------------------------------------- |
| [-a] | all，显示所有内容，包含隐藏文件                            |
| [-l] | （字母L小写） 显示文件详细信息                             |
| [-h] | human-readable,需要配合-l(字母L小写)选项，所谓的人性化显示 |

**练习：**

```powershell
touch 123.txt
touch .123.txt
12
```

查看：

```powershell
ls -a
1
```

显示:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123205957311.png)

```powershell
ls -l
1
```

显示：
![ls -l](https://img-blog.csdnimg.cn/20200123175745776.png#pic_center)

```powershell
ls -l -h
1
```

显示：
![ls -l -h](https://img-blog.csdnimg.cn/20200123175840477.png#pic_center)
只是文件大小显示改变。
选项可以合并，且顺序不影响

```powershell
ls -lh
1
```

显示与前者相同。
查看所有文件和文件夹（包括隐藏文件）

```powershell
ls -lah
1
```

显示：
![ls -lah](https://img-blog.csdnimg.cn/2020012318003670.png#pic_center)

### 3.ls配合通配符

| 通配符 | 功能                                                         |
| ------ | ------------------------------------------------------------ |
| *      | 代表任意多个任意字符，可以没有字符                           |
| ?      | 代表就是一个任意字符，至少一个字符                           |
| [ ]    | 代表一个字符，取值范围在[ ]中，例如[1234 ]匹配1、2、3、4中的任意一个，[a-g] 匹配从a到g范围内的任意一个 |

应用：
批量查询、修改文件。
**练习：**

```powershell
touch 123.txt
touch 124.txt
touch 125.txt
touch 126.txt
touch 223.txt
touch 323.txt
touch 111.txt
touch 131.txt
touch 11.txt
touch 12345.txt
ls
1234567891011
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123185620112.png#pic_center)
找到以1开头的文件

```powershell
ls 1*
1
```

显示：
![ls 1*](https://img-blog.csdnimg.cn/20200123185419150.png)
找到以3结尾的文件

```powershell
ls *3.txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123185456616.png#pic_center)

```powershell
ls 1*1.txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020012318553211.png#pic_center)

```powershell
ls ?1?.txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020012318571288.png#pic_center)
查询含有1的文件：

```powershell
ls *1*.txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123185808538.png#pic_center)

```powershell
ls 12[12345].txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123185911503.png#pic_center)

```powershell
ls 12[1-6].txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123185932138.png#pic_center)

```powershell
rm 1*1.txt
ls
12
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123190043255.png#pic_center)

```powershell
rm *.txt
ls -a
12
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123190228262.png#pic_center)
删除文件时，只删除了非隐藏文件
删除全部文件用

```powershell
rm .*.txt
ls -a
12
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123190332772.png#pic_center)

## 二、cd命令

cd是英文单词**change directory**的缩写，其功能就是跳转目录。
它与之前的几个命令不同的是，cd是BASH内置命令，没有帮助文档与相关手册。
所以在使用`$which cd`的时候是看不到它的二进制路径的，因为系统中不存在cd命令的二进制文件。

| 命令 | 功能                                                         |
| ---- | ------------------------------------------------------------ |
| cd   | 切换到当前用户的家目录（home/用户名）                        |
| cd ~ | 和 cd 效果一样                                               |
| cd … | 跳转到上一级目录                                             |
| cd - | 在最近两个目录来回切换，有点像图形界面的 Alt + Tab切换窗口的感觉 |

cd后面的路径可以是两种：
**相对路径**和**绝对路径**。
**相对路径**：
指相对当前目录的路径；
**绝对路径**：
是指全路径。可以从 /（根目录）开始，或者是~（家目录）开始。
注意：
Linux下目录名称以及文件名称**大小写**是有区别的！
**练习：**

```powershell
mkdir aaa
cd aaa
mkdir bbb
cd bbb
mkdir ccc
cd ccc
pwd
1234567
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123191003854.png#pic_center)
回到家目录：

```powershell
cd ~
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123191039910.png#pic_center)
再次到ccc：

```powershell
cd -
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123191126692.png#pic_center)
逐级回到家目录：

```powershell
cd ..
cd ..
cd ..
123
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123191220173.png#pic_center)

```powershell
cd ../Downloads
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123191255231.png#pic_center)

## 三、创建和删除的操作

### 1.mkdir命令

创建一个新的目录。

| 选项 | 功能             |
| ---- | ---------------- |
| [-p] | 可以递归创建目录 |

注意：
新建的目录名称不能重名。

### 2.rm命令

删除文件或目录。

| 选项 | 功能                                         |
| ---- | -------------------------------------------- |
| [-f] | 强制删除，忽略不存在的文件，无需提示         |
| [-r] | 递归删除目录下的内容，删除文件夹就用这个选项 |

**练习：**

```powershell
touch 123.txt
1
```

说明：
创建文件时，如果文件存在，只修改创建时间，不改变内容。
创建嵌套目录：

```powershell
mkdir -p a/b/c
1
```

结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123192118884.png#pic_center)
在Linux中，文件和文件夹不能重名。

```powershell
touch 123
makdir 123
12
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123192342504.png#pic_center)
先创建文件夹，再创建同名文件时，相当于改变创建时间。

```powershell
rm -f aabbcc
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020012319244872.png#pic_center)

```powershell
rm -r *
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123192531516.png#pic_center)

## 四、复制、移动文件

### 1.拷贝文件

**cp命令**：
拷贝文件或者目录，类似DOS中的copy。

```powershell
cp 源文件 目标文件
1
```

| 选项 | 功能                                                         |
| ---- | ------------------------------------------------------------ |
| [-i] | interactive互动，覆盖文件时有提示                            |
| [-r] | 如果cp跟上的是目录，那么将会递归拷贝目录下的所有子目录和文件 |

**tree命令**：
tree命令可以将目录结构显示出来（树状显示）。

| 选项 | 功能                  |
| ---- | --------------------- |
| [-d] | directory，只显示目录 |

有的同学系统中默认没有此命令。所以会提示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123194535797.png#pic_center)
可以用

```powershell
sudo snap install tree
1
```

命令安装。
**练习：**

```powershell
mkdir -p aaa/bbb/ccc
tree
12
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123193005684.png#pic_center)

```powershell
touch aaa/123.txt
tree
12
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123193114950.png#pic_center)

```powershell
tree ~
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123193150613.png#pic_center)

```powershell
cd /
mkdir -p ~/Desktop/a/b/c
tree ~/Desktop
123
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020012319334349.png#pic_center)
只看目录

```powershell
tree -d ~/Desktop
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123193413245.png#pic_center)

```powershell
cd Desktop/
cp aaa/123.txt .
tree ~
123
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123193818464.png#pic_center)

```powershell
cp aaa/123.txt test.txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123193937707.png#pic_center)
带提示：

```powershell
cp -i aaa/123.txt .
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123194131638.png#pic_center)
拷贝文件夹：

```powershell
cp -r a b
tree
12
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123194220883.png#pic_center)
如果目标文件夹存在相同名称文件，则会提示覆盖。

### 2.移动文件

**mv命令**：
mv是move的缩写，用来移动文件/目录。
小技巧：
如果需要重命名，也可以使用mv命令覆盖当前文件/目录 达到效果

| 选项 | 功能                         |
| ---- | ---------------------------- |
| [-i] | interactive,覆盖文件时有提示 |

**练习**：

```powershell
mkdir -p a/b/c
touch a/b/c/123.txt a/b/c/124.txt a/b/c/125.txt
tree
123
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123195044584.png#pic_center)

```powershell
mv a/b/c/123.txt .
mv a/b/c .
12
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123195026596.png#pic_center)
移动文件夹**不需要-r**选项。
如果目标文件夹存在相同名称文件，则会提示覆盖。
带提示：

```powershell
mv -i c/124.txt 123.txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/202001231953323.png#pic_center)

```powershell
mv -i 123.txt readme.txt
1
```

可实现修改文件名的功能。

## 五、查看文件内容相关命令

| 命令                 | 功能                                                         |
| -------------------- | ------------------------------------------------------------ |
| cat 文件名           | concatenate,查看文件内容、创建文件、文件合并、追加文件内容等 |
| more 文件名          | more,分屏显示文件内容（内容多一般用more）                    |
| grep 搜索内容 文件名 | grep，搜索文件内容                                           |

### 1.查看文件

**cat**：

- 查看文件内容、创建文件、文件合并、追加文件内容等；
- 命令会一次性显示所有内容，所以适合查看内容较少的文件。

| 选项 | 功能                             |
| ---- | -------------------------------- |
| [-b] | 显示每一行的行号                 |
| [-n] | 只显示有内容的行号，空行不算一行 |

**more**：
此命令可以分屏显示文件内容，每次只显示一页内容。所以适合查看内容多的文件。
使用more的操作按键：

| 操作按键     | 功能                        |
| ------------ | --------------------------- |
| 空格键       | 显示下一页                  |
| 回车键 Enter | 显示下一行                  |
| b            | back，显示上一页            |
| f            | forward，向前和空格效果一样 |
| q            | quit，退出                  |
| / 搜索文字   | 搜索文本中的内容            |

```powershell
cat 123.txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123201720865.jpg#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

```powershell
cat -b 123.txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123201459432.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

```powershell
cat -n 123.txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123201842261.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

```powershell
more 123.txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123201924557.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 2.查找文件内容

**grep**（global search regular expression(RE) and print out the line，全面搜索正则表达式并把行打印出来）是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。

| 选项 | 功能                          |
| ---- | ----------------------------- |
| [-n] | line-number，显示匹配行及行号 |
| [-v] | invert-match，选中不匹配的行  |
| [-i] | ignore-case，忽略大小写       |

grep常用查找方式：

- 在file_name中 搜索Hello_world这个单词

```powershell
grep Hello_world file_name
grep "Hello_world" file_name
12
```

- 在多个文件中查找

```powershell
grep "Hello_world" file_1 file_2 file_3 ...
1
```

- 常用两种模式查找

| 参数   | 功能                      |
| ------ | ------------------------- |
| ^hello | 行首，搜索以hello开头的行 |
| world$ | 行尾，搜索以world结束的行 |

注意：
区分大小写。

```powershell
grep corley 123.txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123203132249.png#pic_center)
带行号：

```powershell
grep -n corley 123.txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123203217515.png#pic_center)
偏移字节数：

```powershell
grep -b corley 123.txt
1
```

显示;
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123203258880.png#pic_center)
不区分大小写：

```powershell
grep -in corley 123.txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123203344810.png#pic_center)
不包含corley：

```powershell
grep -inv corley 123.txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123203709499.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
字符中间有空格时，加双引号：

```powershell
grep -inv "hello corley" 123.txt
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123203832136.png#pic_center)

## 六、重定向&管道输出符

### 1.重定向

符号 **> >>**

- Linux中允许将命令执行结果 重定向到一个文件；
- 将本应该显示在终端上的内容**输出/追加**到指定文件中。

| 符号 | 功能                                      |
| ---- | ----------------------------------------- |
| >    | 输出重定向到一个文件或设备 覆盖原来的文件 |
| >>   | 输出重定向到一个文件或设备 追加原来的文件 |

echo会在终端中显示参数指定的文字，通常会和**重定向**联合使用

### 2.管道输出

符号 **|**

- Linux允许将一个命令的输出通过管道作为另外一个命令的输入。

```powershell
 command 1 |  command 2 |  command 3 … …
1
echo hello world > abc
vi abc
12
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123204358795.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

```powershell
ls -lh > abc
1
```

结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020012320475470.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

```powershell
tree >> abc
1
```

结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123205017988.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
管道符：

```powershell
ls -lh ~

ls -lah ~

ls -lah ~ > b

grep -n vi b
1234567
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123205213221.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
相当于管道输出：

```powershell
ls -lah ~ | grep -n vi
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123205309618.png#pic_center)

```powershell
ls -lah ~/.*vi*
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123205453819.png#pic_center)
这种输出相对不直观。

```powershell
ls -lah ~ | grep -in vi
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200123205639795.png#pic_center)

# Python全栈 Linux基础之4.远程管理命令

## 一、关机重启命令

**shutdown**命令可以安全关闭或者重新启动系统。直接使用 shotdown命令，默认表示1分钟后关机。
命令格式：

```powershell
$shutdown [选项] <参数>
1
```

| 选项 | 功能               |
| ---- | ------------------ |
| [-r] | 重新启动           |
| [-c] | 取消之前的关机计划 |

**参数**：
[时间]：设置多久时间后执行shutdown指令；
[警告信息]：要传送给所有登入用户的信息。

### 举例：

- 一分钟以后关机

```powershell
$shutdown  
1
```

- 立刻关机

```powershell
$shutdown now
1
```

- 在今天的19：00关机

```powershell
$shutdown 19:00
1
```

- 10分钟以后关机

```powershell
$shutdown +10
1
```

![$shutdown +10](https://img-blog.csdnimg.cn/20200128172528216.png)

- 10分钟以后关机，同时发出警告信息

```powershell
$shutdown +10 "System will shutdown after 10 minutes"
1
```

- 取消关机计划

```powershell
$shutdown -c
1
```

- 重启

```powershell
$shutdown -r
1
```

- 马上重启

```powershell
$shutdown -r now
1
```

`reboot`命令也可以用来重新启动正在运行的Linux操作系统，和 `shutdown -r now`一样。

## 二、网络配置命令

| 命令     | 功能                                                        |
| -------- | ----------------------------------------------------------- |
| ifconfig | configure a network interface,查看/配置计算机当前的网卡信息 |
| ping     | 测试目标ip地址的连接是否正常                                |

### 1.ifconfig命令

`ifconfig`命令用于配置和显示Linux中网卡信息。
查看网卡信息：

```powershell
$ifconfig
1
```

![$ifconfig](https://img-blog.csdnimg.cn/20200128173536248.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
快速定位IP地址：

```powershell
$ifconfig | grep inet
1
```

![$ifconfig | grep inet](https://img-blog.csdnimg.cn/20200128173706548.png)
说明：
一台计算机中可能会有一个**物理网卡**和多个**虚拟网卡**，在Linux中物理网卡名字一般是**ensXX**；
**127.0.0.1**是一个比较特殊的地址，称之为本地回环地址，可以用来测试本机网卡是否正常工作。#

### 2.ping命令

ping命令用来测试主机之间网络的连通性。
执行ping指令会使用ICMP传输协议，发出要求回应的信息。一般用于检测计算机之间的网络通讯是否正常。
由于ping命令的工作原理，服务器人员给往往将ping用作动词。经常说：“ping一下某某计算机”。
**ping目标主机**：

```powershell
$ping IP地址
1
```

如

```powershell
ping 180.76.76.76
1
```

![ping 180.76.76.76](https://img-blog.csdnimg.cn/20200128175750256.png)
**检测本地网卡是否正常**：

```powershell
$ping 127.0.0.1
1
```

![$ping 127.0.0.1](https://img-blog.csdnimg.cn/20200128175651469.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
**说明**：
结束ping的执行使用**Ctrl+C**。在Linux中终止一个终端程序绝大多数都可以使用Ctrl+C。

## 三、远程登录

### 1.SSH远程登录

**SSH(Secure Shell)** 是一种网络协议，用于计算机之间的加密登录。
最早的时候，互联网通信都是明文通信，一旦被截获，内容就暴露无疑。1995年，芬兰学者Tatu Ylonen设计了SSH协议，将登录信息全部加密，成为互联网安全的一个基本解决方案，迅速在全世界获得推广，目前已经成为Linux系统的标准配置。
**OpenSSH**：
SSH只是一种协议，存在多种实现OpenSSH就是其中一种,它是一款软件，应用非常广泛，在Mac以及Ubuntu中都自带OpenSSH。
**SSH客户端命令**：

```powershell
ssh [-p port] user@remote
1
```

其中：

- user 是远程端上的用户名，默认是当前用户；
- remote是远程端的地址，可以是IP/域名；
- port是远程端的端口，默认是22。

Ubuntu下**SSH**分：

- openssh-client（客户端）
- openssh-server (服务端)

**检测是否有开启ssh服务**：

```powershell
$ps -e | grep ssh
1
```

![$ps -e | grep ssh](https://img-blog.csdnimg.cn/20200128180638656.png)
说明：
其中sshd 为server端的守护进程，如果没有出现sshd，那么很有可能你的系统中没有安装server端。或者ssh服务没有启动。
**开启ssh服务**：

```powershell
sudo /etc/init.d/ssh start
1
```

![sudo /etc/init.d/ssh start](https://img-blog.csdnimg.cn/20200128180717310.png)
**安装openssh-server**：
如果显示上述命令找不到，是因为我们的Ubuntu系统默认没有服务端，所以可以通过下面命令安装。

```powershell
$sudo apt-get install openssh-server
1
```

![$sudo apt-get install openssh-server](https://img-blog.csdnimg.cn/20200128180553108.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
可能出现错误：

```powershell
$ sudo apt-get install openssh-server
正在读取软件包列表... 完成
正在分析软件包的依赖关系树       
正在读取状态信息... 完成       
有一些软件包无法被安装。如果您用的是 unstable 发行版，这也许是
因为系统无法达到您要求的状态造成的。该版本中可能会有一些您需要的软件
包尚未被创建或是它们已被从新到(Incoming)目录移出。
下列信息可能会对解决问题有所帮助：

下列软件包有未满足的依赖关系：
 openssh-server : 依赖: openssh-client (= 1:7.1p1-4)
                  依赖: openssh-sftp-server 但是它将不会被安装
                  推荐: ssh-import-id 但是它将不会被安装
E: 无法修正错误，因为您要求某些软件包保持现状，就是它们破坏了软件包间的依赖关系。
1234567891011121314
```

原因：
openssh-server 需要依赖openssh-client，但是很明显，我们系统自带的版本和目前要安装的server版本不同。所以我们重新安装一下client版本。
解决：

```powershell
corley@ubuntu:~$ sudo apt-get install openssh-client=1:7.1p1-4
正在读取软件包列表... 完成
正在分析软件包的依赖关系树       
正在读取状态信息... 完成       
建议安装：
  ssh-askpass libpam-ssh keychain monkeysphere
下列软件包将被【降级】：
  openssh-client
升级了 0 个软件包，新安装了 0 个软件包，降级了 1 个软件包，要卸载 0 个软件包，有 0 个软件包未被升级。
需要下载 581 kB 的归档。
解压缩后将会空出 36.9 kB 的空间。
您希望继续执行吗？ [Y/n] y
获取:1 http://mirror.neu.edu.cn/ubuntu xenial/main amd64 openssh-client amd64 1:7.1p1-4 [581 kB]
已下载 581 kB，耗时 33秒 (17.6 kB/s)                                           
dpkg：警告：即将把 openssh-client 从 1:7.2p2-4 降级到 1:7.1p1-4
123456789101112131415
正在将 openssh-client (1:7.1p1-4) 解包到 (1:7.2p2-4) 上 ...
正在处理用于 man-db (2.7.5-1) 的触发器 ...
正在设置 openssh-client (1:7.1p1-4) ...
正在安装新版本配置文件 /etc/ssh/ssh_config ...
1234
```

这样可以看到降级成功，然后我们再次安装openssh-server就可以了。

### 2.远程拷贝指令

**SCP(Secure copy)** 是linux系统下基于ssh登陆进行安全的远程文件拷贝命令。
scp是基于ssh的，连接后，不需要登录，直接拷贝。
命令格式：

```powershell
scp -P port 源文件路径 目标文件路径
# 将本地目录下的123.txt拷贝到远程桌面目录下
scp -P port 123.txt user@remote:Desktop/123.txt

# 把远程桌面目录下的123.txt文件复制到本地当前目录下
scp -P port user@remote:Desktop/123.txt 123.txt

# 加上 -r 选项可以传送文件夹
# 把当前目录下的demo文件夹复制到远程家目录下的Desktop
scp -r demo user@remote:Desktop

# 把远程家目录下的Desktop复制到当前目录下的demo文件夹
scp -r user@remote:Desktop demo
12345678910111213
```

| 选项 | 功能                                                         |
| ---- | ------------------------------------------------------------ |
| -r   | 若给出的源文件是目录文件，则 scp 将递归复制该目录下的所有子目录和文件，目标文件必须为一个目录名 |
| -P   | 若远程 SSH 服务器的端口不是 22，需要使用大写字母 -P 选项指定端口 |

例如：

```powershell
#从服务器端复制到客户端
scp -P 22 cl@192.168.0.113:Desktop/corley/A/a/123.txt .

#从客户端复制到服务器端
scp -P 22 124.txt cl@192.168.0.113:Desktop/corley/A/a/
12345
```

### 3.SSH登录原理

**加密：**
防止中间人攻击。
常用加密方式为非对称加密：
公钥 – 加密数据；
私钥 – 解密数据。
**登录过程：**

- 远程主机收到用户的登录请求，将自己的公钥发送给用户；
- 用户使用这个公钥，将登录密码加密后，发送回来；
- 远程主机使用自己的私钥解密登录密码，如果密码正确，就同意登录。

### 4.SSH常见配置

#### 免密登录

- 配置公钥：
  执行`ssh-keygen`即可生成SSH钥匙，一路回车即可。
- 上传公钥到服务器：
  执行`ssh-copy-id -p port user@remote`，可以让远程服务器记住我们的公钥。
  如`ssh-copy-id cl@192.168.0.113`

#### 配置别名

每次都输入`ssh -p port user@remote`，非常不方便，而且还不好记忆。
而配置别名可以让我们进一步偷懒，譬如用：`ssh mac`来替代上面这么一长串，那么就在~/.ssh/config里面追加以下内容：

```powershell
Host xxx
    HostName ip地址
    User H
    Port 22
1234
```

如

```powershell
Host clmac
    HostName 192.168.0.113
    User cl
    Port 22
1234
```

保存之后，即可用`ssh clmac`实现远程登录了，scp同样可以使用