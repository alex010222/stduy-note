```### 此资源由 58学课资源站 收集整理 ###
	想要获取完整课件资料 请访问：58xueke.com
	百万资源 畅享学习

```
## NFS---共享存储系统

```bash
#network file system 网络文件系统

#NFS主要使用在局域网下，让不同的主机之间可以共享文件、或者目录数据。主要用于linux系统上实现文件共享的一种协议，其客户端主要是Linux。没有用户认证机制，且数据在网络上传送的时候是明文传送，一般只能在局域网中使用。不需要输入账号密码，在配置文件中，定义好可访问该NFS的机器，ip地址即可。还得借助其他用户认证的插件，结合NFS，提高安全性。支持多节点同时挂载及并发写入。
```



## RPC

```bash
#RPC（Remote Procedure Call），远程过程调用，是一种通过网络从远程计算机程序上请求服务，而不需要了解底层网络技术的协议

#本地调用：本地写好一个代码文件，本地运行。
#远程调用：将写好的代码文件放在远程服务器上，在自己电脑上远程调用，执行该代码文件，执行结果会通过网络把数据发回来。
```



## NFS和RPC的关系

```bash
#NFS是通过网络来进行数据传输，因此会使用一些端口来传输数据，但是NFS在传输数据时，使用的端口是随机选择，总是会发生变化，那么NFS在传输数据的时候，就需要通过RPC协议来实现。
```



## NFS工作流程

```bash
1.NFS服务端启动后、将自己的端口信息，注册到rpcbind服务中
2.NFS客户端通过TCP/IP的方式，连接到NFS服务端提供的rpcbind服务，并且从该服务中获取具体的端口信息
3.NFS客户端拿到具体端口信息后，将自己需要执行的函数，通过网络发给NFS服务端对应的端口
4.NFS服务端接收到请求后，通过rpc.nfsd进程判断该客户端是否有权限连接
5.NFS服务端的rpc.mount进程判断客户端是否有对应的操作权限
6.最终NFS服务端会将客户端请求的函数，识别为本地可以执行的命令，传递给内核、最终内核驱动硬件

结论:nfs的客户端、服务端之间的通信基于rpc协议，且必须运行rpcbind服务
```

---

![image-20220423201711130](day35%E4%B8%AA%E4%BA%BA%E7%AC%94%E8%AE%B0.assets/image-20220423201711130.png)



## rpcbind流程

```
* 用户访问网站程序，由程序在NFS客户端上发出存取文件的请求，此时NFS客户端的RPC服务就会通过网络向NFS服务器的RPC服务的111端口发出NFS文件存取功能的请求
* NFS服务器RPC找到对应注册的NFS端口，通知NFS客户端RPC服务
* 此时NFS客户端获取到正确的端口，开始与NFS服务端连接存取数据
* NFS客户端把数据存取成功后返回给前端程序，告知用户存取结果，完成一次存取请求
```



![image-20220423202243428](day35%E4%B8%AA%E4%BA%BA%E7%AC%94%E8%AE%B0.assets/image-20220423202243428.png)



## NFS配置

```
NFS服务配置文件： /etc/exports

语法：NFS共享目录  NFS客户端地址（参数）
```

```
NFS 配置客户端地址类型：

*           ：   所有客户端

172.16.1.7  ：   指定某一个客户端          

172.16.1.7/24 ： 整个网段

192.168.0.0/24(rw)  192.168.1.0/24(ro) ：  代表共享给不同网段


```

```bash
NFS 配置文件参数

ro 只读

rw 读写

root_squash 当nfs客户端以root访问时，它的权限映射为NFS服务端的匿名用户，它的用户ID/GID会变为nfsnobody

no_root_squash 同上，但映射客户端的root为服务器的root，不安全，避免使用

all_squash 所有nfs客户端用户映射为匿名用户，生产常用参数，降低用户权限，增大安全性。

sync 数据同步写入到内存与硬盘，优点数据安全，缺点性能较差

async 数据写入到内存，再写入硬盘，效率高，但可能内存数据会丢



#共享选项：
ro：只读，不常用
rw：读写
sync：实时同步，直接写入磁盘
async：异步，先缓存在内存再同步磁盘
anonuid：设置访问nfs服务的用户的uid，uid需要在/etc/passwd中存在
anongid：设置访问nfs服务的用户的gid
root_squash ：默认选项 root用户创建的文件的属主和属组都变成nfsnobody,其他人nfs-server端是它自己，client端是nobody。
no_root_squash：root用户创建的文件属主和属组还是root，其他人server端是它自己uid，client端是nobody。
all_squash： 不管是root还是其他普通用户创建的文件的属主和属组都是nfsnobody

说明：
请用如下的参数，即可，生产环境用这个

anonuid和anongid参数和all_squash一起使用。

all_squash表示不管是root还是其他普通用户从客户端所创建的文件在服务器端的拥有者和所属组都是nfsnobody；服务端为了对文件做相应管理，可以设置anonuid和anongid进而指定文件的拥有者和所属组



#如：
all_squash ,将web-7的任意用户root,bob01,，在该共享目录下的操作，全部改为nfsnobody以实现权限控制

web-7   /test-nfs   172.16.1.31:/nfs-data  

无论是root去读写 /test-nfs
还是bob01读写 /test-nfs

创建的数据，都会被改为user，group都是 默认的nfsnobody

anonuid=id号

anongid=

集合这俩参数，就可以限制在 该nfs共享目录下的所有用户操作，统一被限制为了某个指定的用户
```



## 注意的点

```bash
[root@nfs-31 ~]#showmount -e 172.16.1.31            #查看指定ip的挂载信息
Export list for 172.16.1.31:
/nfs/share *


[root@nfs-31 /nfs/share]#cat /etc/exports
/nfs/uoload 172.16.1.0/24(rw,sync,all_squash,anonuid=200,anongid=200)
#代表读写权限，同步，全部用户视为nsfnobody进行权限控制，限制在该nfs共享目录下的所有用户操作都映射为uid为200的用户


[root@rsync-41 ~]#mount -t nfs 172.16.1.31:/home/chaoge /nfs_chaoge/
#挂载记得指定nfs 然后目标目录，本地挂载目录


```





## NFS故障案例

1.客户端未挂载NFS

```
[root@web-7 ~]#
[root@web-7 ~]#umount /usr/share/nginx/html
[root@web-7 ~]#


重新挂载
mount -t nfs 172.16.1.31:/nfs-nginx /usr/share/nginx/html/

```

2.服务端出问题，。nfs挂了

导致nginx页面卡死，nginx网页目录操作也都卡死

此时明确了共享存储出问题了

去共享存储NFS服务器上找原因

```
发现nfs挂了，重启即可
systemctl restart nfs
```

3.nfs修复后，客户端的挂载可以恢复



4.如果真的nfs死机了，且暂时无法恢复，你还得快速恢复网站的业务，可以强制取消挂载

```
使用强制卸载参数
，先看看挂载了什么

mount -l |grep nfs

umount -fl 挂载点  # 取消挂载即可

然后最终还是要以恢复NFS为主

```















