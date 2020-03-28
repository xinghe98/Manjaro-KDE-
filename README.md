# manjaro kde调教记录

## 基础配置

- ### 更换源

``` shell
sudo vi /etc/pacman.conf
```

#### 加入以下内容：

> [archlinuxcn] 
> 
> SigLevel = Never
>
> Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch

#### 换为国内源：

``` shell
sudo pacman-mirrors -c China
sudo pacman -Syyu
```

#### 安装yay：

```shell
sudo pacman -S yay
```

- ### 文明上网

```shell
sudo pacman -S v2ray
```

#### 将config.json文件放入/etc/v2ray后：

``` shell
sudo systemctl restart v2ray
```

#### 配置pac模式：

系统设置>>自动代理url：

> http://124.156.118.80/autoproxy.pac

#### 终端代理：

```shell
sudo pacman -S proxychains
sudo vi /etc/proxychains.conf
```

修改最后一行为：

> socks5 127.0.0.1 20808

*kcptun设置(根据需求)：*

前往[github项目地址](https://github.com/xtaci/kcptun/releases)下载linux-amd64，解压得到两个文件：

> client_linux_amd64 -c  *"你的json格式配置文件路径"*

后台启动并写入sh脚本开机自动启动：

``` shell
vi kcp.sh
```

加入以下内容：

> \#!/bin/bash 
>
> nohup Documents/kcptun/client_linux_amd64 -c Documents/kcptun/kcp.json > dev/null 2 >&1 &

为其添加可执行权限：

```shell
chmod +x ./kcp.sh
```

前往系统设置>>开机和关机>>自动启动>>添加脚本

- ### 安装输入法

  ``` shell
  sudo pacman -S fcitx-im             # 全部安装 
  sudo pacman -S fcitx-configtool     # 图形化配置工具 
  sudo pacman -S fcitx-googlepinyin
  sudo pacman -Sy fcitx-cloudpinyin
  ```

  将以下内容写入~/.xprofile后重启：

> export LC_ALL=zh_CN.UTF-8<br/>
> export GTK_IM_MODULE=fcitx<br/>
> export QT_IM_MODULE=fcitx<br/>
> export XMODIFIERS="@im=fcitx"

## 主题美化

- ### 终极效果展示

<img src="https://github.com/xinghe98/Manjaro-KDE-/blob/master/2020-03-28_12-35.png" alt="show" style="zoom:75%;" />

- ### 我的主题

> 全局主题：sweet
>
> plasma样式：tensent
>
> 应用程序风格：kvantum-dark
>
> 图标：tela-blua
>
> kvantum manager*(可以设置窗口透明度，pacman可安装)*：Sweet
>
> 字体：思源黑体CN*(自行安装，方法百度)*

#### 将默认面板删除，新建空面板放置顶部*（面板和系统托盘图标长什么样取决于plasma样式）*，我的小部件从左至右依次有：

> 1. Simple Menu
> 2. 调度器
> 3. 全局菜单
> 4. 系统托盘
> 5. 时间

*不喜欢可以到[KDE主题网站](https://store.kde.org/)下载安装，但请提前安装ocs-url，不装也行，但下载后有点麻烦，安装方式：*

``` shell
sudo pacman -S ocs-url
```

#### 安装latte-dock：

```shell
sudo pacman -S latte-dock
```

- ### conky 配置

#### conky可以实时显示系统信息，可嵌入桌面，要的就是极客感，下载conky：

```shell
sudo pacman -S conky conky-manager
```

新建文件夹~/.conky，将conky配置文件复制到该目录下，我的配置文件[github地址](https://github.com/xinghe98/myvimrc)(内含vim、nvim配置)：

```shell
git clone https://github.com/xinghe98/myvimrc.git
mv myvimrc/myconky ~/.conky/
```

应用程序打开 conky manager，选择配置文件。

- ### 终端配置

#### 安装fish并修改默认shell：

```shell
sudo pacman -S fish
chsh -c /usr/bin/fish 
```

安装oh-my-fish:

```shell
curl -L https://get.oh-my.fish | fish
```

配置fish：`fish_config`自动打开web浏览器。

#### Konsole基础配置：

设置>>编辑当前方案

- ### nvim 配置

```shell
sudo pacman -S neovim
```

复制前面github上下载的我的配置到nvim目录中：

```shell
mkdir ~/.config/nvim
mv myvimrc/init.vim ~/.config/nvim
```

安装插件管理器：

```shell
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

提供python支持：

```shell
pip install pynvim jedi
```

#### vim 展示：

![show](https://github.com/xinghe98/Manjaro-KDE-/blob/master/2020-03-28_12-41.png)

## 软件推荐

> QQ：deepin-wine-tim *(yay安装)*
>
> 微信：deepin-wine-wechat *(yay安装)*
>
> 截图软件：flameshot
>
> markdown编辑器：Typora
>
> 办公软件：wps-office

## 磁盘清理

```shell
    sudo pacman -Scc #清理无用包
    journalctl --disk-usage #查看日志文件
    sudo journalctl --vacuum-size=50M #删除指定大小日志文件
    sudo rm /var/lib/systemd/coredump/* #删除崩溃日志
```

### 说明：

1. wps按照需要安装字体：` sudo pacman -S ttf-wps-fonts`
2. 解决微信字体发虚：

```shell
yay -S lib32-freetype2-infinality-ultimate
sudo pacman -S wqy-bitmapfont wqy-microhei wqy-microhei-lite wqy-zenhei
```

3. 网易云音乐，百度网盘都有linux版本，自己去搜就好了。微信和qq只能凑合使用。
