---
name: tool-schema-designer
description: Design LLM tool definitions Claude calls correctly - names, descriptions, strict JSON schemas, token-cheap outputs, actionable errors - and test them with real prompts. Use for "design tools for my agent", "Claude calls my tool wrong", "MCP tool descriptions".
---

# Tool Schema Designer

## 1. Inventory tasks, not endpoints
List user tasks; design one tool per task (e.g. `search_orders`), not a wrapper per API route. Target 3–15 tools.

## 2. Naming & description
- `verb_noun`, snake_case, namespaced if many (`crm_search_contacts`).
- Description (2–4 sentences): what it does, when to use / not use, key limits, example.

## 3. Schema
- Strict JSON Schema, `additionalProperties: false`, required fields minimal.
- Enums over free text; documented units/formats (ISO dates, cents); sensible defaults; pagination (`limit`, `cursor`).
- Each property has a description with an example.

## 4. Output
- Return only what the next step needs; human-readable IDs; truncate with "N more, use cursor".
- `response_format: concise|detailed` when outputs vary a lot.

## 5. Errors
Actionable: what failed + how to fix ("date must be YYYY-MM-DD; got 13/11/26"). No stack traces.

## 6. Test
10 realistic prompts → check tool choice, argument correctness, number of calls, tokens. Fix descriptions where Claude hesitates or picks the wrong tool. Re-run.

Deliver: tool definitions (JSON / SDK / MCP code), test prompts with results, change log.
