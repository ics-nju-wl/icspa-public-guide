# PA 0 实验环境配置

在构建一台能计算的机器之前，我们首先要尝试将计算的对象——数据妥善地存放在计算机中。

## 步骤1. 安装并配置虚拟机

### Step 1.1. 安装虚拟机和客户操作系统

如果你当前正使用的宿主操作系统（Host OS）不是恰好为32位的GNU/Linux系统（绝大多数情况下不是），那么推荐安装虚拟机来搭建实验环境。本教程推荐使用<font color=red>VMware Player</font>作为虚拟机平台，也可以使用<font color=red>VirtualBox</font>。
VMware Player下载地址：

https://www.vmware.com/products/player/playerpro-evaluation.html

在安装过程中，忽略序列号的输入环节，使用邮箱注册后，就可以免费使用。
或，VirtualBox下载地址：

https://www.virtualbox.org/wiki/Downloads

在宿主系统中成功安装虚拟机软件后，新建虚拟机并安装客户操作系统（Guest OS）。本实验所使用的客户操作系统为GNU/Linux，推荐使用<font color=red>Debian 10</font>操作系统。下载地址为

https://www.debian.org/distrib/netinst

选择Small CDs or USB sticks项下的i386体系结构镜像，按照说明进行安装，语言选择为US English。为了将来运行的方便，在安装过程中，推荐安装桌面环境。框架代码的测试环境为MATE桌面系统，这是老的Gnome 2的一个分支，比较符合一些老用户的使用习惯，你也可以尝试使用其他的桌面环境（比如xfce）。以MATE为例，在图形化的桌面环境中，命令行终端可以通过`Applications -> System Tools -> MATE Terminal`打开。

本实验指导在 [附录C. 安装Debian操作系统的图片示意](../ch/ch_appendix_C_local_vm.md) 中提供了完整的安装过程示意图。

### Step 1.2. 配置客户操作系统

在操作系统安装完毕后，需要进行的第一个操作是将当前用户加入到sudoer list。所需要进行的具体操作步骤为：

> username@debian:~$ su - root
> 
> Password: \<input your password\>
> 
> root@debian:/home/username# adduser username sudo
> 
> root@debian:/home/username# exit

完成上述操作后，`Log Out`系统并再次登入，就可以在普通用户的身份下使用`sudo cmd`的方式执行特权命令了。

<font color=red>在此提醒大家注意：千万不要使用root用户身份来进行开发和日常的使用，应当使用普通用户身份进行工作，遇到需要执行特权命令时，使用sudo。</font>

如果你发现系统中没有sudo命令，那么可以在使用su切换到root用户后，使用命令

> root@debian: apt-get install sudo

来安装sudo。

安装完操作系统后，推荐继续安装VMware提供的`vmware-tools`。此时有两个选择，一个是使用`vmware-tools`，另一个则是使用`open-vm-tools`。

在安装之前，需要额外安装编译环境和Linux头文件包通过执行以下命令来安装相应的依赖（具体版本号可能会发生变化）

> sudo apt-get install linux-headers-4.9.0-3-all-i386 net-tools
> 
> sudo apt-get install build-essential

**选择1：安装`vmware-tools`**

`vmware-tools`针对客户操作系统提供了一些很方便的功能，如自适应调整分辨率、共享文件夹等。在安装过程中VMware Player就会提示下载安装。`vmware-tools`镜像下载完成后，在VMware Player的菜单中选择`install vmware tools...`，就可以看到在客户操作系统中挂载了一个光盘驱动器，其中就包含`vmware-tools`的安装文件。

安装好必要的依赖后，将vmware-tools光盘中的`VMwareTools-version-num.tar.gz`压缩包拷贝出来并解压，在解压得到的目录下执行命令

> sudo ./vmware-install.pl

在第一步可能会提示是否安装`open-vm-tools`，<font color=red>请忽略</font>，并坚持安装`vmware-tools`。如果没有遇到其他问题，一路选择默认选项即可。在完成安装之后，你将获得包括屏幕分辨率自动调整、与宿主系统共享剪贴板、共享文件夹等功能，有助于更方便地使用客户操作系统的功能。

**选择2：安装`open-vm-tools`（更简单一点）**

在安装好依赖之后，执行：

> sudo apt-get install open-vm-tools

> sudo apt-get install open-vm-tools-desktop

在安装完上面任意一款增强工具后，你可以在宿主系统中创建一个文件夹，并在vmware软件菜单中通过`Virtual Machine Settings -> Options -> Shared Folders`页面指定创建的文件夹为共享文件夹，在客户系统中共享文件夹位于`/mnt/hgfs/`目录下。有了共享文件夹，就可以方便地在虚拟机和宿主机之间传递文件了。

## 步骤2. 安装必要的软件

PA实验使用`GNU/Linux`平台开展实验，项目代码使用C语言编写并用gcc编译。同时依赖于一些开发库如`readline`和`SDL`。为了避免在开发过程中遇到太多麻烦，建议使用本教程所推荐的配置展开PA实验。通过以下命令安装必要的软件（使用ctrl-c拷贝下面这条命令，ctrl-shift-v粘贴到terminal中执行即可）

> sudo apt-get install build-essential libreadline-dev libsdl1.2-dev vim git tmux dialog python python-rsa openssl curl

以下为关键环境和软件包的版本，供参照：
| **类型** |**名称** |**版本** |**查看命令** |
| ---------| ----- | ----- | ------------------------------------ |
|客户操作系统	|Debian 10 buster	|OS: Debian 10 buster<br>Kernel: i686 Linux 4.19.0-5-686	|uname -a|
|编译器	|gcc	|8.3.0	|gcc -v|
|readline开发包	|libreadline-dev	|7.0-5 i386	|apt list libreadline-dev|
|SDL开发包	|libsdl1.2-dev	|1.2.15+dfsg2-4 i386	|apt list libsdl1.2-dev|

注意`SDL`的版本，我们使用老的`1.x`版本而不是新的`2.x`版本。我们推荐使用vim来进行开发，对vim不熟悉的同学可以参照其官网 提供的内容，鸟哥的Linux私房菜 也是一个不错的初学者入口。当然善用万能的Google和百度能够解决你面临的几乎所有困难。PA使用git来管理项目代码，在本教程最后有一个入门教程：`附录B. Git入门教程`。

## 步骤3. 获取框架代码

通过以下步骤完成获取PA框架代码并完成配置。

### Step 3.1. 使用git clone命令获取框架代码

目前我们将代码托管在了github.com，可以通过以下命令获取框架代码：

> git clone https://github.com/ics-nju-wl/icspa-public.git

或者克隆镜像地址（速度可能更快）：

> git clone https://gitee.com/wlicsnju/icspa-public.git

在获取成功后，会在当前目录下创建名为icspa-public的文件夹，其中包含了PA的框架代码和教程。

### Step 3.2. 配置学号

1. （可选）修改`Makefile.git`中的`STU_ID`为你的学号
2. 执行以下命令对目录进行clean并测试必要的环境配置情况

> make clean

### Step 3.3. 对框架代码进行浏览

下面我们来对框架代码进行整体浏览。PA整体的框架代码结构树节选如下图所示（更完整的树形结构可以通过`tree`命令查看，你需要通过`apt-get`来安装`tree`命令）。其中，`nemu`就是我们要重点实现的模拟器相关的代码；`testcase`中包含了一系列以用户程序的形式呈现的测试用例；而`kernel`则是一个简单的操作系统，用于配合之后的相关功能实现。在PA1的阶段，我们仅需要关注`nemu`中的项目代码和测试用例；在PA2初期我们引入`testcase`，并在PA2的后期引入`kernel`。

```
icspa-public/
├── game                   // 包含游戏相关代码
├── include                // PA整体依赖的一些文件
│   ├── config.h          // 一些配置用的宏
│   └── trap.h            // trap相关定义，不可改动
├── kernel                 // 一个微型操作系统内核
├── Makefile               // 帮助编译和执行工程的Makefile
├── Makefile.git           // 和git有关的部分
├── nemu                   // NEMU
│   └── src
│       └── main.c        // NEMU入口
└── testcase               // 测试用例
└── scripts                // 框架代码功能脚本，其中包含objdump4nemu-i386反汇编工具
└── libs                   // 框架代码所使用的库，不可改动
└── docs                   // 包含本教程
```


|完成以上步骤之后，就可以开始做你的PA实验了<br>✿✿ヽ(°▽°)ノ✿|
|:----:|

