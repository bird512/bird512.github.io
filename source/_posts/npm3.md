layout: post
title:  npm3体验
comment: true
tags: [技术, react, npm]
---
# 安装

由于NODE的长期支持版本(LTS)为4.5.0,这个版本附带的npm版本2.X，所以目前大家默认安装的npm都是2.X.

查看当前npm版本

```
npm -v
```

安装npm3作为默认npm，将会替换当前的npm2，运行npm -v将会返回3.X.X。我用这方法全局安装并使用npm3已有几个月了，没有发现任何bug(在这期间同组的其它成员还都在使用npm2)。
```
npm install npm@3 -g

```
回退到npm2，运行npm -v将会返回2.X.X。

```
npm install npm@2 -g
```

别名安装npm3. 保持当前npm版本不变，但是可以使用npm3 install来以npm3的方法install modules. 如果有同学不想冒险直接安装npm3，可以使用这个方法。

```
npm install npm3 -g
```

# npm3新功能

npm3最大的改动就是把原来的树形包依赖改成扁平结构。之前树形结构在windows用户手动拷贝 node_modules时经常会出现文件路径过长无法拷贝的错误，到npm3之后就不会再有这个问题了。还有一个好处是扁平结构下大大减少了文件数量，在树形结构下因兄弟包之间是无法知道对方是否已经加载了哪个子包，所当有多个包依赖于同一个子module的时候会产生重复加载。
npm3依赖包规则在官网上可以找到 https://docs.npmjs.com/how-npm-works/npm3  我这里做个总结：

1. 当用户使用npm install xxx -save导入xxx包的时候，如果xxx下面有子module，子module首先会试着拷到顶层node_modules下面。
	- 如果顶层node_modules下已经存在这个同名子module并且版本号相同，则跳过。这样在build的时候xxx会找到顶层node_modules下的子module.
	- 如果顶层node_modules下已存存在这个同名子module但版本号不同，则把这个子module保留xxx的node_modules下面。


2. 当两个或以上的包依赖于同一个子module的不同版本时候，哪个拷到顶层node_module哪个保留在包自身的node_modules下面，这个规则是跟据包的加载顺序，哪个先加载就哪个先抢占顶层目录。
	- 如果包是用npm install xxx -save加载的，则后install的为后。
	- 如果是空node_modules文件夹，跟据package.json用npm install一次性加载，则跟据包名的字母排序：例如包名b开头的排在a开问的包后面。

