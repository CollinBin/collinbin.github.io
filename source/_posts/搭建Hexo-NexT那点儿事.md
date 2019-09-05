---
title: 搭建 Hexo-Next 那点儿事
date: 2019-09-04 15:26:18
tags: [Hexo,NexT,GitHub Pages,博客]
categories: 博客
description: 从毕业到现在也工作了6年多，之前积累东西一直在 Evernote 上，或者工作中随手写在公司 Wiki 上，最近LP封给我“大勤快”这个称号，于是一冲动决定整个像样点儿的独立博客。很多小伙伴应该也想搭建一个自己的博客，网上也有一堆详细的教程。在此，我详细的总结一下我搭建的基于 Hexo-Next 的博客步骤，主要分享一些我的修改经验，帮大家填填坑，更多个性化操作需要大家以后去摸索。（本文基于 MAC 介绍）

---

# 安装 Node.js

## 一、通过 Homebrew 安装 node（推荐）

### 1、安装 Homebrew  

* 可以通过 `brew -v` 来查看是否安装了 Homebrew，如果能正确显示 Homebrew 的版本号，说明 Homebrew 已安装

	![](/images/1.png)

* 如果没有安装 Homebrew，通过终端命令安装

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### 2、安装 node

* 在终端中输入命令

```
brew link node
brew uninstall node
brew install node
```

* 验证 node 是否安装成功
	
	下发命令 `node -v`、`npm -v`，能正确显示版本号即表示安装成功
	
	![](/images/2.png)
	
* 如果没有梯子的话，可以使用阿里的国内镜像进行加速。

```
npm config set registry https://registry.npm.taobao.org
```

>参考：**[淘宝 NPM 镜像](https://npm.taobao.org/)**

## 二、通过安装包来安装 node

* 下载安装包

	可以从 **[Node.js官方](https://nodejs.org/zh-cn/)** 下载 node 的安装包

	![](/images/3.png)

### 三、删除 node

如果之前已经安装过 node，再次安装会产生冲突，需要先删除

* 如果是通过 Homebrew 安装的，下发命令 `brew uninstall node` 即可；

* 执行命令行 

```
sudo rm -rf /usr/local/{bin/{node,npm},lib/node_modules/npm,lib/node,share/man/*/node.*}
```
	
# 配置 Git

MAC 上安装 Git 主要有两种方式

首先，查看电脑上是否安装 Git，终端输入：

```
git --version
```

安装过则会输出 Git 版本号，例如：

```
➜  ~ git --version
git version 2.20.1 (Apple Git-117)
```

## 1、通过 Homebrew 安装

终端输入：

```
brew install git
```
## 2、通过 Xcode 安装

直接从 AppStore 安装 Xcode，Xcode 集成了 Git，不过默认没有安装，你需要运行 Xcode，选择菜单 `'Xcode' -> 'Preferences'`，在弹出窗口中找到 `'Downloads'`，选择 `'Command Line Tools'`，点 `'Install'` 就可以完成安装了。

## 3、设置 username 和 email

```
git config --global user.name "xxx"
git config --global user.email "xxx@gmail.com"
```
	
## 4、通过终端命令创建 ssh key

```
ssh-keygen -t rsa -C "xxx@gmail.com"
```
	
复制 `.ssh/id_rsa.pub` 文件里的 key

```
cat .ssh/id_rsa.pub
```

## 5、登录 GitHub（默认你已经注册了 GitHub 账号），添加 ssh key，点击 Settings，如图

![](/images/4.png)

点击 `New SSH key`，如图

![](/images/5.png)

添加 `key`，如图

![](/images/6.png)

## 6、验证

```
ssh -T git@github.com 
```

终端输出结果：

```
➜  ~ ssh -T git@github.com
Hi CollinBin! You've successfully authenticated, but GitHub does not provide shell access.
```

项目建成后，点击 `Settings`，向下拉到最后有个 `GitHub Pages`，点击 `Choose a theme` 选择一个主题。再回到 `GitHub Pages`，点击 `http://xxx.com/xxx.github.io/`
 主页链接，就可以看到自己的网页。
 
## 7、新建 page 项目

新建一个项目，输入自己的项目名，后面一定要加 `.github.io`，`README` 初始化也要勾上，如图：

![](/images/7.png)

# 安装 Hexo

* 在合适的位置新建一个文件夹，用来存放博客文件。使用终端进入该目录下，执行命令：

```
npm i hexo-cli -g
```

安装完后输入 `hexo -v` 验证是否成功。

* 然后需要初始化我们的网站，执行 `hexo init` 初始化，输入 `npm install` 安装必备的组件。

* 这样本地的网站配置基本就弄好了，输入 `hexo g` 生成静态网页，然后输入 `hexo s` 打开本地服务器，如下：

![](/images/8.png)

>注意：由于我安装 NexT 主题，终端执行后显示有所区别。后面章节会讲解到 NexT 主题。

* 然后浏览器打开 **[http://localhost:4000/](http://localhost:4000/)** 就可以看到我们的博客了。
按 `Ctrl+C` 关闭本地服务器。

# 链接 GitHub 与本地

打开博客根目录下的 `_config.yml` 文件，这是博客的配置文件***（并非主题的配置文件）***，可以在这里面修改与博客有关的相关信息。

修改最后一行的配置，如下：

```
deploy:
  type: git
  repository: git@github.com:xxx/xxx.github.io.git
  branch: master
```

>注意：冒号后有空格，仓库直接复制之前在 GitHub 创建好的仓库地址，尽量使用 SSH key 的，不要使用 HTTPS 的，如下：

![](/images/9.png)

# 写文章、发布文章

* 安装一个扩展，终端定位到博客根目录下，执行命令：

```
npm i hexo-deployer-git
```

* 新建文章，执行命令：

```
hexo new post "article title"
```

打开 `./source/_posts` 的目录，可以发现下面多了一个 `.md` 文件，这就是刚才创建的文章文件。

编写完 Markdown 文件后，根目录下执行 `hexo g` 生成静态网页，然后输入 `hexo s` 可以预览本地效果，最后执行 `hexo d` 上传到 GitHub 上。或者可以直接执行 `hexo g -d` 生成并且上传。这时打开你的 `http://xxx.com/xxx.github.io/` 主页就能看到发布的文章了。

# 绑定域名

现在默认的域名还是 `xxx.github.io` ，如果想弄个专属的域名，首先你得先去买一个域名，xx云等等的都能买，看个人喜好了。

配置 DNS 推荐使用 DNSPOD 的服务，使用国外的 DNS 解析服务可能有被墙的风险。

首先在域名供应商处修改 DNS 服务器为：

```
f1g1ns1.dnspod.net.
f1g1ns2.dnspod.net.
```

然后到 DNSPOD 进行域名解析，向 DNS 配置中添加 3 条记录

```
@          A             192.30.252.153
@          A             192.30.252.154
www      CNAME           xxx.github.io.
```
>xxx 替换成自己的 GitHub 主页名称

对DNS的配置不是立即生效的，过10分钟可以再去访问的域名看看有没有配置成功。

然后打开 GitHub 博客项目，点击 `Settings`，拉到下面 `Custom domain` 处，填上你自己的域名，保存：

![](/images/10.png)

这时博客的项目根目录应该会出现一个名为 `CNAME` 的文件了。如果没有的话，打开本地博客 `/source` 目录，新建 `CNAME` 文件，注意没有后缀。然后在里面写上你的域名，不加 `www`，保存。最后运行 `hexo g -d` 上传到GitHub，这样就不会每次发布之后，GitHub Page 里的 `Custom domain` 都被重置掉了。

# Hexo 多终端发布

网站的部署其实就是生成静态文件，hexo 下所有生成的静态文件都会放在 `public/` 文件夹中，所有的 deploy 其实就是将 `public/` 文件夹中的内容上传到 Git 仓库。

## 换了电脑、想多终端发布博客怎么办？

在 `guthub.io` 的仓库下创建一个分支来管理

1. 克隆仓库到本地

```
git clone git@github.com:xxx.github.io.git
```

2. 删除文件夹里除了 `.git` 的其他所有文件

3. 创建一个叫 `hexo`（或者 `blog`，名字随意）的分支，并切换到这个分支

```
git checkout -b hexo
```

4. 把之前本地博客文件夹内的所有文件全部复制到 `hexo` 分支的本地目录下

5. 添加文件，推送到远程仓库

```
# 添加所有文件到暂存区
git add --all

# 提交
git commit -m ""

# 推送 hexo 分支的文件到 GitHub 仓库
git push --set-upstream origin hexo
```

6. 如果更新博客，先执行 Git 三部曲：

```
git add all
git commit -m ""
git push origin hexo
```

然后执行 `hexo g -d` 部署到远程

7. 今后如果换电脑或者多终端更新博客时，配置好基本环境后***（Node、npm、Git、Hexo）***，然后克隆 hexo 分支***（也可以在 GitHub 上将默认分支改为 hexo 分支）***，安装依赖 `npm install`。

```
git clone -b hexo git@github.com:xxx.github.io.git
```

## 综上所述 ##

* 新建博客 `hexo new post “你好，hexo”` ，然后去 `source/posts/` 编辑文章。

* 以后每次写完博客，通过 `hexo g -d` 发布博客，然后通过 git 三部曲 `git add .`；`git commit -m “注释”` ；`git push origin hexo` 推送到 GitHub 上即可。

# 安装 NexT 主题

NexT 主题的安装方式非常简单

* 下载主题，定位到 Hexo 根目录，终端执行：

```
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

* 启用主题

打开站点配置文件 `_config.yml` 找到 `theme` 字段，将其值更改为 `next`。

```
theme: next
```

执行 `hexo s`，看看效果。

* 如果想配置炫酷的网页效果，网上教程有很多，推荐一个比较完整、可读性比较强的文章：**[Hexo 博客美化配置](http://ylong.net.cn/hexo_conf.html)**

# 参考文章

**[超详细Hexo+Github博客搭建小白教程](https://zhuanlan.zhihu.com/p/35668237)**

**[NexT官网](https://theme-next.iissnan.com/)**

**[hexo多终端发布博客](http://houyimin.cn/2018/09/25/hexo%E5%A4%9A%E7%BB%88%E7%AB%AF%E5%8F%91%E5%B8%83%E5%8D%9A%E5%AE%A2/)**

