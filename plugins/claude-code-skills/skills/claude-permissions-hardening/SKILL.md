---
name: claude-permissions-hardening
description: Audit and harden Claude Code permissions - allow/ask/deny rules, default mode, extra directories, secrets protection, sandbox and network - producing a safe per-project settings.json. Use for "secure Claude Code", "stop it reading .env", "safe permission setup".
---

# Claude Code Permissions Hardening

## 1. Audit
Read `~/.claude/settings.json`, `.claude/settings.json`, `.claude/settings.local.json` (and managed settings if present). Report:
- Overly broad allows (`Bash(*)`, `Bash(curl:*)`, `WebFetch` everywhere).
- Missing denies for secrets and destructive commands.
- `defaultMode` too permissive; extra directories outside the project.
- Hooks/MCP servers that run unknown code.

## 2. Baseline (adapt to the stack)
```json
{
  "permissions": {
    "allow": ["Bash(npm run test:*)", "Bash(npm run lint:*)", "Bash(git status)", "Bash(git diff:*)"],
    "ask":   ["Bash(git push:*)", "Bash(npm install:*)"],
    "deny":  ["Read(./.env)", "Read(./.env.*)", "Read(./secrets/**)", "Read(~/.ssh/**)", "Read(~/.aws/**)",
              "Bash(rm -rf:*)", "Bash(curl:*)", "Bash(wget:*)", "Bash(sudo:*)"]
  }
}
```
Team rules → `.claude/settings.json` (committed); personal → `.local`.
Consider enabling sandboxing (filesystem/network isolation) where supported.

## 3. Defense in depth
- PreToolUse hook to block patterns rules can't express.
- Keep secrets out of repo; use env/secret manager.
- Review MCP servers: source, scopes, tokens.

## 4. Verify
Try a denied read and a denied command; confirm they're blocked; confirm normal workflow (tests, lint) isn't slowed by prompts.

Deliver: findings table (Risk | Where | Fix), new settings files, and what changed for the user's daily flow. Check current docs for exact rule syntax.
