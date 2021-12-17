# Append Files

# Bitbake
被嵌入式系统构建系统来调用的任务执行器和调度器。获取更多Bitbake信息，请查看 [Bitbake用户手册](2)

# 嵌入式编译系统(OpenEmbedded Build System)
Yocto 项目特定的编译系统。这个嵌入式编译系统基于一个叫 Poky 的项目，它用 [Bitbake](#Bitbake) 作为任务执行器。整个 Yocto 项目文档集，嵌入式编译系统有时简称为“构建系统”。如果是其他构建系统，例如涉及属主系统或目标系统，文档会明确说明。

>
>提示
>
> 获取一些关于 Poky 的历史信息，请查看 [Poky](#Poky) 词条

# Poky
Poky 它的发音是 Pock-ee， 是一个嵌入式参考发行版和一个参考测试配置。Poky 提供了以下部分：
* 一个基本功能发行版用来说明如何定制发行版：
* 意思是它可以用来测试 Yocto 项目组件（例如 poky 可用来验证 yocto 项目）
* 一套工具可以下载 yocto 项目

Poky 不是一个产品级别的发行版。但是它一个很好的定制化入门起点

>
>提示
>Poky曾经最初被OpenedHand作为一个开源项目创建。OpenedHand从已有的嵌入式构建系统为为嵌入式Linux开发出Poky来创建一个商业支持版的构建系统。当英特尔公司收购了OpenedHand后，poky 项目就变成了Yocto项目的基础构建系统。
# Recipe
[1]: https://docs.yoctoproject.org/dev-manual/common-tasks.html#appending-other-layers-metadata-with-your-layer
[2]: https://docs.yoctoproject.org/bitbake/index.html
