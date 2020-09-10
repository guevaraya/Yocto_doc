Yocto 项目应用程序开发和可扩展软件开发工具包（eSDK）
===

<p align="right" >Scott Rifenbark<br>
Scotty 文档服务公司<br>
 <a href="mailto:srifenbark@gmail.com"><font size=100>&lt;srifenbark@gmail.com&gt;</font></a>
 </p>

版权所有 © 2010-2019 Linux 基金会

此文允许被复制，分发以及或者修改此文档，但需要遵循知识共享组织发布的共享协议[2.0 UK: England & Wales][1]条款

>本文要点
>* Yocto 项目应用程序开发和可扩展软件开发工具包（eSDK）这个手册对应 Yocto 发行版版 3.0。请确保你使用的这个发行版手册是最新的，可去 [Yocto 项目文档网站页面][2]选择查看。网站上的手册会比从 Yocto 项目发行版内置的 Tar 包文件新。
>* 如果通过网页搜索到本手册，手册的版本可能不是你期望的（例如：检索出来是一个比你用的更旧的 Yocto 项目版本）。你可以通过访问[正式发行版页面][3]查看所有的 Yocto 项目主要发行版。如果需要不同 Yocto 项目发行版对应的本手册版本，可访问 [Yocto 项目文档网站页面][2]，然后点击下拉框“正在发行中的文档”或“已归档文档”来选择手册。
>* 如提交本手册不严谨或问题之处，请发邮件 yocto@yoctoproject.com 到 Yocto 项目讨论组或者登陆 freenode 的 #yocto 频道。


|**修订历史**|||
|-|-|-|
|版本 2.1| 2016年4月|同步发布 Yocto 项目 2.1 版本|
|版本 2.2| 2016年10月|同步发布 Yocto 项目 2.2 版本|
|版本 2.3| 2017年6月|同步发布 Yocto 项目 2.3 版本|
|版本 2.4| 2017年10月|同步发布 Yocto 项目 2.4 版本|
|版本 2.5| 2018年10月|同步发布 Yocto 项目 2.5 版本|
|版本 2.6| 2018年11月|同步发布 Yocto 项目 2.6 版本|
|版本 2.7| 2019年6月|同步发布 Yocto 项目 2.7 版本|
|版本 3.0| 2019年10月|同步发布 Yocto 项目 3.0 版本|

**目录**
<!-- GFM-TOC -->
* [1. 介绍](#sdk-intro)
  * [1.1. 介绍](#sdk-manual-intro)
    * [1.1.1. 交叉编译工具链](#the-cross-development-toolchain)
    * [1.1.2. Sysroots](#sysroot)
    * [1.1.3. QEMU 模拟器](#the-qemu-emulator)
  * [1.2. SDK 开发模型](#sdk-development-model)
* [2. 使用可扩展 SDK](#第二章-使用可扩展-sdk)
  * [2.1. 为什么要使用可扩展的 SDK 以及里面有些什么](#sdk-extensible-sdk-intro)
  * [2.2. 安装可扩展 SDK](#22-安装可扩展-sdk)
  * [2.3. 运行可扩展 SDK 环境配置脚本](#23-运行可扩展-sdk-环境配置脚本)
  * [2.4.  在你的 SDK 流程中运用 devtool](#using-devtool-in-your-sdk-workflow)
    * [2.4.1. 用 devtool add 增加一个应用程序](#sdk-use-devtool-to-add-an-application)
    * [2.4.2. 用 devtool modify 修改已有的组件源码](#sdk-devtool-use-devtool-modify-to-modify-the-source-of-an-existing-component)
    * [2.4.3. 用 devtool upgrade 更新来创建一个新配方，用来兼容软件的新版本](#sdk-devtool-use-devtool-upgrade-to-create-a-version-of-the-recipe-that-supports-a-newer-version-of-the-software)
  * [2.5. 进一步理解 devtool 的创建操作](##sdk-a-closer-look-at-devtool-add)
    * [2.5.1. 名称和版本号](#sdk-name-and-version)
    * [2.5.2. 依赖检查与映射](#sdk-dependency-detection-and-mapping)
    * [2.5.3. 许可证检查](#sdk-license-detection)
    * [2.5.4. 增加只用 Makefile 编译的软件](#sdk-adding-makefile-only-software)
    * [2.5.5. 添加本机工具](#sdk-adding-native-tools)
    * [2.5.6. 添加 Node.js 模块](#sdk-adding-node-js-modules)
  * [2.6. 如何使用配方（Recipes）](#sdk-working-with-recipes)
    * [2.6.1. 查找日志和运行文件](#sdk-finding-logs-and-work-files)
    * [2.6.2. 设置配置参数](#sdk-setting-configure-arguments)
    * [2.6.3. 配方（Recipe）之间共享文件](#sdk-sharing-files-between-recipes)
    * [2.6.4. 打包](#sdk-packaging)
  * [2.7. 恢复目标设备的原始状态](#sdk-restoring-the-target-device-to-its-original-state)
  * [2.8. 安装新增的条目到可扩展 SDK](#sdk-installing-additional-items-into-the-extensible-sdk)
  * [2.9. 实现更新一个已安装的可扩展 SDK](#sdk-applying-updates-to-an-installed-extensible-sdk)
  * [2.10 创建一个带附加组件的衍生 SDK](#sdk-creating-a-derivative-sdk-with-additional-components)
* [3. 使用标准的 SDK](#)
  * [3.1. 为什么要用标准的 SDK 以及它有哪些功能](#)
  * [3.2. 安装 SDK](#)
  * [3.3. 运行 SDK 的环境配置脚本](#)
* [4. 使用 SDK 的交叉工具链目录](#)
  * [4.1. Autotools 相关项目](#)
  * [4.2. Makefile 相关项目](#)
* [A. 获取 SDK](#)
  * [A.1. 查找预构建的 SDK 安装程序](#)
  * [A.2. 构建一个 SDK 安装程序](##sdk-building-an-sdk-installer)
  * [A.3. 解压根文件系统](#)
  * [A.4. 安装标准的 SDK 目录结构](#)
  * [A.5. 安装可扩展 SDK 的目录结构](#)
* [B. 定制可扩展 SDK](#)
  * [B.1. 定制可扩展 SDK](#)
  * [B.2. 调整可扩展 SDK 以适配构建主机的配置](#)
  * [B.3. 修改可扩展 SDK 安装包名称](#)
  * [B.4. 实现可扩展 SDK 安装后的更新](#)
  * [B.5. 修改 SDK 的默认安装路径](#)
  * [B.6. 实现额外的安装项到可扩展 SDK](#)
  * [B.7. 简化可扩展 SDK 安装包的下载大小](#)
* [C. 定制标准的 SDK](#)
  * [C.1. 添加独立的包到标准 SDK](#)
  * [C.2. 添加 API 文档到标准 SDK](#)
  


<a id="sdk-intro">第一章 介绍</a>
===

目录索引
  * [1.1. 介绍](#sdk-manual-intro)
    * [1.1.1. 交叉编译工具链](#the-cross-development-toolchain)
    * [1.1.2. Sysroots](#sysroot)
    * [1.1.3. QEMU 模拟器](#the-qemu-emulator)
  * [1.2. SDK 开发模型](#sdk-development-model)
  
<a id="sdk-manual-intro">1.1. 介绍</a>
====
欢迎阅读 Yocto 项目应用程序开发和可扩展软件开发的 SDK手册。本手册主要涉及如何使用 Yocto 可扩展和标准 SDK 来开发应用程序和镜像的相关信息。
> **小提示**
>
> Yocto 项目 2.0 早前版本，应用程序开发主要是通过使用 ADT 套件，常见尽可能的交叉编译工具链和其他工具。yocto 项目 2.1 版本以后应用程序开发用丰富的可扩展 SDK 和 更多传统的标准 SDK 可以完成。
所有的 SDK 的组成如下：
* **交叉工具链**：工具链包含一个编译器，调试器和许多各种各样的工具。
* **库文件及其头文件，符号表**：指定镜像（例如他们匹配的镜像）的库文件，头文件和符号表
* **环境变量配置脚本**：这个 *.sh* 脚本文件只需要运行一次，通过定义变量和做一些使用 SDK 的准备工作来配置交叉开发的环境变量。

另外，一个可扩展 SDK 的工具允许你容易的添加一个应用程序和镜像的库文件，修改现有组件的源码，测试目标硬件的变化，并很容易的集成一个应用层序到 [OpenEmbedded 构建系统][4]里。

你可以用一个 SDK 去独立的开发和测试代码使其运行在一些在目标机器上。这些 SDK 是完全独立的。二进制程序是链接到他们本地 libc 的拷贝，这样使其对目标系统没有依赖。为了实现这点，动态加载器的指针在安装的时候配置后地址就不能动态改变了。这就是为什么有封装 populate_sdk 和 populate_sdk_ext 的分别。

SDK 的另一个特性是构造一整套的交叉工具链是为了支持任意给定的指令架构。这个特性的好处是目标硬件可以通过编译选项控制 gcc 编译。这些选项通过环境变量脚本设置并包含在变量里，例如 [CC][5] 和 [LD][6]。这样会减少工具使用的内存占用。要知道，尽管这样，每个目标仍然需要一个 sysroot 应该这些二进制是针对特定目标板的。

SDK 开发环境组成如下：

* 独立的 SDK 是指定架构的交叉编译和对应的 sysroot（目标文件和本地文件），这些都是通过 OpenEmbedded 构建系统编译处理的（例如 SDK）。交叉工具链和 sysroot 都是基于 Metadata 的配置和扩展的，这个允许在宿主机器上交叉开发目标硬件。另外，可扩展 SDK 包含了 devtool 的功能。
* QEMU 模拟器可以允许模拟目标硬件。它不是 SDK 里的一部分。你必须单独编译和引用这个模拟器。QENU 在围绕 SDK 程序开发扮演重要的角色。

总得来说，可扩展和标准的 SDK 有很多共同的特性。但是可扩展 SDK 作为强大的开发工具帮助你更快速的开发应用程序。下面的表格汇总了标准 SDK 和可扩展 SDK 之间在构建上的一些区别：
|特性<img width=100/> |标准SDK<img width=200/> |可扩展SDK <img width=300/> |
|-|-|-|
|交叉链|是|是<sup>①</sup>|
|调试器|是|是<sup>①</sup>|
|大小|大约100Mb|1Gb以上（迷你版本工具链大约300Mb）|
|devtol|否|是|
|编译镜像|否|是|
|可更新|否|是|
|可管理的 sysroot<sup>②</sup>|否|是|
|包安装|否<sup>③</sup>|是<sup>④</sup>|
|结构方式|安装包|共享状态|

<sub>
① 如果SDK_EXT_TYPE 等于 “full” 或者 SDK_INCLUDE_TOOLCHAIN 等于 “1”的时候（默认值），可扩展 SDK 包含交叉链和调试器。<br>
② Sysroot 是通过 devltool 来管理的。因此如果你尝试添加额外的库，会有小概率损坏 SDK 的sysroot。<br>
③ 你可以添加运行时包管理到标准 SDK，这个默认是不支持的。<br>
④ 你必须为想要安装 “包” 的用户构建和编译共享模式变量给可扩展 SDK 的用户。<br>
</sub>

</sub>

<a id="the-cross-development-toolchain">1.1.1. 交叉开发工具链 </a>
====

[交叉开发工具链][7]包含交叉编译器，交叉链接器和交叉调试器，这些用来开发目标硬件的用户应用程序。
另外，对于可扩展 SDK，工具链也内置`devtool` 功能。这样工具链可通过运行 SDK 安装脚本来创建，或通过[构建目录][8]的 metadata 配置或目标设备的扩展来实现。交叉工具链需要对应的目标设备 sysroot 才能工作 。

 <a id="sysroot">1.1.2. Sysroots</a>
=====
宿主和目标板的 sysroot 包含所依赖的头文件和库文件，最终生成二进制文件在目标架构上运行。目标板的 sysroot 通过 OpenEmbedded 构建系统编译到目标版本的根文件系统镜像里，使用的 metadata 的配置和构建交叉工具链的一样。


<a id="#the-qemu-emulator">1.1.3. QEMU 模拟器</a>
=====
QEMU 模拟器可以模拟在应用程序或镜像运行在你的硬件上。QEMU 不是 SDK 的一部分，但是它有几种生成方法：
* 如果你克隆了 poky 的 GIT 仓来创建的[源码目录][9]并且你已经执行了环境变量的配置脚本，QEMU 就已经安装好并自动可用的。
* 如果你下载的是 Yocto 项目发行版并解压创建的源码目录，你也执行了环境变量配置脚本，QEMU 就已经安装好并自动可用的。
* 如果你已经安装了 交叉工具链的压缩包并你已经运行了工具链的环境变量的配置脚本，QEMU 也是已经安装并自动可用的。

<a id="#sdk-development-model">1.2. SDK 开发模型</a>
=====
从本质上看，SDK 在开发流程中如下所示：

 ![1.2. SDK 开发模型][10]

SDK 可安装到任意机器上并用于开发应用程序，镜像和内核。SDK 甚至可被 QA 工程师或版本工程师使用。基本概念就是说已经安装 SDK 的机器就没有必要再关联已安装 Yocto 项目的机器了。一个开发者可以独立的编译和测试机器上的对象，然后当这个对象可以准备集成到镜像里的时候，可以很容易的让它在 Yocto项目的机器上直接编译。一旦这个对象可用了，直接用 Ycoto 项目编译编译修改了的镜像。

建议按照下面这些步骤：

1. **为你的目标硬件安装 SDK：** 关于如何安装SDK的信息，请查看[安装 SDK ][11]部分

2. **下载或编译目标镜像：** Yocto 项目支持多个目标体系架构，并有多个预编译内核镜像和文件系统镜像。

如果你将要在目标硬件上开发应用程序，要到 [machines][12] 下载目录选择一个目标机型目录来下载内核镜像和根文件系统。下载目录包含用于在实际硬件的开发支持。例如，目录里包含 `.hddimg` 文件将内核镜像的文件系统，启动加载器等的合成到一起。请确保获取的文件是你开发过程中专门需要的。

如果你正在开发应用程序然后在 QEMU 模拟器上 运行和测试，请到  [machines/qemu][12] 目录下载。从这目录进入选择你要的目标体系架构（例如：qemux86_64 对应英特尔® 64位架构的）。下载内核，根文件系统和过程所需其他的文件。

> 提示
>
> 使用 QEMU 根文件系统，你需要解压它。查看[“解压根文件系统”][14]部分获取关于如何解压根文件系统的信息.

3. **开发和测试应用程序：** 这时，你需要有工具开发你的应用程序。如果你需要独立安装和使用 QEMU 模拟器，你可以去 [QEMU 主页][13]下载和学习关于模拟器。查看 Yocto 项目开发任务手册的[“快速使用模拟器（QEMU）”][14]章节获取在 Yocto 项目使用 QEMU 的相关信息。

本手册余下部分主要描述如何使用可扩展和标准的 SDK。这些信息也在附录的表格里，主要是关于如何编译，安装和修改 SDK。

第二章 使用可扩展 SDK
=====

**目录结构**
* [2.1. 为什么要使用可扩展的 SDK 以及里面有些什么](#sdk-extensible-sdk-intro)
* [2.2. 安装可扩展 SDK](#22-安装可扩展-sdk)
* [2.3. 运行可扩展 SDK 环境配置脚本](#23-运行可扩展-sdk-环境配置脚本)
* [2.4.  在你的 SDK 流程中运用 devtool](#using-devtool-in-your-sdk-workflow)
* [2.4.1. 用 devtool add 增加一个应用程序](#sdk-use-devtool-to-add-an-application)
* [2.4.2. 用 devtool modify 修改已有的组件源码](#sdk-devtool-use-devtool-modify-to-modify-the-source-of-an-existing-component)
* [2.4.3. 用 devtool upgrade 更新来创建一个新配方，用来兼容软件的新版本](#sdk-devtool-use-devtool-upgrade-to-create-a-version-of-the-recipe-that-supports-a-newer-version-of-the-software)
* [2.5. 进一步理解 devtool 的创建操作](##sdk-a-closer-look-at-devtool-add)
* [2.5.1. 名称和版本号](#sdk-name-and-version)
* [2.5.2. 依赖检查与映射](#sdk-dependency-detection-and-mapping)
* [2.5.3. 许可证检查](#sdk-license-detection)
* [2.5.4. 增加只用 Makefile 编译的软件](#sdk-adding-makefile-only-software)
* [2.5.5. 添加本机工具](#sdk-adding-native-tools)
* [2.5.6. 添加 Node.js 模块](#sdk-adding-node-js-modules)
* [2.6. 如何使用配方（Recipes）](#sdk-working-with-recipes)
* [2.6.1. 查找日志和运行文件](#sdk-finding-logs-and-work-files)
* [2.6.2. 设置配置参数](#sdk-setting-configure-arguments)
* [2.6.3. 配方（Recipe）之间共享文件](#sdk-sharing-files-between-recipes)
* [2.6.4. 打包](#sdk-packaging)

这一章介绍了可扩展 SDK 和如何安装它。具体信息包含 SDK 的组成部分，如何安装它，并概要介绍 devtool 功能展示。可扩展 SDK 使其很容易的添加一个新的应用程序和库文件到镜像，修改现有组件的源码，在目标硬件验证测试，作为附加可轻松集成到 [OpenEmbedded 的编译系统](#build-system-term)上。

>提示
>
>要比较 可扩展 SDK 和标准 SDK 的主要特性，请查看[“介绍”](#sdk-manual-intro)部分。

除过了通过 devtool 使用其功能外，你可以直接使用工具链，例如从 Makefile 到 Autotools。请查看[“直接使用 SDK 工具链”](#sdk-working-projects)章节获取更多信息。

2.1. 使用可扩展 SDK 的原因以及里面有些什么?
=====

可扩展 SDK 提供提供了交叉编译环境的工具链和为特定镜像定制的库文件。如果你想拥有使用带有强大的 devltool 命令工具链的经验，而这些命令都是为 Yocto 项目环境定制的，你需要用可扩展 SDK。

安装的可扩展 SDK 包含了几个文件和目录。基本上说，它包含 SDK 环境配置脚本，一些设置文件，一个内部编译系统以及 devtool 的功能。


2.2. 安装可扩展 SDK
=====

首先你需要做的事情是通过运行 \*.sh 安装脚本在[编译主机](#hardware-build-system-term)上安装 SDK。

你可以下载 tar 格式的安装包，它包含预编译的工具链，用于运行 QEMU 模拟器的脚本，内部编译系统，devtool 和支持文件，这些文件在发行版索引里对应的工具链目录里。工具链支持多个32位和 x86_64 目录下的64位架构体系。Yocto 项目提供的工具链是基于 core-image-sato 和 core-image-minimal 镜像，它还包含了用于开发此镜像的库文件。

安装包脚本的名字的前缀代表了主机系统，接着后面的字符串代表体系架构的类型。可扩展 SDK 使用字符串“-ext”代表。下面的例子是典型：

```
     poky-glibc-host_system-image_type-arch-toolchain-ext-release_version.sh

     源自:
         host_system 对应的字符串代表主机开放系统的体系架构：

                    i686 or x86_64.

         image_type 代表 SDK 编译出来的镜像名称：

                    core-image-sato or core-image-minimal

         arch 代表正在调测的目标架构体系：

                    aarch64, armv5e, core2-64, i586, mips32r2, mips64, ppc7400, or cortexa8hf-neon

         release_version Yocto 项目发布版本号

                    3.0, 3.0+snapshot
```

例如，下面的 SDK 安装包是在64 位主机系统开发调测 i586 目标体系架构，用最新的 3.0 来编译出 core-imge-sato 镜像：
```
poky-glibc-x86_64-core-image-sato-i586-toolchain-ext-3.0.sh
```
    
            
> 提示
>
>可二选一的下载 SDK，也可编译 SDK 安装包。获取安装包编译的信息，可查阅[“编译安装包”](#sdk-building-an-sdk-installer)部分。

SDK 和工具链都是内置的，默认都被安装在用户目录的 poky_sdk 目录。在安装的时候你可以选择把可扩展 SDK 安装在任意地方。但是需求其文件在正常操作中确保可被写入，安装目录也对使用 SDK 的用户要有写入权限。

下面的命令展示了如何运行64位x86的主机系统下构建64位x86体系架构的安装包。这个例子假定 SDK 安装包位于 `~/Download/` 并有可执行权限。

>提示
>
>如果你对 SDK 的安装目录没有写权限，安装包会提示给你然后退出。这种情况需要你添加合适的权限然后再运行安装包。


```
     $ ./Downloads/poky-glibc-x86_64-core-image-minimal-core2-64-toolchain-ext-2.5.sh
     Poky (Yocto Project Reference Distro) Extensible SDK installer version 2.5
     ==========================================================================
     Enter target directory for SDK (default: ~/poky_sdk):
     You are about to install the SDK to "/home/scottrif/poky_sdk". Proceed [Y/n]? Y
     Extracting SDK..............done
     Setting it up...
     Extracting buildtools...
     Preparing build system...
     Parsing recipes: 100% |##################################################################| Time: 0:00:52
     Initialising tasks: 100% |###############################################################| Time: 0:00:00
     Checking sstate mirror object availability: 100% |#######################################| Time: 0:00:00
     Loading cache: 100% |####################################################################| Time: 0:00:00
     Initialising tasks: 100% |###############################################################| Time: 0:00:00
     done
     SDK has been successfully set up and is ready to be used.
     Each time you wish to use the SDK in a new shell session, you need to source the environment setup script e.g.
      $ . /home/scottrif/poky_sdk/environment-setup-core2-64-poky-linux
```
            
2.3. 运行可扩展 SDK 环境配置脚本
=====

            


原文: https://www.yoctoproject.org/docs/3.0/sdk-manual/sdk-manual.html

译者：[巴龙](https://github.com/guevaraya)
校对：[校对者ID](https://github.com/校对者ID)
版权所有© 2010-2019 Linux 基金会

[1]: https://creativecommons.org/licenses/by-sa/2.0/uk/
[2]: http://www.yoctoproject.org/documentation
[3]: https://wiki.yoctoproject.org/wiki/Releases
[4]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#build-system-term
[5]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#var-CC
[6]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#var-LD 
[7]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#cross-development-toolchain
[8]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#build-directory
[9]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#source-directory
[10]: https://www.yoctoproject.org/docs/3.0/sdk-manual/figures/sdk-environment.png
[11]: https://www.yoctoproject.org/docs/3.0/sdk-manual/sdk-manual.html#sdk-installing-the-sdk
[12]: http://downloads.yoctoproject.org/releases/yocto/yocto-3.0/machines
[13]: http://downloads.yoctoproject.org/releases/yocto/yocto-3.0/machines/qemu
[14]: https://www.yoctoproject.org/docs/3.0/sdk-manual/sdk-manual.html#sdk-extracting-the-root-filesystem
[15]: http://wiki.qemu.org/Main_Page
[16]: http://www.yoctoproject.org/docs/3.0/dev-manual/dev-manual.html#dev-manual-qemu
