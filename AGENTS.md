# Project Bluefin Organization-Wide Copilot Instructions

This document provides essential information for coding agents working across all Project Bluefin repositories.

## Organization Overview

**Project Bluefin** is a cloud-native desktop operating system project that reimagines the Linux desktop experience. The organization maintains several repositories that together create a complete desktop Linux distribution built on Fedora using container technologies with atomic updates.

## Core Principles

- **Container-native**: All components are built and distributed as OCI containers
- **Atomic updates**: System updates are transactional and can be rolled back
- **Cloud-native workflows**: Built with modern DevOps practices and GitOps principles
- **Developer-focused**: Optimized for developers while remaining accessible to general users
- **Open source**: Apache 2.0 licensed, community-driven development

## Repository Structure

### Main Repositories

- **[bluefin](https://github.com/ublue-os/bluefin)** - Main OS build system and image configurations
- **[bluefin-lts](https://github.com/ublue-os/bluefin-lts)** - Long-term support variant
- **[common](https://github.com/projectbluefin/common)** - Shared OCI layer with common configuration files
- **[bluefin-docs](https://github.com/projectbluefin/bluefin-docs)** - Documentation website
- **[.github](https://github.com/projectbluefin/.github)** - Organization-wide configuration (this repo)

### Related Projects

- **[bazzite](https://github.com/ublue-os/bazzite)** - Gaming-focused variant
- **[aurora](https://github.com/ublue-os/aurora)** - KDE Plasma variant
- Built on **[Universal Blue](https://github.com/ublue-os)** infrastructure

## Development Standards

### Commit Conventions

**REQUIRED**: Use [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/#specification) for all commits and PR titles.

Format: `<type>(<scope>): <description>`

Common types:
- `feat:` - New features
- `fix:` - Bug fixes
- `docs:` - Documentation changes
- `ci:` - CI/CD changes
- `refactor:` - Code refactoring
- `chore:` - Maintenance tasks
- `build:` - Build system changes

### AI Attribution

All AI-assisted commits **MUST** disclose what tool and model they are using in the "Assisted-by" commit footer:

```
Assisted-by: [Model Name] via [Tool Name]
```

Example:
```
feat: add container build optimization

Optimize multi-stage build to reduce image size.

Assisted-by: Claude 3.5 Sonnet via GitHub Copilot
```

### Code Quality

- Make **minimal, surgical changes** - only modify what's necessary
- Preserve existing working code unless specifically asked to change it
- Test builds locally before pushing when possible
- Follow repository-specific conventions (check individual AGENTS.md files)

## Build Tools

Most repositories use these common tools:

- **Just** - Command runner (like Make but simpler)
- **Podman/Buildah** - Container building and management
- **GitHub Actions** - CI/CD automation
- **Renovate** - Automated dependency updates

## Renovate Configuration

All repositories inherit from **[ublue-os/renovate-config](https://github.com/ublue-os/renovate-config)** which provides:
- Best practices preset
- Semantic commit messages
- Custom managers for image-versions YAML files
- Automated dependency updates

Repositories can extend this with local overrides in `.github/renovate.json5`.

## Documentation

- **Public docs**: https://docs.projectbluefin.io
- **Repository-specific**: Check each repo's README.md and AGENTS.md
- **Contributing**: See CONTRIBUTING.md in individual repositories

## Common Patterns

### Container Build Structure

Most image repositories follow this pattern:
```dockerfile
FROM base-image AS stage-name
COPY files /destination
RUN build-scripts.sh
```

### Just Commands

Common Just recipes across repositories:
- `just build` - Build the container/project locally
- `just test` - Run tests
- `just validate` - Validate configuration

### Configuration Files

- `.github/renovate.json5` - Renovate configuration
- `.github/workflows/` - GitHub Actions workflows
- `AGENTS.md` - Repository-specific agent instructions
- `Justfile` - Build automation recipes

## Testing Philosophy

- Validate locally before pushing when possible
- CI/CD will catch issues, but local testing is faster
- Don't break existing tests unless specifically asked
- Only fix test failures related to your changes

## Support and Community

- **Discord**: https://discord.gg/projectbluefin (community chat)
- **GitHub Issues**: For bug reports and feature requests
- **GitHub Discussions**: For questions and community discussion

## Important Notes

- **Trust these instructions** - They represent the current state of the project
- **Check repository-specific AGENTS.md** - For detailed repo-specific guidance
- **Ask for clarification** - If instructions are unclear or contradictory
- **Stay focused** - Make minimal changes to accomplish the specific task

## Related Organizations

Project Bluefin is part of the broader **Universal Blue** ecosystem:
- **[ublue-os](https://github.com/ublue-os)** - Parent organization and infrastructure
- **[Fedora Project](https://fedoraproject.org/)** - Upstream Linux distribution
- **[GNOME](https://www.gnome.org/)** - Desktop environment

---

*This is an organization-wide configuration. For repository-specific instructions, always check the AGENTS.md file in the specific repository you're working on.*
