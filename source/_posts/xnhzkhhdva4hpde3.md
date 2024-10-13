---
title: php实现使用github授权登录
urlname: xnhzkhhdva4hpde3
date: '2024-10-11 06:02:25 +0000'
tags:
  - php
  - github
  - 一键登录
categories: 学无止境
copyright_author_href: 'https://www.xiaohuihui.cc'
copyright_url:
copyright_author:
cover:
---

## <font style="color:rgb(51, 51, 51);">github 官网文档</font>

[https://docs.github.com/cn/developers/apps/building-oauth-apps/authorizing-oauth-apps#web-application-flow](https://docs.github.com/cn/developers/apps/building-oauth-apps/authorizing-oauth-apps#web-application-flow)<font style="color:rgb(51, 51, 51);">  
</font>

### <font style="color:rgb(51, 51, 51);">原理</font>

- <font style="color:rgba(0, 0, 0, 0.75);">客户端发起请求 redirect 到 OAuth 接入方并附带上 client_id</font>
- <font style="color:rgba(0, 0, 0, 0.75);">用户在 redirect 之后的网站上输入用户名和密码</font>
- <font style="color:rgba(0, 0, 0, 0.75);">登陆成功之后，OAuth 接入方会返回给服务端一个 code。</font>
- <font style="color:rgba(0, 0, 0, 0.75);">服务端拿到 code 之后，拿着 client_secret 和 code 向 OAuth 接入方申请获得 Token</font>
- <font style="color:rgba(0, 0, 0, 0.75);">服务端拿到 Token 之后，进入授权窗口</font>
- <font style="color:rgba(0, 0, 0, 0.75);">授权成功，跳转到客户端网站。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/27022430/1728626938756-2f6539e4-dba9-476e-8ca8-61dc3fb10fd8.png)

#### <font style="color:rgb(79, 79, 79);">创建好 github 账号后点击设置</font>

![](https://cdn.nlark.com/yuque/0/2024/png/27022430/1728626976527-973ecdc4-e078-454d-81b4-4365bf2c998b.png)

#### <font style="color:rgb(79, 79, 79);">之后进入前期工作</font>

# github 页面开通

### 步骤一

![](https://cdn.nlark.com/yuque/0/2024/png/27022430/1728627062227-f0276371-2ff7-4910-bd7d-605820df6446.png)

### 步骤二

![](https://cdn.nlark.com/yuque/0/2024/png/27022430/1728627111878-52336048-2df9-4b5b-a7b3-1fdafa01f479.png)

### **<font style="color:rgb(79, 79, 79);">步骤三</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/27022430/1728627155897-03023d7e-80f6-4597-b4d5-b290cfffa70f.png)

### **<font style="color:rgb(79, 79, 79);">步骤四添加好必填项</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/27022430/1728627210018-3fec0d9f-cc6d-41f5-8d3e-f93d3963395f.png)

注册应用后会生成 client_id， 然后需要创建密码

![](https://cdn.nlark.com/yuque/0/2024/png/27022430/1728627252705-9847427d-9340-4086-9913-e760fb5f17cd.png)

```html
#页面首页地址（登录成功后跳转到的地址）：
http://huicmf.frp.toushizhiku.com:18080/app/user
#授权回调地址（代码中处理函数逻辑的请求地址）：
http://huicmf.frp.toushizhiku.com:18080/app/user/github
```

# 代码实现

#### <font style="color:rgb(79, 79, 79);">配置好之后接下来是代码实现</font>

![](https://cdn.nlark.com/yuque/0/2024/png/27022430/1728627329489-26ac1d11-49a6-44cb-9d59-ede2f1ee892e.png)

```html
<a
  class="tooltips"
  href="https://github.com/login/oauth/authorize?client_id=Ov23li51234Of&redirect_uri=%2F"
  title="使用GitHub一键登录"
>
  <img src="__STATIC__/images/github.png" alt="GitHub" />
</a>
```

### 方法函数

```php
public function github(Request $request)
  {
    try {
      $time = time();
      // 如果已登录，直接跳转
      if ($request->session()->has('session_userInfo')) {
        return redirect('/app/user');
      }
      // GitHub应用程序的客户端ID和密钥
      $client_id     = "";
      $client_secret = "";
      $redirect_uri  = $request->get('redirect_url', $request->path());
      $code          = $request->get('code');
      if (empty($code)) {
        return redirect('https://github.com/login/oauth/authorize?client_id='.$client_id.'&redirect_url='.urlencode($redirect_uri).'&scope=user');
      }
      $options      = [
        'http' => [
        'header'  => "Content-Type: application/x-www-form-urlencoded\r\nAccept: application/json\r\nUser-Agent: {$request->header('user-agent')}\r\n",
        'method'  => 'POST',
        'content' => http_build_query([
                                      'client_id'     => $client_id,
                                      'client_secret' => $client_secret,
                                      'code'          => $code
                                      ])
        ]
        ];
      $context      = stream_context_create($options);
      $response     = file_get_contents('https://github.com/login/oauth/access_token', false, $context);
      $params       = json_decode($response, true);
      $access_token = $params['access_token'];
      // 使用访问令牌访问GitHub API
      $options   = array(
        'http' => array(
          'header'  => "Authorization: Bearer $access_token\r\nAccept: application/json\r\nUser-Agent: {$request->header('user-agent')}\r\n",
          'method'  => 'GET',
          'timeout' => 120
        )
      );
      $context   = stream_context_create($options);
      $user_info = file_get_contents('https://api.github.com/user', false, $context);
      $user_data = json_decode($user_info, true);

      $getIp = $request->getRealIp();
      $user  = UserModel::where(['email' => $user_data['email']])->find();

      //这一部分是授权成功后处理用户信息（写入或更新数据表数据，可参考，改成自己的）
      if (empty($user)) {
        $data        = [
          'username'    => $user_data['login'],
          'nickname'    => $user_data['name'],
          'email'       => $user_data['email'],
          'avatar'      => $user_data['avatar_url'],
          'last_time'   => $time,
          'last_ip'     => $getIp,
          'status'      => 1,
          'join_time'   => $time,
          'join_ip'     => $getIp,
          'check_email' => 1,
          'mobile'      => "",
          'level'       => 0,
          'role'        => 0,
          'end_time'    => 0
                ];
                $user        = UserModel::create($data);
                $sessionUser = $user->toArray();
            } else {
                $user->last_ip   = $getIp;
                $user->last_time = $time;
                $user->save();

                $sessionUser                = $user->toArray();
                $sessionUser['last_ip']     = $getIp;
                $sessionUser['last_time']   = $time;
                $sessionUser['update_time'] = $time;
                unset($sessionUser['password']);
                unset($sessionUser['salt']);
            }
      // end

            $request->session()->set('session_userInfo', $sessionUser);

            return redirect($redirect_uri);
        } catch (\Throwable $throwable) {
            return $this->transfer('github授权出错了');
        }
    }

    protected function transfer(string $msg, int $time = 3000)
    {
        return response("<div style=\"display: flex;justify-content: center;padding: 30px;width: 100%;color: #F56E0DFF;\">$msg</div><script>setTimeout(function(){
    window.location.href = '/';
}, {$time})</script>");
    }
```

完成，收工。
