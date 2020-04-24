---
layout: post
title: Docker命令-pull 
description: "Docker pull 命令"
modified: 2014-12-24
tags: [Docker]

---

### 获取镜像

Docker Hub 上有大量的高质量的镜像可以用，这里我们就说一下怎么获取这些镜像。
从 Docker 镜像仓库获取镜像的命令是 docker pull。其命令格式为：
```
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
```
具体的选项可以通过 docker pull --help 命令看到
![](http://www.foyiwo.com/server/../Public/Uploads/2019-04-30/5cc85c995d8cb.png)

这里我们说一下镜像名称的格式。
- Docker 镜像仓库地址：地址的格式一般是 <域名/IP>[:端口号]。默认地址是 Docker Hub。
- 仓库名：如之前所说，这里的仓库名是两段式名称，即 <用户名>/<软件名>。对于 Docker Hub，如果不给出用户名，则默认为 library，也就是官方镜像。

##### 比如：
```
docker pull mattrayner/lamp
```

![](http://www.foyiwo.com/server/../Public/Uploads/2019-04-30/5cc86227c3589.png)

上面的命令中没有给出 Docker 镜像仓库地址，因此将会从 Docker Hub 获取镜像。而镜像名称是 mattrayner/lamp

从下载过程中可以看到我们之前提及的分层存储的概念，镜像是由多层存储所构成。下载也是一层层的去下载，并非单一文件。下载过程中给出了每一层的 ID 的前 12 位。并且下载结束后，给出该镜像完整的 sha256 的摘要，以确保下载一致性。

在使用上面命令的时候，你可能会发现，你所看到的层 ID 以及 sha256 的摘要和这里的不一样。这是因为官方镜像是一直在维护的，有任何新的 bug，或者版本更新，都会进行修复再以原来的标签发布，这样可以确保任何使用这个标签的用户可以获得更安全、更稳定的镜像。

如果从 Docker Hub 下载镜像非常缓慢，可以参照 [镜像加速器](https://yeasy.gitbooks.io/docker_practice/install/mirror.html "镜像加速器") 一节配置加速器。









