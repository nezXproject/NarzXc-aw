# 🦞 NarzXclaw v1.0

A private, local-first personal AI assistant. The brain (Ollama), memory (SQLite) and voice (Whisper + Piper) run on your
machine. Talk to it from Telegram, Discord, Slack, WhatsApp, a phone web app, or your terminal.

## Quick start

```bash
./install.sh                # or: pip install -e '.[voice]'
narzxclaw chat              # talk to it right now, no accounts needed
narzxclaw onboard           # guided setup: model, personality, chat apps, voice
narzxclaw run               # DM your bot → pairing code → narzxclaw approve CODE
narzxclaw service           # always on
```

Docker: `docker compose run --rm narzxclaw onboard && docker compose up -d` (pull a model with `docker compose exec ollama ollama pull llama3.1:8b`).

## What it can do

| Power | Details |
|---|---|
| Memory | Remembers facts you tell it (`/memory`, `/forget <id>`), kept in a local SQLite file |
| Reminders | “Remind me in 10 minutes…” arrives in the same chat app, or the web app |
| Files | Reads/writes a private workspace folder only (`~/.narzxclaw/workspace`) |
| Math & time | Exact calculator and clock |
| Web (opt-in) | DuckDuckGo search and page fetch; blocks private/LAN addresses |
| Voice | Voice notes on Telegram/WhatsApp/Discord; tap-to-talk in the phone web app |
| Personalities | `/mode lobster`, `captain`, `zen`, `pro`; replies in the language you write in |

Tool calling needs a model that supports it (Llama 3.1+, Qwen 2.5 and similar). Other models fall back to plain chat automatically.

## Pairing and safety

- Unknown senders get a 6-digit code; you run `narzxclaw approve CODE`. It takes effect instantly. Codes expire after 1 hour.
- Gateway binds to `127.0.0.1`; every API call needs a bearer token. Phone access: `tailscale serve --bg 8787`.
- Telegram, Slack and Discord connect outbound only. No open ports.
- Rate limit per sender. File tools can't leave the workspace. Web fetch refuses loopback/LAN targets.
- `narzxclaw export` dumps your data; `narzxclaw wipe` deletes it.

## Voice

`pip install -e '.[voice]'` then `narzxclaw voice-setup en` (or `id` for Indonesian), enable voice in `onboard`.
For Indonesian speech recognition keep the default `small` Whisper model (multilingual).

## Honest caveats

- “Local” covers the AI and your data, not the chat networks: messages still pass through WhatsApp/Telegram/etc.
- WhatsApp uses the unofficial Baileys library; Meta can restrict accounts that use it. Use a spare number if that matters.
- The core logic is covered by unit tests (`python -m unittest discover -s tests`). The chat adapters, WhatsApp bridge,
  voice pipeline and Docker image could not be exercised against real accounts or hardware in the build environment,
  so run `narzxclaw doctor` and `narzxclaw chat` first and expect small first-run fixes.
- The Piper Indonesian voice URL in `voice-setup` was not verified; if it fails, pick a voice from the
  `rhasspy/piper-voices` repo and set `voice.piper_model` in the config.
- Web search parses DuckDuckGo's HTML page, which can change without notice.
