这里我就以亚马逊云服务为例了，其实各家的服务

### 注册

注册地址： https://portal.aws.amazon.com/billing/signup#/start

注册过程比较麻烦，需要用信用卡，中间的信息填写的内容也比较多，并且目前必须注册国际账号才能个人使用EC2，亚马逊的还专门打电话告知了我之后又发送了邮件告诉了我具体的注册方式。注册的密码必须是大小写字母加数字!

![img](https:////upload-images.jianshu.io/upload_images/6763420-2fa049239c3e723c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

### 登陆

登陆地址： [https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fconsole%2Fhome%3Fstate%3DhashArgs%2523%26isauthcode%3Dtrue&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fhomepage&forceMobileApp=0](https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fconsole%2Fhome%3Fstate%3DhashArgs%23%26isauthcode%3Dtrue&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fhomepage&forceMobileApp=0)

登陆时可使用账户别名或者账户ID，所以注册的信息记好，登陆时使用邮箱登陆或者用户名登陆即可。

![img](https:////upload-images.jianshu.io/upload_images/6763420-ab6e9f25fae8dc5b.png?imageMogr2/auto-orient/strip|imageView2/2/w/742/format/webp)

### 创建实例

1、登陆之后，在控制面板页面左上角选择“服务”，可以查看亚马逊有哪些服务可以使用，因为我使用的是EC2，所以选择第一个EC2就可以了。

![img](https:////upload-images.jianshu.io/upload_images/6763420-52d5b5f03b5760b0.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

![img](https:////upload-images.jianshu.io/upload_images/6763420-fca385ff77418328.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

2、选择EC2后，会跳转到EC2 Dashboard，左侧会显示和EC2相关的配置菜单，点击实例，可以查看当前创建好的实例，如果没有则是空。

![img](https:////upload-images.jianshu.io/upload_images/6763420-c6e7f4bcd44ab6c7.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

![img](https:////upload-images.jianshu.io/upload_images/6763420-0d5e7f4dd358e2ca.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

   3、点击“启动实例”按钮，会跳转到实例配置页面：

步骤 1: 选择一个 Amazon 系统映像(AMI)

   在这里可以选择实例的系统，如果是免费用户可以勾选“仅免费套餐”，然后选择实例系统，主要有Linux系统和Windows系统，包括Amazon Linux 2 AMI、SUSE Linux Enterprise Server 15、Red Hat Enterprise Linux 7.6、Ubuntu Server 18.04 LTS、Microsoft Windows Server 2016 Base等等，根据自己的喜好选择系统，系统也支持X86和ARM可供选择，点击选择可进入下一步。

步骤 2: 选择一个实例类型

在这里选择实例的配置，因为是免费账户，所以只能选择免费的实例，免费的套餐会有提示，配置是1个CPU，1G内存，点击下一步“配置实例详细信息”。

![img](https:////upload-images.jianshu.io/upload_images/6763420-873ed4eedad5bf4a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

步骤 3: 配置实例详细信息

在这里可以配置创建时实例的一些信息，可以自己进行配置，也可以直接默认，点击下一步“添加新存储”。

![img](https:////upload-images.jianshu.io/upload_images/6763420-143a43475aa56e2d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

步骤 4: 添加存储

该项中可以设置硬盘存储分区，默认只配置好了根目录，卷类型可以选择，也支持添加新卷，也可以选择默认。配置好之后点击下一步“添加标签”。

![img](https:////upload-images.jianshu.io/upload_images/6763420-99bbae27a2ab1572.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

步骤 5: 添加标签

   该项中可以默认或者自定义标签，标签由区分大小写的键值对组成。例如，您可以定义一个键为“Name”且值为“Webserver”的标签。可将标签副本应用于卷和/或实例。也可以不进行配置，直接进行下一步“配置安全组”。

步骤 6: 配置安全组

   该项中，用于配置实例中安全组的信息，针对实例可以将实例加入已有的安全组或者是选择新建安全组， 安全组名称与描述可自定义，此处默认开启的是SSH类型，端口号是22，用于使用ssh方式登陆服务器。因为后面将要说到需要配置nginx，所以此处可以添加两个规则用于nginx访问：

点击添加http规则：类型选择HTTP，端口范围是80，可以是TCP，来源可以改为“任意位置”，描述可以自定义填写。

点击添加https规则：类型选择HTTPS，端口范围是443，可以是TCP，来源可以改为“任意位置”，描述可以自定义填写。

点击下一步“审核和启动”。

![img](https:////upload-images.jianshu.io/upload_images/6763420-33c1d1e8da44f6f3.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

步骤 7: 核查实例启动

该项中会显示之前几个步骤中配置的相关信息，确认无误后点击启动，会填出需要选择现有密钥对或着新密钥对，因为是新建的实例在这里是没有密钥对的，所以选择创建新密钥对，填写密钥对名称， 填写之后一定要下载密钥对，因为这个后面需要用到，并且这是没有下载的话后面就无法再获取到这个密钥对文件了，弹出框也会提示“创建文件后，您将无法再次下载该文件”。具体创建密钥对的其他方式后面再说。填写密钥对以后，点击“启动实例”按钮，会跳转到启动状态页面，点击右下角“查看实例”到EC2 dashboard查看创建的实例。

![img](https:////upload-images.jianshu.io/upload_images/6763420-1e48544cede21947.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

![img](https:////upload-images.jianshu.io/upload_images/6763420-9cc8fc4d0dbbac7a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

![img](https:////upload-images.jianshu.io/upload_images/6763420-67426ca23f0bf728.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

至此，实例已经创建完毕，后面就是做相关的配置。

### 配置实例

1、在实例一栏中可以看见已经创建的实例，列表会显示实例ID、IPV4、实例状态、密钥名称等，也可以点击右上角小齿轮按钮，配置需要显示的列。

![img](https:////upload-images.jianshu.io/upload_images/6763420-9631c50259ca8f80.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

2、配置安全组

在创建实例的时候可能会忘记配置HTTP会其他相关的规则，则可以在这里重新进行配置

勾选要配置的安全组，在上面点击“操作”按钮，或者在要配置的安全组那一行右键都行，然后可以进行删除安全组、添加入站或者出站规则等，这里我们添加入站规则，即与上面的操作一样，如果不开启80或443端口的话，后面配置好nginx外部是无法访问的：

点击添加http规则：类型选择HTTP，端口范围是80，可以是TCP，来源可以改为“任意位置”，描述可以自定义填写。

点击添加https规则：类型选择HTTPS，端口范围是443，可以是TCP，来源可以改为“任意位置”，描述可以自定义填写。

3、配置弹性IP

在实例中会有公有IP和私有IP，弹性公网IP是一种NAT IP。它实际位于云服务提供商的公网网关上，通过NAT方式映射到了被绑定的云主机实例的私网网卡上。因此，绑定了弹性公网IP的云主机可以直接使用这个IP进行公网通信，但是在它的私网网卡上并不能看到这个IP地址。

点击“分配新地址”按钮，会跳到分配新地址页面，然后选择IPV4地址池，可以是自己配置，也可以选择亚马逊的自动分配的，这里选择亚马逊池，点击“分配”按钮，点击后就会显示分配好的弹性IP。

返回到弹性IP列表，选择刚配置好的弹性IP，此时是没有绑定实例和私有IP的，选中该列，点击上面的“操作”按钮，选择“关联地址”，跳转到关联地址页面，选择实例以及私有IP，实例就是刚才创建的实例，私有IP的查看也是在实例列表中，亚马逊也有提示：如果你将一个弹性 IP 地址与您的实例相关联，您目前的公有 IP 地址将被释放,选择好后点击关联，这样后面使用ssh连接实例时，就可以使用配置好的弹性IP地址。

![img](https:////upload-images.jianshu.io/upload_images/6763420-03b5564cf2174ce9.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

4、密钥对配置

刚才说到配置实例时需要选择密钥对，这里可以配置新密钥对，用于后续创建实例时使用，也可以导入密钥对。

点击左侧“密钥对”选项，跳转到密钥对列表，点击左上角“创建密钥对”按钮，输入密钥对名称，点击”创建“，之后密钥对会自动下载到本地，用于之后登陆时使用。

需要删除密钥对的话直接右键删除或者勾选之后点击删除即可。

   至此，实例的一些常用基本配置就已经完成了，接下来在本地对已经启动的实例进行连接。

### 连接

   本地连接实例时比较麻烦，在实例里表中，勾选想要连接的实例，会弹出连接实例的方式：一种是使用SSH客户端本地连接，一种是直接从浏览器连接但是需要安装Java，这里使用SSH客户端本地进行连接。

官方给的建议是使用PuTTY进行连接，但是Mac安装PuTTY特别麻烦，并且需要安装一些其他的东西，过于繁琐，这里我使用了SecureCRT进行连接，下载地址：https://www.vandyke.com/products/securecrt/

同时官方文档也给出其他连接方式，但是比较恶心的是，文档内容特别多，在这里我们只看使用[SSH方式连接AWS EC2 Linux版本实例](https://docs.aws.amazon.com/zh_cn/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)，这是官方给的方式，下面我写出来的是已经填过的坑，连接实例是有先决条件的，就是以下七条：

1、安装SSH客户端

2、安装 AWS CLI 工具

3、获得实例的 ID

4、获得实例的公有 DNS 名称

5、查找私有密钥并验证权限

6、获取用于启动实例的 AMI 的默认用户名称

7、允许从您的 IP 地址到您的实例的入站 SSH 流量

1、安装SSH客户端

   一般Mac有自带的终端就可以使用，但是因为首次连接的使用是使用密钥文件进行连接的，所以终端工具不是很方便了，刚才推荐的secureCRT，可以使用密钥对文件转换后的密进行服务器登陆，需要将下载下来的密钥对文件即后缀名为pem的文件转为密钥即可。

安装secureCRT，Mac傻瓜式安装；

转换密钥对文件为密钥串：

使用命令：ssh-keygen -y -f XXX.pem > XXX.pem.pub转换为密钥串文件；

使用命令：cat XXX.pem.pub查看密钥串；

配置secureCRT

点击 + 新建，会弹出对话框，直接continue； 

![img](https:////upload-images.jianshu.io/upload_images/6763420-79116267edb8f401.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

port和firewall默认，填写hostname，即上面说到的弹性IP，填写username，即用于启动实例的 AMI 的默认用户名称，如果是ubuntu的系统，则username就是ubuntu，其他的后面会说到；

点击continue，sessionname和description随便填，就是到时候显示的名称而已；点击done

列表会显示创建好的项，显示的名字就是刚才sessionname，右键，点击Properties；

在弹出框中选择SSH2，在右面的Authentication区域，如果Properties不可点击，则先点击一下左边的列表区域中的Public Key，再点击Properties； 

![img](https:////upload-images.jianshu.io/upload_images/6763420-2b2d3ad96741b48f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

在弹出框中，上方勾选“use session public key setting”，然后在下面的Use identity or certificate file中填写刚才使用命令生成的密钥串的那堆字符 

![img](https:////upload-images.jianshu.io/upload_images/6763420-6df47657885cb850.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

点击ok关闭对话框即可。

至此，secureCRT配置完毕，即SSH客户端配置完毕；使用Windows的朋友可以使用Xshell连接。现在还无法连接AWS，还需要接下来的几步。

2、安装 AWS CLI 工具

首先是官方文档：https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/cli-chap-install.html，然后填坑。官方推荐Mac使用pip安装，并且又有一堆先决条件，但是问题又有一堆，需要的东西比较复杂，下面的是比较简单快捷的方式：

安装Python，Mac有自带的Python不需要再次进行安装，版本需要在2.6.5以上，Windows用户请自行安装；

安装 AWS CLI

下载 AWS CLI 捆绑安装程序,在终端执行命令：curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"

解压缩程序包(必须安装了unzip)：unzip awscli-bundle.zip

运行安装程序： sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws

查看是否安装成功：aws --version出现版本号则表示安装成功

  这种方式是Mac下最简单的了，如果使用pip安装，则aws --version命令会提示无效，则需要配置环境变量，主要是配置python的环境变量，比较麻烦，所以上面的方式是比较简单的。

配置 AWS CLI

配置AWS CLI使用 aws configure命令比较快捷

获取 IAM 用户的访问密钥 ID 和秘密访问密钥。

  访问密钥包含访问密钥 ID 和秘密访问密钥，用于签署对 AWS 发出的编程请求。如果没有访问密钥，您可以使用AWS 管理控制台进行创建。建议您使用 AWS 账户根用户 访问秘钥而不是使用 IAM 账户根用户访问秘钥。IAM 让您可以安全地控制对您的 AWS 账户中 AWS 服务和资源的访问。

  仅当创建访问密钥时，您才能查看或下载秘密访问密钥。以后您无法恢复它们。不过，您随时可以创建新的访问密钥。您还必须拥有执行所需 IAM 操作的权限。有关更多信息，请参阅 IAM 用户指南 中的访问 IAM 资源所需的权限。

1、打开 [IAM 控制台](https://console.aws.amazon.com/iam/home?#home)。

2、在控制台的导航窗格中，选择 Users。

3、选择您的 IAM 用户名称（而不是复选框）。

4、选择安全证书选项卡，然后选择创建访问秘钥。

5、要查看新访问秘钥，请选择显示。

在终端执行：aws configure会出现下面的内容，根据提示进行填写：

1、AWS Access Key ID [None]: 访问密钥 ID，在实例列表查看

例： AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE

2、AWS Secret Access Key [None]：私有访问密钥

例：AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

3、Default region name [None]：默认登陆名称，ubuntu系统为ubuntu

例：Default region name [None]: us-west-2

4、Default output format [None]：默认输出文件类型，json或text

例：Default output format [None]: json

至此，AWS CLI基本配置完成，如果要进行更多配置，可查看[官方文档](https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/cli-chap-configure.html)

3、获得实例的 ID

   在EC2 dashboard，实例列表中查看实例ID；

4、获得实例的公有 DNS 名称

   在EC2 dashboard，实例列表中查看实例的公有 DNS 名称，列名为“公有DNS（IPv4）”；

5、查找私有密钥并验证权限

这步就是使用后缀名为pem的文件进行登陆系统，前面已经配置了secureCRT就不需要这步了；

如果还需要的话，则使用下面的方式进行：

您的密钥必须不公开可见，SSH 才能工作。如果需要，请使用此命令：

chmod 400 xxxx.pem

通过其 公有 DNS 连接到您的实例，如实例为：ec2-13-59-115-229.us-east-2.compute.amazonaws.com

则使用命令：ssh -i xxxx.pem 服务器用户名@ec2-13-59-115-229.us-east-2.compute.amazonaws.com

6、获取用于启动实例的 AMI 的默认用户名称

对于 Amazon Linux 2 或 Amazon Linux AMI，用户名称是 ec2-user。

对于 Centos AMI，用户名称是 centos。

对于 Debian AMI，用户名称是 admin 或 root。

对于 Fedora AMI，用户名为 ec2-user 或 fedora。

对于 RHEL AMI，用户名称是 ec2-user 或 root。

对于 SUSE AMI，用户名称是 ec2-user 或 root。

对于 Ubuntu AMI，用户名称是 ubuntu。

另外，如果 ec2-user 和 root 无法使用，请与 AMI 供应商核实。

7、允许从您的 IP 地址到您的实例的入站 SSH 流量

   该步骤即配置安全组，开放22端口用于SSH连接，80或443用于访问Web程序；

8、使用secureCRT连接实例服务器

在secureCRT中直接点击要登陆的服务器，如下则表示登陆成功：

![img](https:////upload-images.jianshu.io/upload_images/6763420-7a76da56e5d20fc4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

至此，实例已经全部配置完成，并成功连接。

### 安装Nginx

因为使用的是ubuntu，则可以直接使用命令进行安装：sudo apt-get install nginx

安装完成后，启动nginx，执行命令：sudo service nginx start

检测nginx在服务器是否安装成功，执行命令：curl http://localhost

如果出现：如下图则表示安装成功。

![img](https:////upload-images.jianshu.io/upload_images/6763420-ce846032139bea08.png?imageMogr2/auto-orient/strip|imageView2/2/w/556/format/webp)

参考：https://www.jianshu.com/p/a43c3e0f0127

访问

直接在PC浏览器地址栏输入弹性IP地址，如果出现下图则表示nginx安装成，并且服务器实例已经可用，后续就可以在服务器部署相关程序。

![img](https:////upload-images.jianshu.io/upload_images/6763420-b28f9acd610c5d62.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

### 其他

删除实例：在实例列表要删除的实例上右键-实例状态-终止即可，终止即代表删除实例，但是不会马上删除，一般十分钟左右会自动销毁。

![img](https:////upload-images.jianshu.io/upload_images/6763420-286a9c9540a638d1.png?imageMogr2/auto-orient/strip|imageView2/2/w/1166/format/webp)

设置服务器root密码：

使用命令：sudo passwd root

会提示需要输入两次密码，输入即可

设置服务器用户密码：

设置完密码后，后续登陆可以直接使用终端或者其他SSH客户等登录，使用弹性IP地址、用户名、密码即可

切换到root用户，使用命令passwd 你的用户名

输入两次密码即可

### 总结

总的来说，配置的流程相对比较繁琐，尤其是配置AWS CLI里面内容比较多，官方文档给的也比较多，如果疑问，请联系[bo.wang1016@outlook.com](mailto:bo.wang1016@outlook.com)

### 参考

AWS Command Line Interface介绍：https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/cli-chap-welcome.html

Amazon EC2 实例介绍：https://docs.aws.amazon.com/zh_cn/AWSEC2/latest/UserGuide/Instances.html

AWS EC2搭建web服务器：https://www.jianshu.com/p/a43c3e0f0127



作者：恪晨
链接：https://www.jianshu.com/p/a045d4217175
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。