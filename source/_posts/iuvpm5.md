---
title: Git基础知识
urlname: iuvpm5
date: '2022-04-14 01:46:47 +0000'
tags:
  - git
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
cover: >-
  https://cdn.nlark.com/yuque/0/2022/png/27022430/1649900825959-5cd5db54-330c-4de3-9808-ca353657f745.png#clientId=ueab694e8-573a-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=uc0c2e85d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=600&originWidth=900&originalType=url%E2%88%B6=1&rotation=0&showTitle=false&size=86685&status=done&style=none&taskId=ufbff039f-63f5-41a3-9b92-38d4e0c7f8b&title=
---

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649900825959-5cd5db54-330c-4de3-9808-ca353657f745.png#clientId=ueab694e8-573a-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=uc0c2e85d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=600&originWidth=900&originalType=url∶=1&rotation=0&showTitle=false&size=86685&status=done&style=none&taskId=ufbff039f-63f5-41a3-9b92-38d4e0c7f8b&title=)

# 什么是 Git

从一般开发者的角度来看，Git 有以下功能：

1. 从服务器上克隆数据库（包括代码和版本信息）到单机上。
2. 在自己的机器上创建分支，修改代码。
3. 在单机上自己创建的分支上提交代码。
4. 在单机上合并分支。
5. 新建一个分支，把服务器上最新版的代码 fetch 下来，然后跟自己的主分支合并。
6. 生成补丁（patch），把补丁发送给主开发者。
7. 看主开发者的反馈，如果主开发者发现两个一般开发者之间有冲突（他们之间可以合作解决的冲突），就会要求他们先解决冲突，然后再由其中一个人提交。如果主开发者可以自己解决，或者没有冲突，就通过。
8. 一般开发者之间解决冲突的方法，开发者之间可以使用 pull 命令解决冲突，解决完冲突之后再向主开发者提交补丁。

从主开发者的角度（假设主开发者不用开发代码）看，Git 有以下功能：

1. 查看邮件或者通过其它方式查看一般开发者的提交状态。
2. 打上补丁，解决冲突（可以自己解决，也可以要求开发者之间解决以后再重新提交，如果是开源项目，还要决定哪些补丁有用，哪些不用）。
3. 向公共服务器提交结果，然后通知所有开发人员。

# Git 初入

如果是第一次使用 Git，你需要设置署名和邮箱：

```git
git config --global user.name "用户名"
git config --global user.email "电子邮箱"
```

将代码仓库 clone 到本地，其实就是将代码复制到你的机器里，并交由 Git 来管理：

```git
git clone git@github.com:someone/symfony-docs-chs.git
```

你可以修改复制到本地的代码了（symfony-docs-chs 项目里都是 rst 格式的文档）。当你觉得完成了一定的工作量，想做个阶段性的提交：
向这个本地的代码仓库添加当前目录的所有改动：

```git
git add .
```

或者只是添加某个文件：

```git
git add -p
```

我们可以输入

```git
git status
```

来看现在的状态，如下图是添加之前的：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649900978641-272da7c2-b01b-4f6a-b5a9-4070fb738057.png#clientId=ueab694e8-573a-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u26c9e243&margin=%5Bobject%20Object%5D&name=image.png&originHeight=470&originWidth=1082&originalType=url∶=1&rotation=0&showTitle=false&size=111634&status=done&style=none&taskId=ufa7ab167-5193-4489-84f7-d42e10ceda1&title=)
下面是添加之后 的
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649900992662-de020754-c112-42e2-a2e8-ceabaa234271.png#clientId=ueab694e8-573a-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u7c4df1b6&margin=%5Bobject%20Object%5D&name=image.png&originHeight=406&originWidth=1244&originalType=url∶=1&rotation=0&showTitle=false&size=94662&status=done&style=none&taskId=u8b110088-73b0-4ed7-8bcb-52b0ed37169&title=)

可以看到状态的变化是从黄色到绿色，即 unstage 到 add。

# GitHub 与 Git

> Git 是一个分布式的版本控制系统，最初由 Linus Torvalds 编写，用作 Linux 内核代码的管理。在推出后，Git 在其它项目中也取得了很大成功，尤其是在 Ruby 社区中。目前，包括 Rubinius、Merb 和 Bitcoin 在内的很多知名项目都使用了 Git。Git 同样可以被诸如 Capistrano 和 Vlad the Deployer 这样的部署工具所使用。

> GitHub 可以托管各种 git 库，并提供一个 web 界面，但与其它像 SourceForge 或 Google Code 这样的服务不同，GitHub 的独特卖点在于从另外一个项目进行分支的简易性。为一个项目贡献代码非常简单：首先点击项目站点的“fork”的按钮，然后将代码检出并将修改加入到刚才分出的代码库中，最后通过内建的“pull request”机制向项目负责人申请代码合并。已经有人将 GitHub 称为代码玩家的 MySpace。

# Git 提交信息及几种不同的规范

在团队协作中，使用版本管理工具 Git、SVN 几乎都是这个行业的标准。当我们提交代码的时候，需要编写提交信息（commit message）。
而提交信息的主要用途是：告诉这个项目的人，这次代码提交里做了些什么。如，我更新了 React Native Elements 的版本，那么它就可以是：[T] upgrade react native elements。对应的我修改的代码就是：package.json 和 yarn.lock 中的文件。一般来说，建议小步提交，即按自己的 Tasking 步骤来的提交，每一小步都有对应的提交信息。这样做的主要目的是：防止一次修改中，修改过多的文件，导致后期修改、维护、撤销等等困难。
而对于不同的团队来说，都会遵循一定的规范，本文主要会介绍以下几种写法：

- 工作写法
- 常规写法
- 开源库写法

# Git 工具推荐

## SourceTree：

SourceTree 方便用来查看一些项目，寻找其中的一些问题。
gitflow 分支合并、查看
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1649903156572-5d9828a7-61df-404e-8ae8-cb35c44c70f0.png#clientId=u2837663f-9cd5-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ue6bd8f05&margin=%5Bobject%20Object%5D&name=image.png&originHeight=681&originWidth=1023&originalType=url∶=1&rotation=0&showTitle=false&size=503264&status=done&style=none&taskId=u89ff8403-c63e-4fb3-9d0e-2ead9efe57a&title=)

来源：《GitHub 漫游指南》
[https://github.phodal.com/](https://github.phodal.com/)
