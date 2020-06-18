---
layout: post
title: MySQL · 前世今生
description: ""
modified: 2014-12-24
tags: [高级MySql]
---

### 1. MySQL之父
<p style="text-align: center;"><img src="https://upload-images.jianshu.io/upload_images/6748624-554af6456b1a10c6.jpg?imageMogr2/auto-orient/strip|imageView2/2/format/webp"></p>


是的，就是他：

**Michael "Monty" Widenius**

- 1962年3月3日出生于芬兰赫尔辛基
- 开源 MySQL数据库的创始成员
- MySQL AB公司的首席技术官
- MySQL数据库命名人、MariaDB创始人兼首席技术官
- 独自完成撰写MySQL数据库服务器端95%的代码。



### 2. MySQL名字的由来
MySQL、MariaDB、MaxDB这三个名字可不是灵光一闪取的，而是有着渊源：

- Monty有一个女儿，名叫My，于是他将自己开发的第一个数据库命名为MySQL。
- Monty有一个儿子，名为Max，于是2003年，SAP公司与MySQL公司建立合作伙伴关系后，Monty将与SAP合作开发的数据库命名为MaxDB。
- 而现在最新的MariaDB数据中的Maria则是Monty小女儿的名字。
  
可以看出，这些产品都是 Monty 的亲儿子们

### 4.历史

MySQL里历史最早可以追溯到1979年(那一年中美打破了长达30年的隔阂，正式建交），当时，Monty和Allan Larsson合作开了一家名为Tcx的咨询公司。为拓展业务，在一个月朗风清的夜里，Michael基于BASIC语言开发出了他的第一个数据库报表产品————**UNIREG**

受限于当年的硬件条件，**UNIREG**是运行在瑞典人制造的ABC800计算机上，ABC800的内存只有32KB，CPU是频率只有4MHz的Z80。

公司业务情况暂不清楚，但Tcx公司持续存活，时间推到4年后(1983年),Monty遇到了David Axmark，两人相见恨晚，开始合作运营TcX，Monty负责技术，David搞管理，慢慢的，TcX将UNIREG移植到其他更加强大的硬件平台，主要是Sun的平台。

随着业务发展，Tcx公司服务的客户越来越多，突然有一天，Monty接到了一个项目需要为**UNIREG**提供更加通用的SQL接口以满足客户的需求。于是，Monty找到了David Hughes（mSQL的发明人），商讨合作，然后，经过多番测试，发现mSQL的速度不尽如人意，无法满足客户的需求。

于是，为了挣这一单，Monty狠狠心，准备重新设计了一套全新的系统，这时间是漫长的，直到10年后(1995年)，MySQL才发行了第一个内部版本。

然后紧接着1996年，MySQL发布第一个正式版，该正式版当时只能运行在Sun Solaris上。

为了发展MySQL，在接下来的两年内，MySQL被移植到不同的平台，同时加入了不少新的特性。到1998时，MySQL能够运行在10多种操作系统之上，其中包括应用非常广泛的 FreeBSD、Linux、Windows 95和Windows NT等

MySQL的发展并不是一帆风顺的，MySQL3.22时，仍然存在很多问题，如不支持事务操作、子查询、外键、存储过程和视图等功能。也正因为这些缺陷，当时许多Oracle和SQL Server的用户对MySQL根本不屑一顾。

时间再次推到了1999的冬天，瑞典的中部城市Uppsala下了很大一场雪。但著名的商业公司MySQL AB就在该城市注册成立了。并于同年发布了包含事务型存储引擎BDB的MySQL 3.23。

在集成BDB存储引擎的过程中，MySQL开发团队得到了很好的锻炼，为后来能将InnoDB整合以及开发开放插件式的存储引擎架构打下了坚实的基础。

在MySQL从诞生之初，就制定了双重的授权标准：

- 个人使用是免费的。
- 如果用于商业网站搭建或者Windows平台下就必须购买商业许可证。

但在2000年的时，MySQL AB公司做了一个重大的决定，决定将MySQL以GPL许可证而开源了，也就是说商业用户也无需再购买授权许可证，但必须把他们的源码公开。

虽然MySQL AB在短期上，经济上遭受了巨大的打击，损失了将近80%的收入，但他们依然坚持GPL许可模式。

同一时间，芬兰公司Heikki开始接触MySQL AB，讨论将Heikki的存储引擎InnoDB整合到MySQL数据库中的可行性。

可喜的是，双方的合作非常顺利，并于2001年推出MySQL 4.0 Alpha版本。

经过两年的公开测试和应用，到了2003年，包含InnoDB的MySQL已经变得非常稳定了。随即在同一年，MySQL推出4.1版，第一次使得MySQL支持子查询，支持Unicode和预编译SQL等功能

直到MySQL5.0的发布，这个版本是有史以来MySQL最大的变化，添加了存储过程、服务端游标、触发器、查询优化以及分布式事务等在大家在现在看来一个"正常数据库管理系统"应当拥有的一整套功能。

**收购、收购**

2008年2月，当时的业界开源老大Sun公司动用10亿美元收购了MySQL，造就了开源软件的收购最高价。
这次交易给开源交易设立了一个新的基准。在此之前的交易金额(JBoss、Zimbra、XenSource、Gluecode)从没接近过10亿美元，全部加起来才差不多与Sun Microsystems购买MySQL的花费持平。

MySQL被收购之后，MySQL图标停止使用，取而代之的是Sun/MySQL图标。

MySQL和Sun合并之后，推出了MySQL 5.1GA版和MySQL 5.4 Beta版。5.4的推出照搬了4.1和5.0当时的开发模式，让5.4和6.0并行处于Beta开发阶段。

螳螂捕蝉，黄雀在后。
2009年，数据库老大Oracle大笔一挥，开出74亿美元的支票，将Sun Microsystems和MySQL通盘收于旗下。

**MySQL大事件**

- 1999年，MySQL AB在瑞典正式宣布成立。
- 2000年，ISAM华丽转身MyISAM存储引擎。同年MySQL开放了自己的源代码，并且基于GPL许可协议。同年9月innoDB推出。
- 2003年，MySQL4.0发布,正式集成innodb
- 2005年，MySQL 5.0发布。同年Oracle把InnoDB引擎的开发公司innobase收购完成。MySQL明确地表现出迈向高性能数据库的发展步伐。
- 2006年，sun公司收购了MySQL公司，出价10亿美元。
- 2009年，Oracle公司收购sun，将MySQL纳入囊中。
- 2010年，MySQL 5.5正式版发布，Oracle完成了大量改进，并将innodb改成默认引擎。
- 2013年，MySQL 5.6 GA版本发布。
- 2015年 - MySQL 5.7 GA版本横空出世，其性能、新特性、性能分析带来了质的改变。


来源： https://www.jianshu.com/p/9bb6e5690521













