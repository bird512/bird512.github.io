layout: post
title:  Docker 容器内time zone与主机的time zone不一致的解决方法
comment: true
tags: [技术, docker]
---



暂时找到了两种方法，这里备忘一下。

1. 以volume的形式把主机的/etc/localtime共享到容器里，亲测可行。

   ```
   docker run -v /etc/localtime:/etc/localtime xxx
   ```

   ​

2. 在DockerFile里把time_zone echo到镜像里，还没亲自测试过 。

   ```
   RUN echo "$TIME_ZONE" > /etc/time
   ```

   ​

