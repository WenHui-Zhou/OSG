OSG+VS2012安装及环境配置
=================

这篇文章是OSG学习的第一篇文章，暑假折腾的OSG的安装和环境配置没来得及记下，刚刚重新过了一遍，现在把它们记下来，以备后续的之需。

OSG安装
-----

上官网下载最新的OSG版本，并安装。我的OSG版本为3.4 。安装的目录为C盘下的OSG目录。

一下是重要文件的目录：

> bin 文件夹 ————  C:\OSG\bin
> 
> include文件夹 ———— C:\OSG\include
> 
> lib 文件夹 ———— C:\OSG\lib
> 
> data 文件夹 ———— C:\OSG\data

OSG在VS2012中的环境配置
----------------

按照正常的流程在VS2012中新建一个win32的控制台程序。

下面开始环境的配置：

 - 打开项目的属性设置，在VC++目录下添加如下路径：


配置名     | 路径
-------- | ---
可执行文件目录 | C:\OSG\bin;
包含目录    | C:\OSG\include;
引用目录     | C:\OSG\lib;
库目录     | C:\OSG\lib;

 -  在链接器目录下找到输入输入，在输入里头添加如下设置：

在附依赖项中添加如下文件 ：

OpenThreadsd.lib

osgd.lib

osgDBd.lib

osgUtild.lib

osgGAd.lib

osgViewerd.lib

osgTextd.lib


以上就配置好了环境。下面是一个代码示例：


    #ifdef DEBUG                                   //debug模式
    #pragma comment(lib,"osgViewerd.lib")
    #pragma comment(lib,"osgDBd.lib")
    #pragma comment(lib,"OpenThreadsd.lib")
    #pragma comment(lib,"osgd.lib")
    #else                                         //release 模式
    #pragma comment(lib,"osgViewer.lib")
    #pragma comment(lib,"osgDB.lib")
    #pragma comment(lib,"OpenThreads.lib")
    #pragma comment(lib,"osg.lib")
    #endif // DEBUG
    
    #include <osgViewer/Viewer>
    #include <osgDB/ReadFile>
    
    int main()
    {
    	osg::ref_ptr<osgViewer::Viewer> viewer = new osgViewer::Viewer;
    	viewer->setSceneData(osgDB::readNodeFile("cow.osg"));
    	return viewer->run();
    }

以上代码将显示一头牛的模型。