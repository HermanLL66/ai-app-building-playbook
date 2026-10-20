# A reusable workflow for building apps with AI

This guide is for people who can describe a problem but may not know how to structure a software project. An AI coding assistant can help investigate, implement, test, and document the solution. You still define the intended behavior and decide whether the result meets your needs.

The examples below come from a macOS volume utility. Replace the audio details with your own domain.

## 1. Describe a problem, then make success observable

Begin with a real situation: several apps are playing sound, and you want independent control. Convert that situation into a small product brief.

| Part | Example |
| --- | --- |
| User | A Mac user listening to multiple apps |
| Problem | Changing system volume affects everything |
| Essential action | Adjust one app between 0% and 100% |
| Must preserve | Other apps, default output device, and hardware volume |
| Recovery | Pause or quit to restore the original playback path |
| Out of scope | Per-browser-tab control, microphone gain, cloud accounts |

Use [PROJECT_BRIEF.md](../templates/PROJECT_BRIEF.md) to write your own. Keep the first version small enough that you can personally try its main behavior.

## 2. Investigate the hardest capability before polishing the interface

Ask the assistant to inspect the available platform APIs, minimum OS version, permissions, and build tools. For unfamiliar or changing APIs, ask it to consult official documentation and identify the symbols it intends to use.

For this example, the uncertain part is obtaining and replaying individual application audio through Core Audio taps. A prototype that reads an audio process and observes a known test signal is more informative than an elaborate settings screen.

Define an exit condition for the experiment. If the platform returns zeros instead of the expected signal, report that result and investigate permission or routing. Do not treat a successful API return code as proof that useful data arrived.

## 3. Choose boundaries around failure

Ask for a short architecture explanation with the responsibilities of each component. Keep it tied to failure modes:

- The UI owns user interaction and saved preferences.
- A worker owns operating-system audio operations, which may stop responding.
- A small C processor owns real-time sample processing and atomic gain values.
- Verification tools generate known signals and check the resulting behavior.

This division is specific to an audio utility. A file organizer might instead separate preview, filesystem changes, and undo records. A web app might separate its UI, API, and persistence layer.

The reusable question is: **If one operation fails or hangs, what should remain usable?**

## 4. Build one complete slice

A complete first slice crosses the important boundaries. In this app, that means discovering one playing application, applying one gain value, and releasing the channel afterward.

Finish that path before adding search, presets, icons, or visual refinement. Ask the assistant to name the changed files, run the build, and explain how you can reproduce the behavior. Save a working checkpoint in version control.

Then add the next capability, such as independent mute or persistence. Keep each change narrow enough that a regression has a likely cause.

## 5. Turn observations into useful follow-up prompts

“It does not work” is a starting point. A reproduction makes it actionable:

```text
Expected: lowering App A leaves App B unchanged.
Actual: the slider changes, but App A sounds the same.
Steps: start the supplied tone, enable control, set A to 25%.
Environment: [OS version, CPU, output device, app version].
Evidence: [a small relevant report or error message].

Trace the setting from UI state through the worker to the audio callback.
Identify where the expected behavior stops. Fix that cause and re-run the
same reproduction. Do not count a changed label as a successful audio test.
```

Prefer relevant evidence over entire logs. Redact usernames, local paths, device identifiers, credentials, and unrelated application information before sharing publicly.

## 6. Check at several levels

| Check | What it can establish | What it cannot establish |
| --- | --- | --- |
| Compile and package | Sources compile and the bundle passes local signature verification | Real audio behavior |
| DSP unit tests | Gain math, mute, smoothing, buffer handling | OS permissions or app discovery |
| Mock worker checks | Protocol, timeout, cancellation, crash handling, update coalescing | Actual Core Audio routing |
| Generated audio checks | Controlled behavior through real audio paths on that machine | Every third-party app or device |
| Third-party app check | Behavior for the tested app and configuration | Universal compatibility |
| Personal listening check | Whether the experience meets the user's practical needs | A precise gain measurement |

The [reference app](../examples/volume-mixer/README.md) includes these automated checks where practical, with manual instructions for checks that need an audio session or OS permission.

Inspect structured reports. Some diagnostic modes save `passed: false` while the enclosing app exits normally; exit status alone is insufficient.

## 7. Verify recovery as well as success

Small utilities often interact with resources the user already depends on. Check what happens when:

- Permission is unavailable or denied.
- The controlled process exits, restarts, or goes idle.
- The output device changes.
- An operation never returns.
- The user cancels setup, pauses control, or quits.
- The user changes a setting rapidly.

The expected outcome should be concrete: control stops, the interface remains responsive, original playback can continue, and the app explains what needs attention.

Do not automatically restart system services or modify unrelated settings as a routine recovery step. First collect evidence and bound the app's own work.

## 8. Generalize product assumptions deliberately

An app may begin with a very specific use case. Before sharing it, distinguish the underlying capability from personal defaults.

For the public Volume Mixer example, that meant removing a messaging-app identity check, preferred ordering, and a preset tied to two named apps. Discovery now follows audio processes, and the user chooses the settings.

Keep explicit integration tests when they provide useful evidence. A Chrome fixture can validate one browser without making the product exclusive to Chrome. State that boundary in the documentation.

## 9. Make publication reproducible

Include the following in the repository:

- A concise explanation of the problem and the development method.
- Source code, required tools, and working build commands.
- Copyable prompts with inputs and success conditions.
- Verification commands, evidence boundaries, and known limitations.
- A license and a clear description of any signing or distribution limits.

Publish selected source files and documentation. Keep local build output, personal reports, private conversations, and machine-specific configuration out of the repository.

Use [VERIFICATION.md](../templates/VERIFICATION.md) to record what was actually checked. If a claim has no supporting observation, narrow the claim or perform the missing check.

## A compact working rhythm

Write the target behavior. Ask for a small implementation. Run it. Inspect the result. Give one precise correction. Preserve the working state. Repeat until the original acceptance criteria are satisfied.

The value of the method comes from that feedback loop. The prompt library helps express each step; it does not replace the need to inspect the app.
