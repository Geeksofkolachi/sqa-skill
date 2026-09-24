---
name: test
description: Senior SQA testing for a web app. Modes: readonly (strict read-only audit), full (complete functional QA), security (security/API/performance), localization (i18n/l10n audit). Produces a Markdown report plus a CSV bug sheet. Use when the user asks to QA, test, audit, security-review, or localization-test an application.
version: 2.3.0
user-invocable: true
argument-hint: "readonly|full|security|localization [project name] [app url] [environment]"
---

# /test

Senior SQA Lead with 15+ years of experience. Pick the mode from the first argument (ask if absent):

| Mode | Follow | Report |
|---|---|---|
| `readonly` | `../test-readonly/references/prompt.md` | `<project-name>-SQA_ReadOnly_Test_Report.md` |
| `full` | `../test-full/references/prompt.md` | `<project-name>-SQA_Test_Report.md` |
| `security` | `../test-security/references/prompt.md` | `<project-name>-Security_API_Performance_Test_Report.md` |
| `localization` | `../test-localization/references/prompt.md` | `<project-name>-SQA_Localization_Test_Report.md` |

Read that mode's prompt file and follow it exactly.

Both deliverables are prefixed with the project name, slugified the same way: spaces → hyphens, filename-unsafe characters stripped. For project `Acme Customer Portal` in `full` mode that is `Acme-Customer-Portal-SQA_Test_Report.md` and `Acme-Customer-Portal-bugs.csv`. This naming overrides any fixed filename in the mode's `references/prompt.md`.

## Token budget

Testing is the expensive part, not the report. Keep it cheap:

1. **Read pages as text, not pictures.** Use the accessibility tree / page text (`read_page`, `get_page_text`) for every check about content, labels, state, structure, validation messages and console/network errors. A screenshot costs roughly 20x a text read of the same page.
2. **Screenshot only what is genuinely visual** — layout breakage, overflow, RTL mirroring, contrast, images — and only once per confirmed bug, as evidence. Never screenshot to "see where I am".
3. **Agree the scope before starting.** State the flow list and a page budget, get a yes, then test. Do not crawl the whole app by default.
4. **Write findings out as you go.** Append each validated bug to the CSV immediately; never hold the accumulated evidence in context to write at the end, and never re-read the report to add to it.
5. **Run long crawls in a subagent** (one per mode or per flow group) so the page dumps stay out of the main conversation and only the findings come back.
6. **One pass per check.** Do not re-verify a page you already read unless a fix is being retested.

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
