---
title: PHP使用Solr全文搜索引擎(windows)
urlname: mkvdtz
date: '2022-07-18 01:23:51 +0000'
tags:
  - php
  - sole
  - 搜索
  - 全文搜索
  - 搜索引擎
  - windows
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
copyright_url:
copyright_author:
cover:
---

> 在 windows 上使用 solr 全文搜索引擎

# 安装 solr

## 安装 jre 运行环境

已正确安装 JRE 运行环境。
JRE 安装请参考《JRE 安装指南》。

## 下载 solr

[下载地址](https://solr.apache.org/downloads.html)：https://solr.apache.org/downloads.html ，如图所示：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658127114583-33d0d408-62e0-482f-b7b6-cc5306a5be60.png#clientId=u591e7470-37f3-4&from=paste&height=680&id=u909eb52e&originHeight=751&originWidth=1151&originalType=binary∶=1&rotation=0&showTitle=false&size=68403&status=done&style=none&taskId=u1ee3913b-ec2f-4836-8a4e-9e37e94bca2&title=&width=1042.415131853756)

或者：
[http://archive.apache.org/dist/lucene/solr/](http://archive.apache.org/dist/lucene/solr/)

各系统适用版本见下表。

| **安装包版本**     | **适用系统**                                                   |
| ------------------ | -------------------------------------------------------------- |
| solr-X.X.X-src.tgz | Solr 源代码。可在不使用官方 Git 存储库的情况下在 Solr 上进行开 |
| solr-X.X.X.tgz     | Linux/Unix/OSX                                                 |
| solr-X.X.X.zip     | Microsoft Windows                                              |

## 安装 Solr

**使用解压工具解压安装包，解压即可启动**。解压后目录结构如表所示：

| **目录名称** | **功能**                                       | **次级文件/文件目录**    | **功能**                                                                                                                                                                              |
| ------------ | ---------------------------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bin/         | 包含几个重要的脚本，它们将使 Solr 的使用更容易 | Solr、solr.cmd           | 是 Solr 的控制脚本，也称为 bin/solr(\*nix) / bin/solr.cmd(Windows)。此脚本是启动和停止 Solr 的首选工具。在 SolrCloud 模式下运行时，您还可以创建集合或核心、配置身份验证和使用配置文件 |
|              |                                                | post                     | 提供了用于发布内容到 Solr 一个简单的命令行界面                                                                                                                                        |
|              |                                                | solr.in.sh、solr.in.cmd  | 是 \*nix 和 Windows 系统的属性文件。Java、Jetty 和 Solr 的系统级属性在此处配置。使用 bin/solr/时可以覆盖其中许多设置 bin/solr.cmd，但这允许您在一个地方设置所有属性                   |
|              |                                                | install_solr_services.sh | 此脚本在 \*nix 系统上用于将 Solr 安装为服务                                                                                                                                           |
| contrib/     | 包括用于 Solr 特殊功能的附加插件               | /                        | /                                                                                                                                                                                     |
| dist/        | 包含主要的 Solr .jar 文件                      | /                        | /                                                                                                                                                                                     |
| docs/        | 包含指向 Solr 的在线 Javadocs 的链接           | /                        | /                                                                                                                                                                                     |
| example/     | 包含多种类型的示例，用于演示各种 Solr 功能     | /                        | /                                                                                                                                                                                     |
| licenses/    | 包含 Solr 使用的第 3 方库的所有许可证          | /                        | /                                                                                                                                                                                     |
| server/      | 是 Solr 应用程序的核心所在                     | /solr-webapp             | Solr 的 UI 管理                                                                                                                                                                       |
|              |                                                | /lib                     | Jetty libraries                                                                                                                                                                       |
|              |                                                | /logs                    | 日志文件                                                                                                                                                                              |
|              |                                                | /resources               | 日志配置                                                                                                                                                                              |
|              |                                                | /solr/configsets         | 示例配置集                                                                                                                                                                            |

## 启动 solr

Solr 包括一个名为 bin/solr(Linux/MacOS) 或 bin\solr.cmd(Windows)的命令行界面工具。该工具允许您启动和停止 Solr、创建核心和集合、配置身份验证以及检查系统状态。
要使用它来启动 Solr，只需在解压后的 bin 目录下打开命令行，执行命令：

```
./solr start
```

几条常用命令

```shell
solr start –p 端口号 #单机版启动solr服务
solr restart –p 端口号 #重启solr服务
solr stop –p 端口号 #关闭solr服务

```

这将在后台启动 Solr，侦听端口 8983。当您在后台启动 Solr 时，脚本将等待以确保 Solr 正确启动，然后再返回命令行提示符。
启动完成后默认监听 8983 端口，[管理界面](http://localhost:8983/solr/)访问地址：http://localhost:8983/solr/

## 停止 Solr

在 bin 目录下执行命令：
./solr stop -all
注：停止时必须指定端口。

# 安装 php 扩展 solr .dll

## 0、环境

1. 操作系统：Windows10
2. solr: 8.9.0
3. php：7.3 （nts）

## 1、扩展说明

**扩展地址：**
[https://pecl.php.net/ ](https://pecl.php.net/)
**确定线性与非线性**
Non Thread Safe (NTS) x64 / Thread Safe (TS) x64
通过 phpinfo(); 查看其中的 Thread Safety 项，这个项目就是查看是否是线程安全
如果是：enabled，一般来说应该是 ts 版，否则是 nts 版。

##

2、安装 php 扩展
打开扩展下载地址：
[https://pecl.php.net/package/solr](https://pecl.php.net/package/solr)
选择版本后面的 dll 文件（我用的是 2.5.1 版本的 dll）
[https://windows.php.net/downloads/pecl/releases/solr/2.5.1/php_solr-2.5.1-7.3-nts-vc15-x64.zip](https://windows.php.net/downloads/pecl/releases/solr/2.5.1/php_solr-2.5.1-7.3-nts-vc15-x64.zip)
并点击下载。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658108004278-e1ac5886-8688-4801-a581-e5ebf234ea61.png#clientId=u7953d6aa-a114-4&from=paste&height=552&id=u01c49dde&originHeight=610&originWidth=1247&originalType=binary∶=1&rotation=0&showTitle=false&size=83774&status=done&style=none&taskId=u3ed7378b-9369-4a50-84ca-29b7e037cd1&title=&width=1129.3585312090652)
下载成功后，解压获取：

1. php_solr.dll
2. php_solr.pdb

将压缩包的 php_solr.dll、php_solr.pdb 放到你的 php 扩展目录下 php/ext/ 下。
php.ini 中加入 extension=php_solr.dll
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658108202536-3d93ee60-5d56-4be8-8aa3-91c280883f42.png#clientId=u7953d6aa-a114-4&from=paste&height=254&id=ua670e885&originHeight=280&originWidth=626&originalType=binary∶=1&rotation=0&showTitle=false&size=42409&status=done&style=none&taskId=u2e04a2ca-59af-471e-ba06-7220993c8c5&title=&width=566.9434166294104)

我的集成环境位置： D:\phpEnv\php\php-7.3\ext
重启服务器，查看 phpinfo()，
是否有显示 solr 扩展加载成功。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658108254208-edbc8d29-1a2f-483b-b39e-44219d65f46d.png#clientId=u7953d6aa-a114-4&from=paste&height=601&id=u5fe2cd3d&originHeight=664&originWidth=1047&originalType=binary∶=1&rotation=0&showTitle=false&size=61684&status=done&style=none&taskId=u449d70c2-4b36-495c-b1b3-6b695283693&title=&width=948.2264492188382)

# 常用操作

**创建 core，-c 指定创建的 core 名**
./solr create -c test_core1 1
**删除 core，-c 指定删除的 core 名**
./solr delete -c test_core1

![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658127465096-e38fc348-6514-4fc7-a764-ce946777deb2.png#clientId=u591e7470-37f3-4&from=paste&id=ua150db6d&originHeight=575&originWidth=1267&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=u7817f55c-446d-4bab-8977-7ab117c7e06&title=)
![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658127470711-73c2ff90-c5ed-438a-b040-e14c99ded58a.png#clientId=u591e7470-37f3-4&from=paste&id=uddf79326&originHeight=228&originWidth=640&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=ua65c7a53-a5a8-4097-a2eb-dcaf0120f7b&title=)
![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658127476667-41a12875-1ffb-4285-bd57-21ce09aa06a6.png#clientId=u591e7470-37f3-4&from=paste&id=u801b6d25&originHeight=304&originWidth=641&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=ud0110b6f-c137-4eb6-87e2-57ab7dc379c&title=)
![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658127481012-dc1b0ebf-6f3e-49c9-bd52-96e2259dd09e.png#clientId=u591e7470-37f3-4&from=paste&id=u17ca993b&originHeight=240&originWidth=620&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=u8344ee63-3135-4bf6-b002-f9544c51bc8&title=)
![](https://cdn.nlark.com/yuque/0/2022/png/27022430/1658127485301-62701194-113b-487c-92be-12289aabb291.png#clientId=u591e7470-37f3-4&from=paste&id=u9b3fd6ae&originHeight=300&originWidth=446&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=uebc5e4ad-238f-4943-b6bc-4c5eb294d5f&title=)

## 二、创建核心

### 2.1 创建核心前准备工作

每个核心都是 solr 的一个实例，一个 solr 服务可以创建多个核心，每个核心都可以进行自己独立配置。

- 1.切换至 solr_home\server\solr 目录，例如：E:\solr-8.1.1\server\solr，在该目录下创建一个文件夹，文件夹名称与核心名称相同。 ![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127788037-3dd84813-0a7f-456b-a0e1-57e0833147f6.webp#clientId=u591e7470-37f3-4&from=paste&id=uc850f99b&originHeight=300&originWidth=814&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=u93f16027-4409-4e7c-ac5d-17010743d21&title=)\_v_images/2019062
- 2.将 E:\solr-8.1.1\server\solr\configsets_default 路径下的 conf 文件夹复制到 new_core 目录（E:\solr-8.1.1\server\solr\new_core）下。

### 2.2 创建核心

#### 方式一：打开 solr 界面，进行如图顺序操作。

这种方式 需要 在 E:\solr-8.1.1\server\solr 目录下创建 new_core 文件夹
![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127788016-424a695c-b554-4066-b999-736c33b8b2c3.webp#clientId=u591e7470-37f3-4&from=paste&id=u72b236cd&originHeight=618&originWidth=1300&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=u7c769d2b-f93a-425b-9906-0a60d499df7&title=)

#### 方式二：bin 目录下输入命令：solr create -c new_core （推荐）

## 三、schema

每个核心 core 中都有这么一个 schema 配置文件。 schema 文件可以对索引库的数据类型进行定义，对字段是否进行索引、存储等进行配置，需要针对狠心单独进行配置。 schema 的数据类型进本够用，如果不能满足需求，比如说对中文分词、拼音分词等，就可以自定义分词器。 Solr8.1.1 的 schema 的配置文件名 为 managed-schema,home\server\solr\new_core\conf\managed-schema（E:\solr-8.1.1\server\solr\core_issuer\conf）。

### 3.1 schema 主要成员

- （1） fieldType：为 field 定义类型，最主要作用是定义分词器，分词器决定着如何从文档中检索关键字。
- （2） analyzer：fieldType 的子元素，是分词器，由 tokenizer 和 filter 组成。例如

<fieldType name="text_tr" class="solr.TextField" positionIncrementGap="100"> <analyzer> <tokenizer class="solr.StandardTokenizerFactory"/> <filter class="solr.TurkishLowerCaseFilterFactory"/> <filter class="solr.StopFilterFactory" words="lang/stopwords_tr.txt" ignoreCase="false"/> <filter class="solr.SnowballPorterFilterFactory" language="Turkish"/> </analyzer> </fieldType> 复制代码

- （3） field：字段，用来创建索引,如果这个字段需要生成索引，则需要设置的 indexed 为 true，需要存储设置 stored 属性为 true。例如：

<field name="age" type="pint" indexed="true" stored="true"/> 复制代码

### 3.2 添加索引字段

- 方式一：直接修改 managed-schema 配置文件（不推荐，修改后需要重启服务），例如：

<field name="age" type="pint" indexed="true" stored="true"/>

- 方式二：通过 solr 页面进行添加。（推荐，不需要重启服务）

![](\_v_images/20190623133627274_22114.png =811x)

### 3.3 配置中文分词工具

- 1.下载中文分词器 IKAnalyzer，[下载地址](https://link.juejin.cn?target=https%3A%2F%2Fpan.baidu.com%2Fs%2F19_nVrOrUrj00rlu13OGOkg)，密码：igt9。
- 2.解压压缩包包，目录如下：

![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127788037-71d0ec37-c053-45ac-a19b-1f4495e4ee58.webp#clientId=u591e7470-37f3-4&from=paste&id=u93455122&originHeight=138&originWidth=518&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=u2185b132-c94c-49dd-af88-fd0f403e627&title=)

- 3.将两个 jar 包复制到该路径下：E:\solr-8.1.1\server\solr-webapp\webapp\WEB-INF\lib。
- 4.另外将三个配置文件复制到该路径下：E:\solr-8.1.1\server\solr-webapp\webapp\WEB-INF\classes。如果没有 classes 文件夹就新建一个。
- 5.在 schema 中添加分词器。

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

- 6.在 ext.dic 文件中添加自定义的中文词组
- 需要重启 solr

效果如下：
![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127788089-a128751b-080b-4cb5-8903-2951345cf1f8.webp#clientId=u591e7470-37f3-4&from=paste&id=u64162da3&originHeight=699&originWidth=1358&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=u40384c2d-3fcf-4d16-8162-bbc5c27fa9c&title=)

## 四、导入索引数据（mysql 数据为例）

- 1.创建 mysql 数据库

![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127788040-b8b6f2fa-b4f0-4559-a787-1a52871abc8c.webp#clientId=u591e7470-37f3-4&from=paste&id=u59084290&originHeight=236&originWidth=692&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=u42bbddfe-6bf1-4bc2-9055-a0ea4b5a550&title=)

- 2.在该路径下 solr_home\server\solr\new_core\conf（E:\solr-8.1.1\server\solr\core_issuer\conf）下新建 my-data-config.xml 文件

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

- 3.用 solr 添加数据库字段对应的索引字段，添加后打开 managed-schema 文件会看到：

```shell
<field name="name" type="string" indexed="true" stored="true"/>
<field name="age" type="pint" indexed="true" stored="true"/>
<field name="hobby" type="string" indexed="true" stored="true"/>

```

**请勿添加 id 字段，该字段已存在，添加会报错**

- 4.打开该路径下文件：solr_home\server\solr\new_core\conf\solrconfig.xml（E:\solr-8.1.1\server\solr\core_issuer\conf\solrconfig.xml），随便找一个与 requestHandler 同级节点上添加以下配置。如图：

![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127789705-05706f41-625e-4027-a4d1-1cb9a19bb621.webp#clientId=u591e7470-37f3-4&from=paste&id=u450b1c77&originHeight=236&originWidth=934&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=u46162f86-3d9e-4302-97bd-de0e839379b&title=)

```shell
<requestHandler name="/dataimport" class="org.apache.solr.handler.dataimport.DataImportHandler">
	<lst name="defaults">
		<str name="config">my-data-config.xml</str>
	</lst>
</requestHandler>


```

- 5.将 solr_home\dist（E:\solr-8.1.1\dist）目录下的 solr-dataimporthandler-8.1.1.jar 和 MySQL 驱动（随便找个 MySQL 驱动）复制到 solr_home\server\solr-webapp\webapp\WEB-INF\lib（E:\solr-8.1.1\server\solr-webapp\webapp\WEB-INF\lib）目录下。
- 6.重启 solr 服务。
- 7.打开 solr 页面，进行下面操作。

![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127789821-cd1c7622-59de-4e9a-b69c-c9ce68fdef58.webp#clientId=u591e7470-37f3-4&from=paste&id=uae005891&originHeight=664&originWidth=1209&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=u2cc0c352-2afe-4061-944a-cee608cb397&title=)

- 8.检测数据是否导入成功

![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127789862-dbbacbb7-e01b-46be-bcd7-80aad8c48a24.webp#clientId=u591e7470-37f3-4&from=paste&id=u6a98f6db&originHeight=704&originWidth=933&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=u0a7e4698-424e-449d-9eaa-47f921414b1&title=)

### 如果对索引字段进行中文分词

添加对索引字段时，fileType ：选择 text_ik
![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127789803-bc546502-4fcb-43cf-a968-b9fda978fe7b.webp#clientId=u591e7470-37f3-4&from=paste&id=u4e01f864&originHeight=721&originWidth=974&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=u6b78e123-0319-4dda-9b2c-1c44ee53a2a&title=)
效果如图： 查询 **鲁西化工** 的结果
![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127789921-c9625e48-e98f-4529-83f4-57a6a48b77fc.webp#clientId=u591e7470-37f3-4&from=paste&id=ub8eb0e9d&originHeight=854&originWidth=1369&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=ua8e921d1-371b-47bf-a129-19bd9fc6cba&title=)

##

## 七、删除索引库（删除所有数据）的两种方法

- 1.删除索引库所有数据的方法一：直接用 solr 可视化界面删除

![](https://cdn.nlark.com/yuque/0/2022/webp/27022430/1658127790457-5b348534-e2ba-4b26-960f-80171f49d3e4.webp#clientId=u591e7470-37f3-4&from=paste&id=u00b14a0e&originHeight=513&originWidth=813&originalType=url∶=1&rotation=0&showTitle=false&status=done&style=none&taskId=u3e1c5470-5f3a-448f-8f88-556681399a8&title=)
