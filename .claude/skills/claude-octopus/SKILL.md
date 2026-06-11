```markdown
# claude-octopus Development Patterns

> Auto-generated skill from repository analysis

## Overview

This skill teaches the core development patterns, workflows, and conventions used in the `claude-octopus` repository. The project is written in TypeScript and focuses on extensible provider integration, robust release management, cross-platform testing, and resilient scripting. You'll learn how to contribute new providers, manage releases, harden test suites, improve error handling in hooks, and keep documentation in sync with implementation. The skill also covers the repository’s coding conventions and provides practical code examples for each workflow.

---

## Coding Conventions

### File Naming

- **Kebab-case** is used for all file names.
  - Example:  
    ```
    scripts/lib/model-resolver.sh
    tests/unit/test-cursor-provider.sh
    ```

### Import Style

- **Relative imports** are preferred.
  - Example (TypeScript):
    ```typescript
    import { resolveModel } from './model-resolver';
    ```

### Export Style

- **Named exports** are used throughout.
  - Example (TypeScript):
    ```typescript
    export function resolveModel() { ... }
    ```

### Commit Messages

- **Prefixes:** `fix:`, `test:`, `chore:`, `release:`, `docs:`
- **Average length:** ~62 characters
- **Examples:**
  - `fix: correct provider detection logic for Cursor`
  - `release: v1.2.3`
  - `docs: update usage section in README`

---

## Workflows

### Provider Integration and Hardening

**Trigger:** When adding a new LLM provider or updating provider detection/auth logic  
**Command:** `/add-provider`

1. Implement the new provider module in `scripts/lib/<provider>.sh`.
2. Wire the provider into:
    - `scripts/lib/dispatch.sh`
    - `scripts/lib/providers.sh`
    - `scripts/lib/doctor.sh`
    - `scripts/lib/model-resolver.sh`
    - `scripts/lib/preflight.sh`
    - `scripts/lib/smoke.sh`
    - `scripts/lib/workflows.sh`
    - `scripts/orchestrate.sh`
3. Update helpers as needed:
    - `scripts/helpers/build-fleet.sh`
    - `scripts/helpers/check-providers.sh`
    - `scripts/helpers/octo-model-config.sh`
4. Update `scripts/install-deps.sh` if new dependencies are required.
5. Add or modify provider detection logic (e.g., auth file grep, binary checks).
6. Add or update tests in `tests/unit/test-<provider>-provider.sh`.
7. Address review feedback by tightening detection logic or fixing edge cases.

**Example:**  
```bash
# scripts/lib/cursor.sh
function cursor_auth_check() {
  grep -q 'CURSOR_API_KEY' ~/.cursor_auth || return 1
}
export -f cursor_auth_check
```

---

### Release Version Bundle

**Trigger:** When cutting a new release after a batch of changes  
**Command:** `/release`

1. Update `CHANGELOG.md` with release notes.
2. Update `README.md` for version or install instructions.
3. Update the version in `package.json`.
4. Update `.claude-plugin/marketplace.json` and `.claude-plugin/plugin.json`.
5. Commit with a message like `release: vX.Y.Z` or `chore: release vX.Y.Z`.
6. Merge the release branch or PR into `main`.

**Example:**  
```json
// package.json
{
  "version": "1.2.3"
}
```

---

### Test Suite Migration or Hardening

**Trigger:** When upgrading test infrastructure or fixing test flakiness  
**Command:** `/migrate-tests`

1. Bulk edit test files to source new helpers or frameworks (e.g., `test-framework.sh`).
2. Replace assertion helpers or pipelines for compatibility (e.g., use herestrings instead of `echo | grep`).
3. Fix or update test logic for new code paths or platform quirks (e.g., BSD grep, SIGPIPE).
4. Add new unit or regression tests for recent bugs.
5. Update test framework or helper scripts as needed.

**Example:**  
```bash
# Before
echo "$output" | grep "expected result"

# After (for macOS compatibility)
grep "expected result" <<< "$output"
```

---

### Hook Hardening and Error Trapping

**Trigger:** When making plugin hooks robust against silent errors  
**Command:** `/harden-hooks`

1. Add or update EXIT trap logic to each hook script (`hooks/*.sh`).
2. Guard pipelines (e.g., `grep`, `ls`) with `|| true` to prevent non-zero exits under `set -e/pipefail`.
3. Update hook logic for compatibility with different bash versions (e.g., macOS bash 3.2).
4. Add or update regression/unit tests to enforce error-trap presence and correct behavior.
5. Fix any discovered silent-fail paths and re-run tests.

**Example:**  
```bash
trap 'echo "Error on line $LINENO"; exit 1' ERR EXIT

ls somefile.txt || true
```

---

### Command or Skill Docs and Logic Sync

**Trigger:** When fixing or clarifying command/skill documentation or ensuring implementation matches docs  
**Command:** `/sync-command-docs`

1. Edit `.claude/commands/*.md` and/or `commands/octo-*.md` to update banners, usage, or logic.
2. Edit corresponding implementation scripts or logic to match documentation (e.g., `orchestrate.sh`, direct dispatch fixes).
3. Update or add related tests (unit or smoke) to enforce new doc/logic contract.
4. Address review feedback for clarity or correctness.
5. Merge changes after verification.

**Example:**  
```markdown
# .claude/commands/develop.md

> Usage: octo develop [options]

Runs the development workflow for the current model.
```
```bash
# scripts/orchestrate.sh
if [[ $1 == "develop" ]]; then
  # Implementation matches documented usage
fi
```

---

## Testing Patterns

- **Test files:** Use the pattern `*.test.ts` for TypeScript tests and `test-*.sh` for shell-based tests.
- **Framework:** Not explicitly specified; custom or shell-based test runners are likely.
- **Test locations:**  
  - Unit tests: `tests/unit/test-*.sh`
  - Integration tests: `tests/integration/*.sh`
  - Smoke tests: `tests/smoke/*.sh`
- **Assertion style:** Use of shell assertions and grepping output; herestrings for cross-platform compatibility.
- **Adding new tests:** Place new provider tests in `tests/unit/test-<provider>-provider.sh`.

**Example:**  
```bash
# tests/unit/test-cursor-provider.sh
output=$(./scripts/lib/cursor.sh --check)
grep "OK" <<< "$output"
```

---

## Commands

| Command           | Purpose                                                        |
|-------------------|----------------------------------------------------------------|
| /add-provider     | Add or update a provider integration and detection logic       |
| /release          | Bundle changes and cut a new release                          |
| /migrate-tests    | Migrate or harden the test suite for compatibility and coverage|
| /harden-hooks     | Add error trapping and diagnostics to all event hooks          |
| /sync-command-docs| Sync command/skill documentation with implementation           |
```
