---
name: session-handoff
description: Save a Claude session's state before context runs out or at end of day - goal, decisions, changed files, open issues, exact next steps - into HANDOFF.md, and resume cleanly in a new session. Use for "handoff", "save progress", "continue tomorrow", "context almost full".
---

# Session Handoff

## Save (write `HANDOFF.md` at repo root or project folder)
```markdown
# Handoff — <date> — <task>
## Goal
One sentence + acceptance criteria.
## Status
Done / In progress / Not started (bullets).
## Decisions (and why)
- ...
## Files touched
- path — what changed
## Commands
Build/test/run commands that work; current branch; uncommitted changes (`git status --short`).
## Open issues / risks
- ...
## Next steps (ordered, each doable in one go)
1. ...
## Don't
Dead ends already tried.
```
Rules: facts only, verified (run `git status`, tests) before writing; max ~80 lines; no secrets.

## Resume
1. Read `HANDOFF.md` (+ CLAUDE.md).
2. Verify state matches (branch, tests). Report differences.
3. Confirm the first next step with the user, then continue. Update HANDOFF.md as steps complete; delete or archive it when the task is done.

Tip: pair with a PreCompact hook that reminds Claude to refresh HANDOFF.md.
