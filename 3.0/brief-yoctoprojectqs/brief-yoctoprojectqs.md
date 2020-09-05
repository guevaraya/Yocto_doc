[#]: translator: (guevaraya)
[#]: reviewer: ( )
[#]: publisher: ( )
[#]: url: ( https://www.yoctoproject.org/docs/3.0/brief-yoctoprojectqs/brief-yoctoprojectqs.html)
[#]: subject: (Yocto Project Quick Build)

快速构建 Yocto 项目
======
版权所有@ 2010-2019 Linux 基金会

此文允许被复制，分发以及或者修改此文档，但需要遵循知识共享组织发布的共享协议[2.0 UK: England & Wales][1]条款

热烈欢迎！
======
热烈欢迎！这篇简短的文档指导你通过 Yocto 完成一个典型镜像的构建。文档也介绍了如何为特定硬件的配置构建。你也可以用 Yocto 构建一个 叫Poky的嵌入式参考系统。

>__提示__
>
>+ 本文的例子假定你本地使用的 Linux 系统是最新的 Ubuntu 发行版。如果用 Yocto 项目构建一个镜像（[主机端][2]）的机器不是原生的 Linux 系统，你可以仍旧通过 Poky容器在跨平台（CROPS）基础上完成这三步。可在 Yocto 项目的开发任务手册的[“设置使用交叉平台（CROPS）”][3]段落查看更多信息。
>+ 不能使用[Windows子系统Linux（WSL）][4]构建主机。Yocto 项目不兼容 WSL。 

如果你想了解更多关于 Yocto 项目的概念或背景信息，请参照 Yocto 项目的概述和基础手册。

可兼容的 Linux 发行版
======

请确保你的[主机][5]满足一下需求：
+ 50 GB 的剩余空间
+ 可支持的 Linux 发行版（例如：最新的发行版Fedora，openSUSE，CentOS，Debian，或 Ubuntu）。支持 Yocto 项目的 Linux 发行版列表，请见：Yocto 项目的开发任务手册的[Linux发行版][6]章节。
+ Git 1.8.3.1 以上版本
+ tar 1.27 以上版本
+ Python 3.4.0 以上版本
如果构建的主机不满足以上列表任一版本要求，你可采取一定措施构建系统确保可以使用 Yocto 项目。可参看 Yocto 项目参考手册的[GIT,Tar Python版本需求]章节获取更多信息。

构建主机应用
======

You must install essential host packages on your build host. The following command installs the host packages based on an Ubuntu distribution:
你需要安装一些必要的主机应用。下面是在 Ubuntu 发行版上安装主机应用的命令：

>__提示__
>
> 在所有兼容的 Linux 发行版上需要安装的主机应用，请见 Yocto 项目参考手册的[“构建主机的必要应用”][6]章节。
``` 
$ sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib \
     build-essential chrpath socat cpio python python3 python3-pip python3-pexpect \
     xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev \
     pylint3 xterm
``` 
使用 Git 克隆 Poky
======

一旦你的机器按照安装说明已经完成，你需要在你的构建主机上获取一个 Poky 仓库的拷贝。用以下命令拷贝 Poky 仓。
``` 
     $ git clone git://git.yoctoproject.org/poky
     Cloning into 'poky'...
     remote: Counting objects: 432160, done.
     remote: Compressing objects: 100% (102056/102056), done.
     remote: Total 432160 (delta 323116), reused 432037 (delta 323000)
     Receiving objects: 100% (432160/432160), 153.81 MiB | 8.54 MiB/s, done.
     Resolving deltas: 100% (323116/323116), done.
     Checking connectivity... done.
```          
转到 poky 目录下并查看所有标签：
```
     $ cd poky
     $ git fetch --tags
     $ git tag
     1.1_M1.final
     1.1_M1.rc1
     1.1_M1.rc2
     1.1_M2.final
     1.1_M2.rc1
        .
        .
        .
     yocto-2.5
     yocto-2.5.1
     yocto-2.5.2
     yocto-2.6
     yocto-2.6.1
     yocto-2.6.2
     yocto-2.7
     yocto_1.5_M5.rc8
 ```           
 
例如，检出 yocto-3.0 的分支：

```
     $ git checkout tags/yocto-3.0 -b my-yocto-3.0
     Switched to a new branch 'my-yocto-3.0'
```

上面的 Git 检出命令创建了一个名为 my-yocto-3.0 的本地分支。这个分支的文件与 yocto 项目 yocto-3.0 的 “zeus” 开发分支的文件完全一样。

获取更多 Yocto 项目有关仓库的选项和信息，请见 Yocto 项目开发任务手册的[“本地 Yocto 项目源码文件”][7]章节。

构建你的镜像
======

用下面命令构建你的镜像。构建过程是从源码创建了一个完成的 Linux 发行版，包括工具链。
>__提示__
>
>+ 如果你的构建主机存在防火墙且没有代理，你可能会遇到编译过程中无法获取源码的问题（如：获取失败或 Git 错误）。
>+ 如果你不知道你的代理如何设置，咨询你的本地网络提供商获取这方面信息。检查浏览器的网络配置也可以试一下。实在不行，你可以在 Yocto 项目的 Wiki 的[“网络代理配置”][8]页面获取更多信息。

__1. 初始化构建环境：__ 在 Poky 目录下, 运行环境配置脚本 [oe-init-build-env][9] 定义 Yocto 项目的构建变量。
 ```
$ cd ~/poky
     $ source oe-init-build-env
     You had no conf/local.conf file. This configuration file has therefore been
     created for you with some default values. You may wish to edit it to, for
     example, select a different MACHINE (target hardware). See conf/local.conf
     for more information as common configuration options are commented.

     You had no conf/bblayers.conf file. This configuration file has therefore been
     created for you with some default values. To add additional metadata layers
     into your configuration please add entries to conf/bblayers.conf.

     The Yocto Project has extensive documentation about OE including a reference
     manual which can be found at:
         http://yoctoproject.org/documentation

     For more information about OpenEmbedded see their website:
         http://www.openembedded.org/


     ### Shell environment set up for builds. ###

     You can now run 'bitbake <target>'

     Common targets are:
         core-image-minimal
         core-image-sato
         meta-toolchain
         meta-ide-support

     You can also run generated qemu images with a command like 'runqemu qemux86'
 ```
 除此之外，本例子中的这个脚本创建一个[构建目录][10]，位置在[源码目录][11]的 build 下。当脚本运行后，当前的工作目录被设置为构建目录。后面当构建完成后，构建目录包含所有构建动态创建的文件。
 
 
__2. 检查你的本地配置文件:__ 构建环境配置了之后，一个位于构建目录的 conf 目录下叫 local.conf 的本地配置文件就可以生效了。对于本例，默认构建的目标机器是 qemux86, 这个适合模拟。包管理器使用 RPM 包管理器。

>__提示__
>
>你可以用镜像显著的加速你的构建并解决获取失败问题。增加这下面几行到构建目录的 local.conf 来使用镜像下载：
>     SSTATE_MIRRORS = "\
>     file://.* http://sstate.yoctoproject.org/dev/PATH;downloadfilename=PATH \n \
>     file://.* http://sstate.yoctoproject.org/2.7/PATH;downloadfilename=PATH \n \
>     file://.* http://sstate.yoctoproject.org/3.0/PATH;downloadfilename=PATH \n \
>     "                      
> 上面的例子展示了如何为 Yocto 项目 2.7,3.0和开发域添加 sstate 路径。有关 sstate 位置的完整索引，请参照 [http://sstate.yoctoproject.org/][12]。

__3. 开始构建：__ 用下面的命令接着为目录机器构建一个系统镜像，本例的镜像名为 core-image-sato：
> $ bitbake core-image-sato
关于 bitebake 命令的信息，请参考Yocto 项目概述和概要手册的 “[Bitebake][13]”章节，或查看 BiteBak 用户手册的 “[Bitebak命令][14]”章节。

__4. 用 QEMU 模拟你的镜像：__ 一旦你的镜像构建后，你需要运行 QEMU，这是一个 yocto 项目自带的快速模拟器：
```
     $ runqemu qemux86
```
如果你想学习更多关于运行 QEMU 的信息，请见Yocto 项目开发任务手册的 “[使用快速模拟器（QEMU）][15]”章节。

__5. 退出 QEMU：__ 通过单击关机图标或在QEMU的文本框输入 CtrL-C 键退出 QEMU

为特定机器定制构建
======

目前为止，您所做的是构建了一个仅用于模拟的镜像。本段介绍如何在 Yocto 项目的开发环境中为特定的硬件定制硬件配置层。
大体上说，层就是包含一套相关的指令集和配置信息，这些指令和配置告诉 Yocto 项目具体要做什么。将相关的元数据根据功能隔离出特定的层有助于模块化开发，并可以使层的元数据很容易复用。
>__提示__
>
>为了简便，层名字均以 “meta-”开头。

下面三步是介绍如何增加一个硬件层：

1.__找到一个层：__ 已经存在很多硬件层。[Yotco 项目源码仓][16]已经有很多硬件层。这个例子增加一名叫 [meta-altera][17] 的硬件层。

2.__克隆一个层__ 用 Git 在您的机器上制作一个本地拷贝。你可以把拷贝放到 Poky 仓的主目录里。
```
     $ cd ~/poky
     $ git clone https://github.com/kraj/meta-altera.git
     Cloning into 'meta-altera'...
     remote: Counting objects: 25170, done.
     remote: Compressing objects: 100% (350/350), done.
     remote: Total 25170 (delta 645), reused 719 (delta 538), pack-reused 24219
     Receiving objects: 100% (25170/25170), 41.02 MiB | 1.64 MiB/s, done.
     Resolving deltas: 100% (13385/13385), done.
     Checking connectivity... done.
```
现在硬件层已经和其他 Poky 参考仓一块作为构建主机的元数据，它包含了所有来自 Altera（属于英特尔）的支持硬件的元数据。

3.__修改特定硬件的配置：__ 构建特定机器的[MACHINE][18]变量在 local.conf 文件中。例如，设置 MACHINE 变量为 “cyclone5”。这个配置信息就生效了：https://github.com/kraj/meta-altera/blob/master/conf/machine/cyclone5.conf。
>__提示__
>
> 参照[“检查您的配置文件”][19]步骤里可以很容易的构建配置文件。

4. __增加你的层到配置层文件：__ 您可以在构建中使用层之前，您需要添加您的 bblayer.conf 文件，这个在[编译路径][20]的 conf 目录下。

使用 bitbake-layers add-layer  命令添加层到配置文件中：
```
     $ cd ~/poky/build
     $ bitbake-layers add-layer ../meta-altera
     NOTE: Starting bitbake server...
     Parsing recipes: 100% |##################################################################| Time: 0:00:32
     Parsing of 918 .bb files complete (0 cached, 918 parsed). 1401 targets, 123 skipped, 0 masked, 0 errors.
 ```
您可查看更多增加层的信息，请见“[用 bitebake-layer 脚本添加一个层][21]”章节。

完成这些步骤就增加了 meta-alera 层到您的 Yocto 项目开发环境并且完成 “cyclone5”机器的构建配置。

>__提示__
>
>以上步骤仅用于演示目的。如果您想尝试为“cyclone5”构建一个镜像，你可以阅读 Altera 的 README。

创建自己的通用层
======
可能您有一个应用程序或者特定的一些行为需要隔离。您可以用 bitbake-layers create-layer 命令创建您的通用层。这个工具通过带有配置文件 layer.conf 的子目录自动创建层，这个 recipes-example 子目录包含 example.bb 配方，一个授权文件和 README。

下面命令就是通过工具在 poky 目录创建一个名叫 meta-mylayer的层：
```
     $ cd ~/poky
     $ bitbake-layers create-layer meta-mylayer
     NOTE: Starting bitbake server...
     Add your new layer with 'bitbake-layers add-layer meta-mylayer'
 ```
 获取更多关于如何创建层，请见 用 Yocto 项目开发任务手册的“[bitbake-layer 脚本创建通用层][22]”章节。


下一步该做什么
======

现在您已经有了使用 Yocto 项目的经验，您可能会自问“现在该做什么？” Yocto 项目有很多源码信息包括网站，wiki页面和用户手册：

+ __网站：__ [Yocto 项目的官网][23]提供了背景资料，最新的构建，重大新闻，完整的开发文档和可以融入的丰富的项目开发社区。
+ __开发者视频教程：__ [Yocto 项目入门视频][24]-为不熟悉 Yocto 项目但熟悉 Linux 主机构建的用户提供了 30分钟时长的新开发者视频教程。虽然这个视频有些过时，但是对于初学者来说上面的基本概念和介绍都是有用的。
+ __Yocto 项目概述和概念手册：__ [Yocto 项目概述和概念手册][25]是一个对学习 Yocto 项目非常有用的地方。手册介绍了 Yocto 项目和其开发环境。同时也提供了 Yocto 项目有关各方面的概念信息。
+ __Yocto项目WIKI:__ [Yocto 项目WIKI][26]提供了进一步深入了解 Yocto 项目的额外信息，如发布信息，项目计划和质量信息。
+ __Yocto项目邮件列表:__ 相关的邮件列表提供了讨论，补丁提交和公告的论坛。多个邮件列表根据大家关心的不同领域分组。查看Yocto 项目参考手册的“[邮件列表][27]”章节可获取完整的邮件列表信息。
+ __连接和其他文档的综合列表:__ Yocto 项目参考手册的“[链接和相关文档][28]”章节提供了所有相关链接和其他用户文档的综合列表。


via: https://www.yoctoproject.org/docs/3.0/brief-yoctoprojectqs/brief-yoctoprojectqs.html

译者：[guevara](https://github.com/guevaraya)
校对：[校对者ID](https://github.com/校对者ID)
版权所有© 2010-2019 Linux 基金会

[1]: https://creativecommons.org/licenses/by-sa/2.0/uk/
[2]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#hardware-build-system-term
[3]: http://www.yoctoproject.org/docs/3.0/dev-manual/dev-manual.html#setting-up-to-use-crops
[4]: https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux
[5]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#hardware-build-system-term
[6]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#required-packages-for-the-build-host
[7]: http://www.yoctoproject.org/docs/3.0/dev-manual/dev-manual.html#locating-yocto-project-source-files
[8]: https://wiki.yoctoproject.org/wiki/Working_Behind_a_Network_Proxy
[9]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#structure-core-script
[10]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#build-directory
[11]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#source-directory
[12]: http://sstate.yoctoproject.org/
[13]: http://www.yoctoproject.org/docs/3.0/overview-manual/overview-manual.html#usingpoky-components-bitbake
[14]: http://www.yoctoproject.org/docs/3.0/bitbake-user-manual/bitbake-user-manual.html#bitbake-user-manual-command
[15]: http://www.yoctoproject.org/docs/3.0/dev-manual/dev-manual.html#dev-manual-qemu
[16]: http://git.yoctoproject.org/
[17]: https://github.com/kraj/meta-altera
[18]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#var-MACHINE
[19]: https://www.yoctoproject.org/docs/3.0/brief-yoctoprojectqs/brief-yoctoprojectqs.html#conf-file-step
[20]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#build-directory
[21]: http://www.yoctoproject.org/docs/3.0/dev-manual/dev-manual.html#adding-a-layer-using-the-bitbake-layers-script
[22]: http://www.yoctoproject.org/docs/3.0.1/dev-manual/dev-manual.html#creating-a-general-layer-using-the-bitbake-layers-script
[23]: http://www.yoctoproject.org/
[24]: http://vimeo.com/36450321
[25]: http://www.yoctoproject.org/docs/3.0.1/overview-manual/overview-manual.html
[26]: https://wiki.yoctoproject.org/
[27]: http://www.yoctoproject.org/docs/3.0.1/ref-manual/ref-manual.html#resources-mailinglist
[28]: http://www.yoctoproject.org/docs/3.0.1/ref-manual/ref-manual.html#resources-links-and-related-documentation


