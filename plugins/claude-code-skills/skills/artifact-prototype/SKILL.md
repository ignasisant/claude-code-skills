---
name: artifact-prototype
description: Prototype a UI idea as a Claude artifact (Design/canvas or HTML), review and iterate it with the user, then implement the approved design in their project. Use for "prototype this", "mock it up then build it", "design first, then code".
---

# Artifact Prototype → Implement

Flow: Context → Brief → Prototype artifact → Review loop → Handoff → Implement → Verify.
Each phase ends with a gate. Never write project code before the design is approved.

## 0. Project context (before designing)
If a repo is available, scan it so the prototype is buildable:
- Stack and UI framework (React, Vue, Svelte…), styling (Tailwind, CSS modules, CSS vars).
- Design tokens: colors, fonts, spacing, radius (tailwind.config, theme files, `:root` vars).
- Reusable components (Button, Input, Card, Layout…).
Summarize in ≤6 lines. No repo → "greenfield", pick sensible defaults.

## 1. Idea brief
Turn the idea into a short brief (ask only about real gaps):
- Goal and target user
- Key screens/flows (max 3 for v1)
- Required content/data, constraints (mobile-first, a11y, brand)
- 3–5 acceptance criteria (pass/fail) — reused in step 6
Gate: user confirms the brief.

## 2. Create the prototype artifact
- Preferred: a Claude Design/canvas artifact. Fallback: a single self-contained HTML artifact/file.
- Use the project's tokens/components when found.
- 2 variants if direction is unclear; 1 if the brief is precise.
- Realistic content (no lorem ipsum). Cover empty, loading, error and filled states.
- Desktop and mobile frames.

## 3. Review loop
- Self-critique first: brief, acceptance criteria, hierarchy, consistency, WCAG AA contrast and target sizes (see `artifact-design-review`).
- Collect user feedback (chat or artifact comments). Update the same artifact so link and history stay.
- Max ~3 rounds; summarize each round in 1–2 lines.
Gate: user explicitly approves one version/variant.

## 4. Handoff spec
Before coding, write a compact spec (see `prototype-handoff-spec`):
- Component tree → mapping to existing components (reuse > new)
- Tokens, breakpoints, states, interactions
- Files to create/modify

## 5. Implement in the project
- Follow repo conventions (lint, naming, folder structure). Small, reviewable diffs.
- Tokens over hardcoded values. Semantic, accessible markup (labels, focus, roles).
- Add/adjust tests if the project has them.
- Don't commit/push unless asked; branch first if on the default branch.

## 6. Verify
- Run lint, tests and build.
- Run the dev server, screenshot desktop + mobile, compare with the approved artifact (see `visual-match-verify`).
- Check each acceptance criterion → pass/fail list.
- Report: files changed, deviations from design and why, next steps. Keep it short.

## Rules
- The artifact is the visual reference; the project's conventions win on code.
- One artifact per prototype; iterate in place.
- If the user means the Canva app (not a canvas), use a Canva connector for step 2 if available.
