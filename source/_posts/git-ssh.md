layout: post
title:  Git服务器SSH配置
comment: true
tags: [技术, git]



## 用户端

### 设置本地git用户配置

```
$ git config --global user.name "username"
$ git config --global user.email "user@email.com"
```

### 创建SSH Key，私钥和公钥

把邮件地址换成自己的邮件地址，然后一路回车，使用默认值即可，无需设置密码。

```
$ ssh-keygen -t rsa -C "user@email.com"
```

### 上传公钥到服务器(这步应该在服务器新建了用户git之后)

```
$ cd ~/.ssh
$ scp -r ~/.ssh/id_rsa.pub git@192.168.1.104:~/ 
```

 

## 服务器端

### 创建一个git用户，用来运行git服务

```
$ sudo adduser git
$ su git
$ cd ~
$ mkdir .ssh
```

### ssh服务端配置

禁用git用户shell登录

出于安全考虑，前面创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：

```
git:x:1001:1001:,,,:/home/git:/bin/bash
```

改为：

```
git:x:1001:1001:,,,:/home/git:/usr/local/bin/git-shell
```



### 把用户上传的公钥添加到authorized_keys

```
$ cd ~
$ mkdir .ssh 
$ cd .ssh
$ touch authorized_keys
$ cat ~/id_rsa.pub >> ~/.ssh/authorized_keys
$ rm ~/id_rsa.pub
```

### 最后记得加上权限

```
$ chmod 600 ~/.ssh/authorized_keys
$ chmod 700 ~/.ssh
```

### 授权登陆

```
$ sudo vim /etc/ssh/sshd_config

RSAAuthentication yes 
PubkeyAuthentication yes
AuthorizedKeysFile      /home/git/.ssh/authorized_keys
```



