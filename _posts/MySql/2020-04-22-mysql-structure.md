---
layout: post
title: MySQL · 整体概述
description: ""
modified: 2014-12-24
tags: [高级MySql]

---
### MySQL架构
MySQL相比于常规数据库,他在架构上显得如此与众不同，得益于该架构，MySQL可以在许多种场景中应用并发挥较好的作用。

也正是由于该架构，MySQL即可被运用到高要求的环境(如:Web类应用)，也可以被嵌入到应用程序中，同时还可以支持数据仓库、内容索引和部署软件、高可用的冗余系统、在线事务处理系统等各种应用类型。

MySQL最重要也是最与众不同的是他的**存储引擎架构**，该架构把查询处理及其他系统任务和数据的存储/提取相分离。

因此，在配置MySQL时，可以根据所支持系统的特性和要求的性能、以及综合其他需求来选择MySQL的存储引擎。

<p style="text-align: center;"><img src="https://img-blog.csdn.net/20150730152113099?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center">
<div style="text-align: center;">MySQL架构图</div>
</p>

上面架构图表现的是如此的清晰，一层套一层，环环相扣，下层为上层提供服务，而它的最外层则是：**Connectors层**

MySQL封装的是如此的好，导致我们绝大多数的开发者都只需面对：**Connectors层**，典型使用场景如下：

1. 新建一个Spring项目
2. pom.xml文件里引入MySQL的依赖
3. 配置文件里配置一下MySQL的链接和账号密码
4. 写SQL

是的，在这整一套过程中，我们仅仅在与**Connectors层**交互，剩下的都是MySQL自动帮你完成了












