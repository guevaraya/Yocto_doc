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
  
via:https://www.yoctoproject.org/docs/3.0/sdk-manual/sdk-manual.html

[1]: https://creativecommons.org/licenses/by-sa/2.0/uk/
[2]: http://www.yoctoproject.org/documentation
[3]: https://wiki.yoctoproject.org/wiki/Releases


