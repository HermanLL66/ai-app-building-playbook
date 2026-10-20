# Copyable prompts

These are reusable templates reconstructed for this guide, not the original conversation. Replace bracketed fields and provide the relevant files or reports. The same structure works with a coding assistant that can inspect your project and run its tools.

## 1. Turn an idea into a brief

```text
I want an app that helps [user] do [task] because [current problem].
Target platform: [platform and minimum version].
The first version needs [up to three essential behaviors].
Constraints: [privacy, dependencies, performance, distribution].

Turn this into a short project brief with observable acceptance criteria,
explicit non-goals, and the main technical uncertainty. Ask only questions
whose answers would materially change the implementation.
```

## 2. Inspect a project before changing it

```text
Read the project instructions, README, build scripts, and relevant source.
Explain how [user action] reaches [observable result]. Name the files on
that path, the external APIs involved, and existing checks. Identify the
smallest change needed for [requested behavior]. Then implement and verify
that change within the stated scope.
```

## 3. Validate a platform capability

```text
Before building the full interface, validate whether [platform] supports
[capability] under [constraints]. Consult official documentation for the
APIs and minimum versions. Implement a small experiment with a known input
and an observable output. State what would count as a failed experiment.
Report the actual result; do not substitute an API success code for proof
that the expected data or behavior was produced.
```

## 4. Build the smallest useful version

```text
Implement the first working slice from PROJECT_BRIEF.md: [specific slice].
Keep the dependency list small and follow existing project conventions.
Include the setup, main action, and cleanup path. Build it and run the
relevant checks. Finish with the changed files, exact commands and results,
and one short recipe I can use to try the behavior myself.
```

## 5. Fix a demonstrated failure

```text
Bug: [one sentence].
Expected: [observable result].
Actual: [observable result].
Reproduction: [numbered steps].
Environment: [OS, app version, relevant device or configuration].
Evidence: [relevant report or error].

Trace the behavior through the implementation. Reproduce the problem where
possible, identify the cause, make a focused fix, and repeat the same check.
Add a regression check when it would catch a meaningful future failure.
If the environment prevents verification, state exactly what is missing.
```

## 6. Check the result independently

```text
Review the claim that [feature] works. Identify which current checks use
mock data, which exercise the real integration, and which observe the
user-visible result. Where possible, use a known input and an independent
observation path. Check failure and cleanup behavior too. Report only the
conclusions supported by the evidence and list remaining manual checks.
```

For audio, compare against a native baseline captured by the same method. Missing callbacks are missing evidence, not proof of successful mute.

## 7. Keep a slow integration from freezing the app

```text
[Operation] may block indefinitely. Inspect its callers and resource
ownership. Keep user interaction responsive, provide cancellation and a
bounded deadline, and ensure repeated UI changes do not queue unbounded
work. Propose an appropriate isolation boundary for this project, implement
it, and verify timeout, cancellation, crash, cleanup, and latest-setting
behavior with controlled failure cases.
```

## 8. Refine the interface after the main path works

```text
The main behavior now works. Improve [specific screen or interaction] for
[audience]. Preserve the verified behavior. Use clear labels and explicit
loading, empty, success, and error states. Check keyboard access, readable
text at the minimum window size, and whether the status reflects the actual
backend state. Summarize how I can review the change.
```

## 9. Generalize a personal app for public use

```text
Audit this project for assumptions tied to one named app, account, device,
or personal workflow. Generalize only the assumptions that restrict the
intended audience. Preserve useful integration tests and label their scope.
Make public-facing text English, remove personal paths and local reports,
and document supported platforms and known limits. Rebuild and repeat the
checks affected by the change.
```

## 10. Package the project for sharing

```text
Prepare this project for publication at [explicit destination]. Include a
README, prerequisites, reproducible build and verification commands, known
limitations, and a description of the development method. Use an explicit
file allowlist. Inspect it for credentials, personal information, unrelated
files, generated output, and unsupported claims. Separate source-build
instructions from signed end-user distribution. Publish within the stated
authorization and verify the repository contents afterward.
```

## A filled-in example

```text
Build a native macOS utility that adjusts the local playback volume of
individual applications. Target macOS 14.4 or later. Use SwiftUI/AppKit for
the interface and investigate Core Audio process taps for the audio path.

Requirements:
- Discover audio-producing apps without preferring a named application.
- Provide independent 0–100% volume and mute, and remember settings.
- Leave the default output device and hardware master volume unchanged.
- Pause or quit to release the app's audio resources.
- Process audio locally without recording or uploading it.

First validate the platform capability using a quiet generated signal.
Then build a small end-to-end slice before adding UI polish. Keep audio
operations from blocking the UI. Verify DSP math, worker failures, real
audio behavior, and cleanup at distinct test levels. Clearly state which
third-party apps and devices still need manual testing.
```

For the official Codex guidance behind the behavior/context/constraints/verification structure, see [Prompting Codex](https://learn.chatgpt.com/docs/prompting#prompting-codex). The domain-specific prompts and engineering decisions above are this repository's examples.
