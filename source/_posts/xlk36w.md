---
title: webman使用笔记
urlname: xlk36w
date: '2022-04-21 01:15:40 +0000'
tags:
  - webman
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
---

# 安装 webman

> 初始备注：
>
> 不要使用 php8.0 及以上版本（除非不用 think-cache 缓存插件）think-cache 无法友好的支持 php8.0 及以上版本。
>
> ### 目前使用 php7.4

## composer 安装

1、创建项目

```shell
composer create-project workerman/webman
```

2、运行

进入 webman 目录

debug 方式运行(用于开发调试)

```shell
php start.php start
```

daemon 方式运行(用于正式环境)

```shell
php start.php start -d
```

注意  
webman 从 1.2.3 版本开始专门为 windows 系统提供了启动脚本(需要为 php 配置好环境变量)，windows 用户请双击 windows.bat 即可启动 webman，或者运行 php windows.php 启动 webman。

3、访问

浏览器访问 http://ip 地址:8787

# 基础功能

## 需要安装的 composer 扩展（默认以熟悉的 tp 为主）

### 安装 think-template

1、composer 安装

```shell
composer require topthink/think-template
```

2、修改配置 config/view.php 为

```php
<?php
use support\view\ThinkPHP;

return [
    'handler' => ThinkPHP::class,
];
```

> 提示  
> 其它配置选项通过 options 传入，例如

```php
return [
    'handler' => ThinkPHP::class,
    'options' => [
        'view_suffix' => 'html',
        'tpl_begin' => '{',
        'tpl_end' => '}'
    ]
];
```

3、thinkphp 模板的例子

修改配置 config/view.php 为

```php
<?php
use support\view\ThinkPHP;

return [
    'handler' => ThinkPHP::class
];
```

app/controller/User.php 如下

```php
<?php
namespace app\controller;

use support\Request;

class User
{
    public function hello(Request $request)
    {
        return view('user/hello', ['name' => 'webman']);
    }
}
```

文件 app/view/user/hello.html 如下

```php
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <title>webman</title>
</head>
<body>
hello {$name}
</body>
</html>
```

更多文档参考 [think-template](https://www.kancloud.cn/manual/think-template/content)

### 安装 ThinkORM

```shell
composer -W require webman/think-orm
```

经测试实际安装此扩展会报错，所以我们使用手动安装并配置 think-orm。

[webman/think-orm](https://www.workerman.net/plugin/14) 实际上是一个自动化安装 toptink/think-orm 的插件，如果你的 webman 版本低于 1.2 无法使用插件请参考文章[手动安装并配置 think-orm](https://www.workerman.net/a/1289)。

### 安装 ThinkCache

```shell
composer -W require webman/think-cache
```

[webman/think-cache](https://www.workerman.net/plugin/15) 实际上是一个自动化安装 toptink/think-cache 的插件。

暂对 php8.0 及以上不友好，安装后使用会报错。

#### 配置文件

配置文件为 config/thinkcache.php

#### 使用

```php
 <?php
  namespace app\controller;

  use support\Request;
  use think\facade\Cache;

  class User
  {
      public function db(Request $request)
      {
          $key = 'test_key';
          Cache::set($key, rand());
          return response(Cache::get($key));
      }
  }
```

#### Think-Cache 使用文档

[ThinkCache 文档地址](https://github.com/top-think/think-cache)

### 安装 redis 组件

webman 的 redis 组件默认使用的是[illuminate/redis](https://github.com/illuminate/redis)，也就是 laravel 的 redis 库，用法与 laravel 相同。

使用 illuminate/redis 之前必须先给 php-cli 安装 redis 扩展。

> 使用命令 php -m | grep redis 查看 php-cli 是否装了 redis 扩展。注意：即使你在 php-fpm 安装了 redis 扩展，不代表你在 php-cli 可以使用它，因为 php-cli 和 php-fpm 是不同的应用程序，可能使用的是不同的 php.ini 配置。使用命令 php --ini 来查看你的 php-cli 使用的是哪个 php.ini 配置文件。

#### 安装

```shell
composer require -W illuminate/redis
```

````php
## 配置
redis配置文件在`config/redis.php`
```php
return [
    'default' => [
        'host'     => '127.0.0.1',
        'password' => null,
        'port'     => 6379,
        'database' => 0,
    ]
];
````

#### 示例

```php
<?php
namespace app\controller;

use support\Request;
use support\Redis;

class User
{
    public function db(Request $request)
    {
        $key = 'test_key';
        Redis::set($key, rand());
        return response(Redis::get($key));
    }
}
```

#### 具体使用教程：

[https://www.workerman.net/doc/webman/db/redis.html](https://www.workerman.net/doc/webman/db/redis.html)

### 安装 think 验证器

### 安装

```shell
composer require topthink/think-validate
```

> 验证器也不支持 php8.0 及以上，如果需要支持需要自行查找更多方案。

#### 基本用法

```php
<?php
namespace app\index\validate;

use Tinywan\Validate\Validate;

class UserValidate extends Validate
{
    protected $rule =   [
        'name'  => 'require|max:25',
        'age'   => 'require|number|between:1,120',
        'email' => 'require|email'
    ];

    protected $message  =   [
        'name.require' => '名称必须',
        'name.max'     => '名称最多不能超过25个字符',
        'age.require'   => '年龄必须是数字',
        'age.number'   => '年龄必须是数字',
        'age.between'  => '年龄只能在1-120之间',
        'email.require'        => '邮箱必须是数字',
        'email.email'        => '邮箱格式错误'
    ];
}
```

<font style="color:rgb(33, 37, 41);">验证器调用代码如下：</font>

```php
$data = [
    'name'  => 'Tinywan',
    'age'  => 24,
    'email' => 'Tinywan@163.com'
];
$validate = new \app\index\validate\UserValidate;

if (!$validate->check($data)) {
    var_dump($validate->getError());
}
```

<font style="color:rgb(33, 37, 41);">更多用法可以参考 6.0 完全开发手册的</font>[验证](https://www.kancloud.cn/manual/thinkphp6_0/1037623)<font style="color:rgb(33, 37, 41);">章节</font>

<font style="color:rgb(33, 37, 41);"></font>

### 安装验证码

#### gregwar/captcha

项目地址 [https://github.com/Gregwar/Captcha](https://github.com/Gregwar/Captcha)

#### 安装

```shell
composer require gregwar/captcha 1.*
```

#### 基本使用 1

**建立文件 app/controller/Login.php**

```php
<?php
namespace app\controller;

use support\Request;
use Gregwar\Captcha\CaptchaBuilder;

class Login
{
    /**
     * 测试页面
     */
    public function index(Request $request)
    {
        return view('login/index');
    }

    /**
     * 输出验证码图像
     */
    public function captcha(Request $request)
    {
        // 初始化验证码类
        $builder = new CaptchaBuilder;
        // 生成验证码
        $builder->build();
        // 将验证码的值存储到session中
        $request->session()->set('captcha', strtolower($builder->getPhrase()));
        // 获得验证码图片二进制数据
        $img_content = $builder->get();
        // 输出验证码二进制数据
        return response($img_content, 200, ['Content-Type' => 'image/jpeg']);
    }

    /**
     * 检查验证码
     */
    public function check(Request $request)
    {
        // 获取post请求中的captcha字段
        $captcha = $request->post('captcha');
        // 对比session中的captcha值
        if (strtolower($captcha) !== $request->session()->get('captcha')) {
            return json(['code' => 400, 'msg' => '输入的验证码不正确']);
        }
        return json(['code' => 0, 'msg' => 'ok']);
    }

}
```

**<font style="color:rgb(44, 62, 80) !important;">建立模版文件</font>\*\***<font style="color:rgb(0, 0, 0);">app/view/login/index.html</font>\*\*

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>验证码测试</title>
  </head>
  <body>
    <form method="post" action="/login/check">
      <img src="/login/captcha" /><br />
      <input type="text" name="captcha" />
      <input type="submit" value="提交" />
    </form>
  </body>
</html>
```

#### 基本使用 2 （直接在控制器里使用并验证）

```php

$this->validate = new \think\Validate();
$param = $request->all();

$this->validate->rule([
    'captcha|验证码'   => 'require',
    'uniqid|uniqid' => 'require',
    'username|用户名'  => 'require',
    'password|密码'   => 'require',
]);
$this->validate->message([
    'captcha.require'  => '验证码不能为空',
    'uniqid.require'   => 'uniqid不能为空',
    'username.require' => '登录名不能为空',
    'password.require' => '密码不能为空',
]);
if ( ! $this->validate->check($param)) {
    return json($this->validate->getError());
}
```

###

### <font style="color:rgb(33, 37, 41);">webman/casbin 权限控制插件</font>

#### 简介

<font style="color:rgb(33, 37, 41);">webman casbin 权限控制插件。它基于 </font>[PHP-Casbin](https://github.com/php-casbin/php-casbin)<font style="color:rgb(33, 37, 41);">, 一个强大的、高效的开源访问控制框架，支持基于</font><font style="color:rgb(0, 0, 0);">ACL</font><font style="color:rgb(33, 37, 41);">, </font><font style="color:rgb(0, 0, 0);">RBAC</font><font style="color:rgb(33, 37, 41);">, </font><font style="color:rgb(0, 0, 0);">ABAC</font><font style="color:rgb(33, 37, 41);">等访问控制模型。</font>

#### 依赖

- [ThinkORM](https://www.workerman.net/doc/webman/db/others.html)<font style="color:rgb(33, 37, 41);">（默认）</font>
- [PHP-DI](https://github.com/PHP-DI/PHP-DI)
- [illuminate/database](https://www.workerman.net/doc/webman/db/tutorial.html)<font style="color:rgb(33, 37, 41);">（可选）</font>

<font style="color:rgb(33, 37, 41);"></font>

#### 安装

```shell
composer require casbin/webman-permission
```

#### 使用

1. 依赖注入配置  
   修改配置 config/container.php，其最终内容如下：

```php
$builder = new \DI\ContainerBuilder();
$builder->addDefinitions(config('dependence', []));
$builder->useAutowiring(true);
return $builder->build();
```

2. 数据库配置

> 默认策略存储是使用的 ThinkORM。 如使用 laravel 的数据库 illuminate/database，请按照官方文档按照相应的依赖包：[https://www.workerman.net/doc/webman/db/tutorial.html](https://www.workerman.net/doc/webman/db/tutorial.html)

🚀 (1) 模型配置  
📒📒📒 使用 ThinkORM（默认） 📒📒📒

修改数据库 thinkorm.php 配置  
📕📕📕 使用 laravel 数据库（可选） 📕📕📕

修改数据库 database.php 配置  
修改数据库 permission.php 的 adapter 适配器为 laravel 适配器

(2) 创建 casbin_rule 数据表

```sql
CREATE TABLE `casbin_rule` (
	`id` BIGINT ( 20 ) UNSIGNED NOT NULL AUTO_INCREMENT,
	`ptype` VARCHAR ( 128 ) NOT NULL DEFAULT '',
	`v0` VARCHAR ( 128 ) NOT NULL DEFAULT '',
	`v1` VARCHAR ( 128 ) NOT NULL DEFAULT '',
	`v2` VARCHAR ( 128 ) NOT NULL DEFAULT '',
	`v3` VARCHAR ( 128 ) NOT NULL DEFAULT '',
	`v4` VARCHAR ( 128 ) NOT NULL DEFAULT '',
	`v5` VARCHAR ( 128 ) NOT NULL DEFAULT '',
	PRIMARY KEY ( `id` ) USING BTREE,
	KEY `idx_ptype` ( `ptype` ) USING BTREE,
	KEY `idx_v0` ( `v0` ) USING BTREE,
	KEY `idx_v1` ( `v1` ) USING BTREE,
	KEY `idx_v2` ( `v2` ) USING BTREE,
	KEY `idx_v3` ( `v3` ) USING BTREE,
	KEY `idx_v4` ( `v4` ) USING BTREE,
    KEY `idx_v5` ( `v5` ) USING BTREE
) ENGINE = INNODB CHARSET = utf8mb4 COMMENT = '策略规则表';
```

(3) 配置 config/redis 配置

重启 webman

```shell
php start.php restart
```

或者

```shell
php start.php restart -d
```

#### 用法

安装成功后，可以这样使用:

```php
use Tinywan\Casbin\Permission;

// adds permissions to a user
Permission::addPermissionForUser('eve', 'articles', 'read');
// adds a role for a user.
Permission::addRoleForUser('eve', 'writer');
// adds permissions to a rule
Permission::addPolicy('writer', 'articles','edit');
```

你可以检查一个用户是否拥有某个权限:

```php
if (Permission::enforce("eve", "articles", "edit")) {
    echo '恭喜你！通过权限认证';
} else {
    echo '对不起，您没有该资源访问权限';
}
```

#### 具体教程：

[https://github.com/php-casbin/webman-permission](https://github.com/php-casbin/webman-permission)

更多 API 参考 [Casbin API](https://casbin.org/docs/en/management-api) 。

### 路由自动解析

<font style="color:rgb(33, 37, 41);">当 app 目录结构非常复杂，webman 无法自动解析时可以安装 webman 的</font>[自动路由插件](https://www.workerman.net/plugin/17)<font style="color:rgb(33, 37, 41);">，它会自动检索所有的控制器并为其自动配置对应的路由，让其通过 url 可以访问。</font>

#### 安装

```shell
composer require webman/auto-route
```

> **<font style="color:rgb(44, 62, 80);background-color:rgb(243, 249, 255);">提示</font>**  
> <font style="color:rgb(44, 62, 80);background-color:rgb(243, 249, 255);">你仍然可以在</font><font style="color:rgb(0, 0, 0);">config/route.php</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 249, 255);">中手动设置某些路由，自动路由插件会优先使用</font><font style="color:rgb(0, 0, 0);">config/route.php</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 249, 255);"> 里的配置。</font>
