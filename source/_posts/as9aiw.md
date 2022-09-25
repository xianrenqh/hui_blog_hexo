---
title: 使用小米运动（zeep-life）刷步数源码
urlname: as9aiw
date: '2022-09-21 06:02:08 +0000'
tags:
  - 小米运动
  - 刷步数
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.net'
copyright_url:
copyright_author:
cover:
---

下载 app 后登录成功后， 绑定第三方，比如微信，
绑定好后可以运行源码，
手机号、密码、等

```php
<?php
if(isset($_POST["mode"])){
	$mode=$_POST["mode"];
}else{
	die("ERROR");
}
switch ($mode) {
	case "info":
		$json = file_get_contents('cache/json.txt');
		if ($json){
		  $json = json_decode($json,true);
		}else{
		  $json = array(
			"mob" => "",
			"pas" => "",
			"userid" => "",
			"token" => "",
			"min" => "",
			"max" => "",
			"return" => ""
		  );
		}
		$data_string = array(
			"code" => 200,
			"msg" => "成功",
			"data" => $json
		);
		$data_string = json_encode($data_string);
		die($data_string);
	case "save":
		if(isset($_POST["mob"]) && isset($_POST["pas"]) && isset($_POST["min"]) && isset($_POST["max"])){
			$json = file_get_contents('cache/json.txt');
			if ($json){
				$json = json_decode($json,true);
			}else{
				$json = array(
					"mob" => "",
					"pas" => "",
					"userid" => "",
					"token" => "",
					"min" => "",
					"max" => "",
					"return" => ""
				);
			}
			$json["mob"] = $_POST["mob"];
			$json["pas"] = $_POST["pas"];
			$json["min"] = $_POST["min"];
			$json["max"] = $_POST["max"];
			$json = json_encode($json,true);
			$fp = fopen('cache/json.txt', 'w');
			fwrite($fp, $json);
			fclose($fp);
		}else{
			die("ERROR");
		}
    	$data_string = array(
			"code" => 200,
		);
		$data_string = json_encode($data_string);
		die($data_string);
	case "run":
    	die(run());
}

function login(){
	$json = file_get_contents('cache/json.txt');
	if (!$json){
		return false;
	}
	$json = json_decode($json,true);
	$token = $json['token'];
	$url = "https://account-cn.huami.com/v1/client/app_tokens?app_name=com.xiaomi.hm.health&dn=api-user.huami.com%2Capi-mifit.huami.com%2Capp-analytics.huami.com&login_token=" . $token;
	$return_content = post_json_data($url,"","",2);
	$return_content = json_decode($return_content, true);
	if(isset($return_content['token_info']['app_token'])){
		return $return_content['token_info']['app_token'];
	}else{
		$mob = $json['mob'];
		$pas = $json['pas'];
		$url = "https://api-user.huami.com/registrations/+86" . $mob . "/tokens";
		$data_string = array(
			"client_id" => "HuaMi",
			"password" => $pas,
			"redirect_uri" => "https://s3-us-west-2.amazonaws.com/hm-registration/successsignin.html",
			"token" => "access"
		);
		$data_string = http_build_query($data_string);
		if(post_json_data($url,$data_string,"",0)){
			$json = file_get_contents('cache/json.txt');
			$json = json_decode($json,true);
			$token = $json['token'];
			$url = "https://account-cn.huami.com/v1/client/app_tokens?app_name=com.xiaomi.hm.health&dn=api-user.huami.com%2Capi-mifit.huami.com%2Capp-analytics.huami.com&login_token=" . $token;
			$return_content = post_json_data($url,"","",2);
			$return_content = json_decode($return_content, true);
			if(isset($return_content['token_info']['app_token'])){
				return $return_content['token_info']['app_token'];
			}
		}
		return false;
	}
}

function run(){
	$token = login();
	$json = file_get_contents('cache/json.txt');
	$json = json_decode($json,true);
	if (!$token){
		$data_string = array(
			"code" => 401,
			"message" => "账号或密码错误"
		);
		$data_string = json_encode($data_string);
		$json["return"] = "账号或密码错误";
		$json = json_encode($json,true);
		$fp = fopen('cache/json.txt', 'w');
		fwrite($fp, $json);
		fclose($fp);
		return $data_string;
	}
	$userid = $json["userid"];
	$min = $json["min"];
	$max = $json["max"];
	$step = mt_rand($min,$max);
	$data = date("Y-m-d");
	$qq = '[{"summary":"{\"slp\":{\"ss\":73,\"lt\":304,\"dt\":0,\"st\":1589920140,\"lb\":36,\"dp\":92,\"is\":208,\"rhr\":0,\"stage\":[{\"start\":269,\"stop\":357,\"mode\":2},{\"start\":358,\"stop\":380,\"mode\":3},{\"start\":381,\"stop\":407,\"mode\":2},{\"start\":408,\"stop\":423,\"mode\":3},{\"start\":424,\"stop\":488,\"mode\":2},{\"start\":489,\"stop\":502,\"mode\":3},{\"start\":503,\"stop\":512,\"mode\":2},{\"start\":513,\"stop\":522,\"mode\":3},{\"start\":523,\"stop\":568,\"mode\":2},{\"start\":569,\"stop\":581,\"mode\":3},{\"start\":582,\"stop\":638,\"mode\":2},{\"start\":639,\"stop\":654,\"mode\":3},{\"start\":655,\"stop\":665,\"mode\":2}],\"ed\":1589943900,\"wk\":0,\"wc\":0},\"tz\":\"28800\",\"stp\":{\"runCal\":1,\"cal\":6,\"conAct\":0,\"stage\":[],\"ttl\":';
	$aa = $step . ',\"dis\":144,\"rn\":0,\"wk\":5,\"runDist\":4,\"ncal\":0},\"v\":5,\"goal\":8000}","data":[{"stop":1439,"value":"WhQAUA0AUAAAUAAAUAAAUAAAUAAAWhQAUAYAcBEAUAYAUA8AUAsAUAYAUDIAUCQAUDkAUCkAUD4AUC0AUFcAUD8AUCkAUCEAUCwAUCsAUB4AUCQAUBsAUCcAUBQAUDcAUBoAUCYAUFcAUCAAUDkAUCEAWhQAWhQAWhQAUBAAUEgAUDsAUAgAWhQAUDwAUCEAUAIAUAsAUDoAUD8AWhQAWhQAWhQAWhQAWhQAWhQAAS0QEAsAWhQAAR8SEBcHYC4AUCoAUBMAUAIAUAYAUAsAUCsAUAUAUBIAUBIAUBsAUBgAUAoAUBsAUBUAUBkAUDIAUC0AUC4AUBAAWhQAUCsAUB8AUAIAUB8AUDUAUEEAUDUAUBkAUCYAUEoAUCYAUBIAUCAAUCkAUDAAUB4AUB0AUDEAUCUAUCgAUAQAWhQAUA8AUDwAUB8AUCUAUBQAUB4AUAUAWhQAUAAAUA8AUBkAUCgAUCwAUCkAUCgAYCIAYCIAYCgAUAoAWhQAUBwAWhQAUBoAUDkAUD4AYAkAYAYAWhQAWhQAUB4AWhQAUAQAUBcAUBAAUAUAWhQAUB0AcBYAehQAcBoAehQAehQAehQAcAMAcAMAehQAcAIAehQAcBIAcA0AehQAehQAcAsAcAYAcAEAcAoAehQAehQAcAwAehQAehQAehQAcAEAehQAehQAcAsAehQAehQAcA8AcBkAcAYAcBkAcC0AcAQAcBsAcAMAWhQAUAMAWhQAUBEAUAIAWhQAWhQAWhQAehQAehQAehQAehQAehQAehQAcAAAcB8AcBMAehQAehQAcDkAcBAAcAEAcAMAcAMAcCwAcA8AcAAAcAAAcCIAcAAAcCcAcB4AehQAcAkAehQAcCMAehQAehQAcAoAehQAehQAehQAcBgAcBgAcAkAehQAcAcAcCgAcBQAcA0AcAwAcCcAcCkAcAAAUAAAUAAAUB4AUBwAUAAAUAAAUCkAUBIAUBMAUCgAUA8AUBEAUD0AUCAAYAMAYCkAUBsAUB4AYCgAahQAUBkAWhQAWhQAUCAAUBcAUA8AUBAAUAcAUB8AUCEAUCMAUCkAYAMAYAAAUBsAUBEAUBgAUAUAUB0AUAAAUAAAUAAAUAAAUAAAUAQAUAAAUAAAUAAAUAAAWwAAUAAAcAAAcAAAcAAAcAAAcAAAcAAAcA0AcAAAcAAAcAAAcAIAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcA8AehQAehQAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAEAeRMAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAsAcAAAcAAAcAAAcAAAcAAAcAoAcAAAcBMAcAAAcAAAcAAAcAAAcAAAcAAAcA4AcAcAehQAehQAcAAAcAAAcAIAehQAehQAcAAAcAAAcAAAcAAAcAAAcAIAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcBcAehQAehQAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAehQAcAMAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcBUAeQAAcAAAcAAAcAAAcFgAcAAAcAAAcAAAcBkAeQAAcAAAcAAAcAAAcAAAcE0AcAQAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAeVAAehQAehQAcAAAcAAAcAAAcAAAcAUAeRwAUAAAUFUAUAAAUAAAUAAAUAAAUAAAUCMAeQAAcAAAcAAAcE0AUAAAUAAAUAAAUAAAUAAAUAAAcAAAcAAAcAAAcE4AcAAAcAAAcAAAcAAAcAgAcBAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAkAcAAAcAAAcAAAcAAAcBwAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAYAcBAAeQAAcB8AeQAAcAAAcAAAcAAAeSoAcAAAcAAAcAAAcAAAcAAAcAsAcAAAeScAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcCAAcAAAUAAAUAAAUAAAUAAAUAAAUBEAehQAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcBwAehQAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcBYAcAAAcAAAcAAAcAYAcAAAcAAAcCsAcAAAcAAAcAgAcAAAcAAAcBsAeRQAcAAAcAAAcAEAcAAAcAAAcAAAcAAAcAAAcAAAcA8AcAAAcAAAcBoAcAAAcAEAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAQAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcBIAcAAAcA0AcBAAcAAAcAAAcAAAcAAAehQAehQAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcAAAcCgAcAAAcBkAcAAAcB0AcAAAcAAAcBgAcAAAUAEAUBsAWhQAUB4AWhQAUCkAWQ8AUCsAUA0AWTUAXBAAWhQAUBMAUAQAUAcAUAoAUA8AUBkAUBcAUCoAUAIAUBQAWhQAWhQAUBIAUBQAUAcAWhQAUBYAWhQAUAgAWhQAWhQAUAkAUE0AUHUAAWMTEEcKYDoAYAgAUAMAWhQAUAUAUAYAUAkAUB4AUAsAUAIAUBMAWhQAAVQdAWAlEDYAYCQAUAQAUBgAUAgAUAUAUBQAUAIAWhQAUAkAUAMAUA4AWhQAehQAcAoAcAIAehQAcB0AcCcAUCsAUAEAUAgAUAoAUAIAUAsAUAIAWhQAWhQAUAgAUA0AWhQAUAYAWhQAUAEAWhQAWhQAUBAAUBQAUBIAUBcAUAoAYBAAYAIAAUkZAUglAVYSYBcAYAoAYCAAYAsAUBUAUB0AUBAAUBEAUCAAUBUAUBYAUA0AUB4AUBcAUBsAUBMAUBUAYAsAYAwAYAsAUB4AUBoAUBoAUBoAUBQAUAcAWhQAUBgAUBkAUBsAUBUAUBAAUCAAUCYAUB8AUB4AUBwAUAcAUBsAUBwAUBwAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAAfgAA","did":"321123","tz":32,"src":17,"start":0}],"data_hr":"\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+\/v7+","summary_hr":"{\"ct\":0,\"id\":[]}","date":"';
	$zz = $data . '"}]';
  	$data_string = $qq . $aa . $zz;
	$data_string = 'userid=' . $userid . '&last_sync_data_time=1589917081&device_type=0&last_deviceid=DA932FFFFE8816E7&data_json=' . $data_string;
	$url = "https://api-mifit-cn.huami.com/v1/data/band_data.json?&t=" . time() . "456";
	$return_content = post_json_data($url,$data_string,$token,1);
	$return_content = json_decode($return_content, true);
	if ($return_content['code'] == 1){
		$data_string = array(
			"code" => 200,
			"message" => "已成功修改为" . $step . "步"
		);
		$data_string = json_encode($data_string);
		$json["return"] = "已成功修改为" . $step . "步";
		$json = json_encode($json,true);
		$fp = fopen('cache/json.txt', 'w');
		fwrite($fp, $json);
		fclose($fp);
	}else{
		$data_string = array(
			"code" => 401,
			"message" => "修改失败"
		);
		$data_string = json_encode($data_string);
		$json["return"] = "修改失败";
		$json = json_encode($json,true);
		$fp = fopen('cache/json.txt', 'w');
		fwrite($fp, $json);
		fclose($fp);
	}
	return $data_string;
}
function convertUrlQuery($query){
	$queryParts = explode('&', $query);
	$params = array();
	foreach ($queryParts as $param) {
		$item = explode('=', $param);
		$params[$item[0]] = $item[1];
	}
	return $params;
}
function post_json_data($url, $data_string, $cookie="", $mode=0) {
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_POST, 1);
    curl_setopt($ch, CURLOPT_URL, $url);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
	if($cookie){
		curl_setopt($ch, CURLOPT_HTTPHEADER, array(
			'apptoken: ' . $cookie,
			'User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 13_4_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/15E148 MicroMessenger/7.0.12(0x17000c2d) NetType/WIFI Language/zh_CN'
		));
    }else{
		curl_setopt($ch, CURLOPT_HTTPHEADER, array(
			'Content-Type: application/x-www-form-urlencoded;charset=UTF-8',
			'User-Agent: MiFit/4.6.0 (iPhone; iOS 14.0.1; Scale/2.00)'
		));
	}
    curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
	curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, 0);
	curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, 0);
	switch ($mode) {
		case 1://getlogintoken
			$return_content = curl_exec($ch);
			curl_close($ch);
			return $return_content;
		case 2://getapptoken
			curl_setopt($ch, CURLOPT_POST, 0);
			$return_content = curl_exec($ch);
			curl_close($ch);
			return $return_content;
		default ://login
			curl_exec($ch);
			$Headers =  curl_getinfo($ch);
			$content = curl_exec($ch);
			$url = parse_url($Headers['redirect_url']);
			$param_arr = convertUrlQuery($url['query']);
			if(isset($param_arr['access'])){
				$return_content = $param_arr['access'];
				$url = "https://account.huami.com/v2/client/login";
				$data_string = array(
					"app_name" => "com.xiaomi.hm.health",
					"app_version" => "4.6.0",
					"code" => $return_content,
					"country_code" => "CN",
					"device_id" => "2C8B4939-0CCD-4E94-8CBA-CB8EA6E613A1",
					"device_model" => "phone",
					"grant_type" => "access_token",
					"third_name" => "huami_phone",
				);
				$data_string = http_build_query($data_string);
				$return_content = post_json_data($url,$data_string,"",1);
				$return_content = json_decode($return_content, true);
				$json = file_get_contents('cache/json.txt');
				$json = json_decode($json,true);
				$json["token"] = $return_content["token_info"]["login_token"];
				$json["userid"] = $return_content["token_info"]["user_id"];
				$json = json_encode($json,true);
				$fp = fopen('cache/json.txt', 'w');
				fwrite($fp, $json);
				fclose($fp);
				$return_content = true;
			}else{
				$return_content = false;
			}
			curl_close($ch);
			return $return_content;
	}
}
?>
```
