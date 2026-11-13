---
name: artifact-dashboard-from-data
description: Turn a CSV, spreadsheet, SQL query or API into a shareable Claude artifact dashboard - KPIs, charts, filters, light/dark, mobile - with data refresh where supported. Use for "make a dashboard from this data", "KPI page", "share these metrics".
---

# Artifact Dashboard from Data

## 1. Understand the data
Load and profile: columns, types, ranges, nulls, row count, date granularity. Clean (types, dupes, outliers flagged, not silently dropped). Note sources.

## 2. Decide what matters
Ask/infer audience and 3–5 questions the dashboard answers. Pick KPIs (with period-over-period delta) and one chart per question:
trend → line · compare categories → sorted bar · part of whole (≤5) → stacked bar · distribution → histogram · relation → scatter.

## 3. Build
- Prefer the platform's Dashboard artifact type if available; else a single self-contained HTML page.
- Layout: KPI row → main trend → breakdowns → detail table (sortable, searchable).
- Filters: date range + 1–2 key dimensions; all charts react.
- Color tokens for light/dark, accessible contrast, no rainbow palettes; numbers formatted with units.
- Mobile: single column, no horizontal scroll.

## 4. Data freshness
Static snapshot (state the "as of" date) or live: connect via available data connector/API capability, with loading and error states. Never embed secrets in the page.

## 5. Verify
Recompute 3 KPIs independently; check totals match source; screenshot desktop + mobile.

Deliver the artifact link, data notes (cleaning, assumptions) and how to refresh.
