---
title: Python能用来做什么
category: Python
---

Python唯一的缺点就是速度没有编译类和低级的C, C++快。
简单地说，Python的标准实现将源代码编译成中间格式，这种格式叫字节码（byte code），然后运行时去解释字节码。字节码是可移植的，因为它是独立于平台的格式。然而，因为Python不会被编译成机器码，一些程序将会比C等语言运行地更慢。
你是否关心执行速度取决于你写的是什么类型的程序。Python已经被优化地很好，Python代码在一些领域运行地很快。更进一步说，当你在Python脚本中做一些真实的工作，例如处理一个文件或建造一个图形化用户界面，你的程序将以C的速度运行，因为这样的任务会直接分派给Python解释器中的已编译的C代码。从根本上说，你从Python的快速开发中的获益要大于执行速度的损失，尤其是在现代计算机的速度下更是如此。
但是，即使是现代计算机的速度下，也有一些任务是要求很高的执行速度的，例如数值计算和动画。这些要求代码以C的速度运行。即使这样，你仍然可用Python来开发，只不过你需要将程序分割开，需要高速运行的部分用C来开发，然后用Python脚本将它们联系到一起。

### 谁在用Python

Google在它的web搜索系统里广泛使用Python。
Youtube的分享服务基本上是由Python写的。
Dropbox存储服务代码，无论是它的服务端和桌面客户端软件，基本上是由Python写的。
EVE Online，一个大型多人在线游戏，广泛使用Python
广泛使用的BitTorrent点对点文件分享服务项目是由一个Python程序创建的。
工业光魔（Industrial Light & Magic）和皮克斯（Pixar）等使用Python生产动画电影。
ESRI使用Python作为它的受欢迎的GIS地图产品的终端自定义工具
Google的App Engine的web开发框架使用Python作为开发语言。
NSA使用Python来做公开密钥加密和智能分析
iRobot使用Python来开发商业和军事机器人装置。
文明4游戏的自定义脚本化事件完全由Python实现。
一童一电脑（One Laptop Per Child）项目用Python构建它的用户界面和活动模型。
Netflix和Yelp都已把Python在其软件基础设施中的地位写入文档。
Intel，思科，Hewlett-Packard，Seagate，Qualcomm和IBM使用Python做硬件测试。
摩根大通（JPMorgan Chase）、瑞银集团（UBS）、Getco、Citadel将Python用做金融市场预测。
NASA、洛斯阿拉莫斯国家实验室（Los Alamos）、费米国立加速器实验室（Fermilab），喷气推进实验室（JPL）等等使用Python做科学计算任务。

### Python能用来做什么

作为一个通用用途的语言，Python可用作从网站开发到机器人和航天器控制。具体来说，有几个领域，如系统编程、图形化界面、Internet脚本、组件集成、数据库编程、数值和科学计算、游戏、图像、数据挖掘、机器人、Excel。

#### 系统编程
Python的内置操作系统服务的接口使得它适合作为可移植的、可维护的系统管理工具（有时被称为shell工具）。Python程序可以搜索文件和目录树、启动其他程序、做通过处理器和线程做并行计算等等。
Python的标准库绑定了可移植操作系统接口（POSIX），并且支持所有常见的操作系统工具：环境变量、文件、sockets、pips、processes、多线程、正则表达式模式匹配、命令行参数、标准流式接口（standard stream interfaces）、shell命令启动器、文件名expandsion、zip file工具、XML和JSON语法分析器、CSV文件处理器等等。   Python的系统接口被设计为可移植的，举个例子，一个复制目录树的脚本可以不加修改在任何主要Python平台上运行。

#### 图形化界面
Python的简单性使得它适合用来做桌面端的图形化界面。Python内置被称作tkinter的Tk图形化界面接口。tkinter使得Python程序可以实现可移植的图形化界面。Python/tkinter无差别运行在微软Windows、X Windows（在Unix和Linux上）和Mac OS上。一种免费扩展包，PMW扩展了tkinter的工具集。基于C++库的wxPython图形化界面接口提供了另一种实现可移植图形化界面的方式。
高级工具集例如Dabo的底层基于wxPython和tkinter。使用适当的库，你可以使用其他工具集来实现Python图形化界面。例如：使用Qt的PyQt、使用GTK的PyGTK、使用MFC的PyWin32、使用.NET的IronPython、使用Jython或JPype的Swing。

#### Internet脚本
Python的标准Internet模块使Python程序可应用在多方面的网络任务上。脚本可以使用sockets通信，从表单提取数据送到服务端的CGI脚本，通过FTP传送文件、语法解析和生成XML和JSON文档、发送、接收、组合、语法解析email、通过URL获取网页、语法解析获取到的HTML、通过XML-RPC、SOAP、Telnet通信等等。Python的库使得这些任务惊人的简单。
在网上有一大堆第三方工具可做Python的Internet编程。举例来说，HTMLGen由Python类的描述生成HTML文件，mod_python包在Apache web服务器上高效地运行Python，Jython系统提供Python/Java无缝整合，而且支持服务端applet运行在客户端。
现在有很多成熟的web开发框架，如Django、TurboGear、web2py、Pylons、Zope和WebWare，这些框架支持用Python构建多特性的、高质量的网站。这些框架包含一些如下特性：对象关系映射、MVC架构、服务端脚本编程、模版、AJAX支持，这些特性用来提供完备的企业级web开发解决方案。

#### 组件集成
Python可以被C/C++系统扩展和集成，这使得Python作为灵活的胶水语言在为其他系统和组件编写脚本方面大有可为。举例来说，集成C语言库到Python允许Python测试和启动库的组件，并且在产品中嵌入Python允许现场编写定制代码，这种定制并不需要重新编译整个项目。
类似SWIG和SIP代码产生器的工具能自动化大部分连接Python中的被编译组件的工作。Cython系统允许程序员混合Python和类C代码。更大点的框架，类似Python的Windows支持的COM、基于Java的Jython、基于.NET的IronPython提供另外可选的方式为组件编写脚本。举例来说，在windows上，Python脚本可以用框架去为Word和Excel编写脚本，访问Silverlight等等。

#### 数据库编程
对于传统数据库的要求，有Python对所有常用的关系型数据库系统的接口—Sybase、Oracle、Informix、ODBC、PostgreSQL、SQLite等等。Python也定义了一个可移植的接口，利用这个接口可以编写独立于底层数据库的代码，不论底层数据库是什么，存取数据库数据只用一套代码即可。例如，如果你实现了这个可移植的接口，那么为Mysql编写的代码就可能大部分不做更改地运行在其他系统如Oracle上。 Python本身从2.5就含有SQLite，支持原型和基本的程序存储需求。
在非SQL方面，Python有pickle模块提供简单的对象持久化系统，它允许Python简单存取Python对象到文件。在Web方面，有第三方开源系统，如ZODB和Durus为Python脚本提供完全的面向对象数据库系统。其他的如SQLObject和SQLAlchemy实现了对象关系映射（ORM），而ORM是指将Python类模型和数据库表对应起来。Pymongo是MongoDb的一个接口，它以接近于Python的lists和dictonaries的方式存储数据，它的文本会被Python的标准库json模块语法解析和创建。
其他系统仍会提供更多的方法存储数据，包括谷歌的App Engine的datastore，还有其他使用云存储方式，如Azure、PiCloud、OpenStack和Stackato。

#### 值和科学计算
数值计算领域也重度使用Python。这是一个传统来讲不属于脚本语言的领域，但现在发展成Python最吸引人的用户案例。NumPy是Python的高性能数值计算扩展，其中包括对数学库的接口等。
Python的另一些数值的工具支持动画、3D可视化、并行处理等。流行的Scipy和ScientificPython扩展提供了一些科学计算工具库，并且使用NumPy作为核心组件。Python的PyPy实现在科学计算领域也获得了关注，一部分原因是因为其排序算法会运行地10倍到100倍快速。

#### 更多：游戏，图像，数据挖掘，机器人，Excel...
更多的使用Python的领域见以下。你会发现这些工具允许你用Python做：
游戏编程和多媒体，使用pygame、cgkit、pyglet、pysoy、panda3D等。
串口通信，使用PySerial扩展。
图像处理，使用PIL和更新的分支Pillow、PyOpenGL、Bender、Maya等。
机器人控制，使用PyRo。
自然语言分析，使用NLTK。
Excel表格函数和宏编程，使用PyXLL、DataNitro插件。
媒体文件内容和元数据标签处理，使用PyMedia、ID3、PIL/Pillow等。
人工智能，使用PyBrain神经网络库和Milk机器学习工具集。
专业系统编程，使用PyCLIPS、Pyke、Pyrolog、PyDatalog。
网络监测，使用zenoss。
Python脚本设计和建模，使用PythonCAD、PythonOCC、FreeCAD等。
文档处理和生成，使用ReportLab、Sphinx、Cheetah、PyPDF等。
用xml库包进行XML语法解析，使用xmlrpclib模块和其他第三方库。
JSON和CSV文件处理，使用json、csv模块。
数据挖掘，使用Orange框架、Pattern包、Scrapy。

你甚至可以用PySolFC程序玩纸牌，并且你当然也可以编写Python脚本干一些更偏门一点的事，如处理每天的系统管理，处理你的邮件，管理你的文档和媒体库。你将在PyPI网站找到一些有帮助的链接。

虽然Python有很广泛的应用范围，但Python在很多应用范围都是作为集成组件的角色。当编译语言例如C的前端使得Python在很多领域都有用处。作为一个支持集成的通用的语言，Python广泛适用于很多领域。