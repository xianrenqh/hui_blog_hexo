---
title: php使用图鉴识别图片、二维码等
urlname: utkx6dpuwvrakgx5
date: '2025-01-15 01:43:01 +0000'
tags:
  - php
  - 图鉴
  - 识图
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.cc'
copyright_url:
copyright_author:
cover:
---

> 图鉴，是基于人工智能的高可用图片识别平台<font style="color:rgb(51, 51, 51);">，可以识别二维码、验证码、缺图验证码等。</font>

## 图鉴函数方法：

```php
<?php

declare(strict_types=1);

class ImgCaptcha
{

    /**
     * 图鉴 http://www.ttshitu.com/
     *
     * @param string $image  图片地址
     * @param string $typeid 识别类型
     *
     * @return string
     */
    public static function ttShiTu(string $image = '', string $typeid = '3'): string
    {
        $api_url = 'http://api.ttshitu.com/predict';
        $info    = [
            'username' => '',
            'password' => '',
            'typeid'   => $typeid,
            'image'    => $image
        ];
        $res     = self::getHttpResponse($api_url, [], json_encode($info));
        $data    = json_decode($res, true);
        $captcha = '';
        if ($data['success'] === true) {
            $captcha = $data['data']['result'];
        }

        return $captcha;
    }

    // 请求外部资源
    private static function getHttpResponse($url, $header = [], $post = null, $timeout = 10)
    {
        $ch = curl_init($url);
        curl_setopt($ch, CURLOPT_TIMEOUT, $timeout);
        curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
        curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, false);
        if ($header) {
            curl_setopt($ch, CURLOPT_HTTPHEADER, $header);
        } else {
            $httpheader[] = "Accept: */*";
            $httpheader[] = "Accept-Language: zh-CN,zh;q=0.9";
            $httpheader[] = "Connection: close";
            curl_setopt($ch, CURLOPT_HTTPHEADER, $httpheader);
        }
        curl_setopt($ch, CURLOPT_HEADER, false);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
        if ($post) {
            curl_setopt($ch, CURLOPT_POST, true);
            curl_setopt($ch, CURLOPT_POSTFIELDS, $post);
        }
        $response = curl_exec($ch);
        curl_close($ch);

        return $response;
    }
}

```

## 调用方法：

```php
// 解析验证码
    private function getCaptchaInfo(string $image = '', string $typeid = '3'): string
    {
        $captcha = \ImgCaptcha::ttShiTu($image, $typeid);

        return $captcha;
    }
```

## 使用方法：

```php
$captcha      = $this->getCaptchaInfo($img_url, '33');
```
