---
name: artifact-design-review
description: Structured review of a UI prototype or Claude artifact (canvas, HTML, screenshot) against the brief, usability heuristics and WCAG AA, producing prioritized fixes and applying them in place. Use for "review this design", "critique the prototype", "is this ready to build?".
---

# Artifact Design Review

Goal: turn "looks ok?" into a prioritized, actionable fix list, then iterate the same artifact.

## Inputs
- The artifact (link, HTML, or screenshot) — render it; never review from source alone.
- The brief/acceptance criteria if they exist. If not, infer the goal in one line and confirm.

## 1. Render and capture
- Screenshot at 1440px and 390px wide (Playwright or browser). Check light and dark if supported.
- Click through key interactions and states (empty, loading, error, filled).

## 2. Evaluate (score each 1–5, one line of evidence)
1. **Goal fit** — does the main screen serve the primary task in <5 s?
2. **Hierarchy** — one clear primary action per view; scan path obvious.
3. **Consistency** — spacing scale, type scale, colors, component reuse.
4. **Content** — real copy, clear labels/CTAs, helpful empty/error text.
5. **States** — all states designed, none missing.
6. **Responsive** — no horizontal scroll at 390px; touch targets ≥44px.
7. **Accessibility** — contrast ≥4.5:1 text / 3:1 UI, focus visible, labels, no color-only meaning.
8. **Buildability** — maps to existing components/tokens; nothing exotic without reason.

## 3. Findings
Table: `# | Severity (Blocker/Major/Minor) | Where | Problem | Fix`.
Max 10 items, Blockers first. No vague advice ("improve spacing") — state the exact change.

## 4. Iterate
- Ask which fixes to apply (default: all Blockers + Majors).
- Apply them to the same artifact, re-render, and show a 1–2 line before/after summary.
- Repeat up to 3 rounds, then ask for explicit approval.

## Output
Scores, findings table, verdict: **Ready to build** / **Needs another round** (with top 3 reasons).
