```### 此资源由 58学课资源站 收集整理 ###
	想要获取完整课件资料 请访问：58xueke.com
	百万资源 畅享学习

```
# 用于linux和windows共享的samba服务

```
ftp是客户端、服务端两个

服务端是  vsftpd
linux客户端是  ftp命令 ，以及其他各种支持ftp协议的工具，如windows下提供很多软件，支持图形化上传下载ftp，xftp


windows访问ftp
命令行操作
C:\Users\yu>ftp
ftp> bye

C:\Users\yu>
C:\Users\yu>
C:\Users\yu>ftp 10.0.0.31
连接到 10.0.0.31。
220 (vsFTPd 3.0.2)
200 Always in UTF8 mode.
用户(10.0.0.31:(none)): ops01
331 Please specify the password.
密码:
230 Login successful.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
41.png
4444444.jpg
佳强.png
文杰.png
226 Directory send OK.
ftp: 收到 48 字节，用时 0.00秒 48000.00千字节/秒。
ftp>


图形化连接ftp设备

指定协议语法
ftp://10.0.0.31/


nfs

```

# samba服务端的部署

client、server软件的使用，部署流程

```
1.安装samba软件
 yum install samba -y
 
2.修改配置文件,依然是设置一个共享文件夹
samba的软件配置文件在
ls /etc/samba/
lmhosts  smb.conf  smb.conf.example

修改 /etc/samba/smb.conf
添加自定义的，共享文件夹的配置

[root@nfs-31 /opt]#tail -7 /etc/samba/smb.conf

[smb_share]
    comment=myself share dir
    path = /my_smb/
    guest ok=no
    public = no
    writable = yes


3.创建共享文件夹
mkdir /my_smb/


4.samba也有用户认证机制，需要通过pdbedit命令设置samba的用户信息
4.1 pdbedit命令是给linux以及存在的用户，设置一个密码
useradd  samba01

4.2 使用pdbedit命令，给samba的用户设置密码
-a 添加smb用户
-u 指定用户名

# 123123
[root@nfs-31 /opt]#pdbedit -a -u samba01
new password:
retype new password:

5.修改smb共享文件夹的权限
chown -R samba01:samba01   /my_smb/

6.给该目录下创建些数据
touch 大胆妖孽-大威天龙.png

7.启动samba服务
[root@nfs-31 /my_smb]#systemctl start smb




8.后续你的确需要部署samba服务，如何使用samba？
做哪些后续的学习呢？
说白了，就是学samba的配置文件，里面的参数，是什么功能，就有什么作用
samba是一个软件，所有的功能，都被以配置文件形式定义好了
配置文件是最重要的，控制软件功能的一个文件
程序启动会去读取配置文件中的参数，以打开、关闭不同的功能


9.验证进程、端口
[root@nfs-31 /my_smb]#netstat -tunlp|grep smb
tcp        0      0 0.0.0.0:445             0.0.0.0:*               LISTEN      2759/smbd           
tcp        0      0 0.0.0.0:139             0.0.0.0:*               LISTEN      2759/smbd           
tcp6       0      0 :::445                  :::*                    LISTEN      2759/smbd           
tcp6       0      0 :::139                  :::*                    LISTEN      2759/smbd           
[root@nfs-31 /my_smb]#
[root@nfs-31 /my_smb]#
[root@nfs-31 /my_smb]#ps -ef|grep smb
root       2759      1  0 15:48 ?        00:00:00 /usr/sbin/smbd --foreground --no-process-group
root       2761   2759  0 15:48 ?        00:00:00 /usr/sbin/smbd --foreground --no-process-group
root       2762   2759  0 15:48 ?        00:00:00 /usr/sbin/smbd --foreground --no-process-group
root       2763   2759  0 15:48 ?        00:00:00 /usr/sbin/smbd --foreground --no-process-group










```

## samb客户端认证

```
linux客户端
需要安装工具
yum install samba-client -y


2.使用该命令，连接samba机器即可

smbclient //10.0.0.31/smb_share   -U samba01
# 输入samba01的密码即可
#进入后，输入 ? 查看samba提供的命令，也就是作用







windowos也有客户端
配置比较繁琐
参考图片
http://apecome.com:9494/03%E7%B3%BB%E7%BB%9F%E6%9C%8D%E5%8A%A1%E7%AF%87/pic/151645101550_.pic.jpg

使用 windows的win快捷键+ r，打开运行窗口
访问samba的协议是

\\10.0.0.31\smb_share

此时输入账号密码
samba01
123123





```











# 今天任务

1.完成9个虚拟机的创建，按照上午讲解的要求来

2.完成网卡脚本，sed的练习题，写出正确的正则sed命令

3.完成ftp、samba服务的搭建练习，做好笔记

4.分别拿windows、linux测试ftp、samba服务的搭建，访问，文件数据操作

5.预习rsync

```
后续的预习博客，看整理
账户 luffy-linux
密码 luffycity

http://book.luffycity.com/linux-book/%E9%AB%98%E6%80%A7%E8%83%BDWeb%E9%9B%86%E7%BE%A4%E5%AE%9E%E6%88%98/Rsync%E6%95%B0%E6%8D%AE%E5%A4%8D%E5%88%B6.html#rsync%E5%AE%A2%E6%88%B7%E7%AB%AF%E6%8E%92%E9%94%99
```

