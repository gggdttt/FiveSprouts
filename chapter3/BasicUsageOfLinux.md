# 1. 常用命令总结

ubuntu命令很多，要熟练运用所有的命令有一定的难度。但大部分情况下，我们只要掌握常用的那些就够了，剩下的在必要时查资料即可。下面就对我们平时常用的ubuntu命令进行简单介绍:

## 1.1 文件/文件夹管理

- `ls` 列出当前目录下的所有文件（不显示隐藏文件）
- `ls -a` 列出当前目录下的所有文件（显示隐藏文件）
- `ls -l`列出当前目录下所有文件的详细信息
- `cd` 或者 `cd ~`进入用户主目录
- `cd ..` 回到上一级目录
- `cd -`返回进入此目录之前所在的目录
- `mkdir dirname` 新建目录
- `rmdir dirname` 删除空目录
- `rm filename` 删除文件
- `rm -rf dirname` 删除非空目录及其包含的所有文件
- `mv file1 file2`将文件1重命名为文件2
- `mv file1 dir1` 将文件1移动到目录1中
- `find 路径 -name “字符串”` 查找路径所在范围内满足字符串匹配的文件和目录

## 1.2 程序安装与卸载

- `apt-get` 程序安装与卸载命令的标志，需要管理员权限
- `install` 安装指定程序，举例：`sudo apt-get install vim`
- `remove` 卸载指定的程序，一般最好加上“--purge”执行清除
   式卸载；并在程序名称后添加*号。举例：`sudo apt-get remove --purge nvidia*`  卸载 nvidia 的驱动及其配置文件
- `update` 更新本地软件源文件，需要管理员权限，举例：`sudo apt-get update`

## 1.3 打包/解压

这里需要先解释几个参数：

| 参数 | 含义                       | 参数 | 含义                 |
| :--- | :------------------------- | :--- | :------------------- |
| -c   | 建立压缩档案               | -z   | 有gzip属性的         |
| -t   | 查看内容                   | -j   | 有bz2属性的          |
| -u   | 更新原压缩包中的文件       | -Z   | 有compress属性的     |
| -x   | 解压                       | -v   | 显示所有过程         |
| -r   | 向压缩归档文件末尾追加文件 | -O   | 将文件解开到标准输出 |

上表左边五个参数是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但只能用其中一个。右边五个参数是根据需要在压缩或解压时可选的。
下面进行举例说明:

 ### 1.3.1**压缩**

- `tar -cvf jpg.tar *.jpg` 将目录里所有jpg文件打包成tar.jpg
- `tar -czf jpg.tar.gz *.jpg`   将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz
- `tar -cjf jpg.tar.bz2 *.jpg` 将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2
- `tar -cZf jpg.tar.Z *.jpg`   将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z
- `rar a jpg.rar *.jpg` rar格式的压缩，需要先下载rar for linux
- `zip jpg.zip *.jpg` zip格式的压缩，需要先下载zip for linux

### 1.3.2 **解压**

- `tar -xvf file.tar` 解压 tar包
- `tar -xzvf file.tar.gz` 解压tar.gz
- `tar -xjvf file.tar.bz2`   解压 tar.bz2
- `tar -xZvf file.tar.Z`   解压tar.Z
- `unrar e file.rar` 解压rar
- `unzip file.zip` 解压zip

### 1.3.3 **总结**
 .tar 用 tar -xvf 解压
 .gz 用 gzip -d或者gunzip 解压
 .tar.gz和.tgz 用 tar -xzf 解压
 .bz2 用 bzip2 -d或者用bunzip2 解压
 .tar.bz2用tar -xjf 解压
 .Z 用 uncompress 解压
 .tar.Z 用tar -xZf 解压
 .rar 用 unrar e解压
 .zip 用 unzip 解压

## 1.4 用户管理

- `sudo useradd username` 创建一个新的用户username
- `sudo passwd username` 设置用户username的密码
- `sudo groupadd groupname` 创建一个新的组groupname
- `sudo usermod -g groupname username` 把用户username加入到组groupname中
- `sudo chown username:groupname dirname` 将指定文件的拥有者改为指定的用户或组

## 1.5 系统管理

- `uname -a` 查看内核版本
- `cat /etc/issue` 查看ubuntu版本
- `sudo fdisk -l` 查看磁盘信息
- `df -h` 查看硬盘剩余空间
- `free -m` 查看当前的内存使用情况
- `ps -A` 查看当前有哪些进程
- `kill 进程号`或者 `killall 进程名` 杀死进程
- `kill -9 进程号` 强制杀死进程



# 2.常用软件的安装

## 2.1安装Vim(必备)

```bash
sudo apt-get install vim  
```

## 2.2 安装Sublime Text 3

```bash
sudo add-apt-repository ppa:webupd8team/sublime-text-3    
sudo apt-get update    
sudo apt-get install sublime-text  
```

## 2.3 安装Chrome

```bash
$ wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
$ sudo apt-get install libappindicator1 libindicator7
$ sudo dpkg -i google-chrome-stable_current_amd64.deb 
$ sudo apt-get -f install
```

## 2.4 安装搜狗输入法

> 虽然自带的也还行

```bash
$ echo deb http://archive.ubuntukylin.com:10006/ubuntukylin trusty main | sudo tee /etc/apt/sources.list.d/ubuntukylin.list
$ sudo apt-get update   
$ sudo apt-get install sogoupinyin
```

> 注 : 如果提示没有公钥,无法验证下列数字签名 xxx
> 如果重启后只有搜狗输入法,则在命令行使用fcitx-configtool命令,添加系统输入法

```bash
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-　keys xxxx
$ sudo apt-get update
```

## 2.5 安装WPS Office（可选）

```bash
$ sudo apt-get install wps-office
```

## 2.6 安装git和vpnc（必备）

```bash
$ sudo apt-get install git vpnc
```

## 2.7 安装openssh-server（必备）

```bash
$ sudo apt-get install openssh-server
```

## 2.8 安装解压unrar

> 系统默认不带解压缩rar文件的功能，手动安装unrar程序

```bash
$ sudo apt-get install unrar
 解压命令:unrar x test.rar 
```

## 2.9 安装经典菜单指示器（可选）

```bash
sudo add-apt-repository ppa:diesch/testing  
sudo apt-get update  
sudo apt-get install classicmenu-indicator  
```

## 2.10 安装系统指示器SysPeek（可选）

```shell
sudo add-apt-repository ppa:nilarimogard/webupd8    
sudo apt-get update    
sudo apt-get install syspeek 
```

## 2.11 安装axel

> axel是Linux命令行界面的多线程下载工具，
> 比wget的好处就是可以指定多个线程同时在命令行终端里下载文件。
> 安装之后，就可以代替wget用多线程下载了。

```bash
sudo apt-get install axel  
```

## 2.12 ExFat文件系统驱动（虚拟机不需要）

> Ubuntu默认不支持exFat文件系统的挂载，需要手动安装exfat的支持
> 装上exfat-fuse之后就可以挂载exfat分区的磁盘了
> (比如你的U盘

```bash
sudo apt-get install exfat-fuse  
```

## 2.13 网易云音乐（狗头）

> 登陆官网下载Linux版本中的Ubuntu 16.04 64bit的deb包
>
> 然后输入安装命令

```bash
sudo apt-get install -f
sudo dpkj -i netease-cloud-music_1.0.0_i386_ubuntu14.04.deb
```

## 2.14 VLC视频播放器

```bash
sudo apt-get install vlc
```

## 2.15 wiznote(为知笔记)

```bash
sudo add-apt-repository ppa:wiznote-team 
sudo apt-get update 
sudo apt-get install wiznote
```

## 2.16 shutter（截图)

> 设置快捷键：
> 打开系统设置 -> 键盘 -> 快捷键 -> 自定义快捷键 -> 点击" + "
> 名字随便起，命令：shutter -s 点击确定，
> 再点禁用，键盘按下ctrl+alt+a，完成设置

```shell
sudo apt-get install shutter
```

> 话说，Snipaste都说了好几年的Linux版，官方是不是忘了这回事？

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261240871.png" alt="image-20210826124054787" style="zoom:50%;" />

## 2.17 BleachBit（系统清理软件）

```bash
sudo add-apt-repository ppa:n-muench/programs-ppa
sudo apt-get update 
sudo apt-get install bleachbit 
```



# 3. 其他推荐资料

*  [ArchWiki](https://wiki.archlinux.org/)
  这个虽然是ArchLinux这个发行版的维基百科，但是里面资料超多，基本上配置什么服务、出现什么疑难杂症，都可以参考ArchWiki。

* 《鸟哥的 Linux 私房菜》系列

  这本书就不用过多介绍了，最具知名度的Linux入门书《鸟哥的Linux私房菜》，全面而详细地介绍了Linux操作系统。如果想要学习Linux，必备入门书籍。唯一的缺陷可能是中文。

