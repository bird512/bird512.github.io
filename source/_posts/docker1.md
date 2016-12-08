layout: post
title:  在docker容器运行时更改restart策略 
comment: true
tags: [技术, docker]
---


前几天配了一个tomcat容器用来跑UAT，但是这个容器有时候会死掉(原因下次再分析)从而影响了测试人员的测试工作。当初docker run这个容器的时候没有考滤到这个问题没有加—restart=always。google后发现一个可以运行中的容器加restart策略：

1. 查看当前容器restart 策略

   ```
   docker inspect -f "{{ .HostConfig.RestartPolicy }}" <container_id>
   ```

   ​

2. 设置restart策略

   ```
   docker update --restart=always <container_id>
   ```

   ​