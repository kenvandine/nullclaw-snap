# Snap Agent Responsibilities

This snap is now maintained by [automated-ken](https://github.com/kenvandine/automated-ken), a self-hosted agentic snap-maintenance dashboard.

## Automated Responsibilities

- **Version Detection**: Automatically polls upstream for new releases
- **Version Bump PRs**: Opens PRs to update the pinned version when new upstream releases are detected
- **CI Monitoring**: Monitors build workflows and asks Copilot cloud agent to fix failing builds (with follow-up PRs)
- **YARF Testing**: Runs YARF (Yet Another Release Framework) tests
- **Channel Promotion**: Manages promotion from edge -> candidate -> stable

## Important Notes

- **Do not hand-edit the pinned version** in snap/snapcraft.yaml
- The removed workflow's job (upstream release polling) is now automated-ken's responsibility
- All build and publish operations now follow the canonical pattern defined in `.github/workflows/automated-snap-build.yml`

## Maintainer/Agent Guidelines

- All version updates are now automated
- CI failures are automatically diagnosed and fixed by the agent
- Channel promotion follows a strict edge -> candidate -> stable pipeline
- Manual intervention should be rare and only for exceptional cases
