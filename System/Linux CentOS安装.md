### Linux CentOS 6.0 安装

- 设备中，双击CD/DVD(IDE)，选择ISO映像文件。
- 点击：开启此虚拟机
- Basic Storage Device，再选择清除存储中的碎片数据。
- HostName：node01
- 时区选上海
- RootPwd：wjc@123
- 创建分区：Create Custom Layout.
  - 磁盘驱动器名称：dev/sda(第一块磁盘)，sdb，sdc....，分为3个分区。
  - boot 引导程序区 ：操作系统的引导程序，方便启动Linux内核。
    - Mount Point：/boot，Type：ext4，Size：200Mb
  - swap 交换区：内存不足时，启动进程，将内存的数据写入交换区中。保障资源紧张的情况下，可运行多个进程
    - Type：swag，Size：2048MB（内存的一倍）
  - 用户区 ：用户的数据和安装的软件
    - Mount Point：/，Type：ext4，Size：剩余所有。
- 格式化，并将分区的信息写入磁盘中，防止数据丢失。
- 安装引导程序到/boot下。

### 配置网络服务

- 找到网卡位置。
  - cd /etc/sysconfig/network-scripts/
- 配置TCP/IP协议。
  - 编辑ifcfg-eth0：interface config ethernet 以太网网络接口配置。第一块网卡。
  - 删除网卡物理地址和唯一 标识。(方便后期克隆虚拟机，以防多个虚拟机拥有相同的网卡物理地址，出现网络问题。删除70-persistent-net.rules文件，新文件会重新生成物理地址和标识，并绑定新主机名）
  - 启用网卡。配置IPADDR，NETMASK, GATEWAY, DNS1
  - 查看ifconfig, 重启服务。然后测试。
    - centOS 6: service network restart.
    - centOS 8:  重载配置文件: nmcli c reload ens33;   重启网卡: nmcli c up ens33.
- ifcfg-eth0配置参数
  - HWADDR：网卡物理地址。
  - UUID：唯一标识
  - ONBOOT：yes  开机自动启用网络连接。
  - BOOTPROTO=static  设置为none禁止DHCP，设置为static启用静态IP地址，设置为dhcp开启DHCP服务动态地址协议，自动获取ip
  - IPADDR= 192.168.91.3 IP地址
  - NETMASK=255.255.255.0 子网掩码
  - GATEWAY=192.168.91.2 网关，（虚拟机网路编辑器中）
  - DNS1=114.114.114.114 第一个DNS服务器
  - DNS2=192.168.91.2. 虚拟网卡的网关地址也可以作为一个DNS服务器来解析地址。

### Linux CentOS 8.0 安装

下载地址:https://www.centos.org/download/

系统安装包分为: boot和dvd. DVD的完整镜像，大小7G多. boot类型的镜像，只有500M大小

boot是最小化引导的ISO镜像，需要通过BaseOS和AppStream存储库安装软件包.

1. 添加虚拟机时, 选择固件类型: UEFI 和 Lengacy BIOS

   - Legacy是传统BIOS,使用Int 13中断读取磁盘，每次只能读64KB，非常低效.
   - UEFI，新式的BIOS, 全称“统一的可扩展固件接口”(Unified Extensible Firmware Interface)， 是一种详细描述类型接口的标准. 负责加电自检（POST）、联系操作系统以及提供连接操作系统与硬件的接口。

2. 设置网络和主机名.打开后, 等一会, 安装源和软件变的可选.

3. 安装目的地: 创建分区Custom.

   - boot 启动分区 ：操作系统的引导程序，方便启动Linux内核。
     - Mount Point：/boot/efi，Type：EFI System，Size：128Mb
   - swap 交换分区：内存不足时，启动进程，将内存的数据写入交换区中。保障资源紧张的情况下，可运行多个进程
     - Type：swag，Size：2048MB（内存的一倍）
   - root分区 ：System. 根分区, 所有未指定挂载点的目录.
     - Mount Point：/，Type：xfs，Size：剩余所有。
   - home: Data. 用户目录, 存放普通用户的数据. 适合多用户, 用户的数据和安装的软件
     - Mount Point：/home，Type：xfs，Size：8G。

   确定后, 先删旧的,再创建新的.

4. 安装源

   1. 填写aliyun镜像文件地址

   - 协议选择http
   - 地址填写：[mirrors.aliyun.com/centos/8/BaseOS/x86_64/os/](http://mirrors.aliyun.com/centos/8/BaseOS/x86_64/os/)

5. 软件选择: Server.

6. Root密码和用户.

7. 网络配置：未添加DNS，导致网络失败。

> 更多新特性，请参考阅读红帽的官方文档https://access.redhat.com/documentation/zh-cn/red_hat_enterprise_linux/8/html/8.0_release_notes/notable_changes_to_containers