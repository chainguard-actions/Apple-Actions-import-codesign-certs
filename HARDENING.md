<!-- markdownlint-disable -->

# Hardening Report: Apple-Actions--import-codesign-certs/v6.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Apple-Actions--import-codesign-certs/v6.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All four workflow files reference GitHub Actions using mutable version tags instead of full 40-character commit SHA hashes, making them vulnerable to supply-chain attacks if the tag is moved. Failing references: `actions/checkout@v6.0.0`, `actions/setup-node@v5.0.0`, `actions/upload-artifact@v5.0.0`.

Locations:

- `.github/workflows/check-dist.yml:22`
- `.github/workflows/check-dist.yml:28`
- `.github/workflows/check-dist.yml:51`
- `.github/workflows/eslint.yml:19`
- `.github/workflows/eslint.yml:24`
- `.github/workflows/knip.yml:18`
- `.github/workflows/knip.yml:23`
- `.github/workflows/prettier.yml:19`
- `.github/workflows/prettier.yml:24`

### missing-permissions (severity: medium)

None of the four workflow files declare a top-level `permissions:` block, and no individual job within any of them declares job-level permissions. Without explicit permissions, workflows run with the default (potentially broad) GITHUB_TOKEN permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/eslint.yml:1`
- `.github/workflows/knip.yml:1`
- `.github/workflows/prettier.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

All four workflow files (.github/workflows/check-dist.yml, eslint.yml, knip.yml, prettier.yml) were updated: (1) Replaced mutable version tags with full 40-character commit SHAs — actions/checkout@v6.0.0 → @1af3b93b6815bc44a9784bd300feb67ff0d1eeb3, actions/setup-node@v5.0.0 → @a0853c24544627f65ddf259abe73b1d18a591444, actions/upload-artifact@v5.0.0 → @330a01c490aca151604b8cf639adc76d48f6c5d4 — with the original tag preserved in a comment. (2) Added top-level `permissions: contents: read` to each workflow, which is the minimum needed for checkout operations.

