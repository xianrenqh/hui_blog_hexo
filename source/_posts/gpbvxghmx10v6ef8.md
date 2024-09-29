---
title: tp6+使用cli(cmd)命令行模式访问控制器
urlname: gpbvxghmx10v6ef8
date: '2024-09-27 07:53:06 +0000'
tags:
  - thinphp
  - cli
  - 命令行
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.cc'
'<font style="color:rgb(38, 38, 38);">copyright_url': >-
  </font>https://blog.csdn.net/IT_guoguo/article/details/124491909<font
  style="color:rgb(38, 38, 38);">
'</font><font style="color:rgb(38, 38, 38);">copyright_author': </font>山上小和尚
'<font style="color:rgb(33, 37, 41);">cover': </font>
---

> <font style="color:rgb(51, 51, 51);"> 因为 thinkphp6 简称 tp6,默认不支持直接 cli 命令行模式访问控制器,于是利用官方的 command 实现了访问控制器.</font>

<font style="color:rgb(51, 51, 51);">实现效果如下 ：</font>

<font style="color:rgb(51, 51, 51);">php think action 模块/控制器/方法</font>

<font style="color:rgb(51, 51, 51);"></font>

## <font style="color:rgb(51, 51, 51);">1、新建 command，Action：</font>

> 在 app\command 创建文件：Action.php

```php
<?php
declare (strict_types=1);

namespace app\command;

use think\console\Command;
use think\console\Input;
use think\console\input\Argument;
use think\console\input\Option;
use think\console\Output;
use think\Exception;

class Action extends Command
{

    protected function configure()
    {
        $this->setName("action")->addArgument("route", Argument::OPTIONAL, "your run route path!")//路由地址必须输入
        ->addOption('option', 'o', Option::VALUE_REQUIRED,
            'set Controller Argument') // 额外选项 用来传递方法参数 --option name=yourname,password=yourpass
        ->setDescription("Command run Controller Action !");
    }

    protected function execute(Input $input, Output $output)
    {
        try {
            $Argument = $input->getArguments();
            if ($Argument['command'] == 'action') {
                if ($input->hasOption('option')) {
                    $params         = $this->option(trim($input->getOption('option')) ?? trim($input->getOption('option')));
                    $class_fun      = $this->route($Argument['route']);
                    $fun            = $class_fun['fun'];
                    $namespaceClass = "{$class_fun['app']}\\{$class_fun['module']}\\{$class_fun['controller']}\\{$class_fun['class']}";
                    $classObj       = app($namespaceClass);
                    $result         = call_user_func_array([$classObj, $fun], $params); // $classObj->$fun($params);
                    $output->writeln($result);
                } else {
                    $class_fun = $this->route($Argument['route']);
                    //throw new Exception("类".$class_fun['class']);
                    //throw new Exception("方法".$class_fun['fun']);
                    $namespaceClass = "{$class_fun['app']}\\{$class_fun['module']}\\{$class_fun['controller']}\\{$class_fun['class']}";
                    $fun            = $class_fun['fun'];
                    $classObj       = app($namespaceClass);
                    $result         = call_user_func_array([$classObj, $fun], []); // $classObj->$fun($params);
                    $output->writeln($result);
                }
            } else {
                throw new Exception("方法不存在");
            }
        } catch (\ArgumentCountError $d) {
            var_dump("function error or not exisit! class = {$namespaceClass} && func = {$fun}");
        } catch (\Exception $e) {
            var_dump($e->getMessage());
        }

    }

    public function route($route = '')
    {
        if ($route) {
            $app        = "app";
            $controller = "controller";
            $route      = explode("/", $route);
            $module     = isset($route[0]) ? $route[0] : "index";
            $class      = isset($route[1]) ? $route[1] : 'index';
            $fun        = isset($route[2]) ? $route[2] : 'index';

            // $class_fun['class'] = $app."\\".$module."\\".$controller."\\".$class;
            $class_fun['class']      = $class;
            $class_fun['fun']        = $fun;
            $class_fun['module']     = $module;
            $class_fun['app']        = $app;
            $class_fun['controller'] = $controller;

        }

        return $class_fun;
    }

    public function option($option)
    {
        /* 整理成数组 start */
        $params     = array();
        $option_arr = explode(",", $option);
        foreach ($option_arr as $key => $val) {
            $temp_params = explode("=", $val);
            $params[]    = $temp_params[1];
        }

        /* 整理成数组 end */

        return $params;
    }

}

```

## 2、<font style="color:rgb(51, 51, 51);">在 config/sonsole.php 中，添加</font>

```php
// 指令定义
    'commands' => [
        'action' => '\app\command\Action',
    ],
```

## 3、使用案例：

```shell
php think action common/test/index --option name=jack,age=10
```

<font style="color:rgb(51, 51, 51);">对应控制器方法：</font>

```php
public function index($params=array()){


        print_r($params);
        print_r("cmd输出\n");
        return "结果";

    }
```
