<!-- markdownlint-disable -->

# Hardening Report: Apple-Actions--import-codesign-certs/v5.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Apple-Actions--import-codesign-certs/v5.0.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All four workflow files reference GitHub Actions using mutable version tags instead of immutable 40-character commit SHAs. This exposes the workflows to supply-chain attacks if the referenced action tags are moved or compromised. Affected refs: actions/checkout@v4.2.2, actions/setup-node@v4.4.0, actions/upload-artifact@v4.6.2.

Locations:

- `.github/workflows/check-dist.yml:21`
- `.github/workflows/check-dist.yml:27`
- `.github/workflows/check-dist.yml:51`
- `.github/workflows/eslint.yml:18`
- `.github/workflows/eslint.yml:24`
- `.github/workflows/knip.yml:18`
- `.github/workflows/knip.yml:24`
- `.github/workflows/prettier.yml:18`
- `.github/workflows/prettier.yml:24`

### missing-permissions (severity: medium)

None of the four workflow files declare a top-level 'permissions:' block, and no job within any of these files declares its own 'permissions:' block. Without explicit permissions, workflows run with the default (potentially broad) GITHUB_TOKEN permissions, violating the principle of least privilege.

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
1. unpinned-uses: Pinned all action references to full 40-character commit SHAs — actions/checkout@v4.2.2 → 11bd71901bbe5b1630ceea73d27597364c9af683, actions/setup-node@v4.4.0 → 49933ea5288caeca8642d1e84afbd3f7d6820020, actions/upload-artifact@v4.6.2 → ea165f8d65b6e75b540449e92b4886f43607fa02. Version tag comments preserved for readability.
2. missing-permissions: Added `permissions: {}` top-level block to all four workflow files, enforcing least-privilege (no GITHUB_TOKEN permissions granted by default).

