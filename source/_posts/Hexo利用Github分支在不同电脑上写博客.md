---
title: Hexo利用Github分支在不同电脑上写博客
date: 2017-05-12 21:34:47
tags: [hexo,github]
comments: true
---
##Hexo利用Github分支在不同电脑上写博客
### 前言
hexo部署上去github上面的是存放着的静态资源文件，而如果想要在其他电脑进行git clone下来继续书写文章是不可以的。我们需要做的是继续使用存放Hexo生成的网站原始的文件。可以新建一个仓库来存放hexo的原始文件，但是考虑到如果每个GitHub Pages都需要额外的一个仓库存放这些文件，就显得特别冗余了。
### 怎么做
这个时候就可以用分支的思路！一个分支用来存放Hexo生成的网站原始的文件，另一个分支用来存放生成的静态网页。
<!--more--> 

#### 新建hexo分支
默认一个 master 分支的，Github page要求你的网站文件必须存放在这个 master 分支上，这个没得选；所以我们需要新建另外一个分支来保存我们的hexo原始文件；

![Markdown](http://i2.muimg.com/1949/b26128e044370454.png)
#### 确认配置hexo deploy参数
为了保证 hexo d 命令可以正确部署到 master 分支，在hexo 的配置文件 _config.yml 文件中配置参数如下：
```
deploy:
  type: git
  repo: git@github.com:pengpn/pengpn.github.io.git
  branch: master

```
如果你发现无法 hexo d ，使用下面的命令安装git deployer插件后重试即可。
```
npm install hexo-deployer-git --save
```
#### 推送到hexo分支
确保当前是hexo分支
```
git checkout hexo
```
上一步的deploy参数正确配置后，文章写完使用 hexo g -d 命令就可以直接部署了，生成的博客静态文件会自动部署到 username.github.io 仓库的 master 分支上，这时候通过浏览器访问 http://username.github.io 就可以看到你的博客页面里。

网站页面是保存了，但这时候我们还没有保存我们的hexo原始文件，包括我们的文章md文件，我们千辛万苦修改的主题配置等。。。接下来使用下面的步骤将他们都统统推送到 hexo 分支上去
```
git add .

git commit -m “change description”

git push origin hexo
```
这样就OK了，我们的原始文件就都上去了，换电脑也不怕了。

####日常写博客
有时候我们可能会在不同的电脑上写博客，那在不同的电脑上配置 hexo、git、node.js，以及配置git ssh key等都要折腾一下的，这是免不了的
##### 已有环境
如果在电脑上已经写过博客，那么可以在已有的工作目录下同步之前写的博客。在你的仓库目录下右键’git bash shell’，起来bash命令行，然后

git pull

这样你的状态就更新了，之后就是 hexo 命令写文章啦。。。

写完 hexo g -d 部署好后，使用
```
git add .

git commit -m “change description”

git push origin hexo
```

推送上去。
##### 新的环境
到了新的电脑上时，我们需要将项目先下载到本地，然后再进行hexo初始化。
（记得，不需要hexo init这条指令）。
```
git clone git@github.com:pengpn/pengpn.github.io.git

cd pengpn.github.io

npm install hexo

npm install

npm install hexo-deployer-git –save
```
之后开始写博客，写好部署好之后，别忘记 git add , ….git push origin hexo…推上去。。。