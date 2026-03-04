# FreeClaw Bootstrap Plan

## Goal
Create a radically simplified OpenClaw fork with unrestricted agent execution (root-capable, no sandbox/allowlist policy gates).

## v0 Scope
1. Single runtime process
2. Single chat interface (Telegram first)
3. Single agent loop (LLM -> tool call -> result -> LLM)
4. Unrestricted tools:
   - shell exec
   - read/write/edit files
   - HTTP fetch
5. Minimal persistence (JSON session + logs)
6. No safety policy enforcement in tool dispatcher

## Code Strategy
- Keep the fork as base for attribution + reuse.
- Build a new thin runtime path under `src/freeclaw/`.
- Add a dedicated launch command entrypoint for FreeClaw.
- Avoid touching unrelated OpenClaw modules initially.

## Milestones
- M1: boot + config load + provider call
- M2: tool loop + unrestricted exec
- M3: Telegram ingress/egress
- M4: logging + restart resilience
- M5: packaging (Docker/systemd)
