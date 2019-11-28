[#]: translator: (guevaraya)
[#]: reviewer: ( )
[#]: publisher: ( )
[#]: url: ( https://www.yoctoproject.org/docs/3.0/brief-yoctoprojectqs/brief-yoctoprojectqs.html)
[#]: subject: (Yocto Project Quick Build)

快速构建 Yocto 工程
======
所有权@ 2010-2019 Linux基金会

此文允许被复制，分发以及或者修改此文档，但需要遵循知识共享组织发布的共享协议[2.0 UK: England & Wales][1]条款

热烈欢迎！
======
热烈欢迎！这篇简短的文档指导你通过 Yocto 工程完成一个典型镜像的构建。文档也介绍了如何为特定硬件的配置构建。你也可以用 Yocto 构建一个 叫Poky的嵌入式参考系统。

小提示：
>
1.本文的例子假定你本地使用的 Linux 系统是最新的 Ubuntu 发行版。如果用 Yocto 工程构建一个镜像（[主机端][2]）的机器不是原生的 Linux 系统，你可以仍旧通过 Poky容器在跨平台（CROPS）基础上完成这三步。可在 Yocto 工程的开发任务手册的[“设置使用交叉平台（CROPS）”][3]段落查看更多信息。
2.不能使用[Windows子系统Linux（WSL）][4]构建主机。Yocto 工程不兼容 WSL。 

如果你想了解更多关于 Yocto 工程的概念或背景信息，请参照 Yocto 工程的概述和基础手册。

via: https://www.yoctoproject.org/docs/3.0/brief-yoctoprojectqs/brief-yoctoprojectqs.html

[1]: https://creativecommons.org/licenses/by-sa/2.0/uk/
[2]: http://www.yoctoproject.org/docs/3.0/ref-manual/ref-manual.html#hardware-build-system-term
[3]：http://www.yoctoproject.org/docs/3.0/dev-manual/dev-manual.html#setting-up-to-use-crops
[4]: https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux
