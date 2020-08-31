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
    * [1.1.3. QEMU 模拟器]（#1.1.3. QEMU 模拟器）
  * [1.2. SDK 开发模型](#sdk-development-model)
* [2. 使用可扩展性 SDK](#sdk-extensible)
  * [2.1. 为什么要使用可扩展的 SDK 以及里面有些什么](#sdk-extensible-sdk-intro)
  * [2.2. 安装可扩展 SDK](#sdk-installing-the-extensible-sdk)
  * [2.3. 运行可扩展 SDK 的环境配置脚本](#sdk-running-the-extensible-sdk-environment-setup-script)
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
  * [A.2. 构建一个 SDK 安装程序](#)
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
|包安装|否<sup>③</sup>|
