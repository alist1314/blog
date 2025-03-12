---
abbrlink: ''
categories:
- - 技术
date: 2024-03-7
tags:
- Git
- 软件
- 技术
title: Git - 配置公钥私钥
updated: '2025-03-12T15:19:55.306+08:00'
---
- [before](https://www.cnblogs.com/bubu99/articles/17924054.html#before)
- [方案一，明文添加用户名和密码(不推荐)](https://www.cnblogs.com/bubu99/articles/17924054.html#%E6%96%B9%E6%A1%88%E4%B8%80%E6%98%8E%E6%96%87%E6%B7%BB%E5%8A%A0%E7%94%A8%E6%88%B7%E5%90%8D%E5%92%8C%E5%AF%86%E7%A0%81%E4%B8%8D%E6%8E%A8%E8%8D%90)
- [方案2：基于凭证助手的方式](https://www.cnblogs.com/bubu99/articles/17924054.html#%E6%96%B9%E6%A1%882%E5%9F%BA%E4%BA%8E%E5%87%AD%E8%AF%81%E5%8A%A9%E6%89%8B%E7%9A%84%E6%96%B9%E5%BC%8F)
- [方案三，基于公钥私钥的形式](https://www.cnblogs.com/bubu99/articles/17924054.html#%E6%96%B9%E6%A1%88%E4%B8%89%E5%9F%BA%E4%BA%8E%E5%85%AC%E9%92%A5%E7%A7%81%E9%92%A5%E7%9A%84%E5%BD%A2%E5%BC%8F)
- [返回目录](https://www.cnblogs.com/Neeo/p/10864123.html#more)

## before

默认的，本地每次推送代码到远程仓库，都要输入用户名和密码，想要一劳永逸的解决这个问题，可以有以下几种方式：

- 方案1. 在remote add时，在远程仓库的url中手动添加用户名和密码。不推荐这种方式，因为在本地暴露了你的用户名和密码信息。
- 方案2. 基于凭证助手的方式，也就是输入一次用户名密码，就会保存到本地，后续就无需再次输入密码。觉得第三种麻烦，可以用这种。
- 方案3. 以公钥私钥的形式来完成。配置起来相对麻烦一些，但也没那么难。

接下来演示下三种方案。
额外的，我由于在本地反复的演示三种配置，所以，我这里再补充个清除本地认证的命令：

```bash
# git 清理本地账户信息，下面三条命令分别执行，如果有哪条报错了，也不用管它
git config —system —unset credential.helper 
git config --global --unset credential.helper 
git config --local --unset credential.helper
```

如果需要自取。

注意，后续的所有命令，都是基于Windows + git bash终端(注意，不是系统的cmd终端) + 码云仓库的环境下执行的。

## 方案一，明文添加用户名和密码(不推荐)

> windows + 码云

这种方式上面也说了，不推荐因为在本地暴露了你的用户名和密码信息，虽然它暴露在了你自己的本地电脑上了。
来看怎么配置：

```bash
# 如果你之前已经添加了远程仓库地址，那么你应该首先把之前的仓库地址干掉
git remote remove origin 
# 然后，改造下原来的仓库地址，注意是HTTPS的仓库地址 
# 原来的 https://gitee.com/wangzhangkai/ppqb.git 
# 改造后的，也就是添加了用户名和密码的 
https://user:pwd@gitee.com/wangzhangkai/ppqb.git user:替换为你的用户名，这个用户名可以是这个连接中的https://gitee.com/wangzhangkai/ppqb.git中的wangzhangkai，也可以是你在码云中认证的手机号 pwd: 替换为你的密码 
# 然后再执行添加你改造后的地址 
git remote add origin https://user:pwd@gitee.com/wangzhangkai/ppqb.git
```

完事你后续的push操作就不需要再认证了。

## 方案2：基于凭证助手的方式

> windows + 码云

这种方式比较简单，首先在你的git bash中执行：

```bash
# 如果你之前已经添加了远程仓库地址，那么你下一步操作就不用动了，没有添加过的话，就执行下，注意是HTTPS的仓库地址 
git remote add origin https://gitee.com/wangzhangkai/ppqb.git 
# 然后，执行下面的命令，在本地打开凭证助手，后续push时，
# 只需要输入一次用户名和密码，也有可能没有输入用户名密码提示，大概因为你之前输入的用户名密码已经缓存过了， 
# 所以提示输入就输入一次，不提示就能直接推到远程仓库，后续也不需要在输入用户名密码就ok了，反正在本地已经保存了。 
git config --global credential.helper store
```

基本上这个命令适合第一次push代码前执行一次，然后整个项目只需要输入一次用户名密码就行了，这样本地也就保存了。

## 方案三，基于公钥私钥的形式

> windows + 码云

我这里还是以本地git结合码云来演示。这种配置方式的原理就是：

- 在你的本地电脑上通过工具和命令来生成公钥和私钥文件。
  - Windows下，这个工具由git bash终端内置了，注意是git bash终端，不是Windows系统的cmd终端。
  - Mac系统终端中已经内置好了。
- 私钥保存在本地电脑不动它，然后将公钥文件中的公钥拷贝到远程仓库的相关配置中(gitee/github/gitlab，不同的软件配置位置不太一样，但原理是一样的)。然后将公钥拷贝到代码仓库平台。
- 后续push代码都是以ssl进行连接的，所以，配置时，你从代码仓库平台拷贝链接时，注意拷贝SSL类型的，而不是HTTPS类型的，这点尤为重要，别拷贝错了。

首先，要在本地生成公钥私钥，并且拿到公钥，私钥生成在哪就让它在哪呆着，我们不管。
**windows**

```bash
# 注意，Windows中必须打开 git bash 终端才行，直接打开Windows的cmd或者powershell不行的，这点我遇到好多学生犯错了
# git bash中生成密钥的命令如下
ssh-keygen -t rsa

# 示例，输入完了命令，然后一路回车。
12061@zhangkai MINGW64 /d/mydata/ppqb (master)
$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/12061/.ssh/id_rsa):
Created directory '/c/Users/12061/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/12061/.ssh/id_rsa
Your public key has been saved in /c/Users/12061/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:1s4nsBui3a+p09tIgb15iTqo6oerM5GCL12kRKeNkfU 12061@zhangkai
The key's randomart image is:
+---[RSA 3072]----+
|   o.            |
|  + ..           |
| . *  E          |
|  + o  o .       |
|.o o  . S .      |
|= . .  . X .     |
|.+.. ...B * .    |
|+.o..oo=.B o     |
|=B+.. ++B+o      |
+----[SHA256]-----+


# 上面命令生成的公钥和私钥的位置
c/Users/12061/.ssh/id_rsa      # 私钥，放着别动
c/Users/12061/.ssh/id_rsa.pub  # 公钥，你可以在本地通过文本文件打开它获取公钥内容，也可以通过命令获取
# 这里演示通过git bash 的cat命令获取公钥，~代指了c/Users/12061路径，结果我就不展示了
cat ~/.ssh/id_rsa.pub
```

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/1168165-20230524113942678-1339203127.png)

**Mac系统**

```bash
# Mac在终端中直接执行命令，然后一路回车即可 # 生成秘钥 ssh-keyen -t rsa # 查看公钥 ~代表的是你的用户路径 
cat ~/.ssh/id_rsa.pub
```

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/1168165-20230524101752440-1505196840.png)

在本地拿到了公钥，接下来就需要将公钥拷贝到远程仓库的相关配置中了，我这里截图以码云为例。

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/1168165-20230604105456666-1547237928.png)

再然后，就是将remote add一个ssh的连接了。
拷贝ssh链接。

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/1168165-20230604105513054-1151157163.png)

然后git bash中执行：

```bash
# 如果你之前已经添加了远程仓库地址，那么你应该首先把之前的仓库地址干掉 
git remote remove origin 
# 然后再重新配置 
git remote add origin git@gitee.com:wangzhangkai/ppqb.git
```

完事你提交代码就不需要输入用户名和密码了。


