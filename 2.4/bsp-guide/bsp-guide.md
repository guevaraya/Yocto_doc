hYocto 项目 BSP 开发指导
======

<p align="right">Scott Rifenbark<br>
Scott的文档服务公司<br>
<a href="mailto:srifenbark@gmail.com"><font size=100>&lt;srifenbark@gmail.com&gt;</font></a>
</p>

版权所有@ 2010-2017  Linux 基金会

此文允许被复制，分发以及或者修改此文档，但需要遵循知识共享组织发布的共享协议2.0 UK: England & Wales条款

**手册说明**

* 这个 Yocto 项目 BSP 的开发指导版本是针对 Yocto 项目正式版本 2.4 的。请确认你拿到的是正式版本对应的最新手册，并使用 [Yocto 项目文档页面][2]的手册。
* 若需此文档相关的 Yocto 项目其他的版本，去 [Yocto 项目文档页面][2]选择下拉框的 “Active Releases” 按钮，选择你想要的相关 Yocto 项目文档。
* 报告这个文档的任何错误或问题，请发邮件到 Yocto 项目讨论组 yocto@yoctoproject.com 或 登陆 freenode 的 #yocto 频道。

|**修订历史**||
|-|-|
|版本 0.9	24 2010年11月|2010年11月24日|
|发布 Yocto 项目初始稿 0.9 版本||
|版本 1.0	|2011年4月6日|
|发布 Yocto 项目 1.0 版本|
|版本 1.0.1|2011年6月23日|
|发布 Yocto 项目 1.0.1 版本|
|版本 1.1|2011年10月6日|
|发布 Yocto 项目 1.1 版本|
|版本 1.2|2012年4月|
|发布 Yocto 项目 1.2 版本|
|版本 1.3	|2012年10月|
|发布 Yocto 项目 1.3 版本|
|版本 1.4|2013年4月|
|发布 Yocto 项目 1.4 版本|
|版本 1.5|2013年10月|
|发布 Yocto 项目 1.5 版本|
|版本 1.5.1 |2014年1月|
|发布 Yocto 项目 1.5.1 版本|
|版本 1.6|2014年4月|
|发布 Yocto 项目 1.6 版本|
|版本 1.7	|2014年10月|
|发布 Yocto 项目 1.7 版本|
|版本 1.8|2015年4月|
|发布 Yocto 项目 1.8 版本|
|版本2.0|2015年10月|
|发布 Yocto 项目 2.0 版本|
|版本 2.1|2016年4月|
|发布 Yocto 项目 2.1 版本|
|版本 2.2|2016年10月|
|发布 Yocto 项目 2.2 版本|
|版本2.3|2017年6月|
|发布 Yocto 项目 2.3 版本|
|版本 2.4|2017年10月|
|发布 Yocto 项目 2.4 版本|

**目录**
<!-- GFM-TOC -->
* [1. 板级开发包（BSP） - 开发指导](#bsp)
  * [1.1. BSP 层](#bsp-layers)
  * [1.2. 准备您用于开发 BSP 层的编译主机](#preparing-your-build-host-to-work-with-bsp-layers)
  * [1.3. 文件系统布局示例](#bsp-filelayout)
    * [1.3.1. 授权许可文件](#bsp-filelayout-license)
    * [1.3.2. 自述说明文件](#bsp-filelayout-readme)
    * [1.3.3. 源码自述说明文件](#bsp-filelayout-readme-sources)
    * [1.3.4. 用户二进制预编译](#bsp-filelayout-binary)
    * [1.3.5. 层配置文件](#bsp-filelayout-layer)
    * [1.3.6. 硬件配置选项](#bsp-filelayout-machine)
    * [1.3.7. 其他特定的BSP配方文件](#bsp-filelayout-misc-recipes)
    * [1.3.8. 支持图形显示的相关文件](#bsp-filelayout-recipes-graphics)
    * [1.3.9. Linux 内核配置文件](#bsp-filelayout-kernel)
  * [1.4. 开发一个板级支持包（BSP）](#developing-a-board-support-package-bsp)
  * [1.5. 发布 BSP 包的要求和建议](#requirements-and-recommendations-for-released-bsps)
    * [1.5.1. 发布 BSP 包的要求](#released-bsp-requirements)
    * [1.5.2. 发布 BSP 包的建议](#released-bsp-recommendations)
  * [1.6. 为 BSP 定制一个配方](#customizing-a-recipe-for-a-bsp)
  * [1.7. BSP 许可的注意事项](#bsp-licensing-considerations)
  * [1.8. 使用 Yocto 项目的 BSP 工具](#using-the-yocto-projects-bsp-tools)
    * [1.8.1. 常见特性](#common-features)
    * [1.8.2. 用 yocto-bsp 脚本创建一个新的 BSP 层](#creating-a-new-bsp-layer-using-the-yocto-bsp-script)
    * [1.8.3. 管理 yocto-kernel 的内核补丁和配置项](#managing-kernel-patches-and-config-items-with-yocto-kernel)
<!-- GFM-TOC -->


<a id="bsp">Chapter 1. Board Support Packages (BSP) - Developer's Guide</a>
======

**目录**

<!-- GFM-TOC -->
* [1.1. BSP 层](#bsp-layers)
* [1.2. 准备您用于开发 BSP 层的编译主机](#preparing-your-build-host-to-work-with-bsp-layers)
* [1.3. 文件系统布局示例](#bsp-filelayout)
  * [1.3.1. 授权许可文件](#bsp-filelayout-license)
  * [1.3.2. 自述说明文件](#bsp-filelayout-readme)
  * [1.3.3. 源码自述说明文件](#bsp-filelayout-readme-sources)
  * [1.3.4. 用户二进制预编译](#bsp-filelayout-binary)
  * [1.3.5. 层配置文件](#bsp-filelayout-layer)
  * [1.3.6. 硬件配置选项](#bsp-filelayout-machine)
  * [1.3.7. 其他特定的BSP配方文件](#bsp-filelayout-misc-recipes)
  * [1.3.8. 支持图形显示的相关文件](#bsp-filelayout-recipes-graphics)
  * [1.3.9. Linux 内核配置文件](#bsp-filelayout-kernel)
  * [1.4. 开发一个板级支持包（BSP）](#developing-a-board-support-package-bsp)
* [1.5. 发布 BSP 包的要求和建议](#requirements-and-recommendations-for-released-bsps)
  * [1.5.1. 发布 BSP 包的要求](#released-bsp-requirements)
  * [1.5.2. 发布 BSP 包的建议](#released-bsp-recommendations)
* [1.6. 为 BSP 定制一个配方](#customizing-a-recipe-for-a-bsp)
* [1.7. BSP 许可的注意事项](#bsp-licensing-considerations)
* [1.8. 使用 Yocto 项目的 BSP 工具](#using-the-yocto-projects-bsp-tools)
  * [1.8.1. 常见特性](#common-features)
  * [1.8.2. 用 yocto-bsp 脚本创建一个新的 BSP 层](#creating-a-new-bsp-layer-using-the-yocto-bsp-script)
  * [1.8.3. 管理 yocto-kernel 的内核补丁和配置项](#managing-kernel-patches-and-config-items-with-yocto-kernel)

一个板级开发包（BSP）是一个信息集合，它定义了如何支持特定硬件设备，设备集或一个硬件平台。BSP 包含设备里的硬件功能信息和内核额外需要的硬件驱动的配置信息。BSP 也罗列了除过基本且可选的平台特性的 Linux 软件栈外，还有额外需要的软件组件列表。

这个指导包含了 BSP 层信息，定义了一个组件的结构以便 BSP 包遵循一个通用的可理解的布局，讨论了如何为一个 BSP 定制一个配方，解释了 BSP 许可问题，提供了说明如何用 Yocto 项目的 [BSP 工具](#using-the-yocto-projects-bsp-tools)创建和管理一个 [BSP 层](#bsp-layers)的相关信息。

<a id="bsp-layers">1.1. BSP Layers</a>
=======

via:https://www.yoctoproject.org/docs/2.4/bsp-guide/bsp-guide.html


作者：[Scott Rifenbark](mailto:srifenbark@gmail.com)
译者：[guevara](https://github.com/guevaraya)
校对：[校对者ID](https://github.com/校对者ID)
版权所有© Linux 基金会





[1]: mailto:srifenbark@gmail.com
[2]: http://www.yoctoproject.org/documentation
https://www.yoctoproject.org/docs/2.4/bsp-guide/bsp-guide.html#bsp-layers
