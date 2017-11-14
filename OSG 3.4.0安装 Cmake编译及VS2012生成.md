OSG 3.4.0安装 Cmake编译及VS2012生成
==========================

> 当前系统为 win10，OSG版本为3.4.0，使用VS的版本为2012（似乎那个版本都差不多）

第一步准备工作
---

下载OSG源代码：[OSG源码](https://cmake.org/download/)

下载第三方库：[3rdParty](http://members.iinet.net.au/~bchrist/3rdParty_VC10_x86_x64.zip)

下载数据源：[数据源](http://www.openscenegraph.org/downloads/stable_releases/OpenSceneGraph-3.0/data/OpenSceneGraph-Data-3.0.0.zip)

下载编译工具：[Cmake](https://cmake.org/)

> Cmake是一个跨平台的安装工具，可以用简单的语句来描述所有平台的安装过程（CmakeLists）。他能过输出各种各样的makefile(适用于linux)或project文件（VS的工程文件等），能够测试编译器所支持的C++特性。Cmake并不直接建构出最终的软件，而是产生标准的建构档，（如Unix的makefile或Windows VS的project/workspaces）。然后在依据相应的集成环境用标准的方式构建软件。

第二步编译OSG源码
===

> 在D盘建立文件夹OSG：
> 
> D:\OSG\OpenSceneGraph : 源码解压位置
> 
> D:\OSG\3rdParty : 第三方库解压位置
> 
> D:\OSG\OpenSceneGraph-Data-3.0.0: 数据源解压位置

运行Cmake,将OpenSceneGraph文件夹中CmakeLists.txt,拖到Cmake界面。点击configure，选择VS12的编译器，finish后进行相关设置：

 - ACTUAL_3RDPARTY_DIR 值：D:/OSG/3rdParty
 - BUILD_OSG_EXAMPLES:勾上
 - CMAKE_INSTALL_PERFIX : D:/OSG/OpenSceneGraph;

点击configure后:

 - 将Advanced打勾
 - 将BUILD_MFC_EXAMPLE 设置为on,然后进行最后一次的configure配置
 - 点击generate，构建完成

第三步 编译生成
--------

 - 用VS2012打开OpenSceneGraph.sln,生成-批生成，对ALL_BUILD进行生成，选择Debug和Release两个版本。这一步要花费很多时间。
 - 生成完成之后，再次点击生成，批生成，生成install下的debug和release版本。

第四步 数据转移
--------

这时候经过编译后，我们需要将文件整理一下：

 - 将将D:\OSG\OpenSceneGraph下的bin，include，lib文件夹拷贝到C:\OSG中
 - 将D:\OSG\OpenSceneGraph-Data-3.0.0下文件拷贝到C:\OSG\data中。

第五步 环境变量设置
----------

对系统变量设置如下：

 - OSG_FILE_PATH: C:\OSG\data
 - PATH：C:\OSG\bin;

结语
--

到这里OSG安装完成，创建OSG项目可以参考[这里。](https://github.com/WenHuiXie/OSG/blob/master/OSG+VS2012%E5%AE%89%E8%A3%85%E5%8F%8A%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE.md)