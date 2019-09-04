---
title: 搭建 Hexo-Next 那点儿事
date: 2019-09-04 15:26:18
tags: [Hexo,NexT]
categories: Hexo
description: 从毕业到现在也工作了6年多，之前积累东西一直在 Evernote 上，或者工作中随手写在公司 Wiki 上，最近LP封给我“大勤快”这个称号，于是一冲动决定整个像样点儿的独立博客。很多小伙伴应该也想搭建一个自己的博客，网上也有一堆详细的教程。在此，我详细的总结一下我搭建的基于 Hexo-Next 的博客步骤，主要分享一些我的修改经验，帮大家填填坑，更多个性化操作需要大家以后去摸索。（本文基于 MAC 介绍）

---

# 安装 Node.js

## 一、通过 homebrew 安装 node（推荐）

### 1、安装 homebrew  

* 可以通过 `brew -v` 来查看是否安装了 homebrew，如果能正确显示 homebrew 的版本号，说明 homebrew 已安装

	![](/images/1.png)

* 如果没有安装 homebrew，通过终端命令安装

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
	参考：[淘宝 NPM 镜像](https://npm.taobao.org/)
	
## 二、通过安装包来安装 node

* 下载安装包

	可以从 [Node.js官方](https://nodejs.org/zh-cn/) 下载 node 的安装包

	![](/images/3.png)

### 三、删除 node

>如果之前已经安装过 node，再次安装会产生冲突，需要先删除

* 如果是通过 homebrew 安装的，下发命令 `brew uninstall node` 即可；

* 执行命令行 

```
sudo rm -rf /usr/local/{bin/{node,npm},lib/node_modules/npm,lib/node,share/man/*/node.*}
```
	
# 安装 Git

>MAC 上安装 Git 主要有两种方式

首先，查看电脑上是否安装 Git，终端输入：

```
git
```

安装过则会输出：

```
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone      Clone a repository into a new directory
   init       Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add        Add file contents to the index
   mv         Move or rename a file, a directory, or a symlink
   reset      Reset current HEAD to the specified state
   rm         Remove files from the working tree and from the index

examine the history and state (see also: git help revisions)
   bisect     Use binary search to find the commit that introduced a bug
   grep       Print lines matching a pattern
   log        Show commit logs
   show       Show various types of objects
   status     Show the working tree status

grow, mark and tweak your common history
   branch     List, create, or delete branches
   checkout    Switch branches or restore working tree files
   commit     Record changes to the repository
   diff       Show changes between commits, commit and working tree, etc
   merge      Join two or more development histories together
   rebase     Reapply commits on top of another base tip
   tag        Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch      Download objects and refs from another repository
   pull       Fetch from and integrate with another repository or a local branch
   push       Update remote refs along with associated objects

'git help -a' and 'git help -g' list available subcommands and some
concept guides. See 'git help <command>' or 'git help <concept>'
to read about a specific subcommand or concept.
```

## 1、通过 homebrew 安装

终端输入：

```
brew install git
```
## 2、通过 Xcode 安装

直接从 AppStore 安装 Xcode，Xcode 集成了 Git，不过默认没有安装，你需要运行 Xcode，选择菜单 "Xcode"->"Preferences"，在弹出窗口中找到 "Downloads"，选择 "Command Line Tools"，点 "Install" 就可以完成安装了。

# 配置 Git

## 1、设置 username 和 email

```
git config --global user.name "***"
git config --global user.email "***@gmail.com"
```
	
## 2、通过终端命令创建 ssh key

```
ssh-keygen -t rsa -C "***@gmail.com"
```
	
复制 .ssh/id_rsa.pub 文件里的 key

```
cat .ssh/id_rsa.pub
```

## 3、登录 GitHub（默认你已经注册了 GitHub 账号），添加 ssh key，点击 Settings，如图

![](/images/4.png)

点击New SSH key，如图

![](/images/5.png)

添加key，如图

![](/images/6.png)

## 4、验证

```
ssh -T git@github.com 
```

终端输出结果：

```
➜  ~ ssh -T git@github.com
Hi CollinBin! You've successfully authenticated, but GitHub does not provide shell access.
```

## 5、新建 page 项目

新建一个项目，输入自己的项目名，后面一定要加 `.github.io`，README 初始化也要勾上，如图：

![](/images/7.png)

# 安装 Hexo



