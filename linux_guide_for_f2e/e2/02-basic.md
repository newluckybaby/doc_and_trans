                                                                                                  #基础知识#

本章将会介绍Linux的基础知识，包括文件系统、文件权限、桌面环境以及多个系统概念之间的关系。

##文件系统##

> 「一切皆文件」所描述的是*nix的重要特征，其扩展含义是任何IO资源，如文档、目录、硬盘、调制解调器、键盘、打印设备，甚至进程间通讯和网络通讯都被视做通过文件系统命名空间表示的流式文件。
>
> 这种方式的优点是大多数资源都可以使用相同的工具集与接口，这些资源被分类成不同的类型。当你打开文件的同时也就创建了一个文件描述符，这时文件路径成为一个寻址系统而文件描述符号则是字节流的IO接口。匿名管道或网络套接字创建文件描述符的过程类似，只是方法不同。因此「一切皆文件」称为「一切皆文件描述符」或许更为准确些。

以上引用来自 [Wikipedia/Everything_is_a_file](http://en.wikipedia.org/wiki/Everything_is_a_file)

###分区类型###

Linux常用的分区类型包括btrfs、ext3、ext4、reiserfs、reiser4、xfs、zfs等，较为常用和成熟的是ext3，它们之间的比较可以查看 [Wikipedia/Comparison_of_file_systems](http://en.wikipedia.org/wiki/Comparison_of_file_systems)。

###目录结构###

与Windows不同，Linux中并没有盘符一说，它通过目录结构来区分不同功能的文件，下表列出的是目录及其描述。

<table style="width:100%;">
<colgroup>
	<col style="width:20%;">
	<col style="width:80%;">
</colgroup>
<tr>
	<th>目录</th>
	<th>描述</th>
</tr>
<tr>
	<td>/</td>
	<td>文件系统根目录</td>
</tr>
<tr>
	<td>/bin</td>
	<td>表示「binaries」，用于存放某些例如 ls、cp 这些所有用户都需要的基础工具的二进制包。</td>
</tr>
<tr>
	<td>/sbin</td>
	<td>表示「system(superuser) binaries」，用于存放某些例如init等用于启动、维护或者修复系统的基础工具二进制包。</td>
</tr>
<tr>
	<td>/etc</td>
	<td>系统配置文件或系统数据文件。</td>
</tr>
<tr>
	<td>/dev</td>
	<td>表示「devices」，用于存放外围设备的文件表现。</td>
</tr>
<tr>
	<td>/dev/null</td>
	<td>被称作「位桶」或「黑洞」的伪设备，因为任何写入内容都会被丢弃，所以常被用来处理管道中不需要的数据。</td>
</tr>
<tr>
	<td>/dev/random</td>
	<td>读此文件时此伪设备将会产生一个伪随机数，它会在系统噪音熵不足的情况下利用系统噪音产生随机数序列，然后SSH一类的程序会将其用来加密随机数据来产生密钥。</td>
</tr>
<tr>
	<td>/dev/urandom</td>
	<td>与/dev/random 基本类似，不过既是在没有足够可用的系统噪音熵的情况下也可以返回伪随机数。</td>
</tr>
<tr>
	<td>/dev/zero</td>
	<td>可替代null，通常用于硬盘抹除。例如 <code>dd if=/dev/zero of=/dev/... bs=64k)</code></td>
</tr>
<tr>
	<td>/home</td>
	<td>用户目录</td>
</tr>
<tr>
	<td>/mnt</td>
	<td>表示「mount」，用于存储系统挂载点。</td>
</tr>
<tr>
	<td>/lib</td>
	<td>用于存放系统库以及类似kernel模块或者设备驱动等重要文件。</td>
</tr>
<tr>
	<td>/root</td>
	<td>超级用户「root」的用户目录。这个目录通常只有在初始化文件系统或者home下的用户目录需要执行维护时才会用到。</td>
</tr>
<tr>
	<td>/tmp</td>
	<td>用于存放临时文件，通常会在系统启动时被清除。</td>
</tr>
<tr>
	<td>/usr</td>
	<td>起初为用户主目录，现在用途变更为用于存储一些类似X窗口系统、KDE、Perl的可执行文件、库以及共享资源，这些文件不像某些系统文件那么重要。</td>
</tr>
<tr>
	<td>/usr/bin</td>
	<td>用于存放不在/bin、/sbin、/etc等目录中的二进制程序。</td>
</tr>
<tr>
	<td>/usr/include</td>
	<td>用于存放整个系统的开发头文件，这些头文件大多数被C语言使用#inlucde语法载入，include目录的命名也来自于此。</td>
</tr>
<tr>
	<td>/usr/lib</td>
	<td>用于处于/usr或者其他目录下可执行程序所依赖的包和数据文件。</td>
</tr>
<tr>
	<td>/usr/local</td>
	<td>与/usr在就结构上类似，但其子目录用于存储非系统发行版本的部分，例如自定义程序或者BSD包管理器安装的文件。</td>
</tr>
<tr>
	<td>/var</td>
	<td>「variable」的缩写，用于存储经常变动的文件，主要指文件大小的变动。例如系统发送给用户的邮件或者进程锁文件。</td>
</tr>
<tr>
	<td>/var/log</td>
	<td>用于存储系统日志文件。</td>
</tr>
<tr>
	<td>/var/mail</td>
	<td>用于存储所有接受的邮件，非root的用户能够访问他门各自的邮件，通常情况下，此目录为指向/var/spool/mail的一个软链接。</td>
</tr>
<tr>
	<td>/var/spool</td>
	<td>spool目录，包含打印任务，mail spool和其他队列任务。</td>
</tr>
<tr>
	<td>/var/tmp</td>
	<td>用于存放系统重启时需要保留的临时文件。</td>
</tr>
<tr>
	<td>/proc</td>
	<td>包含所有进程数据，通常情况下，此目录内容在由任何进程在读kernel时由kernel即时创建，不过事实上它并不存储在硬盘上。它的内容是对系统以及运行在系统熵的进程的反馈。如果你使用mount命令，可以发现它是以一个特殊的文件系统类型「proc」挂载在此处。</td>
</tr>
<tr>
	<td>/opt</td>
	<td>用于存储一些附加的软件，这里存储的程序可能比/usr下面的还要多。</td>
</tr>
<tr>
	<td>/media</td>
	<td>U盘以及播放器等可移除设备的挂载点。</td>
</tr>
<tr>
	<td>/srv</td>
	<td>用于存储服务器数据。</td>
</tr>
<tr>
	<td>/boot</td>
	<td>用于存储系统启动所依赖的重要文件。</td>
</tr>
<tr>
	<td>/sys</td>
	<td>用于存储硬件信息。</td>
</tr>
</table>

以上引用来自 [Wikipedia/Unix_directory_structure](http://en.wikipedia.org/wiki/Unix_directory_structure)

###挂载###

在Linux环境中，常见的挂载包括U盘、移动硬盘、samba、windows网络共享等情况。当前大部分的发行版都默认安装了ntfs3g包，因此手动执行``mount``的情况并不多。关于常见命令以及一些概念可以通过[Wikipedia/Mount_(Unix)](http://en.wikipedia.org/wiki/Mount_(Unix))来查看。需要注意的是，大多数挂载出现的问题的主要由于文件系统和编码的不同引起的。

###文件链接###

文件链接分为硬链接和软链接（符号链接），其主要不同点简单说在于软链接创建的指向原文件的新文件，而硬链接并不创建新文件，只是inode链接计数增加。它们之间的区别可查看[Linux中的软链接与硬链接](http://www.linuxeden.com/html/front/20100929/104946.html)。

我们通过下列方式来创建软链接：

    ln -s source_file link_name
    
如果移除参数 s,则表示创建了一个名称为link_name指向source_file的硬链接。需要主要的是，硬链接的创建会有几个限制条件，例如不能创建指向目录的硬链接、不能跨文件系统创建硬链接。

###文件类型##

Linux下的文件可以分为以下几类：

<table style="width:100%;text-align:center;">
<tr>
	<th>类型</th>
	<td>常规文件</td>
	<td>目录</td>
	<td>符号链接</td>
	<td>命名管道</td>
	<td>socket</td>
	<td>设备文件</td>
	<td>门</td>
</tr>
<tr>
	<th>符号表示</th>
	<td>-</td>
	<td>d</td>
	<td>l</td>
	<td>p</td>
	<td>s</td>
	<td>c（字符设备）、b（块设备）</td>
	<td>D</td>
</tr>
</table>

详细解释可查看 [Wikipedia/Unix_file_types](http://en.wikipedia.org/wiki/Unix_file_types)

##文件权限管理##

文件权限管理是Linux系统中最常执行的操作，例如你需要为一个shell脚本增加一个可执行权限或者为某一用户增加指定文件的读或者写权限。

###文件权限描述###

变更文件权限前，通常会查看此文件当前的权限状态，例如执行：

    ls -dl /etc/passwd

输出结果为：

    -rwxr-—r-- ... /etc/passwd

前十个字符，则表示的是文件类型与文件权限，其中r表示读（read）权限，w表示写（write）权限，x则表示执行（execute）权限，完整的解释如下图所示。

![2-1 文件权限](https://raw.github.com/sodabiscuit/doc_and_trans/master/linux_guide_for_f2e/e2/resources/02-01.png)

###文件权限管理###

文件权限管理常用的命令包括``chown``、``chgrp``、``chmod``。其中``chown``表示更改所有者，``chgrp``表示更改其所属权限组，下面的内容主要讲述我们最为常用的``chmod``。

####绝对模式####

``chmod``绝对模式的命令形式如下：

    chmod path/to/file 777

其中的777表示其拥有权限的绝对模式表示，其计算方式可参考下表：

<table style="text-align:center;">
<tr>
	<th colspan="3">owner</th>
	<th colspan="3">group</th>
	<th colspan="3">everyone</th>
</tr>
<tr>
	<td>read</td>
	<td>write</td>
	<td>execute</td>
	<td>read</td>
	<td>write</td>
	<td>execute</td>
	<td>read</td>
	<td>write</td>
	<td>execute</td>
</tr>
<tr>
	<td>400</td>
	<td>200</td>
	<td>100</td>
	<td>40</td>
	<td>20</td>
	<td>10</td>
	<td>4</td>
	<td>2</td>
	<td>1</td>
</tr>
</table>

####相对模式####

``chmod``相对模式的命令形式如下：

    chmod a+x path/to/file 777

其中的「a」、「+」、「x」的含义可参考下表：

<table style="text-align:center;">
<tr>
	<th>u</th>
	<th>g</th>
	<th>o</th>
	<th>a</th>
</tr>
<tr>
	<td>owner</td>
	<td>group</td>
	<td>everyone</td>
	<td>all</td>
</tr>
</table>

<table style="text-align:center;">
<tr>
	<th>+</th>
	<th>-</th>
	<th>=</th>
</tr>
<tr>
	<td>add</td>
	<td>remove</td>
	<td>emulate absolute mode</td>
</tr>
</table>

##桌面环境##

与Window不同，Linux的桌面环境可以自定义，我们常见的Linux桌面环境包括GNOME、KDE、LXDE、Xfce等。桌面环境的主要组成部分是窗口管理器，窗口管理器可分为常见的X窗口管理器（例如GNOME采用的窗口管理器 Metacity），平铺式窗口管理器（例如awesome）。更多的桌面环境和窗口管理器可查看：

* [Wikipedia/桌面环境](http://zh.wikipedia.org/wiki/%E6%A1%8C%E9%9D%A2%E7%8E%AF%E5%A2%83)
* [Wikipedia/X窗口管理器](http://zh.wikipedia.org/zh-cn/X%E7%AA%97%E5%8F%A3%E7%AE%A1%E7%90%86%E5%99%A8)