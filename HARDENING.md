<!-- markdownlint-disable -->

# Hardening Report: Apple-Actions--import-codesign-certs/v6.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Apple-Actions--import-codesign-certs/v6.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All four workflow files reference GitHub Actions using mutable version tags instead of immutable 40-character SHA commit hashes. This exposes the workflows to supply-chain attacks if the upstream action tags are moved or compromised. Failing references: `actions/checkout@v6.0.2`, `actions/setup-node@v6.2.0`, `actions/upload-artifact@v7.0.0`. Each should be pinned to a full SHA, e.g. `actions/checkout@<40-hex-sha> # v6.0.2`.

Locations:

- `.github/workflows/check-dist.yml:22`
- `.github/workflows/check-dist.yml:26`
- `.github/workflows/check-dist.yml:46`
- `.github/workflows/eslint.yml:18`
- `.github/workflows/eslint.yml:23`
- `.github/workflows/knip.yml:18`
- `.github/workflows/knip.yml:23`
- `.github/workflows/prettier.yml:18`
- `.github/workflows/prettier.yml:23`

### missing-permissions (severity: medium)

None of the four workflow files define a top-level `permissions:` block, and no job within any of them defines job-level permissions either. Without explicit permissions, workflows run with the default token permissions (which may be `write` for many scopes depending on repository settings), violating the principle of least privilege. Each workflow should declare `permissions: {}` or the minimal set of scopes required (e.g. `contents: read`).

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/eslint.yml:1`
- `.github/workflows/knip.yml:1`
- `.github/workflows/prettier.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all four workflow files (.github/workflows/check-dist.yml, eslint.yml, knip.yml, prettier.yml):
1. unpinned-uses: Pinned all three actions to full 40-character SHA hashes with version tag comments: actions/checkout@v6.0.2 → @de0fac2e4500dabe0009e67214ff5f5447ce83dd, actions/setup-node@v6.2.0 → @6044e13b5dc448c55e2357c09f80417699197238, actions/upload-artifact@v7.0.0 → @bbbca2ddaa5d8feaa63e36b76fdaad77386f024f.
2. missing-permissions: Added top-level `permissions: contents: read` block to all four workflow files, granting only the minimum scope needed to check out code.

