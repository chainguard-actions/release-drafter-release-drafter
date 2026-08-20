<!-- markdownlint-disable -->

# Hardening Report: release-drafter--release-drafter/v7.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **release-drafter--release-drafter/v7.0.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'Update major tag' step in release.yml directly interpolates `${{ steps.version.outputs.major }}` and `${{ steps.version.outputs.tag }}` (both `steps.*.outputs.*` contexts, which are untrusted per the check definition) inside a `run:` shell command string. An attacker who can influence the step output values could inject arbitrary shell commands. Offending lines:
  `git tag v${{ steps.version.outputs.major }} ${{ steps.version.outputs.tag }} -f`
  `git push origin refs/tags/v${{ steps.version.outputs.major }} -f`

Locations:

- `.github/workflows/release.yml:51`
- `.github/workflows/release.yml:52`

### unpinned-uses (severity: high)

Multiple workflow files and the common-setup composite action reference external actions using mutable version tags or branch names instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag or branch is moved to a malicious commit. Failing references include:
- ci.yml: `actions/checkout@v6` (×3)
- codeql-analysis.yml: `actions/checkout@v6`, `github/codeql-action/init@v4`, `github/codeql-action/autobuild@v4`, `github/codeql-action/analyze@v4`
- draft.yml: `release-drafter/release-drafter@master`
- licensed.yml: `actions/checkout@v6`, `ruby/setup-ruby@v1`, `licensee/setup-licensed@v1.3.2`
- release.yml: `actions/checkout@v6`, `release-drafter/release-drafter@master`
- renovate-build.yml: `actions/checkout@v6`
- stale.yml: `actions/stale@v10`
- .github/actions/common-setup/action.yml: `actions/setup-node@v6`

Locations:

- `.github/workflows/ci.yml:18`
- `.github/workflows/ci.yml:31`
- `.github/workflows/ci.yml:40`
- `.github/workflows/codeql-analysis.yml:21`
- `.github/workflows/codeql-analysis.yml:26`
- `.github/workflows/codeql-analysis.yml:33`
- `.github/workflows/codeql-analysis.yml:38`
- `.github/workflows/draft.yml:10`
- `.github/workflows/licensed.yml:30`
- `.github/workflows/licensed.yml:35`
- `.github/workflows/licensed.yml:38`
- `.github/workflows/release.yml:17`
- `.github/workflows/release.yml:44`
- `.github/workflows/renovate-build.yml:13`
- `.github/workflows/stale.yml:10`
- `.github/actions/common-setup/action.yml:9`

### missing-permissions (severity: medium)

Two workflow files have no top-level `permissions:` key and no job-level `permissions:` keys on any of their jobs. Without explicit permissions, workflows inherit the default repository permissions (which may be broad), violating the principle of least privilege.
- draft.yml: triggers on push to master, uses `release-drafter/release-drafter@master` with no permissions declared.
- stale.yml: scheduled workflow using `actions/stale@v10` with no permissions declared.

Locations:

- `.github/workflows/draft.yml:1`
- `.github/workflows/stale.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all three findings:
1. script-injection in release.yml: Moved steps.version.outputs.major and steps.version.outputs.tag into an env: block (MAJOR and TAG), then referenced them as "${MAJOR}" and "${TAG}" in the shell script.
2. unpinned-uses: Pinned all 9 distinct action references across 7 files to full 40-char commit SHAs. Note: release-drafter/release-drafter@master ref was not found (repo may have renamed default branch), so pinned to v7 SHA (34d80673e067bdc0c24568d3af899c216adcfaa9) in both draft.yml and release.yml.
3. missing-permissions: Added permissions blocks to draft.yml (contents: write, pull-requests: read) and stale.yml (issues: write, pull-requests: write) to enforce least privilege.

