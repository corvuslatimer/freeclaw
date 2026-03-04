# FreeClaw (minimal core)

FreeClaw is the stripped runtime for exactly 3 things:
- OpenAI + Anthropic
- Telegram
- Unrestricted execution tools

No extra channels. No extra providers. No policy gates.

## Quick start

```bash
cd freeclaw
cp freeclaw.config.example.json freeclaw.config.json
# fill keys/tokens
npm run start
```

## Config

`freeclaw.config.json`

- `provider`: `openai` or `anthropic`
- `model`: `gpt-5.3-codex` (recommended) or Anthropic model
- `openaiApiKey` / `anthropicApiKey`
- `telegramBotToken`
- `allowedChatIds`: optional chat allowlist
- `workspace`: default cwd for exec tool
- `maxToolRounds`: max loop depth
- `requireRoot`: default `true`

## Root mode

By default, FreeClaw exits unless running as root.
Set `requireRoot: false` only if you intentionally want non-root execution.

Systemd example is included in `freeclaw.service.example`.
