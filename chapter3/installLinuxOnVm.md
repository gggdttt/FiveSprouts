# Intro

说了再多也不如自己上手尝试一下linux。这里我们通过虚拟机软件来模拟安装一个在win10下运行的linux（虚拟机软件可以看作是一个平台，win10运行这个软件，然后这个软件来模拟运行linux）。

选择这种方式的主要原因是它的错误代价非常小，不需要重新安装整个系统，就算发生错误也只要删除对应的文件即可，不影响计算机的正常使用。

# **VMware安装ubuntu**

## 1. 准备工作

首先需要下载VMware Workstation Pro 12虚拟机，可以自行在百度上寻找密钥。

然后需要下载ubuntu镜像，我这里下载的是Ubuntu 18.04，下载地址在ubuntu官网：https://ubuntu.com/download/desktop。 下载最新的LTS版本就行（稳定版），不要下载beta版可能bug比较多。

![image-20210826114245545](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261142615.png)

## 2. **VMware安装ubuntu**

* 打开安装好的虚拟机，创建新的虚拟机：

![image-20210826114332010](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261143111.png)

* 选择自定义：

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261144828.png" alt="image-20210826114402756" style="zoom:50%;" />

* 兼容性选择如下：

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261144708.png" alt="image-20210826114448648" style="zoom:67%;" />



* 接下来这步一定<u>**先不要选择**</u>下载的光盘映像文件，如果选了点`下一步`系统会就直接帮你配置好后面的配置。因为我们要根据自己的要求进行选择配置我们的虚拟机，所以我们这步选择`稍后安装操作系统`。



<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261146111.png" alt="image-20210826114640037" style="zoom:67%;" />

* 选择操作系统为`Linux`

![image-20210826114751268](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261147310.png)

* 为虚拟机命名，将其路径改为合适的位置（`development_tool`)

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261148231.png" alt="image-20210826114828171" style="zoom:67%;" />

* 配置处理器

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261149335.png" alt="image-20210826114927291" style="zoom:67%;" />

* 这步配置我们给虚拟机的内存，这个要根据你自己的电脑配置给虚拟机分内存，内存分配得多虚拟机性能就优秀，但是别给虚拟机的内存太多以至于影响自己主系统的使用，我的电脑内存为16G，分给虚拟机2G。

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261150679.png" alt="image-20210826115036612" style="zoom:67%;" />

* 配置网络，选择使用网络地址转换：

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261151534.png" alt="image-20210826115100471" style="zoom:67%;" />

* 选择LSI logic IO控制器：

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261155134.png" alt="image-20210826115556089" style="zoom:50%;" />

* 选择磁盘类型：

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261156802.png" alt="image-20210826115618759" style="zoom:50%;" />

* 选择磁盘：

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261156519.png" alt="image-20210826115644462" style="zoom:50%;" />

* 给定容量（取决于你电脑的容量）：

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261157750.png" alt="image-20210826115713683" style="zoom:67%;" />

* 完成创建：

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261157428.png" alt="image-20210826115742365" style="zoom:67%;" />

* 在这里我们的虚拟机就创建完成了，下面我们打开虚拟机设置，设置安装镜像文件的位置（就是之前下载好的Ubuntu光盘映像文件）

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261158373.png" alt="image-20210826115811262" style="zoom:50%;" />

* 设置虚拟化：

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261159415.png" alt="image-20210826115921329" style="zoom:67%;" />



* 设置`iso`所在位置：

![设置镜像所在位置](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261200041.png)



* 设置好后我们点击“开启此虚拟机”，在正常安装系统时，我们要在BIOS里设置启动项，把电脑设置为光盘启动，但是我们这个虚拟机他可以自己识别这台虚拟机有没有安装系统，所以我们启动时他就可以直接进入系统安装界面。我们进入安装界面后我们选择“English”，点击install ubuntu进入下一步；

![image-20210826120150355](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261201421.png)

* 下面是键盘选择键盘选默认English（US）即可

![image-20210826120204891](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261202985.png)

* 下面这两个选项第一个我们选择的是**正常安装**，这个安装完后就是会出现一些系统自带的软件，我们不用的可以卸载掉，第二个选项是**最小安装**他只会安装我们系统要用到的基本工具。下面两个都勾选上下面第一个是安装ubuntu时下载更新，第二个为图形或无线硬件以及其他的媒体格式安装第三方软件。**我这里选择的是正常安装，如果想要一个纯净的系统就可以选择最小安装**。

![image-20210826120225160](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261202250.png)

* 给磁盘分区，我们进行手动分区，选择最后一个“something else”（其他选项）

![image-20210826120244143](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261202228.png)

* 进来后进行分区，先分一个启动分区boot，一般启动分区的大小为200M就可以了。boot包含了操作系统的内核和在启动系统过程中所要用到的文件，我们要给他单独分出来。要是不分当我们磁盘文件写满了磁盘空间不足时或者根分区出现问题了，我们的系统就没法启动，所以我们给他单独分出来。

![image-20210826120357262](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261203336.png)

* boot分区完成后我们分一个交换分区“swap”，swap分区，在我们windows中就是虚拟内存，在liunx中如果没有swap分区，系统内存用完后，系统他会杀死一部分程序，这个在我们使用中是非常可怕的，你正在用电脑，电脑内存满了，你的程序莫名其妙被关闭了，这是绝对不能允许的，所以我们必须分一个swap分区。还有一点swap分区的大小，一般我们电脑的内存小于等于4G时，swap我们给他是电脑内存的1.5-2倍；大于4G时，电脑内存多少swap就给多少。比如我的电脑内存为4G，那么swap我们就给6G=6144M-8G=8192；我的电脑内存为8G,16G...我们的swap就给8G,16G...跟我们电脑内存一样大就可以了。在这里因为是虚拟机我给swap分了2G。

![image-20210826120421021](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261204086.png)

* 下面我们给我们分最后一个分区“/”（根分区），根分区就是系统根目录所在的分区，一般根目录下面只有目录，不要直接有文件。Linux只有一个根目录，就是“/”，其它目录都是它的子目录。这里把剩下的磁盘空间都给“/”就可以了。（这里说一下"/home" 用户的home目录所在地，这个分区的大小取决于有多少用户。如果是多用户共同使用一台电脑的话，这个分区是完全有必要的，额外分割出/home有个最大的好处，当你重新安装系统时，你不需要特别去备份你的个人文件，只要在安装时，选择不要格式化这个分区，重新挂载为/home就不会丢失你的数据。因为我们是虚拟机，这台虚拟机是我们自己使用的，所以/home没有分出来，如果有需要可以自己分一个/home）

![image-20210826120442111](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261204178.png)

* 接下来就是输入你的位置，自行选择即可

* 下面是设置用户名密码，下面选择自动登录或者登陆时需要密码即可：

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261206424.png" alt="image-20210826120620334" style="zoom:67%;" />

* 以上配置好后就正是安装了，接下就是等待安装完成。
* 安装完成后会提示重新启动，启动后输入用户名密码就进入ubuntu桌面了。

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261207289.png" alt="image-20210826120756182" style="zoom:50%;" />

## 3. 安装VMware Tools工具

安装完linux之后，会发现有无法全屏的问题。这里需要安装VMware的工具来解决这个问题。

* 启动ubuntu，在VMware菜单栏 - 虚拟机 - 安装VMware Tools点击

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261211017.png" alt="image-20210826121145854" style="zoom:67%;" />

* 点击后，ubuntu桌面出现一个光盘文件，打开后

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261212655.png" alt="image-20210826121228569" style="zoom:67%;" />

* 我们把`VMware Tools-10.1.6-5214329.tar.gz`这个压缩包打开(版本号可能会有差异)

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261213290.png" alt="image-20210826121301211" style="zoom:67%;" />

* 点击提取，提取到home（主目录）下，方便寻找

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261213032.png" alt="image-20210826121319913" style="zoom:67%;" />

* 然后我们在ubuntu中打开终端，输入“ls”-->“cd vmware-tools-distrib”-->“sudo ./vmware-install.pl”

* 输入root密码，然后一路“y”加回车

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261213453.png" alt="image-20210826121354273" style="zoom:67%;" />

* 等待安装完成，然后重启ubuntu

### 设置共享文件夹

VMware Tools工具安装完成后就可以设置共享文件夹，共享文件夹可以帮助我们主机与虚拟机之间进行文件传输。在虚拟机设置中找到共享文件夹启用共享文件夹，然后添加文件夹目录：

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261216526.png" alt="image-20210826121609442" style="zoom:67%;" />

* 我们在ubuntu文件管理/mnt/hgfs中就是我们创建的共享文件夹，我们可以用这个来进行主机与虚拟机之间的文件传输。

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261216628.png" alt="image-20210826121627551" style="zoom:67%;" />

这里我们的ubuntu就安装完成了。

