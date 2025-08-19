```### 此资源由 58学课资源站 收集整理 ###
	想要获取完整课件资料 请访问：58xueke.com
	百万资源 畅享学习

```
# day13作业

> 1.如何查看系统所有环境变量，且过滤出与root相关的变量。
>
> 系统全局的，本身内置的变量+用户的变量===系统全局的变量
>
> set

![image-20220317154558442](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317154558442.png)

> 2.如何查看⽤户个⼈的环境变量，且过滤出与root相关的变量。

![image-20220317154635004](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317154635004.png)

> 3.解释下PS1变量，以及如何修改使⽤PS1。
>
> 请注意，linux是区分大小写的，`PS1`
>
> set设置变量
>
> unset可以取消变量的赋值
>
> 

```
# PS1变量是Linux系统中控制命令提示符的。
[root@localhost ~]# set | grep PS1
PS1='[\u@\h \W]\$ '
# 其中各参数含义：
\u 用户名
\h 主机名
\W 显示用户所处目录的最后一级
\w 显示用户所处的绝对路径
\t 以24小时制，显示时间
\$ 显示用户的身份提示符，自动识别root（#）还是普通用户（$）

# 修改PS1，使用赋值法重新定义PS1变量即可


```

![image-20220317160110967](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317160110967.png)

> 4.如何永久修改PS1变量⽣效？
>
> 分别演示root、和普通⽤户的配置。
>
> - 在linux命令行中，变量的临时赋值操作，是临时生效，对当前ssh会话生效，用户切换，注销登录，变量都丢失
> - 如果想永久生效，配置信息，写入到文件中（要求系统每次一登录就加载这个文件，实现永久生效）
> - 针对整个系统的，全局的配置文件（所有root用户，以及普通用户，登录这个机器，都会加载这个文件）
>   - `/etc/profile`
> - 针对用户个人的，该文件，存在于用户家目录下的，环境变量配置文件，名字是？
>   - 特点是，只有该用户，登录后，系统加载他的 /home/username家目录下的文件，方可生效
>   - `~/.bash_profile`

PS1变量临时敲打临时生效，重新登录后系统重新加载用户用户环境变量，设置丢失，无法永久生效

![image-20220317160751384](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317160751384.png)

```
1.写入到系统全局环境变量配置文件中 /etc/profile
```

![image-20220317160917527](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317160917527.png)

![image-20220317161148629](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317161148629.png)

```
2.写入到用户个人的配置文件（用户家目录下的.bash_profile文件）
```

![image-20220317162239327](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317162239327.png)

![image-20220317162414169](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317162414169.png)

> 5.按如下要求，创建⽤户，以及解决⽤户故障。

```
1.创建⽤户，⽆家⽬录 useradd jack01 -M 
2.登录⽤户jack01 [root@yuanlai-0224 ~]# su - jack01 
su: warning: cannot change directory to /home/jack01: No such file or directory 


-bash-4.2$
```

```
用户出现该故障表示系统读取不到用户的个人配置文件，在使用useradd创建用户的时候，系统会自动去/etc/skel目录下去拷贝所有用户个人环境变量配置文件到用户家目录/home/jack下。要解决该故障，将/etc/skel目录下的所有相关配置文件拷贝到用户家目录下即可
```

![image-20220317164315189](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317164315189.png)

> 6.解释suid的作⽤，以及代码演示suid如何使⽤。

```
suid就是让普通用户，执行命令时，临时获得root的身份，

即只要用户对设有 SUID 的文件有执行权限，那么当用户执行此文件时，会以文件属主的身份去执行此文件，一旦文件执行结束，身份的切换也随之消失


suid 特殊权限，
1.给一个二进制命令，添加suid权限
2. 一个普通用户，执行这个命令，就是以root身份去执行





```

![image-20220317170709958](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317170709958.png)

![image-20220317170843532](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317170843532.png)

## 关于linux文件类型的补充

```
file 去查看文件类型

file 可以判断出 text类型

file可以判断出二进制命令类型 


[jerry01@yuchao-linux01 ~]$ # 编译型语言的特点，比如100行的C语言，最终编译生成了/bin/touch指令，用户只能看到命令的二进制数据，看不到程序的源码，这是编译语言
[jerry01@yuchao-linux01 ~]$ 

[jerry01@yuchao-linux01 ~]$ # 解释型语言，你写了100行的python，bash代码，不需要用户主动编译，直接用python解释器，或者bash解释去 读取，该文件，运行该脚本文件即可，这是解释型语 言
[jerry01@yuchao-linux01 ~]$ 




```



> 
>
> 7.解释sgid的作⽤，以及代码演示sgid如何使⽤

```
sgid是获得文件属组的权限，主要用于文件夹，为某个目录设置sgid之后，在该目录中的创建的文件，都以目录的属组权限为准，而不属于创建该文件的用户权限，实现了多个用户共享一个目录。

[root@localhost ~]# mkdir apple
[root@localhost ~]# ll -d apple/
drwxr-xr-x. 2 root root 6 Mar 17 20:52 apple/
[root@localhost ~]# groupadd banana
[root@localhost ~]# chgrp banana apple/
[root@localhost ~]# ll -d apple/
drwxr-xr-x. 2 root banana 6 Mar 17 20:52 apple/
[root@localhost ~]# mkdir apple/iPhone4
[root@localhost ~]# ll apple/
total 0
drwxr-xr-x. 2 root root 6 Mar 17 20:58 iPhone4
[root@localhost ~]# chmod g+s apple/
[root@localhost ~]# ll -d apple/
drwxr-sr-x. 3 root banana 21 Mar 17 20:58 apple/
[root@localhost ~]# mkdir apple/iPhone4s
[root@localhost ~]# touch apple/iPhone5
[root@localhost ~]# ll apple/
total 0
drwxr-xr-x. 2 root root   6 Mar 17 20:58 iPhone4
drwxr-sr-x. 2 root banana 6 Mar 17 21:00 iPhone4s
-rw-r--r--. 1 root banana 0 Mar 17 21:02 iPhone5
```

> 8.完成红帽rhce认证考题。

```
1.创建⼀个共享⽬录/home/admins 
```

![image-20220317171727494](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317171727494.png)

```
2.要求该⽬录属组是adminuser,adminuser组内成员对该⽬录的权限是，可读，可写，可执⾏。 
```

![image-20220317172139243](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317172139243.png)

```
3.其他⽤户均⽆任何权限（root特例） 
```

![image-20220317172319304](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317172319304.png)

```
4.进⼊/home/admins创建的⽂件，⾃动继承adminuser组的权限。 
```

![image-20220317172700432](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317172700432.png)

> 9.解释sbit的作⽤，代码演示sbit如何使⽤。

```
sbit,粘滞位，当目录有了该特殊权限后，这个目录除了root用户特殊以外，任何用户都只能删除、移动自己创建的文件，而不能影响到其他人。
```

![image-20220317180259460](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317180259460.png)

> 10.根据课程在线⽂档，查看umask篇教程，尝试解决如下问题，代码演示。
>
> 学习umask，目的是为了让你理解，系统创建文件，文件夹的默认权限是多少
>
> umask是给系统设置的一个权限遮罩码，可以用于降低，文件，文件夹的创建默认权限
>
> 千万别去该他，理解权限是如何计算的就好了.

```
1.解释umask的作用

umask 命令是用来限制新文件权限的掩码，防止文件或文件夹创建的时候权限过大

2.如何使⽤umask

Linux用户默认创建文件/文件夹的初始权限=默认最高权限-umask
（文件默认没有x权限，其默认最大权限为666，若遮罩码设置为奇数，会自动+1变成偶数
```

![image-20220317183127282](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317183127282.png)

```
3.如何修改umask，会产⽣什么结果
# 临时修改
[root@localhost tmp]# umask 011
[root@localhost tmp]# touch test-umask.txt
[root@localhost tmp]# ll test-umask.txt 
-rw-rw-rw-. 1 root root 0 Mar 17 18:41 test-umask.txt

# 永久修改（数据写入文件，每次开机都会加载）
将'umask 033'写入用户环境变量文件 ~/.bashrc重新登录会话，检查umask值，查看默认权限
```

![image-20220317185506872](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317185506872.png)

> 11.根据课程在线⽂档，查看⽂件特殊属性，尝试解决如下问题，代码演示。
>
> 
>
> ```
> 关于云服务器，经常被攻击的事
> 
> 服务器常见被攻击角度，防御角度
> ssh端口用默认的22（改端口，密码弄的难一点，甚至禁用密码登录，采用公私钥登录）
> 	root口令太简单
> 	普通用户，密码太简单
> 
> 各种基础服务软件，端口不改ntp,nfs,samba （端口改掉）
> 
> 
> 各种主流中间件，端口不该，密码不用，。比如mysql,.redis,nginx,tomcat,mongodb（改端口，。设置密码，添加安全模式）
> 
> 先破解了机器
> 机器上放了定时任务，每分钟，去黑客的机器上，下载这个病毒脚本，在运行
> 
> 给这个定时任务文件，加锁，禁止我删除该病毒文件，修改病毒文件，
> 
> lsattr file.txt 发现了该文件有一个 i 特殊权限
> 
> 
> 
> ```
>
> 



```
1.锁定/var/log/my_website.log ，限制⽂件只能追加写⼊数据，不得删除⽂件、或清除⽂件内容。
```

![image-20220317192953134](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317192953134.png)

```
2.去除/var/log/my_website.log的⽂件锁定，清空⽂件内容后，再删除⽂件。
```

![image-20220317193901916](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317193901916.png)

> 12.根据课程在线⽂档，查看⽂件特殊属性，尝试解决如下问题，代码演示。

```
1.锁定⽂件/var/spool/cron/root，限制⽂件不得被删除、改名、修改内容。
```

![image-20220317194231962](day13%E4%BD%9C%E4%B8%9A.assets/image-20220317194231962.png)

```
2.去除/var/spool/cron/root⽂件的锁定限制，删除⽂件。

[root@localhost cron]# chattr -i root 
[root@localhost cron]# rm -rf root 
```

> 13.如何查看⽂件/var/spool/cron/root是否存在特殊属性。

```
lsattr命令用于查看文件扩展的属性
 -R 递归列出目录及其下内容的属性
 -a 列出目录中的所有文件，包括以“.”开头的文件属性
 -d 列出目录本身的属性
```

> 14.请判断如下⽂件具体类型？
>
> 

```
# http://yuchaoit.cn/nginx-logo.png

[root@localhost download]# file nginx-logo.png 
nginx-logo.png: PNG image data, 121 x 32, 1-bit colormap, non-interlaced

# http://yuchaoit.cn/404.html
[root@localhost download]# file 404.html 
404.html: HTML document, ASCII text

# http://yuchaoit.cn/all_nginx.tgz
[root@localhost download]# file all_nginx.tgz 
all_nginx.tgz: gzip compressed data, from Unix, last modified: Thu Mar 17 15:08:08 2022

# http://www.yuchaoit.cn/%E9%B8%A1%E6%B1%A4.txt
[root@localhost download]# file 鸡汤.txt 
鸡汤.txt: UTF-8 Unicode text

# /usr/bin/ls
[root@localhost download]# file /usr/bin/ls
/usr/bin/ls: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=c5ad78cfc1de12b9bb6829207cececb990b3e987, stripped

# /usr/bin/python
[root@localhost download]# file /usr/bin/python
/usr/bin/python: symbolic link to `python2'
[root@localhost download]# file /usr/bin/python2
/usr/bin/python2: symbolic link to `python2.7'
[root@localhost download]# file /usr/bin/python2.7
/usr/bin/python2.7: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=db43afedf61f35fbdf896c13894b501cb8bf1dad, stripped
```

> 15.普通⽂件，常⻅后缀有哪些? ⾄少写5个

```
.txt  .log  .jpg  .png  .html
```

> 16.find补充练习

```
1.找出根下名字叫做yuchao.pwd的⽂件
find / -type f -name 'yuchao.pwd'


2.找出/var下所有超过2M的log⽂件。
find /var -type f -size +2M -name '*.log'

*.log
	冠希哥.log
	吴彦祖.log

*.log*
	冠希哥.log.tar
	冠希哥.log
	吴彦祖.log
	
	



3.限定搜索最⼤⽬录为3层，找出系统中的log⽂件。
find / -maxdepth 3   -type f -name '*.log'


-maxdepth 搜索深度，以用户指定的 搜索路径为起点

find 搜索路径 -maxdepth 深度  -type f  -name '*.log'


最顶级的目录
/

第一级目录
/etc
/var
/opt
/usr
/tmp
/home

第二级
/etc/sysconfig
/var/log
/opt/nginx/
/usr/bin/
/usr/sbin/
/tmp/呵呵/
/home/cc03/

第三级

/home/cc03/王力宏.log

第四级
/home/cc03/my/王力宏.log





4.限定搜索最⼤⽬录为2层，找出系统中权限为640的⽂件，且显示其详细信息。
-perm 640

find / -maxdepth 2 -type f -perm 640 | xargs -i ls -l  {}
find / -maxdepth 2 -type f -perm 640  -ls




5.限定搜索最⼤⽬录为2层，找出系统中权限为644的⽂件，且统计有多少个。
find / -maxdepth 2 -type f -perm 644 | wc -l



```

>  17.按照如下要求，查看系统时间

```
1.查看当前时分秒
[root@localhost ~]# date +%T
20:21:34

2.查看当前⽇期
[root@localhost ~]# date +%F
2022-03-17

3.查看当前系统⽇期和时分秒
[root@localhost ~]# date '+%T %F'
21:18:41 2022-03-17

4.格式化显示时间，格式为 15:27:14 17-03-2022
[root@localhost ~]# date '+%H:%M:%S %d-%m-%Y'
21:23:44 17-03-2022

5.试⼀试，如何纠正，同步系统为正确的互联⽹时间。
[root@localhost ~]# yum -y install ntp ntpdate
[root@localhost ~]# ntpdate ntp2.aliyun.com
17 Mar 21:30:18 ntpdate[7106]: step time server 203.107.6.88 offset -1.582667 sec
```

