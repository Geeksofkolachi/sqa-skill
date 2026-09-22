# sqa-skill

Senior SQA testing skills for Claude Code. One plugin, three commands:

| Command | What it does | Report |
|---|---|---|
| `/test-readonly` | Strict read-only audit: UI, data, navigation, responsive, console, GET APIs | `SQA_ReadOnly_Test_Report.md` |
| `/test-full` | Full functional, regression, exploratory, API, UI/UX, permissions testing | `SQA_Test_Report.md` |
| `/test-security` | Security, authz, API, business-logic, performance, 100K-user readiness | `Security_API_Performance_Test_Report.md` |

Every mode also writes `<project-name>-bugs.csv` using `templates/bug-sheet-template.csv`.

## Install

**Option A — as a plugin (from a git repo):**
```
/plugin marketplace add mtahir08/sqa-skills
/plugin install sqa-skill
```
Commands appear as `/sqa-skill:test-full` etc.

**Option B — as plain skills (from this zip):**
```bash
unzip sqa-skill.zip && cp -R sqa-skill/skills/* ~/.claude/skills/
```
Commands appear as `/test-full` etc. in every project.

## Use

```text
/test-full
Project name: Acme Customer Portal
Application URL: https://staging.example.com
Environment: Staging
Credentials: [authorized test accounts]
Scope: full functional, responsive, API, permissions, UI/UX
```

## Layout

```
sqa-skill/
├── .claude-plugin/plugin.json
├── README.md
└── skills/
    ├── test-readonly/  SKILL.md, references/prompt.md, templates/bug-sheet-template.csv
    ├── test-full/      SKILL.md, references/prompt.md, templates/bug-sheet-template.csv
    └── test-security/  SKILL.md, references/prompt.md, templates/bug-sheet-template.csv
```
