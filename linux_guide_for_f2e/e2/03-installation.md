#安装#

本章将会简单介绍Linux的安装过程，选择的发行版本为Ubuntu。

##选择合适的版本##

选择Ubuntu作为演示版本，首要原因是它的易用性，另外考虑Ubuntu大的用户群体和良好的社区环境可使学习成本降到最低。Ubuntu有自己独特的发行策略，一般来说我们选择LTS版本即可。

> **LTS** 是长支持（Long Term Support）版本的缩写。
>
> 我们每六个月就会发行一个新的桌面和服务器版本，也就是说你总是能获取开源世界提供的最新、最优秀的应用程序。基于安全的原则，桌面或服务器版本都会提供至少十八个月的安全更新支持。
>
> 长支持版本的发布周期是两年。长支持的桌面版本会提供三年的支持，服务器版本则为五年。不过从12.04长支持版本开始，桌面版本的支持时间延长至五年。长支持版本不需要支付任何额外的费用，我们以自己最大的努力为每个用户提供支持。另外，Ubuntu的版本升级都是免费的。

![ubuntu截至二零一三年四月的发布计划](https://raw.github.com/sodabiscuit/doc_and_trans/master/linux_guide_for_f2e/e2/resources/03-01.png)

以上内容来自[Ubuntu wiki/LTS](https://wiki.ubuntu.com/LTS/)

##选择安装方式##

通常来说，Ubuntu常用的两种安装方式是U盘(或CD)和网络安装，考虑到安装的复杂度，我们以传统安装方式为例，网络安装的步骤可查看指南第一版中对应章节[安装Ubuntu桌面环境](https://github.com/sodabiscuit/doc_and_trans/blob/master/linux_guide_for_f2e/e1/install_ubuntu_desktop.wiki)。

安装盘的制作包含以下两个步骤：

1. 下载光盘镜像；
2. 使用烧录工具将镜像烧录至U盘或CD；

### 下载光盘镜像 ###

国内用户可选择较为常用的网易镜像下载：

    http://mirrors.163.com/ubuntu-releases/

### 烧录至载体###

如果在Mac或者其他Linux发行版本中，使用``dd``命令即可完成烧录过程。具体步骤可以参考[Wikipedia/Dd](http://en.wikipedia.org/wiki/Dd_\(Unix\))。在Windows环境下可参考[Ubuntu官方U盘烧录教程](http://www.ubuntu.com/download/help/create-a-usb-stick-on-windows)。

##基本安装步骤##

Ubuntu桌面版的安装已经基本上接近自动化，按照安装指引一步步进行既可，因为最新的发行版本中文件复制和配置可以并行操作，安装时间并不会太久。不过需要注意的是分区步骤，默认buntu的自动分区除去swap外，只会设置一个挂载点，即``/``，这样可能会对后期系统升级、更换带来影响，所在建议在分区步骤时选择手动分区。下面的分区方案可作为参考，以硬盘容量为120G为例：

    / 20G
    /boot 250M
    /swap 4G（约等于内存）
    /home ~95G(剩余空间)
    
另外，如果你选择安装的时32位的版本，如果要开始大内存支持，需要安装PAE包。
