---
name: visual-match-verify
description: Verify an implemented UI matches its approved prototype/artifact - Playwright screenshots at desktop and mobile, side-by-side and pixel diff, state checks and a pass/fail report with concrete fixes. Use after implementing a design, or for "does it match the mockup?", "visual QA".
---

# Visual Match Verify

Goal: objective evidence that the build matches the approved design, plus exact fixes where it doesn't.

## 1. Setup
- Reference: approved artifact (render it) or exported PNGs.
- Build: start the project's dev server (`npm run dev` etc.); wait for ready.
- Same viewport, same data/content, same theme for both. Disable animations (`prefers-reduced-motion`, `* { transition: none !important }`).

## 2. Capture
Playwright script (Node or Python) that for each screen/state:
- Sets viewport 1440×900 and 390×844.
- Navigates, sets state (mock data, click to open, force error), waits for network idle + fonts.
- Saves `ref-<screen>-<vp>.png` and `impl-<screen>-<vp>.png`.

## 3. Compare
- Pixel diff with `pixelmatch` (or `PIL.ImageChops`); threshold 0.1, report % differing pixels and save `diff-*.png`.
- Build a side-by-side image (ref | impl | diff) for each pair.
- Look at the images yourself; pixel % alone misleads (fonts, antialiasing).

## 4. Check beyond pixels
- Layout: spacing/alignment off by >4px, wrapping, overflow, horizontal scroll on mobile.
- Tokens: computed styles of key elements vs. spec (color, font-size, radius).
- States: every designed state reachable and rendered.
- A11y quick pass: axe-core run, focus visible, tab order.

## 5. Report
Table: `Screen | Viewport | Diff % | Verdict (Match/Minor/Mismatch) | Issue | Fix (file:line)`.
Then the acceptance criteria as pass/fail. Offer to apply fixes and re-run.

## Rules
- Keep scripts and screenshots in a scratch folder, not the repo, unless asked.
- Never "fix" by changing the reference; flag intentional deviations instead.
