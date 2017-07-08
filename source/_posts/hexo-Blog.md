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
git init
git add source
git commit -m"xxx"
git branch hexo
git checkout hexo
git remote add origin 仓库地址
git push origin hexo
```
此时相应的GitHub仓库会新建一个hexo分支，这个分支保存了所有的配置信息，时实现多终端同步的关键

### 在其余设备上完成clone和push更新
此时在其余设备上更新博客时，应当先把GitHub仓库的hexo分支clone下来，并进行配置
操作步骤如下：
``` bash
git clone -b 仓库地址
cd yourname.github.io
npm install
hexo new post "文章名字"
git add source
git commit -m"xxx"
git push origin hexo
hexo g
hexo d
```
### 不同终端更新文章
在不同终端更新文章时的操作步骤如下：
``` bash
git pull origin hexo
hexo new post "新的博文"
git add source
git commit -m"xx"
git push origin hexo
hexo g
hexo d
```