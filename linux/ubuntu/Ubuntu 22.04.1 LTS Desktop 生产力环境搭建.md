---
link: https://juejin.cn/post/7137891969585381390
title: Ubuntu 22.04.1 LTS Desktop 生产力环境搭建
description: 一次意外搞挂了Ubuntu20.04LTS，不得不从头安装Ubuntu22.04。顺便重新梳理适合开发者的系统搭建、配置和工具。
keywords: Ubuntu
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-08-31T04:27:14.000Z
publisher: 稀土掘金
stats: paragraph=88 sentences=119, words=515
---
## 不幸

一个夏日炎炎的下午，喝着咖啡，耍着电脑，突然Ubuntu22.04.1的推送顺着WiFi就来了。心想这都推出有一阵子了，应该趋于"成熟"，便开始升级。不幸总是在不经意间降临，就在升级Firefox的snap时，升级程序就这么卡住了......先是Firefox的升级程序卡主，然后整个更新程序卡主。当时的想法：大不了reinstall吧，多次在图形界面中无法重启，最后在命令行中 `reboot`，于是悲剧就发生了：电脑再次启动时，提示系统损坏，联系管理员......

## 挣扎

按照Windows或Mac的惯性思路，遭遇安装过程中意外重启导致的安装失败，可以通过重复安装步骤来修复。在[官网下载](https://link.juejin.cn?target=https%3A%2F%2Fubuntu.com%2Fdesktop "https://ubuntu.com/desktop")Ubuntu22.04.1的安装程序，并[制作](https://link.juejin.cn?target=https%3A%2F%2Fwww.balena.io%2Fetcher%2F "https://www.balena.io/etcher/")为启动盘。引导电脑，选择reinstall。结果安装程序一个大大的警告贴在我脸上：大致意思是我只有一个分区（自从不用FAT32文件系统后就再没分区），安装程序将会删除所有文件，并格式化磁盘......眼前一黑......

数分钟后，我接受了现实。选择直接用U盘启动Ubuntu最小系统，挂载电脑的SSD，先把工程文件夹全部打包

tar -vzcf ./backup.tar.gz ./Engineerings

如果不打包也能备份，但是碎片文件过多(170万个文件)，这个备份和恢复过程可能需要数个小时。我选择先压缩，然后备份压缩包。打包过程大约过了半小时，然后将打包文件（7G）复制到移动SSD中。保险期间，再将文件扔到苹果系统中检查是否有错。

## 新生

选择直接安装系统，选择不要使用在线更新（这样安装过程会更快），不多久就装好了，再连上WiFi，更新系统。先说说第一眼感受：20和22的差别不大，GUI确实漂亮一些（但有限）。不知道20LTS的一些老Bug是否有被修复，这可能需要一些时日来体会了。以下是关于安装软件和配置系统的备忘，有兴趣的同学可以拿去参考。经过一段时间的摸索，终于将系统安装完毕。顺便整理了整个安装与配置过程，算是（3年前）Ubuntu18开发环境的一个升级吧。

### 基础

#### 软件源

##### APT

先将软件库替换到国内的镜像服务器（以下是阿里云镜像），因为某些原因，如果不改动源的设置，可能会造成一些影响：

```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
sudo sed -i 's/http://cn.archive.ubuntu.com/https://mirrors.aliyun.com/g' /etc/apt/sources.list

sudo apt update && sudo apt upgrade -y
```

Ubuntu在国内已经搭建了镜像，即便是不替换源，速度也不至于像从前那样龟速，比较显著的感受在Ubuntu商店已经比以前快很多，然而这个国内并没有三方镜像（可能时因为软件总体还是很糟糕，没人愿意去做吧）。

`Nala`是APT的外壳，相比apt有更好看的表现（字符界面），并能够更方便地配置镜像地址。通过执行：

sudo nala fetch

nala会跑完500多个镜像服务器，并列出响应速度最快的前16个服务器，我们可以选择一个或者多个（多个就用空格隔开）。其余操作根 `APT`一致，例如 `install`， `update`和 `upgrade`。如果选用 `Nala`作为软件管理，那么下文中的 `apt`命令都改为 `nala`即可。

```bash

echo "deb https://deb.volian.org/volian/ scar main" | sudo tee /etc/apt/sources.list.d/volian-archive-scar-unstable.list
wget -qO - https://deb.volian.org/volian/scar.key | sudo tee /etc/apt/trusted.gpg.d/volian-archive-scar-unstable.gpg > /dev/null

sudo apt update && sudo apt install nala -y

sudo nala fetch
```

#### 常用类库与基础工具

* 常用的编译工具
* git / zsh / curl / wget：阿里云的code.aliyun.com服务加强了git基于ssh-rsa的认证，所以需要设置特定的配置文件，不然否则会造成无法git账户无法ssh连接
* neovim：经常会在字符界面中做一些文件小改动的操作，neovim非常合适。
* [oh-my-zsh](https://link.juejin.cn?target=https%3A%2F%2Fohmyz.sh%2F "https://ohmyz.sh/") /[power line 10k](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fromkatv%2Fpowerlevel10k "https://github.com/romkatv/powerlevel10k")：颜值即正义（如果需要这个，先安装下文的Nerd字体，并设置命令行的默认字体为Nerd系列，再回到这一步）。
* OpenSSH-Server：偶尔会把电脑留在公司，但又突然像要某个文件......

* 压缩与解压缩
* 其他工具：tmux：分屏，tree：目录查看

```bash
sudo apt install make gcc libtool libtool-bin autoconf automake cmake g++ pkg-config -y
sudo apt install git zsh curl wget -y
ssh-genkey

tee ~/.ssh/config <'EOF'
Host code.aliyun.com
    User git
    PubkeyAcceptedAlgorithms +ssh-rsa
    HostkeyAlgorithms +ssh-rsa
EOF

sudo apt install neovim

sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

chsh -s $(which zsh)

git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc

sudo apt install openssh-server

sudo apt install 7zip unrar zip unzip

sudo apt install tmux tree
```

#### 输入法

目前搜狗输入法与Ubuntu22还是有一点兼容性问题：如果不使用 `zsh`作为默认 `shell`还是可以选择搜狗输入法，否则fcitx程序会无法启动搜狗输入法。解决方法目前只能使用系统自带的智能拼音。

##### 搜狗拼音

[搜狗输入法](https://link.juejin.cn?target=https%3A%2F%2Fshurufa.sogou.com%2Flinux%3Fr%3Dpinyin "https://shurufa.sogou.com/linux?r=pinyin")。如果在安装时时区(Timezone)选择国内，则直接从第4步开始：

1. Settings -> Region & Language -> Manage Installed Languages；

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d31a3b76fc5242298e791a097ae6c5b2~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

1. Install / Remove Languages，第一次开启会安装依赖；
2. 然后勾选Chinese (Simplified)；
3. 安装 `fcitx` > `sudo apt install fcitx -y`(--fix-missing)
4. 第1步的方式打开语言支持，Keyboard选择输入框架 `Fcitx4`![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7aa76a0dae8f41f0b387b529fb0da534~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)
5. 删除ibus输入法 `sudo apt remove --purge ibus`
6. 安装[搜狗输入法](https://link.juejin.cn?target=https%3A%2F%2Fshurufa.sogou.com%2Flinux%3Fr%3Dpinyin "https://shurufa.sogou.com/linux?r=pinyin") `sudo dpkg -i ~/Downloads/sogoupinyin_4.xxx.deb`
7. 安装依赖 `sudo apt install libqt5qml5 libqt5quick5 libqt5quickwidgets5 qml-module-qtquick2 libgsettings-qt1`
8. 重启系统，并右上角看到键盘图标
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f518aa11c8a34fe89fd220af664d2907~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)
9. 选择 `Configiure`，进行配置
10. 在 `Input Method Configuration`对话框的下面，点 `+`
11. 弹出 `Add input method`对话框，去掉默认选中的 `Only Show Current Language`选项，在搜索框中输入 `sogou`
12. 一路OK和关闭。

##### 智能拼音

1. Settings -> Region & Language - > Manage Installed Languages
2. 确保有 `&#x6C49;&#x8BED;&#xFF08;&#x4E2D;&#x56FD;&#xFF09;`已经安装，没有安装点 ` Install / Remove Languages` 添加
3. 确保Keyboard input method system是 `IBus`
4. Settings -> Keyboard -> Input Sources 添加
5. 选择 `Chinese`，在子选项中选择 `Chinese (Intelligent Pinyin)`

如果觉得智能拼音的UI过于单调，或希望调整字体大小，可以通过 `Extension Manager`找 `IBus Tweaker`的插件，对 `IBus`输入法配置。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9c09ad3d4476412aa363ab0c14362c98~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

#### Chrome浏览器

没啥好说的，官方网站[下载](https://link.juejin.cn?target=https%3A%2F%2Fwww.google.com%2Fchrome%2F "https://www.google.com/chrome/")，登录账号后所有插件会自动同步，以下是我的常用插件：

ID名称描述poahndpaaanbpbeafbkploiobpiiiekoDisplay #Anchors显示网页锚点mihcahmgecmbnbcchbopgniflfhgnkffGoogle Mail CheckerGmail邮箱检测oldceeleldhonbafppcapldpdifcinjiGrammar & Spell Checker英语语法与拼写检测chklaanhfefbnpoihckbnefhakgolnmcJSONVue检测纯JSON数据并格式化mooikfkahbdckldjjndioackbalphokdSelenium IDE前端测试dhdgffkkebhmkfjojejmpbldmpobfkfoTampermonkey油猴cjpalhdlnbpafiamejdnhcphjbkeiagmuBlock Origin广告屏蔽（有时候它会用力过猛）nhdogjmejiglipccpnnnanhbledajbpdVue.js devtoolsVUE 程序调试gppongmhjkpfnbhagpmjfkannfbllamgWappalyzer分析网站采用哪些技术栈

#### Firefox 浏览器

Ubuntu22 系统内置snap版本的Firefox浏览器，受限于snap应用，不能使用三方应用，并且打开速度不如正常安装的软件。所以，一般有三种选择：

1. 作为偶尔一用的调试工具，无视它，可以跳过这小节；
2. 索性卸载掉，眼不见为净；
3. 卸载snap版本，再重新安装完整的Firefox；

```sh
sudo snap remove firefox
sudo add-apt-repository ppa:mozillateam/ppa
echo '
Package: *
Pin: release o=LP-PPA-mozillateam
Pin-Priority: 1001
' | sudo tee /etc/apt/preferences.d/mozilla-firefox
echo 'Unattended-Upgrade::Allowed-Origins:: "LP-PPA-mozillateam:${distro_codename}";' | sudo tee /etc/apt/apt.conf.d/51unattended-upgrades-firefox
sudo apt install firefox
```

#### 字体

开发者常用[Nerd Fonts](https://link.juejin.cn?target=https%3A%2F%2Fwww.nerdfonts.com%2Ffont-downloads "https://www.nerdfonts.com/font-downloads")和Mono等宽字体，还有这里

[github.com/ryanoasis/n...](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fryanoasis%2Fnerd-fonts "https://github.com/ryanoasis/nerd-fonts")

[www.nerdfonts.com/font-downlo...](https://link.juejin.cn?target=https%3A%2F%2Fwww.nerdfonts.com%2Ffont-downloads "https://www.nerdfonts.com/font-downloads")

字体文件(ttf)下载后，直接用系统自带的 `Fonts Manager`打开并安装到系统中，也可以手动安装（我认为手动安装更快，拷贝到相关目录，执行清理缓存即可）

```bash
sudo mkdir /usr/local/share/fonts/somefonts
sudo cp ~/Downloads/fonts.ttf /usr/local/share/fonts/somefonts

fc-cache -f -v
```

常用中文字体有：

* [阿里巴巴普惠体](https://link.juejin.cn?target=https%3A%2F%2Fics.alibaba.com%2Fproject%2FHn8mXx "https://ics.alibaba.com/project/Hn8mXx")（免费，好看）
* 微软雅黑、苹果苹方（商业字体）

#### 私有云(ownCloud)

```ruby
curl https://download.owncloud.com/desktop/ownCloud/stable/latest/linux/Ubuntu_22.04/INSTALL.sh | sudo bash
```

直接可以完成下载和安装，就是那个gpg证书版本老了，每次会有提示。

#### 开发环境

既然是程序员的电脑，自然离不开开发环境。我主要从事Web全栈程序开发，从服务器的数据库到前段的CSS都有涉及。另外一方面，除了开发工作外，有许多优秀的工具是源码下载使用的，所以需要相关的语言环境作为支撑。

* NodeJS：建议使用nvm管理工具以及nrm的类库源管理，一台电脑上同时安装多个NodeJS版本
* Python：同样，使用anaconda 可以管理多个python版本库，还附带了常用的python工具箱
* Docker：直接用官方提供的安装脚本，并同时安装docker-compose
* 其他开发语言：[Go](https://link.juejin.cn?target=https%3A%2F%2Fgo.dev%2Fdl "https://go.dev/dl") /[Ruby](https://link.juejin.cn?target=https%3A%2F%2Fwww.ruby-lang.org%2F "https://www.ruby-lang.org/") /[Rust](https://link.juejin.cn?target=https%3A%2F%2Fwww.rust-lang.org%2F "https://www.rust-lang.org/") /[Lua](https://link.juejin.cn?target=https%3A%2F%2Fwww.lua.org%2F "https://www.lua.org/")
* Java JRE

```bash

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash

npm i -g npm nrm

wget https://repo.anaconda.com/archive/Anaconda3-2022.05-Linux-x86_64.sh | bash

curl -sSL https://get.docker.com/ | sh

sudo usermod -aG docker $USER

pip3 install pyqt5 pyqtwebengine pathlib docker-compose

sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <'EOF'
{
  "registry-mirrors": ["https://****.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker

GO_LATEST=1.19.linux-amd64
wget https://go.dev/dl/go${GO_LATEST}.tar.gz -O ~/Downloads/go.tar.gz
sudo tar -C /usr/local -xzf ~/Downloads/go.tar.gz

export PATH=$PATH:/usr/local/go/bin

sudo apt install ruby-full -y

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

LUA_LATEST=5.4.4
wget http://www.lua.org/ftp/lua-${LUA_LATEST}.tar.gz -O ~/Downloads/lua.tar.gz
tar zxf lua.tar.gz
cd lua-${LUA_LATEST}
make linux test
sudo make install

sudo apt install default-jdk
sudo apt install openjdk-18-jdk
```

### 图形界面工具

有些工具可以通过 `snap`安装，由于Ubuntu预装了Snapcraft，可以直接用。如果没有的，则先安装 `snapd`。

* VSCode：程序员第一神器 ，可以直接从官网下载deb包文件，但缺点是每次升级都得去官网下载（Ubuntu下不像Windows或Mac静默下载重启更新），所以最好是用 `apt`安装，每次update可以附带着一起升级了，下载速度也快。需要手动添加gpg证书和安装包源。
* [XMind](https://link.juejin.cn?target=https%3A%2F%2Fwww.xmind.cn%2Fdownload%2F "https://www.xmind.cn/download/")，思维导图工具，几年的进化已经有用非常漂亮的界面，且免费版本已经足够日常使用。
* [Marktext](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fmarktext%2Fmarktext "https://github.com/marktext/marktext")：Markdown编辑器
* GIMP [Snap]：Linux下的Photoshop
* VLC [Snap]：卡片必备
* Redis Desktop Manager [Snap]：redis的GUI客户端
* [DBeaver](https://link.juejin.cn?target=https%3A%2F%2Fdbeaver.io%2Fdownload%2F "https://dbeaver.io/download/")：支持主流的关系型数据库的GUI客户端，免费版也足够用
* [Studio 3T](https://link.juejin.cn?target=https%3A%2F%2Fstudio3t.com%2Fdownload%2F "https://studio3t.com/download/")：Java开发的MongoDB客户端，前不久开放免费使用（30天后限制功能也能用），替代Robo 3T
* Sublime：图形界面下，纯文本文件备用编辑器
* FileZilla：SSH或FTP拷贝文件还是很方便的
* aMule：电驴下载工具
* Darktable[Snap]：图片浏览工具
* Kazam：录屏软件
* bleachbit：系统清理工具
* [draw.io](https://link.juejin.cn?target=https%3A%2F%2Fapp.diagrams.net%2F "https://app.diagrams.net/")：草图绘制工具
* [Angry IP Scanner](https://link.juejin.cn?target=https%3A%2F%2Fangryip.org%2Fdownload%2F%23linux "https://angryip.org/download/#linux")：局域网地址扫描工具

```bash

wget -O- https://packages.microsoft.com/keys/microsoft.asc | sudo gpg --dearmor | sudo tee /usr/share/keyrings/vscode.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/vscode.gpg] https://packages.microsoft.com/repos/vscode stable main" | sudo tee /etc/apt/sources.list.d/vscode.list
sudo apt update && sudo apt install code -y

sudo snap install vlc redis-desktop-manager gimp darktable darwio

tar -C ~/Downloads -xzf ~/Downloads/studio-3t-linux-x64.tar.gz
bash ~/Downloads/studio-3t-linux-x64.sh

wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo gpg --dearmor -o /usr/share/keyrings/sublimetext-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/sublimetext-keyring.gpg] https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt update && sudo apt install sublime-text -y

sudo apt install amule filezilla kazam bleachbit -y
```

### 主题与扩展

Ubuntu拥有不逊于苹果的美观和可定制特性，根据个人的喜好调教工作环境可以提高不少工作效率。

#### 扩展

* Blur my shell：super+A 的那个luanch pad 的背景改为模糊（而不是纯色）
* cpufreq：gnome左上显示当前CPU频率，第一次点击图标安装性能设置工具。此时可以删除调系统自带的性能设置 `irqbalanc`。记得将电脑性能改为平衡模式 `balance`
* caffine：禁止系统进入休眠模式
* TopHat：系统资源使用监控。需要另外安装 `gir1.2-gtop-2.0`

#### 主题

从[Gnome-look](https://link.juejin.cn?target=https%3A%2F%2Fwww.gnome-look.org%2Fbrowse%3Fcat%3D135%26ord%3Drating "https://www.gnome-look.org/browse?cat=135&ord=rating")找自己喜欢的主题包，事先安装gnome-tweaks / gnome-shell-extensions，通过extension-manager 开启user-theme。以Orchis为例（方法大同小异）：

1. 克隆主题源代码到本地
2. 在源代码目录中执行install，此时脚本会将所需要的以来选项和文件都处理完成

第二种方法（全手动）

1. 在主目录下（~/）建立两个隐藏目录 `.themes`和 `.icons`
2. 下载主题资源包，解压缩到对应的目录中
3. 打开 `tweak`选择对应的主题

推荐关注[必应墙纸](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fniumoo%2Fbing-wallpaper "https://github.com/niumoo/bing-wallpaper")，每天都会更新bing上的美图，看中后可以直接作为桌面背景使用。

```arduino

sudo apt install gnome-shell-extension-manager

sudo apt install gnome-tweaks gnome-shell-extensions chrome-gnome-shell -y
```

### 命令行工具

* htop 监控
* neofetch：炫耀用系统概览信息
* preload ：分析用户习惯，在重启电脑后预加载应用程序，实现秒开程序的效果

```lua
sudo apt install htop neofetch preload -y
```

### Windows 虚拟机

Linux已经可以完成大部分的工作，但是还不是全部。有些专门的软件也许只能在Windows操作系统下运行，面对这种情况，可以选择VirtualBox+Windows7的组合。

[www.virtualbox.org/wiki/Linux_...](https://link.juejin.cn?target=https%3A%2F%2Fwww.virtualbox.org%2Fwiki%2FLinux_Downloads "https://www.virtualbox.org/wiki/Linux_Downloads")

```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/oracle-virtualbox-2016.gpg] https://download.virtualbox.org/virtualbox/debian jammy contrib" | sudo tee /etc/apt/sources.list.d/virtual-box.list
wget -O- https://www.virtualbox.org/download/oracle_vbox_2016.asc | sudo gpg --dearmor --yes --output /usr/share/keyrings/oracle-virtualbox-2016.gpg
sudo apt update && sudo apt --fix-broken install && sudo apt install virtualbox-6.1
```

除了非得运行Windows的程序所要使用到虚拟机外，其实大部分办公需求可以用Android模拟器

### 环境设置

根据自己的使用习惯调整系统的风格：

* 调整dock位置（屏幕下方居中）并设置隐藏（扩大显示面积）；
* Theme的基础颜色
* 非常好用的桌面切换快捷键：Ctrl+Alt+左右方向键：快速切换工作桌面。Ctrl+Shift+Alt+左右方向：将当前窗口带到新工作区
* 快捷键设置 Settings -> Keyboard -> Keyboard Shortcuts，以下仅供参考：
  1. `Launchers`：打开命令行 Super+`，文件夹F2，计算器F3，浏览器F1，打开设置F12
  2. `Navigation`：隐藏所有窗口（显示桌面）Super+M
  3. `Custom Shortcuts`：设置其他常用程序
* 开启日落护眼模式：settings-> displays->Night Light
* 设置随机启动软件（图形界面，GNOME）Super+A，输入startup
* 移除不用的软件包：sudo apt autoremove
* 运行 `.AppImage`文件： `sudo apt install libfuse2`然后在文件的权限中选择执行即可双击打开。
* 设置Termianl为半透明背景（透过界面能看到后面的窗口，有些场景下非常好用）
* 笔记本电脑：开启电池百分比显示
* 如果是高分辨率屏幕，则需要开启更细节的缩放比例，默认情况下只有成倍增加，开启后可以25%一档调整。
* 设置头像和开启指纹
* 设置一些命令行下的快捷命令，如：查看各分区剩余空间，查看ip地址，从命令行中复制到剪切板；
* 将系统同时打开文件数量调高到26万

```ini

alias df='df -h -x squashfs -x tmpfs -x devtmpfs -x overlay'

alias ip='ip -4 -br address'

alias cc='xclip -sel clip < '

alias dps='docker ps --format "{{.ID}}\t{{.Status}}\t{{.Ports}}\t{{.Names}}"'

sudo echo "vm.max_map_count=262144" >> /etc/sysctl.conf
sysctl -w vm.max_map_count=262144
```

### 一些备忘

有些软件需要手动安装，并指定搜索路径。但是这些软件又不会在desktop的软件中出现，这是就需要稍微操作

1. 在 `$HOME/.local/share/applications/ `中创建 `.desktop`文件，具体创建方式随便找个别的文件照猫画虎即可。
2. 执行命令 `update-desktop-database $HOME/.local/share/applications/`更新

修改应用程序关联，mime文件类型，在： `/usr/share/applications/default.list`

桌面程序启动一般更新在：$XDG_DATA_DIRS/applications

命令行或一些无法设定字体的界面中的奇怪的中文字体问题解决：以管理员身份修改 `/etc/fonts/conf.avail/64-language-selector-perfer.conf` XML文件，将fontconfig.alias[sans-serif].prefer.family 加入一个喜欢的中文字体即可。寻找中文字体名称可以打开Font Manager选择（字体同名）

```xml

<fontconfig>
    <alias>
        <family>sans-seriffamily>
        <prefer>
            <family>.PingFang SCfamily>
        prefer>
    alias>
    <alias>
        <family>seriffamily>
        <perfer>perfer>
    alias>
    <alias>
        <family>monospacefamily>
        <perfer>perfer>
    alias>

fontconfig>
```

## 总结

作为主要用于开发的电脑，毫不夸张地说，Ubuntu要比Windows强不少。且不谈开发环境无限接近服务器（用WindowsServer就当我没说），程序的编译速度（C语言编译ESP32源代码，相差3倍）、文件系统与文本的兼容性都要强于Windows。可供选择的软件非常丰富，不管是ARM、AMD64或X86的架构，如果会自行编译，几乎畅通无阻。如果习惯用命令行，那种精准的指令操作以及强大的脚本编排会让你再也回不去（图形界面）。

略有不足的地方主要体现在国内的一些软件。例如会议软件，微信等。不过腾讯会议和钉钉有Linux版本可供下载。实在不幸还可以借助虚拟技术和WINE来弥补缺陷。一定要说Linux最大的缺点，就是游戏：目前可以用STEAM，但其中大部分游戏没Linux版本，就完全无法运行了。

有相当多的开发者在Windows10/11中安装WSL（1或者2）包括我，但还是受制于Windows的性能需求，会被"吃掉"不少资源。如果Linux优化够好，可以让一台7年前的轻便笔记本电脑仍可服役，换成Windows怕是只能码字。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/26d55074883d450c9e8e24b343e86166~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c314f36da1f64e2d9147831122ab3d64~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)
