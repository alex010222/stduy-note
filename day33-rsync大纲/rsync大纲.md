```### 此资源由 58学课资源站 收集整理 ###
	想要获取完整课件资料 请访问：58xueke.com
	百万资源 畅享学习

```
第1章 Rsync本地模式和远程模式
1.命令说明

纯通过rsync的命令，来实现，数据目录A  拷贝到数据目录B

也就是模拟cp的用法 很简单



```
rsync  [选项]  源数据   目的数据

1.安装
yum install rsync -y


2.命令语法，分几个模式


- 本地模式

rsync 参数   源路径  目标路径

rsync  -xxxxx    /var/log    /tmp



- 远程模式，推送方式，把自己的数据推送到另一台机器上（上传）
语法1 ，rsync默认走ssh协议
rsync 参数  源路径  user@ip:目标路径

rsync  -avzP  /var/log/      root@10.0.0.31:/tmp/



语法2
rsync 参数 源路径  user@ip::目标路径


- 远程模式，拉取方式，拉取别人机器的数据到自己的机器上（下载）
rsync  参数   user@ip:源路径   目标路径
rsync  参数   user@ip::源路径目标路径


rsync -avzP  root@10.0.0.31:/var/log/   /tmp/








参数解释
    -v        详细模式输出
    -a        归档模式，递归的方式传输文件，并保持文件的属性，等同于 -rlptgoD
    -r        递归拷贝目录
    -l        保留软链接
    -p        保留原有权限
    -t         保留原有时间（修改）
    -g        保留属组权限
    -o         保留属主权限
    -D        等于--devices  --specials    表示支持b,c,s,p类型的文件
    -R        保留相对路径
    -H        保留硬链接
    -A        保留ACL策略
    -e         指定要执行的远程shell命令
    -E         保留可执行权限
    -X         保留扩展属性信息  a属性


 比较常用的组合参数
 rsync -avzP
-a    保持文件原有属性
-v    显示传输细节情况
-z    对传输数据压缩传输
-P    显示文件传输的进度信息


你在命令行里，执行命令，喜欢看到命令的执行过程 
-avzP

脚本里面？
bash xxx.sh
rsync -az  





```



2.本地模式

linux机器本身，数据来回发送

```
# 以后cp可以放一旁了，用rsync当cp使就行



# /var/log/50G 

cp /var/log/   /tmp/
touch /var/log/new1.file
cp /var/log/   /tmp/



# 用rsync，支持增量备份
# /var/log/50G 

rsync  -avzP /var/log/   /tmp/ 
touch /var/log/new1.file
rsync  -avzP /var/log/   /tmp/ 










```



```
对文件同步
[root@rsync-41 ~]#yum install rsync -y
把本地的的/var/log/messages 文件 拷贝到/opt下

拷贝单个文件
[root@rsync-41 ~]#rsync -avzP /var/log/messages /opt
sending incremental file list
messages
        985,500 100%   56.79MB/s    0:00:00 (xfr#1, to-chk=0/1)

sent 123,300 bytes  received 35 bytes  246,670.00 bytes/sec
total size is 985,500  speedup is 7.99

拷贝单个大文件，拷贝大文件时，要注意限速，否则占用磁盘IO太多
--bwlimit=10

先生成一个5G文件
dd bs=100M count=50 if=/dev/zero  of=/var/log/my_self.log

rsync -avzP /var/log/my_self.log  /opt

iotop查看磁盘的读写IO情况

限制单个大文件的传输，速度只给他20M每秒


[root@rsync-41 ~]#rsync -avzP --bwlimit=20   /var/log/my_self.log  /opt
sending incremental file list
my_self.log
    393,117,696   7%   20.14MB/s    0:03:55  






对目录同步（注意语法的区别）

拷贝后的数据，是否携带该目录本身
[root@rsync-41 ~]#rsync -avzP /var/log    /opt

不拷贝该目录本身，拷贝目录下的数据
[root@rsync-41 ~]#rsync -avzP /var/log/    /opt

测试文件夹的增量拷贝
[root@rsync-41 ~]#rsync -avzP /test1/   /test2
sending incremental file list

sent 118 bytes  received 12 bytes  260.00 bytes/sec
total size is 0  speedup is 0.00
[root@rsync-41 ~]#
[root@rsync-41 ~]#
[root@rsync-41 ~]#
[root@rsync-41 ~]#echo "123" >/test1/1.png 
[root@rsync-41 ~]#
[root@rsync-41 ~]#rsync -avzP /test1/   /test2
sending incremental file list
1.png
              4 100%    0.00kB/s    0:00:00 (xfr#1, to-chk=4/6)

sent 175 bytes  received 35 bytes  420.00 bytes/sec
total size is 4  speedup is 0.02
[root@rsync-41 ~]#






无差异化拷贝
使用--delete参数 将目标目录的数据清空，保证完全和源目录的数据一致

rsync -azvP --delete  /test1/   /test2/

[root@rsync-41 /test2]#rsync -azvP --delete  /test1/   /test2/
sending incremental file list
deleting 行者孙.png
./
白龙马.png
              0 100%    0.00kB/s    0:00:00 (xfr#1, to-chk=0/12)

sent 269 bytes  received 55 bytes  648.00 bytes/sec
total size is 4  speedup is 0.01
[root@rsync-41 /test2]#
[root@rsync-41 /test2]#
[root@rsync-41 /test2]#ls /test1/
1.png  2.png  3.png  4.png  5.png  孙悟空1  孙悟空2  孙悟空3  孙悟空4  孙悟空5  白龙马.png
[root@rsync-41 /test2]#ls /test2
1.png  2.png  3.png  4.png  5.png  孙悟空1  孙悟空2  孙悟空3  孙悟空4  孙悟空5  白龙马.png


# rsync拷贝文件夹,携带目录本身
# 吧test1目录本身，连带着数据，都拷贝到test2下
rsync -avzP /test1    /test2/
最终会生成
/test2/test1/ 该文件夹的数据，和源数据目录 /test1是一样的









对rsync限速，因为rsync在传输数据时，会占用大量的磁盘IO，以及如果是网络传输的话，占用网络带宽，会导致其他程序受影响

所以rsync这样的备份服务，都是在夜里，凌晨操作，被影响其他程序




--bwlimit



[root@rsync-41 /test2]## 实现 /root  和/tmp 完全一样
[root@rsync-41 /test2]#
[root@rsync-41 /test2]#
[root@rsync-41 /test2]#
[root@rsync-41 /test2]#ls /root  /tmp
/root:
anaconda-ks.cfg  change_network.sh  hello-100.log  hosts.ip  i_am_template-100.log

/tmp:
anaconda-ks.cfg  change_network.sh  hello-100.log  hosts.ip  i_am_template-100.log
[root@rsync-41 /test2]#rsync -avzP --delete  /root/    /tmp/

```





3.远程模式

把/root下的数据，拷贝到 /tmp下

```
把rsync-41   /root下的数据

拷贝到 nfs-31   /tmp下

```



实现如scp的作用



	#PUSH 推送模式，上传模式
	
	把rsync-41   /root下的数据，拷贝到 nfs-31   /tmp下
	
	登录rsync41
	用ip形式、再用主机名形式
	添加无差异化参数，该参数，慎用！搞清楚了你在做什么！
	
	rsync -avzP  --delete  /root/    root@172.16.1.31:/tmp/
	
	
	
	
	
	#PULL 拉取模式（你要琢磨，数据最终放在了哪）
	# 把rsync-41   /root下的数据，拷贝到 nfs-31   /tmp下
	
	rsync -avzP   root@172.16.1.41:/root/    /tmp/
	
	
	# 拉取rsync41的/etc/passwd文件到 nfs-31的/opt下，使用主机名通信
	
	rsync -avzP root@rsync-41:/etc/passwd    /opt/
	
	
	
	#传输目录注意
	
	
	#传输整个目录,包含目录本身
	
	rsync -avzP   root@172.16.1.41:/root    /tmp/
	
	
	#只传输目录下的文件,不包含目录本身
	rsync -avzP   root@172.16.1.41:/root/    /tmp/
	
	
	#不同主机之间同步数据 --delete
	rsync -avzP --delete   root@172.16.1.41:/root    /tmp/
	
	#坑:如果/和一个空目录进行完全同步,那么效果和删根一样
	
	
	#坑:传输过程不限速导致带宽被占满 ,--bwlimit=50
	
	远程传输 nfs-31下的 /tmp/2G.log   备份到  rsync-41的/opt下
	
	rsync -avzP   /tmp/2G.log   root@172.16.1.41:/opt
	
	
	-a   保持文件原有属性
	-v   显示传输过程
	-z   压缩传输数据
	-P   显示传输进度
	
	
	远程备份文件，且改名
	[root@nfs-31 /tmp]#rsync -avzP   /tmp/2G.log   root@172.16.1.41:/opt/2G.logggggggggggggggggggggg
	
	
	远程传输 nfs-31下的 /tmp/2G.log   备份到  rsync-41的/opt下，且是无差异化备份
	等于清空原有/opt下的数据
	
	rsync -avzP --delete   /tmp/2G.log   root@172.16.1.41:/opt/2G.log
	
	
	
	
	
	
	





第2章 Rsync服务模式-服务端配置
0.为什么需要服务模式
	Rsync 借助 SSH 协议同步数据存在的缺陷:
	1.使用系统用户（不安全） /etc/passwd
	2.使用普通用户（会导致权限不足情况）
	3.守护进程传输方式: rsync 自身非常重要的功能(不使用系统用户，更加安全)

1.安装rsync

```
yum install rsync -y
```



2.修改配置文件

复制粘贴如下代码即可

```
cat > /etc/rsyncd.conf << 'EOF'
uid = www 
gid = www 
port = 873
fake super = yes
use chroot = no
max connections = 200
timeout = 600
ignore errors
read only = false
list = false
auth users = rsync_backup
secrets file = /etc/rsync.passwd 
log file = /var/log/rsyncd.log
#####################################
[backup]
comment = yuchaoit.cn about rsync
path = /backup

[data]
comment = this is secord backup dir,to website data..
path = /data
EOF
```



3.创建用户以及数据目录

```
根据你的配置文件中定义的信息，创建对应的用户，备份的目录
该无法登录的用户，只是用于运行进程的账户
useradd -u 1000 -M -s /sbin/nologin www

创建配置文件中定义的2个备份目录
mkdir -p /data/   /backup

修改备份目录的权限
[root@rsync-41 ~]#chown -R www:www /data/
[root@rsync-41 ~]#chown -R www:www /backup/
[root@rsync-41 ~]#ll -d /data /backup/
drwxr-xr-x 2 www www 6 Apr 20 11:34 /backup/
drwxr-xr-x 2 www www 6 Apr 20 11:34 /data

```

4.创建rsync专用的账户密码（这一步很重要，有错基本也是来这排查）

````
1.创建密码文件，写入账户和密码，用于和客户端连接时候的认证
vim /etc/rsync.passwd

2.写入账户密码
[root@rsync-41 ~]#cat /etc/rsync.passwd 
rsync_backup:chaoge666

3.待会客户端向rsync服务器推送数据，就得用这个账号密码！！！！


4.这一步，非常重要，rsync要求降低密码文件的权限，且必须是600

chmod 600 /etc/rsync.passwd 

[root@rsync-41 ~]#ll /etc/rsync.passwd 
-rw------- 1 root root 23 Apr 20 11:36 /etc/rsync.passwd


````







5.加入开机自启动

```
设置rsyncd服务，运行，且开机自启

systemctl start rsyncd


检查rsyncd服务是否运行，以及该服务的运行日志 

[root@rsync-41 ~]#cp /etc/rsyncd.conf.bak /etc/rsyncd.conf
[root@rsync-41 ~]#
[root@rsync-41 ~]#
[root@rsync-41 ~]#systemctl restart rsyncd
[root@rsync-41 ~]#
[root@rsync-41 ~]#systemctl status rsyncd
● rsyncd.service - fast remote file copy program daemon
   Loaded: loaded (/usr/lib/systemd/system/rsyncd.service; disabled; vendor preset: disabled)
   Active: active (running) since Wed 2022-04-20 11:46:57 CST; 4s ago
 Main PID: 6078 (rsync)
   CGroup: /system.slice/rsyncd.service
           └─6078 /usr/bin/rsync --daemon --no-detach

Apr 20 11:46:57 rsync-41 systemd[1]: Started fast remote file copy program daemon.
Apr 20 11:46:57 rsync-41 systemd[1]: Starting fast remote file copy program daemon...
Apr 20 11:46:57 rsync-41 rsyncd[6078]: params.c:Parameter() - Ignoring badly formed line in config file: ignore errors
Apr 20 11:46:57 rsync-41 rsyncd[6078]: rsyncd version 3.1.2 starting, listening on port 873
[root@rsync-41 ~]#
[root@rsync-41 ~]#










systemctl start vsftpd
xxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxx

然后该怎么办？看日志
看日志
看日志
看日志
看日志
看日志
如何看？
如何看？
如何看？

思路1，看vsftpd服务指定的日志文件在哪
思路2， 检查是否运行了，以及它的运行日志部分信息
systemctl status vsftpd

检查是否开机运行
systemctl is-enabled vsftpd








```



6.检查服务是否运行

```
systemctl status rsyncd


# 无论是学习期间还是上班了，都养成好习惯
# 给别人启动了某程序后，给自己启动某程序
务必去检查，验证是否正确

[root@rsync-41 ~]#ps -ef|grep 'rsync' | grep -v 'grep'
root       6078      1  0 11:46 ?        00:00:00 /usr/bin/rsync --daemon --no-detach



[root@rsync-41 ~]#netstat -tunlp|grep rsync
tcp        0      0 0.0.0.0:873             0.0.0.0:*               LISTEN      6078/rsync          
tcp6       0      0 :::873                  :::*                    LISTEN      6078/rsync     


```







第3章 Rsync服务模式-客户端配置
1.安装rsync

```
yum install rsync -y
```



2.配置密码文件及授权

```
此时rsync客户端，需要把数据推送到rsync服务端
但是需要账户认证，这个账户密码，是服务端指定好的
回头去看笔记
rsync_backup
chaoge666

客户端需要做的操作有2个，提供密码认证

1. 生成密码文件，每次连接都指定这个密码文件

2. 生成密码变量，让当前系统中存在叫做 RSYNC_PASSWORD 这个变量，以及变量的值，是配置文件中的密码即可




```

进行客户端数据发送

## 下载，服务端rsync-41的数据





rsync_backup
chaoge666



## 推送，备份，发送nfs-31的数据发给rsync-41

```
把客户端的数据，发送给服务端的backup备份模块下

语法，不一样了，注意语法的写法！！！

吧客户端的 /tmp/200M.log 备份，发送到rsync-41机器上的 backup模块下

rsync -avzP   /tmp/200M.log  账户@主机名::模块名

# 默认无密码变量，也无密码文件，需要你自己输入该rsync_backup虚拟用户的密码
# 需要交互式的输入密码，无法再脚本中使用rsync同步命令
# rsync基本都是和脚本结合使用
rsync -avzP   /tmp/200M.log  rsync_backup@rsync-41::backup


非交互式密码的操作，如下2个方法
1. 生成密码文件，每次连接都指定这个密码文件（在客户端生成）

echo 'chaoge666'  > /etc/my_rsync.pwd
还必须降低密码文件的权限才行，必须是600
chmod 600 /etc/my_rsync.pwd

此时可以传输数据了，往data模块下传输
rsync -avzP  --password-file=/etc/my_rsync.pwd   /tmp/200M.log  rsync_backup@rsync-41::data

如果是脚本中的话，去掉vP显示过程的参数去掉
rsync -az  --password-file=/etc/my_rsync.pwd   /tmp/200M.log  rsync_backup@rsync-41::data



2. 生成密码变量，让当前系统中存在叫做 RSYNC_PASSWORD 这个变量，以及变量的值，是配置文件中的密码即可
export RSYNC_PASSWORD='chaoge666'


rsync -avzP    /tmp/200M.log  rsync_backup@rsync-41::backup





```

## 下载备份服务器的数据

```
rsync -avzP rsync_backup@rsync-41::backup   /tmp/

要输入密码
1.的确没指定密码文件
2.是否有密码变量呢？

如何需要输入密码呢？
撤销这个密码变量
unset RSYNC_PASSWORD
或者重新登录，只要密码变量失效，就必须得输入密码了，或者使用密码文件


rsync -avzP rsync_backup@rsync-41::backup   /tmp/

非交互式的密码认证方式
1，使用密码变量
export RSYNC_PASSWORD='chaoge666'

2.指定密码文件

rsync -avzP --password-file=/etc/my_rsync.pwd  rsync_backup@rsync-41::backup   /tmp/


rsync -avzP --password-file=/etc/my_rsync.pwd  rsync_backup@rsync-41::backup/222222222.log   /tmp/
```

# 关于rsync的常见错误

rsync由于配置步骤比较细节，比较坑比较多，你可能会遇见各种错误

遇见错误如何排查？









练习：
1.客户端推送 /backup 目录下所有内容至 Rsync 服务端
2.客户端拉取 Rsync 服务端 backup 模块数据至本地客户端的 /backup 目录
3.Rsync 实现数据无差异同步
4.Rsync 的 Limit 限速