---
title: Linux学习记录--命令练习
date: 2017-04-19 22:36:12
tags: [Linux, 后端]
keywords: [Linux, 后端]
categories: 技术
description: Linux学习记录--命令练习
toc: true
---

## 基础操作
1、列出目录下指定的目录: `ls -d 目录名称`
2、思考: 在一个目录下删除这个目录然后使用`pwd`这个命令会打印出什么。仍然会打印出当前所在目录，虽然已经被删除了
3、**mkdir**:
  * `mkdir -m 777 zhou`: 创建权限为777的目录
  * `mkdir -vp shiyanlou/{lib/,bin/,doc/{info,product}}`: 创建多个目录(**只能创建目录，即使加上后缀名也是目录。linux中应该是不以后缀名区分文件类型的**)并显示信息。安装`tree`可以以树状结构显示某个目录

4、`myrm(){D=/tmp/$(date +%Y%m%d%H%M%S); mkdir -p $D; mv "$@" $D && echo "moved to $D ok" || "moved to $D fail"}`: 将一个文件(需要以存在)移动到/tmp/下以当前时间命名的文件夹下。`$@`表示传入脚本的参数。**注意**linux命令行中`'`包括的为字符串，写的是什么就是什么。而`"`包括的里面如果有变量什么的会替换成变量的值，和不用`"`包括是一样的。

5、将`time`命令的执行结果保存到文件中，可以使用如下命令`{ time date; } 2>1.txt`或`(time date) 2>2.txt`。大括号两边必须有空格并且命令以分号结束。还有time命令的输出信息是打印在标准错误输出上的。
