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
  * [2.4.  在你的 SDK 流程中运用 devtool](#24-在你的-sdk-流程中运用-devtool)
    * [2.4.1. 用 devtool add 增加一个应用程序](#241-使用-devtool-add-添加一个应用程序)
    * [2.4.2. 用 devtool modify 修改已有的组件源码](#242-使用-devtool-modify-修改源码的已有组件)
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
* [2.4.  在你的 SDK 流程中运用 devtool](#24-在你的-sdk-流程中运用-devtool)
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

一旦你已经安装了 SDK，在实际使用 SDK 之前就必须先运行 SDK 环境配置脚本。这个配置脚本所在目录使在安装 SDK 使选择的，如果不是 poky_sdk 的默认目录就是你选择的安装目录。

在运行脚本之前，需要确保对应的体系架构是我们即将要开发的。环境配置脚本以字符串“environment-setup”开头，并包含要调试的体系架构名称。例如，下面命令跳转到 SDK 安装目录然后 source 执行环境配置脚本。这个例子的配置脚本用 i586架构体系调试开发 IA架构体系。

```
     $ cd /home/scottrif/poky_sdk
     $ source environment-setup-core2-64-poky-linux
     SDK environment now set up; additionally you may now run devtool to perform development tasks.
     Run devtool --help for further details.
```

运行配置脚本定义了很多环境变量，他们用来更好的使用 SDK（如：PATH，[CC][17]，[LD][18] 等等）。如果你想查看脚本所输出的所有的环境变量，研读下安装文件本身就可以了。

2.4. 在你的 SDK 流程中运用 devtool
=====

可扩展 SDK 的基石是一个名叫 devtool 的命令行工具。这个工具提供了很多有用的特性：在可扩展 SDK 里编译，测试和软件打包，也可以通过 OpenEmbedded 编译系统集成到镜像里。

> 小贴士
>
>对使 devtool 的使用不局限于可扩展 SDK ，可以用 devtool 帮你容易的开发任何项目，这些项目都能编译出镜像的一部分。

devtool 命令行的组织方式与 [Git][19]的子命令的功能方式很像。你可以运行 devtool --help 查看所有的命令。
> 查看 Yocto 项目参考手册的["devtool 快速参考"][20]获取 devtool 的快速参考。

下面三个 devtool 的子命令提供了开发的入口点：
* _devtool add:_ 辅助添加需要编译的软件
* _devtool modify:_ 配置一个环境让你可以修改已有的组件源
* _devtool upgrade:_ 更新已有的配方以便编译更新一套源文件。

在编译系统里，“配方（recipes）”代表 devtool里的软件包。当执行 devtool add 后，一个配方被自动创建。当你执行 devtool modify 命令后，指定的现有配方用来决定从哪里获取源码以及如何打补丁。这两个情况下，配置好环境是为了让你编译想要修改的配方源码。新配方及其源码默认都在 SDK 的 “workspace” 目录下。

这个段落的后面主要描述 devtool add， devtool modify 和 devtool upgrade 的执行流程。

2.4.1. 使用 devtool add 添加一个应用程序
======
devtool add 命令可在已有的源代码基础上生成一个配方。这个命令使用了很多 devtool 的[工作目录层][21]。这个命令很灵活的帮你将源码解压到工作目录或一个特定的本地 Git 仓。如果用的是已经存在的代码就不需要解压。

根据特定的需求场景，devtool add 的参数和选项形成不同的组合。下面图表展示了使用 devtool add 命令可能用到的常见开发流程：

![sdk-devtool-add-flow][22]

1. **生成新配方**:  流程图的顶部展示了用 devtool add 基于现有的源码生成配方的三个场景。

在共享的开发环境下，通常其他开发者的职责各种代码。作为一个开发者，你可能关心如何在 Yocto 项目的开发中使用代码。你需要获取源码，配方以及完成工作的可操作区域。
这个图表中，三个可能的场景囊括在 devtool add 的流程里：
* **左边**：图表最左边的场景代表常见的源码不在本地需要解压的情况。这种情况下源码解压在默认的工作目录-你不需要工作目录之外的特定目录文件。因此所有你需要的东西都在工作目录：
```
$ devtool add recipe fetchuri
```
用这个命令，devtool 解压上游源码文件到本地的 Git 仓源码目录。然后这个命令会在工作目录创建一个名叫 recipe 的配方和一个对应的 append 配方文件。如果没有提供配方名称，命令会尝试自己生成一个。
* **中间**：图表里的中间场景也代表源码不在本地的情况。这样的话，代码同样在上游并需要解压到本地，其中一些目录不在默认的工作目录。

Middle: The middle scenario in the figure also represents a situation where the source code does not exist locally. In this case, the code is again upstream and needs to be extracted to some local area - this time outside of the default workspace.
>提示
>
> 如果需要，devtool 在解压的过程中总是会在本地创建一个 Git 仓。
进一步说，这个例子里的第一个位置参数 srctree 定义了devtool add 命令在工作量目录之外的解压目录。它需要指定一个空目录：
```
$ devtool add recipe srctree fetchuri 
```
总之，从 fetchuri 获取源码然后作为 Git 代码仓解压到 srctree 本地目录。
在工作目录里，devtool 创建一个名称为recipe和带有append 的配方文件。

* **右边：** 图表右边的场景代表 srctree 在 devtool 工作目录之外已提前配置的情况。

下面命令创建一个名为 recipe的新配方，并能识别现有的源码：
```
$ devtool add recipe srctree
```
这个命令检查源码并为源码创建一个叫 recipe 的配方然后将其放到工作目录。
由于解压的源码已经存在，devtool 不需要尝试搬运源码到工作目录-仅在工作目录需要一个新配方。
配方目录里，该命令同时创建了一个包含 *.bbappend 文件的 append 相关目录。

2. **配方编辑：** 你可以用 devtool edit-recipe 命令用 $EDITOR 变量定义的编辑器修改配方文件：
```
$ devtool edit-recipe recipe
```
在编辑器里对配方的编辑修改将会在后面的构建中生效。
3. **编译配方或重建镜像：** 下一步的动作取决于如何处理新的代码。
如果你需要最终会将编译物移动到目标硬件的目录，那就用下面的 devtool 命令：
```
$ devtool build recipe
```
另一方面，如果你想将包含配方包的镜像从工作目录发布到设备上（例如：用于测试目录），你可以用 devtool build-image 命令：
```
$ devtool build-image image
```
4. ** 部署编译生成物： ** 当你用 devtool build 命令编译你的配方时，你可能想看到编译生成物在目标硬件上的运行效果。

> 提示
>
> 这个步骤是假定你已经有了一个可以在 QEMU 或 实际硬件上运行的构建镜像。也假定镜像部署到了目标板上，并安装了 SSH，如果镜像运行在实际的硬件上，你也有从开发主机访问的网络权限。

你可以用 devtool deploy-target 命令部署编译输出物到目标硬件：
```
$ devtool deploy-target recipe target
```
target 是一个作为一个 SSH 服务器运行的目标机器。
你也可以通过 devtool build-image 命令部署编译的镜像到实际硬件。但是 devtool 不能提供特定的命令让你部署镜像到实际硬件。

5. **使用配方收尾你的工作：** devtool finish 一般会创建对应的补丁到本地 Git 仓，生成一个新配方将其移动到一个更稳定的层里，然后重置配方以便在工作目录之外可正常编译。
```
 $ devtool finish recipe layer
```

>小提示
>
>任何你要生成的补丁必须要提交到源码目录的 Git 仓。

如上所，devtool finish 移动最终的配方到它的稳定层。
devtool finish 命令作为最终的工序，恢复标准层和上游源码的状态以便在这些区域构建配方而不是在工作目录。

>小提示
>
>当你不想继续工作，你可以用 devtool reset 命令恢复的工作区域。如果你用了这个命令，请记住源码树还是保留的。

2.4.2. 使用 devtool modify 修改源码的已有组件
======

devtool modify 命令用来如何在已有代码上做准备工作，然后用已有的配方来构建软件。此命令很方便的让你从上游解压源码，保持对其跟踪并收集从其他开发者来的补丁文件。
根据你的特定需求，你可以选择 devtool modify 命令的参数和选项形成不同的组合。下面图表展示了 devtool modify 命令的不同的组合：

![使用 devtool modify 修改源码的已有组件][23]

1. **准备修改源码**： 流程图最上面展示了三个应用场景：我们可以用 devtool modify  命令在源码文件做一些准备工作。每个场景有以下前提假设：
* 配方在 devtool 的本地工作目录的外部层（layer）已经存在。
* 源码文件在上游压缩形式存在或在以解压展开状态在本地。

进一步说，他们的源码文件既可以是远程上游的也可是本地的。
典型的情况是其他开发者为使用 Yocoto Project 和它们的自己本地的配方而创建一个层。

* **左边**：图标最左边的场景代表源码不在本地的常见的情况。它需要从上游源码解压。这种情况，源码被解压到默认的 devtool 的本地工作目录里。这个场景的配方（recipe）就是他的工作目录外面的层（layer）文件。（例如：meta-layername）

下面命令定义配方默认并解压源码文件：

```
$ devtool modify recipe
```

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
[17]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#var-CC
[18]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#var-LD
[19]: https://www.yoctoproject.org/docs/3.0/overview-manual/overview-manual.html#git
[20]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#ref-devtool-reference
[21]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#devtool-the-workspace-layer-structure
[22]: https://www.yoctoproject.org/docs/3.0/sdk-manual/figures/sdk-devtool-add-flow.png
[23]: https://www.yoctoproject.org/docs/3.0/sdk-manual/figures/sdk-devtool-modify-flow.png
