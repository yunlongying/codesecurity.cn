---
title: Python之禅
date: 2020-06-25 23:11:52
tags:
    - 随笔
---

# 背景

* <font size=4>1、Python也被称为是一门清晰的语言</font>

因为它的作者在设计它的时候，总的指导思想是，<font color=red>**对于一个特定的问题，只要有一种最好的方法来解决就好了。**</font>这在由Tim Peters写的python格言（称为The Zen of Python）里面表述为：

<!--more-->

```
There should be one-- and preferably only one --obvious way to do it.
```
有意思的是，这正好和Perl语言（另一种功能类似的高级动态语言）的中心思想TMTOWTDI（There's More Than One Way To Do It）完全相反。这似乎是人们常把Perl和Python互相比较的重要原因。

* <font size=4>2、Python语言是一门清晰的语言的另一个意思是很强的限制性语法</font>

它的作者有意的设计限制性很强的语法，使得不好的编程习惯（例如if语句的下一行不向右缩进）都不能通过编译。这样有意的强制程序员养成良好的编程习惯。其中很重要的一项就是Python的缩进规则。

Python的出身没有 Java、Go那么万众瞩目，早期的Python，在Java、PHP、JS、C++等的重重包围下，尽管受众不广，但仍然得以生存，主要因为Python的设计哲学使其具备了十足的生命力。在这种设计哲学的指引下，Python 逐渐发展成为了一个特别简明友好、容易上手、功能强大的语言。



# Python之禅
>编写 Python 代码时就有了避繁就简的理念，最终在某一天由一个叫 Tim Peters 的人撰写了出来，即 “Python之禅”。它并非出自 Python 创始人之手，但已被官方认可为编程原则。Python之禅就是指Python的修行方法，Python之禅体现了Python的设计理念，下面我们来一起感受一下。

在python解释器中直接输入import this即可查看python之禅。

```
# D:\blog> python
Python 3.7.0 (v3.7.0:1bf9cc5093, Jun 27 2018, 04:59:51) [MSC v.1914 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!

>>>
```

# 翻译和解释

```
Python之禅 by Tim Peters
 
优美胜于丑陋（Python 以编写优美的代码为目标）
明了胜于晦涩（优美的代码应当是明了的，命名规范，风格相似）
简洁胜于复杂（优美的代码应当是简洁的，不要有复杂的内部实现）
复杂胜于凌乱（如果复杂不可避免，那代码间也不能有难懂的关系，要保持接口简洁）
扁平胜于嵌套（优美的代码应当是扁平的，不能有太多的嵌套）
间隔胜于紧凑（优美的代码有适当的间隔，不要奢望一行代码解决问题）
可读性很重要（优美的代码是可读的）
即便假借特例的实用性之名，也不可违背这些规则（这些规则至高无上）
 
不要包容所有错误，除非你确定需要这样做（精准地捕获异常，不写 except:pass 风格的代码）
 
当存在多种可能，不要尝试去猜测
而是尽量找一种，最好是唯一一种明显的解决方案（如果不确定，就用穷举法）
虽然这并不容易，因为你不是 Python 之父（这里的 Dutch 是指 Guido ）
 
做也许好过不做，但不假思索就动手还不如不做（动手之前要细思量）
 
如果你无法向人描述你的方案，那肯定不是一个好方案；反之亦然（方案测评标准）
 
命名空间是一种绝妙的理念，我们应当多加利用（倡导与号召）
```

