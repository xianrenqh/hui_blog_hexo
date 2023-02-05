---
title: 一篇搞懂Git 和 SVN 的区别【原理篇】
urlname: leb0vw
date: '2022-04-19 10:05:51 +0000'
tags:
  - git
  - svn
categories: 学无止境
copyright_author_href: 'https://segmentfault.com/u/huolang_5a14e07d2f2ff'
copyright_url: 'https://segmentfault.com/a/1190000039978493'
copyright_author: 火狼
---

## 前言

Git 和 SVN 都是版本管理系统，但是他们
命令区别后面会简单进行一个对比，我们先从原理的角度分析

## 4.git 和 svn 命令

先来复习哈命令

| **作用**       | **git**                                   | **svn**             |
| -------------- | ----------------------------------------- | ------------------- |
| 版本库初始化   | git init                                  | svn create          |
| clone          | git clone                                 | svn co（checkout）  |
| add            | git add （.除去.gitignore，\*所有的文件） | svn add             |
| commit         | git commit                                | svn commit          |
| pull           | git pull                                  | svn update          |
| push           | git push                                  | -                   |
| 查看工作状态   | git status                                | svn status          |
| 创建分支       | git branch <分支名>                       | svn cp <分支名>     |
| 删除分支       | git branch -d <分支名>                    | svn rm <分支名>     |
| 分支合并       | git merge <分支名>                        | svn merge <分支名>  |
| 工作区差异     | git differ （-cached / head）             | svn diff            |
| 更新至历史版本 | git checkout <commit>                     | svn update -r <rev> |
| 切换 tag       | git checkout <tag>                        | svn switch <tag>    |
| 切换分支       | git checkout branch                       | svn switch branch   |
| 还原文件       | git checkout - path                       | svn revert path     |
| 删除文件       | git rm path                               | svn rm path         |
| 移动文件       | git mv path                               | git mv path         |
| 清除未追踪文件 | git clean                                 | svn status sed -e   |

## 1.存贮区别

大家想想为什么我们代码管理为什么一般用 git，原型图和高保真管理一般用 SVN?
1.git 是分布式的，有本地和远程两个版本库，SVN 是集中式，只有一个远程版本库；
2.git 的内容是按元数据方式存贮，所有控制文件在.git 中，svn 是按文件处理，所有资源控制文件在.svn 中；
3.svn 的分支是一个目录，git 不是；
4.git 没有一个全局的版本号，svn 有；
5.git 内容存贮是使用 SHA-1 哈希算法，能确保代码完整性;
6.git 有工作区，暂存区，远程仓库，git add 将代码提交到暂存区， commit 提交到本地版本库，push 推送到远程版本库。svn 是 add 提交到暂存，commit 是提交到远程版本库。
![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1650362992187-7a03197b-6c8e-4a6e-a3c5-e1f3c71e5423.png#clientId=u628dacaa-ae7d-4&from=paste&id=u9bf41b5f&originHeight=261&originWidth=732&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=u16605eef-3685-427f-9af3-30ce347df4a&title=)
所以可以很清楚的看出因为原型图和高保真都是以单个文件为单位，所以适合用 SVN 管理，而我们代码时以行数为单位，适合 Git

## 2.文件.svn 和.git 区别

1..svn 目录
随便打开一个.svn 的目录可以看到结构：
如果无法查看.svn，window 电脑-点击查看-勾选隐藏文件；
mac 直接 shift + command + .

```git
├── pristine 各个版本纪录，这个文件一般较大
├── tmp
├── entries 当前版本号
├── format 文本文件， 放了一个整数，当前版本号
├── wc.db 二进制文件
├── wc.db-journal 二进制文件
```

2..git 目录结构
你可能对这些目录结构很陌生，没关系，直接在终端输入 git help gitrepository-layout 回车，你会发现浏览器会打开一个 html 文件，实际上就会打开安装 git 下面的一个 html 文档

```git
├── hooks 钩子文件
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── fsmonitor-watchman.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample commit时会触发这个钩子
│   ├── pre-push.sample push触发
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── prepare-commit-msg.sample
│   ├── update.sample update触发
├── info
│   ├── exclude 忽略的文件
├── object git数据对象，包括commit，trees，二进制对象，标签等
├── COMMIT_EDITMSG 上一次提交的注释信息
├── log 各个refs的历史信息
├── refs 每个分支指向那个提交
├── config 本项目配置信息，包括仓库地址，分支，用户账号等
├── description 项目描述
├── HEAD 当前分支的最后一次提交
├── index 索引文件，存贮git add把要添加的项
├── packed-refs 分支标识文件
```

所以可以看出 git 在处理代码方面功能比 svn 要强大一些

## 3..git 文件动态分析

### 3.1 add 阶段

1.执行 git init 会生成一个初始化的.git,会发现上面有些目录文件没有，因为有些文件是指定的命令后才会生成 2.新建一个 test.txt,随便写点内容，执行 git status

```git
On branch master  // 默认一个master 分支

No commits yet

Untracked files: // 未提交的文件
  (use "git add <file>..." to include in what will be committed)

    test.txt

nothing added to commit but untracked files present (use "git add" to track)
```

运行 find . -type f

```git
./config
./HEAD
./info/exclude
./description
./hooks/commit-msg.sample
./hooks/pre-rebase.sample
./hooks/pre-commit.sample
./hooks/applypatch-msg.sample
./hooks/fsmonitor-watchman.sample
./hooks/pre-receive.sample
./hooks/prepare-commit-msg.sample
./hooks/post-update.sample
./hooks/pre-applypatch.sample
./hooks/pre-push.sample
./hooks/update.sample
./index
```

3.执行 git add text.txt,显示

```git
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

    new file:   test.txt
```

运行 find . -type f

```git
./config
./objects/61/de0edff4ebeeff225da34006cbe6427638fadc  # 比之前多了一个文件
./HEAD
./info/exclude
./description
./hooks/commit-msg.sample
./hooks/pre-rebase.sample
./hooks/pre-commit.sample
./hooks/applypatch-msg.sample
./hooks/fsmonitor-watchman.sample
./hooks/pre-receive.sample
./hooks/prepare-commit-msg.sample
./hooks/post-update.sample
./hooks/pre-applypatch.sample
./hooks/pre-push.sample
./hooks/update.sample
./index
```

4.总结：可以看出 git add 后 test.txt 被标记为 staged 状态，而且 object 多了一个 61/de0edff 文件，所以 object 可以存贮 git 仓库内容，以二进制方式存贮。 5.我们可以查看下文件来源

```git
git cat-file -p 61de0edf
打印 test
```

6.git 如何管理和归档文件
我们常见的文件系统(NTFS、FAT、FAT32)是基于地址方式检索文件，即先给具体的地址，然后从地址编号对应的存储单元读取文件内容，而 git 是基于内容检索，是对整个内容检索，得到一个真实的存储位置，类似哈希映射。

### 3.2 commit 阶段

1.执行 git commit -m 'add test'

```git
1 file changed, 1 insertion(+)
create mode 100644 test.txt
```

2.运行 find . -type f

```git
./test.txt
./.git/config
./.git/objects/61/de0edff4ebeeff225da34006cbe6427638fadc
./.git/objects/ed/fd7e903f8f622f9a52542adfa077552608202d
./.git/objects/26/ef8e81bc27b4a67f251145a4f83782364fa9fa
./.git/HEAD
./.git/info/exclude
./.git/logs/HEAD
./.git/logs/refs/heads/master
./.git/description
./.git/hooks/commit-msg.sample
./.git/hooks/pre-rebase.sample
./.git/hooks/pre-commit.sample
./.git/hooks/applypatch-msg.sample
./.git/hooks/fsmonitor-watchman.sample
./.git/hooks/pre-receive.sample
./.git/hooks/prepare-commit-msg.sample
./.git/hooks/post-update.sample
./.git/hooks/pre-applypatch.sample
./.git/hooks/pre-push.sample
./.git/hooks/update.sample
./.git/refs/heads/master
./.git/index
./.git/COMMIT_EDITMSG
```

可以看出 commit 后在 add 的基础上 object 多了两个文件 ed/fd7e90 和 26/ef8e8，从文件的归档路径和命名可以看出 git 使用 SHA-1 算法对文件内容进行了校验
还多了一个 COMMIT_EDITMSG ，里面是上一次提交的注释信息 3.使用 git cat-file 查看来源

```git
git cat-file -t edfd7e90
// 终端输出tree

git cat-file -t 26ef8e8
// 终端输出commit

git cat-file -p edfd7e90
// 终端输出 100644 blob 61de0edff4ebeeff225da34006cbe6427638fadc    test.txt

git cat-file -p 26ef8e8
// 终端输出 tree edfd7e903f8f622f9a52542adfa077552608202d
author 信息  1612668900 +0800
committer author 信息  1612668900 +0800
```

ed/fd7e90 是一个 commit 对象，tree 属性指向了 26/ef8e8，纪录了文件操作，作者，提交者信息；
26/ef8e8 是一个 tree 对象，blob 属性指向了 blob 对象 61/de0edf，纪录了文件名；
61/de0edf 是一个 blob 对象，纪录了文件内容。
三个文件关系：
![](https://cdn.nlark.com/yuque/0/2022/jpeg/27022430/1650362992194-a4905996-c53d-4ff4-a58f-9052a6f76764.jpeg#clientId=u628dacaa-ae7d-4&from=paste&id=uba2d9c63&originHeight=142&originWidth=732&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=u9a9a7836-6925-41ce-8dd0-adfa2f12349&title=)
所以现在知道为什么 object 文件会很大的吧

### 3.3 branch

git branch 获取分支列表
列表保存到 refs/heads/master 下面

### 3.4 git 对象模型

通过上面 3.2 的分析知道，在 git 系统中有四种尅性的对象：
1.commit：指向一个 tree，纪录了文件操作，作者，提交者信息；
2.tree：对象关系树，管理 tree 和 blob 的关系；
3.blob：保存文件内容；
4.tag：标记提交。

### 3.5 git 生命周期钩子

1.钩子初始化：
上面说到的 hooks 下面都是生命周期脚本，初始化仓库（git init）或 git clone 都会初始化.git 文件； 2.钩子是本地的，因为不会提交到代码仓库，只不过 clone 的时候会初始化； 3.钩子分类：

| **钩子名**         | **作用**                                                                                                 |
| ------------------ | -------------------------------------------------------------------------------------------------------- |
| pre-commit         | 每次 git commit 之前会触发，很常见的应用就是在 package.json 结合 husky 和 lint-staged 做代码 eslint 校验 |
| prepare-commit-msg | 在 pre-commit 在文本编辑器生成提交信息被调用，方便的修改自动生成的 squash 和 merage 提交                 |
| commit-msg         | 用户输入提交信息被调用，就是 commit -m 后面那个提交信息，可以用来规范提交信息                            |
| post-commit        | commit-msg 后执行，通知 git commit 的结果                                                                |
| post-checkout      | git checkout 被调用                                                                                      |
| pre-rebase         | git rebase 更改之前运行                                                                                  |
| pre-receive        | git push 后执行，存在于远程仓库中，服务端远程钩子                                                        |
| update             | pre-receive 后调用                                                                                       |
| post-receive       | push 推送成功后被调用，通知 push 的用户                                                                  |

## 结语

看到这里 git 和 svn 很多迷惑都解开了吧， 原创码字不易，欢迎 star！
