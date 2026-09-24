---
name: test
description: Senior SQA testing for a web app. Modes: readonly (strict read-only audit), full (complete functional QA), security (security/API/performance). Produces a Markdown report plus a CSV bug sheet. Use when the user asks to QA, test, audit, or security-review an application.
version: 2.1.0
user-invocable: true
argument-hint: "readonly|full|security|localization [project name] [app url] [environment]"
---

# /test

Senior SQA Lead with 15+ years of experience. Pick the mode from the first argument (ask if absent):

| Mode | Follow | Report |
|---|---|---|
| `readonly` | `../test-readonly/references/prompt.md` | `SQA_ReadOnly_Test_Report.md` |
| `full` | `../test-full/references/prompt.md` | `SQA_Test_Report.md` |
| `security` | `../test-security/references/prompt.md` | `Security_API_Performance_Test_Report.md` |
| `localization` | `../test-localization/references/prompt.md` | `SQA_Localization_Test_Report.md` |

Read that mode's prompt file and follow it exactly.

## Required inputs

Ask for anything missing: project name (names the CSV), application URL, environment, authorized credentials or roles, scope and exclusions. Never expose secrets in reports.

## Safety

- `readonly`: login is the only permitted write; after that no POST/PUT/PATCH/DELETE or any state-changing action.
- `full`: authorized app and test credentials only; test data only; nothing destructive.
- `security`: authorized app, APIs, accounts and domains only; no destructive testing, DoS, exfiltration, malware, or third-party attacks.
- `localization`: authorized app and test accounts only; negative cases are in scope, destructive actions on real data are not.

## Deliverables

The mode's Markdown report plus `<project-name>-bugs.csv` (spaces → hyphens, unsafe characters stripped), using `../test-readonly/templates/bug-sheet-template.csv` and preserving this header exactly:

```csv
Title,Status,Priority,Type,Steps to Reproduce,Actual Result,Expected Result,URL,Labels,Story Points,Due Date,Estimated Hours
```

One row per validated bug, `N/A` where unavailable. If no bugs are found, still produce both files.
