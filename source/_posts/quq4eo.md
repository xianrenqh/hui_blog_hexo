---
title: Env环境变量类库
urlname: quq4eo
date: '2022-06-17 01:16:25 +0000'
tags: []
categories: []
---

tags: [env,环境变量,类库]

categories: <font style="color:rgb(38, 38, 38);">学无止境</font>

copyright_author_href: https://www.xiaohuihui.net

<font style="color:rgb(38, 38, 38);">copyright_url:  
</font><font style="color:rgb(38, 38, 38);">copyright_author: </font>

<font style="color:rgb(33, 37, 41);">cover:</font>

---

```php
<?php
  /**
  * Created by PhpStorm.
  * User: 小灰灰
  * Date: 2022-04-26
  * Time: 21:53:40
  * Info:
  */
  namespace support\lib;
Env::load();
class Env
{

  /**
  * 环境变量数据
  * @var array
  */
  protected static $data = [];

  /**
  * 读取环境变量定义文件
  * @access public
  *
  * @param string $file 环境变量定义文件
  *
  * @return void
  */
  public static function load($file = '.env')
  {
    if ( ! is_file($file)) {
      return;
    }
    $env = parse_ini_file($file, true);
    self::set($env);
  }

  /**
  * 获取环境变量值
  * @access public
  *
  * @param string $name    环境变量名
  * @param mixed  $default 默认值
  */
  public static function get($name = null, $default = null)
  {
    if ($name == null) {
      return self::$data;
    }
    $name = strtoupper($name);
    $name = strtoupper(str_replace('.', '_', $name));
    if (isset(self::$data[$name])) {
      return self::$data[$name];
    }

    return $default;
  }

  /**
  * 设置环境变量值
  * @access public
  *
  * @param string|array $env   环境变量
  * @param mixed        $value 值
  *
  * @return void
  */
  public static function set($env, $value = null)
  {
    if (is_array($env)) {
      $env = array_change_key_case($env, CASE_UPPER);
      foreach ($env as $key => $val) {
        if (is_array($val)) {
          foreach ($val as $k => $v) {
            self::$data[$key.'_'.strtoupper($k)] = $v;
          }
        } else {
          self::$data[$key] = $val;
        }
      }
    } else {
      $name              = strtoupper(str_replace('.', '_', $env));
      self::$data[$name] = $value;
    }
  }
}
```

使用方法：

```php
use support\lib\Env;

Env::get('SQL.DB_DEFAULT', 'mysql')
```

.env 文件：

```plain
[SQL]
DB_DEFAULT = mysql
HOSTNAME = 127.0.0.1
DATABASE = database
DBNAME = root
DBPASS = 123456
HOSTPOINT = 3306
PREFIX = cmf_
```

<font style="color:rgb(51, 51, 51);">  
</font>
