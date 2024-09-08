---
title: git命令行拉取代码-git入门
urlname: woprme
date: '2022-05-16 03:23:03 +0000'
tags:
  - git
  - 命令行
  - 入门
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
'<font style="color:rgb(38, 38, 38);">copyright_url': >-
  </font>github_<font style="color:rgb(77, 77, 77);">.com</font>_<font
  style="color:rgb(77, 77, 77);">/gafish/gafish.github</font>_<font
  style="color:rgb(77, 77, 77);">.com</font>_
'<font style="color:rgb(38, 38, 38);">copyright_author': '</font><font style="color:rgb(77, 77, 77);">gafish</font>'
---

## **<font style="color:rgb(79, 79, 79);">Git 简介</font>**

## <font style="color:rgb(79, 79, 79);">Git 是一种</font>分布式<font style="color:rgb(79, 79, 79);">版本控制系统，它可以不受网络连接的限制，</font>_<font style="color:rgb(79, 79, 79);">加上</font>_<font style="color:rgb(79, 79, 79);">其它众多优点，目前已经成为程序开发人员做项目版本管理时的首选，非开发人员也可以用 Git 来做自己的文档版本管理工具。</font>

_<font style="color:rgb(77, 77, 77);">2013</font>_<font style="color:rgb(77, 77, 77);">年，淘宝前端团队开始全面采用 Git 来做项目管理，我也是那个时候开始接触和使用，从一开始的零接触到现在的重度依赖，真是感叹 Git 的强大。</font>

<font style="color:rgb(77, 77, 77);">Git 的 api 很多，但其实平时项目中 90%的需求都只需要用到几个基本的功能即可，所以本文将从 实用主义 和 深入探索 2 个方面去谈谈如何在项目中使用 Git，一般来说，看完 实用主义 这一节就可以开始在项目中动手用。</font>

<font style="color:rgb(77, 77, 77);"></font>

### **<font style="color:rgb(79, 79, 79);">常用操作</font>**

<font style="color:rgb(77, 77, 77);">所谓实用主义，就是掌握了以下知识就可以玩转</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">，轻松应对 90%以上的需求。以下是实用主义型的 Git 命令列表，先大致看一下</font>

- <font style="color:rgb(51, 51, 51);">git clone</font>
- <font style="color:rgb(51, 51, 51);">git config</font>
- <font style="color:rgb(51, 51, 51);">git branch</font>
- <font style="color:rgb(51, 51, 51);">git checkout</font>
- <font style="color:rgb(51, 51, 51);">git status</font>
- <font style="color:rgb(51, 51, 51);">git add</font>
- <font style="color:rgb(51, 51, 51);">git commit</font>
- <font style="color:rgb(51, 51, 51);">git push</font>
- <font style="color:rgb(51, 51, 51);">git pull</font>
- <font style="color:rgb(51, 51, 51);">git log</font>
- <font style="color:rgb(51, 51, 51);">git tag</font>

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1652671686498-4e6505ed-9ce6-4102-8c33-40c9b3e80ff9.png)![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1652671686664-5fc180ee-3e8c-4b88-9462-ab42b30ad2cc.png)![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1652671686899-ade3969a-681e-4639-950b-a70514bfb16c.png)![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1652671686737-ab17c403-4dc4-4566-92cb-b183855f0497.png)![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1652671686998-830421b9-cb6e-4d43-bf5b-2903e5a03401.png)![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1652671687435-20cff5e6-38a9-47c6-855b-7fb6754210a1.png)![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1652671687671-da8a76e6-df6d-4cf3-9a51-218ac8ab1077.png)![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1652671688072-682c9f07-f473-4ee4-a827-e5a4f0c28643.png)![](https://cdn.nlark.com/yuque/0/2022/gif/27022430/1652671688031-09984018-a557-44bb-82aa-e2839cfd72ef.gif)![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1652671688362-d3137c8d-d944-43b5-bb0f-cfae811111ab.png)![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1652671688436-f5282664-beec-4ec5-b12d-84aaec2c25a5.png)

<font style="color:rgb(77, 77, 77);">接下来，将通过对：https://github</font>_<font style="color:rgb(77, 77, 77);">.com</font>_<font style="color:rgb(77, 77, 77);">/gafish/gafish.github</font>_<font style="color:rgb(77, 77, 77);">.com</font>_

<font style="color:rgb(77, 77, 77);">仓库进行实例操作，讲解如何使用</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">拉取代码到提交代码的整个流程。</font>

#### **<font style="color:rgb(79, 79, 79);">git clone</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">从 git 服务器拉取代码</font>

<font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(193, 132, 1);">clone</font><font style="color:rgb(51, 51, 51);"> https://github.com/gafish/gafish.github.com.git</font>

<font style="color:rgb(77, 77, 77);">代码</font>_<font style="color:rgb(77, 77, 77);">下载</font>_<font style="color:rgb(77, 77, 77);">完成后在当前文件夹中会有一个</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">gafish.github</font>_<font style="color:rgb(77, 77, 77);">.com</font>_<font style="color:rgb(77, 77, 77);">的目录，通过</font><font style="color:rgb(77, 77, 77);">cd gafish.github</font>_<font style="color:rgb(77, 77, 77);">.com</font>_<font style="color:rgb(77, 77, 77);">命令进入目录。</font>

#### **<font style="color:rgb(79, 79, 79);">git config</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">配置开发者用户名和邮箱</font>

<font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(193, 132, 1);">config</font><font style="color:rgb(51, 51, 51);"> user.name gafishgit </font><font style="color:rgb(193, 132, 1);">config</font><font style="color:rgb(51, 51, 51);"> user.email gafish@qqqq.com</font>

<font style="color:rgb(77, 77, 77);">每次代码提交的时候都会生成一条提交记录，其中会包含当前配置的用户名和邮箱。</font>

#### **<font style="color:rgb(79, 79, 79);">git branch</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">创建、重命名、查看、删除项目分支，通过</font><font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);"> </font><font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">Git</font><font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">做项目开发时，一般都是在开发分支中进行，开发完成后合并分支到主干。</font>

<font style="color:rgb(51, 51, 51);">git branch daily/0.0.0</font>

<font style="color:rgb(77, 77, 77);">创建一个名为</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">daily/0.0.0</font><font style="color:rgb(77, 77, 77);">的日常开发分支，分支名只要不包括特殊字符即可。</font>

<font style="color:rgb(51, 51, 51);">git branch -m daily</font><font style="color:rgb(80, 161, 79);">/0.0.0 daily/</font><font style="color:rgb(152, 104, 1);">0.0</font><font style="color:rgb(152, 104, 1);">.1</font>

<font style="color:rgb(77, 77, 77);">如果觉得之前的分支名不合适，可以为新建的分支重命名，重命名分支名为</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">daily/0.0.1</font>

<font style="color:rgb(51, 51, 51);">git branch</font>

<font style="color:rgb(77, 77, 77);">通过不带参数的 branch 命令可以查看当前项目分支列表</font>

<font style="color:rgb(51, 51, 51);">git branch -d daily/0.0.1</font>

<font style="color:rgb(77, 77, 77);">如果分支已经完成使命则可以通过</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">-d</font><font style="color:rgb(77, 77, 77);">参数将分支删除，这里为了继续下一步操作，暂不执行删除操作</font>

#### **<font style="color:rgb(79, 79, 79);">git checkout</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">切换分支</font>

<font style="color:rgb(51, 51, 51);">git checkout daily/0.0.1</font>

<font style="color:rgb(77, 77, 77);">切换到</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">daily/0.0.1</font><font style="color:rgb(77, 77, 77);">分支，后续的操作将在这个分支上进行</font>

#### **<font style="color:rgb(79, 79, 79);">git status</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">查看文件变动状态</font>

<font style="color:rgb(77, 77, 77);">通过任何你喜欢的编辑器对项目中的</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">README.md</font><font style="color:rgb(77, 77, 77);">文件做一些改动，保存。</font>

<font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(193, 132, 1);">status</font>

<font style="color:rgb(77, 77, 77);">通过</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">git status</font><font style="color:rgb(77, 77, 77);">命令可以看到文件当前状态</font><font style="color:rgb(77, 77, 77);">Changes not staged for commit:</font><font style="color:rgb(77, 77, 77);">(</font>_<font style="color:rgb(77, 77, 77);">改动文件未提交到暂存区</font>_<font style="color:rgb(77, 77, 77);">)</font>

```plain
On branch daily/0.0.1Changes not staged for commit: (use "git add ..." to update what will be committed) (use "git checkout -- ..." to discard changes in working directory) modified: README.mdno changes added to commit (use "git add" and/or "git commit -a")
```

#### **<font style="color:rgb(79, 79, 79);">git add</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">添加文件变动到暂存区</font>

> git add README.md

<font style="color:rgb(77, 77, 77);">通过指定文件名 README.md 可以将该文件添加到暂存区，如果想添加所有文件可用 git add .命令，这时候可通过 git status 看到文件当前状态 Changes to be committed:(</font>_<font style="color:rgb(77, 77, 77);">文件已提交到暂存区</font>_<font style="color:rgb(77, 77, 77);">)</font>

```plain
On branch daily/0.0.1Changes to be committed: (use "git reset HEAD ..." to unstage) modified: README.md
```

#### **<font style="color:rgb(79, 79, 79);">git commit</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">提交文件变动到版本库</font>

> <font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(166, 38, 164);">commit</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">-</font><font style="color:rgb(51, 51, 51);">m </font><font style="color:rgb(80, 161, 79);">'这里写提交原因'</font>

<font style="color:rgb(77, 77, 77);">通过</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">-m</font><font style="color:rgb(77, 77, 77);">参数可直接在命令行里输入提交描述文本</font>

#### **<font style="color:rgb(79, 79, 79);">git push</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">将本地的代码改动推送到服务器</font>

> <font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(166, 38, 164);">push</font><font style="color:rgb(51, 51, 51);"> origin daily/</font><font style="color:rgb(152, 104, 1);">0</font><font style="color:rgb(51, 51, 51);">.</font><font style="color:rgb(152, 104, 1);">0</font><font style="color:rgb(51, 51, 51);">.</font><font style="color:rgb(152, 104, 1);">1</font>

<font style="color:rgb(77, 77, 77);">origin</font><font style="color:rgb(77, 77, 77);">指代的是当前的 git 服务器地址，这行命令的意思是把</font><font style="color:rgb(77, 77, 77);">daily/0.0.1</font><font style="color:rgb(77, 77, 77);">分支推送到服务器，当看到命令行返回如下字符表示推送成功了。</font>

<font style="color:rgb(51, 51, 51);">Counting objects: </font><font style="color:rgb(152, 104, 1);">3</font><font style="color:rgb(51, 51, 51);">, done.Delta compression </font><font style="color:rgb(166, 38, 164);">using</font><font style="color:rgb(51, 51, 51);"> up </font><font style="color:rgb(166, 38, 164);">to</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(152, 104, 1);">8</font><font style="color:rgb(51, 51, 51);"> threads.Compressing objects: </font><font style="color:rgb(152, 104, 1);">100</font><font style="color:rgb(51, 51, 51);">%</font><font style="color:rgb(51, 51, 51);"> (</font><font style="color:rgb(152, 104, 1);">2</font><font style="color:rgb(51, 51, 51);">/</font><font style="color:rgb(152, 104, 1);">2</font><font style="color:rgb(51, 51, 51);">), done.Writing objects: </font><font style="color:rgb(152, 104, 1);">100</font><font style="color:rgb(51, 51, 51);">%</font><font style="color:rgb(51, 51, 51);"> (</font><font style="color:rgb(152, 104, 1);">3</font><font style="color:rgb(51, 51, 51);">/</font><font style="color:rgb(152, 104, 1);">3</font><font style="color:rgb(51, 51, 51);">), </font><font style="color:rgb(152, 104, 1);">267</font><font style="color:rgb(51, 51, 51);"> bytes </font><font style="color:rgb(51, 51, 51);">|</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(152, 104, 1);">0</font><font style="color:rgb(51, 51, 51);"> bytes</font><font style="color:rgb(51, 51, 51);">/</font><font style="color:rgb(51, 51, 51);">s, done.Total </font><font style="color:rgb(152, 104, 1);">3</font><font style="color:rgb(51, 51, 51);"> (delta </font><font style="color:rgb(152, 104, 1);">1</font><font style="color:rgb(51, 51, 51);">), reused </font><font style="color:rgb(152, 104, 1);">0</font><font style="color:rgb(51, 51, 51);"> (delta </font><font style="color:rgb(152, 104, 1);">0</font><font style="color:rgb(51, 51, 51);">)remote: Resolving deltas: </font><font style="color:rgb(152, 104, 1);">100</font><font style="color:rgb(51, 51, 51);">%</font><font style="color:rgb(51, 51, 51);"> (</font><font style="color:rgb(152, 104, 1);">1</font><font style="color:rgb(51, 51, 51);">/</font><font style="color:rgb(152, 104, 1);">1</font><font style="color:rgb(51, 51, 51);">), completed </font><font style="color:rgb(166, 38, 164);">with</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(152, 104, 1);">1</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(166, 38, 164);">local</font><font style="color:rgb(51, 51, 51);"> objects.To https:</font><font style="color:rgb(51, 51, 51);">/</font><font style="color:rgb(51, 51, 51);">/</font><font style="color:rgb(51, 51, 51);">github.com</font><font style="color:rgb(51, 51, 51);">/</font><font style="color:rgb(51, 51, 51);">gafish</font><font style="color:rgb(51, 51, 51);">/</font><font style="color:rgb(51, 51, 51);">gafish.github.com.git </font><font style="color:rgb(51, 51, 51);">\*</font><font style="color:rgb(51, 51, 51);"> [</font><font style="color:rgb(166, 38, 164);">new</font><font style="color:rgb(51, 51, 51);"> branch] daily</font><font style="color:rgb(51, 51, 51);">/</font><font style="color:rgb(152, 104, 1);">0.0</font><font style="color:rgb(152, 104, 1);">.1</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">-</font><font style="color:rgb(51, 51, 51);">></font><font style="color:rgb(51, 51, 51);"> daily</font><font style="color:rgb(51, 51, 51);">/</font><font style="color:rgb(152, 104, 1);">0.0</font><font style="color:rgb(152, 104, 1);">.1</font>

<font style="color:rgb(77, 77, 77);">现在我们回到 Github 网站的项目首页，点击</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">Branch:master</font><font style="color:rgb(77, 77, 77);">下拉按钮，就会看到刚才推送的</font><font style="color:rgb(77, 77, 77);">daily/00.1</font><font style="color:rgb(77, 77, 77);">分支了</font>

#### **<font style="color:rgb(79, 79, 79);">git pull</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">将服务器上的最新代码拉取到本地</font>

> <font style="color:rgb(51, 51, 51);">git pull origin daily/0.0.1</font>

<font style="color:rgb(77, 77, 77);">如果其它项目成员对项目做了改动并推送到服务器，我们需要将最新的改动更新到本地，这里我们来模拟一下这种情况。</font>

<font style="color:rgb(77, 77, 77);">进入 Github 网站的项目首页，再进入</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">daily/0.0.1</font><font style="color:rgb(77, 77, 77);">分支，在线对</font><font style="color:rgb(77, 77, 77);">README.md</font><font style="color:rgb(77, 77, 77);">文件做一些修改并保存，然后在命令中执行以上命令，它将把刚才在线修改的部分拉取到本地，用编辑器打开</font><font style="color:rgb(77, 77, 77);">README.md</font><font style="color:rgb(77, 77, 77);">，你会发现文件已经跟线上的内容同步了。</font>

_<font style="color:rgb(77, 77, 77);">如果线上代码做了变动，而你本地的代码也有变动，拉取的代码就有可能会跟你本地的改动冲突，一般情况下</font>\_\_<font style="color:rgb(77, 77, 77);"> </font>_<font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">会自动处理这种冲突合并，但如果改动的是同一行，那就需要手动来合并代码，编辑文件，保存最新的改动，再通过</font><font style="color:rgb(77, 77, 77);">git add .</font><font style="color:rgb(77, 77, 77);">和</font><font style="color:rgb(77, 77, 77);">git commit -m 'xxx'</font><font style="color:rgb(77, 77, 77);">来提交合并。</font>

#### **<font style="color:rgb(79, 79, 79);">git log</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">查看版本提交记录</font>

> <font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(193, 132, 1);">log</font>

<font style="color:rgb(77, 77, 77);">通过以上命令，我们可以查看整个项目的版本提交记录，它里面包含了</font><font style="color:rgb(77, 77, 77);">提交人</font><font style="color:rgb(77, 77, 77);">、</font><font style="color:rgb(77, 77, 77);">日期</font><font style="color:rgb(77, 77, 77);">、</font><font style="color:rgb(77, 77, 77);">提交原因</font><font style="color:rgb(77, 77, 77);">等信息，得到的结果如下：</font>

```plain
commit c334730f8dba5096c54c8ac04fdc2b31ede7107aAuthor: gafish qqqq.com>Date: Wed Jan 11 09:44:13 2017 +0800 Update README.mdcommit ba6e3d21fcb1c87a718d2a73cdd11261eb672b2aAuthor: gafish qqqq.com>Date: Wed Jan 11 09:31:33 2017 +0800 test.....
```

<font style="color:rgb(77, 77, 77);">提交记录可能会非常多，按</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">J</font><font style="color:rgb(77, 77, 77);">键往下翻，按</font><font style="color:rgb(77, 77, 77);">K</font><font style="color:rgb(77, 77, 77);">键往上翻，按</font><font style="color:rgb(77, 77, 77);">Q</font><font style="color:rgb(77, 77, 77);">键退出查看</font>

#### **<font style="color:rgb(79, 79, 79);">git tag</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">为项目标记里程碑</font>

> <font style="color:rgb(51, 51, 51);">git tag publish/</font><font style="color:rgb(152, 104, 1);">0</font><font style="color:rgb(51, 51, 51);">.</font><font style="color:rgb(152, 104, 1);">0</font><font style="color:rgb(51, 51, 51);">.</font><font style="color:rgb(152, 104, 1);">1</font><font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(166, 38, 164);">push</font><font style="color:rgb(51, 51, 51);"> origin publish/</font><font style="color:rgb(152, 104, 1);">0</font><font style="color:rgb(51, 51, 51);">.</font><font style="color:rgb(152, 104, 1);">0</font><font style="color:rgb(51, 51, 51);">.</font><font style="color:rgb(152, 104, 1);">1</font>

<font style="color:rgb(77, 77, 77);">当我们完成某个功能需求准备发布上线时，应该将此次完整的项目代码做个标记，并将这个标记好的版本发布到线上，这里我们以</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">publish/0.0.1</font><font style="color:rgb(77, 77, 77);">为标记名并发布，当看到命令行返回如下内容则表示发布成功了</font>

```plain
Total 0 (delta 0), reused 0 (delta 0)To https://github.com/gafish/gafish.github.com.git * [new tag] publish/0.0.1 -> publish/0.0.1
```

#### **<font style="color:rgb(79, 79, 79);">.gitignore</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">设置哪些内容不需要推送到服务器，这是一个配置文件</font>

> <font style="color:rgb(193, 132, 1);">touch</font><font style="color:rgb(51, 51, 51);"> .gitignore</font>

<font style="color:rgb(77, 77, 77);">.gitignore</font><font style="color:rgb(77, 77, 77);">不是</font><font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">命令，而在项目中的一个文件，通过设置</font><font style="color:rgb(77, 77, 77);">.gitignore</font><font style="color:rgb(77, 77, 77);">的内容告诉</font><font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">哪些文件应该被忽略不需要推送到服务器，通过以上命令可以创建一个</font><font style="color:rgb(77, 77, 77);">.gitignore</font><font style="color:rgb(77, 77, 77);">文件，并在编辑器中打开文件，每一行代表一个要忽略的文件或目录，如：</font>

<font style="color:rgb(51, 51, 51);">demo.htmlbuild/</font>

<font style="color:rgb(77, 77, 77);">以上内容的意思是</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">将忽略</font><font style="color:rgb(77, 77, 77);">demo.html</font><font style="color:rgb(77, 77, 77);">文件 和</font><font style="color:rgb(77, 77, 77);">build/</font><font style="color:rgb(77, 77, 77);">目录，这些内容不会被推送到服务器上</font>

#### **<font style="color:rgb(79, 79, 79);">小结</font>**

<font style="color:rgb(77, 77, 77);">通过掌握以上这些基本命令就可以在项目中开始用起来了，如果追求实用，那关于</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">的学习就可以到此结束了，偶尔遇到的问题也基本上通过</font><font style="color:rgb(77, 77, 77);">Google</font><font style="color:rgb(77, 77, 77);">也能找到答案，如果想深入探索</font><font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">的高阶功能，那就继续往下看</font><font style="color:rgb(77, 77, 77);">深入探索</font><font style="color:rgb(77, 77, 77);">部分。</font>

## **<font style="color:rgb(79, 79, 79);">深入探索</font>**

### **<font style="color:rgb(79, 79, 79);">基本概念</font>**

#### **<font style="color:rgb(79, 79, 79);">工作区(Working Directory)</font>**

<font style="color:rgb(77, 77, 77);">就是你在电脑里能看到的目录，比如上文中的</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">gafish.github</font>_<font style="color:rgb(77, 77, 77);">.com</font>_<font style="color:rgb(77, 77, 77);">文件夹就是一个工作区</font>

#### **<font style="color:rgb(79, 79, 79);">本地版本库(Local Repository)</font>**

<font style="color:rgb(77, 77, 77);">工作区有一个隐藏目录</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">.git</font><font style="color:rgb(77, 77, 77);">，这个不算工作区，而是</font><font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">的版本库。</font>

#### **<font style="color:rgb(79, 79, 79);">暂存区(stage)</font>**

<font style="color:rgb(77, 77, 77);">本地版本库里存了很多东西，其中最重要的就是称为</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">stage</font><font style="color:rgb(77, 77, 77);">(或者叫 index)的暂存区，还有</font><font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">为我们自动创建的第一个分支</font><font style="color:rgb(77, 77, 77);">master</font><font style="color:rgb(77, 77, 77);">，以及指向</font><font style="color:rgb(77, 77, 77);">master</font><font style="color:rgb(77, 77, 77);">的一个指针叫</font><font style="color:rgb(77, 77, 77);">HEAD</font><font style="color:rgb(77, 77, 77);">。</font>

#### **<font style="color:rgb(79, 79, 79);">远程版本库(Remote Repository)</font>**

<font style="color:rgb(77, 77, 77);">一般指的是</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">服务器上所对应的仓库，本文的示例所在的</font><font style="color:rgb(77, 77, 77);">github</font><font style="color:rgb(77, 77, 77);">仓库就是一个远程版本库</font>

#### **<font style="color:rgb(79, 79, 79);">以上概念之间的关系</font>**

<font style="color:rgb(77, 77, 77);">工作区</font><font style="color:rgb(77, 77, 77);">、</font><font style="color:rgb(77, 77, 77);">暂存区</font><font style="color:rgb(77, 77, 77);">、</font><font style="color:rgb(77, 77, 77);">本地版本库</font><font style="color:rgb(77, 77, 77);">、</font><font style="color:rgb(77, 77, 77);">远程版本库</font><font style="color:rgb(77, 77, 77);">之间几个常用的</font><font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">操作流程如下图所示：</font>

#### **<font style="color:rgb(79, 79, 79);">分支(Branch)</font>**

<font style="color:rgb(77, 77, 77);">分支是为了将修改记录的整个流程分开存储，让分开的分支不受其它分支的影响，所以在同一个数据库里可以同时进行多个不同的修改</font>

#### **<font style="color:rgb(79, 79, 79);">主分支(Master)</font>**

<font style="color:rgb(77, 77, 77);">前面提到过</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">master</font><font style="color:rgb(77, 77, 77);">是</font><font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">为我们自动创建的第一个分支，也叫主分支，其它分支开发完成后都要合并到</font><font style="color:rgb(77, 77, 77);">master</font>

#### **<font style="color:rgb(79, 79, 79);">标签(Tag)</font>**

<font style="color:rgb(77, 77, 77);">标签是用于标记特定的点或提交的历史，通常会用来标记发布版本的名称或版本号(如：</font><font style="color:rgb(77, 77, 77);">publish/0.0.1</font><font style="color:rgb(77, 77, 77);">)，虽然标签看起来有点像分支，但打上标签的提交是固定的，不能随意的改动，参见上图中的</font><font style="color:rgb(77, 77, 77);">1.0</font><font style="color:rgb(77, 77, 77);">/</font><font style="color:rgb(77, 77, 77);">2.0</font><font style="color:rgb(77, 77, 77);">/</font><font style="color:rgb(77, 77, 77);">3.0</font>

#### **<font style="color:rgb(79, 79, 79);">HEAD</font>**

<font style="color:rgb(77, 77, 77);">HEAD</font><font style="color:rgb(77, 77, 77);">指向的就是当前分支的最新提交</font>

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">以上概念了解的差不多，那就可以继续往下看，下面将以具体的操作类型来讲解</font><font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);"> </font><font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">Git</font><font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">的高阶用法</font>

### **<font style="color:rgb(79, 79, 79);">操作文件</font>**

#### **<font style="color:rgb(79, 79, 79);">git add</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">添加文件到暂存区</font>

<font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(166, 38, 164);">add</font><font style="color:rgb(51, 51, 51);"> -i</font>

<font style="color:rgb(77, 77, 77);">通过此命令将打开交互式子命令系统，你将看到如下子命令</font>

<font style="color:rgb(51, 51, 51);">_</font><font style="color:rgb(51, 51, 51);">_</font><font style="color:rgb(51, 51, 51);">_</font><font style="color:rgb(51, 51, 51);">Commands</font><font style="color:rgb(51, 51, 51);">_</font><font style="color:rgb(51, 51, 51);">_</font><font style="color:rgb(51, 51, 51);">_</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(152, 104, 1);">1</font><font style="color:rgb(51, 51, 51);">: status </font><font style="color:rgb(152, 104, 1);">2</font><font style="color:rgb(51, 51, 51);">: </font><font style="color:rgb(166, 38, 164);">update</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(152, 104, 1);">3</font><font style="color:rgb(51, 51, 51);">: revert </font><font style="color:rgb(152, 104, 1);">4</font><font style="color:rgb(51, 51, 51);">: </font><font style="color:rgb(166, 38, 164);">add</font><font style="color:rgb(51, 51, 51);"> untracked </font><font style="color:rgb(152, 104, 1);">5</font><font style="color:rgb(51, 51, 51);">: patch </font><font style="color:rgb(152, 104, 1);">6</font><font style="color:rgb(51, 51, 51);">: diff </font><font style="color:rgb(152, 104, 1);">7</font><font style="color:rgb(51, 51, 51);">: quit </font><font style="color:rgb(152, 104, 1);">8</font><font style="color:rgb(51, 51, 51);">: help</font>

<font style="color:rgb(77, 77, 77);">通过输入序列号或首字母可以选择相应的功能，具体的功能解释如下：</font>

- <font style="color:rgb(77, 77, 77);">status</font><font style="color:rgb(77, 77, 77);">：功能上和</font><font style="color:rgb(77, 77, 77);">git add -i</font><font style="color:rgb(77, 77, 77);">相似，没什么鸟用</font>
- <font style="color:rgb(77, 77, 77);">update</font><font style="color:rgb(77, 77, 77);">：详见下方</font><font style="color:rgb(77, 77, 77);">git add -u</font>
- <font style="color:rgb(77, 77, 77);">revert</font><font style="color:rgb(77, 77, 77);">：把已经添加到暂存区的文件从暂存区剔除，其操作方式和</font><font style="color:rgb(77, 77, 77);">update</font><font style="color:rgb(77, 77, 77);">类似</font>
- <font style="color:rgb(77, 77, 77);">add untracked</font><font style="color:rgb(77, 77, 77);">：可以把新增的文件添加到暂存区，其操作方式和</font><font style="color:rgb(77, 77, 77);">update</font><font style="color:rgb(77, 77, 77);">类似</font>
- <font style="color:rgb(77, 77, 77);">patch</font><font style="color:rgb(77, 77, 77);">：详见下方</font><font style="color:rgb(77, 77, 77);">git add -p</font>
- <font style="color:rgb(77, 77, 77);">diff</font><font style="color:rgb(77, 77, 77);">：比较暂存区文件和本地版本库的差异，其操作方式和</font><font style="color:rgb(77, 77, 77);">update</font><font style="color:rgb(77, 77, 77);">类似</font>
- <font style="color:rgb(77, 77, 77);">quit</font><font style="color:rgb(77, 77, 77);">：退出</font><font style="color:rgb(77, 77, 77);">git add -i</font><font style="color:rgb(77, 77, 77);">命令系统</font>
- <font style="color:rgb(77, 77, 77);">help</font><font style="color:rgb(77, 77, 77);">：查看帮助信息</font>

<font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(166, 38, 164);">add</font><font style="color:rgb(51, 51, 51);"> -p</font>

<font style="color:rgb(77, 77, 77);">直接进入交互命令中最有用的</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">patch</font><font style="color:rgb(77, 77, 77);">模式</font>

<font style="color:rgb(77, 77, 77);">这是交互命令中最有用的模式，其操作方式和</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">update</font><font style="color:rgb(77, 77, 77);">类似，选择后</font><font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">会显示这些文件的当前内容与本地版本库中的差异，然后您可以自己决定是否添加这些修改到暂存区，在命令行</font><font style="color:rgb(77, 77, 77);">Stage deletion [y,n,q,a,d,/,?]?</font><font style="color:rgb(77, 77, 77);">后输入</font><font style="color:rgb(77, 77, 77);">y,n,q,a,d,/,?</font><font style="color:rgb(77, 77, 77);">其中一项选择操作方式，具体功能解释如下：</font>

- <font style="color:rgb(77, 77, 77);">y：接受修改</font>
- <font style="color:rgb(77, 77, 77);">n：忽略修改</font>
- <font style="color:rgb(77, 77, 77);">q：退出当前命令</font>
- <font style="color:rgb(77, 77, 77);">a：添加修改</font>
- <font style="color:rgb(77, 77, 77);">d：放弃修改</font>
- <font style="color:rgb(77, 77, 77);">/：通过正则表达式匹配修改内容</font>
- <font style="color:rgb(77, 77, 77);">?：查看帮助信息</font>

<font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(166, 38, 164);">add</font><font style="color:rgb(51, 51, 51);"> -u</font>

<font style="color:rgb(77, 77, 77);">直接进入交互命令中的</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">update</font><font style="color:rgb(77, 77, 77);">模式</font>

<font style="color:rgb(77, 77, 77);">它会先列出工作区</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">修改</font><font style="color:rgb(77, 77, 77);">或</font><font style="color:rgb(77, 77, 77);">删除</font><font style="color:rgb(77, 77, 77);">的文件列表，</font><font style="color:rgb(77, 77, 77);">新增</font><font style="color:rgb(77, 77, 77);">的文件不会被显示，在命令行</font><font style="color:rgb(77, 77, 77);">Update>></font><font style="color:rgb(77, 77, 77);">后输入相应的列表序列号表示选中该项，回车继续选择，如果已选好，直接回车回到命令主界面</font>

> <font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(166, 38, 164);">add</font><font style="color:rgb(51, 51, 51);"> </font>_<font style="color:rgb(160, 161, 167);">--ignore-removal .</font>_

<font style="color:rgb(77, 77, 77);">添加工作区</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">修改</font><font style="color:rgb(77, 77, 77);">或</font><font style="color:rgb(77, 77, 77);">新增</font><font style="color:rgb(77, 77, 77);">的文件列表，</font><font style="color:rgb(77, 77, 77);">删除</font><font style="color:rgb(77, 77, 77);">的文件不会被添加</font>

#### **<font style="color:rgb(79, 79, 79);">git commit</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">把暂存区的文件提交到本地版本库</font>

> <font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(166, 38, 164);">commit</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">-</font><font style="color:rgb(51, 51, 51);">m </font><font style="color:rgb(80, 161, 79);">'第一行提交原因'</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">-</font><font style="color:rgb(51, 51, 51);">m </font><font style="color:rgb(80, 161, 79);">'第二行提交原因'</font>

<font style="color:rgb(77, 77, 77);">不打开编辑器，直接在命令行中输入多行提交原因</font>

> <font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(166, 38, 164);">commit</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">-</font><font style="color:rgb(51, 51, 51);">am </font><font style="color:rgb(80, 161, 79);">'提交原因'</font>

<font style="color:rgb(77, 77, 77);">将工作区</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">修改</font><font style="color:rgb(77, 77, 77);">或</font><font style="color:rgb(77, 77, 77);">删除</font><font style="color:rgb(77, 77, 77);">的文件提交到本地版本库，</font><font style="color:rgb(77, 77, 77);">新增</font><font style="color:rgb(77, 77, 77);">的文件不会被提交</font>

> <font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(166, 38, 164);">commit</font><font style="color:rgb(51, 51, 51);"> </font>_<font style="color:rgb(160, 161, 167);">--amend -m '提交原因'</font>_

<font style="color:rgb(77, 77, 77);">修改最新一条提交记录的提交原因</font>

> <font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(166, 38, 164);">commit</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">-</font><font style="color:rgb(51, 51, 51);">C HEAD</font>

<font style="color:rgb(77, 77, 77);">将当前文件改动提交到</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">HEAD</font><font style="color:rgb(77, 77, 77);">或当前分支的历史 ID</font>

#### **<font style="color:rgb(79, 79, 79);">git mv</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">移动或重命名文件、目录</font>

> <font style="color:rgb(51, 51, 51);">git mv </font><font style="color:rgb(228, 86, 73);">a</font><font style="color:rgb(152, 104, 1);">.md</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(228, 86, 73);">b</font><font style="color:rgb(152, 104, 1);">.md</font><font style="color:rgb(51, 51, 51);"> -f</font>

<font style="color:rgb(77, 77, 77);">将</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">a.md</font><font style="color:rgb(77, 77, 77);">重命名为</font><font style="color:rgb(77, 77, 77);">b.md</font><font style="color:rgb(77, 77, 77);">，同时添加变动到暂存区，加</font><font style="color:rgb(77, 77, 77);">-f</font><font style="color:rgb(77, 77, 77);">参数可以强制重命名，相比用</font><font style="color:rgb(77, 77, 77);">mv a.md b.md</font><font style="color:rgb(77, 77, 77);">命令省去了</font><font style="color:rgb(77, 77, 77);">git add</font><font style="color:rgb(77, 77, 77);">操作</font>

#### **<font style="color:rgb(79, 79, 79);">git rm</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">从工作区和暂存区移除文件</font>

> <font style="color:rgb(51, 51, 51);">git rm </font><font style="color:rgb(228, 86, 73);">b</font><font style="color:rgb(152, 104, 1);">.md</font>

<font style="color:rgb(77, 77, 77);">从工作区和暂存区移除文件</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">b.md</font><font style="color:rgb(77, 77, 77);">，同时添加变动到暂存区，相比用</font><font style="color:rgb(77, 77, 77);">rm b.md</font><font style="color:rgb(77, 77, 77);">命令省去了</font><font style="color:rgb(77, 77, 77);">git add</font><font style="color:rgb(77, 77, 77);">操作</font>

> <font style="color:rgb(51, 51, 51);">git rm </font><font style="color:rgb(80, 161, 79);">src</font><font style="color:rgb(51, 51, 51);">/ -r</font>

<font style="color:rgb(77, 77, 77);">允许从工作区和暂存区移除目录</font>

#### **<font style="color:rgb(79, 79, 79);">git status</font>**

> <font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(193, 132, 1);">status</font><font style="color:rgb(51, 51, 51);"> -s</font>

<font style="color:rgb(77, 77, 77);">以简短方式查看工作区和暂存区文件状态，示例如下：</font>

<font style="color:rgb(77, 77, 77);">M demo.html</font>

<font style="color:rgb(77, 77, 77);">?? test.html</font>

> <font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(193, 132, 1);">status</font><font style="color:rgb(51, 51, 51);"> </font>_<font style="color:rgb(160, 161, 167);">--ignored</font>_

<font style="color:rgb(77, 77, 77);">查看工作区和暂存区文件状态，包括被忽略的文件</font>

### **<font style="color:rgb(79, 79, 79);">操作分支</font>**

#### **<font style="color:rgb(79, 79, 79);">git branch</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">查看、创建、删除分支</font>

> <font style="color:rgb(51, 51, 51);">git branch -</font><font style="color:rgb(228, 86, 73);">a</font>

<font style="color:rgb(77, 77, 77);">查看本地版本库和远程版本库上的分支列表</font>

> <font style="color:rgb(51, 51, 51);">git branch -r</font>

<font style="color:rgb(77, 77, 77);">查看远程版本库上的分支列表，</font>_<font style="color:rgb(77, 77, 77);">加上</font>_<font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">-d</font><font style="color:rgb(77, 77, 77);">参数可以删除远程版本库上的分支</font>

> <font style="color:rgb(51, 51, 51);">git branch -D</font>

<font style="color:rgb(77, 77, 77);">分支未提交到本地版本库前强制删除分支</font>

> <font style="color:rgb(51, 51, 51);">git branch -vv</font>

<font style="color:rgb(77, 77, 77);">查看带有最后提交 id、最近提交原因等信息的本地版本库分支列表</font>

#### **<font style="color:rgb(79, 79, 79);">git merge</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">将其它分支合并到当前分支</font>

> <font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(166, 38, 164);">merge</font><font style="color:rgb(51, 51, 51);"> </font>_<font style="color:rgb(160, 161, 167);">--squash</font>_

<font style="color:rgb(77, 77, 77);">将待合并分支上的</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">commit</font><font style="color:rgb(77, 77, 77);">合并成一个新的</font><font style="color:rgb(77, 77, 77);">commit</font><font style="color:rgb(77, 77, 77);">放入当前分支，适用于待合并分支的提交记录不需要保留的情况</font>

> <font style="color:rgb(77, 77, 77);">git merge --no-ff</font>

<font style="color:rgb(77, 77, 77);">默认情况下，</font><font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">执行"</font><font style="color:rgb(77, 77, 77);">快进式合并</font><font style="color:rgb(77, 77, 77);">"(fast-farward merge)，会直接将</font><font style="color:rgb(77, 77, 77);">Master</font><font style="color:rgb(77, 77, 77);">分支指向</font><font style="color:rgb(77, 77, 77);">Develop</font><font style="color:rgb(77, 77, 77);">分支，使用</font><font style="color:rgb(77, 77, 77);">--no-ff</font><font style="color:rgb(77, 77, 77);">参数后，会执行正常合并，在</font><font style="color:rgb(77, 77, 77);">Master</font><font style="color:rgb(77, 77, 77);">分支上生成一个新节点，保证版本演进更清晰。</font>

> <font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(166, 38, 164);">merge</font><font style="color:rgb(51, 51, 51);"> </font>_<font style="color:rgb(160, 161, 167);">--no-edit</font>_

<font style="color:rgb(77, 77, 77);">在没有冲突的情况下合并，不想手动编辑提交原因，而是用</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">自动生成的类似</font><font style="color:rgb(77, 77, 77);">Merge branch 'test'</font><font style="color:rgb(77, 77, 77);">的文字直接提交</font>

#### **<font style="color:rgb(79, 79, 79);">git checkout</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">切换分支</font>

> <font style="color:rgb(51, 51, 51);">git checkout -</font><font style="color:rgb(228, 86, 73);">b</font><font style="color:rgb(51, 51, 51);"> daily/</font><font style="color:rgb(152, 104, 1);">0.0</font><font style="color:rgb(51, 51, 51);">.</font><font style="color:rgb(152, 104, 1);">1</font>

<font style="color:rgb(77, 77, 77);">创建</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">daily/0.0.1</font><font style="color:rgb(77, 77, 77);">分支，同时切换到这个新创建的分支</font>

> <font style="color:rgb(51, 51, 51);">git checkout HEAD demo.html</font>

<font style="color:rgb(77, 77, 77);">从本地版本库的</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">HEAD</font><font style="color:rgb(77, 77, 77);">(也可以是提交 ID、分支名、Tag 名) 历史中检出</font><font style="color:rgb(77, 77, 77);">demo.html</font><font style="color:rgb(77, 77, 77);">覆盖当前工作区的文件，如果省略</font><font style="color:rgb(77, 77, 77);">HEAD</font><font style="color:rgb(77, 77, 77);">则是从暂存区检出</font>

> <font style="color:rgb(51, 51, 51);">git checkout </font><font style="color:rgb(152, 104, 1);">--orphan</font><font style="color:rgb(51, 51, 51);"> new_branch</font>

<font style="color:rgb(77, 77, 77);">这个命令会创建一个全新的，完全没有历史记录的新分支，但当前源分支上所有的最新文件都还在，真是强迫症患者的福音，但这个新分支必须做一次</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">git commit</font><font style="color:rgb(77, 77, 77);">操作后才会真正成为一个新分支。</font>

> <font style="color:rgb(51, 51, 51);">git checkout -</font><font style="color:rgb(228, 86, 73);">p</font><font style="color:rgb(51, 51, 51);"> other_branch</font>

<font style="color:rgb(77, 77, 77);">这个命令主要用来比较两个分支间的差异内容，并提供交互式的界面来选择进一步的操作，这个命令不仅可以比较两个分支间的差异，还可以比较单个文件的差异。</font>

#### **<font style="color:rgb(79, 79, 79);">git stash</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">在</font><font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);"> </font><font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">Git</font><font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">的栈中保存当前修改或删除的工作进度，当你在一个分支里做某项功能开发时，接到通知把昨天已经测试完没问题的代码发布到线上，但这时你已经在这个分支里加入了其它未提交的代码，这个时候就可以把这些未提交的代码存到栈里。</font>

> <font style="color:rgb(51, 51, 51);">git stash</font>

<font style="color:rgb(77, 77, 77);">将未提交的文件保存到 Git 栈中</font>

<font style="color:rgb(51, 51, 51);">git stash list</font>

<font style="color:rgb(77, 77, 77);">查看栈中保存的列表</font>

> <font style="color:rgb(51, 51, 51);">git stash show </font><font style="color:rgb(64, 120, 242);">stash@</font><font style="color:rgb(51, 51, 51);">{</font><font style="color:rgb(152, 104, 1);">0</font><font style="color:rgb(51, 51, 51);">}</font>

<font style="color:rgb(77, 77, 77);">显示栈中其中一条记录</font>

> <font style="color:rgb(51, 51, 51);">git stash drop </font><font style="color:rgb(64, 120, 242);">stash@</font><font style="color:rgb(51, 51, 51);">{</font><font style="color:rgb(152, 104, 1);">0</font><font style="color:rgb(51, 51, 51);">}</font>

<font style="color:rgb(77, 77, 77);">移除栈中其中一条记录</font>

> <font style="color:rgb(51, 51, 51);">git stash </font><font style="color:rgb(166, 38, 164);">pop</font>

<font style="color:rgb(77, 77, 77);">从 Git 栈中检出最新保存的一条记录，并将它从栈中移除</font>

> <font style="color:rgb(51, 51, 51);">git stash apply </font><font style="color:rgb(64, 120, 242);">stash@</font><font style="color:rgb(51, 51, 51);">{</font><font style="color:rgb(152, 104, 1);">0</font><font style="color:rgb(51, 51, 51);">}</font>

<font style="color:rgb(77, 77, 77);">从 Git 栈中检出其中一条记录，但不从栈中移除</font>

> <font style="color:rgb(51, 51, 51);">git stash branch new_banch</font>

<font style="color:rgb(77, 77, 77);">把当前栈中最近一次记录检出并创建一个新分支</font>

> <font style="color:rgb(51, 51, 51);">git stash </font><font style="color:rgb(80, 161, 79);">clear</font>

<font style="color:rgb(77, 77, 77);">清空栈里的所有记录</font>

> <font style="color:rgb(51, 51, 51);">git stash </font><font style="color:rgb(166, 38, 164);">create</font>

<font style="color:rgb(77, 77, 77);">为当前修改或删除的文件创建一个自定义的栈并返回一个 ID，此时并未真正存储到栈里</font>

> <font style="color:rgb(51, 51, 51);">git stash store xxxxxx</font>

<font style="color:rgb(77, 77, 77);">将</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">create</font><font style="color:rgb(77, 77, 77);">方法里返回的 ID 放到</font><font style="color:rgb(77, 77, 77);">store</font><font style="color:rgb(77, 77, 77);">后面，此时在栈里真正创建了一个记录，但当前修改或删除的文件并未从工作区移除</font>

<font style="color:rgb(152, 104, 1);">$ </font><font style="color:rgb(51, 51, 51);">git stash create09eb9a97ad632d0825be1ece361936d1d0bdb5c7</font><font style="color:rgb(152, 104, 1);">$ </font><font style="color:rgb(51, 51, 51);">git stash store 09eb9a97ad632d0825be1ece361936d1d0bdb5c7</font><font style="color:rgb(152, 104, 1);">$ </font><font style="color:rgb(51, 51, 51);">git stash liststash@{</font><font style="color:rgb(152, 104, 1);">0</font><font style="color:rgb(51, 51, 51);">}: Created via </font><font style="color:rgb(80, 161, 79);">"git stash store"</font><font style="color:rgb(51, 51, 51);">.</font>

### **<font style="color:rgb(79, 79, 79);">操作历史</font>**

#### **<font style="color:rgb(79, 79, 79);">git log</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">显示提交历史记录</font>

<font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(193, 132, 1);">log</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">-</font><font style="color:rgb(51, 51, 51);">p</font>

<font style="color:rgb(77, 77, 77);">显示带提交差异对比的历史记录</font>

<font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(193, 132, 1);">log</font><font style="color:rgb(51, 51, 51);"> demo.html</font>

<font style="color:rgb(77, 77, 77);">显示</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">demo.html</font><font style="color:rgb(77, 77, 77);">文件的历史记录</font>

<font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(166, 38, 164);">log</font><font style="color:rgb(51, 51, 51);"> --since=</font><font style="color:rgb(80, 161, 79);">"2 weeks ago"</font>

<font style="color:rgb(77, 77, 77);">显示 2 周前开始到现在的历史记录，其它时间可以类推</font>

<font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(166, 38, 164);">log</font><font style="color:rgb(51, 51, 51);"> --before=</font><font style="color:rgb(80, 161, 79);">"2 weeks ago"</font>

<font style="color:rgb(77, 77, 77);">显示截止到 2 周前的历史记录，其它时间可以类推</font>

<font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(193, 132, 1);">log</font><font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">-</font><font style="color:rgb(152, 104, 1);">10</font>

<font style="color:rgb(77, 77, 77);">显示最近 10 条历史记录</font>

<font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(193, 132, 1);">log</font><font style="color:rgb(51, 51, 51);"> f5f630a..HEAD</font>

<font style="color:rgb(77, 77, 77);">显示从提交 ID</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">f5f6</font>_<font style="color:rgb(77, 77, 77);">30</font>_<font style="color:rgb(77, 77, 77);">a</font><font style="color:rgb(77, 77, 77);">到</font><font style="color:rgb(77, 77, 77);">HEAD</font><font style="color:rgb(77, 77, 77);">之间的记录，</font><font style="color:rgb(77, 77, 77);">HEAD</font><font style="color:rgb(77, 77, 77);">可以为空或其它提交 ID</font>

<font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(193, 132, 1);">log</font><font style="color:rgb(51, 51, 51);"> </font>_<font style="color:rgb(160, 161, 167);">--pretty=oneline</font>_

<font style="color:rgb(77, 77, 77);">在一行中输出简短的历史记录</font>

<font style="color:rgb(51, 51, 51);">git </font><font style="color:rgb(166, 38, 164);">log</font><font style="color:rgb(51, 51, 51);"> --pretty=</font><font style="color:rgb(166, 38, 164);">format</font><font style="color:rgb(51, 51, 51);">:</font><font style="color:rgb(80, 161, 79);">"%h"</font><font style="color:rgb(51, 51, 51);"> </font>

<font style="color:rgb(77, 77, 77);">格式化输出历史记录</font>

<font style="color:rgb(77, 77, 77);">Git</font><font style="color:rgb(77, 77, 77);">用各种</font><font style="color:rgb(77, 77, 77);">placeholder</font><font style="color:rgb(77, 77, 77);">来决定各种显示内容，我挑几个常用的显示如下：</font>

- <font style="color:rgb(77, 77, 77);">%H: commit hash</font>
- <font style="color:rgb(77, 77, 77);">%h: 缩短的 commit hash</font>
- <font style="color:rgb(77, 77, 77);">%T: tree hash</font>
- <font style="color:rgb(77, 77, 77);">%t: 缩短的 tree hash</font>
- <font style="color:rgb(77, 77, 77);">%P: parent hashes</font>
- <font style="color:rgb(77, 77, 77);">%p: 缩短的 parent hashes</font>
- <font style="color:rgb(77, 77, 77);">%an: 作者名字</font>
- <font style="color:rgb(77, 77, 77);">%aN: mailmap 的作者名</font>
- <font style="color:rgb(77, 77, 77);">%ae: 作者邮箱</font>
- <font style="color:rgb(77, 77, 77);">%ad: 日期 (--date= 制定的格式)</font>
- <font style="color:rgb(77, 77, 77);">%ar: 日期, 相对格式(1 day ago)</font>
- <font style="color:rgb(77, 77, 77);">%cn: 提交者名字</font>
- <font style="color:rgb(77, 77, 77);">%ce: 提交者 email</font>
- <font style="color:rgb(77, 77, 77);">%cd: 提交日期 (--date= 制定的格式)</font>
- <font style="color:rgb(77, 77, 77);">%cr: 提交日期, 相对格式(1 day ago)</font>
- <font style="color:rgb(77, 77, 77);">%d: ref 名称</font>
- <font style="color:rgb(77, 77, 77);">%s: commit 信息标题</font>
- <font style="color:rgb(77, 77, 77);">%b: commit 信息内容</font>
- <font style="color:rgb(77, 77, 77);">%n: 换行</font>

#### **<font style="color:rgb(79, 79, 79);">git cherry-pick</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">合并分支的一条或几条提交记录到当前分支末梢</font>

<font style="color:rgb(51, 51, 51);">git cherry-pick 170a305</font>

<font style="color:rgb(77, 77, 77);">合并提交 ID</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">170a</font>_<font style="color:rgb(77, 77, 77);">30</font>_<font style="color:rgb(77, 77, 77);">5</font><font style="color:rgb(77, 77, 77);">到当前分支末梢</font>

#### **<font style="color:rgb(79, 79, 79);">git reset</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">将当前的分支重设(reset)到指定的</font><font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);"> </font><font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">或者</font><font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">HEAD</font>

<font style="color:rgb(51, 51, 51);">git reset </font><font style="color:rgb(152, 104, 1);">--mixed</font><font style="color:rgb(51, 51, 51);"> </font>

<font style="color:rgb(77, 77, 77);">--mixed</font><font style="color:rgb(77, 77, 77);">是不带参数时的默认参数，它退回到某个版本，保留文件内容，回退提交历史</font>

<font style="color:rgb(51, 51, 51);">git reset </font><font style="color:rgb(152, 104, 1);">--soft</font><font style="color:rgb(51, 51, 51);"> </font>

<font style="color:rgb(77, 77, 77);">暂存区和工作区中的内容不作任何改变，仅仅把</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">HEAD</font><font style="color:rgb(77, 77, 77);">指向</font>

<font style="color:rgb(51, 51, 51);">git reset </font><font style="color:rgb(152, 104, 1);">--hard</font><font style="color:rgb(51, 51, 51);"> </font>

<font style="color:rgb(77, 77, 77);">自从</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">以来在工作区中的任何改变都被丢弃，并把</font><font style="color:rgb(77, 77, 77);">HEAD</font><font style="color:rgb(77, 77, 77);">指向</font>

#### **<font style="color:rgb(79, 79, 79);">git rebase</font>**

<font style="color:rgb(79, 79, 79);background-color:rgb(238, 240, 244);">重新定义分支的版本库状态</font>

<font style="color:rgb(51, 51, 51);">git rebase branch_name</font>

<font style="color:rgb(77, 77, 77);">合并分支，这跟</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">merge</font><font style="color:rgb(77, 77, 77);">很像，但还是有本质区别，看下图：</font>

<font style="color:rgb(77, 77, 77);">合并过程中可能需要先解决冲突，然后执行</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">git rebase --continue</font>

<font style="color:rgb(51, 51, 51);">git rebase -</font><font style="color:rgb(228, 86, 73);">i</font><font style="color:rgb(51, 51, 51);"> HEAD~~</font>

<font style="color:rgb(77, 77, 77);">打开文本编辑器，将看到从</font><font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">HEAD</font><font style="color:rgb(77, 77, 77);">到</font><font style="color:rgb(77, 77, 77);">HEAD~~</font><font style="color:rgb(77, 77, 77);">的提交如下</font>

<font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">pick </font><font style="color:rgb(152, 104, 1);background-color:rgb(250, 250, 250);">9</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">a54fd4 添加</font><font style="color:rgb(166, 38, 164);background-color:rgb(250, 250, 250);">commit</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">的说明 pick </font><font style="color:rgb(152, 104, 1);background-color:rgb(250, 250, 250);">0</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">d4a808 添加 pull 的说明# Rebase </font><font style="color:rgb(152, 104, 1);background-color:rgb(250, 250, 250);">326</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">fc9f.</font><font style="color:rgb(152, 104, 1);background-color:rgb(250, 250, 250);">.0</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">d4a808 onto d286baa## Commands:# p, pick </font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">=</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);"> use </font><font style="color:rgb(166, 38, 164);background-color:rgb(250, 250, 250);">commit</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);"># r, reword </font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">=</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);"> use </font><font style="color:rgb(166, 38, 164);background-color:rgb(250, 250, 250);">commit</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">, but edit the </font><font style="color:rgb(166, 38, 164);background-color:rgb(250, 250, 250);">commit</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);"> message# e, edit </font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">=</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);"> use </font><font style="color:rgb(166, 38, 164);background-color:rgb(250, 250, 250);">commit</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">, but stop </font><font style="color:rgb(166, 38, 164);background-color:rgb(250, 250, 250);">for</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);"> amending# s, squash </font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">=</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);"> use </font><font style="color:rgb(166, 38, 164);background-color:rgb(250, 250, 250);">commit</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">, but meld </font><font style="color:rgb(166, 38, 164);background-color:rgb(250, 250, 250);">into</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);"> previous </font><font style="color:rgb(166, 38, 164);background-color:rgb(250, 250, 250);">commit</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);"># f, fixup </font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">=</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);"> </font><font style="color:rgb(166, 38, 164);background-color:rgb(250, 250, 250);">like</font><font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);"> "squash</font>
