# sqa-skill

Senior SQA testing as a Claude plugin. One command, three modes:

```text
/test readonly    strict read-only audit  → SQA_ReadOnly_Test_Report.md
/test full        complete functional QA  → SQA_Test_Report.md
/test security    security / API / perf   → Security_API_Performance_Test_Report.md
```

Every mode also writes `<project-name>-bugs.csv`.

## Install

```
/plugin marketplace add Geeksofkolachi/sqa-skill
/plugin install test
```

Or on claude.ai: **Plugins → Add → Add marketplace**, paste `Geeksofkolachi/sqa-skill`, then install the plugin.

## Use

```text
/test full
Project name: Acme Customer Portal
Application URL: https://staging.example.com
Environment: Staging
Credentials: [authorized test accounts]
Scope: full functional, responsive, API, permissions, UI/UX
```

## Layout

```
.claude-plugin/          plugin.json, marketplace.json
skills/test/             the /test command (routes by mode)
skills/test-readonly/    read-only prompt + CSV template
skills/test-full/        full-QA prompt + CSV template
skills/test-security/    security prompt + CSV template
```

Note: on claude.ai a plugin surfaces only the skill whose name matches the plugin name, which is why
`test` is the single entry point. In Claude Code the other three are also directly available as
`/test:test-full` and so on.
