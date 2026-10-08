---
name: test-mobile
description: Senior SQA functional test of a native mobile app — Android (APK/AAB on a device or emulator) and iOS (simulator .app build, or an assisted manual pass for a TestFlight build). Covers install/launch, onboarding, auth, core flows, offline/network, permissions, background/resume, rotation, deep links, and crash/ANR log review. Produces <project>-SQA_Mobile_Test_Report.md and <project>-bugs.csv.
version: 2.5.0
user-invocable: true
argument-hint: "[project name] [android|ios|both] [apk/aab/.app path, or installed package/bundle id] [environment]"
---

# /test-mobile

Act as a Senior SQA Lead with 15+ years of mobile experience. Follow `references/prompt.md` in this folder exactly.

## How to run

`/test mobile …` and `/test-mobile …` are identical. Arguments are optional — with none, ask for them.

```text
/test mobile "Rahmah Connect" android com.rahmaconnect.app staging   # installed on emulator
/test mobile "Rahmah Connect" android ~/builds/app.apk staging       # from an APK or AAB
/test mobile "Rahmah Connect" ios com.gok.rahmaconnect staging       # installed on simulator
/test mobile "Rahmah Connect" ios ~/builds/App.app staging           # from a simulator build
/test mobile "Rahmah Connect" both staging                           # both platforms
```

If the third argument looks like a path, treat it as a build artifact; otherwise treat it as an installed package name or bundle id.

## Required inputs

Ask for anything missing before testing:

- Project name (prefixes both deliverables)
- Platform: `android`, `ios`, or `both`
- Either a build artifact **or** the id of an already-installed app — see below
- Environment (staging / QA / UAT / production backend)
- Authorized test credentials or roles
- Scope, priorities, excluded modules

## Already installed? Skip the install step

If the app is already on the emulator or simulator, no artifact is needed. Ask for the package name (Android) or bundle id (iOS) instead, or list what is installed and let the user pick:

```bash
adb shell pm list packages -3            # third-party packages
xcrun simctl listapps booted | grep CFBundleIdentifier
```

Then launch it directly — `adb shell monkey -p <package> -c android.intent.category.LAUNCHER 1`, or the iOS Simulator tool's `launch` with `bundle_id` and no `app_path`. Record the installed version so the report says what was tested:

```bash
adb shell dumpsys package <package> | grep versionName
```

Note in the report that an already-installed build was used, and that clean-install and upgrade paths were therefore **not** covered unless the user asks you to reinstall. Only run `pm clear` or uninstall after confirming with them — it wipes their logged-in state.

## What can actually be automated

| Artifact | Automatable | How |
|---|---|---|
| Android `.apk` | Yes | `adb install -r`, then drive with ARTEMIS (`mobile_run_task`) |
| Android `.aab` | Yes, after conversion | `bundletool build-apks --mode=universal` → `.apks` → extract the universal APK → install as above |
| iOS simulator `.app` | Yes | iOS Simulator tool: `launch`, then `inspect` / `tap` / `text` / `screenshot` |
| Already installed on an emulator/simulator | Yes | Launch by package/bundle id, no artifact needed |
| iOS `.ipa` from TestFlight | **No** | A TestFlight build is device-signed; it cannot be installed on a simulator, and no tool here drives a physical iPhone |

For TestFlight, ask the team for a simulator build of the same commit (`xcodebuild -sdk iphonesimulator`) and test that. If no simulator build exists, run the **assisted manual pass**: produce the step-by-step test script and the empty bug sheet, the human executes it on their device, and you turn their observations into the report. Say explicitly in the report which platform was automated and which was manual.

## Safety

Authorized build and test accounts only. Test data only — no destructive actions on real user data, no production payment flows outside the provider's test mode. Never expose credentials, tokens, or device identifiers in the report. If multiple devices are connected, ask which to use before launching anything.

## Deliverables

- `<project-name>-SQA_Mobile_Test_Report.md`
- `<project-name>-bugs.csv`

Slugify the project name: spaces → hyphens, filename-unsafe characters stripped. Use `templates/bug-sheet-template.csv` and preserve this exact header:

```csv
Title,Status,Priority,Type,Steps to Reproduce,Actual Result,Expected Result,URL,Labels,Story Points,Due Date,Estimated Hours,Assignee
```

The `URL` column holds the screen name or deep link instead of a web URL. One row per validated bug, `N/A` where unavailable. `Assignee` stays empty for the user to fill. If no bugs are found, still produce both files.
