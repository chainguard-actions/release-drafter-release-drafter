# Autolabeler action

This folder only serves as an alias when users reference the action, which
requires the presence of the [`action.yml`](./action.yml) file

```yaml
steps:
  # runs autolabeler
  - uses: release-drafter/release-drafter/autolabeler@latest
  # ⚠️ targets root `action.yaml` - runs drafter
  - uses: release-drafter/release-drafter@latest
```

## Privacy

This Action contacts Chainguard's licensing server to verify authorization. Connection metadata (IP address, GitHub repository identifier, timestamp, and any metadata encoded in the auth token) is transmitted to Chainguard, Inc. even if authorization is denied in accordance with our [Privacy Notice](https://www.chainguard.dev/legal/privacy-notice)
