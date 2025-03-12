---
abbrlink: ''
categories:
- - Git
date: '2024-08-12T08:00:40.343779+08:00'
tags:
- Git
title: Git GUI的使用教程
updated: '2025-03-12T15:06:37.314+08:00'
---
## 概述：

git本身提供Git Bash和Git Gui两种模式，我更推荐使用Git Bash方式对Git进行操作， 但是对于大部分只是想简单地使用一下Git进行版本控制和团队开发的朋友而言，仍然更加钟情于可视化界面。

所以本文对Git官方自带的可视化工具Git Gui的使用进行介绍，尽量做到图文并茂，让所有初探Git的朋友都能够快速入门，使用Git进行项目开发。

## 作用:

1、从服务器上克隆完整的Git仓库（包括代码和版本信息）到单机上。

2、在自己的机器上根据不同的开发目的，创建分支，修改代码。

3、在单机上自己创建的分支上提交代码。

4、在单机上合并分支。

5、把服务器上最新版的代码fetch下来，然后跟自己的主分支合并。

6.版本控制，可回滚

## **一、权限校验-SSH Key：**

**1.复制客户端key：**

      启动GUI，菜单-帮助，【Step1-创建密钥】Generate SSH KEY

![](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121505055.png)

 **2.服务端添加可以**

     在github的ssh key中添加可以key即可

![](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121505056.png)

**PS**：到这里提交就不用输密码了

## 二、克隆远程项目到本地：

![](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121505057.png)

## 三、上传到远程：

#包含缓存改动，提交，上传

![](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121505058.png)

 PS：如果需要添加描述信息，就需要在操作前填写(提交时必须填写描述，其他操作可以不填写)

## **四、获取远程代码：**

比如你在公司做好的东东，今夜难眠十分亢奋，回家准备继续搬砖，那咱们就在家里的电脑上，同上进行好各种安装配置账号，先把公司做好的东东嫩下来（不过公司是内网不可以，但是假如是Github上是可以的）

![](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121505059.png)

![](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121505060.png)

### 相关连接：

[https://www.runoob.com/w3cnote/git-gui-window.html](https://www.runoob.com/w3cnote/git-gui-window.html) .....................................................................................git教程

[https://blog.csdn.net/qq\_46514118/article/details/122529831](https://blog.csdn.net/qq_46514118/article/details/122529831) .........................................................................[ssh免密登录配置](https://blog.csdn.net/xingxiupaioxue/article/details/125066433)

[https://blog.csdn.net/mansir123/article/details/121692125](https://blog.csdn.net/mansir123/article/details/121692125) .............................................................................git Gui汉化

[https://blog.csdn.net/weixin\_40283460/article/details/121968351](https://blog.csdn.net/weixin_40283460/article/details/121968351) ..................................................................**git stash暂存命令**

