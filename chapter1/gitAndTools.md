# Version control and tools

## 1.Version control based on Git

### 1.1 Definition

> **版本控制**（英语：Version control）是维护工程[蓝图](https://zh.wikipedia.org/wiki/藍圖)的标准作法，能追踪工程蓝图从诞生一直到定案的过程。此外，版本控制也是一种[软件工程](https://zh.wikipedia.org/wiki/軟體工程)技巧，借此能在软件开发的过程中，确保由不同人所编辑的同一程序文件都得到同步。 [ref:版本控制](https://zh.wikipedia.org/wiki/%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6)

### 1.2 Why do we need version control?

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

  这时候项目经理让我们自己去解决一下代码里面的矛盾，然后再拿他这里来合并、拷贝到他的电脑上。如果现代软件工程使用这种方式进行合作，简直就是人类史上最大的灾难之一！(=_=!)

* 场景2：经过了一天的追赶ddl，终于项目成功上线了。但是在上线完项目之后忽然发现它导致了另外一个关键服务器的宕机。这个时候为了恢复服务器让其正常运转，我们需要取消这次上线将代码回退到上次提交的版本，如果没有进行版本控制是很难完成回退的（除非每个版本的代码都在磁盘上进行备份），而且不便于管理各个版本的代码。

以上两个场景是比较常见的适合使用`git` 的场景：<u>场景1</u>用git来进行团队间的合作，<u>场景2</u>用git来实现不同版本之间的切换（任何一次`commit`都会被记录形成一个节点，可以在对应的git的`log`中查看，如下图为我的项目的一部分`log`）。

> 在[计算机领域](https://zh.wikipedia.org/wiki/電腦運算)，日志文件（**logfile**）是一个记录了发生在运行中的[操作系统](https://zh.wikipedia.org/wiki/操作系统)或其他[软件](https://zh.wikipedia.org/wiki/软件)中的事件的文件，或者记录了在[网络聊天](https://zh.wikipedia.org/wiki/网络聊天)软件的用户之间发送的消息。**日志记录**（**Logging**）是指保存日志的行为。最简单的做法是将日志写入单个存放日志的文件。

![Local log of git](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/image-20210815110341585.png)

不难发现，每行都是我或者我的队友的一次提交记录。右击单条记录，有若干选项可以操作（为了方便理解我直接贴了[tortoise](#tortoise)的图形用户界面，实际上git还是推荐纯命令行操作，因为很酷）。

![Right click on singal record](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/image-20210815110414371.png)

当我和别人一起合作时，需要注意的是如果像场景一中那样两个人同时修改了一处代码，其中有一个人是无法直接提交的。当出现`conflict`的时候，一定要在<u>**询问过对方之后**</u>再决定是accept自己的代码还是对方的代码，而不能直接强行覆盖（很多新手不管三七二十一就直接把自己的部分push了上去或者未询问他人就直接覆盖了他的代码），强行覆盖会导致很严重的后果。

### 1.3 Installation

> 墙裂推荐git指令练习的网站：https://learngitbranching.js.org/

1. 进入[官网]()找到相关链接下载并安装

   * [windows](https://git-scm.com/download/win)
   * [macOS](https://git-scm.com/download/mac)
   * [Linux](https://git-scm.com/download/linux)

2. 除了`linux`下的安装外，`windows`和`macOS`都是下载安装包之后直接按步骤安装即可。

   >  这里推荐在你的计算机的根目录或者合适的地方创建一个`development`文件夹，然后在`development` 下创建一个`development_tools`用来安装所有相关工具，同时创建一个`development_env`来存放后续可能需要的各种包。这样子可以方便管理你的工具、环境以及后续的代码。

   将其安装到`development_tools`下：

   ```bash
   └─ {Your Path}
       └─ development
           ├─ development_tools #这个文件夹专门用来安装各种安装工具
           ├─ development_codes #这个文件夹用来存放开发用的代码
           └─ development_env #这个文件夹专门存放各种开发用的开发环境
   ```

   

3. 在安装完git之后，比较推荐的一种使用方式是通过git远程托管代码到git服务器上，这样有利于协作。但是如果你只是单纯想进行版本控制也可以安装在本地（甚至不需要安装，目前很多IDE都自带了版本控制的功能，比如`IDEA`等一系列IDE。

   通过git远程托管代码到git服务器上也有多种方式，我比较推荐的是<u>**将代码托管到Github**</u>，因为其比较简单易用,详见[github的使用](#github)。但是如果对代码安全等有较高要求或者想自己部署一个git服务器，可以尝试自己部署一个GitLab服务，详见[gitlab的搭建和使用](#gitlab)。

### 1.4 Git command

不管是通过`github` 还是自己搭建一个`gitlab`,都需要了解git相关的command。

> 再次墙裂推荐git指令练习的网站：https://learngitbranching.js.org/
>
> 把这个通关基本上关于git命令的理解就没啥大问题了。

可以在[Appendix1](#appendix1)中查看git的常用command（不推荐死记硬背，需要的时候查阅或者Google就行，这里只是让你知道git是有这些功能的）。



## 2. GitHub

<a name="github"></a>

### 2.1 What is GitHub?

GitHub是21世纪最大的~~同性交友~~开源代码托管平台，你可以在上面找到来自全世界的小伙伴们开发的有趣的插件、工具等。自从被巨硬收购后貌似变得更好用了？

### 2.2 How to host your code on GitHub?

* 点击 "New" 按钮

![Click on "new" button](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/image-20210816173214826.png)

* 创建项目仓库（Repositories)

  这里有几点需要说明的是：

  * `Repository name`是必填，`Description` 是选填并且会显示在你的`README.md`文件中
  * 根据你自己的项目属性来选择`Public`还是`Private`属性
  * `Add a README file`：一般都勾选，自动生成`README.md`文件，省的自己创建
  * `Add .gitignore` ：根据需求，如果有不需要上传到git服务器的文件或者过于巨大的文件需要勾选，凡是文件名出现在这个文件内的文件，git会自动忽视它（即任何和它相关的记录都不会有）
  * `Choose a license`：根据需求以及你的Repository的性质来决定是否需要license以及需要什么种类的license

![Details setting](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/image-20210816180400641.png)

* 在GitHub上完成创建

![Create Successfully](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/image-20210816183300693.png)

* 点击`code`,复制HTTPS下的Clone链接

![copy url](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/image-20210816183508954.png)

* 选择合适的文件夹，右键选择`Git Bash Here`，会弹出一个命令行界面

![Click on Git Bash](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/image-20210816184515375.png)



* 输入comand，回车之后clone完成

```bash
$ git clone { your link }
```

![clone](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/image-20210816184820947.png)

> 需要注意的是，如果你是第一次使用git，需要登录一下github的账号作为验证。

* clone完成

![clone successfully](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/image-20210816185604117.png)



* Clone 完成之后，你就可以开始和你的队友一起合作写代码了。你们可以选择都在`master`分支下工作（如果2-4人确实这样比较方便）但是我比较推荐去尝试下一人一个分支（`branch`) 去工作，每个任务完成了再合并该部分到`master`分支上，这样比较符合真实开发环境。
* 将代码push到github的步骤：

```bash
# 将修改提交到暂存区,这里的'.'代表将所有发生了修改的文件都添加进去
# 如果不想全部添加也可以用{文件名}替代 '.'
$ git add .

# 将修改提交到本地分支
# 这里的-m {message} 代表的是此次提交的说明，你也可以下载一个插件来自动生成非常详细的说明
$ git commit -m '{your message}'

# 将修改push到远程
$ git push
```

> 这里只是最简单但也最常用的一种提交，更多更复杂的git操作请移步墙裂推荐的网站https://learngitbranching.js.org/ 学习。



## 3. Tortoise

<a name="tortoise"></a>

### 3.1 What is Tortoise?

`小乌龟`是我最喜欢的git UI（没有之一！）

[官网链接](https://tortoisegit.org/)

如果你觉得用`Git Bash`太麻烦，而`Git GUI`又非常不好用，那么我非常推荐你尝试一下`Tortoise`这款软件。

### 3.2 Install Tortoise

* 进入官网，选择下载（截至2021/8/17, `tortoise`的中文语言包也已经开发完毕，如果需要可以自行下载解压，但是不推荐）

![Tortoise Website](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/image-20210817014618548.png)

* 一路默认安装到头即可(安装路径不一定要在C盘，推荐将其安装在`development/development_tools`中方便管理)
* 安装完毕，即可在右键中看到`小乌龟`集成的相关选项，其中git的所有功能都被用图形用户界面表示了出来，哪怕不记得命令也能很轻松得进行版本控制了。

![Tortoise shown on right-click menu](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/image-20210817015139330.png)



## 4. GitLab

<a name="gitlab"></a>

> ref: [linux中国](https://zhuanlan.zhihu.com/p/40371444)

### 4.1 What is GitLab and Why choose GitLab ?

GitHub非常简单易用，不幸的是，尽管 GitHub 是全球最大的代码托管商，但是它自己的软件别人却无法使用。因为它不是开源的，所以你不能获取源代码并编译你自己的 GitHub。所以你无法下载 GitHub 并在您自己的服务器上编译并运行它，也意味着无法定制化开发自己需要的功能。

万一你想要私有仓库或需要更多权限控制，最好的方法就是在你的服务器上运行 Git。不仅你能够省去一笔钱，你还能够在你的服务器有更多的操作。在大多数情况下，大多数高级 Linux 用户已经拥有自己的服务器并在其中部署了属于自己的Git服务。

GitLab 是一个非常优秀的开源项目，用户可以直接获取到源代码，使得用户可以在自己的服务器上运行类似于 GitHub 的项目管理系统。我们可以使用 GitLab 为团队成员或公司运行类似于 GitHub 的服务（<u>更加安全且可以根据自己的需求定制化开发</u>）。GitLab 的一些功能包括：Git 仓库管理，代码评论，问题跟踪，活动源和维基。它配备了 GitLab CI，用于持续集成和交付。

### 4.2 How to set up personal GitLab?

首先我们需要有一台服务器作为我们的git服务器，然后在这台git服务器上安装git服务。安装完成之后再在git的基础上配置GitLab。

### 4.3 Install Git on Your Own Server（Linux）

> 下面这部分可能对于新手来说会有些复杂，这里只是介绍一下步骤，可以在熟悉linux之后再回过来实现一遍。

首先我们考虑一个简单案例，我们有一个远程服务器和一台本地服务器，现在我们需要使用这两台机器来工作。为了简单起见，我们就分别叫它们为远程服务器和本地服务器。

> GitLab部分所有服务器安装的系统都假设为Ubuntu,所以安装命令为`apt-get`。如果服务器为CentOS系统则使用`yum`安装。

* 首先，在两边的机器上安装 Git。你可以从依赖包中安装 Git，但我选择偷懒：

```bash
sudo apt-get install git-core
```

* 为 Git 创建一个用户。

```bash
sudo useradd git
passwd git
```

* 为了容易的访问服务器，我们设置一个免密 ssh 登录。首先在你本地电脑上创建一个 ssh 密钥：

```bash
ssh-keygen -t rsa
```

* 这时会要求你输入保存密钥的路径，这时只需要点击回车保存在默认路径。第二个问题是输入访问远程服务器所需的密码。它生成两个密钥——公钥和私钥。记下您在下一步中需要使用的公钥的位置。

* 现在您必须将这些密钥复制到服务器上，以便两台机器可以相互通信。在本地机器上运行以下命令：

```bash
cat ~/.ssh/id_rsa.pub | ssh git@remote-server "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

> 如果从来没有用过linux,对于上述命令可能会觉得非常陌生。但是问题不大，这里先复制，后面会有专门的章节介绍Linux。

* 现在，用 `ssh` 登录进服务器并为 Git 创建一个项目路径。你可以为你的仓库设置一个你想要的目录。

* 现在跳转到该目录中：

```bash
cd /home/swapnil/project-1.git
```

* 现在新建一个空仓库,仓库名可以根据自己的需求来取

```bash
git init --bare
Initialized empty Git repository in /home/swapnil/project-1.git
```

* 现在我们需要在本地机器上新建一个基于 Git 版本控制仓库：

```bash
mkdir -p /home/swapnil/git/project
```

* 进入创建仓库的目录：

```bash
cd /home/swapnil/git/project
```

* 现在在该目录中创建项目所需的文件。留在这个目录并启动 `git`：

```bash
git init
Initialized empty Git repository in /home/swapnil/git/project
```

* 把所有文件添加到仓库中：

```bash
git add .
```

* 现在，每次添加文件或进行更改时，都必须运行上面的 `add` 命令。 您还需要为每个文件更改都写入提交消息。提交消息基本上说明了我们所做的更改。

```bash
git commit -m "message" -a
[master (root-commit) 57331ee] message
 2 files changed, 2 insertions(+)
 create mode 100644 GoT.txt
 create mode 100644 writing.txt
```

> 在上面的命令中 `-a` 选项意味着提交仓库中的所有文件。 如果您只更改了一个，则可以指定该文件的名称而不是使用 `-a`。
>
> 举一个例子：
>
> ``` bash
> git commit -m "message" GoT.txt
> [master e517b10] message
>  1 file changed, 1 insertion(+)
> ```

* 到现在为止，我们一直在本地服务器上工作。现在我们必须将这些更改推送到远程服务器上，以便通过互联网访问，并且可以与其他团队成员进行协作。

```bash
git remote add origin ssh://git@remote-server/repo-<wbr< a="">>path-on-server..git
```

* 现在，您可以使用 `pull` 或 `push` 选项在服务器和本地计算机之间推送或拉取：

```bash
git push origin master
```

* 如果有其他团队成员想要使用该项目，则需要将远程服务器上的仓库克隆到其本地计算机上：

```bash
git clone git@remote-server:/home/swapnil/project.git
```

> 这里 `/home/swapnil/project.git` 是远程服务器上的项目路径，在你本机上则会改变。

* 然后进入本地计算机上的目录（使用服务器上的项目名称）：

```bash
cd /project
```

* 现在其他人可以编辑文件，写入提交更改信息，然后将它们推送到服务器：

```bash
git commit -m 'corrections in GoT.txt story' -a
```

* 然后推送改变：

```bash
git push origin master
```

### 4.4 配置使用 GitLab

> 这一部分是接在3.3 完成的基础上进行的，如果觉得3.3没有完成可以先跳过

* GitLab 为不同的操作系统提供了软件包。 在我们安装 GitLab 之前，您可能需要配置 SMTP 电子邮件服务器，以便 GitLab 可以在需要时随时推送电子邮件。官方推荐使用 Postfix。所以，先在服务器上安装 Postfix：

```bash
sudo apt-get install postfix
```

* 在安装 Postfix 期间， 如果你一不小心跳过了部分配置步骤，你可以使用这个命令来重新配置它：

```bash
sudo dpkg-reconfigure postfix
```

> 运行此命令时，请选择 “Internet Site”并为使用 Gitlab 的域名提供电子邮件 ID。

* 在剩下的步骤中，都选择默认选项。
* 我们使用 `wget` 来下载软件包(记得使用最新的GitLab的版本[GitLab官网](https://docs.gitlab.com/ee/install/)，不要直接复制)：

```bash
# 记得将链接替换为最新的
wget https://downloads-packages.s3.amazonaws.com/ubuntu-14.04/gitlab_7.9.4-omnibus.1-1_amd64.deb
```

* 然后安装：

```bash
sudo dpkg -i gitlab_7.9.4-omnibus.1-1_amd64.deb
```

* 配置并启动 GitLab 

```bash
sudo gitlab-ctl reconfigure
```

* 现在需要在配置文件中配置域名，以便您可以访问 GitLab。打开文件。

```bash
nano /etc/gitlab/gitlab.rb
```

* 在这个文件中编辑 `external_url` 并输入服务器域名。保存文件，然后从 Web 浏览器中打开新建的一个 GitLab 站点。

* 默认情况下，它会以系统管理员的身份创建 `root`，并使用 `5iveL!fe` 作为密码。 登录到 GitLab 站点，然后更改密码。
* 密码更改后，登录该网站并开始管理您的项目。



## Appendix1

#### 1 新建库

``` bash
# 在当前目录新建一个Git代码库
$ git init

# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]

# 下载一个项目和它的整个代码历史
$ git clone [url]
```

#### 2 配置git

Git的配置也可以直接操作`.gitconfig`文件，也可以通过命令来配置。

下面是通过命令配置：

``` bash
# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e [--global]

# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```

#### 3 增加或删除相关文件

``` bash
# 添加指定文件到暂存区
$ git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
$ git add [dir]

# 添加当前目录的所有文件到暂存区
$ git add .

# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现分次提交
$ git add -p

# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]
```

#### 4 提交代码

``` bash
# 提交暂存区到仓库区
$ git commit -m [message]

# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]

# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a

# 提交时显示所有diff信息
$ git commit -v

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...
```

#### 5 分支管理

``` bash

# 列出所有本地分支
$ git branch

# 列出所有远程分支
$ git branch -r

# 列出所有本地分支和远程分支
$ git branch -a

# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]

# 新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 新建一个分支，指向指定commit
$ git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
$ git checkout [branch-name]

# 切换到上一个分支
$ git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]

# 合并指定分支到当前分支
$ git merge [branch]

# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]
```

#### 6 标签管理

``` bash

# 列出所有tag
$ git tag

# 新建一个tag在当前commit
$ git tag [tag]

# 新建一个tag在指定commit
$ git tag [tag] [commit]

# 删除本地tag
$ git tag -d [tag]

# 删除远程tag
$ git push origin :refs/tags/[tagName]

# 查看tag信息
$ git show [tag]

# 提交指定tag
$ git push [remote] [tag]

# 提交所有tag
$ git push [remote] --tags

# 新建一个分支，指向某个tag
$ git checkout -b [branch] [tag]
```

#### 7 查看信息

``` bash

# 显示有变更的文件
$ git status

# 显示当前分支的版本历史
$ git log

# 显示commit历史，以及每次commit发生变更的文件
$ git log --stat

# 搜索提交历史，根据关键词
$ git log -S [keyword]

# 显示某个commit之后的所有变动，每个commit占据一行
$ git log [tag] HEAD --pretty=format:%s

# 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
$ git log [tag] HEAD --grep feature

# 显示某个文件的版本历史，包括文件改名
$ git log --follow [file]
$ git whatchanged [file]

# 显示指定文件相关的每一次diff
$ git log -p [file]

# 显示过去5次提交
$ git log -5 --pretty --oneline

# 显示所有提交过的用户，按提交次数排序
$ git shortlog -sn

# 显示指定文件是什么人在什么时间修改过
$ git blame [file]

# 显示暂存区和工作区的差异
$ git diff

# 显示暂存区和上一个commit的差异
$ git diff --cached [file]

# 显示工作区与当前分支最新commit之间的差异
$ git diff HEAD

# 显示两次提交之间的差异
$ git diff [first-branch]...[second-branch]

# 显示今天你写了多少行代码
$ git diff --shortstat "@{0 day ago}"

# 显示某次提交的元数据和内容变化
$ git show [commit]

# 显示某次提交发生变化的文件
$ git show --name-only [commit]

# 显示某次提交时，某个文件的内容
$ git show [commit]:[filename]

# 显示当前分支的最近几次提交
$ git reflog
```

#### 8 远程同步

``` bash
# 下载远程仓库的所有变动
$ git fetch [remote]

# 显示所有远程仓库
$ git remote -v

# 显示某个远程仓库的信息
$ git remote show [remote]

# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]

# 上传本地指定分支到远程仓库
$ git push [remote] [branch]

# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force

# 推送所有分支到远程仓库
$ git push [remote] --all
```

#### 9 撤销

``` bash

# 恢复暂存区的指定文件到工作区
$ git checkout [file]

# 恢复某个commit的指定文件到暂存区和工作区
$ git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
$ git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
$ git reset --keep [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]

# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop
```
