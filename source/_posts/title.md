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
