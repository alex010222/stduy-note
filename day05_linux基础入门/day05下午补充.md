```### 此资源由 58学课资源站 收集整理 ###
	想要获取完整课件资料 请访问：58xueke.com
	百万资源 畅享学习

```
# 下午补充

## 终端是什么概念

终端就是一个可以让你操作的地方，输入linux命令的地方，你打开终端，就可以输入指令，发给操作系统。

服务器本身，自带的几个终端。

```
ctrl + alt +  f1 ~f7 组合键

ctrl + alt + f1  ，一个终端，基本对应有一个人再用

ctrl + alt + f2 ~ f7

对应了7个终端
```

xshell这样的终端

## 关于linux命令的语法

![image-20220303144425235](day05%E4%B8%8B%E5%8D%88%E8%A1%A5%E5%85%85.assets/image-20220303144425235.png)



1.一般情况下，【参数】是可选的，一些情况下【文件或路径】也是可选的

```
# 比如ls命令的操作，可自由添加参数，以及后面的文件对象
# 什么都不加的形式
[root@lamp-241 ~]# pwd
/root
[root@lamp-241 ~]# 
[root@lamp-241 ~]# touch ./很不错.txt
[root@lamp-241 ~]# 
[root@lamp-241 ~]# 
[root@lamp-241 ~]# ls
很不错.txt
[root@lamp-241 ~]# ls  .
很不错.txt
[root@lamp-241 ~]# 
[root@lamp-241 ~]# ls  ./
很不错.txt


# 添加选项的形式  -l 列出详细信息 ，以及操作对象的可选
[root@lamp-241 ~]# 
[root@lamp-241 ~]# ls  -l  
total 0
-rw-r--r-- 1 root root 0 Mar  3 22:46 很不错.txt
[root@lamp-241 ~]# 
[root@lamp-241 ~]# 
[root@lamp-241 ~]# ls -l .
total 0
-rw-r--r-- 1 root root 0 Mar  3 22:46 很不错.txt
[root@lamp-241 ~]# 
[root@lamp-241 ~]# 
[root@lamp-241 ~]# ls  -l ./
total 0
-rw-r--r-- 1 root root 0 Mar  3 22:46 很不错.txt
[root@lamp-241 ~]# 
[root@lamp-241 ~]# 
[root@lamp-241 ~]# ls  -l  /root
total 0
-rw-r--r-- 1 root root 0 Mar  3 22:46 很不错.txt


```





2.参数 ， 同一个命令，跟上不同的参数执行不同的功能

```
ls    -l 参数 ，显示详细信息  

可以用ls --help参数，查看ls的参数有哪些，以及作用


组合参数 ，命令，后面可以跟上多个可选参数，写法也有俩
支持组合参数
也支持单独写参数

组合参数 -lh  等于 -l -h
[root@lamp-241 ~]# ls -lh 
查看日志文件的详细信息，与大小
[root@lamp-241 ~]# ls -lh  /var/log/




```

![image-20220303145234706](day05%E4%B8%8B%E5%8D%88%E8%A1%A5%E5%85%85.assets/image-20220303145234706.png)





3.执行linux命令，添加参数的目的是让命令更加贴切实际工作的需要！

```
需要用到什么参数，就添加，否则可以不加
想看到文件的详细信息，就加-l
ls 不加参数，看到文件名即可

```



4.linux命令，参数之间，普遍应该用一个或多个空格分割！

```
[root@lamp-241 ~]# ls       -lh      /var/log/

```

## 关于命令提示符（修改命令提示符）

![image-20220303145932496](day05%E4%B8%8B%E5%8D%88%E8%A1%A5%E5%85%85.assets/image-20220303145932496.png)

```
1.切换用户显示 su -  用户名

```

![image-20220303150238488](day05%E4%B8%8B%E5%8D%88%E8%A1%A5%E5%85%85.assets/image-20220303150238488.png)

```
2. 修改主机名

退出用户登录  logout

hostnamectl  set-hostname  主机名

```



关于最后一个命令提示符，默认表示，用户所处路径的最后一个文件夹

```
[root@linux0224 opt]# cd  /var/log/
[root@linux0224 log]# 

```

![image-20220303151623428](day05%E4%B8%8B%E5%8D%88%E8%A1%A5%E5%85%85.assets/image-20220303151623428.png)

## tab键补全

linux有大量的命令，你记不住，单词

以及有大量的文件路径，你也记住不太长

linux系统，提供了tab补全，让你自动的，补充这些命令，或者补充这些文件路径

```
1.关于命令的补全


2.关于路径的补全
让你找到网卡的配置文件


```

关于命令的补全

![image-20220303151904532](day05%E4%B8%8B%E5%8D%88%E8%A1%A5%E5%85%85.assets/image-20220303151904532.png)

关于文件的补全1

![image-20220303152106974](day05%E4%B8%8B%E5%8D%88%E8%A1%A5%E5%85%85.assets/image-20220303152106974.png)

```
关于文件补全，找到网卡文件的练习
[root@linux0224 ~]# ls  /etc/sysconfig/network-scripts/ifcfg-ens33 


```

## 关于环境变量的学习

打印linux系统上的一个特殊值来看



### 简单图解变量的作用

![image-20220303155832461](day05%E4%B8%8B%E5%8D%88%E8%A1%A5%E5%85%85.assets/image-20220303155832461.png)

### 解读PATH变量

![image-20220303160537471](day05%E4%B8%8B%E5%8D%88%E8%A1%A5%E5%85%85.assets/image-20220303160537471.png)

### 修改PATH变量

> 试一试，去掉 /usr/bin这个路径，你的ls就没法直接使用了。
>
> 这个练习，只是为了让你们理解PATH的作用，你可以不执行，但是跟着练，理解的根深。

```
1. 查看PATH的值
[root@linux0224 ~]# echo ${PATH}
/usr/local/mysql/bin/:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin


2.给PATH重新赋值即可
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/root/bin

3.但是发现了ls这样的命令
没办法，简写去用了，你只能手动的补全它的绝对路径，才行

4.修复PATH变量，加入ls的那个目录
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin"

```



# 布置作业练习































