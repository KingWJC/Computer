### 文件系统

Filesystem Hierarchy Standard (文件系统层次化标准)

- /boot : 系统启动文件
- /home : 用户目录
- /root : 管理员目录
- /bin : 可执行文件,用户命令.
- /sbin : 管理命令.
- /lib : 库文件
- /dev : 设备文件
- /etc : 配置文件
- /tmp : 临时文件 /var/tmp
- /var : 可变化的文件.

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

两种方式：手动编译源码和软件安装包。

Redhat提供了rpm管理体系，包含已编译好的软件包，已包含依赖检查，但部分需要人为解决。

rpm 安装和卸载：

rpm -qa | grep pattern 查看已安装的软件名。

rpm -q，qi，ql，qc，qd 查询指定软件名。

rpm -q —scripts packagename  | more .使用管道查询指定软件名的脚本

rpm -e packagename  卸载

rpm -ivh —prefix path rpmpackagename

安装后，自动配置环境变量：在user/bin 下有文件链接到阵势的安装目录。

查询命令的安装包：type ifconfig.；rpm -qf /sbin/ifconfig；在安装的记录中根据安装路径查找。

可以从Linux的完整安装包中安装:

- 虚拟机中CD/DVD配置中，选择Linux-dvd版本。
- cd /dev ；mount cdrom /mnt；将光驱挂载的mnt目录下。 cd /mnt/Packages；选择安装。
- df -h 查看磁盘挂载情况。

### Yum安装和配置

Yum，基于RPM包管理，是一个shell前端包管理器。

配置修改Yum源：阿里云中，下载新的 CentOS-Base.repo 到 /etc/yum.repos.d/

[centos镜像-centos下载地址-centos安装教程-阿里巴巴开源镜像站](https://developer.aliyun.com/mirror/centos?spm=a2c6h.13651102.0.0.3e221b11DrEo5T)

yum clean all ； yum makecache; 下载依赖关系

配置本地源：挂在DVD光驱，修改配置文件。

name=local；baseurl：file:///mount；gpgcheck=1； onable=1;

yum repolist

### yum安装包出错

- 错误：YumRepo Error: All mirror URLs are not using ftp, http[s] or file.
- 更换Yum 源镜像，编辑 /etc/yum.repos.d/CentOS-Base.repo
- 删除所有，并用下面代码替换。

```sql
[base]
name=CentOS-6
failovermethod=priority
baseurl=https://vault.centos.org/6.9/os/x86_64/
gpgcheck=0
```

### DNF

yum 已经被 dnf 取代，dnf 是它的一个现代化的分支，它保留了大部分 yum的接口。