# Open Wegram Bot - OWB
## A Smooth-Operating Two-Way Private Messaging Telegram Bot 🤖 (Zero Cost)

[简体中文](README.md) | English

This is a two-way private messaging Telegram bot based on Cloudflare Worker that can be easily deployed without requiring a server, database, or your own domain name.

Users can send messages to you through your bot, and you can reply directly to these messages, enabling two-way communication.

## ✨ Features

- 🔄 **Two-Way Communication** - Easily receive and reply to messages from users
- 💾 **No Database Required** - Completely stateless design, zero storage costs
- 🌐 **No Personal Domain Required** - Use the free domain provided by Cloudflare Worker
- 🚀 **Lightweight Deployment** - Complete setup within minutes
- 💰 **Zero Running Cost** - Operates within Cloudflare's free plan limits
- 🔒 **Secure and Reliable** - Uses official Telegram API and secure tokens
- 🔌 **Multiple Bot Support** - Register multiple private chat bots with a single deployment

## 🛠️ Prerequisites

- Cloudflare account
- Telegram account

## 📝 Setup Steps

### 1. Get Your Telegram UID

> [!NOTE]
> You need to know your Telegram user ID (UID), which is a string of numbers used to forward messages to you.

You can get it by:

- Sending any message to [@userinfobot](https://t.me/userinfobot), which will tell you your UID

Note down your numeric ID (e.g., `123456789`).

### 2. Create a Telegram Bot

1. Search for and open [@BotFather](https://t.me/BotFather) in Telegram
2. Send the `/newbot` command
3. Follow the prompts to set your bot's name and username (username must end with `bot`)
4. Upon success, BotFather will send you a Bot API Token (format similar to: `000000000:ABCDEFGhijklmnopqrstuvwxyz`)
5. Securely save this Bot API Token

### 3. Deploy Cloudflare Worker

1. Log into the [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. Navigate to the `Workers & Pages` page
3. Click `Create Worker` to create a new Worker
4. Give your Worker a name (e.g., `open-wegram-bot`)
5. Delete the default code and paste the code from this project's `worker.js`
6. Modify the parameters in the `CONFIG` object at the beginning of the code:

```js
const CONFIG = {
    prefix: 'public',  // Custom URL prefix to prevent unauthorized use of your service
    secretToken: 'Replace with your own security token'  // Must contain uppercase letters, lowercase letters, and numbers, at least 16 characters long
};
```

> [!IMPORTANT]
> The `prefix` parameter serves as part of the URL path to prevent unauthorized use of your service. You can set it to any string, and only those who know this prefix can register/use your service. This way, you can safely share it with trusted friends without worrying about misuse.

7. Click `Save and Deploy` to save and deploy

### 3.1 (Optional) Bind a Custom Domain 🌐

> [!TIP]
> Binding a custom domain to your Worker provides more convenient access!

Cloudflare allows you to bind your own domain to your Worker, enabling you to access the Worker through your domain.

If you plan to offer your bot as a public service or want more convenient bot management, setting up a custom domain is recommended.

After binding, you can use addresses like `https://your-domain.com/YOUR_PREFIX/install/...` to register/uninstall bots.

### 4. Register Your Telegram Bot

After deploying the Worker, you'll get a URL like `https://your-worker-name.your-subdomain.workers.dev`.

Now you need to register your Bot:

1. Visit the following URL in your browser to register your Bot (replace the parameters accordingly):

```
https://your-worker-name.your-subdomain.workers.dev/YOUR_PREFIX/install/YOUR_TELEGRAM_UID/BOT_API_TOKEN
```

Or if you've bound a custom domain:

```
https://your-domain.com/YOUR_PREFIX/install/YOUR_TELEGRAM_UID/BOT_API_TOKEN
```

For example:
```
https://telegram-private-msg.username.workers.dev/public/install/123456789/000000000:ABCDEFGhijklmnopqrstuvwxyz
```

2. If you see a success message, your Bot has been successfully registered

> [!NOTE]
> One Worker instance can register multiple different Bots! Just repeat the above registration steps using different Bot API Tokens.

## 📱 How to Use

### Receiving Messages 📩

Once set up, any messages sent to your Bot will be forwarded to your Telegram account, with sender information displayed below the message.

### Replying to Messages 📤

To reply to a user's message:
1. Find the forwarded message you want to reply to in Telegram
2. Reply directly to that message (using Telegram's reply function)
3. Your reply will be automatically sent to the original sender

### Uninstalling the Bot ❌

If you want to uninstall the Bot, visit the following URL (replace with your parameters):

```
https://your-worker-name.your-subdomain.workers.dev/YOUR_PREFIX/uninstall/BOT_API_TOKEN
```

Or if you're using a custom domain:

```
https://your-domain.com/YOUR_PREFIX/uninstall/BOT_API_TOKEN
```

## 🔒 Security Notes

> [!IMPORTANT]
> Please keep your Bot API Token and Secret Token secure, as they are crucial to your service's security.

> [!WARNING]
> **Do not change your Secret Token once it's set!** After changing, all registered bots will stop working properly as they cannot match the original token. If you need to change it, all bots will need to be re-registered.

- Choose a secure and memorable Secret Token during initial setup
- Avoid using simple or common prefix names
- Do not share sensitive information with others

## ⚠️ Usage Limitations

> [!NOTE]
> Cloudflare Worker's free plan has a limit of 100,000 requests per day.

For a personal private chat bot, this limit is usually generous enough. Even if you register multiple bots, you're unlikely to reach this limit unless your bots are extremely active.

If you anticipate higher usage, consider upgrading to Cloudflare's paid plan.

## 🔍 Troubleshooting

- **Messages not forwarded**: Ensure the Bot is correctly registered and check the Worker logs
- **Cannot access registration URL**: Try using a custom domain to solve access issues
- **Reply message fails**: Check if you're correctly using Telegram's reply function
- **Registration fails**: Ensure your `secretToken` meets the requirements (contains uppercase and lowercase letters and numbers, at least 16 characters long)

## 🤝 Contributions and Contact

If you have any questions, suggestions, or want to contribute code, please submit an Issue/PR or contact me at:

- [LINUX DO](https://linux.do)

## 📄 License

- GPL v3, hoping you'll improve and continue to open source rather than rebranding and closing the source. Thank you.

---

Hope this tool makes your Telegram private messaging experience more convenient! 🎉 If you just want to use it directly, please visit [@WegramBot](https://t.me/wegram_bot)