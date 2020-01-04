[#]: collector: (guevaraya)
[#]: translator: (guevaraya)
[#]: reviewer: ( )
[#]: publisher: ( )
[#]: url: ( )
[#]: subject: (software-view-feature)
[#]: via: (https://www.yoctoproject.org/software-overview/features/)
[#]: author: (https://www.yoctoproject.org/)

[软件概述][1] : 特性列表
======

|项目特性|描述|
|:--:|:-|
| CII下的优秀实践(Best practives) |Linux 基金会旗下的核心基础计划（CII）的最佳实践标识是开源软件遵循最佳实践的一种标志。 项目可以自愿，免费的用这个网站程序解释他们是如何遵循每个最佳实践。CII的最佳实践标识的灵感来自于 Github 上的项目标识。使用者可以快速的评估开源软件（FLOSS）是否遵循最佳实践，从而最大可能的创作出高质量的安全软件。Yocto 项目已经注册和拥有下面标识：![cii best practices passing][3] |
|二进制再现性|如果一个发布商没有指定依赖哪些软件包或他们的编译顺序，编译系统就会根据添加时机随机的包含软件包。这个极大的破坏了编译再现性。Yocto 项目管理依赖以此避免混乱达到 99.8% 的core-image minimal编译再现性，在扩展测试中这个稍微小一点。在大多数的情况下，时间戳都被处理了，其他情况案例正在生效中。|
|跨平台开发框架 CROPS| CROPS 是一个开发源代码，跨平台的开发框架，它是利用 Docker 容器来提供简易的管理，可扩展的环境，允许开发者为不同体系的系统编译镜像，如主机操作系统 Windows, Linux和 Mac|
|可扩展 SDK| Yocto 项目的可扩展 SDK 工具可以很容易添加应用程序和库文件到镜像，也可修改现有组件的源码并在目标硬件上测试验证。相比标志 SDK 的主要的优点可改进开发团队的流程，因为它和 OpenEmbedded 编译系统是紧密集成的。|
|Toaster| 它是 Yocto 项目下 OpenEmbedded 编译系统的一个网页接口。这些接口允许你配置和运行你的构建。相关的构建信息汇总在数据库里，你也可以用 Toaster 在多个远程服务器上配置和运行构建。Toaster 的诊断能力很强大。|
|多配置 Multi-Config|编译系统可以用一个命令自动和高效的知道多个架构同时编译|
|二进制编译Binary Builds|Yocto项目允许不包含源码的情况下编译出二进制文件|
|开源许可证清单生成器|Yocto 项目跟踪所有涉及编译的开源代码的许可证，然后输出这些开源许可证的清单和参考信息|

原文：https://www.yoctoproject.org/software-overview/features/

[1]: https://github.com/guevaraya/Yocto_doc/blob/master/software-overview.md
[2]: https://www.yoctoproject.org/software-overview/features/
[3]: https://bestpractices.coreinfrastructure.org/projects/765/badge

