---
name: claude-plugin-packager
description: Package skills, slash commands, subagents, hooks and MCP servers into a Claude Code plugin with plugin.json, a marketplace.json, versioning and README, then validate and test-install it. Use for "make this a plugin", "publish my skills as a plugin marketplace".
---

# Claude Code Plugin Packager

## 1. Inventory
Find what to ship: `.claude/skills/*/SKILL.md`, `.claude/commands/*.md`, `.claude/agents/*.md`, hooks in settings, `.mcp.json`. Confirm the list with the user.

## 2. Layout
```
my-plugin/
  .claude-plugin/plugin.json
  skills/<name>/SKILL.md
  commands/<name>.md
  agents/<name>.md
  hooks/hooks.json
  .mcp.json
  README.md  CHANGELOG.md  LICENSE
```
Hooks/MCP paths use `${CLAUDE_PLUGIN_ROOT}`, never absolute paths.

## 3. plugin.json
```json
{"name":"my-plugin","version":"1.0.0","description":"One line of value","author":{"name":"..."},"license":"MIT","keywords":["..."]}
```
Kebab-case name; semver; description says what + when.

## 4. Marketplace (optional)
Repo root `.claude-plugin/marketplace.json`:
```json
{"name":"my-marketplace","owner":{"name":"..."},"plugins":[{"name":"my-plugin","source":"./my-plugin","description":"..."}]}
```

## 5. Quality gate
- Every skill has name + trigger-rich description; no secrets; no hardcoded user paths.
- Commands have `description` frontmatter; agents have scoped `tools`.
- Hook scripts executable and quoted.

## 6. Test install
`/plugin marketplace add ./path` → `/plugin install my-plugin@my-marketplace` → run one skill, one command, trigger one hook. Fix and bump version.

## 7. Deliver
Tree, files, install instructions for users (`/plugin marketplace add <github-user/repo>`), and a changelog entry.
