---
abbrlink: ''
categories: []
date: '2024-08-10T07:46:30.106566+08:00'
tags: []
title: Github Action自动化部署Hexo博客和Qexo管理后台
updated: '2025-03-12T15:14:57.636+08:00'
---
## 一、Hexo博客搭建

> **Hexo是个快速、简洁且高效的博客框架，它是一款基于Node.js的静态博客生成程序，作者是中国台湾tommy351。它的安装运行等甚至生成文章页面 生成目录，网站配置都是在爱代码模式下进行的。还有就是要学会使用Hexo，就得学会使用Git，并且对Git常用基础命令要有所了解，还有就是需要安装Node.js，这个软件是Hexo本地搭建必不可少的工具，值得一提的是Hexo博客可以部署到GitHub、Gitee、GitLab、Coding、七牛，都是完全免费的，可以让你实现免服务器，免域名搭建一个完整的博客。**

### 1、安装git

**Git是目前世界上最先进的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理。也就是用来管理你的hexo博客文章，上传到GitHub的工具。**

**Windows：**下载并安装 git：[https://git-scm.com/download/win](https://isedu.top/index.php/goto?url=aHR0cHM6Ly9naXQtc2NtLmNvbS9kb3dubG9hZC93aW4=&cid=144)
**对于中国大陆地区用户，可以前往 [淘宝 Git for Windows 镜像](https://isedu.top/index.php/goto?url=aHR0cHM6Ly9ucG0udGFvYmFvLm9yZy9taXJyb3JzL2dpdC1mb3Itd2luZG93cy8=&cid=144) 下载 git 安装包。**

**Linux (Ubuntu, Debian)：`sudo apt-get install git-core`**

**Linux (Fedora, Red Hat, CentOS)：`sudo yum install git-core`**

### 2、安装nodejs

**Hexo是基于nodeJS编写的，所以需要安装一下nodeJs和里面的npm工具。**

**windows：打开nodejs：[https://nodejs.org/en/download/](https://isedu.top/index.php/goto?url=aHR0cHM6Ly9ub2RlanMub3JnL2VuL2Rvd25sb2FkLw==&cid=144) 选择LTS版本。**

**linux：安装完后，打开命令行**

```powershell
sudo apt-get install nodejs sudo apt-get install npm
```

**然后检查一下有没有安装成功**

```powershell
node -v npm -v
```

### 3、安装Hexo

**先创建一个文件夹blog，在这个文件夹下的空白地方，右键git bash打开【Windows】**，**【Linux】如下**

```powershell
mkdir blog cd blog
```

**然后安装hexo**

```powershell
npm install -g hexo-cli
```

**然后初始化hexo**，**这个hexoblog可以随便填**

```powershell
hexo init hexoblog
```

**用cd进入hexoblig里（或者直接打开这个文件夹，在空白地方右键 \*_git bash打开\*_** ）

```powershell
cd hexoblog
```

**这个时候hexoblog文件夹里有指定文件夹目录下有：**

```powershell
node_modules: 依赖包 public：存放生成的页面 scaffolds：生成文章的一些模板 source：用来存放你的文章 themes：主题 _config.yml: 博客的配置文件 db.json：source解析所得到的 package.json：项目所需模块项目的配置信息
```

**然后本地运行测试一下**

```powershell
hexo g hexo s
```

\*\*hexo generate 顾名思义，生成静态文章，可以用 hexo g缩写
hexo server 顾名思义，启动服务 本地运行，可以用 hexo s缩写\*\*

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511877.png "img")

**在浏览器输入 localhost:4000 就可以看到你生成的博客了。**

**使用ctrl+c可以把服务关掉**

### 4.在GitHub创建一个放博客文件的仓库

**没有账号的注册一个，登录后，点击右上角New repository**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511880.png "img")

**创建一个和你用户名相同的仓库，后面加.github.io，只有这样，将来要部署到GitHub page的时候，才会被识别，也就是xxx.github.io，其中xxx就是你注册GitHub的用户名。我这里是已经建过了。点击create repository。**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511881.png "img")

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511881.png "img")

### 5\. 生成SSH添加到GitHub

**在博客hexoblog根目录 右键点击 Git Bash Here**

```powershell
git config --global user.name "yourname" git config --global user.email "youremail" ssh-keygen -t rsa -C "youremail"
```

**找到这个文件夹。打开 id\_rsa.pub**

**其中，id\_rsa是你这台电脑的私人秘钥，不能给别人看的，id\_rsa.pub是公共秘钥，可以随便给别人看。把这个公钥放在GitHub上，这样当你链接GitHub自己的账户时，它就会根据公钥匹配你的私钥，当能够相互匹配时，才能够顺利的通过git上传你的文件到GitHub上。**

\*\*点击GitHub的右上角setting中 -> 点击左边SSH and GPG keys -> 点击New SSH key
title随便填，把C盘的id\_rsa.pub里面的信息复制到key里。\*\*

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511882.png "img")

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511883.png "img")

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511884.png "img")

**查看是否成功**

```powershell
ssh -T git@github.com
```

**这个时候要输入一次yes，如果前面生成ssh时设置了密码，需要输入密码**，**然后再回车**

### 6\. 将hexo部署到GitHub

**打开站点配置文件 \_config.yml，拉到最后，修改为1422756921就是你的GitHub账户**

```powershell
deploy: type: git repo: git@github.com:1422756921/1422756921.github.io.git branch: main
```

**注意：现在GitHub的默认分支已经是main了，不是master ！！！！**

**这个时候需要先安装deploy-git ，也就是部署的命令,这样你才能用命令部署到GitHub**

```powershell
npm install hexo-deployer-git --save
```

**然后其中`hexo clean`清除了你之前生成的东西，`hexo deploy` 部署文章，可以用`hexo d`缩写**

```powershell
hexo clean hexo g hexo deploy
```

**输入hexo deploy之后会出现一个小弹窗，要你输入GitHub的username和password。（用户名是邮箱）**

## 二、Github Actions自动化部署 Hexo博客

> 简单说，就是把hexo博客编译前的源代码上传到github代码仓库，Action在代码发生变动的时候，自动通过安装一系列nodejs环境和相关依赖，编译生成html页面到github pages仓库。
> 
> 再简单点说，就是把本地生成博客的工作，全部交给Action执行。
> 
> 好处就是随时随地都能修改或增加博文

### 1、先建一个私有仓库（自动化仓库）

**先建一个私有仓库（myhexo），这个仓库存放的是编译前的文件，也就是你电脑本地的文件，这个仓库是拿来做自动化的**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511885.png "img")

**也就是一共两个仓库**

- 一个公有仓库存编译好的hexo（pages仓库，用户名例如是`1422756921.github.io`）
- 一个私有仓库存**本地电脑编译前**的文件（自动化仓库，用户名是`myhexo`）

### 2、上传编译前的代码

**创建完私有仓库后，在本地博客文件中复制几个文件到另外一个文件夹，其中包括`.github`，`scaffolds`，`source`，`themes`，`_config.yml`，`package.json`，`package-lock.json`**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511886.png "img")

**还有一个很重要的一步：打开themes/bamboo主题模板文件，[主题源码](https://isedu.top/index.php/goto?url=aHR0cHM6Ly9naXRodWIuY29tL3l1YW5nMDEvaGV4by10aGVtZS1iYW1ib28=&cid=144)`.git`文件删除，Hexo博客根目录修改配置文件使用bamboo主题**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511887.png "img")

**然后回到myhexo根目录右键打开git bash**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511888.png "img")

```powershell
git init #把这个目录变成Git可以管理的仓库 git add . #添加当前目录文件到缓存区（别漏命令后面的点） git commit -m "first commit" #提交缓存区内容到本地库，并备注first commit #下面两条命令二选一，就行了 git remote add origin https://github.com/用户名/自动化仓库名.git #利用https关联远程仓库 git remote add origin git@github.com:用户名/自动化仓库名.git #利用ssh关联远程仓库 git push -u origin master #把本地库的所有内容推送到远程库上
```

**同样`SSH`和`HTTPS`均可。`SSH`在绑定过`ssh key`的设备上无需再输入密码，`HTTPS`则需要输入密码，但是`SSH`偶尔会遇到端口占用的情况。**

### 3、获取 Github token

**打开[https://github.com/settings/tokens](https://isedu.top/index.php/goto?url=aHR0cHM6Ly9naXRodWIuY29tL3NldHRpbmdzL3Rva2Vucw==&cid=144)**
**点击 Generate new token 新建个 token**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511889.png "img")

**note随便填，Expiration选择No expiration，勾选repo和workflow，其他没什么了，然后点生成就好了**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511890.png "img")

**把token复制下来**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511891.png "img")

**打开自动化仓库myhexo的`Settings` \-> `Secrets and variables` -> `Actions` -> `New repository secret`**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511892.png "img")

**一共有三个变量名`GITHUBTOKEN`，`GITHUBUSERNAME`，`GITHUBEMAIL`，逐一添加**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511893.png "img")

| 变量名 | 常量释义 |
| --- | --- |
| GITHUBMAIL | Github 用户邮箱地址 |
| GITHUBTOKEN | Github token |
| GITHUBUSERNAME | Github 用户名 |

### 4、添加workflows

**接下来点击`Actions` \-> `set up a workflow yourself`**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511894.png "img")

**复制以下代码到里面**

```powershell
name: 自动部署 on: push: branches: - master release: types: - published jobs: deploy: runs-on: ubuntu-latest steps: - name: 检查分支 uses: actions/checkout@v2 with: ref: master - name: 安装 Node uses: actions/setup-node@v1 with: node-version: "16.x" - name: 安装 Hexo run: | export TZ='Asia/Shanghai' npm install hexo-cli -g - name: 缓存 Hexo uses: actions/cache@v1 id: cache with: path: node_modules key: ${{runner.OS}}-${{hashFiles('**/package-lock.json')}} - name: 安装依赖 if: steps.cache.outputs.cache-hit != 'true' run: | npm install --save - name: 生成静态文件 run: | hexo clean hexo generate - name: 部署 #此处master:master 指从本地的master分支提交到远程仓库的master分支(不是博客的分支写master即可)，若远程仓库没有对应分支则新建一个。如有其他需要，可以根据自己的需求更改。 run: | cd ./public git init git config --global user.name '${{ secrets.GITHUBUSERNAME }}' git config --global user.email '${{ secrets.GITHUBEMAIL }}' git add . git commit -m "${{ github.event.head_commit.message }} $(date +"%Z %Y-%m-%d %A %H:%M:%S") Updated By Github Actions" git push --force --quiet "https://${{ secrets.GITHUBUSERNAME }}:${{ secrets.GITHUBTOKEN }}@github.com/${{ secrets.GITHUBUSERNAME }}/${{ secrets.GITHUBUSERNAME }}.github.io.git" master:master # GitHub配置
```

**粘贴上去后点击Commit changes...**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511895.png "img")

**就大功告成了，可以点击Actions查看运行进程了**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511896.png "img")

**最后，需要到GitHub pages那个仓库里面把默认页改成master就好了**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511897.png "img")

## 三、搭建Hexo博客后台管理Qexo

> Qexo，一个快速、美观、强大的在线hexo管理器，支持使用 Vercel 零成本一键部署,，您只需要配置一个免费数据库。特色功能：自定义图床上传图片，在线配置编辑，在线页面管理，开放 API，自动检查更新，在线一键更新，快速接入友情链接，简单的说说短文，类似不算子的统计，自动填文章模板

### 1、注册MongoDB

**首先我们要去[注册](https://isedu.top/index.php/goto?url=aHR0cHM6Ly93d3cubW9uZ29kYi5jb20vY2xvdWQvYXRsYXMvcmVnaXN0ZXI=&cid=144)MongoDB来给Qexo提供数据库，选择Free计划**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511898.png "img")

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511899.png "img")

**新建成功后会自动跳到”Security”的”Quickstart”**

记住username，密码，之后需要连接数据库

![image-20230811200842458](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511900.png "image-20230811200842458")

**等待几分钟数据库新建完成后，允许所有IP段访问（0.0.0.0）**

![image-20230811200943929](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511901.png "image-20230811200943929")

**进入刚刚建立好的数据库总览页面，点击Connect**

![image-20230811201051740](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511902.png "image-20230811201051740")

**然后连接方式选择MangoDB Shell**

![image-20230811201241934](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511903.png "image-20230811201241934")

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511904.png "img")

### 2、部署到Vercel

**点击下面的按钮部署，再选择GitHub存储库**

[![部署到 Vercel](https://cdn.jsdelivr.us/gh/jason5ng32/MyIP@main/public/Vercel.svg "部署到 Vercel")](https://isedu.top/index.php/goto?url=aHR0cHM6Ly92ZXJjZWwuY29tL25ldy9jbG9uZT9yZXBvc2l0b3J5LXVybD1odHRwczovL2dpdGh1Yi5jb20vYW0tYWJ1ZHUvUWV4bw==&cid=144)

**第一次部署会直接爆炸，问题不大，这是因为我们还没有设置数据库，请无视并重新进入项目, 在项目设置界面添加环境变量 Environment Variables**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511905.png "img")

**照着下列表格来添加**

| 名称 | 意义 | 示例 |
| --- | --- | --- |
| MONGODB\_HOST | MongoDB 数据库连接地址 | mongodb+srv://cluster0.xxxx.mongodb.net |
| MONGODB\_PORT | MongoDB 数据库通信端口 默认应填写 27017 | 27017 |
| MONGODB\_USER | MongoDB 数据库用户名 | chenrui |
| MONGODB\_DB | MongoDB 数据库名 | Cluster0 |
| MONGODB\_PASS | MongoDB 数据库密码 | JWo0xxxxxxxx |

**添加完之后到顶部的”Deployments”然后”Redeploy”**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511906.png "img")

### 3、初始化Qexo

**若没有 Error 信息即可打开域名进入初始化引导**

![image-20230811201918647](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511907.png "image-20230811201918647")

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511908.png "img")

**之前于 [Github 设置](https://isedu.top/index.php/goto?url=aHR0cHM6Ly9naXRodWIuY29tL3NldHRpbmdzL3Rva2Vucw==&cid=144) 生成的 Token (建议使用 Classic) 可以填以下栏目，需要 Repo & Workflow 下的权限 _不建议给出所有权限_**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511909.png "img")

**您的 Vercel 账户密钥 在 [此处](https://isedu.top/index.php/goto?url=aHR0cHM6Ly92ZXJjZWwuY29tL2FjY291bnQvdG9rZW5z&cid=144) 生成**

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511910.png "img")

Qexo 部署所在项目的 ID 位于项目的 Settings -> General -> Project ID

`prj_xxxxxxxxxxxxx`

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511911.png "img")

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511912.png "img")

![img](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503121511914.png "img")

**最终大功告成**

**祝你使用愉快**
原文链接： [https://isedu.top/index.php/archives/144/](https://isedu.top/index.php/archives/144/)

