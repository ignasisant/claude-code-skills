---
name: claude-hooks-builder
description: Design, write and test Claude Code hooks (PreToolUse, PostToolUse, UserPromptSubmit, Stop, SessionStart, Notification, PreCompact) - auto-format, run tests, block risky commands, inject context, notify. Use for "add a hook", "auto-run lint after edits", "block rm -rf".
---

# Claude Code Hooks Builder

## 1. Clarify the goal
Map the request to an event:
| Goal | Event | Matcher |
|---|---|---|
| Block/validate a tool call | PreToolUse | `Bash`, `Edit|Write`, `mcp__.*` |
| Format/lint/test after edits | PostToolUse | `Edit|Write|MultiEdit` |
| Add context or reject prompts | UserPromptSubmit | — |
| Enforce "done" criteria | Stop / SubagentStop | — |
| Load project context | SessionStart | `startup|resume` |
| Desktop/Slack alert | Notification | — |
| Save state before compaction | PreCompact | — |

## 2. Choose scope
- `.claude/settings.json` (team, committed) · `.claude/settings.local.json` (personal) · `~/.claude/settings.json` (all projects).

## 3. Write the hook
```json
{"hooks":{"PostToolUse":[{"matcher":"Edit|Write","hooks":[{"type":"command","command":"\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/format.sh","timeout":30}]}]}}
```
Script rules:
- Read JSON from stdin (`jq -r '.tool_input.file_path'`), never trust it blindly; quote all paths.
- Exit 0 = ok; exit 2 = block, stderr is shown to Claude; other codes = non-blocking error.
- Advanced: print JSON `{"decision":"block","reason":"..."}` or `hookSpecificOutput.permissionDecision` (allow/deny/ask).
- Fast (<2 s typical), idempotent, no network unless needed. `chmod +x`.
- Skip files outside the project and generated dirs (node_modules, dist).

## 4. Test
- Pipe a sample payload: `echo '{"tool_name":"Bash","tool_input":{"command":"rm -rf /"}}' | ./hook.sh; echo $?`.
- Run `/hooks` in Claude Code to confirm it's registered; trigger it once for real.

## 5. Deliver
Settings JSON diff, script files, test command, and a 3-line note: what it does, how to disable, known limits.

## Safety
Hooks run with the user's permissions. Show the script before installing; never download remote scripts into hooks.
