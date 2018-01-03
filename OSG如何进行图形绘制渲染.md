OSG如何进行图形绘制渲染
=============

一.OSG场景树
--------

OSG的场景树决定了OSG的进行图形绘制的方式与步骤，是组织整个绘制画面非常重要的一个部分。

场景图采用自顶向下、分层的树状数据结构来组织空间数据集。下面是一段OSG简单场景的代码示例：

    int main()
    {
	    osg::ref_ptr<osgViewer::Viewer> viewer = new osgViewer::Viewer();
	    osg::ref_ptr<osg::Group> = new osg::Group();
	    osg::ref_ptr<osg::Node> node = osgDB::readNodeFile("cow.osg");
	    root->addChild(node.get());
	    viewer->setSceneData(root.get());
	    viewer->realize();
	    viewer->run();
    }

**解析：**

    osg::ref_ptr<osgViewer::Viewer> viewer = new osgViewer::Viewer(); 
创建了整个场景的**视景体**，视景体是指成像景物所在空间的集合。它是一个空间集合体，投影变换的目的就是定义一个视景体。语句`viewer->setSceneData(root.get())` 将场景树加入视景体，作为整个场景需要绘制的内容viewer相当于一个相框，场景树相当于相框上的照片。

    osg::ref_ptr<osg::Group> = new osg::Group();

Group在场景树中称为组节点，另外还有Node与Geode叶节点。共同组成一棵场景树。

**Geode叶节点**

主要用于保存几何信息，通过`addDrawable` 函数来关联需要渲染的几何体信息`Geometry`。

实例：

    osg::ref_ptr<osg::Geode> geode = new osg::Geode;
    osg::ref_ptr<osg::Geomtry> geom = new osg::Geomtry;
    geode->addDrable(geom);
    //设置顶点
    osg::ref_ptr<osg::Vec3Array> v = new osg::Vec3Array();
    v->push_back(osg::Vec3(-0.5,0.0,-0.5));
    ...
    geom->setVertexArray(v.get());
    //设置法线
    osg::ref_ptr<osg::Vec3Array> normal = new osg::Vec3Array();
    normal->push_back(osg::Vec3(1.0,0.0,0.0));
    geom->setNormalArray(normal.get());   
    geom->setNormalBinding(osg::Geometry::BIND_OVERALL);
    //设置纹理坐标
    osg::ref_ptr<osg::Vec2Array>vt = new osg::Vec2Array();
    vt->push_back()...
    geometry->setTexCoordArray(0,vt.get())//索引，纹理坐标
    geometry->addPrimitiveSet(new osg::DrawArrays(osg::PrimitiveSet::QUADS,0,4));  //设置顶点的关联方式
    //设置颜色数组
    osg::ref_ptr<osg::Vec4> color = new osg::Vec4;
    color->push_back();
    ...
    geom->setColorArray(color.get());
    root->addChild(geode);
    viewer->setSceneData(root.get());

如上，顶点数据按照QUAD的方式（四边形）组织起来，同时设置了纹理坐标，每一个纹理坐标与顶点坐标按照顺序（索引）绑定起来，设置了纹理之后纹理将随顶点变化。此外设置了颜色数组，颜色数据将与顶点数据相互绑定，四方形中间的区域颜色将按插值填充。