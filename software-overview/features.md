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
|二进制可再现性|If a distribution isn’t specific about which packages to pull in to support dependencies, or their order, build systems can arbitrarily include packages based on when dependencies are filled. This harms build reproducibility. The Yocto Project® controls dependencies avoiding contamination and has achieved reproducibility of 99.8% in “core-image minimal” and slightly less in expanded tests. Timestamps have been addressed in a number of cases and other cases are an ongoing effort.|

[1]: https://github.com/guevaraya/Yocto_doc/blob/master/software-overview.md
[2]: https://www.yoctoproject.org/software-overview/features/
[3]: https://bestpractices.coreinfrastructure.org/projects/765/badge

