---
title: PHP使用Solr全文搜索引擎(windows)
urlname: mkvdtz
date: '2022-07-18 01:23:51 +0000'
tags: []
categories: []
---

tags: [php,sole,搜索,全文搜索,搜索引擎,windows]

categories: <font style="color:rgb(38, 38, 38);">学无止境</font>

copyright_author_href: https://www.xiaohuihui.net

<font style="color:rgb(38, 38, 38);">copyright_url:  
</font><font style="color:rgb(38, 38, 38);">copyright_author: </font>

<font style="color:rgb(33, 37, 41);">cover:</font>

---

> <font style="color:rgb(51, 51, 51);">在 windows 上使用 solr 全文搜索引擎</font>

# <font style="color:rgb(51, 51, 51);">安装 solr</font>

## 安装 jre 运行环境

<font style="color:rgb(77, 77, 77);">已正确安装 JRE 运行环境。</font>  
<font style="color:rgb(77, 77, 77);">JRE 安装请参考《JRE 安装指南》。</font>

## <font style="color:rgb(77, 77, 77);">下载 solr</font>

[下载地址](https://solr.apache.org/downloads.html)<font style="color:rgb(77, 77, 77);">：https://solr.apache.org/downloads.html ，如图所示：</font>

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658127114583-33d0d408-62e0-482f-b7b6-cc5306a5be60.png)

<font style="color:rgb(77, 77, 77);">或者：</font>

[http://archive.apache.org/dist/lucene/solr/](http://archive.apache.org/dist/lucene/solr/)

<font style="color:rgb(77, 77, 77);"></font>

<font style="color:rgb(77, 77, 77);">各系统适用版本见下表。</font>

|   **<font style="color:rgb(79, 79, 79);">安装包版本</font>**   |                          **<font style="color:rgb(79, 79, 79);">适用系统</font>**                          |
| :------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------: |
| <font style="color:rgb(79, 79, 79);">solr-X.X.X-src.tgz</font> | <font style="color:rgb(79, 79, 79);">Solr 源代码。可在不使用官方 Git 存储库的情况下在 Solr 上进行开</font> |
|   <font style="color:rgb(79, 79, 79);">solr-X.X.X.tgz</font>   |                         <font style="color:rgb(79, 79, 79);">Linux/Unix/OSX</font>                         |
|   <font style="color:rgb(79, 79, 79);">solr-X.X.X.zip</font>   |                       <font style="color:rgb(79, 79, 79);">Microsoft Windows</font>                        |

## <font style="color:rgb(79, 79, 79);">安装 Solr</font>

**<font style="color:rgb(77, 77, 77);">使用解压工具解压安装包，解压即可启动</font>**<font style="color:rgb(77, 77, 77);">。解压后目录结构如表所示：</font>

| **<font style="color:rgb(79, 79, 79);">目录名称</font>** |                    **<font style="color:rgb(79, 79, 79);">功能</font>**                    |  **<font style="color:rgb(79, 79, 79);">次级文件/文件目录</font>**   |                                                                                       **<font style="color:rgb(79, 79, 79);">功能</font>**                                                                                        |
| :------------------------------------------------------: | :----------------------------------------------------------------------------------------: | :------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|     <font style="color:rgb(79, 79, 79);">bin/</font>     | <font style="color:rgb(79, 79, 79);">包含几个重要的脚本，它们将使 Solr 的使用更容易</font> |      <font style="color:rgb(79, 79, 79);">Solr、solr.cmd</font>      | <font style="color:rgb(79, 79, 79);">是 Solr 的控制脚本，也称为 bin/solr(\*nix) / bin/solr.cmd(Windows)。此脚本是启动和停止 Solr 的首选工具。在 SolrCloud 模式下运行时，您还可以创建集合或核心、配置身份验证和使用配置文件</font> |
|                                                          |                                                                                            |           <font style="color:rgb(79, 79, 79);">post</font>           |                                                                    <font style="color:rgb(79, 79, 79);">提供了用于发布内容到 Solr 一个简单的命令行界面</font>                                                                     |
|                                                          |                                                                                            | <font style="color:rgb(79, 79, 79);">solr.in.sh、solr.in.cmd</font>  |          <font style="color:rgb(79, 79, 79);">是 \*nix 和 Windows 系统的属性文件。Java、Jetty 和 Solr 的系统级属性在此处配置。使用 bin/solr/时可以覆盖其中许多设置 bin/solr.cmd，但这允许您在一个地方设置所有属性</font>          |
|                                                          |                                                                                            | <font style="color:rgb(79, 79, 79);">install_solr_services.sh</font> |                                                                      <font style="color:rgb(79, 79, 79);">此脚本在 \*nix 系统上用于将 Solr 安装为服务</font>                                                                      |
|   <font style="color:rgb(79, 79, 79);">contrib/</font>   |        <font style="color:rgb(79, 79, 79);">包括用于 Solr 特殊功能的附加插件</font>        |            <font style="color:rgb(79, 79, 79);">/</font>             |                                                                                           <font style="color:rgb(79, 79, 79);">/</font>                                                                                           |
|    <font style="color:rgb(79, 79, 79);">dist/</font>     |           <font style="color:rgb(79, 79, 79);">包含主要的 Solr .jar 文件</font>            |            <font style="color:rgb(79, 79, 79);">/</font>             |                                                                                           <font style="color:rgb(79, 79, 79);">/</font>                                                                                           |
|    <font style="color:rgb(79, 79, 79);">docs/</font>     |      <font style="color:rgb(79, 79, 79);">包含指向 Solr 的在线 Javadocs 的链接</font>      |            <font style="color:rgb(79, 79, 79);">/</font>             |                                                                                           <font style="color:rgb(79, 79, 79);">/</font>                                                                                           |
|   <font style="color:rgb(79, 79, 79);">example/</font>   |   <font style="color:rgb(79, 79, 79);">包含多种类型的示例，用于演示各种 Solr 功能</font>   |            <font style="color:rgb(79, 79, 79);">/</font>             |                                                                                           <font style="color:rgb(79, 79, 79);">/</font>                                                                                           |
|  <font style="color:rgb(79, 79, 79);">licenses/</font>   |     <font style="color:rgb(79, 79, 79);">包含 Solr 使用的第 3 方库的所有许可证</font>      |            <font style="color:rgb(79, 79, 79);">/</font>             |                                                                                           <font style="color:rgb(79, 79, 79);">/</font>                                                                                           |
|   <font style="color:rgb(79, 79, 79);">server/</font>    |           <font style="color:rgb(79, 79, 79);">是 Solr 应用程序的核心所在</font>           |       <font style="color:rgb(79, 79, 79);">/solr-webapp</font>       |                                                                                    <font style="color:rgb(79, 79, 79);">Solr 的 UI 管理</font>                                                                                    |
|                                                          |                                                                                            |           <font style="color:rgb(79, 79, 79);">/lib</font>           |                                                                                    <font style="color:rgb(79, 79, 79);">Jetty libraries</font>                                                                                    |
|                                                          |                                                                                            |          <font style="color:rgb(79, 79, 79);">/logs</font>           |                                                                                       <font style="color:rgb(79, 79, 79);">日志文件</font>                                                                                        |
|                                                          |                                                                                            |        <font style="color:rgb(79, 79, 79);">/resources</font>        |                                                                                       <font style="color:rgb(79, 79, 79);">日志配置</font>                                                                                        |
|                                                          |                                                                                            |     <font style="color:rgb(79, 79, 79);">/solr/configsets</font>     |                                                                                      <font style="color:rgb(79, 79, 79);">示例配置集</font>                                                                                       |

## <font style="color:rgb(79, 79, 79);">启动 solr</font>

<font style="color:rgb(77, 77, 77);">Solr 包括一个名为 bin/solr(Linux/MacOS) 或 bin\solr.cmd(Windows)的命令行界面工具。该工具允许您启动和停止 Solr、创建核心和集合、配置身份验证以及检查系统状态。  
</font><font style="color:rgb(77, 77, 77);">要使用它来启动 Solr，只需在解压后的 bin 目录下打开命令行，执行命令：</font>

````

<font style="color:rgb(153, 153, 153);background-color:rgb(250, 250, 250);">.</font><font style="color:rgb(166, 127, 89);background-color:rgb(250, 250, 250);">/</font><font style="background-color:rgb(250, 250, 250);">solr </font><font style="color:rgb(221, 74, 104);background-color:rgb(250, 250, 250);">start</font><font style="background-color:rgb(250, 250, 250);"></font>

<font style="color:rgb(153, 153, 153);background-color:rgb(250, 250, 250);">~~~</font>

<font style="color:rgb(153, 153, 153);background-color:rgb(250, 250, 250);"></font>

<font style="color:rgb(153, 153, 153);background-color:rgb(250, 250, 250);">几条常用命令</font>

```shell
solr start –p 端口号 #单机版启动solr服务
solr restart –p 端口号 #重启solr服务
solr stop –p 端口号 #关闭solr服务

```

<font style="color:rgb(153, 153, 153);background-color:rgb(250, 250, 250);"></font>

<font style="color:rgb(77, 77, 77);">这将在后台启动 Solr，侦听端口 8983。当您在后台启动 Solr 时，脚本将等待以确保 Solr 正确启动，然后再返回命令行提示符。
</font><font style="color:rgb(77, 77, 77);">启动完成后默认监听8983端口，</font>[管理界面](http://localhost:8983/solr/)<font style="color:rgb(77, 77, 77);">访问地址：http://localhost:8983/solr/</font>

## <font style="color:rgb(79, 79, 79);"> 停止Solr</font>
<font style="color:rgb(77, 77, 77);">在bin目录下执行命令：</font>

<font style="color:rgb(153, 153, 153);background-color:rgb(250, 250, 250);">.</font><font style="color:rgb(166, 127, 89);background-color:rgb(250, 250, 250);">/</font><font style="background-color:rgb(250, 250, 250);">solr stop </font><font style="color:rgb(166, 127, 89);background-color:rgb(250, 250, 250);">-</font><font style="background-color:rgb(250, 250, 250);">all</font>

<font style="color:rgb(77, 77, 77);">注：停止时必须指定端口。</font>

# <font style="color:rgb(51, 51, 51);">安装php扩展solr .dll</font>
## 0、环境
1. 操作系统：Windows10
2. solr: 8.9.0
3. php：7.3 （nts）

## 1、扩展说明
**扩展地址：**

[<font style="background-color:rgb(250, 250, 250);">https://pecl.php.net/ </font>](https://pecl.php.net/ )
**<font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);">确定线性与非线性</font>**

<font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);">Non Thread Safe </font><font style="color:rgb(153, 153, 153);">(</font><font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);">NTS</font><font style="color:rgb(153, 153, 153);">)</font><font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);"> x64 / Thread Safe </font><font style="color:rgb(153, 153, 153);">(</font><font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);">TS</font><font style="color:rgb(153, 153, 153);">)</font><font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);"> x64</font>

<font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);">通过phpinfo</font><font style="color:rgb(153, 153, 153);">();</font><font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);"> 查看其中的 Thread Safety 项，这个项目就是查看是否是线程安全</font>

<font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);">如果是：enabled，一般来说应该是ts版，否则是nts版。</font>

##
2、安装php扩展
打开扩展下载地址：

[https://pecl.php.net/package/solr](https://pecl.php.net/package/solr)

选择版本后面的dll文件（我用的是2.5.1版本的dll）

[https://windows.php.net/downloads/pecl/releases/solr/2.5.1/php_solr-2.5.1-7.3-nts-vc15-x64.zip](https://windows.php.net/downloads/pecl/releases/solr/2.5.1/php_solr-2.5.1-7.3-nts-vc15-x64.zip)

并点击下载。

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658108004278-e1ac5886-8688-4801-a581-e5ebf234ea61.png)

下载成功后，解压获取：

1. <font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);">php_solr.dll </font>
2. <font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);">php_solr.pdb</font>

<font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);">将压缩包的php_solr.dll、php_solr.pdb 放到你的 php 扩展目录下 php/ext/ 下。 </font>

<font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);">php.ini中加入 extension</font><font style="color:rgb(166, 127, 89);">=</font><font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);">php_solr.dll </font>

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658108202536-3d93ee60-5d56-4be8-8aa3-91c280883f42.png)



<font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);">我的集成环境位置： </font>D:\phpEnv\php\php-7.3\ext

<font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);">重启服务器，查看phpinfo</font><font style="color:rgb(153, 153, 153);">()</font><font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);">，</font>

<font style="color:rgb(0, 0, 0);background-color:rgb(250, 250, 250);">是否有显示solr扩展加载成功。</font>

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658108254208-edbc8d29-1a2f-483b-b39e-44219d65f46d.png)

# <font style="color:rgb(79, 79, 79);"> 常用操作</font>
**<font style="color:rgb(77, 77, 77);">创建core，-c指定创建的core名</font>**

<font style="color:rgb(153, 153, 153);background-color:rgb(250, 250, 250);">.</font><font style="color:rgb(166, 127, 89);background-color:rgb(250, 250, 250);">/</font><font style="background-color:rgb(250, 250, 250);">solr create </font><font style="color:rgb(166, 127, 89);background-color:rgb(250, 250, 250);">-</font><font style="background-color:rgb(250, 250, 250);">c test_core1 </font><font style="color:rgb(153, 153, 153);background-color:rgb(250, 250, 250);">1</font>

**<font style="color:rgb(77, 77, 77);">删除core，-c指定删除的core名</font>**

<font style="color:rgb(153, 153, 153);">.</font><font style="color:rgb(166, 127, 89);">/</font>solr delete <font style="color:rgb(166, 127, 89);">-</font>c test_core1



![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658127465096-e38fc348-6514-4fc7-a764-ce946777deb2.png)

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658127470711-73c2ff90-c5ed-438a-b040-e14c99ded58a.png)

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658127476667-41a12875-1ffb-4285-bd57-21ce09aa06a6.png)

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658127481012-dc1b0ebf-6f3e-49c9-bd52-96e2259dd09e.png)

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658127485301-62701194-113b-487c-92be-12289aabb291.png)



## 二、创建核心
### 2.1 创建核心前准备工作
每个核心都是solr的一个实例，一个solr服务可以创建多个核心，每个核心都可以进行自己独立配置。

+ 1.切换至solr_home\server\solr目录，例如：E:\solr-8.1.1\server\solr，在该目录下创建一个文件夹，文件夹名称与核心名称相同。 ![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127788037-3dd84813-0a7f-456b-a0e1-57e0833147f6.webp)_v_images/2019062
+ 2.将E:\solr-8.1.1\server\solr\configsets_default路径下的conf文件夹复制到new_core目录（E:\solr-8.1.1\server\solr\new_core）下。

### 2.2 创建核心
#### 方式一：打开solr界面，进行如图顺序操作。
这种方式 需要 在 E:\solr-8.1.1\server\solr 目录下创建 new_core文件夹

![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127788016-424a695c-b554-4066-b999-736c33b8b2c3.webp)

#### 方式二：bin目录下输入命令：solr create -c new_core （推荐）
## 三、schema
每个核心core中都有这么一个 schema配置文件。 schema文件可以对索引库的数据类型进行定义，对字段是否进行索引、存储等进行配置，需要针对狠心单独进行配置。 schema的数据类型进本够用，如果不能满足需求，比如说对中文分词、拼音分词等，就可以自定义分词器。 Solr8.1.1 的schema的配置文件名 为managed-schema,home\server\solr\new_core\conf\managed-schema（E:\solr-8.1.1\server\solr\core_issuer\conf）。

### 3.1 schema主要成员
+ （1） fieldType：为field定义类型，最主要作用是定义分词器，分词器决定着如何从文档中检索关键字。
+ （2） analyzer：fieldType的子元素，是分词器，由tokenizer和filter组成。例如

<fieldType name="text_tr" class="solr.TextField" positionIncrementGap="100">     <analyzer>       <tokenizer class="solr.StandardTokenizerFactory"/>       <filter class="solr.TurkishLowerCaseFilterFactory"/>       <filter class="solr.StopFilterFactory" words="lang/stopwords_tr.txt" ignoreCase="false"/>       <filter class="solr.SnowballPorterFilterFactory" language="Turkish"/>     </analyzer>   </fieldType> 复制代码

+ （3） field：字段，用来创建索引,如果这个字段需要生成索引，则需要设置的indexed为true，需要存储设置stored属性为true。例如：

<field name="age" type="pint" indexed="true" stored="true"/> 复制代码

### 3.2 添加索引字段
+ 方式一：直接修改managed-schema配置文件（不推荐，修改后需要重启服务），例如：

<font style="color:rgb(51, 51, 51);background-color:rgb(248, 248, 248);"><field </font><font style="color:rgb(51, 51, 51);">name</font><font style="color:rgb(51, 51, 51);background-color:rgb(248, 248, 248);">=</font><font style="color:rgb(221, 17, 68);">"age"</font><font style="color:rgb(51, 51, 51);background-color:rgb(248, 248, 248);"> type=</font><font style="color:rgb(221, 17, 68);">"pint"</font><font style="color:rgb(51, 51, 51);background-color:rgb(248, 248, 248);"> indexed=</font><font style="color:rgb(221, 17, 68);">"true"</font><font style="color:rgb(51, 51, 51);background-color:rgb(248, 248, 248);"> stored=</font><font style="color:rgb(221, 17, 68);">"true"</font><font style="color:rgb(51, 51, 51);background-color:rgb(248, 248, 248);">/></font>

+ 方式二：通过solr页面进行添加。（推荐，不需要重启服务）

<font style="color:rgb(51, 51, 51);">![](_v_images/20190623133627274_22114.png =811x)</font>

### 3.3 配置中文分词工具
+ 1.下载中文分词器IKAnalyzer，[下载地址](https://link.juejin.cn?target=https%3A%2F%2Fpan.baidu.com%2Fs%2F19_nVrOrUrj00rlu13OGOkg)，密码：igt9。
+ 2.解压压缩包包，目录如下：

![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127788037-71d0ec37-c053-45ac-a19b-1f4495e4ee58.webp)

+ 3.将两个jar包复制到该路径下：E:\solr-8.1.1\server\solr-webapp\webapp\WEB-INF\lib。
+ 4.另外将三个配置文件复制到该路径下：E:\solr-8.1.1\server\solr-webapp\webapp\WEB-INF\classes。如果没有classes文件夹就新建一个。
+ 5.在schema中添加分词器。

```shell
<fieldType name="text_ik" class="solr.TextField">
  <analyzer type="index">
	  <tokenizer class="org.wltea.analyzer.lucene.IKTokenizerFactory" useSmart="false" conf="ik.conf"/>
	  <filter class="solr.LowerCaseFilterFactory"/>
  </analyzer>
  <analyzer type="query">
	  <tokenizer class="org.wltea.analyzer.lucene.IKTokenizerFactory" useSmart="true" conf="ik.conf"/>
	  <filter class="solr.LowerCaseFilterFactory"/>
  </analyzer>
</fieldType>

```

+ 6.在ext.dic文件中添加自定义的中文词组
+ 需要重启solr

效果如下：

![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127788089-a128751b-080b-4cb5-8903-2951345cf1f8.webp)

## 四、导入索引数据（mysql数据为例）
+ 1.创建mysql数据库

![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127788040-b8b6f2fa-b4f0-4559-a787-1a52871abc8c.webp)

+ 2.在该路径下solr_home\server\solr\new_core\conf（E:\solr-8.1.1\server\solr\core_issuer\conf）下新建my-data-config.xml文件

```shell
<?xml version="1.0" encoding="UTF-8" ?>
<dataConfig>
    <dataSource type="JdbcDataSource" driver="com.mysql.jdbc.Driver" url="jdbc:mysql://localhost:3306/solr_test"
                user="root" password=""/>
    <document>
        <entity name="user" query="select * from user">
            <field column="id" name="id"/>
            <field column="age" name="age"/>
            <field column="name" name="name"/>
            <field column="hobby" name="hobby"/>
        </entity>
    </document>
</dataConfig>

```

+ 3.用solr添加数据库字段对应的索引字段，添加后打开managed-schema文件会看到：

```shell
<field name="name" type="string" indexed="true" stored="true"/>
<field name="age" type="pint" indexed="true" stored="true"/>
<field name="hobby" type="string" indexed="true" stored="true"/>

```

**请勿添加id字段，该字段已存在，添加会报错**

+ 4.打开该路径下文件：solr_home\server\solr\new_core\conf\solrconfig.xml（E:\solr-8.1.1\server\solr\core_issuer\conf\solrconfig.xml），随便找一个与requestHandler同级节点上添加以下配置。如图：

![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127789705-05706f41-625e-4027-a4d1-1cb9a19bb621.webp)

```shell
<requestHandler name="/dataimport" class="org.apache.solr.handler.dataimport.DataImportHandler">
	<lst name="defaults">
		<str name="config">my-data-config.xml</str>
	</lst>
</requestHandler>


```

+ 5.将solr_home\dist（E:\solr-8.1.1\dist）目录下的solr-dataimporthandler-8.1.1.jar和MySQL驱动（随便找个MySQL驱动）复制到solr_home\server\solr-webapp\webapp\WEB-INF\lib（E:\solr-8.1.1\server\solr-webapp\webapp\WEB-INF\lib）目录下。
+ 6.重启solr服务。
+ 7.打开solr页面，进行下面操作。

![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127789821-cd1c7622-59de-4e9a-b69c-c9ce68fdef58.webp)

+ 8.检测数据是否导入成功

![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127789862-dbbacbb7-e01b-46be-bcd7-80aad8c48a24.webp)

### 如果对索引字段进行中文分词
添加对索引字段时，fileType ：选择text_ik

![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127789803-bc546502-4fcb-43cf-a968-b9fda978fe7b.webp)

效果如图： 查询 **鲁西化工** 的结果

![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127789921-c9625e48-e98f-4529-83f4-57a6a48b77fc.webp)

##
## 七、删除索引库（删除所有数据）的两种方法
+ 1.删除索引库所有数据的方法一：直接用solr可视化界面删除

![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127790457-5b348534-e2ba-4b26-960f-80171f49d3e4.webp)

````
