---
title: php使用Google Authenticator (Google2fa)进行二次验证
urlname: mbx3ei0pz3fn1yog
date: '2024-04-15 07:40:36 +0000'
tags:
  - Google2fa，Google Authenticator，Authenticator
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.cc'
copyright_author: 小灰灰
copyright_url:
cover:
---

# 什么是 google2fa

Google2FA 是 Google Authenticator 的简称，是一种基于时间的一次性密码（TOTP）算法，用于实现双因素身份验证。双因素身份验证通过要求用户除了输入密码外，还需提供另一个因素（通常是生成的一次性验证码），以提高账户的安全性。
Google Authenticator 生成的验证码是基于时间的，每隔一段时间就会生成一个新的验证码。用户在登录时需要输入当前时刻生成的验证码，以验证其身份。
Google2FA 库是一个用于 PHP 的库，可以帮助开发人员轻松地集成双因素身份验证功能到他们的应用程序中。通过 Google2FA，开发人员可以生成密钥、生成二维码供用户扫描、验证用户输入的验证码等操作，从而实现安全的双因素身份验证。

# 为什么要使用 google2fa

使用 Google2FA 或类似的库可以为应用程序增加额外的安全层，防止未经授权的访问和保护用户数据安全。这种双因素身份验证在许多网站和应用程序中被广泛采用，以提高账户的安全性。

# 在 php 中使用

> 默认环境： php8.0
> 要求环境：php7.1+

## 1、安装 google2fa

使用 Composer 安装它：

```shell
composer require pragmarx/google2fa
```

要生成内联 QRCodes，您需要安装一个 QR 码生成器，例如 [BaconQrCode](https://github.com/Bacon/BaconQrCode)：

```shell
composer require bacon/bacon-qr-code
```

## 2、安装 php 扩展（php_imagick）

> 此扩展组件是 baconQrCode 需要创建二维码生成使用。

可参考网址：
[PHP 安装 Imagick 扩展-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/2017351)

根据环境下载合适的 imagick 扩展 和 imagemagick 程序 1.下载拓展
下载地址一: [http://windows.php.net/downloads/pecl/releases/imagick/](http://windows.php.net/downloads/pecl/releases/imagick/)
下载地址二: [https://pecl.php.net/package/imagick](https://pecl.php.net/package/imagick)
要点: 注意对应 php 版本 ts 还是 nts x86 还是 x64
这里以 phpinfo()为准

2. 安装拓展
   解压上述文件后，将 php_imagick.dll 复制到 php/ext 目录，或者其他你的存放拓展的目录
   修改 php.ini 加上 extension=php_imagick.dll，注意 php 可能有多个 ini，以 phpinfo 为准
   此时复制解压上述文件目录中其他 dll 到 php 目录，重启 apache，此时 phpinfo 显示拓展安装成功，但是 ImageMagick number of supported formats 为 0，到这里成功安装了一半
3. 下载 imagemagick 程序
   下载地址：[http://windows.php.net/downloads/pecl/deps/](http://windows.php.net/downloads/pecl/deps/)
   imagemagick 还有官网下载，此处不鼓励从 imagemagick 官方下载，他们的网站上我并没有找到历史版本下载，安装失败的几率很大
   下载与 phpinfo 提示一致的版本，此时需要注意  1.软件版本对应     2.vc11 还是 vc14 3.x86 还是 x64 都要以 phpinfo 为准

## 3、在 php 控制器中使用

> 默认使用的是 thinkphp6.0 ，其他框架类同

### 引入类库：

```php
use think\facade\Request;
use PragmaRX\Google2FA\Google2FA;
// 创建二维码
use BaconQrCode\Renderer\ImageRenderer;
use BaconQrCode\Renderer\Image\ImagickImageBackEnd;
use BaconQrCode\Renderer\RendererStyle\RendererStyle;
use BaconQrCode\Writer;
```

### 创建二维码：

```php
//实例化
$google2fa = new Google2FA();

$secretKey    = $google2fa->generateSecretKey(32);
$companyName  = "HuiCMF-webman"; //这里可以是标题等
$companyEmail = "xiaohuihui";    //用户名。这里可以是邮箱、手机号、用户ID等唯一值

$qrCodeUrl = $google2fa->getQRCodeUrl($companyName, $companyEmail, $secretKey);

$writer       = new Writer(new ImageRenderer(new RendererStyle(400), new ImagickImageBackEnd()));
$qrcode_image = base64_encode($writer->writeString($qrCodeUrl));
//创建成功的秘钥记得保存到数据库，验证数据的时候需要用到

echo '<img src="data:image/png;base64, '.$qrcode_image.' "/>';
```

创建成功后的二维码可以使用第三方工具扫码，绑定到数据里。最后一部分会讲

### 数据验证：

```php
// 获取验证码
        $secret    = Request::param('secret');
        $google2fa = new Google2FA();
        //获取用户的秘钥密钥（一般从数据库获取）
        $secretKey = 'IDTD5IIARQSORYKN3UXO4AZKHF5IRKXF';
        $timestamp = false;
        $timestamp = $google2fa->verifyKeyNewer($secretKey, $secret, $timestamp);

        if ($timestamp !== false) {
            dump($timestamp);
            //这里更新用户信息 $timestamp
            // successful
        } else {
            // failed
            return "验证失败";
        }
```

> 请注意，以上代码中使用的 verifyKeyNewer：
> 攻击者可能能够监视用户输入其凭据和一次性密钥。如果不采取进一步的预防措施，密钥将保持有效，直到它不再在服务器时间窗口内。为了防止使用已经使用过的一次性密钥，可以使用 verifyKeyNewer 函数。

> 请注意，$timestamp 要么是 false（如果密钥无效或以前曾使用过），要么是提供的密钥的 unix 时间戳除以 30 秒的密钥再生周期。

# Google Authenticator Apps

> 可以使用身份验证码的 app 参考

1. Google Authenticator
2. 微软 Authenticator
3. LastPass
4. 1Password
5. 腾讯身份验证器
6. ......
