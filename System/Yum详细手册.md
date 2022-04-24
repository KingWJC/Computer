# 作用

1. 基于RPM包构建的软件更新机制，
2. 自动解决软件包依赖关系，
3. 所有软件包由集中的YUM软件仓库提供。

# 安装

```bash
# 1.检查本机是否已安装 yum
# rpm -qa 列出所有被安装的rpm package
# grep 使用正则表达式搜索文本，并把匹配的行打印出来
# | 一个管道命令，用于组合命令
# yum 指定要查找的安装包
rpm -qa | grep yum

# 2.有的话就卸载包，--nodeps表示不含依赖
rpm -e --nodeps yum

# 组合的写法：rpm -qa | grep yum | xargs rpm -e --nodeps

# 3.查看当前Linux系统的版本
lsb_release -a

# 4.确认发型版本为CentOS 7 之后，查找对应的镜像文件 http://mirrors.163.com/centos/7/os/x86_64/Packages/ 中查找yum，下载3个包：
cd /Downloads
wget http://mirrors.163.com/centos/7/os/x86_64/Packages/yum-3.4.3-168.el7.centos.noarch.rpm
wget http://mirrors.163.com/centos/7/os/x86_64/Packages/yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
wget http://mirrors.163.com/centos/7/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.31-54.el7_8.noarch.rpm

# 5.安装文件
rpm -ivh yum-*

# 6.检查是否安装成功
yum makecache
```

# 配置

影响yum使用的主要文件

1. 基本设置: /etc/yum.conf

2. 仓库配置: /etc/yum.repos d/.repo

3. 日志文件: /var/log/yum.log

   注：客户端文件路径：/etc/yum.repos.d/.repo，在此路径下，错误的配置文件会影响正确的配置文件
   .repo基本配置项

```bash
 # .repo基本配置项
 [源名称]:自定义名称，具有唯一性
  name:本软件源的描述字串
  baseurl:指定YUM服务端的URL地址
  enabled:是否启用此频道
  gpgcheck:是否验证待安装的RPM包
  gpgkey:用于RPM软件包验证的密钥文件
```

配置源

```bash
# 1.备份
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

# 2.下载新的 CentOS-Base.repo 到 /etc/yum.repos.d/
# centos8（centos8官方源已下线，建议切换centos-vault源）
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-8.5.2111.repo
或者
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-8.5.2111.repo

# centos6（centos6官方源已下线，建议切换centos-vault源）
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-6.10.repo
或者
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-6.10.repo

# CentOS 7
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
或者
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo

# 3.生成缓存
yum makecache 
```

# 错误

```bash
# 执行yum命令时报错：
SyntaxError: invalid syntax File "/usr/bin/yum", line 30 except KeyboardInterrupt, e: 
SyntaxError: invalid syntax File “/usr/libexec/urlgrabber-ext-down“, line 28 except OSError
```

原因：

1. 因为yum采用python作为命令解释器，原来系统自带的python解释器为python2，然后有可能之前在配置开发环境时，将python默认的解释器设为了python3，导致按python3解析python2的语法出错了。
2. 同理，在使用pip，virtualenv等依赖原版本python的工具也会出现此类问题

Python版本自查：

1. 其中键入`python -V` 显示的版本就是**当前默认软连接的版本**,
2. 键入`python`后，按住`Tab`键（有可能需要按两次）补全，可以看到现有的所有python版本

解决方案：

1. 如果你已经误删了Centos系统自带的python版本，则要重新安装python2.7版本。

2. 若修改了原来的软链接，使得python实际指向的是新版本的python的话，那pip等仍依赖旧版本的工具**要么**重装，**要么**修改它py文件的首行(即py文件头的环境变量）。

   ```bash
   # 以yum为例
   vim /usr/bin/yum
   vim /usr/libexec/urlgrabber-ext-down
   
   # 修改第一行为你对应的初始python版本号：
   #!/usr/bin/python2.7
   ```