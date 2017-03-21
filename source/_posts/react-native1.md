layout: post
title: could not install *smartsocket* listener Address already in use错误解决
comment: true
tags: [技术, reacct native]
----


使用Genymotion模拟器调试react-native时如果碰到error: could not install *smartsocket* listener: Address already in use错误，这个错误应该就是你的Genymotion使用的Android SDK 跟你本机上的Android SDK路径不一致。解决方法:

1. 命令行输入 android，打开Android SDK Manager，找到你当前的SDK Path。eg: /usr/local/Cellar/android-sdk/24.4.1_1

2. 设置你的Genmontion的Android SDK路径为1里找到的路径。

   Genymontion->Settings->ADB->Use custom Android SDK tools

```
error: could not install *smartsocket* listener: Address already in use
ADB server didn't ACK
* failed to start daemon *
error: cannot connect to daemon

```
