---
name: test-security
description: Senior security, API, authorization, business-logic, performance, scalability, and 100K-user-readiness review of a web app. Produces Security_API_Performance_Test_Report.md and <project>-bugs.csv. Use when the user runs /test-security or asks for a security / pentest-style / performance review.
version: 2.3.0
user-invocable: true
argument-hint: "[project name] [app url] [environment]"
---

# /test-security

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

Test only the supplied authorized application, APIs, accounts, and domains. No destructive testing, denial-of-service, uncontrolled traffic, data exfiltration, malware, persistence, backdoors, or attacks on unrelated third parties. Where a check could be disruptive, simulate or safely validate instead.

## Deliverables

- `<project-name>-Security_API_Performance_Test_Report.md`
- `<project-name>-bugs.csv` — replace spaces with hyphens, strip filename-unsafe characters (e.g. `Acme Customer Portal` → `Acme-Customer-Portal-bugs.csv`). This naming rule overrides any fixed CSV filename in `references/prompt.md`.

Use `templates/bug-sheet-template.csv` as the CSV schema. Preserve this exact header and column order:

```csv
Title,Status,Priority,Type,Steps to Reproduce,Actual Result,Expected Result,URL,Labels,Story Points,Due Date,Estimated Hours
```

One row per validated bug; `N/A` for unavailable values. If no bugs are found, still produce both files (CSV with header only).
