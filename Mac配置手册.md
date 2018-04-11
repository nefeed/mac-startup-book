# Mac配置手册

## 允许非认证开发者应用程序启动

```Shell
# 输入指令并输入密码，允许任何来源的应用程序
sudo spctl --master-disable
```

## HomeBrew

### 首先安装HomeBrew来管理应用
```shell
# 安装指令
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### HomeBrew常用指令说明
```shell
# 搜索应用
brew search [TEXT|/REGEX/] 
# 查看应用信息
brew (info|home|options) [FORMULA...]
# 安装应用
brew install FORMULA...
# 抓取所有应用的最新版本信息（相当于git fetch的功能）
brew update
# 更新应用版本（无制定则更新所有本地安装的应用）
brew upgrade [FORMULA...]
# 卸载某应用
brew uninstall FORMULA...
# 显示本地安装的应用
brew list [FORMULA...]
```

### 如何删除HomeBrew
```shell
cd `brew --prefix`
rm -rf Cellar
brew prune
rm `git ls-files`
rm -rf Library .git .gitignore bin/brew
rm -rf README.md share/man/man1/brew
rm -rf Library/Homebrew Library/Aliases 
rm -rf Library/Formula Library/Contributions
rm -rf .git
rm -rf ~/Library/Caches/Homebrew
```

## 安装***oh-my-zsh***代替Terminal中使用的***Bash***
### 替换系统默认zsh
首先可以通过HomeBrew安装zsh最新版本来替换MacOS提供的旧版本zsh
```shell
brew install zsh
```
可通过```zsh --version```命令查看zsh的版本
使用```echo $ZSH_VERSION```命令查看当前使用的zsh版本

### 安装oh-my-zsh
```shell
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
之后默认Shell的~/.bashrc文件就不再加载了，替代的事~/.zshrc

### 更改zsh主题
1. 编辑```~/.zshrc```
2. 修改```ZSH_THEME=[主题名]```

> 注：主题文件在 ~/.oh-my-zsh/themes 目录，以[主题名].zsh-theme存储
> 如果不需要主题，设置
> ```ZSH_THEME=""```
> 如果需要主题
> ```ZSH_THEME=[主题名]```
>
> 当前我使用的是***af-magic***主题

## 安装Git并配置全局环境
```shell
# 安装
brew install git
# 配置全局git用户名
git config --global user.name "你的名字（最好是中文全名，例章华隽）"
# 配置全局git邮箱
git config --global user.email "你的邮箱地址（最好是公司邮箱，例huajun.zhang@xbongbong.com）"
# 生成私钥和密钥，路径~/.ssh/
ssh-keygen -t rsa -C "你上面输入的邮箱地址"
```

### 安装HomeBrew Cask
HomeBrew Cask相当于Mac端的软件管理工具，类推Win平台的概念，好比XX软件助手等
```shell
brew install brew-cask
```
之后可以通过指令进行软件安装，如安装Java
```shell
brew cask intall java
```
如果需要安装历史版本需要先允许HomeBrew可以使用homebrew-cask-versions：
```shell
# 允许使用homebrew-cask-versions
brew tap caskroom/versions
# 安装Java8
brew cask install java8
```
如果想要快速切换两套Java环境，可以在环境中定义指令进行切换：
```shell
# 以oh-my-zsh为例，一下内容直接粘贴到~/.zshrc后进行修改即可

### JAVA_HOME begin
export JAVA_8_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_141.jdk/Contents/Home
export JAVA_10_HOME=/Library/Java/JavaVirtualMachines/jdk-10.jdk/Contents/Home

# 该命令降切换JDK环境至jdk1.8
alias jdk8="export JAVA_HOME=$JAVA_8_HOME"
# 该命令降切换JDK环境至jdk10
alias jdk10="export JAVA_HOME=$JAVA_10_HOME" 

# 最后安装的版本，这样当自动更新时，始终指向最新版本
export JAVA_HOME=`/usr/libexec/java_home`  
# export JAVA_HOME=$JAVA_8_HOME  
### JAVA_HOME end
```

### 替换HomeBrew的源为中科大（USTC）源
```shell
# 替换brew.git:
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

# 替换homebrew-core.git:
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
```
切换回默认源的指令如下：
```shell
# 重置brew.git:
cd "$(brew --repo)"
git remote set-url origin https://github.com/Homebrew/brew.git

# 重置homebrew-core.git:
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core.git
```

### 替换HomeBrew Cask的源为中科大（USTC）源
```shell
cd "$(brew --repo)"/Library/Taps/caskroom/homebrew-cask
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git
```
切换回默认源的指令如下：
```shell
cd "$(brew --repo)"/Library/Taps/caskroom/homebrew-cask
git remote set-url origin https://github.com/caskroom/homebrew-cask
```

### 替换HomeBrew Bottle
请在运行 brew 前设置环境变量 HOMEBREW_BOTTLE_DOMAIN ，值为 https://mirrors.ustc.edu.cn/homebrew-bottles 。
```shell
# 对于zsh用户而言
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc
```

## MacOS显示全部文件或隐藏隐藏文件

```shell
# 显示全部文件
defaults write com.apple.finder AppleShowAllFiles -bool true
osascript -e 'tell application "Finder" to quit'

# 不显示全部文件
defaults write com.apple.finder AppleShowAllFiles -bool false
osascript -e 'tell application "Finder" to quit'
```

## 通过Terminal指令快速使用Visual Studio Code打开文件或文件夹
```shell
# 以oh-my-zsh为例，一下内容直接粘贴到~/.zshrc后进行修改即可

# VScode
alias vscode='/Applications/Visual\ Studio\ Code.app/Contents/Resources/app/bin/code'
```

# 以下内容可以忽略，主要是安装一下必要的环境和配置

## 在Terminal中进行网络代理和关闭
```shell
# 以oh-my-zsh为例，一下内容直接粘贴到~/.zshrc后进行修改即可

# 网络代理开关配置
alias proxy='export all_proxy=socks5://127.0.0.1:1086'
alias unproxy='unset all_proxy'
```

## 安装一些常用的第三方软件
```shell
# 安装maven环境
brew install maven
# 安装node环境
brew install node
# 安装vim工具
brew install vim
# 安装MySQL
brew install mysql
# 安装指令高亮工具
brew install highlight
# 安装zsh指令高亮工具
brew install zsh-syntax-highlighting
# 安装curl（模拟请求）工具
brew install curl
# 安装autojump（快速跳转）工具
brew install autojump
# 安装fuck（自动修正指令）工具
brew install thefuck
# 安装钉钉
brew cask install dingtalk
# 安装微信
brew install wxmac
# 安装QQ
brew cask install qq
# 安装chrome
brew cask install google-chrome
# 安装网易云音乐
brew cask install neteasemusic
# 安装视频播放器
brew cask install iina
```