layout: post
title:  Docker 容器内time zone与主机的time zone不一致的解决方法
comment: true
tags: [技术, docker]
---



暂时找到了两种方法，这里备忘一下, 两个一起用确保

1. 以volume的形式把主机的/etc/localtime共享到容器里，亲测可行。

   ```
   docker run -v /etc/localtime:/etc/localtime xxx
   ```

   ​

2. 在DockerFile里把time_zone echo到镜像里. 如果服务器只在国内的就按下面第一种直接写死好了

   ```
   RUN echo "Asia/shanghai" > /etc/time
   ```
   or
   ```
   RUN echo "$TIME_ZONE" > /etc/time
   ```

   ​

