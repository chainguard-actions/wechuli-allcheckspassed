<!-- markdownlint-disable -->

# Hardening Report: wechuli--allcheckspassed/v2.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wechuli--allcheckspassed/v2.5.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Two `run:` steps in update-main-version.yml directly interpolate user-controlled `workflow_dispatch` inputs into shell commands without going through an env var. Rule (a) violation: `run: git tag -f ${{ github.event.inputs.major_version }} ${{ github.event.inputs.target }}` and `run: git push origin ${{ github.event.inputs.major_version }} --force`. An attacker with write access (or a malicious input) could inject arbitrary shell commands via these inputs.

Locations:

- `.github/workflows/update-main-version.yml:28`
- `.github/workflows/update-main-version.yml:30`

### unpinned-uses (severity: high)

Multiple workflow files reference Actions using mutable version tags instead of pinned 40-character commit SHAs, making them vulnerable to supply-chain attacks if the tag is moved. Failing references: ci.yml — `actions/checkout@v6`, `actions/setup-node@v6` (×2 jobs each); codeql.yml — `actions/checkout@v6`, `github/codeql-action/init@v4`, `github/codeql-action/analyze@v4`; update-main-version.yml — `actions/checkout@v6`.

Locations:

- `.github/workflows/ci.yml:15`
- `.github/workflows/ci.yml:17`
- `.github/workflows/ci.yml:27`
- `.github/workflows/ci.yml:29`
- `.github/workflows/codeql.yml:52`
- `.github/workflows/codeql.yml:59`
- `.github/workflows/codeql.yml:62`
- `.github/workflows/codeql.yml:88`
- `.github/workflows/update-main-version.yml:21`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed script-injection in update-main-version.yml by moving workflow_dispatch inputs (major_version and target) into env: blocks and referencing them as double-quoted shell variables. Fixed unpinned-uses across all three workflow files by pinning: actions/checkout@v6 → SHA d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v6 → SHA 249970729cb0ef3589644e2896645e5dc5ba9c38, github/codeql-action/init@v4 → SHA ff2f1c621b7f889edc0d3c761ac2e6a3f8cdb0dd, github/codeql-action/analyze@v4 → SHA ff2f1c621b7f889edc0d3c761ac2e6a3f8cdb0dd. All SHAs were resolved via lookup_action_sha.

