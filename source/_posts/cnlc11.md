---
title: elasticsearch8.x使用笔记
urlname: cnlc11
date: '2022-09-16 06:09:20 +0000'
tags:
  - elasticsearch
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
copyright_url:
copyright_author:
cover:
---

> [ElasticSearch](https://www.elastic.co/cn/enterprise-search/)是一个基于 Lucene 的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于 RESTful web 接口。Elasticsearch 是用 Java 开发的，并作为 Apache 许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。

在做搜索的时候想到了 ElasticSearch ，而且其也支持 PHP，所以就做了一个简单的例子做测试，感觉还不错，做下记录。

# 1、环境

1. php 8.0
2. elasticsearch 8.4.1 [下载](https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.4.1-windows-x86_64.zip)
3. thinkphp6.0
4. php elasticsearch 8.4

# 2、安装【windows】

1、下载 elasticsearch-8.4.1-windows-x86_64 包，并解压
2、ES 自带 jdk 的，所以可以不用安装 java 组件
3、打开 bin 文件夹，直接双击 elasticsearch.bat 运行
4、访问：http://127.0.0.1:9200/，如果出现如下数据，那启动正常
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1663309015056-cf880cb3-6a8b-4da8-aeb4-1920932fcec4.png#clientId=ueeadb8ff-7c2c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=406&id=u405df5a0&margin=%5Bobject%20Object%5D&name=image.png&originHeight=448&originWidth=757&originalType=binary∶=1&rotation=0&showTitle=false&size=14886&status=done&style=none&taskId=u7cbbb0cd-7e96-4319-89b8-1b83b631a46&title=&width=685.5849303330091)

# 3、安装【Linux】

暂无测试

# 4、SSL 安全

暂无

# 5、数据界面工具

## 1、kibana【官方】

Kibana 是为 Elasticsearch 设计的开源分析和可视化平台。你可以使用 Kibana 来搜索，查看存储在 Elasticsearch 索引中的数据并与之交互。你可以很容易实现高级的数据分析和可视化，以图表的形式展现出来。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1663310319574-4f3c7fed-0696-4c7a-914c-8fcc6c69bbc4.png#clientId=ueeadb8ff-7c2c-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u732fbabb&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1536&originWidth=2864&originalType=url∶=1&rotation=0&showTitle=false&size=576152&status=done&style=none&taskId=u5766e684-ce19-4424-8889-d9863a09ac8&title=)
安装参考：[https://www.cnblogs.com/chenqionghe/p/12503181.html](https://www.cnblogs.com/chenqionghe/p/12503181.html)

## 2、elasticsearch-head 插件

> elasticsearch-head 是 Web 前端，用于浏览和与 Elastic Search 集群进行交互，可用于集群管理、数据可视化、增删改查工具 Elasticsearch 语句可视化等。

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1663309790115-5a0c385b-f905-42e9-8942-68c84db9555c.png#clientId=ueeadb8ff-7c2c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=497&id=u0679ac24&margin=%5Bobject%20Object%5D&name=image.png&originHeight=549&originWidth=997&originalType=binary∶=1&rotation=0&showTitle=false&size=53958&status=done&style=none&taskId=u3eea04e0-5ed1-4c03-a145-e41113998dc&title=&width=902.9434287212814)

elasticsearch-head github 源码地址：
[https://github.com/mobz/elasticsearch-head](https://github.com/mobz/elasticsearch-head)

### 插件的安装【Linux】

下载 elasticsearch-head 插件
进入 elasticsearch-head 源码目录中，执行 npm install
在运行 npm install 时，可能会存在 Head 插件 phantomjs 权限问题

```shell
npm WARN deprecated phantomjs-prebuilt@2.1.16: this package is now deprecated
npm WARN deprecated request@2.88.2: request has been deprecated, see https://github.com/request/request/issues/3142
npm WARN deprecated har-validator@5.1.5: this
npm WARN deprecated fsevents@1.2.13: fsevents 1 will break on node v14+ and could be using insecure binaries. Upgrade to fsevents 2.

> phantomjs-prebuilt@2.1.16 install E:\JavaDevelopment\elasticsearch\elasticsearch-head-master\node_modules\phantomjs-prebuilt
> node install.js

PhantomJS not found on PATH
Downloading https://github.com/Medium/phantomjs/releases/download/v2.1.1/phantomjs-2.1.1-windows.zip
Saving to C:\Users\ysxx\AppData\Local\Temp\phantomjs\phantomjs-2.1.1-windows.zip
Receiving...

Error making request.
Error: connect ETIMEDOUT 140.82.114.4:443
    at TCPConnectWrap.afterConnect [as oncomplete] (net.js:1107:14)

Please report this full log at https://github.com/Medium/phantomjs
npm WARN elasticsearch-head@0.0.0 license should be a valid SPDX license expression
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.13 (node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.13: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})

npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! phantomjs-prebuilt@2.1.16 install: `node install.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the phantomjs-prebuilt@2.1.16 install script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\ysxx\AppData\Roaming\npm-cache\_logs\2020-12-28T01_28_03_195Z-debug.log
```

**解决方案：在 npm install 命令后加 -g 参数**

```shell
npm install -g
```

**在 install 成功之后，执行 npm run start 启动 head 插件**
在 cmd 命令行可以看到 web server 地址，在浏览器访问该地址 http://localhost:9100，如果出现 elasticsearch-head 插件连不上 elasticsearch 服务，需要在 elasticsearch 安装目录下的 config 文件夹，找到 elasticsearch.yml 文件，添加两行配置：

```shell
#表示是否支持跨域，默认为false
http.cors.enabled: true
#当设置允许跨域，默认为*,表示支持所有域名
http.cors.allow-origin: "*"
```

### 插件的安装【window 浏览器】

打开谷歌浏览器应用插件网址：
[https://chrome.google.com/webstore/category/extensions?hl=zh-CN](https://chrome.google.com/webstore/category/extensions?hl=zh-CN)
搜索：elasticsearch-head
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1663309964980-79cba546-1a82-4d28-8be4-b3602b1f70b9.png#clientId=ueeadb8ff-7c2c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=638&id=u5fb66667&margin=%5Bobject%20Object%5D&name=image.png&originHeight=704&originWidth=1367&originalType=binary∶=1&rotation=0&showTitle=false&size=57811&status=done&style=none&taskId=u1d042b99-7935-40d3-80e6-75b949bf195&title=&width=1238.0377804032012)
直接进入后点击下载【安装】，即可在浏览器上安装成功此插件。
直接打开插件即可。

# 6、php 系统安装 ES

> 默认程序使用 thinkphp6.0，安装方式使用 composer

在命令行执行：

```shell
composer require elasticsearch/elasticsearch
```

正确等待响应后会安装成功。

# 7、php-ES 的使用

引入类库：

> 注意，这里和 7.x 及之前的版本不同。

```php
use Elastic\Elasticsearch\ClientBuilder;

$client = ClientBuilder::create()->setHosts(['127.0.0.1:9200'])->build();
```

**完整类库：**

```php
<?php
/**
 * Created by PhpStorm.
 * User: 小灰灰
 * Date: 2022-09-16
 * Time: 11:05:20
 * Info:
 */

namespace app\portal\service;

use Elastic\Elasticsearch\ClientBuilder;

class ElasticsearchService
{

    protected $client;

    protected $index_name;

    protected $type;

    // 构造函数
    public function __construct($indexName = 'article_index')
    {
        $this->index_name = $indexName;
        $params           = ['127.0.0.1:9200'];
        $this->client     = ClientBuilder::create()->setHosts($params)->build();
    }

    /**
     * 判断索引是否存在
     * @return bool
     */
    public function exist_index()
    {
        $params = ['index' => $this->index_name];

        return $this->client->indices()->exists($params)->asBool();
    }

    /**
     * 创建索引
     * 同一索引只能创建一次
     * @return bool
     */
    public function create_index()
    {
        $res = $this->exist_index();
        if ( ! $res) {
            $param = [
                'index' => $this->index_name,
                'body'  => []
            ];

            return $this->client->indices()->create($param)->asBool();
        }
    }

    /**
     * 获取索引结构
     * @return array
     */
    public function get_index()
    {
        $params = ['index' => $this->index_name];

        try {
            return $this->client->indices()->get($params)->asArray();
        } catch (\Exception $e) {
            return [];
        }
    }

    /**
     * 删除索引
     * @return array
     */
    public function delete_index()
    {
        $params = ['index' => $this->index_name];

        try {
            return $this->client->indices()->delete($params)->asBool();
        } catch (\Exception $e) {
            return false;
        }
    }

    /*
     * 将内容加入文档
     */
    public function add_article($data)
    {
        $params = [
            'body'  => $data,
            'id'    => 'article_'.$data['id'],
            'index' => $this->index_name,
        ];
        $this->client->index($params)->asArray();
    }

    /**
     * 删除一个文档
     */
    public function delete_article($id)
    {
        $params = [
            'index' => $this->index_name,
            'id'    => $id
        ];

        try {
            return $this->client->delete($params)->asBool();
        } catch (\Exception $e) {
            return false;
        }
    }

    /**
     * 查询文档
     */
    public function find_article()
    {
        $params = [
            'index' => 'articles_index',
            'body'  => [
                'query'     => [
                    'match' => ['post_content' => '内容']
                ],
                'size'      => 10,  //每页数量
                'from'      => 0,    //从第几条数据开始
                'highlight' => [  //高亮
                    'fields'                  => [
                        'post_content' => [
                            "pre_tags"  => "<b style='color:red'>",
                            "post_tags" => "</b>"
                        ]
                    ],
                    "boundary_scanner_locale" => "zh_CN",
                    'encoder'                 => 'HTML',
                    'number_of_fragments'     => 0
                ]
            ]
        ];

        return $this->client->search($params)->asArray();
    }

}

```

# 补充：

## 高亮

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1663314887045-ef45f493-be27-4510-a3f0-f66cf20e896a.png#clientId=ueeadb8ff-7c2c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=791&id=u5ec28e31&margin=%5Bobject%20Object%5D&name=image.png&originHeight=873&originWidth=1549&originalType=binary∶=1&rotation=0&showTitle=false&size=34491&status=done&style=none&taskId=ub5fad4ff-be34-457f-9c19-e3bbf826f41&title=&width=1402.8679750143078)

### 高亮参数

| **参数**                | **说明**                                                                                                                                                                                                                                                             |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| boundary_chars          | 包含每个边界字符的字符串。默认为,! ?\\ \\ n。                                                                                                                                                                                                                        |
| boundary_max_scan       | 扫描边界字符的距离。默认为 20。                                                                                                                                                                                                                                      |
| boundary_scanner        | 指定如何分割突出显示的片段，支持 chars, sentence, or word 三种方式。                                                                                                                                                                                                 |
| boundary_scanner_locale | 用来设置搜索和确定单词边界的本地化设置，此参数使用语言标记的形式（“en-US”, “fr-FR”, “ja-JP”）                                                                                                                                                                        |
| encoder                 | 表示代码段应该是 HTML 编码的:默认(无编码)还是 HTML (HTML-转义代码段文本，然后插入高亮标记)                                                                                                                                                                           |
| fields                  | 指定检索高亮显示的字段。可以使用通配符来指定字段。例如，可以指定 comment*\*来获取以 comment*开头的所有文本和关键字字段的高亮显示。                                                                                                                                   |
| force_source            | 根据源高亮显示。默认值为 false。                                                                                                                                                                                                                                     |
| fragmenter              | 指定文本应如何在突出显示片段中拆分:支持参数 simple 或者 span。                                                                                                                                                                                                       |
| fragment_offset         | 控制要开始突出显示的空白。仅在使用 fvh highlighter 时有效。                                                                                                                                                                                                          |
| fragment_size           | 字符中突出显示的片段的大小。默认为 100。                                                                                                                                                                                                                             |
| highlight_query         | 突出显示搜索查询之外的其他查询的匹配项。这在使用重打分查询时特别有用，因为默认情况下高亮显示不会考虑这些问题。                                                                                                                                                       |
| matched_fields          | 组合多个匹配结果以突出显示单个字段，对于使用不同方式分析同一字符串的多字段。所有的 matched_fields 必须将 term_vector 设置为 with_positions_offsets，但是只有将匹配项组合到的字段才会被加载，因此只有将 store 设置为 yes 才能使该字段受益。只适用于 fvh highlighter。 |
| no_match_size           | 如果没有要突出显示的匹配片段，则希望从字段开头返回的文本量。默认为 0(不返回任何内容)。                                                                                                                                                                               |
| number_of_fragments     | 返回的片段的最大数量。如果片段的数量设置为 0，则不会返回任何片段。相反，突出显示并返回整个字段内容。当需要突出显示短文本(如标题或地址)，但不需要分段时，使用此配置非常方便。如果 number_of_fragments 为 0，则忽略 fragment_size。默认为 5。                          |
| order                   | 设置为 score 时，按分数对突出显示的片段进行排序。默认情况下，片段将按照它们在字段中出现的顺序输出(order:none)。将此选项设置为 score 将首先输出最相关的片段。每个高亮应用自己的逻辑来计算相关性得分。                                                                 |
| phrase_limit            | 控制文档中所考虑的匹配短语的数量。防止 fvh highlighter 分析太多的短语和消耗太多的内存。提高限制会增加查询时间并消耗更多内存。默认为 256。                                                                                                                            |
| pre_tags                | 与 post_tags 一起使用，定义用于突出显示文本的 HTML 标记。默认情况下，突出显示的文本被包装在和标记中。指定为字符串数组。                                                                                                                                              |
| post_tags               | 与 pre_tags 一起使用，定义用于突出显示文本的 HTML 标记。默认情况下，突出显示的文本被包装在和标记中。指定为字符串数组。                                                                                                                                               |
| require_field_match     | 默认情况下，只突出显示包含查询匹配的字段。将 require_field_match 设置为 false 以突出显示所有字段。默认值为 true。                                                                                                                                                    |
| tags_schema             | 设置为使用内置标记模式的样式。                                                                                                                                                                                                                                       |
| type                    | 使用的高亮模式:unified, plain, or fvh. 默认为 unified。                                                                                                                                                                                                              |

### boundary_scanner

指定如何分割突出显示的片段，支持 chars, sentence, or word 三种方式。默认 unified highlighter 使用
sentence，fvh highlighter 使用 chars。

- chars

使用 boundary_chars 指定的字符突出显示边界。boundary_max_scan 设置控制扫描边界字符的距离。只适用于 fvh highlighter。

- sentence

根据 Java 的 BreakIterator 确定，在下一个句子边界处中断突出显示的片段。配置了此参数后上面例子返回内容,可以看到其每个分段都是一个句子

```shell
"highlight" : {
  "content" : [
	"<em>简</em><em>单</em><em>查</em><em>询</em>字符串——URI Search 实际业务中我们通常都是使用一个完整<em>的</em>请求体从elasticsearch获得数据。",
	"然而在有些时候我们为了调试系统或者数据<em>的</em>时候需要临时进行一些<em>简</em><em>单</em><em>的</em><em>查</em><em>询</em>时候，编写完整<em>的</em>请求体JSON可能会稍微麻烦。而elasticsearch提供了一种基于URI<em>的</em><em>简</em><em>单</em><em>的</em><em>查</em><em>询</em>方"
  ]
}

```

- word

根据 Java 的 BreakIterator 确定，在下一个单词边界处中断突出显示的片段。使用此参数上面的例子返回内容，此时每个分段并不是一个完整的句子。

```shell
"highlight" : {
  "content" : [
	"<em>简</em><em>单</em><em>查</em><em>询</em>字符串",
	"实际业务中我们通常都是使用一个完整<em>的</em>请求体从",
	"然而在有些时候我们为了调试系统或者数据<em>的</em>时候需要临时进行一些<em>简</em><em>单</em><em>的</em><em>查</em><em>询</em>时候",
	"编写完整<em>的</em>请求体",
	"<em>的</em><em>简</em><em>单</em><em>的</em><em>查</em><em>询</em>方"
  ]
}

```

### boundary_scanner_locale

用来设置搜索和确定单词边界的本地化设置

1. 可以通过https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#forLanguageTag-java.lang.String-获得更多语言标签
2. 默认参数为[Locale.ROOT]{https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html#ROOT}

### fragmenter

指定文本应如何在突出显示片段中拆分。只适用于 plain highlighter。默认为 span。

1. simple:将文本分割成相同大小的片段
2. span:将文本分割为大小相同的片段，但尽量避免在突出显示的术语之间分割文本。这在查询短语时很有用

### type

type 字段允许强制使用特定的高亮策略。可以配置的参数:unified, plain and fvh。下面是一个使用 plain 策略的例子:

```shell
GET article/_search
{
    "query" : {
        "match": { "content": "简单的查询" }
    },
    "highlight" : {
        "fields" : {
            "content" : {"type" : "plain"}
        }
    }
}

```

三种不同的高亮策略的区别

1. unified（通用高亮策略）

其使用的是 Lucene 的 Unified Highlighter。此高亮策略将文本分解成句子，并使用 BM25 算法对单个句子进行评分，支持精确的短语和多术语(模糊、前缀、正则表达式)突出显示。这个是默认的高亮策略。

1. plain （普通高亮策略）

其使用的是 Lucene 的 standard Lucene highlighter。它试图在理解词的重要性和短语查询中的任何词定位标准方面反映查询匹配逻辑。此高亮策略是和在单个字段中突出显示简单的查询匹配。如果想用复杂的查询在很多文档中突出显示很多字段，还是使用 unified

1. Fast vector highlighter（快速向量策略）

其使用的是 Lucene 的 Fast Vector highlighter。使用此策略需要在映射中将对应字段中属性 term_vector 设置为 with_positions_offsets。这个策略以后会单独介绍。

### pre_tags 和 post_tags

默认情况下，高亮显示将以和包装突出显示的文本。这可以通过设置 pre_tags 和 post_tags 来控制，例如:

```shell
GET article/_search
{
    "size":5,
    "query": {
        "match": {
            "content": "简单的查询"
        }
    },
    "highlight": {
        "boundary_scanner_locale":"zh_CN",
        "fields": {
            "content": {
               "pre_tags": [
                    "<h1>"
                ],
                "post_tags": [
                    "</h2>"
                ]
            }
        }
    }
}

```

当然也可以使用系统预设的标签模式:

```shell
GET article/_search
{
    "size":5,
    "query": {
        "match": {
            "content": "简单的查询"
        }
    },
    "highlight": {
        "boundary_scanner_locale":"zh_CN",
        "tags_schema" : "styled",
        "fields": {
            "content": {

            }
        }
    }
}

```

### fragment_size 和 number_of_fragments

fragment_size 主要控制了每个高亮片段的大小，而 number_of_fragments 控制了返回多少个高亮片段。下面例子中就是控制返回两个长度 10 的片段

```shell
GET article/_search
{
    "query": {
        "bool": {
            "must": [
                {
                    "match": {
                        "content": "简单的查询"
                    }
                }
            ]
        }
    },
    "highlight": {
        "boundary_scanner_locale": "zh_CN",
        "fields": {
            "content": {
              "fragment_size" : 10,
              "number_of_fragments" : 2
            }
        }
    }
}

```

### order

order 控制了返回对象中 highlight 片段的排序。下面例子中返回的高亮片段将会根据分数顺序输出。假如设置了 none 则是按照顺序输出。

```shell
GET article/_search
{
    "query": {
        "bool": {
            "must": [
                {
                    "match": {
                        "content": "简单的查询"
                    }
                }
            ]
        }
    },
    "highlight": {
        "order" : "score",
        "boundary_scanner_locale": "zh_CN",
        "fields": {
            "content": {}
        }
    }
}

```

对一个字段进行查询的时候希望高亮其他字段
在请求参数 highlight 中可以设置多个字段，同时需要为每个字段配置 highlight_query。通过配置这个字段可以实现对 content 进行匹配查询同时高亮 article_title 和 content 字段的内容。同时使用此方法可以实现使用使用查询条件查询,而最终高亮字符串内容。

```shell
GET article/_search
{
    "query": {
        "match": {
            "content": "查询"
        }
    },
    "highlight": {
        "fields": {
            "content": {
                "type": "unified",
                "highlight_query": {
                    "bool": {
                        "must": {
                            "match": {
                                "content": {
                                    "query": "字符串"
                                }
                            }
                        },
                        "minimum_should_match": 0
                    }
                }
            },
            "article_title": {
                "type": "unified",
                "highlight_query": {
                    "bool": {
                        "must": {
                            "match": {
                                "article_title": {
                                    "query": "字符串"
                                }
                            }
                        },
                        "minimum_should_match": 0
                    }
                }
            }
        }
    }
}

```

## highlighter 如何确定高亮内容

为了从查询的词汇中获得搜索片段位置，高亮策略显示需要知道原始文本中每个单词的起始和结束字符偏移量。目前根据模式不同获取这些数据途径不同

1. 检索列表，如果在映射中 index_options 设置了 offsets，unified 会将其中数据应用在文档中，而不会重新分析文本。它直接对文档进行原始查询，并从索引中提取匹配的偏移数据。在字段内容很大的时候，使用此配置很重要，因为它不需要重新分析文本内容。和 term_vectors 相比，它还需要更少的磁盘空间。
2. 术语向量，如果在映射中 term_vector 设置为 with_positions_offsets 则 unified highlighter 使用 term_vector 来突出显示字段。对于大字段（大于 1MB）和多术语查询它的速度会比较快。而 fvh highlighter 总是使用 term_vector。
3. 普通的高亮策略（
   Plain highlighting），当没有其他选择的时候，unified highlighter 使用此模式，他在内存中创建一个小的索引（index），通过运行 Lucene 的查询执行计划来访问文档的匹配信息，对需要高亮显示的每个字段和每个文档进行处理。plain highlighter 总是使用此策略。注意此方式在大型文本上可能需要大量的时间和内存。在使用此策略时候可以设置分析的文本字符的最大数量限制为 1000000。这个数值可以通过修改索引的 index.highlight.max_analyzed_offset 参数来改变。
