+++
title = "抖音视频下载与音视频转码工具"
date = 2024-11-19T22:00:00+08:00
description = "抖音视频下载与音视频转码工具"
[taxonomies]
tags = ["rust", "tauri2", "douyin"]
+++

Downloader 是一个跨平台的抖音视频下载器，目前支持 macOS 与 windows，并具有音视频转码功能。

**由 Tauri2 + Rust + React + Typescript 开发**

## 下载安装

[macOS-apple](https://bin.616161616.xyz/Downloader_1.0.5_aarch64.dmg)

[macOS-intel](https://bin.616161616.xyz/Downloader_1.0.5_x64.dmg)

[windows-x64](https://bin.616161616.xyz/Downloader_1.0.5_x64-setup.exe)

**MacOS 用户安装程序时出现以下警告是正常的, 请点击取消即可**

![image-1](/images/downloader_1.png)

**然后在设置中允许打开就 OK 了**

![image-2](/images/downloader_2.png)

**然后会再次弹出一次警告**

![image-3](/images/downloader_3.png)

**程序打开报损坏则继续在设置中允许打开或者在终端输入以下命令**

```sh
xattr -c /Applications/Downloader.app
```

## Cookie 获取教程

1. 打开浏览器，访问 [抖音](https://www.douyin.com)
2. 登录抖音账号
3. 按 **_F12_** 打开开发人员工具
4. 选择 **_网络_** 选项卡
5. 勾选 **_保留日志_**
6. 在 **_筛选器_** 输入框输入 **_cookie-name:odin_tt_**
7. 访问 **_抖音网页版_** 中任意用户主页 或者访问 [关注页](https://www.douyin.com/follow)
8. 在开发人员工具窗口选择任意一个数据包\(如果无数据包，重复步骤 7\)
9. 全选并复制 **_Cookie_** 的值
10. 运行程序 ，根据提示写入 **_Cookie_**

### 截图示例

![cookie](/images/downloader_cookie.png)
