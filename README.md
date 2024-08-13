[#]: translator: (guevaraya)
[#]: reviewer: ( )
[#]: publisher: ( )
[#]: url: ( https://www.yoctoproject.org/docs/)
[#]: subject: (yocto 文档索引)
Yocto 文档中文翻译项目
======
此文档是Yocto官方文档的个人翻译项目，允许被复制，分发以及或者修改此文档，但需要遵循知识共享组织发布的共享协议[2.0 UK: England & Wales][1]条款


正式发布的文档：

- [YP-CORE unable(dev)][2]
- [YP-CORE WARRIOR 4.3][22]
- [YP-CORE WARRIOR 4.0.20][23]
- [YP-CORE SUMO 4.0.16][33]
- [YP-CORE WARRIOR 3.2.4][24]
- [YP-CORE THUD 3.1.33][25]
- [YP-CORE THUD 3.0.4][26]
- [YP-CORE SUMO 5.0.2][27]

可以在以上列表选择发布的文档版本。您也可在维基百科的[提示与技巧][3]页面查看最新的文档，部分文档还处于移植和开发阶段的手册。

[![已翻译][31]][30]
[![翻译中][29]][30]

Yocto 项目® 入门 
======
<font color=#00ffff size=3> 如果您还没有准备好，建议首先阅读[软件概述][4]。然后再回到此页面 </font>

|[![Yocto Project Quick Build][5]][9] |[![what-i-wish-id-known][6]][10] |[![transitioning-to-a-custom-environment][7]][11] |[![what-i-wish-id-known][8]][13] |
|:---:|:---:|:---:|:---:|
|[**Yocto快速构建**][9] <br>主要是指导在模拟器和实际机器上如何快速编译和运行 |[**应知 应会**][10] <br> 点亮您的项目明珠，作为开发者在入手前应该知道哪些东西|[**定制开发环境**][11] <br>如何配置系统，然后下一步深入学习入门文档和开发手册|[**开发工具包（ESDK）**][12] <br> 对应用工程师来说，需要足够学习 Yocto 项目以便于与系统工程师有效的沟通协作|

开发者手册
======



|[**YOCTO 项目概述和概念手册**][14] |[**YOCTO 项目开发任务手册**][15]  |[**YOCTO 项目板级支持包（BSP）开发指南**][16] |
|:-|:-|:-|
|这个手册介绍 Yocto 的概念，软件概述，常见方法（BKMs）和其他一些适合新手的更多入门信息|提供镜像和应用程序开发过程的概述|定义一个 BSP 组件的框架，以便于认知的布局&nbsp; &nbsp;|
|[**YOCTO 项目分析和跟踪手册**][17]|[**YOCTO 项目 Linux 内核开发手册**][18] ||
|展示了一套通用且有用的项目分析和跟踪方案包括他们的应用程序和对应的工具。|提供了 Yocto 项目的 Linux 内核的 Metadata 的背景信息，并介绍了在 Yocto 项目环境中直接用 Linux 内核工具的可以直接运行的常见任务。|&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;|

工具手册
======

| [**Toaster 用户手册**][19] <br> |[**Bitbake 用户手册**][20] <br>|
|:---|:---|
|Toaster 是一个基于 Bitbake 的网络接口的 Yocto 编译系统，这个手册仅说明网络接口| 提供了 Batbake 工具的用户参考手册，嵌入式开源编译系统用它来任务执行和调度工具，从而编译出镜像。|


参考手册
======

| [**Yocto 项目参考手册**][21] <br> |[**Yocto 项目全文档集（Meta 手册）**][28] <br> |
|:---|:---|
|提供了Yocto 项目工具和组件的完整参考，如嵌入式开源和 Poky 的参考发行版 |提供了完整的，可检索的仅 HTML 格式的 Yocto 项目手册，这个手册（即 Meta手册）仅包含无文本的手册或指南|


原文:https://docs.yoctoproject.org/

[1]: https://creativecommons.org/licenses/by-sa/2.0/uk/deed.zh
[2]: https://docs.yoctoproject.org/dev/
[3]: https://wiki.yoctoproject.org/wiki/TipsAndTricks
[4]: software-overview/software-overview.md
[5]: sources/quick_build.png
[6]: sources/what_i_wish_know.png
[7]: sources/customized.png
[8]: sources/extend_sdk.png
[9]: 5.0.2/brief-yoctoprojectqs/README.md 
[#]: 3.0/brief-yoctoprojectqs/brief-yoctoprojectqs.md
[10]: what-i-wish-id-known/what-i-wish-id-known.md
[11]: latest/transitioning-to-a-custom-environment.md
[12]: 5.0.2/sdk-manual/index.md
[13]: 5.0.2/ref-manual/ref-manual.md
[14]: 3.5/overview-manual/README.md
[#]: 3.0/overview-manual/overview-manual.md
[15]: 3.5/dev-manual/README.md
[#]: 3.0/dev-manual/dev-manual.md
[16]: 3.5/bsp-guide/README.md
[17]: 3.5/profile-manual/README.md
[18]: 3.5/kernel-dev/README.md
[19]: 3.5/toaster-manual/README.md
[20]: 3.5/bitbake-user-manual/README.md
[21]: 3.5/ref-manual/README.md
[28]: 3.0/mega-manual/README.md

[22]: https://docs.yoctoproject.org/4.3/
[23]: https://docs.yoctoproject.org/4.0.20
[24]: https://docs.yoctoproject.org/3.2.4/
[25]: https://docs.yoctoproject.org/3.1.13/
[26]: https://docs.yoctoproject.org/3.0.4/
[27]: https://docs.yoctoproject.org/5.0.2/
[28]: https://docs.yoctoproject.org/4.3.3/
[29]: sources/translating.svg
[30]: overview-manual/overview-manual.md
[31]: sources/translated.svg
[32]: sources/untranslated.svg
[33]: https://docs.yoctoproject.org/4.0.16/
