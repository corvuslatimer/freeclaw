# FreeClaw

Minimal, unrestricted agent runtime.

## Scope
- Providers: OpenAI, Anthropic
- Channel: Telegram
- Tools: exec, read, write, fetch
- Execution: root-first (`requireRoot: true`)
- Personality/memory context files: `SOUL.md`, `TOOLS.md`, `MEMORY.md`, `AGENTS.md`, `USER.md`, `HEARTBEAT.md`

## Quick start
```bash
cp freeclaw.config.example.json freeclaw.config.json
# set keys/tokens
npm run start
```
