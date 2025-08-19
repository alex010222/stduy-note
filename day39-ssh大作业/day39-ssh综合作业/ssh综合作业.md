```### 此资源由 58学课资源站 收集整理 ###
	想要获取完整课件资料 请访问：58xueke.com
	百万资源 畅享学习

```
# 根据要求完成部署

根据如下要求，完成部署过程



1.恢复7、8、9、31、41所有机器的快照

```
7 8 9     web服务  nginx

172.16.1.xx


nfs-31  提供共享文件存储


rsync-41 提供数据备份的机器



```



2.在61机器，远程一键脚本化，部署这5台机器

```
master-61机器远程的，操作目标机器，让它安装好对应的服务

web-7  
1.安装软件

2.修改配置文件

3.启动服务

4.挂载nfs



根据服务相关性，需要有先后的部署关系
rsync-41
1.安装软件

2.修改配置文件

3.创建rsync对应的数据目录，配置文件，授权

4.启动服务




nfs-31   +  lrsync实时同步
1.安装软件

2.修改配置文件

3.创建nfs相关的数据目录，授权

3.启动服务

4.安装lsync

5.修改lsync配置文件

6.启动服务





```





3.检查整体应用可用性

```
1.从nginx作为入口，nginx默认的网页根目录 
/usr/share/nginx/html 写入数据

2.同步到nfs机器上


3.同步到rsync机器上


```





学习方法建议

>1.理解部署架构
>
>2.手敲部署过程，别复制粘贴，(1.复制粘贴可能会出现各种语法错误)
>
>否则你永远搞不清其中每一个命令的语法，可能遇见的坑。



# 阶段1：手工部署

备份项目综合架构要求

把你要部署的1,2,3,45流程，捋清楚了，脚本自然也就出来了







![备份项目综合架构要求](pic/%E5%A4%87%E4%BB%BD%E9%A1%B9%E7%9B%AE%E7%BB%BC%E5%90%88%E6%9E%B6%E6%9E%84%E8%A6%81%E6%B1%82.png)



## 完成需求思路

```
1.确认连接方式
2.连接后开始部署


阶段1



阶段2



阶段3




```





Master-61建议登录的别名

```
alias sshweb7='ssh root@172.16.1.7 -p 22999'
alias sshweb8='ssh root@172.16.1.8 -p 22999'
alias sshweb9='ssh root@172.16.1.9 -p 22999'
alias sshnfs31='ssh root@172.16.1.31 -p 22999'
alias sshrsync41='ssh root@172.16.1.41 -p 22999'


写入/etc/profile


```







## windows部分

```
让windows可以免密登录master-61机器

1. windows创建公私钥，默认会存放在什么路径下
~/.ssh/id_rsa
~/.ssh/id_rsa.pub

ssh-keygen 去哪执行
在windows中下载一个支持使用linux命令的工具
git-bash工具

yu@DESKTOP-1TDLFH9 MINGW64 ~/.ssh
$ ls
id_rsa  id_rsa.pub  
known_hosts(存放目标机器的指纹公钥，意义在于？当你下次连接该目标机器的时候，就无序再确认机器的指纹了)

生成公私钥对儿
ssh-keygen -t rsa
yu@DESKTOP-1TDLFH9 MINGW64 ~/.ssh
$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/yu/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/yu/.ssh/id_rsa
Your public key has been saved in /c/Users/yu/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:UIRiwabE0gzUJhD04WzJqA6AmXqNSntfFvxJa+QHKAk yu@DESKTOP-1TDLFH9
The key's randomart image is:
+---[RSA 3072]----+
|BO.o.. oo        |
|o+@oB ..         |
|=+oE ..          |
|+ oo. o..        |
|+.o .o +S+       |
|+o.   . * +      |
|.o .   o * .     |
|  . . o . .      |
|     .           |
+----[SHA256]-----+



发送windows的公钥，给需要免密登录的机器上，目标机器 git-bash执行

ssh-copy-id （这个命令，等于把本地的公钥，写入到目标机器的~/.ssh/）


yu@DESKTOP-1TDLFH9 MINGW64 ~/.ssh
$ ssh-copy-id  root@10.0.0.61
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/c/Users/yu/.ssh/id_rsa.pub"
The authenticity of host '10.0.0.61 (10.0.0.61)' can't be established.
ECDSA key fingerprint is SHA256:Csqwr63+SZRFFOug/IGoFTgRe8hDSI/QalSMBcC6IaU.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@10.0.0.61's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'root@10.0.0.61'"
and check to make sure that only the key(s) you wanted were added.



分别检查，客户端，服务端的，密钥文件信息

windows客户端的，目标机器的公钥
yu@DESKTOP-1TDLFH9 MINGW64 ~/.ssh
$ cat known_hosts
10.0.0.61 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL/Sx3bAaNcKqo7pC4FTYk3gyZ6hd1D/DKUWVfOd4gZb/8XwlAxWauceHe/BAsW5Z8pEmG6AjSyHM8ckOs94c7Y=


linux服务端，可以看到windows机器的，公钥信息

[root@master-61 ~]#cat ~/.ssh/authorized_keys 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCyHEvexmoo7E/dkKjYg66uwo4i2q+dEdZ7nP+eTx4xmY6xUMohsK/LjE5ZuNN8GNPLD92fMGczX3Pz+tFkb4ujVb6yJkqVDVv5P48bZoC+10gYXAjX+mopa058iTKwkS9kcis5Geuomb7aReLnvnoP1q6qYe2rqhYQzLoB9xdjvrWeJ8ay/Z3ON4FR/a5Py2azPFPihIMKuTQasivHyq6BWImD44tLCgZaYrJRKLRge39aFAufxt2nIeeaZIsr55BZavAPzyLqpPez446geMZvEh1wgIZc0+ULSDy55gNXV9m12nBCVEHDUkn7IID9gw7zfMCq2s2eetPldDJnlVQpT2eFtn7nhCbkc0mxn+qCxEgSGQLDmUxnplN9kCBxsQfo5rT35guViMy1emw6dk26tX/sszyW9dFPYENNF0GfZBRw2Haj0zNNH5hoPrh+SLqY6d//fptHBrbgikVN474ewXmNeLThm0sl6BYrw/tUskV+CWW2+emNT49hcGPk4js= yu@DESKTOP-1TDLFH9

这个信息就和windows的 id_rsa.pub

yu@DESKTOP-1TDLFH9 MINGW64 ~/.ssh
$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCyHEvexmoo7E/dkKjYg66uwo4i2q+dEdZ7nP+eTx4xmY6xUMohsK/LjE5ZuNN8GNPLD92fMGczX3Pz+tFkb4ujVb6yJkqVDVv5P48bZoC+10gYXAjX+mopa058iTKwkS9kcis5Geuomb7aReLnvnoP1q6qYe2rqhYQzLoB9xdjvrWeJ8ay/Z3ON4FR/a5Py2azPFPihIMKuTQasivHyq6BWImD44tLCgZaYrJRKLRge39aFAufxt2nIeeaZIsr55BZavAPzyLqpPez446geMZvEh1wgIZc0+ULSDy55gNXV9m12nBCVEHDUkn7IID9gw7zfMCq2s2eetPldDJnlVQpT2eFtn7nhCbkc0mxn+qCxEgSGQLDmUxnplN9kCBxsQfo5rT35guViMy1emw6dk26tX/sszyW9dFPYENNF0GfZBRw2Haj0zNNH5hoPrh+SLqY6d//fptHBrbgikVN474ewXmNeLThm0sl6BYrw/tUskV+CWW2+emNT49hcGPk4js= yu@DESKTOP-1TDLFH9



```







## master-61管理机

```
1.修改ssh端口为22999
2.关闭用户名密码登录
3.开启通过公私钥登录
```

## 被管理机

```
1.修改ssh端口为22999
2.关闭用户名密码登录
3.开启通过公私钥登录
4.指定监听内网地址，172.16.1.xx

```

## 要求部署效果

```
1.master-61机器只能通过公私钥登录，禁止用户密码连接
2.所有主机的ssh端口全都是22999
3.被管理的机器只能通过内网、且使用公私钥连接。
```





# 阶段2：脚本部署ssh



阶段1的ssh环境部署，是手动操作；

现在需要实现脚本一键部署；

```
1.管理机自动创建公私钥
2.管理机自动分发公钥到备管理机
3.远程修改被管理机的ssh连接端口为22999，监听地址是172.16.1.xx
4.远程修改被管理机不允许密码登录，只能是密钥登录
5.修改完毕后，验证是否生效，远程查看所有被管理主机的主机名
```

## 参考写法

- 思路不唯一
- 可优化还很多
- 脚本是一个工艺品，不断打磨，不断完善

## 批量修改配置文件

友情提醒

- 客户端机器需要安装sshpass命令

- ```
  这个sshpass命令只存在master-61机器上即可
  
  实现了公钥面交互分发的命令如下
  在master-61机器上执行 
  ssh-copy-id命令，分发公钥，但是默认需要输入远程机器的密码
  使用 sshpass即可面交互输入密码
  以及面指纹确认的参数 -o StrictHostKeyChecking=no
  
  
  sshpass -p '123123' ssh-copy-id 172.16.1.${ip} -o StrictHostKeyChecking=no > /tmp/create_ssh.log 2>&1
  
  
  
  
  
  ```

- 

- 客户端机器是否允许公钥登录

- ```
  检查目标机器是否允许了公钥登录，一般情况下默认允许的
  
  
  ```

- 

- 目标机器的sshd配置文件是初始化的



```

#1.管理机自动创建公私钥
echo "正在创建公私钥..."
if [ -f /root/.ssh/id_rsa ]
then
  echo "密钥对已经存在,请检查！"
else
  ssh-keygen -f /root/.ssh/id_rsa -N '' > /tmp/create_ssh.log 2>&1
fi

echo '====================分割线=============================='
#2.管理机自动分发公钥到备管理机
echo "正在分发公钥中...分发的机器列表是{7,8,31,41}"
for ip in {7,8,9,31,41}
do
  sshpass -p '123123' ssh-copy-id 172.16.1.${ip} -o StrictHostKeyChecking=no > /tmp/create_ssh.log 2>&1
  echo "正在验证免密登录结果中...."
  echo "远程获取到主机名: $(ssh 172.16.1.${ip} hostname)"
done
echo '====================分割线=============================='

#3.远程修改被管理机的ssh连接端口为22999，监听地址是172.16.1.xx
for ip in {7,8,9,31,41}
do
    echo "修改172.16.1.${ip}的ssh端口中..."
    ssh root@172.16.1.${ip} "sed -i '/Port 22/c Port 22999' /etc/ssh/sshd_config"
done



echo '====================分割线=============================='

#4.远程修改被管理机不允许密码登录，只能是密钥登录
for ip in {7,8,9,31,41}
do
    echo "禁止密码登录参数修改中...当前操作的机器是172.16.1.${ip}"
    ssh root@172.16.1.${ip} "sed -i '/^PasswordAuthentication/c PasswordAuthentication no' /etc/ssh/sshd_config"
    echo "允许公钥登录参数修改中...当前操作的机器是172.16.1.${ip}"
    ssh root@172.16.1.${ip}  "sed -i  '/PubkeyAuthentication/c PubkeyAuthentication yes'  /etc/ssh/sshd_config"
done
echo '====================分割线=============================='
# 5.修改监听内网地址
for ip in {7,8,9,31,41}
do
    echo "修改监听地址中...当前操作的机器是172.16.1.${ip}"
    ssh root@172.16.1.${ip} "sed -i '/ListenAddress 0.0.0.0/c ListenAddress 172.16.1.${ip}' /etc/ssh/sshd_config"
done

echo '====================分割线=============================='

# 6.批量验证ssh修改情况
for ip in {7,8,9,31,41}
do
	echo "当前查看的机器是172.16.1.${ip}"
	ssh root@172.16.1.${ip} "grep -E '^(Port|PasswordAuthentication|PubkeyAuthentication|ListenAddress)' /etc/ssh/sshd_config"
done

echo '====================脚本执行完毕=============================='
```



## 当前完成到了这个里

````
master-61可以免密操作  
7 8 9 31 41这几个机器了

````

还缺少远程的批量重启sshd服务，让sshd_config配置生效



## 批量重启ssh服务验证结果

```
创建验证脚本如下


```



1.批量重启sshd服务

重启服务，单独拆分为了一个脚本，作用就是重启服务

```
for ip in {7,8,9,31,41}
do
    echo "重启sshd服务中，当前操作的机器是172.16.1.${ip}"
    ssh root@172.16.1.${ip} "systemctl restart sshd"
    echo "==========================================="
done
```

重启完毕了服务，验证下修改的结果是否正确，远程查看配置文件信息



2.远程查看主机信息

这个脚本，作用就是远程查看主机的配置文件信息



```
[root@master-61 ~]#cat show_config.sh 
for ip in {7,8,9,31,41}
do
    echo "远程获取主机名中，当前操作的机器是172.16.1.${ip}"
    ssh -p 22999 root@172.16.1.${ip}  "hostname"
    echo "远程获取主机sshd配置信息，当前操作的机器是172.16.1.${ip}"
    ssh -p 22999 root@172.16.1.${ip} "grep -E '^(Port|PasswordAuthentication|PubkeyAuthentication|ListenAddress)' /etc/ssh/sshd_config"
    echo "远程查看sshd端口情况，当前操作的机器是172.16.1.${ip}"
    ssh -p 22999 root@172.16.1.${ip}  "netstat -tunlp|grep sshd|grep -v grep"
    echo "========================分割线============================="
done

```

### 此时还剩下master-61机器未修改了

```
web-7
web-8
web-9

nfs-31
rsync-41

全部完成了 sshd的配置文件修改，修改了
端口
监听地址
禁止密码登录
允许公钥登录



下一步就是该master-61机器的安全性，
禁止密码登录
允许公钥登录即可



```









此时，master-61，以及所有的目标机器以及全部配置好了ssh环境，可以进行服务安装 了

吧你以前部署操作，整理为一个健康的脚本，执行即可







# 阶段3：远程一键安装综合备份架构

- 上述的阶段2，一键搭建好了sshd的安全连接环境
- 只要编写一键安装服务的脚本即可
- 注意服务的启动顺序



## rsync服务

根据刚才的画图理解流程，判断出先部署rsync服务



```
# 1.安装
yum install rsync -y

# 2.配置文件
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
EOF

# 3.创建用户
groupadd www -g 666
useradd www -g 666 -u 666 -M -s /sbin/nologin

# 4.创建目录,授权
mkdir -p /backup
chown -R www.www /backup

# 5.创建密码文件，授权
echo 'rsync_backup:yuchao666' > /etc/rsync.passwd
chmod 600 /etc/rsync.passwd

# 6.启动服务
systemctl start rsyncd
systemctl enable rsyncd

# 7.检查服务
netstat -tunlp|grep rsync
```

远程拷贝、远程安装

```
[root@master-61 ~]#scp -P 22999 install_rsync.sh root@172.16.1.41:/opt/
[root@master-61 ~]#ssh -p 22999 root@172.16.1.41 "bash /opt/install_rsync.sh"


远程检查rsync部署操作
[root@master-61 /0224_scripts]#sshrsync41   "cat /etc/rsync.passwd;ls -ld /backup;id www"
rsync_backup:yuchao666
drwxr-xr-x 2 www www 6 Apr 28 10:39 /backup
uid=666(www) gid=666(www) groups=666(www)

```





## nfs服务(nfs-31)

```
# 0. yum源阿里云yum配置


# 1.安装服务
yum install nfs-utils rpcbind -y

# 2.创建nfs限定的用户、组
groupadd www -g 666
useradd www -g 666 -u 666 -M -s /sbin/nologin

# 3.创建共享目录，修改权限
mkdir /nfs-yuchao-nginx 
chown -R www.www /nfs-yuchao-nginx 

# 4.创建配置文件
cat > /etc/exports <<EOF
/nfs-yuchao-nginx 172.16.1.0/24(rw,sync,all_squash,anonuid=666,anongid=666)
EOF

# 5.启动服务
systemctl start nfs

# 6.检查服务
showmount -e 127.0.0.1
```

远程安装

```
1.远程发送配置文件
[root@master-61 ~]#scp -P 22999  install_nfs.sh root@172.16.1.31:/opt/
install_nfs.sh 

2.远程执行
[root@master-61 ~]#ssh -p 22999 root@172.16.1.31 "bash /opt/install_nfs.sh"



```







## nfs+lsyncd服务

```
# 1.安装服务
yum install lsyncd -y

# 2.生成配置文件
cat >/etc/lsyncd.conf <<EOF
settings {
    logfile      ="/var/log/lsyncd/lsyncd.log",
    statusFile   ="/var/log/lsyncd/lsyncd.status",
    inotifyMode  = "CloseWrite",
    maxProcesses = 8,
    }

sync {
    default.rsync,
    source    = "/nfs-yuchao-nginx",
    target    = "rsync_backup@172.16.1.41::backup",
    delete= true,
    exclude = {".*"},
    delay=1,
    rsync     = {
        binary    = "/usr/bin/rsync",
        archive   = true,
        compress  = true,
        verbose   = true,
        password_file="/etc/rsync.passwd",
        _extra={"--bwlimit=200"}
        }
    }
EOF

# 3.创建密码文件
echo "yuchao666" > /etc/rsync.passwd
chmod 600 /etc/rsync.passwd

# 4.启动
systemctl start lsyncd

# 5.检查服务
ps -ef|grep lsyncd |grep -v grep


```

远程安装lsyncd

```
1.远程发送配置文件
[root@master-61 ~]#scp -P 22999  install_lsyncd.sh root@172.16.1.31:/opt/install_lsyncd.sh 

2.远程执行
[root@master-61 ~]#ssh -p 22999 root@172.16.1.31 "bash /opt/install_lsyncd.sh"



```

## 测试rsync+nfs

```
[root@master-61 ~]#ssh -p 22999 root@172.16.1.31 "touch /nfs-yuchao-nginx/超哥666.png"
[root@master-61 ~]#
[root@master-61 ~]#ssh -p 22999 root@172.16.1.41 "ls /backup"
超哥666.log
超哥666.png

```



## Web7/8/9机器

```
# 1.安装服务
yum install nginx -y

# 2.创建配置文件
cat >/etc/nginx/nginx.conf <<EOF
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}
http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;
    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 4096;
    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;


server {
  listen 81;
  server_name localhost;
  location / {
   	root html;
   	index index.html;
 						 }
			}

}
EOF

# 3.启动服务
systemctl start nginx

# 4.检查服务
netstat -tunlp|grep nginx

# 5.挂载目录
yum install nfs-utils -y
mount -t nfs 172.16.1.31:/nfs-yuchao-nginx /usr/share/nginx/html


```

远程部署

```
[root@master-61 ~]#scp -P 22999 install_nginx.sh  root@172.16.1.7:/opt
[root@master-61 ~]#ssh -p 22999 root@172.16.1.7 "bash /opt/install_nginx.sh"


for server in {7,8,9}
do
	scp -P 22999 install_nginx.sh  root@172.16.1.${server}:/opt
	ssh -p 22999 root@172.16.1.${server} "bash /opt/install_nginx.sh"
done

```

## 最终测试

```
1.在共享存储中，创建网页数据文件，提供给所有web机器使用
cat >index.html<<EOF
<meta charset=utf8>
心若在、梦就在。
于超老师带你学linux，加油吧少年。
EOF


scp -P 22999 index.html root@172.16.1.31:/nfs-yuchao-nginx/





2.检查数据备份情况
ssh -p 22999 root@172.16.1.41 "ls -l /backup"


3.检查网站情况
for web in {7,8,9}
do
	curl 172.16.1.${web}:81
done


4. 浏览器访问
http://10.0.0.7:81/
http://10.0.0.8:81/
http://10.0.0.9:81/

5.再次修改页面，查看数据
cat >index.html<<EOF
<meta charset=utf8>
心若在、梦就在。
于超老师带你学linux，加油吧少年。
EOF
scp -P 22999 index.html root@172.16.1.31:/nfs-yuchao-nginx/

[root@master-61 ~]#ssh -p 22999 root@172.16.1.41 "cat /backup/index.html"
<meta charset=utf8>
心若在、梦就在。
于超老师带你学linux，加油吧少年。

```























