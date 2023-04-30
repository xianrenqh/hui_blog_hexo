---
title: PHP Faker 教程
urlname: nkr3gz
date: '2022-04-15 09:07:00 +0000'
tags:
  - php
  - faker
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
---

# 什么是 PHP Faker

Faker 是一个生成假数据的 PHP 库，Faka 数据通常用于测试或用一些伪数据填充数据库，Faker 受到 Perl 的 Data :: Faker 和 Ruby 的 Faker 的极大启发。
说白了就是：

> 我们在创建完数据表格后往往需要做一些假数据，而 Faker 就是这样的工具。安装 Faker

# 安装 Faker

在项目所在的文件夹中打开命令行输入以下命令：

```shell
composer require fzaninotto/faker
```

运行以后会在 vendor 文件夹下生成：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/27022430/1650013870646-a6ba9653-b5ac-4919-b345-5745aed19374.png#clientId=u7ddbe57f-a192-4&from=paste&id=u7c034536&originHeight=184&originWidth=507&originalType=url∶=1&rotation=0&showTitle=false&size=12116&status=done&style=none&taskId=ud741bed8-b7a5-470a-8cfb-f31e12aff33&title=)

# 实际使用

在项目中的实际使用方法（以 Thinkphp 为例）：

```php
<?php
namespace MyFaker;
//use会调用类注册, 前提是你得先导入相应的类注册方法 (autoload.php)
use Faker\Factory;
class FakerData
{
    //使用faker生成假数据
    public static function createInfo()
    {
        $faker = Factory::create('zh_CN');//选择中文
        $data = [
            $faker->name,//随机姓名
            $faker->address,//随机地址
            $faker->email,//随机邮箱
            $faker->numberBetween(20,60),//年龄随机在20-60之间
            $faker->randomElement(['农民','工人','程序员'])//随机职业
        ];
        //调试工具
         dump($data);
    }
    public function insertInfo(){
      $faker = Factory::create('zh_CN');
        $data = [];
        //创建数据
        for ($i = 0; $i < 3; $i++){
            $data[$i]['title'] = $faker->name();
            $data[$i]['version'] = '1.0';
            $data[$i]['status'] = $faker->numberBetween(0,1);
            $data[$i]['user_id'] = 1;
            $data[$i]['description'] = $faker->realText(10, 2);
            $data[$i]['coll_num'] = 0;
            $data[$i]['create_time'] = $faker->date('Y-m-d H:i:s','now');
            $data[$i]['update_time'] = $faker->date('Y-m-d H:i:s','now');
            $data[$i]['appid'] = $faker->md5();
            $data[$i]['appsecret'] = $faker->md5();
            $data[$i]['code'] = $faker->md5();
        }
        $table = $this->table('hbb_item');
        $table->insert($data)->save();
    }

}

```

createInfo 输出：

> array:5 [
> > 0 => "景畅"
> > 1 => "重庆白云区"
> > 2 => "adipisci.dignissimos@sohu.com"
> > 3 => 55
> > 4 => "工人"
> > ]

# github 地址

[https://github.com/fzaninotto/Faker](https://github.com/fzaninotto/Faker)
