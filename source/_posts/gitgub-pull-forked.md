title:  github下fork后同步源的新更新
comment: true
tags: [技术, git]
---

上周因项目中用的一个react lib [react-data-grid](https://github.com/adazzle/react-data-grid)，想要在这上面新增一个功能，于是上去fork，修改再pull了一个request，已经被原作者merge进去了。 今天又发现了一个问题想上去改改，发现我原先fork的branch已经落后于源branch了

![img](http://odshs6yxx.bkt.clouddn.com/11.png)

[这里](https://segmentfault.com/q/1010000002590371)已经有一个比较完整的方案了。我这边备忘一下。

1. 首先要先确定一下是否建立了主repo的远程源：
```
git remote -v
```

2. 如果里面只能看到你自己的两个源(fetch 和 push)，那就需要添加主repo的源.
```
git remote add upstream URL
git remote -v
```
然后你就能看到upstream了。

3. 如果想与主repo合并：
```
git fetch upstream
git merge upstream/master
git push origin master
```