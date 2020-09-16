# Python全栈（六）项目前导之1.Redis介绍及数据类型介绍

## 一、数据库发展历史

### 1.背景

随着**互联网+大数据**时代的来临，传统的关系型数据库已经不能满足中大型网站日益增长的访问量和数据量。这个时候就需要一种能够**快速存取数据**的组件来缓解数据库服务的**I/O压力**，来解决系统性能上的瓶颈。

### 2.数据库的发展简史

数据库的发展大致分为5个阶段。

- 在互联网+大数据时代来临之前，企业的一些内部信息管理系统，一个单个数据库实例就能满足系统的需求。
  **单数据库实例**适用于用户访问量较少的情况，系统直接查询数据库，原理如下：
  ![单数据库实例](https://img-blog.csdnimg.cn/20200321153735718.png#pic_center)
- 随着系统访问用户的增多，数据量的增大，单个数据库实例已经满足不了系统的读取需求，此时需要**缓存（memcache）+单数据库实例**：
  用户访问量很大时，将常用的数据放到缓存，用户请求时直接访问缓存，当需要访问的数据不存在于缓存中时，再请求数据库，降低了与数据库直接交互的频率，降低了性能要求。
  原理示意如下：
  ![缓存+单数据库实例](https://img-blog.csdnimg.cn/20200321153848479.png#pic_center)
- 缓存可以缓解系统的读取压力，但是数据量的写入压力持续增大，此时需要用到**缓存+主从数据库+读写分离**。
  原理如下：
  ![缓存+主从数据库+读写分离](https://img-blog.csdnimg.cn/20200321154142939.png#pic_center)
- 数据量再次增大，读写分离以后，主数据库的写库压力出现瓶颈，此时要用到**缓存+主从数据库集群+读写分离+分库分表**。
  原理如下：
  ![缓存+主从数据库集群+读写分离+分库分表](https://img-blog.csdnimg.cn/20200321154309986.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
- 互联网+大数据时代来临，关系型数据库不能很好的存取一些并发性高，实时性高的，并且数据格式不固定的数据，此时需要用到**nosql+主从数据库集群+读写分离+分库分表**。
  原理如下：
  ![nosql+主从数据库集群+读写分离+分库分表](https://img-blog.csdnimg.cn/20200321155737867.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 二、Redis的介绍和安装

### 1.Redis概念

Redis是一个C语言开发的、**高性能**的、**开源**的、**键值对存储数据**的**nosql**数据库。

- NoSQL
  not only sql，泛指非关系型数据库，包括Redis、MongoDB、Hbase等。
- 关系型数据库
  MySQL、Oracle、SqlServer等。

### 2.Redis特性

- Redis支持**数据的持久化**，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用
- Redis不仅仅支持简单的**key-value**类型的数据，同时还提供List、set等数据类型
- Redis支持数据的**备份**

### 3.Redis的作用和使用

#### Redis的主要作用：

**快速存取**。

#### Redis应用场景

点赞、秒杀、直播平台的在线好友列表、商品排行榜和单点登录等场景。
这些应用在短时间内都有大量的数据交互，都要求数据库有极高的效率。

#### 使用

- 官网地址
  https://redis.io/
- 命令地址
  http://doc.redisfans.com/
  http://redis.cn/commands.html#thash

可以根据需要查看。

### 4.Redis五大数据类型

- string
- List
- set
- hash
- zset

### 5.Redis的安装和启动

主要包括Ubuntu、Kali和Windows的安装。

#### Ubuntu上安装Redis

```powershell
# 安装
sudo apt-get install redis-server

# 查看帮助命令
redis-server --help

# 编辑Redis配置文件
sudo vim /etc/redis/redis.conf
# 将daemonize no改为 daemonize yes保存退出

# 启动
redis-server

# 打开服务
sudo service redis start

# 关闭服务
sudo service redis stop
123456789101112131415161718
```

#### Kali中安装Redis

```powershell
# 下载
wget http://download.redis.io/releases/redis-5.0.8.tar.gz

# 解压
tar xzf redis-5.0.8.tar.gz

# 切换目录
cd redis-5.0.8

# 安装
make
1234567891011
```

Redis服务端开启：

```powershell
src/redis-server
1
```

显示：

```powershell
7088:C 21 Mar 2020 16:39:13.749 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
7088:C 21 Mar 2020 16:39:13.749 # Redis version=5.0.8, bits=64, commit=00000000, modified=0, pid=7088, just started
7088:C 21 Mar 2020 16:39:13.749 # Warning: no config file specified, using the default config. In order to specify a config file use src/redis-server /path/to/redis.conf
7088:M 21 Mar 2020 16:39:13.749 * Increased maximum number of open files to 10032 (it was originally set to 1024).
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 5.0.8 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 7088
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

7088:M 21 Mar 2020 16:39:13.750 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
7088:M 21 Mar 2020 16:39:13.750 # Server initialized
7088:M 21 Mar 2020 16:39:13.750 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
7088:M 21 Mar 2020 16:39:13.750 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
7088:M 21 Mar 2020 16:39:13.750 * DB loaded from disk: 0.000 seconds
7088:M 21 Mar 2020 16:39:13.750 * Ready to accept connections


123456789101112131415161718192021222324252627282930
```

服务端开启成功，出现堵塞，可以再开一个终端使用客户端连接：

```powershell
redis-cli
1
```

显示并测试：

```powershell
127.0.0.1:6379> set name corley
OK
127.0.0.1:6379> get name
"corley"
127.0.0.1:6379> 

123456
```

可以在配置文件中进行设置取消堵塞：
执行`vim redis.conf`命令打开配置文件，找到**daemoize no**处将**no**改为**yes**，保存退出，用命令`src/redis-server redis.conf`即可开启服务，不会堵塞，显示：

```powershell
7127:C 21 Mar 2020 16:48:39.111 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
7127:C 21 Mar 2020 16:48:39.111 # Redis version=5.0.8, bits=64, commit=00000000, modified=0, pid=7127, just started
7127:C 21 Mar 2020 16:48:39.111 # Configuration loaded
123
```

#### Windows中安装Redis

先点击https://download.csdn.net/download/CUFEECR/12260885下载后解压。
在Windows中不需要下载安装包安装，可以直接点击解压后的文件夹中的应用程序进行交互（双击redis-server.exe启动redis服务器，双击redis-cli.exe打开redis客户端），或者在命令行中操作：
命令行切换到redis路径下，命令行输入`redis-server`即可开启服务，再打开一个命令行输入`redis-cli`即连接到服务端，可以进行操作，同时还可将redis目录**加入环境变量**，更方便操作，操作如下：

- 复制redis路径
- 右键此电脑点击属性
- 点击高级系统设置，如下
  ![点击高级系统设置](https://img-blog.csdnimg.cn/20200321172615329.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
- 设置环境变量
  ![环境变量设置](https://img-blog.csdnimg.cn/20200321172443282.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
- 逐步点击确定并退出

此时再重新打开一个命令行，不需要切换路径，直接输入`redis-sever`即可打开redis服务。
但是通过上述方式打开redis服务是临时的，一旦redis-server退出服务即关闭，可以执行`redis-server --service-start`永久开启服务，此时可以在Windows系统服务中看到Redis正在运行，如下：
![redis正在运行](https://img-blog.csdnimg.cn/2020032117320354.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
还可以通过`redis-server --service-stop`关闭服务。

### 6.常见命令

- dbsize
  查看当前数据库的key数量
- keys *
  查看key的内容
- flushdb
  清空当前数据库的key的数量，相当于删库
- flushall
  清空所有库的key(慎用)
- exists key
  判断key是否存在

## 三、Redis的配置文件

Linux中配置文件路径为 **/etc/redis/redis.conf**，Windows中配置文件即为Redis目录下的**redis.windows-service.conf**和**redis.windows.conf**。配置文件中有一些关键的参数说明：

> 当redis作为守护进程运行的时候，它会写一个 pid 到 /var/run/redis.pid 文件里面。 daemonize no
>
> 设置数据库的数目，Redis已默认创建16个数据库，不需要再自己创建，编号0-15
> databases 16
>
> 根据给定的时间间隔和写入次数将数据保存到磁盘 下面的例子的意思是：
> 900 秒内如果至少有 1 个 key 的值变化，则保存
> 300秒内如果至少有 10 个 key 的值变化，则保存
> 60 秒内如果至少有 10000 个 key 的值变化，则保存
> save 900 1
> save 300 10
> save 60 10000
>
> 监听端口号，默认为 6379，如果你设为 0 ，redis 将不在 socket 上监听任何客户端连接。
> port 6379
>
> Redis默认只允许本地连接，不允许其他机器连接
> bind 127.0.0.1
> 主从数据库配置
> slaveof <masterip> <masterport>

daemonize的进一步说明：
daemonize是守护线程，默认为**no**，启动redis服务端后堵塞，修改为yes后再用命令`src/redis-server redis.conf`启动，不会发生堵塞
Kali中后台启动redis可以用命令`ps -aux | grep redis`，停止服务用`kill 8975`
更多配置文件说明可点击https://www.cnblogs.com/kreo/p/4423362.html查看。

## 四、Redis-String类型

string是redis最基本的类型，一个key对应一个value，示意如下：
![key-value](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcua2FuY2xvdWQuY24vOWIvOTAvOWI5MDA2M2ZjOTEwZjNlZGYzYzk3MGI3ZTRiYmZiZDNfNDAweDkwLmdpZg#pic_center#pic_center)
string可以包含任何数据，最大不能超过512M。

### 1.set/get/del/append/strlen

> set ---- 设置值
> get ---- 获取值
> mset ---- 设置多个值
> mget ---- 获取多个值
> append ---- 添加字段
> del ---- 删除
> strlen ---- 返回字符串长度

```powershell
127.0.0.1:6379> set name corley               
OK                                            
127.0.0.1:6379> get name                      
"corley"
# 重复设置会覆盖                                      
127.0.0.1:6379> mset name corley1 age 18 sex 1
OK                                            
127.0.0.1:6379> mget name age sex             
1) "corley1"                                  
2) "18"                                       
3) "1" 
# 返回的整数表示值的长度                                       
127.0.0.1:6379> append name 2                 
(integer) 8                                   
127.0.0.1:6379> get name                      
"corley12"                                    
127.0.0.1:6379> del sex                       
(integer) 1                                   
127.0.0.1:6379> strlen age                    
(integer) 2   
# 设置过期时间                                
127.0.0.1:6379> expire name 3                 
(integer) 1                                   
127.0.0.1:6379> get name                      
"corley12"                                    
127.0.0.1:6379> get name                      
(nil)                                         
127.0.0.1:6379> get name                      
(nil)        
# 如果有name则3秒后过期，则否先创建name再3秒后过期                                 
127.0.0.1:6379> setex name 3 corley           
OK                                            
127.0.0.1:6379> get name                      
(nil)                                         
127.0.0.1:6379>                               
1234567891011121314151617181920212223242526272829303132333435
```

### 2.incr/decr/incrby/decrby

> incr ---- 增加
> decr ---- 减少
> incrby ----- 指定增加多少
> decrby ----- 指定减少多少

```powershell
127.0.0.1:6379> set num 1
OK
127.0.0.1:6379> incr num
(integer) 2
127.0.0.1:6379> incr num
(integer) 3
127.0.0.1:6379> incr num
(integer) 4
127.0.0.1:6379> incr num
(integer) 5
127.0.0.1:6379> decr num
(integer) 4
127.0.0.1:6379> decr num
(integer) 3
127.0.0.1:6379> incrby num 3
(integer) 6
127.0.0.1:6379> decrby num 2
(integer) 4
127.0.0.1:6379>
12345678910111213141516171819
```

`decr`和`decrby`命令可以使数减少到负数。

### 3.getrange/setrange

> getrange ---- 获取指定区间范围内的值，类似between…and
> setrange ---- 从第几位开始替换，下脚本从零开始
> `0 -1`表示全部

```powershell
127.0.0.1:6379> set name Corley
OK
127.0.0.1:6379> getrange name 0 2
"Cor"
127.0.0.1:6379> setrange name 4 a
(integer) 6
127.0.0.1:6379> get name
"Corlay"
127.0.0.1:6379> setrange name 4 ang
(integer) 7
127.0.0.1:6379> get name
"Corlang"
127.0.0.1:6379> getrange name 0 -1
"Corlang"
127.0.0.1:6379>
123456789101112131415
```

## 五、Redis-List类型

List是**单值多value**类型。
List（列表）是简单的字符串列表，按照插入顺序排序，可以添加一个元素到列表的头部（左边）或者尾部（右边）。
List的底层实际是**链表**。

### 1.lpush/rpush/lrange

> lpush/rpush/lrange ---- 从左加入元素/从右加入元素/获取指定长度
> lpush list01 1 2 3 4 5 ---- 倒序排列
> rpush list02 1 2 3 4 5 ---- 正序排列
> lrange list01 0 -1 ---- 获取list01中的所有值

```powershell
127.0.0.1:6379> lpush l1 1 2 3 4 5
(integer) 5
127.0.0.1:6379> rpush l2 1 2 3 4 5
(integer) 5
127.0.0.1:6379> lrange l1 0 -1
1) "5"
2) "4"
3) "3"
4) "2"
5) "1"
127.0.0.1:6379> lrange l2 0 -1
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
127.0.0.1:6379>
1234567891011121314151617
```

### 2.lpop/rpop

> lpop ---- 移除最左的元素
> rpop ---- 移除最右的元素

```powershell
127.0.0.1:6379> lpop l1
"5"
127.0.0.1:6379> lrange l1 0 -1
1) "4"
2) "3"
3) "2"
4) "1"
127.0.0.1:6379> rpop l1
"1"
127.0.0.1:6379> lrange l1 0 -1
1) "4"
2) "3"
3) "2"
127.0.0.1:6379>
1234567891011121314
```

### 3.lindex

按照索引下标获得元素（从左到右）。

```powershell
127.0.0.1:6379> lrange l1 0 2
1) "4"
2) "3"
3) "2"
127.0.0.1:6379> lindex l1 1
"3"
127.0.0.1:6379>
1234567
```

### 4.llen

获取列表长度。

```powershell
127.0.0.1:6379> llen l1
(integer) 3
127.0.0.1:6379> llen l2
(integer) 5
127.0.0.1:6379>
12345
```

### 5.lrem key

删除多个相同值。

> lrem list01 2 1 在list01中删除2个1

```powershell
127.0.0.1:6379> lpush l3 1 3 2 2 3 4 5
(integer) 7                           
127.0.0.1:6379> lrange l3 0 -1        
1) "5"                                
2) "4"                                
3) "3"                                
4) "2"                                
5) "2"                                
6) "3"                                
7) "1"                                
127.0.0.1:6379> lrem l3 1 3           
(integer) 1                           
127.0.0.1:6379> lrange l3 0 -1        
1) "5"                                
2) "4"                                
3) "2"                                
4) "2"                                
5) "3"                                
6) "1"                                
127.0.0.1:6379> lrem l3 2 2           
(integer) 2                           
127.0.0.1:6379> lrange l3 0 -1        
1) "5"                                
2) "4"                                
3) "3"                                
4) "1"                                
127.0.0.1:6379>                       
123456789101112131415161718192021222324252627
```

### 6.ltrim key

截取指定范围的值后再赋值给key。

> ltrim list01 0 2 截取list01从0到2的数据在赋值给list01

```powershell
127.0.0.1:6379> lpush l4 1 2 3 4
(integer) 4                     
127.0.0.1:6379> ltrim l4 0 2    
OK                              
127.0.0.1:6379> lrange l4 0 -1  
1) "4"                          
2) "3"                          
3) "2"                          
127.0.0.1:6379>                 
123456789
```

### 7.rpoplpush

> rpoplpush list1 list2 将list1中最后一个压入list2中第一位

```powershell
127.0.0.1:6379> lrange l1 0 -1 
1) "4"                         
2) "3"                         
3) "2"                         
127.0.0.1:6379> lrange l2 0 -1 
1) "1"                         
2) "2"                         
3) "3"                         
4) "4"                         
5) "5"                         
127.0.0.1:6379> rpoplpush l1 l2
"2"                            
127.0.0.1:6379> lrange l1 0 -1 
1) "4"                         
2) "3"                         
127.0.0.1:6379> lrange l2 0 -1 
1) "2"                         
2) "1"                         
3) "2"                         
4) "3"                         
5) "4"                         
6) "5"                         
127.0.0.1:6379>                
1234567891011121314151617181920212223
```

### 8.lset key index value

> lset list01 0 x 将list01中第1位换成x

```powershell
127.0.0.1:6379> lset l1 1 5
OK
127.0.0.1:6379> lrange l1 0 -1
1) "4"
2) "5"
127.0.0.1:6379>
123456
```

### 9.linsert key before/after

> linsert list01 before x y 在x之前加字段y

```powershell
127.0.0.1:6379> lrange l4 0 -1       
1) "4"                               
2) "3"                               
3) "2"                               
127.0.0.1:6379> linsert l4 before 6 7
(integer) -1                         
127.0.0.1:6379> linsert l4 before 3 7
(integer) 4                          
127.0.0.1:6379> linsert l4 after 2 5 
(integer) 5                          
127.0.0.1:6379> lrange l4 0 -1       
1) "4"                               
2) "7"                               
3) "3"                               
4) "2"                               
5) "5"                               
127.0.0.1:6379>        
```

# Python全栈（六）项目前导之2.Redis数据类型和Python操作Redis

## 一、Redis-Hash类型

hash是一个键值对集合。
hash是一个string类型的field和value的**映射表**，hash特别适合**存储对象**。

### 1.hset/hget/hmset/hmget/hgetall/hdel

意义分别是：
**设值/取值/设值多个值/取多个值/取全部值/删除值**。
测试如下：

```powershell
127.0.0.1:6379> keys *
 1) "l3"
 2) "name"
 3) "age"
 4) "data"
 5) "l4"
 6) "proxies"
 7) "first"
 8) "l1"
 9) "l2"
10) "num"
127.0.0.1:6379> hset user id 1
(integer) 1
127.0.0.1:6379> hget user id
"1"
127.0.0.1:6379> hset user name corley
(integer) 1
127.0.0.1:6379> hget user name
"corley"
127.0.0.1:6379> hset user id 2
(integer) 0
127.0.0.1:6379> hget user id
"2"
127.0.0.1:6379> hgetall user
1) "id"
2) "2"
3) "name"
4) "corley"
127.0.0.1:6379> hmset users id 1 name corley sex 1
OK
127.0.0.1:6379> hmget users id name sex
1) "1"
2) "corley"
3) "1"
127.0.0.1:6379> hgetall users
1) "id"
2) "1"
3) "name"
4) "corley"
5) "sex"
6) "1"
127.0.0.1:6379> hdel user id
(integer) 1
127.0.0.1:6379> hgetall user
1) "name"
2) "corley"
127.0.0.1:6379> hdel user name
(integer) 1
127.0.0.1:6379> hgetall user
(empty list or set)
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
```

`get *`会发现之前的值都还在，这是Redis使用持久化技术将数据保存到了本地磁盘，查看Redis目录，可以看到有一个文件**dump.rdb**，就是用于保存数据的。
键可以重复，重复时会覆盖，但是在使用时应该尽量避免使用重复的键。

### 2.hlen

获取哈希长度。
测试如下：

```powershell
127.0.0.1:6379> type users
hash  
127.0.0.1:6379> hlen users
(integer) 3                                   
1234
```

### 3.hexists key

判断key里面的某个值是否存在，存在返回1 ，不存在返回0。
测试如下：

```powershell
127.0.0.1:6379> hexists users id
(integer) 1
127.0.0.1:6379> hexists users height
(integer) 0
1234
```

### 4.hkeys/hvals

列举出所有的key和value。
测试如下：

```powershell
127.0.0.1:6379> hkeys users          
1) "id"                              
2) "name"                            
3) "sex"                             
127.0.0.1:6379> hvals users          
1) "1"                               
2) "corley"                          
3) "1"                               
12345678
```

## 二、Redis-Set类型

set是string类型的无序集合。
和普通的集合一样，Set中不存在不重复的值。

### 1.sadd/smembers/sismember

表示**添加/查看集合/查看是否存在**。
举例：

> sadd set01 1 2 2 3 3 去掉重复添加
> smembers set01 得到set01的所有元素
> sismember set01 1 如果存在返回1，不存在返回0

测试如下：

```powershell
127.0.0.1:6379> sadd se 1 2 2 3 3 4
(integer) 4
127.0.0.1:6379> smembers se
1) "1"
2) "2"
3) "3"
4) "4"
127.0.0.1:6379> sismember se 2
(integer) 1
127.0.0.1:6379> sismember se 5
(integer) 0
1234567891011
```

### 2.scard

获取集合里面的元素个数。
测试如下：

```powershell
127.0.0.1:6379> scard se
(integer) 4
12
```

### 3.srem key value

删除集合中元素。
测试如下：

```powershell
127.0.0.1:6379> srem se 3
(integer) 1
127.0.0.1:6379> smembers se
1) "1"
2) "2"
3) "4"
127.0.0.1:6379> srem se 5
(integer) 0
12345678
```

### 4.srandmember key

随机返回数据。
测试如下：

```powershell
127.0.0.1:6379> srandmember se
"1"                           
127.0.0.1:6379> srandmember se
"2"                           
127.0.0.1:6379> srandmember se
"4"                           
127.0.0.1:6379> srandmember se
"2"                           
127.0.0.1:6379> srandmember se
"2"                           
127.0.0.1:6379> srandmember se
"1"                           
127.0.0.1:6379> smembers se   
1) "1"                        
2) "2"                        
3) "4"                        
12345678910111213141516
```

### 5.spop key

随机弹出元素。
测试如下：

```powershell
127.0.0.1:6379> spop se
"2"
127.0.0.1:6379> spop se
"4"
127.0.0.1:6379> spop se
"1"
127.0.0.1:6379> sadd se1 1 7 5 3 2 4 7
(integer) 6
127.0.0.1:6379> spop se1
"7"
127.0.0.1:6379> spop se1
"3"
127.0.0.1:6379> spop se1
"2"
127.0.0.1:6379> smembers se1
1) "1"
2) "4"
3) "5"
123456789101112131415161718
```

### 6.smove key1 key2

移动元素。
举例说明：

> smove set01 set03 2 将set01中的2 移动到set03中

测试如下：

```powershell
127.0.0.1:6379> smove se1 se 1
(integer) 1
127.0.0.1:6379> smembers se
1) "1"
1234
```

### 7.集合操作

与数学中的集合操作类似，其中：

- SDIFF
  差集操作
- SINTER
  交集操作
- SUNION
  并集操作

测试如下：

```powershell
127.0.0.1:6379> sadd set01 1 2 3 4 5
(integer) 5                         
127.0.0.1:6379> sadd set02 1 2 3 a b
(integer) 5                         
127.0.0.1:6379> SDIFF set01 set02   
1) "4"                              
2) "5"                              
127.0.0.1:6379> SINTER set01 set02  
1) "1"                              
2) "2"                              
3) "3"                              
127.0.0.1:6379> SUNION set01 set02  
1) "4"                              
2) "a"                              
3) "1"                              
4) "2"                              
5) "3"                              
6) "5"                              
7) "b"                              
12345678910111213141516171819
```

## 三、Redis-Zset类型

ZSet是Redis中的有序集合类型。

### 1.zadd/zrange

设置和根据范围获取值。
测试如下：

```powershell
127.0.0.1:6379> zadd ze 60 v1 70 v2 80 v3 90 v4 100 v5
(integer) 5
127.0.0.1:6379> zrange ze 0 -1
1) "v1"
2) "v2"
3) "v3"
4) "v4"
5) "v5"
127.0.0.1:6379> zadd ze1 60 v1 70 v2 80 v3 100 v4 90 v5
(integer) 5
127.0.0.1:6379> zrange ze1 0 -1
1) "v1"
2) "v2"
3) "v3"
4) "v5"
5) "v4"
127.0.0.1:6379> zadd ze2 60 v1 70 v2 80 v3 90 v4 90 v5
(integer) 5
127.0.0.1:6379> zrange ze2 0 -1
1) "v1"
2) "v2"
3) "v3"
4) "v4"
5) "v5"
127.0.0.1:6379> zadd ze3 name v1 age v2 80 v3 90 v4 90 v5
(error) ERR value is not a valid float
127.0.0.1:6379> zadd ze4 60 v1 70 v2 80 v1 90 v4 90 v5
(integer) 4
127.0.0.1:6379> zrange ze4 0 -1 withscores
1) "v2"
2) "70"
3) "v1"
4) "80"
5) "v4"
6) "90"
7) "v5"
8) "90"
12345678910111213141516171819202122232425262728293031323334353637
```

显然，zset的值只能是数值型（浮点数），如果是字符串会报错；
如果键相同，后边对应的值会覆盖前面对应的值。

### 2.zrangebyscore key start end

根据开始结束范围来取值；
结束不包括用 **(**；
limit限制从指定索引开始获取指定条数据。
测试如下：

```powershell
127.0.0.1:6379> zrangebyscore ze 60 80
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> zrangebyscore ze 60 (80
1) "v1"
2) "v2"
127.0.0.1:6379> zrangebyscore ze 60 80 limit 1 2
1) "v2"
2) "v3"
127.0.0.1:6379> zrangebyscore ze 60 80 limit 1 2
1) "v2"
2) "v3"
127.0.0.1:6379> zrangebyscore ze 60 80 limit 0 2
1) "v1"
2) "v2"
12345678910111213141516
```

### 3.zrem key

删除元素。
测试如下：

```powershell
127.0.0.1:6379> zrem ze v1
(integer) 1
127.0.0.1:6379> zrange ze 0 -1
1) "v2"
2) "v3"
3) "v4"
4) "v5"
1234567
```

### 4.zcard/zcount/zrank

获取**数据总数/指定范围的数据个数/指定数据对应下标**。
测试如下：

```powershell
127.0.0.1:6379> zcard ze
(integer) 4
127.0.0.1:6379> zcount ze 70 90
(integer) 3
127.0.0.1:6379> zrank ze v4
(integer) 2
123456
```

## 四、Python操作Redis

### 1.Python中redis的安装和连接

如果未安装redis库，需要先通过命令安装：

```powershell
pip install redis
1
```

连接方式如下 ：

```python
r = redis.StrictRedis(host='localhost',port=6379,db=0)
1
```

### 2.Python操作Redis-String

简单测试：

```python
import redis

r = redis.StrictRedis(host='localhost', port=6379, db=0)  # 与redis.Redis()效果相同

print(r)
12345
```

打印

```python
Redis<ConnectionPool<Connection<host=localhost,port=6379,db=0>>>
1
```

显然，r是一个redis对象。

封装类操作：

```python
import redis


class StringRedis(object):
    def __init__(self):
        self.r = redis.StrictRedis()  # 因连接参数为默认，所以可以不带参数

    def string_set(self, key, item):
        '''字符串设置值'''
        res = self.r.set(key, item)  # 返回布尔值
        print(res)

    def string_get(self, key):
        '''字符串取值'''
        res = self.r.get(key) # 返回字节型数据
        return res


if __name__ == '__main__':
    s = StringRedis()
    s.string_set('user', 'corley')
    res = s.string_get('user')
    print(type(res), res)
1234567891011121314151617181920212223
```

打印

```python
True
<class 'bytes'> b'corley'
12
```

进一步完善操作：

```python
import redis


class StringRedis(object):
    def __init__(self):
        self.r = redis.StrictRedis()  # 因连接参数为默认，所以可以不带参数

    def string_set(self, key, item):
        '''字符串设置值'''
        res = self.r.set(key, item)  # 返回布尔值
        print(res)

    def string_get(self, key):
        '''字符串取值'''
        res = self.r.get(key) # 返回字节型数据
        return res

    def string_mset(self, items):
        '''字符串设置多个值'''
        if isinstance(items, dict):
            res = self.r.mset(items) # 返回布尔值
            print(res)

    def string_mget(self, keys):
        '''字符串取多个值'''
        if isinstance(keys, list):
            return self.r.mget(keys)

    def string_del(self, key):
        '''删除值'''
        if self.r.exists(key):
            self.r.delete(key)
        else:
            return '%s Not Found' % key

if __name__ == '__main__':
    s = StringRedis()
    s.string_set('user', 'corley')
    res1 = s.string_get('user')
    print(type(res1), res1)
    d = {
        'age':18,
        'sex':1
    }
    s.string_mset(d)
    res2 = s.string_mget(['user', 'age', 'sex'])
    print(type(res2), res2)
    res3 = s.string_del('user')
    res4 = s.string_del('name')
    print(res3, res4)
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
```

打印

```python
True
<class 'bytes'> b'corley'
True
<class 'list'> [b'corley', b'18', b'1']
None name Not Found
12345
```

### 3.Python操作Redis-List

```python
import redis


class ListRedis(object):
    def __init__(self):
        self.r = redis.StrictRedis()

    def list_lpush(self, key, item):
        '''列表设置值'''
        res = self.r.lpush(key, item)  # 返回布尔值
        print(res)

    def list_lpop(self, list):
        '''列表弹出值'''
        res = self.r.lpop(list) # 返回字节型数据
        return res


if __name__ == '__main__':
    l = ListRedis()
    l.list_lpush('li', 1)
    l.list_lpop('li')
12345678910111213141516171819202122
```

打印

```python
1

12
```

Python中List添加值一次只能添加一个，如有多个，可以使用循环。
再次测试：

```python
import redis


class ListRedis(object):
    def __init__(self):
        self.r = redis.StrictRedis()

    def test_push(self):
        res = self.r.lpush('test','1')
        res = self.r.rpush('test','2')
        res = self.r.rpush('test','3')

    def test_pop(self):
        res = self.r.lpop('test')
        print(res)
        res = self.r.rpop('test')
        print(res)

    def test_range(self):
        res = self.r.lrange('test',0,-1)
        print(res)


if __name__ == '__main__':
    l = ListRedis()
    l.test_push()
    l.test_range()
    l.test_pop()
12345678910111213141516171819202122232425262728
```

打印

```python
[b'1', b'2', b'3']
b'1'
b'3'
123
```

### 4.Python操作Redis-Set

简单操作示例如下：

```python
import redis


class SetRedis(object):
    def __init__(self):
        self.r = redis.StrictRedis()

    def test_sadd(self):
        res = self.r.sadd('set1','1','2')

    def test_del(self):
        res = self.r.srem('set1',1)

    def test_pop(self):
        res = self.r.spop('set1')

    def test_members(self, key):
        res = self.r.smembers(key)
        print(res)


if __name__ == '__main__':
    s = SetRedis()
    s.test_sadd()
    s.test_members('set1')
    s.test_del()
    s.test_members('set1')
    s.test_pop()
    s.test_members('set1')
1234567891011121314151617181920212223242526272829
```

打印

```python
{b'2', b'1'}
{b'2'}
set()
123
```

### 5.Python操作Redis-Hash

用Python操作Redis-Hash示例如下：

```python
import redis


class HashRedis(object):
    def __init__(self):
        self.r = redis.StrictRedis()

    def test_hset(self):
        dic = {
            'id': 1,
            'name': 'huawei'
        }
        res = self.r.hmset('mobile', dic)

    def test_hgetall(self):
        res = self.r.hgetall('mobile')
        print(res)

    def test_hexists(self):
        res = self.r.hexists('mobile', 'id')
        print(res)


if __name__ == '__main__':
    h = HashRedis()
    h.test_hset()
    h.test_hgetall()
    h.test_hexists()
12345678910111213141516171819202122232425262728
```

打印

```python
{b'id': b'1', b'name': b'huawei'}
True

123
```

## 五、Redis主从配置

### 1.主从概念

在实际过程中，缓存可能因为内存占用更多或其他不可预料的因素而崩溃，这样会使得用户直接访问数据库，从而导致主数据据库的访问压力大大增加，所以为了使缓存不易发生数据库崩溃，应该对缓存建立主从配置，从而提高系统的容灾性能。
![缓存的主从配置](https://img-blog.csdnimg.cn/20200326125000476.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
Redis主从配置特性如下：

- ⼀个master可以拥有多个slave，⼀个slave⼜可以拥有多个slave，如此下去，形成了强⼤的多级服务器集群架构
- master用来写数据，slave用来读数据
  读数据比写数据多得多，据统计，网站的读写比率是10:1
- 通过主从配置可以实现读写分离，扩展性能
- master和slave都是一个redis实例(redis服务)

示意如下：
![master-slave](https://img-blog.csdnimg.cn/20200326125313741.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 2.主从配置过程

以下相关操作均是在Linux（Kali）进行的。
可以在Redis配置文件中设置守护线程参数**daemonize为yes**，以便后面更方便地开启Redis服务，如下：
![daemonize yes](https://img-blog.csdnimg.cn/20200326142610842.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

#### 配置主机和开启服务

- 修改redis.conf配置文件

> bind 0.0.0.0 # 或者改成本机IP

- 开启主机服务

```powershell
src/redis-server redis.conf
1
```

#### 配置从机和开启服务

- 复制redis.conf配置文件

```powershell
cp redis.conf slave.conf
1
```

- 修改slave.conf文件
  有3处修改的地方：

> bind 192.168.186.132(主机IP，通过ifconfig名获取)
> …
> port 6378(从机端口)
> …
> replicaof 192.168.186.132(主机IP) 6379(主机端口) # 旧版本Redis中为slaveof

在配置文件中位置示意如下：
![bind](https://img-blog.csdnimg.cn/20200326141331396.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
![port](https://img-blog.csdnimg.cn/20200326140758792.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
![replicaof](https://img-blog.csdnimg.cn/20200326145736846.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

- 开启从机服务

```powershell
src/redis-server slave.conf
1
```

#### 主从数据操作测试

- 连接主从客户端

```powershell
src/redis-cli -p 6379
src/redis-cli -h 192.168.186.132 -p 6378
12
```

- 读写数据
  在主机上写数据：

```powershell
set name corley
1
```

在从机上读数据：

```powershell
get name
1
```

具体演示过程如下：
![redis master-slave config test](https://img-blog.csdnimg.cn/20200326145423294.gif#pic_center)
可以看出，**主机才有写权限，从机只能读**。

# Python全栈（六）项目前导之3.初试Git

## 一、什么是Git

### 1.Git定义

Git是一个 **分布式** 的 **版本控制** **软件**。

- 软件
  类似于QQ、office等安装到电脑或其他电子设备上才能使用的工具。
- 版本控制
  类似于毕业论文、写文案、视频剪辑等，需要**反复修改和保留原历史数据**。
- 分布式

### 2.分布式版本控制的发展历程

#### 文件（夹）拷贝

最开始时简单复制，再在复制的基础上进行修改，保存了所有的版本，不仅会占大量内存，而且不易于管理和与他人共享同步，如下：
![file-copy](https://img-blog.csdnimg.cn/20200327170312384.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

#### 本地版本控制

后来发展到只有一个文件，在软件中保存了该文件的多个版本，但是还是不利于不同的开发者之间的共享同步，如下：
![本地版本控制](https://img-blog.csdnimg.cn/20200327170411110.png#pic_center)

#### 集中式版本控制

再发展，到集中式的版本控制，如SVN，有一个控制中心保存了所有版本，但是由于本地无版本，如中心出现问题，本地无完整版本，整个项目就可能丢失或不能正常进行，如下：
![集中式版本控制](https://img-blog.csdnimg.cn/20200327170543758.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

#### 分布式版本控制

再到现在的分布式版本控制，如Git，版本会先提交到本地，再同步到中心，从而提升了容错性，如下：

![分布式版本控制](https://img-blog.csdnimg.cn/20200327170704860.png#pic_center?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

## 二、版本控制原因和安装Git

### 1.版本控制的原因

要保留之前所有的版本，以便回滚和修改。
具体原因如下：

- 具体写程序的过程是一个反复编辑、不厌其烦地编辑的过程，修改的时候不希望破坏掉修改前的状态，最好就是每修改一段事件，能够保存一个状态，类似系统的自动快照一样，当后面出现问题的时候，可以自由选择复原到之前的某个快照状态；
- 最后发布软件的时候很多时候会有多个版本，而生成软件的源代码却往往只有一份，只是在最后编译生成的时候用到不同的部分，大部分代码还是共用的，所以往往需要版本控制，几个版本复制几个文件夹出来；
- 有了版本控制，可以浏览所有开发的历史纪录，掌握团队的开发进度，而且作任何修改都不再害怕，因为可以轻易的复原回之前正常的版本，也可以透过分支和标签的功能来进行软件发行的不同版本。

**开发永远是个过程，而不是结果。** 版本控制的过程也是过程追踪记录、成就达成的过程。

### 2.安装Git

需要先下载安装文件，可以点击https://git-scm.com/download选择合适的系统和版本进行下载，如果下载速度太慢可以通过镜像下载。
也可直接点击https://download.csdn.net/download/CUFEECR/12275488进行下载。
下载之后点击安装，选择安装目录，并完成安装。
安装完成之后，鼠标右键看到**Git GUI Here**和**Git Bash Here**则说明安装成功。
整个安装过程是将Git安装到了**本地**。
可右键选择**Git Bash Here**，会弹出一个命令窗口，可以输入`git --vsesion`查看git版本，输入`git --help`查看帮助文档，示例如下：

```bash
Lenovo@LAPTOP-61GNF3CH MINGW64 ~/Desktop
$ git --version
git version 2.25.0.windows.1

Lenovo@LAPTOP-61GNF3CH MINGW64 ~/Desktop
$ git --help
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone             Clone a repository into a new directory
   init              Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add               Add file contents to the index
   mv                Move or rename a file, a directory, or a symlink
   restore           Restore working tree files
   rm                Remove files from the working tree and from the index
   sparse-checkout   Initialize and modify the sparse-checkout

examine the history and state (see also: git help revisions)
   bisect            Use binary search to find the commit that introduced a bug
   diff              Show changes between commits, commit and working tree, etc
   grep              Print lines matching a pattern
   log               Show commit logs
   show              Show various types of objects
   status            Show the working tree status

grow, mark and tweak your common history
   branch            List, create, or delete branches
   commit            Record changes to the repository
   merge             Join two or more development histories together
   rebase            Reapply commits on top of another base tip
   reset             Reset current HEAD to the specified state
   switch            Switch branches
   tag               Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch             Download objects and refs from another repository
   pull              Fetch from and integrate with another repository or a local branch
   push              Update remote refs along with associated objects

'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
See 'git help git' for an overview of the system.

12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152
```

## 三、Git管理文件

- 进入要管理的文件夹
- 右键选择**Git Bash Here**
- 初始化

```bash
git init
1
```

- 查看目录下的文件状态

```bash
git status
1
```

- 管理文件

  - 管理指定文件

  ```bash
  git add xxx
  1
  ```

  - 管理所有文件

  ```bash
  git add .
  1
  ```

- 个人信息（用户名、邮箱）配置
  首次使用Git时操作。sho

```bash
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
12
```

- 生成版本

```bash
git commit -m '描述信息'
1
```

- 查看版本

```bash
git log
1
```

测试如下：

```bash
Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest
$ git init
Initialized empty Git repository in E:/projecttest/.git/

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)

# 创建新文件index.html后
Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        index.html # 红色

nothing added to commit but untracked files present (use "git add" to track)

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git add index.html

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   index.html # 绿色


Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git commit -m 'v1'

# 首次使用会提示配置个人信息
*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: unable to auto-detect email address (got 'Lenovo@LAPTOP-61GNF3CH.(none)')

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git config --global user.email "xxx@gmail.com"

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git config --global user.name "Corley"

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git commit -m 'v1'
[master (root-commit) 1e8ddbf] v1
 1 file changed, 15 insertions(+)
 create mode 100644 index.html

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git log
commit 1e8ddbf90b34a2c11374813cd6d154d73283428e (HEAD -> master)
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:51:36 2020 +0800

    v1


# 创建新文件
Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ touch readme.txt

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        readme.txt

nothing added to commit but untracked files present (use "git add" to track)

# 修改index.html
Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   index.html

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git add index.html

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   index.html

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        readme.txt


Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git commit -m 'v1.1'
[master 5f6b74b] v1.1
 1 file changed, 1 insertion(+)

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        readme.txt

nothing added to commit but untracked files present (use "git add" to track)

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git log
commit 5f6b74bb67e375a4f9ebc2658893b5eaab2e3bb1 (HEAD -> master)
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:58:08 2020 +0800

    v1.1

commit 1e8ddbf90b34a2c11374813cd6d154d73283428e
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:51:36 2020 +0800

    v1

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git add .

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git submit -m 'v1.15'
git: 'submit' is not a git command. See 'git --help'.

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git commit -m 'v1.15'
[master 2ec3695] v1.15
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 readme.txt

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git log
commit 2ec36954c41be1836644d2f18d185ca8e1f67c78 (HEAD -> master)
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:01:43 2020 +0800

    v1.15

commit 5f6b74bb67e375a4f9ebc2658893b5eaab2e3bb1
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:58:08 2020 +0800

    v1.1

commit 1e8ddbf90b34a2c11374813cd6d154d73283428e
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:51:36 2020 +0800

    v1

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git status
On branch master
nothing to commit, working tree clean
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184
```

## 四、Git的三大区域

Git的三大区域包括：

- 工作区
  - 已控制文件
  - 新增和变动文件（红色标识）
- 暂存区（绿色标识）
- 版本库

图示如下：
![Git的三大区域](https://img-blog.csdnimg.cn/20200327173358460.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
代码改进，添加新功能，测试：

```bash
Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   index.html

no changes added to commit (use "git add" and/or "git commit -a")

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git add .

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   index.html


Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git commit -m 'v2'
[master 742d05c] v2
 1 file changed, 6 insertions(+)

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git status
On branch master
nothing to commit, working tree clean

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git log
commit 742d05cf56bd15331d6c762a2f6f86b56c14a45b (HEAD -> master)
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:06:18 2020 +0800

    v2

commit 2ec36954c41be1836644d2f18d185ca8e1f67c78
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:01:43 2020 +0800

    v1.15

commit 5f6b74bb67e375a4f9ebc2658893b5eaab2e3bb1
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:58:08 2020 +0800

    v1.1

commit 1e8ddbf90b34a2c11374813cd6d154d73283428e
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:51:36 2020 +0800

    v1

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   index.html

no changes added to commit (use "git add" and/or "git commit -a")

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git add .

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   index.html


Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git git commit -m '-v3'
git: 'git' is not a git command. See 'git --help'.

The most similar command is
        init

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git commit -m '-v3'
[master f12719f] -v3
 1 file changed, 6 insertions(+)

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git log
commit f12719f22469243dd4e95bc3de7da2f524627938 (HEAD -> master)
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:17:09 2020 +0800

    -v3

commit 742d05cf56bd15331d6c762a2f6f86b56c14a45b
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:06:18 2020 +0800

    v2

commit 2ec36954c41be1836644d2f18d185ca8e1f67c78
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:01:43 2020 +0800

    v1.15

commit 5f6b74bb67e375a4f9ebc2658893b5eaab2e3bb1
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:58:08 2020 +0800

    v1.1

commit 1e8ddbf90b34a2c11374813cd6d154d73283428e
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:51:36 2020 +0800

    v1
...skipping...
commit 742d05cf56bd15331d6c762a2f6f86b56c14a45b
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:06:18 2020 +0800
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125
```

## 五、版本回滚和文件恢复

### 1.回滚

- 回滚到之前的版本

```bash
git log 
git reset --hard 版本号
12
```

- 回滚到之后的版本

```bash
git reflog
git reset --hard 版本号
12
```

其中，`log`命令查看回滚之前的版本，`reflog`查看回滚之后的版本。
往前回滚测试：

```bash
Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git reset --hard 742d05cf56bd15331d6c762a2f6f86b56c14a45b
HEAD is now at 742d05c v2

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git log
commit 742d05cf56bd15331d6c762a2f6f86b56c14a45b (HEAD -> master)
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:06:18 2020 +0800

    v2

commit 2ec36954c41be1836644d2f18d185ca8e1f67c78
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:01:43 2020 +0800

    v1.15

commit 5f6b74bb67e375a4f9ebc2658893b5eaab2e3bb1
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:58:08 2020 +0800

    v1.1

commit 1e8ddbf90b34a2c11374813cd6d154d73283428e
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:51:36 2020 +0800

    v1


12345678910111213141516171819202122232425262728293031
```

此时会发现代码回到V2时的版本。
往后回滚测试：

```bash
Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git reflog
742d05c (HEAD -> master) HEAD@{0}: reset: moving to 742d05cf56bd15331d6c762a2f6f86b56c14a45b
f12719f HEAD@{1}: commit: -v3
742d05c (HEAD -> master) HEAD@{2}: commit: v2
2ec3695 HEAD@{3}: commit: v1.15
5f6b74b HEAD@{4}: commit: v1.1
1e8ddbf HEAD@{5}: commit (initial): v1

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git reset --hard f12719f
HEAD is now at f12719f -v3
123456789101112
```

### 2.文件恢复

- 对修改文件的恢复

```bash
git checkout -- xxx
1
```

- 使文件从暂存区回到工作区
  即使文件名称颜色由绿变红

```bash
git reset HEAD xxx
1
```

测试示例如下：
![git checkout resetHEAD test](https://img-blog.csdnimg.cn/20200327182622922.gif#pic_center)

# Python全栈（六）项目前导之4.Git分支和GitHub的使用

## 一、初识分支

### 1.分支定义

在开发中，master表示主线。
在开发新功能时，会创建一个分支，等到开发完成后，会合并产生一个新版本。
分支可以给使用者提供多个环境，意味着你可以把你的工作从开发主线上分离开来，以免影响开发主线。
主线上永远都是正式版本，分支上测试没有问题之后，会将其添加到主线，这样才不会影响主线上的正式版本的发行使用。
主线和分支间、各分支之间做了代码隔离。

### 2.git分支常见命令

- 查看当前所在分支

```bash
git branch
1
```

- 创建分支

```bash
git branch name
1
```

- 切换分支

```bash
git checkout name
1
```

- 合并分支

```bash
git merge name
1
```

- 删除分支

```bash
git branch -d name
1
```

## 二、基于分支修复线上bug

### 1.紧急修复线上bug的思路

大致思路是另建一个分支，专门用于解决bug，等到bug解决后再与主线合并，图示如下：
![紧急修复bug](https://img-blog.csdnimg.cn/20200329084611635.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 2.修复分支bug实现

- 查看目前处在的分支

```bash
git branch
1
```

- 创建分支

```bash
git branch 分支名字
1
```

- 切换分支

```bash
git checkout 分支名称
1
```

- 分支合并（可能产生冲突）

```bash
git merge 要合并的分支
1
```

- 删除分支

```bash
git branch -d 分支名称
1
```

示例如下：

```bash
Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git branch
* master

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git branch dev

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git branch
  dev
* master

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git checkout dev
Switched to branch 'dev'

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (dev)
$ git branch
* dev
  master

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (dev)
$ vim index.html

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (dev)
$ git add .

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (dev)
$ git commit -m 'shop'
[dev e780977] shop
 1 file changed, 4 insertions(+), 2 deletions(-)

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (dev)
$ git log
commit e780977495aa5bc83c6aef32913743345b290c05 (HEAD -> dev)
Author: Corley <cutercorleytd@gmail.com>
Date:   Sat Mar 28 16:50:08 2020 +0800

    shop

commit f12719f22469243dd4e95bc3de7da2f524627938 (master)
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:17:09 2020 +0800

    -v3

commit 742d05cf56bd15331d6c762a2f6f86b56c14a45b
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:06:18 2020 +0800

    v2

commit 2ec36954c41be1836644d2f18d185ca8e1f67c78
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:01:43 2020 +0800

    v1.15

commit 5f6b74bb67e375a4f9ebc2658893b5eaab2e3bb1
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:58:08 2020 +0800

    v1.1

commit 1e8ddbf90b34a2c11374813cd6d154d73283428e
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:51:36 2020 +0800

    v1

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (dev)
$ cat index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Project</title>
</head>
<body>
    <ul>
        <li>English</li>
        <li>French</li>
        <li>Spain</li>
                <li>Russia</li>
    </ul>


        <ul>
        <li>Math</li>
        <li>Physics</li>
    </ul>


        <ul>
        <li>History</li>
        <li>Science</li>
    </ul>
        <ul>
                <li>Art</li>
        </ul>
</body>
</html>

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (dev)
$ git checkout master
Switched to branch 'master'

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ cat index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Project</title>
</head>
<body>
    <ul>
        <li>English</li>
        <li>French</li>
        <li>Spain</li>
                <li>Russia</li>
    </ul>


        <ul>
        <li>Math</li>
        <li>Physics</li>
    </ul>


        <ul>
        <li>History</li>
        <li>Science</li>
    </ul>

</body>
</html>
Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git branch bug

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git branch
  bug
  dev
* master

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git checkout bug
Switched to branch 'bug'

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (bug)
$ vim index.html

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (bug)
$ git add .

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (bug)
$ git commit -m 'nobug'
[bug 2694c72] nobug
 1 file changed, 2 insertions(+), 1 deletion(-)

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (bug)
$ git log
commit 2694c72bcf04da286b6941fb820ade8f856a694e (HEAD -> bug)
Author: Corley <cutercorleytd@gmail.com>
Date:   Sat Mar 28 16:56:43 2020 +0800

    nobug

commit f12719f22469243dd4e95bc3de7da2f524627938 (master)
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:17:09 2020 +0800

    -v3

commit 742d05cf56bd15331d6c762a2f6f86b56c14a45b
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:06:18 2020 +0800

    v2

commit 2ec36954c41be1836644d2f18d185ca8e1f67c78
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:01:43 2020 +0800

    v1.15

commit 5f6b74bb67e375a4f9ebc2658893b5eaab2e3bb1
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:58:08 2020 +0800

    v1.1

commit 1e8ddbf90b34a2c11374813cd6d154d73283428e
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:51:36 2020 +0800

    v1

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (bug)
$ git checkout master
Switched to branch 'master'

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git merge bug
Updating f12719f..2694c72
Fast-forward
 index.html | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ cat index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Project</title>
</head>
<body>
    <ul>
        <li>English</li>
        <li>French</li>
        <li>Spain</li>
                <li>Russia</li>
    </ul>


        <ul>
        <li>Math</li>
        <li>Physics</li>
    </ul>


        <ul>
        <li>History</li>
        <li>Science</li>
        <li>Bug Solved</li>
    </ul>

</body>
</html>

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git branch -d bug
Deleted branch bug (was 2694c72).

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git branch
  dev
* master

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git checkout dev
Switched to branch 'dev'

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (dev)
$ vim index.html

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (dev)
$ git add .

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (dev)
$ git commit -m 'shopok'
[dev 9668e01] shopok
 1 file changed, 1 insertion(+)

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (dev)
$ git checkout master
Switched to branch 'master'

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git merge dev
Auto-merging index.html
Merge made by the 'recursive' strategy.
 index.html | 5 ++++-
 1 file changed, 4 insertions(+), 1 deletion(-)

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git add .

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git commit -m 'noconflict'
On branch master
nothing to commit, working tree clean

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git log
commit 76fdfa89581308fd3e21e06a6e87a30cd26a11ee (HEAD -> master)
Merge: 2694c72 9668e01
Author: Corley <cutercorleytd@gmail.com>
Date:   Sat Mar 28 17:05:11 2020 +0800

    Merge branch 'dev'

commit 9668e01c33ab314cc53eb7de84f6c4cc704102a0 (dev)
Author: Corley <cutercorleytd@gmail.com>
Date:   Sat Mar 28 17:04:32 2020 +0800

    shopok

commit 2694c72bcf04da286b6941fb820ade8f856a694e
Author: Corley <cutercorleytd@gmail.com>
Date:   Sat Mar 28 16:56:43 2020 +0800

    nobug

commit e780977495aa5bc83c6aef32913743345b290c05
Author: Corley <cutercorleytd@gmail.com>
Date:   Sat Mar 28 16:50:08 2020 +0800

    shop

commit f12719f22469243dd4e95bc3de7da2f524627938
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:17:09 2020 +0800

    -v3

commit 742d05cf56bd15331d6c762a2f6f86b56c14a45b
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:06:18 2020 +0800

    v2

commit 2ec36954c41be1836644d2f18d185ca8e1f67c78
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:01:43 2020 +0800

    v1.15

commit 5f6b74bb67e375a4f9ebc2658893b5eaab2e3bb1
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:58:08 2020 +0800

    v1.1

commit 1e8ddbf90b34a2c11374813cd6d154d73283428e
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:51:36 2020 +0800

    v1

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176177178179180181182183184185186187188189190191192193194195196197198199200201202203204205206207208209210211212213214215216217218219220221222223224225226227228229230231232233234235236237238239240241242243244245246247248249250251252253254255256257258259260261262263264265266267268269270271272273274275276277278279280281282283284285286287288289290291292293294295296297298299300301302303304305306307308309310311312313314315316317318319320321322323324325326327328329330331332333334335336337338339340341342
```

合并应该在主线中进行，合并之后可以删除分支。
合并时可能遇到冲突，可以**手动解决**，打开文档会看到有标注，根据正确的目标修改要解决的冲突。
也可以**用工具**来解决冲突，如Beyond Compare等。
主线上必须是正式版本；分支上是开发版本。

## 三、GitHub的使用

情景假设：
一个程序猿在一个公司上班，在公司码代码后，回到家中需要继续码，有下图中的方式实现代码同步：

- 携带电脑或U盘拷贝
- 传到百度网盘等云端存储
- 使用GitHub进行代码托管

![GitHub代码托管](https://img-blog.csdnimg.cn/20200329122808161.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
显然，程序猿一般都会选择使用Github来同步代码。
使用GitHub实现代码托管的步骤如下：

- 注册账号
- 创建仓库
- 本地代码推送到远程仓库

常见命令如下：

- 给仓库起别名

```bash
git remote add origin 远程仓库地址
1
```

- 将代码推送到Github

```bash
git push -u origin 分支
1
```

- 初次下载（克隆）代码

```bash
git clone 远程仓库地址
1
```

演示如下：
![GitHub code test](https://img-blog.csdnimg.cn/20200329134433945.gif#pic_center)

## 四、代码同步和忘记推送代码的解决

### 1.Github实现家和公司代码的同步

- 切换分支

```bash
git checkout 分支
1
```

- 在公司下载完代码后，继续开发
  切换到dev分支进行开发

```bash
git checkout dev
1
```

- 把master分支合并到dev

```bash
git merge master
1
```

- 提交代码

```bash
git add .
git commit -m "xxx"
git push origin dev
123
```

- 开发完毕，要上线
  将dev分支合并到master，进行上线

```bash
git checkout master
git merge dev
git push origin master
123
```

- 把dev分支也推送到远程

```bash
git checkout dev 
git merge master
git push origin dev
123
```

测试如下：
在公司开发并提交：

```bash
Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (master)
$ git pull origin dev
From https://github.com/corleytd/projecttest
 * branch            dev        -> FETCH_HEAD
 * [new branch]      dev        -> origin/dev
Already up to date.

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (master)
$ git checkout dev
Switched to a new branch 'dev'
Branch 'dev' set up to track remote branch 'dev' from 'origin'.

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (dev)
$ git merge master
Updating 9668e01..76fdfa8
Fast-forward
 index.html | 1 +
 1 file changed, 1 insertion(+)

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (dev)
$ vim index.html

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (dev)
$ git add .

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (dev)
$ git status
On branch dev
Your branch is ahead of 'origin/dev' by 2 commits.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   index.html


Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (dev)
$ git commit -m 'devincomany'
[dev 3c1ae7e] devincomany
 1 file changed, 1 insertion(+)

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (dev)
$ git push origin dev
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 326 bytes | 326.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/corleytd/projecttest.git
   9668e01..3c1ae7e  dev -> dev


123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354
```

回到家中pull代码进行开发并上线提交：

```bash
Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git push -u origin dev
Everything up-to-date
Branch 'dev' set up to track remote branch 'dev' from 'origin'.

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git pull origin master
From https://github.com/corleytd/projecttest
 * branch            master     -> FETCH_HEAD
Already up to date.

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git merge dev
Already up to date.

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git pull origin dev
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
Unpacking objects: 100% (3/3), 306 bytes | 21.00 KiB/s, done.
From https://github.com/corleytd/projecttest
 * branch            dev        -> FETCH_HEAD
   9668e01..3c1ae7e  dev        -> origin/dev
Updating 76fdfa8..3c1ae7e
Fast-forward
 index.html | 1 +
 1 file changed, 1 insertion(+)

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git merge dev
Already up to date.

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ vim index.html

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git add .

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git commit -m 'inhomefinished'
[master 3796244] inhomefinished
 1 file changed, 1 insertion(+)

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git log
commit 379624423128adf931c09717f93ada473b48bd90 (HEAD -> master)
Author: Corley <cutercorleytd@gmail.com>
Date:   Sun Mar 29 14:01:57 2020 +0800

    inhomefinished

commit 3c1ae7e45ed113a5b8d2ca6191830494ece7961a (origin/dev)
Author: Corley <cutercorleytd@gmail.com>
Date:   Sun Mar 29 13:57:06 2020 +0800

    devincomany

commit 76fdfa89581308fd3e21e06a6e87a30cd26a11ee (origin/master)
Merge: 2694c72 9668e01
Author: Corley <cutercorleytd@gmail.com>
Date:   Sat Mar 28 17:05:11 2020 +0800

    Merge branch 'dev'

commit 9668e01c33ab314cc53eb7de84f6c4cc704102a0 (dev)
Author: Corley <cutercorleytd@gmail.com>
Date:   Sat Mar 28 17:04:32 2020 +0800

    shopok

commit 2694c72bcf04da286b6941fb820ade8f856a694e
Author: Corley <cutercorleytd@gmail.com>
Date:   Sat Mar 28 16:56:43 2020 +0800

    nobug

commit e780977495aa5bc83c6aef32913743345b290c05
Author: Corley <cutercorleytd@gmail.com>
Date:   Sat Mar 28 16:50:08 2020 +0800

    shop

commit f12719f22469243dd4e95bc3de7da2f524627938
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:17:09 2020 +0800

    -v3

commit 742d05cf56bd15331d6c762a2f6f86b56c14a45b
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:06:18 2020 +0800

    v2

commit 2ec36954c41be1836644d2f18d185ca8e1f67c78
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 16:01:43 2020 +0800

    v1.15

commit 5f6b74bb67e375a4f9ebc2658893b5eaab2e3bb1
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:58:08 2020 +0800

    v1.1

commit 1e8ddbf90b34a2c11374813cd6d154d73283428e
Author: Corley <cutercorleytd@gmail.com>
Date:   Fri Mar 27 15:51:36 2020 +0800

    v1

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git pull origin dev
From https://github.com/corleytd/projecttest
 * branch            dev        -> FETCH_HEAD
Already up to date.

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git pull origin master
From https://github.com/corleytd/projecttest
 * branch            master     -> FETCH_HEAD
Already up to date.

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126
```

### 2.忘记推送代码的补救

- 在公司拉代码

```bash
git pull origin dev
1
```

- 提交代码

```bash
git add .
git commit -m "xxx" 
12
```

但是并没有提交到GitHub托管。

- 回家继续写代码
  拉代码，发现并没有公司的代码

```bash
git pull origin dev
1
```

无奈，继续开发其他功能

- 把dev分支也推送到远程

```bash
git add .
git commit -m "xxx"
git push origin dev
123
```

- 到公司继续写代码
  拉代码，把昨天的代码拉到本地(可能存在冲突)

```bash
git pull origin dev
1
```

- 手动解决冲突，继续开发
  把dev分支也推送到远程

```bash
git add .
git commit -m "xxx"
git push origin dev
123
```

进行测试如下：

- 在公司开发忘记推送代码

```bash
Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (dev)
$ git branch
* dev
  master

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (dev)
$ vim demo.py

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (dev)
$ git add .
warning: LF will be replaced by CRLF in demo.py.
The file will have its original line endings in your working directory

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (dev)
$ git commit -m 'donotforget'
[dev 666839c] donotforget
 1 file changed, 1 insertion(+)
 create mode 100644 demo.py

12345678910111213141516171819
```

- 回家开发，发现在公司忘记上传代码

```bash
Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (master)
$ git pull origin dev
From https://github.com/corleytd/projecttest
 * branch            dev        -> FETCH_HEAD
Already up to date.

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (dev)
$ vim demo2.py

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (dev)
$ ls
demo2.py  index.html  readme.txt

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (dev)
$ git add .
warning: LF will be replaced by CRLF in demo2.py.
The file will have its original line endings in your working directory

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (dev)
$ git commit -m 'homenewfunc'
[dev 17139e4] homenewfunc
 1 file changed, 1 insertion(+)
 create mode 100644 demo2.py

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/projecttest (dev)
$ git push origin dev
fatal: HttpRequestException encountered.
   ▒▒▒▒▒▒▒▒ʱ▒▒▒▒
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 334 bytes | 334.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/corleytd/projecttest.git
   3c1ae7e..17139e4  dev -> dev


1234567891011121314151617181920212223242526272829303132333435363738
```

- 到公司后合并继续开发

```bash
Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (dev)
$ cat demo2.py
print('hello,newfunc')

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (dev)
$ vim demo.py

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (dev)
$ git add .
warning: LF will be replaced by CRLF in demo.py.
The file will have its original line endings in your working directory

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (dev)
$ git commit -m 'fantastic'
[dev c4e8ff1] fantastic
 1 file changed, 3 insertions(+), 1 deletion(-)

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/testincompany/projecttest (dev)
$ git push origin dev
Logon failed, use ctrl+c to cancel basic credential prompt.
error: unable to read askpass response from 'E:/Git/mingw64/libexec/git-core/git-gui--askpass'
Username for 'https://github.com': corleytd
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 8 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (8/8), 945 bytes | 472.00 KiB/s, done.
Total 8 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
To https://github.com/corleytd/projecttest.git
   17139e4..c4e8ff1  dev -> dev
```

# Python全栈（六）项目前导之5.使用GitHub进行多人协同开发

## 一、rebase的使用

rebase可以保持提交记录简洁，不分叉。
热巴瑟的主要用法有：

- 合并多个commit为一个完整commit
- 将某一段commit粘贴到另一个分支上

其中第一种用法为`git rebase -i HEAD~NUM`。
假设现已开发了四个版本，现需要将后三个合并，可以操作如下：

```bash
Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase
$ git init
Initialized empty Git repository in E:/Test/pro_rebase/.git/

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ touch 1.py

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ git add .

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ git commit -m 'v1'
[master (root-commit) 14ba83e] v1
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 1.py

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ git log
commit 14ba83ee063f927cc11826188651d684424277af (HEAD -> master)
Author: Corley <cutercorleytd@gmail.com>
Date:   Tue Mar 31 10:04:30 2020 +0800

    v1

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ touch 2.py

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ git add .

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ git commit -m 'v2'
[master b1ce2da] v2
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 2.py

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ git log
commit b1ce2da891238d6694c5e0853e300272403479b8 (HEAD -> master)
Author: Corley <cutercorleytd@gmail.com>
Date:   Tue Mar 31 10:04:56 2020 +0800

    v2

commit 14ba83ee063f927cc11826188651d684424277af
Author: Corley <cutercorleytd@gmail.com>
Date:   Tue Mar 31 10:04:30 2020 +0800

    v1

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ touch 3.py

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ git add .

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ git commit -m 'v3'
[master b076bba] v3
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 3.py

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ git log
commit b076bba0c3948ce15fe40b31ce04e86074d91641 (HEAD -> master)
Author: Corley <cutercorleytd@gmail.com>
Date:   Tue Mar 31 10:05:45 2020 +0800

    v3

commit b1ce2da891238d6694c5e0853e300272403479b8
Author: Corley <cutercorleytd@gmail.com>
Date:   Tue Mar 31 10:04:56 2020 +0800

    v2

commit 14ba83ee063f927cc11826188651d684424277af
Author: Corley <cutercorleytd@gmail.com>
Date:   Tue Mar 31 10:04:30 2020 +0800

    v1

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ touch 4.py

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ git addd .
git: 'addd' is not a git command. See 'git --help'.

The most similar command is
        add

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ git add .

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ git commit  -m 'v4'
[master 08fd4d1] v4
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 4.py

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ git log
commit 08fd4d139d53972ab0a12a3e43bfabd371653d22 (HEAD -> master)
Author: Corley <cutercorleytd@gmail.com>
Date:   Tue Mar 31 10:06:27 2020 +0800

    v4

commit b076bba0c3948ce15fe40b31ce04e86074d91641
Author: Corley <cutercorleytd@gmail.com>
Date:   Tue Mar 31 10:05:45 2020 +0800

    v3

commit b1ce2da891238d6694c5e0853e300272403479b8
Author: Corley <cutercorleytd@gmail.com>
Date:   Tue Mar 31 10:04:56 2020 +0800

    v2

commit 14ba83ee063f927cc11826188651d684424277af
Author: Corley <cutercorleytd@gmail.com>
Date:   Tue Mar 31 10:04:30 2020 +0800

    v1

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ git rebase -i HEAD~3
[detached HEAD fc3a407] v2 & v3 & v4
 Date: Tue Mar 31 10:04:56 2020 +0800
 3 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 2.py
 create mode 100644 3.py
 create mode 100644 4.py
Successfully rebased and updated refs/heads/master.

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/pro_rebase (master)
$ git log
commit fc3a407e30eebff4c4540dc699c823ea4b63d1c5 (HEAD -> master)
Author: Corley <cutercorleytd@gmail.com>
Date:   Tue Mar 31 10:04:56 2020 +0800

    v2 & v3 & v4

commit 14ba83ee063f927cc11826188651d684424277af
Author: Corley <cutercorleytd@gmail.com>
Date:   Tue Mar 31 10:04:30 2020 +0800

    v1


123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152
```

## 二、多人协同开发工作流

多人开发工作流思路如下：
![多人协同开发工作流思路](https://img-blog.csdnimg.cn/20200331194801861.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 1.创建组织和项目

- 创建项目目录

```bash
mkdir xxx
1
```

- 基本版本开发

```bash
touch app.py
git add .
git commit -m 'basicversion'
123
```

- 创建组织
- 创建组织下的仓库
  创建组织和仓库的示例如下：
  ![Github组织和仓库创建](https://img-blog.csdnimg.cn/20200331183155764.gif#pic_center)
- 推送代码到组织下的仓库

```bash
git remote add origin xxx
git push origin master
12
```

创建之后得到的基础版本如下：
![基本版本](https://img-blog.csdnimg.cn/20200401083345602.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

- 给版本打标签

```bash
git tag -a v1-m 'xxx'
git push origin --tags # 推送标签
12
```

测试如下：

```bash
Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test
$ mkdir multi

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test
$ cd multi/

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/multi
$ git init
Initialized empty Git repository in E:/Test/multi/.git/

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/multi (master)
$ touch app.py

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/multi (master)
$ git add .

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/multi (master)
$ git commit -m '基本版本'
[master (root-commit) ef92c45] 基本版本
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 app.py

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/multi (master)
$ git log
commit ef92c4505df01fd99a3cdf3b5f4c7e1ab2064572 (HEAD -> master)
Author: Corley <cutercorleytd@gmail.com>
Date:   Tue Mar 31 10:37:34 2020 +0800

    基本版本

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/multi (master)
$ git remote add origin git@github.com:multidevtest/test_pro.git

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/multi (master)
$ git push -u origin master
Warning: Permanently added the RSA host key for IP address '52.74.223.119' to the list of known hosts.
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 217 bytes | 108.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:multidevtest/test_pro.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/multi (master)
$ git tag -a v1 -m 'v1tag'

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/multi (master)
$ git push -u origin --tags
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Writing objects: 100% (1/1), 155 bytes | 155.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To github.com:multidevtest/test_pro.git
 * [new tag]         v1 -> v1

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/multi (master)
$ git push origin --tags
Everything up-to-date

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/multi (master)
$ git checkout -b dev
Switched to a new branch 'dev'

Lenovo@LAPTOP-61GNF3CH MINGW64 /e/Test/multi (dev)
$ git branch
* dev
  master

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869
```

### 2.邀请成员

协同开发时，需要所有成员都可以对同一个项目进行操作，需要邀请成员并赋予权限，否则无法开发。
github支持两种创建项目的方式供多人协同开发：

- 添加合作者
  将用户添加到仓库合作者中之后，该用户就可以向当前仓库提交代码。
- 创建组织
  将成员邀请进入组织，组织下可以创建多个仓库，组织成员可以向组织下仓库提交代码。
  被邀请者默认对组织中的项目具有读权限，要想其可以开发项目，需要给予其写的权限。

邀请成员的操作如下：
![邀请成员](https://img-blog.csdnimg.cn/20200331183717141.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 3.成员开发

- 成员克隆代码并进行开发

```bash
mkdir xxx
git clone xxx
git checkout -b devc1
touch c1.py
1234
```

- 上交代码

```bash
git add .
git commit -m 'xxx'
git push origin devc1
123
```

### 4.代码审查（code review）

- 组长创建规则
  示例如下：
  ![GitHub create rule](https://img-blog.csdnimg.cn/20200331190416841.gif#pic_center)
- 成员提交code review申请（**New pull request**）
  ![成员 new pull request](https://img-blog.csdnimg.cn/20200331190935335.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
- 组长检查代码（code review）

### 5.提测上线（预发布）

由**专门团队**或项目**leader**执行以下步骤：

- 基于dev分支创建release分支

```bash
git checkout dev 
git checkout -b release
12
```

- 测试，DEBUG
- 合并到master
  本地将release合并到master分支，并上传。

```bash
git checkout master
git merge release
git push origin master
123
```

- 在master分支打tag

```bash
git tag -a v2 -m 'xxx' 
git push origin --tags
12
```

其他相关工作如下：

- dev分支合并release分支，以保证是最新代码

```bash
git checkout dev
git merge release
12
```

- 运维人员下载代码做上线工作

# Python全栈（六）项目前导之6.Git补充与Vue的介绍和使用

## 一、Git补充

### 1.Git配置文件

- 项目配置文件
  位置：`项目/.git/config`
  配置项设置如下：

  ```bash
  git config --local user.name 'corley'
  git config --local user.email 'corley@xxx.com'
  12
  ```

- 全局配置文件
  位置：`~/.gitconfig`
  配置项设置如下：

  ```bash
  git config --global user.name 'corley'
  git config --global user.name 'corley@xxx.com'
  12
  ```

- 系统配置文件
  位置：`/etc/.gitconfig`
  配置项设置如下：

  ```bash
  git config --system user.name 'corley'
  git config --system user.name 'corley@xxx.com'
  12
  ```

说明：
项目配置文件设置仅对当前项目有效；
全局配置文件设置针对所有项目有效；
系统配置文件设置需要有root权限。

### 2.免密码登录设置

#### （1）URL中实现

通常，GitHub地址为https://github.com/multidevtest/test_pro.git；
在URL中加入用户名和密码后，Giothub地址为https://cname:cpass@github.com/multidevtest/test_pro.git。
通过git进行远程连接时，需要使用加入用户名和密码的地址：

```bash
git remote add origin https://cname:cpass@github.com/multidevtest/test_pro.git
git push origin master
12
```

#### （2）SSH实现

步骤如下：

- 生成公钥和私钥
  默认放在~/.ssh目录下,id_rsa.pub公钥、id_rsa私钥

```bash
ssh-keygen  -t  rsa
1
```

- 获取并拷贝公钥的内容

```bash
cat  ~/.ssh/id_rsa.pub
1
```

- Github用户设置中配置ssh-key
- 获取ssh地址
- 在本地进行远程连接时使用ssh地址

```bash
git remote add origin git@github.com:multidevtest/test_pro.git
1
```

- 正常使用

示例如下：
![git ssh nopass set](https://img-blog.csdnimg.cn/20200402095833205.gif#pic_center)

### 3.git忽略文件

在项目路径下创建 **.gitignore**文件用于保存不想上传到GitHub上开放的文件，让Git不再管理当前目录下的这些文件（夹）。
.gitignore文件中存放文件的格式示例如下：

> *.h # 不管理所有以h为后缀 的文件
> !a.h # 管理文件名为a.h的文件
> files/ # 不管理files目录下的所有文件
> *.py[c|a|d] # 不管理后缀名为pya、pyb、pyc的文件

更详细的用法可参考https://github.com/github/gitignore。
设置.gitignore文件演示如下：
![GitHub .gitignore test](https://img-blog.csdnimg.cn/20200402101052107.gif#pic_center)

### 4.github相关文档

- issues
  文档以及任务管理。
- wiki
  项目说明文档。

issues和wiki文档操作示例如下：
![GitHub issues wiki test](https://img-blog.csdnimg.cn/20200402101231976.gif#pic_center)

## 二、VSCode插件安装

VSCode是一个前端开发的利器，因为VSCode有大量插件可以使用，从而可以大大提高前端开发的效率。
如未安装VSCode，可点击https://code.visualstudio.com/选择合适的版本下载安装。
开发Vue项目时，可能使用 **.vue**的单文件开发，就需要一些插件来帮我们识别 **.vue**文件。
前端开发特别是Vue开发的常用插件如下：

- jshint
  js代码规范检查。
- Beautify
  一键美化代码的插件。
- Vetur
  .vue文件识别插件。
- Javascript(ES6) code snippets
  ES6语法提示。
- Auto Rename Tag
  自动重命名标签。
  标签都是成对出现的，开始标签修改了，结束标签也会跟着修改。
- Auto Close Tag
  自动闭合标签。
  针对一些非标准的标签，这个插件是很有用的。
- vue helper
  一些Vue代码的快捷代码。
- vscode-icons
  提供了很多类型的文件夹icon，不同类型的文件夹使用不同的icon，会让文件查找更直观。
- open in browser
  右键可以选择在默认浏览器或指定浏览器中打开当前网页。

在VSCode中安装插件的演示如下：
![vscode extensions install](https://img-blog.csdnimg.cn/20200402101816158.gif#pic_center)

## 三、Vue介绍和基本使用

### 1.Vue介绍

#### Vue的概念

Vue是一套用于构建前后端分离的框架，刚开始是由**国内优秀选手尤雨溪**开发出来的，目前是全球**最**流行的前端框架。
使用vue开发网页很简单，并且技术生态环境完善，社区活跃 ，是前后端找工作必备技能 ！
学习者可以进入官网https://cn.vuejs.org/v2/guide/学习Vue ，由于Vue的开发者是中国人，所以官方文档也是中文，看着更直白方便，不需要看英文材料或二手翻译的中文材料就可以学习，当然也希望以后可以看到更多的中文官方文档 。

#### Vue的安装

vue的安装大体上分成三种方式：

- 通过script标签引用
- 通过npm(node package manager)来安装
- 通过vue-cli命令行来安装

作为初学者，建议直接通过第一种方式来安装，把心思集中在vue的学习上，而不是安装上。
通过script标签导入Vue的几种用法如下：

- 开发环境下，直接导入

```javascript
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
1
```

- 指定版本号导入

```javascript
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
1
```

- 使用本地**vue.js**文件
  本地vue.js可点击https://download.csdn.net/download/CUFEECR/12294805下载，然后拷贝到项目目录。

```javascript
<script src="xxx/vue.js"></script>
1
```

- 生产环境下，使用压缩后的文件

```javascript
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.min.js"></script>
1
```

### 2.基本使用

#### （1）Vue初始化和data参数使用

要使用Vue，首先需要**创建一个Vue对象**，并且在这个对象中传递**el参数**，el参数全称是element，用来找到一个给vue渲染的根元素。
并且我们可以传递一个**data参数**，data中的所有值都可以直接在根元素下使用 **{{}}** 来使用。

- vue初始化：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        pass
    })
</script>
12345678910111213141516171819
```

- 绑定元素并测试：

```html
<!DOCTYPE html>
<div lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>

    <body>
        <div id="app">
            username:<p>{{username}}</p>
        </div>
    </body>

</div>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el: "#app",
        data: {
            "username": "Python"
        }
    })
</script>
123456789101112131415161718192021222324252627
```

显示如下：
![vue init bind test](https://img-blog.csdnimg.cn/20200402103307331.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

- 在Vue根元素之外进行测试

```html
<!DOCTYPE html>
<div lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>

    <body>
        <div id="app">
            username:<p>{{username}}</p>
        </div>
        <!-- 这里渲染不了 -->
		<p>{{username}}</p>
    </body>

</div>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el: "#app",
        data: {
            "username": "Python"
        }
    })
</script>
1234567891011121314151617181920212223242526272829
```

显示：
![vue outside rootel test](https://img-blog.csdnimg.cn/20200402103749139.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
显然，其中data中的数据，**只能在vue的根元素下使用**，在其他地方是不能被vue识别和渲染的。

#### （2）Vue中方法的使用

也可以在vue对象中添加methods，这个属性中专门用来存储自己的函数。methods中的函数也可以在模板中使用，并且在methods中的函数来访问data中的值，只需要通过`this.属性名`就可以访问到了，不需要额外加`this.data.属性名`来访问。

- 增加方法再进行测试：

```html
<!DOCTYPE html>
<div lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>

    <body>
        <div id="app">
            username:<p>{{username}}</p>
            password:<p>{{password}}</p>
            {{hello()}}
            <p></p>
            {{tell(password)}}
        </div>
    </body>

</div>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el: "#app",
        data: {
            "username": "Python",
            'password':'123'
        },
        methods:{
            hello: function(){
                return 'Good evening ' + this.username
            },
            tell:function(passwd){
                return 'Your password is ' + passwd
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940
```

显示：
![vue method test](https://img-blog.csdnimg.cn/20200402103834713.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，在调用方法如果需要传入参数时，因为调用方法时已经使用了 **{{}}** ，所以不需要再在参数中加 **{{}}** 引用。

- 增加事件测试：

```html
<!DOCTYPE html>
<div lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>

    <body>
        <div id="app">
            username:<p>{{username}}</p>
            password:<p>{{password}}</p>
            {{hello()}}
            <p></p>
            {{tell(password)}}
            <p></p>
            <!-- <button @click="change">改变</button> -->
            <button v-on:click="change">改变</button>
        </div>
    </body>

</div>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el: "#app",
        data: {
            "username": "Python",
            'password':'123'
        },
        methods:{
            hello: function(){
                return 'Good evening ' + this.username
            },
            tell:function(passwd){
                return 'Your password is ' + passwd
            },
            change:function(){
                this.username = 'Java'
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546
```

显示如下：
![vue button event test](https://img-blog.csdnimg.cn/20200402104151154.gif#pic_center)

## 四、Vue模板语法

### 1.显示文本

在html中通过 **{{}}**（双大括号）中可以把Vue对象中的文本插入到网页中。
并且只要Vue对象上对应的值发生改变了，那么html中双大括号中的值也会立马改变。
测试如下：

```html
<!DOCTYPE html>
<div lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>

    <body>
        <div id="app">
            <p>{{username}}</p>
            <button v-on:click="change">点击修改</button>
        </div>
    </body>

</div>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    let vm = new Vue({
        el: "#app",
        data: {
            "username": "床前明月光"
        },
        methods: {
            change: function(){
                this.username = '疑是地上霜';
            }
        }
    });
</script>
12345678910111213141516171819202122232425262728293031323334
```

显示：
![vue text change test](https://img-blog.csdnimg.cn/20200402104934125.gif#pic_center)
如果在html的{{}}中，第一次渲染完成后，不想要跟着后期数据的更改而更改，那么可以使用**v-once**指令来实现。
例如：

```html
<!DOCTYPE html>
<div lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>

    <body>
        <div id="app">
            <p v-once>1.{{username}}</p>
            <p v>2.{{username}}</p>
            <button v-on:click="change">点击修改</button>
        </div>
    </body>

</div>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    let vm = new Vue({
        el: "#app",
        data: {
            "username": "床前明月光"
        },
        methods: {
            change: function(){
                this.username = '疑是地上霜';
            }
        }
    });
</script>
1234567891011121314151617181920212223242526272829303132333435
```

显示：
![vue text nochange test](https://img-blog.csdnimg.cn/20200402105534347.gif#pic_center)

### 2.渲染原生HTML

有时候我们的Vue对象中，或者是后台返回给我们一个段原生的html代码，我们需要渲染出来，那么如果直接通过{{}}渲染，会把这个html代码当做字符串。这时候我们就可以通过**v-html**指令来实现。
测试如下：

```html
<!DOCTYPE html>
<div lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>

    <body>
        <div id="app">
            username:<p>{{username}}</p>
            password:<p>{{password}}</p>
            <p></p>
            <p>{{code}}</p>
            <p v-html='code'>{{code}}</p>
        </div>
    </body>

</div>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el: "#app",
        data: {
            "username": "Python",
            'password':'123',
            'code':'<a href="https://www.baidu.com">百度一下</a>'
        },
        
    })
</script>
12345678910111213141516171819202122232425262728293031323334
```

显示：
![vue raw html test](https://img-blog.csdnimg.cn/20200402105751882.gif#pic_center)
显然，要想渲染HTML，可以使用`<p v-html='code'>{{code}}</p>`，这与`<p v-html='code'></p>`等效；
如果想直接显示原生HTML代码，直接引用变量即可，当成字符串显示。

### 3.属性绑定

如果我们想要在html元素的属性上绑定我们Vue对象中的变量，那么需要通过**v-bind**或者直接使用冒号 **:** 来实现。
测试如下：

```html
<!DOCTYPE html>
<div lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>

    <body>
        <div id="app">
            username:<p>{{username}}</p>
            password:<p>{{password}}</p>
            <p></p>
            <!-- 等效用法: <img :src="imgsrc" alt="百度logo"> -->
            <img v-bind:src="imgsrc" alt="百度logo">
        </div>
    </body>

</div>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el: "#app",
        data: {
            "username": "Python",
            'password':'123',
            'code':'<a href="https://www.baidu.com">百度一下</a>',
            'imgsrc':"https://www.baidu.com/img/bd_logo1.png"
        },
        
    })
</script>
1234567891011121314151617181920212223242526272829303132333435
```

显示：
![vue attribute bind test](https://img-blog.csdnimg.cn/20200402110128564.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，属性不需要用 **{{}}** 包含，直接引用变量名即可；
如果显示纯粹的文本则需要 **{{}}** 。

# Python全栈（六）项目前导之7.Vue模板语法

## 一、属性绑定

### 1.属性绑定class

一般的通过CSS设置标签的方式如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
    <style>
        .title{
            font-size: 20px;
            color: red;
        }
    </style>
</head>
<body>
    <p class="title">中国，加油</p>
</body>
</html>
1234567891011121314151617
```

显示：
![vue attribute bind class normal](https://img-blog.csdnimg.cn/20200403114843401.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

#### 通过数组方式实现

通过数组方式测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
    <style>
        .title{
            font-size: 20px;
            color: red;
        }
        .sub-title{
            font-weight: 800;
        }
    </style>
</head>
<body>
    <p class="title">中国，加油</p>
    <div id="app">
        <p v-bind:class="[class1,class2]">世界，你好</p>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{
            class1:'title',
            class2:'sub-title'
        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536
```

显示：
![vue attribute bind class array](https://img-blog.csdnimg.cn/20200403114912342.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
经过测试，通过`<p v-bind:class="[title,sub-title]">世界，你好</p>`的方式会报错。
通过下列代码也可以达到同样的效果：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
    <style>
        .title{
            font-size: 20px;
            color: red;
        }
        .sub-title{
            font-weight: 800;
        }
    </style>
</head>
<body>
    <p class="title">中国，加油</p>
    <div id="app">
        <p v-bind:class="[class1,class2]">世界，你好</p>
        <!-- 和下列代码效果相同 <p v-bind:class="class1">世界，你好</p> -->
        <p v-bind:class="[class1]">世界，你好</p>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{
            // class1:'title',
            class1:'title sub-title',
            // class2:'sub-title'
        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839
```

#### 通过对象方式实现

对象可以理解成字典。
通过对象方式实现测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
    <style>
        .title{
            font-size: 20px;
            color: red;
        }
        .sub-title{
            font-weight: 800;
        }
    </style>
</head>
<body>
    <p class="title">床前明月光</p>
    <div id="app">
        <p v-bind:class="{title:class1,'sub-title':class2}">疑是地上霜</p>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{
            class1:true,
            class2:true
        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536
```

显示：
![vue attribute bind class object](https://img-blog.csdnimg.cn/20200403114942646.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

### 2.属性绑定Style

#### 通过对象方式实现

通过对象有两种方式绑定style：

- 读取对应样式的值

  ```markup
  <li :style="{backgroundColor:pBgColor,fontSize:pFontSize}">Vue模板语法</li>
  1
  ```

- 直接读取整个字符串

  ```markup
  <li :style="liStyle">Vue模板语法</li>
  1
  ```

用第一种方式实现：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
    <style>
        .title{
            font-size: 20px;
            color: red;
        }
        .sub-title{
            font-weight: 800;
        }
    </style>
</head>
<body>
    <p class="title">过去岁月不可追，未来日子你别催。</p>
    <div id="app">
        <p v-bind:style="{'background-color':color}">莫愁身外七八事，且尽眼前两三杯。</p>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{
            color:'red'
        }
    })
</script>
1234567891011121314151617181920212223242526272829303132333435
```

显示：
![vue attribute bind style object](https://img-blog.csdnimg.cn/20200403115021450.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
像`background-color`这种中间有 **-** 的变量名称，直接使用会报错不能正常显示，有两种解决办法：

- 将其用引号引起来
- 变成驼峰命名的方式，如改为**backgroundColor**。

显然，如果有多个样式要定义时，直接在p标签中定义会显得很冗长，可以将定义style的代码都放入Vue对象中，即第二种方式，测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
    <style>
        .title{
            font-size: 20px;
            color: red;
        }
        .sub-title{
            font-weight: 800;
        }
    </style>
</head>
<body>
    <p class="title">仿佛昨天才恋爱，转眼青春就不在。</p>
    <div id="app">
        <p :style="pstyle">当年那个万人迷，如今已成老太太。</p>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{
            pstyle:{
                'background-color':'red',
                fontSize:"20px"
            }               
        }
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738
```

显示：
![vue attribute bind style shortcut](https://img-blog.csdnimg.cn/20200403115050846.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

#### 通过数组方式实现

数组方式测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
    <style>
        .title{
            font-size: 20px;
            color: red;
        }
        .sub-title{
            font-weight: 800;
        }
    </style>
</head>
<body>
    <p class="title">没事喝杯酒，有闲下盘棋。</p>
    <div id="app">
        <p v-bind:style="[class1,class2]">醉里乾坤大，输赢都不急。</p>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{
            class1:{
                'background-color':'red',
                fontSize:"20px"
            },
            class2:{
                fontWeight:'800'
            }              
        }
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738394041
```

显示：
![vue attribute bind style array](https://img-blog.csdnimg.cn/20200403115143532.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

## 二、执行Javascript和条件判断

### 1.使用JavaScript表达式

在使用了v-bind的html属性中，或者在使用了 **{{}}** 的文本中，可以执行JavaScript。
执行JS测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <p>{{poem}}</p>
        <p v-bind:style="{color:color?'red':'blue'}">{{poem.split('，').reverse().join('，')}}</p>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            poem:'没事喝杯酒，有闲下盘棋，醉里乾坤大，输赢都不急',
            color:false
        }
    })
</script>
123456789101112131415161718192021222324252627
```

显示：
![vue execute JavaScript](https://img-blog.csdnimg.cn/20200403115207145.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，在属性绑定时，可以执行**三目运算符操作**，从而使字体显示蓝色。
注意：
只能是**JavaScript表达式**，不能是语句，比如`var a=1;a=2;`这样的是js语句是不能执行的。

### 2.条件判断

在模板中，可以根据条件进行渲染。
条件是用**v-if**、**v-else-if**和**v-else**来组合实现的。
条件判断测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <p v-if="weather == 'sunny'">今天出去玩</p>
        <p v-else-if="weather == 'rainy'">今天待在家</p>
        <p v-else>睡觉吧</p>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            weather:'snowy'
        }
    })
</script>
123456789101112131415161718192021222324252627
```

显示：
![vue condition judgment test 1](https://img-blog.csdnimg.cn/20200403115244966.gif#pic_center)
有时候我们想要在一个条件中加载多个html元素，那么我们可以通过template元素来实现。
再次测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <template v-if="age < 18">
            <p>学习</p>
            <p>玩耍</p>
        </template>
        <template v-else-if="age >= 18 && age < 25">
            <p>找工作</p>
            <p>找女神</p>
        </template>
        <template v-else>
            <p>结婚</p>
            <p>升职</p>
        </template>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            age:30
        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536
```

显示：
![vue condition judgment test 2](https://img-blog.csdnimg.cn/20200403115313974.gif#pic_center)

另外，在模板中，Vue会尽量重用已有的元素，而不是重新渲染，这样可以变得更加高效。
例如，在开发用户登录时，如果允许用户在不同的登录方式之间切换，如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <template v-if="loginType == 'username'">
            <label for="">用户名：</label>
            <input type="text" placeholder="请输入用户名"></input>
        </template>
        <template v-else-if="loginType == 'email'">
            <label for="">邮箱：</label>
            <input type="text" placeholder="请输入邮件地址"></input>
        </template>
        <button v-on:click="changeLoginType">切换登录类型</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            loginType:'username'
        },
        methods:{
            changeLoginType:function(){

                this.loginType = this.loginType == 'username'?'email':'username'
            }
        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839
```

显示：
![vue condition judgment relogin test](https://img-blog.csdnimg.cn/20200403115343265.gif#pic_center)
显然，在username的输入框中输入完信息，切换到邮箱中，之前的信息还是保留下来，这样肯定不符合需求的。
如果我们想要让html元素每次切换的时候都重新渲染一遍，可以在需要重新渲染的元素上加上**唯一的key属性**，其中key属性推荐使用**整型或字符串类型**。
input标签增加key属性后再测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <template v-if="loginType == 'username'">
            <label for="">用户名：</label>
            <input type="text" placeholder="请输入用户名" key='username'></input>
        </template>
        <template v-else-if="loginType == 'email'">
            <label for="">邮箱：</label>
            <input type="text" placeholder="请输入邮件地址" key='email'></input>
        </template>
        <button v-on:click="changeLoginType">切换登录类型</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            loginType:'username'
        },
        methods:{
            changeLoginType:function(){

                this.loginType = this.loginType == 'username'?'email':'username'
            }
        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839
```

显示：
![vue condition judgment relogin key test](https://img-blog.csdnimg.cn/20200403115411559.gif#pic_center)
显然，此时切换登录方式时，会清空输入内容。
当然，也可以通过JS的形式来清空输入内容；
key属性的值可以是包含字符串和整形的任意值，但是尽量应该与该标签的其他属性或所在的父标签的属性等保持一致，以便标识。
此时，其他元素仍然会被高效地复用，因为它们没有添加key属性。

#### v-show和v-if的对比

v-if是“**真正**”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建；
v-if也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
相比之下，v-show就简单得多：不管初始条件是什么，元素总是会被渲染，并且只是简单地基于CSS进行切换。
一般来说， v-if有更高的切换开销，而v-show有更高的初始渲染开销。
因此，如果需要非常**频繁地切换**，则使用**v-show较好**；
如果在运行时**条件很少改变**，则使用**v-if较好**。

## 三、循环

在模板中可以用v-for指令来循环数组、对象等。

### 1.循环数组

进行测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <table>
            <tr>
                <td>标题</td>
                <td>作者</td>
            </tr>
            <tr v-for="book in books">
                <td>{{book.title}}</td>
                <td>{{book.author}}</td>
            </tr>
        </table>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            books:[
                {
                    'title':'Python',
                    'author':'author1'
                },
                {
                    'title':'Java',
                    'author':'author2'
                },
                {
                    'title':'PHP',
                    'author':'author3'
                },
                {
                    'title':'C++',
                    'author':'author4'
                }
            ]
        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051
```

显示：
![vue loop test 1](https://img-blog.csdnimg.cn/20200403115439601.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
可以引入序号并显示：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <table>
            <tr>
                <td>序号</td>
                <td>标题</td>
                <td>作者</td>
            </tr>
            <tr v-for="(book, index) in books">
                <td>{{index+1}}</td>
                <td>{{book.title}}</td>
                <td>{{book.author}}</td>
            </tr>
        </table>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            books:[
                {
                    'title':'Python',
                    'author':'author1'
                },
                {
                    'title':'Java',
                    'author':'author2'
                },
                {
                    'title':'PHP',
                    'author':'author3'
                },
                {
                    'title':'C++',
                    'author':'author4'
                }
            ]
        }
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253
```

显示：
![vue loop test 2](https://img-blog.csdnimg.cn/20200403115459547.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
注意：
`(book, index) in books`中，括号中的第一个为循环中的单个对象，在该例中为book，第二个为索引，这与变量名称无关，只与位置有关。

### 2.循环对象

循环对象跟循环数组是一样的，并且都可以在循环的时候使用接收多个参数。
循环单个对象，进行测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <p v-for="value in user">{{value}}</p>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            user:{
                'name':'Corley',
                'age':18,
                'address':'xxx'
            }
        }
    })
</script>
1234567891011121314151617181920212223242526272829
```

显示：
![vue loop test 3](https://img-blog.csdnimg.cn/20200403115553902.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
此时得到的是字典对象中的值，是默认的。
可以稍微改动得到字典中的键：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <p v-for="(value,key) in user">
            {{key}} : {{value}}
        </p>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            user:{
                'name':'Corley',
                'age':18,
                'address':'xxx'
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031
```

显示：
![vue loop test 4](https://img-blog.csdnimg.cn/20200403115621595.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

## 四、保持状态

循环得到的元素，如果没有使用**key**属性来唯一标识，如果后期的数据发生了更改，默认是会重用的，并且元素的顺序不会跟着数据的顺序更改而更改。
例如：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <div v-for="book in books">
            <label for="">{{book}}</label>
            <input type="text" v-bind:placeholder="book">
        </div>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            books:[
                'Python','Java','PHP','C++'
            ]
        }
    })
</script>
123456789101112131415161718192021222324252627282930
```

显示：
![vue stay state test 1](https://img-blog.csdnimg.cn/2020040311564353.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
对列表进行随机排序测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <div v-for="book in books">
            <label for="">{{book}}</label>
            <input type="text" v-bind:placeholder="book">
        </div>
        <button @click="changeBooks">重新排序</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            books:[
                'Python','Java','PHP','C++'
            ]
        },
        methods:{
            changeBooks:function(){
                // 根据参数的正负进行不同方式的排序
                this.books.sort(function(a,b){
                    // 返回一个正数或负数，可以代表True或False
                    return 5 - Math.random() * 10
                })
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940
```

显示：
![vue stay state test 2](https://img-blog.csdnimg.cn/20200403115717394.gif#pic_center)
排序用到方法`sort()`函数，根据参数的True或者False进行不同方式的排序。
但是在一个输入框输入值之后，再重新排序，输入的值并没有随着之前填入的列表项而移动，而是保持原位不动，示意如下：
![vue stay state test 3](https://img-blog.csdnimg.cn/20200403115742963.gif#pic_center)
显然，这不是我们想看到的，我们需要加入**key属性**来使之保持同步：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <div v-for="book in books" v-bind:key="book">
            <label for="">{{book}}</label>
            <input type="text" v-bind:placeholder="book">
        </div>
        <button @click="changeBooks">重新排序</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            books:[
                'Python','Java','PHP','C++'
            ]
        },
        methods:{
            changeBooks:function(){
                // 根据参数的正负进行不同方式的排序
                this.books.sort(function(a,b){
                    // 返回一个正数或负数，可以代表True或False
                    return 5 - Math.random() * 10
                })
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940
```

显示：
![vue stay state test 4](https://img-blog.csdnimg.cn/20200403115804454.gif#pic_center)
显然，此时数据的位置跟着元素的位置同步变化。

## 五、触发视图更新

### 1.直接赋值更新视图

直接赋值改变数组更新视图测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <div v-for="book in books">
            <label for="">{{book}}</label>
        </div>
        <button @click="updateList">改变目录</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            books:[
                'Python','Java','PHP','C++'
            ]
        },
        methods:{
            updateList:function(){
                // 直接赋值
                this.books = ['C#','Android','Web']
            }
        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536
```

显示：
![vue update view test 1](https://img-blog.csdnimg.cn/20200403115829343.gif#pic_center)
也可以通过改变对象的方式来更新视图，如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <div v-for="value in user">
            <label for="">{{value}}</label>
        </div>
        <button @click="updateList">改变目录</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            books:[
                'Python','Java','PHP','C++'
            ],
            user:{
                'name':'Corley',
                'age':18,
                'address':'xxx'
            }
        },
        methods:{
            updateList:function(){
                // 直接赋值
                // this.books = ['C#','Android','Web']
                this.user.name = 'Tony',
                this.user.age = 20
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940414243
```

显示：
![vue update view test 2](https://img-blog.csdnimg.cn/20200403115853794.gif#pic_center)

### 2.调用函数改变视图

Vue对一些方法进行了包装和变异，以后数组通过这些方法进行数组更新，会出发视图的更新。

#### push()

添加元素。
`push()`函数测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <div v-for="book in books">
            <label for="">{{book}}</label>
        </div>
        <button @click="updateList">更新视图</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            books:[
                'Python','Java','PHP','C++'
            ],
        },
        methods:{
            updateList:function(){
                // 使用方法
                // 将元素加入列表末尾
                this.books.push('Redis')
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637
```

显示：
![vue update view push test](https://img-blog.csdnimg.cn/2020040311591832.gif#pic_center)

#### pop()

删除数组最后一个元素。
`pop()`函数测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <div v-for="book in books">
            <label for="">{{book}}</label>
        </div>
        <button @click="updateList">更新视图</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            books:[
                'Python','Java','PHP','C++'
            ],
        },
        methods:{
            updateList:function(){
                // 使用方法
                // 弹出列表末尾元素
                this.books.pop()
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637
```

显示：
![vue update view pop test](https://img-blog.csdnimg.cn/2020040311594197.gif#pic_center)

#### shift()

删除数组的第一个元素。
`shift()`函数测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <div v-for="book in books">
            <label for="">{{book}}</label>
        </div>
        <button @click="updateList">更新视图</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            books:[
                'Python','Java','PHP','C++'
            ],
        },
        methods:{
            updateList:function(){
                // 使用方法
                // 删除列表开始元素
                this.books.shift()
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637
```

显示：
![vue update view shift test](https://img-blog.csdnimg.cn/2020040312034273.gif#pic_center)

#### unshift(item)

在数组的开头位置添加一个元素。
`unshift()`函数测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <div v-for="book in books">
            <label for="">{{book}}</label>
        </div>
        <button @click="updateList">更新视图</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            books:[
                'Python','Java','PHP','C++'
            ],
        },
        methods:{
            updateList:function(){
                // 使用方法
                // 向开头添加元素
                this.books.unshift('HTML')
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637
```

显示：
![vue update view unshift test](https://img-blog.csdnimg.cn/20200403120431987.gif#pic_center)

#### splice(index,howmany,item1,…,itemX)

向数组中添加或者删除或者替换元素。
即`splice()`函数有3种用法：

- ```
  splice()
  ```

  函数向指定位置添加元素：

  此时有3个参数，意义分别是：

  - 添加元素起始位置的下标（从0开始）
  - 长度，标识添加时，参数值为0
  - 添加的值
    用法的简单演示如下：
    ![splice 添加元素 用法](https://img-blog.csdnimg.cn/20200403113623148.png)
    测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <div v-for="book in books">
            <label for="">{{book}}</label>
        </div>
        <button @click="updateList">更新视图</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            books:[
                'Python','Java','PHP','C++'
            ],
        },
        methods:{
            updateList:function(){
                // 使用方法
                // 向列表指定位置添加元素
                this.books.splice(0,0,'MySQL')
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637
```

显示：
![vue update view splice add test](https://img-blog.csdnimg.cn/20200403120459542.gif#pic_center)
此时相当于调用`shift()`函数。

- ```
  splice()
  ```

  函数向指定位置删除元素：

  此时有2个参数：

  - 删除元素起始位置的下标（从0开始）
  - 删除的元素个数
    用法简单演示如下：
    ![splice 删除元素 用法](https://img-blog.csdnimg.cn/20200403113957574.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
    测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <div v-for="book in books">
            <label for="">{{book}}</label>
        </div>
        <button @click="updateList">更新视图</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            books:[
                'Python','Java','PHP','C++'
            ],
        },
        methods:{
            updateList:function(){
                // 使用方法
                // 向列表指定位置删除元素
                this.books.splice(0,2)
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637
```

显示：
![vue update view splice delete test](https://img-blog.csdnimg.cn/20200403120518528.gif#pic_center)

- ```
  splice()
  ```

  函数向指定位置替换元素：

  此时有多个（不少于3个）参数：

  - 替换元素起始位置的下标（从0开始）
  - 被替换的元素个数（不能为0）
  - 其余为要替换的值
    如果只有3个参数，则所有被替换的位置被替换成一个值；
    如果多于3个参数，则从第三个参数后边的参数被一一对应到列表中的相应位置。
    用法简单演示如下：
    ![splice 替换元素 用法](https://img-blog.csdnimg.cn/20200403114528486.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
    测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <div v-for="book in books">
            <label for="">{{book}}</label>
        </div>
        <button @click="updateList">更新视图</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            books:[
                'Python','Java','PHP','C++'
            ],
        },
        methods:{
            updateList:function(){
                // 使用方法
                // 从列表指定位置替换元素
                this.books.splice(0,2,'Redis','MySQL')
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637
```

显示：
![vue update view splice replace test](https://img-blog.csdnimg.cn/20200403120538235.gif#pic_center)

#### sort(function)

排序。
`sort()`函数测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <div v-for="book in books">
            <label for="">{{book}}</label>
        </div>
        <button @click="updateList">更新视图</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            books:[
                'Python','Java','PHP','C++'
            ],
        },
        methods:{
            updateList:function(){
                // 使用方法
                // 随机排序
                this.books.sort(function (x,y){
                    a = Math.random()
                    b = Math.random()
                    return a - b
                })
            }
        }
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738394041
```

显示：
![vue update view sort test](https://img-blog.csdnimg.cn/20200403120559395.gif#pic_center)

#### reverse()

将数组元素进行反转。
`reverse()`函数测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <div v-for="book in books">
            <label for="">{{book}}</label>
        </div>
        <button @click="updateList">更新视图</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
    new Vue({
        el:"#app",
        data:{ 
            books:[
                'Python','Java','PHP','C++'
            ],
        },
        methods:{
            updateList:function(){
                // 使用方法
                // 顺序反转
                this.books.reverse()
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637
```

显示：
![vue update view reverse test](https://img-blog.csdnimg.cn/20200403120618980.gif#pic_center)

### 3.视图更新注意事项

#### 对于数组

- 直接修改数组中的单个值不会触发视图更新。
  例如：

```javascript
this.books[0] = 'Python';
1
```

这是不会触发试图更新的。
应该改成用`splice()`或者是用`Vue.set`方法来实现：

```javascript
Vue.set(this.books,0,'Python');
1
```

进行测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id="app">
        <div v-for="book in books">
            <label for="">{{book}}</label>
        </div>
        <button @click="updateList">更新视图</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            books:[
                'Python','Java','PHP','C++'
            ],
        },
        methods:{
            updateList:function(){
                this.books[0] = 'HTML'
            }
        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536
```

显示：
![vue update view attention test 1](https://img-blog.csdnimg.cn/20200404091946797.gif#pic_center)
显然，此时视图不会发生改变，要想改变数组中的单个值，可以用下面的方式：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id="app">
        <div v-for="book in books">
            <label for="">{{book}}</label>
        </div>
        <button @click="updateList">更新视图</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            books:[
                'Python','Java','PHP','C++'
            ],
        },
        methods:{
            updateList:function(){
                // this.books[0] = 'HTML'
                Vue.set(this.books,1,'HTML')
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637
```

显示：
![vue update view attention test 2](https://img-blog.csdnimg.cn/20200404092006909.gif#pic_center)
此时可以正常改变单个值。

#### 对于对象

对象不会受到不能修改单个值的限制，即可以单独直接修改对象的某个属性，但是如果动态的给对象添加属性，也不会触发视图更新，只能通过`Vue.set`来添加。
测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id="app">
        <div v-for="value in user">
            <label for="">{{value}}</label>
        </div>
        <button @click="updateDict">更新视图</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            user:{
                name:'Corley',
                age:18,
                address:'xxx'
            }
        },
        methods:{
            updateDict:function(){
                this.user.name = 'Tony'
            }
        }
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738
```

显示：
![vue update view attention test 3](https://img-blog.csdnimg.cn/20200404092028262.gif#pic_center)
对于对象，不能动态增加原来没有的属性，如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id="app">
        <div v-for="value in user">
            <label for="">{{value}}</label>
        </div>
        <button @click="updateDict">更新视图</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            user:{
                name:'Corley',
                age:18,
                address:'xxx'
            }
        },
        methods:{
            updateDict:function(){
                this.user.position = 'XXXX'
            }
        }
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738
```

显示：
![vue update view attention test 4](https://img-blog.csdnimg.cn/20200404092048842.gif#pic_center)
显然，此时是没有发生改变的，即不能通过增加对象的属性来改变试图，此时也可以通过调用Vue对象的set方法来增加属性：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id="app">
        <div v-for="value in user">
            <label for="">{{value}}</label>
        </div>
        <button @click="updateDict">更新视图</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            user:{
                name:'Corley',
                age:18,
                address:'xxx'
            }
        },
        methods:{
            updateDict:function(){
                // this.user.position = 'XXXX'
                Vue.set(this.user,'position','XXXX')
            }
        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839
```

显示：
![vue update view attention test 5](https://img-blog.csdnimg.cn/2020040409210873.gif#pic_center)
显然，只能更新一次。

# Python全栈（六）项目前导之8.Vue事件绑定、计算属性、表单输入绑定和自定义组件

## 一、事件绑定

### 1.HTML元素事件绑定

事件绑定就是在HTML元素中，通过**v-on**或者 **@** 绑定事件。
事件代码可以直接放到v-on后面，也可以写成一个函数。
测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id="app">
        <p>{{count}}</p>
        <button v-on:click="count+=1">加一</button>
        <button @click="decrease">减一</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            count:0
        },
        methods:{
            // 函数简写形式
            decrease(){
                this.count -= 1
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334
```

显示：
![vue event bind test 1](https://img-blog.csdnimg.cn/20200404103922855.gif#pic_center)
可以达到控制数的增减的效果。
同时，可以对函数传参，如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id="app">
        <p>{{count}}</p>
        <button v-on:click="count+=1">加一</button>
        <button @click="decrease(2)">减一</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            count:0
        },
        methods:{
            // 函数简写形式
            decrease(value){
                this.count -= value
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334
```

显示：
![vue event bind test 2](https://img-blog.csdnimg.cn/20200404103959386.gif#pic_center)

### 2.event参数

如果在事件处理函数中，想要获取原生的**DOM事件**，那么在调用的时候可以在html代码中传递一个`$event`参数。
参数测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id="app">
        <p>{{count}}</p>
        <button v-on:click="count+=1">加一</button>
        <button @click="decrease(2,$event)">减一</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            count:0
        },
        methods:{
            // 函数简写形式
            decrease(value,event){
                this.count -= value
                console.log(event)
            }
        }
    })
</script>
1234567891011121314151617181920212223242526272829303132333435
```

显示：
![vue event bind test 3](https://img-blog.csdnimg.cn/20200404104021858.gif#pic_center)
显然，`$event`参数表示的是与事件（鼠标点击）相关的一些参数和属性。
再次测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id="app">
        <p>{{count}}</p>
        <button v-on:click="count+=1">加一</button>
        <button @click="decrease(2,$event)" name="Corley" value="Vue">减一</button>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            count:0
        },
        methods:{
            // 函数简写形式
            decrease(value,event){
                this.count -= value
                console.log(event.target.value)
                console.log(event.target.name)
            }
        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536
```

显示：
![vue event bind test 4](https://img-blog.csdnimg.cn/20200404104055779.gif#pic_center)
显然，event参数携带了点击按钮事件的很多信息，还包括了Button的相关属性。

## 二、计算属性和监听属性

### 1.计算属性computed

一般情况下属性都是放到data中的，但是有些属性可能是需要经过一些**逻辑计算**后才能得出来，那么我们可以把这类属性变成**计算属性**。
进行测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id="app">
        <label for="">长：</label>
        <input type="number" name="length" v-model:value="length">
        <label for="">宽：</label>
        <input type="number" name="width" v-model:value="width">
        <label for="">面积：</label>
        <input type="number" name="area" v-bind:value="area" readonly>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            length:0,
            width:0
        },
        // 计算属性
        computed:{
            area:function(){
                return this.length * this.width
            }
        }
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738
```

显示：
![vue compute attribute test](https://img-blog.csdnimg.cn/20200404104315331.gif#pic_center)
显然，面积随着长和宽的变化而动态变化。
其中，**v-model**是双向绑定，绑定了Vue对象中的data数据。
在上例中，`v-model`将input标签的value与data中的length绑定，即保持一致，也便于后边计算面积时HTML表单元素和属性的数据同步获得即时数据。
`v-model:value="length"`可以简写成`v-model="length"`，效果是一样的。
可能你会觉得这个计算属性与函数好像有点重复。实际上，计算属性更加智能，它是基于它们的响应式依赖进行缓存的，也就是说只要相关依赖（比如以上例子中的area）没有发生改变，那么这个计算属性的函数不会重新执行，而是直接返回之前的值。这个缓存功能让计算属性访问**更加高效**。

### 2.计算属性的set方法

计算属性默认只有get，不过在需要时也可以提供一个set，但是**提供了set就必须要提供get方法**。
get方法测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id="app">
        <div>
            <label for="">省：</label>
            <input type="text" v-model:value="province">
        </div>
        <div>
            <label for="">市：</label>
            <input type="text" v-model:value="city">
        </div>
        <div>
            <label for="">区：</label>
            <input type="text" v-model:value="district">
        </div>
        <div>
            <label for="">详细地址：</label>
            <input type="text" v-model:value="address" readonly>
        </div>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            province:'',
            city:'',
            district:'',
        },
        // 计算属性
        computed:{
            address:{
                get:function(){
                    var result = ""
                    if(this.province){
                        result = this.province + "省"
                    }
                    if(this.city){
                        result += this.city + "市"
                    }
                    if(this.district){
                        result += this.district + "区"
                    }
                    return result
                }
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061
```

显示：
![vue compute attribute get test](https://img-blog.csdnimg.cn/20200404104346482.gif#pic_center)

增加`set()`方法测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id="app">
        <div>
            <label for="">省：</label>
            <input type="text" v-model:value="province">
        </div>
        <div>
            <label for="">市：</label>
            <input type="text" v-model:value="city">
        </div>
        <div>
            <label for="">区：</label>
            <input type="text" v-model:value="district">
        </div>
        <div>
            <label for="">详细地址：</label>
            <input type="text" v-model:value="address">
        </div>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            province:'',
            city:'',
            district:'',
        },
        // 计算属性
        computed:{
            address:{
                get:function(){
                    var result = ""
                    if(this.province){
                        result = this.province + "省"
                    }
                    if(this.city){
                        result += this.city + "市"
                    }
                    if(this.district){
                        result += this.district + "区"
                    }
                    return result
                },
                set:function(value){
                    // 用/包含来表明这是JavaScript中的正则表达式
                    var result = value.split(/省|市|区/)
                    if(result && result.length > 0){
                        this.province = result[0]
                    }
                    if(result && result.length > 1){
                        this.city = result[1]
                    }
                    if(result && result.length > 2){
                        this.district = result[2]
                    }
                }
            }
        }
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374
```

显示：
![vue compute attribute get set test](https://img-blog.csdnimg.cn/20200404104425891.gif#pic_center)

用 **/** 包含来表明这是JavaScript中的正则表达式，用来分割匹配字符串中的省、市、区。
注意：
`get()`和`set()`方法是不能重命名的，否则不能达到效果。

### 3.监听属性watch

监听属性可以针对某个属性进行监听，只要这个属性的值发生改变了，那么就会执行相应的函数。
测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id="app">
        <label>搜索：</label>
        <input type="text" v-model:value="keyword">
        <p>结果：</p>
        <p>{{result}}</p>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            keyword:'',
            result:''
        },
        // 监听属性
        watch:{
            keyword:function(newvalue,oldvalue){
                this.result = '加载中...'
                // 定时器
                setTimeout(() => {
                    this.result = '搜索结果：' + newvalue
                }, 1000);
            }
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940
```

显示：
![vue watch attribute test](https://img-blog.csdnimg.cn/20200404104524847.gif#pic_center)
这个效果可以在一定程度上模拟百度搜索时下拉显示推荐搜索内容的效果。

## 三、表单输入绑定

### 1.input标签输入绑定

**v-model**指定可以实现表单值与属性的双向绑定，即表单元素中更改了值会自动的更新属性中的值，属性中的值更新了会自动更新表单中的值。
v-model在内部为不同的输入类型使用不同的属性并抛出不同的事件：

- text和textarea元素使用value属性和input事件；
- checkbox和radio使用checked属性和change事件；
- select字段使用value属性和change事件。

下面分别进行测试：

- text输入绑定测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id="app">
        <input type="text" v-model:value="message" placeholder="请输入...">
        <p>输入的内容是：{{message}}</p>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            message:''
        },
    })
</script>
123456789101112131415161718192021222324252627
```

显示：
![vue form input bind input text test](https://img-blog.csdnimg.cn/20200404104615309.gif#pic_center)

- textarea测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id="app">
        <textarea name="" v-model:value="message" id="" cols="30" rows="10"></textarea>
        <p>输入的内容是：{{message}}</p>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            message:''
        },
    })
</script>
123456789101112131415161718192021222324252627
```

显示：
![vue form input bind textarea test](https://img-blog.csdnimg.cn/20200404104749182.gif#pic_center)

- checkbox测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <input type="checkbox"  value="Python" v-model="checkedNames">
        <label for="">Python</label>
        <input type="checkbox"  value="Java" v-model="checkedNames">
        <label for="">Java</label>
        <input type="checkbox"  value="PHP" v-model="checkedNames">
        <label for="">PHP</label>
        <br>
        <span>Checked names: {{checkedNames}}</span>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            checkedNames:[]
        },
    })
</script>
123456789101112131415161718192021222324252627282930313233
```

显示：
![vue form input bind checkbox test](https://img-blog.csdnimg.cn/20200404104818673.gif#pic_center)

- radio测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <input type="radio" value="男" v-model="gender">
        <label>男</label>
        <br>
        <input type="radio" value="女" v-model="gender">
        <label>女</label>
        <br>
        <span>Picked: {{gender}}</span>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            gender:''
        },
    })
</script>
1234567891011121314151617181920212223242526272829303132
```

显示：
![vue form input bind radio test](https://img-blog.csdnimg.cn/20200404104839526.gif#pic_center)

- select测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <select v-model="selected">
            <option disabled value="">请选择</option>
            <option value="1">A</option>
            <option>B</option>
            <option value="33">C</option>
        </select>
        <span>Selected: {{selected}}</span>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            selected:''
        },
    })
</script>
1234567891011121314151617181920212223242526272829303132
```

显示：
![vue form input bind select test](https://img-blog.csdnimg.cn/20200404104909722.gif#pic_center)
可以看出：
如果option标签有value值，被绑定的就是value值；
如果没有value值，被绑定的就是选项值，即对应的文本值。

### 2.修饰符

#### .lazy

在默认情况下，v-model在每次input事件触发后将输入框的值与数据进行同步 。
如果不想实时修改、而是光标移开之后再修改，可以添加**lazy修饰符**，从而转变为使用change事件进行同步，可以称之为**懒加载**。
测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
    	<!-- 在"change"时而非"input"时更新，即光标移除input输入框的时候更新 -->
        <input type="text" v-model:value.lazy="message" placeholder="请输入...">
        <p>输入的内容是：{{message}}</p></p>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            message:''
        },
    })
</script>
12345678910111213141516171819202122232425262728
```

显示：
![vue form input bind lazy test](https://img-blog.csdnimg.cn/20200404104935119.gif#pic_center)
`v-model:value.lazy="message"`可以简写为`v-model.lazy="message"`。

#### .number

如果想自动将用户的输入值转为数值类型，可以给v-model添加**number修饰符**。
.number测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <input type="text" v-model:value.number="number" placeholder="请输入...">
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            number:0
        },
    })
</script>
1234567891011121314151617181920212223242526
```

显示：
![vue form input bind number test](https://img-blog.csdnimg.cn/20200404105002711.gif#pic_center)
显然，输入的非数字字符都被过滤掉，只剩下数字。
这通常很有用，因为即使在`type="number"`时，HTML输入元素的值也总会返回字符串。如果这个值无法被`parseFloat()`解析，则会返回原始的值。

#### .trim

如果要自动过滤用户输入的首尾空白字符，可以给v-model添加**trim修饰符**。
.trim测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <input type="text" v-model:value.trim="message" placeholder="请输入...">
        <p>{{message}}</p>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el:'#app',
        data:{
            message:''
        },
        watch:{
            message:function(newvalue,oldvalue){
                console.log(this.message)
            }
        }
    })
</script>
1234567891011121314151617181920212223242526272829303132
```

显示：
![vue form input bind trim test](https://img-blog.csdnimg.cn/20200404105058530.gif#pic_center)

## 四、自定义组件的基本使用

有时候有一组html结构的代码，并且还可能绑定了事件，这段代码可能有多个地方都被使用到了，如果都通过拷贝，会导致很多重复的代码，包括事件部分的代码都是重复的。
这个时候我们可以把这些代码封装成一个组件，以后在使用的时候就跟使用普通的html元素一样，拿过来用就可以了，这就是**自定义组件**。
自定义组件的基本使用测试如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <button-click></button-click>
        <button-click></button-click>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('button-click',{
        template:'<button @click="count+=1">点击了{{count}}次</button>',
        data:function(){
            return {
                count: 0
            }
        }
    })
    new Vue({
        el:'#app',
        data:{
            
        },
    })
</script>
1234567891011121314151617181920212223242526272829303132333435
```

显示：
![vue custom components test](https://img-blog.csdnimg.cn/20200404105120168.gif#pic_center)
显然，自定义组件可以正常使用，并且之间不会相互影响；
同时，大大提高了代码的复用性。
以上我们创建了一个叫做**button-click**的组件，这个组件实现了记录点击次数的功能。后期如果我们想要使用，就直接通过**button-click**使用就可以了。
因为组件是**可复用**的Vue实例，所以它们与**new Vue**接收相同的选项，包括data、computed、watch、methods以及生命周期钩子等，仅有的**例外是像el这样根实例特有的选项**。
同时需要注意：
组件中的**data必须为一个函数**。

# Python全栈（六）项目前导之9.Vue自定义组件和生命周期函数

## 一、给组件添加属性和单一根元素

### 1.自定义组件添加属性

原始的html元素一般都有自己的一些属性，我们自己创建的组件，也可以通过**props属性**来添加自己的属性。这样在使用创建的组件的时候就可以传递不同的参数了。
测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <book-list v-bind:books="books"></book-list>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('book-list',{
        // 给组件添加属性
        props:['books'],
        //  多行代码时用``包含（不是单引号）
        template:`
            <table>
                <tr>
                    <th>序号</th>
                    <th>标题</th>
                </tr>
                <tr v-for="(book,index) in books">
                    <td>{{index + 1}}</td>
                    <td>{{book.title}}</td>
                </tr>
            </table>
        `,
    })
    new Vue({
        el:'#app',
        data:{
            books:[
                {
                    'title':'Python',
                    'id':1
                },
                {
                    'title':'Java',
                    'id':2
                },
                {
                    'title':'PHP',
                    'id':3
                },
            ],
        },
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556
```

显示：
![vue add attribute to component](https://img-blog.csdnimg.cn/2020040421240295.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
在创建的组件中，不能直接使用Vue对象中的数据，需要将数据绑定到新建组件的prop属性中，并且在使用新创建的组件需要属性**绑定**到相应的数据才能正常引用数据。

### 2.单一根元素

自定义的组件中可以出现多个html元素，但是根元素只能有一个，其余的元素必须包含在这个根元素中。
比如以下是一个组件中模板的代码：

```markup
template:`
	<h3>{{ title }}</h3>
	<div v-html="content"></div>
`
1234
```

会报错，可以改成：

```markup
template:`
	<div>
		<h3>{{ title }}</h3>
		<div v-html="content"></div>
	</div>
`
123456
```

在新建组建的模板中添加多个标签并进行测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <book-list v-bind:books="books"></book-list>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('book-list',{
        // 给组件添加属性
        props:['books'],
        //  多行代码时用``包含（不是单引号）
        template:`
        <table>
            <tr>
                <th>序号</th>
                <th>标题</th>
            </tr>
            <tr v-for="(book,index) in books">
                <td>{{index + 1}}</td>
                <td>{{book.title}}</td>
            </tr>
        </table>
        <div>返回顶部</div>
        `,
    })
    new Vue({
        el:'#app',
        data:{
            books:[
                {
                    'title':'Python',
                    'id':1
                },
                {
                    'title':'Java',
                    'id':2
                },
                {
                    'title':'PHP',
                    'id':3
                },
            ],
        },
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657
```

与之前显示相同，即模板中的根元素有两个时是不能正常显示的，只能有一个根元素。
将它们放入一个根元素后再测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>
<body>
    <div id='app'>
        <book-list v-bind:books="books"></book-list>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('book-list',{
        // 给组件添加属性
        props:['books'],
        //  多行代码时用``包含（不是单引号）
        template:`
        <div>
            <table>
            <tr>
                <th>序号</th>
                <th>标题</th>
            </tr>
            <tr v-for="(book,index) in books">
                <td>{{index + 1}}</td>
                <td>{{book.title}}</td>
            </tr>
            </table>
            <div>返回顶部</div>
        </div>        
        `,
    })
    new Vue({
        el:'#app',
        data:{
            books:[
                {
                    'title':'Python',
                    'id':1
                },
                {
                    'title':'Java',
                    'id':2
                },
                {
                    'title':'PHP',
                    'id':3
                },
            ],
        },
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859
```

显示：
![vue one root element](https://img-blog.csdnimg.cn/20200404212652395.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
此时可以正常显示，即要满足**单一根元素**原则。

## 二、子组件事件和传递事件到父组件

子组件中添加事件跟之前的方式是一样的，如果发生某个事件后想要通知父组件，可以使用`this.$emit`函数来实现。
测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <book-list v-for="book in books" v-bind:book="book" @check-changed="check"></book-list>
        <div v-for="book in bookcomponent">{{book.title}}</div>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('book-list', {
        props: ['book'],
        template: `
        <div>
            <span>{{book.title}}</span>
            <input type="checkbox" @click="onCheck">
        </div>        
        `,
        methods:{
            onCheck:function(){
                this.$emit('check-changed',this.book)
            }
        }
    })
    new Vue({
        el: '#app',
        data: {
            books: [{
                    'title': 'Python',
                    'id': 1
                },
                {
                    'title': 'Java',
                    'id': 2
                },
                {
                    'title': 'PHP',
                    'id': 3
                },
            ],
            bookcomponent: []
        },
        methods:{
            check:function(book){
                // indexOf()函数返回元素的下标，大于等于0则表明数组中存在该元素
                var index = this.bookcomponent.indexOf(book)
                if(index >= 0){
                    this.bookcomponent.splice(index,1)
                }
                else{
                   this.bookcomponent.push(book)
                }
            }
        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869
```

显示：
![vue subcomp pass event to supercomp](https://img-blog.csdnimg.cn/20200404212802505.gif#pic_center)
`indexOf()`函数返回元素在数组中的下标，如果值大于等于0，则说明该数组中存在该元素。
显然，上面子组件被选择后，会同步显示到父组件中，其中的时间流和数据流的流转原理如下：
![vue subcomponent pass event to supercomponent](https://img-blog.csdnimg.cn/20200404212826932.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
其中，红色标示①→④表示**事件流**，即点击复选框触发的事件的流转；
蓝色标识⑤→⑩标识**数据流**，即父组件从子组件获取数据并即时同步的过程；
子组件触发的事件通过与数据流的交织传递最终到父组件，并达到需要的效果。
注意：
因为html中大小写是不敏感的，所以在定义子组件传给父组件事件名称的时候，尽量避免使用**checkChanged**这种驼峰命名法，而是使用**check-changed**这种规则。

## 三、自定义组件v-model

一个组件上的v-model默认会利用**value属性**和**input事件**，并且像单选框、复选框等类型的输入控件可能会将value特性用于不同的目的，这时候我们可以在定义组件的时候，通过设置**model选项**来实现不同的处理方式。
模拟一个计步器，测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <stepper v-model:value="stepcount"></stepper>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('stepper', {
        props: ['count'],
        model:{
            // 触发v-model事件的情况
            event:'count-changed',
            prop:'count'
        },
        template: `
        <div>
            <button @click="sub">-</button>
            <span>{{count}}</span>
            <button @click="add">+</button>
        </div>
        `,
        methods:{
            sub:function(){
                this.$emit('count-changed',this.count - 1) 
            },
            add:function(){
                this.$emit('count-changed',this.count + 1) 
            }
        }
    })
    new Vue({
        el: '#app',
        data: {
            stepcount:0
        },
        methods:{

        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455
```

显示：
![vue custom v-model](https://img-blog.csdnimg.cn/20200404212857378.gif#pic_center)
说明：
在自定义的组件中，

- props定义的属性是给组件外部调用组件的时候使用的；
- model中定义的`prop:'count'`是告诉后面使用v-model的时候，要修改哪个属性；
- `event:'count-changed'`是告诉v-model，后面触发哪个事件的时候要修改属性。

数据流和时间流传递原理如下：
![vue custom v-model principal](https://img-blog.csdnimg.cn/20200404212927754.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
其中，红色表示事件流，蓝色表示数据流；
通过数据与事件的相互作用，达到了自定义组件中v-model的目的。

## 四、Vue插槽

### 1.插槽的定义和简单使用

我们定义完一个组件后，可能在使用的时候还需要往这个组件中**插入新的元素或者文本**，这时候就可以使用插槽来实现。
测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <navigation-link :url="url">百度一下</navigation-link>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('navigation-link',{
        props:['url'],
        template:`
        <a v-bind:href="url">
            <slot></slot>
        </a>
        `
    })
    new Vue({
        el: '#app',
        data: {
            url:'https://www.baidu.com'
        },
        methods:{

        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940
```

显示：
![vue slot test](https://img-blog.csdnimg.cn/20200404213212780.gif#pic_center)
即在新建的组件中插入的内容（百度一下）是通过插入到插槽标签`<slot>`中实现的。
如果在模板中的标签中定义了文本，则在HTML正文中的定义出的组件中的文本的**优先级更大**，会显示出来，如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <navigation-link :url="url">百度一下</navigation-link>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('navigation-link',{
        props:['url'],
        template:`
        <a v-bind:href="url">
            <slot>小度小度</slot>
        </a>
        `
    })
    new Vue({
        el: '#app',
        data: {
            url:'https://www.baidu.com'
        },
        methods:{

        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940
```

即**百度一下**的优先级更高，会显示，与之前测试的效果相同。
不仅是文本，插槽可以包含任何模板代码，包含HTML语句，例如：

```markup
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <navigation-link :url="url">
            <span class="fa fa-user">不懂你就</span>
            百度一下
        </navigation-link>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('navigation-link',{
        props:['url'],
        template:`
        <a v-bind:href="url">
            <slot></slot>
        </a>
        `
    })
    new Vue({
        el: '#app',
        data: {
            url:'https://www.baidu.com'
        },
        methods:{

        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940414243
```

显示：
![vue slot html test](https://img-blog.csdnimg.cn/202004041946427.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
如果没有包含一个元素，则该组件起始标签和结束标签之间的任何内容都会被抛弃。
即在自定义组件时，如果**没有包含**slot元素，在HTML中的自定义组件中插入任意的文本或HTML元素都是**不会被渲染到网页**的，如下：

```markup
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <navigation-link :url="url">
            <span class="fa fa-user">不懂你就</span>
            百度一下
        </navigation-link>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('navigation-link',{
        props:['url'],
        template:`
        <a v-bind:href="url">
        </a>
        `
    })
    new Vue({
        el: '#app',
        data: {
            url:'https://www.baidu.com'
        },
        methods:{

        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142
```

显示：
![vue slot component no slot](https://img-blog.csdnimg.cn/20200404195205854.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
显示空白，即没有元素被渲染出来。

### 2.定义多个插槽

我们还可以通过给**插槽命名**来实现多个插槽，测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <navigation-link :url="url">
            <template v-slot:first>第一个插槽</template>
            <template v-slot:second>第二个插槽</template>
            <template v-slot:third>第三个插槽</template>
        </navigation-link>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('navigation-link',{
        props:['url'],
        template:`
        <a v-bind:href="url">
            <slot name="first"></slot>
            <br>
            <slot name="second"></slot>
            <br>
            <slot name="third"></slot>
        </a>
        `,
        data:function(){
            return {
                name:'Python'
            }
        }
    })
    new Vue({
        el: '#app',
        data: {
            url:'https://www.baidu.com',
            name:'Corley'
        },
        methods:{

        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354
```

显示：
![vue slot name multi test](https://img-blog.csdnimg.cn/20200404213310615.gif#pic_center)
如果有部分未命名，则为默认插槽，测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <navigation-link :url="url">
            <template v-slot:first>第一个插槽</template>
            <template v-slot:second>第二个插槽</template>
            <template>默认插槽</template>
            <template v-slot:third>第三个插槽</template>
        </navigation-link>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('navigation-link',{
        props:['url'],
        template:`
        <a v-bind:href="url">
            <slot name="first"></slot>
            <br>
            <slot name="second"></slot>
            <br>
            <slot name="third"></slot>
            <br>
            <slot></slot>
        </a>
        `,
        data:function(){
            return {
                name:'Python'
            }
        }
    })
    new Vue({
        el: '#app',
        data: {
            url:'https://www.baidu.com',
            name:'Corley'
        },
        methods:{

        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657
```

显示：
![vue slot partname default test](https://img-blog.csdnimg.cn/20200405103345241.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

### 3.插槽的作用域

子组件中调用父组件属性测试：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <navigation-link :url="url">{{name}}</navigation-link>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('navigation-link',{
        props:['url'],
        template:`
        <a v-bind:href="url">
            <slot>小度小度</slot>
        </a>
        `
    })
    new Vue({
        el: '#app',
        data: {
            url:'https://www.baidu.com',
            name:'Corley'
        },
        methods:{

        }
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738394041
```

显示：
![vue slot invoke superattribute test](https://img-blog.csdnimg.cn/20200404213711660.gif#pic_center)
很明显，定义的子组件可以访问到父组件中的属性。
在定义的组件中增加name属性进行测试：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <navigation-link :url="url">{{name}}</navigation-link>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('navigation-link',{
        props:['url'],
        template:`
        <a v-bind:href="url">
            <slot>小度小度</slot>
        </a>
        `,
        data:function(){
            return {
                name:'Python'
            }
        }
    })
    new Vue({
        el: '#app',
        data: {
            url:'https://www.baidu.com',
            name:'Corley'
        },
        methods:{

        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546
```

与前者效果相同。
可以看到，自定义的组件的插槽只能访问到父组件中的属性，而不能访问到自己的属性。
在定义组件时绑定测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <container>
            <template v-slot:header="headerProps">
                顶部{{headerProps.navs}}
            </template>
            <template  v-slot:footer="footerProps">
                底部{{footerProps.contact}}
            </template>
        </container>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('container',{
        props:['url'],
        template:`
        <div>
            <div>
                <slot name='header' :navs="navs"></slot>
            </div>
            <div>
                <slot name='footer' :contact="contact"></slot>
            </div>
        </div>
        `,
        data:function(){
            return {
                navs:['网页','咨询','视频'],
                contact:'xxx'
            }
        }
    })
    new Vue({
        el: '#app',
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152
```

显示：
![vue slot scope test](https://img-blog.csdnimg.cn/20200404213804467.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
可以总结：
默认情况下，slot可以**直接使用父组件中的数据**；
要想slot使用自定义组件中的数据，需要**slot标签进行绑定**（v-bind）。

## 五、生命周期函数

### 1.Vue生命周期定义

生命周期函数代表的是Vue实例或者是Vue组件，在网页中各个生命阶段所执行的函数。
Vue生命周期可以类比Python中的对象。
生命周期函数可以分为三个阶段：

- 创建阶段
- 运行阶段
- 销毁阶段

在生命周期的各个阶段的执行过程可以从下图（官方文档提供）得出：
![vue lifecycle](https://img-blog.csdnimg.cn/20200405101632164.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)

### 2.创建阶段

创建期间的函数有`beforeCreate`、`created`、`beforeMount`和`mounted`。

#### beforeCreate

Vue或者组件刚刚实例化，data、methods都还没有被创建。
`beforeCreate()`方法测试：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el: '#app',
        data:{
            username:'Corley'
        },
        methods:{
            hello:function(){
                return 'hello worid'
            }
        },
        beforeCreate(){
            console.log('*************'),
            console.log(this.name),
            console.log(this.hello),
            console.log('*************')
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940
```

显示：
![vue lifecycle beforecreate](https://img-blog.csdnimg.cn/20200404214030541.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

#### created

此时data和methods已经被创建，可以使用了，但模板还没有被编译。
`created()`方法测试：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el: '#app',
        data:{
            username:'Corley'
        },
        methods:{
            hello:function(){
                return 'hello worid'
            }
        },
        created(){
            console.log('*************'),
            console.log(this.username),
            console.log(this.hello),
            console.log('*************')
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940
```

显示：
![vue lifecycle created](https://img-blog.csdnimg.cn/20200404214049479.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

#### beforeMount

created的下一阶段，此时模板已经被编译了，但是并没有被挂在到网页中。
`beforeMount()`方法测试：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <p id='username'>{{username}}</p>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el: '#app',
        data:{
            username:'Corley'
        },
        methods:{
            hello:function(){
                return 'hello worid'
            }
        },
        beforeMount(){
            console.log('*************'),
            console.log(document.getElementById('username').innerText),
            console.log('*************')
        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839
```

显示：
![vue lifecycle beforemount](https://img-blog.csdnimg.cn/20200404214118202.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
即在调用`beforeMount()`方法时还是原生的HTML代码，未挂载到网页。

#### mounted

模板代码已经被加载到网页中，此时创建期间所有事情都已经准备好了，网页开始运行。
`mounted()`方法测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <p id='username'>{{username}}</p>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el: '#app',
        data:{
            username:'Corley'
        },
        methods:{
            hello:function(){
                return 'hello worid'
            }
        },
        mounted(){
            console.log('*************'),
            console.log(document.getElementById('username').innerText),
            console.log('*************')
        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839
```

显示：
![vue lifecycle mounted](https://img-blog.csdnimg.cn/2020040421413726.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

### 3.运行阶段

运行期间的函数有`beforeUpdate`、`updated`。

#### beforeUpdate

在网页网页运行期间，data中的数据可能会进行更新。
在这个阶段，数据只是在data中更新了，但是并没有在模板中进行更新，因此网页中显示的还是之前的。
`beforeUpdate()`方法测试：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <input type="text" v-model="username">
        <p id='username'>{{username}}</p>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el: '#app',
        data:{
            username:'Corley'
        },
        methods:{
            hello:function(){
                return 'hello worid'
            }
        },
        beforeUpdate(){
            console.log('*************'),
            console.log(document.getElementById('username').innerText),
            console.log('*************')
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940
```

显示：
![vue lifecycle beforeupdate](https://img-blog.csdnimg.cn/20200404214159948.gif#pic_center)
显然，每次获取到的都是更新之前的值。

#### updated

此时数据在data中更新了，也在网页中更新了。
`updated()`方法测试：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <input type="text" v-model="username">
        <p id='username'>{{username}}</p>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el: '#app',
        data:{
            username:'Corley'
        },
        methods:{
            hello:function(){
                return 'hello worid'
            }
        },
        updated(){
            console.log('*************'),
            console.log(document.getElementById('username').innerText),
            console.log('*************')
        }
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940
```

显示：
![vue lifecycle updated](https://img-blog.csdnimg.cn/20200404214226327.gif#pic_center)
此时显示的是更新之后的数据。

### 4.销毁阶段

销毁期间的函数有beforeDestroy、destroyed。

#### beforeDestroy

Vue实例或者是组件在被销毁之前执行的函数，在这一个函数中Vue或者组件中所有的属性都是可以使用的。
`beforeDestroy()`方法测试：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <p id='username'>{{username}}</p>
        <error-view v-bind:message="message" v-if="message"></error-view>
        <button @click="message=''">销毁</button>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('error-view',{
        props:['message'],
        template:'<p style="color:red">{{message}}</p>',
        beforeDestroy(){
            console.log('组件已销毁')
        }
    })

    new Vue({
        el: '#app',
        data:{
            username:'Corley',
            message:'错误信息'
        },
        methods:{
            hello:function(){
                return 'hello worid'
            }
        },
        updated(){
            console.log('*************'),
            console.log(document.getElementById('username').innerText),
            console.log('*************')
        }
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
```

显示：
![vue lifecycle beforedestroy](https://img-blog.csdnimg.cn/2020040421462610.gif#pic_center)
由于直接销毁Vue对象即相当于关闭当前网页，查看不到`beforeDestroy()`方法调用时的现象，所以转化为查看销毁创建的组件时的现象。

#### destroyed

Vue实例或者是组件被销毁后执行的，此时Vue实例上所有东西都会解绑，所有事件都会被移除，所有子元素都会被销毁。
测试如下：

```markup
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <p id='username'>{{username}}</p>
        <error-view v-bind:message="message" v-if="message"></error-view>
        <button @click="message=''">销毁</button>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.component('error-view',{
        props:['message'],
        template:'<p style="color:red">{{message}}</p>',
        destroyed() {
            this.$destroy() 
            console.log("destoryed");
        }
    })

    new Vue({
        el: '#app',
        data:{
            username:'Corley',
            message:'错误信息'
        },
        methods:{
            hello:function(){
                return 'hello worid'
            }
        },
    })
</script>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546
```

显示：
![vue lifecycle destroyed](https://img-blog.csdnimg.cn/20200404214645945.gif#pic_center)
调用系统的`destroy()`方法来使组件销毁，从而可以观察到销毁的现象。

# Python全栈（六）项目前导之10.Vue练习和路由基本使用

## 一、过滤器的使用

### 1.过滤器概念

过滤器就是数据在真正渲染到页面中的时候，可以使用过滤器进行一些处理，把最终处理的结果渲染到网页中。
可以把过滤器理解成一个方法对变量进行处理。

### 2.过滤器的定义和使用

过滤器包括本地和全局两种，定义方法分别如下：

- 本地过滤器
  在一个组件的选项中定义。

```javascript
filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}
1234567
```

- 全局过滤器
  创建 Vue 实例之前定义。

```javascript
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})

new Vue({
  // ...
})

12345678910
```

使用也有两种方式：

- 在文本值即双花括号中

```javascript
{{ message|capitalize }}
1
```

- 在**v-bind**表达式中（从版本2.1.0中开始支持）

```html
<div v-bind:id="rawId|formatId"></div>
1
```

对于两种方式，过滤器都应该被添加在JavaScript表达式的尾部，由**管道符号**指示。
进行测试：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <p>{{user|strstrip}}</p>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.filter('strstrip',function(value){
        return value.replace(' ', '')
    })
    new Vue({
        el: '#app',
        data:{
            user:'      Cor ley'
        },
        methods:{
            
        },
    })
</script>
1234567891011121314151617181920212223242526272829303132333435
```

显示：
![vue filter test](https://img-blog.csdnimg.cn/20200405180446272.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，过滤器过滤掉了字符串前面的空格，显然JS中的`replace()`函数只能替换第一次匹配到的字符串。
再次传入参数测试：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue模板语法</title>
</head>

<body>
    <div id='app'>
        <p>{{user|strstrip('-')}}</p>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    Vue.filter('strstrip',function(value,str){
        return value.replace(' ', str)
    })
    new Vue({
        el: '#app',
        data:{
            user:'  Cor ley'
        },
        methods:{
            
        },
    })
</script>
1234567891011121314151617181920212223242526272829303132333435
```

显示：
![vue filter params test](https://img-blog.csdnimg.cn/20200405180511122.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，只是第一个字符串被替换掉了。

## 二、用Vue实现简单的图书管理系统

代码中样式部分引用了**Bootstrap**的样式，可以利用其模板更高效地建立美观的样式。

初始开发测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css">
    <title>图书管理系统</title>
</head>

<body>
    <div id='app'>
        <div class="container">
            <h1>图书管理系统</h1>
            <form class="navbar-form navbar-left" role="search">
                <div class="form-group">
                    <label for="">名称：</label>
                    <input type="text" class="form-control" placeholder="请输入图书名字">
                </div>
                <div class="form-group">
                    <label for="">作者：</label>
                    <input type="text" class="form-control" placeholder="请输入图书作者">
                </div>
                <div class="form-group">
                    <label for="">价格：</label>
                    <input type="text" class="form-control" placeholder="请输入图书价格">
                </div>
                <button type="submit" class="btn btn-default">添加</button>
            </form>
            <table class="table">
                <tr>
                    <th>序号</th>
                    <th>名称</th>
                    <th>作者</th>
                    <th>价格</th>
                    <th>时间</th>
                    <th>操作</th>
                </tr>
                <tr v-for="(book,index) in books">
                    <td>{{index + 1}}</td>
                    <td>{{book.name}}</td>
                    <td>{{book.author}}</td>
                    <td>{{book.price}}</td>
                    <td>{{book.addtime}}</td>
                    <td>删除</td>
                </tr>
            </table>
        </div>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el: '#app',
        data: {
            books: [{
                    name: 'Python',
                    author: 'AUPY',
                    price: 50,
                    addtime: new Date()
                },
                {
                    name: 'Java',
                    author: 'AUJA',
                    price: 40,
                    addtime: new Date()
                },

            ]
        },
        methods: {

        },
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081
```

显示：
![图书管理系统 初始效果](https://img-blog.csdnimg.cn/20200405180615713.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
增加图书添加功能测试：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css">
    <title>图书管理系统</title>
</head>

<body>
    <div id='app'>
        <div class="container">
            <h1>图书管理系统</h1>
            <form class="navbar-form navbar-left" role="search">
                <div class="form-group">
                    <label for="">名称：</label>
                    <input type="text" class="form-control" v-model="adding_book.name" placeholder="请输入图书名字">
                </div>
                <div class="form-group">
                    <label for="">作者：</label>
                    <input type="text" class="form-control" v-model="adding_book.author" placeholder="请输入图书作者">
                </div>
                <div class="form-group">
                    <label for="">价格：</label>
                    <input type="text" class="form-control" v-model="adding_book.price" placeholder="请输入图书价格">
                </div>
                <button type="submit" class="btn btn-default" @click="add">添加</button>
            </form>
            <table class="table">
                <tr>
                    <th>序号</th>
                    <th>名称</th>
                    <th>作者</th>
                    <th>价格</th>
                    <th>时间</th>
                    <th>操作</th>
                </tr>
                <tr v-for="(book,index) in books">
                    <td>{{index + 1}}</td>
                    <td>{{book.name}}</td>
                    <td>{{book.author}}</td>
                    <td>{{book.price}}</td>
                    <td>{{book.addtime}}</td>
                    <td>删除</td>
                </tr>
            </table>
        </div>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el: '#app',
        data: {
            books: [{
                    name: 'Python',
                    author: 'AUPY',
                    price: 50,
                    addtime: new Date()
                },
                {
                    name: 'Java',
                    author: 'AUJA',
                    price: 40,
                    addtime: new Date()
                },

            ],
            adding_book:{
                name: '',
                author: '',
                price: 0,
                addtime:new Date()
            }
        },
        methods: {
            add(){
                // JSON.parse()将参数转化为JS可识别的变量，JSON.stringify()将对象转化为字符串
                var book = JSON.parse(JSON.stringify(this.adding_book))
                this.books.push(book)
                // 添加完成后清空输入框
                this.adding_book = {
                    name: '',
                    author: '',
                    price: 0,
                    addtime:new Date()
                }
            }
        },
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374757677787980818283848586878889909192939495969798
```

显示：
![vue book management system add function](https://img-blog.csdnimg.cn/20200405180639286.gif#pic_center)
在`add()`方法中，`JSON.stringify()`用于将对象转化为字符串，`JSON.parse()`用于将字符串等变量转化为JavaScript可识别的变量，从而摆脱因v-model的双向绑定带来的浅拷贝问题，JSON的两个函数相当于做了一次深拷贝。
在添加功能中可能会遇到一个问题：输入信息点击添加按钮不能正常添加，即添加的信息并不显示出来，如下：
![vue book management system add problem](https://img-blog.csdnimg.cn/20200406094300464.gif#pic_center)
点击添加按钮时可以看到左上角的刷新图标在变化，意思是每次点击添加时页面都重新刷新了一遍，这是因为点击触发了表单的默认提交操作，使得页面刷新，添加的数据也就不存在了，所以未显示，要想正常添加，需要**阻止表单的自动提交**，这只需要给**click属性**添加 **.prevent** 修饰符即可，如下：

```markup
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css">
    <title>图书管理系统</title>
</head>

<body>
    <div id='app'>
        <div class="container">
            <h1>图书管理系统</h1>
            <form class="navbar-form navbar-left" role="search">
                <div class="form-group">
                    <label for="">名称：</label>
                    <input type="text" class="form-control" v-model="adding_book.name" placeholder="请输入图书名字">
                </div>
                <div class="form-group">
                    <label for="">作者：</label>
                    <input type="text" class="form-control" v-model="adding_book.author" placeholder="请输入图书作者">
                </div>
                <div class="form-group">
                    <label for="">价格：</label>
                    <input type="text" class="form-control" v-model="adding_book.price" placeholder="请输入图书价格">
                </div>
                <button type="submit" class="btn btn-default" @click.prevent="add">添加</button>
            </form>
            <table class="table">
                <tr>
                    <th>序号</th>
                    <th>名称</th>
                    <th>作者</th>
                    <th>价格</th>
                    <th>时间</th>
                    <th>操作</th>
                </tr>
                <tr v-for="(book,index) in books">
                    <td>{{index + 1}}</td>
                    <td>{{book.name}}</td>
                    <td>{{book.author}}</td>
                    <td>{{book.price}}</td>
                    <td>{{book.addtime}}</td>
                    <td>删除</td>
                </tr>
            </table>
        </div>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el: '#app',
        data: {
            books: [{
                    name: 'Python',
                    author: 'AUPY',
                    price: 50,
                    addtime: new Date()
                },
                {
                    name: 'Java',
                    author: 'AUJA',
                    price: 40,
                    addtime: new Date()
                },

            ],
            adding_book:{
                name: '',
                author: '',
                price: 0,
                addtime:new Date()
            }
        },
        methods: {
            add(){
                // JSON.parse()将参数转化为JS可识别的变量，JSON.stringify()将对象转化为字符串
                var book = JSON.parse(JSON.stringify(this.adding_book))
                this.books.push(book)
                // 添加完成后清空输入框
                this.adding_book = {
                    name: '',
                    author: '',
                    price: 0,
                    addtime:new Date()
                }
            }
        },
    })
</script>

123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899
```

显示：
![vue book management system add modify](https://img-blog.csdnimg.cn/20200406094823141.gif#pic_center)
显然，此时再添加可以正常添加，并且页面也没有再刷新。
增加删除功能进行测试：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css">
    <title>图书管理系统</title>
</head>

<body>
    <div id='app'>
        <div class="container">
            <h1>图书管理系统</h1>
            <form class="navbar-form navbar-left" role="search">
                <div class="form-group">
                    <label for="">名称：</label>
                    <input type="text" class="form-control" v-model="adding_book.name" placeholder="请输入图书名字">
                </div>
                <div class="form-group">
                    <label for="">作者：</label>
                    <input type="text" class="form-control" v-model="adding_book.author" placeholder="请输入图书作者">
                </div>
                <div class="form-group">
                    <label for="">价格：</label>
                    <input type="text" class="form-control" v-model="adding_book.price" placeholder="请输入图书价格">
                </div>
                <button type="submit" class="btn btn-default" @click.prevent="add">添加</button>
            </form>
            <table class="table">
                <tr>
                    <th>序号</th>
                    <th>名称</th>
                    <th>作者</th>
                    <th>价格</th>
                    <th>时间</th>
                    <th>操作</th>
                </tr>
                <tr v-for="(book,index) in books">
                    <td>{{index + 1}}</td>
                    <td>{{book.name}}</td>
                    <td>{{book.author}}</td>
                    <td>{{book.price}}</td>
                    <td>{{book.addtime}}</td>
                    <td><button class="btn btn-danger" @click="del(index)">删除</button></td>
                </tr>
            </table>
        </div>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el: '#app',
        data: {
            books: [{
                    name: 'Python',
                    author: 'AUPY',
                    price: 50,
                    addtime: new Date()
                },
                {
                    name: 'Java',
                    author: 'AUJA',
                    price: 40,
                    addtime: new Date()
                },

            ],
            adding_book:{
                name: '',
                author: '',
                price: 0,
                addtime:new Date()
            }
        },
        methods: {
            add(){
                // JSON.parse()将参数转化为JS可识别的变量，JSON.stringify()将对象转化为字符串
                var book = JSON.parse(JSON.stringify(this.adding_book))
                this.books.push(book)
                this.adding_book = {
                    name: '',
                    author: '',
                    price: 0,
                    addtime:new Date()
                }
            },
            del:function(index){
                this.books.splice(index, 1)
            }
        },
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100
```

显示：
![vue book management system delete function](https://img-blog.csdnimg.cn/20200405180714536.gif#pic_center)
增加搜索功能测试：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css">
    <title>图书管理系统</title>
</head>

<body>
    <div id='app'>
        <div class="container">
            <h1>图书管理系统</h1>
            <form class="navbar-form navbar-left" role="search">
                <div class="form-group">
                    <label for="">名称：</label>
                    <input type="text" class="form-control" v-model="adding_book.name" placeholder="请输入图书名字">
                </div>
                <div class="form-group">
                    <label for="">作者：</label>
                    <input type="text" class="form-control" v-model="adding_book.author" placeholder="请输入图书作者">
                </div>
                <div class="form-group">
                    <label for="">价格：</label>
                    <input type="text" class="form-control" v-model="adding_book.price" placeholder="请输入图书价格">
                </div>
                <button type="submit" class="btn btn-default" @click.prevent="add">添加</button>
                <div class="form-group">
                    <label for="">搜索：</label>
                    <input type="text" class="form-control" v-model="keyword" placeholder="请输入搜索内容">
                </div>
            </form>
            <table class="table">
                <tr>
                    <th>序号</th>
                    <th>名称</th>
                    <th>作者</th>
                    <th>价格</th>
                    <th>时间</th>
                    <th>操作</th>
                </tr>
                <tr v-for="(book,index) in book_result">
                    <td>{{index + 1}}</td>
                    <td>{{book.name}}</td>
                    <td>{{book.author}}</td>
                    <td>{{book.price}}</td>
                    <td>{{book.addtime}}</td>
                    <td><button class="btn btn-danger" @click="del(index)">删除</button></td>
                </tr>
            </table>
        </div>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>


<script>
    new Vue({
        el: '#app',
        data: {
            books: [{
                    name: 'Python',
                    author: 'AUPY',
                    price: 50,
                    addtime: new Date()
                },
                {
                    name: 'Java',
                    author: 'AUJA',
                    price: 40,
                    addtime: new Date()
                },

            ],
            adding_book:{
                name: '',
                author: '',
                price: 0,
                addtime:new Date()
            },
            keyword:''
        },
        methods: {
            add(){
                // JSON.parse()将参数转化为JS可识别的变量，JSON.stringify()将对象转化为字符串
                var book = JSON.parse(JSON.stringify(this.adding_book))
                this.books.push(book)
                this.adding_book = {
                    name: '',
                    author: '',
                    price: 0,
                    addtime:new Date()
                }
            },
            del:function(index){
                this.books.splice(index, 1)
            }
        },
        computed:{
            book_result(){
                kw = this.keyword
                // const that = this
                if(this.keyword){
                    // filter()是数组中的过滤方法，item代表books中的每一条数据
                    var newbooks = this.books.filter(function(item){
                        if(item.name.indexOf(kw) >= 0){
                        // if(item.name.indexOf(that.keyword) >= 0){
                            // 返回true代表返回当前的item数据
                            return true
                        }
                        else{
                            // 返回false代表不返回当前的item数据
                            return false
                        }
                    })
                    return newbooks
                }
                else{
                    return this.books
                }
                
            }
        }
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130
```

显示：
![vue book management system search function](https://img-blog.csdnimg.cn/20200405180734660.gif#pic_center)
其中：
`filter()`函数是用来过滤数组的，对数组中的每一项进行判断，如果返回true则返回当前元素，否则不返回；
由于`filter()`函数内部独立于外部，因此不能通过**this**来引用Vue中data的数据，有两种解决的办法：

- 在`filter()`外部给keyword重新赋值，并在内部引用

```javascript
book_result(){
    kw = this.keyword
    if(this.keyword){
        // filter()是数组中的过滤方法，item代表books中的每一条数据
        var newbooks = this.books.filter(function(item){
            if(item.name.indexOf(kw) >= 0){
                // 返回true代表返回当前的item数据
                return true
            }
            else{
                // 返回false代表不返回当前的item数据
                return false
            }
        })
        return newbooks
    }
    else{
        return this.books
    }
                
}
123456789101112131415161718192021
```

- 在`filter()`外部重命名**this**关键字并在内部调用keyword属性

```javascript
book_result(){
    const that = this
    if(this.keyword){
        // filter()是数组中的过滤方法，item代表books中的每一条数据
        var newbooks = this.books.filter(function(item){
            if(item.name.indexOf(that.keyword) >= 0){
                // 返回true代表返回当前的item数据
                return true
            }
            else{
                // 返回false代表不返回当前的item数据
                return false
            }
        })
        return newbooks
    }
    else{
        return this.books
    }
                
}
123456789101112131415161718192021
```

## 三、Router路由基本使用

### 1.Vue路由基本概念

在网页中，经常需要发生页面更新或者跳转，这时候我们就可以使用Vue-Router来帮我们实现。
**Vue-Router**是用来将一个Vue程序的多个页面进行路由的，比如一个Vue程序（或者说一个网站）有登录、注册、首页等模块，那么我们就可以定义 **/login**、**/register**、**/** 来映射每个模块。
Vue-Router是定义了url规则与具体的View映射的关系，可以在一个单页面中实现数据的更新。

安装方式也有3种：

- 使用CDN

  - 加载最新版

  ```javascript
  <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
  1
  ```

  - 加载指定版本

  ```javascript
  <script src="https://unpkg.com/vue-router@3.0.7/dist/vue-router.js"></script>
  1
  ```

- 下载到本地

```javascript
<script src="../../lib/vue-router.js"></script>
1
```

- 使用npm安装

```bash
npm install vue-router
1
```

### 2.基本使用

进行测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <router-link to='/'>首页</router-link>
        <!-- 路由匹配到的组件将渲染到router-view中 -->
        <router-view></router-view>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<script>
    var index = Vue.extend({
        template:'<h1>这是首页</h1>'
    })


    var router = new VueRouter({
        routes:[
            {path:'/',component:index}
        ]
    })

    new Vue({
        el:'#app',
        data:{

        },
        router:router
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738394041
```

显示：
![vue router mainpage](https://img-blog.csdnimg.cn/20200405180822645.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
说明：

- 在vue-router中，使用`<router-link>`来加载链接，用to指定跳转的链接。vue-router最终会把`<router-link>`渲染成`<a>`标签。
- `<router-view>`是路由的出口，也就是相应url下的代码会被渲染到这个地方来。
- `Vue.extend`是用来加载模板的。
- Vue-Router创建一个路由对象。
- routes定义一个url与组件的映射，这个就是路由。

用component和extend都可以创建组件，但是有一定区别：

- component创建的组件有名字；
- extend创建的组件没有名字。

多页之间动态切换测试：

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <router-link to='/'>首页</router-link>
        <router-link to='/music'>音乐频道</router-link>
        <router-view></router-view>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<script>
    var index = Vue.extend({
        template:'<h1>这是首页</h1>'
    })
    var music = Vue.extend({
        template:'<h1>这是音乐频道</h1>'
    })


    var router = new VueRouter({
        routes:[
            {path:'/',component:index},
            {path:'/music',component:music},
        ]
    })

    new Vue({
        el:'#app',
        data:{

        },
        router:router
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142434445
```

显示：

![vue router multipage change](https://img-blog.csdnimg.cn/20200405180906910.gif#pic_center)

## 四、动态路由和组件复用

### 1.动态路由

在路由中有一些参数是会变化的，比如查看某个用户的个人中心，那肯定需要在url中加载这个人的id，这时候就需要用到动态路由了。
测试：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <router-link to='/'>首页</router-link>
        <router-link to='/profile/123'>个人中心</router-link>
        <router-view></router-view>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<script>
    var index = Vue.extend({
        template:'<h1>首页</h1>'
    })
    var profile = Vue.extend({
        template:'<h1>{{$route.params.userid}}个人中心</h1>'
    })


    var router = new VueRouter({
        routes:[
            {path:'/',component:index},
            {path:'/profile/:userid',component:profile},
        ]
    })

    new Vue({
        el:'#app',
        data:{

        },
        router:router
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142434445
```

显示：
![vue router dynamic](https://img-blog.csdnimg.cn/20200405181106521.gif#pic_center)
说明：

- :userid表示动态参数；
- `this.$route.params`中记录了路由中的参数

可以查看 **$route**和 **$router**属性：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <router-link to='/'>首页</router-link>
        <router-link to='/profile/123'>个人中心</router-link>
        <router-view></router-view>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<script>
    var index = Vue.extend({
        template:'<h1>首页</h1>'
    })
    var profile = Vue.extend({
        template:'<h1>{{$route.params.userid}}个人中心</h1>',
        mounted(){
            console.log(this.$route)
            console.log(this.$router)
        }
    })


    var router = new VueRouter({
        routes:[
            {path:'/',component:index},
            {path:'/profile/:userid',component:profile},
        ],
        
    })

    new Vue({
        el:'#app',
        data:{

        },
        router:router
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
```

显示：
![vue router route router params](https://img-blog.csdnimg.cn/20200405181137440.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
显然，都是关于渲染路由的相关信息，都含有userid参数，但是router的数据显然比route更多。

### 2.组件复用

当使用路由参数时，例如从`/user/foo`导航到`/user/bar`，原来的组件实例会被复用。因为两个路由都渲染同一个组件，比起销毁再创建，复用则显得更加高效。不过，这也意味着组件的**生命周期钩子不会再被调用**。

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
    <div id="app">
        <router-link to='/'>首页</router-link>
        <router-link to='/profile/123'>个人中心</router-link>
        <router-link to='/profile/4546'>个人中心</router-link>
        <router-view></router-view>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<script>
    var index = Vue.extend({
        template:'<h1>首页</h1>'
    })
    var profile = Vue.extend({
        template:'<h1>{{$route.params.userid}}个人中心</h1>',
        mounted(){
            console.log(this.$route)
            console.log(this.$router)
        }
    })


    var router = new VueRouter({
        routes:[
            {path:'/',component:index},
            {path:'/profile/:userid',component:profile},
        ],
        
    })

    new Vue({
        el:'#app',
        data:{

        },
        router:router
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051
```

显示：
![vue router component reuse](https://img-blog.csdnimg.cn/20200405181201853.gif#pic_center)
显然，在两个个人中心之间切换时，没有再调用生命周期函数，直接复用之前已经渲染出来的Vue组件和元素。
复用组件时，想要对路由参数的变化作出响应即重新渲染，有两种方式：

- 监听变化对象

```javascript
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // 对路由变化作出响应...
    }
  }
}
12345678
```

- 使用导航守卫

```javascript
const User = {
  template: '...',
  beforeRouteUpdate (to, from, next) {
    // react to route changes...
    // don't forget to call next()
  }
}
```

# Python全栈（六）项目前导之11.Vue-Router的使用

## 一、匹配404错误

在路由规则中， ***** 匹配任意字符，所以只要在路由中添加一个 ***路由**，那么以后没有匹配到的url都会被导入到这个视图中，可以用来匹配未找到的地址，即**404**。
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
    <div id="app">
        <router-link to='/'>首页</router-link>
        <router-link to='/profile/123'>个人中心</router-link>
        <router-view></router-view>
    </div>
</body>
</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<script>
    var index = Vue.extend({
        template:'<h1>这是首页</h1>'
    })
    var profile = Vue.extend({
        template:'<h1>这是{{$route.params.userid}}个人中心</h1>',
        mounted(){
            if(this.$route.params.userid  != '123'){
                this.$router.replace('/404')
            }
        }
    })
    var notfound = Vue.extend({
        template:'<h1>您找的页面已经到火星啦！</h1>'
    })


    var router = new VueRouter({
        routes:[
            {path:'/',component:index},
            {path:'/profile/:userid',component:profile},
            // 匹配未找到的userid
            {path:'/404',component:notfound},
            // 匹配未定义的地址
            {path:'*',component:notfound},            
        ]
    })

    new Vue({
        el:'#app',
        data:{

        },
        router:router
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657
```

显示：
![vue router 404](https://img-blog.csdnimg.cn/20200406164128835.gif#pic_center)
可以匹配两类页面未找到的情况：

- 由于url错误造成的路径找不到的错误
  这一类直接在路径中添加通配符匹配。
- 由于参数错误找不到对应的数据比如用户id造成的
  这一类需要在模板中判断。

注意：
在增加匹配404时必须将 ***** 放在最后一个路径中，因为前面的都已经匹配过而未匹配到时，说明要访问的链接不在可以访问的链接范围内，此时需要来匹配404，即通配符匹配。

## 二、嵌套路由

有时候在路由中，主要的部分是相同的，但是下面的具体内容可能是不同的。
比如访问用户的个人中心是`/user/111/profile/`，查看用户发的贴子是`/user/111/posts/`等，这时候就需要用到嵌套路由。

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue路由</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css"
        integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
</head>

<body>
    <div id="app">
        <nav class="navbar navbar-default">
            <div class="container-fluid">
                <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
                    <ul class="nav navbar-nav">
                        <li class="active">
                            <router-link to='/'>首页</router-link>
                        </li>
                        <li>
                            <router-link to='/user/123'>个人中心</router-link>
                        </li>
                    </ul>
                </div><!-- /.navbar-collapse -->
            </div><!-- /.container-fluid -->
        </nav>
        <div class="container">
            <router-view></router-view>
        </div>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<script>
    var index = Vue.extend({
        template: '<h1>这是首页</h1>',
    })
    var user = Vue.extend({
        template: `
        <div>
            <h1>这是个人中心</h1>
            <ul class="nav nav-pills">
                <li role="presentation" class="active">
                    <router-link to='/user/123/notice'>公告</router-link>
                </li>
                <li role="presentation" class="active">
                    <router-link to='/user/123/comment'>评论</router-link>
                </li>
                <li role="presentation" class="active">
                    <router-link to='/user/123/follow'>关注</router-link>
                </li>
            </ul>
            <div class="container">
                <router-view></router-view>
            </div>
        </div>
        `,
    })
    var notice = Vue.extend({
        template: `
        <div>
            <h3>公告1</h3>
            <h3>公告2</h3>
        </div>
        `
    })
    var comment = Vue.extend({
        template: `
        <div>
            <h3>评论1</h3>
            <h3>评论2</h3>
        </div>        
        `
    })
    var follow = Vue.extend({
        template: `
        <div>
            <h3>您关注的</h3>
            <h3>关注您的</h3>
        </div>        
        `
    })
    var router = new VueRouter({
        routes: [{
                path: '/',
                component: index
            },
            {
                path: '/user/:userid',
                component: user,
                children:[
                    // 设置默认显示通知
                    {path:'',component:notice},
                    {path:'notice',component:notice},
                    {path:'comment',component:comment},
                    {path:'follow',component:follow},
                ]
            },
        ]
    })

    new Vue({
        el: '#app',
        data: {

        },
        router: router
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116
```

显示：
![vue router nesting](https://img-blog.csdnimg.cn/20200406164202808.gif#pic_center)
可以看到：
如果在路由下面再定义路由，需要在children属性中添加，同时需要在该路由对应的模板中构建**link和view**。

## 三、编程式导航

之前我们都是使用`<router-link>`，用户点击时页面进行更新，但有时候我们需要在JavaScript中手动修改页面的跳转，这时候就要用到编程式导航。
即有两种导航路由的方式：

- 声明式
  即之前一直用的方式

  ```markup
  <router-link to='...'>XXX</router-link>
  1
  ```

- 编程式

  ```javascript
  router.push(...)
  1
  ```

**$router.push**跳转的原理：
用`router.push`方法导航到不同的URL，这个方法会向**history栈**添加一个新的记录，当用户点击浏览器后退按钮时，会回到之前的URL；
当点击`<router-link>`时，`router.push`方法会在内部调用，也就是说，点击`<router-link :to="...">`等同于调用`router.push(...)`。
`router.push`方法常用的方式有以下几种：

- 字符串

```javascript
router.push('home')
1
```

- 对象

```javascript
router.push({ path: 'home' })
1
```

- 命名的路由

```javascript
router.push({ name: 'user', params: { userId: '123' }})
1
```

- 带查询参数

```javascript
router.push({ path: 'register', query: { plan: 'private' }})
1
```

通过JS指定路由还有一种方式`router.replace(location, onComplete?, onAbort?)`，它跟`router.push`很像，区别是它不会向**history**添加新记录，而是**替换掉**当前的**history**记录。

`router.go(n)`方法的参数是一个整数，意思是在history记录中向前或者后退多少步，类似于`window.history.go(n)`。

```javascript
// 在浏览器记录中前进1步，等同于history.forward()
router.go(1)

// 后退1步记录，等同于history.back()
router.go(-1)

// 前进3步记录
router.go(3)

// 如果参数数值过大、history记录不够用，会前进或后退失败
router.go(-100)
router.go(100)
123456789101112
```

测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue路由</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css"
        integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
</head>

<body>
    <div id="app">
        <button @click="toPost">列表</button>
        <button @click="search">搜索</button>
        <button @click="toProfile">个人中心</button>
        <button @click="toNext">前进</button>
        <button @click="toPrevious">后退</button>
        <router-view></router-view>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<script>
    var post = Vue.extend({
        template:'<h1>这是列表</h1>'
    })
    var search = Vue.extend({
        template:'<h1>这是搜索结果</h1>'
    })
    var profile = Vue.extend({
        template:'<h1>这是{{$route.params.userid}}个人中心</h1>'
    })

    var router = new VueRouter({
        routes:[
            {path:'/post',component:post},
            {path:'/search',component:search},
            {path:'/profile/:userid',component:profile,name:'myprofile'},
        ]
    })

    new Vue({
        el: '#app',
        data: {

        },
        router: router,
        methods:{
            toPost:function(){
                this.$router.push('/post')
            },
            toProfile:function(){
                // this.$router.push('/profile/123')
                this.$router.push({name:'myprofile',params:{userid:123}})
            },
            search(){
                this.$router.push({path:'search',query:{kw:'vue'}})
            },
            toNext(){
                this.$router.go(1)
            },
            toPrevious(){
                this.$router.go(-1)
            },
        }
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768697071727374
```

显示：
![vue router programming navigation](https://img-blog.csdnimg.cn/20200406164248458.gif#pic_center)
说明：
（1）通过push进行路由导航时，传递参数有2种常见形式`profile/123`和`search?kw=vue`，分别对应一下两种传参方式：

- 对于

  ```
  profile/123
  ```

  型的参数

  有两种方式传递这种类型的参数：

  - 直接在路径中定义
    例如：`this.$router.push('/profile/123')`
  - 通过**params**传递
    例如：`this.$router.push({name:'myprofile',params:{userid:123}})`，如果使用这种方式，需要在定义路由映射时给该路由命名。

- 对于`search?kw=vue`型的参数：
  通过**query**参数传递，例如：

```javascript
this.$router.push({path:'search',query:{kw:'vue'}})
1
```

（2）在`router.push()`的参数中提供了path时，params参数会被忽略，但是query不会被忽略；如果想要使用params参数，需要提供路由的name或手写完整的带有参数的path，这与（1）中一致。
例如：`router.push({ path: '/user', params: { 123 }})`的参数是无效的，路由指向`/user`，而`router.push({ name: 'user', params: { 123}})`和`router.push({ path: '/user/123' })`的参数是有效的，都指向`/user/123`。

## 四、命名路由和视图

### 1.命名路由

有时候，通过一个名称来标识一个路由显得更方便一些，特别是在链接一个路由，或者是执行一些跳转的时候。
可以在创建Router实例的时候，在routes配置中给某个路由设置名称。
要链接到一个命名路由，可以给**router-link**的**to属性**传一个对象，例如：

```html
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
1
```

这与`router.push({ name: 'user', params: { userId: 123 }})`是等效的。
测试：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue路由</title>
</head>

<body>
    <div id="app">
        <router-link :to="{name:'index'}">Home</router-link>
        <router-view></router-view>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<script>
    var home = Vue.extend({
        template:'<h1>这是首页</h1>'
    })
    
    var router = new VueRouter({
        routes:[
            {path:'/',component:home,name:'index'},
        ]
    })

    new Vue({
        el: '#app',
        data: {

        },
        router: router,
        methods:{
        }
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738394041424344
```

显示：
![vue router rename route](https://img-blog.csdnimg.cn/20200406164320825.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
还可以传入参数：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue路由</title>
</head>

<body>
    <div id="app">
        <router-link :to="{name:'index',params:{userid:123}}">Home</router-link>
        <router-view></router-view>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<script>
    var home = Vue.extend({
        template:'<h1>这是首页</h1>'
    })
    
    var router = new VueRouter({
        routes:[
            {path:'/user/:userid',component:home,name:'index'},
        ]
    })

    new Vue({
        el: '#app',
        data: {

        },
        router: router,
        methods:{
        }
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738394041424344
```

显示：
![vue router rename route params](https://img-blog.csdnimg.cn/20200406164352267.gif#pic_center)

### 2.命名视图

有时候想同时 (同级) 展示多个视图，而不是嵌套展示，例如创建一个布局，有sidebar(侧导航) 和main(主内容) 两个视图，这个时候命名视图就派上用场了。
可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。
如果router-view没有设置名字，那么默认为default。
一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件，并在**components**中实现name与组件的映射。
进行测试：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
    <title>Vue路由</title>
</head>

<body>
    <div id="app">
        <div class="header panel-heading">
            <router-view name="header"></router-view>
        </div>
        <div class="body panel-body">
            <router-view name="left" class="col-md-6"></router-view>
            <router-view name="right" class="col-md-6"></router-view>
        </div>
        <div class="footer panel-footer">
            <router-view name="footer"></router-view>
        </div>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<script>
    var headerc = Vue.extend({
        template: '<h1>顶部</h1>'
    })
    var leftc = Vue.extend({
        template: '<h1>左边</h1>'
    })
    var rightc = Vue.extend({
        template: '<h1>右边</h1>'
    })
    var footerc = Vue.extend({
        template: '<h1>底部</h1>'
    })

    var router = new VueRouter({
        routes: [{
            path: '/',
            components: {
                header:headerc,
                left:leftc,
                right:rightc,
                footer:footerc
            },
            name: 'index'
        }, ]
    })

    new Vue({
        el: '#app',
        data: {

        },
        router: router,
        methods: {}
    })
</script>
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768
```

显示：
![vue router rename view](https://img-blog.csdnimg.cn/20200406164441735.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

## 五、重定向和别名

重定向是通过routes配置的**redirect参数**来完成的。
测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
    <title>Vue路由</title>
</head>

<body>
    <div id="app">
        <router-view></router-view>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<script>
    var login = Vue.extend({
        template:'<h1>请登录</h1>'
    })

    var router = new VueRouter({
        routes: [
            {path:'/',redirect:'/login'},
            {path:'/login',component:login},
            
            ]
    })

    new Vue({
        el: '#app',
        data: {

        },
        router: router,
        methods: {}
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142434445
```

显示：
![vue router redirect](https://img-blog.csdnimg.cn/20200406164616907.gif#pic_center)
同时，重定向的目标也可以是一个**命名的路由**，如下：

```markup
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
    <title>Vue路由</title>
</head>

<body>
    <div id="app">
        <router-view></router-view>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<script>
    var login = Vue.extend({
        template:'<h1>请登录</h1>'
    })

    var router = new VueRouter({
        routes: [
            {path:'/',redirect:{name:'relogin'}},
            {path:'/login',component:login,name:'relogin'},
            
            ]
    })

    new Vue({
        el: '#app',
        data: {

        },
        router: router,
        methods: {}
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142434445
```

测试效果与之前相同。
还可以**命别名**，测试如下：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
    <title>Vue路由</title>
</head>

<body>
    <div id="app">
        <router-view></router-view>
    </div>
</body>

</html>


<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<script>
    var login = Vue.extend({
        template:'<h1>请登录</h1>'
    })

    var router = new VueRouter({
        routes: [
            {path:'/',redirect:'/login'},
            {path:'/login',component:login,alias:'/signin'},
            
            ]
    })

    new Vue({
        el: '#app',
        data: {

        },
        router: router,
        methods: {}
    })
</script>
123456789101112131415161718192021222324252627282930313233343536373839404142434445
```

显示：
![vue router redirect rename](https://img-blog.csdnimg.cn/20200406164651902.gif#pic_center)
别名说明：
`/login`的别名是`/signin`，意味着，当用户访问`/signin`时，URL会保持为`/signin`，但是路由匹配则为`/login`，就像用户访问`/login`一样。

更多关于**Vue-Router**的内容可参考官方文档https://router.vuejs.org/zh/。

# Python全栈（六）项目前导之12.Vue-Cli的使用

## 一、nvm和npm的安装

### 1.nvm的安装

#### NVM相关概念

**nvm（Node Version Manager）** 是一个用来管理node版本的工具。
我们之所以需要使用node，是因为我们需要使用node中的**npm(Node Package Manager)**，使用npm的目的是为了能够方便的管理一些前端开发的包。

#### NVM的安装和配置

可以点击https://download.csdn.net/download/CUFEECR/12308708或https://github.com/coreybutler/nvm-windows/releases下载nvm的安装包。
下载安装之后还需要配置环境变量，配置过程如下：
![nvm 配置环境变量](https://img-blog.csdnimg.cn/20200406175856276.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
打开cmd输入nvm，如果显示类似以下界面：
![cmder nvm](https://img-blog.csdnimg.cn/20200406180006877.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70)
说明已经安装成功。
Mac或者Linux安装nvm可以参考https://github.com/creationix/nvm。

#### nvm中常用命令

nvm的常见命令包括：

- `nvm install [version]`
  安装指定版本的node.js。
- `nvm use [version]`
  使用某个版本的node。
- `nvm list`
  列出当前安装了哪些版本的node。
- `nvm uninstall [version]`
  卸载指定版本的node。
- `nvm node_mirror [url]`
  设置nvm的镜像。
- `nvm npm_mirror [url]`
  设置npm的镜像。

#### node安装

安装完nvm后，我们就可以通过nvm来安装node了。这里以10.16.0版本为例介绍安装**node.js**。
命令如下：

```bash
nvm install 10.16.0
1
```

我第一次安装的时候命令行如下：
![nvm 安装 等待](https://img-blog.csdnimg.cn/20200406180512570.png#pic_center)
这个界面显示了很久，网络也很正常，这个时候一般是服务器下载源的问题。因为node的服务器地址是https://nodejs.org/dist/，这个域名的服务器是在国外，因此会比较慢，甚至可能会发生超时。此时需要通过以下命令设置nvm的下载源：

```bash
nvm node_mirror https://npm.taobao.org/mirrors/node/
nvm npm_mirror https://npm.taobao.org/mirrors/npm/
12
```

之后再执行node的安装命令，即可秒下秒安装了，如果看到以下的输出结果，及说明安装成功：
![nvm install node success](https://img-blog.csdnimg.cn/20200406181245765.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

### 2.npm的安装和使用

npm在安装node的时候就会**自动安装**，前提条件是你需要设置当前的node的版本10.16.0，如下：
![nvm use 10.16.0](https://img-blog.csdnimg.cn/20200406181544723.png#pic_center)
然后就可以使用npm了。

#### 初始化：

在新的项目中，需要先执行`npm init`初始化，如下：

```bash
λ npm init                                                                                                                                  
This utility will walk you through creating a package.json file.                                                                            
It only covers the most common items, and tries to guess sensible defaults.                                                                 
                                                                                                                                            
See `npm help json` for definitive documentation on these fields                                                                            
and exactly what they do.                                                                                                                   
                                                                                                                                            
Use `npm install <pkg>` afterwards to install a package and                                                                                 
save it as a dependency in the package.json file.                                                                                           
                                                                                                                                            
Press ^C at any time to quit.                                                                                                               
package name: (test) demo                                                                                                                   
version: (1.0.0)                                                                                                                            
description:                                                                                                                                
entry point: (index.js)                                                                                                                     
test command:                                                                                                                               
git repository:                                                                                                                             
keywords:                                                                                                                                   
author:                                                                                                                                     
license: (ISC)                                                                                                                              
About to write to E:\Test\package.json:                                                                                                     
                                                                                                                                            
{                                                                                                                                           
  "name": "demo",                                                                                                                           
  "version": "1.0.0",                                                                                                                       
  "description": "",                                                                                                                        
  "main": "index.js",                                                                                                                       
  "scripts": {                                                                                                                              
    "test": "echo \"Error: no test specified\" && exit 1"                                                                                   
  },                                                                                                                                        
  "author": "",                                                                                                                             
  "license": "ISC"                                                                                                                          
}                                                                                                                                           
                                                                                                                                            
                                                                                                                                            
Is this OK? (yes)                                                                                                                           
                                                                                                                                            
12345678910111213141516171819202122232425262728293031323334353637
```

![npm init](https://img-blog.csdnimg.cn/20200406181821785.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
除了包名**demo**外，其他均可回车选择默认选项。
执行之后就会在项目目录中创建一个**package.json**文件，用于保存本项目中用到的包。

#### 安装包：

安装包分为**全局安装**和**本地安装**：

- 全局安装是安装在当前node环境中，在可以在cmd中当作命令使用。
- 本地安装是安装在当前项目中，只有当前这个项目能使用，并且可以通过require引用。

两种安装方式主要是有无 **-g参数** 的区别。

**（1）本地安装**
本地安装的命令有3种：

```bash
npm install vue   # 本地安装
npm install vue --save   # 本地安装，并且保存到package.json的dependice中
npm install vue --save-dev # 本地安装，并且保存到package.json的dependice-dev中
123
```

将安装包放在 **./node_modules** 下（运行npm命令时所在的目录），如果没有node_modules目录，会在当前执行npm命令的目录下生成**node_modules**目录。
可以通过`require()`来引入本地安装的包。
本地安装示例如下：

```bash
λ npm install vue --save
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN demo@1.0.0 No description
npm WARN demo@1.0.0 No repository field.

+ vue@2.6.11
added 1 package from 1 contributor and audited 1 package in 8.865s
found 0 vulnerabilities
12345678
```

此时生成了两个新文件如下：
![npm install vue --save](https://img-blog.csdnimg.cn/20200406182550363.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
**（2）全局安装**
全局安装的命令有两种：

```bash
npm install vue -g   #全局安装
npm install -g @vue/cli  #全局安装vue-cli
12
```

将安装包放在`/usr/local`下或者**node的安装目录**。
可以**直接在命令行里使用**。

#### 包管理

- 卸载包：

```bash
npm uninstall [package]
1
```

- 更新包：

```bash
npm update [package]
1
```

- 搜索包：

```bash
npm search [package]
1
```

#### 镜像使用cnpm

由于npm的服务器在国外，所以安装的时候可能会很慢，这时可以安装**cnpm**，并且指定镜像为淘宝的镜像：

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
1
```

此时就可以执行**cnpm**命令了，如下：

```bash
λ cnpm
Usage: cnpm [option] <command>
Help: http://cnpmjs.org/help/cnpm

  Extend command
    web                            open cnpm web (ex.: cnpm web)
    check [ingoreupdate]           check project dependencies, add ignoreupdate will not check modules' latest version(ex.: cnpm check, cnpm check -i)
    doc [moduleName]               open document page (ex.: cnpm doc urllib)
    sync [moduleName]              sync module from source npm (ex.: cnpm sync urllib)
    user [username]                open user profile page (ex.: cnpm user fengmk2)

  npm command use --registry=https://r.npm.taobao.org
    where <command> is one of:
    add-user, adduser, apihelp, author, bin, bugs, c, cache,
    completion, config, ddp, dedupe, deprecate, docs, edit,
    explore, faq, find, find-dupes, get, help, help-search,
    home, i, info, init, install, isntall, la, link, list, ll,
    ln, login, ls, outdated, owner, pack, prefix, prune,
    publish, r, rb, rebuild, remove, restart, rm, root,
    run-script, s, se, search, set, show, shrinkwrap, star,
    start, stop, submodule, tag, test, tst, un, uninstall,
    unlink, unpublish, unstar, up, update, v, version, view,
    whoami
      npm <cmd> -h     quick help on <cmd>
      npm -l           display full usage info
      npm faq          commonly asked questions
      npm help <term>  search for help on <term>
      npm help npm     involved overview

      Specify configs in the ini-formatted file:
          C:\Users\Lenovo\.cnpmrc
      or on the command line via: npm <command> --key value
      Config info can be viewed via: npm help config

12345678910111213141516171819202122232425262728293031323334
```

以后就可以使用cnpm来安装包了。
用**cnpm**演示**全局安装**示意如下：

```bash
λ cnpm install -g @vue/cli                                                                                                                  
Downloading @vue/cli to E:\NVM\nodejs\node_modules\@vue\cli_tmp                                                                             
Copying E:\NVM\nodejs\node_modules\@vue\cli_tmp\_@vue_cli@4.3.0@@vue\cli to E:\NVM\nodejs\node_modules\@vue\cli                             
Installing @vue/cli's dependencies to E:\NVM\nodejs\node_modules\@vue\cli/node_modules                                                      
[1/32] deepmerge@^4.2.2 installed at node_modules\_deepmerge@4.2.2@deepmerge                                                                
[2/32] commander@^2.20.0 installed at node_modules\_commander@2.20.3@commander                                                              
[3/32] @vue/cli-ui-addon-webpack@^4.3.0 installed at node_modules\_@vue_cli-ui-addon-webpack@4.3.0@@vue\cli-ui-addon-webpack                
[4/32] @vue/cli-ui-addon-widgets@^4.3.0 installed at node_modules\_@vue_cli-ui-addon-widgets@4.3.0@@vue\cli-ui-addon-widgets                
[5/32] debug@^4.1.0 installed at node_modules\_debug@4.1.1@debug                                                                            
[6/32] ejs@^2.7.1 installed at node_modules\_ejs@2.7.4@ejs                                                                                  
[7/32] envinfo@^7.5.0 installed at node_modules\_envinfo@7.5.0@envinfo                                                                      
[8/32] cmd-shim@^3.0.3 installed at node_modules\_cmd-shim@3.0.3@cmd-shim                                                                   
[9/32] isbinaryfile@^4.0.5 installed at node_modules\_isbinaryfile@4.0.6@isbinaryfile                                                       
[10/32] fs-extra@^7.0.1 installed at node_modules\_fs-extra@7.0.1@fs-extra                                                                  
[11/32] boxen@^4.1.0 installed at node_modules\_boxen@4.2.0@boxen                                                                           
[12/32] javascript-stringify@^1.6.0 installed at node_modules\_javascript-stringify@1.6.0@javascript-stringify                              
[13/32] import-global@^0.1.0 installed at node_modules\_import-global@0.1.0@import-global                                                   
[14/32] lru-cache@^5.1.1 existed at node_modules\_lru-cache@5.1.1@lru-cache                                                                 
[15/32] minimist@^1.2.5 existed at node_modules\_minimist@1.2.5@minimist                                                                    
[16/32] leven@^3.1.0 installed at node_modules\_leven@3.1.0@leven                                                                           
[17/32] resolve@^1.15.1 existed at node_modules\_resolve@1.15.1@resolve                                                                     
[18/32] lodash.clonedeep@^4.5.0 installed at node_modules\_lodash.clonedeep@4.5.0@lodash.clonedeep                                          
[19/32] slash@^3.0.0 installed at node_modules\_slash@3.0.0@slash                                                                           
[20/32] @vue/cli-shared-utils@^4.3.0 installed at node_modules\_@vue_cli-shared-utils@4.3.0@@vue\cli-shared-utils                           
[21/32] js-yaml@^3.13.1 installed at node_modules\_js-yaml@3.13.1@js-yaml                                                                   
[22/32] shortid@^2.2.15 installed at node_modules\_shortid@2.2.15@shortid                                                                   
[23/32] recast@^0.18.8 installed at node_modules\_recast@0.18.10@recast                                                                     
[24/32] validate-npm-package-name@^3.0.0 installed at node_modules\_validate-npm-package-name@3.0.0@validate-npm-package-name               
[25/32] download-git-repo@^3.0.2 installed at node_modules\_download-git-repo@3.0.2@download-git-repo                                       
[26/32] yaml-front-matter@^3.4.1 installed at node_modules\_yaml-front-matter@3.4.1@yaml-front-matter                                       
[27/32] globby@^9.2.0 installed at node_modules\_globby@9.2.0@globby                                                                        
[28/32] vue@^2.6.11 installed at node_modules\_vue@2.6.11@vue                                                                               
[29/32] vue-jscodeshift-adapter@^2.0.2 installed at node_modules\_vue-jscodeshift-adapter@2.0.3@vue-jscodeshift-adapter                     
[30/32] jscodeshift@^0.7.0 installed at node_modules\_jscodeshift@0.7.0@jscodeshift                                                         
platform unsupported @vue/cli-ui@4.3.0 › vue-cli-plugin-apollo@0.21.3 › nodemon@1.19.4 › chokidar@2.1.8 › fsevents@^1.2.7 Package require os
(darwin) not compatible with your platform(win32)                                                                                           
[fsevents@^1.2.7] optional install error: Package require os(darwin) not compatible with your platform(win32)                               
[31/32] inquirer@^7.1.0 installed at node_modules\_inquirer@7.1.0@inquirer                                                                  
[32/32] @vue/cli-ui@^4.3.0 installed at node_modules\_@vue_cli-ui@4.3.0@@vue\cli-ui                                                         
execute post install 5 scripts...                                                                                                           
[1/5] scripts.postinstall ejs@^2.7.1 run "node ./postinstall.js", root: "E:\\NVM\\nodejs\\node_modules\\@vue\\cli\\node_modules\\_ejs@2.7.4@
ejs"                                                                                                                                        
Thank you for installing EJS: built with the Jake JavaScript build tool (https://jakejs.com/)                                               
                                                                                                                                            
[1/5] scripts.postinstall ejs@^2.7.1 finished in 242ms                                                                                      
[2/5] scripts.postinstall @vue/cli-ui@4.3.0 › apollo-server-express@2.11.0 › apollo-server-types@0.3.0 › apollo-engine-reporting-protobuf@0.
4.4 › @apollo/protobufjs@^1.0.3 run "node scripts/postinstall", root: "E:\\NVM\\nodejs\\node_modules\\@vue\\cli\\node_modules\\_@apollo_prot
obufjs@1.0.3@@apollo\\protobufjs"                                                                                                           
[2/5] scripts.postinstall @vue/cli-ui@4.3.0 › apollo-server-express@2.11.0 › apollo-server-types@0.3.0 › apollo-engine-reporting-protobuf@0.
4.4 › @apollo/protobufjs@^1.0.3 finished in 208ms                                                                                           
[3/5] scripts.postinstall @vue/cli-ui@4.3.0 › vue-cli-plugin-apollo@0.21.3 › nodemon@^1.19.4 run "node bin/postinstall || exit 0", root: "E:
\\NVM\\nodejs\\node_modules\\@vue\\cli\\node_modules\\_nodemon@1.19.4@nodemon"                                                              
Love nodemon? You can now support the project via the open collective:                                                                      
 > https://opencollective.com/nodemon/donate                                                                                                
                                                                                                                                            
[3/5] scripts.postinstall @vue/cli-ui@4.3.0 › vue-cli-plugin-apollo@0.21.3 › nodemon@^1.19.4 finished in 262ms                              
[4/5] scripts.postinstall @vue/cli-ui@4.3.0 › apollo-server-express@2.11.0 › apollo-server-core@2.11.0 › @apollographql/apollo-tools@0.4.5 ›
 apollo-env@0.6.2 › core-js@^3.0.1 run "node -e \"try{require('./postinstall')}catch(e){}\"", root: "E:\\NVM\\nodejs\\node_modules\\@vue\\cl
i\\node_modules\\_core-js@3.6.4@core-js"                                                                                                    
Thank you for using core-js ( https://github.com/zloirock/core-js ) for polyfilling JavaScript standard library!                            
                                                                                                                                            
The project needs your help! Please consider supporting of core-js on Open Collective or Patreon:                                           
> https://opencollective.com/core-js                                                                                                        
> https://www.patreon.com/zloirock                                                                                                          
                                                                                                                                            
Also, the author of core-js ( https://github.com/zloirock ) is looking for a good job -)                                                    
                                                                                                                                            
[4/5] scripts.postinstall @vue/cli-ui@4.3.0 › apollo-server-express@2.11.0 › apollo-server-core@2.11.0 › @apollographql/apollo-tools@0.4.5 ›
 apollo-env@0.6.2 › core-js@^3.0.1 finished in 184ms                                                                                        
[5/5] scripts.postinstall @vue/cli-ui@4.3.0 › vue-cli-plugin-apollo@0.21.3 › apollo@2.26.0 › git-parse@1.0.3 › babel-polyfill@6.26.0 › core-
js@^2.5.0 run "node -e \"try{require('./postinstall')}catch(e){}\"", root: "E:\\NVM\\nodejs\\node_modules\\@vue\\cli\\node_modules\\_core-js
@2.6.11@core-js"                                                                                                                            
[5/5] scripts.postinstall @vue/cli-ui@4.3.0 › vue-cli-plugin-apollo@0.21.3 › apollo@2.26.0 › git-parse@1.0.3 › babel-polyfill@6.26.0 › core-
js@^2.5.0 finished in 180ms                                                                                                                 
anti semver @vue/cli-ui@4.3.0 › apollo-server-express@2.11.0 › @types/cors@2.8.6 › @types/express@* delcares @types/express@*(resolved as 4.
17.4) but using ancestor(apollo-server-express)'s dependency @types/express@4.17.2(resolved as 4.17.2)                                      
anti semver @vue/cli-ui@4.3.0 › apollo-server-express@2.11.0 › apollo-server-core@2.11.0 › @types/graphql-upload@8.0.3 › @types/express@* de
lcares @types/express@*(resolved as 4.17.4) but using ancestor(apollo-server-express)'s dependency @types/express@4.17.2(resolved as 4.17.2)
anti semver @vue/cli-ui@4.3.0 › apollo-server-express@2.11.0 › apollo-server-core@2.11.0 › @types/graphql-upload@8.0.3 › @types/koa@2.11.3 ›
 @types/cookies@0.7.4 › @types/express@* delcares @types/express@*(resolved as 4.17.4) but using ancestor(apollo-server-express)'s dependenc
y @types/express@4.17.2(resolved as 4.17.2)                                                                                                 
deprecate @vue/cli-ui@4.3.0 › vue-cli-plugin-apollo@0.21.3 › apollo@2.26.0 › git-parse@1.0.3 › babel-polyfill@6.26.0 › core-js@^2.5.0 core-j
s@<3 is no longer maintained and not recommended for usage due to the number of issues. Please, upgrade your dependencies to the actual vers
ion of core-js@3.                                                                                                                           
Recently updated (since 2020-03-30): 33 packages (detail see file E:\NVM\nodejs\node_modules\@vue\cli\node_modules\.recently_updates.txt)   
  Today:                                                                                                                                    
    → @vue/cli-shared-utils@4.3.0 › ora@3.4.0 › cli-spinners@^2.0.0(2.3.0) (15:17:55)                                                       
    → recast@^0.18.8(0.18.10) (01:25:09)                                                                                                    
  2020-04-05                                                                                                                                
    → jscodeshift@0.7.0 › @babel/core@7.9.0 › json5@^2.1.2(2.1.3) (06:58:52)                                                                
    → jscodeshift@0.7.0 › @babel/register@7.9.0 › find-cache-dir@2.1.0 › pkg-dir@3.0.0 › find-up@3.0.0 › locate-path@3.0.0 › p-locate@3.0.0 
 › p-limit@^2.0.0(2.3.0) (23:40:45)                                                                                                         
    → @vue/cli-ui@4.3.0 › vue-cli-plugin-apollo@0.21.3 › ts-node@^8.4.1(8.8.2) (08:51:08)                                                   
  2020-04-04                                                                                                                                
    → cmd-shim@3.0.3 › mkdirp@~0.5.0(0.5.5) (01:12:26)                                                                                      
    → @vue/cli-ui@4.3.0 › fkill@6.2.0 › taskkill@3.1.0 › execa@3.4.0 › cross-spawn@^7.0.0(7.0.2) (17:56:44)                                 
    → @vue/cli-ui@4.3.0 › prismjs@^1.19.0(1.20.0) (09:04:54)                                                                                
    → jscodeshift@0.7.0 › @babel/preset-env@7.9.0 › @babel/compat-data@7.9.0 › browserslist@4.11.1 › electron-to-chromium@^1.3.390(1.3.397) 
 (20:02:29)                                                                                                                                 
    → jscodeshift@0.7.0 › @babel/preset-env@7.9.0 › @babel/compat-data@7.9.0 › browserslist@4.11.1 › caniuse-lite@^1.0.30001038(1.0.3000103 
9) (12:33:00)                                                                                                                               
    → @vue/cli-ui@4.3.0 › vue-cli-plugin-apollo@0.21.3 › apollo@2.26.0 › mkdirp@^1.0.3(1.0.4) (01:03:08)                                    
  2020-04-03                                                                                                                                
    → isbinaryfile@^4.0.5(4.0.6) (22:30:07)                                                                                                 
    → globby@9.2.0 › @types/glob@7.1.1 › @types/node@*(13.11.0) (00:51:10)                                                                  
    → inquirer@7.1.0 › rxjs@^6.5.3(6.5.5) (09:34:30)                                                                                        
  2020-04-02                                                                                                                                
    → jscodeshift@0.7.0 › flow-parser@0.*(0.122.0) (13:14:47)                                                                               
  2020-04-01                                                                                                                                
    → @vue/cli-shared-utils@^4.3.0(4.3.0) (16:01:02)                                                                                        
    → @vue/cli-ui-addon-webpack@^4.3.0(4.3.0) (16:01:03)                                                                                    
    → @vue/cli-ui-addon-widgets@^4.3.0(4.3.0) (16:01:02)                                                                                    
    → @vue/cli-ui@^4.3.0(4.3.0) (16:01:22)                                                                                                  
    → download-git-repo@3.0.2 › download@7.1.0 › decompress@^4.2.0(4.2.1) (22:00:54)                                                        
    → @vue/cli-ui@4.3.0 › apollo-server-express@2.11.0 › apollo-server-core@2.11.0 › @apollographql/apollo-tools@^0.4.3(0.4.5) (02:23:04)   
    → download-git-repo@3.0.2 › download@7.1.0 › decompress@4.2.1 › decompress-tarbz2@4.1.1 › unbzip2-stream@^1.0.9(1.4.0) (15:00:41)       
    → @vue/cli-ui@4.3.0 › apollo-server-express@2.11.0 › apollo-server-core@2.11.0 › @types/graphql-upload@8.0.3 › @types/koa@2.11.3 › @typ 
es/content-disposition@*(0.5.3) (06:24:51)                                                                                                  
    → @vue/cli-ui@4.3.0 › vue-cli-plugin-apollo@0.21.3 › apollo@^2.20.0(2.26.0) (02:23:22)                                                  
    → @vue/cli-ui@4.3.0 › vue-cli-plugin-apollo@0.21.3 › apollo@2.26.0 › apollo-codegen-flow@^0.34.5(0.34.5) (02:23:16)                     
    → @vue/cli-ui@4.3.0 › vue-cli-plugin-apollo@0.21.3 › apollo@2.26.0 › apollo-codegen-core@^0.36.5(0.36.5) (02:23:13)                     
    → @vue/cli-ui@4.3.0 › vue-cli-plugin-apollo@0.21.3 › apollo@2.26.0 › apollo-codegen-scala@^0.35.5(0.35.5) (02:23:16)                    
    → @vue/cli-ui@4.3.0 › vue-cli-plugin-apollo@0.21.3 › apollo@2.26.0 › @oclif/plugin-plugins@1.7.9 › cli-ux@5.4.5 › cli-progress@^3.4.0(3 
.6.1) (01:55:00)                                                                                                                            
    → @vue/cli-ui@4.3.0 › vue-cli-plugin-apollo@0.21.3 › apollo@2.26.0 › apollo-codegen-swift@^0.36.5(0.36.5) (02:23:16)                    
    → @vue/cli-ui@4.3.0 › vue-cli-plugin-apollo@0.21.3 › apollo@2.26.0 › apollo-codegen-typescript@^0.36.5(0.36.5) (02:23:16)               
    → @vue/cli-ui@4.3.0 › vue-cli-plugin-apollo@0.21.3 › apollo@2.26.0 › apollo-codegen-core@0.36.5 › apollo-language-server@^1.21.0(1.21.0 
) (02:23:08)                                                                                                                                
  2020-03-31                                                                                                                                
    → jscodeshift@0.7.0 › @babel/preset-env@7.9.0 › @babel/compat-data@7.9.0 › browserslist@^4.9.1(4.11.1) (04:28:11)                       
    → @vue/cli-ui@4.3.0 › apollo-server-express@2.11.0 › apollo-server-types@0.3.0 › apollo-engine-reporting-protobuf@0.4.4 › @apollo/proto 
bufjs@1.0.3 › @types/node@^10.1.0(10.17.18) (07:28:11)                                                                                      
All packages installed (1041 packages installed from npm registry, used 24s(network 22s), speed 1.76MB/s, json 905(2.3MB), tarball 35.84MB) 
[@vue/cli@4.3.0] link E:\NVM\nodejs\vue@ -> E:\NVM\nodejs\node_modules\@vue\cli\bin\vue.js                                                  
                                                                                                                                            
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135
```

安装后的文件目录结构大致如下：
![npm install vue-cli](https://img-blog.csdnimg.cn/2020040618445718.png#pic_center)

### 手动安装npm

有时候使用nvm安装完node后，npm没有自动安装，这时候可以到https://github.com/npm/cli/releases下载6.10.1的版本，然后下载完成后，解压后，放到nvm安装目录下的**v10.16.0/node_modules**下，然后修改名字为npm，并且把**npm/bin**中的**npm和npm.cmd**两个文件放到v10.16.0根目录下。

## 二、vue-cli创建项目

**vue-cli**是和vue进行深度组合的工具，可以快速帮我们创建vue项目，并且把一些脚手架相关的代码给我们创建好。
真正使用vue开发项目，都是用**vue-cli**来创建项目的。
node环境安装后，通过命令即可完成全局或本地安装。
创建项目有2种方式：

- 命令行创建
- 界面创建

用**命令行创建项目**步骤：

- 在指定路径下使用`vue create [项目名称]`命令创建项目。
- 提示选择安装哪些包（默认是Babel和ESLint），也可以手动选择。
- 回车选择默认选项即可创建成功。

说明：
如果在创建项目的时候比较慢，可以在创建的时候使用淘宝的下载源：
`vue create -r https://registry.npm.taobao.org [项目名称]`；
如果不想在每次创建项目的时候都指定淘宝链接，可以找到配置文件 **.npmrc**，然后设置`registry=https://registry.npm.taobao.org`。

用**界面创建项目**步骤：

- 打开cmd，进入到项目所在路径下。然后执行`vue ui`，就会自动打开一个浏览器界面。
- 按照指引进行选择，然后一顿下一步即可创建。

创建项目测试如下：

```bash
E:\Test  (demo@1.0.0)
λ vue create vue-project
?  Your connection to the default npm registry seems to be slow.
   Use https://registry.npm.taobao.org for faster installation? Yes


Vue CLI v4.3.0
? Please pick a preset: default (babel, eslint)


Vue CLI v4.3.0
✨  Creating project in E:\Test\vue-project.
�  Initializing git repository...
⚙️  Installing CLI plugins. This might take a while...


> yorkie@2.0.0 install E:\Test\vue-project\node_modules\yorkie
> node bin/install.js

setting up Git hooks
done


> core-js@3.6.4 postinstall E:\Test\vue-project\node_modules\core-js
> node -e "try{require('./postinstall')}catch(e){}"


> ejs@2.7.4 postinstall E:\Test\vue-project\node_modules\ejs
> node ./postinstall.js

added 1192 packages from 846 contributors in 45.553s
�  Invoking generators...
�  Installing additional dependencies...

added 54 packages from 39 contributors in 12.316s
⚓  Running completion hooks...

�  Generating README.md...

�  Successfully created project vue-project.
�  Get started with the following commands:

 $ cd vue-project
 $ npm run serve

123456789101112131415161718192021222324252627282930313233343536373839404142434445
```

创建之后的目录结构为：
![vue-cli 创建项目目录](https://img-blog.csdnimg.cn/20200407140400985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)
**文件说明**：

- node_modules
  本地安装的包的文件夹。

- public
  项目出口文件。

- src

  项目源文件，包括：

  - assets
    资源文件，包括字体，图片等。
  - components
    组件文件。
  - App.vue
    入口组件。
  - main.js
    webpack在打包的时候的入口文件。

- babel.config.js
  es*转低级js语言的配置文件，即处理版本向下兼容的文件。

- package.json
  项目包管理文件。

此时运行项目：

```bash
E:\Test  (demo@1.0.0)
λ cd vue-project\

E:\Test\vue-project (master -> origin)
λ npm run serve

> vue-project@0.1.0 serve E:\Test\vue-project
> vue-cli-service serve

 INFO  Starting development server...
98% after emitting CopyPlugin

 DONE  Compiled successfully in 4409ms                                                                                           11:46:00 AM


  App running at:
  - Local:   http://localhost:8080/
  - Network: http://192.168.0.102:8080/

  Note that the development build is not optimized.
  To create a production build, run npm run build.
123456789101112131415161718192021
```

创建成功后即可在浏览器中访问http://localhost:8080/，看到以下界面说明创建成功：
![vue-cli 初次运行效](https://img-blog.csdnimg.cn/20200407141037414.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

## 三、组件的定义和导入

- 定义：
  组件定义跟之前的方式是一模一样的，只不过现在模板代码专门放到`.vue`格式的template标签中，所以不再需要在定义组件的时候传入template参数。
  另外，因为需要让别的组件使用本组件，因此需要用`export default`将组件对象进行导出。
- 导入：
  因为组件在不同的文件中，所以如果需要引用，那么必须进行导入。用ES6语法的`import XXX from XXX`。

具体演示如下 ：
（1）在components目录中新建文件（例如MyPage.vue），进行编辑：

```html
<template>
    <div class='mypage'>
        <h1>我的VIEW</h1>
        <p class="info">{{info}}</p>
    </div>
</template>



<script>
export default{
    data:function(){
        return{
            info:'这是我的第一个组件页面'
        }
    }
}
</script>



<style>
    .info{
        background-color: rgb(0, 183, 255);
    }
</style>
1234567891011121314151617181920212223242526
```

（2）此时再编辑**App.vue**：

```html
<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <my-page></my-page>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import MyPage from './components/MyPage.vue'

export default {
  // name: 'App',
  // components: {
  //   HelloWorld
  // }
  name:'App',
  components:{
    'my-page':MyPage
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

1234567891011121314151617181920212223242526272829303132333435
```

（3）此时运行后查看网页：
![vue-cli 我的view](https://img-blog.csdnimg.cn/20200407141834828.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0NVRkVFQ1I=,size_16,color_FFFFFF,t_70#pic_center)

## 四、局部样式和Sass语法

### 1.局部样式

默认情况下在`.vue`文件中的样式一旦写了，那么会变成全局的。
如果只是想要在当前的组件中起作用，那么可以在style中加上一个**scoped属性**即可。
示例代码如下：

```markup
<style scoped>
	.info{
	   background-color: red;
}
</style>
12345
```

如果在自定义组件的样式中加scoped属性，则全局样式对该组件无效；
如果在App.vue中定义scoped属性，则其定义的属性对其他组件的样式无影响。

### 2.sass语法

很多前端开发者在写样式代码的时候，不喜欢用原生css，比较喜欢用比如sass或者less。
这里我们以sass为例，我们可以通过以下两个步骤来实现：

- 安装**loader**
  webpack在解析scss文件的时候，会加载**sass-loader**和**node-loader**，因此我们首先需要通过npm来安装：

  ```bash
  npm install node-sass@4.12.0 --save-dev
  npm install sass-loader@7.0.3 --save-dev
  12
  ```

- 指定sass语言
  在指定style的时候，添加`lang="sass"`属性，这样就会将style中的代码识别为sass语法。

# Python全栈（六）项目前导之13.项目前导小结

## 一、Redis

### 1.Redis介绍

#### 是什么

是一个C语言开发的、开源的nosql数据库。

#### 特性

- 支持数据据持久化
- 支持数据备份
- 数据类型较多

#### 应用场景

- 缓存
- 计数
- 点赞
- 在线人数

#### 安装

- Ubuntu
- Kali
- Windows

#### 配置文件

> port 6379 端口
> databases 16 数据库个数（0-15）
> save 9000 1：900秒内有一个数据变化则保存 bind
> bind 127.0.0.1：默认保存本地
> daemonize yes：后台启动

#### 五大数据类型

- string
  有set、get、mset、mget、del、strlen、append、incr、decr、incrby、decrby、setrange（替换—）、getrange（根据范围获取值）等方法。
- list
  有lpush、rpush、lrange、lindex、llen、lpop、lrem（删除指定个数相同元素）、lpop、rpop、lset、ltrim（根据范围截取并重新赋值）、linsert Before/after等方法。
- hash
  有hset、hget、hmset、hmget、ggetall、hdel、hlen、hexists、hvals/hkeys等方法。
- set无序集合
  有sadd、smembers、sismember、scard（元素个数）、srem、srandmember、spop、smove、sdiff、sunion、sinter等方法。
- zset有序集合
  有zadd、zrange、zrem、zcount、zcard、zrank、zrangebyscore等方法。

### 2.Redis和Python的交互

- 安装

```bash
pip install redis
1
```

- 连接

```python
redis.Redis(host='',port=6379,db=')
redis.StrictRedis(host='',port=6379,db=')
12
```

- 操作
  不同数据类型的方法与redis命令一致或类似。

### 3.Redis的主从配置

减少主库的写入压力。

- 主配置
  - 修改配置文件bind参数
  - 开启主机服务
- 从配置
  - 复制配置文件
  - 修改配置文件
  - 修改配置文件参数
  - 开启服务

## 二、Git

### 1.用Git管理项目文件

#### 管理项目文件步骤：

- 初始化

```bash
git init
1
```

- 添加提交

```bash
git add .
git commit -m 'xxx'
12
```

#### 项目文件状态变化：

- 红色表示新增或修改文件
- 绿色表示文件已经被Git管理起来

#### 回滚：

- 往后回滚：

```bash
git log
git reset --hard 版本号

123
```

- 往前回滚

```bash
git reflog
git reset --hard 版本号
12
```

#### 分支

- 查看分支

```bash
git branch
1
```

- 创建分支

```bash
git branch 分支名
1
```

- 切换分支

```bash
git checkout 分支名
1
```

- 创建并切换分支

```bash
git checkout -b 分支名
1
```

- 分支合并

```bash
git merge 分支名
1
```

#### 解决冲突

- 手动解决
  找到冲突的位置并修改。
- 工具解决
  **beyond compare**等。

#### 分支与版本管理

- 主分支为线上版本
- 开发分支开发功能

#### rebase的使用

整合提交记录，合并多个版本，如下：

```bash
git rebase -i [版本号/HEAD~3]
1
```

### 2.GitHub

代码托管、多人协同开发、code review等。

## 三、Vue

### 1.Vue的基本使用

- 计算属性
- 表单绑定
- 自定义组件
- 生命周期函数

### 2.Vue-Router

- 安装
- 使用

```html
<html>
    <router-link></router-link>
    <router-view></router-view>
</html>

<script>
    var router = new VueRouter({
    orutes:[
        {path:'路径',componment:'组件'}，
        // 动态路由
        {path:'路径/:userid',componment:'组件'}，
        // 匹配404错误
        {path:'*',componment:'组件'}，
        // 路由嵌套
        {
            path:'路径',
            component:"组件',
            children:[
                {path:'路径',componment:'组件'}，
                {path:'路径',componment:'组件'}，
            ]
        }

    ]
})

new Vue({
    router:router
})

</script>
12345678910111213141516171819202122232425262728293031
```

- 编程式导航

```javascript
new Vue({
    el:'#app',
    router:router,
    methods:{
        xxx:function(){
            this.$router.push('路径')
        }
    }
})
123456789
```

### 3.Vue-Cli

- nvm的安装
- npm的安装
- vue-cli创建项目
- 组件的定义和导入