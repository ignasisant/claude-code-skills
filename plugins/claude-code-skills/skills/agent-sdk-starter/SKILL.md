---
name: agent-sdk-starter
description: Scaffold a production-ready agent on the Claude Agent SDK (Python or TypeScript) - custom tools, MCP servers, permissions, sessions, streaming, logging and deployment. Use for "build an agent with the Claude Agent SDK", "custom agent backend".
---

# Agent SDK Starter

## 1. Spec
Agent's job, users, tools it needs (custom functions, MCP servers, files, shell, web), where it runs (CLI, API, worker), language (Python `claude-agent-sdk` / TS `@anthropic-ai/claude-agent-sdk`).

## 2. Scaffold
```
agent/
  src/agent.(py|ts)      # options + query loop
  src/tools/             # custom tools, one file each
  prompts/system.md
  tests/
  .env.example           # ANTHROPIC_API_KEY
```

## 3. Core
- Options: system prompt from file, `allowed_tools`/`allowedTools` minimal, permission mode explicit, `max_turns`, working dir.
- Custom tools via the SDK's in-process MCP server helper; strict input schemas, compact text outputs, clear errors.
- Stream messages; log tool calls, tokens and cost per run.
- Sessions: persist session id to resume conversations.

## 4. Safety
Deny destructive tools by default; `canUseTool`/permission callback for sensitive actions; never log secrets; timeouts on every tool.

## 5. Test
Unit tests for tools; 5 scripted end-to-end prompts; check cost per run.

## 6. Deploy
Minimal HTTP wrapper (FastAPI/Express) or queue worker, Dockerfile, health check, env-based config.

Verify against current SDK docs for exact option names before finalizing. Deliver: repo tree, code, run commands, sample transcript.
