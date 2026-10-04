# ChatGPT → Vercel → Telegram relay

Relay for sending important notifications from ChatGPT to Telegram through a Vercel protected endpoint.

## Authentication and deployment protection

The project uses Vercel Deployment Protection with **All Deployments** and Vercel Authentication. Keep this protection enabled for production domains. The Vercel `web_fetch_vercel_url` connector obtains the Vercel automation bypass internally when it fetches a protected deployment. Do not pass a relay key, bot token, chat ID, cookies, or bypass token in the URL or connector arguments.

The app requires these environment variables in Vercel:

- `TELEGRAM_BOT_TOKEN` — Telegram bot token.
- `TELEGRAM_CHAT_ID` — destination chat ID.

`RELAY_KEY` is no longer used.

## Sending a notification

Call `web_fetch_vercel_url(full_url)` with a URL in this shape:

```text
https://chatgpt-telegram-relay.vercel.app/api/send?text=<URL-encoded-message>
```

Optional buttons use:

- `url` and `button`
- `url2` and `button2`
- `url3` and `button3`

Each URL must use HTTPS. If a button label is omitted, a default label is used. Example with one HTTPS button:

```text
https://chatgpt-telegram-relay.vercel.app/api/send?text=<URL-encoded-message>&url=https%3A%2F%2Fexample.com%2Fmail%2F123&button=Открыть%20письмо
```

Always URL-encode parameter values, especially message text and button labels. The endpoint accepts GET and POST, requires a non-empty message, limits messages to 3500 Unicode code points and button labels to 64 Unicode code points, applies an 8 second Telegram API timeout, and returns HTTP 200 only when Telegram responds with a successful `ok: true`. Failed Telegram requests return a non-2xx response.

## Local development

```bash
vercel dev
```

Set `TELEGRAM_BOT_TOKEN` and `TELEGRAM_CHAT_ID` in the local environment. Do not commit credentials.
