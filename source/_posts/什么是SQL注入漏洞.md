---
title: 什么是SQL注入漏洞
date: 2020-06-25 20:04:26
tags:
    - 常见web漏洞
---

# 概念
SQL注入（SQLi）是一种注入攻击，可以执行恶意SQL语句。它通过将任意SQL代码插入数据库查询，使攻击者能够完全控制Web应用程序后面的数据库服务器。攻击者可以使用SQL注入漏洞绕过应用程序安全措施；可以绕过网页或Web应用程序的身份验证和授权，并检索整个SQL数据库的内容；还可以使用SQL注入来添加，修改和删除数据库中的记录。

<!--more-->

SQL注入漏洞可能会影响使用SQL数据库（如MySQL，Oracle，SQL Server或其他）的任何网站或Web应用程序。犯罪分子可能会利用它来未经授权访问用户的敏感数据（客户信息，个人数据，商业机密，知识产权等）。SQL注入攻击是最古老，最流行，最危险的Web应用程序漏洞之一。


# 原理

## 攻击原理
SQL注射能使攻击者绕过认证机制，完全控制远程服务器上的数据库。 SQL是结构化查询语言的简称，它是访问数据库的事实标准。目前，大多数Web应用都使用SQL数据库来存放应用程序的数据。几乎所有的Web应用在后台 都使用某种SQL数据库。跟大多数语言一样，SQL语法允许数据库命令和用户数据混杂在一起的。如果开发人员不细心的话，用户数据就有可能被解释成命令， 这样的话，远程用户就不仅能向Web应用输入数据，而且还可以在数据库上执行任意命令了。

1、SQL注入式攻击的主要形式有两种
>1、直接将代码插入到与SQL命令串联在一起并使得其以执行的用户输入变量。
由于其直接与SQL语句捆绑，故也被称为直接注入式攻击法。
2、一种间接的攻击方法，它将恶意代码注入要在表中存储或者作为原数据存储的字符串。
在存储的字符串中会连接到一个动态的SQL命令中，以执行一些恶意的SQL代码。注入过程的工作方式是提前终止文本字符串，然后追加一个新的命令。如以直接注入式攻击为例。就是在用户输入变量的时候，先用一个分号结束当前的语句。然后再插入一个恶意SQL语句即可。由于插入的命令可能在执行前追加其他字符串，因此攻击者常常用注释标记“—”来终止注入的字符串。执行时，系统会认为此后语句被注释，故后续的文本将被忽略，不被编译与执行。

2、SQL注入产生条件：
>1、Web开发人员无法保证所有的输入都已经过滤
2、攻击者利用发送给SQL服务器的输入数据构造可执行的SQL代码
3、数据库未做相应的安全配置-使用非root用户，限制删除权限

3、如何查找SQL漏洞：借助逻辑推理
>1、识别系统的输入点：get、post、HTTP头信息
2、了解哪些类型的请求会触发异常：特殊字符、单引号、双引号
3、检测服务器响应中的异常

4、如何进行SQL注入攻击：
>1、数字注入：id=-1 OR 1=1
2、字符串注入：使用#或者-- 注释掉后面的查询条件


## 分类
1）数字型注入
输入参数为整型时，如Id、年龄和页码等。参数不用被引号括起来，如?id=1 
测试：
id=1 and 1=1

2）字符型注入
输入参数为字符串型时，如姓名、职业、住址等。参数要被引号扩起来,如?name="test"
测试：
当输入的参 x 为字符型时，通常 abc.php 中 SQL 语句类型大致如下： select * from <表名> where id = ‘x’ 这种类型我们同样可以使用 and ‘1’=’1 和 and ‘1’=’2来判断：
Url 地址中输入 ```http://xxx/abc.php?id= x’ and ‘1’=’1 ```页面运行正常，继续进行下一步。
Url 地址中继续输入 ```http://xxx/abc.php?id= x’ and ‘1’=’2``` 页面运行错误，则说明此 Sql 注入为字符型注入。

两者最大的区别：字符型注入一般要使用单引号进行闭合，而数字型注入则不需要；


# 危害
>1、从数据库中读取敏感数据；
2、篡改数据库数据；
3、对数据库执行管理权限操作；
4、执行系统命令导致程序危害发生；

说明：SQL漏洞一般被列为高危漏洞。


# 防御

* <font size=4>1、检查变量数据类型和格式</font>

>如果你的SQL语句是类似where id={$id}这种形式，数据库里所有的id都是数字，那么就应该在SQL被执行前，检查确保变量id是int类型；如果是接受邮箱，那就应该检查并严格确保变量一定是邮箱的格式，其他的类型比如日期、时间等也是一个道理。总结起来：只要是有固定格式的变量，在SQL语句执行前，应该严格按照固定格式去检查，确保变量是我们预想的格式，这样很大程度上可以避免SQL注入攻击。

输入变量检查：
>1、数字注入：判断是否是数字
2、字符串注入：使用正在表达式匹配字符串


* <font size=4>2、过滤特殊符号</font>

>对于无法确定固定格式的变量，一定要进行特殊符号过滤或转义处理。

* <font size=4>3、绑定变量，使用预编译语句</font>　　

>MySQL的 mysqli 驱动提供了预编译语句的支持，不同的程序语言，都分别有使用预编译语句的方法。
实际上，绑定变量使用预编译语句是预防SQL注入的最佳方式，使用预编译的SQL语句语义不会发生改变，在SQL语句中，变量用问号?表示，黑客即使本事再大，也无法改变SQL语句的结构

* <font size=4>4、其他预防SQL注入的方法</font>

>1、不要使用动态SQL
2、不要将敏感数据保留在纯文本中
3、限制数据库权限和特权
4、避免直接向用户显示数据库错误
5、对访问数据库的Web应用程序使用Web应用程序防火墙（WAF）
6、定期测试与数据库交互的Web应用程序
7、将数据库更新为最新的可用修补程序


# 总结
SQL注入是一种流行的攻击攻击方法，但是通过采取适当的预防措施，例如确保数据加密，保护和测试Web应用程序，以及您是最新的补丁程序，您可以采取有意义的步骤来保持您的数据安全。

