---
title: 微信公众号对接开发api自动回复功能-php
urlname: canwz3d9ntd3yz9a
date: '2023-10-20 06:26:48 +0000'
tags: []
categories: []
---

tags: []

categories: <font style="color:rgb(38, 38, 38);">学无止境</font>

copyright_author_href: https://www.xiaohuihui.cc

<font style="color:rgb(38, 38, 38);">copyright_url:  
</font><font style="color:rgb(38, 38, 38);">copyright_author: </font>

<font style="color:rgb(33, 37, 41);">cover:</font>

---

<font style="color:rgb(51, 51, 51);">  
</font><font style="color:rgb(51, 51, 51);">微信公众号对接开发api自动回复功能，包含文字回复以及图文回复</font>

<font style="color:rgb(51, 51, 51);">php 代码，直接上代码</font>

<font style="color:rgb(51, 51, 51);"></font>

```php
<?php
/**
 * by：小灰灰
 * 接口配置信息：
 * URL：http://******.com/api/wx/valid
 * Token：123456
 */

// 接收参数
define("TOKEN", '123456');

$wechatObj = new wx();
if (isset($_GET['echostr'])) {
    $wechatObj->valid();
} else {
    $wechatObj->responseMsg();
}

class wx
{

    /**
     * 微信公众号验证
     */
    public function valid()
    {
        $echoStr = $_GET["echostr"];
        if ($this->checkSignature()) {
            echo $echoStr;
            exit;
        }
    }

    /**
     * 回复消息
     */
    public function responseMsg()
    {
        $postArr = file_get_contents("php://input");
        $postObj = simplexml_load_string($postArr, 'SimpleXMLElement', LIBXML_NOCDATA);

        $msgType  = $postObj->MsgType;
        $toUser   = $postObj->FromUserName;
        $fromUser = $postObj->ToUserName;

        //判断该数据包是否是订阅的事件推送
        if (strtolower($msgType) == 'event') {
            if ($postObj->Event == 'subscribe') { //如果是订阅事件
                $followMeText = "这里可以设置默认关注回复词哦";
                if (empty($followMeText)) {
                    $followMeText = "嘻嘻，最近小编花了好大时间做了这个自动回复功能！现在可以直接回复关键词就ok拉！";
                }
                $contentStrq = $content = $followMeText;
                $this->subscribeMsg($contentStrq, $toUser, $fromUser);
            }
        } else {
            $content = trim($postObj->Content);

            $resData = [
                [
                    'title'       => '这是title',
                    'pic_url'     => '',
                    'description' => '这是描述',
                    'link'        => 'https://baidu.com'
                ],
                [
                    'title'       => '最新ChatGPT4免费使用教程。免登录直接就可以使用了。',
                    'pic_url'     => 'https://i0.hdslb.com/bfs/archive/3bd476e6e008cf38e274adf63276da189dbf7aae.jpg@336w_190h_!web-video-rcmd-cover.webp',
                    'description' => '最新ChatGPT4免费使用教程。免登录直接就可以使用了。',
                    'link'        => 'https://www.bilibili.com/video/BV1UN411J7Ct/?spm_id_from=333.1007.tianma.1-1-1.click'
                ]
            ];
            if (empty($resData)) {
                $resText = "这里设置，如果没有获取到数据，回复的词";
                $this->responseTextMsg($resText, $toUser, $fromUser);
            } else {
                $this->responseImageNewsMsg($resData, $toUser, $fromUser);
            }
        }
    }

    //关注
    private function subscribeMsg($content, $toUser, $fromUser)
    {
        $template = <<<EOXML
        <xml>
            <ToUserName><![CDATA[%s]]></ToUserName>
            <FromUserName><![CDATA[%s]]></FromUserName>
            <CreateTime>%s</CreateTime>
            <MsgType><![CDATA[%s]]></MsgType>
            <Content><![CDATA[%s]]></Content>
        </xml>
EOXML;
        echo sprintf($template, $toUser, $fromUser, time(), 'text', $content);
        die();
    }

    /**
     * responseImageMsg
     *
     * @param $msg
     * @param $toUser
     * @param $fromUser
     *
     * @return string
     */
    function responseImageNewsMsg($msg, $toUser, $fromUser)
    {
        $arr      = $msg;
        $template = "<xml>
            <ToUserName><![CDATA[%s]]></ToUserName>
            <FromUserName><![CDATA[%s]]></FromUserName>
            <CreateTime>%s</CreateTime>
            <MsgType><![CDATA[%s]]></MsgType>
            <ArticleCount>".count($arr)."</ArticleCount>
        <Articles>";
        foreach ($arr as $k => $v) {
            $template .= "<item>
                <Title><![CDATA[".$v['title']."]]></Title>
                <Description><![CDATA[".$v['description']."]]></Description>
                <PicUrl><![CDATA[".$v['pic_url']."]]></PicUrl>
                <Url><![CDATA[".$v['link']."]]></Url>
            </item>";
        }
        $template .= "</Articles>";
        $template .= "</xml>";

        echo sprintf($template, $toUser, $fromUser, time(), 'news');
        die();
    }

    /**
     * @param $content
     * @param $toUser
     * @param $fromUser
     */
    function responseTextMsg($content, $toUser, $fromUser)
    {
        $msgType = 'text';
        //修改为
        if ( ! is_utf8($content)) {
            $content = iconv('gb2312', 'UTF-8//IGNORE', $content);
        }
        $template = <<<EOXML
        <xml>
            <ToUserName><![CDATA[%s]]></ToUserName>
            <FromUserName><![CDATA[%s]]></FromUserName>
            <CreateTime>%s</CreateTime>
            <MsgType><![CDATA[%s]]></MsgType>
            <Content><![CDATA[%s]]></Content>
        </xml>
EOXML;
        echo sprintf($template, $toUser, $fromUser, time(), $msgType, $content);
        die();
    }

    private function checkSignature()
    {
        $signature = $_GET["signature"];
        $timestamp = $_GET["timestamp"];
        $nonce     = $_GET["nonce"];

        $token  = TOKEN;
        $tmpArr = array($token, $timestamp, $nonce);
        sort($tmpArr);
        $tmpStr = implode($tmpArr);
        $tmpStr = sha1($tmpStr);

        if ($tmpStr == $signature) {
            return true;
        } else {
            return false;
        }
    }

}

```
