---
title: Linux学习记录--有趣的linux命令
date: 2017-04-09 14:21:00
tags: [Linux, 后端]
keywords: [Linux, 后端]
categories: 技术
description: Linux学习记录--有趣的linux命令
toc: true
---

## 输出图形字符的命令
**banner**:
  * 安装: `sudo apt install sysvbanner`
  * eg: `banner linux`
  * ![](Linux学习记录--有趣的linux命令/banner.png)

**printerbanner**:
  * eg: `printerbanner -w 30 A` `-w`指定宽度
  * ![](Linux学习记录--有趣的linux命令/printerbanner.png)

**toilet**:
  * eg: `toilet love`
  * ![](Linux学习记录--有趣的linux命令/toilet.png)

**figlet**:
  * eg: `figlet love`
  * ![](Linux学习记录--有趣的linux命令/figlet.png)

## 监督你的眼睛
`xeyes`
后台运行: `nohup xeyes &`
只能运行在图形界面

## 数字雨
`cmatrix`
![](Linux学习记录--有趣的linux命令/cmatrix.png)

## 跳动的火焰
`aafire`
安装: `sudo apt install libaa-bin`
![](Linux学习记录--有趣的linux命令/aafire.gif)

## 幸运饼干
`fortune`: 打印出一段随机的名言
![](Linux学习记录--有趣的linux命令/fortune.png)

## 以动物说话形式打印出一段话
`cowsay`
`cowsay -l`: 打印出所有支持的动物种类
`cowsay -f elephant hello world`: `-f`指定动物类型
`fortune | cowsay -f daemon`: 可以与`fortune`命令相结合
![](Linux学习记录--有趣的linux命令/cowsay.png)

## Space Invaders
`ninvaders`
![](Linux学习记录--有趣的linux命令/ninvaders.gif)

## 彩色的火焰
`cacafire`
安装: `sudo apt install caca-utils`
![](Linux学习记录--有趣的linux命令/cacafire.gif)

## bb
`bb`
![](Linux学习记录--有趣的linux命令/bb.gif)
