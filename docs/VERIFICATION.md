# Publication verification

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

## Checks not performed for this generalized edition

- The optional real-audio, engine, independent-output, and Chrome integration diagnostics were not rerun for this new bundle identity.
- The English interface was not visually reviewed in this publication pass.
- Broad app, output-device, and OS-version compatibility has not been established.
- The GitHub Actions workflow is provided; a hosted CI run is not part of the local results above.
- Developer ID signing, notarization, and end-user distribution are not configured.

The source includes the audio diagnostics and [instructions to run them](../examples/volume-mixer/README.md#verification). Local compilation and mock-worker results do not establish those unperformed checks.
