---
title: 你了解linux文件或目录默认权限吗？
date: 2020-07-17 21:23:23
tags:
    - 安全知识
---

当我们登录系统之后创建一个文件总是有一个默认权限的，那么这个权限是怎么来的呢？这就不得不讲umask了。umask设置了用户创建文件的默认权限，它与chmod的效果刚好相反，umask设置的是权限“补码”，而chmod设置的是文件权限码。一般在/etc/profile、$ [HOME]/.bash_profile或$[HOME]/.profile中设置umask值。

<!--more-->

# 一、什么是umask？
你的系统管理员必须要为你设置一个合理的 umask值，以确保你创建的文件具有所希望的缺省权限，防止其他非同组用户对你的文件具有写权限。在已经登录之后，可以按照个人的偏好使用umask命 令来改变文件创建的缺省权限。相应的改变直到退出该shell或使用另外的umask命令之前一直有效。一般来说，umask命令是在/etc /profile文件中设置的，每个用户在登录时都会引用这个文件，所以如果希望改变所有用户的umask，可以在该文件中加入相应的条目。如果希望永久 性地设置自己的umask值，那么就把它放在自己$HOME目录下的.profile或.bash_profile文件中。

# 二、如何计算umask值
umask 命令允许你设定文件创建时的缺省模式，对应每一类用户(文件属主、同组用户、其他用户)存在一个相应的umask值中的数字。对于文件来说，这一数字的最 大值分别是6。系统不允许你在创建一个文本文件时就赋予它执行权限，必须在创建后用chmod命令增加这一权限。目录则允许设置执行权限，这样针对目录来 说，umask中各个数字最大可以到7。
该命令的一般形式为：
`umask xxx`
其中xxx为umask的值，范围是000-777。

* 1、计算方式一

在计算umask值时，可以针对各类用户分别在这张表中按照所需要的文件/目录创建缺省权限查找对应的umask值。

| umask值 | 文件 | 目录 |
| ------- | ---- | ---- |
| 0       | 6    | 7    |
| 1       | 6    | 6    |
| 2       | 4    | 5    |
| 3       | 4    | 4    |
| 4       | 2    | 3    |
| 5       | 2    | 2    |
| 6       | 0    | 1    |
| 7       | 0    | 0    |

例如，umask值002 所对应的文件和目录创建缺省权限分别为644和775。


* 2、计算方式二

另外一种计算umask值的方法。我们只要记住umask是从权限中“拿走”相应的位即可。
例如，对于umask值002，相应的文件和目录缺省创建权限是什么呢？
>第一步，我们首先写下具有全部权限的模式，即777 (所有用户都具有读、写和执行权限)。
第二步，在下面一行按照umask值写下相应的位，在本例中是002。
第三步，在接下来的一行中记下上面两行中没有匹配的位。这就是目录的缺省创建权限。稍加练习就能够记住这种方法。
第四步，对于文件来说，在创建时不能具有执行权限，只要拿掉相应的执行权限比特即可。

这就是上面的例子， 其中umask值为002：
>1) 文件的最大权限 rwx rwx rwx (777)
2) umask值为002 --- --- -w-
3) 目录权限 rwx rwx r-x (775) 这就是目录创建缺省权限
4) 文件权限 rw- rw- r-- (664) 这就是文件创建缺省权限

下面是另外一个例子，假设这次umask值为022：
>1) 文件的最大权限 rwx rwx rwx (777)
2)umask值为022 --- -w- -w-
3) 目录权限 rwx r-x r-x (755) 这就是目录创建缺省权限
4) 文件权限 rw- r-- r-- (644) 这就是文件创建缺省权限

# 三、常用的umask值
下表列出了一些umask值及它们所对应的目录和文件权限。
常用的umask及对应的文件与目录权限

| umask值 | 文件 | 目录 |
| ------- | ---- | ---- |
| 022     | 644  | 755  |
| 027     | 640  | 750  |
| 002     | 664  | 775  |
| 006     | 660  | 771  |
| 007     | 660  | 770  |

# 四、umask命令
如果想知道当前的umask 值，可以使用umask命令：
`$umask`
如果想要改变umask值，只要使用umask命令设置一个新的值即可：
`$ umask 002`
确认一下系统是否已经接受了新的umask值：

查看umask值

```
[root@iZuf6c8miiew84sybwsxsvZ umask]# umask
0022
```
新建文件权限为644
```
[root@iZuf6c8miiew84sybwsxsvZ umask]# touch test.txt
[root@iZuf6c8miiew84sybwsxsvZ umask]# ll
total 0
-rw-r--r-- 1 root root 0 Jul 17 21:17 test.txt
```
新建目录权限为755
```
[root@iZuf6c8miiew84sybwsxsvZ umask]# mkdir test1
[root@iZuf6c8miiew84sybwsxsvZ umask]# ll
total 4
drwxr-xr-x 2 root root 4096 Jul 17 21:18 test1
-rw-r--r-- 1 root root    0 Jul 17 21:17 test.txt
```
修改umask值为027
```
[root@iZuf6c8miiew84sybwsxsvZ umask]# umask 027
```
新建目录权限为750
```
[root@iZuf6c8miiew84sybwsxsvZ umask]# mkdir test2
[root@iZuf6c8miiew84sybwsxsvZ umask]# ll
total 8
drwxr-xr-x 2 root root 4096 Jul 17 21:18 test1
drwxr-x--- 2 root root 4096 Jul 17 21:18 test2
-rw-r--r-- 1 root root    0 Jul 17 21:17 test.txt
```
