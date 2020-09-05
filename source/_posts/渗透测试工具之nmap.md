---
title: 渗透测试工具之nmap
date: 2020-06-26 12:35:50
tags:
    - 渗透测试
---
# 简介
Nmap  （网络映射器）是Gordon Lyon最初编写的一种安全扫描器，用于发现计算机网络上的主机和服务，从而创建网络的“映射”。为了实现其目标，Nmap将特定数据包发送到目标主机，然后分析响应.NMAP强大的网络工具，用于枚举和测试网络。

<!--more-->

Nmap是一款开源免费的网络发现（Network Discovery）和安全审计（Security Auditing）工具。软件名字Nmap是Network Mapper的简称。一般情况下，Nmap用于列举网络主机清单、管理服务升级调度、监控主机或服务运行状况。Nmap可以检测目标机是否在线、端口开放情况、侦测运行的服务类型及版本信息、侦测操作系统与设备类型等信息。


### NMAP的功能包括：
>1、主机发现
识别网络上的主机。例如，列出响应TCP和/或ICMP请求或打开特定端口的主机。
2、端口扫描
枚举目标主机上的开放端口。
3、版本检测
询问远程设备上的网络服务以确定应用程序名称和版本号。
4、OS检测
确定网络设备的操作系统和硬件特性。

首先需要进行主机发现，随后确定端口状态，然后确定端口上运行的具体应用程序和版本信息，然后可以进行操作系统的侦测。而在这四项功能的基础上，nmap还提供防火墙和 IDS 的规避技巧，可以综合运用到四个基本功能的各个阶段。另外nmap还提供强大的NSE(Nmap Scripting Language)脚本引擎功能，脚本可以对基本功能进行补充和扩展。


### Nmap的优点：
>1、灵活
支持数十种不同的扫描方式，支持多种目标对象的扫描
2、强大
Nmap可以用于扫描互联网上大规模的计算机
3、可移植
支持主流操作系统：Windows/Linux/Unix/MacOS等等；源码开放，方便移植
4、简单
提供默认的操作能覆盖大部分功能，基本端口扫描nmap targetip，全面的扫描```nmap –A <目标IP>```
5、自由
Nmap作为开源软件，在GPL License的范围内可以自由的使用
6、文档丰富
Nmap官网提供了详细的文档描述。Nmap作者及其他安全专家编写了多部Nmap参考书籍
7、社区支持
Nmap背后有强大的社区团队支持


# 安装
官网下载```https://nmap.org/download.html```对应操作系统的安装包，一键安装。
Linux可以直接rpm命令安装：
```
rpm -vhU https://nmap.org/dist/nmap-7.80-1.x86_64.rpm
```

也可以手功能下载rpm包，然后手工安装。
```
[root@iZuf~]# nmap -v
Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-26 12:29 CST
Read data files from: /usr/bin/../share/nmap
WARNING: No targets were specified, so 0 hosts scanned.
Nmap done: 0 IP addresses (0 hosts up) scanned in 0.04 seconds
           Raw packets sent: 0 (0B) | Rcvd: 0 (0B)
```

# 用法
```
sT   TCP connect()扫描，这种方式会在目标主机的日志中记录大批连接请求和错误信息。
-sS      半开扫描，很少有系统能把它记入系统日志。不过，需要Root权限。
-sF  -sN     秘密FIN数据包扫描、Xmas Tree、Null扫描模式
-sP      ping扫描，Nmap在扫描端口时，默认都会使用ping扫描，只有主机存活，Nmap才会继续扫描。
-sU      UDP扫描，但UDP扫描是不可靠的
-sA      这项高级的扫描方法通常用来穿过防火墙的规则集
-sV      探测端口服务版本
-Pn      扫描之前不需要用ping命令，有些防火墙禁止ping命令。可以使用此选项进行扫描
-v   显示扫描过程，推荐使用
-h   帮助选项，是最清楚的帮助文档
-p   指定端口，如“1-65535、1433、135、22、80”等
-O   启用远程操作系统检测，存在误报
-A   全面系统检测、启用脚本检测、扫描等
-oN/-oX/-oG      将报告写入文件，分别是正常、XML、grepable 三种格式
-T4      针对TCP端口禁止动态扫描延迟超过10ms
-iL      读取主机列表，例如，“-iL C:\ip.txt”
```


# 举例

1）端口扫描
>nmap默认发送一个ARP的PING数据包，来探测目标主机1-1000范围内所开放的所有端口
nmap <目标IP>

2）nmap详细扫描
>并对结果返回详细的描述输出。其中：-vv =切换以激活日志所需的非常详细的设置。
nmap -vv <目标IP>

3）指定端口扫描范围
>nmap默认只扫描1-10000范围内的端口，一般应用自定义端口都大于10000，所以通过-p参数指定端口范围。一般全端口扫描，即1-65535
nmap -p1-65535 <目标IP>


4）操作系统类型的探测
>nmap -O <目标IP>


5）nmap万能开关
>nmap -A <目标IP>

