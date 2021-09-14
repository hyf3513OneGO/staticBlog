---
title: Centos安装mysql
date: 2021-09-12 21:50:44
tags:
---
# 引言
CentOS作为一款优秀的Linux发行版，受到了很多用户的青睐，但是新手往往会发现一个奇怪的地方，就是CentOS yum指令无法直接安装 MySQL 这一点让用惯了yum一键安装软件的用户着实感到很不方便，首先先来介绍一下为什么CentOS不能默认安装MySQL

> MariaDB数据库管理系统是MySQL的一个分支，主要由开源社区在维护，采用GPL授权许可
。开发这个分支的原因之一是：甲骨文公司收购了MySQL后，有将MySQL闭源的潜在风险，
因此社区采用分支的方式来避开这个风险。

由于MySQL被甲骨文公司收购，为了防止闭源，因此Centos默认将mysql换为了mariaDB
那我们要安装mysql的话就需要额外下载mysql的 rpm包
## 解决方案

### 第一步
先检查是否安装了Mysql
` rpm -qa | grep mysql1`

![](https://img.maocdn.cn/img/2020/12/07/2012071300_1.png)

如果没有回显则说明之前没有安装MySQL可以继续下一步，否则需要先卸载mysql
卸载命令
`yum remove mysql`

### 第二步

下载mysql的repo源
`wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm`

[![2012071301_2.png](https://img.maocdn.cn/img/2020/12/07/2012071301_2.png)](https://img.wang/image/sou-gou-jie-tu-20nian-12yue-07ri-1301-2.foEv7)

repo源的含义
>  repo文件是Fedora中yum源（软件仓库）的配置文件，通常一个repo文件定义了一个或者多个软件仓库的细节内容，例如我们将从哪里下载需要安装或者升级的软件包，repo文件中的设置内容将被yum读取和应用！ repo文件是Fedora中yum源（软件仓库）的配置文件，通常一个repo文件定义了一个或者多个软件仓库的细节内容，例如我们将从哪里下载需要安装或者升级的软件包，repo文件中的设置内容将被yum读取和应用！

安装mysql-community-release-el7-5.noarch.rpm包
`rpm -ivh mysql-community-release-el7-5.noarch.rpm`

[![2012071304_4.png](https://img.maocdn.cn/img/2020/12/07/2012071304_4.png)](https://img.wang/image/sou-gou-jie-tu-20nian-12yue-07ri-1304-4.fL9VV)
到这一步，就相当于完成了引入mysql到yum的repo源内
之后就是我们熟悉的yum安装软件的流程

### 第三步
yum安装mysql
`yum install mysql`


