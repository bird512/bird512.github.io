layout: post
title: GitHub Pages + Hexo搭建博客
comment: true
tags: [技术, 静态独立博客, Hexo, GitHub Page, Git]
date: 2016-9-1 22:40:50
---

------
# 一、 前言
这是一篇使用GitHub Pages和Hexo搭建免费独立博客的总结。


如果是小小白，可以先花时间去了解下：
* [Git](http://git-scm.com/book/zh/v2)
* [GitHub](https://github.com/)
* [GitHub Pages](https://pages.github.com/)
* [Hexo](https://hexo.io/zh-cn/)
* [Markdown](http://www.appinn.com/markdown/#autoescape)
<!-- more -->

# 二、 必要配置
## 2.1 GitHub Pages 仓库
### 2.1.1 创建对应仓库
在自己的GitHub账号下创建一个新的仓库，命名为username.github.io（username是你的账号名)。

在这里，要知道，GitHub Pages有两种类型：User/Organization Pages 和 Project Pages，而我所使用的是User Pages。

简单来说，User Pages 与 Project Pages的区别是：
1. User Pages 是用来展示用户的，而 Project Pages 是用来展示项目的。
2. 用于存放 User Pages 的仓库必须使用username.github.io的命名规则，而 Project Pages 则没有特殊的要求。
3. User Pages 将使用仓库的 master 分支，而 Project Pages 将使用 gh-pages 分支。
4. User Pages 通过 http(s)://username.github.io  进行访问，而 Projects Pages通过 http(s)://username.github.io/projectname 进行访问。

### 2.1.2 相关资料
* [GitHub Pages Basics / User, Organization, and Project Pages](https://help.github.com/articles/user-organization-and-project-pages/)

## 2.4 Hexo
### 2.4.1 安装Hexo
安装Hexo相当简单。在安装之前，必须检查电脑中是否已经安装下列应用程序：
* [Node.js](http://nodejs.org/)
* [Git](http://git-scm.com/)
如果您的电脑中已经安装上述必备程序，那么恭喜您！接下来只需要使用 npm 即可完成 Hexo 的安装。
```bash
$ npm install -g hexo-cli
```

### 2.4.2 使用Hexo建站
安装完后，在你喜欢的文件夹内（例如D：\Hexo），点击鼠标右键选择Git bash，输入以下指令：

```bash
$ hexo init
```
该命令会在目标文件夹内建立网站所需要的所有文件。接下来是安装依赖包：

```bash
$ npm install
```

这样，我们就已经搭建起本地的Hexo博客了。可以先执行以下命令（在对应文件夹下），然后再浏览器输入localhost:4000查看。

```bash
$ hexo g
$ hexo server
```

这个博客只是本地的，别人是浏览不了的，之后需要部署到GitHub上。

### 2.4.3 相关资料
* [Hexo 官方文档](https://hexo.io/zh-cn/docs/)

## 三、一般的搭建方法
在上面，我们已经配置好了所需的所有东西，也成功地搭建了一个本地Hexo博客。现在，需要使用GitHub Pages搭建一个别人能够访问的Hexo博客了。

### 3.1 使用默认theme
我们继续使用上面的文件夹D:\Hexo（也可以新建一个文件夹重新生成），然后编辑该文件夹下的_config.yml。

默认生成的_config.yml：
```bash
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type:
```

修改后的_config.yml：
```bash
deploy:
  type: git
  repo: 对应仓库的SSH地址（可以在GitHub对应的仓库中复制）
  branch: 分支（User Pages为master，Project Pages为gh-pages）
```

为了能够使Hexo部署到GitHub上，需要安装一个插件：

```bash
$ npm install hexo-deployer-git --save
```

然后，执行下列指令即可完成部署：

```bash
$ hexo g -d
```

之后，可以通过在浏览器键入：username.github.io进行浏览，开心吧~

### 3.2 其他theme
如果想要使用其他主题，可以使用git clone将别人的主题拷贝到D:\Hexo\themes下，然后将_config.yml中的theme: landscape改为对应的主题名字。

详细步骤可以参考网上的指南。

## 四、 优化部署与管理
### 4.1 概述
Hexo部署到GitHub上的文件，是.md（你的博文）转化之后的.html（静态网页）。因此，当你重装电脑或者想在不同电脑上修改博客时，就不可能了（除非你自己写html o(^▽^)o ）。

其实，Hexo生成的网站文件中有.gitignore文件，因此它的本意也是想我们将Hexo生成的网站文件存放到GitHub上进行管理的（而不是用U盘或者云备份啦(╬▔皿▔)凸）。这样，不仅解决了上述的问题，还可以通过git的版本控制追踪你的博文的修改过程，是极赞的。

但是，如果每一个GitHub Pages都需要创建一个额外的仓库来存放Hexo网站文件，我感觉很麻烦（10个项目需要20个仓库(ˉ▽ˉ；)...）。

所以，我利用了分支！！！

简单地说，每个想建立GitHub Pages的仓库，起码有两个分支，一个用来存放Hexo网站的文件，一个用来发布网站。

下面以我的博客作为例子详细地讲述。

### 4.2 我的博客搭建流程
1. 因为hexo init会覆盖当前文件夹的git信息，所以先在某个文件夹下执行hexo init copyme，生成copyme文件夹备用;
2. 创建仓库，bird512.github.io；
3. 创建两个分支：master 与 hexo；
4. 设置hexo为默认分支（因为我们只需要手动管理这个分支上的Hexo网站文件）；
5. 使用git clone git@github.com:bird512/bird512.github.io.git拷贝仓库；
6. 在本地bird512.github.io文件夹，把第一步生成的copyme文件夹下的所有东西都拷过来，通过Git bash依次执行npm install 和 npm install hexo-deployer-git（此时当前分支应显示为hexo）;
7. 修改_config.yml中的deploy参数:
```
	url: https://bird512.github.io/
	deploy:
	  type: git
	  repo: https://github.com/bird512/bird512.github.io.git
	  branch: master
```
8. 依次执行git add .、git commit -m "..."、git push origin hexo提交网站相关的文件；
9. 执行hexo g -d生成网站并部署到GitHub上。

这样一来，在GitHub上的bird512.github.io仓库就有两个分支，一个hexo分支用来存放网站的原始文件，一个master分支用来存放生成的静态网页。

### 4.3 我的博客管理流程
#### 4.3.1 日常修改
在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理：

1. 依次执行git add .、git commit -m "..."、git push origin hexo指令将改动推送到GitHub（此时当前分支应为hexo）；
2. 然后才执行hexo g -d发布网站到master分支上。


