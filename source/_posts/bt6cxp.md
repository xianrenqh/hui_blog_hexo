---
title: php json 处理gbk转码utf-8问题（ json_encode转换数组，值为null）
urlname: bt6cxp
date: '2022-04-11 02:44:16 +0000'
tags:
  - php
  - json
  - 转码
categories:
  - 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
---

遇到个很幼稚的问题，用 json_encode 把数组转换为 json 时，发现转化的值为 null。怎么回事呢？查找手册：发现了下面的话：
该函数只能接受 UTF-8 编码的数据（译注：指字符/字符串类型的数据）
原来数组中有中文，需要转码哦，写个转换字符编码的函数吧：

```php
function encodeConvert($str, $fromCode, $toCode)
{
    if (strtoupper($toCode) == strtoupper($fromCode)) {
        return $str;
    }
    if (is_string($str)) {
        if (function_exists('mb_convert_encoding')) {
            return mb_convert_encoding($str, $toCode, $fromCode);
        } else {
            return iconv($fromCode, $toCode, $str);
        }
    } elseif (is_array($str)) {
        foreach ($str as $k => $v) {
            $str[$k] = encodeConvert($v, $fromCode, $toCode);
        }
return $str;
}

    return $str;
}
```

对于数组，通过下面方式 json_encode 调用，一切 ok ~~~

```php
$json_api=json_encode(encodeConvert($json_api,'gb2312','utf-8'));
$json_api=json_decode(json_decode($json_api));
```
