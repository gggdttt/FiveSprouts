# Intro

如果不喜欢将自己的博客放在GitHub上，也可以将其部署在自己的服务器中。

这节将简单介绍如何购买、管理自己的云服务器。

这里我就以亚马逊云服务为例了，各家的服务在大体上没有很大的差别。

### 1.注册

注册地址： https://portal.aws.amazon.com/billing/signup#/start

注册过程比较麻烦，需要用信用卡，中间的信息填写的内容也比较多，并且目前必须注册国际账号才能个人使用EC2，亚马逊的还专门打电话告知了我之后又发送了邮件告诉了我具体的注册方式。注册的密码必须是大小写字母加数字!

![AWS注册界面](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261655922.png)

### 2. 登陆

登陆时可使用账户别名或者账户ID，所以注册的信息记好，登陆时使用邮箱登陆或者用户名登陆即可。

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261656560.png" alt="登陆界面" style="zoom:67%;" />

### 3. 创建实例

1. 登陆之后，在控制面板页面左上角选择“服务”，可以查看亚马逊有哪些服务可以使用，因为我使用的是EC2，所以选择第一个EC2就可以了。

![image-20210826165653049](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261656105.png)

![管理平台](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261657726.png)

2. 选择EC2后，会跳转到EC2 Dashboard，左侧会显示和EC2相关的配置菜单，点击实例，可以查看当前创建好的实例，如果没有则是空。

![EC2 Dashboard](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261657938.png)

![单个实例](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261658959.png)

3. 点击“启动实例”按钮，会跳转到实例配置页面：

   **步骤1： 选择一个 Amazon 系统映像(AMI)**

   在这里可以选择实例的系统，如果是免费用户可以勾选“仅免费套餐”，然后选择实例系统，主要有Linux系统和Windows系统，包括Amazon Linux 2 AMI、SUSE Linux Enterprise Server 15、Red Hat Enterprise Linux 7.6、Ubuntu Server 18.04 LTS、Microsoft Windows Server 2016 Base等等，根据自己的喜好选择系统，系统也支持X86和ARM可供选择，点击选择可进入下一步。

​		**步骤 2: 选择一个实例类型**

在这里选择实例的配置，因为是免费账户，所以只能选择免费的实例，免费的套餐会有提示，配置是1个CPU，1G内存，点击下一步“配置实例详细信息”。

![image-20210826170020498](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261700547.png)

​		**步骤 3: 配置实例详细信息**

在这里可以配置创建时实例的一些信息，可以自己进行配置，也可以直接默认，点击下一步“添加新存储”。

![image-20210826170030119](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261700183.png)

​		**步骤 4: 添加存储**

该项中可以设置硬盘存储分区，默认只配置好了根目录，卷类型可以选择，也支持添加新卷，也可以选择默认。配置好之后点击下一步“添加标签”。

![image-20210826170101790](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261701843.png)

​		**步骤 5: 添加标签**

   该项中可以默认或者自定义标签，标签由区分大小写的键值对组成。例如，您可以定义一个键为“Name”且值为“Webserver”的标签。可将标签副本应用于卷和/或实例。也可以不进行配置，直接进行下一步“配置安全组”。

​		**步骤 6: 配置安全组**

   该项中，用于配置实例中安全组的信息，针对实例可以将实例加入已有的安全组或者是选择新建安全组， 安全组名称与描述可自定义，此处默认开启的是SSH类型，端口号是22，用于使用ssh方式登陆服务器。因为后面将要说到需要配置nginx，所以此处可以添加两个规则用于nginx访问：

点击添加http规则：类型选择HTTP，端口范围是80，可以是TCP，来源可以改为“任意位置”，描述可以自定义填写。

点击添加https规则：类型选择HTTPS，端口范围是443，可以是TCP，来源可以改为“任意位置”，描述可以自定义填写。

点击下一步“审核和启动”。

![配置安全组](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261707982.png)

​		**步骤 7: 核查实例启动**

该项中会显示之前几个步骤中配置的相关信息，确认无误后点击启动，会填出需要选择现有密钥对或着新密钥对，因为是新建的实例在这里是没有密钥对的，所以选择创建新密钥对，填写密钥对名称， 填写之后一定要下载密钥对，因为这个后面需要用到，并且这是没有下载的话后面就无法再获取到这个密钥对文件了，弹出框也会提示“创建文件后，您将无法再次下载该文件”。具体创建密钥对的其他方式后面再说。填写密钥对以后，点击“**启动实例**”按钮，会跳转到启动状态页面，点击右下角“**查看实例**”到EC2 dashboard查看创建的实例。

![image-20210826171002126](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261710210.png)

![创建密钥对](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261710153.png)

<img src="https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261710946.png" alt="image-20210826171042866" style="zoom:80%;" />

至此，实例已经创建完毕，后面就是做相关的配置。

### 4. 配置实例

在实例一栏中可以看见已经创建的实例，列表会显示实例ID、IPV4、实例状态、密钥名称等，也可以点击右上角小齿轮按钮，配置需要显示的列。

![需要配置的实例](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261711231.png)

#### 4.1 配置安全组

在创建实例的时候可能会忘记配置HTTP会其他相关的规则，则可以在这里重新进行配置

勾选要配置的安全组，在上面点击“操作”按钮，或者在要配置的安全组那一行右键都行，然后可以进行删除安全组、添加入站或者出站规则等，这里我们添加入站规则，即与上面的操作一样，如果不开启80或443端口的话，后面配置好nginx外部是无法访问的：

点击添加http规则：类型选择HTTP，端口范围是80，可以是TCP，来源可以改为“任意位置”，描述可以自定义填写。

点击添加https规则：类型选择HTTPS，端口范围是443，可以是TCP，来源可以改为“任意位置”，描述可以自定义填写。

#### **4.2 配置弹性IP**

在实例中会有公有IP和私有IP，弹性公网IP是一种NAT IP。它实际位于云服务提供商的公网网关上，通过NAT方式映射到了被绑定的云主机实例的私网网卡上。因此，绑定了弹性公网IP的云主机可以直接使用这个IP进行公网通信，但是在它的私网网卡上并不能看到这个IP地址。

点击“分配新地址”按钮，会跳到分配新地址页面，然后选择IPV4地址池，可以是自己配置，也可以选择亚马逊的自动分配的，这里选择亚马逊池，点击“分配”按钮，点击后就会显示分配好的弹性IP。

返回到弹性IP列表，选择刚配置好的弹性IP，此时是没有绑定实例和私有IP的，选中该列，点击上面的“操作”按钮，选择“关联地址”，跳转到关联地址页面，选择实例以及私有IP，实例就是刚才创建的实例，私有IP的查看也是在实例列表中，亚马逊也有提示：如果你将一个弹性 IP 地址与您的实例相关联，您目前的公有 IP 地址将被释放,选择好后点击关联，这样后面使用ssh连接实例时，就可以使用配置好的弹性IP地址。

![弹性IP](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261712287.png)

#### **4.3 密钥对配置**

刚才说到配置实例时需要选择密钥对，这里可以配置新密钥对，用于后续创建实例时使用，也可以导入密钥对。

点击左侧“密钥对”选项，跳转到密钥对列表，点击左上角“创建密钥对”按钮，输入密钥对名称，点击”创建“，之后密钥对会自动下载到本地，用于之后登陆时使用。

需要删除密钥对的话直接右键删除或者勾选之后点击删除即可。

至此，实例的一些常用基本配置就已经完成了，接下来在本地对已经启动的实例进行连接。

### 5.连接

我个人比较推荐的连接工具是[XShell](https://www.netsarang.com/zh/xshell/)。学生邮箱可以白嫖。

下载安装好XShell(傻瓜式安装即可)，启动：

![img](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261714465.png)

名称：随便起一个比如“aws001”。

主机：填EC2实例给的公有IP。

端口：22。

![img](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261716753.png)

在用户身份验证里把方法改为“Public Key”，用户名如果是按照网上教程里推荐的那样选的是 Ubuntu 系统，那这里就填“ubuntu”，其他的可以试试“ec2-user”，“root”或者其他系统名。

用户密钥里选择在启动实例时提示下载的.pem密钥文件，然后确定，即可成功连接。

### 6. 安装Nginx

因为使用的是ubuntu，则可以直接使用命令进行安装：sudo apt-get install nginx

安装完成后，启动nginx，执行命令：sudo service nginx start

检测nginx在服务器是否安装成功，执行命令：curl http://localhost

如果出现：如下图则表示安装成功。

![success](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261718279.png)

下面尝试进行访问，直接在PC浏览器地址栏输入弹性IP地址，如果出现下图则表示nginx安装成，并且服务器实例已经可用，后续就可以在服务器部署相关程序。

![image-20210826171905685](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108261719726.png)


