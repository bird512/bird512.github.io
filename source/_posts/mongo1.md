layout: post
title:  Mongo Docker 安装及用户配置
comment: true
tags: [技术, docker,mongo]
---


1. 下载mongo 3.4版本

```
docker pull mongo:3.4
```

1. 跑一个mongo容器。 —auth表示开启验证模式，mongo默认是不开启验证模式的

```
docker run -p 27017:27017 --name mongo1 -v $PWD/db:/data/db  -v  $PWD/configdb:/data/configdb -d mongo:3.4 --auth 
```

1. 进入mongo1容器进行初始用户设置

```
docker exec -it mongo1 mongo admin
```

1. 建立超级用户.

```
db.createUser({user:"bird521",pwd:"xxx",roles:["__system"]})
```

有了这个超级用户之后就可以这个用户登陆，还可以再建其它user. 一般项目中用一个readWriteAnyDatabase就够了。

```
mongo -u bird512 -p xxx -authenticationDatabase admin
use admi
db.createUser({user:"projectUser",pwd:"xxx",roles:[{role:"readWriteAnyDatabase",db:"admin"}]})
```





   ​

