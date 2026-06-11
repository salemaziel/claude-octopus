---
name: provider-integration-and-hardening
description: Workflow command scaffold for provider-integration-and-hardening in claude-octopus.
allowed_tools: ["Bash", "Read", "Write", "Grep", "Glob"]
---

# /provider-integration-and-hardening

Use this workflow when working on **provider-integration-and-hardening** in `claude-octopus`.

## Goal

Adds a new model/provider (e.g., Cursor Agent) or hardens provider integration, including wiring, detection logic, and auth checks across multiple scripts and helpers.

## Common Files

- `scripts/lib/<provider>.sh`
- `scripts/lib/dispatch.sh`
- `scripts/lib/providers.sh`
- `scripts/lib/doctor.sh`
- `scripts/lib/model-resolver.sh`
- `scripts/lib/preflight.sh`

## Suggested Sequence

1. Understand the current state and failure mode before editing.
2. Make the smallest coherent change that satisfies the workflow goal.
3. Run the most relevant verification for touched files.
4. Summarize what changed and what still needs review.

## Typical Commit Signals

- Implement new provider module in scripts/lib/<provider>.sh
- Wire provider into scripts/lib/dispatch.sh, scripts/lib/providers.sh, scripts/lib/doctor.sh, scripts/lib/model-resolver.sh, scripts/lib/preflight.sh, scripts/lib/smoke.sh, scripts/lib/workflows.sh, and scripts/orchestrate.sh
- Update helpers (e.g., scripts/helpers/build-fleet.sh, scripts/helpers/check-providers.sh, scripts/helpers/octo-model-config.sh)
- Update scripts/install-deps.sh if needed
- Add/modify provider detection logic (e.g., auth file grep, binary checks)

## Notes

- Treat this as a scaffold, not a hard-coded script.
- Update the command if the workflow evolves materially.