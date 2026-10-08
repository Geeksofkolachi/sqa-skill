# sqa-skill

Senior SQA testing as a Claude plugin. One command, five modes — web and native mobile.

| Command | What it does | Report |
|---|---|---|
| `/test readonly` | Strict read-only audit, no writes after login | `<project>-SQA_ReadOnly_Test_Report.md` |
| `/test full` | Complete functional QA of a web app | `<project>-SQA_Test_Report.md` |
| `/test security` | Security, API and performance review | `<project>-Security_API_Performance_Test_Report.md` |
| `/test localization` | i18n / l10n audit, guest and authenticated | `<project>-SQA_Localization_Test_Report.md` |
| `/test mobile` | Native Android and iOS app testing | `<project>-SQA_Mobile_Test_Report.md` |

Every mode also writes `<project>-bugs.csv`. Filenames are always prefixed with the project name.

## Install

```
/plugin marketplace add Geeksofkolachi/sqa-skill
/plugin install test
```

On claude.ai: **Plugins → Add → Add marketplace**, paste `Geeksofkolachi/sqa-skill`, then install **test**.

Restart Claude Code after installing or updating — skills are loaded at startup.

## How to run

Every mode works two ways. They are identical; pick whichever you prefer:

```text
/test <mode> [arguments]      the router
/test-<mode> [arguments]      the mode directly
```

You can also type the command with no arguments and answer the questions it asks.

### Web modes

```text
/test full "Acme Portal" https://staging.example.com staging
/test readonly "Acme Portal" https://staging.example.com production
/test security "Acme Portal" https://staging.example.com staging
/test localization "Acme Portal" https://staging.example.com staging
```

Equivalent direct forms: `/test-full`, `/test-readonly`, `/test-security`, `/test-localization`.

It will ask for anything missing — test credentials, scope, exclusions.

### Mobile mode

```text
/test mobile "<project>" <android|ios|both> <artifact path or app id> <environment>
```

`/test-mobile` is the identical direct form.

**Android — app already installed on a device or emulator** (pass the package name):

```text
/test mobile "Rahmah Connect" android com.rahmaconnect.app staging
```

**Android — from a build artifact** (`.apk`, or `.aab` which it converts with bundletool):

```text
/test mobile "Rahmah Connect" android ~/builds/rahma-staging.apk staging
```

**iOS — app already installed on a booted simulator** (pass the bundle id):

```text
/test mobile "Rahmah Connect" ios com.geeksofkolachi.rahmaconnect staging
```

**iOS — from a simulator build**:

```text
/test mobile "Rahmah Connect" ios ~/builds/RahmaConnect.app staging
```

**Both platforms in one run**:

```text
/test mobile "Rahmah Connect" both staging
```

Don't know the id? Find it yourself, or just let the skill list them for you:

```bash
adb devices -l && adb shell pm list packages -3
```

```bash
xcrun simctl listapps booted | grep CFBundleIdentifier
```

### What mobile mode can and cannot automate

| Artifact | Automated | How |
|---|---|---|
| Already installed on an emulator / simulator | Yes | Launched by package name or bundle id — no artifact needed |
| Android `.apk` | Yes | `adb install -r`, then driven by ARTEMIS |
| Android `.aab` | Yes | Converted with `bundletool --mode=universal`, then installed |
| iOS simulator `.app` | Yes | iOS Simulator tool — launch, inspect, tap, screenshot |
| **iOS `.ipa` from TestFlight** | **No** | Device-signed: it cannot be installed on a simulator, and no tool here drives a physical iPhone |

For TestFlight, ask for a simulator build of the same commit (`xcodebuild -sdk iphonesimulator`). If there isn't one, run `/test mobile` anyway and say it's TestFlight — it switches to an **assisted manual pass**: it writes the step-by-step test script and the empty bug sheet, you run it on your phone, and it turns your observations into the report. The report states which platform was automated and which was manual.

### Safety notes for mobile

- If more than one device or emulator is connected, it asks which serial to use rather than picking one.
- It will not `pm clear` or uninstall an already-installed build without asking — that wipes your logged-in state.
- Testing an already-installed build means clean-install and upgrade paths are not covered; the report says so.

## Keeping it up to date

```
/plugin marketplace update sqa-skills
/plugin update test
```

Then restart. On claude.ai, **Sync automatically** picks up new versions on its own.

## Layout

```
.claude-plugin/            plugin.json, marketplace.json
skills/test/               the /test router
skills/test-readonly/      prompt + CSV template
skills/test-full/          prompt + CSV template
skills/test-security/      prompt + CSV template
skills/test-localization/  prompt + CSV template
skills/test-mobile/        prompt + CSV template
```

On claude.ai a plugin surfaces only the skill whose name matches the plugin name, which is why `test` is
the single entry point there. In Claude Code every mode is also directly available as `/test:test-mobile`
and so on.
