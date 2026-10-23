# 🦞 NarzXclaw

A private, local-first personal AI assistant. The brain (Ollama), memory (SQLite), and voice (Whisper + Piper)
all run on your machine. Chat with it from Telegram, Discord, Slack, WhatsApp, or by voice from your phone.

## Quick start

```bash
# 1. Install Ollama (https://ollama.com), then:

unzip narzxclaw.zip && cd narzxclaw
pip install -e .            # add '.[voice]' for speech-to-text
narzxclaw onboard           # guided setup: model, chat apps, voice
narzxclaw run               # wake the lobster
narzxclaw service           # optional: install as an always-on background service
```

`narzxclaw doctor` checks everything. `narzxclaw allow <channel> <id>` lets another person in.

## How it works

```
Telegram / Discord / Slack ──(outbound connections)──┐
WhatsApp ── Baileys bridge (node) ──► local gateway ─┼─► Brain ─► Ollama
Phone browser/PWA (text + voice) ─► local gateway ───┘      └─► SQLite memory
```

- Telegram uses polling, Slack uses Socket Mode, Discord uses its gateway: all outbound, so **no open ports or public URLs**.
- Only senders on the per-channel **allowlist** get answers. Strangers get their ID so you can approve them.
- Config (including bot tokens) lives in `~/.narzxclaw/config.json` with `0600` permissions.

## Phone voice (iPhone + Android)

The gateway serves a small web app (installable via "Add to Home Screen") with a mic button.
Browsers only allow the microphone over HTTPS, so use Tailscale, which keeps everything private to your devices:

1. Install Tailscale on this machine and your phone.
2. `tailscale serve --bg 8787`
3. Open the `https://<machine>.<tailnet>.ts.net` URL on your phone, paste the token from `narzxclaw token`.

Voice setup: `pip install -e '.[voice]'`, install [Piper](https://github.com/rhasspy/piper) and download a `.onnx` voice,
then run `narzxclaw onboard` and enable voice. Telegram voice notes are transcribed too.

## Honest caveats

- **"Local" covers the AI and your data, not the chat networks.** Messages to and from WhatsApp, Telegram, etc. still pass
  through those companies' servers (WhatsApp/Signal-style end-to-end encryption notwithstanding).
- **WhatsApp uses the unofficial Baileys library.** It works by acting as a linked device. Meta can restrict accounts that do this, so use a spare number if you care about the account.
- This is a v0.1 starter: it has chat + memory + voice, but no tools (web search, calendar, files) yet.
  The natural next step is adding tool-calling in `brain.py`.
- The chat adapters were written against current library APIs but could not be run against live accounts in the build environment, so expect to fix small things on first run (`narzxclaw doctor` helps).
- Always on requires the machine to stay awake; a spare mini PC, Raspberry Pi 5, or Mac mini works well.
