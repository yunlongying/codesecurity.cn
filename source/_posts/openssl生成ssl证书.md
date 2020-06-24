---
title: openssl生成ssl证书
date: 2020-06-22 22:35:16
tags:
    - SSL证书
    - 产品安全知识
---

# 摘要
HTTPS通信必须要有SSL证书，那么怎么获取SSL证书呢？
Openssl提供了制作ssl证书的方法，我们平时开发和测试时可以使用openssl自己生成证书，当然这种方式生成的是自签名证书，默认浏览器是不信任的，会提示证书不可信问题。
商用环境都是到合法的CA机构购买证书，这样浏览器访问网址时就不会提示证书不可信问题了。

<!--more-->

数字证书和HTTPS的概念可以参考：
[什么是HTTP和HTTPS](http://www.codesecurity.cn/2020/06/14/%E4%BB%80%E4%B9%88%E6%98%AFHTTP%E5%92%8CHTTPS/)

[什么是数字签名和数字证书](http://www.codesecurity.cn/2020/06/13/%E4%BB%80%E4%B9%88%E6%98%AF%E6%95%B0%E5%AD%97%E7%AD%BE%E5%90%8D%E5%92%8C%E6%95%B0%E5%AD%97%E8%AF%81%E4%B9%A6/)

# Openssl
>openssl 是目前最流行的 SSL 密码库工具，其提供了一个通用、健壮、功能完备的工具套件，用以支持SSL/TLS 协议的实现。

* 构成部分

>密码算法库
密钥和证书封装管理功能
SSL通信API接口

* 用途

>建立 RSA、DH、DSA key 参数
建立 X.509 证书、证书签名请求(CSR)和CRLs(证书回收列表)
计算消息摘要
使用各种 Cipher加密/解密
SSL/TLS 客户端以及服务器的测试
处理S/MIME 或者加密邮件


# 创建根证书CA

（1）查看openssl的配置文件openssl.cnf的存放位置（即openssl的安装位置）
```
openssl version -a
```

![openssl版本](openssl版本.png)

（2）查看openssl的配置文件openssl.cnf，因为配置文件中对证书的名称和存放位置等相关信息都做了定义。
```
vim /etc/pki/tls/openssl.cnf
```

![openssl默认配置](openssl默认配置.png)

（3）创建为根证书CA所需的目录及文件
根据配置文件信息，到CA根目录，若没有则自己创建
```
cd /etc/pki/CA
```

创建配置文件信息中所需的目录及文件
```
mkdir -pv {certs,crl,newcerts,private}
touch {serial,index.txt}
```

（4）指明证书的开始编号
```
echo 01 >> serial
```

（5）生成根证书的私钥（注意：私钥的文件名与存放位置要与配置文件中的设置相匹配）。
```
(umask 077; openssl genrsa -out private/cakey.pem 2048)
```

参数说明：
genrsa  --产生rsa密钥命令
-aes256--使用AES算法(256位密钥)对产生的私钥加密，这里没有此参数，则只是用了rsa算法加密。
-out  ---输出路径，这里指private/ca.key.pem
这里的参数2048，指的是密钥的长度位数，默认长度为512位

（6）生成自签证书，即根证书CA，自签证书的存放位置也要与配置文件中的设置相匹配，生成证书时需要填写相应的信息。
```
openssl req -new -x509 -key /etc/pki/CA/private/cakey.pem -out cacert.pem -days 365
```

参数说明：
-new：表示生成一个新证书签署请求
-x509：专用于CA生成自签证书，如果不是自签证书则不需要此项
-key：用到的私钥文件
-out：证书的保存路径
-days：证书的有效期限，单位是day（天），默认是openssl.cnf的default_days

这样子，根证书CA就已经生成完成了。


# 颁发证书
在需要证书的服务器上生成私钥，然后通过此私钥生成证书签署请求，最后将请求通过可靠的方式发送给根证书CA的主机。根证书CA服务器在拿到证书签署请求后，即可颁发那一服务器的证书。

4.1 在需要证书的服务器上，生成证书签署请求
（1）生成私钥，该私钥的位置可随意定
```
(umask 077; openssl genrsa -out test.key 2048)
```

（2）生成证书签署请求
```
openssl req -new -key test.key -out test.csr -days 365
```

4.2 在根证书服务器上，颁发证书
（1）颁发证书，即签名证书，生成crt文件
我们创建一个req文件夹来接受服务器发送过来的文件（签署请求的csr文件、key文件等）
```
mkdir /etc/pki/CA/req
```
颁发证书
```
openssl ca -in /etc/pki/CA/req/test.csr -out /etc/pki/CA/certs/test.crt -days 365
```

查看证书信息
```
openssl x509 -in /etc/pki/CA/certs/test.crt -noout -serial -subject
```
但是有些格式不是我们需要的，请看下面的两个格式转换。

（2）格式转换为pfx格式的私钥
```
openssl pkcs12 -export -out test.pfx -inkey /etc/pki/CA/req/test.key -in test.crt
```
注意，-inkey的值test.key是需要证书服务器上生成的私钥key文件。

（3）格式转换为cer格式的公钥
```
openssl x509 -inform pem -in test.crt -outform der -out test.cer
```
查看cer证书信息
```
openssl x509 -in test.cer -text -noout
```
若报错unable to load certificate，则说明你打开的证书编码是der格式，需要用以下命令
```
openssl x509 -in test.cer -inform der -text -noout
```
参数含义：
-inform pem，由于输入的test.crt文件是以pem编码的，故需要指定以pem编码来读取。
-outform der，输出的test.cer文件需要以der编码。

至此，服务器的证书颁发就完成了，只需要将此签名证书发送给服务器，服务器就可以使用此签名证书了。

