# VPS SSH Watch

Free VPS availability monitor via GitHub Actions.

Every 10 minutes it tries to connect to the VPS over SSH. If SSH is unavailable, it sends a Telegram message.

## Files

- `.github/workflows/vps-ssh-watch.yml` - GitHub Actions monitor.

## Telegram setup

1. Open Telegram bot [@BotFather](https://t.me/BotFather).
2. Send `/newbot`.
3. Create a bot and copy its token.
4. Open your new bot and send `/start`.
5. Open this URL in browser, replacing the token:

```text
https://api.telegram.org/botTELEGRAM_BOT_TOKEN/getUpdates
```

6. Find `chat.id` in the response. This is `TELEGRAM_CHAT_ID`.

Test message:

```text
https://api.telegram.org/botTELEGRAM_BOT_TOKEN/sendMessage?chat_id=TELEGRAM_CHAT_ID&text=test
```

## GitHub secrets

In the GitHub repository:

```text
Settings -> Secrets and variables -> Actions -> New repository secret
```

Add:

```text
VPS_HOST
VPS_USER
VPS_PORT
VPS_SSH_KEY
TELEGRAM_BOT_TOKEN
TELEGRAM_CHAT_ID
```

`VPS_PORT` can be omitted if SSH uses port `22`.

`VPS_SSH_KEY` must be a private key that can SSH into the server. Do not paste it into code or logs.

## Recommended SSH key

Create a separate deploy/check key for monitoring:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/vps_watch_key -C "vps-watch"
```

Add the public key to the VPS:

```bash
ssh-copy-id -i ~/.ssh/vps_watch_key.pub user@host
```

Then put the private key content into the GitHub secret:

```bash
cat ~/.ssh/vps_watch_key
```

## Manual run

After pushing this project to GitHub:

```text
Actions -> VPS SSH Watch -> Run workflow
```

If SSH is unavailable, the bot sends a Telegram notification.
