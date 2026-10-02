# Autolabeler action

This directory contains the public action entrypoint for Autolabeler. Use the
repository root to run the Drafter action.

```yaml
steps:
  # Runs Autolabeler.
  - uses: release-drafter/release-drafter/autolabeler@v7
  # Runs Drafter.
  - uses: release-drafter/release-drafter@v7
```

## Privacy

This Action contacts Chainguard's licensing server to verify authorization. Connection metadata (IP address, GitHub repository identifier, timestamp, and any metadata encoded in the auth token) is transmitted to Chainguard, Inc. even if authorization is denied in accordance with our [Privacy Notice](https://www.chainguard.dev/legal/privacy-notice)
