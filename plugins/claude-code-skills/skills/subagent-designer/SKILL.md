---
name: subagent-designer
description: Design focused Claude Code subagents (.claude/agents/*.md) with clear trigger descriptions, least-privilege tools, model choice and a tight system prompt, then test delegation. Use for "create a subagent", "reviewer agent", "split work into agents".
---

# Subagent Designer

## 1. Decide if a subagent fits
Good: isolated, repeatable tasks that would flood main context (code review, test running, research, log analysis). Bad: tiny one-off steps or tasks needing the full conversation.

## 2. Spec (confirm with user)
- Job in one sentence; inputs it receives; output format it returns (short, structured).
- Scope: project `.claude/agents/` or user `~/.claude/agents/`.

## 3. Write the file
```markdown
---
name: code-reviewer
description: Reviews changed code for bugs, security and style. Use PROACTIVELY after code changes or when asked to review.
tools: Read, Grep, Glob, Bash
model: sonnet
---
You are a senior reviewer. Steps: 1) `git diff` 2) ... Return: table `Severity | File:line | Issue | Fix`, max 10 items.
```
Rules:
- Description = when to use + trigger words ("use proactively after…").
- Tools: minimum needed; omit Write/Edit for reviewers. Omitting `tools` inherits all - avoid.
- Model: cheaper/faster for mechanical work, stronger for reasoning.
- Prompt: role, numbered steps, stop conditions, exact output format, what NOT to do.

## 4. Test
- Explicit: "Use the code-reviewer subagent on my last change."
- Implicit: a prompt that should auto-delegate; confirm it triggers and doesn't over-trigger.
- Check the returned output is compact and actionable.

## 5. Deliver
Agent file(s), one example invocation each, and a note on when NOT to use them.
