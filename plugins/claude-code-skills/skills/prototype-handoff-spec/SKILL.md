---
name: prototype-handoff-spec
description: Convert an approved prototype or Claude artifact into an implementation spec mapped to the user's real codebase - component tree, existing components to reuse, tokens, states, breakpoints and file plan. Use before coding a design, or for "handoff", "spec this design", "how do we build this?".
---

# Prototype → Handoff Spec

Goal: a spec a developer (or agent) can implement without guessing, grounded in the actual repo.

## 1. Read the codebase first
- Framework, router, styling system, state management, test setup.
- Inventory UI components: `grep`/glob for components dir; note name, props, variants.
- Extract tokens (tailwind config, theme file, CSS vars). Note gaps vs. the design.

## 2. Decompose the design
- List screens → sections → components (tree, max 3 levels).
- For each component: **Reuse** (existing, with props) / **Extend** (new variant) / **New** (justify).
- Map every color, font size, spacing, radius to a token. Flag values with no token → propose token or nearest match.

## 3. Behavior
- States per component: default, hover, focus, active, disabled, loading, empty, error.
- Interactions and transitions (trigger → result, duration).
- Data: props/API shape, loading and error handling, validation rules.
- Breakpoints: what changes at each (layout, hidden/shown, stacking).
- Accessibility: roles, labels, keyboard order, focus management, announcements.

## 4. File plan
Table: `File | Action (create/modify) | Purpose`. Keep order of implementation (shared → leaf → page).

## 5. Acceptance checklist
Carry over the brief's acceptance criteria + add: tokens only, all states present, keyboard usable, mobile no h-scroll, tests pass.

## Output format
1. Summary (3 lines) 2. Component tree with Reuse/Extend/New 3. Token map + gaps 4. States & interactions 5. File plan 6. Acceptance checklist.
Keep it compact; link to the artifact instead of re-describing visuals.
