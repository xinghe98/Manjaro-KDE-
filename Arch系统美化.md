# ArchLinux 系统调教记录

## 前言
我在3年前写过一篇manjaro的美化教程，但实际情况是因为某次不小心删掉了congfig文件，
后来再也没用过linux，最近装上Arch后发现比起manjaro莫名其妙的bug更少，
于是有了此篇教程。

### 注意
- 本教程同样适用于manjaro系统
- 本教程不含Arch安装教程
- 大部分内容与之前的[教程](https://zhuanlan.zhihu.com/p/119187757) 类似

## 必要操作
### 换源
```shell该项目
sudo vi /etc/pacman.conf该项目
```

最后一行加入如下内容：
```
[archlinuxcn]
SigLevel = Never
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```
*经过多次测试，中科大的源在我这速度是最快的* 
```shell
sudo pacman -Syyu
```
### 安装yay
```shell
sudo pacman -S yay
```

### 文明上网
```shell
sudo pacman -S clash

cd ~/.config/clash

wget -o config.yaml '你的订阅链接'

wget -o Country.mmdb 'https://github.com/Dreamacro/maxmind-geoip/releases/download/20230112/Country.mmdb'

clash
```
浏览器进入本地的clash，地址：`https://clash.razord.top` 
填写你的端口号等信息，进去后即可配置


最后请将`/usr/bin/clash` 加入到开机启动。
### 安装输入法

```shell

sudo pacman -S fcitx-im

sudo pacman -S fcitx-configtool

sudo pacman -Sy fcitx-cloudpinyin

vi ~/.xprofile

```
将以下内容写入`xprofile` 文件中：
```shell
export LC_ALL=zh_CN.UTF-8
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

## 全面美化
- 效果展示
![](https://i.328888.xyz/2023/01/15/2Hhgo.png) 

在多次使用花哨主题后发现，还是淳朴一点比较耐看。

### 主题安装
经过对比发现，`layan` 和 `whiteSur` 是比较合适的亮色主题，两款主题在github上均能找到源码，离谱的是，有一键安装脚本。

#### 图标安装
使用`tela-blue` 主题的图标该[项目](https://github.com/vinceliuice/Tela-icon-theme) 同样有官方一键安装脚本

- 之前的教程是使用kde商店在线安装或者ocs-url安装，但如果代理不是从路由器走的，会出现无法下载的情况，因此建议先寻找心仪的主题，然后手动安装。

#### 安装latte-dock
```shell
sudo pacman -S latte-dock
latte-dock
```

启动`latte-dock` 后右键根据自己的喜好进行配置
#### 添加顶栏面板
1. 先将底部现有面板删除
2. 右键桌面->新建空面板->拖动至顶部
3. 根据喜好添加挂件
	- 我的挂件（从左至右）：
		- simple menu
		- 虚拟桌面器（多桌面工作极其实用）
		- 全局菜单
		- 系统托盘
		- 时钟
		- 搜索（krunner）

一般来说，有了krunner可以不再需要dock栏内的程序启动器，你可以设置一个快捷键映射成命令`krunner` ，像下面这样：
![](https://i.328888.xyz/2023/01/16/2xoeH.png) 
### 终端美化

#### 安装`zsh` 和 `oh-my-zsh` 
```shell
sudo pacman -S zsh
wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh
sh install.sh
```
- 打开终端
- 点击设置将命令改为`/bin/zsh` ，给zsh做一些简单配置
```shell
vim ~/.zshrc
```

- 位于第18行的`ZSH_THEME` 修改主题，预览图可上`oh-my-zsh`官网搜索。
- 使用zsh插件
- 修改第80行，添加zsh-autosuggestions,zsh-syntax-highlightting
- 下载插件：
```shell
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

到此为止，终端已经满足日常使用，如还需其他骚操作可自行搜索，本文不做介绍。

### vim 美化
#### 说明
- 如果你写代码不使用vim的话，此部分内容可忽略，直接上`vs-code` 或者其他IDE。
- 我使用Linux的一个重要原因是Linux对vim支持不要太好，win下经常会出现问题，而且win系统本身好像不支持`esc` 和 `Caps Lock` 键位对换
- 我常用的语言`Python` `Golang` `NodeJs` 在vim上使用可以完全不使用鼠标操作
- 我所说的`vim` 指的是 `neovim` 
- 我的配置参考了（或者说照抄？）B站@TheCW的配置。
- 我的`vimrc` 已放至github：https://github.com/xinghe98/myvimrc
#### 终极效果图演示部分
- Python

![](https://i.328888.xyz/2023/01/16/2SUcF.md.gif) 

- Golang

![](https://i.328888.xyz/2023/01/16/2S1Tt.md.gif) 

- markdown

![](https://i.328888.xyz/2023/01/16/2oUTz.md.png)  

#### 正经内容
- 安装必要的语言（golang非必要）
```shell
sudo pacman -S python-pip go nodejs npm yarn
```

<++>
- 下载neovim
```shell
sudo pacman -S neovim
```
- 安装插件管理器
	- 虽然当下比较流行使用lua进行配置，不过我不会写lua
```shell
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

- 创建配置文件目录
```shell
mkdir ~/.config/nvim
```

将我的配置文件下载后直接复制
```shell
mv myvimrc/init.vim ~/.config/nvim
```

安装python支持
```shell
pip install pynvim jedi
```

- 进入neovim安装插件即可

需要说明的是，有些插件需要进入其安装目录下`yarn install ` 进行安装，并且！不同的语言如果要更好的coc支持，需要在命令行模式下运行（比如）`:CocInstall CoC-Golang`。

**注意，原本是有coc-python的，但已经不维护了，现在推荐使用 coc-pyright** 

由于插件过多，有的插件需要一些配置项，有的插件里面又要安装其他插件（比如COC），这里不能一一介绍，可以之后遇到再找原因，比起百度，直接读官方文档和issue会更靠谱。

## 优秀（常用）软件推荐
下面没写安装命令的，名字就是包名
- flameshot

配合自定义快捷键快速截图

- linuxqq

懂得都懂，再也不用wine了

- qqmusic-bin

网易云也ok但是本机设置手动代理后会出现无法打开的情况，关键是他的系统托盘图标大小不适配，强迫症无法接受。

- 我在使用的字体

`https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/Noto/Sans-Mono/complete` 

- WPS 
除了这个也没啥能用的了吧。

```shell
yay -S ttf-wps-fonts wps-office-mui-zh-cn wps-office-mime-cn wps-office-cn ttf-ms-fonts
```

# 结尾
该文档并非边操作边写，因此会有遗漏以及不完善，具体问题需靠自己。


最后放上我的最最最终的效果
![](https://i.328888.xyz/2023/01/16/2s0CX.png)
