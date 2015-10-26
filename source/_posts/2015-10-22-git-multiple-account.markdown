---
layout: post
title: "在同一台电脑上配置Git多账号共存"
date: 2015-10-22 23:43:31 +0800
comments: true
categories: [git, ssh]
---


工作中使用git管理代码，自己也在github上提交代码，就需要2个git账号共存：
##分别生成ssh key
###第一个账号
```java
ssh-keygen -t rsa -b 4096 -C "111111@111.com"
Enter file in which to save the key (C:/Users/Administrator/.ssh/id_rsa): id_rsa_work
```
###第二个账号
```java
ssh-keygen -t rsa -b 4096 -C "222222@222.com"
Enter file in which to save the key (C:/Users/Administrator/.ssh/id_rsa): id_rsa_github
```

##新密钥添加到SSH agent中
```java
ssh-add ~/.ssh/id_rsa_work
ssh-add ~/.ssh/id_rsa_github
```
如果出现Could not open a connection to your authentication agent的错误，就试着用以下命令：
```java
ssh-agent bash
ssh-add ~/.ssh/id_rsa_work
ssh-add ~/.ssh/id_rsa_github
```

##修改config文件
在~/.ssh目录下找到config文件，如果没有就创建：
```java
cd ~/.ssh
touch config
```

然后修改config的内容如下：

该文件用于配置私钥对应的服务器
公司 user(111111@111.com)
```java
Host 111.com
 HostName 111.com
 User git
 IdentityFile C:/Users/Administrator/.ssh/id_rsa_work
```

第二个 user(222222@222.com)用于github
```java
Host github.com
 HostName github.com
 User git
 IdentityFile C:/Users/Administrator/.ssh/id_rsa_github
```

##把生成的~/.ssh/id_rsa_github.pub的内容加到github的账号key列表中


##测试
```java
ssh -T git@github.com
```
结果出现：
```
Hi YourGitHubAccount! You've successfully authenticated, but GitHub does not provide shell access.
```
说明可以成功连上github了