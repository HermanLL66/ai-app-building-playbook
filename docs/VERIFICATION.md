# Verification record by revision

## Experimental 0.2.0 source candidate

The current source changes the interface, gain range, ramp, limiter, buffering, and capture/render topology. Historical checks below apply only to their original revision. No 0.2.0 binary is published.

| Check | Status | Evidence boundary |
| --- | --- | --- |
| Native English SwiftUI preview, normal and minimum sizes, including failure state | Visually reviewed with labeled sample data | Layout/readability only; no live audio or third-party compatibility |
| English source build and fast checks | Passed locally | Compilation, strict ad hoc signature check, DSP/transport sanitizers, helper watchdog, input-metadata comparisons, and mock-worker lifecycle |
| Shared-backend local real-audio measurements | Passed in development bundle; English-bundle run pending | See the separate sanitized record below; digital paths only |
| Read-only input-state comparisons | Unchanged in development-bundle test; English-bundle run pending | Selected properties only; not far-end voice or conferencing AEC/AGC |
| Actual calls and remote listening | Not validated | Requires a real call and human acceptance |
| Bluetooth and broader OS/device coverage | Not validated | Requires configuration-specific sessions |

The [preview](assets/interface-preview.png) explicitly says **SAMPLE DATA**. The source's absence of microphone capture or gain-setting code does not replace a real-call check. Commands and their interpretation are in the [example README](../examples/volume-mixer/README.md#verification).

## Shared-backend development measurements

On October 20, 2026, the local development bundle 1.1.0 was checked on macOS 14.6.1, Apple Silicon, built-in stereo output at 48 kHz. It uses the revised backend shared by this source candidate. This is **not** a real-audio result for the English 0.2.0 bundle: its distinct identity requires its own permission and integration check. The [sanitized record](verification/revised-backend-local.json) omits local paths, process/object IDs, and device identifiers.

- A 20.24-second engine exercise passed two independent sources, more than four seconds of actual idle/resume, process restart and in-place replacement, independent mute, and cleanup.
- Default input and output identity, sample rate, and supported volume/mute controls remained unchanged; each observed output AUHAL had physical input I/O disabled. No microphone audio was recorded.
- Chrome independently captured replay matched its native baseline at 50%, 25%, 150%, 200%, and mute for a quiet signal. A further 60.89 seconds at 50% maintained the connection; disable restored the native baseline and acknowledged cleanup before the worker exited.
- FIFO underflow counters increased during this run. These checks establish digital gain and bounded connection behavior, not glitch-free playback or acoustic quality.
- Both English source trees built and passed strict ad hoc signature verification, DSP/transport sanitizer tests, blocked-helper and parent-exit checks, metadata comparison tests, and mock-worker protocol/lifecycle checks locally.

Far-end voice level, conferencing AEC/AGC, Bluetooth, and longer human listening sessions remain unvalidated. A helper-discovery startup race was found during repeat launch and the bounded startup allowance was increased from two to eight seconds before the successful sustained run. This failed experiment is retained in the interpretation rather than silently presented as an all-passing history.

## Historical publication verification

The generalized English reference example was checked locally on macOS 14.6.1, Apple Silicon (`arm64`), with Apple Swift 5.10.

| Check | Result | Scope |
| --- | --- | --- |
| `./test.sh` | Passed | DSP tests with AddressSanitizer and UndefinedBehaviorSanitizer; 10,000 callback cycles |
| `./build.sh` | Passed | Current-architecture compilation, packaging, ad hoc signing, strict bundle verification |
| `--verify-worker` and inspection of JSON `passed` | Passed | Seven mock-worker checks, including timeouts, cancellation, crashes, cleanup, and rapid updates |
| `zsh -n` on both scripts | Passed | Shell syntax |
| Local Markdown targets and code fences | Passed | Links to repository files exist; fences are balanced |
| Selected publication files | Reviewed | No authored Chinese text, app-specific messaging preset, personal filesystem paths, local reports, or build output |

The rapid-update check retained the final 37% setting after 1,001 slider changes and sent two scans in this run.

The first copy to the Desktop encountered Finder metadata that interfered with signature verification. The build script now makes up to three metadata-cleanup and verification attempts, and still fails if the copied bundle cannot be verified. The final local build passed.

## Hosted verification

The [first GitHub Actions run](https://github.com/HaomingLyu613/ai-app-building-playbook/actions/runs/39372163621) completed successfully for commit `a752470e6905458eee132db94c2b8c11ed6bc18e`. It ran the DSP sanitizer tests, built and verified the bundle, and checked the mock worker on the hosted macOS runner. This extends the local checks above; it does not exercise real audio.

## Remaining verification limits

- The optional real-audio, engine, independent-output, and Chrome integration diagnostics were not rerun for this new bundle identity.
- The earlier publication did not include a visual review; the current candidate has a separate sample-data preview review described above.
- Broad app, output-device, and OS-version compatibility has not been established.
- Developer ID signing, notarization, and end-user distribution are not configured.

The source includes the audio diagnostics and [instructions to run them](../examples/volume-mixer/README.md#verification). Local compilation and mock-worker results do not establish those unperformed checks.
