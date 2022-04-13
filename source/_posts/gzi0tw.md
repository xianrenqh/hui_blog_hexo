---
title: php简单的生成html静态页面代码
urlname: gzi0tw
date: '2022-04-12 10:08:19 +0000'
tags:
  - html静态页
  - php
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
---

```php
<?php
class CreateHtml
{
    function mkdir($prefix = 'article'){
        $y = date('Y');
        $p = DIRECTORY_SEPARATOR;
        $filePath = 'article' . $p . $y . $p  . $p;
        $a = explode($p, $filePath);
        $path='';
        foreach ($a as $dir) {
            $path .= $dir . $p;
            if (!is_dir($path)) {
                //echo '没有这个目录'.$path;
                mkdir($path, 0755);
            }
        }
        return $filePath . $p;
    }

    function start(){
        ob_start();
    }

    function end(){
        $info = ob_get_contents();
        $fileId = '12345';
        $postfix = '.shtml';
        $path = $this->mkdir($prefix = 'article');
        $fileName = $fileId . $postfix;
        $file = fopen($path . $fileName, 'w ');
        fwrite($file, $info);
        fclose($file);
        ob_end_flush();
    }
}

?>
<?php
$s = new CreateHtml();
$s->start();
?>
<html>
<body>
我是要输出的html页面代码， 你呢
</body>
</html>
<?php
$s->end();
?>
```
