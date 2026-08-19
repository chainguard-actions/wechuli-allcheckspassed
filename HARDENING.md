<!-- markdownlint-disable -->

# Hardening Report: wechuli--allcheckspassed/v2.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wechuli--allcheckspassed/v2.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): Direct expression interpolation of user-controlled workflow_dispatch inputs inside run: shell commands. In 'Tag new target' step, `${{ github.event.inputs.major_version }}` and `${{ github.event.inputs.target }}` are interpolated directly into the shell command `git tag -f ${{ github.event.inputs.major_version }} ${{ github.event.inputs.target }}`. In 'Push new tag' step, `${{ github.event.inputs.major_version }}` is interpolated directly into `git push origin ${{ github.event.inputs.major_version }} --force`. An attacker with workflow_dispatch access could inject arbitrary shell commands via these inputs.

Locations:

- `.github/workflows/update-main-version.yml:29`
- `.github/workflows/update-main-version.yml:31`

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags instead of pinned 40-character SHA digests, making them vulnerable to supply-chain attacks if the referenced tag is moved. Failing references: ci.yml — actions/checkout@v4, actions/setup-node@v4; codeql.yml — actions/checkout@v4, github/codeql-action/init@v3, github/codeql-action/analyze@v3; test.yml — actions/checkout@v4, actions/setup-node@v4; update-main-version.yml — actions/checkout@v4.

Locations:

- `.github/workflows/ci.yml:13`
- `.github/workflows/ci.yml:16`
- `.github/workflows/codeql.yml:55`
- `.github/workflows/codeql.yml:62`
- `.github/workflows/codeql.yml:79`
- `.github/workflows/test.yml:12`
- `.github/workflows/test.yml:15`
- `.github/workflows/update-main-version.yml:22`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed script injection in update-main-version.yml by moving github.event.inputs.major_version and github.event.inputs.target into env: blocks and referencing them as $MAJOR_VERSION and $TARGET in the shell commands. Pinned all 8 unpinned action references to full 40-character SHAs: actions/checkout@v4→11d5960a..., actions/setup-node@v4→49933ea5..., github/codeql-action/init@v3→4187e74d..., github/codeql-action/analyze@v3→4187e74d... across ci.yml, codeql.yml, test.yml, and update-main-version.yml.

