<!-- markdownlint-disable -->

# Hardening Report: release-drafter--release-drafter/v7.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **release-drafter--release-drafter/v7.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action at .github/actions/common-setup/action.yml references `actions/setup-node@v6`, which uses a mutable version tag instead of a pinned 40-character commit SHA. This means the action could be silently updated to a different (potentially malicious) version without any change to this repository. It should be pinned to a full SHA, e.g. `actions/setup-node@<40-hex-char-sha> # v6`.

Locations:

- `.github/actions/common-setup/action.yml:9`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned `actions/setup-node@v6` to its full commit SHA `249970729cb0ef3589644e2896645e5dc5ba9c38` in `.github/actions/common-setup/action.yml`. The mutable tag is preserved as an inline comment (`# v6`) for readability.

