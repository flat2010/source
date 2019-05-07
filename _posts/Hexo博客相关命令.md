---
title: Hexo博客相关命令
date: 2019-05-07 11:07:13
tags: [Hexo, 个人博客]
categories: [Hexo博客] 
comments: true
toc: true
---



<centerhead>文章创建和发布</centerhead>

### 1.1 创建
#### 1.1.1 正式文章
&emsp;&emsp;正式文章，默认放到_posts目录下，使用默认参数启动Hexo时会显示在文章列表中，会发布在博客上。
```bash
$ hexo new "文章名字"
```
&emsp;&emsp;如果创建文章的时候要制定布局，需要加入布局参数：
```bash
$ hexo new 布局名称 "文章名字"
```
&emsp;&emsp;执行后相应的文章会放到布局文件夹下，Hexo默认的布局有三种：
- post：对应目录`_posts`；
- page: 对应目录`_pages`；
- draft: 对应目录`_drafts`；

#### 1.1.2 创建草稿
&emsp;&emsp;创建一篇草稿，默认放到_drafts目录下，启动Hexo时要带`--draft`参数才能看到，否则不会在显示在文章列表中，**不会发布在博客上**，要发布需要先转成正式文章。
```bash
$ hexo new draft "草稿名字"
```
&emsp;&emsp;<font color="red" size=2>注：该功能正好提供了一个私密空间（文章仅自己可见），可以利用该特性保留一下不想删除但又不想发布在博客上的文章。</font>

### 1.2 发布
&emsp;&emsp;发布前最好先执行下清理工作，再重新生成，最后发布。**前面两步非必须。**
#### 1.2.1 清理缓存
&emsp;&emsp;执行该命令会先清理本机中已生成的文章（缓存），非必须。
```shell
$ hexo clean
```

#### 1.2.2 重新生成
&emsp;&emsp;执行该命令会在本地重新生成要发布的文章(静态文件)，非必须，但如果先前执行了清理工作，就需要执行该命令。
```bash
$ hexo generate
```
&emsp;&emsp;或者简写为：
```bash
$ hexo g
```

#### 1.2.3 发布正式文章
&emsp;&emsp;执行该命令会发布本地生成的博客文章（**不会发布草稿**）。
```bash
$ hexo publish
```
&emsp;&emsp;或者简写为：
```bash
$ hexo p
```

#### 1.2.4 发布草稿
&emsp;&emsp;如果要将一篇草稿发布为正式文章，则使用：
```bash
$ hexo p draft "草稿名称"
```
&emsp;&emsp;这个命令实际做的工作是把你指定的`_drafts`目录下的文章移动到`_posts`目录下，从而成为正式文章。

#### 1.2.5 部署到服务器
&emsp;&emsp;执行该命令会将本地发布的博客部署到相应的服务器([Github](https://github.com/))上。
```bash
$ hexo deploy
```
&emsp;&emsp;或者简写为：
```bash
$ hexo d
```

<br><br><br>
<centerhead>启动</centerhead>

### 2.1 启动
#### 2.1.1 默认启动
&emsp;&emsp;执行该命令，将会以默认模式（**不显示草稿文章，默认端口4000**）启动Hexo，启动后Hexo会自动追踪文章的更新，动态更新显示而无需重启Hexo服务。
```bash
$ hexo server
```
&emsp;&emsp;或者简写为：
```bash
$ hexo s
```

#### 2.1.2 静态启动
&emsp;&emsp;执行该命令，Hexo会启动为静态模式，此时Hexo只会处理 public 文件夹内的文件，而不会处理文件变动。在执行时，应该先自行执行`hexo generate`，此模式通常用于生产环境（production mode）下。
```bash
$ hexo server -s
```
&emsp;&emsp;或者简写为：
```bash
$ hexo s -s
```

#### 2.1.3 指定端口或IP
&emsp;&emsp;如果需要指定web访问端口及IP地址，需要分别指定`-p、-i`参数：
```bash
$ hexo s -p 端口号 -i IP地址
```

#### 2.1.4 显示草稿
&emsp;&emsp;如果要在本地显示草稿文章，需要带上`--draft`参数：
```bash
$ hexo s --draft
```