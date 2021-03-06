---
title: "linux"
date: 2016-5-20 15:01:00
---
[TOC]

## 改变所属群组, chgrp

**适用于Liunx系列系统**

```
chgrp [-R] dirname/filename ...
```

`-R` : 意为递归修改

## 改变文件拥有者, chown

**适用于Liunx系列系统**

```
chown [-R] 账号名称 文件或目录
chown [-R] 账号名称:组名 文件或目录
```

`-R` : 意为递归修改

可以同时修改文件所有者和所属群组

## 改变权限, chmod

**适用于Liunx系列系统**

Linux文件的基本权限就有九个，分别是owner/group/others三种身份各有自己的read/write/execute权限。

### 数字形式修改权限

我们可以使用数字来代表各个权限，各权限的分数对照表如下：

|权限|对应数字|
|-|-|
|r|4|
|w|2|
|x|1|

每种身份(owner/group/others)各自的三个权限(r/w/x)分数是需要累加的，例如当权限为： `[-rwxrwx---]` 分数则是：

owner = rwx = 4+2+1 = 7

group = rwx = 4+2+1 = 7

others= --- = 0+0+0 = 0

变更文件权限的命令为：

```
chmod [-R] xyz 文件或目录
```

### 字母形式修改权限

|命令|用户类型|修改操作|权限类型|内容|
|-|-|-|-|-|
|chmod|u<br>g<br>o<br>a|+(加入)<br>-(除去)<br>=(设定)|r<br>w<br>x|文件或目录|

样例：

```
chmod  u=rwx,go=rx  .bashrc
# -rwxr-xr-x .bashrc
chmod  a+w  .bashrc
# -rwxrwxrwx .bashrc
```


## 安装博通 BCM43142 网卡驱动

**适用于 Fedora 23**

> 首先应当确认自己的网卡型号： `lspci | grep Network`
> 如果后面的芯片型号包括 BCM4312，BCM43142 等可以应用本教程

首先需要添加 RPMFusion 以支持添加更多软件来源：

Fedora 22 and later:

```bash
su -c 'dnf install http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm'
```

更多版本请参见： http://rpmfusion.org/Configuration

然后执行下列命令以安装需要的包：

```bash
sudo dnf install kernel-devel akmod-wl
```

然后执行下列命令以重新编译内核模块：

```bash
sudo akmods
```

然后执行下列命令以加载内核代码：

```bash
sudo modprobe wl
```

最后重启网络管理器：

```bash
sudo systemctl restart NetworkManager
```

## 隐藏标题栏后如何取消最大化

**适用于 Gnome 3.14**

- alt+space可以唤出相关选单，取消最大化即可
- 使用鼠标自顶向下拖动即可解除最大化

## 修改快捷链接

**适用于 Gnome 3.14**

有时候会出现快捷链接方式错误的现象，这个需要进入`/usr/share/applications`文件夹寻找对应的`.desktop`文件来修改。

`.desktop`文件只是一个文本文档，大致内容如下：

```
[Desktop Entry]
Version=1.0
Type=Application
Name=PyCharm
Icon=/home/xuanwo/Soft/pycharm-5.0.4/bin/pycharm.png # 指定图标
Exec="/home/xuanwo/Soft/pycharm-5.0.4/bin/pycharm.sh" %f # 指定可执行文件
Comment=Develop with pleasure!
Categories=Development;IDE; # 指定分类
Terminal=false
StartupWMClass=jetbrains-pycharm
```

只需要按照正确的地址修改链接即可~

## 桌面快捷方式位置

**适用于 Gnome 3.14**

桌面快捷方式文件放置于/usr/share/applications或者~/.local/share/applications处。
前一个目录存储的桌面快捷方式每个用户都可以使用，而后一个目录则含有仅仅为特定用户创建的快捷方式。

## 解决/usr/bin/ld: cannot find -lxxx错误

**适用于Linux系列系统**

### 原因

1. 系统没有安装相对应的lib
1. 相对应的lib版本不对
1. lib(.so档)的符号链接不正确，没有连结到正确的函式库文件(.so)

### 解决方案

1. 先判断在`/usr/lib`下的相对应的函式库文件(.so)的符号链接是否正确，若不正确改成正确的连结目标即可解决问题。
1. 若不是符号链接的问题引起，而是系统缺少相对应的lib，则安装lib即可解决。

### 样例

> 包名格式
> `lxxx`对应so档一般为`libxxx.so`，对应apt软件包一般为`libxxx-dev`

**符号链接不正确**
```
/usr/bin/ld: cannot find -lXtst
```
检查发现存在`libXtst.so.6`，使用命令`ln -s libXtst.so.6 libXtst.so`建立软连接即可。

**未安装对应包**
```
/usr/bin/ld: cannot find -lc
```
不是符号链接的问题，则使用命令`apt-get install libc-dev`

## 解决fatal error: Python.h: No such file or directory错误

**适用于 Ubuntu**

安装指定的Python版本

```
apt-get install python3.5-dev
```

*具体版本取决于编译需要*

## aira2及其前端的配置

**适用于 Ubuntu**

### 安装aria2

```bash
sudo apt-get install aria2
```
### 前端配置

可以选择uget或者Web化前端，uget直接在设置中开启即可，此处主要介绍一下前端配置。

首先clone代码：

```bash
git clone https://github.com/ziahamza/webui-aria2.git
```

然后创建`~/.aria2/aria2.conf`，内容请参考[此处](http://aria2c.com/usage.html)

根据自己的需要修改即可，需要注意的有以下地方：

- 涉及到地址的地方最好使用绝对路径，相对路径会出现找不到文件之类的奇怪问题
- 不支持中文路径
- 按照自己的需要设置`rpc-secre`并修改`webui-aria2`路径下的`configuration.js`文件（*直接在浏览器中填写的话，无法保存*）

### 后台启动

首先使用`aria2c`命令尝试启动，若测试一切正常，则可以使用`aria2c -D`命令使其在后台运行。

### 开机自启

在Ubuntu上可以通过修改`/etc/rc.local`的方式设置开机自启，只需要填写自己需要的命令即可。使用Sudo权限打开文件，在`exit 0`的上方加入：

```
/usr/bin/aria2c --conf-path=/home/xuanwo/.aria2/aria2.conf &
```

按照自己的需求修改，需要注意以下地方：

- `rc.local`在启动时没有初始化path，故aria2c不会自动加载配置文件，需要自己手动指定配置文件地址并使用绝对地址运行aria2c，否则无法正确开机自启。
- `-D`参数错误退出是不会输出stderr信息，推荐使用`&`来让进程运行于后台
- 一定要确保命令正确退出，否则，轻则程序没有正确自启，重则无法进入桌面

> 如果发现没有正确自启，可以使用`systemctl status re.loacl.service -l`命令查看输出的信息。


## Ubuntu支持Cisco Anyconnent VPN

**适用于 Ubuntu 15.04 **

由于Cisco的Anyconnect是一个私有的程序，只有在部署了Cisco VPN的地方才能够下载到相应的程序。因此我们可以转而使用开源的Openconnent：

```
sudo apt-get install network-manager-openconnect-gnome
```

安装完毕后，在网络管理器中添加网络-VPN，就能看到关于`Cisco Anyconnect兼容的VPN`选项。

## JetBrains开发工具的安装

**适用于 Linux 系列系统**

在官网上下载压缩包，在合适的位置解压缩，然后运行`bin`文件夹中对应sh文件即可。

> 运行完毕后建议不要在移动文件夹，否则自动创建的快捷方式会失效，需要自行修改位置。


## 查看端口占用情况

**适用于 Linux 系列系统**

```bash
netstat -ap | grep <port>
```

使用过程中经常出现端口被占用导致无法启动程序的情况，只要使用此命令查看端口占用，然后使用`kill`杀死占用端口的进程即可

## 查看运行中的进程

**适用于 Linux 系列系统**

```bash
ps -aux
```
> 需要注意的是，只会显示当前用户开启的进程，如果想查看所有进程，需要添加`sudo`

## 杀死某个僵死的进程

**适用于 Linux 系列系统**

```bash
kill pid
```

> 在ZSH中输入`kill`然后敲击`tab`，会出现所有运行中的进程，输入名字即可自动转换为pid。

## 安装英伟达私有驱动并进行双显卡切换

**适用于 Ubuntu 系统**

安装英伟达的驱动有两种方法：

- 执行`sudo apt-get install nvidia-<version>`
- 在`附加驱动`中勾选NVIDIA闭源驱动

这两种方法本质上是一样的，当前的最新版本号为352。
安装完成后，在程序菜单中找到`NVIDIA X Server Setting`，然后在`PRIME Setting`中可以选择是使用`低能耗（Intel）`还是`高性能（NVIDIA）`模式。勾选完成后重启即可。

> 按照程序的提示是只需要logout即可，但是我测试之后发现无法正常进入桌面，所以还是直接重启吧~

## 转换MP3编码

**适用于 Ubuntu 系统**

转换MP3编码可以使用`EasyTag`。
```
sudo apt-get install easytag
```
然后选择乱码的MP3，读取使用对应的语言，然后写入为`UTF-8`即可。

## 在后台运行程序

**适用于 Linux 系列系统**

- `<command> &`：后台运行
- `Ctrl+z`： 将命令转到后台并停止运行
- `jobs`：查看后台运行的任务
- `fg %<id>`：将后台任务调至前台运行
- `bg %<id>`：让后台任务从停止状态转为运行状态用

**注意，以上所有的后台任务在退出当前Shell时都会中断**，如果想在退出Shell或者SSH后，任务依然执行，可以使用下列命令：

- `(<command> &)`：将命令使用括号包裹，使其在另一个shell中运行
- `nohup <command> &`：在命令之前加`nohup`，使得命令不受中断信号影响

## 通过SSH传输文件

**适用于 Linux 系列系统**

只需要使用scp命令：

```bash
scp source target
```

这里的源和目标均支持文件和目录，如果复制的是一个目录，需要在scp后添加`-r`参数。

## 将密钥加入SSH-Agent

**适用于 Linux 系列系统**

```bash
ssh-add + <private key>
```

## 查看内核

**适用于 Linux 系列系统**

```bash
uname -a
```

## 修改分屏分辨率

**适用于 Linux 系列系统**

执行如下命令，显示目前显示器支持的分辨率：
```
xrandr
```
结果如下：
```
Screen 0: minimum 8 x 8, current 3286 x 1080, maximum 32767 x 32767
LVDS1 connected primary 1366x768+0+0 (normal left inverted right x axis y axis) 344mm x 194mm
   1366x768      60.01*+
   1360x768      59.80    59.96
   1280x720      60.00
   1024x768      60.00
   1024x576      60.00
   960x540       60.00
   800x600       60.32    56.25
   864x486       60.00
   640x480       59.94
   720x405       60.00
   680x384       60.00
   640x360       60.00
DP1 disconnected (normal left inverted right x axis y axis)
DP2 disconnected (normal left inverted right x axis y axis)
HDMI1 disconnected (normal left inverted right x axis y axis)
HDMI2 disconnected (normal left inverted right x axis y axis)
VGA1 connected 1920x1080+1366+0 (normal left inverted right x axis y axis) 0mm x 0mm
   1024x768      60.00
   800x600       60.32    56.25
   848x480       60.00
   640x480       59.94
VIRTUAL1 disconnected (normal left inverted right x axis y axis)
```

我们可以看到显示器目前是不支持1920x1080分辨率的，因此我们需要首先自行生成相关分辨率的配置参数：
```
cvt 1920 1080
```
输出如下：
```
# 1920x1080 59.96 Hz (CVT 2.07M9) hsync: 67.16 kHz; pclk: 173.00 MHz
Modeline "1920x1080_60.00"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync
```

然后新建显示模式并添加到配置列表中去：
```
sudo xrandr --newmode "1920x1080_60.00"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync
sudo xrandr --addmode VGA1 "1920x1080_60.00"
```

然后我们就可以设定显示屏的分辨率了：
```
xrandr --output VGA1 --mode 1920x1080_60.00
```

如果担心自己设定的分辨率不支持，可以再追加一个`sleep`命令以保证不会困在无效的分辨率下：

```
xrandr --output VGA1 --mode 1920x1080_60.00 && sleep 5
```

此配置仅在当前会话下生效，如果需要永久生效可以考虑修改`xorg.conf`或者设定一个开机执行的脚本。

> 已知问题：
> 使用英伟达的官方驱动会导致分屏分辨率切换异常，暂时无解，使用Intel的Power Save模式可以正常开启分屏

## 去除恼人的错误报告提示

**适用于 Ubuntu 15.04 系列系统**

Ubuntu非常令人讨厌的一点就是时不时弹出的错误报告，又不能给出有效的诊断信息，还总是打乱自己当前正在运行的全屏程序，使用下列命令可以去除这个错误报告提示：

```
sudo apt-get purge apport*
```

## 修改网卡的物理地址

**适用于 Linux 系列系统**

在某些需要网卡物理地址和IP绑定的情形下，会有需要修改网卡物理地址的需求，可以通过如下命令进行修改：

```bash
sudo ifconfig enp5s0 down
sudo ifconfig enp5s0 hw ether XX:XX:XX:XX:XX:XX
sudo ifconfig enp5s0 up
```

## 目录下有.fuse开头的隐藏文件

`.fuse_hiddenXXXX`的隐藏文件表示当前目录正在被某个程序使用，如果需要删除当前文件夹，可以使用

```
lsof /just/the/directory/path
```

来查看当前目录的使用情况，使用

```
kill PID
```

来关闭占用当前目录的程序即可。

## 美化字体

**适用于Fedora23，24**

> 原来的infinality包似乎已经不再维护也没有地方下载了，有热心人重新制作了一个`Infinality Ultimate Bundle`，试用之后感觉效果非常不错。

首先添加相关的repo：

```bash
dnf install http://rpm.danielrenninghoff.com/infinality/fedora/$(rpm -E %fedora)/noarch/infinality-ultimate-repo-$(rpm -E %fedora)-1.noarch.rpm
```

然后安装打过补丁的字体和字体设定以及freetype包：

```bash
dnf install --allowerasing cairo-infinality-ultimate fontconfig-infinality-ultimate freetype-infinality-ultimate
```

如果之前安装过`devel`的包，开发者在库里面同样提供了对应的包，只需要在包名后添加`*-infinality-ultimate-devel`即可。

为了最好的效果，你还可以安装附加的字体包：

```bash
dnf install --allowerasing ibfonts-meta-base ibfonts-meta-extended-lt ibfonts-meta-extended
```

> 某些字体在fedora23升级24时出现冲突，删除即可。

对于试用java的应用，开发者提供了打过补丁的jdk-1.8.0以提供良好的字体渲染。

```bash
dnf install --allowerasing java-1.8.0-openjdk-infinality-ultimate-headless
```

对于某些使用自带jre的软件，比如说`Jetbrains`家的IDE，需要指定它使用系统的jre。对于`Jetbrains`来说，就是删除它自带的jre目录。


## NTFS分区下文件无法正常删除

NTFS设置自动挂载之后发现无法在图形界面下正常删除，但是通过Shell可以。这个问题是因为Gnome在删除文件时会首先将文件移动到`.Trash`目录下，如果没有权限创建`.Trash`目录就会导致删除失败。所以需要修改NTFS分区挂载时的参数：

```bash
nosuid,nodev,nofail,x-gvfs-show,rw,uid=1000,gid=users
```

> 其中，uid可以通过命令`id -u`获得

## 缺少RPM-GPG-KEY-fedora-x86_64文件

从Fedora 23 更新至 Fedora 24 时提示：

```
GPG key retrieval failed: [Errno 14] curl#37 - "Couldn't open file /etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-x86_64"
```

或者：

```
Downloading failed: GPG key retrieval failed: [Errno 14] curl#37 - "Couldn't open file /etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-x86_64"
```

只需要进入`/etc/pki/rpm-gpg/`然后引入对应密钥即可，比如：

```
rpmkeys --import /etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-24-x86_64
```

## 重定向Shell标准输入，输出和错误

在Linux系统中每一个进程都会有对应的三个打开的文件：输入，输出和错误。

|文件|文件描述符|默认值|
|-|-|-|
|标准输入|0|键盘|
|标准输出|1|屏幕|
|标准错误|2|屏幕|

然后我们可以通过这样的形式来重定向一个进程的输入，输出和错误

|命令|说明|
|-|-|
|command > file|将输出重定向到 file。|
|command < file|将输入重定向到 file。|
|command >> file|将输出以追加的方式重定向到 file。|
|n > file|将文件描述符为 n 的文件重定向到 file。|
|n >> file|将文件描述符为 n 的文件以追加的方式重定向到 file。|
|n >& m|将输出文件 m 和 n 合并。|
|n <& m|将输入文件 m 和 n 合并。|
|<< tag|将开始标记 tag 和结束标记 tag 之间的内容作为输入。|

于是我们可以使用这样的技巧：

### 将错误信息重定向到空设备

```bash
command 2>/dev/null
```

### 将标准输出和错误重定向到指定文件

```bash
command > out.put 2>&1
```

## 创建AP热点

> 要求无线网卡的驱动支持AP模式，Intel基本上都支持，博通很大概率不支持

其实网上有很多创建AP热点的教程，但是有一个通病就是操作比较繁琐，控制不太方便，本条目使用`create_ap`脚本来开启热点，并使用`systemd`来管理热点。

首先检查自己的依赖：

- bash (to run this script)
- util-linux (for getopt)
- procps or procps-ng
- hostapd
- iproute2
- iw
- iwconfig (you only need this if 'iw' can not recognize your adapter)
- haveged (optional)
- dnsmasq
- iptables

解决依赖问题之后就可以下载脚本并安装了：

```
git clone https://github.com/oblique/create_ap.git
cd create_ap
make install
```

其中`make install`需要管理员权限。

然后修改`create_ap.service`脚本以便使用`systemd`进行管理。

使用vim打开配置文件：

```
vim /lib/systemd/system/create_ap.service
```

将

```
ExecStart=/usr/bin/create_ap -n -g 10.0.0.1 wlan0 AccessPointSSID
```

修改为：

```
ExecStart=/usr/bin/create_ap <Wifi_name> <Ethernet_name> <SSID> <Password>
```

其中Wifi名是你的无线网卡设备名，有线网卡名是你的有线网卡设备名，这些可以通过`ifconfig`查询到。
SSID就是最后开启的热点名称，Password就是Wifi的密码。

配置好这些后，使用如下命令来进行管理：

- `systemctl start create_ap`   开启热点
- `systemctl stop create_ap`   关闭热点
- `systemctl status create_ap`   查看热点的状态
- `systemctl enable create_ap`   开机自动启动热点


## 为Unity增加系统监视器

`[indicator-sysmonitor](https://github.com/fossfreedom/indicator-sysmonitor)`可以为Unity的状态栏添加一个系统监视器。

比较简单的方式是通过PPA安装：

```
sudo add-apt-repository ppa:fossfreedom/indicator-sysmonitor
sudo apt-get update
sudo apt-get install indicator-sysmonitor
```

也可以自行Clone之后安装：

```
git clone https://github.com/fossfreedom/indicator-sysmonitor.git
cd indicator-sysmonitor
sudo make install
```
