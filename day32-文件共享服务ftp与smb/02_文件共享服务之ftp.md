```### 此资源由 58学课资源站 收集整理 ###
	想要获取完整课件资料 请访问：58xueke.com
	百万资源 畅享学习

```
#  文件共享服务

文件共享服务方案有很多，了解即可

- ftp（简单文件传输服务）
  - 提供用户认证机制
  - 可以输入账号密码
- python -m SimpleHTTPServer
- nginx也提供了文件下载的功能
  - 提供用户认证机制
  - 反向代理，负载均衡
  - web服务器，静态文件服务器的作用
  - 如ftp服务器的作用
- samba(linux和windows之间共享数据)
  - 提供用户认证机制
- nfs（主要用这个）



```
重点学习，工作里用的是
nfs

nginx

平时，简易的快速进行文件下载，下载服务器上的资料
python -m SimpleHTTPServer
```



# 搭建ftp服务

```
1.需要安装vsftpd服务

yum install vsftpd -y

2.修改ftp配置文件，设置账号密码，登录ftp服务器，可以查看某文件夹下的数据资料（共享文件夹）

3.创建一个linux的用户（ftp使用linux的用户信息，不靠谱）
useradd ops01
设置该用户密码
[root@nfs-31 ~]#echo '123456' | passwd --stdin  ops01
Changing password for user ops01.
passwd: all authentication tokens updated successfully.

4.修改ftp配置文件，设置用于共享的目录
[root@nfs-31 ~]#rpm -ql vsftpd |grep '.conf$'
/etc/vsftpd/vsftpd.conf

4.1 关闭所有的匿名用户功能，不安全
找出和匿名用户相关的配置参数
[root@nfs-31 ~]#grep '^anonymous'  /etc/vsftpd/vsftpd.conf
anonymous_enable=NO

4.2添加自定义的共享文件夹配置参数，笔记的解释，别写入linux中，写笔记上，否则可能会导致编码不识别，程序出错

直接在文件最低下，添加如下配置
# 配置解释
# local_root=/data/kefu  指定本地用户的默认数据根目录 
# chroot_local_user=YES 禁锢本地用户的默认数据目录（禁止用户切换到其他目录）
# allow_writeable_chroot=YES 允许ftp用户登录后，可以创建数据

你只需要修改如下三个参数即可
# ftp用户，ops01登录ftp之后，只能看到/test_0224这个文件夹下的数据
## by myself
local_root=/test_0224/
chroot_local_user=YES
allow_writeable_chroot=YES







5.创建用于共享的文件夹
mkdir /test_0224/
touch /test_0224/wenjie.png

别忘记修改文件夹的权限，否则无法读取了，修改为刚才自定义的用户

chown -R ops01:ops01  /test_0224/

[root@nfs-31 ~]#ll -d /test_0224/
drwxr-xr-x 2 ops01 ops01 24 Apr 19 14:53 /test_0224/



6.此时可以重启vsftpd服务
[root@nfs-31 ~]#systemctl restart vsftpd
[root@nfs-31 ~]#
[root@nfs-31 ~]#
[root@nfs-31 ~]#ps -ef|grep vsftpd
root       2221      1  0 15:01 ?        00:00:00 /usr/sbin/vsftpd /etc/vsftpd/vsftpd.conf
root       2226   1168  0 15:02 pts/0    00:00:00 grep --color=auto vsftpd
[root@nfs-31 ~]#
[root@nfs-31 ~]#



```

# 使用客户端，验证ftp的登录，数据查看

```
你可以用另一台机器，安装ftp程序，登录vsftpd服务端

yum install ftp -y


登录ftp设备的命令
ftp 机器的ip地址
如

ftp 172.16.1.31
输入账号密码 ops01 123456
进入之后，输入? 查看ftp提供的命令帮助
ftp> pwd  查看当前的ftp目录位置
257 "/"

ftp提供的上传下载
下载功能
ftp> get 
(remote-file) 文杰.png
(local-file) 文杰1.png
local: 文杰1.png remote: 文杰.png
227 Entering Passive Mode (10,0,0,31,149,223).
150 Opening BINARY mode data connection for 文杰.png (0 bytes).
226 Transfer complete.
ftp> 


上传功能
ftp> 
ftp> put
(local-file) /opt/4111111.jpg
(remote-file) 4444444.jpg
local: /opt/4111111.jpg remote: 4444444.jpg
227 Entering Passive Mode (10,0,0,31,185,67).
150 Ok to send data.
226 Transfer complete.


```

