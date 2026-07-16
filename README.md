# @adebamsbot — AI Image Generator Telegram Bot

Sends an AI-generated image back for any text prompt you send it, using the
free Pollinations.ai image generation API (no API key required).

## 1. Create the bot on Telegram

1. Open Telegram, message **@BotFather**.
2. Send `/newbot`.
3. Give it a display name (e.g. `Adebams Bot`).
4. Set the username to `adebamsbot` (must end in "bot").
5. BotFather will give you a **token** — save it, you'll need it below.

## 2. Push this project to GitHub

```bash
cd adebamsbot
git init
git add .
git commit -m "Initial commit: AI image generator bot"
git branch -M main
git remote add origin https://github.com/<your-username>/adebamsbot.git
git push -u origin main
```

## 3. Deploy on Railway

1. Go to https://railway.app and sign in with GitHub.
2. Click **New Project → Deploy from GitHub repo** → select `adebamsbot`.
3. Once created, go to your service's **Variables** tab and add:
   - `TELEGRAM_BOT_TOKEN` = the token BotFather gave you
4. Railway will detect the `Procfile` and run `python bot.py` as a **worker**
   process (no web port needed since this bot uses polling).
5. Deploy. Check the **Deployments → Logs** tab — you should see
   `Bot starting...`.

## 4. Test it

Open Telegram, search `@adebamsbot`, hit Start, then send any prompt like:

```
a cat riding a skateboard in space, digital art
```

You should get an image back in a few seconds.

## Notes

- Pollinations.ai is free and requires no key, but quality/speed can vary.
  If you want higher quality later, swap the `generate_image` function to
  call OpenAI's DALL·E, Stability AI, or another provider — you'll need to
  add their API key as another Railway environment variable.
- The bot uses polling, not webhooks, so it just needs to stay running —
  no public URL required.
