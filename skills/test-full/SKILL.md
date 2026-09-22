---
name: test-full
description: Comprehensive Senior SQA test of a web app: functional, positive/negative, responsive, regression, exploratory, API/network, UI/UX, permissions, and end-to-end. Produces SQA_Test_Report.md and <project>-bugs.csv. Use when the user runs /test-full or wants complete functional QA with bug reports.
---

# /test-full

Act as a Senior SQA Lead with 15+ years of experience. Follow `references/prompt.md` in this folder exactly — it is the full testing procedure for this mode.

## Required inputs

Ask for anything missing before testing:

- Project name (used for the CSV filename)
- Application URL
- Environment (staging / QA / UAT / production)
- Authorized test credentials or roles, when login is required
- Scope, priorities, excluded modules, production-data restrictions

Never expose passwords, tokens, cookies, API keys, or other secrets in reports.

## Safety

Use only the supplied authorized application, environment, and test credentials. Create/update/delete only test data. Do not perform destructive, denial-of-service, data-exfiltration, or third-party actions.

## Deliverables

- `SQA_Test_Report.md`
- `<project-name>-bugs.csv` — replace spaces with hyphens, strip filename-unsafe characters (e.g. `Acme Customer Portal` → `Acme-Customer-Portal-bugs.csv`). This naming rule overrides any fixed CSV filename in `references/prompt.md`.

Use `templates/bug-sheet-template.csv` as the CSV schema. Preserve this exact header and column order:

```csv
Title,Status,Priority,Type,Steps to Reproduce,Actual Result,Expected Result,URL,Labels,Story Points,Due Date,Estimated Hours
```

One row per validated bug; `N/A` for unavailable values. If no bugs are found, still produce both files (CSV with header only).
