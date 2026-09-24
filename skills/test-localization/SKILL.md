---
name: test-localization
description: Senior SQA localization/i18n audit of a web app as both guest and authenticated user — missing or hardcoded strings, mixed languages, date/number/currency formatting, RTL, text overflow, locale persistence, and guest vs authenticated differences. Produces SQA_Localization_Test_Report.md and <project>-bugs.csv.
version: 2.2.0
user-invocable: true
argument-hint: "[project name] [app url] [locales]"
---

# /test-localization

Act as a Senior SQA Engineer and Localization Testing Specialist. Follow `references/prompt.md` in this folder exactly.

## Required inputs

Ask for anything missing: project name (names the CSV), application URL, environment, supported locales, authorized test credentials for the authenticated pass, scope and exclusions. Never expose credentials in reports.

## Safety

Use only the supplied authorized application and test accounts. Negative cases (empty input, invalid files, failed requests) are in scope; destructive actions on real data are not.

## Deliverables

- `<project-name>-SQA_Localization_Test_Report.md` — coverage, gaps, and the localization summary from section 17 of the prompt. This filename overrides the `SQA_ReadOnly_*` names in `references/prompt.md`.
- `<project-name>-bugs.csv` — spaces → hyphens, unsafe characters stripped.

Use `templates/bug-sheet-template.csv` and preserve this header exactly:

```csv
Title,Status,Priority,Type,Steps to Reproduce,Actual Result,Expected Result,URL,Labels,Story Points,Due Date,Estimated Hours
```

One row per validated bug, titled `[Localization] ...`, `N/A` where unavailable. State coverage gaps explicitly; never claim 100% coverage for anything not verified.
