---
title: 用GitHub搭建Hexo
date: 2020-08-01 12:57:02
tags:
 - 折腾
 - Hexo
---

<!--more-->

操作系统：macOS High Sierra（黑苹果）

| 参考网站                                                     |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [使用hexo搭建github博客](https://www.jianshu.com/p/1bcad7700c46) | [hexo+git搭建博客一步一个坑](https://www.jianshu.com/p/644eee0a4827) |

### 创建GitHub仓库

先在[GitHub](https://github.com/)创建一个仓库`New repository`

仓库名字：`用户名.github.io`

并且需要勾选`Initialize this repository with a README`（虽然我没有做过这步）

创建完之后，在仓库的`Settings`→`GitHub Pages`→`Custom domain`绑定你的域名，将会在`Code`那边生成一个`CNAME`文件，它的内容就是你所绑定的域名

> PS.**中文域名**和转码后的中文域名不共通！如果你用了**中文域名**，那么你绑定的域名只能是**中文域名**、不能用转码的！

### 电脑安装Git和Node.js

下载Git客户端：https://git-scm.com/download/mac

下载Node.js：http://nodejs.org/zh-cn/

如果你不确定是否安装成功，可用下面的命令查看（也可用于查看版本）

`node -v` #查看Node.js版本

`git --version` #查看Git版本

### 安装Hexo

首先，在电脑里找个地方创建文件夹，随便找地方，只要你记得住路径就好

（路径：`硬盘\用户名\blog`）

> PS.最好是建在用户名下面，比较方便点

用命令行进入文件夹

```ruby
cd /users/用户名/blog
```

如果是直接在用户名下面创建文件夹直接：`cd blog`就可进入

这时候，你终端前面就会变成

`电脑名称:blog 用户名`表示已进入你创建的blog文件夹

输入`npm install hexo -g`开始安装Hexo

输入`hexo -v`检查hexo是否安装成功

版本号成功显示，输入`hexo init`初始化该文件夹（这里会比较久，可以去看看别的，过会再来，可能有10~30分钟）

一段时间后，看到显示`“Start blogging with Hexo！”`就说明初始化好了

输入`npm install`安装所需要的组件

安装好后，使用下面命令

`hexo g` #generate 生成静态文件

`hexo s` #server 启动服务器。默认情况下，访问网址为：http://localhost:4000/

一般写完想看看有没有什么问题，用`hexo g`部署之后，再用`hexo s`启动服务器，在本地查阅一遍

### 连接Hexo和Git

重新打开终端（这里我只会返回系统文件夹，有点笨就不写了）

设置你的用户名与邮件地址

```ruby
git config --global user.name "你的GitHub用户名"
git config --global user.email 你的GitHub邮箱
```

使用ssh-keygen生成私钥和公钥

```ruby
ssh-keygen -t rsa
```

首先会出现`/users/用户名/.ssh/id_rsa`这串代码，敲个回车就好

然后下面是`XXXX(y/n)？`选y（确定）

还会让你设置一个密码，会在后面**每次上传博客的时候用到，请务必记住密码**，随你设置，纯数字也行

然后会给你生成两个秘钥文件`id_rsa`和`id_rsa.pub`

在GitHub添加`SSH keys`

登录`GitHub`→点击个人资料`Settings`→`SSH and GPG keys`→`New SSH key`

`title`随便写个，然后将`id_rsa.pub`的内容复制粘贴进去

保存之后在终端输入`ssh -T git@github.com`

如果SSH文件夹里没有公钥的话，会在这里提示你是否创建一个公钥，确认就好，然后输入你刚刚设置的密码，创建一个公钥（如果有多个博客最好分开备份一下，因为会串道）

测试SSH是否成功，等待反馈，如果反馈的是你的用户名就说明成功了

到此，就成功了一大半了

然后我们要去修改一下Hexo根目录下面的`_config.yml`文件

里面有个`Deployment`更改成自己的

```ruby
#Deployment

##Docs: https://hexo.io/docs/deployment.html

deploy:
  type: git
  repository: git@github.com:用户名/用户名.github.io.git
  branch: master
```

HexoBlog部署到git我们需要安装`npm install hexo-deployer-git --save`插件,在blog目录下运行一下命令进行安装

### Hexo一些命令

`hexo n <title>` # 创建文章

`hexo n page <title>` # 创建页面

`hexo clean` # 清除缓存文件 (db.json) 和已生成的静态文件 (public)

`hexo g` #generate 生成静态文件

`hexo s` #server 启动服务器。在本地预览效果，默认情况下，访问网址为： http://localhost:4000/

`hexo d` #deploy 部署网站同步到github。部署网站前，需要预先生成静态文件

`hexo clean && hexo g && hexo d` #清除缓存&&生成文件&&部署到网站

### 上传到GitHub失败

提示错误`push declined due to email privacy restrictions`到GitHub网站，点击`setting`再点击`Emails`看看下面的`Email preferences`第二项`Block command line pushes that expose my email`是否打勾了，把打勾取消掉就好

### 后记

折腾起来还是很简单的，就是一开始各种不懂命令行的意义，所以搞混也很正常

### CNAME

突然想起来还有这个坑

经常会重新部署之后绑定的域名就上不去了，是因为CNAME文件被删除啦！

那么要怎么办呢，我也试过插件，但是后来发现，直接丢到你本地Hexo博客的source文件夹里`/users/用户名/blog/source/`就好了，每次都会给你重新部署到public里`/users/用户名/blog/source/public/`