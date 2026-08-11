<!-- markdownlint-disable -->

# Hardening Report: release-drafter--release-drafter/v7.7.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **release-drafter--release-drafter/v7.7.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Two workflow files reference actions by mutable tag instead of a full 40-character commit SHA, making them vulnerable to supply-chain attacks:
- `.github/workflows/ci.yml`: `uses: amannn/action-semantic-pull-request@v6.1.1` (tag ref)
- `.github/workflows/licensed.yml`: `uses: ruby/setup-ruby@v1` (tag ref)
These should be pinned to their full commit SHAs.

Locations:

- `.github/workflows/ci.yml:21`
- `.github/workflows/licensed.yml:38`

### missing-permissions (severity: medium)

Two workflow files have no top-level `permissions:` key and no job-level `permissions:` blocks, meaning jobs run with the default (potentially broad) GITHUB_TOKEN permissions:
- `.github/workflows/draft.yml`: no permissions declared at any level.
- `.github/workflows/stale.yml`: no permissions declared at any level.

Locations:

- `.github/workflows/draft.yml:1`
- `.github/workflows/stale.yml:1`

### script-injection (severity: high)

In `.github/workflows/release.yml`, the `Update major tag` step directly interpolates `${{ steps.version.outputs.major }}` and `${{ steps.version.outputs.tag }}` inside a `run:` shell block (rule a: direct expression interpolation). If these step outputs were ever influenced by attacker-controlled data, this would allow shell command injection. The expressions should be passed via `env:` variables and then referenced as quoted shell variables.

Offending lines:
  `git tag v${{ steps.version.outputs.major }} ${{ steps.version.outputs.tag }} -f`
  `git push origin refs/tags/v${{ steps.version.outputs.major }} -f`

Locations:

- `.github/workflows/release.yml:46`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions, script-injection

**Notes:**

1. unpinned-uses: Pinned `amannn/action-semantic-pull-request@v6.1.1` to SHA `48f256284bd46cdaab1048c3721360e808335d50` in ci.yml, and `ruby/setup-ruby@v1` to SHA `95ef2b042f9d7a56d8268cba8559e2842e2ad01b` in licensed.yml. Original tags preserved as comments.
2. missing-permissions: Added `permissions: { contents: write, pull-requests: read }` to draft.yml (release-drafter needs to write releases and read PRs), and `permissions: { issues: write, pull-requests: write }` to stale.yml (stale action needs to label and close issues/PRs).
3. script-injection: In release.yml, moved `${{ steps.version.outputs.major }}` and `${{ steps.version.outputs.tag }}` out of the `run:` shell block into an `env:` block as `MAJOR` and `TAG`, then referenced them as quoted shell variables `"v${MAJOR}"`, `"${TAG}"`, and `"refs/tags/v${MAJOR}"` to prevent shell injection.

