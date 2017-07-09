---
title: GitHub +Hexo 博客多设备同步问题
date: 2017-07-08 19:34:39
tags: GitHub+Hexo
---
本文只讲解如何实现多设备更新问题，基于GitHub和Hexo创建博客问题请另参考其他文章

### 在搭建博客的设备上，push本地文件夹Hexo中的必要文件到yourname.github.io仓库的hexo的分支上

在利用Github和Hexo搭建博客时，在本地新建了Hexo文件夹，里面存在构建时需要的所有配置文件，因此需要将配置文件托管到Github上，以供在其余终端更新博客时获得相应的配置文件。
操作步骤如下：
``` bash
git init //初始化本地版本库
git add source //将本地资源添加到暂存区
git commit -m"xxx" //将暂存区资源添加到版本库
git branch hexo //创建分支hexo
git checkout hexo //跳转到分支hexo
git remote add origin 仓库地址 //将本地与github远程仓库对接
git push origin hexo //push到github项目的分支hexo上
```
此时相应的GitHub仓库会新建一个hexo分支，这个分支保存了所有的配置信息，时实现多终端同步的关键
<!--more-->
### 在执行不同终端上完成clone和push更新
为了实现在不同终端上都可以更新文章，在所有的终端上（包括搭建博客时所用的终端）都要执行以下操作：
``` bash
git clone -b hexo 仓库地址 //将博客所在仓库的hexo分支clone到本地
cd yourname.github.io //切换路径
npm install //安装npm依赖包
```
此时，该终端已经可以更新博客了
具体步骤如下：
``` bash
hexo new post "文章名字" //生成新的博客文章
git add source // 将博客文章添加到暂存区
git commit -m"xxx" // 提交博客文章到版本库
git push origin hexo //将博客文章推送到远程仓库的hexo分支(以保证其余终端可以pull,用于同步)
hexo g //生成
hexo d //发布
```
### 不同终端更新文章
为了实现在不同终端上更新文章，需要在所有终端上操作如下步骤：
``` bash
git pull origin hexo //拉取远端最新的资源，更新本地（必须要做，否则不能保证同步更新）
```
完成pull操作后，就可以在此终端上写博文了，步骤如下：
``` bash
hexo new post "博文名称" //新建博文
git add source
git commit -m"xx"
git push origin hexo
hexo g
hexo d
```