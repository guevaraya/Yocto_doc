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
| CII下的优秀实践(Best practives) |Linux 基金会旗下的核心基础计划（CII）的最佳实践标识是开源软件遵循最佳实践的一种标志。 项目可以自愿，免费的用这个网站程序解释他们是如何遵循每个最佳实践。CII的最佳实践标识的灵感来自于 Github 上的项目标识。使用者可以快速的评估开源软件（FLOSS）是否遵循最佳实践，从而最大可能的创作出高质量的安全软件。Yocto 项目已经注册和拥有下面标识：![cii best practices passing][2]|
|二进制再现性|如果一个发布商没有指定依赖哪些软件包或他们的编译顺序，编译系统就会根据添加时机随机的包含软件包。这个极大的破坏了编译再现性。Yocto 项目管理依赖以此避免混乱达到 99.8% 的core-image minimal编译再现性，在扩展测试中这个稍微小一点。在大多数的情况下，时间戳都被处理了，其他情况案例正在生效中。|

[1]: https://github.com/guevaraya/Yocto_doc/blob/master/software-overview.md
[2]: https://www.yoctoproject.org/software-overview/features/
[3]: https://bestpractices.coreinfrastructure.org/projects/765/badge

