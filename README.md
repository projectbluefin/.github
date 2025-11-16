# .github

Organization-wide configuration for Project Bluefin.

## Renovate Configuration

This repository contains the shared Renovate configuration for all Project Bluefin repositories.

### Files

- **`org-inherited-config.json`** - Organization-wide Renovate settings inherited by all repos
- **`renovate-config.json`** - Renovate bot configuration for autodiscovery (inherits from ublue-os/renovate-config)
- **`renovate.json`** - This repo's own Renovate config

### Features

- **Best practices preset** - Uses Renovate's recommended configuration
- **Semantic commits** - Ensures all PRs use conventional commit format
- **Custom managers** - Supports custom image version files in YAML format
- **Autodiscovery** - Automatically enables Renovate for all org repositories
- **Inherits from ublue-os** - Uses ublue-os/renovate-config as the base configuration

### Usage in Repositories

Repositories can inherit the org configuration by adding this to their `.github/renovate.json5`:

```json
{
  "extends": ["github>projectbluefin/.github:org-inherited-config"]
}
```

Or inherit directly from ublue-os:

```json
{
  "extends": ["github>ublue-os/renovate-config:org-inherited-config"]
}
```

Or simply rely on autodiscovery if they don't need custom overrides.
