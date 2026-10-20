# Project instructions for coding assistants

This is a template. Replace bracketed fields before using it in a new project.

## Orientation

Read PROJECT_BRIEF.md, README.md, and the relevant source before editing. Preserve the project's existing conventions. Scope each change to the requested behavior.

## Working method

- Investigate the most uncertain capability with a small reproducible experiment.
- Make changes that can be inspected and verified independently.
- Consult official documentation for unfamiliar or changing platform APIs.
- Keep slow or unreliable integrations from blocking user interaction.
- Make cancellation, resource cleanup, and error states explicit.
- Keep secrets and personal machine data out of source and reports.
- Keep real-time callbacks free of blocking work and allocations when applicable.

## Commands

- Build: [command]
- Fast checks: [command]
- Integration checks: [command and prerequisites]

## Verification and handoff

Run checks appropriate to the change. Distinguish mocks from real integrations. Inspect structured reports when exit status is not authoritative. Report commands, results, limitations, and any manual check still required. Do not describe unobserved behavior as tested.

## Publication

Publish only to an explicitly authorized destination and scope. Review the selected files and public claims first. Exclude private logs, local build output, credentials, and unrelated files.
