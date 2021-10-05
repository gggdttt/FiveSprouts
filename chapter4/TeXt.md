# 1. Intro

关于[TeXt theme](https://github.com/kitian616/jekyll-TeXt-theme) ，是一位大佬用爱发电写出来的一个个人博客模板。其部署非常方便，特别是如果你将其部署在GitHubPage中，只需要fork其至你自己的库然后修改名字，最后clone到本地进行配置修改即可上线。

具体的教程可以查看[官方文档](https://github.com/kitian616/jekyll-TeXt-theme/blob/master/README-zh.md)，非常详细且中英双语。

博客默认是放在GitHub的，最近有人反馈速度很慢，测试发下由于墙的原因，国内访问确实很慢。因此就打算部署两套博客系统：

- 海外访问继续用GitHub，使用域名：http://blog-oversea.bihe0832.com/
- 国内访问用国内服务器，使用域名：http://blog.bihe0832.com/

# 2. 部署步骤

## 2.1 环境介绍

### 2.1.1 服务器环境

- 服务器：

  ```bash
    $ cat /proc/version
    Linux version 3.10.0-862.el7.x86_64 (builder@kbuilder.dev.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-28) (GCC) ) #1 SMP Fri Apr 20 16:44:24 UTC 2018
    $ hostnamectl
       Static hostname: VM_0_17_centos
             Icon name: computer-vm
               Chassis: vm
            Machine ID: c28d40cbc8e3adcb4e32d9779a77b39e
               Boot ID: 32408570e27148c5a18f1e0d97096680
        Virtualization: kvm
      Operating System: CentOS Linux 7 (Core)
           CPE OS Name: cpe:/o:centos:centos:7
                Kernel: Linux 3.10.0-862.el7.x86_64
          Architecture: x86-64
  ```

### 2.1.2 环境部署

Jekyll 依赖Ruby，而且不同的OS 版本对于 Ruby的支持版本也不一样，经过多次尝试，发现下面的组合最合适。由于安装需要多个步骤，因此我在调试通以后直接把所有的步骤，写了一个shell 脚本。

```bash
#!/bin/bash
# author code@bihe0832.com

function checkResult() {  
   result=$?
   echo "result : $result"
   if [ $result -eq 0 ];then
      echo "checkResult: execCommand succ"
   else
    echo "checkResult: execCommand failed"
    exit $result
   fi
}  

gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
\curl -sSL https://get.rvm.io | bash -s stable
source /etc/profile.d/rvm.sh
rvm install ruby-2.4.1
rvm 2.4.1 --default
ruby -v
rvm install rubygems 2.6.12 --force
gem -v
gem sources -l
gem install jekyll bundler github-pages jekyll-paginate
jekyll -v

chmod 755 /usr/local/rvm/gems/ruby-2.4.1/bin/*
```

## 2.2 代码部署

这一步就是把你的Jekyll的代码 checkout到服务器。例如：

```bash
git clone https://github.com/gggdttt/gggdttt.github.io.git 
```

## 2.3 编译构建

在项目根目录直接命令行运行 ` jekyll build ` 即可查看构建结果

```bash
 blog git:(master) ✗/usr/local/rvm/gems/ruby-2.4.1/bin/jekyll build
Already up-to-date.
Configuration file: /bihe0832.github.io/_config.yml
            Source: /bihe0832.github.io
       Destination: /bihe0832.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
                    done in 5.529 seconds.
 Auto-regeneration: disabled. Use --watch to enable
```

如果构建没有报错，你就可以选择将构建结果 `_site `部署到你的服务器。

## 2.4 定时自动构建

这一步是所有的流程中仅次于环境部署的折腾的。由于有多个域名，因此我通过 crontab 自动 checkout 最新的代码并自动构建。结果发现就是不成功，最后通过不断完善日志终于解决。

在编译构建的时候，Shell中使用的命令是：

```bash
/usr/local/rvm/gems/ruby-2.4.1/bin/jekyll build
```

如果你想要在 crontab 中自动构建，要把上面的bin 换为 wrappers ，也就是：

```bash
/usr/local/rvm/gems/ruby-2.4.1/wrappers/jekyll build
```

另外如果已经发布的内容还是频繁修改的话，建议尽量不要用 –incremental 参数

# 3. TeXt 自己构建分类

> Reference:https://smartadpole.github.io/tool/blog/2021/01/18/TeXt-theme-head.html#head
>
> 关于使用 `TeXt theme` 时，想要实现文章分类显示的办法；主要思路是使用 head + 导航页面 + 文章分类的功能；

简介：

- 先给文章文类；
- 编写一个页面，过滤出特定类别；
- 添加 head，并嵌入到上一个页面中，以实现多类别聚合；

## 3.1  带标签的文章

页面示例：`2021-01-18-hello.md`

```
---
layout: article                       # 不变
title:  "TeXt theme 下实现文章分类"     # 文章标题
date:   2021-01-18 17:34:40 +0800     # 编写时间，不要超过当前系统时间，否则编译不通过
key:    text-theme-head               # 文章的唯一 ID，不要重复了
aside:
  toc: true
category: [tool, blog]                # 重点来了，这是类别标签（什么名字都可以，别和其他标签重了）
---
```

最终编译完，页面路径是 `/tool/blog/2021/01/18/hello.html`，很明显，标签已经进入到了 url 中，这也是后续实现文章分类的关键——关键字过滤；

## 3.2 分类页面

![示意图](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261726948.png)

这是一个完成分类的页面，当前页面都是和 blog 构建相关的文章；首先可以确定的是，最终分类所得的列表是一个页面，也就是此处的 `/assets/category/work/tool/blog.html`，也就是说，当你想实现一个文章分类功能时，去编写一个 `/assets/category/**/*.html` 即可；
注意：html 存在于多少层的子目录中，没有关系；

示例：[/assets/category/work/tool/blog.html](https://github.com/smartadpole/smartadpole.github.io/blob/master/assets/category/work/tool/blog.html)

详情，查阅[源码](https://github.com/smartadpole/smartadpole.github.io/blob/master/assets/category/work/tool/blog.html)

```
# 头部 和文章一样；
---
layout: articles
titles:
    en:       Blog
    zh:       博客
    zh-Hans:  博客
    zh-Hant:  博客
sidebar:
    nav: TOOL       # 此处暂不说明   
---
...
        if cat[0] == "blog"               # 重点在这里，就是第一步写上去的标签
...
```

截止目前，已经实现了文章分类的功能；但是去哪里找这个分好类的页面呢?

## 3.3  展示结果

当我们有较多分好类的页面时，那么我们想实现像文档目录一样的功能，怎么做——用 `_data/navigation.yml`；
该文件中默认是标题栏：

```
header:
  - titles:
      en:       software                                      # 名称
      zh:       软件
    url:        /assets/category/work/software/software.html  # 超链接
```

我们就利用这个功能，实现目录结构；
编辑该文件，添加属于自己的目录

```
TOOL:                                                    # 目录 ID，唯一
  - title:      工具                                      # 一级目录名字
    children:
      - title:  博客                                      # 二级目录名字
        url:    /assets/category/work/tool/blog.html     # 二级目录链接（此处填写我们上一步写好的分类页面）
      - title:  Office
        url:    /assets/category/work/tool/office.html
```

而此处的目录 ID，就是要写入文章头部的、用来加载自制目录的关键字：

```
layout: articles
titles:
    en:       Blog
    zh:       博客
sidebar:
    nav: TOOL
```

注：以及目录不支持添加超链接，但是支持名字为空
