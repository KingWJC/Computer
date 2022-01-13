### 文件系统

Filesystem Hierarchy Standard (文件系统层次化标准)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200811110506806.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JsYWNrX3p0NjY2,size_16,color_FFFFFF,t_70)

- /boot : 系统启动文件
- /home : 存放所有用户文件的根目录，是用户主目录的基点，比如用户user的主目录就是/home/user，可以用~user表示
- /root : 管理员目录
- /bin :  存放二进制可执行文bai件(ls,cat,mkdir等)，常用命令一般都在这里。
- /sbin : 管理命令.
- /lib : 库文件
- /dev : 设备文件，Linux中设备都是以文件形式出现，这里的设备可以是硬盘，键盘，鼠标，网卡，终端等。
- /etc : 系统管理和配置文件
- /tmp : 临时文件 /var/tmp
- /var : 可变化的文件，用于存放运行时需要改变数据的文件，也是某些大文件的溢出区，比方说各种服务的日志文件（系统启动日志等）
- /usr : 用于存放系统应用程序，
  - /usr/local 本地系统管理员软件安装目录（安装系统级的应用）
  - /usr/bin和/usr/sbin  安装后的应用程序的文件

和windows区别: 无盘符.

### 环境变量查看和设置。

Windows中，命令查找顺序：（也可以输入命令的绝对路径）

1. 是否windows内部命令。
2. 先在当前工作路径下寻找。
3. 最后在环境变量中Path中查找。

用户变量和系统变量

- 用户变量：当前用户所有，
- 系统变量：所有用户共有。
- 重要变量-Path：记录目录。

echo %path% —输出环境变量中Paht的内容，包含用户变量和系统变量。

### 软件安装

有四种方式，源码解压安装、rpm安装、yum安装

**CentOS 源使用帮助**:http://mirrors.ustc.edu.cn/help/centos.html

安装后，需要配置环境变量： vi /etc/profile  ， source /etc/profile

Redhat提供了rpm管理体系，包含已编译好的软件包，已包含依赖检查，但部分需要人为解决。

应用程序目录结构

![img](https://img-blog.csdnimg.cn/20190422182835310.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI0Mzk0MDkz,size_16,color_FFFFFF,t_70)

#### 源代码安装

源代码安装软件的优点：

-   可以获得最新的软件，及时修复bug；
-   根据用户的需求，灵活定制软件功能

编译源代码步骤

![img](https://img-blog.csdnimg.cn/20190421141838279.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI0Mzk0MDkz,size_16,color_FFFFFF,t_70)

configure、Makefile.in一般是项目管理器自动生成的，而gcc编译器需要安装，使用yum命令安装

1. 下载一个源码文件 Wget.

2. md5sum校验工具，进行完整性校验，防止源码包被别人串改，

   md5sum xxx.tar.gz

3. 解包将软件包释放到/usr/local/src/目录, tar.gz这样的压缩格式，要用tar命令来解压

   tar zxvf xxx.tar.gz -c /usr/local/src/

   ​         z：gz压缩格式，如果是bz2压缩格式，那么选项就需要用j

   ​         x：解压文件

   ​         v：详细的列出处理的文件

   ​         f：指定文件名

   ​		-C, 用来指定解压到哪个目录；

4. 转到源码位置：cd /usr/local/src/xxx

5. 配置：使用./configure，将软件安装到/usr/local/apache2目录中

   ./configre -prefix=/usr/local/apache2

6. 编译：make

7. 安装：make install

#### yum命令

Yum，基于RPM包管理，是一个shell前端包管理器。

1. yum源的配置，编辑 /etc/yum.repos.d/CentOS-Base.repo:

   1. [centos镜像-centos下载地址-centos安装教程-阿里巴巴开源镜像站](https://developer.aliyun.com/mirror/centos?spm=a2c6h.13651102.0.0.3e221b11DrEo5T)

      ```bash
      [base]
      name=CentOS-6
      failovermethod=priority
      baseurl=https://vault.centos.org/6.9/os/x86_64/
      gpgcheck=0
      ```

   2. 配置本地源：挂在DVD光驱，修改配置文件。

      name=local；baseurl：file:///mount；gpgcheck=1； onable=1;

      yum repolist

   3. 安装错误：YumRepo Error: All mirror URLs are not using ftp, http[s] or file.

      需要更换源。

2. yum  install  软件名 [-y]

   -y：如果使用-y，那么在安装软件时命令行就不会出现"Is this ok[y/N]"这条提醒语句了

3. yum clean all  清空yum源的缓存

4. yum clean metadata

5. yum update

6. yum remove 软件名

7. yum list installed  列出已安装的包

8. yum list   列出可安装的包

9. yum  groupinstall  组名   安装组套件

#### rpm 安装和卸载：

1. rpm -qa | grep pattern 查看已安装的软件名。

2. rpm -q，qi，ql，qc，qd 查询指定软件名。

   1. ​	-qa：查看系统中已安装的所有RPM软件包列表
   2. ​	-qi：查看指定软件的详细信息
   3. ​	-ql：查询指定软件包所安装的目录、文件列表
   4. ​	-qc：仅显示指定软件包安装的配置文件
   5. ​	-qd：仅显示指定软件包安装的文档文件

3. rpm -q —scripts packagename  | more .使用管道查询指定软件名的脚本

4. rpm -ivh —prefix path rpmpackagename  安装

   ​	-i：安装一个新的rpm软件包

   ​	-h：以“#”号显示安装的进度

   ​	-v：显示安装过程中的详细信息

5. rpm -e packagename  卸载

6. 安装后，自动配置环境变量：在user/bin 下有文件链接到真实的安装目录。

7. 查询命令的安装包：type ifconfig.；rpm -qf /sbin/ifconfig；在安装的记录中根据安装路径查找。


#### 从Linux的完整安装包中安装:

- 虚拟机中CD/DVD配置中，选择Linux-dvd版本。
- cd /dev ；mount cdrom /mnt；将光驱挂载的mnt目录下。 cd /mnt/Packages；选择安装。
- df -h 查看磁盘挂载情况。

#### DNF

yum 已经被 dnf 取代，dnf 是它的一个现代化的分支，它保留了大部分 yum的接口。

### yum安装运行报错.

- Errors during downloading metadata for repository 'appstream' Error : Failed to download metadata for repo 'AppStream' ; Error: Failed to download metadata for repo 'epel': Cannot download repomd.xml: Cannot download repodata/repomd.xml: All mirrors were tried Could not resolve host: mirrors.aliyun.com
- 原因 : CenOS 8 下, 网络配置未添加DNS
- 编辑/etc/sysconfig/network-scripts/ens33文件, DNS1=114.114.114.114,
- 重载配置文件: nmcli c reload ens33; 重启网卡: nmcli c up ens33.

