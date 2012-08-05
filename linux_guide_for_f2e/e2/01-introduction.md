#介绍#

本章将会介绍Linux的历史、GNU计划，Linus Torvalds、RMS等人物以及Linux的设计思想与文化，本章节大部分内容来自维基百科。

##Linux历史##

###关于Linux###

> MINIX是一个廉价的小型类Unix操作系统，是为在计算机科学用作教学而设计的，作者是安德鲁·斯图尔特·塔能鲍姆。从第三版开始，MINIX是自由软件，而且被“严重的”重新设计。
>
> 1991年，芬兰人林纳斯·托瓦兹在赫尔辛基大学上学，对操作系统很好奇，并且对MINIX只允许在教育上使用很不满(其不允许任何商业使用），于是开始写他自己的操作系统，这就是后来的Linux内核。
>
> 林纳斯·托瓦兹开始在MINIX上开发Linux内核，为MINIX写的软件也可以在Linux内核上使用。后来Linux成熟了，可以在自己上面开发自己了。使用GNU 软件代替MINIX的软件，因为使用从GNU 系统来的源代码可以自由使用，这对新操作系统是有益的。使用GNU GPL 协议的源代码可以被其他项目所使用，只要这些项目使用同样的协议发布。为了让Linux 可以在商业上使用，林纳斯·托瓦兹决定改变他原来的协议（这个协议会限制商业使用)，使用GNU GPL协议来代替。开发者致力于融合GNU 元素到Linux 中，做出一个有完整功能的、自由的操作系统。
>
> Linux的第一个版本在1991年9月被大学FTP server管理员Ari Lemmke发布在Internet上，最初Torvalds称这个内核的名称为"Freax"，意思是自由（"free"）和奇异（"freak"）的结合字，并且附上了"X"这个常用的字母，以配合所谓的类Unix的系统。但是FTP服务器管理员嫌原来的命名“Freax”的名称不好听，把内核的称呼改成“Linux”，当时仅有10000行程序码，仍必须执行于Minix操作系统之上，并且必须使用硬盘开机；随后在10月份第二个版本（0.02版）就发布了，同时这位芬兰赫尔辛基的大学生在comp.os.minix上发布一则讯息
>
>> Hello everybody out there using minix- I'm doing a (free) operation system (just a hobby, won't be big and professional like gnu) for 386(486) AT clones.
>
> 1994年3月，Linux1.0版正式发布，Marc Ewing成立了Red Hat软件公司，成为最著名的Linux经销商之一。
>
> Linux的标志和吉祥物是一只名字叫做Tux的企鹅，标志的由来是因为Linus在澳洲时曾被一只动物园里的企鹅咬了一口，便选择了企鹅作为Linux的标志。更容易被接受的说法是：企鹅代表南极，而南极又是全世界所共有的一块陆地。这也就代表Linux是所有人的Linux。

以上引用来自 [维基百科/Linux](http://zh.wikipedia.org/zh-cn/Linux)

###关于GNU###

> GNU计划，有译为“革奴计划”，是由理查德·斯托曼在1983年9月27日公开发起的自由软件集体协作计划。它的目标是创建一套完全自由的操作系统GNU。理查德·斯托曼最早是在net.unix-wizards新闻组上公布该消息，并附带一份《GNU宣言》等解释为何发起该计划的文章，其中一个理由就是要“重现当年软件界合作互助的团结精神”。
>
> GNU是“GNU's Not Unix”的递归缩写，为避免与gnu（非洲牛羚，发音与“new”相同）这个单词混淆，斯托曼宣布GNU应当发音为“Guh-NOO”（/ˈgnuː/），与“canoe”发音相似。
>
> UNIX是一种广泛使用的商业操作系统的名称。由于GNU将要实现UNIX系统的接口标准，因此GNU计划可以分别开发不同的操作系统。GNU计划采用了部分当时已经可自由使用的软件，例如TeX排版系统和X Window视窗系统等。不过GNU计划也开发了大批其他的自由软件，这些软件也被移植到其他操作系统平台上，例如Microsoft Windows、BSD家族、Solaris及MacOS。

> 为保证GNU软件可以自由地“使用、复制、修改和发布”，所有GNU软件都包含一份在禁止其他人添加任何限制的情况下，授权所有权利给任何人的协议条款，GNU通用公共许可证（GNU General Public License，GPL）。这个就是被称为‘公共版权’的概念。GNU也针对不同场合，提供GNU宽通用公共许可证与GNU自由文档许可证这两种协议条款。

以上引用来自 [维基百科/GNU计划](http://zh.wikipedia.org/zh-cn/GNU%E8%A8%88%E5%8A%83)

###关于GNU/Linux###
> GNU/Linux是GNU计划的支持者与开发者，特别是其创立者理查德·斯托曼对于一个以Linux闻名的类Unix操作系统的称呼。
>
> 由林纳斯·托瓦兹及其他人士开发的Linux并不是一个完整的操作系统，而仅仅是一个类Unix内核。事实上，Linux一开始是以完成Minix内核的功能为目标，林纳斯想做一个“比Minix更好的Minix”。而GNU计划始于1984年，终极目标是完成一套基于自由软件的完整作业操作系统。到1991年Linux的第一个版本公开发行时，GNU计划已经完成除了操作系统内核之外的大部分软件，其中包括了一个壳程序（shell），C语言程序库以及一个C语言编译器。林纳斯·托瓦兹及其他早期Linux开发人员加入了这些工具，而完成了Linux操作系统。但是尽管Linux是在GNU通用公共许可证下发行，它却不是GNU计划的一部分。
>
> 正是由于Linux使用了许多GNU程序，理查德·斯托曼认为应该将该操作系统称为“GNU/Linux”比较恰当。
>
> 有部分Linux套件，包括了Debian，采用了“GNU/Linux”的称呼。但大多数商业Linux套件依然将操作系统称为Linux。有些人也认为“操作系统”一词指的应该只是系统的内核，其他程序都只能算是应用软件，这么一来，该操作系统的内核应叫Linux，而Linux套件是在Linux内核的基础上加入各种GNU工具。
>
> 一些人拒绝使用“GNU/Linux”作为操作系统名称的人认为Linux朗朗上口，短而好记，并且斯托曼直到1990年代中期Linux开始流行后才要求更名。
>
> 大多数GNU/Linux套件使用XFree86或X.Org服务器作为图像系统，并使用GNOME和KDE等桌面管理器。

以上引用来自 [维基百科/GNU/Linux](http://zh.wikipedia.org/zh-cn/GNU/Linux)

##Linux人物##

###关于Linus Torvalds###

> 林纳斯·本纳第克特·托瓦兹（Linus Benedict Torvalds, 1969年12月28日－），生于芬兰赫尔辛基市，拥有美国国籍。他发起了Linux内核的开源项目，并以此广为人知，是当今世界最著名的电脑程序员、黑客之一。他还发起了Git这个开源项目，并为主要的开发者。

以上引用来自 [维基百科/林纳斯·托瓦兹](http://zh.wikipedia.org/zh-cn/%E6%9E%97%E7%BA%B3%E6%96%AF%C2%B7%E6%89%98%E7%93%A6%E5%85%B9)

###关于RMS###

> 理查·马修·斯托曼（英语：Richard Matthew Stallman，简称rms，1953年3月16日－），美国自由软件运动的精神领袖、GNU计划以及自由软件基金会的创立者。作为一个著名的黑客，他的主要成就包括Emacs及后来的GNU Emacs，GNU C 编译器及GDB 调试器。他所写作的GNU通用公共许可证是世上最广为采用的自由软件许可证，为copyleft观念开拓出一条崭新的道路。
>
> 1990年代中期，斯托曼作为一个政治运动者，为自由软件辩护，对抗软件专利及版权法的扩张。他对程式设计方面的投入都放在GNU Emacs上。他的从演讲中获得的收入，已足够维持自己的生活。
>
> 他最大的影响是为自由软件运动竖立道德、政治及法律框架。他被许多人誉为当今自由软件的斗士、伟大的理想主义者。

以上引用来自 [维基百科/理查德·斯托曼](http://zh.wikipedia.org/zh-cn/%E7%90%86%E6%9F%A5%E5%BE%B7%C2%B7%E6%96%AF%E6%89%98%E6%9B%BC)

##Unix/Linux设计思想##

1. 小即是美；
- 让每一个程序只做好一件事情；
- 尽快建立原型；
- 舍高效率而取移植性；
- 使用纯文本来存储数据；
- 充分利用软件的杠杆效应；
- 使用shell脚本来提高杠杆效应和可移植性；
- 避免强制性的用户界面；
- 让每一个程序都成为过滤器；

##关于Linux的电影与文章##

* [操作系统革命](http://movie.douban.com/subject/1437389/)

![操作系统革命](https://github.com/sodabiscuit/doc_and_trans/raw/master/linux_guide_for_f2e/e2/resources/01-01.jpg)

> 《操作系统革命》（Revolution OS）是一部2001年由J·T·S·摩尔（J. T. S. Moore）导演的纪录片电影，该电影追述了GNU、Linux、自由软件运动以及开放源代码运动长达二十余年的历史。该片的主演有理查德·斯托曼、林纳斯·托瓦兹、布鲁斯·斐伦斯、拉里·奥古斯丁与埃里克·雷蒙等。

* [大教堂与市集](http://www.aka.org.cn/Docs/c&b.html)

---

[上一章：前言](00-preface.md) [下一章：基础知识](02-basic.md)

