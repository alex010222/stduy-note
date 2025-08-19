```### 此资源由 58学课资源站 收集整理 ###
	想要获取完整课件资料 请访问：58xueke.com
	百万资源 畅享学习

```
# linux文件操作篇一

# 学习目标

![image-20220307101724702](day07%E4%BB%8A%E6%97%A5%E7%AC%94%E8%AE%B0.assets/image-20220307101724702.png)

# 学习目标

1、了解文件命名规则和工作中的建议命名规则 

2、会创建和删除目录mkdir/rmdir 

3、会创建和删除文件touch/rm 

4、了解复制cp和移动mv的区别会使用tar命令进行压缩和解压缩，（剪切， 复制）网络中文件的传输，打包传输是最好的

```
1.两个硬盘之间，拷贝大量的数据  10GB
2.传输速率，是有很大幅度，上升，下降 ，  300M/s   17K/s （零散文件太多了）
3. 稳定保持在，你的硬盘设备，最大速率，（打包，压缩,散的文件，整合到一起，再去传输）

数据在服务器之间传输，导致文件丢失，（网络中传输，压缩传输也是必须的）
```



 5、掌握VIM的保存退出wq和不保存强制退出q!掌握VIM的快捷键yy,dd,gg,G,u 

```
修改某程序的配置文件，改参数
给你个文本，如何大量替换，批量替换文本内容
```



6、会使用tail命令查看文件 7、会使用find命令按文件名称查找文件



# 文件命名规则

```
touch  '文件名，写在引号里'

touch my_website.html

touch yuchao_all_html.tgz



1.文件，文件夹，名字，做好见名知意
2.需要分割的时候，用下划线

3. 同一个目录下，文件名唯一


[root@fjh001 ~]# touch Yuanlai0224
[root@fjh001 ~]# ll
total 0
-rw-r--r-- 1 root root 0 Mar  7 18:28 yuanlai0224
-rw-r--r-- 1 root root 0 Mar  7 18:28 Yuanlai0224
[root@fjh001 ~]# 
[root@fjh001 ~]# 
[root@fjh001 ~]# mkdir yuanlai0224 
mkdir: cannot create directory ‘yuanlai0224’: File exists
[root@fjh001 ~]# 
[root@fjh001 ~]# # linux的文件夹，文件名，不得重复 
[root@fjh001 ~]# 
[root@fjh001 ~]# 
[root@fjh001 ~]# mkdir Yuanlai0224 
mkdir: cannot create directory ‘Yuanlai0224’: File exists
[root@fjh001 ~]# 
[root@fjh001 ~]# 
[root@fjh001 ~]# # linux一切皆文件 
[root@fjh001 ~]# 
[root@fjh001 ~]# 
[root@fjh001 ~]# mkdir  yuanlai_dir
[root@fjh001 ~]# 
[root@fjh001 ~]# ll
total 0
-rw-r--r-- 1 root root 0 Mar  7 18:28 yuanlai0224
-rw-r--r-- 1 root root 0 Mar  7 18:28 Yuanlai0224
drwxr-xr-x 2 root root 6 Mar  7 18:29 yuanlai_dir
[root@fjh001 ~]# 
[root@fjh001 ~]# 

```

# 文件管理命令

在日常工作中，我们经常需要对Linux的文件或目录进行操作，常见操作包括

新建 

```
touch 创建文件
mkdir 创建文件夹
vi ,vim 也可以创建文件

echo 结合 重定向符号(>)  才能创建文件 
	echo "男儿当自强"  > /opt/man.txt 
```



删除

```
remove  删除
简写
rm
```



更改

```
修改文件内容的命令很多

vim 
```



查看

```
cat 读取文件内容

```



复制

```
copy 拷贝，缩写的命令，就是 cp
```



移动

```
move 缩写
	mv 
	
剪切
重命名
```

mkdir用法

```
1.用法一：mkdir 不加参数，路径（需要包含目录名称）

[root@fjh001 ~]# mkdir  day07_dir
[root@fjh001 ~]# ls
day07_dir  start.exe  yuanlai0224  Yuanlai0224  yuanlai_dir


2.递归创建写法
[root@fjh001 ~]# mkdir  /opt/0224/linux/chaochao  
mkdir: cannot create directory ‘/opt/0224/linux/chaochao’: No such file or directory
[root@fjh001 ~]# 
[root@fjh001 ~]# 
[root@fjh001 ~]# mkdir -p /opt/0224/linux/chaochao
[root@fjh001 ~]# 
[root@fjh001 ~]# 
[root@fjh001 ~]# tree /opt
/opt
├── 0224
│   └── linux
│       └── chaochao
├── day07_dir
├── happy
└── hello

5 directories, 1 file


3.同时创建多个文件夹，且注意绝对，相对路径
当前目录的 王者

/opt下的lol

/ 根目录下的 dnf

上一级目录的 cs文件夹

先判断你在哪
pwd


再去创建文件，文件夹
[root@fjh001 ~]# mkdir   ./王者  /opt/lol/top/瑞雯    /dnf  ../cs  





```

## 什么时候用-p创建

![image-20220307104344563](day07%E4%BB%8A%E6%97%A5%E7%AC%94%E8%AE%B0.assets/image-20220307104344563.png)

```
[root@fjh001 opt]# 
[root@fjh001 opt]# mkdir  -p    ./王者  /opt/lol/top/瑞雯    /dnf/狂战士     ../cs     
[root@fjh001 opt]# 
[root@fjh001 opt]# 

```



# rmdir删除空目录

目录里面，没有任何文件

```
语法是
rmdir  文件夹的路径

且必须要要求，你要删除的文件夹，里面没数据，方可删除，否则提示，该文件夹不为空

[root@fjh001 opt]# mkdir -p   /opt/0224/男生/程志伟/好朋友/超哥
[root@fjh001 opt]# 
[root@fjh001 opt]# 

比如这个命令，一定是报错的
比如这个命令，一定是报错的
比如这个命令，一定是报错的
[root@fjh001 opt]# rmdir  /opt/0224/男生/程志伟

需要递归删除



```



# touch命令

```
1. 当文件不存在，执行touch 是创建该文本文件

touch  hello.txt


2. 当文件，文件夹（名字）已经存在后，touch命令是修改它的时间戳

touch /opt/


3.touch一次性创建多个文件，注意，要保证，路径中的文件夹是存在的，否则报错
```

![image-20220307105304799](day07%E4%BB%8A%E6%97%A5%E7%AC%94%E8%AE%B0.assets/image-20220307105304799.png)



touch 一次性创建多个文件

```
1.在某个目录，创建多个 同级的文件

2.学习shell的花括号用法 ，一次性在同级目录，创建多个文件
分别适用于touch、mkdir
[root@fjh001 ~]# 
[root@fjh001 ~]# touch /opt/王者/坦克/{老夫子,廉颇,吕布,妲己}
[root@fjh001 ~]# 
[root@fjh001 ~]# 
[root@fjh001 ~]# tree -N /opt/
/opt/
└── 王者
    └── 坦克
        ├── 吕布
        ├── 妲己
        ├── 廉颇
        └── 老夫子

2 directories, 4 files
[root@fjh001 ~]# 
[root@fjh001 ~]# mkdir   -p   /opt/lol/中单/{快乐风男,儿童劫,提款机}
[root@fjh001 ~]# 
[root@fjh001 ~]# 



3.使用tree命令，查看文件目录结构
需要安装后使用 1.机器可以上外网 2.用命令安装 yum install  tree -y   # 自动下载安装tree这个软件（命令）

4.使用tree查看目录结构，且显示中文，且显示该文件的类型
tree -NF      # -N 是显示中文 -F 显示文件类型


```

![image-20220307110054356](day07%E4%BB%8A%E6%97%A5%E7%AC%94%E8%AE%B0.assets/image-20220307110054356.png)

### 关于花括号用法 {} 结合touch命令

```
1.创建多个连续的文件，有规律的文件   1 2 3 4 5 
[root@fjh001 快乐风男]# touch 玩家{1..100}.log


```

## rm删除命令



## 虚拟机的快照

为了防止你删库的

- vmware提供了快照功能
  - 游戏存档，1
  - 游戏存档，2
  - 游戏存档，3
  - 游戏进行中
- linux系统
  - 系统快照1，系统刚初始化好
  - 系统快照2，安装了数据库(回到这个系统的快照，回到这个状态)
  - 系统快照3，升级数据库（假如在这报错了，数据库挂了，升级软件，系统中有很多相关的软件版本也都升级）

![image-20220307111935132](day07%E4%BB%8A%E6%97%A5%E7%AC%94%E8%AE%B0.assets/image-20220307111935132.png)

添加快照

![image-20220307112037669](day07%E4%BB%8A%E6%97%A5%E7%AC%94%E8%AE%B0.assets/image-20220307112037669.png)



# 关于rm命令的学习

权限最大化

root + rm（参数）

```
rm 命令和其他一样

rm (remove移除)

语法是
rm   可选参数  可选对象



-r ：递归删除，主要用于删除目录，可删除指定目录及包含的所有内容，包括所有子目录和文件

-f ：强制删除，不提示任何信息。操作前一定要慎重！！！不小心你就删库跑路（放心，跑不掉的）

-i  ：删除前需要确认



（别慌，孰能生巧，学习期间，你的虚拟机你随便删，前提是你做好快照！！删腻了，你上班就不会出错了）


1.rm命令不带参数，只删除1个文件
[root@fjh001 快乐风男]# rm   玩家100.log 
rm: remove regular empty file ‘玩家100.log’? yes

2. rm命令删除多个文件
[root@fjh001 快乐风男]# rm  ./玩家10.log   /opt/lol/中单/快乐风男/玩家20.log 
rm: remove regular empty file ‘./玩家10.log’? yes
rm: remove regular empty file ‘/opt/lol/中单/快乐风男/玩家20.log’? yes
[root@fjh001 快乐风男]# 



3. 当你想强制删除文件，不要去提示了怎么办？
-f 强制删除
[root@fjh001 快乐风男]# ls
玩家11.log  玩家17.log  玩家23.log  玩家29.log  玩家40.log  玩家46.log  玩家51.log  玩家57.log  玩家62.log  玩家68.log  玩家73.log  玩家79.log  玩家84.log  玩家8.log   玩家95.log
玩家12.log  玩家18.log  玩家24.log  玩家36.log  玩家41.log  玩家47.log  玩家52.log  玩家58.log  玩家63.log  玩家69.log  玩家74.log  玩家7.log   玩家85.log  玩家90.log  玩家96.log
玩家13.log  玩家19.log  玩家25.log  玩家37.log  玩家42.log  玩家48.log  玩家53.log  玩家59.log  玩家64.log  玩家6.log   玩家75.log  玩家80.log  玩家86.log  玩家91.log  玩家97.log
玩家14.log  玩家1.log   玩家26.log  玩家38.log  玩家43.log  玩家49.log  玩家54.log  玩家5.log   玩家65.log  玩家70.log  玩家76.log  玩家81.log  玩家87.log  玩家92.log  玩家98.log
玩家15.log  玩家21.log  玩家27.log  玩家39.log  玩家44.log  玩家4.log   玩家55.log  玩家60.log  玩家66.log  玩家71.log  玩家77.log  玩家82.log  玩家88.log  玩家93.log  玩家99.log
玩家16.log  玩家22.log  玩家28.log  玩家3.log   玩家45.log  玩家50.log  玩家56.log  玩家61.log  玩家67.log  玩家72.log  玩家78.log  玩家83.log  玩家89.log  玩家94.log  玩家9.log
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# rm -f 玩家11.log 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# rm -f 玩家12.log 
[root@fjh001 快乐风男]# rm -f 玩家13.log 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# rm -f 玩家{1..100}.log
[root@fjh001 快乐风男]# ls
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# rm -f 玩家1.log
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# rm -f 玩家2.log
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# rm -f 玩家3.log
[root@fjh001 快乐风男]# 

4. 关于rm的文件夹删除，需要-r参数使用
[root@fjh001 快乐风男]# rm 亚索
rm: cannot remove ‘亚索’: Is a directory
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# rm -r   亚索
rm: descend into directory ‘亚索’? y
rm: descend into directory ‘亚索/武器’? y
rm: remove directory ‘亚索/武器/武士刀’? y
rm: remove directory ‘亚索/武器’? y
rm: remove directory ‘亚索’? y
[root@fjh001 快乐风男]# ls
[root@fjh001 快乐风男]# 


5.递归，强制删除文件夹
[root@fjh001 快乐风男]# mkdir -p 儿童劫/小学生/空三个大
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# tree -N
.
└── 儿童劫
    └── 小学生
        └── 空三个大

3 directories, 0 files
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# rm -r  -f  ./儿童劫
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# ls


6.危险命令，注意rm命令后面，到底跟着的路径是什么，错一个字符，就删错了，没有回头路

7.确保虚拟机快照备份完毕

8.人生第一次，删除linux所有资料（注意，此操作，不要在你的虚机以外任何地方执行，比如你的同桌的linux）










```

关于rm删除文件

![image-20220307113012219](day07%E4%BB%8A%E6%97%A5%E7%AC%94%E8%AE%B0.assets/image-20220307113012219.png)





## ln命令

ln是link的意思，表示创建一个快捷方式，如同你windows的图标快捷方式



## alias命令（别名命令）

昵称，别名的意思

于超，超弟，超哥，小于

```
alias在系统中是怎么用的呢？

1.查看系统的默认别名，alias
[root@fjh001 快乐风男]# alias
alias cp='cp -i'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias mv='mv -i'
alias rm='rm -i'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
[root@fjh001 快乐风男]# 


2.修改关于rm的 别名
你可以自由修改rm的别名，如修改语法
[root@fjh001 快乐风男]# alias  rm='rm -i'
[root@fjh001 快乐风男]# 


3.你可以自己去定义这样的，好用的别名



```

![image-20220307114538409](day07%E4%BB%8A%E6%97%A5%E7%AC%94%E8%AE%B0.assets/image-20220307114538409.png)

---

![image-20220307115131891](day07%E4%BB%8A%E6%97%A5%E7%AC%94%E8%AE%B0.assets/image-20220307115131891.png)





# cp拷贝命令

```
1.拷贝文件，且改名
[root@fjh001 快乐风男]# cp  /opt/lol/中单/快乐风男/很菜的亚索.txt    /home/突然很强的亚索.txt


2.仅仅拷贝单个文件，保持源文件名
[root@fjh001 快乐风男]# cp ./很菜的亚索.txt  /   


3.拷贝文件夹，以及递归拷贝操作
cp -r 源文件夹路径   目标文件夹路径
```

## 关于cp拷贝文件夹的坑

1.在/home下，是没有这个 英雄联盟文件夹的

![image-20220307115958523](day07%E4%BB%8A%E6%97%A5%E7%AC%94%E8%AE%B0.assets/image-20220307115958523.png)



2. 演示，在/home下以及存在，同名的文件夹了，是啥样？

![image-20220307120338359](day07%E4%BB%8A%E6%97%A5%E7%AC%94%E8%AE%B0.assets/image-20220307120338359.png)





# 移动、剪切、重命名(mv命令)

```
1.从A目录，移动到B目录，移动单个文件
[root@fjh001 快乐风男]# mv  ./蔡文姬.txt /opt/lol/中单
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# pwd
/opt/lol/中单/快乐风男
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# ls
很菜的亚索.txt  摇.txt
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# ls /opt/lol/中单/
儿童劫  快乐风男  提款机  蔡文姬.txt


2.mv结合相对路径去移动
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# mv  ../蔡文姬.txt   /opt
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# tree -N /opt/
/opt/
├── lol
│   └── 中单
│       ├── 儿童劫
│       ├── 快乐风男
│       │   ├── 很菜的亚索.txt
│       │   └── 摇.txt
│       └── 提款机
├── 王者
│   └── 坦克
│       ├── 吕布
│       ├── 妲己
│       ├── 廉颇
│       └── 老夫子
└── 蔡文姬.txt



3. 文件的重命名，在当前目录，重命名
[root@fjh001 快乐风男]# mv 很菜的亚索.txt   努力学习怎么放大招的亚索.txt 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# ls
努力学习怎么放大招的亚索.txt  摇.txt


4.移动文件目录，且重命名
[root@fjh001 快乐风男]# mv  ./努力学习怎么放大招的亚索.txt /opt/垃圾压缩.txt
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# 
[root@fjh001 快乐风男]# tree -N /opt/
/opt/
├── lol
│   └── 中单
│       ├── 儿童劫
│       ├── 快乐风男
│       │   └── 摇.txt
│       └── 提款机
├── 垃圾压缩.txt
├── 王者
│   └── 坦克
│       ├── 吕布
│       ├── 妲己
│       ├── 廉颇
│       └── 老夫子
└── 蔡文姬.txt

7 directories, 7 files



5.移动文件夹（剪切）
[root@fjh001 opt]# # 移动 /opt/lol 到根目录去
[root@fjh001 opt]# 
[root@fjh001 opt]# 
[root@fjh001 opt]# mv  /opt/lol/  /


6.移动文件夹，且改名字



```

## 这里也有关于文件夹是否存在的坑

![image-20220307121828756](day07%E4%BB%8A%E6%97%A5%E7%AC%94%E8%AE%B0.assets/image-20220307121828756.png)







# 压缩、解压缩

```
打包，默认是没有压缩功能，不节省磁盘空间

打包+压缩，将一堆零散的文件，打包到一起，之后再压缩，节省磁盘空间。
```



# tar命令

官网.tgz 

all_databases.tgz 



```
打包
命令：tar

作用：将多个文件打包成一个文件

语法：tar 选项 打包之后的文件名    要打包的文件或目录1 要打包的文件或目录2  要打包的文件或目录3 

常见参数：
用不同的参数，有不同的作用
tar实现，到底是打包，还是压缩，或者是解压缩，就看给的参数是什么.


 -c，create 创建的意思 ，打包

​ -v，显示打包文件过程

​ -f，指定打包的文件名，此参数是必须加的，且必须在最后一位

​ -u，update缩写，更新原打包文件中的文件（了解）

​ -t，查看打包的文件内容（了解）  （不解压，看看里面有什么）

-x  解包，解压缩  （将一个单个的压缩文件，解压其中内容）

-z  压缩操作，是tar命令，去调用gzip命令的过程，压缩的参数

提示：

tar命令打包的文件，通常称为tar包，如 yuchao-all.tar文件
提问：

​ 这个.tar是个谁看的？是给centos看的，还是给运维超哥看的？
```

实践



## 压缩文件名的规范

>用tar命令压缩的文件，一般后缀如

```
*.tar   仅仅是打包了

*.tar.gz  打包+压缩

*.tgz    打包+压缩


```





## tar的打包用法

```
1.准备一堆 比较大的文件，以及文件夹

打包多个文件

先生成四个测试的数据文件
[root@fjh001 0224_day07]# echo 机器人{1..500000}号 >> robot.txt
[root@fjh001 0224_day07]# echo 机器人{1..500000}号 >> robot.txt
[root@fjh001 0224_day07]# echo 机器人{1..500000}号 >> robot.txt
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# ll -h
total 66M
-rw-r--r-- 1 root root 36M Mar  7 22:30 robot.txt
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# echo 机器人{1..900000}号 >> robot.txt
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# ll -h
total 66M
-rw-r--r-- 1 root root 53M Mar  7 22:30 robot.txt
[root@fjh001 0224_day07]# cp robot.txt robot.txt1 
[root@fjh001 0224_day07]# cp robot.txt robot.txt2
[root@fjh001 0224_day07]# cp robot.txt robot.txt3
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# ll -h
total 209M
-rw-r--r-- 1 root root 53M Mar  7 22:30 robot.txt
-rw-r--r-- 1 root root 53M Mar  7 22:31 robot.txt1
-rw-r--r-- 1 root root 53M Mar  7 22:31 robot.txt2
-rw-r--r-- 1 root root 53M Mar  7 22:31 robot.txt3
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# 


2.将这个四个文件，打包到一起 （打包，整合到一起）
[root@fjh001 0224_day07]# # tar  参数  打包文件名   你要打包的内容
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# tar -cvf all_robots.tar  robot.txt robot.txt1 robot.txt2 robot.txt3  
robot.txt
robot.txt1
robot.txt2
robot.txt3
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# ll -h
total 417M
-rw-r--r-- 1 root root 209M Mar  7 22:34 all_robots.tar
-rw-r--r-- 1 root root  53M Mar  7 22:30 robot.txt
-rw-r--r-- 1 root root  53M Mar  7 22:31 robot.txt1
-rw-r--r-- 1 root root  53M Mar  7 22:31 robot.txt2
-rw-r--r-- 1 root root  53M Mar  7 22:31 robot.txt3


3.查看打包文件中的内容
[root@fjh001 0224_day07]# tar -tf  all_robots.tar 
robot.txt
robot.txt1
robot.txt2
robot.txt3


4.解包，拆包，拆开打包文件（拆快递）
[root@fjh001 0224_day07]# tar -xvf all_robots.tar 
robot.txt
robot.txt1
robot.txt2
robot.txt3
[root@fjh001 0224_day07]# ll
total 426144
-rw-r--r-- 1 root root 218183680 Mar  7 22:34 all_robots.tar
-rw-r--r-- 1 root root  54544475 Mar  7 22:30 robot.txt
-rw-r--r-- 1 root root  54544475 Mar  7 22:31 robot.txt1
-rw-r--r-- 1 root root  54544475 Mar  7 22:31 robot.txt2
-rw-r--r-- 1 root root  54544475 Mar  7 22:31 robot.txt3


5.更新，加入一个文件到打包文件中
[root@fjh001 0224_day07]# tar -uf all_robots.tar test1.log 
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# ll -h
total 365M
-rw-r--r-- 1 root root 287M Mar  7 22:39 all_robots.tar
-rw-r--r-- 1 root root  78M Mar  7 22:38 test1.log



```

## tar的压缩用法

```
1.打包且压缩

打包，不压缩的时候，是287M
[root@fjh001 0224_day07]# ll -h
total 573M
-rw-r--r-- 1 root root 287M Mar  7 22:39 all_robots.tar
-rw-r--r-- 1 root root  53M Mar  7 22:30 robot.txt
-rw-r--r-- 1 root root  53M Mar  7 22:31 robot.txt1
-rw-r--r-- 1 root root  53M Mar  7 22:31 robot.txt2
-rw-r--r-- 1 root root  53M Mar  7 22:31 robot.txt3
-rw-r--r-- 1 root root  78M Mar  7 22:38 test1.log


2.打包且压缩的用法
[root@fjh001 0224_day07]# tar -czvf all_files.tar.gz   ./*   
./robot.txt
./robot.txt1
./robot.txt2
./robot.txt3
./test1.log
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# ll -h
total 324M
-rw-r--r-- 1 root root 38M Mar  7 22:42 all_files.tar.gz
-rw-r--r-- 1 root root 53M Mar  7 22:30 robot.txt
-rw-r--r-- 1 root root 53M Mar  7 22:31 robot.txt1
-rw-r--r-- 1 root root 53M Mar  7 22:31 robot.txt2
-rw-r--r-- 1 root root 53M Mar  7 22:31 robot.txt3
-rw-r--r-- 1 root root 78M Mar  7 22:38 test1.log


3.除了-z的压缩参数，还有哪些   windows下的 .zip  .rar  .7z 

-z，压缩为.gz格式 ，记住用这个就好了，主流的80%人都用这个
	你拿到一个 all_files.tar.gz ，这个如何解压？
	tar  -zxvf  all_files.tar.gz

-j，压缩为.bz2格式   
	all_files.tar.bz2   ，如何解压？
    tar -xjvf all_files.tar.bz2 
	
	

-J，压缩为.xz格式
	

-c，create 创建的意思

-x，解压缩

-v，显示打包文件过程

-f，file指定打包的文件名，此参数是必须加的。

-u，update缩写，更新原打包文件中的文件（了解）

-t，查看打包的文件内容（了解）



```

## 如果压缩文件名字呗改了，还能用吗？

能用

你能在互联网上下载的各类软件，基本都是如下俩格式

- mysql.tar.gz
- nginx.tgz



```
1.linux文件名，不重要，具体文件内容，由其属性决定

[root@fjh001 0224_day07]# mv all_files.tar.gz all_files
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# ll
total 38780
-rw-r--r-- 1 root root 39709097 Mar  7 22:42 all_files
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# ll -h
total 38M
-rw-r--r-- 1 root root 38M Mar  7 22:42 all_files
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# vim all_files 
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# ll
total 38780
-rw-r--r-- 1 root root 39709097 Mar  7 22:42 all_files
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# tar -zxvf all_files 
./robot.txt
./robot.txt1
./robot.txt2
./robot.txt3
./test1.log
[root@fjh001 0224_day07]# ll
total 331716
-rw-r--r-- 1 root root 39709097 Mar  7 22:42 all_files
-rw-r--r-- 1 root root 54544475 Mar  7 22:30 robot.txt
-rw-r--r-- 1 root root 54544475 Mar  7 22:31 robot.txt1
-rw-r--r-- 1 root root 54544475 Mar  7 22:31 robot.txt2
-rw-r--r-- 1 root root 54544475 Mar  7 22:31 robot.txt3
-rw-r--r-- 1 root root 81777792 Mar  7 22:38 test1.log
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# mv all_files all_files.tgz
[root@fjh001 0224_day07]# ll
total 331716
-rw-r--r-- 1 root root 39709097 Mar  7 22:42 all_files.tgz
-rw-r--r-- 1 root root 54544475 Mar  7 22:30 robot.txt
-rw-r--r-- 1 root root 54544475 Mar  7 22:31 robot.txt1
-rw-r--r-- 1 root root 54544475 Mar  7 22:31 robot.txt2
-rw-r--r-- 1 root root 54544475 Mar  7 22:31 robot.txt3
-rw-r--r-- 1 root root 81777792 Mar  7 22:38 test1.log
[root@fjh001 0224_day07]# 
[root@fjh001 0224_day07]# 

```

# zip压缩

zip压缩目录，需要添加-r参数

> zip仅仅压缩多个文件的用法

![image-20220307151358483](day07%E4%BB%8A%E6%97%A5%E7%AC%94%E8%AE%B0.assets/image-20220307151358483.png)

> zip压缩文件，和文件夹

```
[root@fjh001 0224_day07]# zip -r  all_files.zip  robot/ test1.log 
  adding: robot/ (stored 0%)
  adding: robot/robot.txt (deflated 87%)
  adding: robot/robot.txt1 (deflated 87%)
  adding: robot/robot.txt2 (deflated 87%)
  adding: robot/robot.txt3 (deflated 87%)
  adding: test1.log (deflated 87%)
[root@fjh001 0224_day07]# 

```





# unzip（解压缩）

官网源码.zip

unzip 官网源码.zip  -d 指定解压缩到哪



```
1.该命令，可能需要安装，用如下命令
[root@fjh001 0224_day07]# yum install unzip -y


2.解压缩，并且指定解压到一个地方 -d 
[root@fjh001 ~]# unzip /all_files.zip -d /0224_day07/
Archive:  /all_files.zip
   creating: /0224_day07/robot/
  inflating: /0224_day07/robot/robot.txt  
  inflating: /0224_day07/robot/robot.txt1  
  inflating: /0224_day07/robot/robot.txt2  
  inflating: /0224_day07/robot/robot.txt3  
  inflating: /0224_day07/test1.log   
[root@fjh001 ~]# 
[root@fjh001 ~]# 



```



# vim快速上手

1.所有你可见到的linux机器，都会默认有vi编辑器，但是它不好用，就好比windows的记事本。

2.你可以选择更强大的编辑器 ，叫vim，需要额外安装

```
yum install vim -y
```



## 学习vim的使用流程

```
1.安装vim
yum install vim -y

2.学习它的几个模式，运转流程
```



图解vim的流程

![image-20220307153213889](day07%E4%BB%8A%E6%97%A5%E7%AC%94%E8%AE%B0.assets/image-20220307153213889.png)

```
查看该文件内容
[root@fjh001 ~]# vim gushi.txt
[root@fjh001 ~]# 
[root@fjh001 ~]# 
[root@fjh001 ~]# cat gushi.txt 
古诗
清明时节雨纷纷
路上行人欲断魂
借问美女何处有
路人遥指三里屯


```





































