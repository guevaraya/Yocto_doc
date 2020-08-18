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
    * [1.1.2. Sysroots](#)
    * [1.1.3. QEMU 模拟器](#)
  * [1.2. SDK Development Model](#)
* [2. Using the Extensible SDK](#)
  * [2.1. Why use the Extensible SDK and What is in It?](#)
  * [2.2. Installing the Extensible SDK](#)
  * [2.3. Running the Extensible SDK Environment Setup Script](#)
  * [2.4. Using devtool in Your SDK Workflow](#)
    * [2.4.1. Use devtool add to Add an Application](#)
    * [2.4.2. Use devtool modify to Modify the Source of an Existing Component](#)
    * [2.4.3. Use devtool upgrade to Create a Version of the Recipe that Supports a Newer Version of the Software](#)
  * [2.5. A Closer Look at devtool add](#)
    * [2.5.1. Name and Version](#)
    * [2.5.2. Dependency Detection and Mapping](#)
    * [2.5.3. License Detection](#)
    * [2.5.4. Adding Makefile-Only Software](#)
    * [2.5.5. Adding Native Tools](#)
    * [2.5.6. Adding Node.js Modules](#)
  * [2.6. Working With Recipes](#)
    * [2.6.1. Finding Logs and Work Files](#)
    * [2.6.2. Setting Configure Arguments](#)
    * [2.6.3. Sharing Files Between Recipes](#)
    * [2.6.4. Packaging](#)
  * [2.7. Restoring the Target Device to its Original State](#)
  * [2.8. Installing Additional Items Into the Extensible SDK](#)
  * [2.9. Applying Updates to an Installed Extensible SDK](#)
  * [2.10. Creating a Derivative SDK With Additional Components](#)
* [3. Using the Standard SDK](#)
  * [3.1. Why use the Standard SDK and What is in It?](#)
  * [3.2. Installing the SDK](#)
  * [3.3. Running the SDK Environment Setup Script](#)
* [4. Using the SDK Toolchain Directly](#)
  * [4.1. Autotools-Based Projects](#)
  * [4.2. Makefile-Based Projects](#)
* [A. Obtaining the SDK](#)
  * [A.1. Locating Pre-Built SDK Installers](#)
  * [A.2. Building an SDK Installer](#)
  * [A.3. Extracting the Root Filesystem](#)
  * [A.4. Installed Standard SDK Directory Structure](#)
  * [A.5. Installed Extensible SDK Directory Structure](#)
* [B. Customizing the Extensible SDK](#)
  * [B.1. Configuring the Extensible SDK](#)
  * [B.2. Adjusting the Extensible SDK to Suit Your Build Host's Setup](#)
  * [B.3. Changing the Extensible SDK Installer Title](#)
  * [B.4. Providing Updates to the Extensible SDK After Installation](#)
  * [B.5. Changing the Default SDK Installation Directory](#)
  * [B.6. Providing Additional Installable Extensible SDK Content](#)
  * [B.7. Minimizing the Size of the Extensible SDK Installer Download](#)
* [C. Customizing the Standard SDK](#)
  * [C.1. Adding Individual Packages to the Standard SDK](#)
  * [C.2. Adding API Documentation to the Standard SDK](#)
  
via:https://www.yoctoproject.org/docs/3.0/sdk-manual/sdk-manual.html

[1]: https://creativecommons.org/licenses/by-sa/2.0/uk/
[2]: http://www.yoctoproject.org/documentation
[3]: https://wiki.yoctoproject.org/wiki/Releases


