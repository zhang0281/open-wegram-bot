# Open Wegram Bot - OWB
## 一个让人呼吸顺畅的 Telegram 双向私聊机器人 🤖（零费用）

简体中文 | [English](README_EN.md)

这是一个基于 Cloudflare Worker 的 Telegram 双向私聊机器人，无需服务器、无需数据库、无需自己的域名即可轻松部署。

用户可以通过您的机器人向您发送消息，您可以直接回复这些消息，实现双向通信。

## ✨ 特色功能

- 🔄 **双向通信** - 轻松接收和回复来自用户的消息
- 💾 **无需数据库** - 完全无状态设计，零存储成本
- 🌐 **无需自己的域名** - 使用 Cloudflare Worker 提供的免费域名
- 🚀 **轻量级部署** - 几分钟内即可完成设置
- 💰 **零成本运行** - 在 Cloudflare 免费计划范围内使用
- 🔒 **安全可靠** - 使用 Telegram 官方 API 和安全令牌
- 🔌 **多机器人支持** - 一个部署可注册多个私聊机器人

## 🛠️ 前置要求

- Cloudflare 账号
- Telegram 账号
- 一个科学工具（仅设置阶段需要，用于访问 Worker 默认域名，自绑域名无视）

## 📝 设置步骤

### 1. 获取 Telegram UID

> [!NOTE]
> 您需要知道自己的 Telegram 用户 ID (UID)，这是一串数字，用于将消息转发给您。

您可以通过以下方式获取：

- 向 [@userinfobot](https://t.me/userinfobot) 发送任意消息，它会告诉您自己的 UID

请记下您的数字 ID（例如：`123456789`）。

### 2. 创建 Telegram Bot

1. 在 Telegram 中搜索并打开 [@BotFather](https://t.me/BotFather)
2. 发送 `/newbot` 命令
3. 按照提示设置您的机器人名称和用户名（用户名必须以 `bot` 结尾）
4. 成功后，BotFather 会发给您一个 Bot API Token（格式类似：`000000000:ABCDEFGhijklmnopqrstuvwxyz`）
5. 请安全保存这个 Bot API Token

### 3. 部署 Cloudflare Worker

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. 导航到 `Workers & Pages` 页面
3. 点击 `Create Worker` 创建新的 Worker
4. 为您的 Worker 取一个名称（例如：`open-wegram-bot`）
5. 删除默认代码，将本项目的代码 `worker.js` 粘贴进去
6. 在代码开头的 `CONFIG` 对象中修改参数：

```js
const CONFIG = {
    prefix: 'public',  // 自定义URL前缀，用于防止他人随意使用您的服务
    secretToken: '请替换为您自己的安全令牌'  // 必须包含大写字母、小写字母和数字，长度至少16位
};
```

> [!IMPORTANT]
> `prefix` 参数作为URL路径的一部分，可以防止他人随意使用您的服务。您可以将其设置为任何字符串，只有知道这个前缀的人才能注册/使用您的服务。这样您可以安全地分享给信任的朋友，而不必担心被滥用。

7. 点击 `Save and Deploy` 保存并部署

### 3.1 (可选) 绑定自定义域名 🌐

> [!TIP]
> 为您的 Worker 绑定自定义域名可以避免使用科学工具访问，更加便捷！

Cloudflare 允许您将自己的域名绑定到 Worker 上，这样您就可以通过自己的域名访问 Worker，而不需要使用被和谐的默认域名。

如果您打算将您的机器人作为公共服务提供，或者希望更方便地管理机器人，则建议配置自定义域名。

绑定后，您可以使用类似 `https://your-domain.com/YOUR_PREFIX/install/...` 的地址来注册/卸载机器人，无需科学工具。

### 4. 注册您的 Telegram Bot

部署 Worker 后，您将获得一个形如 `https://your-worker-name.your-subdomain.workers.dev` 的 URL。

现在您需要注册您的 Bot：

> [!WARNING]
> 由于 Cloudflare Workers 默认域名被和谐，此步骤需要科学。如果您已绑定自定义域名，可以直接使用您的域名进行访问，无需科学工具。

1. 在浏览器中访问以下 URL 来注册您的 Bot（替换相应参数）：

```
https://your-worker-name.your-subdomain.workers.dev/YOUR_PREFIX/install/YOUR_TELEGRAM_UID/BOT_API_TOKEN
```

或者如果您绑定了自定义域名：

```
https://your-domain.com/YOUR_PREFIX/install/YOUR_TELEGRAM_UID/BOT_API_TOKEN
```

例如：
```
https://telegram-private-msg.username.workers.dev/public/install/123456789/000000000:ABCDEFGhijklmnopqrstuvwxyz
```

2. 如果看到成功消息，说明您的 Bot 已经注册成功

> [!NOTE]
> 一个 Worker 实例可以注册多个不同的 Bot！只需重复上述注册步骤，使用不同的 Bot API Token 即可。

## 📱 使用方法

### 接收消息 📩

一旦设置完成，任何人给您的 Bot 发送消息，您都会在自己的 Telegram 账号中收到这些消息，并且消息下方会显示发送者的信息。

### 回复消息 📤

要回复某个用户的消息：
1. 在 Telegram 中找到您想回复的转发消息
2. 直接回复该消息（使用 Telegram 的回复功能）
3. 您的回复会被自动发送给原始发送者

### 卸载 Bot ❌

如果您想卸载 Bot，请访问以下 URL（替换相应参数）：

```
https://your-worker-name.your-subdomain.workers.dev/YOUR_PREFIX/uninstall/BOT_API_TOKEN
```

或者如果您使用自定义域名：

```
https://your-domain.com/YOUR_PREFIX/uninstall/BOT_API_TOKEN
```

## 🔒 安全说明

> [!IMPORTANT]
> 请妥善保管您的 Bot API Token 和安全令牌（Secret Token），这些信息关系到您服务的安全性。

> [!WARNING]
> **请勿随意更改已设置的 Secret Token！** 更改后，所有已注册的机器人将无法正常工作，因为无法匹配原来的令牌。如需更改，所有机器人都需要重新注册。

- 在初始设置时选择一个安全且便于记忆的 Secret Token
- 避免使用简单或常见的前缀名称
- 不要将敏感信息分享给他人

## ⚠️ 使用限制

> [!NOTE]
> Cloudflare Worker 免费套餐有每日 10 万请求的限制。

对于个人使用的私聊机器人来说，这个限制通常足够宽松。即使您注册了多个机器人，除非您的机器人极其活跃，否则不太可能达到这个限制。

如果您预计使用量较大，可以考虑升级到 Cloudflare 的付费计划。

## 🔍 故障排除

- **消息未转发**: 确保 Bot 已正确注册，并检查 Worker 日志
- **无法访问注册 URL**: 确认您是否相信科学，或者考虑绑定自定义域名解决访问问题
- **回复消息失败**: 检查您是否正确使用 Telegram 的回复功能
- **注册失败**: 确保您的 `secretToken` 符合要求（包含大小写字母和数字，长度至少16位）

## 🤝 贡献与联系

如果您有任何问题、建议或想贡献代码，请提 Issue/PR 或通过以下方式联系我：

- [LINUX DO](https://linux.do)

## 📄 许可证

- GPL v3，希望你能完善并继续开源，而不是改头换面闭源，谢谢。

---

希望这个工具能让您的 Telegram 私聊体验更加便捷！🎉 如果你只想直接使用，请访问 [@WegramBot](https://t.me/wegram_bot)