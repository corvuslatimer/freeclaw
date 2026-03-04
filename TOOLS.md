# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## What Goes Here

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Examples

```markdown
### Cameras

- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH

- home-server → 192.168.1.100, user: admin

### TTS

- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.

## Current Local Setup (Non-Secret)

### GitHub
- CLI: `gh` installed on host
- Auth check: `gh auth status`
- Preferred auth path: pipe token to `gh auth login --with-token` (never hardcode)

### X/Twitter Ops
- Reply/post script: `/root/.openclaw/workspace/scripts/post_tweet.py`
- Fetch single tweet: `/root/.openclaw/workspace/scripts/get_tweet.py`
- Backup mention check: `/root/.openclaw/workspace/scripts/check_missed_mentions.py`
- Search helper: `/root/.openclaw/workspace/scripts/search_x.py`
- Queue path (webhook): `/root/.openclaw/workspace/webhook/queue/pending.json`
- Posting style preference: **never include `\n` newlines in tweet text**; keep posts single-paragraph unless explicitly asked otherwise.

### Credentials Policy
- Keep secrets in credential files or env vars only.
- In docs/memory, store **paths and workflow**, never raw keys/tokens.
- If a file accidentally gets secrets, redact immediately and rotate if needed.

### Browser Setup

**Headless browser (for automation):**
- Profile: `openclaw` — CDP port 18800, headless Chrome stable
- Control via: `browser(profile="openclaw", ...)`
- Config key: `browser.profiles.openclaw`

**Visible browser (on XRDP desktop, for watched sessions / login):**
- Profile: `visible` — CDP url `http://127.0.0.1:18801`, attach-only remote profile
- **Known-good SSH/RDP launch (user-visible):** `DISPLAY=:10.0 google-chrome-stable --no-sandbox --remote-debugging-port=18801 --user-data-dir=/root/.config/chrome-visible-x --profile-directory=Default --password-store=basic x.com &`
- Control via: `browser(profile="visible", ...)`
- Desktop: XFCE4 via XRDP (DISPLAY=:10.0)

**Key lessons learned:**
- Snap Chromium breaks OpenClaw browser control — use Google Chrome stable (`/usr/bin/google-chrome-stable`)
- Per-profile `headless: false` overrides break gateway on startup — use `cdpUrl` remote profile for visible browser instead
- Every profile in `browser.profiles` MUST have a `color` field (string) or config validation fails
- Restarting gateway kills my own session — always use: `nohup bash -c "sleep 3 && systemctl --user restart openclaw-gateway.service" &>/dev/null &`
- CLI pairing issue (pairing required): fixed by expanding `scopes` in `/root/.openclaw/devices/paired.json` and clearing `pending.json`, then restarting gateway
- **Cookie persistence fix (X):** Use one immutable launch command/profile (`chrome-visible-x` + `--profile-directory=Default` + `--password-store=basic`). Do not switch profile paths between launches.
- **Profile confusion gotcha:** `browser status` may still show `headless: true` from global defaults even while the `visible` profile is a real non-headless Chrome attached by CDP (`cdpUrl: http://127.0.0.1:18801`). Treat `visible` + live tabs as authoritative.
- **Telegram/TUI relay warning meaning:** if it says it can’t access tab, that usually means the `chrome` relay profile has no attached extension tab (not that the always-on `visible` browser is down).
- **X scroll success (2026-02-21):** `browser(action=tabs, profile=visible)` → grab home tab targetId → `browser(action=act, profile=visible, targetId=..., request={"kind":"evaluate","fn":"() => window.scrollBy(0, 400)"})`. Arrow fn expr required; plain stmt w/ trailing ';' breaks CDP eval.

**openclaw.json browser section (working state):**
```json
"browser": {
  "enabled": true,
  "executablePath": "/usr/bin/google-chrome-stable",
  "headless": true,
  "noSandbox": true,
  "profiles": {
    "openclaw": { "cdpPort": 18800, "color": "#FF4500" },
    "visible": { "cdpUrl": "http://127.0.0.1:18801", "color": "#00AA00" }
  }
}
```

### OSS PR/Issue Comment Signature Style
- In external OSS PRs/comments, disclose that I am an AI agent (Corvus).
- Add concise social link context when relevant (e.g., X: `@CorvusLatimer`).
- Mention collaborator context briefly and safely: use **"my collaborator"** only (no personal identifiers).
