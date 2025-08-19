```### 此资源由 58学课资源站 收集整理 ###
	想要获取完整课件资料 请访问：58xueke.com
	百万资源 畅享学习

```
# 作业题目

# 1.完成正则表达式符号的学习笔记

- 基本正则表达式
- 扩展正则表达式
- 结合grep的用法
  - grep是专门提取数据的，对正则提取到的数据，没办法做太多事

- sed命令
  - sed根据正则表达式，sed本身的命令作用，提取到数据后，还能做些什么事


写笔记参考博客

```
http://apecome.com:9494/03%E7%B3%BB%E7%BB%9F%E6%9C%8D%E5%8A%A1%E7%AF%87/3-16-%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F%E5%92%8C%E4%B8%89%E5%89%91%E5%AE%A2.html
```



# 2.正则练习题

正则练习题参考博客

url

```

```



## 答题要求

1. 可以参考老师的正则，理解题意后，自己再写出1种及以上的正则方案

```
1.看懂老师写的正则，参考，思路的提供
2.自己再去写出另一种正则，即使你就是多了一个字符 
```





## 练习题

```
测试数据：

I am teacher yuchao.
I teach linux,python.
testtttsss000123566666

I like video,games,girls
#I like girls.
$my blog is http://yuchaoit.cn/
#my site is https://www.cnblogs.com/pyyu
My qq num is 877348180.
my phone num  is 15210888008.
```



1. ### `^ `查找以什么开头的行

   ### 找出以`I`开头的行

```
grep   -n  '^I'  test.txt

'^字符'
```

![image-20220412153813812](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412153813812.png)



2. ### `$` 查找以什么结尾的行

  #### 找出以`u`结尾的行

```
结尾字符$
```



```
grep   -n   'u$'   test.txt
```

![image-20220412154149304](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412154149304.png)

#### 找出以`.`结尾的行

```
为了最大程度的降低，字符的特殊含义，别用双引号，否则你要考虑加很多的转义符

grep '\.$' -n t1.log


grep   -n   '\.$'   test.txt

```

![image-20220412154527602](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412154527602.png)

3. ## ^$ 查找和排除空行

4. ```
   ^
   $
   
   ^$ 提取出空白行，没有内容的行，连空格都没有的
   
   ```

3. 

  #### 找出以`I`开头，`s`结尾的行

```
grep   -n   '^I.*s$'   test.txt

Is
Ias
Iaaaaaaaaaaaas
IbS
I               qs



```

![image-20220412154936906](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412154936906.png)

#### 找出空白行

```
# 匹配空行的正则
grep    -n    '^$'   test.txt

# grep -v结果取反
grep    -nv    '.'   test.txt
```

![image-20220412155440883](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412155440883.png)

#### 排除空行，提取有益的信息行

```
grep   -n   '.'   test.txt

grep   -n -v   '^$'   test.txt
```

![image-20220412155655435](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412155655435.png)

![image-20220412155903566](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412155903566.png)

## 4.` . `任意一个字符 不会匹配空行 、可以匹配空格，注意这里的点是正则符、而不是普通的点

```
grep   -n   '.'   test.txt
```

![image-20220412160751042](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412160751042.png)

## 5.` \ `转义特殊字符

```
找出以$开头的行

grep    -n    '^\$'    test.txt


#暂时武略这种写法
grep   -n    "^\\s"    test.txt
```

![image-20220412161342052](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412161342052.png)

6. ## [ ]匹配字符

  #### 匹配小写字母

```
grep    -n  '[a-z]'   test.txt

grep   -n   '[[:lower:]]'   test.txt
```

![image-20220412162049090](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412162049090.png)

#### 找出以`.`或`s`结尾的行

```
grep -E  '\.$|s$'  t1.log

egrep '\.$|s$'

grep -E '(\.|s)$' t1.log

grep -E '[.s]$' t1.log

grep -E '[\.s]$' t1.log



```





```
grep   -n  '[.s]$'   test.txt
```

![image-20220412162413910](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412162413910.png)

#### 找出以`I`开头，且结尾不是`.`的行

```
grep -E '^I.*[^\.]$' t1.log
```



```
grep    -n    '^I.*[^.]$'   test.txt
```

![image-20220412162720875](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412162720875.png)

#### 7) -i 忽略大小写匹配

```
grep   -i  '^i'   test.txt
```

![image-20220412162913363](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412162913363.png)





# 扩展正则练习题

准备测试数据



```
I am teacher yuchao.
I teach linux,python.
testtttsss000123566666

I like video,games,girls
#I like girls.
$my blog is http://www.yuchaoit.cn
#my site is https://www.cnblogs.com/pyyu
My qq num is 877348180.
my phone num  is 15210888008.
```

#### 1）{}匹配手机号

```
往复杂了思考
中国电信号段133、153、173、177、180、181、189、191、193、199
中国联通号段130、131、132、155、156、166、175、176、185、186、166
中国移动号段134(0-8)、135、136、137、138、139、147、150、151、152、157、158、159、172、178、182、183、184、187、188、198
手机号码的规则如下：
第一位： 1
第二位： 3～9
第三到第十一位只要是数字就行
[242-yuchao-class01 root ~]#grep -E '1[3-9][0-9]{9}' t2.log -o
15210888008


```



```
grep 参数
-E 扩展正则
-o 只显示结果
-w 匹配单词（单词表示出现了分割符如,hello,i am super mam.）




grep   -Enow    '[0-9]{11}'   test.txt
```

![image-20220412163628649](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412163628649.png)

#### 匹配QQ号



```
grep   -Enow   '[0-9]{9}'   test.txt
```

![image-20220412165516832](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412165516832.png)

#### 2）查找出所有单词中出现字母连续的行，比如: www，http重复，可以采用分组，向后引用的特性匹配

```
找出至少有2个连续的相同字母，且只能找出至少2次连续的字母
向后引用，提取

这个写法是，括号内的数据，必然要再出现一次
这个写法是，只找出2个字母
grep -E '([a-zA-Z])\1' t2.log
ww
wwwwww

分组、向后引用、+号 组合用法
grep -E '([a-zA-Z])\1+' t2.log


只想提取三个连续一样的字母
两种写法
grep -E '([a-zA-Z])\1{2}' t2.log
grep -E '([a-zA-Z])\1\1' t2.log


括号分组，向后引用，语法还未理解


```

```


```

![image-20220412174153814](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412174153814.png)



#### 3）只查找出同一个字母连续3次的行，比如www

```
grep   -En   '([a-z])\1{2}'    test.txt
```

![image-20220412174236406](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412174236406.png)

#### 4）提取网址www.xxx.com ,提取网站的主机名，域名

```
grep  -Eno    '([a-z])\1{2,}\.[a-z0-9]{1,}\..*'     test.txt


grep   -Eno   '([a-z])\1{2,}\.[a-z0-9]{1,}\.[a-z0-9]{1,}'  test.txt


# 参考写法
[242-yuchao-class01 root ~]#grep -E 'www\..*\.(cn|com)' -o t2.log 
www.yuchaoit.cn
www.cnblogs.com
[242-yuchao-class01 root ~]#grep -E 'www\..*\.c(n|om)' -o t2.log 
www.yuchaoit.cn
www.cnblogs.com



```

![image-20220412211557650](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412211557650.png)

#### 5）提取完整的链接地址https://www.xxx.com

```


grep   -E   'http[s]?://([a-zA-Z0-9])\1{2}\.[a-zA-Z0-9]{1,}\.[a-zA-Z0-9]{1,}[/]?[a-zA-Z0-9]{0,}'  test.txt




grep    -E   'h.*://(.*).(.*).(.*)[/]?.*'  test.txt

grep -E     'h.*://.*[/]?.*'   test.txt



# 提取https://www.xxx.com/xxxxxxxxxxxxxx
[242-yuchao-class01 root ~]#grep -E 'http[s]?://www\..*\..*' -o t2.log 
http://www.yuchaoit.cn
https://www.cnblogs.com/pyyu
[242-yuchao-class01 root ~]#grep -E 'http[s]?://www\..*\.(cn|com).*' -o t2.log 
http://www.yuchaoit.cn
https://www.cnblogs.com/pyyu

```

![image-20220412195024828](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412195024828.png)

# 工作常见需求

#### 1）排除配置文件的注释、空行

```
grep   -Ev   '^#|^$'   test.txt

grep  -v  '^#'  test.txt | grep  '.'

grep -v '^#' chaoge.txt  | grep -v '^$'
```



#### 2）查找某进程是否存在，过滤grep临时进程

```
ps  -ef   |grep ssh |grep -v 'grep'
```

![image-20220412200455950](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412200455950.png)

#### 3）查看sda磁盘的使用率

```
df  -h |grep sda |grep   -oE '[0-9]+%'


```

![image-20220412200521525](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412200521525.png)

#### 4）查看根分区的磁盘使用率

```
df -h|grep '/$' |grep  -oE '[0-9]+%'
```

![image-20220412200654738](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412200654738.png)

#### 5）取出网卡ip地址

```
思路：找到命令中的ip相关，过滤出ip，显示指定ip
ifconfig命令


ifconfig ens33 |grep 'netmask' | grep -Eo '[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}' | head -1


ip addr show命令

ip addr show ens33 | grep 'ens33$' | grep -Eo '[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}' | head -1




```

![image-20220412202046539](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412202046539.png)

#### 6）统计出系统中，所有禁止登录，的用户，且只显示用户名



```
思路：过滤出/etc/passwd  中的nologin项 ，使用grep的-o参数只显示过滤出的用户名

grep   'nologin'   /etc/passwd  |grep  -Eo '^[a-z0-9A-Z]+'

```

![image-20220412202101675](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412202101675.png)

#### 7）找出由root创建的用户

root

useradd创建普通用户

uid 从1000开始



```
思路：root创建的用户从UID从1000开始，过滤出UID超过1000的用户

grep  -E '\b[0-9]{4,}\b' /etc/passwd


```

![image-20220412202951935](day28%E6%AD%A3%E5%88%99%E4%BD%9C%E4%B8%9A.assets/image-20220412202951935.png)
