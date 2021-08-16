# Version control and tools

## 基于Git的版本控制

### 定义

> **版本控制**（英语：Version control）是维护工程[蓝图](https://zh.wikipedia.org/wiki/藍圖)的标准作法，能追踪工程蓝图从诞生一直到定案的过程。此外，版本控制也是一种[软件工程](https://zh.wikipedia.org/wiki/軟體工程)技巧，借此能在软件开发的过程中，确保由不同人所编辑的同一程序文件都得到同步。 [ref:版本控制](https://zh.wikipedia.org/wiki/%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6)

### 为什么需要版本控制？

首先先介绍两个主要使用场景：

* 场景1：一天我和隔壁的工位的小哥被安排到了同一个项目，在基于项目A的基础上扩展开发一个功能B。这时候项目经理拿来了一个U盘让0 我们拷贝代码到自己的电脑。我们兴奋得拷贝完之后就开始了写代码。一天的工作结束了之后，我又掏出u盘，他也掏出了u盘，一起走到项目经理那把u盘给他，告诉他这是今天的工作。项目经理心里苦但也只能默默插入两个u盘然后拷出了两份代码。 但是在将这两份代码合并到他电脑上的源码的时候，又发现了我们的代码存在冲突（对同一个文件的同一部分代码进行了修改的情况）：

  我的代码：

  ```java
  Integer a = 0;
  a = a + 1;
  ```

  隔壁小哥的代码：

  ``` java
  Integer a = 0;
  a = a + 222222;
  ```

  这时候项目经理让我们自己去解决一下代码里面的矛盾，然后再拿他这里来合并、拷贝到他的电脑上。

* 场景2：经过了一天的追赶ddl，终于项目成功上线了。但是在上线完项目之后忽然发现它导致了另外一个关键服务器的宕机。这个时候为了恢复服务器让其正常运转，我们需要取消这次上线将代码回退到上次提交的版本，如果没有进行版本控制是很难完成回退的（除非每个版本的代码都在磁盘上进行备份），而且不便于管理各个版本的代码。

以上两个场景是比较常见的`git` 的使用场景：场景1用git来进行团队间的合作，场景2用git来实现不同版本之间的切换（任何一次`commit`都会被记录形成一个节点，可以在对应的git的`log`中查看，如下图为**<u>5Sprouts</u>**的`log`）。

> 在[计算机领域](https://zh.wikipedia.org/wiki/電腦運算)，日志文件（**logfile**）是一个记录了发生在运行中的[操作系统](https://zh.wikipedia.org/wiki/操作系统)或其他[软件](https://zh.wikipedia.org/wiki/软件)中的事件的文件，或者记录了在[网络聊天](https://zh.wikipedia.org/wiki/网络聊天)软件的用户之间发送的消息。**日志记录**（**Logging**）是指保存日志的行为。最简单的做法是将日志写入单个存放日志的文件。

![某git项目的本地日志界面](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/image-20210815110341585.png)

不难发现，每行都是我的一次提交记录。右击单条记录，有若干选项可以操作（为了方便理解我直接贴了`tortoise`的用户界面，用纯命令行当然都是能实现的）。

![右击单条记录](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/image-20210815110414371.png)

当我和别人一起合作时，需要注意的是如果像场景一中那样两个人同时修改了一处代码，其中有一个人是无法直接提交的。当出现`conflict`的时候，一定要在<u>**询问过对方之后**</u>再决定是accept自己的代码还是对方的代码，而不能直接强行覆盖（很多新手不管三七二十一就直接把自己的部分push了上去或者未询问他人就直接覆盖了他的代码），这是 一定会让别人写的部分出问题的。

### git 的安装和使用

> 墙裂推荐git指令练习的网站：https://learngitbranching.js.org/

1. 进入[官网]()找到相关链接下载并安装

   * [windows](https://git-scm.com/download/win)
   * [macOS](https://git-scm.com/download/mac)
   * [Linux](https://git-scm.com/download/linux)

2. 除了`linux`下的安装外，`windows`和`macOS`都是下载安装包之后直接按步骤安装即可。

   >  这里推荐在你的计算机的根目录或者合适的地方创建一个`development`文件夹，然后在`development` 下创建一个`development_tools`用来安装所有相关工具，同时创建一个`development_env`来存放后续可能需要的各种包。这样子可以方便管理你的工具、环境以及后续的代码

   将其安装到`development_tools`下：

   ```cmd
   └─ {Your Path}
       └─ development
           ├─ development_tools(这个文件夹专门用来安装各种安装工具)
           ├─ development_codes(这个文件夹用来存放开发用的代码)
           └─ development_env(这个文件夹专门存放各种开发用的开发环境)
   ```

   

3. git的使用

   在安装完git之后，比较推荐的一种使用方式是通过git远程托管代码到git服务器上，这样有利于协作。但是如果你只是单纯想

