# Verification record

## Change and environment

- Behavior under review: [value]
- Revision: [commit or local change]
- Date: [YYYY-MM-DD]
- OS, CPU, toolchain, device, integration version: [relevant details]

## Evidence

| Check and exact command | Inputs | Observed result | Conclusion supported |
| --- | --- | --- | --- |
| [build] | [source revision] | [result] | [narrow conclusion] |
| [unit check] | [fixture] | [result] | [narrow conclusion] |
| [integration check] | [real or mock] | [result] | [narrow conclusion] |
| [manual scenario] | [steps] | [result] | [narrow conclusion] |

## Recovery

- Cancel: [observation]
- Quit or cleanup: [observation]
- Missing permission: [observation]
- Unresponsive integration: [observation]
- Existing user settings preserved: [observation]

## Limits and next checks

[What was not exercised? What evidence would resolve the remaining uncertainty?]

Before sharing this record, remove personal paths, identifiers, credentials, and unrelated application details. Preserve relevant failures instead of presenting them as passes.
