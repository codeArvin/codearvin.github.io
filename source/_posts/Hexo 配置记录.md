---
title: Hexo 配置记录
date: 2017-10:30 12:10:11
tags: [hexo]
keywords: [hexo]
categories: 技术
description: Hexo功能配置的记录
toc: true
---
## 为文章添加图片
1、将hexo的_config.yml中的post_asset_folder值设为true，这样每次新建文章时候_posts文件夹下除了你新建的文章还会有一个和你文章名称一样的文件夹，这个文件夹下可以放置和你文章相关的资源
2、运行`npm install https://github.com/CodeFalling/hexo-asset-image --save`
3、在引用图片的时候这样写就可以了：`![logo](文章名称/logo.jpg)`

## 添加评论功能
1、由于maupassant主题评论自带disqus，所以方便很多
2、注册disqus的账号，找到Add Disqus To Site。按照指引，需要你填写自己的唯一disqus识别名称。
3、将上一步的识别名称填写到主题的_config.yml的disqus后面即可
4、如果不是maupassant主题，需要在hexo的配置文件中添加`disqus_shortname: 你的disqus识别名称`

多说配置方式也是一样，注册账号。填写唯一标识就可以了

## hexo的备份

之前不怎么注重备份，在一次恢复系统的时候搞丢了部分本地文件，最后只好自己手动一个一个的恢复。所以自己重要的东西一定要备份好

1. 如果是mac系统的话，直接用系统的 Time Machine 在备份系统的时候顺便就会把hexo进行了备份
2. 使用插件`hexo-git-backup`进行备份。[参考文章](http://hexo.mantoujun.top/2017/04/25/%E5%88%A9%E7%94%A8%20git-backup%20%E6%8F%92%E4%BB%B6%E5%A4%87%E4%BB%BDhexo%E6%95%B0%E6%8D%AE/)
  * **安装**：`npm install hexo-git-backup --save`,hexo版本需为`3.x.x`
  * **添加配置**：在`hexo/_config.yml`添加
  ```javascript
  backup:
    type: git
      	message: update xxx //可选，diy信息
      	theme: coney,landscape  //可选，主题备份
    repository:
        github: git@github.com:xxx/xxx.git,branchName //备份的目的路径，必填branch分支
        gitcafe: git@github.com:xxx/xxx.git,branchName //可选，备份到gitcafe
  ```
  * **用法**：`hexo backup` OR `hexo b`
  * **恢复**：
    * 安装 hexo 环境
    * `git clone`执行克隆
    * 将 backup（取决于备份时候的设定） 分支复制到 hexo 目录
    * 执行`hexo -d`, 重新配置账户
