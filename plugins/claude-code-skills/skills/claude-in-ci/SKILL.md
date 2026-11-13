---
name: claude-in-ci
description: Run Claude Code headless in CI (GitHub Actions, GitLab) - PR review, fix failing tests, triage issues, update changelog - with scoped permissions, turn/budget limits and safe secrets. Use for "Claude in GitHub Actions", "automate PR review", "claude -p in CI".
---

# Claude in CI

## 1. Pick the job
PR review comment · fix failing tests on a branch · issue triage/labels · changelog/release notes · scheduled dependency or docs refresh.

## 2. GitHub Actions (preferred: official action)
```yaml
on: { pull_request: { types: [opened, synchronize] } }
permissions: { contents: read, pull-requests: write }
jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: "Review this PR for bugs and security. Comment max 10 findings."
          claude_args: "--max-turns 8 --allowedTools Read,Grep,Glob,Bash(git diff:*)"
```
Generic CLI: `claude -p "<task>" --output-format json --max-turns N --allowedTools ...`.

## 3. Guardrails
- Least-privilege `permissions:` and `--allowedTools`; never blanket-skip permissions on untrusted input.
- Don't run on fork PRs with secrets; use `pull_request` not `pull_request_target` unless you understand the risk.
- Cap `--max-turns`, set job `timeout-minutes`, add `concurrency` to cancel stale runs.
- Keep CLAUDE.md with build/test commands so runs are cheap and correct.
- Write changes to a branch + PR, never push to main.

## 4. GitLab / others
Install CLI in the job image (`npm i -g @anthropic-ai/claude-code`), run `claude -p`, post results via API.

## 5. Verify
Open a test PR, confirm the run, cost/turns and output. Deliver workflow file, required secrets, and a rollback note.
