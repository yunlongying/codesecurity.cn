---
title: OpenSSL命令教程
date: 2020-06-16 08:53:49
tags:
    - 安全知识
    - OpenSSL命令教程
---

>OpenSSL是一个安全套接字层密码库，其包括常用的密码算法、常用的密钥生成和证书封装管理功能及SSL协议，并提供了丰富的应用程序以供测试。
OpenSSL是一个开源的项目，其由三个部分组成：

<!--more-->

>>1、openssl命令行工具；
>>2、libencrypt加密算法库；
>>3、libssl加密模块应用库；

这里主要学习下openssl命令工具的用法，openssl命令工具有两种运行模式：交换模式和批处理模式。直接输入openssl回车即可进入交互模式，而输入带命令选项的openssl命令则进行批处理模式。


## 1、对称加密算法的应用
利用OpenSSL作对称加密需要使用其子命令enc，其用法为：
>openssl enc -ciphername [-in filename] [-out filename] [-pass arg] [-e] [-d] [-a/-base64] [-A] [-k password] [-kfile filename] [-K key] [-iv IV] [-S salt] [-salt] [-nosalt] [-z] [-md] [-p] [-P] [-bufsize number] [-nopad] [-debug] [-none] [-engine id]

### 其中常用的选项为：
>-e：加密；
-d：解密；
-ciphername：ciphername为相应的对称加密算命名字，如-des3、-ase128、-cast、-blowfish等等。
-a/-base64：使用base-64位编码格式；
-salt：自动插入一个随机数作为文件内容加密，默认选项；
-in FILENAME：指定要加密的文件的存放路径；
-out FILENAME：指定加密后的文件的存放路径；

### 使用案例：

* <font size=4>加密字符串</font>

```
[root@localhost ~]# echo "hello,world" | openssl enc -aes128 -e -a -salt
enter aes-128-cbc encryption password:
Verifying - enter aes-128-cbc encryption password:
U2FsdGVkX1/LT+Ri9pzjjS0FIGXJLNRc8ljvZJ3hf0M=
```

* <font size=4>加解密文件</font>

```
[root@localhost ~]# openssl enc -des3 -e -a -in /etc/fstab -out /tmp/fstab
enter des-ede3-cbc encryption password:
Verifying - enter des-ede3-cbc encryption password:

[root@localhost ~]# cat /tmp/fstab 
U2FsdGVkX1/pdsq5HUjsP5Kpqr378qnZSmH1j9a4KdasuG+6Jy+Mh0cRYA5IUuJ4
732mG1td6x2jvLq0JNpT+WcTFXoH30x1o6KDN6Kwyc26+uTjYb+cwf9ZhZWoEi4c
5Zh1h8S4PwKA9m/ebJAh97RSLuVWqPOsZDJ9w/zE3X0iKnb8nVNEkApB6OYjkV4s
....

[root@localhost ~]# openssl enc -d -des3 -a -salt -in /tmp/fstab 
enter des-ede3-cbc decryption password:
#
# /etc/fstab
# Created by anaconda on Sun Nov 19 02:26:36 2017
....
devpts                  /dev/pts                devpts  gid=5,mode=620  0 0
sysfs                   /sys                    sysfs   defaults        0 0
proc                    /proc                   proc    defaults        0 0
```


## 2、单向加密
OpenSSL单向加密的子命令为dgst，其语法如下：
>openssl dgst [-md5|-md4|-md2|-sha1|-sha|-mdc2|-ripemd160|-dss1] [-c] [-d] [-hex] [-binary] [-out filename] [-sign filename] [-keyform arg] [-passin arg] [-verify filename] [-prverify filename] [-signature filename] [-hmac key] [file...]

### 其常用的选项为：
>[-md5|-md4|-md2|-sha1|-sha|-mdc2|-ripemd160|-dss1]：指定一种单向加密算法；
-out FILENAME：将加密的内容保存到指定的文件中；

* <font size=4>单向加密除了 openssl dgst 工具还有： md5sum，sha1sum，sha224sum，sha256sum ，sha384sum，sha512sum</font>


### 使用案例：

* <font size=4>生成指定文件的特征码</font>

```
[root@localhost ~]# openssl dgst -md5 /tmp/fstab 
MD5(/tmp/fstab)= ef7b65e9d3200487dc06427934ce5c2d

[root@localhost ~]# md5sum /tmp/fstab 
ef7b65e9d3200487dc06427934ce5c2d  /tmp/fstab
```
或
```
[root@localhost ~]# echo hello,world | md5sum
757228086dc1e621e37bed30e0b73e17  -

[root@localhost ~]# echo hello,world | openssl dgst -md5
(stdin)= 757228086dc1e621e37bed30e0b73e1
```


## 3、加密密码
OpenSSL还支持生成密码的hash离散值，其子命令为passwd，语法如下：
>openssl passwd [-crypt] [-1] [-apr1] [-salt string] [-in file] [-stdin] [-noverify] [-quiet] [-table] {password}

### 常用选项为：
-salt STRING：添加随机数；
-in FILE：对输入的文件内容进行加密；
-stdin：对标准输入的内容进行加密；

### 使用案例：

* <font size=4>生成密码的hash值</font>

```
[root@localhost ~]# openssl passwd -1 -salt 123456 PASSWORD
$1$123456$KP0rRo6agiZOiJz8GMOd00
```


## 4、生成随机数
openssl命令也支持生成随机数，其子命令为rand，对应的语法为：
>openssl rand [-out file] [-rand file(s)] [-base64] [-hex] num

### 常用选项有：
-base64：以base64编码格式输出；
-hex：使用十六进制编码格式；
-out FILE：将生成的内容保存在指定的文件中；

### 使用案例：
```
[root@localhost ~]# openssl rand  -base64  10
d0etSF7CA13hhg==
```


## 5、生成密钥对
利用openssl命令的子命令genrsa生成私钥，然后再使用子命令rsa私钥中提取公钥。
genrsa的语法如下：
>openssl genrsa [-out filename] [-passout arg] [-des] [-des3] [-idea] [-f4] [-3] [-rand file(s)] [-engine id] [numbits]

### 常用选项：
-out FILENAME：将生成的私钥保存至指定的文件中；
[-des] [-des3] [-idea]：指定加密算法；
numbits：指明生成的私钥大小，默认是512；
通常来说秘钥文件的权限一般只能由管理员访问，因此可以结合umask命令来设置生成的密钥文件的权限，如：
```
[root@localhost ~]# (umask 077;openssl genrsa -out CA.key 4096)
Generating RSA private key, 4096 bit long modulus
.........................................................................................................................................++
.................................................................++
e is 65537 (0x10001)
[root@localhost ~]# ll CA.key 
-rw-------. 1 root root 3243 Feb  2 06:33 CA.key
```
而随后可利用rsa子命令生成的私钥文件中提取公钥，rsa子命令的语法为：
>openssl rsa [-inform PEM|NET|DER] [-outform PEM|NET|DER] [-in filename] [-passin arg] [-out filename] [-passout arg] [-sgckey] [-des] [-des3] [-idea] [-text] [-noout] [-modulus] [-check] [-pubin] [-pubout] [-engine id]

### 常用选项为：
>-in FILENAME：指明私钥文件的存放路径；
-out FILENAME：指明将公钥的保存路径；
-pubout：根据提供的私钥，从中提取出公钥；

如：
```
[root@localhost ~]# openssl rsa -pubout -in CA.key 
writing RSA key
-----BEGIN PUBLIC KEY-----
MIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIICCgKCAgEA0r92sttB5yUOI3nE2nvj
PeTZaKkFw2f4cVy8x615afGDhw/XvfWqd2X3BqUy9pPyVoYLOrO0fvGWtx0zVy76
HZ/N3vkUdmzQlJahwKl+K2rVYl2U7fw+qO1UHzrvnNqe6p10KURwAsD1nhuRf/ra
SlxUuOPLNjyu5QeSjtoMuYbhk72M+ht+vNuZI8i2e9B6t6HzoHvnmxldjj+4tQje
BCxeWwaerb8iWZ8KiNDGtqu1X20EevvJY7sp7RzzUPT4EKrXQ6BUyl+VeodHiHxp
l/8gVdlDEYIyurjBwNJDl3I+ug+MZwB0BaPSqNdbgcQbwdM/E6SiBIKU366XkZ39
uDneIZEaZIe12k3MlxqvXyLrsHc2V4jNdK+BNF0bU8pd8Z0wJ7B+Fl/k1+4fD5hS
WLOziix36WrqWzgSgOAV4oEwZjLfBTWIPEcDLUO2LhrhHv4S9APi4FAIslu8QlHv
dkzHaG0e6zolsIAHa1wClTVwFFfmABmo2axpc3IAu9EQA4lLJwK5MiDlANHJBTY7
HXlAOGADgJXY3euiUB4oQ/WPcP2XPTmRQcYoey3hRETPbJd6heM6Rfx9TyCxjxeo
xStmZmhHKZZek+h14Q/hmaK946SkPcbbszL1WzK/STwpceDgnijMgStq6fIippLb
zaQiGXIq0SE8FGuYnCTYJPMCAwEAAQ==
-----END PUBLIC KEY-----
```

