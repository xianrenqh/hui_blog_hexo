---
title: composer.json配置详解
urlname: vqx17at0pd576iea
date: '2023-11-20 08:38:25 +0000'
tags: []
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.cc'
copyright_url:
copyright_author:
cover:
---

**composer.json 是用于管理 PHP 项目 依赖关系的配置文件。下面是一些常见的 composer.json 配置项及其含义：**
name: 项目名称。在发布到 Packagist 上时，这个名称会变成该包的唯一标识符。
description: 项目描述，用于简要说明项目的目的和功能。
type: 项目类型，可以是 "library"（库）、"project"（项目）、"metapackage"（元包）等。
keywords: 一组关键词，用于描述项目的特性、功能或主题，有助于其他开发者更容易找到你的项目。
homepage: 项目的主页，可以是项目的官方网站或仓库地址。
license: 项目的许可证类型，用于指定项目的开源许可证。
authors: 项目的作者信息，包括姓名、电子邮件和网址等。
support: 用于提供支持选项的 URL 和/或电子邮件地址。
require: 项目所依赖的其他 PHP 包的版本要求，可以指定包名和版本号，以确保项目能够正常运行。
require-dev: 与开发相关的依赖关系，通常是开发环境下需要的工具或测试框架等。
conflict: 另一个包与该包存在冲突的版本限制。
replace: 包替换另一个包。
provide: 包提供另一个包的功能。
suggest: 建议安装的附加功能或库。
autoload: 自动加载规则的定义，用于告知 Composer 如何加载 PHP 类和文件。
autoload-dev: 自动加载规则的定义，专门用于开发环境。
scripts: 定义项目中使用的自定义脚本（例如测试、构建等）。

下面是一个简单的 composer.json 配置文件的示例：

```
{
    "name": "my-project",
    "description": "A simple PHP project",
    "type": "project",
    "authors": [
        {
            "name": "yzmcms",
            "email": "yzmcms@qq.com"
        }
    ],
    "require": {
        "php": "^7.3|^8.0",
		"elasticsearch/elasticsearch": "~7.0"
    },
    "autoload": {
        "psr-4": {
            "App\\": "app/"
        }
    },
    "scripts": {
        "test": "phpunit"
    }
}
```

在这个示例中，我们定义了 my-project 作为项目名称，指定了一个简短的描述，并将项目类型设置为 project。我们列出了一个作者并指定了 php 版本 和 elasticsearch/elasticsearch 依赖项，该依赖项用于记录日志。我们还定义了一个名为 autoload 的规则，告诉 Composer 如何自动加载我们的类文件。最后，我们定义了一个 test 脚本，它调用 PHPUnit 测试框架来执行项目的测试套件。

这里我们着重记录下依赖项的版本号部分，版本格式：主版本号.次版本号.修订号，版本号递增规则如下：
主版本号：当你做了不兼容的 API 修改，
次版本号：当你做了向下兼容的功能性新增，
修订号：当你做了向下兼容的问题修正。
先行版本号及版本编译信息可以加到“**主版本号.次版本号.修订号**”的后面，作为延伸。

~表示版本号只能改变最末尾那段（如果是~x.y 末尾就是 y, 如果是~x.y.z 末尾就是 z）
~1.2.3 代表 1.2.3 <= 版本号 < 1.3.0
~1.2 代表 1.2 <= 版本号 < 2.0

^表示除了主版本号以外，次版本号和修订号都可以变
^1.2.3 代表 1.2.3 <= 版本号 <2.0.0

**特殊情况 0 开头的版本号:**
^0.3.0 等于 0.3.0 <= 版本号 <0.4.0 注意: 不是 <1.0.0
因为: semantic versioning 的规定是，主版本号以 0 开头表示这是一个非稳定版本（unstable),如果处于非稳定状态,次版本号是允许不向下兼容的。
**所以如果你要指定 0 开头的库那一定要注意:**
危险写法:~0.1 等于 0.1.0 <= 版本号 <1.0.0
保险写法:^0.1 等于 0.1.0 <= 版本号 <0.2.0

当您完成 composer.json 文件的编写后，可以使用 **composer install** 命令来安装所有依赖项。如果您需要新增或移除依赖库，只需编辑 composer.json 文件并在命令行中运行 **composer update** 命令即可更新。
