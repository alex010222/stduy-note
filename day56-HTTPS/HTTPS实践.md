```### 此资源由 58学课资源站 收集整理 ###
	想要获取完整课件资料 请访问：58xueke.com
	百万资源 畅享学习

```
# 为什么需要HTTPS

```
HTTP通信之间的一个安全通道，HTTPS

1. apple公司，要求所有企业上架app stone的APP，必须完全是https通信，否则不允许上架。 人家只支持https协议

2. 你现在能见到的几乎所有的门户网站，都是https协议。

3. 如今网络安全，国家大力推崇，网站的数据安全，服务器安全。也必然很重要。

https必须得上。



```

什么是https呢？

![image-20220602091900181](pic/image-20220602091900181.png)



```
如下明文传输的协议，都是不安全的，因此，至少要放在内网，才保证基本安全

ftp   vsftpd 账户密码验证 都是明文传输，人家直接看到你的账户密码

http协议   ,去登录淘宝，登录发送 login.html  账户密码截取。 
smtp邮箱协议,邮箱协议，账号免密都被明文传输，
telnet协议  ，登录服务器的，登录资产设备，网络设备，服务器设备，都是明文。

非常不安全。。。

你得保证，
client     通信之前
【加一层，加一个对数据加密的层，TLS/SSL层】 
server








```

## 明文传输数据，会导致哪些问题

## 数据机密性

在网络传输数据信息时，对数据的加密是至关重要的，否则所有传输的数据都是可以随时被第三方看到，完全没有机密性可言。

数据机密性示意图

![image-20220602092330750](pic/image-20220602092330750.png)



## 数据完整性

网络传输数据的完整性，也是安全领域中需要考虑的重要环节，如果不能保证传输数据的完整性，那传输过程中的数据就有可能被任何人所篡改，而传输数据双方又不能及早的进行发现。

将会造成互连通讯双方所表达信息的意义完全不一致。因此，对于不完整的数据信息，接收方应该进行相应判断，如果完整性验证错误，就拒绝接收相应的数据。



![image-20220602092717454](pic/image-20220602092717454.png)



## 身份验证问题

网络中传输数据时，很有可能传输的双方是第一次建立连接，进行相互通讯，既然是第一次见面沟通，如何确认对方的身份信息，的确是我要进行通讯的对象呢？

如果不是正确的通讯对象，在经过通讯后，岂不是将所有数据信息发送给了一个陌生人。

> 这也是ssh要做的指纹确认的原因。

```
client 


身份验证

server


比如我们ssh在登录时

首次进行指纹确认，确认你连接的是你要的机器。

client   >  正确的server

client  > 连接请求  > 被黑客截取  > client 发出自己假的指纹

client >  黑客机器连接了

为什么ssh要进行指纹确认，指纹是基于公钥计算而来，你可以自己去解密指纹，和公钥对比，查看是否一致。

这里关于ssh进行身份验证问题的解决，基于指纹解决，还记得，扣 6 不懂 7







```

![image-20220602093600866](pic/image-20220602093600866.png)



## 关于https，和ssh的加密算法（非对称+对称加密）



![image-20220602094216983](pic/image-20220602094216983.png)





# 为什么要用加密算法



![image-20220602100104791](pic/image-20220602100104791.png)



常见的加密算法作用，图解



![image-20220602100418986](pic/image-20220602100418986.png)



---

![image-20220602101145775](pic/image-20220602101145775.png)



# 什么是HTTPS

```
https不是一个单独的协议，

而是基于http而来

是 

https =   http +  ssl（旧版）  Secure Sockets Layer  安全套接字层

或者

https =  http  + tls  （新版）Transport Layer Security） 传输层，安全协议




```

![image-20220602102116463](pic/image-20220602102116463.png)



---

![image-20220602102609312](pic/image-20220602102609312.png)



## 客户端，服务端证书通信流程





![image-20220602103307404](pic/image-20220602103307404.png)



# HTTPS加密过程

> client，和服务端之间基于https通信，的加解密原理流程。

```
学原理，
1. 你能更清晰，部署https的背后的通信流程

2. 出了问题，疑难杂症，你通过你的理论知识，能知道如何去排错。

3. 看到奇怪的问题，通过你的理论知识，能够，看懂，思考出，这个疑难杂症的由来，解决办法。。

```



## 客户端，和服务端之间的https加密，解密全流程





![image-20220602110141568](pic/image-20220602110141568.png)



```
1. 服务端有一对儿数字证书，包含私钥、公钥（非对称加密算法），也被称为CA证书，这个证书由专门的证书服务商提供，受互联网信任。（也可以自己创建CA证书，但是没人认。。）

2. 客户端发起https://请求，默认端口443

3. 服务端接收到请求后自动将自己的CA证书发给客户端

4. 客户端收到CA证书后，浏览器自动判断，是否在有效期内，是否受互联网信任，信任就是小绿锁，否则就是红色大叉。

5. 客户端如果验证证书通过、此时会【生成一个随机数】通过公钥对随机数加密

6. 客户端将这个【公钥加密后的随机数】发给服务端

7. 服务端接到这个【公钥加密后的随机数】后，使用自己的随机数解密，确认建立连接，后续的所有数据交互，通过这个随机数实现数据【对称加密】。
```

![image-20220602104617402](pic/image-20220602104617402.png)





![image-20220602112946142](pic/image-20220602112946142.png)





![image-20220602113452682](pic/image-20220602113452682.png)





## 具体的，网站对https的应用，以及如何查看网站的证书



# 查看公有云，提供的证书购买功能，以及证书知识的介绍

```
各大云厂商
https://help.aliyun.com/document_detail/188316.html?spm=5176.14113079.help.dexternal.57f96b84CwPbL2




```

## 没有证书的场景

http协议通信的网址，浏览器会不信任，他会明确告诉你，这个http的数据是明文传输，会被黑客截取。

用火狐浏览器，更方便，可以看到证书的详细信息。

```
证书是需要绑定域名的

1. 绑定单个域名，如 www.bilibili.com，只能单独的给一个虚拟主机去部署
如
便宜，免费使用，信任度不够，加密算法，强度不够，安全性，不够高
这里的证书，是基于www.bilibili.com 域名，单独颁发的。

server{

	server_name www.bilibili.com;
	证书公钥地址  xxx;
	证书私钥地址  xxx;
}



2. 绑定泛域名，二级域名，如*.bilibili.com

另外一套证书，是基于泛域名颁发，的如 *.bilibili.com
价格昂贵，加密算法更强，非常牢靠
越贵的证书，算法强度越大，全世界的浏览器都信任，如世界银行，如花旗银行，都用高级证书。


就可以给多个nginx去使用
# 这里的配置能理解nginx的设置，扣6  不懂7 

server{
	server_name www.bilibili.com;
	泛域名证书公钥地址  xxx;
	泛域名证书私钥地址  xxx;
}


server{
	server_name api.bilibili.com;
	泛域名证书公钥地址  xxx;
	泛域名证书私钥地址  xxx;
}

server{
	server_name movie.bilibili.com;
	泛域名证书公钥地址  xxx;
	泛域名证书私钥地址  xxx;
}





```

![image-20220602114538531](pic/image-20220602114538531.png)



知道如何去查看证书信息即可，

一般证书，我们就关注2个点

````
证书绑定的域名

证书的有效期

````

## 颁发证书的机构



## 证书的类型

```
DV类型证书：中文全称是域名验证型证书，证书审核方式为通过验证域名所有权即可签发证书。此类型证书适合个人和小微企业申请，价格较低，申请快捷，但是证书中无法显示企业信息，安全性较差。在浏览器中显示锁型标志。

OV类型证书：中文全称是企业验证型证书，证书审核方式为通过验证域名所有权和申请企业的真实身份信息才能签发证书。目前OV类型证书是全球运用最广，兼容性最好的证书类型。此证书类型适合中型企业和互联网业务申请。在浏览器中显示锁型标志，并能通过点击查看到企业相关信息。支持ECC高安全强度加密算法，加密数据更加安全，加密性能更高。

EV类型证书：中文全称是增强验证型证书，证书审核级别为所有类型最严格验证方式，在OV类型的验证基础上额外验证其他企业的相关信息，比如银行开户许可证书。EV类型证书多使用于银行,金融,证券,支付等高安全标准行业。其在地址栏可以显示独特的EV绿色标识地址栏，最大程度的标识出网站的可信级别。支持ECC高安全强度加密算法，加密数据更加安全，加密性能更高。

```

![image-20220602115203673](pic/image-20220602115203673.png)



看看b站是什么类型的

```
GlobalSign RSA OV SSL CA 2018 

从这可以判断出
颁发的企业是  GlobalSign 
算法是RSA ，非对称加密的，RSA算法
SSL  套接字安全层
OV 企业级的证书

CA 证书的意思




```

![image-20220602115543412](pic/image-20220602115543412.png)



如何查看网站的证书信息，以及如何购买哪些种类的证书，证书的时间，价格，日期，算法

都看懂了吗。





# 明确，web服务器，是如何将https证书发给用户浏览器

以及验证证书是否合法，有效期限

使用证书（公钥）实现客户端的http和服务端的http中间有一个ssl通信隧道，保证数据传输带额安全性。



```
上午，给大家讲解为什么网站基于http通信，需要添加https为了保证数据传输的安全性。

需要做的，就是部署nginx，提供http服务，
加上https证书的配置。


```



# 练习一，下一步，购买证书（阿里云部署https）

1.阿里云申请免费的证书（单域名，如）

```
实际的公网中，如何部署https，提供安全可靠的网站

大家有谁的阿里云，域名，申请，备案通过吗？
我想用你的机器，去做测试。。。


程志伟的域名，阿里云

www.wzhizhi.cn （去申请免费的证书）


1.去阿里云 ssl证书控制台，申请20个免费的证书，但是都只能绑定单域名
https://yundun.console.aliyun.com/?p=cas#/certExtend/buy

证书申请，填写个人资料
新建证书管理员的联系人资料
RSA 非对称加密的证书
已经成功提交到CA公司，请您保持电话畅通，并及时查阅邮箱中来自CA公司的电子邮件。
等待7分钟后，你就可以使用这个
www.wzhizhi.cn  证书了。



```

![image-20220602142451694](pic/image-20220602142451694.png)





## 下载证书，部署到阿里云服务器

```
1. 获取证书到阿里云
下载证书，上传到自己的服务器



2. 部署nginx，部署https网站
你的密码已泄露，课下吧密码改了。志伟。


登录阿里云服务器，准备部署https网站


3. 上传证书到阿里云服务器


4. 部署nginx服务，设置虚拟主机

[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key


yum install nginx -y

创建nginx网站的虚拟主机
------------------------------------------------
一个是80端口的http服务，nginx的七层转发
[root@zhizhi conf.d]# cat 80.conf 
server {

	listen 80;
	server_name www.wzhizhi.cn;
	charset utf-8;
	location / {
		root /myweb/;
		index index.html;
	}
}


[root@zhizhi conf.d]# 
[root@zhizhi conf.d]# curl 127.0.0.1
我是志伟的80端口的网站，你们好。
先确保本地可访问，然后打开阿里云的安全组（防火墙）


----------------------------------------------------------------
下一步，编写https的配置文件，支持发送证书给用户浏览器。

一个是443端口的https服务

#以下属性中，以ssl开头的属性表示与证书配置有关。
# 创建touch  443.conf ，建议你和我一样，分开写，便于管理。
# 只需要修改你的https证书的路径即可。


server {
    listen 443 ssl;
    charset utf-8;
    #配置HTTPS的默认访问端口为443。
    #如果未在此处配置HTTPS的默认访问端口，可能会造成Nginx无法启动。
    #如果您使用Nginx 1.15.0及以上版本，请使用listen 443 ssl代替listen 443和ssl on。
    server_name www.wzhizhi.cn; #需要将yourdomain替换成证书绑定的域名。
 	
    ssl_certificate /etc/nginx/ssl-cert/7879876_www.wzhizhi.cn.pem;  #需要将cert-file-name.pem替换成已上传的证书文件的名称。
    ssl_certificate_key /etc/nginx/ssl-cert/7879876_www.wzhizhi.cn.key; #需要将cert-file-name.key替换成已上传的证书私钥文件的名称。
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    #表示使用的加密套件的类型。
    ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3; #表示使用的TLS协议的类型。
    ssl_prefer_server_ciphers on;
    location / {
        root /myweb/;  #Web网站程序存放目录。
        index index.html index.htm;
    }
}


重启nginx
systemctl reload nginx


# 志伟你需要重新创建该证书，重新申请，你这个证书，私钥以及泄露了。。理解扣1


测试访问了
访问它即可。http://www.wzhizhi.cn/index.html

但是，你的443端口，防火墙打开了吗》？



```

![image-20220602144104871](pic/image-20220602144104871.png)



--

![image-20220602144932828](pic/image-20220602144932828.png)



```
申请的，一年有效期的，https证书

加密算法是 RSA的非对称加密算法

绑定的域名是单域名  www.wzhizhi.cn

泛域名，收费版证书
*.wzhizhi.cn

提供给你一对证书文件，你可以下载，部署到nginx上。
这里能听懂 扣 1,


颁发的证书类型是  DV，个人网站https证书

到这里完全看懂扣 6,


```







# 下一步，自建证书（openssl部署自建nginx证书）

一部分业务，是纯内网环境的，需要自建https证书。

公司内部的dns系统，提供的域名，绑定https证书

```
如何自己去创建https证书

证书得创建，包含了创建者的信息

1.要安装openssl命令
yum install openssl openssl-devel -y

把nginx也给装好

[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key


yum install nginx -y





2.创建证书的目录，通过命令去创建
mkdir -p /etc/nginx/ssl-cert/
cd /etc/nginx/ssl-cert/

创建私钥文件
# 阿里云rsa非对称加密算法，密钥长度是2048，输出密钥信息到server.key文件中
# -idea是加密算法的名字

openssl genrsa -idea -out server.key 2048
输出私钥密码，为了保护私钥
必须输入密码才可以创建
chaoge666

[root@lb-5 /etc/nginx/ssl-cert]#cat server.key 
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: IDEA-CBC,904B1F32A100884B

6prXU7IGtpZ4SZ6QM1TZEBNWzjWUuTi+0KfdW3b6ygnvpjz2i5s/EyEROsD2bwQX
f2hGD6NYVsGBLLwMd2hV3CwrSWtcXsy/JgRZAKk4lCmZY5GJegeFK03VmCU/Lwev
h/eQBIjd/cOF/pnaBSscqCaDp57L1pICjedDqyT5wvssJa4Ub0+sdn6P9ao8yMfo
hr6mdCh6IwxXwf6DxfcWCgCPghdNlaxd8zgZybkE8UaelteXXZmHzBE4+znwutG3
NM8i8USoU6Opa9PZcJXmAdvR75djp3qUJZ87Lkm3Ou5r4Lz4gJmj0kFnXrW1Au86
WYA+q7X53FgAIrND9MVv/8ivxja1sQP4bZ3vkuy+G0zXGoEy2DWINfID+4lKaX7F
gQO8mwzMcsEBC7mMb6MTAdLvld+d3X7UQZ+YNI4t4Ytw4/jVwM58EX+ZAb0STvfg
23mIfsqNbRehsXvlF77z9UT8nvZgomb1KY+Ba0Ny3rCL4ZtFm0zzrpgBKR3TDsiI
c2lRwp0TfnnJYsZO3ih2J9s0WyAE4ikQHJ87UByOfTuhIn2Mr3gB/9wNHVeieOuw
VooHi9/sGLth7PNJIz/O+7YVCNEt69kCRQWJS1ouF/rpVbKQ4HwtObVVDAZdSGPg
YSi6ruvhOadwjacEFNb6fxYko/hodAgWCKySKKWVGzy8sFfa4h9XVdGpxLXiAUwH
4Pf3QWooMbJ5QDiI67tBG8TMc2lFQWFJhWSJK7HbC0bIsTjvABBLs2bO06laJX9J
p6C8k5g/iTUoAzIIswX4r1UrRrVyIMi+j2bHs01PkvU7kytj1E4p7tZ5mIiHryO1
UgDNWoJAJYA/pXp5EaHwTHv7xnfrMCqwiNGYXikeL5jqkYROTH1KYmiTY4k3UbX6
eaIG2py1+I4ULmjhy9rZ5XGUuOr6kF80Q6dp3bFe3+DaKGhEF+9snX65wPImyCsg
kVGowZIZSX7uHBqaK5FYBH56lNxjtgYh0sgiuaWkp0Gd68wMDYP2lx3pTwHU4Y60
GoL3zv0x+fdY4SeMsxDBp9rRYihLQU7HVZ3kMm7MwehdiXUDWoCVFnkdvoSstZD5
FxMK37VSCueal+Ov0E4akxHeI7yELVqB7656w1wMI1WWOO1cOjfGIBKC630V9OBv
dyex4zDdKqOqFtIXXvmTXXvRw0g7WAM/NCQ4ji2atCqSEU7MyRxABJaZ7meCofGI
gxBQ8QwSjEmVItl8yzSHdBlpDxnSNjQQ8BVeHlhhpgCRDZBKCymQBpGDBIpVp7SX
rbGmtxavox9X8nfPm8l8fDojt9KXUhyFpiV3aUOBMKlRN/RZ2/Y7FK/m/OHgk1Y4
dFaPEjb0Dmbm+5ZK7ymIG2PQYfCHEY14fRnfPj9slxsPSmhLBtdTZoGbl/324LHp
JVrqBYvVv5ZfuLgMjWAtl2SbWzA9UgVSfUt96zOwyibMvJD8sXBIMWpCem0JqbH9
P+Hpf3klqJh16EqamXI/qlj/KxSbsyQ2dzv5A24VvWm4U0AyScfe+vyWCGa/xw0l
JOAl3FWgMY0xSY8PGWZUQBkyj1Bqq5lmLa0Ag9lPZnQuAUNwJVUskg==
-----END RSA PRIVATE KEY-----



然后基于该私钥文件，创建证书（创建公钥）

# req创建证书，100年  证书规格，类型是-509类型，-newkey rsa:2048 基于rsa非对称加密算法，创建长度是2048的文件，创建证书，指定以哪个私钥去创建
-out server.crt  将公钥输出到server.crt文件中


# 你在创建公钥，证书的时候，会让你填写企业，组织信息

openssl req -days 36500 -x509 -sha256 -nodes -newkey rsa:2048 -keyout server.key -out server.crt



[root@lb-5 /etc/nginx/ssl-cert]## 到这里，就和你去阿里云下载的那个证书一个意思。一对非对称的公私钥就创建好了，下一步交给nginx即可，。看懂6，不懂7 
[root@lb-5 /etc/nginx/ssl-cert]#
[root@lb-5 /etc/nginx/ssl-cert]#ls
server.crt  server.key





```

### 查看证书的过期时间，证书信息

如下脚本是利用openssl查看某个域名的证书过期时间的脚本，你以后可能用的上。

很多时候，运维没关心网站证书的过期时间，导致，突然证书过期了，https无法访问

导致网站挂掉。

吧执行结果，发给运维的邮箱。

额外补充，脚本你复制，理解下就行，是一个小工具。



```

server_name=www.bilibili.com

# 获取网站的证书有效期
# 获取ssl过期时间的命令
# 1.获取有效期的日志
ssl_time=$(echo | openssl s_client -servername ${server_name}  -connect ${server_name}:443 2>/dev/null | openssl x509 -noout -dates|awk -F '=' '/notAfter/{print $2}')


# 2.转换时间戳
# 把日志转变为时间戳，时间戳就可以去计算了
# 转为unix日期格式，然后可以对日期进行计算

ssl_unix_time=$(date +%s -d "${ssl_time}")

# 获取今天时间戳
today=$(date +%s)

# 计算剩余时间
# 利用let命令去数学运算
# 从到期时间，减去今天的日期，然后单位换算，看看过期还有多久
# 最终将时间戳，转为天的单位

let  expr_time=($ssl_unix_time-$today)/24/3600

echo "${server_name} 该ssl证书剩余时间：$expr_time"
```



 























## 全站https（理解，会部署即可）

https的数据加密过程，也会导致请求响应速度慢很多，数据都要加密，解密

因此内网可以不做https，外网入口slb做https即可。



## 【https全站部署，正确图解】



![image-20220602164748581](pic/image-20220602164748581.png)





```

client ，
访问 http://www.linux0224.cn  > https://www.linux0224.cn
↓

slb (ssl  + nginx【http】)  
http://www.linux0224.cn  > https://www.linux0224.cn

证书这里一份
↓

证书再来一份

http://www.linux0224.cn  > https://www.linux0224.cn
web7 web8（ 【ssl】 +   nginx[http]  +   php-fpm   ）







```



### lb-5机器（这里负载均衡，加上upstream配置）

```

准备好slb+ssl

私钥文件
/etc/nginx/ssl-cert/server.key
公钥文件，证书文件
/etc/nginx/ssl-cert/server.crt


创建nginx配置文件

vim /etc/nginx/conf.d/ssl.conf


server {
    listen 80;
    server_name www.linux0224.cn;
    rewrite ^(.*) https://$server_name$1 redirect;
}

# 从80 > 443


upstream myweb {
	# 反正这是后端内网的节点，用户也不可见
	server 172.16.1.7:443;
	server 172.16.1.8:443;
}
server{
    listen 443 ssl;
    server_name www.linux0224.cn;
    ssl_certificate /etc/nginx/ssl-cert/server.crt;
    ssl_certificate_key   /etc/nginx/ssl-cert/server.key  ;

  location / {
       proxy_pass  https://myweb;
       # 加上代理转发参数
       include proxy_params;
  }
}




# 创建好转发参数
vim /etc/nginx/proxy_params

proxy_set_header Host $http_host;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_connect_timeout 30;
proxy_send_timeout 60;
proxy_read_timeout 60;
proxy_buffering on;
proxy_buffer_size 32k;
proxy_buffers 4 128k;







# 这个配置能看懂扣 6，看不懂7

[root@lb-5 /etc/nginx/ssl-cert]#netstat -tunlp |grep '443|80' -E
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      2155/nginx: master  
tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN      2155/nginx: master  


```



### web7   ，web8

两台机器一起操作即可。



ssl.conf



```
1. 拷贝证书文件，用同一份证书
mkdir -p /etc/nginx/ssl-cert/
scp  root@172.16.1.5:/etc/nginx/ssl-cert/*   /etc/nginx/ssl-cert/

这2个步骤，看懂扣 3，不懂4

2. 配置文件

vim /etc/nginx/conf.d/ssl.conf
server{
    listen 443 ssl;
    server_name www.linux0224.cn;
    ssl_certificate /etc/nginx/ssl-cert/server.crt;
    ssl_certificate_key   /etc/nginx/ssl-cert/server.key  ;
  location / {
          root /www;
          index index.html;
  }
}


3.创建测试数据的页面
mkdir /www
#echo 'i am web 8 ++++++++++++++' > /www/index.html
#echo 'i am  web7~~~~~~~~~~~~' > /www/index.html


```



### 测试访问，客户端做好dns解析

```
10.0.0.5 www.linux0224.cn


http://www.linux0224.cn/


```





# 综合，难度加大的玩法（slb支持https，后端依然是http就行）

```
最常见的，入口和外网的数据通信，确保安全，用https
内网的转发，无须设置https。

部署后端应用服务器，接收https请求的时候，得加上一些设置。

```



## 以wordpress为练习入手。

```
client
↓
http://wordpress.linux0224.cc
自动302跳转
https://wordpress.linux0224.cc
↓

slb-5 （nginx配置）
负载均衡， 转发给后端

↓
web组 7和8
nginx + php-fpm的代理转发。


这个流程，能清晰看懂，理解的 扣 3，不理解扣 4



```

![image-20220602172521707](pic/image-20220602172521707.png)





## 先部署lb-5机器

```
我又差点踩坑了。。。。

wordpress安装后，数据写到数据库了，
你昨天是以wordpress.linux0224.cc安装的

最终这个博客产品，写入的url都是这个域名 wordpress.linux0224.cc
因此，你还得用这个域名，不能乱改。。


```

lb-5机器的配置文件



```

[root@lb-5 ~]#cat /etc/nginx/conf.d/wordpress.conf 
# 这里定义后端的入口是  ip:33333
# 因此你后端的节点，nginx入口得是 listen 33333;
# 到这里都OK的扣 6，看不懂7

upstream wordpress_pools {
    server 172.16.1.7:33333;
    server 172.16.1.8:33333;
}

# 80虚拟主机，目的是为了匹配http请求的80端口，强制转发给https的443端口
# 1 . 目的是接收 http://wordpress.yuchaoit.cc;
server {
    listen 80;
    server_name wordpress.linux0224.cc;
    rewrite ^(.*) https://$server_name$1 redirect;
}

# 2. https虚拟主机的设置

server {

    listen 443 ssl;
    server_name wordpress.linux0224.cc;
	# 指定证书的地址
    ssl_certificate /etc/nginx/ssl-cert/server.crt;
    ssl_certificate_key   /etc/nginx/ssl-cert/server.key;
  
  	# 反向代理，这里就基于http协议的转发，给后端机器组即可。
  	# 用的是proxy_pass 
  	location / {
            proxy_pass http://wordpress_pools;
            include proxy_params;
  }
}




# 重新启动nginx
nginx -s reload


```



## 部署wordpress后端机器组



```
nginx 去接收转发而来的请求，然后再通过fastcgi_pass 127.0.0.1:9000
并且转发的时候，添加让http数据，转为fastcgi_pass可认识的变量数据

这个流程记得扣 1，不记得 2
在部署了https之后

slb-5接收到 https的请求后，再转发给后端，需要后端框架支持。
# 让fastcgi协议，也认识https


写入到web7  和web8的nginx目录下
cat fastcgi_params 
fastcgi_param  QUERY_STRING       $query_string;
fastcgi_param  REQUEST_METHOD     $request_method;
fastcgi_param  CONTENT_TYPE       $content_type;
fastcgi_param  CONTENT_LENGTH     $content_length;

fastcgi_param  SCRIPT_NAME        $fastcgi_script_name;
fastcgi_param  REQUEST_URI        $request_uri;
fastcgi_param  DOCUMENT_URI       $document_uri;
fastcgi_param  DOCUMENT_ROOT      $document_root;
fastcgi_param  SERVER_PROTOCOL    $server_protocol;
fastcgi_param  REQUEST_SCHEME     $scheme;
fastcgi_param  HTTPS              $https if_not_empty;

fastcgi_param  GATEWAY_INTERFACE  CGI/1.1;
fastcgi_param  SERVER_SOFTWARE    nginx/$nginx_version;

fastcgi_param  REMOTE_ADDR        $remote_addr;
fastcgi_param  REMOTE_PORT        $remote_port;
fastcgi_param  SERVER_ADDR        $server_addr;
fastcgi_param  SERVER_PORT        $server_port;
fastcgi_param  SERVER_NAME        $server_name;

# PHP only, required if PHP was built with --enable-force-cgi-redirect
fastcgi_param  REDIRECT_STATUS    200;

# enable https
# 因为，入口是https请求，转发到后端，url依然是https://wordpress.linux0224.cc，比如你来看
# 因此在这里要打开这个参数，记住就行，。

fastcgi_param HTTPS on;


生成nginx的配置文件

# 看懂扣 3，不懂 4
server {
    listen 33333; # 因为slb的upstream里面写的就是33333
    # 注意这个域名要和slb对上
    server_name wordpress.linux0224.cc;
    root /mysite/wordpress/;
    index index.php index.html;

    location ~ \.php$ {
        root /mysite/wordpress/;
        # 发给fastcgi去执行php的wordpress源代码程序
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

}

# 到这里，能清晰看懂的，扣 6，不懂7 

 # 见证奇迹的时候到了，。是正确访问？还是又是踩坑？这是一个迷。。。
 
 
# 看看数据库里写入的url域名是什么。

#启动是lb和 web组


你自建的证书，没有绑定域名

阿里云上的证书，绑定域名是通过了一个 txt类型的dns记录，做的绑定解析，限制你必须用单个的域名。




```

## 踩坑解释记录（数据库）

![image-20220602170809870](pic/image-20220602170809870.png)



## 客户端测试

```
10.0.0.5  wordpress.linux0224.cc
```



如果负载均衡有问题，你可以先试试，单个的节点是否正常。

```
10.0.0.7  wordpress.linux0224.cc
```



休息片刻。。。排错中。

tmd...貌似是浏览器有毒，我们的配置，完全没问题，思路也都是对的！！！

可能是灵异事件。。配置，讲解都是没问题的。



带你再次分析一波。。。



## 确保你的网站是正确的（看日志）

通过检测，日志，明确请求负载均衡到了 web7  web8 默认是轮训一人一次

到这里完全看懂扣 1，不懂2



确保你的wordpress后台管理也是对的，以及发表文章都是对的

这个问题，还是数据库中写入了端口的问题。

之前部署，

```
slb 
upstrem {
	server ip:80;
	server ip:80;
}

web7  web8 


告诉你这个思路，如何别踩这个坑。

1. 你的wordpress实验，
```





# 今天作业

```
1. 基于阿里云的 https网站部署，你需要提交作业，提交你的服务器域名，如
https://www.wzhizhi.cn
让我看到你的https的网站
（域名还没弄好的，抓紧）



2. 在你的虚拟机中，实现wordpress的https负载均衡即可，wectener不用弄。

建议是
方案1
	你的wordpress全部重头安装
	也就是你可以这样
	

3. 要求，你的wordpress得难度加大，且是基于https通信的。

slb-5

web7
web8

nfs-31

db-51

是否李姐，李姐扣 1，不懂 2


```

![image-20220602174019848](pic/image-20220602174019848.png)









