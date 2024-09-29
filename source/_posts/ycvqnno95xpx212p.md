---
title: php-阿里云发送短信简单的类库
urlname: ycvqnno95xpx212p
date: '2023-10-09 01:16:23 +0000'
tags:
  - php
  - 阿里云
  - 短信
categories: '<font style="color:rgb(38, 38, 38);">学无止境</font>'
copyright_author_href: 'https://www.xiaohuihui.cc'
'</font><font style="color:rgb(38, 38, 38);">copyright_author': </font>
'<font style="color:rgb(33, 37, 41);">cover': </font>
<font style="color:rgb(38, 38, 38);">copyright_url:
---

<font style="color:rgb(51, 51, 51);">  
</font><font style="color:rgb(51, 51, 51);">简单的阿里云短信发送类库</font>

<font style="color:rgb(51, 51, 51);"></font>

## <font style="color:rgb(51, 51, 51);">类库代码</font>

```php
<?php
/**
 * Created by PhpStorm.
 * User: 小灰灰
 * Date: 2023-10-08
 * Time: 10:58:58
 * Info: 阿里云短信发送插件
 */

namespace support\lib;

class AliSms
{

    protected $error;

    protected $accessKeyId;

    protected $accessKeySecret;

    public function __construct($accessKeyId, $accessKeySecret)
    {
        $this->accessKeyId     = ! empty($accessKeyId) ? $accessKeyId : '';
        $this->accessKeySecret = ! empty($accessKeySecret) ? $accessKeySecret : '';
    }

    public function getError()
    {
        return $this->error;
    }

    /**
     * 发送短信
     */
    public function sendSms($params = array())
    {
        if (empty($params['PhoneNumbers'])) {
            throw new \InvalidArgumentException('缺失手机号$params["PhoneNumbers"]');
        }
        if (empty($params['SignName'])) {
            throw new \InvalidArgumentException('缺失短信签名$params["SignName"]');
        }
        if (empty($params['TemplateCode'])) {
            throw new \InvalidArgumentException('缺失模版CODE$params["TemplateCode"]');
        }

        // fixme 必填: 请参阅 https://ak-console.aliyun.com/ 取得您的AK信息
        $accessKeyId     = $this->accessKeyId;
        $accessKeySecret = $this->accessKeySecret;

        // *** 需用户填写部分结束, 以下代码若无必要无需更改 ***
        if ( ! empty($params["TemplateParam"]) && is_array($params["TemplateParam"])) {
            $params["TemplateParam"] = json_encode($params["TemplateParam"]);
        }

        try {
            $content = $this->request($accessKeyId, $accessKeySecret, "dysmsapi.aliyuncs.com",
                array_merge($params, array(
                    "RegionId" => "cn-hangzhou",
                    "Action"   => "SendSms",
                    "Version"  => "2017-05-25",
                )));
            if ($content->Code == 'OK') {
                return true;
            } else {
                throw new \InvalidArgumentException($content->Message);
            };
        } catch (\Exception $e) {
            throw new \InvalidArgumentException($e->getMessage());
        }
    }

    /**
     * 生成签名并发起请求
     *
     * @param $accessKeyId     string AccessKeyId (https://ak-console.aliyun.com/)
     * @param $accessKeySecret string AccessKeySecret
     * @param $domain          string API接口所在域名
     * @param $params          array API具体参数
     *
     * @return bool|\stdClass 返回API接口调用结果，当发生错误时返回false
     */
    public function request($accessKeyId, $accessKeySecret, $domain, $params)
    {
        $apiParams = array_merge(array(
            "SignatureMethod"  => "HMAC-SHA1",
            "SignatureNonce"   => uniqid(mt_rand(0, 0xffff), true),
            "SignatureVersion" => "1.0",
            "AccessKeyId"      => $accessKeyId,
            "Timestamp"        => gmdate("Y-m-d\TH:i:s\Z"),
            "Format"           => "JSON",
        ), $params);
        ksort($apiParams);

        $sortedQueryStringTmp = "";
        foreach ($apiParams as $key => $value) {
            $sortedQueryStringTmp .= "&".$this->encode($key)."=".$this->encode($value);
        }

        $stringToSign = "GET&%2F&".$this->encode(substr($sortedQueryStringTmp, 1));

        $sign = base64_encode(hash_hmac("sha1", $stringToSign, $accessKeySecret."&", true));

        $signature = $this->encode($sign);

        $url = "http://{$domain}/?Signature={$signature}{$sortedQueryStringTmp}";

        try {
            $content = $this->fetchContent($url);

            return json_decode($content);
        } catch (\Exception $e) {
            throw new \Exception($e->getMessage());
        }
    }

    private function encode($str)
    {
        $res = urlencode($str);
        $res = preg_replace("/\+/", "%20", $res);
        $res = preg_replace("/\*/", "%2A", $res);
        $res = preg_replace("/%7E/", "~", $res);

        return $res;
    }

    private function fetchContent($url)
    {
        if (function_exists("curl_init")) {
            $ch = curl_init();
            curl_setopt($ch, CURLOPT_URL, $url);
            curl_setopt($ch, CURLOPT_TIMEOUT, 5);
            curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
            curl_setopt($ch, CURLOPT_HTTPHEADER, array(
                "x-sdk-client" => "php/2.0.0"
            ));
            $rtn = curl_exec($ch);

            if ($rtn === false) {
                trigger_error("[CURL_".curl_errno($ch)."]: ".curl_error($ch), E_USER_ERROR);
            }
            curl_close($ch);

            return $rtn;
        }

        $context = stream_context_create(array(
            "http" => array(
                "method" => "GET",
                "header" => array("x-sdk-client: php/2.0.0"),
            )
        ));

        return file_get_contents($url, false, $context);
    }

}

```

## 发送服务代码

```php
<?php
/**
 * Created by PhpStorm.
 * User: 小灰灰
 * Date: 2023-10-08
 * Time: 11:01:34
 * Info:
 */

namespace app\common\service;

use support\lib\AliSms;

class SmsCodeService extends AliSms
{

    private $access_key = '';

    private $access_secret = '';

    //签名
    const SIGN_NAME = '';

    //短信模板代码
    const TEMPLATE_SMS_CODE = '';

    public function __construct()
    {
        parent::__construct($this->access_key, $this->access_secret);
    }

    public function sendSmsCode(array $param)
    {
        $query = [
            'SignName'     => self::SIGN_NAME,
            'TemplateCode' => self::TEMPLATE_SMS_CODE
        ];

        $query = array_merge($query, $param);

        return $this->sendSms($query);
    }

}

```

## 调用方式

```php
use app\common\service\SmsCodeService;


public function send_sms()
    {

        $phone = request()->get('phone');
        if (empty($phone)) {
            return $this->error('手机号不能为空');
        }
        if (cmf_check_mobile($phone)) {
            $sms_code  = rand(100000, 999999);
            $param     = [
                'PhoneNumbers'  => $phone,
                'TemplateParam' => json_encode(['code' => $sms_code])
            ];
            $smsServie = new SmsCodeService();
            try {
                $result = $smsServie->sendSmsCode($param);
                if ( ! $result) {
                    return $this->error('发送失败');
                }

                return $this->success('发送成功');

            } catch (\Exception $e) {
                return $this->error($e->getMessage());
            }
        } else {
            return $this->error("请输入正确手机号");
        }

    }


```
