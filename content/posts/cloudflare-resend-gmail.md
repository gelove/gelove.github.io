+++
title = "通过 cloudflare 创建无限数量企业邮箱"
date = 2024-12-15T21:00:00+08:00
description = "通过 cloudflare 创建无限数量企业邮箱"
[taxonomies]
tags = ["cloudflare", "resend", "email"]
+++

如何在互联网大善人 cloudflare 那里白嫖无限数量的企业邮箱，这些邮箱既可以收邮件，也可以发邮件。

可以用来接收网站验证码，注册账户的时候就可以很方便的注册一大堆小号。 还可以把他们当成临时邮箱，与人通信，避免暴露自己真实的邮箱，保护个人隐私。 Cloudflare 是一家提供 CDN、网络安全、DDos 防御和域名服务的公司，人称互联网大善人、赛博活佛。

## 域名

使用 Cloudflare 的前提是要有一个域名。关于域名的购买，可以看一下这期视频。

有了域名以后，我们就把它托管到 Cloudflare 上。我们先登录一下 Cloudflare，没有账号就注册一个。右上角点击添加站点，输入你的域名，

![image](/images/cloudflare_1.jpg)

然后点击继续。这里有一些付费的，我们都不要，直接找下面这个免费的，点击继续。

![image](/images/cloudflare_2.jpg)

更改 DNS (名称服务器)，找到这两个名称服务器。

![image](/images/cloudflare_3.jpg)

我们需要去购买域名的网站更改，把原有的 DNS 地址全部删掉，然后写入之前分配的两个服务器，点击保存。

![image](/images/cloudflare_4.jpg)

我们再回到 Cloudflare，点击立即检查，然后点击继续。这里什么都不用动，直接开始使用就可以了。

左上角点击 Cloudflare 的图标，回到首页，域名这里显示一个绿勾和有效，那就可以开始直接使用了。

![image](/images/cloudflare_5.jpg)

## 收邮件

我们先点击域名，进来以后选择左侧电子邮件，选择电子邮件路由。

第一次进来以后，会有一个新手设置，点击取消新手设置，点击启用电子邮件路由。

![image](/images/cloudflare_6.jpg)

这里面有 4 个 DNS 值，我们点击添加记录并启用，Cloudflare 就会自动添加上这 4 个 DNS 记录。

![image](/images/cloudflare_7.jpg)

我们再点击路由规则，这里有一个 catch-all 地址，我们点击右侧的编辑，

![image](/images/cloudflare_8.jpg)

点击发送到电子邮件。这里填写一个自己的电子邮箱，然后点击保存。

![image](/images/cloudflare_9.jpg)

保存好以后，这里会出现一个待验证。

![image](/images/cloudflare_10.jpg)

Cloudflare 会发送一封验证邮件到这个地址，进入邮箱找到邮件并点击验证链接。刷新一下页面，打上一个绿勾，就配置完成了。

![image](/images/cloudflare_11.jpg)

**这个配置的意思是所有发送到当前域名下的邮件都会自动转到这个 163 邮箱**。这样就实现了无限多个邮箱可以收邮件。

## 发邮件

使用邮箱发送邮件，使用的服务是**[resend.com](https://resend.com)**，这也是一个免费的服务，没有账号请自行注册。

选择左侧的 API Keys，点击 create API Key，名字可填写你的域名，下面两个保持默认，点击添加。

![image](/images/cloudflare_12.jpg)

![image](/images/cloudflare_13.jpg)

就会获得一个 API Key，把它保存好。这里有一个域名，我们需要把域名也添加到这个网站上，就是之前托管到 Cloudflare 的那个域名。

![image](/images/cloudflare_14.jpg)

**好，这一步是重点**。这里给了 3 个 DNS 记录，我们需要把这个记录填回 Cloudflare

![image](/images/cloudflare_15.jpg)

回到 cloudflare

![image](/images/cloudflare_16.jpg)

![image](/images/cloudflare_17.jpg)

我们回到 resend，有一个按钮 verify DNS records，我们点击一下，检查我们之前的配置是否正确。等校验完，页面标成绿色，那就配置完成了。

![image](/images/cloudflare_18.jpg)

### 使用 Python 发邮件

```python
# 先安装依赖
# pip install resend
import resend
# 这里换成自己的resend API Key
resend.api_key = "re_xxxxxxxxxxxxxxxxxxxxxxx"

params: resend.Emails.SendParams = {
   # 发件人可以是自己域名下的任何一个人
  "from": "admin@你的域名",
  "to": ["geloves@163.com"],
  "subject": "hi",
  "html": "<strong>it works!</strong>"
}

email = resend.Emails.send(params)
print(email)
```

### 使用 cURL 发邮件

Authorization 换成自己的 resend API Key

发件人可以是自己域名下的任何一个人

```shell
export RESEND_API_KEY="re_xxxxxxxxxxxxxxxxxxxxxxx"
curl -X POST 'https://api.resend.com/emails' -H "Authorization: Bearer ${RESEND_API_KEY}" -H 'Content-Type: application/json' -d '{"from": "Admin <admin@你的域名>","to":["test@163.com"],"subject":"hi","text": "it works!"}'
```
