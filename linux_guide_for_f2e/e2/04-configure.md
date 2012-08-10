#基础环境配置#

本章将会介绍多语言环境、邮件客户端、办公套件、即时通讯等基础环境的配置，同时会覆盖ubuntu的包管理的一些常用方法。

##包管理系统##

Ubuntu是基于Debian的发行版本，同Debian一样，它采用软件包管理器**APT**来管理软件包。我们常说的APT是高级包装工具（Advanced Packaging Tool）的缩写。在以Debian为基础的发行版本中，可以这样理解APT与**dpkg**的关系，dpkg是包管理器的基础，而APT则是dpkg的前端展现，它在dpkg之上封装了处理仓库源管理以及包之间的依赖关系等功能。

###APT的配置###

在详细了解APT配置之前，我们需要了解软件源的一些分类。一般来讲，从来源上可分为官方源和第三方源，第三方源的例子可以访问[VirtualBox官方安装文档](https://www.virtualbox.org/wiki/Linux_Downloads)。第三方源由于ubuntu中``apt-secure``的安全要求，往往需要手动导入asc公钥，不过PPA是个例外，PPA是Personal   Package Archives的缩写，是Ubuntu母公司Canonical为第三方源提供的一个发布平台，通过PPA，你可以很容易的安装一个较新版本的包，从而某一方面弥补了Ubuntu发布策略的不足（例如ArchLinux采用的滚动发布策略）。

从功能上讲，软件包可以分为以下几类，而官方源、官方源镜像或者第三方源的服务器上文件目录也都是按照这个分类来划分。

> main Canonical支持的开源软件
>
> universe 社区维护的开源软件
>
> restricted 设备的专有驱动
>
> multiverse 被版权和合法性文件限制的软件

####软件源配置####

Ubuntu的软件源配置文件：

    /etc/apt/sources.list
    
如果安装Ubuntu时选择的语言为中文，则默认源为国内的搜狐镜像，可以不用再做更改。如果安装时选择的语言为非中文，考虑到下载速度原因，可将源修改为国内的源。国内较为常用的源除搜狐镜像外，还有网易镜像。修改方法可参考网易给出的[指南文档](http://mirrors.163.com/.help/ubuntu.html)。