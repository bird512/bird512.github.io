layout: post
title:  跑了tomcat的docker容器诡异137退出分析
comment: true
tags: [技术, docker]
---

继上篇，最近发生的一件诡异的事，跑在一台测试服务器上的一个tomcat的docker容器隔几天都会ExitCode:137退出。最近几天一直很平稳没有一个容器退出过，直到今天早上10点左右这台机上所有在tomcat容器都一起停掉了（除了两个加了restart=always已经自启了）。分析：



1. 分析ExitCode。 137 ＝ 128＋9. 表明这是被kill -9结果掉的。
   http://tldp.org/LDP/abs/html/exitcodes.html
   ​

2. inspect这个容器查看具体信息。OOMKilled:false 表明它不是因为out of memory被kill的。难道是被其它什么进程kill的？

   ```
   docker inspect <container_id>
   ```

   输出：

   ```
   "State": {
               "Status": "exited",
               "Running": false,
               "Paused": false,
               "Restarting": false,
               "OOMKilled": false,
               "Dead": false,
               "Pid": 0,
               "ExitCode": 137,
               "Error": "",
               "StartedAt": "2016-12-12T02:06:08.243092091Z",
               "FinishedAt": "2016-12-13T01:57:59.927017549Z"
           },
   ```

   ​

   ​

3. 查看容器log。－－没发现出错信息

   ```
   docker logs <container_id>
   ```

   ​

4. 查看docker deamon log。－－没发现出错信息

   ```
   journalctl -u docker.service
   ```

   不同系统版本有不同查看命令：

   https://www.loggly.com/blog/what-does-the-docker-daemon-log-contain/ 



分析到现在为止貌似已经没辙了，如果还有其它方法请下面留言。



真相：

最后我只能想到一个可能性了：我公司有一个同事因个人原因在云南远程办公，有可能是被他手动kill掉的。 确认后果然是他干的（粗暴的把所有java进程都kill -9了:(）