---
title: ubuntu折腾记录
date: 2017-05-05 8:42:55
tags: [Linux]
keywords: [Linux]
categories: 技术
description: 记录ubuntu使用过程中遇到的问题和各种折腾
toc: true
---
## Linux,window双系统
[教程](http://www.jb51.net/os/windows/298507.html)

## 软件安装不了问题
缺少软件，需安装GDebi。安装命令`sudo apt-get update` `sudo apt-get install gdebi`

## nodejs安装
在官网Download页面选择Installing Node.js via package manager。ubuntu的命令是`curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -` `sudo apt-get install -y nodejs`

## 任务管理器
按window键搜索system monitor显示图形界面的任务管理器

## 标题栏显示网速
安装软件indicator-sysmonitor,可以命令行启动，也可以搜索出来启动

## 开热点
[教程](http://www.linuxdiyf.com/linux/24898.html)
网络连接的文件位置：`/etc/NetworkManager/system-connections`

## 翻墙
蓝灯直接官网安装
[shadowsocks安装教程](http://blog.csdn.net/scythe666/article/details/52015213)

## 大坑
把ibus卸载后系统桌面也没了，网上找到了修复方法但发现电脑始终连不上网。最后只好重装，先在window下删除ubuntu对应的分区，重启后悲剧了。才想起来现在双系统ubuntu好像是主系统，删除后开机引导出错了。**不能直接删除ubuntu的分区**。用U盘PE修复了引导记录，重新安装了一遍ubuntu。重新配置了开发环境。系统换成英文的，在字符界面就不会出现乱码了，但ibus中文输入法按照教程安装方法总是不成功，很难受。最后下载了搜狗输入法，很方便，还有繁简体转换。linux安装软件有的很麻烦，卸载更是要注意，一些关键的依赖是不能直接卸载的，当时一个回车的时间需要大半天的时间来修复，这还是我新装的ubuntu，没有很多东西的情况下。

## shadowsocks & proxychains 实现终端代理
1、安装proxychains: `sudo apt-get install proxychains`
2、启动shadowsocks
3、编辑`/etc/proxychains.conf`，在最后的proxylist里加入`socks5 127.0.0.1 1080`
4、编辑`usr/bin/proxychains`修改里面的`LD_PRELOAD`的值为系统中`libproxychains.so.3`的位置
5、在运行的程序前加上proxychains就可以了。**注意**：proxychains支持的是socks，http, https协议.它们以tcp或者udp协议为基础, ping命令用的是 ICMP 协议， proxychains 不支持。(原来是这样。。。)

## 安装翻译软件
先尝试的是`translate-shell`这个命令行工具，但连接需要翻墙，这时我才知道shadowsocks只能浏览器进行翻墙，配合switchyomega可以实现浏览器端可选择的使用socks代理。但终端就不行了。我又想起来lantern不是vpn吗，但查了一下说是lantern用的是pac(这个是啥)，不嫩进行终端翻墙。最后找到了一个方案是shadowsocks和proxychains进行终端代理，方法在上面，这回可以翻墙但translateshell还是不好用，不知道是我这边的原因还是那边的问题。。。

于是下载了有道词典，但它没有提供快捷键配置项，用起来会很不方便。打开系统设置进行快捷键设置。先要知道有道词典的启动命令。用`compgen -c | grep youdao`找到命令，进行配置就可以了(但我感觉用了快捷键打开会有延迟，还是命令行打开最快)

## 在系统加入自己写的shell
写了一个shell脚本，命令如下
```
#!/bin/bash
location=~/Documents/attendance.txt
if [ $# -eq 0 ];then
echo "need params"
exit
fi
if [ $1 = "--show" ] || [ $1 = "-s" ];then
more $location
elif [ $1 = "--delete" ] || [ $1 = "-d" ];then
sed -i "/$2/d" $location
if [ $? -eq 0 ];then
echo "DELETED"
fi
else
echo "$(date +%Y/%m/%d) $(env LC_TIME=en_US.UTF-8 date +%A) $@" >> $location
if [ $? -eq 0 ];then
echo "SUCCESS"
else
echo "FAIL"
fi
fi

```
几个重要的点：
1、shell文件全局永久可用: 把shell文件所在目录加入PATH变量并加入到`.bashrc`中。`echo "PATH=$PATH:/home/codearvin/myshell" >> .bashrc`
2、为命令设置一个简短的别名:
  * `alias`显示所有可用别名。
  * `alias ad=attendance.sh`只能短时间生效。
  * 查看家目录下的`.bashrc`文件，里面有对别名的定义，在家目录下创建`.bash_aliases`文件并在里面写入别名格式，就会永久生效了。
3、有个坑: `cat test | grep -v somevalue > test`这样test文件就变为空了。属于线程干扰的问题？管道几侧的程序是同时运行的
4、我的终端语言是中文，但我想date显示的全是英文，用下面这个命令: `env LC_TIME=en_US.UTF-8 date`就可以了


## 哈哈 thefuck？
这个工具好玩，可以自动纠正前一个命令的拼写错误
安装:
  * `sudo apt update`
  * `sudo apt install python3-dev python3-pip`
  * `pip3 install --user thefuck`
  * 创建一个别名 `vim ~/.bashrc`
  * 在文件尾加入一行： `eval "$(thefuck --alias fuck)"`
  * 使生效 `source ~/.bashrc`
