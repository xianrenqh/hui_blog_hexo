---
title: 【笔记】thinkphp5.1使用worker-gateway推送
urlname: hommnm
date: '2022-04-26 03:34:42 +0000'
tags:
  - thinkphp
  - think-worker
  - gateway
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
---

# 使用 composer 安装：

```shell
composer require workerman/gateway-worker
composer require workerman/gatewayclient
```

# 开启端口号

默认开启 1236 和 2348 端口

# 创建控制器等文件

## 1、\application\http\Worker.php

```php
<?php
/**
 * Created by PhpStorm.
 * Info:
 */

namespace app\http;

use think\facade\Env;
use think\worker\Server;

class Worker extends Server
{

    protected $protocol = 'websocket';

    protected $host = '0.0.0.0';

    protected $port = '2120';

    protected $socket = '';

    public function onWorkerStart($worker)
    {
        echo "Worker starting...\n";
    }

    public function onMessage($connection, $data)
    {
        //halt($data);
        $connection->send('receive success123');
    }

    public function onError($connection, $code, $msg)
    {
        echo "error [ $code ] $msg\n";
    }

}
```

## 2、\application\http\Event.php

```php
<?php
/**
 * Created by PhpStorm.
 * Info:
 */

namespace app\http;

use think\Db;
use GatewayWorker\Lib\Gateway;

class Event
{

    public static function onConnect($client_id)
    {
        Gateway::sendToClient($client_id, json_encode([
            'type'      => 'init',//客户连接后向用户发送绑定uid请求（1）
            'client_id' => $client_id
        ]));
    }

    /**
     * 当客户端发来消息时触发
     *
     * @param int   $client_id 连接id
     * @param mixed $message   具体消息
     */
    public static function onMessage($client_id, $message)
    {
        $message_data = json_decode($message, true);
        if ( ! $message_data) {
            return;
        }
        $shop_id     = ! empty($message_data['shop_id']) ? $message_data['shop_id'] : 0;
        $admin_id    = ! empty($message_data['admin_id']) ? $message_data['admin_id'] : 0;
        $table_id    = ! empty($message_data['table_id']) ? $message_data['table_id'] : 0;
        $client_type = ! empty($message_data['client_type']) ? $message_data['client_type'] : 0;
        $line_num    = ! empty($message_data['line_num']) ? $message_data['line_num'] : 0;

        //将client_id与uid进行绑定
        $uid = $shop_id.'-'.$client_type.'-'.$admin_id.'-'.$table_id;

        $from_id = $message_data['from_id'];
        // 接收人ID
        $to_id = isset($message_data['to_id']) ? $message_data['to_id'] : 0;

        switch ($message_data['type']) {
            case "bind":
                Gateway::bindUid($client_id, $uid);
                $findClient = Db::name('socket_client')->where([
                    'type'     => $client_type,
                    'shop_id'  => $shop_id,
                    'admin_id' => $admin_id,
                    'table_id' => $table_id
                ])->find();
                if (empty($findClient)) {
                    Db::name('socket_client')->insert([
                        'shop_id'     => $shop_id,
                        'admin_id'    => $admin_id,
                        'table_id'    => $table_id,
                        'client'      => $client_id,
                        'type'        => $client_type,
                        'create_time' => time()
                    ]);
                } else {
                    Db::name('socket_client')->where([
                        'type'     => $client_type,
                        'shop_id'  => $shop_id,
                        'admin_id' => $admin_id,
                        'table_id' => $table_id
                    ])->data(['client' => $client_id, 'update_time' => time()])->update();
                }

                break;
            case "online":
                //判断接收人是否在线
                $onlineStatusForFrom_id = Gateway::isUidOnline($uid);
                Gateway::sendToUid($uid, json_encode(['type' => 'online', 'status' => $onlineStatusForFrom_id]));
                break;
            case "say":
                //发送文字
                $text    = nl2br(htmlspecialchars($message_data['content']));
                $sayData = [
                    'type'     => 'say',
                    'content'  => $text,
                    'from_id'  => $from_id,
                    'to_id'    => $to_id,
                    'log_time' => time()
                ];
                if (Gateway::isUidOnline($to_id)) {
                    $sayData['is_read'] = 1;
                    Gateway::sendToUid($to_id, json_encode($sayData));
                } else {
                    $sayData['is_read'] = 0;
                }
                Gateway::sendToUid($from_id, json_encode($sayData));
                /*$content = $message_data['content'];
                Gateway::sendToAll(json_encode($content));*/
                break;
            case "middleware":
                //向中间件发送消息
                $middleData = [
                    'type'     => 'middleware',
                    'shop_id'  => $shop_id,
                    'admin_id' => $admin_id,
                    'table_id' => $table_id,
                    'line_num' => $line_num
                ];
                Gateway::sendToAll(json_encode($middleData));
                break;
            case "pong":
                return;//心跳检测
                break;
            default:
                Gateway::sendToAll(json_encode(['content' => '发送所有数据']));
        }
    }

    /**
     * 当用户断开连接时触发
     *
     * @param int $client_id 连接id
     */
    public static function onClose($client_id)
    {
        // 向所有人发送
        GateWay::sendToAll("$client_id logout\r\n");
    }

}

```

## 3、\application\http\controller\Index.php

```php
<?php

/**
 * Created by PhpStorm.
 * Info:
 */

namespace app\http\controller;

use GatewayWorker\Lib\Gateway;
use think\Controller;

class Index extends Controller
{

    public function index()
    {

        $portalTitle  = 'hello socket';
        $post_id      = 1;
        $push_content = "content";
        self::pushAll($portalTitle, $post_id, $push_content = '');
    }

    public function pushAll($portalTitle, $post_id, $push_content = '')
    {

        \GatewayClient\Gateway::$registerAddress = '127.0.0.1:1236';
        // 向任意uid的网站页面发送数据
        Gateway::sendToAll(json_encode([
            'type'       => 'say_all',
            'code'       => 1,
            'post_id'    => $post_id,
            'post_title' => $portalTitle,
            'content'    => $push_content
        ]));

        return true;

    }

}

```

# 前端 socket 页面使用：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0"
    />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    hello socket!
    <script src="https://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>

    <script>
      link();

      function link() {
        var url = "ws://127.0.0.1:2348";
        socket = new WebSocket(url);
        socket.onopen = function () {
          console.log("连接成功~~~~");
        };
        socket.onmessage = function (res) {
          console.log("获取数据：" + res.data);
          let getRes = jQuery.parseJSON(res.data);
          if (getRes.type == "init") {
            let getClientId = getRes.client_id;
            sendBindClient(getClientId);
          }
        };
        socket.onclose = function () {
          console.log("断开连接");
        };
      }

      //获取client_id并写入数据库
      function sendBindClient(client_id) {
        if (client_id != "") {
          $.post(
            "/api.php/notice/add_client",
            {
              shop_id: "10001",
              table_id: "25",
              client_id: client_id,
              type: "2",
            },
            function (res) {
              console.log(res);
            }
          );
        }
      }
    </script>
  </body>
</html>

<?php
?>
```
