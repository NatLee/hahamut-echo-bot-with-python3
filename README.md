# Hahamut Echo Bot with python3

> [!WARNING]
> **Archived: the API this bot talks to no longer exists.**
> Bahamut [announced on 2024-05-21](https://forum.gamer.com.tw/C.php?bsn=60404&snA=39690) that Hahamut's Bot feature (神秘好友) would stop, then [replaced Hahamut with 巴哈姆特 Now!](https://forum.gamer.com.tw/C.php?bsn=60404&snA=39749), which lists bots as removed. Hahamut itself shut down on 2024-07-22.
> Checked again on 2026-11-04: the `messagePush` endpoint returns 404 and `haha.gamer.com.tw` redirects to Bahamut's "missing page".
> The repo stays up, read-only, as a reference. The code has nothing left to connect to.

A tiny `echo` bot for Hahamut (哈哈姆特不EY), the chat service Bahamut used to run at `haha.gamer.com.tw`. Whatever you sent the bot, it sent straight back. That's the whole feature.

![The bot echoing "123" back in a Hahamut chat, 2021-03-25](./docs/example.png)

## How it worked

Hahamut bots used webhooks, much like LINE or Messenger bots:

1. You registered a bot in Hahamut's developer dashboard, got an access token, and pointed the bot's webhook at wherever `run.py` was reachable. Heroku or an `ngrok` tunnel was the usual trick.
2. When someone messaged the bot, Hahamut POSTed JSON like this to `/`:

   ```json
   {
     "botid": "bot@<ID>",
     "time": 1616654861527,
     "messaging": [
       { "sender_id": "<SENDER_ID>", "message": { "text": "123" } }
     ]
   }
   ```

3. `run.py`, a small Flask app, read the text and the sender, then POSTed the same text back to Hahamut's `messagePush` endpoint with `?access_token=<TOKEN>`.

`config.yml` held the token, the app secret and the host/port Flask listened on. If you borrow this code for something similar, note that Hahamut also signed every webhook call (an `x-baha-data-signature` header, HMAC-SHA1 of the body with the app secret), and `run.py` never checked it.

## Running it (historical)

These steps worked while Hahamut was alive:

```bash
pip install -r requirements.txt
# put your bot's token in config.yml
python3 run.py
```

## Links

- [Hahamut bot API docs](https://sites.google.com/gamer.com.tw/hahamut-bot) (the official docs, still online as of 2026-11-04 even though the API is gone)
- [Bot shutdown announcement, 2024-05-21](https://forum.gamer.com.tw/C.php?bsn=60404&snA=39690)
- [巴哈姆特 Now! replacing Hahamut, 2024-06-21](https://forum.gamer.com.tw/C.php?bsn=60404&snA=39749)
