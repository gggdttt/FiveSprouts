# 如何用Hexo+Github Pages快速搭建自己的博客

本文章快速搭建博客，有一个前提，那就是对git有所了解，如果你对git的使用即原理很熟悉的话，那么这个博客搭建就会很快，不超过一个小时。

## 1.介绍工具

1. **git**: git可以再我们的电脑上创建一个本地仓库（在这就是我们的博客仓库），然后把我们的网页部署到github上的github Pages上。
2.  **node.js**: 需要它来要安装Hexo
3. **Hexo**: 这个东西可以把我们本地的`Markdown`文件(后缀`.md`)的文件，处理，渲染，然后变成静态网页，也就是我们的博客。

## 2.安装工具

### 2.1 安装Node.js

直接在官网下载即可。https://nodejs.org/zh-cn/

![Node.js](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261523905.png)

### 2.2 安装git

这个也是[Git官网](https://git-scm.com/)下载。可以参考第一章的tools中的安装步骤。


### 2.3 安装Hexo

由于Hexo这玩意是托管在国外的，在国内下载可能会限速，所以可以使用国内的淘宝的镜像。在cmd中输入以下：

```bash
 npm config set registry https://registry.npm.taobao.org
 npm config get registry//**这个是用来检查换成功没**
```

![在这里插入图片描述](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261506321.png)

安装Hexo

```bash
 npm install hexo-cli -g
```

安装hexo-deployer-git

```bash
npm install hexo-deployer-git --save
```

## 3.搭建博客

### 3.1 初始化博客

首先在合适的路径下创建一个文件夹，来装博客的主体。
比如`D://development//project`，文件夹下右键鼠标，点击Git Bash Here，进入Git命令框，执行以下操作:
![init hexo](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261441185.png)

国内如果不换镜像这一步可能会很耗时。

初始化好之后，一个崭新的博客出现在了我们眼前。
![在这里插入图片描述](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261441031.png)
这时候，再bash中输入

```bash
hexo s
```

![Demo](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261506180.png)

直接网页搜索localhost:4000，博客雏形就出来了。
![初始化](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261506882.png)



### 3.2 github新建repo

同第一章tools中关于github新建库的描述，但是仓库名必须是{用户名.github.io}



![效果](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261531175.png)

### 3.3 配置SSH钥匙

SSH是个协议，就像HTTPS一样，保证安全的。
我们要把项目托管上去，github需要知道一把钥匙，这是公钥，我们自己有一把钥匙，叫私钥。
下面配置：
**1.创建钥匙：**

```bash
 $ ssh-keygen -t rsa -C "your_email@example.com"
#这将按照你提供的邮箱地址，创建一对密钥
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/you/.ssh/id_rsa): [Press enter]
```

直接回车

```bash
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
Your identification has been saved in /c/Users/you/.ssh/id_rsa.
Your public key has been saved in /c/Users/you/.ssh/id_rsa.pub.
The key fingerprint is:
01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@example.com
```

然后钥匙的位置在用户文件下，叫做 **.ssh**, 有个后缀是pub的，这个就是公钥，私钥是另一把。把公钥的内容复制下来，到github，点你的头像→setting，去配置公钥。

![配置公钥](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261533676.png)
好了配置好公钥了，下面检查一下

```bash
 $ ssh -T git@github.com
The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?
```

输入yes就好了，下面就要设置用户信息了

```bash
$ git config --global user.name "github名字"//用户名
$ git config --global user.email  "github注册的邮箱"//填写自己的邮箱
```

下面点击的你新建的仓库，有个：

![clone库](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261534334.png)

复制你的项目地址，到Hexo的配置文件里：就是
![_config.yml](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261535520.png)
找到deploy（部署)，开始只有个type的，加上我写的，只用改repository就行了，branch不用改：

```yaml
deploy:  type: git  repository: git@github.com:{Your name}/{Your repo}.git  branch: master
```

> **记住这有个坑，** 就是冒号后面有个空格，这是规定，如果没写空格，后面会部署失败，这就像python一样。

下一步就是部署:

在Hexo里，

```java
hexo generate -deploy
```

如果报错：说明最开始让你安装的deploy没成功，所以在安装一次就行了，他也有提示（会让你安装），如下：

```bash
ERROR Deployer not found: git
```

只需要安装好即可

```bash
npm install hexo-deployer-git --save
```

### 3.4 访问博客

你的博客就部署完成了，这时候你的博客，别人就能看见了，只需要在任何一个浏览器输入：
你的项目名，比如我就是：
https://gggdttt.github.io/ ，这是github给我们的公开地址。

### 3.5 美化博客

如果想使自己的博客好看点可以换主题：
[Hexo官网主题](https://link.segmentfault.com/?url=https%3A%2F%2Fhexo.io%2Fthemes%2F)
建议换个，使用者多点的主题，这种的说明文档写的比较好，方便新人上手。
美化博客你只需要点进官网开源的主题，然后找到这个主题的github链接，都会有详细教程。

## 4.实现原理

首先来介绍为Hexo：
**Hexo是Node.js在github开源的快速开发博客的一个框架。**
那么Hexo究竟怎么帮助我们实现的呢，这里说一下大概原理，我们开始安装Hexo，是通过

```bash
npm install hexo-cli -g
```

安装好之后，我们通过**hexo+命令**这样的方式来操作Hexo的一些插件，就比如：hexo server、hexo generate，他实际上执行hexo会通过node调用hexo-cli中的`entry`函数(比如，可以把’hexo init’视为’node hexo-cli/entry.js init’)。
当执行hexo server时，Hexo的Markdown文档会进行两次渲染，变成我们的博客。
1.第一次是遍历我们的根目录下的source文件，就是我们博客的页面，他会输出一些article对象，就是我们每个页面等。
2.第二次渲染会遍历我们themes下的文件，这个文件下的内容，是我们美化要修改的地方，hexo generate他会输出生成根目录下的public文件夹。
这个public是我们要托管到github上的博客，当我们hexo deploy完成，就会把我们的博客push，部署到github上，每个github账号都会有一个站点，并且有无限的项目站点。而github pages 就是来托管我们的页面的，通过这个github pages给我们的域名来访问我们的博客，我们每次修改博客时，也只是提交的public文件，hexo generate产生、hexo deploy部署的页面。

