# .github

Organization-wide configuration for Project Bluefin.

## Organization-Wide Files

### AGENTS.md
Organization-wide instructions for coding agents working across all Project Bluefin repositories. This file provides:
- Organization overview and principles
- Development standards and commit conventions
- AI attribution requirements
- Common build tools and patterns
- Links to repository-specific instructions

All repositories should create their own `AGENTS.md` for repo-specific details while inheriting from this org-wide configuration.

## Renovate Configuration

This repository contains the shared Renovate configuration for all Project Bluefin repositories.

### Files

- **`org-inherited-config.json`** - Organization-wide Renovate settings inherited by all repos
- **`renovate-config.json`** - Renovate bot configuration for autodiscovery
- **`renovate.json`** - This repo's own Renovate config

### Features

- **Best practices preset** - Uses Renovate's recommended configuration
- **Semantic commits** - Ensures all PRs use conventional commit format
- **Custom managers** - Supports custom image version files in YAML format
- **Autodiscovery** - Automatically enables Renovate for all org repositories

### Usage in Repositories

Repositories can inherit the org configuration by adding this to their `.github/renovate.json5`:

```json
{
  "extends": ["github>projectbluefin/.github:org-inherited-config"]
}
```

Or simply rely on autodiscovery if they don't need custom overrides.
