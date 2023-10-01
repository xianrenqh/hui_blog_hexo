---
title: php redis数据分页的简单操作代码
urlname: kvbkvgbt61nwkuzf
date: '2023-09-25 01:55:06 +0000'
tags: []
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.cc'
copyright_url:
copyright_author:
cover:
---

> 目前使用的是 thinkphp，所以基本引入类库的方法使用的是 tp 的引入方法，其他引入方式一样

##

php 类库：

```php
<?php

/**
 * Created by PhpStorm.
 * User: 小灰灰
 * Date: 2023-09-23
 * Time: 14:08:19
 * Info:
 */

namespace lib;

use Redis;

class RedisPage
{

    protected $_redis;

    protected $_redis_ip;

    protected $_redis_port;

    protected $_redis_db;

    protected $_hash_prefix;

    protected $_pre_dir;

    /**
     * RedisPage constructor.
     * 初始化redis配置
     */
    public function __construct($ip = '', $port = 6379, $db = 2, $hash_prefix = 'toushi', $pre_dir = 'test')
    {
        $this->_redis_ip    = $ip ? $ip : '127.0.0.1';
        $this->_redis_port  = $port ? $port : 6379;
        $this->_redis_db    = $db ? $db : 0;
        $this->_hash_prefix = $hash_prefix ? $hash_prefix : '';
        //实例化redis数据库连接
        $this->_redis = new Redis();
        $this->_redis->connect($this->_redis_ip, $this->_redis_port);
        $this->_redis->select($this->_redis_db);

        $this->_pre_dir = $pre_dir ? $pre_dir.':' : 'test:';
    }

    /**
     * @notes   : 设置分页数据至redis数据库
     * 采用hash table 与 集合 Score字段
     */
    public function set_page_info($id, $data)
    {
        if ( ! is_numeric($id) || ! is_array($data)) {
            return false;
        }
        $hashName = $this->_hash_prefix.'_'.$id;

        //设置键值对的过期时间，【秒】
        //$this->_redis->expire($this->_pre_dir.$hashName, 100);
        $this->_redis->hMSet($this->_pre_dir.$hashName, $data);

        $this->_redis->zAdd($this->_pre_dir.$this->_hash_prefix.'_sort', $id, $id);

        return true;
    }

    /**
     * 创建单key对应的数据
     * @return void
     */
    public function setKeyData($key, $data)
    {
        $preFix = 'toushi';
        $data   = serialize($data);
        $res    = $this->_redis->set($preFix.$key, $data);

        return $res;
    }

    /**
     * 根据key获取对应数据
     *
     * @param $key
     *
     * @return mixed
     */
    public function getKeyData($key)
    {
        $preFix = 'toushi';
        $res    = $this->_redis->get($preFix.$key);
        $res    = unserialize($res);

        return $res;
    }

    /**
     * 判断key是否存在
     *
     * @param $key
     *
     */
    public function keyIsExist($key)
    {
        $preFix = 'toushi';
        $res    = $this->_redis->exists($preFix.$key);

        return $res ? true : false;
    }

    /**
     * @notes   :获取分页数据
     */
    public function get_page_info($page, $pageSize, $key = [])
    {
        if ( ! is_numeric($page) || ! is_numeric($pageSize)) {
            return false;
        }
        $limit_s = ($page - 1) * $pageSize;
        $limit_e = ($limit_s + $pageSize) - 1;
        //获取在该区间内所有带 score 的有序集合成员列表
        //ZREVRANGE 排序【倒叙】
        $range     = $this->_redis->ZREVRANGE($this->_pre_dir.$this->_hash_prefix.'_sort', $limit_s, $limit_e);
        $count     = $this->_redis->zCard($this->_pre_dir.$this->_hash_prefix.'_sort'); //统计总数
        $pageCount = ceil($count / $pageSize); //总页数

        $page_data = [];
        foreach ($range as $item) {
            if (count($key) > 0) {
                $page_data[] = $this->_redis->hMGet($this->_pre_dir.$this->_hash_prefix.'_'.$item,
                    $key); //获取hash table 中所有的集合数据
            } else {
                $page_data[] = $this->_redis->hGetAll($this->_pre_dir.$this->_hash_prefix.'_'.$item);
            }
        }

        $return_data = [
            'data'      => $page_data, // 返回的数据
            'page'      => $page, //当前页
            'pageSize'  => $pageSize,  //每页的记录数
            'total'     => $count, //总条目数
            'pageCount' => $pageCount, //总页数
        ];

        return $return_data;

    }

    /**
     * @notes   :删除记录
     */
    public function del_page_info($id)
    {
        if ( ! is_array($id)) {
            return false;
        }
        foreach ($id as $val) {
            $hashName = $this->_hash_prefix.'_'.$val;
            $this->_redis->del($this->_pre_dir.$hashName);
            $this->_redis->zRem($this->_pre_dir.$this->_hash_prefix.'_sort'.$val);
        }

        return true;
    }

    /**
     * @notes   : 0:清空当前数据库 -1:清空所有数据库
     */
    public function clear_db($db = 0)
    {
        if ($db >= 0) {
            $this->_redis->flushDB();
        } elseif ($db == -1) {
            $this->_redis->flushAll();
        } else {
            // 非法数据库参数
            return false;
        }

        return true;
    }
}


```

## 类库使用方法：

```php
//引入类库：
use lib\RedisPage;

//实例化：
$TASK_NAME = 'rcjk';
$REDIS_DB  = 1;
//获取系统配置文件中redis的配置
$redis_config      = config('cache.stores.redis');
$redis_config_host = $redis_config['host'] ?? '';
$redis_config_port = $redis_config['port'] ?? '';
//这里获取的系统全局配置，也可以走默认配置
$redis = new RedisPage($redis_config_host, $redis_config_port, $REDIS_DB, $TASK_NAME, 'toushi_'.$TASK_NAME);


$key_name = "test_name";
$key = "这是测试存储数据";	//这里可以是任意字符【字符串、数组、等】，都会被转为序列化存储

//创建生成单key：
$redis->setKeyData($key_name, $key);


//获取单个key对应的数据：
$redis->getKeyData($key_name);

//插入数组数据到redis:
$key_id = 1000; 	//这里的key_id设定的是int类型的数据id，如需要其他，请到类库中修改此方法
$arr =[] ;
$redis->set_page_info($key_id, $arr); //插入数据到redis

//获取数据，并进行分页
$redis->get_page_info(1, 100); //获取分页数据

//清空当前库中所有缓存数据
$redis->clear_db($REDIS_DB);

```

目前只是其简单的使用，其他使用方法函数还在研究中...
