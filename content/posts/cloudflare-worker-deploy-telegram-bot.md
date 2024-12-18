+++
title = "通过 Cloudflare Worker 部署 Telegram Bot"
date = 2024-12-18T14:00:00+08:00
description = "通过 Cloudflare Worker 部署 Telegram Bot"
[taxonomies]
tags = ["cloudflare", "Worker", "telegram", "bot"]
+++

## 1 创建 Telegram Bot

1. 在 Telegram 中搜索 “@BotFather”。
2. 向 BotFather 发送 `/newbot` 命令。
3. 按照提示设置 bot 的名称和用户名。
4. 完成后，您将收到一个 API Token，请妥善保存。

## 2 通过 Cloudflare Worker 部署 Bot

### 2.1 代码示例

```js
addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request));
});

async function handleRequest(request) {
  if (request.method === 'POST') {
    try {
      const body = await request.json();
      console.log(body);
      const TOKEN = globalThis.TELE_BOT_TOKEN;
      const url = `https://api.telegram.org/bot${TOKEN}/sendMessage`;

      const response = await fetch(url, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          chat_id: body.message.chat.id,
          text: `You said: ${body.message.text}`,
        }),
      });

      return new Response('OK', { status: 200 });
    } catch (error) {
      return new Response('Error processing request', { status: 500 });
    }
  }
  return new Response('Invalid request', { status: 400 });
}
```

### 2.2 部署步骤

1. 登录 Cloudflare 账户，创建新的 Worker。
2. 将上述代码粘贴到 Worker 编辑器中。
3. 在 Worker 的设置中添加环境变量 `TELE_BOT_TOKEN`，值为您的 Telegram Bot API Token。
4. 部署 Worker。

### 2.3 设置 Webhook

部署完成后，需要设置 Webhook 以便 Telegram 服务器能够向您的 Worker 发送更新。

使用以下命令设置 Webhook（请在打开代理或国外服务器上运行，以避免访问不了的错误）：

```shell
export TELE_BOT_TOKEN=your-bot-token
curl -F "url=https://your-worker-subdomain.workers.dev" https://api.telegram.org/bot$TELE_BOT_TOKEN/setWebhook
```

替换 `your-worker-subdomain` 为实际值。

验证 Webhook 设置：

```shell
curl https://api.telegram.org/bot$TELE_BOT_TOKEN/getWebhookInfo
```

## 3 Cloudflare Worker 使用注意事项

1. **环境变量访问**：使用 `globalThis.` 前缀访问环境变量，例如：

```js
const TOKEN = globalThis.TELE_BOT_TOKEN;
```

2. **代码保存**：有时保存可能不会立即生效，需要重新进入并粘贴代码。
3. **调试**：可以切换到 HTTP 功能，输入测试数据进行调试。
4. **部署**：只有在预览模式下才能点击"部署"按钮。HTTP 模式下无法部署。

通过以上步骤，您就可以成功创建并部署一个基于 Cloudflare Worker 的 Telegram Bot。
