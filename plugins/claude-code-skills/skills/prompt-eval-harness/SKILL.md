---
name: prompt-eval-harness
description: Build an eval suite for a system prompt, skill or agent - test cases, code and LLM graders, multi-model runs, score report and regression checks. Use for "evaluate my prompt", "did this change make it worse?", "compare models".
---

# Prompt Eval Harness

## 1. Define success
Write 3–6 criteria (correctness, format, tone, safety, length, tool use). Each must be gradable.

## 2. Build the dataset (`evals/cases.jsonl`)
20–50 cases: typical (60%), edge (25%), adversarial/should-refuse (15%). Each: `{"id","input","expected"|"rubric","tags"}`. Use real user inputs when available.

## 3. Graders (cheapest first)
- Code: exact/regex/JSON schema/contains/length/tool-called.
- LLM-as-judge: rubric with 1–5 scale + one-line reason; use a strong model, temperature 0, judge blind to variant name.
- Human spot-check 10% to calibrate the judge.

## 4. Runner
Script (Python or TS) that runs each case × variant × model, with concurrency, retries, caching of outputs by hash, and records latency, tokens and cost. Results → `evals/results/<timestamp>.jsonl`.

## 5. Report
Table per variant/model: pass rate per criterion, mean score, cost, p50/p95 latency. List worst 5 failures with input/output. Diff vs. last baseline; flag regressions > 5 points.

## 6. Make it stick
- `npm run eval` / `make eval`; optional CI job on prompt changes.
- Add every production bug as a new case.

Deliver: dataset, graders, runner, first report and the recommended variant with reasons.
