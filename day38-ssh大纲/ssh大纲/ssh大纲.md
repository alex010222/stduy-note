```### 此资源由 58学课资源站 收集整理 ###
	想要获取完整课件资料 请访问：58xueke.com
	百万资源 畅享学习

```
# 前言

```
今天的理论偏多，原理琢磨明白之后
你会发现，其实要学的就是几条命令，创建了几个文件而已

但是其中的原理，需要你搞清楚
```



# 机器准备



# 什么是SSH



![image-20220427092753985](pic/image-20220427092753985.png)



## SSH是一种网络协议





## SSH客户端





# 为什么需要SSH





## 连接服务器的方式







## 不安全登录telnet（实践）

```
1.准备好客户端、服务端

客户端只要可以提供telnet命令即可，默认连接服务器的23端口号
windows

telnet  10.0.0.61 


2.服务端要运行远程连接服务，telnet走的是telnet-server服务端，走的是23端口通信，而不是ssd服务端

运行telnet-server服务
yum install telnet-server telnet -y

启动telnet服务端
[root@m-61 ~]#systemctl start telnet.socket


3.使用telnet命令，登录linux服务端
[C:\~]$ telnet   10.0.0.61


Connecting to 10.0.0.61:23...
Connection established.
To escape to local shell, press 'Ctrl+Alt+]'.

Kernel 3.10.0-862.el7.x86_64 on an x86_64
m-61 login: 
Password: 
^CLogin incorrect

m-61 login: root
Password: 
Last login: Wed Apr 27 09:37:46 from 10.0.0.1
[root@m-61 ~]#






```









## 修改linux认证（允许telnet使用root登录）





## windows中telnet登录



## 安装wireshark抓包工具







### 抓取telnet的明文密码





### 试试抓取ssh的密码





# 学习SSH的任务背景

![image-20220427101054842](pic/image-20220427101054842.png)





# 部署ssh要求









# (实战1)SSH密码登录原理

```
root
123123

先学简单的原理

用root密码登录的原理，为什么是安全的。。。

```



### 1、加密算法（了解）

#### **①对称加密算法(DES)**



#### **②非对称加密算法(RSA)**

![image-20220427102816663](pic/image-20220427102816663.png)



### 2、对称、非对称加密算法区别是？







### 3、SSH认证方式



```
1.关于root ，账户密码连接的加密，解密流程

root@10.0.0.7
123123





2.关于root，公私钥连接的加密，解密流程

3. 配置ssh的免密登录（生成公私钥）

4. 学习修改sshd的服务端配置文件修改（修改端口，禁止密码登录，只允许公钥登录）


```



### （重点）SSH基于用户名密码认证原理



![image-20220427104431960](pic/image-20220427104431960.png)



问题来了，我该如何确认，找到这个原理图中所说的公私钥在哪呢？

```
要测试基于账户密码认证的登录形式，需要准备2个机器

客户端（windows，linux）
windows
打开cmd，xshell，只识别windows提供的dos命令
还提供了一个工具，叫做git-bash工具，利用这个工具，打开的终端，能识别部分的linux命令，去操作你的windows

# 输入如下命令，首次连接某个linux机器
#1.首次连接,需要用户自己确认机器的指纹信息，输入yes确认后
服务端会发送公钥给客户端，记录到客户端的 ~/.ssh/knows_hosts
ssh root@10.0.0.61

2.查看客户端记录的 cat  ~/.ssh/known_hosts  ，记录了服务端的公钥信息
yu@DESKTOP-1TDLFH9 MINGW64 ~/Desktop
$ cat  ~/.ssh/known_hosts
10.0.0.61 ecdsa-sha2-nistp256 
AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL/Sx3bAaNcKqo7pC4FTYk3gyZ6hd1D/DKUWVfOd4gZb/8XwlAxWauceHe/BAsW5Z8pEmG6AjSyHM8ckOs94c7Y=

这个字符串，就是你要连接的linux机器的公钥信息



服务端（linux）
10.0.0.61

此时去你的服务端， 配置文件下寻找 ecdsa该算法的公钥文件
[root@m-61 ~]#ls /etc/ssh/
moduli      sshd_config         ssh_host_ecdsa_key.pub  ssh_host_ed25519_key.pub  ssh_host_rsa_key.pub
ssh_config  ssh_host_ecdsa_key  ssh_host_ed25519_key    ssh_host_rsa_key

过滤该算法提供的公私钥文件
[root@m-61 ~]#ls /etc/ssh/ |grep ecdsa
ssh_host_ecdsa_key
ssh_host_ecdsa_key.pub


查看公钥
[root@m-61 ~]#cat /etc/ssh/ssh_host_ecdsa_key.pub
ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL/Sx3bAaNcKqo7pC4FTYk3gyZ6hd1D/DKUWVfOd4gZb/8XwlAxWauceHe/BAsW5Z8pEmG6AjSyHM8ckOs94c7Y= 



证明，在linux服务端，看到了基于ssh 密码认证的，公私钥文件信息
[root@m-61 ~]#
[root@m-61 ~]#
[root@m-61 ~]#cat /etc/ssh/ssh_host_ecdsa_key.pub
ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL/Sx3bAaNcKqo7pC4FTYk3gyZ6hd1D/DKUWVfOd4gZb/8XwlAxWauceHe/BAsW5Z8pEmG6AjSyHM8ckOs94c7Y= 
[root@m-61 ~]#
[root@m-61 ~]#
[root@m-61 ~]#
[root@m-61 ~]#cat /etc/ssh/ssh_host_ecdsa_key
-----BEGIN EC PRIVATE KEY-----
MHcCAQEEIFRG97Nad3sFsLBv5rL/76jHuc1SUsUikqttZdn2j1FroAoGCCqGSM49
AwEHoUQDQgAEv9LHdsBo1wqqjukLgVNiTeDJnqF3UP8MpRZV853iBlv/xfCUDFZq
5x4d78ECxblnykSYboCNLIczxyQ6z3hztg==
-----END EC PRIVATE KEY-----
[root@m-61 ~]#





```









既然看懂了公私钥，非对称验证形式，我们也就可以利用该特性，完成公钥登录，代替密码登录。（利用客户端的公钥表示自己的身份，而不是用服务端的密码去验证了）





### 5.如何确认目标机器的正确性（了解）

首次连接，服务端是让你输入yes，确认该机器的指纹是否正确

```
指纹信息，是通过公钥，基于sha256加密而来

1.看指纹是什么
yu@DESKTOP-1TDLFH9 MINGW64 ~/Desktop
$ ssh root@10.0.0.61
The authenticity of host '10.0.0.61 (10.0.0.61)' can't be established.
ECDSA key fingerprint is 
SHA256:Csqwr63+SZRFFOug/IGoFTgRe8hDSI/QalSMBcC6IaU.
Are you sure you want to continue connecting (yes/no/[fingerprint])?


2.根据你获取到的公钥信息，通过sha256计算下，得到指纹，在对比这俩指纹是否一致，就明确该机器是你想连接的机器了。。。
你可以在服务端敲
ssh-keygen -E SHA256 -lf /etc/ssh/ssh_host_ecdsa_key.pub

如何再客户端验证该指纹
在客户端，获取到该机器的公钥
yu@DESKTOP-1TDLFH9 MINGW64 ~/Desktop
$ ssh-keyscan  10.0.0.61 |grep ecdsa
# 10.0.0.61:22 SSH-2.0-OpenSSH_7.4
# 10.0.0.61:22 SSH-2.0-OpenSSH_7.4
# 10.0.0.61:22 SSH-2.0-OpenSSH_7.4
# 10.0.0.61:22 SSH-2.0-OpenSSH_7.4
# 10.0.0.61:22 SSH-2.0-OpenSSH_7.4
10.0.0.61 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL/Sx3bAaNcKqo7pC4FTYk3gyZ6hd1D/DKUWVfOd4gZb/8XwlAxWauceHe/BAsW5Z8pEmG6AjSyHM8ckOs94c7Y=

vim 61.pub

对公钥加密256加密，得到指纹
ssh-keygen -E SHA256 -lf  61.pub






```

![image-20220427112520936](pic/image-20220427112520936.png)



在客户端，去验证，你连接的机器，是否是你想连接的

ssh root@10.0.0.61 

基于公钥确认，先得知该机器的公钥信息

```
两个办法，获取到机器公钥信息

1.在客户端扫描该机器的公钥
yu@DESKTOP-1TDLFH9 MINGW64 ~/Desktop
$ ssh-keyscan  10.0.0.61 |grep ecdsa
# 10.0.0.61:22 SSH-2.0-OpenSSH_7.4
# 10.0.0.61:22 SSH-2.0-OpenSSH_7.4
# 10.0.0.61:22 SSH-2.0-OpenSSH_7.4
# 10.0.0.61:22 SSH-2.0-OpenSSH_7.4
# 10.0.0.61:22 SSH-2.0-OpenSSH_7.4
10.0.0.61 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL/Sx3bAaNcKqo7pC4FTYk3gyZ6hd1D/DKUWVfOd4gZb/8XwlAxWauceHe/BAsW5Z8pEmG6AjSyHM8ckOs94c7Y=





2.在服务端
[root@m-61 ~]#cat /etc/ssh/ssh_host_ecdsa_key.pub
ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL/Sx3bAaNcKqo7pC4FTYk3gyZ6hd1D/DKUWVfOd4gZb/8XwlAxWauceHe/BAsW5Z8pEmG6AjSyHM8ckOs94c7Y= 




```







我们在进行指纹认证的时候，是直接输入yes，并未做确认

证明，你yes确认的机器，的确是目标的机器

你yes确认的那个指纹，是通过公钥文件，加密而来，用同样的算法，对该文件加密，就可以看到一样的指纹了



```
1.首次连接，指纹确认
yu@DESKTOP-1TDLFH9 MINGW64 ~/Desktop
$ ssh root@10.0.0.61
The authenticity of host '10.0.0.61 (10.0.0.61)' can't be established.
ECDSA key fingerprint is SHA256:Csqwr63+SZRFFOug/IGoFTgRe8hDSI/QalSMBcC6IaU.
Are you sure you want to continue connecting (yes/no/[fingerprint])?

ECDSA key fingerprint is SHA256:Csqwr63+SZRFFOug/IGoFTgRe8hDSI/QalSMBcC6IaU.



2.服务端发给客户端自己的公钥，客户端记录到本地~/.ssh/known_hosts（通用语windows，linux），表示已知的服务器公钥信息

cat ~/.ssh/known_hosts

此时客户端，还未确认该指纹，就不会吧服务端的公钥，写入到该文件中
你现在可以去比对，指纹是否一致
ssh-keygen扫描公钥
ssh-keyscan命令，可以获取目标机器的公钥信息，扫描目标机器的公钥

在客户端操作
ssh-keyscan  10.0.0.61

yu@DESKTOP-1TDLFH9 MINGW64 ~/Desktop
$ ssh-keyscan  10.0.0.61
# 10.0.0.61:22 SSH-2.0-OpenSSH_7.4
10.0.0.61 ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC/a19vEmUMzhzEM8Hbqy2Cd2KL838ojWDO/TDiMOpaKZ6okJ7kBsy8eK0ukYqMj/xBgco8rlc8nz171rul3HN1uUgu7+c8TNbAyggyiWNmaZzYMbWYwC5ZhhlPA8RU+39Znw4BpwKqEC76h2dAj/OOwEADIJK4GuwEnsTmCy2CSh4hqL7M1MWlsV7Kkc0ZBA4tYmz/qAYakl8PFF+CY7YWMJFWDvqLxGdEcqBkvIJ1hEuB21UDhXCOimYgp94frzcyeFoGv4WKcKw7Dq3lPh9gT038v6bw/EA5PC4ubjmly3XJUKZoRVcg773Oa/TcBOeZfTW2zPK83X0hQNk9T1KJ
# 10.0.0.61:22 SSH-2.0-OpenSSH_7.4
10.0.0.61 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL/Sx3bAaNcKqo7pC4FTYk3gyZ6hd1D/DKUWVfOd4gZb/8XwlAxWauceHe/BAsW5Z8pEmG6AjSyHM8ckOs94c7Y=
# 10.0.0.61:22 SSH-2.0-OpenSSH_7.4
10.0.0.61 ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIC9lqWZdqpEGg6hV4PbSf/n0NI33oaedK/KFP5YWxF6A
# 10.0.0.61:22 SSH-2.0-OpenSSH_7.4
# 10.0.0.61:22 SSH-2.0-OpenSSH_7.4


提取服务端远程连接的公钥
# 10.0.0.61:22 SSH-2.0-OpenSSH_7.4
10.0.0.61 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL/Sx3bAaNcKqo7pC4FTYk3gyZ6hd1D/DKUWVfOd4gZb/8XwlAxWauceHe/BAsW5Z8pEmG6AjSyHM8ckOs94c7Y=


通过命令，对该公钥进行加密，得到指纹
ssh-keygen -E SHA256 -lf /etc/ssh/ssh_host_ecdsa_key.pub








3.你可以去服务端查看公钥信息/etc/ssh/ssh_host_ecdsa_key.pub 
和你客户端的known_hosts做对比，是否一致






```









#### 机器指纹信息



#### ssh-keygen扫描公钥

ssh-keyscan命令，可以获取目标机器的公钥信息，通过web-7扫描 rsync-41机器的公钥





#### 查看机器公钥

直接去rsync-41机器上检查公钥信息



#### 指纹是公钥加密而来(sha256算法)

计算rsync-41机器的指纹，再和我们第一次ssh连接的命令比较，是否一致



## 6、ssh密码认证小结

- SSH是Linux下远程管理的工具，相比Telnet安全，运维人员必备的神器！
- SSH的全称Secure Shell，安全的shell，是Client/Server架构，默认端口号为22，TCP协议
- 必须搞懂SSH通信加密的原理、过程

```
1.需要你总结
- ssh密码认证的原理，与配置文件，涉及的linux命令
- ssh指纹确认与公钥的原理，与配置文件，涉及的linux命令
- ssh密码认证涉及的所有配置文化（服务端，客户端）


```

听说下雨天和学linux更配哦



# (实战2-服务器安全)sshd服务部署

```
软件安装
修改配置文件
启动
使用

```



### 1、搭建所有服务的套路

- 关闭防火墙和selinux(实验环境都先关闭掉)
- 配置yum源(公网源或者本地源)
- 软件安装和检查
- 了解并修改配置文件
- 启动服务检查运行状态并设置开机自启动

### 2、搭建SSH服务

这部分内容可以参考于超老师前面讲解的系统初始化篇操作即可



#### （一）关闭防火墙和selinux

#### （二）配置yum源

#### （三）软件安装

##### ①确认是否安装openssh

由于每一台机器都是默认安装了sshd服务的，这里你可以采用期中综合架构里的任何一台机器测试。

```
[root@m-61 ~]#rpm -qa openssh
openssh-7.4p1-16.el7.x86_64

openssl 命令，给你用，去基于不同的算法，创建证书的命令


安装openssh服务，提供远程sshd服务
[root@m-61 ~]#rpm -qa openssh-server
openssh-server-7.4p1-16.el7.x86_64


# 默认yum有2个功能，1是安装，2是升级

yum install openssh-server openssh -y





```

#### （四）ssh基本安全配置

提升服务器ssh安全，就是来修改该配置文件了，先来看简单的配置；

```
1.禁用root用户登录、降低权限（只能通过普通用户登录，提前创建好可登录的普通普通户）
[root@m-61 ~]# useradd bob01 
[root@m-61 ~]# cat random-pw.txt 
gBPKYvF1
[root@m-61 ~]#
[root@m-61 ~]#
[root@m-61 ~]#cat random-pw.txt | passwd --stdin bob01
Changing password for user bob01.
passwd: all authentication tokens updated successfully.
[root@m-61 ~]#
[root@m-61 ~]#

修改
#PermitRootLogin yes
PermitRootLogin no


2.修改端口，22999
Port 22999

3.查看这俩配置参数
grep -Ei '^permitRootLogin|^port'    /etc/ssh/sshd_config 
grep -Ei '^(permitRootLogin|port)'    /etc/ssh/sshd_config 




```

##### 4.1 pwgen随机密码生成工具

```
yum install pwgen -y



```

#### （五）服务管理

```
[root@m-61 ~]#netstat -tunlp |grep sshd
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      4516/sshd           
tcp6       0      0 :::22                   :::*                    LISTEN      4516/sshd     

重启
systemctl restart sshd

[root@m-61 ~]#
[root@m-61 ~]#netstat -tunlp |grep sshd
tcp        0      0 0.0.0.0:22999           0.0.0.0:*               LISTEN      4654/sshd           
tcp6       0      0 :::22999                :::*                    LISTEN      4654/sshd  
```



#### （六）登录测试

```
加上端口
默认不让root登录了
[C:\~]$ ssh root@10.0.0.61 22999



```



#### （七）恢复默认配置

```
sed改回去

permitRootLogin

port


先看看参数
sed多次处理

看到该参数
sed -e '/^permitRootLogin/Ip'   -e '/^port/Ip'  /etc/ssh/sshd_config -n
Port 22
PermitRootLogin yes

整行内容替换，写入文件内容
sed -i.ori   -e '/^permitRootLogin/Ic PermitRootLogin yes'   -e '/^port/Ic Port 22'  /etc/ssh/sshd_config 

替换方案
sed -i.bak  -e 's#PermitRootLogin no#PermitRootLogin yes#' -e 's#Port 22999#Port 22#' /etc/ssh/sshd_config 





```















