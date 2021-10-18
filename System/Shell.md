### Shell

- Shell是Linux/Unix的一个外壳，你理解成衣服也行。它负责外界与Linux内核的交互，接收用户或其他应用程序的命令，然后把这些命令转化成内核能理解的语言，传给内核，内核是真正干活的，干完之后再把结果返回用户或应用程序。
- Linux/Unix提供了很多种Shell: sh、bash、zsh等，

### 命令

cat /etc/shells    查看系统有几种shell

切换zsh：chsh -s /bin/zsh

ctrl+c 直接停止当前执行的命令． 

ctrl+z 挂起当前执行的命令．

如果按下的是ctrl+z， ping命令就会被挂起，并不会结束．再执行ps -e|grep ping 发现ping这个进程还在． 

执行fg 命令可以重新启动这个被挂起的命令．

### 配置环境变量

```bash
# Linux操作系统下
vim /etc/profile

# 在OSX下,
open ~/.bash_profile
# 修改后编译
source .bash_profile
```

由于每次关闭窗口后，都要重新执行source 才能重新生效，固将路径添加到了～/.zshrc文件里解决。

因为系统每次都会默认执行～/.zshrc 文件，所以添加到这里保证肯定会执行，也可在～/.zshrc文件里加一句 source ~/.bash_profile。