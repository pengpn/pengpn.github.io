---
title: 使用hexo和github搭建个人博客
date: 2017-05-10 22:16:19
tags: #hexo #github
---
## 使用hexo和github搭建个人博客
### 一、安装git
### 二、安装node.js
> 1. Node可以去官网下载，或是在国内下载，由于众所周知的原因，这里放一个[nodejs.cn](http://nodejs.cn/)的链接
> 2. Node内置npm包，我们之后就可以打git bash使用npm进行安装一些依赖，如果觉得太慢，可以使用淘宝镜像cnpm.  
> 3. Git bash 如果提示npm command not found 请查看用户变量有没加上nodejs的路径 
<!--more--> 
#### npm安装hexo速度过慢
- 由于某些大家都知道的缘故，npm官方源在国内的下载速度极其慢，用官网的npm install hexo-cli -g速度非常感人，所以不推荐这种方式。  
- 这里我推荐用淘宝的npm分流——cnpm  
- 安装过程很简单：  
npm install -g cnpm --registry=https://registry.npm.taobao.org  
- 然后等着装完即可，之后的用法和npm一样，无非是把npm install改成cnpm install,但是速度比之前快了不止一个数量级(不过下文为了方便理解，还是会用默认的npm安装，如果你发现速度不好的话，请自行替换成’cnpm’)

### 三、安装Hexo
- 在任意位置点击鼠标右键，选择Git Bash。
- npm install -g hexo-cli 或者 cnpm install -g hexo-cli  
注意：-g是指全局安装hexo。
- 自己新建一个hexo文件夹，例如我就在E:/hexo，  
在此目录下执行hexo init /** 创建一个Hexo的新框架 **/
- 执行hexo generate #生成静态文档
- 执行hexo server #运行本地服务  
访问localhost:4000 可以看到静态页面。  
如果不能访问，那就是其他占用了4000这个端口，hexo默认端口4000，如果你的电脑安装了福昕阅读器，，就是他。此时可以使用hexo server -p 5000切换端口，访问localhost:5000即可。
![](http://i2.muimg.com/588926/1fb1b71ee4564a77.png)

```
├── .deploy       #需要部署的文件
├── node_modules  #Hexo插件
├── public        #生成的静态网页文件
├── scaffolds     #模板
├── source        #博客正文和其他源文件, 404 favicon CNAME 等都应该放在这里
|   ├── _drafts   #草稿
|   └── _posts    #文章
├── themes        #主题
├── _config.yml   #全局配置文件
└── package.json
```


### 四、github
#### 创建Repository
- 创建的时候注意Repository的名字。比如我的Github账号是pengpn，那么我应该创建的Repository的名字是：pengpn.github.io。一定要相同的对应上去。
#### 修改配置文件
- _config.yml

```
deploy:
  type: git
  repo: git@github.com:pengpn/pengpn.github.io.git
  branch: master
```
- 执行hexo generate
- 执行hexo deploy部署
- 我们的博客就已经完全搭建起来了
- 访问pengpn.github.io

> 注意：每次修改本地文件后，需要键入hexo generate才能保存。每次使用命令时，都要在C:\Hexo目录下。每次想要上传文件到Github时，就应该先键入hexo generate保存之后，再键入hexo deploy。大概成功之后是酱紫的


### 五、域名绑定
因为我是在万网上注册的域名，所以这里就是使用阿里云  
- 点击添加解析按钮，如图一次输入：CNAME、@、Github博客域名。选择保存完成个人域名向个人博客的映射。添加解析后，在浏览器输入我们新注册的域名：
![](http://i2.muimg.com/588926/68274bd7aac1e095.png)
- 可以看到网站报出了404错误，这说明我们的域名已经成功映射到了Github网站，但是它找不到我们的博客的位置，所以我们需要实现个人博客向个人域名的映射，进入Github博客的仓库：
![image](http://upload-images.jianshu.io/upload_images/291600-b616fdfde172b082.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 点击上图上方偏右的Create new file按钮，创建一个文件：

![](http://i4.buimg.com/588926/48d84b76ccbf211b.png)

- 文件名为CNAME(注意：没有扩展名)，文件内容为个人域名(注意：没有http://，没有www)，然后选择下方的Commit new file按钮。然后在浏览器端重新输入我们的域名，我们可以看到域名绑定成功：
![](http://i1.piimg.com/588926/64845a166756a19b.png)

### 六、书写文章
#### 新建文章和新建页面
```
hexo new "title"  # 生成新文章：\source\_posts\title.md
hexo new page "title"  # 生成新的页面，后面可在主题配置文件中配置页面
```
生成文章或页面的模板放在博客文件夹根目录下的 scaffolds/ 文件夹里面，文章对应的是 post.md ，页面对应的是page.md，草稿的是draft.md
###编辑文章  
打开\source\_posts\对应的名字.md 例如这里就是title.md
```
 ---
 title: 使用hexo和github搭建个人博客
date: 2017-05-10 22:16:19
tags: #hexo #github
---
 #这里开始使用markdown格式输入你的正文。
<!--more--> 
#more标签以下的内容要点击“阅读全文”才能看见，#more标签以上的内容为你首页显示文章的摘要部分
```

### 常用命令总结
> hexo init [folder] # 初始化一个网站。如果没有设置 folder ，Hexo 默认在目前的文件夹建立网站
hexo new [layout] <title> # 新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。如果标题包含空格的话，请使用引号括起来
hexo version # 查看版本
hexo clean # 清除缓存文件 (db.json) 和已生成的静态文件 (public)
hexo g # 等于hexo generate # 生成静态文件
hexo s # 等于hexo server # 本地预览
hexo d # 等于hexo deploy # 部署，可与hexo g合并为 hexo d -g

## 七、安装主题
### 热门主题
- [iissnan/hexo-theme-next](https://github.com/iissnan/hexo-theme-next)， 7859个star。
- [litten/hexo-theme-yilia](https://github.com/litten/hexo-theme-yilia)， 3398个star。
- [TryGhost/Casper](https://github.com/TryGhost/Casper)， 901个star。
- [wuchong/jacman](https://github.com/wuchong/jacman)， 767个star。
- [A-limon/pacman](https://github.com/A-limon/pacman)， 501个star。
- [daleanthony/uno](https://github.com/daleanthony/uno)， 467个star。
- [orderedlist/modernist](https://github.com/orderedlist/modernist)， 394个star。
### 主题下载
把下来的文件夹解压和更名为next，并复制到theme目录下
### 配置文件
- 在根目录下的_config.yml主要是对网站的总属性进行设置
如：网站标题，网站logo,网站插件使用等全局的属性
- 主题目录下的_config.yml主要是针对网站的布局，导航等特性设置进行设置
### 启用主题
打开站点配置文件_config.yml， 找到 theme 字段，并将其值更改为 next
```
theme: next
```
**注意:后有个空格必须要有空格哦**
然后 hexo s 即可在localshost:4000地址里预览主题效果
### 更换主题外观
next主题有三个样式
```
# Schemes
#scheme: Muse
scheme: Mist
#scheme: Pisces
```
### 更换语言
更换语言为中文，在根目录配置文件下配置language: zh-Hans
```
# Site
title: PENGPN
subtitle: 生活、技术个人博客
description: Think
author: PENGPN
language: zh-Hans
timezone:
```
### 更多设置
大部分的设定都能在NexT的官方文档 里面找到，如侧栏、头像、打赏、评论、订阅、连接、分享、数据统计等等，在此就不多讲了，照着文档走就行了，接下只是个性定制的问题。
http://theme-next.iissnan.com/getting-started.html


### 参考
- [史上最详细“截图”搭建Hexo博客并部署到Github](http://jingyan.baidu.com/article/d8072ac47aca0fec95cefd2d.html)
- [2017年最新基于hexo搭建个人免费博客——从零开始](https://segmentfault.com/a/1190000008738089)
- [hexo博客搭建时遇到的一些问题](http://chitanda.me/2015/06/11/tips-for-setup-hexo/)
- [Hexo折腾记](https://segmentfault.com/a/1190000006831597)
- [手把手教你用Hexo+Github 搭建属于自己的博客](http://blog.csdn.net/slow_wakler/article/details/57080576)
- [利用Github Pages + Hexo搭建个人博客](http://threehao.com/2016/08/22/Github%20Pages%20+%20Hexo/)
- [使用 Hexo 生成一套静态博客网页](https://ninghao.net/blog/1412)
[记录第一次搭建hexo](http://www.jianshu.com/p/017e01718d41)
- [hexo搭建的Github博客绑定域名](http://www.jianshu.com/p/cea41e5c9b2a?open_source=weibo_search)
