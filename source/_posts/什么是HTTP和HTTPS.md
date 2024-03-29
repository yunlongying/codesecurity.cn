---
title: 什么是HTTP和HTTPS
date: 2020-06-14 12:23:42
tags:
    - 安全知识
---

# 一、HTTP
>HyperText Transfer Protocol，超文本传输协议，是一个基于请求与响应，无状态的，应用层的协议，常基于TCP/IP协议传输数据，是互联网上使用最广泛的一种协议，所有WWW文件必须遵循的标准。HTTP协议传输的数据都是未加密的，也就是明文的，因此使用HTTP协议传输隐私信息非常不安全。设计HTTP的初衷是为了提供一种发布和接收HTML页面的方法。

<!--more-->

* <font size=4>使用TCP端口为：80</font>

## HTTP特点

>1、简单快速：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、HEAD、POST。

>2、灵活：HTTP允许传输任意类型的数据对象。传输的类型由Content-Type加以标记。

>3、无连接：限制每次连接只处理一个请求。服务器处理完请求，并收到客户的应答后，即断开连接，但是却不利于客户端与服务器保持会话连接，为了弥补这种不足，产生了两项记录http状态的技术，一个叫做Cookie,一个叫做Session。

>4、无状态：无状态是指协议对于事务处理没有记忆，后续处理需要前面的信息，则必须重传。



# 二、HTTPS
>Hyper Text Transfer Protocol over Secure Socket Layer，安全的超文本传输协议，网景公式设计了SSL(Secure Sockets Layer)协议用于对Http协议传输的数据进行加密，保证会话过程中的安全性。
HTTPS协议可以理解为HTTP协议的升级，就是在HTTP的基础上增加了数据加密。在数据进行传输之前，对数据进行加密，然后再发送到服务器。这样，就算数据被第三者所截获，但是由于数据是加密的，所以你的个人信息当然是安全的。这就是HTTP和HTTPS的最大区别。

* <font size=4>使用TCP端口默认为443</font>

## HTTPS特点

>1、内容加密：采用混合加密技术，中间者无法直接查看明文内容

>2、验证身份：通过证书认证客户端访问的是自己的服务器

>3、保护数据完整性：防止传输的内容被中间人冒充或者篡改


## HTTPS的缺点
>1、HTTPS协议多次握手，导致页面的加载时间延长近50%；

>2、HTTPS连接缓存不如HTTP高效，会增加数据开销和功耗；

>3、申请SSL证书需要钱，功能越强大的证书费用越高。

>4、SSL涉及到的安全算法会消耗 CPU 资源，对服务器资源消耗较大。


# HTTP和HTTPS区别

>1、https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。

>2、http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。

>3、http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。

>4、http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。


