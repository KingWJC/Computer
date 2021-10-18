## Macbook软件管理工具Homebrew

### 什么是Homebrew与Homebrew Cast

- Homebrew 是基于 OS X 的套件管理工具，是一个开源的 Ruby 脚本，主要用来下载一些不带界面的命令行下的工具和第三方库来进行二次开发，是从下载源码解压然后./configure && make install,同时会包含相关依存库。并自动配置好各种环境变量。
- Homebrew Cask是一套建立在 Homebrew 基础之上的 OS X 软件安装命令行工具，是 Homebrew 的扩展。主要用来下载一些带界面的应用软件，下载好后会自动安装。是已经编译好了的应用包(.dmg/.pkg).省掉了自己去下载、解压、拖拽(安装)等蛋疼的步骤。

有什么优势

- 通过 Homebrew 下载安装的软件全部来自对应的软件官网，无需担心下载源的安全问题。
- 依存于系统既有的库，减少了空间占用和冗余
- 使用 Git 进行管理和更新
- 安装软件 / 软件包 / 软件都在一个目录下，方便管理，这也是 Homebrew 能如此受欢迎的最大原因之一
- 对于brew来说，本来就是专门为macOs提供的missing package的manage，他是基于ruby和git来实现的，所有相关软件的安装都是来源于对应的官方网站，所以优点自来，对于appstore的软件来说拥有了相关的认证和权限，安装时的速度和拓展当然不如homebrew

### 安装

- 官网安装Homebrew

- - /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)”

- 国内镜像安装

- - 获取官网install脚本并保存名为 brew_install：[raw.githubusercontent.com/Homebrew/in…](https://raw.githubusercontent.com/Homebrew/install/master/install)
  - 打开 brew_install 文件，修改如下：

- - - **BREW_REPO** = “https://mirrors.ustc.edu.cn/brew.git “.freeze
    - **CORE_TAP_REPO** = “https://mirrors.ustc.edu.cn/homebrew-core.git“.freeze

- - 打开终端执行脚本：

  - - /usr/bin/ruby brew_install

### 更改默认源（中科院）

- 替换镜像地址

- - cd "$(brew —repo)"
  - git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

- 替换核心软件仓库

- - cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
  - git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

- 替换 cask 软件仓库（提供 macOS 应用和大型二进制文件）

- - cd "$(brew --repo)"/Library/Taps/homebrew/homebrew-cask
  - git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git

- 替换 Bottles 源（Homebrew 预编译二进制软件包）

- - bash（默认 shell）用户：

  - - echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
    - source ~/.bash_profile

  - zsh 用户：

  - - echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
    - source ~/.zshrc

- 应用生效:

- - $ brew update  

### Homebrew的问题解决

- 诊断Homebrew的问题:

- - $ brew doctor

- 重置brew.git设置:

- - $ cd "$(brew --repo)"
  - $ git fetch
  - $ git reset --hard origin/master

- homebrew-core.git同理:

- - $ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
  - $ git fetch
  - $ git reset --hard origin/master

- 应用生效:

- - $ brew update  

#### 重置更新源

\# 重置brew.git:

$ cd "$(brew --repo)"

$ git remote set-url origin https://github.com/Homebrew/brew.git

\# 重置homebrew-core.git:

$ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"

$ git remote set-url origin https://github.com/Homebrew/homebrew-core.git

关闭自动更新

- 执行命令：

- - export HOMEBREW_NO_AUTO_UPDATE=true

- 如果想要重启后设置依然生效，可以把上面这行加入到当前正在使用的shell的配置文件中，比如我正在使用的是zsh，那么执行以下语句：

- - vi ~/.zshrc
  - 然后在合适的位置，加入上面那一行配置。

Brew命令

- 安装：brew install [软件名]

- 更新软件，把所有的Formula目录更新，并且会对本机已经安装并有更新的软件用*标明：brew update

- 卸载：brew uninstall [软件名]

- 更新某具体软件：brew upgrade

- 查看安装软件列表和软件路径：brew list

- 查看安装路径：brew —cache

- 显示软件内容信息：brew info git

- 用浏览器打开官方网页：brew home

- 检查错误：brew doctor

- 显示包依赖树：brew deps --installed —tree

- 查看那些已安装的程序需要更新：brew outdated

- 显示安装的服务：brew services list

- 安装服务启动、停止、重启：brew services start/stop/restart serverName

- brew cask uninstall 软件名 卸载通过 Homebrew Cask 安装的软件

- brew search google 这里是查找所有与 google 有关的软件，google 关键词可以自行替换

- brew cask info 软件名 查找相关软件的信息

- brew cask cleanup 删除 Homebrew Cask 下载的包

- brew cask list 列出通过 Homebrew Cask 安装的包

- brew cask update 更新 Homebrew Cask

- brew tap。查看安装的扩展列表。

- - brew tap可以为brew的软件的 跟踪,更新,安装添加更多的的tap formulae
  - 如果你在核心仓库没有找到你需要的软件,那么你就需要安装第三方的仓库去安装你需要的软件
  - tap命令的仓库源默认来至于Github，但是这个命令也不限制于这一个地方
  - https://segmentfault.com/a/1190000012826983

- brew config

疑问

对于在macOs的终端执行命令类的拓展，需要打开相应的shell对应的配置文件进行添加路径操作，bash对应的是/.bash_profile,zsh对应的是/.zshrc。在修改后别忘了source进行编译才能生效.

```basic
Warning: homebrew/core is shallow clone. To get complete history run:
 git -C "$(brew --repo homebrew/core)" fetch --unshallow
```

#### 安装Python

- Mac自带Python2.7版本

- - Python —version
  - Which python

- 运行Python的安装工具安装pip

- - sudo easy_install pip

- 安装python3及pip3

- - Brew install python3

  - 成功安装完成python3.7版本之后，新路径会自动在.bash_profile文件中添加，如果没有，可以自己拷贝以下两行（外部环境变量）到末尾即可，

  - - 终端执行 open ~/.bash_profile
    - PATH=“/Library/Frameworks/Python.framework/Versions/3.6/bin:${PATH}"
    - export PATH

  - zsh设置默认启动的python版本：

  - - vi ~/.zshrc

\# Setting PATH for Python 2.7

PATH="/System/Library/Frameworks/Python.framework/Versions/2.7/bin:${PATH}"

export PATH

\# Setting PATH for Python 3.7.2_2(修改为实际安装的版本号，下同)

PATH=“/usr/local/Cellar/python/3.7.2_2/bin:${PATH}”

\# alias python=python3

alias python2='/System/Library/Frameworks/Python.framework/Versions/2.7/bin/python2.7'

alias python3='/usr/local/Cellar/python/3.7.2_2/bin/python3.7'

- - - source ~/.zshrc

  - bash设置默认启动的python版本：

  - - alias python=“/Library/Frameworks/Python.framework/Versions/3.7/bin/python3.7"
    - 执行python，查看默认版本。

  - Pip3安装库的位置：/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages/

### 生成SSH key

SSH key提供了一种与GitHub代码库通信的方式，通过这种方式，能够在不输入密码的情况下，将GitHub作为自己的remote端服务器，进行版本控制

- 检查SSH keys是否存在

- - 使用命令：ls -al ~/.ssh，查看文件id_rsa.pub 或 id_dsa.pub是否存在。

- 生成新的SSH Key

- - 使用命令：ssh-keygen -t rsa -C “your_email@example.com”，默认会在相应路径下（/your_home_path）生成id_rsa和id_rsa.pub两个文件
  - 设置口令passphrase后，进行版本控制时，每次与GitHub通信都会要求输入passphrase，以避免某些“失误”

- 将新生成的key添加到ssh-agent中

- - 使用命令启动ssh-agent代理：eval "$(ssh-agent -s)” 
  - 为 ssh-agent 添加私钥：ssh-add ~/.ssh/id_rsa
  - 通过 ssh-agent 来管理私钥，把私钥加载进内存，之后便不用再输入私钥。

- 将SSH key公钥从文件拷贝到粘贴板

- - 使用命令：pbcopy < ~/.ssh/id_rsa.pub

配制多个SSH秘钥

- command + shift +g 打开文件夹快捷键, ~/.ssh,进入SSH文件夹,并新建文件夹mygithub

- 生成新的SSH Key时,输入/users/hu/.ssh/mygithub/id_rsa,之后回车

- 配置config文件:

- - \#mygithub的配制
  - Host github.com
  - ​	HostName github.com
  - ​	IdentityFile ~/.ssh/mygithub/id_rsa
  - ​	User git

- 创建文件夹: mkdir +文件夹名，回车

- 创建文件: touch + 文件名.

#### MacOS操作参考:

https://wild-flame.github.io/guides/docs/mac-os-x-setup-guide/iTerm2/README;
