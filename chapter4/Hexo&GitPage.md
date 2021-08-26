## 如何用Hexo+Github Pages快速搭建自己的博客

**保姆级教程**
本文章快速搭建博客，有一个前提，那就是对git有所了解，如果你对git的使用即原理很熟悉的话，那么这个博客搭建就会很快，不超过一个小时。当然你下载资源很慢，就当我没说，嘻嘻。当然如果你不了解，也没关系，凡事都是从0开始的，我也会在文中讲解的，跟着我的步骤一步步来，废话少说，开始！

# 温馨提示

1.首先要知道搭建网站你的目的是什么，有动力，才有坚持下去的勇气，也才会有拨开云雾见太阳的高兴(博客搭建好的时候)。

2.资源下载慢是很大的问题，下面会解答。

3.如果你github网站访问速度巨慢，建议谷歌或者百度，毕竟面向谷歌编程嘛,哈哈哈。

4.如果搭建过程中，遇到问题，可以评论，我看见就回。
打个广告：[我的博客](https://link.segmentfault.com/?url=https%3A%2F%2Fchenqd123.github.io)

# 文章导读

1.介绍要下载的东西
2.安装
3.搭建
4.原理解释（之所以放在最后，是给自己总结，明白其原理，回过头才会理解更深,当然后面美化博客，也需要知道大概原理。）

# 1.介绍工具，安装的东西

如果不耐烦，可以直接跳过到安装步骤。
1.git：按我自己的理解，git可以再我们的电脑上创建一个本地仓库（在这就是我们的博客仓库），然后把我们的网页部署到github上的github Pages上。
2.node.js:不安装这玩意，就没法玩。因为他要安装Hexo。
3.Hexo：这个东西可以把我们本地的Markdown文件，后缀.md的文件，处理，渲染，然后变成静态网页，也就是我们的博客。
**所有看官们知道了吧，慢慢安装吧，不安装好，博客就凉凉了。**

# 2.安装工具

## （1）安装Node.js

这个玩意推荐大家去[官网下载](https://link.segmentfault.com/?url=https%3A%2F%2Fnodejs.org%2Fen%2Fdownload%2F)，网不好建议下载个火狐浏览器。进入官网，直接选自己是多少位的就行。不知道的到我的电脑去看。我选的windows 64-bit。
![在这里插入图片描述](https://segmentfault.com/img/remote/1460000023346637)

## （2）安装git

这个也是[Git官网](https://link.segmentfault.com/?url=https%3A%2F%2Fgit-scm.com%2Fdownload%2Fwin)下载，跟上面一样，选择对应的版本下载即可。
下载好了就安装，安装一直next就行，然后cmd里git version，有版本号就成功了。然后去桌面有键就有下面两个git了。上面那个是图形化管理工具，由于做的比较丑，我们还是选用下面的命令行来操作。
![在这里插入图片描述](https://segmentfault.com/img/remote/1460000023346638)

## （3）安装主角Hexo

由于Hexo这玩意是托管在国外的，所以啊，下载会巨慢，所以我们就用国内的淘宝镜像。在cmd中输入以下：

```java
 npm config set registry https://registry.npm.taobao.org
 npm config get registry//**这个是用来检查换成功没**
```

![在这里插入图片描述](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261506321.png)

安装Hexo

```java
 npm install hexo-cli -g
```

安装hexo的一个相当于命令的东西吧。后面原理会讲。

```java
npm install hexo-deployer-git --save
```

第二句是安装hexo部署到git page的deployer

换好镜像，安装好Hexo，搭建博客的前戏就完了，可以喝杯咖啡，起来走走，劳逸结合嘛。

# 3.搭建博客

这个部分，如果网速好，很快就完事了，网速不好，就静静等吧。

## （1）初始化博客

首先自己选个地方，创建一个文件夹，来装我们博客的主体。
比如F:Projectsblog，文件夹下右键鼠标，点击Git Bash Here，进入Git命令框，执行以下操作。
![在这里插入图片描述](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261441185.png)
这里是最耗时间的了，因为主题，资源比较多，下载会很慢，不过，只要是网速问题，我们都能解决，就是通过换淘宝镜像，提高速度。其他的不懂谷歌呀，哈哈。
初始化好之后，一个崭新的博客出现在了我们眼前。
![在这里插入图片描述](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261441031.png)
这时候，再bush中输入

```java
hexo s
```

![在这里插入图片描述](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261506180.png)

直接网页搜索localhost:4000，你的博客雏形就出来了。这TM才叫功夫不负有心人。
当然这只是你本地仓库的博客，我们写博客不就是为了装B吗，不让别人看见，那能叫装B吗？下贱，哈哈哈！开玩笑，写博客肯定是提升自己呀，怎么扯到装B上了。
![在这里插入图片描述](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261506882.png)

## （2）github的使用

**敲黑板！！！重点来了！！！**
部署博客到GIthub - Pages，文章开头不是说了吗，如果你了解git那么，这部分，你可以略过，小白们还是来学下。

首先，注册个github账号吧，总不可能，用别人东西，还不给别人增加流量吧，拒绝白嫖，嘻嘻嘻，**搭建好点赞你懂得**！！！！

进入正题，其实不要把github想的很难，主要是英文把你难着了吧，哈哈哈，如果英语差可以谷歌浏览器翻译。

github其实就是个托管代码的，我们在这上面建库，然后把代码push上来，当然我们的博客不用push上来，后面讲解原理。

![在这里插入图片描述](https://segmentfault.com/img/remote/1460000023346643)

## （3）配置SSH钥匙：

SSH是个协议，就像HTTPS一样，保证安全的。
我们要把项目托管上去，github需要知道一把钥匙，这是公钥，我们自己有一把钥匙，叫私钥。
下面配置：
**1.创建钥匙：**

```java
 $ ssh-keygen -t rsa -C "your_email@example.com"
#这将按照你提供的邮箱地址，创建一对密钥
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/you/.ssh/id_rsa): [Press enter]
```

直接回车

```java
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
Your identification has been saved in /c/Users/you/.ssh/id_rsa.
Your public key has been saved in /c/Users/you/.ssh/id_rsa.pub.
The key fingerprint is:
01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@example.com
```

然后钥匙的位置在用户文件下，叫做 **.ssh**,有个后缀是pub的，这个就是公钥，私钥是另一把，把公钥的内容复制下来，到github，点你的头像，有个setting，设置里去配置公钥。

![在这里插入图片描述](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261441276.png)
好了配置好公钥了，下面检查一下

```java
 $ ssh -T git@github.com
The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?
```

输入yes就好了，下面就要设置用户信息了

```java
$ git config --global user.name "github名字"//用户名
$ git config --global user.email  "github注册的邮箱"//填写自己的邮箱
```

下面点击的你新建的仓库，有个：![在这里插入图片描述](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261441548.png)
复制你的项目地址，到Hexo的配置文件里：就是
![在这里插入图片描述](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261506646.png)
找到deploy（部署），开始只有个type的，加上我写的，只用改repository就行了，branch不用改：

```java
deploy:  type: git  repository: git@github.com:chenQD123/chenQD-blog.git  branch: master
```

**记住这有个坑，** 就是冒号后面有个空格，这是规定，如果没写空格，后面会部署失败，这就像python一样。
然后就是美汁汁的部署你的博客到github上。

```java
在Hexo里，hexo generate -deploy生成+部署
```

如果报错：说明最开始让你安装的deploy没成功，所以在安装一次就行了，他也有提示（会让你安装），如下：

```java
ERROR Deployer not found: git
```

只需要安装好即可

```java
npm install hexo-deployer-git --save
```

## （4）部署博客

好了，结束了，看到这里，你的博客就部署完成了，这时候你的博客，别人就能看见了，只需要在任何一个浏览器输入：
你的项目名，比如我就是：
[https://chenqd123.github.io/](https://link.segmentfault.com/?url=https%3A%2F%2Fchenqd123.github.io%2F)，这也是github给我们的公开地址，不如说是github Pages给的。就是github pages帮我们托管的博客。

## （5）美化博客

如果想使自己的博客好看点可以换主题：
[Hexo官网主题](https://link.segmentfault.com/?url=https%3A%2F%2Fhexo.io%2Fthemes%2F)
建议换个，使用者多点的主题，这种的说明文档写的比较好，你开荒也会容易点。
美化博客你只需要点进官网开源的主题，然后找到这个主题的github链接，他会有详细教程的，从安装开始。建议休息下，身体才是革命的本钱。

# 4.实现原理

首先来介绍为Hexo：
**Hexo是Node.js在github开源的快速开发博客的一个框架。**
那么Hexo究竟怎么帮助我们实现的呢，这里说一下大概原理，我们开始安装Hexo，是通过

```java
npm install hexo-cli -g
```

安装好之后，我们通过**hexo+命令**这样的方式来操作Hexo的一些插件，就比如：hexo server、hexo generate，他实际上执行hexo会通过node调用hexo-cli中的[entry函数](https://link.segmentfault.com/?url=https%3A%2F%2Fgithub.com%2Fhexojs%2Fhexo-cli%2Fblob%2F5e0969ffa64dec427428a245ab2d65beaf23123b%2Flib%2Fhexo.js%23L13)(比如，可以把’hexo init’视为’node hexo-cli/entry.js init’)。
当执行hexo server时，Hexo的Markdown文档会进行两次渲染，变成我们的博客。
1.第一次是遍历我们的根目录下的source文件，就是我们博客的页面，他会输出一些article对象，就是我们每个页面等。
2.第二次渲染会遍历我们themes下的文件，这个文件下的内容，是我们美化要修改的地方，hexo generate他会输出生成根目录下的public文件夹。
这个public是我们要托管到github上的博客，当我们hexo deploy完成，就会把我们的博客push，部署到github上，每个github账号都会有一个站点，并且有无限的项目站点。而github pages 就是来托管我们的页面的，通过这个github pages给我们的域名来访问我们的博客，我们每次修改博客时，也只是提交的public文件，hexo generate产生、hexo deploy部署的页面。

# 结语

写到这了。
文章到这里就结束了，第一次写这么长的博客，遇到问题请评论，我看见绝对回！
回过头来想，这搭建博客的一路也许遇到了很多问题，但这都是提高我们自己的必经之路，要相信付出才会收获，所以enjoy it。
如果对你有帮助，**点赞点赞**，码了这么多的字，没有功劳也有苦恼呀，呜呜呜（撕心裂肺）。