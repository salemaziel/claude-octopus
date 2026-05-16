---
command: octo
description: "[Legacy] Redirects to /octo:auto — the smart router"
version: 3.0.0
category: workflow
tags: [router, legacy, redirect]
created: 2025-02-03
updated: 2026-03-18
---

# /octo:octo → /octo:auto (Legacy Redirect)

This command has been renamed to `/octo:auto`. Invoking `/octo:octo` still works for backward compatibility.

## EXECUTION CONTRACT (Mandatory)

When the user invokes `/octo:octo <query>`, you MUST:

1. Inform the user: "Note: `/octo:octo` has been renamed to `/octo:auto`. Routing your request now."
2. Immediately invoke: `Skill(skill: "octo:auto", args: "<full user query>")`
3. **Fallback** — if the `octo:auto` skill is unavailable or the invocation fails: tell the user the smart router could not be reached, then handle the query directly (do not silently drop it). Suggest the user run `/octo:setup` if `octo:auto` appears to be missing.

Do NOT duplicate the routing logic here — delegate entirely to `/octo:auto`. The fallback is a graceful-degradation safety net only, not a second routing implementation.
