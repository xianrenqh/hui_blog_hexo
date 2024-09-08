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

## <font style="color:rgb(33, 37, 41);">前言</font>

<font style="color:rgb(33, 37, 41);">Git 和 SVN 都是版本管理系统，但是他们  
</font><font style="color:rgb(33, 37, 41);">命令区别后面会简单进行一个对比，我们先从原理的角度分析</font>

## <font style="color:rgb(33, 37, 41);">4.git 和 svn 命令</font>

<font style="color:rgb(33, 37, 41);">先来复习哈命令</font>

| **<font style="color:rgb(33, 37, 41);">作用</font>**       | **<font style="color:rgb(33, 37, 41);">git</font>**                                   | **<font style="color:rgb(33, 37, 41);">svn</font>**             |
| ---------------------------------------------------------- | ------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| <font style="color:rgb(33, 37, 41);">版本库初始化</font>   | <font style="color:rgb(33, 37, 41);">git init</font>                                  | <font style="color:rgb(33, 37, 41);">svn create</font>          |
| <font style="color:rgb(33, 37, 41);">clone</font>          | <font style="color:rgb(33, 37, 41);">git clone</font>                                 | <font style="color:rgb(33, 37, 41);">svn co（checkout）</font>  |
| <font style="color:rgb(33, 37, 41);">add</font>            | <font style="color:rgb(33, 37, 41);">git add （.除去.gitignore，\*所有的文件）</font> | <font style="color:rgb(33, 37, 41);">svn add</font>             |
| <font style="color:rgb(33, 37, 41);">commit</font>         | <font style="color:rgb(33, 37, 41);">git commit</font>                                | <font style="color:rgb(33, 37, 41);">svn commit</font>          |
| <font style="color:rgb(33, 37, 41);">pull</font>           | <font style="color:rgb(33, 37, 41);">git pull</font>                                  | <font style="color:rgb(33, 37, 41);">svn update</font>          |
| <font style="color:rgb(33, 37, 41);">push</font>           | <font style="color:rgb(33, 37, 41);">git push</font>                                  | <font style="color:rgb(33, 37, 41);">-</font>                   |
| <font style="color:rgb(33, 37, 41);">查看工作状态</font>   | <font style="color:rgb(33, 37, 41);">git status</font>                                | <font style="color:rgb(33, 37, 41);">svn status</font>          |
| <font style="color:rgb(33, 37, 41);">创建分支</font>       | <font style="color:rgb(33, 37, 41);">git branch <分支名></font>                       | <font style="color:rgb(33, 37, 41);">svn cp <分支名></font>     |
| <font style="color:rgb(33, 37, 41);">删除分支</font>       | <font style="color:rgb(33, 37, 41);">git branch -d <分支名></font>                    | <font style="color:rgb(33, 37, 41);">svn rm <分支名></font>     |
| <font style="color:rgb(33, 37, 41);">分支合并</font>       | <font style="color:rgb(33, 37, 41);">git merge <分支名></font>                        | <font style="color:rgb(33, 37, 41);">svn merge <分支名></font>  |
| <font style="color:rgb(33, 37, 41);">工作区差异</font>     | <font style="color:rgb(33, 37, 41);">git differ （-cached / head）</font>             | <font style="color:rgb(33, 37, 41);">svn diff</font>            |
| <font style="color:rgb(33, 37, 41);">更新至历史版本</font> | <font style="color:rgb(33, 37, 41);">git checkout <commit></font>                     | <font style="color:rgb(33, 37, 41);">svn update -r <rev></font> |
| <font style="color:rgb(33, 37, 41);">切换 tag</font>       | <font style="color:rgb(33, 37, 41);">git checkout <tag></font>                        | <font style="color:rgb(33, 37, 41);">svn switch <tag></font>    |
| <font style="color:rgb(33, 37, 41);">切换分支</font>       | <font style="color:rgb(33, 37, 41);">git checkout branch</font>                       | <font style="color:rgb(33, 37, 41);">svn switch branch</font>   |
| <font style="color:rgb(33, 37, 41);">还原文件</font>       | <font style="color:rgb(33, 37, 41);">git checkout - path</font>                       | <font style="color:rgb(33, 37, 41);">svn revert path</font>     |
| <font style="color:rgb(33, 37, 41);">删除文件</font>       | <font style="color:rgb(33, 37, 41);">git rm path</font>                               | <font style="color:rgb(33, 37, 41);">svn rm path</font>         |
| <font style="color:rgb(33, 37, 41);">移动文件</font>       | <font style="color:rgb(33, 37, 41);">git mv path</font>                               | <font style="color:rgb(33, 37, 41);">git mv path</font>         |
| <font style="color:rgb(33, 37, 41);">清除未追踪文件</font> | <font style="color:rgb(33, 37, 41);">git clean</font>                                 | <font style="color:rgb(33, 37, 41);">svn status sed -e</font>   |

## <font style="color:rgb(33, 37, 41);">1.存贮区别</font>

<font style="color:rgb(33, 37, 41);">大家想想为什么我们代码管理为什么一般用 git，原型图和高保真管理一般用 SVN?</font>

<font style="color:rgb(33, 37, 41);">1.git 是分布式的，有本地和远程两个版本库，SVN 是集中式，只有一个远程版本库；  
</font><font style="color:rgb(33, 37, 41);">2.git 的内容是按元数据方式存贮，所有控制文件在.git 中，svn 是按文件处理，所有资源控制文件在.svn 中；  
</font><font style="color:rgb(33, 37, 41);">3.svn 的分支是一个目录，git 不是；  
</font><font style="color:rgb(33, 37, 41);">4.git 没有一个全局的版本号，svn 有；  
</font><font style="color:rgb(33, 37, 41);">5.git 内容存贮是使用 SHA-1 哈希算法，能确保代码完整性;  
</font><font style="color:rgb(33, 37, 41);">6.git 有工作区，暂存区，远程仓库，git add 将代码提交到暂存区， commit 提交到本地版本库，push 推送到远程版本库。svn 是 add 提交到暂存，commit 是提交到远程版本库。</font>

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1650362992187-7a03197b-6c8e-4a6e-a3c5-e1f3c71e5423.png)

<font style="color:rgb(33, 37, 41);">所以可以很清楚的看出因为原型图和高保真都是以单个文件为单位，所以适合用 SVN 管理，而我们代码时以行数为单位，适合 Git</font>

## <font style="color:rgb(33, 37, 41);">2.文件.svn 和.git 区别</font>

<font style="color:rgb(33, 37, 41);">1..svn 目录</font>

<font style="color:rgb(33, 37, 41);">随便打开一个.svn 的目录可以看到结构：  
</font><font style="color:rgb(33, 37, 41);">如果无法查看.svn，window 电脑-点击查看-勾选隐藏文件；  
</font><font style="color:rgb(33, 37, 41);">mac 直接 shift + command + .</font>

```git
├── pristine 各个版本纪录，这个文件一般较大
├── tmp
├── entries 当前版本号
├── format 文本文件， 放了一个整数，当前版本号
├── wc.db 二进制文件
├── wc.db-journal 二进制文件
```

<font style="color:rgb(33, 37, 41);">2..git 目录结构</font>

<font style="color:rgb(33, 37, 41);">你可能对这些目录结构很陌生，没关系，直接在终端输入 git help gitrepository-layout 回车，你会发现浏览器会打开一个 html 文件，实际上就会打开安装 git 下面的一个 html 文档</font>

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

<font style="color:rgb(33, 37, 41);">所以可以看出 git 在处理代码方面功能比 svn 要强大一些</font>

## <font style="color:rgb(33, 37, 41);">3..git 文件动态分析</font>

### <font style="color:rgb(33, 37, 41);">3.1 add 阶段</font>

<font style="color:rgb(33, 37, 41);">1.执行 git init 会生成一个初始化的.git,会发现上面有些目录文件没有，因为有些文件是指定的命令后才会生成</font>

<font style="color:rgb(33, 37, 41);">2.新建一个 test.txt,随便写点内容，执行 git status</font>

```git
On branch master  // 默认一个master 分支

No commits yet

Untracked files: // 未提交的文件
  (use "git add <file>..." to include in what will be committed)

    test.txt

nothing added to commit but untracked files present (use "git add" to track)
```

<font style="color:rgb(33, 37, 41);">运行 find . -type f</font>

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

<font style="color:rgb(33, 37, 41);">3.执行 git add text.txt,显示</font>

```git
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

    new file:   test.txt
```

<font style="color:rgb(33, 37, 41);">运行 find . -type f</font>

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

<font style="color:rgb(33, 37, 41);">4.总结：可以看出 git add 后 test.txt 被标记为 staged 状态，而且 object 多了一个 61/de0edff 文件，所以 object 可以存贮 git 仓库内容，以二进制方式存贮。</font>

<font style="color:rgb(33, 37, 41);">5.我们可以查看下文件来源</font>

```git
git cat-file -p 61de0edf
打印 test
```

<font style="color:rgb(33, 37, 41);">6.git 如何管理和归档文件  
</font><font style="color:rgb(33, 37, 41);">我们常见的文件系统(NTFS、FAT、FAT32)是基于地址方式检索文件，即先给具体的地址，然后从地址编号对应的存储单元读取文件内容，而 git 是基于内容检索，是对整个内容检索，得到一个真实的存储位置，类似哈希映射。</font>

### <font style="color:rgb(33, 37, 41);">3.2 commit 阶段</font>

<font style="color:rgb(33, 37, 41);">1.执行 git commit -m 'add test'</font>

```git
1 file changed, 1 insertion(+)
create mode 100644 test.txt
```

<font style="color:rgb(33, 37, 41);">2.运行 find . -type f</font>

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

<font style="color:rgb(33, 37, 41);">可以看出 commit 后在 add 的基础上 object 多了两个文件 ed/fd7e90 和 26/ef8e8，从文件的归档路径和命名可以看出 git 使用 SHA-1 算法对文件内容进行了校验</font>

<font style="color:rgb(33, 37, 41);">还多了一个 COMMIT_EDITMSG ，里面是上一次提交的注释信息</font>

<font style="color:rgb(33, 37, 41);">3.使用 git cat-file 查看来源</font>

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

<font style="color:rgb(33, 37, 41);">ed/fd7e90 是一个 commit 对象，tree 属性指向了 26/ef8e8，纪录了文件操作，作者，提交者信息；  
</font><font style="color:rgb(33, 37, 41);">26/ef8e8 是一个 tree 对象，blob 属性指向了 blob 对象 61/de0edf，纪录了文件名；  
</font><font style="color:rgb(33, 37, 41);">61/de0edf 是一个 blob 对象，纪录了文件内容。</font>

<font style="color:rgb(33, 37, 41);">三个文件关系：  
</font>![](https://cdn.nlark.com/yuque/0/2022/jpeg/27022430/1650362992194-a4905996-c53d-4ff4-a58f-9052a6f76764.jpeg)<font style="color:rgb(33, 37, 41);">  
</font><font style="color:rgb(33, 37, 41);">所以现在知道为什么 object 文件会很大的吧</font>

### <font style="color:rgb(33, 37, 41);">3.3 branch</font>

<font style="color:rgb(33, 37, 41);">git branch 获取分支列表  
</font><font style="color:rgb(33, 37, 41);">列表保存到 refs/heads/master 下面</font>

### <font style="color:rgb(33, 37, 41);">3.4 git 对象模型</font>

<font style="color:rgb(33, 37, 41);">通过上面 3.2 的分析知道，在 git 系统中有四种尅性的对象：  
</font><font style="color:rgb(33, 37, 41);">1.commit：指向一个 tree，纪录了文件操作，作者，提交者信息；  
</font><font style="color:rgb(33, 37, 41);">2.tree：对象关系树，管理 tree 和 blob 的关系；  
</font><font style="color:rgb(33, 37, 41);">3.blob：保存文件内容；  
</font><font style="color:rgb(33, 37, 41);">4.tag：标记提交。</font>

### <font style="color:rgb(33, 37, 41);">3.5 git 生命周期钩子</font>

<font style="color:rgb(33, 37, 41);">1.钩子初始化：  
</font><font style="color:rgb(33, 37, 41);">上面说到的 hooks 下面都是生命周期脚本，初始化仓库（git init）或 git clone 都会初始化.git 文件；</font>

<font style="color:rgb(33, 37, 41);">2.钩子是本地的，因为不会提交到代码仓库，只不过 clone 的时候会初始化；</font>

<font style="color:rgb(33, 37, 41);">3.钩子分类：</font>

| **<font style="color:rgb(33, 37, 41);">钩子名</font>**         | **<font style="color:rgb(33, 37, 41);">作用</font>**                                                                                                 |
| -------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| <font style="color:rgb(33, 37, 41);">pre-commit</font>         | <font style="color:rgb(33, 37, 41);">每次 git commit 之前会触发，很常见的应用就是在 package.json 结合 husky 和 lint-staged 做代码 eslint 校验</font> |
| <font style="color:rgb(33, 37, 41);">prepare-commit-msg</font> | <font style="color:rgb(33, 37, 41);">在 pre-commit 在文本编辑器生成提交信息被调用，方便的修改自动生成的 squash 和 merage 提交</font>                 |
| <font style="color:rgb(33, 37, 41);">commit-msg</font>         | <font style="color:rgb(33, 37, 41);">用户输入提交信息被调用，就是 commit -m 后面那个提交信息，可以用来规范提交信息</font>                            |
| <font style="color:rgb(33, 37, 41);">post-commit</font>        | <font style="color:rgb(33, 37, 41);">commit-msg 后执行，通知 git commit 的结果</font>                                                                |
| <font style="color:rgb(33, 37, 41);">post-checkout</font>      | <font style="color:rgb(33, 37, 41);">git checkout 被调用</font>                                                                                      |
| <font style="color:rgb(33, 37, 41);">pre-rebase</font>         | <font style="color:rgb(33, 37, 41);">git rebase 更改之前运行</font>                                                                                  |
| <font style="color:rgb(33, 37, 41);">pre-receive</font>        | <font style="color:rgb(33, 37, 41);">git push 后执行，存在于远程仓库中，服务端远程钩子</font>                                                        |
| <font style="color:rgb(33, 37, 41);">update</font>             | <font style="color:rgb(33, 37, 41);">pre-receive 后调用</font>                                                                                       |
| <font style="color:rgb(33, 37, 41);">post-receive</font>       | <font style="color:rgb(33, 37, 41);">push 推送成功后被调用，通知 push 的用户</font>                                                                  |

## <font style="color:rgb(33, 37, 41);">结语</font>

<font style="color:rgb(33, 37, 41);">看到这里 git 和 svn 很多迷惑都解开了吧， 原创码字不易，欢迎 star！</font>
