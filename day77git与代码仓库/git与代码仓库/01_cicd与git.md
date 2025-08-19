```### 此资源由 58学课资源站 收集整理 ###
	想要获取完整课件资料 请访问：58xueke.com
	百万资源 畅享学习

```
# 1.目前我们怎么部署项目的？

```perl
运维，公司对语言的选择，千变万化


v1 陌陌APP只支持聊天功能


1. 拿到源代码
怎么拿？
	公司会部署代码仓库，进行源代码管理（github，gitlab，gitee码云）
	开发会吧代码传到这个仓库里
	运维，测试去下载这个源码
	php代码写的
	
	
	

2. 准备机器环境（测试服务器，线上服务器）
	为什么要有测试环境(公司内部的，内侧环境，社会用户是访问不了)？
		软件代码必然会有bug，需要层层测试通过后，确认bug很少了，可以达到上线标准了
		【vip充值功能，才能查看小姐姐的联系方式，bug，没做好用户的权限校验，不充钱也能看】
	软件达到的上线标准，
		才会进入到生产服务器的部署（ip，域名，是面向社会用户）
	linux机器 + mysql + php-fpm + nginx
	

3. 启动
	参考你理解的wordpress，就是部署php源码程序的流程，到这听懂111111


4. 项目开始运行再线上，可能会出现N种未知的错误，出了问题，得公司的技术团队，支持解决
	拿钱干活，日常到底要干啥，
	- 运维，主要就是软件发版时忙一点，准备上线的环境
	- 日常的项目运行中，对服务器的维护，通过搭建监控系统等，确保系统的可用性
	听懂111111

	黑客攻击，发送了大量的请求，无用的请求
	线上代码运行，代码是当用户访问到，基于某个url，请求，nginx代理转发给了某个模块
	（你不点登录，会触发哪个登录代码吗？登录的代码，是有bug的，但是你运行项目时，没执行该功能代码，因此项目时可以运行起来，但是不代表项目时正确）
	因此公司才需要招测试工程师去一个个功能的点击，最终确认该软件是没问题的
	基于这个理念，网站上线了，有某个优惠券的功能，开始发现不了有bug，但是再用户（也可以作为一个测试的角色去理解），代码触发执行，发生了bug，发现如优惠券，无法用的bug，F12抓请求，发现请求是500了，因此公司团队的，运维，开发，介入，修复 bug，重启，重新上线。
	这段概念，理解666666
	
	
	
5. 后期的项目更新,网站加了新功能，陌陌这个公司，APP，仅仅是聊天交友，得上线直播板块功能
	- 软件发版更新
		1. 开发，推送v2版本的代码，到代码仓库
		2. 下载代码，手动，或者shell脚本，将代码上传至目标服务器，干掉旧进程，替换源代码，重启新进程，确保软件更新
		3. 测试访问是否正常。
		
		看懂333333

6. 疑问？为什么要学这个cicd章节？

	如果你负责的业务太多，陌陌玩过把（app的业务功能），N个功能，N个板块，N套源代码，运行了N个进程，需要运维去维护。
	运维，可能要维护多个系统，上述流程，都需要你去，上线准备环境，准备机器，安装软件，改配置，重启，更新，发版，等。、。。。。。
	
	php 启动，部署的方式都不一样
	python 启动，部署的方式都不一样
	java  启动，部署的方式都不一样


2.学习完毕cicd之后如何部署？按如下思路来。



	必须得至少通过shell脚本完成,能实现环境的批量化，可复制操作。 111111
	
	还有就是更新，上线等一个流水线的操作，手动就别想了，那么人家陌陌公司，中大型公司，业务量大，就必须要求技术团队的能力要跟得上业务，
	
	要求，运维，都得懂些开发的知识，打造devsop，运维开发流水线团队。
	部署私有代码仓库，构建工具，实现，鼠标一点，就更新
	运维能实现，前期把构建项目的环境准备好，后期，连鼠标都可以扔了，不用点。
	开发推送代码，运维搭建的这个流水线，自动将代码更新到测试，线上环境，。
	运维只需要躺着喝咖啡，看日志就行。
	这套系统学不学。
	学1111111

	
	
	是一个小公司，5个左右的业务，招1,2普通运维，手工维护也差不多能行

	
那么，本阶段的课程，正式开始。休息片刻，理解下业务层面的知识。






```



devops运维工程师进阶之路

![image-20220714103048241](pic/image-20220714103048241.png)



# 2.什么是版本控制系统

1.当公司的服务器架构越来越复杂，需要频繁的发布新配置文件，以及新代码；如何确保多个版本的存在？

```
引入版本控制系统，的工作中应用


微信的APP，软件发布，源代码的版本管理

v1 版本支持 "语音功能"
v2 版本 "支持语音转文字功能"
v3 版本，"支持视频聊天功能"


运维的配置文件管理

v1 nginx.conf "支持首页www的虚拟主机配置"

v2 nginx.conf  "支持三个业务的虚拟主机配置，本次添加了3个server{}配置"


1111

具体版本控制系统，能实现什么效果 ，接着看



```



2.但是如果机器部署数量较多，发布的效率必然很低；（自动化构建系统）





devops，产品经理需要去定义的，开发模式。

软件开发理论。开发运维测试，一套流水线



3.并且如果代码没有经过测试环境，预生产环境层层测试，最终才到生产环境，不经过测试的部署，会导致很严重的bug，因此必须要进行一定的代码测试。（再运维构建系统中，还支持对代码的扫描，检测，bug检测，如漏洞检测。）

```
1. 开发提交了一堆代码 *.php，提交到代码仓库

2. 运维部署的构建系统，下载代码，自动化测试，扫描代码（大多数bug，漏洞全扫出来，只有通过了代码的质量检查之后，下一步的动作，才进入，部署到具体的生产服务器上

```



因此，再互联网公司中的开发，模式，就是 开发 > 测试 >运维，一套流水线，确保软件的高质量发布，切频繁发布，频繁迭代更新。

这就是devops文化，



因此从devops部署理念来看，任何一家单位都必须实现CICD（持续集成、持续交付）的理念，实现自动化代码集成、自动化代码部署。

```
如何落地，打造一个devops开发团队呢？
让开发，和运维再同一个组，运维也得懂开发的一些知识，部署软件更新流水线，能实现，快捷的软件发布，软件更新，软件测试。

学会了这套系统的运维，就是devops运维工程师了。、



目前主流的解决方案，学习的技术就是

git       代码版本控制系统
jenkins   代码构建系统
gitlab    私有代码仓库
sonarqube 代码扫描工具



这套理念，大致听懂6666



```









# 3.你以后的成长之路是如何？



![image-20220714104921597](pic/image-20220714104921597.png)





![image-20220714105429077](pic/image-20220714105429077.png)



```
1.今天的学习重点

1. 是理论，理解devops高级运维工程，要懂得架构体系知识

2. 学习git工具


```







![image-20220714110122452](pic/image-20220714110122452.png)





# 4.开始学习自动化构建系统组件之一，git



![image-20220714111920286](pic/image-20220714111920286.png)为什么要版本记录？不用行吗？



![image-20220714112210413](pic/image-20220714112210413.png)



当你学了git工具之后，对linux的源码管理，配置文件管理

可以用在你自己的windows机器上，

word文档版本管理

execel文档

markdown笔记，版本管理，都可以通过git实现多个版本的管理，

历史版本回退。



![image-20220714112602469](pic/image-20220714112602469.png)





![image-20220714112815377](pic/image-20220714112815377.png)

什么是版本控制系统



# 什么是cicd，俩名词

##  ci ，持续集成

![image-20220714113426141](pic/image-20220714113426141.png)





## 什么是ci/cd

在源代码，持续合并多次的过程中，每次合并，到master主干线，都要

![image-20220714113743773](pic/image-20220714113743773.png)







# 具体进行源码的版本管理，有哪些软件呢？

- svn老旧，非互联网公司可能用，矿场，机场，传统行业电子制造业，软件系统
  - 源码管理
- git工具，所有互联网公司必备（linux的老父亲，林纳斯托瓦兹开发的，开发git工具）
  - linux内核源码，



![image-20220714114229075](pic/image-20220714114229075.png)



### 什么是svn，集中式的版本控制系统

不好用

![image-20220714114618875](pic/image-20220714114618875.png)



# 分布式版本控制系统git的强大

![image-20220714114944718](pic/image-20220714114944718.png)



```
git工具，基本是开发工程师去写代码，进行的多版本维护的使用

运维，可以基于git实现对自己配置文件的多版本管理

运维主要就是学习
如何对一个git代码仓库，进行命令操作

查看版本日志
版本提交
版本回退
这些操作


```



![image-20220714115518456](pic/image-20220714115518456.png)





12.10继续

休息会

课间，思考下，理解理解git的图



```
这个git的架构，还是有一定的理解难度的

等学完了git的实际操作，分支，远程仓库后，即可理解


```







# 5.git全流程



以学习git工具为主，实践





## linux安装

```
yum install git -y

[root@web-7 ~/git-learn]#git --version
git version 1.8.3.1



```



## 身份设置

![image-20220714121346250](pic/image-20220714121346250.png)

```
一般用如下命令，设置身份即可
# 给git设置配置信息 --global 参数，身份信息，会写入 ~/.gitconfig


git config --global user.name "wenjie"
git config --global user.email "wenjie01@163.com"
# 开启git命令的颜色支持
git config --global color.ui true

查看配置文件
cat ~/.gitconfig

查看当前机器的git身份配置信息
[root@web-7 ~/git-learn]#cat ~/.gitconfig
[user]
	name = wenjie
	email = wenjie01@163.com
[color]
	ui = true


[root@web-7 ~/git-learn]#git config --list
user.name=wenjie
user.email=wenjie01@163.com
color.ui=true


看懂刷  222222



```



![image-20220714121624656](pic/image-20220714121624656.png)











## git三个使用场景

![image-20220714121936612](pic/image-20220714121936612.png)

```
git能够记录，源代码目录中，所有文件的状态，这个记录的数据，都在 .git这个隐藏文件夹中

```



使用git命令，有3个场景



### 玩法1，本地已经写好了代码，需要用git去管理

git的使用流程套路

```
1. git init . 初始化本地仓库

2.  写代码，如 hello.sh

3. 通过git add . 进行本地文件追踪管理，文件信息被记录到 暂存区


4.  git commit -m '注释'  ，将暂存区的数据，提交到local repo  本地仓库，进行第一个版本的记录



```







```
[root@web-7 ~/git-learn/my_shell]#pwd
/root/git-learn/my_shell
[root@web-7 ~/git-learn/my_shell]#
[root@web-7 ~/git-learn/my_shell]#ls
hello.sh
[root@web-7 ~/git-learn/my_shell]#
[root@web-7 ~/git-learn/my_shell]#
[root@web-7 ~/git-learn/my_shell]#ls -a
.  ..  hello.sh


没有被git进行管理

使用git进行，从0 到1管理


1. 初始化生成 .git文件夹
[root@web-7 ~/git-learn/my_shell]#git init .
初始化了一个空的git仓库 再 /root/git-learn/my_shell/.git/
Initialized empty Git repository in /root/git-learn/my_shell/.git/

查看这个.git本地仓库的文件夹，有什么内容
[root@web-7 ~/git-learn/my_shell]#ls .git
branches  config  description  HEAD  hooks  info  objects  refs


这个时候，/root/git-learn/my_shell
该文件夹，就被git管理了
再这个目录下对文件的修改类操作，就会被git进行记录了

命令，查看git工作区的状态

[root@web-7 ~/git-learn/my_shell]#git status
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	hello.sh
nothing added to commit but untracked files present (use "git add" to track)


追踪文件属性
[root@web-7 ~/git-learn/my_shell]#git add hello.sh
[root@web-7 ~/git-learn/my_shell]#
[root@web-7 ~/git-learn/my_shell]#
[root@web-7 ~/git-learn/my_shell]#
[root@web-7 ~/git-learn/my_shell]#git status
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#	new file:   hello.sh
#



提交版本了，提交第一个版本

[root@web-7 ~/git-learn/my_shell]#git commit -m 'v1 第一次提交'
[master (root-commit) 67d8f28] v1 第一次提交
 1 file changed, 1 insertion(+)
 create mode 100644 hello.sh


此时再用git status查看工作区的状态是如何
此时工作区就很干净了，需要提交的代码，没了


[root@web-7 ~/git-learn/my_shell]#git status
# On branch master
nothing to commit, working directory clean






```

![image-20220714122610754](pic/image-20220714122610754.png)



![image-20220714122837453](pic/image-20220714122837453.png)



## 图解git玩法流程

![image-20220714123543025](pic/image-20220714123543025.png)



### 从零新建git本地仓库，编写代码



### 克隆远程仓库中的代码





## 图解git工作流



# day77开始=================



## 昨日回顾



![image-20220715095119291](pic/image-20220715095119291.png)



##  gitignore文件忽略文件不用被git管理

![image-20220715100201703](pic/image-20220715100201703.png)



## 图解多人协作写代码，分布式源码版本控制，与运维的联系

![image-20220715101518009](pic/image-20220715101518009.png)







# ============================

## 6.git实战流程





## 从零初始化git本地仓库

```
git init /my_code/

```





## 查看本地仓库的状态

```
[root@tomcat-10 /my_code]#echo "123123123" > 打起精神来.log
[root@tomcat-10 /my_code]#
[root@tomcat-10 /my_code]#
[root@tomcat-10 /my_code]#
[root@tomcat-10 /my_code]#git status
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	"\346\211\223\350\265\267\347\262\276\347\245\236\346\235\245.log"
nothing added to commit but untracked files present (use "git add" to track)

```



## 加入暂存区

```
[root@tomcat-10 /my_code]#git add 打起精神来.log 
[root@tomcat-10 /my_code]#
[root@tomcat-10 /my_code]#
[root@tomcat-10 /my_code]#git status
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#	new file:   "\346\211\223\350\265\267\347\262\276\347\245\236\346\235\245.log"
#
[root@tomcat-10 /my_code]#

```





## 从暂存区移除文件



## 本地仓库的细节知识

````
2个场景




场景1，本地仓库，以及有了第一个版本

[root@tomcat-10 /my_shell]#git add hehehehe.sh 
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#	new file:   hehehehe.sh
#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]## 看懂11111
[root@tomcat-10 /my_shell]#git log
commit 3b4420b805b297239d9b3adc8c8605e593cab5d9
Author: pyyu <yc_uuu@163.com>
Date:   Fri Jul 15 18:56:09 2022 +0800

    那我走？






场景2，本次仓库是空的，还没有任何版本

[root@tomcat-10 /my_shell]## 2个选择，1是提交一个版本  2是撤销暂存区的记录 ，看懂22222
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#git log
fatal: bad default revision 'HEAD'
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#git status
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#	new file:   xixixixix.sh
#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#git rm --cached xixixixix.sh 
rm 'xixixixix.sh'
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#git status
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	xixixixix.sh
nothing added to commit but untracked files present (use "git add" to track)
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]## git add  git commit
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]## 再空本地仓库中的，撤销动作  git rm --cached file  看懂 11111




````













## 重新跟踪、提交文件

```
基于撤销的动作基础上，再次版本提交

[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#git status
# On branch master
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	hehehehe.sh
nothing added to commit but untracked files present (use "git add" to track)
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#git add hehehehe.sh 
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#git commit -m 'heheheh 那我走？'
[master 0c750ff] heheheh 那我走？
 1 file changed, 1 insertion(+)
 create mode 100644 hehehehe.sh
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#git status
# On branch master
nothing to commit, working directory clean
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#git log
commit 0c750ff9db54f7d7a1397c413753236f3f9dfe67
Author: pyyu <yc_uuu@163.com>
Date:   Fri Jul 15 19:03:27 2022 +0800

    heheheh 那我走？

commit 3b4420b805b297239d9b3adc8c8605e593cab5d9
Author: pyyu <yc_uuu@163.com>
Date:   Fri Jul 15 18:56:09 2022 +0800

    那我走？


```

















## gi t如何查看文件状态

```
git status

```

![image-20220715110536568](pic/image-20220715110536568.png)







## 基于git的文件重命名

![image-20220715112000413](pic/image-20220715112000413.png)



```
记住如下用法


如果你要在某个版本仓库下，修改文件的名字，如何改（修改，删除，都会对文件的元属性进行修改）
git mv
git rm 
俩命名

# 需求，记住如下正确改名的玩法即可


1. 原本的数据是
[root@tomcat-10 /my_shell]#ls
hehehehe.sh  xixixixix.sh

这俩文件都被提交为了一个版本记录，此时暂存区是空了


2. 需求是 修改xixixixix.sh 改为 java后缀
正确玩法应该是
git mv xixixixix.sh xixixixi.java

git commit -m '于超 重命名了 xixixi.sh 为xixix.java'


3




```

![image-20220715110849982](pic/image-20220715110849982.png)



![image-20220715111052422](pic/image-20220715111052422.png)





## 基于git的删除文件

```
记住，再git本地仓库中，只要是被git管理的，记录为某个版本的文件，就不能直接
rm去删

先记住正确删除的玩法


git rm去删

[root@tomcat-10 /my_shell]#git rm *.log
rm '1.log'
rm '2.log'
rm '3.log'
rm '4.log'
rm '5.log'
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#
[root@tomcat-10 /my_shell]#git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#	deleted:    1.log
#	deleted:    2.log
#	deleted:    3.log
#	deleted:    4.log
#	deleted:    5.log


再提交一个存档，版本



```



## 理解git为什么要提交版本

![image-20220715112600435](pic/image-20220715112600435.png)



![image-20220715112822589](pic/image-20220715112822589.png)



1.30继续

多动手瞧瞧







# ====下午继续====================

![image-20220715135918573](pic/image-20220715135918573.png)



## 提交暂存区数据到local repo



多次提交版本的玩法



### 提交v1版本





### 提交v2版本



### 提交v3版本

```
[root@tomcat-10 ~/springboot-bucket]#git log --oneline -4
953662c v3
e3e79e8 v2
22d5088 v1
c45df36 开发了hello.py

```



### 





## git版本历史

### git log

```
通过git log 查看当前的本地git仓库，所有的commit 提交记录

```







## git版本回退

![image-20220715140903786](pic/image-20220715140903786.png)

```
git对文件的状态提交了N个版本，（就好比你的vmware的快照）
```



### 回退上一个版本，回到v2

```
[root@tomcat-10 ~/springboot-bucket]#git reset --hard HEAD^
HEAD is now at e3e79e8 v2
[root@tomcat-10 ~/springboot-bucket]#
[root@tomcat-10 ~/springboot-bucket]#
[root@tomcat-10 ~/springboot-bucket]#git log --oneline -4
e3e79e8 v2
22d5088 v1
c45df36 开发了hello.py
8f146f0 !1 修复 AOP的案例，调用/doError时异常不打印 Merge pull request !1 from 海王星Ivan/master


没创建一个文件，就提交了一个记录

v2  > v3  （v3.sh）

v3 版本回退到了 v2 （v2.sh  还没创建v3.sh的状态）
```



### 再回到第一个v1

```

[root@tomcat-10 ~/springboot-bucket]#
[root@tomcat-10 ~/springboot-bucket]#git reset --hard HEAD^
HEAD is now at 22d5088 v1


```



# 想让我们自己瞎搞的提交记录，全部消失（不显示，回退）

```
[root@tomcat-10 ~/springboot-bucket]#git reset --hard 8f146f0
HEAD is now at 8f146f0 !1 修复 AOP的案例，调用/doError时异常不打印
[root@tomcat-10 ~/springboot-bucket]#
[root@tomcat-10 ~/springboot-bucket]#
[root@tomcat-10 ~/springboot-bucket]#ls
LICENSE           springboot-echarts      springboot-rabbitmq-rpc  springboot-starter
README.md         springboot-hibernate    springboot-redis         springboot-swagger2
springboot-aop    springboot-jwt          springboot-restful       springboot-thymeleaf
springboot-async  springboot-mongodb      springboot-resttemplate  springboot-transaction
springboot-batch  springboot-multisource  springboot-schedule      springboot-websocket
springboot-cache  springboot-mybatis      springboot-shiro
springboot-cxf    springboot-rabbitmq     springboot-socketio
[root@tomcat-10 ~/springboot-bucket]## 我们自己的提交记录都没了，因为版本回退到，自带的某一个版本了
[root@tomcat-10 ~/springboot-bucket]#
[root@tomcat-10 ~/springboot-bucket]#
[root@tomcat-10 ~/springboot-bucket]##看懂111


```





### 

![image-20220715141624774](pic/image-20220715141624774.png)



### 



## 穿梭未来（如何再git所有的版本中，来回切换）



```
回到了2018年

如何再回去？回到今天2022年提交的 v1 v2 v3 找回来。

版本回退的核心点，找到commit id把

git log只能看到当前HEAD指向的最新的记录，以及历史的记录


通过git reflog查看到你对这个本地仓库做了哪些操作



```



![image-20220715142313873](pic/image-20220715142313873.png)









































