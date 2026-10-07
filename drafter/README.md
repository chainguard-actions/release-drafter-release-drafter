# Drafter action

This directory contains an alternative public entrypoint for the Drafter
action. The repository root runs the same action.

```yaml
steps:
  - uses: release-drafter/release-drafter@v7
  # This entrypoint is equivalent to the repository root.
  - uses: release-drafter/release-drafter/drafter@v7
```

## Privacy

This Action contacts Chainguard's licensing server to verify authorization. Connection metadata (IP address, GitHub repository identifier, timestamp, and any metadata encoded in the auth token) is transmitted to Chainguard, Inc. even if authorization is denied in accordance with our [Privacy Notice](https://www.chainguard.dev/legal/privacy-notice)
