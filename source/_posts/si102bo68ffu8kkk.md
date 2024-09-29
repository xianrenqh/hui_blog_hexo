---
title: 使用workerman接收websocket消息，并在thinkphp6控制器里处理取得的数据
urlname: si102bo68ffu8kkk
date: '2024-07-18 01:29:21 +0000'
tags:
  - workerman
  - websocket
  - thinkphp
  - php
categories: '<font style="color:rgb(38, 38, 38);">学无止境</font>'
copyright_author_href: 'https://www.xiaohuihui.cc'
'</font><font style="color:rgb(38, 38, 38);">copyright_author': </font>
'<font style="color:rgb(33, 37, 41);">cover': </font>
<font style="color:rgb(38, 38, 38);">copyright_url:
---

# <font style="color:rgb(51, 51, 51);">一、创建 workerman 服务，并启动</font>

> <font style="color:rgb(51, 51, 51);">用以接收 websocket 消息</font>

## <font style="color:rgb(51, 51, 51);">1、安装 workerman 服务</font>

```plain
composer require workerman/workerman
```

## 2、根目录下创建文件：**<font style="color:#DF2A3F;">server.php</font>**

### 方法 1：使用回调函数

```php
<?php
require_once __DIR__.'/vendor/autoload.php';

use Workerman\Worker;
use Workerman\Connection\AsyncTcpConnection;

$worker = new Worker();

$worker->onWorkerStart = function ($worker) {

    $con = new AsyncTcpConnection('ws://127.0.0.1:7600/wcf/socket_receiver');

    // websocket握手成功后
    $con->onWebSocketConnect = function (AsyncTcpConnection $con,) {
        $con->send('hello');
    };

    // 当收到消息时
    $con->onMessage = function (AsyncTcpConnection $con, $data) {
        echo $data.PHP_EOL;
        //方法一：使用回调函数给控制器传递消息
        $gogogoInstance = new \app\wx\controller\Gogogo();
        call_user_func_array([$gogogoInstance, 'getMsgData'], [$data]);
    };

    $con->onError = function (AsyncTcpConnection $con, $code, $msg) {
        echo "error $code $msg\n";
    };

    $con->connect();
};

Worker::runAll();

```

### 方法 2：使用 thinkphp 容器及中间件

```php
<?php
require_once __DIR__.'/vendor/autoload.php';
require_once __DIR__.'/app/wx/controller/Index.php';

use Workerman\Worker;
use Workerman\Connection\AsyncTcpConnection;

use think\Container;

$worker = new Worker();

$worker->onWorkerStart = function ($worker) {

    $con = new AsyncTcpConnection('ws://127.0.0.1:7600/wcf/socket_receiver');

    // websocket握手成功后
    $con->onWebSocketConnect = function (AsyncTcpConnection $con,) {
        $con->send('hello');
    };

    // 当收到消息时
    $con->onMessage = function (AsyncTcpConnection $con, $data) {
        echo $data.PHP_EOL;
        $webSocketService = Container::getInstance()->make(\App\Service\WebSocketService::class);
        $webSocketService->handleMessage($data);

    };

    $con->onError = function (AsyncTcpConnection $con, $code, $msg) {
        echo "error $code $msg\n";
    };

    $con->connect();
};

Worker::runAll();

```

## 3、创建中间件（如果需要【方法 2 代码】）

文件：app\service\WebSocketService.php

```php
namespace app\service;

class WebSocketService
{

    public function handleMessage($message)
    {
        // 这里可以调用控制器方法
        $controller = new \app\wx\controller\Gogogo();
        $controller->getMsgData($message);
    }
}
```

## 4、创建控制器处理数据

文件：app/wx/controller/Gogogo.php

```php
namespace app\wx\controller;

class Gogogo
{

    public function getMsgData($message)
    {
        // 处理接收到的WebSocket消息
        file_put_contents("t.txt", gettype($message).PHP_EOL, FILE_APPEND);
        file_put_contents("t.txt", $message.PHP_EOL, FILE_APPEND);
        // 你可以在这里进行其他业务逻辑处理
    }
}
```
