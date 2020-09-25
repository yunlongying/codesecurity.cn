---
title: Python常用命令
date: 2020-06-26 22:22:22
tags:
    - 随笔
---

本文主要总结分享Python使用过程中的一些小技巧，重点关注Python命令，不包括Python代码相关的使用技巧。主要包括pip包管理、启动HTTP服务、格式化Json字符串等等。

<!--more-->

## 一、安装依赖包
| Pip命令                                                           | 功能                                   |
| ------------------------------------------------------------------- | ---------------------------------------- |
| pip list                                                            | 列出已安装的包                    |
| pip freeze > requirements.txt                                       | 导出已经安装的依赖包到requirements.txt |
| pip install <包名>                                                | 安装依赖包                          |
| pip install -r requirements.txt                                     | 安装requirements.txt中的所有依赖包 |
| pip uninstall <包名>                                              | 卸载依赖包                          |
| pip uninstall -r requirements.txt                                   | 卸载requirements.txt中的所有依赖包 |
| pip install -U <包名>                                             | 升级依赖包                          |
| pip install -U pip                                                  | 升级pip本身                          |
| pip show -f <包名>                                                | 显示包所在的目录                 |
| pip search <搜索关键字>                                        | 搜索依赖包                          |
| pip list -o                                                         | 查询可以升级的依赖包           |
| pip download django -d ./ --trusted-host mirrors.cloud.aliyuncs.com | 下载依赖包，不安装              |
| pip download -d ./ -r requirements.txt                              | 下载requirements.txt中的所有依赖包到本地 |
| pip install django -i ```https://mirrors.aliyun.com/pypi/simple```        | 指定镜像源安装依赖包           |

说明：
1、wheel 本质上是一个 zip 包格式，用于 python 模块的安装，它的出现是为了替代 Eggs。
pip也可以直接安装wheel包。如果发布模块，推荐使用 wheel 格式。

2、国内pypi镜像
阿里：```https://mirrors.aliyun.com/pypi/simple```
中国科学技术大学：```http://pypi.mirrors.ustc.edu.cn/simple/```

3、指定全局安装源
在unix和macos，配置文件为：$HOME/.pip/pip.conf
在windows上，配置文件为：%HOME%\pip\pip.ini
[global]
  timeout = 6000
  index-url = ```https://mirrors.aliyun.com/pypi/simple```


## 二、启动HTTP服务
当需要临时分享或者传递比较大的文件时，启动一个HTTP服务，然后把HTTP链接发给对方，对方就可以通过网页下载文件，很方便。

1）Python2启动HTTP服务
python -m SimpleHTTPServer 端口
```
[root@iZuf6c8miiew84sybwsxsvZ pkg]# python -m SimpleHTTPServer 9998
Serving HTTP on 0.0.0.0 port 9998 ...
```

2）Python3启动HTTP服务
python -m http.server 端口
```
C:\blog> python -m http.server 9998
Serving HTTP on 0.0.0.0 port 9998 (http://0.0.0.0:9998/) ...
```

<br />

## 三、格式化接送字符串
在没有格式化json字符串的工具的情况下，可以使用python自带的json.tool模块对json字符串快速进行格式化，方便简单。

1）管道方式
```
PS C:\test> echo '[ { "a":1, "b":2, "c":3, "d":4, "e":5 } ]' | python -m json.tool
[
    {
        "a": 1,
        "b": 2,
        "c": 3,
        "d": 4,
        "e": 5
    }
]
```
2）文件方式
```
PS C:\test> python -m json.tool test.txt
[
    {
        "a": 1,
        "b": 2,
        "c": 3,
        "d": 4,
        "e": 5
    }
]
```
<br />

***

## 四、打印sys.path内容
用python -m site打印当前python环境和site-packages相关调试信息。python -m site作用是显示sys.path的值内容，也就是python搜索模块的目录，作用类似于linux下的PATH。
```
PS C:\test> python -m site
sys.path = [
    'C:\\test',
    'C:\\Users\\ying\\AppData\\Local\\Programs\\Python\\Python37\\python37.zip',
    'C:\\Users\\ying\\AppData\\Local\\Programs\\Python\\Python37\\DLLs',
    'C:\\Users\\ying\\AppData\\Local\\Programs\\Python\\Python37\\lib',
    'C:\\Users\\ying\\AppData\\Local\\Programs\\Python\\Python37',
    'C:\\Users\\ying\\AppData\\Local\\Programs\\Python\\Python37\\lib\\site-packages',
]
USER_BASE: 'C:\\Users\\ying\\AppData\\Roaming\\Python' (doesn't exist)
USER_SITE: 'C:\\Users\\ying\\AppData\\Roaming\\Python\\Python37\\site-packages' (doesn't exist)
ENABLE_USER_SITE: True
```

