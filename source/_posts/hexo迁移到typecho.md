---
abbrlink: ''
categories: []
date: '2025-03-14T14:28:10.774202+08:00'
tags: []
title: Hexo迁移到Typecho
updated: '2025-03-14T14:28:11.416+08:00'
---
上一篇文章做了 Hexo 的优缺点总结，搞得有点跃跃欲试转到 typecho。于是上网找了下方法，已经有大神写好脚本了，操作起来还是比较容易的。

这次使用到的是 [hexo-2-typecho](https://github.com/jiangmitiao/hexo-2-typecho) 迁移插件。在 hexo 项目中，执行 `npm i hexo-2-typecho` 安装。

## 生成 SQL 文件

如果是单用户博客，基本什么都不用做，执行 hexo 部署时，会自动导出 sql 文件。

调用 `hexo g` 命令，在目录下生成 typecho 文件夹（如下图）。

![image.png](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503141404191.png)

## 导入数据库

这里推荐使用数据库工具 Navicat，如果有其他合适的也可以。

导入之前，需要对 sql 文件进行替换。

把`t_relationships`，`t_contents`，`t_metas`换成你对应的数据表名。

如果你安装 typecho 时没改前缀，则默认对应`typecho_relationships`，`typecho_contents`，`typecho_metas`，批量替换下就行。

![image.png](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503141404192.png)

拖动文件进去，运行 SQL 文件。

![image.png](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503141404193.png)

导入完成。

![image.png](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503141404194.png)

## 收尾工作

接下来你可能还需要对文章图片路径进行批量修改；

需要设置 typecho 的伪静态、文章路径；

找个漂亮主题；插入统计代码等等。

![image.png](https://gcore.jsdelivr.net/gh/alist1314/picture@main/pic/202503141404195.png)

## 结尾

最后我还是没选择 typecho，继续使用 Hexo。

一是我完成了语雀自动同步和部署的脚本，写文章变得很容易了。

二是 typecho 没有自带的图片上传，靠外链插入图片。

三是没找到喜欢的主题，懒得再折腾主题和插件了。

四是 php 和 sql 不是我最擅长的，还是用 node 和 md 搞搞静态算了。
